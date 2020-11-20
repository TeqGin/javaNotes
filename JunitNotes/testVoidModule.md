# 测试无返回值的模块

> Q:如何测试没有返回值的模块？
>
> A1:如果使用assertEquals的话需要在模块开始和结束发生一定的`数值`变化并且是经过该模块时会且唯一会发生的逻辑，如类成员变量的值发生改变，这样即使没有返回值也能通过校验类成员变量改变的量是否与预期一致进行断言。（还未实现过）
>
> A2：如果模块中发生较多`数值`变化，较难进行校验，那么如果模块中有`log`或`System.out`等特殊且唯一的字符串输出，则可以通过抓取这些输出并进行字符串匹配来进行断言。
>
> `数值`:指Java中所有基础变量类型值的变化

- A2code

```java
    @Test
    public void testDeposit(){
        //重定向输出到outputContent
        final ByteArrayOutputStream outputContent = new ByteArrayOutputStream();
        System.setOut(new PrintStream(outputContent));

        int n = 10;
        b.Deposit(n);
        assertThat(outputContent.toString(), containsString("Deposited money successed, the total money is :" + n));

        n = -2;
        b.Deposit(n);
        //获取输出并进行字符串匹配
        assertThat(outputContent.toString(), containsString("Failed to deposite, money should be larger than 0"));
    }
```



