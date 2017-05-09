## Maven Problems Collections<br>
记录在开发和使用中遇到的，看到的maven相关问题。<br>

* ** Fatal error compiling: 无效的目标发行版: 1.8 ->**<br>
  当在使用`maven build`编译一个maven工程时候，若是控制台输出类似如下错误信息:<br>
```shell
Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:3.1:compile (default-compile) 
on project xiangbideWXBG: Fatal error compiling: 无效的目标发行版: 1.8 -> [Help 1]
```
则说明当前的jdk1.8版本不能用来编译maven工程，那么可以在pom.xml中，强制显示的配置适用的jdk版本来对项目进行编译。<br>
```xml
## pom.xml
	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<configuration>
					<source>1.7</source>
					<target>1.7</target>
				</configuration>
			</plugin>
		</plugins>
	</build>

```
