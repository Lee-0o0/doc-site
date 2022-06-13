# Java进阶— —日期时间

本文主要介绍Java中有关日期时间的API。



## 一、Date

`java.util.Date`是最早的日期时间处理类，其用距离`1970-1-1 00:00:00` 的毫秒值表示时间，根据时区会有不同的显示。

下面看一下常用的`Date`类方法：

|             方法             |                 说明                  |
| :--------------------------: | :-----------------------------------: |
|       `public  Date()`       | 构造方法，创建当前的日期时间Date对象  |
|  `public  Date(long date)`   |   构造方法，将时间戳转换为Date对象    |
|      `long  getTime()`       | 获取距离1970-01-01 00：00：00的毫秒值 |
| `boolean  after(Date when)`  |       比较Date对象是否晚于when        |
| `boolean  before(Date when)` |       比较Date对象是否早于when        |

构造方法的源码解析：

```java
private transient long fastTime;       //即距离1970-01-01 00：00：00的毫秒值

public Date() {
    this(System.currentTimeMillis());
}

public Date(long date) {
    fastTime = date;
}
```

`Date`测试代码：

```java
@Test
public void test01(){
    Date date1 = new Date();       //获取当前日期时间对象
    Date date2 = new Date(1000);  //获取距离1970-01-01 00:00:00 一秒后的日期时间对象，即1970-01-01 00:00:01
    System.out.println("date1："+date1);
    System.out.println("date2："+date2);
    System.out.println("date1距离1970-01-01 00：00：00的毫秒值："+date1.getTime());
    System.out.println("date1("+date1+")晚于date2("+date2+")："+date1.after(date2));
    System.out.println("date1("+date1+")早于date2("+date2+")："+date1.before(date2));
}
```

执行结果：

![image-20200831222057673](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-06-13/7bc43b2e62adc378d183b8afd823d1c0--12e2--image-20200831222057673.png)



## 二、Calendar

由于`Date`的局限性，目前`Date`类的许多方法已经建议废除了，后来又推出了`Calendar`，增加了对日期的操作。

`Calendar`的声明为：

```java
public abstract class Calendar
extends Object
implements Serializable, Cloneable, Comparable<Calendar>{...}
```

其是一个抽象类，其实现类有三个如下：

```java
public class GregorianCalendar extends Calendar {...}
class JapaneseImperialCalendar extends Calendar {...}
public class BuddhistCalendar extends GregorianCalendar {...}
```

UML图如下：

![image-20200831223841417](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-06-13/9dfd6c09be8dc3a05eb6a78207eb137a--03b7--image-20200831223841417.png)

如果我们想要获取`Calendar`的实例，则需要通过静态方法`getInstance()`获取其子类的对象实例。

```java
@Test
public void test01(){
    Calendar calendar = Calendar.getInstance();
    System.out.println(calendar);
}
```

结果：

```markdown
java.util.GregorianCalendar[time=1598885267747,areFieldsSet=true,areAllFieldsSet=true,lenient=true,zone=sun.util.calendar.ZoneInfo[id="Asia/Shanghai",offset=28800000,dstSavings=0,useDaylight=false,transitions=29,lastRule=null],firstDayOfWeek=1,minimalDaysInFirstWeek=1,ERA=1,YEAR=2020,MONTH=7,WEEK_OF_YEAR=36,WEEK_OF_MONTH=6,DAY_OF_MONTH=31,DAY_OF_YEAR=244,DAY_OF_WEEK=2,DAY_OF_WEEK_IN_MONTH=5,AM_PM=1,HOUR=10,HOUR_OF_DAY=22,MINUTE=47,SECOND=47,MILLISECOND=747,ZONE_OFFSET=28800000,DST_OFFSET=0]
```

这一大串是啥啊？谁看得懂啊？

所以我们需要提取出看得懂日期时间：

```java
@Test
public void test01(){
    Calendar calendar = Calendar.getInstance();
    System.out.println("时间："+
                       calendar.get(Calendar.YEAR)+ "-" +
                       calendar.get(Calendar.MONTH)+"-" +
                       calendar.get(Calendar.DAY_OF_MONTH)+" " +
                       calendar.get(Calendar.HOUR_OF_DAY) +":" +
                       calendar.get(Calendar.MINUTE) + ":" +
                       calendar.get(Calendar.SECOND));
}
```

![image-20200831225123397](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-06-13/f7342b2bea471b5fb80d66416c06752f--2b90--image-20200831225123397.png)

实际上，现在是8月份，那为什么显示的是7月份呢？因为在`Calendar`类中，月份从0开始，即1月份为0，所以8月份为7，这又是反人类的设计。

