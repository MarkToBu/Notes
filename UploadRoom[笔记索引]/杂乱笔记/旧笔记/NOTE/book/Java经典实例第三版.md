



  #                                 Java经典实例第三版


## 第三章：Java字符串
     Java字符串是一个预定义的类型。也就是说，一个字符串并不是一个字符数组，而是一个对象。所以他有自己的方法。（endWith(""),equals("")）. String对象点构造，就不能在改变。但是可以让变量因哟红另一个字符串。   可以通过== charAt() ==方法，从原来的字符串中提取对应的字符串。
-      需要改变字符串中的字符，可以使用StringBuilder对象（初始化为String对象的值），在修改StringBUilder对象，最后调用toString（）方法转化String对象。
-      *字符串的不变性是Java虚拟机的一个基本特征。* Stirng类换被修饰为final类，因此他不能被子类化。
### 使用String 的方法 
1.     substring() 分解字符串

```
         public static void main(String[] args) {
		 String a="Java is great.";
		 System.out.println(a);
		 
		 String b=a.substring(5);    //is great.
		 System.out.println(b);
	     
		 String c=a.substring(5, 7);  // is
		 System.out.println(c);
		 
		 String d=a.substring(5, a.length());  //is great.
		 System.out.println(d);
	}

```

2.     利用StringTOkenizer分解字符串：
      将一个字符串分解为单词或者token（标记）。
      解决之道：

```
    public class StringTokenizerDemo {
    public static void main(String[] args) {
         StringTokenizer st=new StringTokenizer("Hello world of Java");
         while(st.hasMoreTokens()) {
        	 System.out.println("Toke:"+st.nextToken());
         }
         
         //根据其他分隔符来划分字符串
         
         StringTokenizer  stringTokenizer=new StringTokenizer("Hello,world|of|Java", 		  ", |",true);
	      while (stringTokenizer.hasMoreTokens()) {
	    	  System.out.println("Token"+stringTokenizer.nextElement());
	      }
         }
		}

```
-  有时使用正则会，会更灵活的代替stringTokenizer.

```
        //统计分离出数字
        CharSequence inputString="123..4..5678..9..";
	      StringBuilder sb=new StringBuilder("");   //承接字符串
		Matcher token=  Pattern.compile("\\d+").matcher(inputString);
          while (token.find()) {
        	  String courseString=token.group(0);
        	  int courseNumber=Integer.parseInt(courseString);
        	  sb.append(courseNumber).append();
        	 System.out.println(courseNumber);  
          }
          System.out.println(sb.toString());

```
-  连接字符串
  解决方案：StringBuidler对象，调用其append方法。也可以使用SgringBuffer对象。
        

```
    public static void main(String[] args) {
		StringBuilder builder=new StringBuilder();  
	    builder.append("");
        
        
        //StringBuffer的使用
        StringBuffer sb=new StringBuffer();
        while(st.hasMoreElements()){
             sb.append(st.nextToken())
             sb.append(",");
        }
        return sb.toString();
		}
        
      

```

-  处理单个字符
   解决办法：   采用循环一级String的charAt（）方法

```
     public static void main(String[] args) {
		StringBuilder builder=new StringBuilder(); 
	    builder.append("");
	    
	    //StrCharAt.java
	    String a="A quick bronze fox leapt a lazy bovine";
	    for(int i=0;i<a.length();i++)
	    	System.out.println("Char"+i+" is " + a.charAt(i));
	    
		}
	}
```

####  Unicode字符和String字符的转换

```
StringBuffer b=new StringBuffer();
	  for( char c = 'a';c<'b'; c++) {
		  b.append(c);
		  b.append('\u00a5');
		  b.append('\u01FC');
		  b.append('\u0391');
		  b.append('\u03A9');
		  
		  for( int i=0;i<b.length();i++) {
			  System.out.println("Character #" + i + " is " +b.charAt( i));
		  }
		  System.out.println("Accumulated charcters are" +b);

```

-  改变字符串的顺序
   解决之道：  StringBuilder类 （使用StringTokenizer对象和Statck（堆栈）对象）。
              堆栈对象LIFO
              
```
         //StringBuffer的reverse方法   //反转字符串
		  String sh="abcdef";
		  System.out.println(new StringBuffer(sh).reverse());

```

```
     //颠倒单词显示
		  String  sr="Good is Not Good ,But bad Is realy Bad";
		  //向栈中压入元素
		  Stack myStack=new Stack();
		  StringTokenizer stringTokenizer=new StringTokenizer(sr);
		  while(stringTokenizer.hasMoreTokens()) {
			  myStack.push(stringTokenizer.nextElement());
			  
		  }
		  
		  //打印堆栈中的元素
		  System.out.print('"'+ sh +'"' + "backwards by word is:\n\t");
	       while (!myStack.isEmpty()) {
	    	   System.out.print(myStack.pop());
	    	   System.out.print(' ');
	       }
	       System.out.println('"');

```

- 注意，reverse（）方法在集合中同时也有同名方法，作用是将元素反转，注意区分

```
  create array list object
      ArrayList arrlst = new ArrayList();
      
      // populate the list
      arrlst.add("A");
      arrlst.add("B");
      arrlst.add("C");
      arrlst.add("D");
      arrlst.add("E");

      System.out.println("The initial list is :"+arrlst);
      
      // reverse the list
      Collections.reverse(arrlst);
      
      System.out.println("The Reverse List is :"+arrlst);
   }
}
```

现在编译和运行上面的代码示例，将产生以下结果。

```
The initial list is :[A, B, C, D, E]
The Reverse List is :[E, D, C, B, A]
```

###  

	public static void main(String[] args) {
	
			//按照字符或单词颠倒字符串：
	
			// TODO Auto-generated method stub
	
	        Stack myStack = new Stack();
	
	        String sh = "exec";
	        
	        String ok = str.toUpperCase();
	
	        System.out.println(new StringBuffer(sh).reverse());
	
	        //reverse方法在集合中也有，反转集合的元素
	    
	        }

### 控制字母的大小写   toLowerCase( )
### 控制字母的大小写   toLowerCase( )

```
public static void main(String[] args){
		String str = "ETOAK";
		String ok = str.toLowerCase();
		System.out.println(ok);
		System.out.println(str);
	}
	
	public static void main(String[] args){
		String str = "etoak";
		String ok = str.toUpperCase();   //
		System.out.println(ok);
		System.out.println(str);
	}
```

### 转义字符    

```
/t        制表符  
/n        换行符
/r        回车
/f        换页
```

### 去空格  Trim（）

  str1=str.trim()



##    第四章： 正则表达式的匹配模式  [Regular Expression] => Regex

   开始： JDK 1.4  java.util.regex

###   4.1：正则语法
    ^       行或 字符的开始
    $       行或字符的结束 
    .       任意字符
    [...]   封闭集合中的一个字符。  例如：[abcde] 可以匹配  a、b、c、或e
    [^...]  求补集中匹配的字符
    {}      分组
    |       或，选项
    {m,n}   进行n-m次匹配
    {m,}    进行m次以上的匹配
    {m}     进行m次匹配
    {,m}    至少进行m次匹配
    
    \t      制表符
    \r      火车
    \f      换页
    \w      任意单词字符
    \W      非单词字符
    \d      数字字符 。  \d+表示 一个数字
    \D      非数字字符串
    \s      空白字符串
    \S      非空白字符串
  == 注意： Java中的特殊字符（如 反斜线号本身，双引号 等等）需要加上转义字符（反斜线号） ==
    如：   "Yoi side it\."   必须写为     " \"You side it\\.\" "
    ^T[aeiou]\w    表示以T为首字母，后接着一个元音字母（aeiou）,最后正跟这任意多个字母，最后是
    ^Q[^u]\d+\..   字母Q+非u字母+任意多个数字+句号
    
​    
###    4.2：正则表达式：测试模式

java.uitl.regex有啷个类组成：Pattern和Matcher

       问题：     Java正则表达式，看看给定的模式是否能给一个给定的字符串匹配。java.util.regex

```
    if(inputString.maches(string.RegexPattern)){
    //匹配.....用它做一些事情
    }

```
当程序中多次使用正则表达式。则构造和使用Pattern和其Matcher(s)会更有效。

构造一个Pattern进行匹配实例：
```
public class RESSimple{
    publc static void main(String [] args) throws RESytaxException{
        String pattern="^Q[^u]\\d+\\.";
        String input="QA777.is the next filght. It is on time.";
        Re r=new  Re(pattern);
        boolean found=r.match(input); 
        }
 }


```

正则表达式公共API

Pattern  final类型

Matcher  final类型

以上API正则在程序匹配中的一般步骤是：
      1. 通过调用静态方法Pattern.complie()创建一个模式；
      2. 为每个String（或CharSequece）调用pattern.matcher(CharSequence)以从模式中请求一个Mathcher;

