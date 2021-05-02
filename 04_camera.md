---
# Camera

### PerspectiveCamera
最常用的相機為 `PerspectiveCamera` 透視相機
[`PerspectiveCamera`](https://threejs.org/docs/#api/en/cameras/PerspectiveCamera)可以帶入4個參數
```js
const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height, 1, 100)
```
第一個參數為視場(FOV), FOV越大, 視角越廣
第二個參數為長寬比, width / height , 可以宣告一個物件來儲存這個值
```js
const sizes = {
    width: 800,
    height: 600
}
```
第三和第四個參數為`near`和`far`,為相機可以觀察到的距離, 就像玩遊戲一樣, 有時候場景(樹木)要到一定的距離才會出現

### OrthographicCamera
這個相機就沒有透視, 每個邊都是平行的
較常使用在2D場景
參數前四個分別為視錐 左、右、上、下 的位置
[視錐wiki](https://en.wikipedia.org/wiki/Viewing_frustum)
```js
const camera = new THREE.OrthographicCamera(- 1, 1, 1, - 1, 0.1, 100)
```
如果用正交顯示3D物件, 物件比例會改變, 這時必須將左右參數乘上畫面長寬比
```js
const aspectRatio = sizes.width / sizes.height
const camera = new THREE.OrthographicCamera(- 1 * aspectRatio, 1 * aspectRatio, 1, - 1, 0.1, 100)
```


# Controls 控制

可以用官方提供的控制器來移動Camera
[`OrbitControls`](https://threejs.org/docs/index.html#examples/en/controls/OrbitControls)用軌道控制為範例,需要到`node_modules`下匯入`OrbitControls.js`檔案

```js
import { OrbitControls } from '../node_modules/three/examples/jsm/controls/OrbitControls';

// 與之前的範例相同,故省略

const controls = new OrbitControls(camera, canvas);
controls.enableDamping = true; // 開啟尼阻 (可選可不選, 預設為 false)
function animate() {
	requestAnimationFrame( animate ); // js原生函式
	controls.update(); // 因為有對畫面做更新, 必須要用update()更新
	renderer.render( scene, camera );
}
animate();
```