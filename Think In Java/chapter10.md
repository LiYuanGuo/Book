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