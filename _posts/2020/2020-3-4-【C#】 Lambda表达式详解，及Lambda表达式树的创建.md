---
layout: post
category: web
title: 【C#】 Lambda表达式详解，及Lambda表达式树的创建
tagline: by 恬静的小魔龙
tag: Other
---

原文地址：https://www.cnblogs.com/mq0036/p/7427892.html
原文作者：jack_Meng
原文出处：博客园


----------
##C# Lambda表达式详解，及Lambda表达式树的创建

   每次写博客，第一句话都是这样的：程序员很苦逼，除了会写程序，还得会写博客！当然，希望将来的一天，某位老板看到此博客，给你的程序员职工加点薪资吧！因为程序员的世界除了苦逼就是沉默。我眼中的程序员大多都不爱说话，默默承受着编程的巨大压力，除了技术上的交流外，他们不愿意也不擅长和别人交流，更不乐意任何人走进他们的内心！

   题外话说多了，咱进入正题：

   上一节中，我们讲到：**在 2.0 之前的 C# 版本中，声明委托的唯一方法是使用命名方法。  C# 2.0 引入了匿名方法，而在 C# 3.0 及更高版本中，Lambda 表达式取代了匿名方法，作为编写内联代码的首选方式。 ** 有一种情况下，匿名方法提供了 Lambda 表达式中所没有的功能。  您可使用匿名方法来忽略参数列表。  这意味着匿名方法可转换为具有各种签名的委托。  这对于 Lambda 表达式来说是不可能的。  有关 lambda 表达式的更多特定信息，*请参见 Lambda 表达式（C# 编程指南）*。 

结下红色那段话的意思：微软告诉你：我们在C#2.0之前就有委托了，在2.0之后又引入了匿名方法，C#3.0之后，又引入了Lambda表达式，他们三者之间的顺序是：委托->匿名变量->Lambda表达式，微软的一步步升级，带给我们编程上的优美，简洁，可读性强.....在此，不多夸微软，怕他们看到这篇博客后骄傲，怕他们尾巴能翘到天上，不知天高地厚。嘿嘿，说多了！

温故而知新，可以做老师，咱们来温故下委托和匿名表达式。

委托如下：

```
delegate int calculator(int x, int y); //委托类型
        static void Main()
        {
            calculator cal = new calculator(Adding);
            int He = cal(1, 1);
            Console.Write(He);
        }

        /// <summary>
        /// 加法
        /// </summary>
        /// <param name="x"></param>
        /// <param name="y"></param>
        /// <returns></returns>
        public static int Adding(int x, int y)
        {
            return x + y;
        }
```
匿名方法如下：

```
 delegate int calculator(int x, int y); //委托
        static void Main()
        {
            calculator cal = delegate(int num1,int num2)
            {
                return num1 + num2;
            };
            int he = cal(1, 1);
            Console.Write(he);
        }
```
下面我们来讲解Lambda表达式：

 按照上边的加法，我们用Lambda表达式来实现，代码如下：
 

```
delegate int calculator(int x, int y); //委托类型
        static void Main()
        {
            calculator cal = (x, y) => x + y;//Lambda表达式，大家发现没有，代码一个比一个简洁
            int he = cal(1, 1);
            Console.Write(he);
        }
```
那么我们详细讲讲Lambda表达式：

若要创建 Lambda 表达式，需要在 Lambda 运算符 => 左侧指定输入参数（如果有），然后在另一侧输入表达式或语句块。 例如，lambda 表达式 x => x * x 指定名为 x 的参数并返回 x 的平方值。 如上面的示例所示，你可以**将此表达式分配给委托类型**：

"Lambda表达式"是一个特殊的匿名函数，是一种高效的类似于函数式编程的表达式，Lambda简化了开发中需要编写的代码量。它可以包含表达式和语句，并且可用于创建委托或表达式目录树类型，支持带有可绑定到委托或表达式树的输入参数的内联表达式。所有Lambda表达式都使用Lambda运算符=>，该运算符读作"goes to"。Lambda运算符的左边是输入参数(如果有)，右边是表达式或语句块。Lambda表达式x => x * x读作"x goes to x times x"。举几个简单的Lambda表达式，如下：

