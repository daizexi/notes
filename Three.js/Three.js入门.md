# 1. 安装Three.js以及结合Vue、Vite使用

> 1. 使用npm create vite@latest，创建Vite + Vue项目。
>
> 2. 使用npm i three --save，将Three.js下载为dependencies。
>
> 3. 在components目录下，运行touch first-three.vue。
>
> 4. import * as THREE from 'three'，在first-three.vue中使用Three.js。
>
>    ```{vue}
>    <tempalte>
>      <div ref="el"></div>
>    </tempalte>
>    
>    <script>
>      export default defineCompoent({
>        setup(props,ctx){
>          const el = ref(null);
>          onMounted(() => {
>            ...
>            el.value.appendChild(renderer.domElement)
>          })
>        }
>        return {
>        el
>      }
>      })
>    </script>
>    ```

# 2.Three.js的基本组件

> - 场景
>
>   - 创建场景scene的方法
>
>     ```{js}
>     const scene = new THREE.Scene();
>     ```
>
>   - 场景的作用
>
>     - 场景是一个容器，用于保存、跟踪所要渲染的物体和需要的光源。如果不创建场景，那么Three.js就无法完成渲染。
>     
>   - 场景相关方法和属性
>
>     1. add()，用于向场景中添加对象。
>     2. remove()，用于移除场景中的对象。
>     3. getObjectByName()，利用name属性获取场景中的特定对象。（每个物体都可以设置name属性，这个API的作用就是通过这个name属性获取场景中对应的物体）。
>     4. fog，用于指定场景中的雾化效果。
>     5. overrideMaterial，用于为场景中所有物体强制设定相同的材质。
>     6. children，场景对象的children属性将指向场景中所有对象组成的数组（包含摄像机和光源）。
>
>   - 场景的雾化效果
>
>     - 雾化效果的作用
>
>       - 雾化效果可以使场景中物体离摄像机越远就变得越模糊。
>
>     - 在场景中使用雾化效果
>
>       - 使用场景对象的fog属性。
>
>         ```{ts}
>         import * as THREE from 'three';
>         
>         const scene = new THREE.Scene();
>         {
>           const color: number = 0xffffff
>           const near: number = 1
>           const far: number = 10
>           scene.fog = new THREE.Fog(color,near,far)
>         }
>         ```
>
>         Color，用于指定雾的颜色。
>
>         near，用于指定一个距离摄像机较近的距离，距离摄像机小于这个距离的物体将不受雾化影响。
>
>         far，用于指定一个距离摄像机较远的距离，距离摄像机距离大于这个距离的物体颜色表现为完全被雾化效果覆盖（物体颜色为雾的颜色）。
>
>         从near到far，效果表现为从物体颜色到雾颜色的fade效果。
>
>     - 强制为场景中所有物体规定为某个材质
>
>       - 使用场景属性overrideMaterial
>
>         - 当为场景对象设置了overrideMaterial属性，那么场景中所有的物体都将被设置为overrideMaterial指向的材质（即使物体已设置了材质）。
>
>           ```{ts}
>           interface MeshType{
>             color: number; //设置材质的颜色，默认值为0xffffff
>           }
>           
>           const scene = new THREE.Scene();
>           const meshObj: MeshType = {color: 0xffffff};
>           scene.overrideMaterial = new THREE.MeshLambertMaterial(meshObj);
>           ```
>
>           
>
>       - 作用
>
>         - 使用overrideMaterial属性可以减少Three.js中场景中的材质数量，提高渲染效率。
>
> - 摄像机
>
>   - 摄像机的作用
>
>     - 摄像机对象决定了我们可以看到什么样的场景（场景中的哪些对象可以被看到）。
>
>   - 创建摄像机
>
>     ```{ts}
>     const camera = new THREE.PerspectiveCamera(args)
>     ```
>     
>   - 选择合适的摄像机的方法
>
>     - Three.js提供了2种不同的摄像机。（除此之外，Three.js还提供了一些用于支持VR设备的特殊摄像机）
>
>       1. 正交投影摄像机，new THREE.PerspectiveCamera
>       2. 透视投影摄像机，new THREE.OrthographicCamera
>
>     - 正交投影摄像机
>
>       - 正交投影摄像机的特点（常用于二维效果）
>
>         - 使用正交投影摄像机，物体距离摄像机的远近并不影响物体最终渲染出的大小。
>
>       - 正交投影摄像机的构造函数THREE.OrthographicCamera()
>
>         THREE.OrthographicCamera()构造函数接收的参数
>
>         1. left: number，可视范围的左平面，超出这个平面的物体将不会被渲染（也就是看不到）。
>         2. right: number，可视范围的右平面。
>         3. top: number，可视范围的上平面。
>         4. bottom: number，可视范围的下平面。
>         5. near: number，从距离摄像机near的平面处，开始渲染物体。
>         6. far: number，从距离摄像机far的平面处，不再渲染物体。
>         7. zoom: number，放缩场景。
>
>     - 透视投影摄像机（常用于三维效果）
>
>       - 透视投影摄像机的特点
>         - 使用透视投影摄像机，物体距离摄像机的远近将会影响物体渲染出的大小，物体距离摄像机越远，物体的大小就会越小。
>
>       - 透视投影摄像机的构造函数THREE.PerspectiveCamera()
>
>         THREE.PerspectiveCamera()构造函数接收的参数 
>
>         1. fov: number，fov表示视场，视场用于定义摄像机可以看到的角度，如，人类眼睛有180左右的视场。
>         2. aspect，aspect用于指定渲染结果的横向尺寸和纵向尺寸的比值，如果使用浏览器窗口作为输出界面，那么aspect可以设置为window.innerWidth / window.innerHeight。
>         3. near，near用于指定从距离摄像机多近的距离开始渲染。
>         4. far，far用于指定摄像机可以看多远。
>         5. zoom，用于指定是否放大或缩小场景，当zoom设置大于1，那么将会放大场景，当zoom设置小于1，将会缩小场景，zoom等于1，不对场景进行放缩。
>
>   - 设置摄像机拍摄的位置
>     - 使用camera.lookAt(new THREE.Vector3(x,y,z))
>       - 由于new THREE.Vector3(x,y,z)可以用于表示三维空间中的一个点，所以在Three.js中，当我们需要设置物体的position时，一般都使用THREE.Vector3()。
>
> - 渲染器对象
>
>   - 渲染器对象的作用
>
>     - 渲染器对象用于完成渲染工作，它将基于摄像机对象的角度来计算如何渲染某个物体。
>
>   - 创建一个基于WebGL的渲染器
>
>     ```{ts}
>     const renderer = new THREE.WebGLRenderer();
>     renderer.setClearColor(new THREE.Color(0x000000)); //设置渲染场景时的背景颜色
>     renderer.setSize(window.innerWidth, window.innerHeight);
>     renderer.shadowMap.enabled = true; //允许阴影投影
>     ```
>     
>   - 使用渲染器完成渲染，并将结果挂载到el上
>
>     ```{ts}
>   renderer.render(scene, camera)
>     el.value.appendChild(renderer.domElement)
>   ```
>     
>
> - 常用几何体
>
>   - BoxGeometry
>
>     - BoxGeometry作用
>       - 创建一个正方体或长方体
>     
>     - 创建BoxGeometry
>     
>     ```{ts}
>     const Box = new THREE.BoxGeometry(x,y,z);
>     ```
>     
>   - PlaneGeometry
>   
>     - PlaneGeometry的作用
>   
>       - 创建一个平面。
>   
>     - 创建PlaneGeometry
>   
>       ```{ts}
>       const Plane = new THREE.PlaneGeometry(width,height);
>       ```
>   
>     - 如果使用PlaneGeometry创建一个平面作为地板
>   
>       ```{ts}
>       floor.receiveShadow = true;
>       ```
>   
>       物体的这个属性可以使其接受投影。
>   
> - 材质
>
>   - THREE.MeshLambertMaterial
>
>     - 创建方法
>
>       ```{ts}
>       new THREE.MeshLambertMaterial() //材质颜色默认为白色。
>       ```
>
>     - MeshLambertMaterial材质的作用
>
>       - MeshLambertMaterial材质用于模拟一些不发光但是可以对光源产生反应的材质（如，现实中的木材、石材）。
>       - 实际上MeshLambertMaterial材质用于模拟一些粗糙的材质，不能用于模拟光滑的且对光源有良好反射效果的材质。
>
>   - THREE.MeshBasicMaterial
>
>     - 创建方法
>
>       ```{ts}
>       new THREE.MeshBasicMaterial()
>       ```
>
>     - MeshbasicMaterial材质的作用
>
>       - 基础材质不受光照影响，但是可以在没有设置光源的时候显示出来。
>
>     - 
>
>   - 
>
> - 

