

# 【Android Xml生成和解析】
## 拼接字符串方式生成 Xml 文件
MainActivity.java 代码片段
```java
public void click1(View view) throws Exception {
	StringBuilder sb = new StringBuilder();
	sb.append("<?xml version='1.0' encoding='utf-8' standalone='yes' ?>");
	sb.append("<smses>");
	for (int i = 0; i < 50; i++) {
	sb.append("<sms>");
	sb.append("<address>");
	sb.append(5554 + i);
	sb.append("</address>");
	sb.append("<body>");
	sb.append("我是短信内容" + i);
	sb.append("</body>");
	sb.append("<time>");
	sb.append(new Date().getTime());
	sb.append("</time>");
	sb.append("</sms>");
	}
	sb.append("</smses>");
	// 使用系统提供的方法 获取文件输出流
	FileOutputStream fos = this.openFileOutput("info.xml", MODE_PRIVATE);
	fos.write(sb.toString().getBytes());
	fos.close();
 }
```
技能：
上面的代码虽然也可以生成 xml 文件，但是无法对特殊字符进行处理，比如如果短信内容包含“</>”
符号，那么 xml 解析器就无法完成正确的解析。因此使用的前提是你确定数据内容没有特殊字符。

## 使用 XmlSerializer 生成 Xml 文件

注意： 
使用 XmlSerializer 生成 xml 文件是推荐的方式，因为该 api 内部已经实现了对特殊字符的处理。

MainActivity.java 代码片段
```java
public void click2(View view) throws Exception { 
	//在 data 目录中创建 info2.xml 文件，获取输出流
	FileOutputStream fos = openFileOutput("info2.xml", MODE_PRIVATE);
	//通过 Xml 类创建一个 Xml 序列化器
	XmlSerializer serializer = Xml.newSerializer();
	//给序列化器设置输出流和输出流编码
	serializer.setOutput(fos, "utf-8");
	/* 让序列化器开发写入 xml 的头信息，其本质是写入内容：
	* "<?xml version='1.0' encoding='utf-8' standalone='yes' ?>"	*/
	serializer.startDocument("utf-8", true);
	/*开始写入 smses 标签，
	* 第一个参数是该标签的命名空间， 这里不需要	*/
	serializer.startTag(null, "smses");
	for (int i = 0; i < 50; i++) {
		//开始 sms 标签
		serializer.startTag(null, "sms");
		//开始 address 标签
		serializer.startTag(null, "address");
		//在 address 标签中间写入文本
		serializer.text("" + (5554 + i));
		//结束 address 标签
		serializer.endTag(null, "address");
		//开始 body 标签
		serializer.startTag(null, "body");
		//在 body 标签中间写入文本
		serializer.text("我是内容<>" + i);
		//结束 body 标签
		serializer.endTag(null, "body");
		serializer.startTag(null, "time");
		serializer.text(new Date().getTime()+"");
		serializer.endTag(null, "time");
		//结束 sms 标签
		serializer.endTag(null, "sms");
	}
	//结束 smses 标签
	serializer.endTag(null, "smses");
	//结束文档
	serializer.endDocument();
	fos.close();
}
```

## 使用 Pull 解析 Xml 格式数据
asserts 资源目录中的文件只能读不能写，多用于随 apk 一起发布的固定不变的数据，比如可以将国内所有的城市列表放在里面。

MainActivity.java 代码片段

```java
//使用 pull 解析 xml 数据
public void click3(View v) throws Exception {
/* 将解析出来的数据封装在 Sms 中，然后保存到 ArrayList 中
* Sms 是自定义的一个 JavaBean	*/
    ArrayList<Sms> smses = null;
    Sms sms = null;
    //调用父类提供的 getAssets（）方法获取 AssertManager 对象
    AssetManager assetManager = getAssets();
    //使用 assetManager 的 open 方法直接获取输入流对象
    InputStream inputStream = assetManager.open("info2.xml");
    //通过 Xml 的静态方法获取 Xml 解析器
    XmlPullParser parser = Xml.newPullParser();
    //设置输入流和编码
    parser.setInput(inputStream, "utf-8");
    //获取事件类型
    int event = parser.next();
    //如果没有解析到文档的结尾，则循环解析
    while (event != XmlPullParser.END_DOCUMENT) {
        //获取当前解析到的标签名称
        String tagName = parser.getName();
        //如果是“开始标签”事件
        if (event == XmlPullParser.START_TAG) {
            //判断当前解析到的开始标签是哪个
            if ("smses".equals(tagName)) {
                smses = new ArrayList<Sms>();
            } else if ("sms".equals(tagName)) {
                sms = new Sms();
            } else if ("address".equals(tagName)) {
                sms.setAddress(parser.nextText());
            } else if ("body".equals(tagName)) {
                sms.setBody(parser.nextText());
            } else if ("time".equals(tagName)) {
                sms.setTime(parser.nextText());
            }
            //如果是“结束标签”事件
        } else if (event == XmlPullParser.END_TAG) {
            if ("sms".equals(tagName)) {
                //如果是 sms 结尾，则将创建的 sms 对象添加到集合中
                smses.add(sms);
            }
        }
        //继续获取下一个事件
        event = parser.next();
    }
    inputStream.close();
    //将数据展示到界面
    showSms(smses);
}

/*
* 将短信显示到 TextView 中
*/
private void showSms(ArrayList<Sms> smses) {
    StringBuilder sb = new StringBuilder();
    for (Sms s : smses) {
        sb.append(s.toString() + "\n");
    }
    tv_sms.setText(sb.toString());
}
```

注意：
在上面的代码中我们不仅学到如何解析 xml，还学到了如何读取 aserts 目录中的数据。

## Pull 解析和 SAX 解析对比
Pull 解析器的运行方式与 SAX 解析器相似，都属于事件驱动模式。它提供了类似的事件，如：开始元 素和结束元素事件，使用 parser.next()可以进入下一个元素并触发相应事件。事件将作为数值代码被发送， 因此可以使用一个 switch 对感兴趣的事件进行处理。当元素开始解析时，调用 parser.nextText()方法可以获 取下一个 Text 类型元素的值。

SAX 解析器的工作方式是自动将事件推入事件处理器进行处理，因此你不能控制事件的处理主动结 束；
而 Pull 解析器的工作方式为允许你的应用程序代码主动从解析器中获取事件，正因为是主动获取事件， 因此可以在满足了需要的条件后不再获取事件，结束解析。