```
delegate bool MyBol(int x, int y);
        delegate bool MyBol_2(int x, string y);
        delegate int calculator(int x, int y); //委托类型
        delegate void VS();
        static void Main()
        {
            MyBol Bol = (x, y) => x == y;
            MyBol_2 Bol_2 = (x, s) => s.Length > x;
            calculator C = (X, Y) => X * Y;
            VS S = () => Console.Write("我是无参数Labada表达式");
            //
            int[] numbers = { 5, 4, 1, 3, 9, 8, 6, 7, 2, 0 };
            int oddNumbers = numbers.Count(n => n % 2 == 1);
            //
            List<People> people = LoadData();//初始化
            IEnumerable<People> results = people.Where(delegate(People p) { return p.age > 20; });
        }

        private static List<People> LoadData()
        {
            List<People> people = new List<People>();   //创建泛型对象  
            People p1 = new People(21, "guojing");       //创建一个对象  
            People p2 = new People(21, "wujunmin");     //创建一个对象  
            People p3 = new People(20, "muqing");       //创建一个对象  
            People p4 = new People(23, "lupan");        //创建一个对象  
            people.Add(p1);                     //添加一个对象  
            people.Add(p2);                     //添加一个对象  
            people.Add(p3);                     //添加一个对象  
            people.Add(p4);
            return people;
        }

    }

    public class People
    {
        public int age { get; set; }                //设置属性  
        public string name { get; set; }            //设置属性  
        public People(int age, string name)      //设置属性(构造函数构造)  
        {
            this.age = age;                 //初始化属性值age  
            this.name = name;               //初始化属性值name  
        }
    } 
```
##Func<T>委托
 T 是参数类型，这是一个泛型类型的委托，用起来很方便的。

 先上例子
 

```
 static void Main(string[] args)
        {
            Func<int, string> gwl = p => p + 10 + "--返回类型为string";            
            Console.WriteLine(gwl(10) + "");   //打印‘20--返回类型为string’，z对应参数b，p对应参数a
            Console.ReadKey();
        }
```
说明：我们可以看到，这里的p为int 类型参数， 然而lambda主体返回的是string类型的。

再上一个例子

```
static void Main(string[] args)
        {
            Func<int, int, bool> gwl = (p, j) =>
                {
                    if (p + j == 10)
                    {
                        return true;
                    }
                    return false;
                };
            Console.WriteLine(gwl(5,5) + "");   //打印‘True’，z对应参数b，p对应参数a
            Console.ReadKey();
        }
```
说明：从这个例子，我们能看到，p为int类型，j为int类型，返回值为bool类型。

至此，如果上边的内容都能看懂，那么Lambda也就没什么了！
##Lambda表达式

"Lambda表达式"是一个匿名函数，是一种高效的类似于函数式编程的表达式，Lambda简化了开发中需要编写的代码量。它可以包含表达式和语句，并且可用于创建委托或表达式目录树类型，支持带有可绑定到委托或表达式树的输入参数的内联表达式。所有Lambda表达式都使用Lambda运算符=>，该运算符读作"goes to"。Lambda运算符的左边是输入参数(如果有)，右边是表达式或语句块。Lambda表达式x => x * x读作"x goes to x times x"。可以将此表达式分配给委托类型，如下所示：

```
delegate int del(int i);
static void Main(string[] args)
{
    del myDelegate = x => x * x;
    int j = myDelegate(5); //j = 25
}
```
若要创建表达式目录树类型(后面会详细说明)：

```
using System.Linq.Expressions;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            Expression<del> myET = x => x * x;
        }
    }
}
```
###1、表达式Lambda

　　表达式位于 => 运算符右侧的 lambda 表达式称为“表达式 lambda”。 表达式 lambda 会返回表达式的结果，并采用以下基本形式：

   

```
(input parameters) => expression
```

