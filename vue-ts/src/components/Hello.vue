<template>
  <div>
    {{msg}}
    <p>姓名: {{name}}</p>
    <p>
      <input type="text" placeholder="输入特性名称" @keyup.enter="addFeatures">
    </p>
    <ul>
      <li v-for="f in features" :key="f">
        {{f.name}}
      </li>
      <li>
        特性数量:{{featureCount}}
      </li>
    </ul>
  </div>

</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

// class Feature {
//   constructor(public id: number, public name: string, public version: string) {}
// }
// 将上面的类Feature改为接口约束
interface Feature {
  id: number;
  name: string;
  version: string;
}

interface Result<T> {
  ok: 0 | 1;
  data: T[];
}
function getData<T>(): Result<T> {
  const data: any[] = [
    { id: 1, name: "类型注解", version: "2.0" },
    { id: 2, name: "编译型语言", version: "1.0" }
  ];
  return { ok: 1, data };
}

/*
// 接口的实现
interface Person {
  firstName: string;
  lastName: string;
}
function greeting(person: Person) {
  return `Hello, ${person.firstName} ${person.lastName}`;
}
const user = { firstName: "Jane", lastName: "User" };
console.log(user);
console.log(greeting(user));
*/

// 面向接口编程
interface Person {
  firstName: string;
  lastName: string;
  sayHello(): string; // 要求实现方法, 返回值是一个字符串
}
// 实现接口
class Greeter implements Person {
  constructor(public firstName = "", public lastName = "") {}
  sayHello() {
    return `Hello ${this.firstName} ${this.lastName}`;
  }
}
// 面向接口编程
function greeting(person: Person) {
  return person.sayHello();
}
const user = new Greeter("Jane", "User");
console.log(user);
console.log(greeting(user));


@Component
export default class Hello extends Vue {
  @Prop() private msg!: string; // 属性msg必填项,字符串类型
  @Prop() private name: string = "匿名";

  // 普通的属性相当于 组件的data
  private features: Feature[] = [
    { id: 1, name: "静态类型", version: "1.0" },
    { id: 2, name: "编译", version: "1.0" }
  ]; //  字符串型数组

  // 生命周期
  created() {
    // ...
  }

  // 计算属性的写法
  get featureCount() {
    return this.features.length;
  }

  addFeatures(event: any) {
    console.log(event);
    this.features.push({
      name: event.target.value,
      id: this.features.length + 1,
      version: "1.0"
    });
    event.target.value = "";
  }
}

// 类型注解
let title: string;
let name = "xx";

// 数组类型
let names: (string | number)[];
names = ["tom", "jerry", 123];

// 任意类型
let list: any[] = [1, true, "free"];
list[0] = "lalala";

// 函数中使用类型注解
// function greeting(person: string): string {
//   return "hello, " + person;
// }
// greeting("lz");

// 无返回类型void
function warn(): void {
  alert("warning!!!");
}

// 内置类型: string, number, boolean, void, any

// ts函数中如果声明, 就是必选参
function sayHello(name: string, age: number): string {
  return name + " " + age;
}
sayHello("tom", 20);

// 如果加上? 则是可选参数
function sayHello1(name: string, age?: number): string {
  return name + " " + age;
}

// 返回类型可以是多个和全部
function sayHello2(name: string, age: number = 14): string | number {
  return age;
}

// 函数重载, 多个同名函数, 通过参数数量或者类型不同或者返回值不同
function info(a: { name: string }): string;
function info(a: string): object;
function info(a: any): any {
  if (typeof a === "object") {
    return a.name;
  } else {
    return { name: a };
  }
}
info({ name: "jerry" });
info("jerry");

class Shape {
  readonly foo: string = "foo";
  protected area: number;

  constructor(public color: string, width: number, height: number) {
    this.area = width * height;
    this.color = color;
  }

  shoutout() {
    return (
      "I`m " + this.color + " width an area of " + this.area + " cm squared."
    );
  }
}
class Square extends Shape {
  constructor(color: string, side: number) {
    super(color, side, side);
    console.log(this.color);
  }
  shoutout() {
    return `我是:${this.color} 面积是:${this.area} 平方厘米`;
  }
}
const square: Square = new Square("blue", 2);
console.log(square.shoutout());

class Employee {
  private _firstName: string = "Mike";
  private _lastName: string = "James";
  get fullName(): string {
    return `${this._firstName} ${this._lastName}`;
  }
  set fullName(newName: string) {
    console.log("您修改了姓名");
    this._firstName = newName.split(" ")[0];
    this._lastName = newName.split(" ")[1];
  }
}
const employee = new Employee();
console.log(employee.fullName);
employee.fullName = "Smith Bob";
console.log(employee.fullName);
</script>

<style scoped>
</style>