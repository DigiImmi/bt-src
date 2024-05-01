#### bt-src 在 releases 中

#### kkFileView 中间件需要修改 index.html 中 example.com
参考 [https://kkview.cn/zh-cn/docs/build.html](https://kkview.cn/zh-cn/docs/build.html)
```
kkFileViewDomain='yourdomain'
sed -i "s/example.com/${kkFileViewDomain}/" index.html
```

#### 

git clone https://github.com/kovacsv/Online3DViewer.git
cd Online3DViewer
npm install
npm run build_dev

将 build、libs 文件夹复制到 website

将 website 目录作为网站目录，可改名

编辑其下 index.html、embed.html，删除 ../ （即重定向build、libs目录）

复制website/index.html
在title附近合适位置添加hidden


#### GeoGebra
