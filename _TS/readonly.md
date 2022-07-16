# readonly

## readonly的作用

### readonly用于标记一个属性是只读的，也就是不可修改的。

```typescript
function foo(config: {readonly bar: number, readonly bas: number})
{
    console.log(config.bar)
    config.bar = 123 //不能将123分配到config.bar，因为config.bar是只读属性。
}
```

## readonly用法

### 直接在属性上使用

- 在对象中使用，规定对象的属性是只读属性。

  

- 在interface或type中使用，声明结构化注解的属性是只读的。

  ```{typescript}
  type Foo = {
      readonly bar: number;
      bas: number;
  }
  
  const foo: Foo = {bar: 123, bas: 123};
  foo.bar = 1 //不能将1分配到foo.bar，因为foo.bar属性是只读的。
  foo.bas = 1
  
  interface Foo{
      readonly bar: number;
      bas: number;
  }
  
  const foo: Foo = {bar: 123, bas: 123};
  foo.bar = 1 //不能将1分配到foo.bar，因为foo.bar属性是只读的。
  foo.bas = 1
  ```

- 在类中使用，规定一个属性是只读的。

	- 在类中定义只读属性的时候，只读属性可以在声明的时候初始化，也可以在构造函数中初始化这个只读属性。

		- 并且易错的是，如果在只读属性声明的时候初始化了，但是在构造函数中依然可以修改。
		
		  ```{typescript}
		  class Foo{
		      readonly bar: number;
		      bas = 1 as number;
		  
		      constructor(bar?: number){
		          this.bar = bar
		      }
		  }
		  
		  class Bar{
		      readonly foo = 1 as number;
		      bas: number = 1
		      constructor(foo?: number){
		          this.foo = foo
		      }
		  
		      log(){
		          console.log(this.bas);
		      }
		  }
		  
		  const obj = new Bar(123)
		  console.log(obj.foo)
		  console.log(obj)
		  obj.log()
		  ```

### readonly用例

- 可以使用readonly接收一个泛型T，用来将这个泛型中的所有属性都标记为readonly。

  ```{typescript}
  type Foo = {
      bar: number;
      bas: number;
  }
  
  const foo1: Foo = {bar: 123, bas: 123};
  
  const foo2: Readonly<Foo> = {bar: 123, bas: 123};
  
  foo1.bar = 1
  foo2.bar = 1 //无法分配到foo2.bar，因为它是只读属性。
  ```

### 使用readonly标记索引签名类型

```{typescript}
interface Foo{
    readonly [key: number]: number;
}

const foo: Foo = {0: 123, 1: 123};

foo[0] = 1 //Foo类型中的索引签名只支持读取。
```

## readonly的自动推断

### 在某些情况下，TS编译器可以把一些特定的属性推断为readonly。

- 在一个class中，如果一个属性只有getter，没有setter，那么这个属性将被推断为是readonly的。

  ```{typescript}
  class Person{
      firstName: string = 'a';
      lastName: string = 'b';
  
      get fullName(){
          return this.firstName + this.lastName;
      }
  }
  
  const obj = new Person();
  obj.fullName = 'c' //无法分配到fullName属性，因为它是只读的。
  ```

## readonly和const比较

### readonly

- readonly有2个特点

	- readonly用于规定属性是只读的，防止属性被修改。
	- 在一些情况下，readonly规定过的属性可以被修改。

		- readonly可以确保类型为规定了只读属性的对象修改，但是其他类型中没有规定该属性是只读类型的对象修改。
		
		  ```{typescript}
		  const foo: {
		      readonly bar: number
		  } = {
		      bar: 123
		  }
		  
		  const foo1: {
		      bar: number
		  } = foo
		  
		  foo1.bar = 1
		  console.log(foo)
		  ```

### const

- const用于规定变量是不可修改的。

