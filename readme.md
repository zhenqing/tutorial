# maven 介绍
Maven 是一个项目管理和构建自动化工具。
>Maven is a build automation tool used primarily for Java projects. In Yiddish, the word maven means "accumulator of knowledge"

>Maven can also be used to build and manage projects written in C#, Ruby, Scala, and other languages.

>Maven addresses two aspects of building software: first, it describes how software is built, and second, it describes its dependencies.

>it uses conventions for the build procedure, and only exceptions need to be written down. An XML file describes the software project being built, its dependencies on other external modules and components, the build order, directories, and required plug-ins. It comes with pre-defined targets for performing certain well-defined tasks such as compilation of code and its packaging.

>Maven dynamically downloads Java libraries and Maven plug-ins from one or more repositories such as the Maven 2 Central Repository, and stores them in a local cache.[4] This local cache of downloaded artifacts can also be updated with artifacts created by local projects. Public repositories can also be updated.

![maven](https://www.ibm.com/developerworks/cn/education/java/j-mavenv2/figure1.jpg)
![maven2](https://www.ibm.com/developerworks/cn/education/java/j-mavenv2/figure2.gif)
- 项目对象模型（POM）
- 依赖项管理模型
- 构建生命周期和阶段

|Item|Desc|
|----|----|
|Initial Release|13 July 2004|
|Written in| Java Jelly |
|Rely on| XML |
|Configure|Conventions|
|Operating system|Cross-platform|
|License| Apache License 2.0|

___
## 简介
-  a standard way to build the projects
- a clear definition of what the project consisted of
- an easy way to publish project information
- a way to share JARs across several projects.

## 核心概念
- POM 项目的信息和maven build项目所需的配置信息
- Artifact 一个项目将要产生的文件，可以是jar文件，源文件，二进制文件，war文件，甚至是pom文件。每个artifact都由groupId:artifactId:version组成的标识符唯一识别。
- Repositories是用来存储Artifact的。如果说我们的项目产生的Artifact是一个个小工具，那么Repositories就是一个仓库，里面有我们自己创建的工具，也可以储存别人造的工具。在pom中声明dependency，编译代码时就会根据dependency去下载工具（Artifact），供自己使用。
- Build Lifecycle 一个项目build的过程。maven的Build Lifecycle分为三种，分别为default（处理项目的部署）、clean（处理项目的清理）、site（处理项目的文档生成）。他们都包含不同的lifecycle。
- Goal 代表一个特定任务。mvn package表示打包的任务。mvn deploy表示部署的任务。mven clean install则表示先执行clean的phase（包含其他子phase），再执行install的phase。
## 安装
在安装 maven 前，先保证你安装了 JDK 。
[Maven官方下载链接](http://maven.apache.org/download.html)  
The installation of Apache Maven is a simple process of extracting the archive and adding the bin folder with the mvn command to the PATH.  
`$ mvn -v `  
显示下面信息说明安装成功:
```
Apache Maven 3.0.3 (r1075438; 2011-03-01 01:31:09+0800)
Maven home: /home/limin/bin/maven3
Java version: 1.6.0_24, vendor: Sun Microsystems Inc.
Java home: /home/limin/bin/jdk1.6.0_24/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "2.6.35-28-generic-pae", arch: "i386", family: "unix"
```
如果你是第一次运行 maven，你需要 Internet 连接，因为 maven 需要从网上下载需要的插件。


## 项目结构
|目录|目的|
|----|----|
|${basedir}|存放 pom.xml和所有的子目录|
|${basedir}/src/main/java|项目的 java源代码|
|${basedir}/src/main/resources|项目的资源，比如说 property文件|
|${basedir}/src/test/java|项目的测试类，比如说 JUnit代码|
|${basedir}/src/test/resources|测试使用的资源|
## 使用
maven创建项目是根据Archetype（原型）创建的。原型对于项目的作用就相当于模具对于工具的作用，我们想做一个锤子，将铁水倒入模具成型后，稍加修改就可以了。类似我们可以根据项目类型的需要使用不同的Archetype创建项目。通过Archetype我们可以快速标准的创建项目。利用Archetype创建完项目后都有标准的文件夹目录结构。
一个 maven 项目在默认情况下会产生 JAR 文件，另外 ，编译后 的 classes 会放在 ${basedir}/target/classes 下面， JAR 文件会放在 ${basedir}/target 下面。  
不用任何 IDE ，只用 maven。  
创建项目的goal为mvn archetype:generate。
`$mvn archetype:generate -DgroupId=com.mycompany.helloworld -DartifactId=helloworld -Dpackage=com.mycompany.helloworld -Dversion=1.0-SNAPSHOT`  
运行这两个命令构建:
```
~$cd helloworld

~$mvn package
```
创建一个简单的web项目，只需要修 -DarchetypeArtifactId为maven-archetype-webapp即可  
>第一次运行 maven 的时候，它会从网上的 maven 库 (repository) 下载需要的程序，存放在你电脑的本地库 (local repository) 中，所以这个时候你需要有 Internet 连接。Maven 默认的本地库是 ~/.m2/repository/ ，在 Windows 下是 %USER_HOME%\.m2\repository\  
>maven 在 helloworld 下面建立了一个新的目录 target/ ，构建打包后的 jar 文件 helloworld-1.0-SNAPSHOT.jar 就存放在这个目录下。编译后的 class 文件放在 target/classes/ 目录下面，测试 class 文件放在 target/test-classes/ 目录下面。

`~$java -cp target/helloworld-1.0-SNAPSHOT.jar com.mycompany.helloworld.App`

## POM
`pom.xml` Project Object Model  
minimum example:  
```
<project>
  <!-- model version is always 4.0.0 for Maven 2.x POMs -->
  <modelVersion>4.0.0</modelVersion>

  <!-- project coordinates, i.e. a group of values which
       uniquely identify this project -->

  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0</version>

  <!-- library dependencies -->

  <dependencies>
    <dependency>

      <!-- coordinates of the required library -->

      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>

      <!-- this dependency is only used for running and compiling tests -->

      <scope>test</scope>

    </dependency>
  </dependencies>
</project>
```

|节点|含义|
|----|----|
|project |pom文件的顶级元素|
|modelVersion |所使用的object model版本，为了确保稳定的使用，这个元素是强制性的。除非maven开发者升级模板，否则不需要修改|
|groupId |是项目创建团体或组织的唯一标志符，通常是域名倒写，如groupId  org.apache.maven.plugins就是为所有maven插件预留的|
|artifactId |是项目artifact唯一的基地址名|
|packaging |artifact打包的方式，如jar、war、ear等等。默认为jar。这个不仅表示项目最终产生何种后缀的文件，也表示build过程使用什么样的lifecycle。|
|version |artifact的版本，通常能看见为类似0.0.1-SNAPSHOT，其中SNAPSHOT表示项目开发中，为开发版本|
|name |表示项目的展现名，在maven生成的文档中使用|
|url|表示项目的地址，在maven生成的文档中使用|
|description| 表示项目的描述，在maven生成的文档中使用|
|dependencies |表示依赖，在子节点dependencies中添加具体依赖的groupId artifactId和version|
|build |表示build配置|
|parent |表示父pom|

### Maven 坐标
Maven 坐标是一组可以惟一标识工件的三元组值。坐标包含了下列三条信息：

- 组 ID
- 工件 ID
- 版本

### 例子
- watchman
- ListingMapping
- jsoup

## Dependency Mechanism

### Dependency Scope
- compile
- provided
- runtime
- test
- system
- import

## 仓库
- 本地仓库  
系统用户的.m2/repository下面
- 远程仓库  
[maven repository](https://mvnrepository.com/)

## 插件
Maven is - at its heart - a plugin execution framework; all work is done by plugins.   
- Build plugins will be executed during the build and they should be configured in the <build/> element from the POM.
- Reporting plugins will be executed during the site generation and they should be configured in the <reporting/> element from the POM.

## 常用指令
`指令执行地点：项目根路径下面（Pom.xml存储地址）`
- mvn complie: 下载依赖并编译生成class文件在${basedir}/target/classes
- mvn package：打包当前项目，根据packaging分为pom、jar、war等在${basedir}/target
- mvn install：打包当前项目，且install到本地的maven仓库，默认路径为:%USER_HOME%/~m2/repository/${groupId}/${artifactId}-${version}.jar
- mvn test：执行所有单元测试
- mvn deploy：将当前项目部署到中央仓库，可以是公开服务器，也可以是私服
- mvn clean：清理之前的构建记录，类似于删除target下面所有文件
- mvn javadoc:javadoc：生成Javadoc
- mvn dependency:purge-local-repository：清理当前项目在本地repository中所依赖的类库
## 附加参数
- 跳过单元测试: -Dmaven.test.skip=true，比如某些特定情况下，打包项目时不想执行单元测试，可以mvn package -Dmaven.test.skip=true
- 跳过生成源码: -Dmaven.source.skip=true
- 跳过生成Javadoc：-Dmaven.javadoc.skip=true
- 强制更新本地repository cache, 强制更新snapshot类型的插件或依赖库: -U，比如maven -U package
- 显示详细错误: mvn -e
- 生成项目相关信息的网站: mvn site
## 生命周期
Build Lifecycle是由phases构成的，下面重点介绍default Build Lifecycle几个重要的phase,phase是有序的（注意实际两个相邻phase之间还有其他phase被省略，完整phase见lifecycle），下面一个phase的执行必须在上一个phase完成后
1. validate 验证项目是否正确以及必须的信息是否可用。若直接以某一个phase为goal，将先执行完它之前的phase，如mvn install 将会先validate、compile、test、package、integration-test、verify最后再执行install phase。
2. generate-sources 生成源码
3. process-sources 处理源码
4. generate-resources 生成资源
5. process-resources 处理资源
6. compile 编译源代码
7. process-test-sources 生成测试源码
8. process-test-resources 处理测试资源
9. test-compile 测试编译
10. test  测试编译后的代码，即执行单元测试代码
11. package 打包编译后的代码，在target目录下生成package文件
12. install 安装package到本地仓库，方便本地其它项目使用
13. deploy 部署，拷贝最终的package到远程仓库和替他开发这或项目共享，在集成或发布环境完成
## 注意事项
`<version>1.0-SNAPSHOT</version>`
