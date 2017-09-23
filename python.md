1. 日常开发都用到哪些python内置的模块
    ```
    1.打包和备份shutil.make_archive
    2.多线程 threading
    3.多进程 multiprocesssing
    4.队列 Queue
    5.HTTP协议服务器 SimpleHTTPServer
    6.系统相关 os.getenv('PYTHONPATH')
    7.time
    8.random
    ```




2. 最熟悉的一个项目,遇到的问题,怎么解决,开发周期,他人协作

3. 可变与不可变类型:
    ```
    可变:list dict 不可变: int str float tuple; python 中的不可变数据类型,不允许变量值发生变化,如果变量值变化,就会新建一个对象,相同值的对象,内存中引用计数会增加.可变数据类型,允许变量值变化,增删不会创建新的对象,变量引用对象地址不变,变的只是变量中引用对象的地址,没有引用计数.
    ```
    
4. 浅拷贝和深拷贝:
    ```
    python中对象的赋值都是对象的引用.
        浅拷贝 就是快捷方式,只是增加一个引用,指向对象,对象内部的元素变化,也会变化
        深拷贝是复制,吧他最底层的对象都复制一份. 直接指向的是最底层每个元素,所以源对象再变,新的对象也不会变的
        
    # copy() 和 deepcopy()
    
    ```
    
5. python __init__和__new__之间的区别
    ```
    __init__是定义了对象的初始化状态,不返回对象
    __new__是创建一个新对象,会隐士调用__init__,返回对象
    创建对象时后者优先调用
    ```
6. 你知道几种设计模式:
    
    ```
    1.简单工厂模式,
    
    2.单例模式
    
    3.生成器
    ```
7. 实例方法和静态方法.类方法
    ```
    class A():
        # 实例方法,只有实例可以用
        def foo(self,x):
            do something
        
        
        # 静态方法,类和实例否可以调用,就一普通方法
        @staticmethod
        def foo(x):
            do something
        
        # 类方法,类和实例都可以用
        @classmethod
        def foo(cls,x)
            do something
    ```
8. 类变量和实例变量
    ```
    类变量类似全局变量,所有实例都可以用
    
    而实例变量只能单个实例使用
    ```
9. python中单下划线和双下划线
    ```
    都是一种约定
    单下划线一般说明为私有变量
    双下划线是python内部的名字  区别于自定义名字
    ```
10. 占位符 % 和{} 区别
    ```
    % 只能传一个简单的类型 
    .format 可以传
    ```
11. **迭代器和生成器**
    ```
    Iterables(迭代器):一般的拥有next()和iter()方法就可以成为一个迭代器了.比如一个列表
    Generators(生成器):是一种特殊的迭代器,并会将全部数据加载到内存,只有在调用时,才在内存中生成
    yield:与return类似 调用函数时并不会直接返回结果,而是返回生成器对象,当碰到yield才会返回一个值
    
    ```
    
12. 合并两个字典
    ```
    python2
    dict(a.items()+b.items())
    python3
    dict(list(a.items())+list(b.items()))
    或者
    z  = a.copy()
    z.update(b)
    ```
13. **装饰器**
    ```
    装饰器的作用是:已经存在的对象添加额外的功能
    用途:日志,性能测试,事务处理,登录判断
    ```
14. 单例模式
    ```python
    #
    #单利模式是一种常见的软件设计模式,通过单例模式可以保证系统中一个类只能生成一个实例,并且易于访问.
    #
    #__new__在__inte__之前被调用,可以通过此方法设计单例模式
    #
    #1.使用__new__方法:
    
    
    class Car(object):
        _first_new = True #默认第一次创建
        _instanct = None # 没有实例
        _first_init = True #默认第一次init成立
        
        def __new__(cls,*args,**kwargs):
            if _first_new:
                cls._first_new = False
                cls._instance = object.__new__(cls)
            return cls._instance
        
        def __init__(self, new_name):
            if Car._first_init:
                Car._first_init = False
                self.new_name = new_name
                
            
    ######2.使用字典特性:
    
        class Car(object):
            _state = {}

            def __new__(cls, *args, **kwargs):
                ob = super(Car, cls).__new__(cls, *args, **kwargs)
                ob.__dict__ = cls._state 所有的内置属性都指向初次对象的 属性
                return ob
                
                
    # 3. 装饰器版本
        
    def first_new(cls, *args, **kwargs):
        instance = {}
        def get_instance():
            if cls not in instance:
                instance[cls] = cls(*args, **kwargs)
                return instance[cls]
    
        return get_instance
        
    @first_new
    class Car:
        pass
        
        
    # 4. import 方法
    把类封装成一个car.py包
    first_car = Car()
    
    from car import first_car
    ```
15. Python中的作用域:
    ```
    # LEGB法则: 
    本地作用域(Local)-->当前作用域外层(被嵌入的作用域//外层包裹函数Enclosing local)-->全局/模块(Global)-->内建(Build-in)
    优先在局部变量值中找,
    ```
16. GIL线程全局锁
    ```
    一个cpu在同一时间只能执行一个线程,IO密集型多线程有用,**多核CPU对多线程来说没任何鸟用 因为释放GIL后 多核竞争导致 线程颠簸**,建议使用多进程
    ```
17. 协程
    ```
    #
    #线程和进程面临内核态 和 用户态 切换 消耗大量时间,协程用户自己控制,yield就是协程的思想
    #
    #生产者消费者模式 协程版
    def consumer(): # 消费者
        r = ''
        while True:
            n = yield r # 传给生产者
            if not n:
                return # 终止
            do_something
            r = '200'
            
    def produce(c): # 生产者
        c.next() # 通过next()启动生成器 拿到结果n
        n = 0
        while n < 5: # 生产者开始造东西
            n += 1
            do_something
            r = c.send(n) # 切换到消费者
            do_otherthing
        c.close() # 不生产了 关闭
        
    if__name__ == '__main__':
    c = consumer()
    produce(c)
    
    生产者通过next() 启动生成器,
    当生产下东西,send()切换到消费者,消费者通过yield拿到消息 ,再通过yield传回,
    生产者拿到消费者结果接着生产,不想生产就close()
    ```
18. 闭包
    > 创建一个闭包,需要的三个条件是:
        1.必须是一个内嵌的函数
        2.内嵌函数必须引用外部的参数
        3.外部函数返回值必须是内部函数
19. lambda 匿名函数
    ```
    lambda x : x+2
    # 等价于
    def add(x):
        return x+2
    ```
20. python函数式编程
    ```
    # 1.filter函数 过滤
        a = [1,2]
        b = filter(lambda x: x>1, a)
        #b=[2]
        
    # 2.map函数 依次执行
        a = map(lambda x:x*2,[1,2])
        #a = [2,4]
        
    # 3.reduce函数
        
    ```
21. python垃圾回收机制
    ```
    #python使用**引用计数**来跟踪和回收垃圾.通过**标记,清除**解决可能的循环引用问题,**分代回收**空间换时间
    #
    #1.引用计数,增加引用,计数增加;减少引用,计数减少;计数为0,对象消失
        优点:简单,实时性; 缺点:维护循环引用消耗资源,
    #
    #2.标记-清除
    把所有可访问到的打标记,没标记的释放
    #
    #3.分代回收
    
    
    
    ```