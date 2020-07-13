# Mybatis笔记

<!-- Mybatis的动态代理开发规则 -->
1.namespace必须是接口的全路径名
2.接口的方法名必须与sql id 一致
3.接口的入参必须与parameterType类型一致
4.接口的返回值必须与resultType类型一致

```
@Test 
public void testGetUserById(){
    SqlSession sqlSession = SqlSessionFactoryUtils.getSqlSessionFactory().openSession();
    //获取接口的代理实现类
    UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
    
    userMapper.getUserById(id);
    sqlSession.close();
}
```





