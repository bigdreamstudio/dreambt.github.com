---
layout: post
category : Test
tags : [MQ, Test, RabbitMQ]
title: 任务分发策略改进模拟与分析
tagline: 阿里巴巴中文站日常回归测试过程改进
date: 2011-10-20 19:22:11.000000000 +08:00
thumbnail: /assets/images/2011/10/python-three-overall.png
excerpt: 在阿里巴巴实习的几个星期里，发现中文站日常回归测试的自动化测试脚本的分发机制存在问题，导致每次自动化回归测试都需要1个多小时的时间。于是，我收集了最近一次的执行数据并做了下面的研究和分析，提出了相应的整改方案。分享于此，希望对需要的同学有所帮助。
---
## 问题发现

在阿里巴巴实习的几个星期里，发现中文站日常回归测试的自动化测试脚本的分发机制存在问题，导致每次自动化回归测试都需要1个多小时的时间。于是，我收集了最近一次的执行数据并做了下面的研究和分析，提出了相应的整改方案。分享于此，希望对需要的同学有所帮助。

## 基本原理

![/assets/images/2011/10/python-three-overall.png](/assets/images/2011/10/python-three-overall.png)

## 模拟场景

10台执行机，每台执行机对应一个线程；为提高效率，时间没有做格式化处理；主进程分发任务，各任务耗时如下（单位ms）：

> 任务1-1:100 任务1-2:800 任务1-3:200 任务1-4:1200 任务1-5:750

> 任务1-6:1300 任务1-7:800 任务1-8:400 任务1-9:600 任务1-10:750

> 任务1-11:2000 任务1-12:1000 任务1-13:300 任务1-14:900 任务1-15:1200

> 任务1-16:750 任务1-17:1300 任务1-18:800 任务1-19:400 任务1-20:600

> 任务1-21:200 任务1-22:3000 任务1-23:1000 任务1-24:800 任务1-25:900

> 任务1-26:800 任务1-27:400 任务1-28:600 任务1-29:750 任务1-30:2000

## 执行机类

<pre>package cn.im47.queue.persistentwork;

import cn.im47.queue.utils.MyChannelFactory;
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.QueueingConsumer;

import java.io.IOException;

public class Receiver {
    private final static String TASK_QUEUE_NAME = "task_queue";
    private Channel channel = null;
    private QueueingConsumer consumer = null;
    private String name = "";
    private static int num = 0;

    public Receiver() {
        this.name = "执行机" + num++;
    }

    public void receive() throws IOException, InterruptedException {
        if (channel == null) {
            channel = MyChannelFactory.getInstance();

            channel.queueDeclare(TASK_QUEUE_NAME, QueueConfig.isDurable(), false, false, null);

            // 设置每次分发给同一个消费者的消息数
            channel.basicQos(QueueConfig.getPrefetchCount());

            consumer = new QueueingConsumer(channel);
            channel.basicConsume(TASK_QUEUE_NAME, QueueConfig.isAutoACK(), consumer);
        }

        System.out.println("[*] Waiting for messages. To exit press CTRL+C");
        while (true) {
            QueueingConsumer.Delivery delivery = consumer.nextDelivery();
            String message = new String(delivery.getBody());

            System.out.println("[ ] " + System.currentTimeMillis() + " " + this.name + ". Received :'" + message + "'");
            // 杯具的“执行机2”蓝屏了
            /*if (this.name.equals("执行机2")) {
                System.out.println("执行机2 挂掉了！！！");
                break;
            }*/
            doWork(message);
            System.out.println("[-] " + System.currentTimeMillis() + " " + this.name + ". Finished :'" + message + "'");

            // 只有在任务真正消费掉的情况下才从队列中删除该消息
            if (isNotAutoACK()) channel.basicAck(delivery.getEnvelope().getDeliveryTag(), false);
        }
    }

    private boolean isNotAutoACK() {
        return !QueueConfig.isAutoACK();
    }

    private void doWork(String message) throws InterruptedException {
        Thread.sleep(30 * Integer.parseInt(message.split(":")[1]));
    }
}</pre>

## 任务分发器

<pre>package cn.im47.queue.persistentwork;

import cn.im47.queue.utils.MyChannelFactory;
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.MessageProperties;

import java.io.IOException;

public class Sender {
    private final static String TASK_QUEUE_NAME = "task_queue";
    private Channel channel = null;
    private String message = "";

    public void send() throws IOException {
        if (channel == null) {
            channel = MyChannelFactory.getInstance();

            // 声明队列
            // QueueConfig.isDurable() 设置队列支持持久化
            channel.queueDeclare(TASK_QUEUE_NAME, QueueConfig.isDurable(), false, false, null);
        }

        // 发送消息
        // MessageProperties.PERSISTENT_TEXT_PLAIN 将消息持久化
        channel.basicPublish("", TASK_QUEUE_NAME, MessageProperties.PERSISTENT_TEXT_PLAIN, message.getBytes());

        System.out.println("[+] Sent message:'" + message + "'");
    }

    public void send(String message) throws IOException {
        this.message = message;
        send();
    }

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }
}</pre>

## 配置类

<pre>package cn.im47.queue.persistentwork;

public class QueueConfig {
    /**
     * 是否自动删除消息
     * true 自动删除消息
     * false channel.basicAck(delivery.getEnvelope().getDeliveryTag(), false);
     */
    private static boolean autoACK = false;

    /**
     * 消息持久性
     */
    private static boolean durable = true;

    /**
     * 限制每次分配一个消息，保证QoS
     */
    private static int prefetchCount = 1;

    public static boolean isAutoACK() {
        return autoACK;
    }

    public static void setAutoACK(boolean autoACK) {
        QueueConfig.autoACK = autoACK;
    }

    public static boolean isDurable() {
        return durable;
    }

    public static void setDurable(boolean durable) {
        QueueConfig.durable = durable;
    }

    public static int getPrefetchCount() {
        return prefetchCount;
    }

    public static void setPrefetchCount(int prefetchCount) {
        QueueConfig.prefetchCount = prefetchCount;
    }
}</pre>

## 模拟数据测试类

<pre>package cn.im47.queue.persistentwork;

import org.junit.Test;

import java.io.IOException;
import java.util.Random;

public class testWork implements Runnable {
    private Receiver receiver = new Receiver();
    private Sender sender = new Sender();
    private int num = 0;
    private int site_id = new Random().nextInt(1212423);

    @Test
    public void testReceive() throws Exception {
        new Thread(new testWork()).start();
        new Thread(new testWork()).start();
        new Thread(new testWork()).start();
        new Thread(new testWork()).start();
        new Thread(new testWork()).start();

        new Thread(new testWork()).start();
        new Thread(new testWork()).start();
        new Thread(new testWork()).start();
        new Thread(new testWork()).start();
        new Thread(new testWork()).start();
        Thread.sleep(3000);

        System.out.println("开始时间：" + System.currentTimeMillis());
        /*sender.send("站点"+site_id+" 任务1-1:100");
        sender.send("站点"+site_id+" 任务1-2:800");
        sender.send("站点"+site_id+" 任务1-3:200");
        sender.send("站点"+site_id+" 任务1-4:1200");
        sender.send("站点"+site_id+" 任务1-5:750");
        sender.send("站点"+site_id+" 任务1-6:1300");
        sender.send("站点"+site_id+" 任务1-7:800");
        sender.send("站点"+site_id+" 任务1-8:400");
        sender.send("站点"+site_id+" 任务1-9:600");
        sender.send("站点"+site_id+" 任务1-10:750");
        sender.send("站点"+site_id+" 任务1-11:2000");
        sender.send("站点"+site_id+" 任务1-12:1000");
        sender.send("站点"+site_id+" 任务1-13:300");
        sender.send("站点"+site_id+" 任务1-14:900");
        sender.send("站点"+site_id+" 任务1-15:1200");
        sender.send("站点"+site_id+" 任务1-16:750");
        sender.send("站点"+site_id+" 任务1-17:1300");
        sender.send("站点"+site_id+" 任务1-18:800");
        sender.send("站点"+site_id+" 任务1-19:400");
        sender.send("站点"+site_id+" 任务1-20:600");
        sender.send("站点"+site_id+" 任务1-21:200");
        sender.send("站点"+site_id+" 任务1-22:3000");
        sender.send("站点"+site_id+" 任务1-23:1000");
        sender.send("站点"+site_id+" 任务1-24:800");
        sender.send("站点"+site_id+" 任务1-25:900");
        sender.send("站点"+site_id+" 任务1-26:800");
        sender.send("站点"+site_id+" 任务1-27:400");
        sender.send("站点"+site_id+" 任务1-28:600");
        sender.send("站点"+site_id+" 任务1-29:750");
        sender.send("站点"+site_id+" 任务1-30:2000");*/

        // 最优
        sender.send("站点" + site_id + " 任务1:3000");
        sender.send("站点" + site_id + " 任务1:2000");
        sender.send("站点" + site_id + " 任务1:2000");
        sender.send("站点" + site_id + " 任务1:1300");
        sender.send("站点" + site_id + " 任务1:1300");
        sender.send("站点" + site_id + " 任务1:1200");
        sender.send("站点" + site_id + " 任务1:1200");
        sender.send("站点" + site_id + " 任务1:1000");
        sender.send("站点" + site_id + " 任务1:1000");
        sender.send("站点" + site_id + " 任务1:900");
        sender.send("站点" + site_id + " 任务1:900");
        sender.send("站点" + site_id + " 任务1:800");
        sender.send("站点" + site_id + " 任务1:800");
        sender.send("站点" + site_id + " 任务1:800");
        sender.send("站点" + site_id + " 任务1:800");
        sender.send("站点" + site_id + " 任务1:800");
        sender.send("站点" + site_id + " 任务1:750");
        sender.send("站点" + site_id + " 任务1:750");
        sender.send("站点" + site_id + " 任务1:750");
        sender.send("站点" + site_id + " 任务1:750");
        sender.send("站点" + site_id + " 任务1:600");
        sender.send("站点" + site_id + " 任务1:600");
        sender.send("站点" + site_id + " 任务1:600");
        sender.send("站点" + site_id + " 任务1:400");
        sender.send("站点" + site_id + " 任务1:400");
        sender.send("站点" + site_id + " 任务1:400");
        sender.send("站点" + site_id + " 任务1:300");
        sender.send("站点" + site_id + " 任务1:200");
        sender.send("站点" + site_id + " 任务1:200");
        sender.send("站点" + site_id + " 任务1:100");

        Thread.sleep(60000);
        //sender.send("任务2:");
    }

