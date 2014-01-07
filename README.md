### 1. maven 中的 package compile install deploy clean这5个命令有什么区别？###
* mvn package: 将编译后的代码打包成相应的格式文件，如jar包
* mvn compile: 编译项目中的代码
* mvn install: 将包安装到本地maven仓库，可以让其他项目作为依赖使用该包
* mvn deploy: 将包发布到远程的maven仓库，并提供给其他开发者使用
* mvn clean: 清除项目目录中的生成结果

### 2. Maven强大的一个重要的原因是它有一个十分完善的生命周期模型(lifecycle)，简单介绍下Maven的生命周期模型。###
maven将工程（Project）的构建过程理解为不同的生命周期(LifeCycle)和阶段（Phase）。 在工程的构建过程中，存在着不同的生命周期，这些生命周期互相独立，之间也没有一定的顺序关系。 每个生命周期又划分为不同的阶段（Phase）。阶段之间有明确的顺序关系， 同一生命周期内的阶段必须按顺序依次执行。

maven内置了三个生命周期，并为每个生命周期内置了一些阶段。 下面列举出maven内置的生命周期及主要的阶段： 

![maven内置的生命周期](http://images.cnitblog.com/blog/376709/201212/24092808-98c3b94fbe834a61b77a48086d5b4f97.png)

* default：构建（Build）
	* validate：验证项目是否正确，所有必需的信息是否可用。
	* compile：编译项目中的代码。
	* test：用相关的单元测试框架测试编译后的代码，这些运行的测试并不会随项目打包和布署。
	* package：将编译后的代码打包成相应的格式文件，如jar包。
	* integration-test： 如果需要在一个综合环境中运行我们的测试，这个阶段将会运行和布署项目到该环境中。
	* verify： 检查项目的包是否正确和符合要求。
	* install：将包安装到本地maven仓库，可以让其他项目作为依赖使用该包。
	* deploy：将包发布到远程的maven仓库，并提供给其他开发者使用。
* clean：清理
	* pre-clean: 准备清理
	* clean: 执行清理工作
	* post-clean: 执行清理后的后续工作
* site：生成项目文档和站点
	* pre-site: 准备生成
	* site: 生成项目站点和文档
	* post-site: 执行生成文档后的后续工作
	* site-deploy: 发布项目文档


### 3. maven 中的profile有什么作用？###
项目开发时，有可能会在多个不同的环境构建，例如不同的操作系统，JDK等等，另外项目打包的产物也可能会发布到不同的环境运行，例如测试环境，生产环境，这时候我们往往需要频繁的修改POM文件来适应各种情况。


###4. 我们在使用maven依赖管理的时候，每个依赖项都有下面这些属性，都有什么作用？###
* groupId: 团体，公司，小组，组织，项目，或者其它团体。团体标识的约定是，它以创建这个项目的组织名称的逆向域名(reverse domain name)开头。
* artifactId: 在groupId下的表示一个单独项目的唯一标识符
* version: 一个项目的特定版本。发布的项目有一个固定的版本标识来指向该项目的某一个特定的版本。
* type: 相应的依赖产品包形式，如jar，war
* scope: 用于限制相应的依赖范围，包括以下的几种变量：
	* compile： 默认的scope，表示 dependency 都可以在生命周期中使用。而且，这些dependencies 会传递到依赖的项目中。
	* provided： 在JDK或容器已提供改依赖才使用，如sevlet.jar
	* runtime:  表示dependency不作用在编译时，但会作用在运行和测试时
	* test: 表示dependency作用在测试时，不作用在运行时
	* system: 跟provided 相似，但是在系统中要以外部JAR包的形式提供，maven不会在repository查找它。
* optional: 依赖是否可选，默认为false,即子项目默认都继承，为true,则子项目必需显示的引入
* exclusions:如果X需要A,A包含B依赖，那么X可以声明不要B依赖，只要在exclusions中声明exclusion.

### 5. 在使用Maven依赖管理功能的过程中， <dependencyManagement>标签的作用是什么？ 如何与<dependency>标签结合使用？ ###
是用于帮助管理chidren的dependencies的。例如如果parent使用dependencyManagement定义了一个dependencyon junit:junit4.0,那么 它的children就可以只引用 groupId和artifactId,而version就可以通过parent来设置，这样的好处就是可以集中管理 依赖的详情.

### 6. 我们如何在没有IDE环境的情况下编译我们的Maven Project？ ###
```
mvn clean            (cleans the project, i.e. delete the target directory)
mvn compile        (compiles the source code)
mvn test              (if required compiles the source code, then runs the unit tests)
mvn package       (compiles, tests then packages the jar or war file)
mvn clean install  (cleans the project, compiles, tests, packages and installs the jar or war file)
mvn install           (compiles, tests, packages and installs the jar or war file)
mvn -o install      (compiles, tests, packages and installs the jar or war file in offline mode)
mvn -Dmaven.test.skip package       (package without running the tests)
mvn -Dmaven.test.skip clean install  (clean and install without running tests)
mvn -o test         (test offline)
```
### 7. 在一些项目中我们看到项目的version是 1.0.0-SNAPSHOT。这个SNAPSHOT有什么作用？为什么用SNAPSHOT？我们应该怎么使用？###
如果一个版本包含字符串"SNAPSHOT"，Maven就会在安装或发布这个组件的时候将该符号展开为一个日期和时间值，转换为UTC时间。例如，"1.0-SNAPSHOT"会在2010年5月5日下午2点10分发布时候变成1.0-20100505-141000-1。

这个词只能用于开发过程中，因为一般来说，项目组都会频繁发布一些版本，最后实际发布的时候，会在这些snapshot版本中寻找一个稳定的，用于正式发布，比如1.4版本发布之前，就会有一系列的1.4-SNAPSHOT，而实际发布的1.4，也是从中拿出来的一个稳定版。

### 8.假设我们有两个Project libA,appC,其中appC包含主执行主程序，libA是一个library库Project。如何将上面的两个Project 打包成一个可运行Jar包，同时Jar包包含所有依赖的库（包含libA)。###
对这个问题的实现参见[appC的pem](https://github.com/gaoleiss/maven-demo/blob/master/appC/pom.xml):

* 将libA放入dependencies
```
<dependency>
    <groupId>com.weardex.test</groupId>
    <artifactId>libA</artifactId>
    <version>1.0-SNAPSHOT</version>
    <optional>true</optional>
</dependency>
```

* 使用`maven-assembly-plugin`构建一个可运行的JAR 文件，并将依赖包的class文件打入到构建的jar里面
```
<build>
        <plugins>
            <plugin>
                <artifactId>maven-assembly-plugin</artifactId>
                <configuration>
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                    <archive>
                        <manifest>
                            <mainClass>com.weardex.test.Main</mainClass>
                        </manifest>
                    </archive>
                </configuration>
                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```

### 9.###
假设我们有三个Project libA,libB,appC,其中appC包含主执行主程序（非web程序），libA,libB是两个library库Project，在appC的resouces中还包含conf/setting.xml cong/db-setting.xml, bin/startup.sh readme.txt等资源文件。
如何利用maven的build功能将这3个Project输出为一个可发布的标准结构（输出可部署的文件夹）。
```
  lib/
      liba.jar
      libb.jar
      appC.jar
  conf/
      setting.xml
      db-setting.xml
  bin/
```

对这个问题的实现参见[根目录的pem](https://github.com/gaoleiss/maven-demo/blob/master/pom.xml):

* 将libA,libB, appC放入dependencies

```
<dependencies>
        <dependency>
            <groupId>com.weardex.test</groupId>
            <artifactId>libA</artifactId>
            <version>1.0-SNAPSHOT</version>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>com.weardex.test</groupId>
            <artifactId>libB</artifactId>
            <version>1.0-SNAPSHOT</version>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>com.weardex.test</groupId>
            <artifactId>appC</artifactId>
            <version>1.0-SNAPSHOT</version>
            <optional>true</optional>
        </dependency>
</dependencies>
``` 

* 使用插件`maven-dependency-plugin`将libA.jar， libB.jar，appC.jar打入到 lib 目录

```
<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-dependency-plugin</artifactId>
	<version>2.8</version>
	<executions>
	    <execution>
		<id>copy-dependencies</id>
		<phase>package</phase>
		<goals>
		    <goal>copy-dependencies</goal>
		</goals>
		<configuration>
		    <outputDirectory>${project.build.directory}/${project.artifactId}/lib</outputDirectory>
		    <overWriteReleases>false</overWriteReleases>
		    <overWriteSnapshots>false</overWriteSnapshots>
		    <overWriteIfNewer>true</overWriteIfNewer>
		</configuration>
	    </execution>
	</executions>
</plugin>
``` 

* 使用插件`maven-resources-plugin`，拷贝appC的config目录已经生成并目录

```
<plugin>
        <artifactId>maven-resources-plugin</artifactId>
        <version>2.6</version>
        <executions>
            <execution>
                <id>copy-resources</id>
                <phase>validate</phase>
                <goals>
                    <goal>copy-resources</goal>
                </goals>
                <configuration>
                    <outputDirectory>${project.build.directory}/${project.artifactId}/conf</outputDirectory>
                    <resources>
                        <resource>
                            <directory>${project.basedir}/appC/src/main/resources/conf</directory>
                            <filtering>true</filtering>
                        </resource>
                    </resources>
                </configuration>
            </execution>
        </executions>
    </plugin>
    <plugin>
        <artifactId>maven-resources-plugin</artifactId>
        <version>2.6</version>
        <executions>
            <execution>
                <id>copy-resources</id>
                <phase>validate</phase>
                <goals>
                    <goal>copy-resources</goal>
                </goals>
                <configuration>
                    <outputDirectory>${project.build.directory}/${project.artifactId}/bin</outputDirectory>
                </configuration>
            </execution>
        </executions>
</plugin>
```
