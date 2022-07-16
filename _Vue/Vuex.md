# Vuex

## Vuex作用

### Vuex是专门为了管理状态而生，并且它是响应式的。

- 当有一个状态会被多个组件使用并且更改时，将这个状态放到Vuex的store对象中是个好的选择。

	- store对象

		- state选项用于储存状态。
		- Getter选项用于定义对于state的一些多次使用的操作函数。
		- Mutation选项用于定义对于state的更改（副作用）处理函数，需要通过commit调用。
		- Action用于处理Mutation无法处理的异步情况，但是Action必须通过commit调用Mutation才可以更改state。
		- Module用于解决store对象过于臃肿的问题。

## 核心概念

### state

- 在Vue组件获取Vuex状态

	- 由于Vuex的状态储存是响应式的，所以想要在某个组件中获取Vuex中储存的状态，最简单的方法就是使用计算属性。

		- 我们可以使用Vue的插件系统将store实例从根组件注入到所有的子组件中，在子组件中就可以使用 this.$ 访问到 store。

- mapState函数

	- mapState函数的作用是，当一个组件需要获取多个状态的时候，将所有这些状态都声明为计算属性操作比较繁琐，这时我们就可以使用mapState函数帮助我们生成计算属性。

		- 当我们指定的计算属性名称与Vuex的状态名一致时，我们可以直接传递一个字符串数组给mapState函数。
		- 当我们需要在使用mapState的同时，定义其他计算属性。

			- 由于mapState函数返回的是一个对象，当我们在使用Options API的时候，这就意味着，我们需要将这个mapState返回的对象混合其他我们定义的计算属性为一个对象，然后传给computed选项。

				- 这时，我们可以用到展开运算符来将对象展开。

- 组件仍然有必要保有局部状态

	- 使用Vuex并不意味着，需要将所有状态都放入Vuex，我们只把需要多个组件访问或修改的状态放入Vuex。

### Getter

- Getter的作用

	- 有时候我们需要对储存在store中的state进行一些操作。

		- 如果我们有多个组件需要进行同一个这样的操作，那么我们可以使用Getter（也就是在store中定义一个Getter函数）。

			- 这样做的好处是当多个组件都需要对state做同样的处理时，我们不需要操作繁琐的在多个组件中复制同一个函数。
			- store中定义的Getter接受state作为其第一个参数。

- getter也接受其他getter作为第二个参数。
- 如何在组件中访问Getter

	- Getter会暴露为 store.getters对象，在组件中可以通过 this.$store.getters.doneTodosCount来访问。

- mapGetters函数

	- mapGetters的作用

		- mapGetters作用类似于mapState，只是用于简化写法，尤其是当我们在某个组件中需要访问多个getter。

	- mapGetters的用法

		- 为mapGetters传入一个字符串数组，其中元素是对应需要映射到组件的局部计算属性的getter。
		- 也可以使用对象形式，这样可以为getter属性映射为其他名称的局部计算属性（也就是在当前组件内换一个名字）。

### Mutation

- Mutation的作用

	- Mutation是更改Vuex的store中的状态的唯一方法。

- Mutation基础用法

	- Mutation的用法类似于事件触发，当在某个组件中触发某个Mutation之后，就会调用对应的处理函数，来修改store中的state。

		- 某个组件中的触发方法

			- this.$store.commit('increment')

				- 这时就会执行store中对应increment的处理函数。

- 为Mutation添加提交载荷

	- 提交载荷的作用主要是，为mutation的处理函数传入额外的参数。
	- 在大多数情况下，载荷会是一个对象，载荷对象将会传给store中的对应mutation的处理函数。
	- 我们也可以选择对象风格的写法来使用Mutation

		- 对象风格的写法是指直接传递一个对象给commit函数，其中这个对象包含type属性来指定对应的Mutation。

	- 使用常量代替Mutation事件类型

		- 使用常量代替Mutation的好处是可以将这些常量放在单独的文件中，这样可以让代码合作者对整个app中包含的mutation一目了然（这在大型项目中很有用）。

- Mutation对应的处理函数必须是同步函数
- mapMutations函数

	- mapMutation函数的用法和mapGetters函数用法一样。

### Action

- Action的作用

	- Action类似于Mutation，但是有2点不同。

		- Action不能直接修改state，Action通过提交Mutation来修改状态。
		- Action可以包含异步操作。

- Action的用法

	- 在Vuex的store中注册Action的方法

		- Action函数接受一个和store实例具有相同方法和属性的context对象，因此可以调用context.commit() 来提交一个Mutation。（同时也可以使用context.state和context.getters来获取state和getters）。

	- 在组件中调用Action的方法

		- this.$store.dispatch('increment')，这样就会触发对应的increment Action。

			- store.dispatch将会返回一个promise（这是为了组合调用store.dispatch）。

		- 通过在组件中也支持使用mapActions函数批量映射Action。

### Module

- Module的作用

	- 当我们的项目比较大时，很多状态会被放在store对象中，这就使得store对象十分臃肿，而Module就是为了解决这个问题。

		- Vuex允许我们将store分割成Module（模块），每个模块都拥有自己的state、mutation、action、getter。（只需要在注册的时候放在module选项中）。

- Module需要注意的地方

	- 不同模块内部的action、mutation、getter都是注册在全局命名空间，这要求我们不要在不同的Module中注册同名的action、mutation、getter。

## 如何在组合式API中访问store

### 可以使用useStore函数，在setup()钩子函数中访问store。