# 3.理解Three.js中的几何体

> 有版本问题，https://dustinpfister.github.io/2018/05/11/threejs-face3/
>
> - 什么是几何体
>
>   - Three.js中的几何体大多是三维空间中的点集和将这些顶点连起来的面。
>
> - 创建Three.js中几何体方法（常规几何体和自定义几何体）
>
>   - 当使用Three.js库中包含的常规几何体时（如，立方体、球等），不需要自定义几何体的所有面，直接调用对应的API就可以。
>   - 当我们需要创建三维环境中的自定义几何体，那么就需要为自定义几何体提供点集（顶点）和面。
>
> - 创建自定义几何体的方法
>
>   - 创建自定义几何体有4步
>
>     1. 创造自定义几何体的点集，将所有顶点放到一个数组中。
>
>        ```{ts}
>        const vertices = [new THREE.Vector3(1,3,1),...,]
>        ```
>
>        Three.Vector3(x: number,y: number,z: number)可以创建一个三维向量，这个三维向量可以用于表示一个三维空间中的点。
>
>     2. 使用点集中的顶点获取组成几何体的三角形面，并将这些面放在一个数组中。（在125及以下版本可用）
>
>        ```{ts}
>        const faces = [new THREE.Face3(a,b,c),...,]
>        ```
>
>        Three.Face3(a: number,b: number,c: number)，可以基于vertices数组创建面，如，Three.Face3(0,1,2)表示这个面是由vertices数组中第0，1，2个元素表示的顶点组成的面。
>
>        <strong>当前Three.js版本只支持使用三角形面来渲染自定义几何体，这是由于渲染效率方面的考虑。</strong>
>
>     3. 创建一个THREE.Geometry构造函数的实例对象，将vertices数组赋值给vertices属性，将faces数组赋值给faces属性。
>
>        ```{ts}
>        const geo = new THREE.Geometry();
>        geo.vertices = vertices;
>        geo.faces = faces;
>        ```
>
>     4. 调用THREE.Geometry的实例的computeFaceNormals()方法。
>
>        ```{ts}
>        geo.computeFaceNormals();
>        ```
>
>        这一步的作用是为自定义几何体的每一个面计算出法向量，法向量是用于计算几何体面对光源时的反应。

