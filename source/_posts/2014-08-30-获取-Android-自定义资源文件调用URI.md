title: 获取 Android 自定义资源文件调用URI
categories:
  - Android
tags:
  - Tips
  - 资源调用
date: 2014-08-30 16:26:52
---

在设置Notification的时候，有时会自己提供一个音乐文件，这是就需要在raw文件夹下存一个音乐文件，并在程序里面调用它，那么调用raw文件夹下的文件的方法就很重要了，做法很简单：

{% codeblock %}
Uri.parse("android.resource://包名/"+资源ID);
{% endcodeblock %}

例如：

{% codeblock %}
Uri.parse("android.resource://cn.decay.notification/"+R.raw.ring);
{% endcodeblock %}

这样就能得到资源的URI了，剩下的就不难弄了。