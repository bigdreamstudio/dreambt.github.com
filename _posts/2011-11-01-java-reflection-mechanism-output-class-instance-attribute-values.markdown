---
layout: post
category : Java
tags : [Java, how-to, reflection]
title: 利用Java反射机制输出类实例的属性值
wordpress_id: 1051
wordpress_url: http://www.im47.cn/?p=1051
date: 2011-11-01 12:51:05.000000000 +08:00
---
<strong>设计目的：</strong>

平时我们调试代码可以使用IDE内置的Debug工具，但有的时候我们可能没有办法利用这些专业的Debug工具进行调试（比如我们自己设计一套监控系统）。于是，我设计了一个可以打印类变量中属性值的Java类，主要利用了Java的反射机制，方便直接调用查看POJO传值是否正确等。更多用途期待大家发现，欢迎交流～

<strong>源代码参考：</strong>

打印类PrintClass：
<pre>package net.im47.utils;

import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * User: siqi
 * Date: 11-10-31
 * Time: 下午10:10
 * 该类用于打印类中的所有属性及其类型、值。
 */
public class PrintClass {
    public static DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");

    public static void print(Object object) {
        StringBuffer str = new StringBuffer();
        str.append("Attribute of instance: " + object.toString() + "\n");
        Field[] fields = object.getClass().getDeclaredFields();
        for (Field field : fields) {
            if (field.getName().equals("serialVersionUID") || field.getName().equals("$VALUES")) return;
            String name = getAttrName(field);
            String type = getAttrType(field);
            String value = getAttrValue(object, field, name);

            str.append("Name:" + name);
            str.append("\t\tType:" + type);
            str.append("\t\tValue:" + value);
            str.append("\n");
        }
        System.out.println(str);
    }

    private static String getAttrName(Field field) {
        return field.getName();
    }

    private static String getAttrType(Field field) {
        return field.getGenericType().toString();
    }

    private static String getAttrValue(Object object, Field field, String attrName) {
        try {
            Method method = object.getClass().getMethod("get" + attrName.substring(0, 1).toUpperCase() + attrName.substring(1));
            Class type = method.getReturnType();
            Object value = method.invoke(object);
            if ("java.util.Date".equals(type.toString())) {
                //dateFormat = DateFormat.getDateInstance(DateFormat.SHORT);
                //dateFormat = DateFormat.getDateInstance(DateFormat.MEDIUM);
                dateFormat = DateFormat.getDateInstance(DateFormat.FULL);
                return dateFormat.format(value);
            } else {
                print(value);
                return value.toString();
            }
        } catch (NoSuchMethodException e) {
            //System.out.println("Error: NoSuchMethod: get" + attrName.substring(0, 1).toUpperCase() + attrName.substring(1));
        } catch (InvocationTargetException e) {
            System.out.println("Error: NoInvocationTarget");
        } catch (IllegalAccessException e) {
            System.out.println("Error: IllegalAccess");
        }
        return "";
    }

    public static void main(String[] args) throws ParseException {
        Date birthday = dateFormat.parse("1987-08-08");
        User user = new User("baitao.jibt", "badsiu129hqw98ed", User.Gender.MALE, 24, birthday, new User.Address("china", "shandong", "jinan"));
        PrintClass.print(user);
    }
}</pre>
待检查类User：
<pre>package net.im47.utils;

import java.util.Date;

/**
 * User: siqi
 * Date: 11-10-30
 * Time: 下午7:22
 */
public class User {
    private String username;
    private String password;
    private Gender gender;
    private int age;
    private Date birthday;
    private Address address;

    public enum Gender {MALE, FEMALE}

    ;

    public User() {

    }

    public User(String username, String password, int age) {
        this.username = username;
        this.password = password;
        this.age = age;
    }

    public User(String username, String password, Gender gender, int age) {
        this.username = username;
        this.password = password;
        this.gender = gender;
        this.age = age;
    }

    public User(String username, String password, Gender gender, int age, Date birthday, Address address) {
        this.username = username;
        this.password = password;
        this.gender = gender;
        this.age = age;
        this.birthday = birthday;
        this.address = address;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public Gender getGender() {
        return gender;
    }

    public void setGender(Gender gender) {
        this.gender = gender;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    public static class Address {
        private String country;
        private String province;
        private String city;

        public Address() {
        }

        public Address(String country, String city, String province) {
            this.country = country;
            this.city = city;
            this.province = province;
        }

        public String getCountry() {
            return country;
        }

        public void setCountry(String country) {
            this.country = country;
        }

        public String getProvince() {
            return province;
        }

        public void setProvince(String province) {
            this.province = province;
        }

        public String getCity() {
            return city;
        }

        public void setCity(String city) {
            this.city = city;
        }
    }
}</pre>
