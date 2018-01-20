---
layout: post
#java使用import时用的内容
title:  java使用import时用的内容
#时间配置
date:   2016-10-11 13:55:50 +0800
#大类配置
categories: 知识
#小类配置
tag: java
---

* content
{:toc}

> import的结构 import + 包名 + 文件名。例：import test.test;
> 使用星可以表示包内所有文件。例：import test.*;

```java
package test;
import test.test;

public class Puppy { 
    
    int puppyAge;
    
    public Puppy(String name) {
        //这个构造器仅有一个参数：name
        System.out.println("Passed Name is :" + name);
    }

    public void setAge(int age) {
        puppyAge = age;
    }
    
    public int getAge( ) {
        System.out.println("Puppy`s age is :" + puppyAge);
        return puppyAge;
    }
    
    public static void main(String []args) {
        Puppy myPuppy = new Puppy("tommy");
        myPuppy.setAge(2);
        myPuppy.getAge();
        /* 你也可以像下面这样访问成员变量 */
        System.out.println("Variable Value :" + myPuppy.puppyAge);
        
        test myTest = new test();
        myTest.age = 100;
        System.out.println("年龄是 : " + myTest.age);
    }
}
```