仅当 lambda 只有一个输入参数时，括号才是可选的；否则括号是必需的。 括号内的两个或更多输入参数使用逗号加以分隔：

```
(x, y) => x == y
```
有时，编译器难以或无法推断输入类型。 如果出现这种情况，你可以按以下示例中所示方式显式指定类型：

```
(int x, string s) => s.Length > x

```
使用空括号指定零个输入参数：

```
() => SomeMethod()
```
###2、语句Lambda

当lambda表达式中，有多个语句时，写成如下形式：

```
(input parameters) => {statement;}
```
例如：

```
delegate void TestDelegate(string s);
…
TestDelegate myDel = n => { string s = n + " " + "World"; Console.WriteLine(s); };
myDel("Hello");
```
看到这里，Lambda的基础知识就学完了，下面来讲解一下实际中是如何运用的，这里写了几个小例子：

```
List<string> Citys= new List<string>()
            {
               "BeiJing",
               "ShangHai",
               "Tianjin",
               "GuangDong"
            };
            var result = Citys.First(c => c.Length > 7);
```

这个是大家熟悉的LINQ语句，如果没学过没关系，这里用的只是很简单的几个方法，相信大家都能看懂。

首先定义一个Citys集合，初始化有一些数据。然后调用LINQ的first方法，查询出来长度大于7的第一个结果，看到了吧，这里用的就是Lambda表达式，

如果我们自己写，还要写循环遍历集合，然后判断字符串长度是否大于7，起码要写四五行代码，而这里只要一行就够了，而且LINQ也要写很长。

这里用的是最简单的Lambda表达式，(input parameters) => expression的形式。

 

下面来看一下，如何自己定义和使用Lambda表达式，首先写下面一个函数：

```
public void LambdaFun(string str,Func<string,string> func)
      {
         Console.WriteLine(func(str));
      }
```
这里用到了Func<T>委托，不懂的可以去百度查资料，这个方法什么都没有做，只是调用了委托方法，并将参数传递过去，下面来看一下使用方法：

```
 LambdaFun("BeiJing 2013", s => 
         {
            if (s.Contains("2013"))
            {
               s = s.Replace("2013", "2014");
            }
            return s;
         });
```
这里将传入字符串中的2013替换成为2014，当然还可以是将其他字符串替换城任何内容，或者是截取，连接等等，完全由我们传入的Lambda表达式决定，到了这里感觉到Lambda表达式的强大了吧。

##lambda表达式树动态创建方法 

```
 static void Main(string[] args)
        {
            //i*j+w*x
            ParameterExpression a = Expression.Parameter(typeof(int),"i");   //创建一个表达式树中的参数，作为一个节点，这里是最下层的节点
            ParameterExpression b = Expression.Parameter(typeof(int),"j");
            BinaryExpression r1 = Expression.Multiply(a,b);    //这里i*j,生成表达式树中的一个节点，比上面节点高一级

            ParameterExpression c = Expression.Parameter(typeof(int), "w");
            ParameterExpression d = Expression.Parameter(typeof(int), "x");
            BinaryExpression r2 = Expression.Multiply(c, d);

            BinaryExpression result = Expression.Add(r1,r2);   //运算两个中级节点，产生终结点

            Expression<Func<int, int, int, int, int>> lambda = Expression.Lambda<Func<int, int, int, int, int>>(result,a,b,c,d);

            Console.WriteLine(lambda + "");   //输出‘(i,j,w,x)=>((i*j)+(w*x))’，z对应参数b，p对应参数a

            Func<int, int, int, int, int> f= lambda.Compile();  //将表达式树描述的lambda表达式，编译为可执行代码，并生成该lambda表达式的委托；

            Console.WriteLine(f(1, 1, 1, 1) + "");  //输出结果2
            Console.ReadKey();
        }
```
为了便于大家理解，这点代码构成的Lambda表达式树如下图：
![这里写图片描述](https://img-blog.csdn.net/20180614145037271?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
其实Lambda表达式并不难，只有理解了其中的原理，还是很快可以上手的！