# 3. Three.js中的物体

> - Three.js中的物体
>
>   - 如何创建Three.js中的物体
>     - 创建物体需要指定几何体和材质，几何体可以是常规几何体（这可以由Three.js提供的API创建），也可以是自定义几何体（指定点集和面集创建），而材质则决定物体的颜色等。
>
> - Mesh
>
>   - Mesh是什么
>
>     - Mesh是Three.js中最常用的物体，翻译过来就是网格。
>     - 网格是由顶点、边和面组成的物体。
>
>   - 创建Mesh的方法
>
>     1. 提供一个几何体
>     2. 提供一个或多个材质
>     3. 调用Mesh(geometry, material)构造函数。
>
>   - Mesh实例的方法
>
>     - translateX(dis: number)，沿x轴将Mesh实例平移dis距离。
>     - translateY(dis: number)，沿y轴将Mesh实例平移dis距离。
>     - translateZ(dis: number)，沿z轴将Mesh实例平移dis距离。
>
>   - Mesh实例的属性
>
>     - visible，用于设置Mesh实例是否会被渲染。（Mesh.visible = false时，Mesh实例不会被渲染）。
>
>     - position，用于设置Mesh实例相对于父对象的位置（父对象一般是场景，但是当有对象组合时，将是其他物体）。
>
>       1. 直接设置
>
>          ```{ts}
>          mesh.position.x = 10
>          mesh.position.y = 0
>          mesh.position.z = -1
>          ```
>
>       2. 使用set
>
>          ```{ts}
>          mesh.position.set(x: number,y: number,z: number)
>          ```
>
>       
>
>     - rotation，用于设置Mesh实例绕每个轴的旋转弧度（弧度制），如，Mesh.rotation.x用于设置Mesh实例绕x轴的旋转弧度。
>
>     - Scale，用于设置Mesh实例在x,y,z轴上的缩放。（scale的值大于1，将会放大，小于1将会缩小）。
>
> - 

# 4.Three.js中的光源

