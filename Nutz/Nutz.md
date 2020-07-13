# Nutz

### 将Spring对应内容替换为Nutz

```java
@Controller = @At 
@RequestMapping = @At
@GetMapping = @At @GET @Ok(跳转view或其他)
@RestController = @At @Ok("json") 返回值是json格式
return "redirect:路径" = @Ok("redirect:路径") //@Ok(">>:路径") >> 等价于redirect
@Ok("http:状态码") 返回状态码信息
@Autowired @Resource @Qualifier =  @Inject
```

##### Nutz中：

```java
public class MainModule {
@At("/post")
@Ok("json")
public User post(@Param("username") String username,
                 @Param("password") String password,
                 HttpServletRequest request,
                 HttpSession session,
                 AdaptorErrorContext errorContext
                 ){
       User user = new User();
       user.setUsername(username);
       user.setPassword(password);
       session.setAttribute("user",user);
       return user;
	}
}
```

##### 等于Spring中：

```java
@RestController
public class MainController{

	@Autowired
	private User user;
	
	@RequestMapping
	public String doHello(@Param("username") String username,
                     @Param("password") String password,
                     Model model
                     ){
        user.setUsername(username);
        user.setPassword(password);
        model.addAttribute("user",user);
        return user;
	}
}
```



### Nutz中的数据库操作：

##### Nutz中的IOC使用js作为配置文件，首先要在配置文件中添加对数据库连接池的内容

```js
/* 
	官方文档称，var ioc 只是为了让Eclipse进行格式化
*/
var ioc = {
    dataSource : {
        /*
        	使用阿里的连接池Druid
        */
        type : "com.alibaba.druid.pool.DruidDataSource",
            events : {
            depose : 'close'
        },
        fields : {
            url : "jdbc:mysql://localhost:3306/nutzdemo",
                username : "root",
                password : "196610121",
                maxWait: 15000, // 若不配置此项,如果数据库未启动,druid会一直等可用连接,卡住启动过程
                defaultAutoCommit : false // 提高fastInsert的性能
        }
    }
};
```

##### 注入Ioc并执行操作：

```java
Ioc ioc = new NutIoc(new JsonLoader("dao.js"));
DataSource ds = ioc.get(DataSource.class);//注入连接池配置
Dao dao = new NutDao(ds);//创建dao
dao.create(Person.class,false);//创建person表
Person person = new Person();
person.setName("ABC");
person.setAge(20);
dao.insert(person);//插入person
System.out.println(person.getId());
```

##### Pojo类：

```java
@Data //lombok
@Table("t_person") //表名
public class Person {

    @Id // 定义为整数型主键 还有@Name 字符型主键（非空唯一） @PK 复合主键
    @Prev(els=@EL("uuid(32)")) //自增规则 可以是uuid() uuid(32) 也可以自定义
    private int id;

    @Name // @Name(casesensitive = false) 忽略大小写
    @ColDefine(width = 8)
    private String name;

	@PK 复合主键 @PK({"masterId","petId"})
	private int masterId;
	private int petId;

    @Column
    private int age;

    @Column
    @ColDefine(customType = "TEXT", type = ColType.VARCHAR,width = 1024)
    private String textBlob;
}

@Table("t_pet")
@PK({"masterId","petId"}) //@PK 复合主键 
public class Pet{
    private int masterId;
    private int petId;
}
//获取
Pet pet = dao.fetchx(Pet.class, 23, 12);
//删除 fetch 23 delete 12
Pet pet = dao.deletex(Pet.class, 23, 12);
```

##### 复杂SQL查询：

```java
//Cnd 拼接
Condition c = Cnd.where("age",">",30).and("name", "LIKE","%K%").asc("name").desc("id");
List<MyObj> list = dao.query(MyObj.class, c, null);

// Criteria 接口
//LT : 小于 (LessThan)
//GTE : 大于等于 (GreatThanEqual)
//LTE : 小于等于 (LessThanEqual)
Criteria cri = Cnd.cri();
criteria.where().andIn().andLT().andLike();
List<MyObj> list = dao.query(MyObj.class, cri, null);
//模糊查询的正确写法
Cnd cnd = Cnd.where("name", "like", "%" + str + "%s");
```

