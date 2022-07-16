# TS中的类型保护

## 类型保护

### 什么是TS中的类型保护

- TS的类型保护一般针对于联合类型，当我们使用if来缩小变量类型的时候，TS可以推断出条块中的变量的类型。

## typeof

### 当在一个条件块中使用typeof判断变量的类型，那么在这个块中，这个变量也将被推断为对应类型。

- if(typeof x === 'string')

- 那么在条件块中，x将被推断为string类型。

  ```{typescript}
  function doSome(x: number | string)
  {
      if(typeof x === 'string')
      {
          console.log(x.substring(0,1));
      }
      console.log(x.substring) //number | string类型上不存在substring方法。
  }
  ```

  

##  instanceof

### 类似于typeof，当在一个条件块中使用instanceof，那么变量也将在这个块中被推断为对应的引用类型。

```{typescript}
class Foo{
    foo = 123;
}
class Bar{
    bar = 123;
}
function doSome(arg: Foo | Bar)
{
    if(arg instanceof Foo)
    {
        console.log(arg.foo)
      	console.log(arg.bar) //Foo类型上不存在bar属性。
    }
    else if(arg instanceof Bar)
    {
        console.log(arg.bar)
    }
}

function doSome(arg: Foo | Bar)
{
    if(arg instanceof Foo)
    {
        console.log(arg.foo);
    }
    else{
        console.log(arg.bar);
    }
}
```



## in操作符

### in操作符可以安全检查一个对象上是否存在某个属性。

- 当in操作符用于条件块，类型保护也会起作用，TS会自动推断条件块中的变量是什么类型。

```{typescript}
interface A{
    x: number;
    x1: string;
}

interface B{
    y: number;
    y1: string;
}

function doSome(arg: A | B)
{
    if('x' in arg)
    {
        arg.x1 = '123';
    }
    else{
        arg.y1 = '123';
    }
}
```