在`Calendar`中，还有一些用于操作日期的方法，下面来介绍：

|                     方法                     |                    说明                     |
| :------------------------------------------: | :-----------------------------------------: |
| `abstract void  add(int field,  int amount)` | 在指定的日期结构field上加上指定的数值amount |
|      `void  set(int field,  int value)`      |      设置指定日期结构field为数值value       |
|            `int  get(int field)`             |           返回指定的日期结构数值            |

上述的日期结构field指的是年、月、日、时、分、秒。

例如，`add()`的用法：

```java
@Test
public void test01(){
    Calendar calendar = Calendar.getInstance();
    calendar.add(Calendar.DAY_OF_MONTH,10);   //十天后
    System.out.println(calendar);            //省略了calendar获取各日期结构，表达意思即可
}
```

结果：

![image-20200831230721506](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-06-13/53a36db5ddb24ff12dd488d10c507895--ab96--image-20200831230721506.png)

```java
@Test
public void test01(){
    Calendar calendar = Calendar.getInstance();
    calendar.add(Calendar.HOUR_OF_DAY,5);   //五小时后
    System.out.println(calendar);            //省略了calendar获取各日期结构，表达意思即可
}
```

![image-20200831230826199](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-06-13/194aca317a8fd92ec763773b2c1f73f4--1218--image-20200831230826199.png)

`set()`的用法：

```java
@Test
public void test01(){
    Calendar calendar = Calendar.getInstance();
    calendar.set(Calendar.YEAR,2000);        //将年份设置为2000
    calendar.set(Calendar.MONTH, 0); // 将月份设置为0，即一月份
    System.out.println(calendar);

}
```

![image-20200831231125363](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-06-13/7ab94550cb860da67b8bd9d768a09a9f--60e4--image-20200831231125363.png)



## 三、SimpleDateFormat

在之前的学习中，`Date`比`Calendar`获取日期时间简单，但是`Date`类输出的时间不方便阅读，所以我们需要对时间进行格式化，这需要`DateFormat`的协助，其UML图如下：

![image-20200831231559555](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-06-13/1f1cbf41c0036fc696efb41a52a70b22--9eac--image-20200831231559555.png)

`DateFormat`为抽象类，所以不能实例化对象，我们可以实例化其子类对象`SimpleDateFormat`进行时间格式化。

其构造方法如下：

|                  构造方法                  |                  说明                   |
| :----------------------------------------: | :-------------------------------------: |
|        `public  SimpleDateFormat()`        |  根据默认模式构造SimpleDateFormat对象   |
| `public  SimpleDateFormat(String pattern)` | 根据模式pattern构造SimpleDateFormat对象 |

模式pattern是是什么呢？简单地说，它是日期时间的显示格式，其参数如下：

![image-20200831233209420](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-06-13/df653036b20e31dee6e6d18c1a50369e--ebfa--image-20200831233209420.png)

例子如下：

给定美国大西洋时间2001-07-04 12:08:56，根据不同的模式，显示结果不同：

![image-20200831233344727](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-06-13/cfb0ab3bfc69b35f31f26b862aa386e4--8c62--image-20200831233344727.png)

在`SimpleDateFormat`中，常用的方法有两个：

|             方法             |                     说明                      |
| :--------------------------: | :-------------------------------------------: |
| `String  format(Date date)`  |  将给定的Date对象按照模式pattern转换为字符串  |
| `Date  parse(String source)` | 将给定的符和模式pattern的字符串转换为Date对象 |

例如，将给定的Date对象按照模式（yyyy-MM-dd HH:mm:ss.SSS）转换为字符串：

```java
@Test
public void test01(){
    DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.SSS");
    Date date = new Date();
    String dateString = dateFormat.format(date);
    System.out.println(dateString);
}
```

![image-20200831233733412](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-06-13/219cb2c865a176e9eb1faebbbde2de9a--40d2--image-20200831233733412.png)

例如，将给定的符和模式（yyyy-MM-dd HH:mm:ss）的字符串转换为Date对象：

```java
@Test
public void test02() throws ParseException {
    String dateString = "2020-01-01 09:12:12";
    DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    Date date = dateFormat.parse(dateString);
    System.out.println(date);
}
```

![image-20200831233948964](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-06-13/5ed4ded738a8c38071d9c33dbe184c20--014f--image-20200831233948964.png)



## 四、Date和Calendar存在的问题

由于`Date`类和`Calendar`类出现的时间较早，设计不规范，所以存在许多问题。

