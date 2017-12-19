# Lua 基本语法

## 注释

单行注释

    --

多行注释

    --[[
      多行注释
      多行注释
     --]]
    推荐:
        --[=[注释内容]=]

## 数据类型

nil、boolean、number、string、userdata、function、thread和table

字符串赋值:

    string1 = "lol hahaha"  -- 字符串表示
    string2 = [[
        <html>
            hahaha
        </html>
    ]]

    print('lol' .. 233) -- 字符串连接
    print(#'lol')   -- #用来计算字符串长度

字符串操作:

    string.upper
    string.find
    string.reverse
    string.len

## Lua 变量

    a = 1              -- 全局变量

    function foo()
        b = 2          -- 全局变量
        local c = 3    -- 局部变量
    end

赋值语句

    a, b = 1, 2        -- a = 1; b = 2
    x, y = y, x        -- swap x for y

    a, b, c = 0, 1
    print(a,b,c)             --> 0   1   nil

    a, b = a+1, b+1, b+2     -- value of b+2 is ignored
    print(a,b)               --> 1   2

    a, b, c = 0
    print(a,b,c)             --> 0   nil   nil

## Lua 循环

while 循环

    while (a < 10)
    do
        print(a)
        a = a + 1
    end

for 循环

    for i = 1, 5 do         -- 1 2 3 4 5
        print(i)
    end

    for i = 5, 1, -1 do     -- 5 4 3 2 1
        print(i)
    end

    --打印数组a的所有值
    for i,v in ipairs(a)
        do print(v)
    end

repeat 循环

    a = 10
    --[ 执行循环 --]
    repeat
        print("a的值为:", a)
        a = a + 1
    until( a > 15 )

## Lua 流程控制

    --[ 0 为 true ]
    if(0)
    then
        print("0 为 true")
    end

    if (a > 1)
    then
        print("a > 1")
    elseif (a < 1)
    then
        print("a < 1")
    else
        print("a == 1")
    end

## Lua 函数

    function foo(a, b)      -- foo(1,2) => 2 1
        return b, a
    end

    -- 可变参数
    function bar(...)
        local arg = {...}
        return #arg
    end

## Lua 运算符

    不等于   ~=
    and     与
    or      或
    not     非
    ..      连接两个字符串
    #       返回字符串长度。获取表长度时，根据的是表的最大索引的值

## Lua 迭代器

范性for的执行过程：

1. 初始化，计算in后面表达式的值，表达式应该返回范性for需要的三个值：迭代函数、状态常量、控制变量；与多值赋值一样，如果表达式返回的结果个数不足三个会自动用nil补足，多出部分会被忽略。
1. 将状态常量和控制变量作为参数调用迭代函数（注意：对于for结构来说，状态常量没有用处，仅仅在初始化时获取他的值并传递给迭代函数）。
1. 将迭代函数返回的值赋给变量列表。
1. 如果返回的第一个值为nil循环结束，否则执行循环体。
1. 回到第二步再次调用迭代函数

**ipairs实现**:

    function iter (a, i)
        i = i + 1
        local v = a[i]
        if v then
            return i, v
        end
    end

    function ipairs (a)
        return iter, a, 0
    end

当Lua调用ipairs(a)开始循环时，他获取三个值：迭代函数iter、状态常量a、控制变量初始值0；然后Lua调用iter(a,0)返回1,a[1]（除非a[1]=nil）；第二次迭代调用iter(a,1)返回2,a[2]……直到第一个nil元素。

**pairs 和 ipairs 区别**:

- pairs: 迭代 table，可以遍历表中所有的 key 可以返回 nil
- ipairs: 迭代数组，不能返回 nil,如果遇到 nil 则退出

## Lua table(表)

table 是 Lua 的一种数据结构，可以用来创建数组、字典

    -- 初始化表
    mytable = {}

    -- 指定值
    mytable[1]= "Lua"

    -- 移除引用
    mytable = nil
    -- lua 垃圾回收会释放内存

    -- 排序
    table.sort(mytable)

当我们获取 table 的长度的时候无论是使用 # 还是 table.getn 其都会在索引中断的地方停止计数，而导致无法正确取得 table 的长度。可以使用以下方法来代替：

    function table_leng(t)
        local leng=0
        for k, v in pairs(t) do
            leng=leng+1
        end
        return leng;
    end

## Lua 模块

定义模块

    -- module.lua
    module = {}

    module.a = "lol"

    local function bar()
        print("bar")
    end

    function module.foo()
        print("foo")
        bar()
    end

    return module

加载模块

    require("module")  or require "module" or
    local m = require("module")
    m.foo()

## Lua 元表(Metatable)

    mytable = {}                          -- 普通表
    mymetatable = {}                      -- 元表
    setmetatable(mytable,mymetatable)     -- 把 mymetatable 设为mytable 的元表

    mytable = setmetatable({},{})         -- 写成一行
    getmetatable(mytable)                 -- 返回mymetatable

[各种元方法](http://www.runoob.com/lua/lua-metatables.html)

## Lua 协同程序(coroutine)

- Lua 协同程序拥有独立的堆栈，独立的局部变量，独立的指令指针，同时又与其它协同程序共享全局变量和其它大部分东西。
- 一个具有多个线程的程序可以同时运行几个线程，而协同程序却需要彼此协作的运行。
- 在任一指定时刻只有一个协同程序在运行，并且这个正在运行的协同程序只有在明确的被要求挂起的时候才会被挂起。

生产者-消费者问题

    local newProductor

    function productor()
        local i = 0
        while true do
            i = i + 1
            send(i)     -- 将生产的物品发送给消费者
        end
    end

    function consumer()
        while true do
            local i = receive()     -- 从生产者那里得到物品
            print(i)
        end
    end

    function receive()
        local status, value = coroutine.resume(newProductor)    -- 启动协程，productor方法被调用
        return value
    end

    function send(x)
        coroutine.yield(x)     -- x表示需要发送的值，值返回以后，就挂起该协同程序
    end

    -- 启动程序
    newProductor = coroutine.create(productor)      -- 创建协程，此时productor方法还没被调用
    consumer()

## Lua 其他知识点

[Lua 文件 I/O](http://www.runoob.com/lua/lua-file-io.html)

[Lua 错误处理](http://www.runoob.com/lua/lua-error-handling.html)