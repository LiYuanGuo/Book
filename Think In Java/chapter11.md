###chapter 11
####持有对象
####
+ 11.1泛型和类型安全的容器
	* 没有使用泛型的时候默认指定能存储Object类型的数据 运行时异常 
	* 使用泛型时只能存储指定类型和其子类 不然编译错误
	* 代码如下：
	
			class Apple{
				public int id(){
					return 1;
				}
			}
		
			class Orange{
				
			}
			
			public class AppleAndOrangeList {
			
				public static void main(String[] args) {
					//没有使用泛型的时候默认指定能存储Object类型的数据 运行时异常 
					//使用泛型时只能存储指定类型和其子类 不然编译错误
					ArrayList apples=new ArrayList();
					apples.add(new Apple());
					apples.add(new Orange());
					for(int i=0;i<apples.size();i++){
						//没有使用泛型的时候 强制转换会出现运行时异常
						((Apple)apples.get(i)).id();
					}
				}
			
			}
+ 11.2 基本概念
	* 一大类Collection（每一个槽中只能保存一个元素）: List、Set(不能有重复元素)、Quere继承自此接口
	* 二大类Map：每个槽中保存两个值（key和value）
+ 11.3 添加一组元素
	* Arrays.asList()、Arrays.<?>asList()
	* Collections.addAll() 
	* 223页