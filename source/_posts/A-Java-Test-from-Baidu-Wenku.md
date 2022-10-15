---
title: 一道有意思的练习题
date: 2018-06-16 20:11:59
tags:
- Java
- C++
- Code
categories:
- Code
---

今天，在查询C++练习题时，发现一个有趣的项目，如图，其分析给了一个数列，但本人认为这是练习对象的一个好机会，因为可以用以下方式实现：
![1]

<!--more-->
首先，需要有一个`Nature`包，其中有`Time`类。
Time类代码如下：
```java
package Nature;

public class Time {
	public static float month = 30;//set 1 month equals to 30 days;
	public static float day = 1;
	public static float year = 360;

	float time;
	public int getTimeByDay() {
		return (int)time;
	}

	public int getTimeByMonth() {
		return (int) (time/this.month);
	}

	public float getTimeByYear() {
		return time/this.year;
	}

	public void add(int t) {
		time += t;
	}

	public Time(int initTime) {
		time = initTime;
	}
}
```
可以看到，Time类中定义了静态常量`day`和`month`,其用来定义标准时间。构造器部分仅仅是初始化，而`add`方法则是将时间累加。
其次，有一个`Rabbit`类，负责处理兔子相关，代码如下：
```java
public class Rabbit {
	private int age;
	private int adultAge = 90;
	public int getAgeByMonth() {
		return (int) (age/Nature.Time.month);
	}

	public int getAdultAgeByMonth() {
		return (int) (adultAge/Nature.Time.month);
	}

	public void syncAge(int a) {
		this.age = a;
	}
	public Rabbit() {
		this.age = (int) Nature.Time.day;
	}
}
```
代码比较简单，就不逐句解释了。转换是为了最后的换算年。
然后是`mainClass`类，负责处理主要过程，代码如下：
```java
import java.util.concurrent.TimeUnit;
import Nature.Time;

public class mainClass {

	public static void main(String[] argv) {
		Nature.Time n = new Nature.Time(0);
		//start from first day;
		int maxRabbitNumber = 1024;
		int i = 0,k = 0;
		Rabbit[] r = new Rabbit[maxRabbitNumber];//1024 rabbits is enough;
		int adultRabbit = 0;
		int initRabbitNum = 1;
		int existRabbitNum = initRabbitNum;
		int oldAdultRabbit = 0;

		for(;n.getTimeByMonth() <= 100;) {
			try {
  			if(i <= (existRabbitNum - 1))
  			System.out.print("[INFO] Adding rabbit in the r.");
  			for(;i <= (existRabbitNum - 1) && i < maxRabbitNumber;i++) {
  				r[i] = new Rabbit();
  				TimeUnit.MILLISECONDS.sleep(50);
  				System.out.print(".");
  			}
  			TimeUnit.MILLISECONDS.sleep(50);
  			System.out.println("");
  			oldAdultRabbit = adultRabbit;
  			for(k = adultRabbit;k <= (existRabbitNum - 1);k++) {
  				if(r[k].getAgeByMonth() >= r[k].getAdultAgeByMonth()) {
  					adultRabbit += 1;
  				}
  			}
  			if((adultRabbit - oldAdultRabbit) != 0)
  				System.out.println("[INFO] There are " + (adultRabbit - oldAdultRabbit)
  					+ " rabbit become adult.");
  			n.add((int) Nature.Time.month);
  			System.out.println("[TIME] Time passed 1 month, and this is "+ n.getTimeByMonth() +" month.");
  			for(k = 0;k <= (existRabbitNum - 1); k++) {
  				r[k].syncAge((int) n.getTimeByDay());
  			}
  			for(k = 0;k < (adultRabbit);k++) {
  				existRabbitNum++;
  			}
  			System.out.println(existRabbitNum);
			}catch(Exception e) {
				System.out.println("[ERR]  Finally, time passes "
						+ n.getTimeByDay() + " days, it means "
						+ n.getTimeByMonth() + " months, about "
						+ n.getTimeByYear() + " years, and "
						+ (existRabbitNum - initRabbitNum) +" rabbits born, "
						+ adultRabbit + " rabbits become adult. Bye.");
				System.exit(0);
			}		
		}
	}
}
```
只需更改`maxRabbitNumber`与`initRabbitNum`即可实现同类问题转换。`TimeUnit.MILLISECONDS.sleep(50);`方法用于休眠50毫秒，让`...`的输出看起来更舒服一些。

另外，关于`Rabbit`类的数组问题，困扰了我一段时间，想到类比分析`String[]`，写了一个`Rabbit r = new Rabbit[maxRabbitNumber];`，可以实现。

最后，项目结构、运行结果如图：
![2]![3]
最终输出：


	>  [ERR]  Finally, time passes 540 days, it means 18 months, about 1.5 years, and 1596 rabbits born, 987 rabbits become adult. Bye.



在此，留一个坑，未来此题需用C++实现。