### 4.1 计算困难问题

例如，现要计算1998-05-19距离如今有多少天，利用`Calendar`和`Date`计算方式如下：

```java
@Test
public void test01() {
    Calendar calendar = Calendar.getInstance();
    calendar.set(1998, 4, 19);             //月份从0开始
    Date oldDate = calendar.getTime();
    Date newDate = new Date();
    Long days = (newDate.getTime() - oldDate.getTime()) / 1000 / 60 / 60 / 24;
    System.out.println("1998-05-19距离今天" + days + "天。");
}
```

计算过程非常复杂，而且由于整除的问题，有可能最终的结果也有一天的误差。



### 4.2 多线程安全问题

例如，现有十个线程，将"2008-08-08 00:00:00"转换为`Date`对象，代码如下：

```java
@Test
public void test02() {
    final SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

    for (int i = 0; i < 10; i++) {
        new Thread(
            new Runnable() {
                @Override
                public void run() {
                    try {
                        Date date = simpleDateFormat.parse("2008-08-08 00:00:00");
                        System.out.println(date);
                    } catch (ParseException e) {
                        e.printStackTrace();
                    }
                }
            })
            .start();
    }
}
```

结果出现异常：

![image-20200901142102255](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-06-13/1580665b5aa37fb2daa1fd67db7a16c9--c1f4--image-20200901142102255.png)

因为多线程使用同一个`SimpleDateFormat`对象进行转换，而`SimpleDateFormat`对象中有一个`Calendar`成员变量，其进行转换操作时，如果一个线程进行`calendar.clear()`操作后，另一个线程进行`calendar.set()`操作，就会引发多线程安全问题。

![image-20200901142552266](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-06-13/7c6a9f2814542b23163b4da6974c2b4c--4922--image-20200901142552266.png)



## 五、java.time

由于`Date`和`Calendar`存在许多弊端，所以Java 8中引入了`java.time`包，用来处理日期时间，解决存在的问题。

### 5.1 基本类介绍

在`java.time`包中的类及其介绍如下：

|   Class（类名）   |                     Description（描述）                      |
| :---------------: | :----------------------------------------------------------: |
|       Clock       | A clock providing access to the current instant, date and time  using a time-zone. |
|     Duration      | A time-based amount of time, such as '34.5  seconds'.（表示秒或纳秒时间间隔） |
|    ==Instant==    | An instantaneous point on the time-line.（时间轴上的瞬时点） |
|   ==LocalDate==   | A date without a time-zone in the ISO-8601 calendar system,  such as `2007-12-03`.（年月日） |
| ==LocalDateTime== | A date-time without a time-zone in the ISO-8601 calendar  system, such as `2007-12-03T10:15:30`.（年月日时分秒） |
|   ==LocalTime==   | A time without a time-zone in the ISO-8601 calendar system,  such as `10:15:30`.（时分秒） |
|   ==MonthDay==    | A month-day in the ISO-8601 calendar system, such as  `--12-03`.（月日） |
|  OffsetDateTime   | A date-time with an offset from UTC/Greenwich in the ISO-8601  calendar system, such as `2007-12-03T10:15:30+01:00`. |
|    OffsetTime     | A time with an offset from UTC/Greenwich in the ISO-8601  calendar system, such as `10:15:30+01:00`. |
|      Period       | A date-based amount of time in the ISO-8601 calendar system,  such as '2 years, 3 months and 4 days'. |
|     ==Year==      | A year in the ISO-8601 calendar system, such as  `2007`.（年） |
|   ==YearMonth==   | A year-month in the ISO-8601 calendar system, such as  `2007-12`.（年月） |
| ==ZonedDateTime== | A date-time with a time-zone in the ISO-8601 calendar system,  such as `2007-12-03T10:15:30+01:00 Europe/Paris`.（带时区的年月日时分秒） |
|    ==ZoneId==     |       A time-zone ID, such as  `Europe/Paris`.(时区ID)       |
|    ZoneOffset     |   A time-zone offset from Greenwich/UTC, such as  +02:00.    |

在`java.time`中的枚举类介绍：

| Enum（枚举类） |        Description（描述）        |
| :------------: | :-------------------------------: |
|   DayOfWeek    | A day-of-week, such as 'Tuesday'. |
|     Month      | A month-of-year, such as 'July'.  |



### 5.2 ZoneId

本节主要介绍时区的相关类`ZoneId`，其中封装了600个时区。

