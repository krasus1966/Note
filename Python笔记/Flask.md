# Flask

```python
from flask import Flask

# 创建flask的应用对象
# __name__表示当前的模块名字
# 模块名，flask以这个模块所在的目录为总目录，默认这个目录中的static为静态目录，templates为模板目录
app = Flask(__name__,
           static_url_path="/python", #访问静态资源的url前缀，默认值是static 
           static_folder="static", #静态文件的目录，默认static
           template_folder="templates" #模板文件的目录，默认是templates
           ) 

# 配置参数的使用方式
# 1.使用配置文件
#app.config.from_pyfile("config.cfg")
# 2.使用对象方式
#class Config(object):
#    DEBUG = True
#app.config.from_object(Config)
# 3.直接操作config
#app.config["DEBUG"] = True

#通过methods限定访问方式
@app.route("/post_only",methods=["POST"])
def post_only():
    return "post only page" 

# 使用url_for的函数通过视图函数的名字找到试图对应的url路径
# 跳转
@app.route("/login")
def login():
    url = url_for("hello_world")
    return redirect(url)

@app.route("/")
def index():
	"""定义的视图函数"""
	return "hello flask"
if __name__='__main__':
	#启动flask程序
	app.run()
    #app.run(host="192.168.1.1",port="5000",debug=True)
```

>*pycharm* 中，修改post和host要在Edit Configurations中的Application中添加**--host=""**,**--post=""**来修改。

