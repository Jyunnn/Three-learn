# 搭配webpack

前置安裝可參考官方文件
https://webpack.js.org/guides/getting-started/


```js
npm install --save-dev webpack-dev-server
```





因為webpack 5不再使用`raw-loader`、`file-loader`和`url-loader`
都改用`Asset Modules`
https://webpack.js.org/guides/asset-modules/

如果想使用之前的`file-loader`, 須改用`asset/resource`
```
module: {
    rules: [
        {
          test: /\.glb/,
          type: 'asset/resource'
        }
    ]
},
```

把模型檔案放在`src`資料夾,並且有使用到模型就會被webpack編譯,

```js
import WorldModel from './world.glb';

// 載入其他必要threeloader ex: GLTFLoader..
// 其他threejs必要設定..

console.log( WorldModel ); // 得到一個編譯後的路徑位置

loader.load( WorldModel , function (gltf ) {
    scene.add( gltf.scene );
})
```

完成載入模型
![WorldModel](https://imgur.com/O1DgI4y)
