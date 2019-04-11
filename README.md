# scrollable-canvas-test

Thanks to: https://konvajs.org/docs/sandbox/Canvas_Scrolling.html

OS とブラウザ、ディスプレイによって、スクロールイベントを待たずにスクロールする場合がある。
この場合、スクロール位置に関係なく同じ位置に描画する要素があるとガタガタとずれて表示されてしまう。

下記は、スクロールイベントを待つ環境。

- macOS 10.14 + Chrome 73 + 非 Retina ディスプレイ ([動画](mov/mac-chrome-1x.mov))
- Windows 10 + Chrome 73

下記は、待たない環境。

- macOS 10.14 + Chrome 73 + Retina ディスプレイ ([動画](mov/mac-chrome-2x.mov))
- macOS 10.14 + Firefox 66
- macOS 10.14 + Safari 12
- Windows 10 + Firefox 66
- Windows 10 + Edge 44


## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn run serve
```

### Compiles and minifies for production
```
yarn run build
```

### Run your tests
```
yarn run test
```

### Lints and fixes files
```
yarn run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
