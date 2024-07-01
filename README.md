---
### bt-src 在 releases 中

---
### kkFileView
参考 [https://kkview.cn/zh-cn/docs/build.html](https://kkview.cn/zh-cn/docs/build.html)  
需要修改 index.html 中 example.com
```
kkFileViewDomain='yourdomain'
sed -i "s/example.com/${kkFileViewDomain}/" index.html
```

---
### GeoGebra
参考 
+ [Apps with Toolbar: Graphing Calculator](https://dev.geogebra.org/examples/html/example-graphing.html)
  + navigation.js 修改自 [https://dev.geogebra.org/examples/html/navigation.js](https://dev.geogebra.org/examples/html/navigation.js)
   + https://fonts.googleapis.com/icon?family=Material+Icons 转为本地 Icons.css
   + 下载了 https://fonts.gstatic.com/s/materialicons/v142/flUhRq6tzZclQEJ-Vdg-IuiaDsNc.woff2
  + navigation.css 修改自 [https://dev.geogebra.org/examples/html/navigation.css](https://dev.geogebra.org/examples/html/navigation.css)
+ [Reference:GeoGebra App Parameters](https://wiki.geogebra.org/en/Reference:GeoGebra_App_Parameters)

---
### hlsjs
参考
 + [embedding-hlsjs](https://github.com/video-dev/hls.js?tab=readme-ov-file#embedding-hlsjs)  
暂不能使用

---
### epubjs-reader
参考
+ [futurepress/epubjs-reader](https://github.com/futurepress/epubjs-reader)   |   [futurepress/epub.js](https://github.com/futurepress/epub.js)
+ 预览样式 [Try it while reading Moby Dick](https://futurepress.github.io/epubjs-reader/)
```
dirTemp=$(mktemp -u)
git clone --single-branch --branch dependabot/npm_and_yarn/epubjs-0.3.92 https://github.com/futurepress/epubjs-reader.git "$dirTemp"
npm install --prefix "$dirTemp"
if [ -d "${dirTemp}/reader" ]; then
  rm -r ./epubjs-reader 2>/dev/null || true
  cp -r "${dirTemp}/reader" ./epubjs-reader
fi
```

---
### foliate-js
参考 [johnfactotum/foliate-js](https://github.com/johnfactotum/foliate-js)
```
dirTemp=$(mktemp -u)
if git clone https://github.com/johnfactotum/foliate-js.git "$dirTemp"; then
  rm -r ./foliate-js 2>/dev/null || true
  cp -r "$dirTemp" ./foliate-js
  sed -i "/\.toolbar\s\+{/,/}/ s/width: 100%;/width: 96%;/" foliate-js/reader.html
  sed -i "s/\(menu\.groups\.layout\.select\)('paginated')/\1('scrolled')/" foliate-js/reader.js
fi
```

---
### mobi-reader
参考 [sevinalucia/mobi-reader](https://github.com/sevinalucia/mobi-reader)  |  [demo](https://webbrowsertools.com/mobi-reader/pwa/build/index.html)  
chrome.js 来自 https://webbrowsertools.com/mobi-reader/pwa/chrome.js
```
dirTemp=$(mktemp -u)
if git clone https://github.com/sevinalucia/mobi-reader.git "$dirTemp"; then
  cp ./mobi-reader/chrome.js chrome.js
  rm -r ./mobi-reader 2>/dev/null || true
  cp -r "${dirTemp}/data/interface" ./mobi-reader
  sed -zi "s/\(\s\+\)config\.storage\.local = e;/&\1config\.storage\.local\.bookurl = window\.location\.hash\.substr(6);/" mobi-reader/index.js
  mv chrome.js ./mobi-reader/chrome.js
  sed -i '/<\/head>/i \    <script type="text\/javascript" src="chrome.js"><\/script>' mobi-reader/index.html
fi
```

---
### ruffle-js
参考
 + [Using Ruffle - Desktop](https://github.com/ruffle-rs/ruffle/wiki/Using-Ruffle#desktop)
 + [Using Ruffle - JavaScript API](https://github.com/ruffle-rs/ruffle/wiki/Using-Ruffle#javascript-api)
```
## ruffle版本号
ruffle_tag1=$(wget -qO- -t1 -T2 "https://api.github.com/repos/ruffle-rs/ruffle/releases" | grep "tag_name" | head -n 1 | awk -F ":" '{print $2}' | sed 's/\"//g;s/,//g;s/ //g')
ruffle_tag2=${ruffle_tag1//-/_}
ruffle_tag2=${ruffle_tag2/_/-}

#下载ruffle
if curl -fsL --output ruffle.zip https://github.com/ruffle-rs/ruffle/releases/download/${ruffle_tag1}/ruffle-${ruffle_tag2}-web-selfhosted.zip; then
  rm -rf Ruffle/rufflejs 2>/dev/null || true
  mkdir -p Ruffle/rufflejs
  unzip -o -d Ruffle/rufflejs ruffle.zip
  sed -i 's/position: relative/position: unset/' Ruffle/rufflejs/ruffle.js
fi
rm -f ruffle.zip 2>/dev/null
```
  
---
### Online3DViewer
参考
 + [Online 3D Viewer](https://3dviewer.net/)

 + [Set Up the Development Environment](https://github.com/kovacsv/Online3DViewer/wiki/Architectural-Documentation)
```
dirTemp=$(mktemp -u)
if git clone https://github.com/kovacsv/Online3DViewer.git "$dirTemp"; then
  npm install --prefix "$dirTemp"
  npm run build_dev --prefix "$dirTemp"
  cp -r "${dirTemp}/build" "${dirTemp}/website/"
  cp -r "${dirTemp}/libs" "${dirTemp}/website/"
  rm -r ./Online3DViewer 2>/dev/null || true
  cp -r "${dirTemp}/website/" ./Online3DViewer
  sed -i "s/\.\.\///" Online3DViewer/index.html Online3DViewer/embed.html
  cp Online3DViewer/index.html Online3DViewer/hidden.html
  sed -i 's/<div class="toolbar" id="toolbar"/& hidden/' Online3DViewer/hidden.html
  sed -i 's/<div class="title"/& hidden/' Online3DViewer/hidden.html
fi
```

