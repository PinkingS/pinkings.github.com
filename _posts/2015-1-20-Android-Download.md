---
layout: post
time: 2015-1-20
title: Android下载文件常见错误解决方法
category: Android
keywords: Android
description: Android下载文件过程中http访问出错，本地文件无法访问等问题解答
---

最近在学习Mars老师的Android开发课程，仿照http://www.cnblogs.com/Laupaul/archive/2012/02/12/2348293.html的代码来写应用，结果中间出了各种各样的问题，在这里总结一下：

###1. `java.lang.NullPointerException`报错，`android.os.NetworkOnMainThreadException`异常

原因：`urlCon.getInputStream()`执行的时候出错导致，得不到`InputStream`。这个异常大概意思是在主线程访问网络时出的异常。 造成这样的错误原因是代码不符合Android规范，Android在4.0之前的版本支持在主线程中访问网络，但是在4.0以后对这部分程序进行了优化，也就是说访问网络的代码不能写在主线程中了。

解决办法：

改用多线程，异步加载的方式：

{% highlight Java %}
    String lrc;

    Handler handler = new Handler() {
        public void handleMessage(Message msg) {
            if(msg.what == 0x123) {
                System.out.println(lrc);
            }
        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_download);
        downloadTxtButton = (Button)findViewById(R.id.downloadTxt);
        downloadMp3Button = (Button)findViewById(R.id.downloadMp3);
        textview = (TextView)findViewById(R.id.textview);
        downloadTxtButton.setOnClickListener(new View.OnClickListener() {
            public void onClick(View view) {
                new Thread() {
                    public void run() {
                        HttpDownloader httpDownloader = new HttpDownloader();
                        lrc = httpDownloader.download("http://127.0.0.1/android_download/white_black.lrc");
                        handler.sendEmptyMessage(0x123);
                        textview.setText(lrc);
                    }
                }.start();
            }
        });
    }
{% endhighlight  %}

###2. android从tomcat读取文件出错：`connect failed: ECONNREFUSED`

本地启tomcat，当想让android从本地服务器上下载文件时，url中IP地址写127.0.0.1或localhost在模拟器中无效：

`W/System.err(12527): java.net.ConnectException: failed to connect to localhost/127.0.0.1 (port 8080): connect failed: ECONNREFUSED (Connection refused)`

改为本机的IP地址也显示无法连接，终于在网上找到一个非常简便的方式：android把127.0.0.1当作模拟器本机，而把计算机本地IP设为10.0.2.2，所以只需要把需连接本地计算机web服务地址改为：http://10.0.2.2即可。

###3.`Only the original thread that created a view hierarchy can touch its views`报错

Android的相关View和控件不是线程安全的，从报错信息可以得知只能由产生View的线程对它做改变，因此将`textview.setText(lrc);`放到`handleMessage()`里即可。

经过一番折腾，终于出了结果：

![img](/assets/image/posts/android_download.jpg)

目前还是新手，所以出了问题就各种不知所措，经验还是需要慢慢积累的~
