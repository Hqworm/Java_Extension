## javase reflect
目录：  
[1. 反射机制的概念]  
[2.反射基本状况]  
	1.Class:		用来描述类本身的相关操作   
	2.Field:		描述类中的属性  
	3.Method:		用来描述类的方法  
	4.Constructor:	描述类的构造方法  
	5.Package:		用来描述类的包  
	6.Annotation:	用来描述类中的注解@Override  
### 1.反射机制的概念
- JAVA反射机制是在运行状态中，对于任意一个实体类，都能够知道这个类的所有属性和方法；
- 对于任意一个对象，都能够调用它的任意方法和属性；
- 这种动态获取信息以及动态调用对象方法的功能称为java语言的反射机制

### 2.反射基本状况
```
	//类用来描述对象  
	//反射机制用来描述所有的类的，每一个类都有属性，方法。。。。  
	
	/*
	 * Class:		用来描述类本身
	 * Field:		描述类中的属性
	 * Method:		用来描述类的方法
	 * Constructor:	描述类的构造方法
	 * Package:		用来描述类的包
	 * Annotation:	用来描述类中的注解@Override
	 * ..总之可以描述类的所有..
	 */
	
```
### 准备test相关的类
1.Animal.java
``` java
package Demo.reflect;

public class Animal {
	public String type;
}

```

2.person.java
``` java
package Demo.reflect;

public class person extends Animal {
	public String name;
	public int age;
	private int id;// int id;
}
```
3.man.java
```
package Demo.reflect;

public class man  {
	
}
```
#### 1.Class:用来描述类本身的相关操作  
##### 1.获取一个Class：
1. Class.forName("全类名");  全类名--包名+类名
``` 
	//1. Class.forName("全类名");  全类名--包名+类名
	clazz1=Class.forName("Demo.reflect.person");
```

2.类名.class(类必须存在,否则异常)
```
	
	//2.类名.class(类必须存在)
	Class clazz2=person.class;
```

3.可以通过对象的引用来获取   对象.getClass()
```
	//3.可以通过对象的引用来获取   对象.getClass();
	person persion1=new person();
	Class clazz3=persion1.getClass();//Object中的方法（object中有9个方法）
```

##### 2.getModifiers()获取 权限类修饰符，权限符，特征符
```
	/*
	 * (1).getModifiers()获取 权限类修饰符，权限符，特征符
	 * (java中将每一个修饰符用一个整数来表示 从0开始 0 1 2 4 8 16 32。。。。)
	 * 0--默认不写     1--public   2--private  4--protected....如果有多个修饰则相加返回
	 */
	int modifiers=clazz1.getModifiers();
	System.out.println(modifiers);
```

##### 3.获取类的名字
```
	//(2)获取类的名字
	String name=clazz3.getName();//全类名
	System.out.println("全类名.getName():"+name);
	String simpleName=clazz3.getSimpleName();//类名
	System.out.println("类名.getSimpleName():"+simpleName);
```

##### 4.类所在的包
```
	//(3)类所在的包
	Package package1=clazz3.getPackage();
	System.out.println(package1.getName());
```	

##### 5.获取父类,以及实现的接口
```
	//(4)获取父类
	man man1=new man();
	Class manClazz1=man1.getClass();
	Class superClass=manClazz1.getSuperclass();//获得manClazz1的父类
	System.out.println(superClass.getSimpleName());	
	
	System.out.println("--------父类--------");
	
	
	//test 
	ArrayList<String> list=new ArrayList<String>();
	ArrayList list2=new ArrayList<>();
	//获取集合对应的Class
	Class listclazz= ArrayList.class;
	Class slistclazz=listclazz.getSuperclass();//只能获取继承但是没有办法获取实现
	while(slistclazz!=null) {//可以找到当前集合的所有父类
		System.out.println(slistclazz.getName());
		slistclazz=slistclazz.getSuperclass();
	}
	
	System.out.println("-------获取所有父类接口-----");
	Class [] listInterfaces= listclazz.getInterfaces();
	for(Class c:listInterfaces) {
		System.out.println(c.getName());
	}
```

##### 6.可以通过Class来创建对象.newInstance()
```
	//(5)可以通过Class来创建对象
	Class man2clazz=man.class;
	//相当于调用了man类中的无参构造方法创建对象,但是重载了构造函数，就会报错。所以类中最好写一个无参构造。
		man m=(man) man2clazz.newInstance();
		System.out.println(m);
```

