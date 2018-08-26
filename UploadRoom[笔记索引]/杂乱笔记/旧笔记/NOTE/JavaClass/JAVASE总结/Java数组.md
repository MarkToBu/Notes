# JavaSE

## 1.数组

  ### 1.0  数组基础

>    **数组: 一组[类型相同]而且[存储空间连续]的数据**

​    如何定义数组:
	  1st way : int[] data1 = new int[3];
	  2nd way : int[] data2 = new int[]{11,22,33};
	  3rd way : int[] data3 = {11,22,33};
   如何访问数组的某个元素:
		使用下标索引值
		System.out.println(data[0]);
		System.out.println(data[1]);
		System.out.println(data[2]);
 如何得到数组的元素个数:
		System.out.println(data.length);//属性 没有()
  如何遍历数组:
		1st way : for循环 + []
		for(int i = 0;i<data.length;i++){
			System.out.println(data[i]);
		}
		
   2nd way : foreach / forin   since JDK5.0
		for(int x : data){
			System.out.println(x);
		}

###  1.1  数组复制

​        数组复制:
	1:直接赋值				不行
		*: 因为根本没有创建新的数组 只是老数组多了个名字而已
	2:纯手动复制				太笨
	3:克隆对象的方法     clone()		不行
	================= 垫场结束 ==================

​      *: System.arraycopy(1,2,3,4,5);
		1:要复制的原数组
		2:原数组的起始位置
		3:要复制到的目标数组
		4:目标数组的起始位置
		5:要复制多少个元素
         *: 这个方法的第一个参数和第三个参数可以是同一个数组
		从而实现数组元素的前后移动

### 1.2  数组排序:

​	1st way : 手动冒泡
		循环嵌套 + 变量交换
		循环嵌套 请把握好两层循环的循环条件
		
​        for(int x = 0;x<data.length-1;x++){
    		for(int y = 0;y<data.length-1-x;y++){
    			if(data[y] > data[y+1]){
    				变量交换;
    			  }
    		  }
    	   }
     2nd way : 自动排序
    	   java.util.Arrays.sort(要排序的数组);

### 代码部分

   ####  遍历数组

```java
public static void main(String args[]){
    char[] data = new char[]{'e','t','o','a','k'};
		//请打印元素 o 和 k
		System.out.println(data[2]);
		System.out.println(data[4]);
		//请使用两种不同的方式 遍历这个数组
		for(int i = 0;i<data.length;i++){
			System.out.println(data[i]);
		}
		for(char x : data){
			System.out.println(x);
		}
}
```

#### 查找最值

```java
	int[] data = new int[]{22,87,66,55,91,37};
		//请找出数组当中最大的和最小的元素...
		int max = 0,min = 99;
		for(int x : data){
			if(x > max){//破纪录了
				max = x;
			}
			if(x < min){//破纪录了
				min = x;
			}
		}
		System.out.println(max);
		System.out.println(min);
```



#### 数组复制来删除

```java
public class ExecArrayCopy{
	public static void main(String[] args){
		int[] data = new int[]{53,87,66,39,14,29};
		//请通过数组复制的方法 删除39
		System.arraycopy(data,3,data,2,1);

		for(int x : data)
			System.out.println(x);
	  
		String []  abc={"aa","bb","aa","cc","bb","ccccc"};
		for(int i=0;i<abc.length-1;i++){
			for(int y=i+1;y<abc.length;y++){
				 if(abc[i].equals(abc[y])){
					 abc[i]="";
				 }
			}
			
		}
		for(String x : abc){
			System.out.println(x);
		}
	}
}
```



#### 冒泡排序

