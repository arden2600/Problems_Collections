## Xml Problems Collection<br>
记录下开发或者其他地方遇到的xml相关问题。<br>

* **The method getTextContent() is undefined for the type Node**<br>
  在做微信开发，要将输出文本xml格式化回复消息接口时候，在适用XMLParse类解析xml时候，调用`org.w3c.dom.NodeList.item`的getTextContent()方法时候遇到了这个问题，说明引用的包出现了问题。这就说明，相同的全限定包中的类**org.w3c.dom**类，出现了至少两个相同的类。我们在工程中却引用了另外一个不包含getTextContent()方法的类。我们仅仅需要将调用的类包指定清楚即可。<br>

  在项目中搜索类**org.w3c.dom**，发现jdk中rt.jar类包中包含了这个类，且这个类中有getTextContent方法。说明，我们要引用的其实是这个类包。还有一个同名类，是因为我们在maven项目的pom.xml中，引用了dom4j这个xml解析类的时候，如下:<br>
```xml
## pom.xml
		<dependency>
			<groupId>dom4j</groupId>
			<artifactId>dom4j</artifactId>
			<version>1.6.1</version>
		</dependency>
```
会将依赖的**xml-apis-*.jar**库包引入项目工程，这个xml-api-*.jar类包中也有一个org.w3c.dom类，在该类中可没有getTextContent方法。<br>
这时候，我们就必须在IDE中设置设置同名包的优先级，eg:在eclipse中，可以右击项目名-->Build Path --> Configure Build Path打开项目路径配置窗口，并选择**Order and Export**选项卡，勾选自己的JDK，点击右边**Up**按钮，将JDK优先级设置在**Maven Dependencies**之上。这个选项卡就是用来配置： 当在项目中引入同步包的优先级。这样，XMLParse类中，就会先引用JDK的rt.jar中dom类。<br>