3. 在结果Matcher中调用一次或多次finder方法。

   | Pattern类的方法Pattern类的方法     | 作用                             | Mathcer                 | 作用             |
   | ---------------------------------- | -------------------------------- | :---------------------- | ---------------- |
   | complie(String patt)               | 构造方法                         | boolean   matches()     | 运行：查找或匹配 |
   | complie(String patt,int flags)     | 构造方法                         | boolean  find()         | 运行：查找或匹配 |
   | Matcher matcher()                  | //为Pattern获取一个Matcher的方法 | boolean find(int start) | 运行：查找或匹配 |
   | String pattern()                   | Informationg方法                 | boolean lookingat()     | 运行：查找或匹配 |
   | boolean matches                    | //Convenience方法                |                         |                  |
   | String[] split(CharSequence input) | //Convenience方法                |                         |                  |
   | String[] split(CharSequence input) | //Convenience方法                |                         |                  |

   | String 于regex相关的方法             |
   | ------------------------------------ |
   | boolean matches(String regex)        |
   | String replaceFirst(String regex)    |
   | String[] spit(String regex)          |
   | String[] split(String regex,int max) |

   - 在标准方法中可以使用以上的步骤，但是如果只是使用了一次的话，更推荐使用String convenience流程；

     ​

方法比较:
    match()   将整个字符串和模式相比较，与String中相同。匹配整个字符串。
    lookingAt（） 从字符串开始进行的模式匹配。
    find()    从字符串的开始进行模式和字符串匹配。
所有方法都已bool值类型返回，

常规使用：
```
   Matcher m=Pattern.compile(patt).matcher(line);
   if(m.find()){
       System.out.println(line+ " matches " + patt);
   }

```

### 4.3查找文本匹配  (实例)
    如果想知道模式匹配了哪些字符。可以在方法成功后调用 information方法匹配信息。
    
    start(), end();
    返回字符串中匹配的开始和结束位置的信息。
    groupCount
    返回和使用括号括起来获取的分组数，如果没有分组，则返回0.
    group(int i)
       如果i<=groupCount()值，则返回与分组匹配的字符。
       在 regex处理中，圆括号内最先进行处理。
       .在正则嵌套时，调用 group（int）方法则可以获得指定嵌套所匹配的内容。

```
 		String patt="Q[^u]\\d+\\.";
 		Pattern r=Pattern.compile(patt);
 		String line="Order Qt300. Now!";
 		Matcher m=r.matcher(line);
 		if(m.find()){
            System.out.println(patt + " matchers \" "+
              m.group{0}  +
              "\" in \" " + line + "\"");
              
        }else{
            System.out.println("NO Match");														
        }

```

###  4.4替换匹配的正文	
   replaceAll(newStirng)
```
       替换全部符合正则表达式的String patt = "d[ae] {1,2}mon";
		String input="Unix hath demons and deamons in it!";
		System.out.println("Input: " + input);
	    
		//从regex的实例中运行方法
		Pattern r=Pattern.compile(patt);
		Matcher matcher=r.matcher(input);
		System.out.println("ReplaceAll: " + matcher.replaceAll("daemon"));
		
		//appendReplacement方法
		matcher.reset();
		StringBuffer sb=new StringBuffer();
		System.out.println("Append methods: ");
		while(matcher.find()) {
			matcher.appendReplacement(sb, "daemon");
		}
		matcher.appendTail(sb);
		System.out.println(sb.toString());
        
```

从文件中读取字符串

```
	//regex 模式
    	Pattern pattern=Pattern.compile("[A-Za-z][a-z]+");
    	BufferedReader reader;
     
			 reader=new BufferedReader(new FileReader(args[0]));
		 
    	String line;
    	while((line=reader.readLine()) != null) {
    		Matcher matcher=pattern.matcher (line);
    		while (matcher.find()) {
    			//最简单的方法
    			System.out.println(matcher.group(0));
    			
    			//获得文本的开始位置
    			int start = matcher.start(0);
    			
    			//获得结束的位置
    			int  end  = matcher.end(0);
    			System.out.println("start=" + start +
    					"; end=" + end );
    			//调用CharSequence.substring(offset,end);
    			System.out.println(line.substring(start, end));

```

- Pattern.complie()的七种标记
  1.   CANON_EQ  启用“规范等价”，以基字符进行匹配。如 e
  2. CASE_INSENSITIVE    进行与大小写无关的匹配
  3. COMMENTS  模式中允许使用空白的注释。
  4. DOTALL    dot(.)匹配包括终止符号在内的所有字符。
  5. MULTILINE  多行模式
  6. UNICODE_CASE 启用Unicode感知
  7. UNIX_LINES 在多行模式下匹配时，只是将“\n"，看做终止符。     

##  第五章  基本数据类型

### 5.0 基本数据类型

   

| 基本数据类型 | 封装对象  | 大小(位) | 内容             |
| ------------ | --------- | -------- | ---------------- |
| byte         | Byte      | 8        | 有符号位整数     |
| short        | Short     | 16       | 有符号整数       |
| int          | Integer   | 32       | 有符号整数       |
| long         | Long      | 64       | 有符号整数       |
| float        | Float     | 32       | IEEE-754浮点数   |
| double       | Double    | 64       | IEEE-754浮点数   |
| char         | Chatacter | 16       | 无符号Unicod字符 |

### 5.1 检查字符串是否为有效数字

```java
main(){
    String str1 ="123";
    String str = "abc";
    double result ;
    
    try{
       result = Double.parstDouble(str1);
    }catch(NumberFormatException ex){
        System.out.println(str1 +":非数字");
           return;
    }
    System.out.println("Ok");
    
    
}
```

###  5.2    数据类型由大变小

强制类型转换

```
int i,k;
double j = 2.75 ;
i = j;   //报错
i = （int） j;
byte b;
b = i;  //报错
b = (byte)i ;
```

### 5.3  数字和对象的互相转换

``` 
Integer i1 = new Ingeter(42);

System.out.println(i1.toString());

int i2 = i1.intValue();



```

### 5.4 使用分数

解决：分子与整数相乘，在除以分母

  缺点： 减低代码的可维护性，效率优先时 使用。

### 5.5 确保浮点数的准确性

解决 ：先比较 INFINTY常量 	然后使用 isNaN()      //Not a Number ,非数

- 定点数（整数）的操作，对于有被零除的情况，只会抛出一个异常。因为整数被零除会认为是逻辑错误。

- 然而浮点数却不会，浮点数被定义为取值范围无限。如果用零去除正的浮点数，程序将产生POSITIVE_INFINITY来标识错误；零去除负数的浮点数，则产生NEGATIVE_INFINITY；若果产生一个吴晓东结果，则说明是NaN(非数)。这个三个公共常量都被定义在Float和Double的封装类中。

- NaN值有特性：  NaN ！= NaN;因此将一个数与NaN比较是没有意义的。因为表达式 

  ​    x == NaN      

  永远不会为True.

  - 判断非数字的方法： Float.isNaN(float) 和 Double.isNaN(double)方法来判断非数字：

     eg:InfNan.java



### 5.6 判断两个浮点数是否相等

 ```Java
FloatCmp.java  
 ```

### 5.6 浮点数的舍入

解决方案：   截取字符串  |  Math.round()  

- Math.round()若给定的方法是float类型，返回长整型；参数float返回int型;

   Math.floor(d);  //向下取整；

  如果需要按照地域常用精度的方式显示某个数，可以使用DecimaFormate或者

  Formatter对象。

### 5.8 对数字的格式化

解决：使用NumberFormat子类

```
java NumFormat2
main(){
    NumberFormat form = NumberFormat.getInstance();
    form.setMinimumIngegerDigits(3);
    form.setMinmumFractionDigits(2);
    from.setMaximumFractionDigits(4);
    form.format(100.123415);
}

```

DecimalFormat模式的字符

- DecimalFormat的模式字符

         #     数字       0    数字(以0开头)       .  特定区域的小数点分割符          ,  特定区域的分组分符
         -  特定区域的负号指示器      %  百分比表示数值       ;  两种格式分开:第一位正数,第二为负数
         '  某个字符需要转义时使用.

​    自定义模式打印字符

```java
NumFormatTest.java
   public static final double intNumber = 1024.25;
   
   public static final double outNumber = 100.2345678;
  
main(){
     NumberFormat defForm = Number.getInstance();
     NumberFormat ourForm = new DecimalFormat("##0.##");
     System.out.prinltn(((DecimalFormat)defForm).toPattern());    //显示特定区域的模式
    defform.format(intNubmer);
    ourForm.format(ourNumber);
    
    
    
}
```

### 5.9  进制之间的转换

解决办法： Integer类提供了方法：  toBinaryString()  //整数转化为二进制

  							valueOf()              // 将二进制转化为整数

```
Ingeter.valueOf(bin,2);
Ingeger.toBinaryString(i);
```



### 5.10 整数序列  

- 连续的整数序列用for循环来处理，不连续用java.uitl.BitSet类。

### 5.11  名词的复数形式

解决办法: 使用三目运算符（条件语句） 或者ChoiceFormat类。

```Java
public static void main(String args[]){
      report(0);
      report(1);
      report(3);
}
public static void report(int i){
    System.out.println( "使用了"+i + "个" + "item" +i == 1 ? "":"s");
}//相当于使用了if..else

```



- 使用 ChoiceFormat类
- ChoiceForma可以用来制作映射，，，菜单也可以。

 ```
ChoiceFormator.class  //可用类，在代码中可以查到
static  double [] limits = {0,1,2};
  	static  String[] formats = {"reviews","review","reviews"};
       //使用ChoiceFormat 将数值转化为英文文本
  	static ChoiceFormat myFormat = new  ChoiceFormat(limits,formats);
      //测试数据
  	static int[] data = {-1,0,1,2,3};
  	public static void report(int n) {
  		System.out.println("We user " + n + " "+ myFormat.format(n));
  	}
  	public static void main(String[] args) { 
         report(0);
         report(1);
  	   report(2);
  	}
 ```