```java
public class ExecArraySort1{
	public static void main(String[] args){
		int[] data = new int[]{53,87,66,39,14};
		//要求对数组进行冒泡排序 请降序排列
		for(int x = 0;x<data.length-1;x++){
			for(int y = 0;y<data.length-1-x;y++){
				if(data[y] > data[y+1]){
					data[y] = data[y] ^ data[y+1];
					data[y+1] = data[y] ^ data[y+1];
					data[y] = data[y] ^ data[y+1];
				}
			}
		}
		for(int x : data){
			System.out.println(x);
		}
	}
}

////请按照个位 + 十位 + 百位 三个数字的和升序排列

public class ExecArraySort2{
	public static void main(String[] args){
		int[] data = {313,384,666,234,127,516};
		//请按照个位 + 十位 + 百位 三个数字的和升序排列
		for(int x = 0;x<data.length-1;x++){
			for(int y = 0;y<data.length-1-x;y++){
				int s1 = (data[y] / 100) + (data[y] / 10 % 10) + (data[y] % 10);
				int s2 = (data[y+1] / 100) + (data[y+1] / 10 % 10) + (data[y+1] % 10);
				if(s1 > s2){
					data[y] = data[y] + data[y+1];
					data[y+1] = data[y] - data[y+1];
					data[y] = data[y] - data[y+1];
				}
			}
		}
		for(int x : data)
			System.out.println(x);
	}
}

//已知 数组当中的元素代表一组同学的体重
		//而且 单位是不同的 100以下的都是公斤 100以上的都是斤
		//请按照提供由高到低降序排列
public class ExecArraySort3{
	public static void main(String[] args){
		int[] data = {65,128,133,68,240,55};
		
		for(int x = 0;x<data.length-1;x++){
			for(int y = 0;y<data.length-1-x;y++){
				int num1 = data[y] < 100 ? data[y]<<1 : data[y];
				int num2 = data[y+1] < 100 ? data[y+1]<<1 : data[y+1];
				if(num1 < num2){
					int temp = data[y];
					data[y] = data[y+1];
					data[y+1] = temp;
				}
			}
		}
		for(int x : data)
			System.out.println(x);
	}
}

//笔试题原题
public class ExecArraySort4{
	public static void main(String[] args){
		char[] data = {'a','C','D','c','d','A','B','b'};
		//要求按照 A a B b C c D d进行排序~
		for(int x = 0;x<data.length-1;x++){
			for(int y = 0;y<data.length-1-x;y++){
				int num1 = data[y]<<1;//A 130  B 132
				int num2 = data[y+1]<<1;
				if(num1 > 193){
					num1 -= 63;
				}
				if(num2 > 193){
					num2 -= 63;
				}
				if(num1 > num2){
					char temp = data[y];
					data[y] = data[y+1];
					data[y+1] = temp;
				}
			}
		}
		for(char x : data){
			System.out.println(x);
		}
	}
}
```

#### 数组复制

```java
public class TestArrayCopy3{
	public static void main(String[] args){
		int[] data1 = new int[]{11,22,33};
		int[] data2 = data1.clone();//clone 克隆
	}
}

public class TestArrayCopyFinal{
	public static void main(String[] args){
		int[] data1 = new int[]{11,22,33};
		int[] data2 = new int[data1.length<<1];
		System.arraycopy(data1,0,data2,0,data1.length);
	}
}

public class TestArrayCopyFinalPlus{
	public static void main(String[] args){
		int[] data = new int[]{11,22,33,44,55};
		/*
		//不喜欢22 想要删掉它
		System.arraycopy(data,2,data,1,3);
		*/
		System.arraycopy(data,0,data,1,4);
		for(int x : data){
			System.out.println(x);
		}

	}
}

public class TestArraySort{
	public static void main(String[] args){
		int[] data = new int[]{53,87,66,39,14};
		//总共需要选多少个最大的数
		for(int i = 0;i<data.length-1;i++){
			//一次从头到尾的比较 = 选一个大数的过程
			for(int j = 0;j<data.length-1-i;j++){
				if(data[j] > data[j+1]){//前大后小
					int temp = data[j];
					data[j] = data[j+1];
					data[j+1] = temp;
				}
			}
		}
		for(int x : data){
			System.out.println(x);
		}

	}
}

 import java.util.*;
public class TestArraySortFinal{
	public static void main(String[] args){
		int[] data = {14,38,66,55,27,52};
		Arrays.sort(data);
		for(int x : data){
			System.out.println(x);
		}
	}
}
```