    public void run() {
        try {
            receiver.receive();
        } catch (IOException e) {
            e.printStackTrace();  //To change body of catch statement use File | Settings | File Templates.
        } catch (InterruptedException e) {
            e.printStackTrace();  //To change body of catch statement use File | Settings | File Templates.
        }
    }
}</pre>

## 实际数据测试类（2011年10月19号）

<pre>package cn.im47.queue.persistentwork;

import org.junit.Test;

import java.io.IOException;
import java.util.Random;

public class testWork implements Runnable {
    private Receiver receiver = new Receiver();
    private Sender sender = new Sender();
    private int num = 0;
    private int site_id = new Random().nextInt(1212423);

    @Test
    public void testReceive() throws Exception {
        new Thread(new testWork()).start();
        new Thread(new testWork()).start();
        new Thread(new testWork()).start();
        new Thread(new testWork()).start();
        new Thread(new testWork()).start();

        new Thread(new testWork()).start();
        new Thread(new testWork()).start();
        new Thread(new testWork()).start();
        new Thread(new testWork()).start();
        new Thread(new testWork()).start();

        new Thread(new testWork()).start();
        new Thread(new testWork()).start();
        new Thread(new testWork()).start();

        Thread.sleep(5000);

        System.out.println("开始时间：" + System.currentTimeMillis());
        /*sender.send("站点" + site_id + " 任务1-1:138");
        sender.send("站点" + site_id + " 任务1-2:66");
        sender.send("站点" + site_id + " 任务1-3:426");
        sender.send("站点" + site_id + " 任务1-4:64");
        sender.send("站点" + site_id + " 任务1-5:434");
        sender.send("站点" + site_id + " 任务1-6:3");
        sender.send("站点" + site_id + " 任务1-7:43");
        sender.send("站点" + site_id + " 任务1-8:55");
        sender.send("站点" + site_id + " 任务1-9:47");
        sender.send("站点" + site_id + " 任务1-10:79");
        sender.send("站点" + site_id + " 任务1-11:88");
        sender.send("站点" + site_id + " 任务1-12:137");
        sender.send("站点" + site_id + " 任务1-13:23");
        sender.send("站点" + site_id + " 任务1-14:240");
        sender.send("站点" + site_id + " 任务1-15:118");
        sender.send("站点" + site_id + " 任务1-16:240");
        sender.send("站点" + site_id + " 任务1-17:81");
        sender.send("站点" + site_id + " 任务1-18:64");
        sender.send("站点" + site_id + " 任务1-19:163");
        sender.send("站点" + site_id + " 任务1-20:122");
        sender.send("站点" + site_id + " 任务1-21:771");
        sender.send("站点" + site_id + " 任务1-22:537");
        sender.send("站点" + site_id + " 任务1-23:154");
        sender.send("站点" + site_id + " 任务1-24:445");
        sender.send("站点" + site_id + " 任务1-25:486");
        sender.send("站点" + site_id + " 任务1-26:239");
        sender.send("站点" + site_id + " 任务1-27:295");
        sender.send("站点" + site_id + " 任务1-28:129");
        sender.send("站点" + site_id + " 任务1-29:407");
        sender.send("站点" + site_id + " 任务1-30:967");
        sender.send("站点" + site_id + " 任务1-31:69");
        sender.send("站点" + site_id + " 任务1-32:174");
        sender.send("站点" + site_id + " 任务1-33:182");
        sender.send("站点" + site_id + " 任务1-34:28");
        sender.send("站点" + site_id + " 任务1-35:1410");
        sender.send("站点" + site_id + " 任务1-36:465");
        sender.send("站点" + site_id + " 任务1-37:524");
        sender.send("站点" + site_id + " 任务1-38:57");
        sender.send("站点" + site_id + " 任务1-39:82");
        sender.send("站点" + site_id + " 任务1-40:87");
        sender.send("站点" + site_id + " 任务1-41:96");
        sender.send("站点" + site_id + " 任务1-42:124");
        sender.send("站点" + site_id + " 任务1-43:18");
        sender.send("站点" + site_id + " 任务1-44:453");
        sender.send("站点" + site_id + " 任务1-45:102");
        sender.send("站点" + site_id + " 任务1-46:128");
        sender.send("站点" + site_id + " 任务1-47:910");
        sender.send("站点" + site_id + " 任务1-48:27");
        sender.send("站点" + site_id + " 任务1-49:226");
        sender.send("站点" + site_id + " 任务1-50:216");
        sender.send("站点" + site_id + " 任务1-51:27");
        sender.send("站点" + site_id + " 任务1-52:161");
        sender.send("站点" + site_id + " 任务1-53:647");
        sender.send("站点" + site_id + " 任务1-54:10");
        sender.send("站点" + site_id + " 任务1-55:21");
        sender.send("站点" + site_id + " 任务1-56:52");
        sender.send("站点" + site_id + " 任务1-57:93");
        sender.send("站点" + site_id + " 任务1-58:92");
        sender.send("站点" + site_id + " 任务1-59:273");
        sender.send("站点" + site_id + " 任务1-60:157");
        sender.send("站点" + site_id + " 任务1-61:38");
        sender.send("站点" + site_id + " 任务1-62:463");
        sender.send("站点" + site_id + " 任务1-63:515");
        sender.send("站点" + site_id + " 任务1-64:81");
        sender.send("站点" + site_id + " 任务1-65:725");
        sender.send("站点" + site_id + " 任务1-66:65");
        sender.send("站点" + site_id + " 任务1-67:354");
        sender.send("站点" + site_id + " 任务1-68:459");
        sender.send("站点" + site_id + " 任务1-69:128");
        sender.send("站点" + site_id + " 任务1-70:604");
        sender.send("站点" + site_id + " 任务1-71:148");
        sender.send("站点" + site_id + " 任务1-72:222");
        sender.send("站点" + site_id + " 任务1-73:497");
        sender.send("站点" + site_id + " 任务1-74:496");
        sender.send("站点" + site_id + " 任务1-75:477");
        sender.send("站点" + site_id + " 任务1-76:117");
        sender.send("站点" + site_id + " 任务1-77:42");
        sender.send("站点" + site_id + " 任务1-78:39");
        sender.send("站点" + site_id + " 任务1-79:291");
        sender.send("站点" + site_id + " 任务1-80:31");
        sender.send("站点" + site_id + " 任务1-81:18");
        sender.send("站点" + site_id + " 任务1-82:317");
        sender.send("站点" + site_id + " 任务1-83:16");
        sender.send("站点" + site_id + " 任务1-84:32");
        sender.send("站点" + site_id + " 任务1-85:38");
        sender.send("站点" + site_id + " 任务1-86:168");
        sender.send("站点" + site_id + " 任务1-87:28");
        sender.send("站点" + site_id + " 任务1-88:146");
        sender.send("站点" + site_id + " 任务1-89:163");
        sender.send("站点" + site_id + " 任务1-90:24");
        sender.send("站点" + site_id + " 任务1-91:25");
        sender.send("站点" + site_id + " 任务1-92:255");
        sender.send("站点" + site_id + " 任务1-93:216");
        sender.send("站点" + site_id + " 任务1-94:43");
        sender.send("站点" + site_id + " 任务1-95:75");*/

        // 最优
        /*sender.send("站点" + site_id + " 任务1-35:1410");
        sender.send("站点" + site_id + " 任务1-30:967");
        sender.send("站点" + site_id + " 任务1-47:910");
        sender.send("站点" + site_id + " 任务1-21:771");
        sender.send("站点" + site_id + " 任务1-65:725");
        sender.send("站点" + site_id + " 任务1-53:647");
        sender.send("站点" + site_id + " 任务1-70:604");
        sender.send("站点" + site_id + " 任务1-22:537");
        sender.send("站点" + site_id + " 任务1-37:524");
        sender.send("站点" + site_id + " 任务1-63:515");
        sender.send("站点" + site_id + " 任务1-73:497");
        sender.send("站点" + site_id + " 任务1-74:496");
        sender.send("站点" + site_id + " 任务1-25:486");
        sender.send("站点" + site_id + " 任务1-75:477");
        sender.send("站点" + site_id + " 任务1-36:465");
        sender.send("站点" + site_id + " 任务1-62:463");
        sender.send("站点" + site_id + " 任务1-68:459");
        sender.send("站点" + site_id + " 任务1-44:453");
        sender.send("站点" + site_id + " 任务1-24:445");
        sender.send("站点" + site_id + " 任务1-5:434");
        sender.send("站点" + site_id + " 任务1-3:426");
        sender.send("站点" + site_id + " 任务1-29:407");
        sender.send("站点" + site_id + " 任务1-67:354");
        sender.send("站点" + site_id + " 任务1-82:317");
        sender.send("站点" + site_id + " 任务1-27:295");
        sender.send("站点" + site_id + " 任务1-79:291");
        sender.send("站点" + site_id + " 任务1-59:273");
        sender.send("站点" + site_id + " 任务1-92:255");
        sender.send("站点" + site_id + " 任务1-16:240");
        sender.send("站点" + site_id + " 任务1-14:240");
        sender.send("站点" + site_id + " 任务1-26:239");
        sender.send("站点" + site_id + " 任务1-49:226");
        sender.send("站点" + site_id + " 任务1-72:222");
        sender.send("站点" + site_id + " 任务1-50:216");
        sender.send("站点" + site_id + " 任务1-93:216");
        sender.send("站点" + site_id + " 任务1-33:182");
        sender.send("站点" + site_id + " 任务1-32:174");
        sender.send("站点" + site_id + " 任务1-86:168");
        sender.send("站点" + site_id + " 任务1-19:163");
        sender.send("站点" + site_id + " 任务1-89:163");
        sender.send("站点" + site_id + " 任务1-52:161");
        sender.send("站点" + site_id + " 任务1-60:157");
        sender.send("站点" + site_id + " 任务1-23:154");
        sender.send("站点" + site_id + " 任务1-71:148");
        sender.send("站点" + site_id + " 任务1-88:146");
        sender.send("站点" + site_id + " 任务1-1:138");
        sender.send("站点" + site_id + " 任务1-12:137");
        sender.send("站点" + site_id + " 任务1-28:129");
        sender.send("站点" + site_id + " 任务1-46:128");
        sender.send("站点" + site_id + " 任务1-69:128");
        sender.send("站点" + site_id + " 任务1-42:124");
        sender.send("站点" + site_id + " 任务1-20:122");
        sender.send("站点" + site_id + " 任务1-15:118");
        sender.send("站点" + site_id + " 任务1-76:117");
        sender.send("站点" + site_id + " 任务1-45:102");
        sender.send("站点" + site_id + " 任务1-41:96");
        sender.send("站点" + site_id + " 任务1-57:93");
        sender.send("站点" + site_id + " 任务1-58:92");
        sender.send("站点" + site_id + " 任务1-11:88");
        sender.send("站点" + site_id + " 任务1-40:87");
        sender.send("站点" + site_id + " 任务1-39:82");
        sender.send("站点" + site_id + " 任务1-17:81");
        sender.send("站点" + site_id + " 任务1-64:81");
        sender.send("站点" + site_id + " 任务1-10:79");
        sender.send("站点" + site_id + " 任务1-95:75");
        sender.send("站点" + site_id + " 任务1-31:69");
        sender.send("站点" + site_id + " 任务1-2:66");
        sender.send("站点" + site_id + " 任务1-66:65");
        sender.send("站点" + site_id + " 任务1-18:64");
        sender.send("站点" + site_id + " 任务1-4:64");
        sender.send("站点" + site_id + " 任务1-38:57");
        sender.send("站点" + site_id + " 任务1-8:55");
        sender.send("站点" + site_id + " 任务1-56:52");
        sender.send("站点" + site_id + " 任务1-9:47");
        sender.send("站点" + site_id + " 任务1-7:43");
        sender.send("站点" + site_id + " 任务1-94:43");
        sender.send("站点" + site_id + " 任务1-77:42");
        sender.send("站点" + site_id + " 任务1-78:39");
        sender.send("站点" + site_id + " 任务1-61:38");
        sender.send("站点" + site_id + " 任务1-85:38");
        sender.send("站点" + site_id + " 任务1-84:32");
        sender.send("站点" + site_id + " 任务1-80:31");
        sender.send("站点" + site_id + " 任务1-87:28");
        sender.send("站点" + site_id + " 任务1-34:28");
        sender.send("站点" + site_id + " 任务1-48:27");
        sender.send("站点" + site_id + " 任务1-51:27");
        sender.send("站点" + site_id + " 任务1-91:25");
        sender.send("站点" + site_id + " 任务1-90:24");
        sender.send("站点" + site_id + " 任务1-13:23");
        sender.send("站点" + site_id + " 任务1-55:21");
        sender.send("站点" + site_id + " 任务1-43:18");
        sender.send("站点" + site_id + " 任务1-81:18");
        sender.send("站点" + site_id + " 任务1-83:16");
        sender.send("站点" + site_id + " 任务1-54:10");
        sender.send("站点" + site_id + " 任务1-6:3");*/

        // 最差
        sender.send("站点" + site_id + " 任务1-6:3");
        sender.send("站点" + site_id + " 任务1-54:10");
        sender.send("站点" + site_id + " 任务1-83:16");
        sender.send("站点" + site_id + " 任务1-81:18");
        sender.send("站点" + site_id + " 任务1-43:18");
        sender.send("站点" + site_id + " 任务1-55:21");
        sender.send("站点" + site_id + " 任务1-13:23");
        sender.send("站点" + site_id + " 任务1-90:24");
        sender.send("站点" + site_id + " 任务1-91:25");
        sender.send("站点" + site_id + " 任务1-51:27");
        sender.send("站点" + site_id + " 任务1-48:27");
        sender.send("站点" + site_id + " 任务1-34:28");
        sender.send("站点" + site_id + " 任务1-87:28");
        sender.send("站点" + site_id + " 任务1-80:31");
        sender.send("站点" + site_id + " 任务1-84:32");
        sender.send("站点" + site_id + " 任务1-85:38");
        sender.send("站点" + site_id + " 任务1-61:38");
        sender.send("站点" + site_id + " 任务1-78:39");
        sender.send("站点" + site_id + " 任务1-77:42");
        sender.send("站点" + site_id + " 任务1-94:43");
        sender.send("站点" + site_id + " 任务1-7:43");
        sender.send("站点" + site_id + " 任务1-9:47");
        sender.send("站点" + site_id + " 任务1-56:52");
        sender.send("站点" + site_id + " 任务1-8:55");
        sender.send("站点" + site_id + " 任务1-38:57");
        sender.send("站点" + site_id + " 任务1-4:64");
        sender.send("站点" + site_id + " 任务1-18:64");
        sender.send("站点" + site_id + " 任务1-66:65");
        sender.send("站点" + site_id + " 任务1-2:66");
        sender.send("站点" + site_id + " 任务1-31:69");
        sender.send("站点" + site_id + " 任务1-95:75");
        sender.send("站点" + site_id + " 任务1-10:79");
        sender.send("站点" + site_id + " 任务1-64:81");
        sender.send("站点" + site_id + " 任务1-17:81");
        sender.send("站点" + site_id + " 任务1-39:82");
        sender.send("站点" + site_id + " 任务1-40:87");
        sender.send("站点" + site_id + " 任务1-11:88");
        sender.send("站点" + site_id + " 任务1-58:92");
        sender.send("站点" + site_id + " 任务1-57:93");
        sender.send("站点" + site_id + " 任务1-41:96");
        sender.send("站点" + site_id + " 任务1-45:102");
        sender.send("站点" + site_id + " 任务1-76:117");
        sender.send("站点" + site_id + " 任务1-15:118");
        sender.send("站点" + site_id + " 任务1-20:122");
        sender.send("站点" + site_id + " 任务1-42:124");
        sender.send("站点" + site_id + " 任务1-69:128");
        sender.send("站点" + site_id + " 任务1-46:128");
        sender.send("站点" + site_id + " 任务1-28:129");
        sender.send("站点" + site_id + " 任务1-12:137");
        sender.send("站点" + site_id + " 任务1-1:138");
        sender.send("站点" + site_id + " 任务1-88:146");
        sender.send("站点" + site_id + " 任务1-71:148");
        sender.send("站点" + site_id + " 任务1-23:154");
        sender.send("站点" + site_id + " 任务1-60:157");
        sender.send("站点" + site_id + " 任务1-52:161");
        sender.send("站点" + site_id + " 任务1-89:163");
        sender.send("站点" + site_id + " 任务1-19:163");
        sender.send("站点" + site_id + " 任务1-86:168");
        sender.send("站点" + site_id + " 任务1-32:174");
        sender.send("站点" + site_id + " 任务1-33:182");
        sender.send("站点" + site_id + " 任务1-93:216");
        sender.send("站点" + site_id + " 任务1-50:216");
        sender.send("站点" + site_id + " 任务1-72:222");
        sender.send("站点" + site_id + " 任务1-49:226");
        sender.send("站点" + site_id + " 任务1-26:239");
        sender.send("站点" + site_id + " 任务1-14:240");
        sender.send("站点" + site_id + " 任务1-16:240");
        sender.send("站点" + site_id + " 任务1-92:255");
        sender.send("站点" + site_id + " 任务1-59:273");
        sender.send("站点" + site_id + " 任务1-79:291");
        sender.send("站点" + site_id + " 任务1-27:295");
        sender.send("站点" + site_id + " 任务1-82:317");
        sender.send("站点" + site_id + " 任务1-67:354");
        sender.send("站点" + site_id + " 任务1-29:407");
        sender.send("站点" + site_id + " 任务1-3:426");
        sender.send("站点" + site_id + " 任务1-5:434");
        sender.send("站点" + site_id + " 任务1-24:445");
        sender.send("站点" + site_id + " 任务1-44:453");
        sender.send("站点" + site_id + " 任务1-68:459");
        sender.send("站点" + site_id + " 任务1-62:463");
        sender.send("站点" + site_id + " 任务1-36:465");
        sender.send("站点" + site_id + " 任务1-75:477");
        sender.send("站点" + site_id + " 任务1-25:486");
        sender.send("站点" + site_id + " 任务1-74:496");
        sender.send("站点" + site_id + " 任务1-73:497");
        sender.send("站点" + site_id + " 任务1-63:515");
        sender.send("站点" + site_id + " 任务1-37:524");
        sender.send("站点" + site_id + " 任务1-22:537");
        sender.send("站点" + site_id + " 任务1-70:604");
        sender.send("站点" + site_id + " 任务1-53:647");
        sender.send("站点" + site_id + " 任务1-65:725");
        sender.send("站点" + site_id + " 任务1-21:771");
        sender.send("站点" + site_id + " 任务1-47:910");
        sender.send("站点" + site_id + " 任务1-30:967");
        sender.send("站点" + site_id + " 任务1-35:1410");

        Thread.sleep(86400000);  // 86400000=1 hour
        //sender.send("任务2:");
    }