- 产生随机数

  解决:java.lang.Math.random;

  1. Math.random();    //该方法只能产生double的值，如果你需要整数值，进行转换

  2. Random rd = new Random();      rd.nextInt(10);

     - java.util.Random  方法不是静态的，必须构造新对象；
     - rd.nextInt(); rd.nextFloat();  rd.nextDouble();   返回值都是基本数据类型；
       - rd.nextGaussian() ; 可以显示下一个随机数；  //高斯分布
       - rd.nextRandm();            //平滑分布

### 5.16&&5.16 取对数。三角函数

Math.PI;

Math.E

Math.cos();

- 取对数

  Math.log(double);

  ...
### 5.19 处理特大的数字

  解决：使用BigIngeter 或者BigDecimal类；

- 构造方法是从字符中提取数字，不能直接输入数字，因为数字巨大。
- 对象不可变。BigIngteger主要用于密码及安全认证， isProbablyPrime()方法来创建公共秘钥对。BigDecimal对天文应用较多。

EG: 例程 见笔记  TepmConverter   

### EG:历程 数字回文

```SQL
package Java经典实例.Chapter5;

 import java.util.*;

public class Palindrome {
	// 回文数
	//8989会触发异常，长度问题
	public static void main(String[] args) {
		
		 long num = new Scanner(System.in).nextLong();
		if(num<0) {
			return ;
		}
		System.out.println(num + "回文数"   +findPalidrom(num));

	}

	static long findPalidrom(long num) {
		if (num < 0)
			throw new IllegalStateException("Negtive");
		if (isPalindrome(num))
			return num;
		System.out.println("Trying" + num);
		return findPalidrom(num + reverseNumber(num));
	}

	protected static final int MAX_DIGITS = 19;

	static long[] digits = new long[MAX_DIGITS];

	static boolean isPalindrome(long num) {
		if (num >= 0 && num <= 9) {
			return true;
		}
		int nDigits = 0;
		while (num > 0) {
			digits[nDigits++] = num % 10;
			num /= 10;
		}
		for (int i = 0; i < nDigits; i++) {
			if (digits[i] != digits[nDigits - i - 1]) {
				return false;
			}

		}
		return true;
	}

	static long reverseNumber(long num) {
		int nDigits = 0;
		while (num > 0) {
			digits[nDigits++] = num % 10;
			num /= 10;
		}
		long ret = 0;
		for (int i = 0; i < nDigits; i++) {
			ret *= 10;
			ret += digits[i];
		}
		return ret;
	}

}

```

## 第六章，日期和时间

### 6.0 简介

   早期日期问题：以英格兰文化为中心，起始日期是 1970-01-01（Unix诞生时间）。这时，Java日期的年份值是整数，最小值为70，表示1970年。99则代表1999年等，100也就是2000年。千年虫问题，Java无法计算1970年之前的日期。- -  --   

- **Calendar类**:为了解决‘以英格兰为中心’ 和 ‘和1940年问题’.

  Calendar类可以表示西方公历。

  - 与日期相关的类，一部分在java.text包中。另一部分在java.uitl包中。

    text包中，的类和接口，是以独立于自然语言的方式处理文本、日期、数字和消息。java.util包具有集合框架、集合类、事件模型、日期与事件工具、国际化工具以及其他各种工具类。大部分需要同时导入。

     ### 6.4 查看当前日期

-  java.util.Data() . toString();

- Calendar类  ：

     **Calendar.getInstance().getTime()**    =>获得一个Date对象 ，在调用toString()方法，或者使用DateFormat对象，在打印。  // 返回本地化的对象。

### 6.2按照指定格式打印日期/事件

  方案：java.text.DataFormat对象

  //无论区域，只要通过DataFormat.getInstance()方法获得，就能以正确方法显示日期：

```
Date today = new Date();
DateFormat  df = DateFormat.getInstance();
System.out.println(df.format(today));
DateFormat df_fr = DateFormat.getDateInstance(DateFormat.Full,Locale.FRENCH);
System.out.println(df_fr.format(today));
```

- SimpleDateFormat

  ```Java
  Date now = new Date();
  System.out.println(now.toString());
  SimpleDateFormat formatter = new SimpleDateFormat("yyyy-MM-dd hh-mm-ss");
  System.out.println(formatter.format(now));

  ```

### 6.5 将字符串转换为日期

方案: 使用**DataFormat**类，

```java
		SimpleDateFormat formatter = new SimpleDateFormat("yyyy-MM-dd");
       String input = new Scanner(System.in).next();
             input  = intput.length == 0 ? "1818-11-11" : input;
         Date t ;
try{
      t = formatter .parse(input);
    System.out.println(t);
}catch (Exception e){
    System.out.println("格式异常");
}
        
```

### 6.7 对日期进行计算

``` Java
 Date now = new Date（）；
long t = now.getTime();
t  = 700    *        60     *   60  * 1000;
Date then = new Date(t);
System.out.println("700年以前是：" + then);

```

- 通过Calendar的add()方法进行计算；、

  ``` java
  import java.text.*;
  import java.util.*;

  public class DateCalAdd
      public static void mian(String [] args){
          Calendar now  = Calendar.getInstance();
         SimpleDateFormat formatter = new SimpleDateFormatt("yyyy-MM-dd hh:mm:ss");
         now.add(Calendar.Years, -2);
         System.out.println("两年以前的日期:" + now.getTime());
      }
  ```

  ​

###  6.8 计算日期之间的间隔

方案：调用Date对象的getTime()方法，并将它们转换为长整型。然后相加减，在进行格式化。

```java
import java.util.*;
public class DateDiff{
    public static void main(String[] args){
        Date d1 =new GergnorianCalendar(2000,11,31,23,59).getTime();
        //当前日期
        Date today = new Date();
        long diff = today.getTime() - d1.getTime();
        System.out.println("两个日期之间相距：" + (diff /(1000*60*60*24)));
        
    }
}
```

- Calendar 类的add（）方法，只能在Calendar对象中增加年、月、日。并不能将两个Calendar对象相加。

  如果系统需要，可以使用Oracle数据库提供的方法进行日期间隔计算函数。 months_between(SYSDATE,DATE);

    日期相加减也可以。

  ​

### 6.9比较日期

方案:   如果日期表示为Date()对象，可以使用equals()方法，以及before()或者after()方法进行比较；

如果日期为长整型，可以使用操作符 "=="  ,以及  '< '  '>'进行比较。

```java
import java.util.*;
import java.text.*;

public class CompareDates{
    public static void main(Stringp[] args){
          DateFormat df = ne SimpleDateFormat("yyyy-MM-dd");
          Date d1 = df.parse(value1);
          Date d2 = df.parse (value2)
          String relation;
        if(d1.equals(d2)){
            relation = "the same date as";
            }
        else if(d1.before(d2)){
            relation = "before";
        }else{
            relation = "after";
        }
      System.out.println( d1 + before + d2);
    }
}
```

### 6.10 第几日   给定日期是第几日

方案 ： 使用Caledar  类的  get()  方法；

###       *: 如何解析日期和时间

​		1st.java.util.Date
			getYear() getMonth()+1 getDate()
			getHours() getMinutes() getSeconds()
		2nd.java.util.Calendar
			get(1)  get(2)+1 get(5)
			get(11) get(12) get(13)
		3rd.java.text.SimpleDateFormat
			format() 从long类型到String
			parse()  从String 到Date再到long (getTime())

```java
 public static void main(String[] args){
	  System.out.println("a");
	  
	  Calendar cal = Calendar.getInstance();
	  System.out.println(cal);
	  System.out.println(cal.get(Calendar.YEAR)+ "年" + (cal.get(Calendar.MONTH)+1)  + "月"
                        +  cal.get(Calendar.DATE)  +"日"+  cal.get(Calendar.HOUR) 
						+"时" +cal.get(Calendar.MINUTE) +"分" +cal.get(Calendar.SECOND)	+"秒"            );
										
}
			
```

###  6.11 	测量花费的时间

方案：  System.currentTimeMillis();  或者System.nanoTime();

将两个数值相减；

```
long start = System.currentTimeMillis();
method();  //执行的方法
long end = System.currentTimeMillis();
long speed = end - start;
/////    时间单位： 毫秒
long start = System.nanoTime();
method();   //执行的方法
long  end  = System.nanoTime();
long elapesd = end - start;
//时间单位：微毫秒

```

//将一个长整型时间值转换为妙

 public static String msToSec(long t){

​        return  Double.toString(t/1000D);

}

### 6.13 休眠

Thread.sleep();

```
while(true){
    System.out.println(new Date() +"\007");
    Thread.sleep(5*60*1000);
}
```

### 6.14  案例：提醒服务

ReminderService()类；



##  第七章    结构化数据、

###   7.1数组

概念： 逻辑结构为线性表。一组类型相同，且存储空间连续的数据。

​       数组元素可以是简单型数据（int,boolean），也可以是复合型数据(对象，数组)。

- 0.3 * 3 ！= 0.9
- Java的数字处理可靠性，可移植性 ， 和编程的灵活性。

###   7.2自动调整数组长度

### 7.3 ArrayList类     --->存储空间动态分配

​       ArrayList是标准类，它封装了数组的功能，并且可以进行自动扩展。该类为Java中的官方类，实现方法为Object数组，但你不能通过数组访问语句来存取ArrayList对象，该数组为private，只能通过调用方法操作ArrayList。