##### 分页查询：

```java
QueryResult orderList = orderDao.getOrderList();
System.out.println(orderList.getList()+"\n"+orderList.getPager());

public QueryResult getOrderList(){
        Ioc ioc = new NutIoc(new JsonLoader("dao.js"));
        DataSource ds = ioc.get(DataSource.class);
        Dao dao = new NutDao(ds);
        Pager pager = dao.createPager(1,10);
        List<Order> list = dao.query(Order.class,null,pager);
        pager.setRecordCount(dao.count(Order.class));
        return new QueryResult(list,pager);
}
```

##### 过滤字段：

```java
//过滤字段使用FieldFilter类
 FieldFilter fieldFilter1 = FieldFilter.create(Object.class,actived "正则表达式，被操作的字段",locked "正则表达式，被忽视的字段",boolean 是否忽视为空字段);
//过滤除id和orderName以外的字段
FieldFilter fieldFilter = FieldFilter.create(Order.class,"^id|orderName");
//过滤id和orderName两个字段
 FieldFilter fieldFilter1 = FieldFilter.locked(Order.class,"^id|orderName");
 
 FieldMatcher make(String actived, //保留字段
 					String locked, //忽略字段
 					boolean ignoreNull, //是否忽略空值
                    boolean ignoreZero, //是否忽略零值
                    boolean ignoreDate, //是否忽略java日期类和其子类
                    boolean ignoreId,//是否忽略@Id注解标注的属性
                    boolean ignoreName,//是否忽略@Name标注的属性
                    boolean ignorePk//是否忽略@Pk标注的属性
                    )
```

##### 动态表名：

```java
 @Table("t_employee_${cid}")
 //cid 是 TableName.run(3,)中的3
 
 TableName.run(3, new Runnable() {
            @Override
            public void run() {
                dao.create(Employee.class,true);
                Employee employee = new Employee();
                employee.setName("PETER");
                employee.setLocation("这是地址");
                dao.insert(employee);

                Employee peter =dao.fetch(Employee.class,"peter");
                System.out.println(peter);
            }
        });
```

##### 一对一映射：

在通常情况下，雇员存在一个管理者Manager，因此应设置一对一映射，修改Employee类：

```java
Employee类为关联类，Manager为被关联类
 @Column
private long managerId;

@One(target = Manager.class,field = "managerId",key = "id")
private Manager manager;
```

在dao中使用以下方法来实现一对一关联操作：

```java
Employee employee = new Employee();
employee.setName("CHARE");
employee.setLocation("这是地址");
employee.setManagerId(1);
Manager manager = new Manager();
manager.setName("Test");
manager.setLocation("Location");
employee.setManager(manager); //将被关联类对象放入
//插入
dao.insertWith(employee,"manager"); //insertWith会将相关联内容也插入对应表中
dao.insertLinks(employee,"manager"); //insertLinks不会存employee,会存manager
//更新
dao.updateWith(employee,"manager");//同时更新employee和manager
dao.updateLinks(employee,"manager");//只更新manager
//删除
dao.deleteWith(employee,"manager");//删除employee和其对应的manager
dao.deleteLinks(employee,"manager");//删除manager而不删除employee
dao.clearLinks(employee,"manager");//清除manager(会删除manager对应的行，但是employee中对应的managerId不会删除)
//查询
Employee employee = dao.fetch(Employee.class,"查询@Name");
dao.fetchLinks(employee,"manager");//=employee.setManager(dao.fetch(Manager.class,employee.getManagerId))
System.out.println(employee);//会打印出Employee(包括其中的manager)
//查询化简
Employee employee = dao.fetchLinks(dao.fetch(Employee.class,"查询@Name"),"manager");
```

##### 一对多映射：

在通常情况下存在一个管理者管理多个雇员的情况，对Manager进行改写：

```java
@Many(target = Employee.class,field = "masterId",key = "id")
private List<Employee> employees;
```

一对多的数据库操作：

