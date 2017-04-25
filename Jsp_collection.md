## Jsp页面问题集合<br>
记录一些时常不注意就遇到的问题。<br>
* ** javax.servlet.jsp.PageContext cannot be resolved to a type **:<br>
在jsp中要引用当前工程路径时候，在表单中是用`action="${pageContext.request.contentPath}"`出现如上提示问题。主要原因是jsp api包没有导入完全引起的。在pom.xml中引用下面的配置即可<br>
```xml
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jsp-api</artifactId>
			<version>2.0</version>
			<scope>provided</scope>
		</dependency>
```
