---
&nbsp;
### 軸

1. x軸 - 水平軸(橫軸)
2. y軸 - 垂直軸
3. z軸 - 縱軸

可以在畫面建立一個參考軸
```js
const axesHelper = new THREE.AxesHelper( 3 );
scene.add( axesHelper );
```

### 位移
`Object3D`中有`.position`屬性, 基於 [`Vector3`](https://threejs.org/docs/index.html?q=Mesh#api/en/math/Vector3), 是一個class
有三個基本屬性`x`, `y`, `z`
可以把物件移動到這個座標上
```js
mesh.position.x = 1
mesh.position.y = 2
mesh.position.z = 1
```

`Vector3`有`.length`的屬性,可以得到該物體到原點的長度
```js
mesh.position.z = -4;
mesh.position.y = 3;
console.log(mesh.position.length()) // 得到 5 , 直角三角形 3:4:5
```
或是獲取到某個物件的距離
```js
console.log(mesh.position.distanceTo(camera.position))
```

### 縮放
`.scale`也是基於`Vector3`, 預設情況 x, y, z都為 1
```js
mesh.scale.x = 0.9
mesh.scale.y = 0.5
mesh.scale.z = 0.4
```

### 旋轉
`.rotation`是基於[`Euler`](https://threejs.org/docs/index.html#api/en/math/Euler)
一樣有x, y, z的屬性
x軸旋轉就像控制飛機的俯仰,
y軸旋轉就像控制飛機的偏擺(左右轉),
z軸旋轉就像讓飛機翻滾
```js
mesh.rotation.x = 0.5;
mesh.rotation.y = 0.7;
```
但是要注意旋轉的順序, 建議是 y > x > z
可以使用`.reorder`來重新排序
```js
mesh.rotation.reorder('yxz')
```
