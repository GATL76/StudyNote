# VSCODE创建C#项目

在VSCODE中,点击侧边的Solution,右键图标,点击"Add new project",然后点击"ASP.NET CORE 空"即创建C#项目.点击"xUnit Test Project"创建测试单元,"dotnet test"运行测试单元."dotnet run"运行主程序

# 抽象类

​	**抽象类**指的是函数成员存在没有被完全实现的类.**该类不能是private**,因为需要通过抽象类的子类实现.**抽象类不能实例化**,只能通过作为基类进行派生或引用子类类型的实例.因此,**抽象类是专为做基类而生**.

​	

```csharp
using System;

namespace Example{
	class Program{
		static void Main(string[] args){
			Console.WriteLine("Hello World!");
		}
	}
	//纯虚类:即接口,要求在纯虚类的前提下类型为public
	interface IVehicle{
		void Stop();
		void Fill();
		void Run();
	}
	//部分抽象类:抽象类内只有部分方法是抽象的
	absctract class Vehicle : IVehicle{
		public void Stop(){
			Console.WriteLine("Stopped!");
		}
		public void Fill(){
			Console.WriteLine("Pay and fill...");
		}
		public abstract void Run();
	}

	class Car: Vehicle{
		public override void Run(){
			Console.WriteLine("Car is running...");
		}
	}
	class Truck:Vehicle{
		public override void Run(){
			Console.WriteLine("Truck is running...");
		}
	}
}
/*
为做基类而生的"抽象类"与"开放/关闭原则":添加同类型变量时不应该修改原有代码
*/
```

​	接口和抽象类都是"软件工程产物".在具体实现的流程中,通常是:具体类->抽象类->接口,内部实现的东西越来越少.抽象类是未完全实现逻辑的类(可以有字段和非public成员,它们代表了"具体逻辑").抽象类为复用而生,专门作为基类来使用,也具有解耦功能.
​	**接口是完全未实现逻辑的类**,它们都不能实例化,<font color=red>只能用来声明变量,引用具体类的实例</font>.



# 接口,依赖反转和单元测试

<img src="I:\Typora\Typora\Project\Picture\Pasted image 20230518202152.png" style="zoom:80%;" />

​	认识接口:

```csharp
using System;
using System.Collections;

namespace InterfaceExample{
	class Program{
		static void Main(string[] args){
			int[] nums1 = new int[] {1, 2, 3, 4, 5};
			ArrayList nums2 = new ArrayList{1, 2, 3, 4, 5};
			Console.WriteLine(Sum(nums2));
			Console.WriteLine(Avg(nums2));
		}

		static int Sum(IEnumerable nums){
			int sum = 0;
			foreach (var n in nums) sum += (int)n;
			return sum;
		}

		static double Avg(IEnumerable nums){
			int sum = 0; double count = 0;
			foreach (var n in nums) {sum += (int)n; count++}
			return sum / count;
		}
	}
}
```

​	接口实例:(松耦合)

```csharp
using System;
using System.Collections;

namespace InterfaceExample{
	class Program{
		static void Main(string[] args){
			var user = new PhoneUser(new NokiaPhone());
			user.UsePhone();
		}
	}

	class PhoneUser{
		private IPhone _phone;
		public PhoneUser(IPhone phone){
			_phone = phone;
		}

		public void UserPhone(){
			_phone.Dail();
			_phone.PickUp();
			_phone.Send();
			_phone.Receive();
		}
	}

	interface IPhone{
		void Dail();
		void PickUp();
		void Send();
		void Receive();
	}

	class NokiaPhone : IPhone{
		public void Dail(){
			Console.WriteLine("Nokia calling...");
		}

		public void PickUp(){
			Console.WriteLine("Hello, this is Tim!");
		}

		public void Receive(){
			Console.WriteLine("Nokia message ring...");			
		}

		public void Send(){
			Console.WriteLine("Hello!");			
		}
	}

	class EricssonPhone : IPhone{
		public void Dail(){
			Console.WriteLine("Ericsson calling...");
		}

		public void PickUp(){
			Console.WriteLine("Hello, this is Tim!");
		}

		public void Receive(){
			Console.WriteLine("Ericsson message ring...");			
		}

		public void Send(){
			Console.WriteLine("Hello!");			
		}
	}
}
```



