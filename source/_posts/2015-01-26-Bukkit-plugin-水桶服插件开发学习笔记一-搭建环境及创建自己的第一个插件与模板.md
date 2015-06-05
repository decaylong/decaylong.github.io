title: '[Bukkit plugin]水桶服插件开发学习笔记一:搭建环境及创建自己的第一个插件与模板'
categories:
  - Bukkit Plugin
tags:
  - 学习笔记
date: 2015-01-26 22:42:32
---

# 1.工具

[JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html "Java SE - Downloads | Oracle Technology Network | Oracle")：插件开发自然少不了Java语言，所以前提还是得懂得基础的Java开发才行

[Maven](http://maven.apache.org/index.html "Maven - Welcome to Apache Maven")：水桶服的插件项目是用Maven工具来进行管理的，所以少不了

[Eclipse](http://eclipse.org/ "Eclipse Luna")、[IntelliJ IDEA](https://www.jetbrains.com/idea/ "IntelliJ IDEA — The Most Intelligent Java IDE")、[NetBeans](https://netbeans.org/ "Welcome to NetBeans")：开发工具IDE，三选一就行了，哪个顺手用哪个或想学哪个用哪个了，随意

[Git](https://github.com/ "GitHub")：使用Git或者其他的版本管理工具来管理你的项目，可选

工具的安装就不讲了，百度一下就有不少资料了。（旧版本的Eclipse可能没有集成M2Eclipse插件，所以需要自己弄一下，不过较新版本的Eclipse就没这烦恼）

<!-- more -->

# 2.创建第一个Bukkit Plugin项目

因为个人想学习IntelliJ IDEA的使用，故以下均是用此工具来讲解了。其他IDE平台其实也大同小异的。

当前最新的IDEA是14.02

[![版本14.02](/images/2015/01/1.png)](/images/2015/01/1.png)

## 创建新工程

点击创建一个新工程，选择Maven项目，下一步。

[![创建一个Maven项目](/images/2015/01/2.png)](/images/2015/01/2.png)

GroupID是项目组织唯一的标识符，实际对应JAVA的包的结构，是main目录里java的目录结构。

ArtifactID就是项目的唯一的标识符，实际对应项目的名称，就是项目根目录的名称。

Version就是这个项目的版本号了。

关于GroupID，假如你有一个域名 example.com 那么你可以填写成 com.example ，比如我的域名是decaylong.net，那么此处我写成 net.decaylong，然后在再加上你的项目的名字，比如我的项目名称是SamplePlugin，那么最后我的GroupID就是 net.decaylong.sampleplugin

ArtifactID在这就是SamplePlugin

[![](/images/2015/01/31.png)](/images/2015/01/31.png)

这里Project相对于Eclipse的WorkSpace的概念，Module相对于Eclipse的project概念，直接把Project name改为你的项目名称就行了

[![](/images/2015/01/4.png)](/images/2015/01/4.png)

项目创建完是这个样子了

[![项目创建完成](/images/2015/01/5.png)](/images/2015/01/5.png)

## 配置项目以开发插件

接下来引用Bukkit的API，打开你的pom.xml文件，加入以下内容到pom.xml文件里的</project>标签前。

build标签配置你的项目的编译版本，看一下[MCstats](http://mcstats.org/global/ "MCstats")，普遍的服务器都是用的1.7版本java，只有少数1.6,而1.8只占1.7的四分之一而已，如果你的项目里会用到1.7及以上版本的特性的话，还是得用相应的版本，如果不用的话，其实1.6版也是无妨的，看需求了。

repositories标签引用了bukkit的API仓库。

dependencies标签指明我们的项目是依赖于bukkitAPI仓库里的哪个版本，关于版本，可以在version标签上注释的那个连接里，找到你想要开发的插件所对应的水桶服版本，替换即可。
{% codeblock %}
	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>2.3.2</version>
				<configuration>
					<source>1.6</source>
					<target>1.6</target>
				</configuration>
			</plugin>
		</plugins>
	</build>
	<repositories>
		<repository>
			<id>bukkit-repo</id>
			<url>http://repo.bukkit.org/content/groups/public/</url>
		</repository>
	</repositories>
	<dependencies>
		<dependency>
			<groupId>org.bukkit</groupId>
			<artifactId>bukkit</artifactId>
			<!-- Change "1.7.2-R0.2" to the version you wanna to develop,find the 
				versions from "http://repo.bukkit.org/content/groups/public/org/bukkit/bukkit/" -->
			<version>1.7.2-R0.2</version>
			<scope>provided</scope>
		</dependency>
	</dependencies>
{% endcodeblock %}

保存后需要更新一下项目的依赖包后方可继续后续的内容。只需在窗口右侧的“Maven Project”里点击“Generate Sources and Update Folders For All Projects”按键，如下图所示，也可以直接右击项目 -> Maven ->Generate Sources and Update Folders For All Projects.

[![更新依赖包](/images/2015/01/update.png)](/images/2015/01/update.png)

这个过程会下载Bukkit的API包，如果下载出问题就开下VPN下载吧，直到运行完成就可以了。

## 插件主类

项目的依赖包初始化后就可以开始创建我们插件的主类，先在src -> main -> java下创建我们的包，名字同前面填写GroupID时一样，这里我创建的是"net.decaylong.sampleplugin";

[![创建包](/images/2015/01/7.png)](/images/2015/01/7.png)

然后在我们的包下新建我们的主类，在新建的包上右键 -> New -> Java Class，名字同ArtifactID的值，这里为SamplePlugin。

[![新建主类](/images/2015/01/8.png)](/images/2015/01/8.png)

插件的主类需要继承BukkitAPI里的JavaPlugin类，所以需要修改下我们的代码，导入相应的包，让我们的类继承JavaPlugin，如下：
{% codeblock %}
package net.decaylong.sampleplugin;

import org.bukkit.plugin.java.*;

/**
 * Created by DecayLong on 2015/1/26.
 */
public class SamplePlugin extends JavaPlugin{
}
{% endcodeblock %}

## 重要的plugin.yml文件

为了让服务器能够加载我们的插件，就得建个plugin.yml文件，让服务器知道我们插件的一些信息，右键main下的resources文件夹 -> New -> File。

name即插件的名称，main是插件的主类（包名.类名），version插件的版本
{% codeblock %}
name: SamplePlugin
main: net.decaylong.sampleplugin.SamplePlugin
version: 1.0-SNAPSHOT
{% endcodeblock %}

##  插件的启用与禁用回调函数

现在既然服务器能够加载我们的插件了，那么我们就得对服务器的加载动作做出反应了，在主类的编辑窗口里按ctrl+O 或者 菜单Code -> Override Methods...; 打开复写函数窗口，按住ctrl键，选择onDisable()及onEnable()这两函数，确定。

[![选择复写函数](/images/2015/01/9.png)](/images/2015/01/9.png)

[![复写函数](/images/2015/01/10.png)](/images/2015/01/10.png)

为了直观的让我们知道我们的插件被服务器加载了，并且我们能对其进行操作，我们在这复写的两个函数里各打印一句话，修改后代码如下：
{% codeblock %}
package net.decaylong.sampleplugin;

import org.bukkit.plugin.java.*;

/**
 * Created by DecayLong on 2015/1/26.
 */
public class SamplePlugin extends JavaPlugin {
    @Override
    public void onDisable() {
        getLogger().info("Plugin: SamplePlugin is now off!");
    }

    @Override
    public void onEnable() {
        getLogger().info("Plugin: SamplePlugin is now on!");
    }
}
{% endcodeblock %}

## 第一个插件的诞生

到这里我们的第一个水桶服插件就开发完成了，接下来就是编译打包然后放服务器里运行一下看看了。点开IDEA窗口右边的“Maven Projects”，展开“Lifecycle”，右击“package”，运行“Run 'SamplePlugin [package]'”。第一次编译需要下载Maven的一些包，所以需要点时间，如果没法下载或者下完后编译有问题，可以考虑开VPN再重新下一遍，毕竟国外的东西，国内想用还是多多少少有些许问题。

[![](/images/2015/01/19.png)](/images/2015/01/19.png)

[![](/images/2015/01/20.png)](/images/2015/01/20.png)

编译完后在项目的根目录下有个target文件夹，编译后的jar包就在这了，复制到服务器的plugins文件夹里，运行服务器就可以看到我们的插件已经加载进服务器了，键入reload命令，插件也会如愿的被停止后再启用

[![服务器加载插件](/images/2015/01/11.png)](/images/2015/01/11.png)

[![被停用](/images/2015/01/12.png)](/images/2015/01/12.png)

# 3.创建插件开发模板

## 创建模板

Maven有个命令可直接依靠当前项目来创建模板
{% codeblock %}mvn archetype:create-from-project{% endcodeblock %}

IDEA的Maven Projects窗口内的工具栏有个“Execute Maven Goal”按键，点击，输入 “clean archetype:create-from-project”，运行。

[![创建模板](/images/2015/01/13.png)](/images/2015/01/13.png)

## 修改模板

整个模板都在target/generated-sources/archetype目录下，那么接下来以在这个文件夹为根目录，需要改的文件如图所示

[![修改文件](/images/2015/01/15.png)](/images/2015/01/15.png)

SamplePlugin.java：右键 -> Refactor -> Rename File...;  修改文件名为“__artifactId__.java”，注意前后各两个下划线，共四个下划线

plugin.yml：打开此文件，内存替换成：
{% codeblock %}
name: ${artifactId}
main: ${package}.${artifactId}
version: ${version}
{% endcodeblock %}

archetype-metadata.xml：因为创建模板的时候，Maven把工程目录下的一些无关紧要的文件也列入模板列表里了，这些文件我们并不需要，所以删除它们的记录以及文件。在archetype-descriptor标签里，只留下directory标签内容为：“src/main/java” 以及 “src/main/resources”的fileSet标签，其他的fileSet标签都删除，然后在directory标签内容为：“src/main/resources”对应的fileSet标签里添加属性“filtered="true"”，修改后内容如下：
{% codeblock %}
<?xml version="1.0" encoding="UTF-8"?>
<archetype-descriptor xsi:schemaLocation="http://maven.apache.org/plugins/maven-archetype-plugin/archetype-descriptor/1.0.0 http://maven.apache.org/xsd/archetype-descriptor-1.0.0.xsd" name="SamplePlugin"
    xmlns="http://maven.apache.org/plugins/maven-archetype-plugin/archetype-descriptor/1.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <fileSets>
    <fileSet filtered="true" packaged="true" encoding="UTF-8">
      <directory>src/main/java</directory>
      <includes>
        <include>**/*.java</include>
      </includes>
    </fileSet>
    <fileSet filtered="true" encoding="UTF-8">
      <directory>src/main/resources</directory>
      <includes>
        <include>**/*.yml</include>
      </includes>
    </fileSet>
  </fileSets>
</archetype-descriptor>
{% endcodeblock %}

改完后把src/main/resources/archetype-resources下的“.idea”文件夹以及“.iml”文件删除

pom.xml：注意是target/generated-sources/archetype文件夹下的pom.xml。把groupId改下，后面加上"_archetype"，便于我们辨认，再把version改为1.0，这是我们的1.0正式版模板，当然这一步看个人爱好了，不想改就不改了，只是为了好看点。

## 安装模板到本地仓库

运行cmd，切换目录到项目的target/generated-sources/archetype文件夹下，即archetype项目的根目录，运行
{% codeblock %}mvn clean install{% endcodeblock %}

[![安装模板到本地仓库](/images/2015/01/16.png)](/images/2015/01/16.png)

到这我们的模板就创建完成了

# 4.使用模板

新建一个Maven项目，勾选“Create from archetype”，因为IDEA的Maven插件没法自己加载本地的archetype，所以得自己手动添加了（Eclipse就没这问题），点击“Add Archetype..”，填写我们的模板的GroupId、ArtifactId、Version（**ps：这三个值是以“target\generated-sources\archetype”这目录下的“pom.xml”文件里的值为准**），然后确认。

[![添加archetype](/images/2015/01/17.png)](/images/2015/01/17.png)



[![18](/images/2015/01/18.png)](/images/2015/01/18.png)

接下来就跟创建第一个项目是一样的操作了。

填写了那三个值后下一步时Maven的一些配置信息，让他默认就行了，项目创建完成后可能Maven处理的速度会比较慢，耐心等待它跑完。如果右上角有提示你导入Maven Projects，请点击 Enable Auto-Import 就行了

