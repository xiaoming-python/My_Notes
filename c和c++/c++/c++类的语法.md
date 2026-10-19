### 类的定义
- 用class关键字表示要创建类
- 用`.`访问类里的元素

```cpp
#include"iostream"
class my_class{//表示创建名为my_class的类
	public://表示公有属性
		int val;//类成员
		void func(){//类方法
			cout<<val<<"\n";
		}
};//结束记得写分号
int main(void){
	my_class cls1;//初始化cls1
	cls.val=11;//用.访问类里的元素，可以修改
	cout<<cls.val<<"\n";
	cls1.func1();//调用类方法
	return 0;
}
```

#### 成员属性
##### public（公有属性）
- 特点：任何地方都可以访问
```cpp
#include"iostream"
class cls{
	public://从public:标签表示开始
	int Val;
};
int main(void){
	cls cls1;
	cout<<cls.val<<"\n";//可以直接访问
	return 0;
}
```
##### protected
- 类自身、派生类、友元可以访问；类外不能直接访问

##### private
- 仅类自身和友元可以访问；派生类和类外都不能直接访问

##### **`class` 默认是 `private`**
```cpp
class A {
    int x; // 默认 private
};
```
##### **`struct` 默认是 `public`**

```cpp
struct B {
        int x; // 默认 public
    };
```

#### 继承
- 继承父类的方法和属性
写法：
```cpp
#inclide"iostream"
class cls1{
	public;
		int val;
};
class cls2:public cls1{//继承cls1的公有属性
	public:
		void func(void){
			cout<<val<<"\n";
		}
}
```
##### **继承方式也有 `public/protected/private`**  
    例如：

```cpp
class D : public B { };
class D : protected B { };
class D : private B { };
```

    但这是继承访问方式，不是类成员自身的访问属性。
    
**继承方式决定了父类的 `public` 和 `protected` 成员，到了子类里会“降级”成什么样。** （父类的 `private` 成员不管怎么继承，子类都无权直接访问，刚才已经说过了）。

C++ 提供了三种继承方式，我们用一张表来看清它们的作用：

|父类中原有的访问属性|`public` 继承|`protected` 继承|`private` 继承|
|---|---|---|---|
|**`public`**|变成 `public`|变成 `protected`|变成 `private`|
|**`protected`**|变成 `protected`|变成 `protected`|变成 `private`|
|**`private`**|不可见|不可见|不可见|

---
