# Flask

#### Flask基础

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

@app.route("/goods/<int:goods_id>") #不加转换器类型，默认是普通的字符串规则（除了斜线以外的字符）
def goods_detail(goods_id):
    return "goods detail page %s" % goods_id 

#自定义转换器
# 1.定义自己的转换器
class RegexConverter(BaseConverter):
    def __init__(self,url_map,regex):
        # 调用父类的初始化方法
        #super(RegexConverter,self).__init__(url_map) #python2.x必须写
        super()
        # 将正则表达式使用的参数保存到对象的属性中，flask会去使用这个属性来进行路由的正则匹配
        self.regex = regex
        
#regex=正则表达式
class MobileNum(BaseConverter):
    def __init__(self,url_map):
        super()
        self.regex = r'1[34578]\d{9}'

# 2.将自定义的转换器添加到flask的应用中
app.url_map.converters["re"] = RegexConverter
app.url_map.converters["mobile"] = MobileNum
# 不加转换器类型，默认是普通的字符串规则（除了斜线以外的字符）
@app.route("/send/<re(r'1[34578]\d{9}'):mobile>")
def send_sms(mobile):
    return "send sms to %s" % mobile

#regex=正则表达式
@app.route("/send2/<mobile:mobile_num>")
def send_sms1(mobile_num):
    return "send sms to %s" % mobile_num


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

> postman:用来给后端提供数据



#### request

```python
#!/usr/bin/env python 
# -*- coding:utf-8 -*-
from flask import Flask,request

app = Flask(__name__)

@app.route("/index",methods = ["GET","POST"])
def index():
    # request中包含了前端发送过来的所有请求数据
    # form和data是用来提取请求体数据
    # 通过request.form可以直接提取请求体中的表单格式的数据，是一个类字典的对象
    # 通过get方法只能拿到多个同名参数的第一个
    name = request.form.get("name")
    age = request.form.get("age")

    # args是用来提取url中的参数(查询字符串)
    city = request.args.get("city")
    return "hello name=%s,age=%s,city=%s" % (name,age,city)

if __name__ == '__main__':
    app.run(debug=True)
```

#### with

````python
# 上下文管理器
# with open("./1.txt","wb") as f:
#     f.write("Hello flask")

class Foo(object):
    def __enter__(self):
        """进入with语句的时候被调用"""
        print ("enter called")

    def __exit__(self,exc_type,exc_val,exc_tb):
        """离开with语句的时候被with调用"""
        print("exc_type: %s" % exc_type)
        print("exc_val: %s" % exc_val)
        print("exc_tb: %s" % exc_tb)

with Foo()as foo:
    print("hello python")
````

#### abort

```python
from flask import Flask, request, abort, Response
app = Flask(__name__)
@app.route("/login", methods=["GET"])
def login():
    # name = request.form.get("name")
    # pwd = request.form.get()
    name = ""
    pwd = ""
    if name != "zhangsan" and pwd != "admin":
        # 使用abort函数可以立即终止视图函数的执行
        # 并可以返回给前端特定的信息
        # 1.传递状态码信息
        abort(404)
        # 2.传递响应体信息
        # resp = Response("Login Failed")
        # abort(resp)
    return "login success"
# 定义错误处理方法
@app.errorhandler(404)
def handle_404_error(err):
    """自定义的处理错误方法"""
    # 这个函数的返回值会是前端用户看到的最终结果
    return u"出现了404错误，错误信息 %s" % err
if __name__ == '__main__':
    app.run(debug=True)

```

#### response

```python
from flask import Flask, request, abort, Response,make_response
app = Flask(__name__)
@app.route("/index")
def login():
    # 1.使用元祖，返回自定义的响应信息
    #        响应体                状态码 响应头
    # return "index page", 400, [{"Itcast","python"},{"city","shenzhen"}]
    # return "index page",400,{"Itcast1":"python1","City1":"sz1"}
    # return "index page","666 itcast status", {"Itcast1":"python1","City1":"sz1"}
    # 2.使用make_response 来构造响应信息
    resp = make_response("index page 2")
    resp.status = "999 itcast" # 设置状态码
    resp.headers["city"] = "sz" # 设置响应头
    return resp
if __name__ == '__main__':
    app.run(debug=True)
```

#### json

```python
from flask import Flask, jsonify
import json
app = Flask(__name__)
@app.route("/index")
def index():
    # json 就是字符串
    data = {
        "name": "python",
        "age": 18
    }
    # json.dumps(字典) 将python的字典转换为json字符串
    # json.loads(字符串) 将字符串转换为python中的字典
    # json_str = json.dumps(data)
    # return json_str, 200, {"Content-Type": "application/json"}
    # jsonify()帮助我们转为json数据，并设置响应头 Content-Type 为 application/json
    #return jsonify(data)
    return jsonify(city="sz",country="China")
if __name__ == '__main__':
    app.run(debug=True)
```

#### Templates

```python
from flask import Flask, render_template
app = Flask(__name__)
@app.route("/index")
def index():
    # index.html 要放到templates文件夹中，flask默认在这个文件夹中渲染模板
    return render_template("index.html", name="python", age=18)
if __name__ == '__main__':
    app.run(debug=True)
```

```python
from flask import Flask, render_template
app = Flask(__name__)
@app.route("/index")
data = {
    "name" = "python",
    "age" = 18
}
def index():
    return render_template("index.html", **data) # **代表解包，解包字典
if __name__ == '__main__':
    app.run(debug=True)
    
html:
        {# 过滤器，直接    | 过滤器  ，可以加多个#}
    <p>{{ "   flask world    " | trim }}</p>
            
def list_step_2(li):
    """自定义过滤器"""
    return li[::2]
# 注册过滤器
app.add_template_filter(list_step_2, "li2")

            
# 装饰器定义自定义过滤器            
@app.template_filter("li2")
def list_step_2(li):
    """自定义过滤器"""
    return li[::2]
```