> - Three.js中光源的作用
>
>   - 在Three.js中，如果不设置光源，那么物体将全部不可见（除了使用基础材质或线框材质的物体）。
>
> - Three.js中不同的光源
>
>   - Three.js中有7种不同的光源
>     1. THREE.AmbientLight，用于创建一个基础光源，这种光源的颜色将会叠加到场景现有的物体的颜色上。
>     2. THREE.PointLight，用于创建一个点光源，这种光源将从空间中的一个点向所有方向发射光线。（点光源不能用于创建阴影）。
>     3. THREE.SpotLight，用于聚光光源，类似于现实中的手电筒、日光灯、台灯。（聚光光源可以用于创建阴影）。
>     4. THREE.DirectionalLight，用于创建平行光，类似于现实中的太阳光。（平行光可以用于创建阴影）。
>     5. THREE.HemisphereLight，用于模拟更自然的室外光线。（不能用于创建阴影）。
>     6. THREE.AreaLight，用于指定散发光线的平面。（不支持投影）。
>     7. THREE.LensFlare，它实际上不是一种光源，但可以为场景中的光源添加镜头光晕效果。
>
> - THREE.AmbientLight
>
>   - THREE.AmbientLight的作用
>
>     - 创建了THREE.AmbientLight之后，颜色将应用于整个场景。
>     - THREE.AmbientLight不能用于生成阴影。
>     - 可以在使用其他光源的同时使用THREE.AmbientLight，这样可以弱化阴影或给场景添加一些额外的颜色。
>
>   - 创建THREE.AmbientLight
>
>     ```{ts}
>     const ambientLight = new THREE.AmbientLight(color，intensity)
>     ```
>
>     color是颜色值，可以是16进制，也可以是使用new THREE.Color()构造函数创建的。（THREE.Color()构造函数接受16进制或RGB）。
>
>     intensity是光的强度，越接近1越强，默认值是1。
>
>   - 使用THREE.AmbientLight
>
>     ```{ts}
>     scene.add(ambientLight);
>     ```
>
>     ambientLight不需要指定位置，当将它添加到场景之后，它会自动应用到全局。
>
> - THREE.SpotLight（最常用的光源之一）
>
>   - THREE.SpotLight的作用
>
>     1. THREE.SpotLight从一点以锥形发射光线。
>     2. THREE.SpotLight可以用于创建阴影。
>     3. THREE.SpotLight是一种具有锥形效果的光源，它产生的光源有方向和角度。
>
>   - 创建THREE.SpotLight
>
>     ```{ts}
>     const spotLight = new THREE.SpotLight(color)
>     spotLight.position.set(x,y,z)
>     spotLight.lookAt(scene.position)
>     ```
>
>     color是光源的颜色。
>
>   - THREE.SpotLight实例的常用属性
>
>     1. angle: number，用于设置发出光束的角度，单位是弧度，默认值Math.PI/3。
>     2. castShadow: boolean，用于指定是否生成阴影。
>     3. color，光源颜色。
>     4. intensity，强度，默认值是1。
>     5. position，光源在场景中的位置。
>     6. visible，是否打开光源。
>
>   - 
>
> - 

# 5.Three.js中的坐标系

> - Three.js支持创建辅助坐标系
>
>   - 使用THREE.AxesHelper()
>
>     ```{typescript}
>     THREE.AxesHelper(size: number)
>     ```
>
>     size表示轴的长度。
>
>     将坐标系对象加入到scene中，就可以看到辅助坐标轴。
>
>     红色表示x轴，绿色表示y轴，蓝色表示z轴。

# 6.dat.GUI库安装、使用

