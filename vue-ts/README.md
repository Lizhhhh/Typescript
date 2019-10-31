# 使用vue/cli生成 ts项目
  - vue create vue-ts(项目名)
    - 选择Manually select features
      - 把TS勾上,其他的看情况


# TypeScript
  - TypeScript是JavaScript的超集,它可编译为纯JavaScript,是一种给JavaScript添加特性的语言扩展.
  - TS有如下特点
    - 类型注解和编译时类型检查
    - 基于类的面向对象编程
    - 泛型
    - 接口
    - 声明文件

# 类
  - 面向对象:
    - 通过extends实现继承
    - 使用public等访问修饰符实现封装
    - 通过方法覆盖实现多态

````
class Shape {
  area: number
  protected color: string

  constructor (color: string, width: number, height: number ) {
    this.area = width * height;
    this.color = color;
  }

  shoutout() {
    return "I`m " + this.color + " width an area of " + this.area + " cm squared."
  }
}

class Square extends Shape{
  constructor(color:string, side: number){
    super(color, side, side);
    console.log(this.color);
  }
  shoutout() {
    return `我是:${this.color} 面积是:${this.area} 平方厘米`
  }
}

const square: Square = new Square('blue', 2);
console.log(square.shoutout());
````
  - public:
    + 公有类型,都可以用
  - private
    + 当前成员被标记成private时,它就不能在声明它的类的外部访问
  - protected:
    + protected成员在派生类中仍然可用访问
  - readonly:
    + 只读属性必须在声明时或构造函数里被初始化
  - 参数属性
    + 给构造函数的参数加上修饰符,能够定义并初始化一个成员属性


# 存取器
  - 当获取和设置属性有额外逻辑时可用使用存取器(又称getter、setter);
````
class Employee {
  private _fullName: string = "Mike James";
  get fullName(): string{
    return this._fullName;
  }
  set fullName(newName: string):void {
    console.log('您修改了用户名');
    this._fullName = newName;
  }
}
const employee = new Employee();
employee.fullName = 'Bob Smith';
````

# 接口
  - 接口仅约束结构,不要求实现,使用更简单
  - 即:仅仅是约束数据类型使用接口是比较好的解决方案之一
````
interface Person{
  firstName: string;
  lastName: string;
}
function greeting(person: Person) {
  return `Hello, ${person.firstName} ${person.lastName}`;
}
const user = {firstName: 'Jane', lastName: 'User'};
console.log(user);
console.log(greeting(user));
````

# 面向接口编程
````
interface Person {
  firstName: string;
  lastName: string;
  sayHello(): string;  // 要求实现方法
}
// 类实现接口
class Greeter implements Person{
  constructor(public firstName ='', public lastName = ''){}
  sayHello() {
    return `Hello ${this.firstName} ${this.lastName}`;
  }
}
// 面向接口编程
function greeting(person: Person){
  return person.sayHello();
}
const user = new Greeter('Jane', 'User');
console.log(user);
console.log(greeting(user));
````

# 泛型(Generics)  - 。-
  - 是指在定义函数、接口或类的时候,不预先指定具体的类型,而在使用的时候在指定类型的一种特性
````
// 不用泛型
interface Result {
  ok: 0 | 1;
  data:Feature
}

````
  - 泛型的使用实例
````
// Feature 接口
interface Feature{
  id: number;
  name: string;
  version: string;
}

// 初始化一个接口类型的feature
private feature: Feature[] = [];

// 声明泛型
interface Result<T>{
  ok: 0 | 1;
  data: T[];
}

// 声明泛型函数
function getData<T>(): Result<T> {
  const data: any[] = [
    {id: 1, name: "类型注解", version:"2.0"},
    {id: 2, name: "编译型语言", version:"1.0"}
  ];
  return { ok: 1, data } as Result<T>;
}


created() {
  this.features = getData<Feature>().data;
}
````

# 装饰器
  - 实际上是工厂函数,传入一个对象,输出处理后的新对象
  - 典型:组件装饰器@Component
````
@Component
export default class Hello extends Vue{}
// 若不加小括号,则装饰器下面紧挨着的对象就是目标对象
````
  - 如果装饰器需要配置,则要以函数形式使用并传入配置
````
@Component({
  props:{   // 属性也可用在这里配置
    name: {
      type: String,
      default: '匿名'
    }
  }
})
export default class Hello extends Vue {}

````