
# 设计模式

**软件设计的金科玉律：复用**

## 设计模式分类

1.创建型（Creational）模式：将对象的部分创建工作延后至其他对象。  
2.结构型（Structual）模式：通过类继承或类对象组合为灵活的结构。  
3.行为型（Behavioral）模式：通过类继承或者对象组合来实现类与对象之间的职责。  

## 设计模式的应用不宜先入为主  

没有一步到位的设计模式，一般是重构到某个设计模式

## 模板化方法  

它给出了一个算法框架，将其中一些步骤延迟到子类进行  
在**父类**中，定义处理流程的框架，**子类**中完成具体实现  

优点：
（1）具体细节步骤实现定义在子类中，子类定义详细处理算法是不会改变算法整体结构。  
（2）代码复用的基本技术，在数据库设计中尤为重要。  
（3）存在一种反向的控制结构，通过一个父类调用其子类的操作，通过子类对父类进行扩展增加新的行为，符合“开闭原则”。  

```c++
#include<iostream>
#include<algorithm>
#include<string>
class DrinkTemplate{//做饮料模板
public:
    virtual void BoildWater()=0;
    virtual void Brew()=0;
    virtual void PourInCup()=0;
    virtual void AddSomething()=0;
    void Make(){
        BoildWater();
        Brew();
        PourInCup();
        AddSomething();
    }
};
class Coffee :public DrinkTemplate{
    virtual void BoildWater(){
        std::cout<<"煮山泉水"<<std::endl;
    }
    virtual void Brew(){
        std::cout<<"冲泡咖啡"<<std::endl;
    }
    virtual void PourInCup(){
        std::cout<<"咖啡加入杯中"<<std::endl;
    }
    virtual void AddSomething(){
        std::cout<<"加糖加牛奶"<<std::endl;
    }
};
 
class Tea :public DrinkTemplate{
    virtual void BoildWater(){
        std::cout<<"煮自来水"<<std::endl;
    }
    virtual void Brew(){
        std::cout<<"冲泡铁观音"<<std::endl;
    }
    virtual void PourInCup(){
        std::cout<<"茶水加入杯中"<<std::endl;
    }
    virtual void AddSomething(){
        std::cout<<"加下雨天氛围"<<std::endl;
    }
};
int main(){
    Tea* tea = new Tea;
    tea->Make();
    Coffee* coffee=new Coffee;
    coffee->Make();
    return 0;
}
```

缺点：  
每个不同的实现都需要定义一个子类，会导致类的个数增加，系统更加庞大。  
