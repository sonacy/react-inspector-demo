# React Inspecotor Demo

> 参考 [react-dev-inspector](https://github.com/zthxxx/react-dev-inspector)， create-rect-app 的demo

## 步骤

1. yarn eject
2. 修改config/webpack.config.js

```js
 //1.  plugins添加 DefinePlugin
 plugins: [
      new webpack.DefinePlugin({
        'process.env.PWD': JSON.stringify(process.cwd()),
      }),
      ...
 ]

 // 2. 添加loader
 {
    test: /\.(js|mjs|jsx|ts|tsx)$/,
    include: paths.appSrc,
    use: [{
        loader: require.resolve('babel-loader'),
        options: ...,
        },{
        loader: 'react-dev-inspector/plugins/webpack/inspector-loader',
        }
    ],
},
```

3. 修改 config/webpackDevServer.config.js

```js
const { createLaunchEditorMiddleware } = require('react-dev-inspector/plugins/webpack')

// 添加before中间件
 before(app, server) {
    app.use(createLaunchEditorMiddleware())
    ...
},
```

4. 添加项目入口, 修改App.js

```jsx
import React from 'react'
import logo from './logo.svg';
import './App.css';
import { Inspector } from 'react-dev-inspector'

const InspectorWrapper = process.env.NODE_ENV === 'development'
  ? Inspector
  : React.Fragment

function App() {
  return (
    <InspectorWrapper>
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
    </InspectorWrapper>
  );
}

export default App;
```

5. yarn start, 启动项目使用