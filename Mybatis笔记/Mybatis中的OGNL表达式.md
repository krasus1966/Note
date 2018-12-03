# Mybatis中的OGNL表达式

| 取值范围 |     标签的属性中      |                               |
| :------: | :-------------------: | :---------------------------: |
| 取值写法 | String与基本数据类型  |        **_parameter**         |
|          | 自定义类型（Message） |       属性名（command）       |
|          |         集合          |        数组：**array**        |
|          |                       |        List：**list**         |
|          |                       |      Map：**_parameter**      |
|  操作符  |    java常用操作符     | +、-、*、/、==、！=、\|\|、&& |
|          |   自己特有的操作符    |   and、or、mod、in、not in    |

| ---                  | ---  | ---                                                 |                  |
| -------------------- | ---- | --------------------------------------------------- | ---------------- |
| 从集合中取出一条数据 | 数组 | array[索引]  (String[])                             |                  |
|                      |      | array[索引] (Message[])                             |                  |
|                      | List | list[索引] (List< String>)                          |                  |
|                      |      | list[索引].属性名(List< Message>)                   |                  |
|                      | Map  | _parameter.key(Map<String,String>)                  |                  |
|                      |      | key.属性名(Map<String,Message>)                     |                  |
| 利用foreach标签      |      | < foreach collection="array" index="i" item="item"> |                  |
| 从集合中取出数据     | 数组 | i:索引（下标）                                      | item/item.属性名 |
|                      | List | i:索引（下标）                                      | item/item.属性名 |
|                      | Map  | i:key                                               | item/item.属性名 |