依赖反转:

<img src="C:\Users\GATL76\AppData\Roaming\Typora\typora-user-images\image-20230521130407512.png" alt="image-20230521130407512" style="zoom:80%;" />

```csharp
//紧耦合
using System;

namespace InterfaceExample{
    class Program{
        static void Main(string[] args){
            var fan = new DeskFan(new PowerSupply());
            Console.WriteLine(fan.Work());
        }
    }
    class PowerSupply{
        public int GetPower(){
            return 100;
        }
    }
    
    class DeskFan{
        private PowerSupply _ powerSupply;
        public DeskFan(PowerSupply powerSupply){
            _powerSupply = powerSupply;
        }
        
        public string Work(){
            int power = _powerSupply.getPower();
            if(power <= 0){
                return "Won't work.";
            }else if(power < 100){
                return "Slow";
            }else if(power < 200){
                return "Work fine";
            }else{
                return "Warning!";
            }
        }
    }
}
//引入接口进行解耦
using System;

namespace InterfaceExample{
    public class Program{
        static void Main(string[] args){
            var fan = new DeskFan(new PowerSupply());
            Console.WriteLine(fan.Work());
        }
    }
    public interface IPowerSupply{
        int GetPower();
    }
    
    public class PowerSupply:IPowerSupply{
        public int GetPower(){
            return 100;
        }
    }
    
    public class DeskFan{
        private IPowerSupply _powerSupply;
        public DeskFan(IPowerSupply powerSupply){
            _powerSupply = powerSupply;
        }
        
        public string Work(){
            int power = _powerSupply.GetPower();
            if(power <= 0){
                return "Won't work.";
            }else if(power < 100){
                return "Slow";
            }else if(power < 200){
                return "Work fine";
            }else{
                return "Warning!";
            }
        }
    }
}
```

测试单元:(参考[该链接](https://blog.csdn.net/iSunwish/article/details/101487524?ops_request_misc=&request_id=&biz_id=102&utm_term=C&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-2-101487524.142^v87^insert_down28v1,239^v2^insert_chatgpt&spm=1018.2226.3001.4187#xunit%E6%B5%8B%E8%AF%95%E5%8D%95%E5%85%83&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-2-101487524.142^v87^insert_down28v1,239^v2^insert_chatgpt))

```csharp
using System;
using InterfaceExample;

namespace Interface.Tests;

public class DeskFanTests
{
    [Fact]
    public void PowerLowerThanZero_OK()
    {
        var fan = new DeskFan(new PowerSupplyLowerThanZero());
        var expected = "Won't work.";
        var actual = fan.Work();
        Assert.Equal(expected, actual);
    }
    class PowerSupplyLowerThanZero : IPowerSupply{
        public int GetPower(){
            return 0;
        }
    }
    [Fact]
    public void PowerHigherThan200_OK(){
        var fan = new DeskFan(new PowerHigherThan200());
        var expected = "Warning!";
        var actual = fan.Work();
        Assert.Equal(expected, actual);
    }
    class PowerHigherThan200 : IPowerSupply{
        public int GetPower(){
            return 210;
        }
    }
}
```

# SOLID原则和反射

## SOLID原则

### 单一职责原则(SRP):

​	一个对象应该只包含单一的职责,并且该职责被完整地封装在一个类中,即定义有且仅有一个原因使类变更.(甲类负责两个不同的职责A和B.当由于A需求发生改变而需要修改T时,有可能会导致原本运行正常的职责B功能发生故障,即A和B被耦合在一起)



### 开放封闭原则(OCP):

​	实体应该对拓展是开放的,对修改是封闭的.即可拓展(extension),不可修改(modification).(代码块在增加新功能时只能在下方继续加判断代码而不能够修改原有代码.)



### 里氏替换原则(LSP):

​	一个对象在其出现的任何地方,都可以用子类实例进行替换,并且不会导致程序的错误.经典例子:正方形不是长方形的子类.原因是正方形多了一个属性"长==宽".这时,对正方形设置不同的长和宽,计算面积的结果是最后设置那项的平方,而不是长*宽,从而发生了与长方形不一致的行为.如果程序依赖了长方形的面积计算方式,并使用正方形替换了长方形,实际表现与预期不符.



### 接口隔离原则(ISP)

​	接口隔离原则表名客户端不应该被强迫实现一些它们不会使用的接口,应该把胖接口中的方法分组,然后用多个接口替代它,每个接口服务于一个子模块.简单地说,就是使用多个专门的接口比使用单个接口要好得多.

​	ISP 的主要观点如下:

​	1)一个类对另一个类的依赖性应当是建立在最小的接口上的.

​		ISP可以达到不强迫客户(接口的使用方法)依赖于它们不用的方法,接口的实现类应该只呈现为单一职责的角色(遵循SRP原则).ISP还可以降低客户之间的相互影响-当某个客户要求提供新的职责(需要变化)而迫使接口发生改变时,影响到其他客户程序的可能性最小.

​	2)客户端程序不应该依赖它不需要的接口方法(功能).

