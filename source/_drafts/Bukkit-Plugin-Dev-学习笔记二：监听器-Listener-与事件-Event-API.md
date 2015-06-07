title: '[Bukkit Plugin Dev]学习笔记二：监听器(Listener)与事件(Event)API'
categories:
  - Bukkit Plugin
tags:
  - 学习笔记
---
#一：简介

监听器(Listener)：顾名思义就是用来监听什么东西的。

事件(Event)：服务器启动成功是一种事件，玩家登录服务器是一种事件，玩家破坏一块方块是一种事件...

监听器和事件是相辅相成的，事件告诉监听器某件事发生了，而监听器则决定要对哪些发生的事件作出反应。

#二：基础

##一个简单的监听器

先看一个简单的监听器，理解事件和监听器是怎么一回事


{% codeblock %}
package net.decaylong.eventsplugin.listener;

import net.decaylong.eventsplugin.EventsPlugin;
import org.bukkit.event.EventHandler;
import org.bukkit.event.Listener;
import org.bukkit.event.player.PlayerLoginEvent;

//实现了Listener接口，所以是个监听器
public class EventsTestListener implements Listener {
	
	//捕获某个事件
    @EventHandler
    public void onLogin(PlayerLoginEvent event) {
        //EventsPlugin.logInfo();这个静态方法(Method)是调用自我的插件主类EventsPlugin的
        //并非水桶服的API之一
        //在服务器控制台打印出某某玩家登录服务器的信息。
        EventsPlugin.logInfo("Player " + event.getPlayer().getName() + " is logging in!");
    }

}
{% endcodeblock %}

这是一个简单的监听器类，实现了 org.bukkit.event 包下的 Listener 接口，这就是一个监听器了。

监听器想要知道它底下有哪些事件需要被监听，那么就需要一个 @EventHandler 注释，告诉监听器，有这注释的方法(Method)是一个事件监听方法(Method)。

onLogin 方法(Method)就是一个处理事件的方法(Method)。方法(Method)名不重要，只要起得能够方便你辨别此方法(Method)的用途就可以任意取了。

想让监听器知道该监听什么事件，那么我们就得给我们的事件处理方法(Method)传入一个想要监听的事件类，如上例的PlayerLoginEvent类，这样监听器就知道我们是要监听玩家登录的事件了。

在这个实例中，在我们监听到了PlayerLoginEvent事件后在控制台打印出某某玩家登陆服务器的消息。

##注册事件

##注销事件

#提高


