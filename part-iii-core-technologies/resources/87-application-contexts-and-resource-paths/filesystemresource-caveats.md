#Q### FileSystemResource caveats

FileSystemResource不是和FileSystemApplicationContext连在一起（因为FileSystemApplicationContext不是一个实际的ResourceLoader）将会按你的期望处理绝对路径和相对路径。相对路径相对于当前的工作路径，当绝对路径是相对于文件系统的根路径。

为了向后兼容（历史原因）：FileSystemApplicationContext是ResourceLoader。FileSystemApplicationContext强制所有和FileSystemResource实例相关来处理所有路径不管他们是否以斜线开始。实际，这意味着下面是一样的：

```
ApplicationContext ctx =
new FileSystemXmlApplicationContext("conf/context.xml");
ApplicationContext ctx =
new FileSystemXmlApplicationContext("/conf/context.xml");
```

如下（尽管他看起来是不同的，其中一个是相对的而另一个是绝对的）

```
FileSystemXmlApplicationContext ctx = ...;
ctx.getResource("some/resource/path/myTemplate.txt");
FileSystemXmlApplicationContext ctx = ...;
ctx.getResource("/some/resource/path/myTemplate.txt");
```

实际中，绝对的文件系统路径是需要的，最好放弃使用绝对路径在FileSystemResource或FileSystemXmlApplicationContext，只是强制使用UrlResource通过使用URL前缀。

```
// actual context type doesn't matter, the Resource will always be UrlResource
ctx.getResource("file:///some/resource/path/myTemplate.txt");
```

```
// force this FileSystemXmlApplicationContext to load its definition via a UrlResource
ApplicationContext ctx =
new FileSystemXmlApplicationContext("file:///conf/context.xml");
```

