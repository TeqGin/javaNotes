# 测试无返回值的模块

**如有错误欢迎指正**

本篇文章是软件测试第二版（朱少民）实验四测试代码。代码仅代表本人想法，该代码也仅是本人学习测试使用，不一定是正确答案。

> Q:如何测试没有返回值的模块？
>
> A1:如果使用assertEquals的话需要在模块开始和结束发生一定的`数值`变化并且是经过该模块时会且唯一发生的逻辑，如类成员变量的值发生改变，这样即使没有返回值也能通过校验类成员变量改变的量是否与预期一致进行断言。（我还未实现过）
>
> A2：如果模块中发生较多`数值`变化，较难进行校验，或者模块中没有发生任何`数值`变化，那么如果模块中有`log`或`System.out`等特殊且唯一的字符串输出，则可以通过抓取这些输出并进行字符串匹配来进行断言。
>
> `数值`:指Java中所有基础变量类型的值的变化

- A2code(借鉴了[@Hoboyz的代码](https://blog.csdn.net/hbmovie/article/details/80402380))
- 使用containsString时需要导入`import static org.hamcrest.CoreMatchers.containsString;`
- [@Hoboyz](https://blog.csdn.net/hbmovie/article/details/80402380)导入了`import static org.hamcrest.Matchers.containsString;`，不知道为什么我在eclipse中导入这个包会报错。

```java
import junit.framework.TestCase;

import org.junit.Test;

import java.io.ByteArrayOutputStream;
import java.io.PrintStream;

import static org.hamcrest.CoreMatchers.containsString;
import static org.junit.Assert.assertThat;

public class BankAccountTest  extends TestCase {
    BankAccount b = new BankAccount("1", "zhangsan", "10", "123456");

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
    
    @Test
    public void testWithDraw(){
        final ByteArrayOutputStream outContent = new ByteArrayOutputStream();
        System.setOut(new PrintStream(outContent));

        int n = 10;
        b.Deposit(n);
        b.WithDraw(n);

        assertThat(outContent.toString(), containsString("You have withdrawed money:"));
        b.WithDraw(10);
        assertThat(outContent.toString(), containsString("Error!"));
    }
    
    @Test
    public void testWithdraw(){
        BankAccountGold b = new BankAccountGold("2222","aaa","111","222");
        final ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
        System.setOut(new PrintStream(byteArrayOutputStream));

        int n = 100;
        int m = 50;
        b.Deposit(n);
        b.WithDraw(m);
        assertThat(byteArrayOutputStream.toString(), containsString("Succeed to withdraw. your balance is :" + (n - m)));

        b.WithDraw(50);
        assertThat(byteArrayOutputStream.toString(), containsString("Succeed to withdraw. your balance is :" + (0)));

        b.WithDraw(999);
        assertThat(byteArrayOutputStream.toString(), containsString("Succeed to withdraw. your balance is :" + (-999)));
        b.WithDraw(1);
        assertThat(byteArrayOutputStream.toString(), containsString("Failed to withdraw ,you cannot overdraft more than 1000!"));

    }

}

```