```java
@Test
public void test01(){
    // 获取系统默认时区
    ZoneId zoneId = ZoneId.systemDefault();
    System.out.println(zoneId);

    // 根据ZoneId字符串创建ZoneId对象
    ZoneId zoneIdOfBerlin = ZoneId.of("Europe/Berlin");
    System.out.println(zoneIdOfBerlin);
}
```

![image-20200901191823116](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-06-13/d03b9e81c7e86ad9b6a357b9e6d916ae--8485--image-20200901191823116.png)

`ZoneId.getAvailableZoneIds();`可以获取所有的时区：

```java
// 获取所有的时区
Set<String> zoneIds = ZoneId.getAvailableZoneIds();
System.out.println(zoneIds.size());      //600
for (String zoneIdString:zoneIds){
    System.out.println(zoneIdString);
}
```

完整的ZoneId列表，请查看：https://blog.csdn.net/qq_41261251/article/details/108348679



### 5.3 LocalDate

`LocalDate`表示年月日，我们可以使用静态方法`LocalDate.now()`获取现在的年月日。

```java
LocalDate now = LocalDate.now();     //获取现在的LocalDate对象,系统默认时区
```

如果要构建某个指定日期的`LocalDate`对象，则可以使用静态方法`LocalDate.of()`方法。

|                             方法                             |                       说明                        |
| :----------------------------------------------------------: | :-----------------------------------------------: |
|   `static LocalDate of(int year,int month,int dayOfMonth)`   |            根据年月日构建LocalDate对象            |
| `static LocalDate of(int year, Month month, int dayOfMonth)` | 根据年月日构建LocalDate对象，其中月份使用枚举类型 |

```java
LocalDate date = LocalDate.of(2008, 8, 8);              // 获取2008-8-8的LocalDate对象
LocalDate date1 = LocalDate.of(2008, Month.APRIL, 1);   // 获取2008-4-1的LocalDate对象
```

`LocalDate`类常用的方法及极少如下：

| 方法                                         | 说明                                                         |
| -------------------------------------------- | ------------------------------------------------------------ |
| `int getYear()`                              | 获取年份                                                     |
| `Month getMonth()`                           | 获取月份，返回值为Month枚举类                                |
| `int getMonthValue()`                        | 获取月份，返回值为1-12                                       |
| `int getDayOfMonth()`                        | 获取日                                                       |
| `DayOfWeek getDayOfWeek()`                   | 获取今天星期几                                               |
| `int getDayOfYear()`                         | 获取该日期是今年的第几天                                     |
| `boolean isLeapYear()`                       | 该年是否为闰年                                               |
| `LocalDate plusYears(long yearsToAdd)`       | 在该日期上**加上**yearsToAdd年，得到一个新的LocalDate对象，还有很多相似的方法 |
| `LocalDate minusYears(long yearsToSubtract)` | 在该日期上**减去**yearsToAdd年，得到一个新的LocalDate对象，还有很多相似的方法 |
| `LocalDate withYear(int year)`               | 修改日期的年份为参数值year，得到一个新的LocalDate对象，还有很大相似的方法 |

在`LocalDate`中，还有`with()`的重载方法，其需要的参数为`TemporalAdjuster `：

`LocalDate with(TemporalAdjuster adjuster)  `

该方法用于更复杂的修改日期操作，比如将日期修改为这个月的最后一天，将日期修改为下一周的第一天等。

`TemporalAdjuster`可以由`TemporalAdjusters `的静态方法获取：

![image-20200902132548571](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-06-13/37583aae857561515ccbd8ceec9b5c3d--8b2c--image-20200902132548571.png)

接下来就通过例子演示：

```java
@Test
public void test02(){
    LocalDate now = LocalDate.now();      //当前日期
    LocalDate firstDayOfMonth = now.with(TemporalAdjusters.firstDayOfMonth());  //这个月第一天
    LocalDate firstDayOfNextMonth = now.with(TemporalAdjusters.firstDayOfNextMonth()); //下个月第一天
    LocalDate firstDayOfYear = now.with(TemporalAdjusters.firstDayOfYear());    //今年第一天
    LocalDate firstDayOfNextYear = now.with(TemporalAdjusters.firstDayOfNextYear());  //明年第一天
    LocalDate lastDayOfMonth = now.with(TemporalAdjusters.lastDayOfMonth()); //这个月最后一天
    LocalDate lastDayOfYear = now.with(TemporalAdjusters.lastDayOfYear());    //今年最后一天
    LocalDate nextFriday = now.with(TemporalAdjusters.next(DayOfWeek.FRIDAY)); //下一个周五
    LocalDate previousMonday = now.with(TemporalAdjusters.previous(DayOfWeek.MONDAY));  //上一个周一

    System.out.println("今天的日期："+now);
    System.out.println("这个月第一天的日期："+firstDayOfMonth);
    System.out.println("下个月第一天的日期："+firstDayOfNextMonth);
    System.out.println("今年第一天的日期："+firstDayOfYear);
    System.out.println("明年第一天的日期："+firstDayOfNextYear);
    System.out.println("这个月最后一天的日期："+lastDayOfMonth);
    System.out.println("今年最后一天的日期："+lastDayOfYear);
    System.out.println("下一个周五的日期："+nextFriday);
    System.out.println("上一个周一的日期："+previousMonday);
}
```

