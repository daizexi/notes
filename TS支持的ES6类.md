# TS支持的ES6类

## 继承

### ES6提供了extends用于实现单继承

- 单继承

	- 单继承是指一个类只能有一个父类

- 语法

	- 如果想要实现继承，那么必须在子类的构造器中调用父类的构造器。（使用super()可以调用父类的构造器，传给super()的参数就是传给父类构造器的参数）。

## 静态

### ES6的类支持静态成员（静态属性和静态函数）

- 静态成员会被所有实例共享。

## 访问修饰符（仅TS可用，当TS被编译为JS后访问修饰符就不再有意义）

### ES6支持访问修饰符，它们决定了成员的可访问性。

- public （默认，如果不指定访问修饰符，那么默认是public）

	- 类、子类、实例都可以访问。

- protected

	- 类、子类可以访问。
	- 实例不可以访问。

- private

	- 类内部可以访问。
	- 子类和实例都不可以访问。

## abstract

### abstract可以作用在类以及类的任何成员上。（有点类似于C++中的接口）。

- 当使用abstract来修饰一个类

	- 那么这个类不能被实例化，我们必须创建一个类来继承abstract类。

- 当使用abstract来修饰一个成员（方法或变量）

	- 那么子类必须实现这个方法或变量。

## 构造器（constructor）

### 构造器对于一个类来说是可选的，并不是每一个类都要有一个构造器。

### 构造器的作用

- 构造器可以用来初始化变量

  - 同时在类的构造器中初始化的变量会直接是实例的属性，而放在构造器之外的方法、变量等，则会存在于实例的原型对象上。（实例的原型对象使用__proto__访问）。

    ```{javascript}
    class Foo{
      constructor(x){
        this.number = x;
      }
    
      getX(){
          return this.number}
      }
    }
    const obj = new Foo(1)
    console.log(obj)
    console.log(obj.__proto__)
    ```

## 属性初始化

### ES7支持在类的构造器外初始化类的成员（变量或方法）。

- 构造器外初始化的成员允许提供一个默认值，同时我们也可以在构造器中对它们覆盖赋值。

	- 如果在构造器中对他们覆盖赋值了，那么这个成员就将存在于实例上，否则它们将存在于实例的原型对象上。
	
	  ```{javascript}
	  class Foo{
	    number = 0
	    constructor(x){
	      this.number = x;
	    }
	  }
	  const obj = new Foo(1)
	  console.log(obj)
	  ```
	
	  

## __proto__和prototype

### prototype

- JS的所有函数都存在一个prototype属性，它将指向一个对象，这个对象是通过构造函数或构造器创造的实例的原型对象。

	- 同样所有原型对象上存在一个属性constructor指向与之关联的构造函数。
	- 当自定义类或构造函数，构造器/构造函数的prototype属性指向的原型对象中只存在一个constructor属性，并且其他变量/方法都将通过\__proto__从Object继承。

### __proto__

- 每次调用构造函数创建实例时，由于new操作符的原理，新对象的[[prototype]]特性会被赋值为这个构造函数的prototype属性指向的原型对象。

	- 而由于浏览器不支持直接访问[[prototype]]特性，所以浏览器实现了一个__proto__属性指向[[prototype]]特性。

```{jsx}
const store = createStore({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: (state) => {
      return state.todos.filter(todo => todo.done)
    },
    doneTodosCount: (state,getters) => {
      return getters.doneTodos.length
    }
  }
})
```

