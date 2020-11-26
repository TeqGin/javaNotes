# 测试无返回值的模块

> Q:如何测试没有返回值的模块？
>
> A1:如果使用assertEquals的话需要在模块开始和结束发生一定的`数值`变化并且是经过该模块时会且唯一发生的逻辑，如类成员变量的值发生改变，这样即使没有返回值也能通过校验类成员变量改变的量是否与预期一致进行断言。（我还未实现过）
>
> A2：如果模块中发生较多`数值`变化，较难进行校验，或者模块中没有发生任何`数值`变化，那么如果模块中有`log`或`System.out`等特殊且唯一的字符串输出，则可以通过抓取这些输出并进行字符串匹配来进行断言。
>
> `数值`:指Java中所有基础变量类型的值的变化

- A2code(借鉴了[@Hoboyz的代码](https://blog.csdn.net/hbmovie/article/details/80402380))
  

```java
    @Test
    public void testDeposit(){
        //重定向输出到outputContent
        final ByteArrayOutputStream outputContent = new ByteArrayOutputStream();
        System.setOut(new PrintStream(outputContent));

        int n = 10;
        //该语句包含打印输出语句
        b.Deposit(n);
        assertThat(outputContent.toString(), containsString("Deposited money successed, the total money is :" + n));

        n = -2;
        //该语句包含打印输出语句
        b.Deposit(n);
        //获取输出并进行字符串匹配
        assertThat(outputContent.toString(), containsString("Failed to deposite, money should be larger than 0"));
    }
```



