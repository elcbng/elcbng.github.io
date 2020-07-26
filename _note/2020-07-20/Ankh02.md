---
layout: note
title:  Week 2 Notes(on background ) by Ankh
author: AnkhZhai
category: 2020暑假第二周
date: 2020-07-26
---

## PART 01. JAVA学习  

1. 什么叫**类**，什么叫**对象**？两者有什么区别？  
类实际上就是一种数据类型，具有属性和操作；对象是由数据和相关方法组成的软件包。
对象是类的实例，类是有公共特性的对象的抽象。
2. 什么是**继承**？**子类**和**超类**的关系如何？  
   新类拥有原类的属性；
		子类继承超类的状态和方法，并且可以重写所继承的方法，使之增加新的用途。

3. 什么是**抽象类**？什么是**最终类**？  
   抽象类是不能直接实例化为对象的类；最终类是不能再被继承的类，类中的方法不能再被重写

4. 什么是**成员变量**？成员变量有几种访问限制？
   成员变量是指与一个类或对象相关的变量，在类体中但不在方法体中声明，其作用域是整个类；有public、protected、friendly、private四种访问限制。

5. 如何声明**类方法**和**实例方法**？
   类方法在声明时加static关键字，若省略则是实例方法。

6. 什么是**抽象方法**？什么是**构造方法**？
   抽象方法没有方法体，必须由子类重写；构造方法是某个方法与声明它的类同名。

7. 什么是方法的**重写**与**重载**？
  重写是子类修改父类的一些方法进行扩展，增大功能；重载是用一个标识符表示同一范围内的多个事物
8. 什么是**异常**？ 哪些异常需要捕获或声明？   
   异常是一个事件，当执行中的程序中断其正常的指令流时出现；非RuntimeException异常类或者其子类的异常都要声明捕捉。

8. **异常处理**器分几个组成部分？  
   try块、catch块和finally块。
 
9. 如何**抛出异常**？  
   通过throw语句或throws语句声明抛出。  
写个小程序练手：  
用两个线程模拟银行账户的存取款操作，两个线程分别操作三次，每次随机存取1000-5000元，实时显示两线程的操作过程对账户的影响。
```java
package hello;


import java.math.BigDecimal;

public class Hello {

    public static void main(String[] args){
        UserAccount userAccount = UserAccount.init();
        MyThread myThread1 = new MyThread(1, userAccount, new BigDecimal(10));
        MyThread myThread2 = new MyThread(0, userAccount, new BigDecimal(100));
        MyThread myThread3 = new MyThread(1, userAccount, new BigDecimal(205));

        myThread1.run();
        myThread2.run();
        myThread3.run();
    }

    static class MyThread implements Runnable{

        private int flag;
        private UserAccount userAccount;
        private BigDecimal num;

        public MyThread(int flag, UserAccount userAccount, BigDecimal num){
            this.flag = flag;
            this.userAccount = userAccount;
            this.num = num;
        }

        @Override
        public void run() {
            if (flag == 0){
                //存钱
                userAccount.add(num);
            }else if (flag == 1){
                //消费
                userAccount.consume(num);
            }
            System.out.println("账户余额为："+ userAccount.getCount());
        }
    }


}


class UserAccount{
    private String username;



    private BigDecimal count;

    public static UserAccount init(){
        return new UserAccount("张三", new BigDecimal(10000));
    }

    private UserAccount(String username, BigDecimal count){
        this.username = username;
        this.count = count;
    }

    public synchronized BigDecimal consume(BigDecimal bigDecimal){
        this.count = this.count.subtract(bigDecimal);
        return this.count;
    }

    public synchronized BigDecimal add(BigDecimal bigDecimal){
        this.count = this.count.add(bigDecimal);
        return this.count;
    }


    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public BigDecimal getCount() {
        return count;
    }

    public void setCount(BigDecimal count) {
        this.count = count;
    }
}

```  
## PART 02. 其他  
这周学得不多，在考科目三和辩论赛中~~迷失了自我~~让其他时间懒得学。反思，下周会补上进度。