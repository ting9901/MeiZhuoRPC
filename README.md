# MeiZhuoRPC
java远程调用框架
## 版本1.0 实现思路
- 实现端spring配置全局的map 维护抽象接口与实现的映射关系
- 利用ApplicationContextAware接口 获取当前的spring容器
- 调用端动态代理要使用的抽象接口 使调用端使用接口方法时忽略底层细节
- 代理回调handler里把对象的接口(方法名,参数等)通过网络IO发给实现端
- 调用端接受到参数，在map里找到对应的实现类的信息 反射调用相应类的方法
- 调用端把返回值解码后把结果传输回实现端
- rpc默认是同步调用 全局维护一个map 映射请求ID与对应线程生成的锁的Condition
- 在代理对象的invoke里 判断返回结果是否返回,否则则用Condition挂起(设超时时间),等待异步调用返回后释放锁，再继续执行