结果：

![image-20200902133317911](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-06-13/c533d59ade1facc0084226a726fe5c4d--81ad--image-20200902133317911.png)

`LocalTime`和`LocalDateTime`的用法与`LocalDate`相似，不再赘述。



### 5.4 日期格式化

由于`SimpleDateFormat`线程不安全，所以在Java 8中引入了新的日期格式化类`DateTimeFormatter`，并且其包含了一些日期格式化字符串。

在`LocalDateTime`中有日期格式化的方法`format()`：

```java 
String format(DateTimeFormatter formatter)  
```

```java
@Test
public void test05(){
    LocalDateTime now = LocalDateTime.now();
    String s = now.format(DateTimeFormatter.BASIC_ISO_DATE);
    System.out.println(s);
}
```

结果：

![image-20200902144538543](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-06-13/fc3aa0b21846ef25b115099fbc672ec6--62c6--image-20200902144538543.png)

完整的`DateTimeFormatter`内置格式：

![image-20200902144656763](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-06-13/35fa49a7ff923efa6ffd72310fd82a88--ecff--image-20200902144656763.png)

我们也可以自定义日期格式：

```java
@Test
public void test05(){
    LocalDateTime now = LocalDateTime.now();
    String s = now.format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));
    System.out.println(s);
}
```

将字符串转换为时间类型也是可以的：

```java
LocalDateTime localDateTime = 
    LocalDateTime.parse("2020-09-02 14:49:26",
                         DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));
```



## 六、java.time一些案例

### 6.1 Date对象转换为LocalDate、LocalTime或LocalDateTime

在Java 8之后，`Date`类添加了一个方法`toInstant()`，可以将`Date`对象转换为`Instant`对象，然后为`Instant`对象添加时区信息，之后就可以转换为`LocalDate`、`LocalTime`或`LocalDateTime`.

```java
@Test
public void test03(){
    Date date = new Date();
    // 1.转换为Instant对象
    Instant instant = date.toInstant();
    // 2.添加时区信息，获取ZonedDateTime对象
    ZonedDateTime zonedDateTime = instant.atZone(ZoneId.systemDefault());
    // 3.获取LocalDate、LocalTime、LocalDateTime对象
    LocalDate localDate = zonedDateTime.toLocalDate();
    LocalTime localTime = zonedDateTime.toLocalTime();
    LocalDateTime localDateTime = zonedDateTime.toLocalDateTime();

    System.out.println("转换前Date对象："+date);
    System.out.println("转换后LocalDate对象："+localDate);
    System.out.println("转换后LocalTime对象："+localTime);
    System.out.println("转换后LocalDateTime对象："+localDateTime);
}

```

![image-20200902134315663](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-06-13/7b12b047ae724792521347aeabe5b608--ac33--image-20200902134315663.png)





### 6.2 Calendar类转换为ZonedDateTime

通过`Calendar`类获取时区`TimeZone`和`Instant`对象，然后将`TimeZone`转换为`ZoneId`，最后通过`ZonedDateTime`的静态方法`static ZonedDateTime ofInstant(Instant instant, ZoneId zone)  `获取。

```java
@Test
public void test04(){
    Calendar calendar = Calendar.getInstance();
    // 1. 获取TimeZone对象
    TimeZone timeZone = calendar.getTimeZone();
    // 2. 自Java 8之后，TimeZone增加了方法toZoneId()，转换为ZonedId
    ZoneId zoneId = timeZone.toZoneId();
    // 3. 自Java 8之后，Calendar增加了方法toInstant()，转换为Instant
    Instant instant = calendar.toInstant();
    // 4. 由静态方法ofInstant()构建ZonedDateTime对象
    ZonedDateTime zonedDateTime = ZonedDateTime.ofInstant(instant, zoneId);

    System.out.println(zonedDateTime);
}
```

![image-20200902135349741](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-06-13/6ed90cb9db9173595cf6c8cdd11a42ab--b6b3--image-20200902135349741.png)



