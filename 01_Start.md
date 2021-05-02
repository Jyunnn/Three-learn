---
# 開始
`Three.js`是基於`WebGL`的JS函式庫
`WebGL`使用上比較不好上手, 所以就出現了`Three.js`來簡化這些工作

# 基礎

要在網頁在呈現出`Three.js`渲染出的效果,須具備4個物件
1. scene 場景 - 想像為一個網頁容器,可以裝入 objects
2. objects 物件 - 可以是一個3D模型,粒子或是燈光
3. camera 相機 - 決定畫面觀看的視角
4. renderer 渲染器 - 將算出來的圖呈現出來

### Scene 場景---
# 開始
`Three.js`是基於`WebGL`的JS函式庫
`WebGL`使用上比較不好上手, 所以就出現了`Three.js`來簡化這些工作

# 基礎

要在網頁在呈現出`Three.js`渲染出的效果,須具備4個物件
1. scene 場景 - 想像為一個網頁容器,可以裝入 objects
2. objects 物件 - 可以是一個3D模型,粒子或是燈光
3. camera 相機 - 決定畫面觀看的視角
4. renderer 渲染器 - 將算出來的圖呈現出來

### Scene 場景

直接創建一個 `Scene`
```js
const scene = new THREE.Scene()
```




### Objects 物件

模型 粒子 燈光都是屬於物件,
用一個正方體為範例, 需要創建一個`Mesh`(網格)
在3D建模軟體中建立的任何模型都是一個`Mesh`
他是一個用很多`Vertex`(頂點)組成的
兩個`Vertex`就是一個`Edge`(邊),三個`Vertex`就是一個`Face`(面)

在`Three.js`中`Mesh`是個class
https://threejs.org/docs/index.html?q=Mesh#api/en/objects/Mesh
可以帶入兩個建構子
1. `Geometry`(幾何體) - 可以是正方體, 球體, 圓柱體..等
2. `Material`(材質) - 就是在建模軟體介面的材質球, 決定顏色, 反射度, 透明度...等等

```js
const geometry = new THREE.BoxGeometry(1, 1, 1) // 建立一個尺寸為1的正方體
const material = new THREE.MeshBasicMaterial({ color: 0xff0000 }) // 材質為 紅色
const mesh = new THREE.Mesh(geometry, material) // 組裝起來就是一個 尺寸為1的紅色正方體
```

然後再放進剛剛的`scene`
```js
scene.add(mesh)
```

### Camera 相機
這個物件是一個不顯示在畫面上的, 他是一個觀察角度
渲染出來的視角就是由`Camera`所決定的
用`PerspectiveCamera`(透視)為例子
https://threejs.org/docs/index.html#api/en/cameras/PerspectiveCamera
可以帶入FOV,長寬比
當然建立完後也是要加入到`Scene`
```js
const camera = new THREE.PerspectiveCamera(100, 800 / 600);
scene.add(camera);
```

因為目前相機建立的位置在原點
這樣是看不到任何東西
必須在加入Scene前改變位置
```js
const camera = new THREE.PerspectiveCamera(100, 800 / 600);
camera.position.z = 3
scene.add(camera);
```

也可以直接用lookAt對準物件
```js
camera.lookAt(mesh.position)
```


### Renderer 渲染器
將算出來的圖渲染到網頁上
```html
<canvas class="demo"></canvas>
```
在網頁建立一個`canvas`

```js
// 選擇一個canvas
const canvas = document.querySelector('canvas.demo')

const renderer = new THREE.WebGLRenderer({
    canvas: canvas
})
// 設定canvas尺寸
renderer.setSize(800, 600)
```
選擇剛剛建立的`canvas`
指定給渲染器再給予長寬

```js
renderer.render(scene, camera)
```
最後將`Scene`和`Camera`傳入函式中
https://threejs.org/docs/index.html?q=render#api/en/renderers/WebGLRenderer.render

畫面就會出現一個紅色的正方形
改變Camera的角度和位置
就可以觀察到該物體是正方體

渲染的尺寸可以與視窗大小一起縮放
```js
const sizes = {
    width: window.innerWidth,
    height: window.innerHeight
}

window.addEventListener('resize', () =>{
    // Update sizes
    sizes.width = window.innerWidth
    sizes.height = window.innerHeight
    camera.aspect = sizes.width / sizes.height // 還要更新Camera長寬比
    camera.updateProjectionMatrix() // 並且套用
    renderer.setSize(sizes.width, sizes.height);
});
```

你可能發現有卷軸跑出來,
加入css
```css
.demo {
    position: fixed;
    top: 0;
    left: 0;
}
```




### 補充: 群組 Group
我可以將物件集合再一起,變成一個`Group`(群組)
`Group`也是基於 `Object3D`
所以可以使用`Object3D`的每個屬性
```js
const group = new THREE.Group() // 建立群組
group.scale.y = 2 // 群組縮放
scene.add(group) // 加入場景

const mesh1 = ...
group.add(mesh1) // 加到群組
const mesh2 = ...
group.add(mesh2) // 加到群組
```


# 官方文件
`Three.js`絕大部分的物件都是由class建立
以`Mesh`為例子
https://threejs.org/docs/index.html?q=Mesh#api/en/objects/Mesh
Properties 基於`Object3D`, 所以可以用`Object3D`裡的屬性
`Object3D`有個`.rotation`屬性可以使用
https://threejs.org/docs/index.html?q=Mesh#api/en/core/Object3D.rotation