​       与Java.util包中所有集合类一样，ArrayList类的存取方法，都是根据java.lang.Object定义的。由于Object类是每个类的先祖，所以List对象可以容纳任何类型的对象。

但是在引用这些对象时，你必须对他们进行实例化。集合类只能容纳对象引用。对于基本数据类型，只能使用他们的封装类。

List访问方法

| add(Object o)        | 给末尾添加给定元素                         |
| -------------------- | ------------------------------------------ |
| add(int i, Object o) | 给指定位置添加元素                         |
| clear()              | 清除集合中的元素                           |
| Contains(Object o)   | 如果Vector包含给定元素，返回真             |
| get(int i)           | 返回指定位置的对象句柄                     |
| indexOf(Object o)    | 如果找到给定对象，则返回索引值，否则返回-1 |
| remove(int i)        |                                            |
| toArray()            | 返回包含集合对象的数组                     |
|                      |                                            |

[^    老版本的Vector类和Hashtalbe类都是集合架构中比较早的类，它们的方法名字不同于现在的集合方法，例如：Vector类的addElement（）方法和elementAt()方法，现在的新方法分别是add()方法和get()方法。另一个不同之处是：对于多线程的支持    ]: 

> - [ ] ArrayList和Vector底层都是以数组实现的
>
> 1.同步特性不同 -> 是否支持多线程同时操作
> 	ArrayList 同一时间允许多个线程同时进行操作
> 		效率较高 但是可能出现并发错误	   - 肯德基
> 	Vector 同一时间只允许一个线程进行操作
> 		效率较低 但是绝对不会出现并发错误    - 把子肉
>
> 2.它们底层实现不同 -> 扩容机制上
> 	ArrayList 扩容机制
> 		JDK6.0及之前		JDK7.0及之后
> 		x*3/2+1			x+(x>>1)
> 	Vector 扩容机制
> 		new Vector(10)		new Vector(10,x)
> 		*2			+x
>
> 3.它们出现的版本不同
> 	ArrayList     since 1.2
> 	Vector	      since JDK1.0	[集合两大鼻祖之一]

 

###  7.4 Iterator(迭代器）

当不知到集合类型的情况之下，查看集合的内容可以 **迭代器  **。



```java
main(String[] args){
    List list= new ArrayList();
    //数据示例
    StructureDemo source = new StructureDemo(15);
    list.add(source.getDate());
    list.add(source.getDate());
    
    //使用迭代器
    Iterator car = list.iterator();
    while( it.hasNext()){
        Objecgt o=car.next();
        Syso(o.toString());
    }
    
}
```

### 链表 结构  LinkedList

通常用来存储未知数量的数据。

```java
//LinkedList使用示例
import java.util.*;
public class TestLinkedList{
	public static void main(String[] args){
		//<Integer>泛型 JDK5.0特性
		//<> 泛型自动推断 钻石语法 JDK7.0特性
		List<Integer> list = new LinkedList<>();//如何创建集合对象
		//如何添加元素
		list.add(777);   //Integer.valueOf(111)
		list.add(444);
		list.add(555);
		list.add(666);
		//集合的工具类 Collections 当中提供一次添加多个元素的方法
		Collections.addAll(list,777,444,555,666);//往哪个集合里面添
		//如何得到元素个数
		System.out.println(list.size());//data.length
		//如何得到第几个元素
		System.out.println(list.get(0));//data[0]
		System.out.println("===============");
		//如何遍历
		//1st way : for + get()
		for(int i = 0;i<list.size();i++){
			System.out.println(list.get(i));
		}
		System.out.println("===============");
		//2nd way : foreach / forin
		for(Integer x : list){
			System.out.println(x);
		}
		System.out.println("===============");
		//3rd way : Iterator牌小推车...
		for(Iterator<Integer> car = list.iterator(); car.hasNext(); ){
			Integer x = car.next();
			System.out.println(x);
		}
		/*
		{
			Iterator<Integer> car = list.iterator();
			while(car.hasNext()){
				Integer x = car.next();
				System.out.println(x);
			}
		}
		*/
		System.out.println("===============");

		//4th way : lambda表达式 JDK8.0新特性
		list.forEach(System.out::println);
		System.out.println("===============");
	}
	public static void kill(Object obj){
		System.out.println(obj);
	}
}
```

> ArrayList 底层采用数组实现
> 	数组最大的优势就是连续存储 为了保证连续存储
> 	它的添加和删除操作 都会比较复杂
> 	它的优势在于查找遍历和随机访问(get(x))效率很高
> 	但是添加和删除效率很低

LinkedList 底层采用链表实现
	它的优势在于添加和删除操作 效率极高
	但是查找遍历和随机访问(get(x)) 效率极低
	*: 使用LinkedList请尽量回避它的get() 

### 7.6使用Hashtable和HashMap进行映射

将一个对象映射为另一个对象，可以使用（散列表）来查找对象。

HashMap类（Jdk 1.2新增）和 Hashtalbe 类提供从一组对象到另一组对象的单向映射表。 

```Java
HashDemo.java
//散列表的使用
HashMap h = new HashMap();
h.put("Adobe","Moungtian View, CA");
h.put("IBM","White Plains, NY");
h.put("O'Reily & Associates ","Sebastopol");
h.put("SUN","Mountain View, CA");
//根据主键查找相对的内容
String queryString = "O'Reilly & Associates";
String resultString = (String)h.get(queryString);
Syso(resultString);

//获取所有键值对，进行操作
  {
  Iterator it = h.values().iterator();
 while( it.hasNext() ){
      String key=(String) it.next();
      String result=h.get(key);
          
  }
}
```

###      7.7Properties类和Preferences类

 java.util 
​	类 Properties
	java.lang.Object
	  继承者 java.util.Dictionary<K,V>
     		 继承者 java.util.Hashtable<Object,Object>
          		继承者 java.util.Properties

​	java.util 类 Propertie   java.util 类 Propertis

```
java.lang.Object
  java.util.Dictionary<K,V>
      java.util.Hashtable<Object,Object>
          java.util.Properties
```

- **所有已实现的接口：** 

  [Serializable](../../java/io/Serializable.html), [Cloneable](../../java/lang/Cloneable.html), [Map](../../java/util/Map.html)<[Object](../../java/lang/Object.html),[Object](../../java/lang/Object.html)> 


- **直接已知子类：** 

  [Provider](../../java/security/Provider.html) 

  - `public class **Properties**extends Hashtable<Object,Object>`
    Properties` 类表示了一个持久的属性集。`Properties` 可保存在流中或从流中加载。属性列表中每个键及其对应值都是一个字符串。 

  一个属性列表可包含另一个属性列表作为它的“默认值”；如果未能在原有的属性列表中搜索到属性键，则搜索第二个属性列表。 

  因为 `Properties` 继承于 `Hashtable`，所以可对 `Properties` 对象应用 `put` 和 `putAll` 方法。但不建议使用这两个方法，因为它们允许调用者插入其键或值不是 `String` 的项。相反，应该使用 `setProperty` 方法。如果在“不安全”的 `Properties` 对象（即包含非 `String` 的键或值）上调用 `store` 或 `save` 方法，则该调用将失败。类似地，如果在“不安全”的 `Properties` 对象（即包含非 `String` 的键）上调用 `propertyNames` 或 `list` 方法，则该调用将失败。 

  [`load(Reader)`](../../java/util/Properties.html#load(java.io.Reader)) `/` [`store(Writer, String)`](../../java/util/Properties.html#store(java.io.Writer, java.lang.String)) 方法按下面所指定的、简单的面向行的格式在基于字符的流中加载和存储属性。除了输入/输出流使用 ISO 8859-1 字符编码外，[`load(InputStream)`](../../java/util/Properties.html#load(java.io.InputStream)) `/` [`store(OutputStream, String)`](../../java/util/Properties.html#store(java.io.OutputStream, java.lang.String)) 方法与 load(Reader)/store(Writer, String) 对的工作方式完全相同。可以使用 [Unicode 转义](http://java.sun.com/docs/books/jls/third_edition/html/lexical.html#3.3)来编写此编码中无法直接表示的字符；转义序列中只允许单个 'u' 字符。可使用 native2ascii 工具对属性文件和其他字符编码进行相互转换。 



### 7.8  排序

实现方式：使用Arrays.sort（）或Collections.sort()   或者实现Comparator接口

在数组中，使用 Arrays.sort();

在集合中使用Collections.sort();

```Java
public static void main(String[] args){
        {  
        //数组的排序
         String strings = { 
        "painful",
        "mainly",
        "gaining",
        "raindrops"
          };
          //排序
          Arrays.sort(strings);
          //遍历查看
          for(int i=0; i<strings.length;i++){
              System.out.println(stirngs[i]);
          }  
        }
       
        {
           List list = new ArrayList();
            list.add("painful");
            list.add("mainly");
            list.add("gaining");
            list.add("raindrops");
            
            //对集合进行排序
            Collections.sort(list);
            for(int i=0; i<list.size(); i++){
                 System.out.println(list.get(i));
            }
        }
   
     
}
```

当缺省的排序方法不能满足要求，可以创建一个对象实现Compartor接口。再将该对象作为排序的依据，

```java
public class SubStringComparator implements Comparator{
    public int compare(Object o1,Object o2){
        String s1 = o1.toString.subString(1);
        String s2 = o2.toString().subString(1);
        return s1.compareTo(s2);
    }
}
```

使用实现Comparator几口的对象作为sort()方法的第二个参数

```java
... main(String[] args){
    String[] Strings = {...};
    Arrays.sort(strings);     //无参数的数组排序方法
    
    Arrays.sort(strings,new SubstringComparator());
    
   //遍历数组
    
}
```

方法3：  在定义类的时候，就实现了内置的比较功能，而不用实现Comparator接口对象。

直接实现java.lang.Comparable接口。

- String类，封装类、java.io包中的file、java.util.Date类都实现了这个接口。
- 自然排序：在没有提供Comparator的情况下，也可以进行的排序。一个类的自然排序要与其equals()方法保持一致，当e1.compareTo((Object)e2) == 0,当且仅当，e1.equals(e2);

       ## 7.9在插入时进行排序

​        方案：  使用Treeset  或者TreeMap对象手工完成。

​      Treeset:使主键保持有序，同时与值简历映射关系。

    ​```