    public void run() {
        try {
            receiver.receive();
        } catch (IOException e) {
            e.printStackTrace();  //To change body of catch statement use File | Settings | File Templates.
        } catch (InterruptedException e) {
            e.printStackTrace();  //To change body of catch statement use File | Settings | File Templates.
        }
    }
}</pre>

## 结果截图

1. 最坏情况模拟（模拟数据）

<pre>[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
开始时间：1319520839578
[+] Sent message:'站点1194091 任务1:100'
[+] Sent message:'站点1194091 任务1:200'
[+] Sent message:'站点1194091 任务1:200'
[ ] 1319520839593 执行机3. Received :'站点1194091 任务1:200'
[ ] 1319520839593 执行机5. Received :'站点1194091 任务1:100'
[ ] 1319520839593 执行机9. Received :'站点1194091 任务1:300'
[+] Sent message:'站点1194091 任务1:300'
[ ] 1319520839593 执行机8. Received :'站点1194091 任务1:200'
[+] Sent message:'站点1194091 任务1:400'
[ ] 1319520839593 执行机10. Received :'站点1194091 任务1:400'
[ ] 1319520839593 执行机4. Received :'站点1194091 任务1:400'
[+] Sent message:'站点1194091 任务1:400'
[+] Sent message:'站点1194091 任务1:400'
[+] Sent message:'站点1194091 任务1:600'
[ ] 1319520839593 执行机2. Received :'站点1194091 任务1:400'
[+] Sent message:'站点1194091 任务1:600'
[+] Sent message:'站点1194091 任务1:600'
[ ] 1319520839593 执行机7. Received :'站点1194091 任务1:600'
[+] Sent message:'站点1194091 任务1:750'
[ ] 1319520839593 执行机1. Received :'站点1194091 任务1:600'
[ ] 1319520839593 执行机6. Received :'站点1194091 任务1:600'
[+] Sent message:'站点1194091 任务1:750'
[+] Sent message:'站点1194091 任务1:750'
[+] Sent message:'站点1194091 任务1:750'
[+] Sent message:'站点1194091 任务1:800'
[+] Sent message:'站点1194091 任务1:800'
[+] Sent message:'站点1194091 任务1:800'
[+] Sent message:'站点1194091 任务1:800'
[+] Sent message:'站点1194091 任务1:800'
[+] Sent message:'站点1194091 任务1:900'
[+] Sent message:'站点1194091 任务1:900'
[+] Sent message:'站点1194091 任务1:1000'
[+] Sent message:'站点1194091 任务1:1000'
[+] Sent message:'站点1194091 任务1:1200'
[+] Sent message:'站点1194091 任务1:1200'
[+] Sent message:'站点1194091 任务1:1300'
[+] Sent message:'站点1194091 任务1:1300'
[+] Sent message:'站点1194091 任务1:2000'
[+] Sent message:'站点1194091 任务1:2000'
[+] Sent message:'站点1194091 任务1:3000'
[-] 1319520842593 执行机5. Finished :'站点1194091 任务1:100'
[ ] 1319520842593 执行机5. Received :'站点1194091 任务1:750'
[-] 1319520845593 执行机3. Finished :'站点1194091 任务1:200'
[ ] 1319520845593 执行机3. Received :'站点1194091 任务1:750'
[-] 1319520845593 执行机8. Finished :'站点1194091 任务1:200'
[ ] 1319520845593 执行机8. Received :'站点1194091 任务1:750'
[-] 1319520848593 执行机9. Finished :'站点1194091 任务1:300'
[ ] 1319520848593 执行机9. Received :'站点1194091 任务1:750'
[-] 1319520851593 执行机4. Finished :'站点1194091 任务1:400'
[-] 1319520851593 执行机10. Finished :'站点1194091 任务1:400'
[-] 1319520851593 执行机2. Finished :'站点1194091 任务1:400'
[ ] 1319520851593 执行机4. Received :'站点1194091 任务1:800'
[ ] 1319520851593 执行机10. Received :'站点1194091 任务1:800'
[ ] 1319520851593 执行机2. Received :'站点1194091 任务1:800'
[-] 1319520857593 执行机7. Finished :'站点1194091 任务1:600'
[ ] 1319520857593 执行机7. Received :'站点1194091 任务1:800'
[-] 1319520857593 执行机6. Finished :'站点1194091 任务1:600'
[-] 1319520857593 执行机1. Finished :'站点1194091 任务1:600'
[ ] 1319520857593 执行机6. Received :'站点1194091 任务1:800'
[ ] 1319520857593 执行机1. Received :'站点1194091 任务1:900'
[-] 1319520865093 执行机5. Finished :'站点1194091 任务1:750'
[ ] 1319520865093 执行机5. Received :'站点1194091 任务1:900'
[-] 1319520868093 执行机3. Finished :'站点1194091 任务1:750'
[ ] 1319520868093 执行机3. Received :'站点1194091 任务1:1000'
[-] 1319520868093 执行机8. Finished :'站点1194091 任务1:750'
[ ] 1319520868093 执行机8. Received :'站点1194091 任务1:1000'
[-] 1319520871093 执行机9. Finished :'站点1194091 任务1:750'
[ ] 1319520871093 执行机9. Received :'站点1194091 任务1:1200'
[-] 1319520875593 执行机4. Finished :'站点1194091 任务1:800'
[-] 1319520875593 执行机10. Finished :'站点1194091 任务1:800'
[-] 1319520875593 执行机2. Finished :'站点1194091 任务1:800'
[ ] 1319520875593 执行机10. Received :'站点1194091 任务1:1300'
[ ] 1319520875593 执行机4. Received :'站点1194091 任务1:1200'
[ ] 1319520875593 执行机2. Received :'站点1194091 任务1:1300'
[-] 1319520881593 执行机7. Finished :'站点1194091 任务1:800'
[ ] 1319520881593 执行机7. Received :'站点1194091 任务1:2000'
[-] 1319520881593 执行机6. Finished :'站点1194091 任务1:800'
[ ] 1319520881593 执行机6. Received :'站点1194091 任务1:2000'
[-] 1319520884609 执行机1. Finished :'站点1194091 任务1:900'
[ ] 1319520884609 执行机1. Received :'站点1194091 任务1:3000'
[-] 1319520892093 执行机5. Finished :'站点1194091 任务1:900'
[-] 1319520898093 执行机3. Finished :'站点1194091 任务1:1000'
[-] 1319520898093 执行机8. Finished :'站点1194091 任务1:1000'
[-] 1319520907093 执行机9. Finished :'站点1194091 任务1:1200'
[-] 1319520911593 执行机4. Finished :'站点1194091 任务1:1200'
[-] 1319520914593 执行机10. Finished :'站点1194091 任务1:1300'
[-] 1319520914593 执行机2. Finished :'站点1194091 任务1:1300'
[-] 1319520941593 执行机7. Finished :'站点1194091 任务1:2000'
[-] 1319520941609 执行机6. Finished :'站点1194091 任务1:2000'
[-] 1319520974609 执行机1. Finished :'站点1194091 任务1:3000'</pre>

2. 原有策略模拟（模拟数据）

<pre>[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
开始时间：1319114460250
[+] Sent message:'站点434443 任务1-1:100'
[+] Sent message:'站点434443 任务1-2:800'
[+] Sent message:'站点434443 任务1-3:200'
[+] Sent message:'站点434443 任务1-4:1200'
[+] Sent message:'站点434443 任务1-5:750'
[ ] 1319114460250 执行机3. Received :'站点434443 任务1-1:100'
[ ] 1319114460250 执行机7. Received :'站点434443 任务1-5:750'
[ ] 1319114460250 执行机10. Received :'站点434443 任务1-4:1200'
[ ] 1319114460250 执行机5. Received :'站点434443 任务1-6:1300'
[+] Sent message:'站点434443 任务1-6:1300'
[ ] 1319114460250 执行机6. Received :'站点434443 任务1-3:200'
[ ] 1319114460250 执行机9. Received :'站点434443 任务1-2:800'
[ ] 1319114460250 执行机8. Received :'站点434443 任务1-7:800'
[+] Sent message:'站点434443 任务1-7:800'
[+] Sent message:'站点434443 任务1-8:400'
[ ] 1319114460250 执行机1. Received :'站点434443 任务1-8:400'
[ ] 1319114460250 执行机4. Received :'站点434443 任务1-9:600'
[+] Sent message:'站点434443 任务1-9:600'
[+] Sent message:'站点434443 任务1-10:750'
[ ] 1319114460250 执行机2. Received :'站点434443 任务1-10:750'
[+] Sent message:'站点434443 任务1-11:2000'
[+] Sent message:'站点434443 任务1-12:1000'
[+] Sent message:'站点434443 任务1-13:300'
[+] Sent message:'站点434443 任务1-14:900'
[+] Sent message:'站点434443 任务1-15:1200'
[+] Sent message:'站点434443 任务1-16:750'
[+] Sent message:'站点434443 任务1-17:1300'
[+] Sent message:'站点434443 任务1-18:800'
[+] Sent message:'站点434443 任务1-19:400'
[+] Sent message:'站点434443 任务1-20:600'
[+] Sent message:'站点434443 任务1-21:200'
[+] Sent message:'站点434443 任务1-22:3000'
[+] Sent message:'站点434443 任务1-23:1000'
[+] Sent message:'站点434443 任务1-24:800'
[+] Sent message:'站点434443 任务1-25:900'
[+] Sent message:'站点434443 任务1-26:800'
[+] Sent message:'站点434443 任务1-27:400'
[+] Sent message:'站点434443 任务1-28:600'
[+] Sent message:'站点434443 任务1-29:750'
[+] Sent message:'站点434443 任务1-30:2000'
[-] 1319114463250 执行机3. Finished :'站点434443 任务1-1:100'
[ ] 1319114463250 执行机3. Received :'站点434443 任务1-11:2000'
[-] 1319114466250 执行机6. Finished :'站点434443 任务1-3:200'
[ ] 1319114466250 执行机6. Received :'站点434443 任务1-12:1000'
[-] 1319114472250 执行机1. Finished :'站点434443 任务1-8:400'
[ ] 1319114472250 执行机1. Received :'站点434443 任务1-13:300'
[-] 1319114478250 执行机4. Finished :'站点434443 任务1-9:600'
[ ] 1319114478250 执行机4. Received :'站点434443 任务1-14:900'
[-] 1319114481250 执行机1. Finished :'站点434443 任务1-13:300'
[ ] 1319114481250 执行机1. Received :'站点434443 任务1-15:1200'
[-] 1319114482750 执行机7. Finished :'站点434443 任务1-5:750'
[ ] 1319114482750 执行机7. Received :'站点434443 任务1-16:750'
[-] 1319114482750 执行机2. Finished :'站点434443 任务1-10:750'
[ ] 1319114482750 执行机2. Received :'站点434443 任务1-17:1300'
[-] 1319114484250 执行机9. Finished :'站点434443 任务1-2:800'
[ ] 1319114484250 执行机9. Received :'站点434443 任务1-18:800'
[-] 1319114484250 执行机8. Finished :'站点434443 任务1-7:800'
[ ] 1319114484250 执行机8. Received :'站点434443 任务1-19:400'
[-] 1319114496250 执行机10. Finished :'站点434443 任务1-4:1200'
[ ] 1319114496250 执行机10. Received :'站点434443 任务1-20:600'
[-] 1319114496250 执行机6. Finished :'站点434443 任务1-12:1000'
[ ] 1319114496250 执行机6. Received :'站点434443 任务1-21:200'
[-] 1319114496250 执行机8. Finished :'站点434443 任务1-19:400'
[ ] 1319114496250 执行机8. Received :'站点434443 任务1-22:3000'
[-] 1319114499250 执行机5. Finished :'站点434443 任务1-6:1300'
[ ] 1319114499250 执行机5. Received :'站点434443 任务1-23:1000'
[-] 1319114502250 执行机6. Finished :'站点434443 任务1-21:200'
[ ] 1319114502250 执行机6. Received :'站点434443 任务1-24:800'
[-] 1319114505250 执行机7. Finished :'站点434443 任务1-16:750'
[ ] 1319114505250 执行机7. Received :'站点434443 任务1-25:900'
[-] 1319114505250 执行机4. Finished :'站点434443 任务1-14:900'
[ ] 1319114505250 执行机4. Received :'站点434443 任务1-26:800'
[-] 1319114508250 执行机9. Finished :'站点434443 任务1-18:800'
[ ] 1319114508250 执行机9. Received :'站点434443 任务1-27:400'
[-] 1319114514250 执行机10. Finished :'站点434443 任务1-20:600'
[ ] 1319114514250 执行机10. Received :'站点434443 任务1-28:600'
[-] 1319114517250 执行机1. Finished :'站点434443 任务1-15:1200'
[ ] 1319114517265 执行机1. Received :'站点434443 任务1-29:750'
[-] 1319114520250 执行机9. Finished :'站点434443 任务1-27:400'
[ ] 1319114520250 执行机9. Received :'站点434443 任务1-30:2000'
[-] 1319114521765 执行机2. Finished :'站点434443 任务1-17:1300'
[-] 1319114523250 执行机3. Finished :'站点434443 任务1-11:2000'
[-] 1319114526250 执行机6. Finished :'站点434443 任务1-24:800'
[-] 1319114529250 执行机5. Finished :'站点434443 任务1-23:1000'
[-] 1319114529250 执行机4. Finished :'站点434443 任务1-26:800'
[-] 1319114532250 执行机10. Finished :'站点434443 任务1-28:600'
[-] 1319114532250 执行机7. Finished :'站点434443 任务1-25:900'
[-] 1319114539765 执行机1. Finished :'站点434443 任务1-29:750'
[-] 1319114580250 执行机9. Finished :'站点434443 任务1-30:2000'
[-] 1319114586250 执行机8. Finished :'站点434443 任务1-22:3000'</pre>

3. 改进策略模拟（模拟数据）

<pre>[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
开始时间：1319114973375
[+] Sent message:'站点887048 任务1:3000'
[+] Sent message:'站点887048 任务1:2000'
[+] Sent message:'站点887048 任务1:2000'
[ ] 1319114973375 执行机4. Received :'站点887048 任务1:3000'
[ ] 1319114973390 执行机7. Received :'站点887048 任务1:1300'
[ ] 1319114973375 执行机8. Received :'站点887048 任务1:2000'
[ ] 1319114973390 执行机10. Received :'站点887048 任务1:2000'
[+] Sent message:'站点887048 任务1:1300'
[+] Sent message:'站点887048 任务1:1300'
[+] Sent message:'站点887048 任务1:1200'
[+] Sent message:'站点887048 任务1:1200'
[+] Sent message:'站点887048 任务1:1000'
[ ] 1319114973390 执行机9. Received :'站点887048 任务1:1200'
[ ] 1319114973390 执行机2. Received :'站点887048 任务1:1200'
[ ] 1319114973390 执行机6. Received :'站点887048 任务1:1300'
[ ] 1319114973390 执行机3. Received :'站点887048 任务1:1000'
[ ] 1319114973390 执行机5. Received :'站点887048 任务1:1000'
[+] Sent message:'站点887048 任务1:1000'
[+] Sent message:'站点887048 任务1:900'
[+] Sent message:'站点887048 任务1:900'
[+] Sent message:'站点887048 任务1:800'
[ ] 1319114973390 执行机1. Received :'站点887048 任务1:900'
[+] Sent message:'站点887048 任务1:800'
[+] Sent message:'站点887048 任务1:800'
[+] Sent message:'站点887048 任务1:800'
[+] Sent message:'站点887048 任务1:800'
[+] Sent message:'站点887048 任务1:750'
[+] Sent message:'站点887048 任务1:750'
[+] Sent message:'站点887048 任务1:750'
[+] Sent message:'站点887048 任务1:750'
[+] Sent message:'站点887048 任务1:600'
[+] Sent message:'站点887048 任务1:600'
[+] Sent message:'站点887048 任务1:600'
[+] Sent message:'站点887048 任务1:400'
[+] Sent message:'站点887048 任务1:400'
[+] Sent message:'站点887048 任务1:400'
[+] Sent message:'站点887048 任务1:300'
[+] Sent message:'站点887048 任务1:200'
[+] Sent message:'站点887048 任务1:200'
[+] Sent message:'站点887048 任务1:100'
[-] 1319115000390 执行机1. Finished :'站点887048 任务1:900'
[ ] 1319115000390 执行机1. Received :'站点887048 任务1:900'
[-] 1319115003390 执行机3. Finished :'站点887048 任务1:1000'
[-] 1319115003390 执行机5. Finished :'站点887048 任务1:1000'
[ ] 1319115003390 执行机3. Received :'站点887048 任务1:800'
[ ] 1319115003390 执行机5. Received :'站点887048 任务1:800'
[-] 1319115009390 执行机9. Finished :'站点887048 任务1:1200'
[-] 1319115009390 执行机2. Finished :'站点887048 任务1:1200'
[ ] 1319115009390 执行机9. Received :'站点887048 任务1:800'
[ ] 1319115009390 执行机2. Received :'站点887048 任务1:800'
[-] 1319115012390 执行机7. Finished :'站点887048 任务1:1300'
[ ] 1319115012390 执行机7. Received :'站点887048 任务1:800'
[-] 1319115012390 执行机6. Finished :'站点887048 任务1:1300'
[ ] 1319115012390 执行机6. Received :'站点887048 任务1:750'
[-] 1319115027390 执行机5. Finished :'站点887048 任务1:800'
[-] 1319115027390 执行机3. Finished :'站点887048 任务1:800'
[ ] 1319115027390 执行机5. Received :'站点887048 任务1:750'
[ ] 1319115027390 执行机3. Received :'站点887048 任务1:750'
[-] 1319115027390 执行机1. Finished :'站点887048 任务1:900'
[ ] 1319115027390 执行机1. Received :'站点887048 任务1:750'
[-] 1319115033390 执行机8. Finished :'站点887048 任务1:2000'
[-] 1319115033390 执行机10. Finished :'站点887048 任务1:2000'
[ ] 1319115033390 执行机8. Received :'站点887048 任务1:600'
[ ] 1319115033390 执行机10. Received :'站点887048 任务1:600'
[-] 1319115033390 执行机9. Finished :'站点887048 任务1:800'
[-] 1319115033390 执行机2. Finished :'站点887048 任务1:800'
[ ] 1319115033390 执行机9. Received :'站点887048 任务1:600'
[ ] 1319115033390 执行机2. Received :'站点887048 任务1:400'
[-] 1319115034890 执行机6. Finished :'站点887048 任务1:750'
[ ] 1319115034890 执行机6. Received :'站点887048 任务1:400'
[-] 1319115036390 执行机7. Finished :'站点887048 任务1:800'
[ ] 1319115036390 执行机7. Received :'站点887048 任务1:400'
[-] 1319115045390 执行机2. Finished :'站点887048 任务1:400'
[ ] 1319115045390 执行机2. Received :'站点887048 任务1:300'
[-] 1319115046890 执行机6. Finished :'站点887048 任务1:400'
[ ] 1319115046890 执行机6. Received :'站点887048 任务1:200'
[-] 1319115048390 执行机7. Finished :'站点887048 任务1:400'
[ ] 1319115048390 执行机7. Received :'站点887048 任务1:200'
[-] 1319115049890 执行机5. Finished :'站点887048 任务1:750'
[-] 1319115049890 执行机3. Finished :'站点887048 任务1:750'
[ ] 1319115049890 执行机5. Received :'站点887048 任务1:100'
[-] 1319115049890 执行机1. Finished :'站点887048 任务1:750'
[-] 1319115051390 执行机8. Finished :'站点887048 任务1:600'
[-] 1319115051390 执行机10. Finished :'站点887048 任务1:600'
[-] 1319115051390 执行机9. Finished :'站点887048 任务1:600'
[-] 1319115052890 执行机6. Finished :'站点887048 任务1:200'
[-] 1319115052890 执行机5. Finished :'站点887048 任务1:100'
[-] 1319115054390 执行机2. Finished :'站点887048 任务1:300'
[-] 1319115054390 执行机7. Finished :'站点887048 任务1:200'
[-] 1319115063390 执行机4. Finished :'站点887048 任务1:3000'</pre>

4. 原有策略模拟（实际数据13台执行机）

<pre>[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
[*] Waiting for messages. To exit press CTRL+C
开始时间：1319518250031
[+] Sent message:'站点1117421 任务1-1:138'
[+] Sent message:'站点1117421 任务1-2:66'
[ ] 1319518250046 执行机7. Received :'站点1117421 任务1-1:138'
[ ] 1319518250046 执行机4. Received :'站点1117421 任务1-2:66'
[+] Sent message:'站点1117421 任务1-3:426'
[ ] 1319518250046 执行机1. Received :'站点1117421 任务1-3:426'
[ ] 1319518250046 执行机3. Received :'站点1117421 任务1-4:64'
[+] Sent message:'站点1117421 任务1-4:64'
[+] Sent message:'站点1117421 任务1-5:434'
[ ] 1319518250046 执行机6. Received :'站点1117421 任务1-5:434'
[ ] 1319518250046 执行机10. Received :'站点1117421 任务1-6:3'
[+] Sent message:'站点1117421 任务1-6:3'
[+] Sent message:'站点1117421 任务1-7:43'
[+] Sent message:'站点1117421 任务1-8:55'
[ ] 1319518250046 执行机8. Received :'站点1117421 任务1-8:55'
[ ] 1319518250046 执行机12. Received :'站点1117421 任务1-7:43'
[+] Sent message:'站点1117421 任务1-9:47'
[ ] 1319518250046 执行机5. Received :'站点1117421 任务1-9:47'
[+] Sent message:'站点1117421 任务1-10:79'
[+] Sent message:'站点1117421 任务1-11:88'
[ ] 1319518250046 执行机13. Received :'站点1117421 任务1-10:79'
[ ] 1319518250046 执行机2. Received :'站点1117421 任务1-11:88'
[+] Sent message:'站点1117421 任务1-12:137'
[ ] 1319518250062 执行机9. Received :'站点1117421 任务1-12:137'
[ ] 1319518250062 执行机11. Received :'站点1117421 任务1-13:23'
[+] Sent message:'站点1117421 任务1-13:23'
[+] Sent message:'站点1117421 任务1-14:240'
[+] Sent message:'站点1117421 任务1-15:118'
[+] Sent message:'站点1117421 任务1-16:240'
[+] Sent message:'站点1117421 任务1-17:81'
[+] Sent message:'站点1117421 任务1-18:64'
[+] Sent message:'站点1117421 任务1-19:163'
[+] Sent message:'站点1117421 任务1-20:122'
[+] Sent message:'站点1117421 任务1-21:771'
[+] Sent message:'站点1117421 任务1-22:537'
[+] Sent message:'站点1117421 任务1-23:154'
[+] Sent message:'站点1117421 任务1-24:445'
[+] Sent message:'站点1117421 任务1-25:486'
[+] Sent message:'站点1117421 任务1-26:239'
[+] Sent message:'站点1117421 任务1-27:295'
[+] Sent message:'站点1117421 任务1-28:129'
[+] Sent message:'站点1117421 任务1-29:407'
[+] Sent message:'站点1117421 任务1-30:967'
[+] Sent message:'站点1117421 任务1-31:69'
[+] Sent message:'站点1117421 任务1-32:174'
[+] Sent message:'站点1117421 任务1-33:182'
[+] Sent message:'站点1117421 任务1-34:28'
[+] Sent message:'站点1117421 任务1-35:1410'
[+] Sent message:'站点1117421 任务1-36:465'
[+] Sent message:'站点1117421 任务1-37:524'
[+] Sent message:'站点1117421 任务1-38:57'
[+] Sent message:'站点1117421 任务1-39:82'
[+] Sent message:'站点1117421 任务1-40:87'
[+] Sent message:'站点1117421 任务1-41:96'
[+] Sent message:'站点1117421 任务1-42:124'
[+] Sent message:'站点1117421 任务1-43:18'
[+] Sent message:'站点1117421 任务1-44:453'
[+] Sent message:'站点1117421 任务1-45:102'
[+] Sent message:'站点1117421 任务1-46:128'
[+] Sent message:'站点1117421 任务1-47:910'
[+] Sent message:'站点1117421 任务1-48:27'
[+] Sent message:'站点1117421 任务1-49:226'
[+] Sent message:'站点1117421 任务1-50:216'
[+] Sent message:'站点1117421 任务1-51:27'
[+] Sent message:'站点1117421 任务1-52:161'
[+] Sent message:'站点1117421 任务1-53:647'
[+] Sent message:'站点1117421 任务1-54:10'
[+] Sent message:'站点1117421 任务1-55:21'
[+] Sent message:'站点1117421 任务1-56:52'
[+] Sent message:'站点1117421 任务1-57:93'
[+] Sent message:'站点1117421 任务1-58:92'
[+] Sent message:'站点1117421 任务1-59:273'
[+] Sent message:'站点1117421 任务1-60:157'
[+] Sent message:'站点1117421 任务1-61:38'
[+] Sent message:'站点1117421 任务1-62:463'
[+] Sent message:'站点1117421 任务1-63:515'
[+] Sent message:'站点1117421 任务1-64:81'
[+] Sent message:'站点1117421 任务1-65:725'
[+] Sent message:'站点1117421 任务1-66:65'
[+] Sent message:'站点1117421 任务1-67:354'
[+] Sent message:'站点1117421 任务1-68:459'
[+] Sent message:'站点1117421 任务1-69:128'
[+] Sent message:'站点1117421 任务1-70:604'
[+] Sent message:'站点1117421 任务1-71:148'
[+] Sent message:'站点1117421 任务1-72:222'
[+] Sent message:'站点1117421 任务1-73:497'
[+] Sent message:'站点1117421 任务1-74:496'
[+] Sent message:'站点1117421 任务1-75:477'
[+] Sent message:'站点1117421 任务1-76:117'
[+] Sent message:'站点1117421 任务1-77:42'
[+] Sent message:'站点1117421 任务1-78:39'
[+] Sent message:'站点1117421 任务1-79:291'
[+] Sent message:'站点1117421 任务1-80:31'
[+] Sent message:'站点1117421 任务1-81:18'
[+] Sent message:'站点1117421 任务1-82:317'
[+] Sent message:'站点1117421 任务1-83:16'
[+] Sent message:'站点1117421 任务1-84:32'
[+] Sent message:'站点1117421 任务1-85:38'
[+] Sent message:'站点1117421 任务1-86:168'
[+] Sent message:'站点1117421 任务1-87:28'
[+] Sent message:'站点1117421 任务1-88:146'
[+] Sent message:'站点1117421 任务1-89:163'
[+] Sent message:'站点1117421 任务1-90:24'
[+] Sent message:'站点1117421 任务1-91:25'
[+] Sent message:'站点1117421 任务1-92:255'
[+] Sent message:'站点1117421 任务1-93:216'
[+] Sent message:'站点1117421 任务1-94:43'
[+] Sent message:'站点1117421 任务1-95:75'
[-] 1319518253046 执行机10. Finished :'站点1117421 任务1-6:3'
[ ] 1319518253046 执行机10. Received :'站点1117421 任务1-14:240'
[-] 1319518273062 执行机11. Finished :'站点1117421 任务1-13:23'
[ ] 1319518273062 执行机11. Received :'站点1117421 任务1-15:118'
[-] 1319518293046 执行机12. Finished :'站点1117421 任务1-7:43'
[ ] 1319518293046 执行机12. Received :'站点1117421 任务1-16:240'
[-] 1319518297046 执行机5. Finished :'站点1117421 任务1-9:47'
[ ] 1319518297046 执行机5. Received :'站点1117421 任务1-17:81'
[-] 1319518305046 执行机8. Finished :'站点1117421 任务1-8:55'
[ ] 1319518305046 执行机8. Received :'站点1117421 任务1-18:64'
[-] 1319518314046 执行机3. Finished :'站点1117421 任务1-4:64'
[ ] 1319518314046 执行机3. Received :'站点1117421 任务1-19:163'
[-] 1319518316046 执行机4. Finished :'站点1117421 任务1-2:66'
[ ] 1319518316046 执行机4. Received :'站点1117421 任务1-20:122'
[-] 1319518329062 执行机13. Finished :'站点1117421 任务1-10:79'
[ ] 1319518329062 执行机13. Received :'站点1117421 任务1-21:771'
[-] 1319518338062 执行机2. Finished :'站点1117421 任务1-11:88'
[ ] 1319518338062 执行机2. Received :'站点1117421 任务1-22:537'
[-] 1319518369046 执行机8. Finished :'站点1117421 任务1-18:64'
[ ] 1319518369046 执行机8. Received :'站点1117421 任务1-23:154'
[-] 1319518378046 执行机5. Finished :'站点1117421 任务1-17:81'
[ ] 1319518378062 执行机5. Received :'站点1117421 任务1-24:445'
[-] 1319518387062 执行机9. Finished :'站点1117421 任务1-12:137'
[ ] 1319518387062 执行机9. Received :'站点1117421 任务1-25:486'
[-] 1319518388046 执行机7. Finished :'站点1117421 任务1-1:138'
[ ] 1319518388046 执行机7. Received :'站点1117421 任务1-26:239'
[-] 1319518391062 执行机11. Finished :'站点1117421 任务1-15:118'
[ ] 1319518391062 执行机11. Received :'站点1117421 任务1-27:295'
[-] 1319518438046 执行机4. Finished :'站点1117421 任务1-20:122'
[ ] 1319518438046 执行机4. Received :'站点1117421 任务1-28:129'
[-] 1319518477046 执行机3. Finished :'站点1117421 任务1-19:163'
[ ] 1319518477046 执行机3. Received :'站点1117421 任务1-29:407'
[-] 1319518493046 执行机10. Finished :'站点1117421 任务1-14:240'
[ ] 1319518493046 执行机10. Received :'站点1117421 任务1-30:967'
[-] 1319518523046 执行机8. Finished :'站点1117421 任务1-23:154'
[ ] 1319518523062 执行机8. Received :'站点1117421 任务1-31:69'
[-] 1319518533046 执行机12. Finished :'站点1117421 任务1-16:240'
[ ] 1319518533046 执行机12. Received :'站点1117421 任务1-32:174'
[-] 1319518567046 执行机4. Finished :'站点1117421 任务1-28:129'
[ ] 1319518567046 执行机4. Received :'站点1117421 任务1-33:182'
[-] 1319518592062 执行机8. Finished :'站点1117421 任务1-31:69'
[ ] 1319518592062 执行机8. Received :'站点1117421 任务1-34:28'
[-] 1319518620062 执行机8. Finished :'站点1117421 任务1-34:28'
[ ] 1319518620062 执行机8. Received :'站点1117421 任务1-35:1410'
[-] 1319518627046 执行机7. Finished :'站点1117421 任务1-26:239'
[ ] 1319518627046 执行机7. Received :'站点1117421 任务1-36:465'
[-] 1319518676046 执行机1. Finished :'站点1117421 任务1-3:426'
[ ] 1319518676046 执行机1. Received :'站点1117421 任务1-37:524'
[-] 1319518684046 执行机6. Finished :'站点1117421 任务1-5:434'
[ ] 1319518684046 执行机6. Received :'站点1117421 任务1-38:57'
[-] 1319518686062 执行机11. Finished :'站点1117421 任务1-27:295'
[ ] 1319518686062 执行机11. Received :'站点1117421 任务1-39:82'
[-] 1319518707046 执行机12. Finished :'站点1117421 任务1-32:174'
[ ] 1319518707062 执行机12. Received :'站点1117421 任务1-40:87'
[-] 1319518741046 执行机6. Finished :'站点1117421 任务1-38:57'
[ ] 1319518741046 执行机6. Received :'站点1117421 任务1-41:96'
[-] 1319518749046 执行机4. Finished :'站点1117421 任务1-33:182'
[ ] 1319518749046 执行机4. Received :'站点1117421 任务1-42:124'
[-] 1319518768062 执行机11. Finished :'站点1117421 任务1-39:82'
[ ] 1319518768062 执行机11. Received :'站点1117421 任务1-43:18'
[-] 1319518786062 执行机11. Finished :'站点1117421 任务1-43:18'
[ ] 1319518786062 执行机11. Received :'站点1117421 任务1-44:453'
[-] 1319518794062 执行机12. Finished :'站点1117421 任务1-40:87'
[ ] 1319518794062 执行机12. Received :'站点1117421 任务1-45:102'
[-] 1319518823062 执行机5. Finished :'站点1117421 任务1-24:445'
[ ] 1319518823062 执行机5. Received :'站点1117421 任务1-46:128'
[-] 1319518837046 执行机6. Finished :'站点1117421 任务1-41:96'
[ ] 1319518837046 执行机6. Received :'站点1117421 任务1-47:910'
[-] 1319518873046 执行机4. Finished :'站点1117421 任务1-42:124'
[ ] 1319518873046 执行机4. Received :'站点1117421 任务1-48:27'
[-] 1319518873062 执行机9. Finished :'站点1117421 任务1-25:486'
[ ] 1319518873062 执行机9. Received :'站点1117421 任务1-49:226'
[-] 1319518875062 执行机2. Finished :'站点1117421 任务1-22:537'
[ ] 1319518875062 执行机2. Received :'站点1117421 任务1-50:216'
[-] 1319518884046 执行机3. Finished :'站点1117421 任务1-29:407'
[ ] 1319518884046 执行机3. Received :'站点1117421 任务1-51:27'
[-] 1319518896062 执行机12. Finished :'站点1117421 任务1-45:102'
[ ] 1319518896062 执行机12. Received :'站点1117421 任务1-52:161'
[-] 1319518900046 执行机4. Finished :'站点1117421 任务1-48:27'
[ ] 1319518900046 执行机4. Received :'站点1117421 任务1-53:647'
[-] 1319518911046 执行机3. Finished :'站点1117421 任务1-51:27'
[ ] 1319518911046 执行机3. Received :'站点1117421 任务1-54:10'
[-] 1319518921046 执行机3. Finished :'站点1117421 任务1-54:10'
[ ] 1319518921046 执行机3. Received :'站点1117421 任务1-55:21'
[-] 1319518942046 执行机3. Finished :'站点1117421 任务1-55:21'
[ ] 1319518942046 执行机3. Received :'站点1117421 任务1-56:52'
[-] 1319518951062 执行机5. Finished :'站点1117421 任务1-46:128'
[ ] 1319518951062 执行机5. Received :'站点1117421 任务1-57:93'
[-] 1319518994046 执行机3. Finished :'站点1117421 任务1-56:52'
[ ] 1319518994062 执行机3. Received :'站点1117421 任务1-58:92'
[-] 1319519044062 执行机5. Finished :'站点1117421 任务1-57:93'
[ ] 1319519044062 执行机5. Received :'站点1117421 任务1-59:273'
[-] 1319519057062 执行机12. Finished :'站点1117421 任务1-52:161'
[ ] 1319519057062 执行机12. Received :'站点1117421 任务1-60:157'
[-] 1319519086062 执行机3. Finished :'站点1117421 任务1-58:92'
[ ] 1319519086062 执行机3. Received :'站点1117421 任务1-61:38'
[-] 1319519091062 执行机2. Finished :'站点1117421 任务1-50:216'
[ ] 1319519091062 执行机2. Received :'站点1117421 任务1-62:463'
[-] 1319519092046 执行机7. Finished :'站点1117421 任务1-36:465'
[ ] 1319519092046 执行机7. Received :'站点1117421 任务1-63:515'
[-] 1319519099062 执行机9. Finished :'站点1117421 任务1-49:226'
[ ] 1319519099062 执行机9. Received :'站点1117421 任务1-64:81'
[-] 1319519100062 执行机13. Finished :'站点1117421 任务1-21:771'
[ ] 1319519100062 执行机13. Received :'站点1117421 任务1-65:725'
[-] 1319519124062 执行机3. Finished :'站点1117421 任务1-61:38'
[ ] 1319519124062 执行机3. Received :'站点1117421 任务1-66:65'
[-] 1319519180062 执行机9. Finished :'站点1117421 任务1-64:81'
[ ] 1319519180062 执行机9. Received :'站点1117421 任务1-67:354'
[-] 1319519189062 执行机3. Finished :'站点1117421 任务1-66:65'
[ ] 1319519189062 执行机3. Received :'站点1117421 任务1-68:459'
[-] 1319519200046 执行机1. Finished :'站点1117421 任务1-37:524'
[ ] 1319519200046 执行机1. Received :'站点1117421 任务1-69:128'
[-] 1319519214062 执行机12. Finished :'站点1117421 任务1-60:157'
[ ] 1319519214062 执行机12. Received :'站点1117421 任务1-70:604'
[-] 1319519239062 执行机11. Finished :'站点1117421 任务1-44:453'
[ ] 1319519239062 执行机11. Received :'站点1117421 任务1-71:148'
[-] 1319519317062 执行机5. Finished :'站点1117421 任务1-59:273'
[ ] 1319519317062 执行机5. Received :'站点1117421 任务1-72:222'
[-] 1319519328046 执行机1. Finished :'站点1117421 任务1-69:128'
[ ] 1319519328046 执行机1. Received :'站点1117421 任务1-73:497'
[-] 1319519387062 执行机11. Finished :'站点1117421 任务1-71:148'
[ ] 1319519387062 执行机11. Received :'站点1117421 任务1-74:496'
[-] 1319519460046 执行机10. Finished :'站点1117421 任务1-30:967'
[ ] 1319519460046 执行机10. Received :'站点1117421 任务1-75:477'
[-] 1319519534062 执行机9. Finished :'站点1117421 任务1-67:354'
[ ] 1319519534062 执行机9. Received :'站点1117421 任务1-76:117'
[-] 1319519539062 执行机5. Finished :'站点1117421 任务1-72:222'
[ ] 1319519539062 执行机5. Received :'站点1117421 任务1-77:42'
[-] 1319519547046 执行机4. Finished :'站点1117421 任务1-53:647'
[ ] 1319519547046 执行机4. Received :'站点1117421 任务1-78:39'
[-] 1319519554062 执行机2. Finished :'站点1117421 任务1-62:463'
[ ] 1319519554062 执行机2. Received :'站点1117421 任务1-79:291'
[-] 1319519581062 执行机5. Finished :'站点1117421 任务1-77:42'
[ ] 1319519581062 执行机5. Received :'站点1117421 任务1-80:31'
[-] 1319519586046 执行机4. Finished :'站点1117421 任务1-78:39'
[ ] 1319519586046 执行机4. Received :'站点1117421 任务1-81:18'
[-] 1319519604046 执行机4. Finished :'站点1117421 任务1-81:18'
[ ] 1319519604062 执行机4. Received :'站点1117421 任务1-82:317'
[-] 1319519607046 执行机7. Finished :'站点1117421 任务1-63:515'
[ ] 1319519607046 执行机7. Received :'站点1117421 任务1-83:16'
[-] 1319519612062 执行机5. Finished :'站点1117421 任务1-80:31'
[ ] 1319519612062 执行机5. Received :'站点1117421 任务1-84:32'
[-] 1319519623046 执行机7. Finished :'站点1117421 任务1-83:16'
[ ] 1319519623046 执行机7. Received :'站点1117421 任务1-85:38'
[-] 1319519644062 执行机5. Finished :'站点1117421 任务1-84:32'
[ ] 1319519644062 执行机5. Received :'站点1117421 任务1-86:168'
[-] 1319519648062 执行机3. Finished :'站点1117421 任务1-68:459'
[ ] 1319519648062 执行机3. Received :'站点1117421 任务1-87:28'
[-] 1319519651062 执行机9. Finished :'站点1117421 任务1-76:117'
[ ] 1319519651062 执行机9. Received :'站点1117421 任务1-88:146'
[-] 1319519661046 执行机7. Finished :'站点1117421 任务1-85:38'
[ ] 1319519661046 执行机7. Received :'站点1117421 任务1-89:163'
[-] 1319519676062 执行机3. Finished :'站点1117421 任务1-87:28'
[ ] 1319519676062 执行机3. Received :'站点1117421 任务1-90:24'
[-] 1319519700062 执行机3. Finished :'站点1117421 任务1-90:24'
[ ] 1319519700062 执行机3. Received :'站点1117421 任务1-91:25'
[-] 1319519725062 执行机3. Finished :'站点1117421 任务1-91:25'
[ ] 1319519725062 执行机3. Received :'站点1117421 任务1-92:255'
[-] 1319519747046 执行机6. Finished :'站点1117421 任务1-47:910'
[ ] 1319519747046 执行机6. Received :'站点1117421 任务1-93:216'
[-] 1319519797062 执行机9. Finished :'站点1117421 任务1-88:146'
[ ] 1319519797078 执行机9. Received :'站点1117421 任务1-94:43'
[-] 1319519812062 执行机5. Finished :'站点1117421 任务1-86:168'
[ ] 1319519812062 执行机5. Received :'站点1117421 任务1-95:75'
[-] 1319519818062 执行机12. Finished :'站点1117421 任务1-70:604'
[-] 1319519824046 执行机7. Finished :'站点1117421 任务1-89:163'
[-] 1319519825046 执行机1. Finished :'站点1117421 任务1-73:497'
[-] 1319519825062 执行机13. Finished :'站点1117421 任务1-65:725'
[-] 1319519840078 执行机9. Finished :'站点1117421 任务1-94:43'
[-] 1319519845062 执行机2. Finished :'站点1117421 任务1-79:291'
[-] 1319519883062 执行机11. Finished :'站点1117421 任务1-74:496'
[-] 1319519887062 执行机5. Finished :'站点1117421 任务1-95:75'
[-] 1319519921062 执行机4. Finished :'站点1117421 任务1-82:317'
[-] 1319519937046 执行机10. Finished :'站点1117421 任务1-75:477'
[-] 1319519963046 执行机6. Finished :'站点1117421 任务1-93:216'
[-] 1319519980062 执行机3. Finished :'站点1117421 任务1-92:255'
[-] 1319520030062 执行机8. Finished :'站点1117421 任务1-35:1410'</pre>

5. 改进策略模拟（实际数据13台执行机）

6. 原有策略模拟（实际数据20台执行机）

7. 改进策略模拟（实际数据20台执行机）

## 结果对比

对比项目          模拟数据13台执行机    实际数据13台执行机    实际数据20台执行机

A.最坏情况模拟    135031ms              2566015ms(42min46s)   ？ms

B.原有策略模拟    126000ms              1780031ms(29min40s)   ？ms

C.19号实际数据    -                     2438000ms(40min38s)

D.25号实际数据    -                     -                     ？ms

E.改进策略模拟    90015ms               1648015ms(27min28s)   ？ms

与最坏比提升      33.3%                 35.77%

与模拟比提升      28.56%                7.41%

与实际比提升      -                     32.4%

## 结果分析

1. 从A、C对比，可以看出现有分发策略优于最坏情况，但是根据B的结果可以看错现有分发策略浪费了大约11分钟的时间（大约占总运行时间的25%，其中包含co自动化脚本时间）。
2. 从B、E对比，可以看出改进的分发策略效果明显。

## 得出结论

> 改进的分发策略可以使自动化脚本运行时间减少7.5%左右，同时改进的分发策略除co自动化脚本以外几乎不浪费时间。

## 备注

1. 代码支持某台执行机宕机测试，但会导致消费队列异常，原因待查。本实验不讨论执行机宕机情况。
2. 结果截图中存在某个任务先消费后生产的情况，与线程调度和输出缓存有关，对本实验结果没有任何影响。