#### 2.Field(描述类中的属性), Field类中的常用方法
##### 1.通过Personclazz获取类中的属性 .getField(string)
```
	//1.通过反射获取class
		Class Personclazz=Class.forName("Demo.reflect.person");
		//2.通过Personclazz获取类中的属性 .getField(string)
		Field nameField=Personclazz.getField("name");
```

##### 2.获取属性的类型，修饰符
```
		// 3.获取属性的类型，修饰符
		int modifiers=nameField.getModifiers();//获取属性修饰符
		System.out.println(modifiers);
		
		Class Ftype=nameField.getType();//获取属性的类型
		System.out.println(Ftype.getName());
```

##### 3.getName()获取属性的名字  （局限：知道属性的名字，且属性要是公有的）
```
		//4.获取属性的名字  （局限：知道属性的名字，且属性要是公有的）
		String fName=nameField.getName();
		System.out.println(fName);
```

##### 4.通过反射来操作属性
```
		
			//5.获取属性的作用主要向里存储值，或取值
			//无反射时：new 对象  创建对象空间，当前空间里有自己的一套自己的方法，属性   “对象.属性”
			//有反射时，通过类本身，不创建对象可以实现存储和取出   属性=类.getField()   属性.赋值(哪个对象，值)  属性自己去赋值，不过要确定是哪个对象的属性（--可以通过new出来一个对象或者通过反射newInstance()）
			person p=(person)Personclazz.newInstance();//person p=new person();
			//赋值  
			nameField.set(p, "一只倔强而老实的虫");
			System.out.println(p.name);
			//取值 值=属性.取值（哪个对象）
			String name=(String) nameField.get(p);
			System.out.println(name);
			
			System.out.println("-------属性名不知属性名的情况下-----");
			Field [] fields=Personclazz.getFields();//在不清楚属性名的情况下如何获得想要的属性，获得所有的属性(包括继承的)然后在进行判断，获得想要的属性
			//只能获取公有的  但是包含继承来的
			for(Field field:fields) {
				System.out.println(field.getName());
			}
			System.out.println(fields.length);
			
			
			System.out.println("----获取私有属性-----");
			// 可以获得本类私有的属性，但是不能获得父类的，继承的 java.lang.NoSuchFieldException
			Field field=Personclazz.getDeclaredField("id");//id为私有的person里定义得
			System.out.println(field.getName());//获取私有属性的id
			
			//field.set(p, 113);//java.lang.IllegalAccessException非法进入异常非法的  不能操作
			field.setAccessible(true);//setAccessible：可进入的; 可使用的;
			field.set(p, 123);
			System.out.println(field.get(p));
			
			System.out.println();
			System.out.println();
			System.out.println("----测试");
			
```

##### 5.reflect的强大（可以更改String的值）---不提倡
```
package Demo.reflect;

import java.lang.reflect.Field;

public class reflectChangeStringValue {
	
	public static void main(String[] args) throws Exception, SecurityException {
		/**
		 * String 底层了解基础  java.lang包
		 *    public final class String
		 * 	  private final char value[];
		 * 但是它的长度不能修改，但private final char value[]，可以通过反射获取value的值（地址），，在通过地址寻找它的值，去修改值来达到修改string
		 */
		String str="abc";
		System.out.println(str);
		//反射技术可以获取私有属性，并且去操作它
		//1.获取string 获得它的那个class 
		Class clazzStr =str.getClass();
		//2.通过clazz获取类中到的value属性
		Field declaredField = clazzStr.getDeclaredField("value");//declaredField:表示string 下 value的属性
		// 3.设置私有属性可以被操作--才能被修改
		declaredField.setAccessible(true);
		//4.private final char value[]   str--char[] value={'a','b','c'}  获取 declaredField属性的值---数组地址，通过地址，找到地址对应的值，把值改掉，但是并不改变地址（属性的值）
		char[] temp = (char[]) declaredField.get(str);
		//5.通过temp的地址引用，找到地址中String对象中数组，修改数组中的每一个元素
		System.out.println(temp.length);
		temp[0]='琼';
		temp[1]='大';
		temp[2]='泡';
		System.out.println("通过反射更改之后的："+str);
		
		
		
		
		
	}
}
```







<!-- 这里是引用要用到的链接  -->
[Class]: reflect.md#代码块  
[1. 反射机制的概念]: reflect.md#1.反射机制的概念
[2.反射基本状况]: reflect.md#2.反射基本状况
