### Electron中使用nodemon的问题

> 只出现在Windows平台中，在使用 nodemon --watch main.js --exec "electron ." 时，出现electron不是内部或外部命令，也不是可运行的程序或批处理文件，解决办法如下：

在package.json文件中，修改script下的start：

```js
"scripts": {
    "start": "nodemon --watch main.js --exec \"electron .\""
  },
```

老师提供的解决方案：

```
"scripts": {
"start": "nodemon --watch main.js --exec \"npm run dev\"",
"dev": "electron ."
},
```



