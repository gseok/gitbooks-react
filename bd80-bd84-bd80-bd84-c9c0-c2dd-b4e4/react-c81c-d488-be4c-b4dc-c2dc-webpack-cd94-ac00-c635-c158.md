#### React 제품 빌드시 webpack 추가옵션

react로 개발하던 project \(app\)을 제품\(production\)으로 빌드시 webpack에 추가하라고 가이드 하는 옵션.

제품으로 빌드시 보통 `Uglify` 와 `OccurrenceOrder` 을 추가하면 좋다.

거기에 더해서, **`webpack`의 `DefinePlugin`에 `production` 옵션**을 추가하면 좋다.

```js
// prod config
const webpack = require('webpack');

module.exports = {
    entry: 'bundle.js',

    output: {
        path: __dirname + '/_site/assets/',
        filename: 'bundle.js'
    },

    plugins: [
        // DefinePlugin에 process env을 production으로 설정!!!
        new webpack.DefinePlugin({
            'process.env': {
                NODE_ENV: JSON.stringify('production')
            }
        }),
        new webpack.optimize.UglifyJsPlugin({
            compressor: {
                warnings: false,
            }
        }),
        new webpack.optimize.OccurrenceOrderPlugin()
    ],

    module: {
            loaders: [
                {
                    test: /\.js$/,
                    loader: 'babel-loader',
                    exclude: /node_modules/,
                    query: {
                        cacheDirectory: true,
                        presets: ['es2015', 'react']
                    }
                }
            ]
        }
};
```



해당 내용은 공식 문서에서도 가이드 하고 있다.

`webpack`의 `DefinePlugin`에 `production` 옵션을 추가하면, 개발자 도구를 사용하시 나오는 `warning` 메시지에 걸리는 부분을 제거하여, 속도\(`performance`\)을 향상시켜준다.