> - dat.GUI的作用
>
>   - dat.GUI是一个辅助可视化交互的库，可以帮助我们实时修改Three.js的参数（如小球的大小）。
>
> - 安装和使用
>
>   - 使用npm安装
>
>     ```{shell}
>     npm i dat.gui --save
>     ```
>
>   - 使用
>
>     - 使用ESM方式导入
>
>       ```{ts}
>       import * as dat from 'dat.gui';
>       ```
>
> - 基本功能
>
>   1. 在完成导入之后，首先需要创建dat.gui对象。
>
>      ```{ts}
>      const gui = new dat.GUI()
>      ```
>
>   2. 创建一个对象，用于储存我们需要实时修改的参数
>
>      ```{ts}
>      const controls = new function(){
>        this.CubeX = 4;
>        this.CubeY = 4;
>        this.CubeZ = 4;
>      }
>      ```
>
>   3. 使用gui对象的add方法，将controls对象中的参数加入
>
>      ```{ts}
>      gui.add(controls,'CubeX',1,20)
>      ```
>
>      gui.add()函数的第一个参数是对象，第二个参数是对象的属性，第三个参数是这个属性的值的浮动下限，第四个参数是这个属性的值的浮动上限。
>
> - gui.add()方法
>
>   - input类型的控件
>
>     ```{ts}
>     gui.add(controls,'CubeX')
>     ```
>
>     gui.add后面两个参数可选，这时表示input类型。
>
>   - 滑块类型控件
>
>     ```{ts}
>     gui.add(controls,'CubeX',20,100)
>     ```
>
>     当gui.add()方法有后面2个参数的时候，表示为滑块。
>
>   - 设置步长
>
>     ```{ts}
>     gui.add(controls,'CubeX',20,100).step(1)
>     ```
>
>   - 下拉框类型控件
>
>     ```{ts}
>     gui.add(controls,'CubeX',{'short': 1, 'normal': 10,'long': 100})
>     ```
>
>     当gui.add()方法的第三个参数是一个对象时，将表现为下拉框。
>
> - onChange()方法
>
>   - onChange()方法是dat.GUI的监听方法，onChange()方法可以监听可视化控件的变动，以调用回调改动Three.js中的物体。（每次修改控件值都会触发重绘）。
>
>     ```{ts}
>     const gui = new dat.GUI()
>     const controls = new function () {
>       this.cubeX = 1
>       this.cubeY = 1
>       this.cubeZ = 1
>     }()
>     gui.add(controls, 'cubeX', 1, 10).onChange(() => {
>       cube.scale.x = controls.cubeX
>     })
>     gui.add(controls, 'cubeY', 1, 10).onChange(() => {
>       cube.scale.y = controls.cubeY
>     })
>     gui.add(controls, 'cubeZ', 1, 10).onChange(() => {
>       cube.scale.z = controls.cubeZ
>     })
>     ```
>
>     在上面的例子中，我们实时修改了立方体的长宽高。

# 7.Three.js中的摄像机控件

> - 摄像机控件作用
>
>   - Three.js默认是无法使用鼠标等实时移动摄像机的，使用摄像机控件，可以实现控制摄像机的移动。
>
> - Three.js中常用的摄像机控件
>
>   - Three.js中有5种常用的摄像机控件
>     1. 第一视角控制器，FirstPersonControls，这个控制器的行为类似于FPS游戏，使用键盘移动，使用鼠标转动。
>     2. 飞行控制器，FlyControls，飞行模拟控制器的行为是使用键盘和鼠标控制摄像机的移动。
>     3. 翻滚控制器，RollControls，翻滚控制器允许绕着z轴旋转。
>     4. 轨迹球控制器，TrackBallControls，最常用的控制器，允许使用鼠标来移动、平移和缩放场景。
>     5. 轨道控制器，OrbitControls，轨道控制器的行为是模拟轨道中的卫星，可以使用鼠标和键盘控制摄像机。
>
> - 轨迹球控制器（https://threejs.org/docs/?q=trackBall#examples/zh/controls/TrackballControls）
>
>   - 导入控制器
>
>     ```{ts}
>     import { TrackballControls } from 'three/examples/jsm/controls/TrackballControls.js'
>     ```
>
>   - 创建控制器对象
>
>     ```{ts}
>     const trackBallControls = new TrackballControls(camera,renderer.domElement)
>     ```
>
>     camera: Camera，是场景中的摄像机。
>
>     renderer.domElement: HTMLDOMElement，是用于事件监听的HTML元素。
>
>   - 控制器实例常用属性
>
>     1. panSpeed，摄像机平移的速度，默认值是0.3
>     2. rotateSpeed，摄像机旋转的速度，默认值是1.0
>     3. zoomSpeed，摄像机缩放的速度，默认值是1.2
>
>   - 控制器实例常用方法
>
>     1. update()，更新控制器，常用于动画循环中。
>
>   - 利用动画循环更新控制器实现控制摄像机
>
>     ```{ts}
>     function animate () {
>       trackBallControls.update()
>       requestAnimationFrame(animate)
>       renderer.render(scene, camera)
>     }
>     ```
>
>     **`requestAnimationFrame()`**是由浏览器提供的，它接受一个回调函数作为参数，浏览器会在重绘之前执行这个回调函数以更新动画。
>
>   - 示例
>
>     ```{ts}
>     const trackBallControls = new TrackballControls(camera, renderer.domElement)
>     trackBallControls.rotateSpeed = 1.0
>     trackBallControls.zoomSpeed = 1.0
>     trackBallControls.panSpeed = 1.0
>     
>     function animate () {
>       trackBallControls.update()
>       requestAnimationFrame(animate)
>       renderer.render(scene, camera)
>     }
>     animate()
>     ```



