# K6 Installation and configuration

## Install K6:

```bash
 brew install k6
```

## Configure To Use With NPM

### 1. create package.json

```bash
npm init
```

### 2. install webpack

```bash
yarn add webpack webpack-cli -D
```

### 3. install babel and presets

```bash
yarn add @babel/core @babel/preset-env babel-loader -D
```

### 4. install k6

```bash
yarn add k6
```

### 5. install core-js

```bash
yarn add core-js -D
```

### 6. create .babelrc

```json
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "useBuiltIns": "usage",
        "corejs": 3
      }
    ]
  ]
}
```

### 7. install dotenv-webpack

```bash
yarn add dotenv-webpack -D
```

### 8. set project structure:

```
project/
| src/
| | tests/
| | | test-1.js
| .babelrc
| .env
| package.json
| webpack.config.js
```

### 9. create webpack.config.js

```javascript
const Dotenv = require("dotenv-webpack");
module.exports = {
  mode: "production",
  entry: {
    test1: "./src/tests/test-1.js",
  },
  output: {
    path: __dirname + "/dist",
    filename: "test.[name].js",
    libraryTarget: "commonjs",
  },
  module: {
    rules: [{ test: /\.js$/, use: "babel-loader" }],
  },
  stats: {
    colors: true,
    warnings: false,
  },
  target: "web",
  externals: /k6(\/.*)?/,
  devtool: "source-map",
  plugins: [new Dotenv()],
};
```

### 10. add scripts to package.json

```json
"scripts": {
  "pretest": "webpack",
  "test:test1": "k6 run ./dist/test.test1.js",
  "test": "npm run test:test1"
}
```
