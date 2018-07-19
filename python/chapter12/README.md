#常用内建模块

##datetime

    from datetime import datetime, timedelta
    
    # 获取当前时间
    print(datetime.now())
    
    # 获取指定日期和时间
    print(datetime(2018, 7, 19, 17, 49, 53))
    
    # 获取当前时间的timestamp，浮点数，小数点后的代表毫秒数
    print(datetime.now().timestamp())
    
    # 把timestamp转换为datetime
    print(datetime.now())
    timestamp = datetime.now().timestamp()
    print(datetime.fromtimestamp(timestamp))
    
    # 把timestamp转换为utc标准时区的时间
    print(datetime.utcfromtimestamp(timestamp))
    
    # str转换为datetime
    print(datetime.strptime('2018-07-19 18:06:05', '%Y-%m-%d %H:%M:%S'))
    
    # datetime转换为str
    print(datetime.now().strftime('%Y-%m-%d %H:%M:%S'))
    
    # 对时间进行加减
    print(datetime.now() + timedelta(days=10))
    
##collections

    from collections import namedtuple
    
    # namedtuple是一个函数，用来创建一个自定义的tuple对象，且规定了tuple元素的个数，并可以用属性而不是索引来引用tuple的某个元素
    Point = namedtuple('Point', ['x', 'y'])
    p = Point(1, 2)
    print(p.x, p.y)
    
    from collections import deque
    
    # deque是为了高效实现插入和删除操作的双向列表，适合用于队列和栈
    q = deque([1, 2, 3])
    q.append(4)
    q.appendleft(0)
    print(q)
    
    from collections import defaultdict
    
    # defaultdict用来在dict中不存在key时返回一个默认值
    dd = defaultdict(lambda: 'N/A')
    dd['key1'] = 'abc'
    print(dd['key1'])
    print(dd['key2'])
    
    from collections import OrderedDict
    
    # dict,key是无序的，如果要有序，使用OrderDict
    od = OrderedDict(dict([('a', 1), ('b', 2), ('c', 3)]))
    print(list(od.keys()))
    
    from collections import Counter
    
    # Count是一个计数器
    c = Counter()
    for ch in 'programming':
        c[ch] = c[ch] + 1
    
    print(c)
    


