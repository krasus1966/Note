### Thymeleaf打包后出现模板找不到的问题

在使用IDEA开发的过程中没有出现找不到模板的问题，然而打包为Jar、War包之后，运行并访问，出现模板不存在：

```
org.springframework.web.util.NestedServletException: Request processing failed; nested exception is org.thymeleaf.exceptions.TemplateInputException: Error resolving template [/fragments/fragments], template might not exist or might not be accessible by any of the configured Template Resolvers (template: "error/500" - line 13, col 6)
```

通过查询发现，在jar的模式下，视图解析出现异常，主要发生在返回视图路径为 "/" 开头的页面，或引用页面以 "/" 开头，而在IDEA开发时不会出现此问题，在templateResolution的resource属性全路径是 templates// ，这个文件路径下是肯定找不到视图文件的。

> 因为idea在启动时，是从文件系统中加载资源，对于`//`,文件系统是支持这种寻址的，但是在jar的内部资源加载就无法使用这种模式