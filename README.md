### bt-src 在 releases 中

### kkFileView 中间件需要修改 index.html 中 example.com
参考 [https://kkview.cn/zh-cn/docs/build.html](https://kkview.cn/zh-cn/docs/build.html)
```
kkFileViewDomain='yourdomain'
sed -i "s/example.com/${kkFileViewDomain}/" index.html
```