​		客户端程序就应该依赖于它需要的接口.



``` cs
//实例1:
/*
输入的接口大于内定的接口,即输入ITank{void Run();void Fire()},而Driver只有IVehicle{void Run();},即接口引脚多路
解决方法:将输入的接口作为两个小接口的组合
*/
using System;
namespace IspExample{
    class Program{
        static void Main(string[] args){
            var driver1 = new Driver(new Car());
            var driver2 = new Driver(new LightTank());
            driver1.Drive();
            driver2.Drive();
        }
    }
    
    class Driver{
        private IVehicle _vehicle;
        public Driver(IVehicle vehicle){
            _vehicle = vehicle;
        }
        public void Drive(){
            _vehicle.Run();
        }
    }
    
    interface IVehicle{
        void Run();
    }
    
    class Car : IVehicle{
        public void Run(){
            Console.WriteLine("Car is running ...");
        }
    }
    
    interface IWeapon{
        void Fire();
    }
    //此处通过接口的继承实现了接口的细分,从而能够只调用一个父类不报错.
    interface ITank : IVehicle, IWeapon{}
    class LightTank : ITank{
        public void Fire(){
            Console.WriteLine("Boom!");
        }
        public void Run(){
            Console.WriteLine("Ka ka ka ...");
        }
    }
}
//实例2
/*
输入的接口小于内有的接口
*/
using System;
namespace IspExample{
    class Program{
        static void Main(string[] args){
            int[] nums1 = {1, 2, 3, 4, 5};
            ArrayList nums2 = new ArrayList{1, 2, 3, 4, 5};
            Console.WriteLine(Sum(nums1));
            Console.WriteLine(Sum(nums2));
            var nums3 = new ReadOnlyCollection(nums1);
            Console.WriteLine(Sum(nums3));//该处报错,因为Sum的接口是ICollection,大于Ienumerable
        }
        //ICollection是int[]和ArrayList都具有的一个接口
        static int Sum(ICollection nums){
            int sum = 0;
           	foreach ( var n in nums){
            	sum += (int)n;   
            }            
            return sum;
        }
        
        //自己构建一个Collection集合
        /*
        当外界迭代该实例时,需要提供一个迭代器IEnumerator.在类内进行声明,防止污染其他的迭代器,即成员类.
        */
        class ReadOnlyCollection : IEnumerable{
            //int[]实例及其构造器
            private int[] _array;
            public ReadOnlyCollection(int[] array){
                _array = array;
            }
            
            public IEnumerator GetEnumerator(){
                return new Enumerator(this);
            }
            public class Enumerator : IEnumerator{
                //进行迭代时,需要传入一个实例
                private ReadOnlyCollection _collection;
                private int _head;
                public Enumerator(ReadOnlyCollection collection){
                    _collection = collection;
                    _head = -1;
                }
                public object Current{
                	get{
                        object o = _collection._array[_head];
                        return o;
                    }
                }
                public bool MoveNext(){
                    if(++_head < _collection._array.Length){
                        return true;
                    }else{
                        return false;
                    }
                }
                public void Reset(){
                    _head = -1;
                }
            }
        }
    }
}
//实例3:显式接口实现
using System;
namespace IspExample{
    class Program{
        static void Main(string[] args){
            Ikill killer = new WarmKiller();
            killer.Kill();
        }
    }
    interface IGentleman{
        void Love();
    }
    interface IKiller{
        void Kill();
    }
    class WarmKiller : IGentleman, IKiller{
        public void Love(){
            Console.WriteLine("I will love you for ever...");
        }
        //显式类型声明,此时只有作为Ikiller的实例时才能够调用.
        void IKiller.Kill(){
            Console.WriteLine("Let me kill the enemy...");
        }
    }
}
```



