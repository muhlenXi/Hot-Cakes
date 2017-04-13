### java 语法总结

#### 主函数入口

```java
public static void main(String[] args) {

}
```

#### 控制台输出语句

```java
System.out.println("hello world");
```

#### 定义基本变量

```java
// 定义一个位变量
byte a = 14;

// 定义一个字符标量
char c = 'A';

// 定义一个短整型变量
short value = 15;

// 定义一个整型变量
int number = 5;

// 定义一个长整型变量
long money = 2000L;

// 定义一个单精度浮点型变量
float r = 2.5F;

// 定义一个双精度浮点型变量
double pi = 3.1415926;

// 定义一个字符串变量
String name = "zhangsan";

// 定义一个布尔值
boolean result = true;
```
#### 定义数组

```java
// 数组静态初始化
int[] nums = new int[]{1,3,5,7,9};
int[] numbers = {2,4,6,8,10};

// 数组动态初始化
int[] ages = new int[10];
System.out.println(ages.length);

// 定义一个字符串数组
String[] strings = {"one","two"};
```

#### 二维数组的定义和遍历

```java
int [][]  array = new int[2][3];
int[][] array1 = {{1,2,3},{4,5,6}};

// 数组的遍历
for (int i = 0; i < array1.length; i++) {
	for (int j = 0; j < array1[i].length; j++) {

         System.out.println(array1[i][j]);
    }
}
```


#### 控制台输入

*复制该语句需要手动导入 import java.util.Scanner;*

```java
Scanner scanner = new Scanner(System.in);
int age = scanner.nextInt();
```

#### 三元运算符

```java
int x = 100;
int y = 50;
String result = x >= y? "x大于y" : "x小于y";
```

#### if - 语句

*`+`用于连接字符串。*

```java
int number = 15;
if (number > 0) {
	System.out.println("number == " + number + " 是正数");
}
```
#### if - else 语句

```objc
int number = -15;
if (number > 0) {
	System.out.println("number == " + number + " 是正数");
} else {
	System.out.println("number == " + number + " 是负数");
}
```

#### if - else if - else 语句

```java
if (age > 60) {
	System.out.println("老年票");
} else if ( age  > 22) {
	System.out.println("成年票");
} else if (age > 8) {
	System.out.println("儿童票");
} else {
	System.out.println("免票");
 }
```

#### switch 语句

*仅仅支持`int`类型。*

```java
int weekday = 2;
switch (weekday) {
	case 1:
          System.out.println("星期一");
          break;  
	case 2:
          System.out.println("星期二");
          break;
	case 3:
          System.out.println("星期三");
          break;
	case 4:
          System.out.println("星期四");
          break;
	case 5:
          System.out.println("星期五");
          break;
	case 6:
          System.out.println("星期六");
          break;
	case 7:
          System.out.println("星期天");
          break;
	default:
          System.out.println("亲，没有这一天！");
}
```
#### while 循环

```java
int num = 0;
while (num < 10)
{
	// Do something
	System.out.println(num);
	num ++;
}
```

#### do - while 循环

```objc
int num1 = 1;
do {
	System.out.println(num1);
	num1 ++;
} while (num1 < 11);
```
#### for 循环语句

```java
for (int i = 0; i < 9 ; i++) {
	// do something
}
```

#### 函数的声明

```java
// 不带参数无返回值
public void printStar(){

	System.out.println("* * * * *");
}
    
// 一个参数无返回值
public void printNumber(int a) {

	System.out.println(a);
}

// 两个参数一个返回值
public  int sum(int a,int b) {
        
	return a + b;
}
```