但是`.rotation`後面有一個`Euler`
就代表`.rotation`是可以用`Euler`裡的屬性
https://threejs.org/docs/index.html?q=Mesh#api/en/math/Euler
可以使用`.x`,`.y`,`.set`這些屬性

結合上面這些屬性,就可以做出
```js
mesh.rotation.set(1,1,3);
```


直接創建一個 `Scene`
```js
const scene = new THREE.Scene()
```




### Objects 物件

模型 粒子 燈光都是屬於物件,
用一個正方體為範例, 需要創建一個`Mesh`(網格)
在3D建模軟體中建立的任何模型都是一個`Mesh`
他是一個用很多`Vertex`(頂點)組成的
兩個`Vertex`就是一個`Edge`(邊),三個`Vertex`就是一個`Face`(面)

在`Three.js`中`Mesh`是個class
https://threejs.org/docs/index.html?q=Mesh#api/en/objects/Mesh
可以帶入兩個建構子
1. `Geometry`(幾何體) - 可以是正方體, 球體, 圓柱體..等
2. `Material`(材質) - 就是在建模軟體介面的材質球, 決定顏色, 反射度, 透明度...等等

```js
const geometry = new THREE.BoxGeometry(1, 1, 1) // 建立一個尺寸為1的正方體
const material = new THREE.MeshBasicMaterial({ color: 0xff0000 }) // 材質為 紅色
const mesh = new THREE.Mesh(geometry, material) // 組裝起來就是一個 尺寸為1的紅色正方體
```

然後再放進剛剛的`scene`
```js
scene.add(mesh)
```

### Camera 相機
這個物件是一個不顯示在畫面上的, 他是一個觀察角度
渲染出來的視角就是由`Camera`所決定的
用`PerspectiveCamera`(透視)為例子
https://threejs.org/docs/index.html#api/en/cameras/PerspectiveCamera
可以帶入FOV,長寬比
當然建立完後也是要加入到`Scene`
```js
const camera = new THREE.PerspectiveCamera(100, 800 / 600);
scene.add(camera);
```

因為目前相機建立的位置在原點
這樣是看不到任何東西
必須在加入Scene前改變位置
```js
const camera = new THREE.PerspectiveCamera(100, 800 / 600);
camera.position.z = 3
scene.add(camera);
```

也可以直接用lookAt對準物件
```js
camera.lookAt(mesh.position)
```


### Renderer 渲染器
將算出來的圖渲染到網頁上
```html
<canvas class="demo"></canvas>
```
在網頁建立一個`canvas`

```js
// 選擇一個canvas
const canvas = document.querySelector('canvas.demo')

const renderer = new THREE.WebGLRenderer({
    canvas: canvas
})
// 設定canvas尺寸
renderer.setSize(800, 600)
```
選擇剛剛建立的`canvas`
指定給渲染器再給予長寬

```js
renderer.render(scene, camera)
```
最後將`Scene`和`Camera`傳入函式中
https://threejs.org/docs/index.html?q=render#api/en/renderers/WebGLRenderer.render

畫面就會出現一個紅色的正方形
改變Camera的角度和位置
就可以觀察到該物體是正方體

渲染的尺寸可以與視窗大小一起縮放
```js
const sizes = {
    width: window.innerWidth,
    height: window.innerHeight
}

window.addEventListener('resize', () =>{
    // Update sizes
    sizes.width = window.innerWidth
    sizes.height = window.innerHeight
    camera.aspect = sizes.width / sizes.height // 還要更新Camera長寬比
    camera.updateProjectionMatrix() // 並且套用
    renderer.setSize(sizes.width, sizes.height);
});
```

你可能發現有卷軸跑出來,
加入css
```css
.demo   /*你的 canvas class*/
{
    position: fixed;
    top: 0;
    left: 0;
}
```




### 補充: 群組 Group
我可以將物件集合再一起,變成一個`Group`(群組)
`Group`也是基於 `Object3D`
所以可以使用`Object3D`的每個屬性
```js
const group = new THREE.Group() // 建立群組
group.scale.y = 2 // 群組縮放
scene.add(group) // 加入場景

const mesh1 = ...
group.add(mesh1) // 加到群組
const mesh2 = ...
group.add(mesh2) // 加到群組
```

### 測試介面
[`dat.GUI`](https://github.com/dataarts/dat.gui)
官網裡的範例幾乎都是用這個工具, 詳細使用方式可以參考他的API文件
```js
import * as dat from 'dat.gui';
const gui = new dat.GUI();
```



### 官方文件
`Three.js`絕大部分的物件都是由class建立
以`Mesh`為例子
https://threejs.org/docs/index.html?q=Mesh#api/en/objects/Mesh
Properties 基於`Object3D`, 所以可以用`Object3D`裡的屬性
`Object3D`有個`.rotation`屬性可以使用
https://threejs.org/docs/index.html?q=Mesh#api/en/core/Object3D.rotation

但是`.rotation`後面有一個`Euler`
就代表`.rotation`是可以用`Euler`裡的屬性
https://threejs.org/docs/index.html?q=Mesh#api/en/math/Euler
可以使用`.x`,`.y`,`.set`這些屬性

結合上面這些屬性,就可以做出
```js
mesh.rotation.set(1,1,3);
```