### 依赖反转原则(DIP)

​	抽象不应该依赖于细节,细节应当依赖于抽象.换言之,要针对抽象(接口)编程,而不是针对实现细节编程.开闭原则(OCP)是面向对象设计原则的基础,也是整个设计的一个终极目标,而依赖反转(DIP)则是实现OCP的一个基础.



## 反射

### 反射的概念

​	反射是dotnet框架,而不是C#语言所特有的.

​	**程序集**:程序集是经由编译器得到的,供进一步编译执行的中间产物.在Windows系统中,它一般表现为后缀为.dll(库文件)或.exe(可执行文件)的格式.

​	**元数据**:元数据是用来描述数据的数据.即程序中的类,类中的函数,变量等信息就是程序的元数据.有关程序以及类型的数据被称为元数据,它们保存在程序集中.

​	**反射**:程序正在运行时,可以查看其他程序集或者自身的元数据.一个运行的程序查看本身或者其他程序的元数据的行为叫做反射.

```C#
//实例
using System;
using System.Reflection;
namespace IspExample{
    class Program{
        static void Main(string[] args){
            ITank tank = new LightTank();//创建静态类型
            var t = tank.GetType()//获取对静态类型的描述
            object o = Activator.CreateInstance(t)//通过激活器接收一个Type类型,此时我们不知道o的静态类型是什么,只知道一定隶属于object
            //读取t下的指定方法
            MethodInfo fireMi = t.GetMethod("Fire");
            MethodInfo runMi = t.GetMethod("Run");
            //第二个参数即待传入的参数,参照获取的方法是否需要即可
            fireMi.Invoke(o,null);
            runMi.Invoke(o,null);
        }
    }
    
    class Driver{
        private IVehicle _vehicle;
        public Driver(IVehicle vehicle){
            _vehicle = vehicle;
        }
        public void Drive(){
            _vehicle.Run();
        }
    }
    
    interface IVehicle{
        void Run();
    }
    
    class Car : IVehicle{
        public void Run(){
            Console.WriteLine("Car is running ...");
        }
    }
    
    interface IWeapon{
        void Fire();
    }
    //此处通过接口的继承实现了接口的细分,从而能够只调用一个父类不报错.
    interface ITank : IVehicle, IWeapon{}
    class LightTank : ITank{
        public void Fire(){
            Console.WriteLine("Boom!");
        }
        public void Run(){
            Console.WriteLine("Ka ka ka ...");
        }
    }
}
```

### 依赖注入

#### Ioc

​	<font color=red>Ioc-Inversion of Control,即"控制反转",是一种设计思想.</font>Ioc意味着将你设计好的对象交给容器控制,而不是传统的在你的对象内部直接控制.传统应用程序是由我们自己在对象中主动控制去直接获取依赖对象,也就是正转;而反转则是由容器来帮忙创建及注入依赖对象.<font color=red>由容器帮我们查找及注入依赖对象,对象只是被动的接收依赖对象,所以是反转</font>.

传统的设计图:

![image-20230526192907439](C:\Users\GATL76\AppData\Roaming\Typora\typora-user-images\image-20230526192907439.png)

有Ioc/DI容器后的设计图:

![](I:\Typora\Typora\Project\Picture\20190920110540399.png)

#### DI

​	<font color=red>DI-Dependency Injection,即"依赖注入".逐渐之间依赖关系由容器在运行期决定</font>.形象的说,<font color=red>即由容器动态的将某个依赖关系注入到组件之中,依赖注入的目的并非为软件系统带来更多功能,而是为了提升组件重用的频率,并为系统搭建一个灵活,可拓展的平台</font>.

