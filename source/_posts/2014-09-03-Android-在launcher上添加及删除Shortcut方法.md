title: Android 在launcher上添加及删除Shortcut方法
categories:
  - Android
tags:
  - Tips
  - 快捷方式
date: 2014-09-03 16:37:51
---

需要在Manifest里添加两个权限：
{% codeblock %}
<uses-permission android:name="com.android.launcher.permission.INSTALL_SHORTCUT" />
<uses-permission android:name="com.android.launcher.permission.UNINSTALL_SHORTCUT" />
{% endcodeblock %}

添加完就可以照搬下面的代码来实现了

**添加Shortcut：**
{% codeblock %}
Intent intent1 = new Intent("android.intent.action.MAIN");
//替换自己项目的
intent1.setClass(MainActivity.this,MainActivity.class);
intent1.addCategory("android.intent.category.LAUNCHER");

Intent intent2 = new Intent();
intent2.putExtra("android.intent.extra.shortcut.INTENT",intent1);
//把Shortcut的名字改为自己项目的
intent2.putExtra("android.intent.extra.shortcut.NAME","MainActivity");
//把context改为自己项目的
intent2.putExtra("android.intent.extra.shortcut.ICON_RESOURCE",Intent.ShortcutIconResource.fromContext(MainActivity.this, R.drawable.icon));
//这里的覆盖参数不管设置为true还是false都没啥效果，不知是为何
intent2.putExtra("duplicate", false);
intent2.setAction("com.android.launcher.action.INSTALL_SHORTCUT");
sendOrderedBroadcast(intent2, null);
{% endcodeblock %}



**删除Shortcut：**
{% codeblock %}
Intent intent1 = new Intent("android.intent.action.MAIN");
//替换为自己项目的
intent1.setClass(MainActivity.this,MainActivity.class);
intent1.addCategory("android.intent.category.LAUNCHER");

Intent intent2 = new Intent();
intent2.putExtra("android.intent.extra.shortcut.INTENT",intent1);
//把Shortcut的名字改为自己项目的
intent2.putExtra("android.intent.extra.shortcut.NAME","MainActivity");
//把context改为自己项目的.  ps:此处不设置图标也无所谓
intent2.putExtra("android.intent.extra.shortcut.ICON_RESOURCE",Intent.ShortcutIconResource.fromContext(MainActivity.this, R.drawable.icon));
intent2.setAction("com.android.launcher.action.UNINSTALL_SHORTCUT");
sendOrderedBroadcast(intent2, null);
{% endcodeblock %}