TreeSetDemo.java；
...

TreeSet tm = TreeSet(String.CASE_INSENSSITIVE_ORDER);
//对大小写不敏感

tm.add("Gosling");
tm.add("da Vinci");
tm.add（"van Gogh");
tm.add("Java To go");
tm.add("Darwin");
tm.add("ToWin");
for(Iterator car = tm.Iterator();car.hasNext();){
    System.out.println(tm.next());
}
    ```

//可以将HasTable后者HashMap对象转换为TreeMap对象 

TreeMap sorted = new TreeMap(哈希table)；

###   7.10排除重复元素

方案：使用Set接口垫 实现类。

HashSet h = new HashSet();

h.add("one");

h.add("two");

h.add("one");

for(Iterator car = h.iterator();it.next; ){

System.out.println(it.next());

}

### 7.11搜索对象

方案：要求集合对象返回查询结果。

- **搜索方法**

  | 方法            | 作用                       | 对象的类                               |
  | --------------- | -------------------------- | -------------------------------------- |
  | binarySerach()  | 快速搜索                   | Arrays，Collections                    |
  | contains()      | 线型搜索                   | ArrayList,HashSet,Hashtable            |
  | containsKey()   | 检查是否包含特定的主键     | HashMayp，Hashtable                    |
  | containsValue() | 主键（或给定值）           | Properties,TreeMap                     |
  | indexOf()       | 若找到给定对象，返回其位置 | ArrayList,LinkedList,List,Stack,Vector |
  | search()        | 线性搜索                   | Stack                                  |

- 使用Arrays.binarySearch()方法对数组进行搜索过程中的过程中，应该先保证数组的有序性。调用Arrays.sort()方法，在进行搜索。

  ```java
  public class ArrayHunt{                  //极简用法
      int [] numbers = {154,45,82,154,56,556,85,485,248,15,5,15,385,28};
      Arrays.sort(numbers);
      i = Arrays.binarySearch(numbers,2);        //在排序过得数组中寻找数字2；
      
      
  }
      
  ```

  - Collections.binarySearch()方法与此类似，同时也要先进行排序。

### 7.12 将集合转换为数组

  方法：toArray()方法。

  ```java
  public class ToArray{
      public static void main(){
          ArrayList al = new ArrayList();
          al.add("aaa");
          al.add("bbb");
          al.add("ccc");
          
          //开始转化
          Object [] ol = al.toArray();
          //类型不能混淆，会抛出ArrayStoreException异常;
          String[] sl = (String[]) al.toArray(new String[0]);
          
      }
  }
  ```

### 7.13 自定义Iterator

  方法：实现Iterator(或者Enumeration)接口。有三个方法：hasNext(),next();reomeve();

  ```java
  public class ArrayIterator implements Iterator{
      protected Object[] data;
      protected int index = 0;
      //构造ArrayIterator 对象 
         public ArrayIterator(Object[] data ){
             setDate(data);
       }
      //设置data数组到已知数组，并重设迭代器
      public void setData(Object[] data){
          this.data = data;
          index = 0；
      }
      //如果不是末尾，就返回真，next()方法返回一个元素；
      public boolean hasNext(){
          return (indext < data.length);
      }
      //返回下一个元素
      public Object next(){
          if(hasNext()){
              return data[index++];
          }
      }
      //删除当前元素
      public void remove(){
           throw new UnsupportedOperationException(
           "This demo doesn't implement the remove method");
      }
      
      //测试
      public static void main(String[] args){
          ArrayIterator it = new ArrayIterator(data);
          while(it.hasNext()){
              System.out.println(it.next());
          }
      }
      
  }
  ```

### 7.14 堆栈

方法： 使用Stack     java.util.Stack   LIFO

堆栈的操作：push()     压栈

​                      peek()   测试是否为空

​                      pop()    弹栈

//新建类基于堆栈

```java
public class	ToyStack{
    protected int MAX_DEPTH = 10;
    protected int depth = 0;
    protected int[] Stack = new int[MAX_DEPTH];    //栈的实际对象
    
    protected void push(int n ){
        stack[depth++] = n;
    }
    protected int pop{
        return stack[--depth];
    }
    protected int peek(){
        return stack[depth -1];
    }
    
}
```

- java.lang.Object是java.util.Stack类的定义依据，只有对象才能被亚茹栈中，所以基本的数据类型是不能完成的。


### 7.16 集合小结

   ![](C:\Users\Administrator\Pictures\悦书PDF截图20180416171344.png)

##  7.16 集合的运行效率

ArrayList的效率并不比数组的运行效率差多少，调用“get”方法要比从数组中提取元素要快的多。Vector的速度慢，但大概是数组速度的三分之二。

若果知道ArrayList的长度，更有利于提高效率。

------

##   第8章  泛型、forEach循环和枚举数据结构（JDK 1.5）

###  8.0 简介

JDK 1.5 中引入了两个新型概念：泛型、自动装箱，自动拆箱，以及两种语言新特性：forEach循环和类型安全的枚举类型。

- Collection和泛型结合使用，泛型能最大限度的提供类型的安全性，并消除从Collection获得的每个对象的请求。

### 8.1使用泛型

```
  List<String>  myList = new ArrayList<String>();

     myList.add("hello");

     myList.add("goodbye");

    //Iterator<String> it = data.iterator();

    for(Iterator<String> it = myList.iterator(); it.hasNext();  ;){

    Stirng s = it.next();

   // it.remove();  

 }



```

 Map<Person,Address>   addressMap = new HashMap<Person,Address>();

在上面的代码中，Person作为key,Address作为value，get()会返回一个Address对象，keySet()方法会返回一个Set<Person>;

###  8.2  使用 “forach”循环

   ```
public static void main(String[] args){
      String[] data = {"Toronto","Stockholm"}；
      for(String s : data){
          System.out.println(s);
      }
}
   ```

### 8.3 使用泛型避免强制类型转换

方法：使用类型<类型名>定义一个类



```java
public class  myStack<T>{
    private int ix =0 ;
    private final int MAX = 10;
    private T[] data = (T[])new Object[MAX];
    public void push(T obj){
         data[ix++] = obj;
    }
    public boolean hasNext(){
        return ix > 0;
    }
    public boolean  hasRoom(){
        return ix < MAX;
    }
     public T pop{
         if(hasNext()){
             return data[--ix];
         }
         throw new ArrayIndexOutOfBoundsException(-1);
     }
}
```

- 使用：

  ```java
  public static void main(String[] args){
      MyStack<String>  ms1 =new MyStack<String>();
      ms1.push("biling");
      ms1.push("account");
      
      while(ms1.hasNext()){
          String name = msl.pop();
          System.out.println(name);
      }
      
     //使用Collection类的老旧方法：类型不安全
      MyStack ms2 = new MyStack();
      ms2.push("biling");
      ms2.push("scottm");
      ms2.push(new Date());
      try{
          String bad  =(String) ms2.pop();
          System.out.println("正確")；
      }catch (ClassCastException ex){
          System.out.println("Error");
      }
     
  }
  ```

  ### 8.5枚举

  类型安全, JDK 1.5;

    enum { BLACK,RED,ORANGE}  color;

    ```java
  public enum MediFancy {
      book{
      public String toString(){ return "Book"},},
          music_id,music_vinyl,movie_vhs,move_dvd;
      public static void main(String[] args){
          MeidaFancy[] data = {book,movie_dvd,music_vinyl };
          for(Mediafancy mf : data){
              System.out.println(mf);
          }
          // 结果是    Book /n     movie_dvd  /n    mucis_vinyl
      } 
      
  }
    ```

  - 列出给定的enum方法获得enum的所有值，

    ```java
    public class EnumList{
         enum State}{ON,OFF,UNKNOW};
         for(State i : State.vaue()){
             System.out.println(i)
         }
    }
    ```

    ​

##  第9章   面向对象的技术    

### 9.1  0标准API中的设计模式

| Factory(工厂)     | 用于产生实例的类                       | getInstance(Calendar类、Format类)，套接字构造方法 |
| ----------------- | -------------------------------------- | ------------------------------------------------- |
| Iterator(迭代器)  | 遍历Collection(集合)，访问每个元素     | Iterator(迭代器)，Enumeration；java.sql.ResultSet |
| Singleton         | 可能只有一个实例存在                   | java.lang.Runtime;  java.awt.Toolkit;             |
| Memento           | 获取对象最后构造的状态                 | 对象串行化                                        |
| Command(命令模式) | 封装需求，可以是需求队列。可取消的操作 |                                                   |
| MVC               | 三层架构                               |

​    ###  9.1  toString()方法的格式化

​      覆盖由 java.lang.Object方法继承而来的toString()方法；

     ```