```java
//插入 多表
Manager manager = new Manager();
manager.setName("管理者");
manager.setLocation("地址");
List<Employee> employees = new ArrayList<Employee>(4);
employees.add(new Employee("测试7","地点"));
employees.add(new Employee("测试8","地点2"));
manager.setEmployees(employees);
dao.insertWith(manager,"employees");//两个表都会插入数据
//插入 仅employee表
Manager manager = dao.fetch(Manager.class,1);
List<Employee> employees = new ArrayList<Employee>(4);
employees.add(new Employee("测试7","地点"));
employees.add(new Employee("测试8","地点2"));
manager.setEmployees(employees);
dao.insertLinks(manager,"employees");//仅插入employee表，前提，必须搜索manager的值后，通过此manager插入，否则插入后employee的managerId为0

//更新 更新操作必须先查询
dao,updateWith(manager,"employees");//更新两个表
dao.updateLinks(manager,"employees");//仅更新employee表
//删除 删除操作必须先查询
dao.deleteWith(manager,"employees");
dao.deleteLinks(manager,"employees");//循环删除
dao.clearLinks(manager,"employees");//DELETE FROM t_employee_ WHERE managerId=3 即通过条件直接删除全部employee

//查询
Manager manager = dao.fetch(Manager.class,1);
dao.fetchLinks(manager,"employees");
//查询优化
Manager manager = dao.fetchLinks(dao.fetch(Manager.class,1));
```

##### 多对多映射：

每个员工都有自己喜欢的食物，可以重复，要使用多对多映射:

```java
//对Employee进行修改
//添加此注解的为宿主对象
@ManyMany(relation = "t_employee_food",
        from="eid", //宿主对象id 如果不想用id就 eid:name 会找@Name
        to="fid" //目标对象id
)
private List<Food> foods;
```

多对多映射的数据库操作：

```java
//Employee或者Food哪一方执行都可以
//从Employee执行
//插入
Employee employee = dao.fetch(Employee.class,27);
Food food = dao.fetch(Food.class,1);
List<Food> foods = new ArrayList<>();
foods.add(food);
employee.setFoods(foods);
dao.insertRelation(employee,"foods");//仅插入 t_employee_food表 增加二者关系
Employee employee = dao.fetch(Employee.class,27);
List<Food> foods = new ArrayList<>();
foods.add(new Food("肉"));
employee.setFoods(foods);
dao.insertLinks(employee,"foods");// 仅插入关系表和food表
dao.insertWith(employee,"foods");//都插入

//更新
dao.updateLinks(employee,"foods");// 仅更新food
dao.updateWith(employee,"foods"); //都更新

//删除
dao.deleteWith(employee,"foods");//删除三个表的对应内容
dao.deleteLinks(employee."foods");//删除food表和关系表内容
dao.clearLinks(employee,"foods");//仅删除t_employee_food表的内容，其他两个表不会进行操作（推荐）
    
//查询
Food food = dao.fetch(Food.class, "Fish");
dao.fetchLinks(food, "employees");
//查询优化
Food food = dao.fetchLinks(dao.fetch(Food.class, "Fish"), "employees");
```

#### SQL总结

- 在映射关系上，尽量不要使用同增的 xxxxWith()方法进行操作，会同时操作两张表内容，容易出错。
- 多对多映射的clearLinks()方法不会操作两张表，仅操作关系表，其他的则会操作两张表
- xxxxLinks()只会操作目标表，不会操作当前表。



### 事务处理

```
Trans.exec(level事务隔离等级，默认TRANSACTION_READ_COMMITTED,new Atom() {
     @Override
     public void run() {
        //需要进行事务处理的内容        
    }
});
```

```
//交叉事务处理
函数 A
    Trans.exec(new Atom(){
        public void run(){
            数据操作 1;
            数据操作 2;
        }
    });

函数 B
    Trans.exec(new Atom(){
        public void run(){
            数据操作 3;
            -> 函数 A();
        }
    });

函数 C
    Trans.exec(new Atom(){
        public void run(){
            数据操作 4;
            -> 函数 A();
        }
    });
// 只有最外层的事务会生效，内层事务不会生效，可以去掉函数A的事务处理
```

### IOC容器

```java
// 在主Module上添加@IocBy注解
@IocBy(type = ComboIocProvider.class,args = {"*js","ioc/","tx"，"*jedis"})//猜测会扫描此路径下的所有内容 tx为AOP支持下的事务管理 *jedis为redis支持
```