​	谁依赖谁:应用程序依赖于Ioc容器;

​	为什么需要依赖:应用程序需要Ioc容器来提供对象需要的外部资源;

​	谁注入谁:Ioc容器注入应用程序某个对象,应用程序依赖的对象;

​	注入了什么:注入某个对象所需要的外部资源(包括对象,资源和常量数据)

```C#
//实例
using System;
using System.Reflection;
using Microsoft.Extensions.DependencyInjection;
namespace IspExample{
    class Program{
        static void Main(string[] args){
            var sc = new ServiceCollection();//创建容器
            sc.AddScoped(typeof(ITank),typeof(LightTank));
            //在当前的AddScoped函数中要对象(输入动态描述),其中第一个参数是接口名,第二个参数是接口实现名
            //该函数实现的是将ITank和LightTank两个接口放入容器
            sc.AddScoped(typeof(IVehicle), typeof(LightTank));
            sc.AddScoped<Driver>();
            //此处将Driver注入到容器中,即实现了接口绑定,此时创建driver实例时不需要输入参数,默认为容器内最新的参数
            var sp = sc.BuildServiceProvider();//创建服务
            
            Itank tank = sp.GetService<ITank>();
            tank.Fire();
            tank.Run();
            var driver = sp.GetService<Driver>();
            driver.Drive();
        }
    }
    
    class Driver{
        private IVehicle _vehicle;
        public Driver(IVehicle vehicle){
            _vehicle = vehicle;
        }
        public void Drive(){
            _vehicle.Run();
        }
    }
    
    interface IVehicle{
        void Run();
    }
    
    class Car : IVehicle{
        public void Run(){
            Console.WriteLine("Car is running ...");
        }
    }
    
    interface IWeapon{
        void Fire();
    }
    //此处通过接口的继承实现了接口的细分,从而能够只调用一个父类不报错.
    interface ITank : IVehicle, IWeapon{}
    class LightTank : ITank{
        public void Fire(){
            Console.WriteLine("Boom!");
        }
        public void Run(){
            Console.WriteLine("Ka ka ka ...");
        }
    }
}
```



### 插件式编程

​	插件:不与主体程序一起编译,但能与主体程序配合使用.

​	一般主体程序都会发布包含程序开发接口,即API(Application Programing Interface)的程序开发包SDK(Software Development Kit).使用SDK中的API接口进行开发会得到比较标准的插件.

​	当使用反射时,我们很容易将耦合变为几近于零,此时开发过程中会缺少约束,需要通过SDK进行约束.

