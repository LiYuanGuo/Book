###chapter 10
####内部类
####
+ 10.1创建内部类
	* 在类定义中放入内部类定义
	* 在外部类的一般方法（非静态方法）之外任何地方创建某个内部类的对象，需要具体的指明这个内部类对象的类型：格式为外部类名.内部类名
	* 所有内部类自动拥有对其外围类所有成员的访问权
+ 10.2 使用他.this和.new
	* 内部类中可以通过外围类.this产生外围类的对象引用
	
			public class Test1 {
	
				class Test2{
					public Test1 getTest1(){
						return Test1.this;
					}
				}

			}

	* 要想创建内部类对象，你必须使用外围类对象来创建内部类对象，因为内部类对象会暗暗链接到创建他的外部类对象上，即在创建外围类对象之前不能创建内部类对象.代码如下:
	
			Test1 t1=new Test1();
			Test1.Test2 t2=t1.new Test2();
			//或者
			Test1.Test2 t3=new Test1().new Test2();

	* 除了内部类是静态内部类，不需要先创建外部类对象，然后再用外部类对象来创建内部类对象，代码如下:
			
			//直接用new 创建，后面直接用外部类名.内部类构造器
			Test1.Test4 t4=new Test1.Test4();
+ 10.3在方法和其他作用域内的内部类
	* 在方法在作用域内的内部类简称局部内部类。代码如下：
	
			public class Parcel5 {
		
				public Inter1 getInter(String s){
					//局部内部类 内部类中的private属性属于方法，因为内部类属于方法，所以private属性在方法内可见
					class Inter2 implements Inter1{
						private String label;
						private Inter2(String s){
							label=s;
						}
					}
					return new Inter2(s);
				}
				
				public static void main(String[] args) {
					Parcel5 i1=new Parcel5();
					i1.getInter("nih");
				}
		
		    }
	* 在任意作用域内嵌入内部类。代码如下：
	
			public class Parcel6 {
				private void internalTrack(boolean b){
					if(b){
						class TrackSlip{
							private String id;
							//内部类中的private属性和方法属于外部if语句块，因为内部类属于外部if语句块，所以private属性和方法在if语句块内可访问
							private TrackSlip(String s){
								id=s;
							}
							String getSlip(){
								return id;
							}
						}
						TrackSlip t=new TrackSlip("test");
						System.out.println(t.getSlip());
					}
					//超出作用范围
					//TrackSlip t=new TrackSlip("test");
				}
				
				public void track(){
					internalTrack(true);
				}
			
				public static void main(String[] args) {
					Parcel6 p=new Parcel6();
					p.track();
				}
			}
+ 10.4 匿名内部类
	* 匿名内部类：创建一个继承自基类（接口）的匿名类的对象，这样通过new创建的匿名类对象自动转型为基类。代码如下：
			
			public class Test5 {
				//匿名内部类
				public Sdf getSdf(){
					//创建一个继承自基类（接口）的匿名类的对象
					//这样通过new创建的匿名类对象自动转型为基类
					return new Sdf(){
			
						@Override
						public void test() {
							
						}
						
					};
				}
				
				public static void main(String[] args) {
					Test5 t5=new Test5();
					Sdf sdf=t5.getSdf();
				}
		
			}
	* 如果在匿名内部类定义中想访问一个外部参数，那么编译器会要求这个参数是final的，否则编译器会提示。代码如下：
		
			public Sdf getSdf(final String s){
					//创建一个继承自基类（接口）的匿名类的对象
					//这样通过new创建的匿名类对象自动转型为基类
					return new Sdf(){
						//匿名内部类定义中想访问一个外部参数，须为final的
						private String name=s;						

						@Override
						public void test() {
							
						}
						
					};
			}
	* 匿名类中没有构造器，也不可能有构造器，因为它根本没有类名字
	* 使用代码块在匿名类中初始化成员变量，充当构造器的作用。代码如下：

			public class Test6 {
	
				public Sdf getSdf(final String s){
					return new Sdf(){
						private String name;
						//匿名类中没有构造器
						//初始化代码块，在匿名类中充当构造器作用
						{
							name=s;
							if("afda".equals(name)){
								System.out.println(name);
							}
							
						}
						
						@Override
						public void test() {
							
						}
						
					};
				}
				
				
				public static void main(String[] args) {
					Test6 t6=new Test6();
					Sdf sdf=t6.getSdf("afda");
				}
			}
+ 10.5 闭包与回调
	* 闭包是一个可调用的对象，它记录了一些信息，这些信息来自于创建它的作用域
	* 内部类就是一个面向外部类的闭包，它包含创建它的外部对象的信息，还拥有一个指向外围对象的引用
+ 10.6 内部不可以被覆盖
	* 一个类继承外部类，在外部类子类中重新定义内部类，这时父类和子类中两个同名的内部类其实是两个完全独立的在各自命名空间的实体，没什么意义
	* 当然可以在外部类子类中内部类明确继承父外部类中内部类
+  10.7 内部类的继承（一个类直接继承外部类的内部类）
	* 先要创建父类外部类对象引用，然后传给内部类子类构造方法中，要让内部类的子类和外部类引用建立连接。代码如下：
	
			public class WithInner {
				class Inner{
					
				}
			}

			public class InnerSon extends WithInner.Inner{
				InnerSon(WithInner withInner){
					//在内部类子类构造器中必须要调用 父内部类对应外部类对象的super方法
					//这样内部类子类对象才能连接到父内部类的外部类对象
					withInner.super();
					System.out.println("先要初始化隐式指向外部类对象引用");
				}
				
				public static void main(String[] args) {
					WithInner withInner=new WithInner();
					InnerSon snnerSon=new InnerSon(withInner);
				}
			}
+ 10.8 局部内部类
	* 在方法中使用局部内部类有名字，将会有有名字的构造方法，能拿来重复创建对象
	* 在方法中使用匿名内部类，没有有名字的构造器，没有类名不能拿来重复创建对象
	* 两个都对方法外不可见
	* 局部内部类class前没有访问修饰符，也不能有访问修饰符，因为不属于外部类属性，只是在方法内可见，属于方法变量，方法的变量不加修饰符
+ 10.9 内部类标志符
	* 包含内部类的.java文件编译生成.class文件:
	* 包括外部类名.clas、外部类名&内部类名.clas
	* 如果是匿名内部类对应生成外部类名&随机数字.class
	* 如果在内部类A中嵌套内部类B生成内部类Bclass文件时对应生成外部类名&内部类B内部类A.class。再在B中嵌套以此类推