​    
     @Override
     public String toString(){
          return "";   
      }


​    
​    
     ```

###  9.2覆盖equals方法

​      euqals()方法和hashCode()方法都是以散列表（Hashtable,HashMap,）的方法使用。

- equals方法的规则：

    1.自反性  ：  x.equals(x)  必定为True;

    2.对称性：  x.equals(y)为ture;则y.equals(x)必定为true;

   3.传递性：   x.equals(y)返回值永远一致；

  4. x.equals(null) 一定会返回False,偶尔会抛出NullPointerException异常。

     - 典型的equals()方法的应用

       ```java
       public class EqualsDemo{
           int int1;
           public EqualsDemo(int i,SomeClass){
               int1  = i;
               if(o == null)
                   throw new Exception("Illegal");
               objl = o ;
           }
           public EqualsDemo{
               this(0,new SomeClass());
           }
           //典型的Equals方法
           @Override
           public boolean equals(Object o){
               if( o == null){
                   return  true;
               }
               if(!（o  instaceof EqualsDemo）){
                     return false;
               }
               EqualsDmeo other = (EqualsDemo)o;
               
               if( int1 != other.int1){
                   return false;
               }
               if(!obj1.euqls(other.obj1))
                   return false;
               return  true;
           }
       }
       ```

       ​

            ###  9.3 覆盖hashCode()方法

   hashCode()方法返回一个int值，可以标识不同的对象。

编写规则：

1.  返回这固定，  hashCode(x)返回的值永远相同，除非调用集合的方法；

2. 对称性:  如果 x.equals(y),则 x.hashCode() == y.hashCode();

3. 如果!x.equals(y),并不是说明  x.hashCode != y.hashCode().。在调用equals()方法之前，

   调用hashCode();

###  9.4 clone()方法

覆盖Objcet.clone()方法；

过程：  1. 覆盖对象的clone()方法；

2. 实现空的接口Cloneable接口；

   ``` java
   public class Clone1 implements Cloneable{
         @Override
       public Object clone(){
           try{
           return super.clone();
           }catch （CloneNotSupportedException ex）{
               throw new InternalError(ex.toString());
           }
       }
   }
   ```

   ### 9.5 Finalize() 方法

   方案：  清除对象之前，进行一些处理。采用finalize()方法重写；或者编写自己的终结**方法**

   **finalize()方法**：当GC回收对象所占用的内存时，先调用finalize()方法。

   但是:当  **调用System.exit()时，finalize()方法将会不执行。同样，内存泄漏或者错误的引用也会组织finalize的运行；**

   addShutdownHook().他可以传给你一个尚未开启的Thread子类对象。建立一个章节代码。

   ```java
   public class ShuutdownDemo{
       public static void main(String[] args){
           Object f = new Object(){
               public void finalize(){
                   System.out.println("finalize()");
               }
           }
           Runtime.getRuntime.addShutdownHook（
                public void run(){
                    System.out.println("Runing Shutdown Hook");
                 }
               ）；
                   if （args.length == 1 && args[0] .equals("-f")）{
                        f = null;
                       System.gc();
                   }
                System.out.println(Calling System.exit());
                System.exit(0);
       }
   }
   ```

   ###  9.8  多态 Polymorphism  /抽象方法

      如果父类的 方法是抽象的，那么编译器就会检查其子类是否有相应的方法。

      系统会自动调用方法来计算面积。

#### 多态的优点: 
非常有利于程序的维护。添加子类，无需修改主程序的方法，

代码具有良好的局部性，提高了代码可靠性和可维护性。

###  9.10 Singleton 模式

 返回JVM中只有一个实例存在。

实现：最简单方法，使用一个私有构造器和一个域来保存结果，以及一个getInstance()方法返回这个实例。

```java
public class Singleton{
    private static Singleton = new Singleton();
    private SIngle(){   //私有化构造器阻止了其他任何类进行实例化
        
    }
    //静态实例化方法
    public static Singleton getInstance(){
        return Singleton;
    }
}
```

-- java.util 中的集合含有singletonList() 、singletonMap() 、singletonSet()方法，分别队形不可变得List、Map和 Set，

###  9.11自定义异常 

  方案：定义Exception类（或RuntimeException类)的子类。

 一般不直接定义Thrwable类的子类，义Exception类（或RuntimeException类)的子类。

 ```java
public class   ChessMoveException extends RuntimeException{

     public ChessMoveException(){

                  super();

         }

        public ChessMoveException(String msg){

             super(msg);        

}

}

 ```



## 第十章 输入和输出

  ###   1.0 简介

  JDK1.5 引入了Formatter和Scanner类，因此功能更强大了，Scanner解析字符串或其他输入格式对象，Formatter允许执行许多被转化为字符串或其他输出格式。

  ###  流 （Stream）和 Reader/Writer

Java.io包中流的部分用于读写数据字节的，单字节（8bit）.

- 需要从特定编码的问价中读取数据，兵写入Java Unicode字符对象时，就要进行转换，转换器被嵌入到功能强大的Reader/Writer。所以，如果处理字符而非字节是，我们就要用Reader/Writer去掉 输入输出流。

结构图

​    ![](C:\Users\Administrator\Pictures\悦书PDF截图20180417110250.png)

 ### 10.1 读取数据

策略： 使用System.in的 BufferedInputStream()读取字节。如果要读取文本，通常使用InputStreamReader  和 BufferReader。

int b = System.in.read();   //读取一个字节

//read方法中可能抛出异常

所以

``` java
int  b =0;
try{
     b = System.in.read();
     }catch(Exception e){
         System.out.rpitnln("Catch" +e);
     }
     System.out.println("Read:" + (char)b);
```

``` java
//当程序中能够读取更大的单位是
Scanner sc = Scanner.create(System.in);
int i=sc.nextInt();
```

//在需要转换字符编码时，使用字符转换器Reader类。使用Redaer类的特殊子类BufferedReader类，可以读取整行字符，

```java
BufferedeReader is = new BufferedReader(new InputStreamReader(System.in));
is.readLine();
```

- 使用BufferedReader对象的操作

  ```java
  try{
      BufferedReader is = new BufferedReader(System.in);
      String inputLine;
      while((inputLine = is.readLine() != null)){
          System.out.println(inputLine);
      }
      is.close();
  } catch (IOException e){
      System.out.println("IOException" + e);
  }
  ```

  ### 10.3 Jdk1.5 的Formatter类

  使用Formatter类  java.util.Formatter;

​            System.out.format("%  dfdfdfdadfadfl %",1942,Math.PI);

     ### 10.4 使用扫描文件（计数器案例）

​     策略： 使用StreamTokenizer类和StreamTokenizer对象。或者使用javaCC扫描工具。

- 从文件中提取user@host.domain这样的名字。提出用户名部分和主机地址部分。

  ```java
  //ScanStringTok.java
  protected void process(LineNumber is){
      String s = null;
      try{
          while((s = is.readLine()) ！= null){
              StringTokenizer st =new StringTokenizer(s,"@",true);
              String user = (String)st.nextElement();
              st.nextElement();
              String host = (String)st.nextElement();
              System.out.println("User name:" + user + "；Host:" + host);
              //...省略处理部分
          }catch(NoSuchElelmentException is){
              System.err.println("Line" + is.getLineNumber()+ ":Invalid inpput " +s);
          }catch(IOException ix){
              System.err.println(e);
          }
      }
  }
  ```

  ​

   ### 10.5 Jdk1.5中使用Scanner类

​    使用Scanner的next()方法。Scanner类能死和别Java的八种类型，包括BigInteger和BigDecimal。它能返回字符串形式的输入记号或者使用正则表达式。



- 简单示例

  ```
  Stirng sampledate = "25 Dec 1944";
  Scanner sDate = Scanner.crreate(sampleDate);
  int dom = sDate.nextInt();
  int mon = sDate.next();
  int year =sDate.nextInt();
  ```

  - **Scanner方法**

    | 返回类型   | has方法             | next方法         | 注释                                        |
    | ---------- | ------------------- | ---------------- | ------------------------------------------- |
    | String     | hasNext()           | next()           | 来自Scanner的下一个完整记号                 |
    | String     | hasNext(Pattern)    | next(Pattern)    | 与给定正则匹配下一个字符串                  |
    | Stirng     | hasNext(String)     | next(Stirng)     | 与regex模式（指定字符串）匹配的下一个记号。 |
    | BigDecimal | hasNextBigDecimal() | nextBigDeciaml() | 输入的下一个记号类型为BigDecimal            |
    | BigIngeter | hasNextBigInteger() | nextBigInteger() | 输入下一个记号类型为BIgInteger              |
    | boolean    | hasNextBoolean()    | nextBoolean()    | 输入下一个记号，类型为boolean               |
    | byte       | hasNextByte()       | nextByte()       | 输入下一个记号为byte                        |
    | double     | hasNextDouble（）   | nextDouble()     | 输入下一个记号为double                      |
    | float      | hasNextFloat()      | nextFloat()      | 输入下一个记号类型为flaot                   |
    | int        | hasNextInt()        | nextInt()        | 输入下一个记号为int                         |
    | String     | N/A                 | nextLine()       | 扫描到行末                                  |
    | long       | hasNextLong()       | nextLong()       | 输入的下一记号类型为long                    |
    | short      | hasNextShort()      | nextShort()      | 输入下一个记号的类型为short                 |

    - Scanner类不提供公用构造器，你必须调用静态的create()来获得输入源，输入源可以是一个File对象、一个InputStream、一个String或者Readable；

    - 使用Scanner的方式是居于迭代器（Iterator）模式，利用  scanner.hasNext()来控制迭代。

      ​


              ### 10.6按文件名打开文件

​    策略：FileReader、FileWriter、FileInputStream或FileOutputStream等对象

​      读取文件，要按照次序创建FileReader对象，和BufferReader对象。

      ```java