​	实例演示具体参照:[链接](https://learn.microsoft.com/zh-cn/dotnet/core/tutorials/library-with-visual-studio-code?pivots=dotnet-7-0)

#### 创建主体程序:

​	选定文件夹,该文件夹用于保存主体文件和类库文件.在该文件下打开终端,输入以下命令,创建新的**解决方案**.

> ​	mkdir main;	cd main;	dotnet new console

​	打开其中的Program.cs,将其名字更改为BabyStroller.

```C#
using System;
using System.IO;
using System.Collections.Generic;
using System.Runtime.Loader;

//不使用SDK进行的插件支持
namespace BabyStroller.App{
    class Program{
        static void Main (string[] args){
            //Enviroment.CurrentDirectory代表程序运行的目录
            //folder代表在当前程序运行的目录下,名称包含"Animals"的目录.
            var folder = Path.Combine(Environment.CurrentDirectory,"Animals");
            //files代表在folder所对应的目录下的文件
            var files = Directory.GetFiles(folder);
            //animalsTypes代表动物的方法
            var animalsTypes = new List<Type>();
            foreach(var file in files){
                //加载指定路径上的程序集文件的内容
                var assembly = AssemblyLoadContext.Default.LoadFromAssemblyPath(file);
                //获取此程序集中定义的所有类型
                var types = assembly.GetTypes();
                foreach(var t in types){
                    //如果获取的类型中含有Voice方法,则将其添加到animalsTypes.
                    if(t.GetMethod("Voice")!=null){
                        animalsTypes.Add(t);
                    }
                }
            }
			//对animalsTypes进行遍历使用
            while(true){
                for(int i = 0; i < animalsTypes.Count; i++){
                    Console.WriteLine($"{i+1},{animalsTypes[i].Name}");
                }
                Console.WriteLine("===========================");
                Console.WriteLine("Please choose animal:");
                int index = int.Parse(Console.ReadLine());
                if(index > animalsTypes.Count || index < 1){
                    Console.WriteLine("No such an aniamal. Try again!");
                    continue;
                }
                Console.WriteLine("Please input the times:");
                int times = int.Parse(Console.ReadLine());
                var t = animalsTypes[index - 1];
                var m = t.GetMethod("Voice");
                var o = Activator.CreateInstance(t);
                m.Invoke(o,new object[] {times});
            }
        }
    }
}
```

#### 创建第三方插件

​	回到最开始放置main文件的地方,创建空白解决方案,解决方案用作一个或多个项目的容器.

> mkdir AnimalsLib;	cd AnimalsLib;	dotnet new sln;	

​	创建名为"AnimalsLib"的新.NET类库项目:

> dotnet new classlib -o AnimalsLib;//-o用于指定放置生成的输出的位置

​	向解决方案添加库项目:

> dotnet sln add AnimalsLib/AnimalsLib.csproj

​	打开类库中的Class1.cs,进行编程,更改名称为Cat

```c#
namespace AnimalsLib;

public class Cat{
    public void Voice(int times){
        for(int i = 0; i < times; i++){
            Console.WriteLine("Moaw!");
        }
    }
}
```

再添加一个.cs文件,更改名称为Sheep:

```c#
namespace AnimalsLib;

public class Sheep{
    public void Voice(int times){
        for(int i = 0; i < times; i++){
            Console.WriteLine("Baa...");
        }
    }
}
```

将终端切换到AnimalsLib,输入以下指令进行编译:

> dotnet build

在main文件中与bin文件平级的位置创建"Animals"文件,将上述得到的.dll文件复制过来(.dll文件在AnimalsLib->bin->Debug->net7.0中)

将终端切换到main文件中,输入以下指令进行程序运行:

> dotnet run



#### SDK进行约束

​	在之前的实例中,我们使用的是纯反射进行插件式编程,接下来我们通过SDK进行约束:

​	在main文件中创建SDK类库:

> dotnet new classlib -o BabyStrollerSDK

​	更改Class1.cs名称为IAnimal,并编写代码.该代码作为Animal的接口,去约束第三方.

```C#
namespace BabyStrollerSDK;
public interface IAnimal{
    void Voice(int times);
}
```

​	再创建另一个.cs文件(UnfinishedAttribute.cs)存放未开发完的类库:

```C#
using System;
namespace BabyStrollerSDK{
    public class UnfinishedAttribute : Attribute{
        
    }
}
```

​	对SDK进行编译,将得到的.dll文件提供给main和第三方:

> dotnet build
>
> dotent add .\BabyStroller.app\BabyStroller.app.csproj reference .\BabyStrollerSDK\BabyStrollerSDK.csproj
>
> dotent add ..\ClassLibrary\AnimalsLib1\AnimalsLib1.csproj reference .\BabyStrollerSDK\BabyStrollerSDK.csproj

​	更改第三方代码:

```C#
using System;
using BabyStrollerSDK;

namespace AnimalsLib1;
public class Sheep : IAnimal{
    public void Voice(int times){
        for(int i = 0; i < times; i++){
            Console.WriteLine("Baa...");
        }
    }
} 
```



​	若有类库未更新完,则添加哎[Unfinished]标签即可.

```C#
using System;
using BabyStrollerSDK;

namespace AnimalsLib1;
[Unfinished]
public class Cat : IAnimal{
    public void Voice(int times){
        for(int i = 0; i < times; i++){
            Console.WriteLine("Moaw!");
        }
    }
}
```

​	将类库重新编译,将编译得到的.dll文件重新添加到main中的Animals里.

> dotnet build



​	更新主体程序:

```C#
using System;
using System.IO;
using System.Collections.Generic;
using System.Runtime.Loader;
using BabyStrollerSDK;

//不使用SDK进行的插件支持
namespace BabyStroller.App{
    class Program{
        static void Main (string[] args){
            var folder = Path.Combine(Environment.CurrentDirectory,"Animals");
            var files = Directory.GetFiles(folder);
            var animalsTypes = new List<Type>();
            foreach(var file in files){
                var assembly = AssemblyLoadContext.Default.LoadFromAssemblyPath(file);
                var types = assembly.GetTypes();
                foreach(var t in types){
                    //基接口需要包含IAnimal,同时没有[Unfinished]标签
                    if(t.GetInterfaces().Contains(typeof(IAnimal))){
                        var isUnfinished = t.GetCustomAttributes(false).Any(a => a.GetType() == typeof(UnfinishedAttribute));
                        if(isUnfinished) continue;
                        animalsTypes.Add(t);
                    }
                }
            }

            while(true){
                for(int i = 0; i < animalsTypes.Count; i++){
                    Console.WriteLine($"{i+1},{animalsTypes[i].Name}");
                }
                Console.WriteLine("===========================");
                Console.WriteLine("Please choose animal:");
                int index = int.Parse(Console.ReadLine());
                if(index > animalsTypes.Count || index < 1){
                    Console.WriteLine("No such an aniamal. Try again!");
                    continue;
                }
                Console.WriteLine("Please input the times:");
                int times = int.Parse(Console.ReadLine());
                var t = animalsTypes[index - 1];
                var m = t.GetMethod("Voice");
                var o = Activator.CreateInstance(t);
                //通过强制类型转换进行调用
                var a = o as IAnimal;
                a.Voice(times);
            }
        }
    }
}
```

# 泛型,partial类,枚举和结构体

## 泛型(generic)

​	泛型,即"参数化类型",**将类型由原来的具体的类型参数化,类似于方法中的变量参数,**此时类型也定义成参数形式(可以称之为类型形参),然后再使用/调用时传入具体的类型(类型实参).

​	**泛型的本质**是为了<font color=red>参数化类型</font>(在不创建新的类型的情况下,通过泛型指定的不同类型来控制形参具体限制的类型).也就是说在泛型使用过程中,操作的数据类型被指定为一个参数,这种参数类型可以用在类,接口和方法中,分别被称为泛型类,泛型接口和泛型方法.



#### 实例1:泛型类

```C#
using System;

namespace HelloGeneric{
    internal class Program{
        public static void Main(string[] args){
            Apple apple = new Apple(){Color = "Red"};
            Book book = new Book(){Name = "NewBook"};
            Box<Apple> box1 = new Box<Apple>(){Cargo = apple};
            Box<Book> box2 = new Box<Book>(){Cargo = book};
            Console.WriteLine(box1.Cargo.Color);
            Console.WriteLine(box2.Cargo.Name);
        }
    }
    class Apple{
        public string Color{get; set;}
    }
    class Book{
        public string Name{get; set;}
    }
    //Box<类型参数>,通过指定类型参数来确定泛型类的具体使用类型
    class Box<TCargo>{
        public TCargo Cargo{get; set;}
    }
}
```

#### 实例2:泛型接口

```C#
using System;

namespace HelloGeneric{
    internal class Program{
        public static void Main(string[] args){
            Student<int> stu = new Student<int>();
            stu.ID = 101;
            stu.Name = "Timothy";
        }
    }
    
    interface IUnique<TId>{
        TId ID {get; set;}
    }
	//类不指定具体类型,通过实例进行指定
    class Student<TId> : IUnique<TId>{
        public TId ID {get; set;}
        public string Name {get; set;}
    }
}
//===================================================================
using System;

namespace HelloGeneric{
    internal class Program{
        public static void Main(string[] args){
            Student stu = new Student();
            stu.ID = 10000000000000;
            stu.Name = "Timothy";
            Console.WriteLine(stu.ID);
        }
    }
    
    interface IUnique<TId>{
        TId ID {get; set;}
    }
	//由类进行类型指定
    class Student : IUnique<ulong>{
        public ulong ID {get; set;}
        public string Name {get; set;}
    }
}
```

#### 实例3:单个/多个类型参数

```C#
//单个类型参数
using System;
using System.Collections.Generic;

namespace HelloGeneric{
    internal class Program{
        public static void Main(string[] args){
            IList<int> list = new List<int>();
            for(int i=0; i<100; i++){
                list.Add(i);
            }
            foreach(var item in list){
                Console.WriteLine(item);
            }
        }
    }
}
//多个类型参数
using System;
using System.Collections.Generic;

namespace HelloGeneric{
    internal class Program{
        public static void Main(string[] args){
            IDictionary<int, string> dict = new Dictionary<int, string>();
            dict[1] = "Timothy";
            dict[2] = "Michael";
            Console.WriteLine($"Student #1 is {dict[1]}");
            Console.WriteLine($"Student #2 is {dict[2]}");
        }
    }
}
```

#### 实例4:泛型方法

```C#
using System;
using System.Collections.Generic;

namespace HelloGeneric{
    internal class Program{
        public static void Main(string[] args){
            int[] a1={1, 2, 3, 4, 5};
            int[] a2={1, 2, 3, 4, 5};
            double[] a3={1.1, 2.2, 3.3, 4.4, 5.5};
            double[] a4={1.1, 2.2, 3.3, 4.4, 5.5};
            var result1 = Zip(a3, a4);
            var result2 = Zip(a1, a2);
            Console.WriteLine(string.Join(",",result1));
            Console.WriteLine(string.Join(",",result2));
        }

        static T[] Zip<T>(T[] a, T[] b){
            T[] zipped = new T[a.Length + b.Length];
            int ai = 0, bi = 0, zi = 0;
            do{
                if(ai < a.Length)zipped[zi++]=a[ai++];
                if(bi < b.Length)zipped[zi++]=b[bi++];
            }while(ai<a.Length||bi<b.Length);
            return zipped;
        }
    }
} 
```

#### 实例5:泛型委托

```C#
//Action委托(无返回值)
using System;
using System.Collections.Generic;

namespace HelloGeneric{
    internal class Program{
        public static void Main(string[] args){
            Action<string> a1=Say;
            a1("Timothy");
            Action<int> a2=Mul;
            a2(1);
        }
        
        static void Say(string str){
            Console.WriteLine($"Hello, {str}!");
        }
        static void Mul(int x){
            Console.WriteLine(x*100);
        }
    }
} 
//Func委托(有返回值)
using System;
using System.Collections.Generic;

namespace HelloGeneric{
    internal class Program{
        public static void Main(string[] args){
            //Lambda表达式
            Func<double,double,double> func1 = (a, b)=>{return a + b;};
            var result = func1(1.1,2.2);
            Console.WriteLine(result);
        }
    }
} 
```

### Partial类

​	指示类型声明未类型的分部定义.可以使用Partial关键字在几个声明之间划分类型的定义.可以根据需要在任意数量的不同源文件中使用任意数量的分部声明.但是,所有声明都必须在相同的程序集和相同的命名空间中.

### 枚举

枚举类型 是由基础[整型数值类型](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/builtin-types/value-types)的一组命名常量定义的[值类型](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/builtin-types/integral-numeric-types)。 若要定义枚举类型，请使用 `enum` 关键字并指定枚举成员 的名称：

```C#
enum Season
{
    Spring,
    Summer,
    Autumn,
    Winter
}
```



### 结构体

结构类型（“structure type”或“struct type”）是一种可封装数据和相关功能的[值类型](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/builtin-types/value-types) 。 使用 `struct` 关键字定义结构类型：

```C#
public struct Coords
{
    public Coords(double x, double y)
    {
        X = x;
        Y = y;
    }

    public double X { get; }
    public double Y { get; }

    public override string ToString() => $"({X}, {Y})";
}
```

```C#
using SYstem;

namespace HelloEnum{
	internal class Program{
		public static void Main(string[] args){
			Student student = new Student(){ID = 101, Name = "Timothy"};
			object obj = student;//装箱,将student这个实例做一个复制,存放到内存堆内,然后通过obj引用堆内存中student实例
			Student student2 = (Student) obj;//拆箱,在值函数中,传递的是值本身,而不是地址
			Console.WriteLine($"#{student2.ID} Name:{student2.Name}")
		}
	}
	struct Student{
		public int ID {get; set;}
		public string Name{get; set;}
	}
}
```

