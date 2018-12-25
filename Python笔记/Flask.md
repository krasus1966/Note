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