BufferedReader is = new BufferedReader(new FileReader("myFile.txt"));
BufferedOutputStream byteOut = new BufferoutputStream(new FileOutputStream(bytes.dat));
byteOut.close();
      ```

 ### 10.7复制文件

  策略：  首先一对流，用于复制数据（或者使用Reader与Writer复制文本）；

```java
import java.io.*;
public class FileIO{
    public static void copyFile(String inName,Stirng outName) throws FileNotFoundException,IOException {
        BufferedInputStream is =new BufferedInputStream(new FileInputStream(inName));
        BufferedOutputStream os = new BufferedOutputStream(new FileOutputStream(outName));
        copyFile(is,os,true);
    }
       //复制文件，从Input拷贝到OutputStream
    public static void copyFile(InputStream is,OutStream os,boolean close) throws IOException{
          int b;
        while((b = is.read())!= -1){
            os.write(b);
        }
        is.close();
        if(close)
            os.close();
    }
    //复制文件，从Reader对象到writer对象
    public static void copyFile(Reader is, Writer os, boolean close) throws IOException{
        int b;
        while((b = is.read()) != -1){
            os.write(b);
        }
        is.close();
        if(close)
            os.close();
    }
    //复制文件，从文件拷贝到PrinteWriter对象
    public static void copyFile(String inName,PrinteWriter pw, boolean close) throws FileNotFoundException{
        BufferedReader is =new BufferedReader(new FileReader(inName));
        copyFile(is,pw,close);
        
    }
    //打开文件并读取第一行
    public static String readLine(String inName){
        BufferedReader is = new BufferedReader(new FileReader(inName));
        String line = null;
        line = is.readLine();
        is.close();
        return line;
    }
    protected static final int BLKSIZ = 8912;
    //复制文件的方法，使用自己设定的缓冲区取代BufferedReader.
    public void copyFileBuffered(String inName,String outName)throws FileNotFoundException, IOException{
        InputStream is =new FileInputStream(inName);
        OutputStream os = new FileOutputStream(outName);
        int count = 0;
        byte b[] = new byte[BLKSIZ];
        while((count =is.reand(b)) != -1){
            
            os.write(b,0.,count);
        }
         is.close();
         os.close();
    }
    //读取Reader中的全部内容，赋值给一个字符串
    public static String readToString(Reader is)throws IOException{
        StringBuffer  buffer = new StringBuffer();
        char[] b = new char[BLKSIZ];
        int  n ;
        //读取一个块，如果有字符，就添加到缓冲区总
        while((n = is.read(b)) > 0){
            buffer.addpend(b,0,n);
        }
        return buffer.toString();
    }
    //读取输入流中的内容，返回字符串
    public  static String inputStreamToString(InputStream is) throws IOException{
         return  readerToString(new InputStreamReader(is));
    }
}
```

### 10.8 把文件读入字符串

Reader is =new FileReader(theFileName)；

String input = FileIO.readerToString(is);

### 10.11  读写不同字符集的文本 

策略：在构造InputStreamReader 对象或者PrintWriter对象时，通过转化器实现特定的表面与Unicede之间的转换。、

只有在InputStreamReader类和OutputStreamWirte类的构造方法中，才能制定编码的名称。不指定的系统平台（或用户）的编码。

如果要为读写文件指定一个非缺省的转换器，就必须通过FileInputStream/FileOutputStream的构造方法，而不是FileReader/FileWriter的构造方法。

```java
BufferedReader fromKanji = BufferedReader(
      new inputSteramReader(new FileInputStream("kanji.txt"),"EUC_JP"));
PrinterWirte toSwedish = new PrintWriter(
    new OutputStreamWriter(new FileOutputStream("sverige.txt"));
    
```

###  10.15 读写二进制数据

策略： 使用DataInputStream或者DataOutputStream类。

```java
public class WriteBinary{
    public static void main(String[] args){
        int i = 42;
        double d =Math.PI;
        String FILENAME = "binary.dat";
        DataOutputStream os = new DataOutputStream(
          new FileOutputStream(FileName));
        os.writeInt(i);
        os.writeDouble(d);
        os.close();
        System.out.println("Wrote" + i + "," + d + "to file" + FIleNAME);
    }
}
```

将对象写入到文件，要用到ObjectDataStream类。

###  10.16定位(Seeking)

  策略：  使用RandomAccessFile类。

java.io.RandomAccessFile类支持随机访问文件。，方法有：

- void seek(long where),将文件指针移动到指定位置；
- int skipBytes(int howMany),将文件向后移动几个字节；
- long getFilePointer(),返回文件指针的当前位置。
- 该类实现了DataInput和DataOutput接口，多有DataStream类的应用也适合在这里使用。

```java
import java.io.*;
public class ReqadRandom{
    final static String FILENAME = "random.dat";
    protected String fileName;
    protected RandomAccessFile seeker;
    public static void main(String[] args){
        
    }
    //构造方法：构造RandomAccess对象
    public ReadRandom(String fname) throws IOException{
       fileName = fname;
        seeker = new RandomAccessFile(fname,"r");
    }
    //读取偏移量
    public int readOffset() throws IOException{
        seeker.seek(0);
        return seeker.readInt();
    }
    public String readMessage() throws IOException{
        seeker.seek(0);
        return seeker.readInt();
    }
    //根据给定的便宜来那个读取数据
    public String readMessage() throws IOException{
        seeker.seek(readOffset());
        return seeker.readLine();
    }
    
}
```



###  10.18  存储和还原串行化对象

策略：使用对象流类：  ObjectInputStream类ObjectOutputStream类，或使用XMLDecoder类和XMLEncoder类，或者使用Java 数据对象。

```java
import java.io.*;
public class TestObjectStream{
	public static void main(String[] args)throws Exception{
		/*
		ObjectInputStream ois = new ObjectInputStream(new FileInputStream("电冰箱.data"));
		Object obj = ois.readObject();
		ois.close();
		Teacher tea = (Teacher)obj;
		System.out.println(tea);
		*/

		Teacher tea = new Teacher("Jay");
		ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("电冰箱.data"));
		oos.writeObject(tea);
		oos.close();

	}
}
class Computer{
	String logo;
	public Computer(String logo){
		this.logo = logo;
	}
}
class Teacher implements Serializable{
	String name;
	//transient => 短暂的 转瞬即逝的 不参与持久化的
	transient Computer pc;
	public Teacher(String name){
		this.name = name;
		pc = new Computer("Lenovo");//联想 又名 来弄我
	}
	@Override
	public String toString(){
		return name;
	}
}
```

### 10.20读写JAR或Zip文档

策略： java.uilt.zip.ZipFIle 类不是IO类，但是能读写JAR或ZIP格式的文件。

ZipFIle类的构造方法能创造一系列ZipEntry类对象表示其中一个条目。

```
ZipFile zip = new ZipFile(fileName);
Enumeration = zipy.entires();
while((all.hasMoreElements())){
    ZipEntry entry = (ZipEntry)all.nextElement();
    if(entyr.isDirectory())
           ....
     else
          ...
}
```

## 第11章 目录和文件的操作

   ### 11.0    java.io.File对象；

### 11.1 获取文件信息：

​     你必须通过构造File对象，才能使用这些方法

   

| 返回类型 | 方法名             | 解释                                     |
| -------- | ------------------ | ---------------------------------------- |
| boolean  | exists()           | 文件存在返回true                         |
| String   | getCanonicalPath() | 获取全名                                 |
| String   | getName()          | 文件名                                   |
| String   | getParent()        | 父目录                                   |
| boolean  | canRead()          | 可读文件，返回True                       |
| boolean  | canWrite           | 如果文件可写，返回True                   |
| long     | lastModified()     | 文件更新时间                             |
| long     | length()           | 文件大小                                 |
| boolean  | isFile()           | 是文件，返回True                         |
| boolean  | idDirectory()      | 如果是目录，返回True（有可能两者都不是） |

### 11.2创建文件

方案： java.io.File对象的createNewFile()方法。（可以使用FileOutputStream对象或FileWriter对象很容易创建新文件，不过要记得关闭文件。）

​     new File("file").createNewFile();

### 11.2 修改文件名

   方案： java.io.File 的renameTo()方法。

- 采用renameTo()方法更改文件名，其参数不是就文件名，而是另一个新文件名引用的File对象。

  ```java
  //构造File对象，不是在磁盘上创建文件
  File f =new File("Rename.java");
  
  //将这个文件重命名为："junk.dat",重命名创建一个目标File对象。
  
  f.renameto(new File(junk.dat));
  ```

  

### 11.4 删除文件 

方案： java.io.File对象的delete()方法。

```
File target = new Filew("Delete.java");
target.delete();
```

### 11.5 创建临时文件

 方案：java.io.File对象的createTempFile()方法或deleteOnExit()方法。

```java
//1. 指派已有文件为临时文件
File bkup = new File("Rename.java~");  //“~”表示备份文件
bkup.deleteOnExit();
//2.建立一个新的临时文件

//foo.tmp是存放于缺省目录的临时文件
File tmp = File.createTempFile("foo","tmp");
tmp.deleteOnExit();

```



### 11.5 更改文件属性

方案： setReadOnly() 方法  或setLastModified()方法

### **11.7列出目录内容**

方案： java.io.File的list（）方法或listFiles()方法。

```
//列出当前目录的文件清单
String[] list = new File(".").list();
File[] list = new File(".").listFiles();
```

### 11.8 获取根目录

  使用静态方法：   File.listRoots();

### 11.9 创建新目录

方案：   java.io.File 类的mkdir() 方法或mkdirs()方法；

​       创建一个或者多个目录。

## 第十二章  JDBC

###  12.0  简介

JDBC 由java.sql和javax.sql两个包中的类组成。

 JDBC优点：可移植性，网络和类型检查功能较好。

​    JDBC分为两部分:的 Java中提供的可移植性的JDBC API，和DBMS厂商提供的数据库专用 的驱动程序。

JDO   Java Data Object,简称JDO，一个访问器（accessor），它要比直接调用JDBC更好用。一种自动写访问器的方式，。

### 12.4 JDBC的安装和连接

方案：  Class.forName()  和DriverManger.getConnection();

- 使用JDBC的步骤：

  ```
  1.装载正确的Driver类，同时会向DriverManger注册；
  2.使用DriverManger.getConncetion()，获取一个Connection对象；
        Connection conn = DriverManger.getConnection(url,name,pass);
   3. 使用Connection对象的createStatement();
       Statement stmt = con.createStatement();
   4.用Statment对象的方法，（executXXXXX）;
     Result rs = stmt.executeQuery("select * from mytable");
   5. 遍历ResultSet
        while(rs.nerxt()) {
             rs.getInt("no");
        }
   6.关闭：  ResultSet   Statement    Connection;
   
  ```

  

- 加载驱动

  ```
  main(){
  //装载  JDBC-ODBC桥驱动程序
       Class c = Class.forName("sun.jdbc.odbc.JdbcOdbcDriver");
   //装载Oracle驱动程序
       Class d = Class.forName("oracle.jdbc.driver.OracleDriver");
  }
  ```

  ### 12.5 与JDBC数据库连接

  方案：使用DriverManger.getConnection();

~~~Java
main(){
    String dbURL = "jdbc:odbc:Companies";
    try{
        Class.forName("sun.jdbc.odbc.JdbcOdbcDriver");
        
        DriverManager.setLogWriter(new PrintWriter(System.err));
        System.out.println("Getting Connection");
        
        Connection conn = DriverManger.getConnection(dbURL,"ian","");
        //如果有sql警告，输出警告
        
        SQLWarning warn = conn.getWarnings();
        while(warn != null){
            System.out.println(warn.getSQLState() +
             warn.getMessage()  +  warn.getErrorCode());
             
             warn = warn.getNextWarning();
        }
        conn.close();
    }
    catch(Exception ex){
        System.out.println("Error");
    }
    
}
~~~

### 20.6 发送一个JDBC查询并获得结果

```
 Class.formName(JDConstants.getProperty("jabadot.userdb.driver"));
 Connection conn = DriverManger.getConnection(JDConstants.getProperty("jdbadot.dburl"));
 Statement stmt = conn.createStatement();
 ResultSet rs = stmt.executeQuer("SELECT * FROM userdb where name = 'ian'");
 while(rs.next()){
     String name = rs.getString(1);
     ...
 }
 rs.close();
 stmt.close();
 conn.close();
 System.exit(0);
```

### 20.7  PrepareStateme

```
   PreparedStatement  ps = conn.prepareStatement("select *  from payroll where personnelNo =? ");

   ps.setInt(1,12345);

   rs  = ps.executeQuery();

```

- 使用PrepaedStatement插入Date要将   java.util.Date转换为java.sql.Date;

  XXX.setDate(1,new java.sql.Date(date.getTime()));

### 20.8 在JDBC中使用存储过程

方案：  CallableStatment;

  CallableStatment cs = conn.prepareCall("{call ListDefunctUsers}");

 Result rs = cs.executeQuery();

  ###  20.9  用ResultSet改变数据

ResultSet      update()方法，最后使用updateRow()；方法；

```java
try{
    conn = DriverManger.getConnection(url,user,pass);
    stmt = con.createStatment(ResultSet.TYPE_SCROLL_SENSITIVE,ResultSet.CONCUR_UPDATABLE);
    rs = stmt.executeQuery("select * from user where nike = "");
                           
    rs.first();
     rs.updateStringf("password","unguessable");
     rs.updateRow();
                           
   rs.close();
   stmt.close();
  conn.close();
}Catch(Exception ex){
 e.printStackTrace();
}
```

###  20.9 用ResultSet改变数据

简介： RowSet 接口 ResultSet的一个子接口，因为RowSet继承了Result，RowSet更趋于完备，你可以使用先前讨论的任何ResultSet处理方法。调用setCommand()和excute（）分别制定和查询

```
Class c = Class.forName("com.sun.rowset.CachedRowSetImpl");
rs = (CachedRowSet) c.newInstance();

rs.setUrl("jdbc:postgresql:tmclub");
rs.setUserName("name");
rs.setPassword("secret");

rs.setCommand("select * from members where name like ?");
rs.setString(1,"I%");

rs.excute();
while(rs.next()){
    if(rs.getInt("id") ==42 ){
        rs.setString("firstname","Marvin");
        rs.updateRow();
        
        rs.acceptChanges();
    }
}

rs.close();
```

##  十三章   XML

### 20.1 简介

XML (eXtensible Markup Language,可扩展标记语言) 是一种具备一定移植性和可读性、可用于程序之间交换文字和数据的格式。XML起源于父标准SGML。

理解XML的一种方式是：他是对HTML的一种整理和加强，并且添加了定义自定义标签的功能。与HTML相比，它带有标识信息性内容而不仅仅是格式的标签。

头文件：  <?xml verson="1.0" encoding="iso-9959-1" ？>

## 第十四章 包和包装机制

###  23.0 简介

   

| 名称        | 功能                 |
| ----------- | -------------------- |
| java.applet | 供浏览器使用的applet |
| java.awt    | 一种GUI              |
| java.lang   | 内置类（字符串）     |
| java.net    | 联网（套接字）       |
| java.io     | 读写                 |
| java.util   | 工具（集合和日期）   |

### 23.1 创建包

```java
package com.etoak.dot;
```

```java
javac -d . *.java
//此语句创建一个相对于当前目录 的路径，并将类文件放入该子类目录。

```

### 23.2 用JavaDoc类写文档

  文档注释以  “/**”开头，必须恰好出现在说明类、方法、字段定义之间.

  

|   关键字    |         用法         |
| :---------: | :------------------: |
|   @author   |         作者         |
|  @version   |       版本表示       |
|   @param    |   参数（用于方法）   |
|   @since    |  最早出现的JDK版本   |
|   @return   |        返回值        |
|   @throws   |   异常类及抛出条件   |
| @deprecated | 引起不推荐使用的警告 |
|    @see     |       交叉参考       |

```java
import  java.applet.*;
import java.awt.*;
import java.awt.event;
    /**
    *    JavadocDemo----显示一个Javadoc注释的Applet
    *    <p>只是一个带注释的版本
    *    @author li
    *    @version   2018/5/10
    *    @see java.applet.Applet;
    *    @see javax.swing.JApplet;
    */

public class JavaDocDemo extends Applet {
            /**
            *    init()是一个Applet方法，由浏览器进行初始化
            *    
            */
    
    public void init(){
         Button b;
        b = new Button("hello");
        add(b);
    }
    
}
```

```java
 $ javadoc -author -version JavadocDemo.java
 //生成的文件和类名是同名，扩展式.html;
```

### 23.4 用jar存档

   用 jar 存档器，是Java生成存档的标准工具

```java
java -verbose HelloWorld
```

### 23.8 将 Servlet 压缩为一个WAR文件

方案：使用jar生成WAR（Web Archive）文件

    ``` Java
jar cvf MyWebApp.war .
// ‘.’表示当前目录
    ```

### 第十五章  Java线程

  java.util.concurrent包 包含以下部分：

- Excutors、 线程池  、以及Future；
- Queeue 和BlockingQueue；
- Lock和Condition，通过JVM的支持来控制锁定和开锁。
- 同步装置， Semphore 和Barrier；
- 原子变量
  - Executor的通用形式是 “线程池”。

### 线程

```
  public class ThreadDemo1 extends Thread{
      @Override
      public void run(){
             while(count -->){
                 ...
             }
      }
  }
```

### 线程的生命周期

停止线程不要使用Thread.stop()  ，在run()方法中主循环开始加一个bool；

### synchronized关键字同步线程





- 性能计时

  System.currentTimeMilis();

## Java 与其他语言结合

### 运行一个程序

方案：  java.lang.Runtime类中的exec()方法，或者在JDK1.5中使用ProcessBuilder的start()方法。

```Java
Proceess p = RUntime.getRuntime().exec("程序名");
```

