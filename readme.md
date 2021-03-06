#Laravel5.4 & Angular2

### package.json

```json

{
  "name": "laravel5-angular2",
  "version": "1.0.0",
  "scripts": {
    "prod": "gulp --production",
    "dev": "gulp watch",
    "postinstall": "cd resources/assets/typescript && typings install"
  },
  "devDependencies": {
    "bootstrap-sass": "^3.0.0",
    "gulp": "^3.9.1",
    "laravel-elixir": "^5.0.0",
    "laravel-elixir-webpack-ex": "0.0.5",
    "node-sass": "^3.8.0",
    "raw-loader": "^0.5.1",
    "sass-loader": "^4.0.0",
    "ts-loader": "^0.8.2",
    "typescript": "^2.0.3",
    "typescript-decorate": "^1.0.0",
    "typescript-extends": "^1.0.1",
    "typescript-metadata": "^1.0.0",
    "typescript-param": "^1.0.0",
    "typings": "^1.4.0",
    "webpack": "^1.13.1"
  },
  "dependencies": {
    "@angular/common": "~2.1.0",
    "@angular/compiler": "~2.1.0",
    "@angular/core": "~2.1.0",
    "@angular/forms": "~2.1.0",
    "@angular/http": "~2.1.0",
    "@angular/platform-browser": "~2.1.0",
    "@angular/platform-browser-dynamic": "~2.1.0",
    "@angular/router": "~3.1.0",
    "@angular/upgrade": "~2.1.0",
    
    "core-js": "^2.4.1",
    "reflect-metadata": "^0.1.8",
    "rxjs": "5.0.0-beta.12",
    "systemjs": "0.19.39",
    "zone.js": "^0.6.25",
    
    "angular-in-memory-web-api": "~0.1.5",
    "bootstrap": "^3.3.7",
    
    "underscore": "^1.8.3",
    "es7-reflect-metadata": "^1.6.0"
  }
}
```

> npm install



### gulpfile.js
```js

var elixir = require('laravel-elixir');
var webpack = require('webpack');
require('laravel-elixir-webpack-ex');

/*
 |--------------------------------------------------------------------------
 | Elixir Asset Management
 |--------------------------------------------------------------------------
 |
 | Elixir provides a clean, fluent API for defining some basic Gulp tasks
 | for your Laravel application. By default, we are compiling the Sass
 | file for our application, as well as publishing vendor resources.
 |
 */

elixir(function(mix) {
    mix.sass('app.scss');

    mix.webpack(
        {
            app: 'main.ts',
            vendor: 'vendor.ts'
        },
        {
            module: {
                loaders: [
                    {
                        test: /\.ts$/,
                        loader: 'ts-loader',
                        exclude: /node_modules/
                    },
                    {
                        test: /\.html$/,
                        loader: 'raw-loader'
                    },
                    {
                        test: /\.scss$/,
                        loaders: ["raw", "sass"]
                    }
                ]
            },
            plugins: [
                new webpack.optimize.CommonsChunkPlugin({
                    name: 'app',
                    filename: 'app.js',
                    minChunks: 5,
                    chunks: [
                        'app'
                    ]
                }),
                new webpack.optimize.CommonsChunkPlugin({
                    name: 'vendor',
                    filename: 'vendor.js',
                    minChunks: Infinity
                }),
                new webpack.ProvidePlugin({
                    '__decorate': 'typescript-decorate',
                    '__extends': 'typescript-extends',
                    '__param': 'typescript-param',
                    '__metadata': 'typescript-metadata'
                })
            ],
            resolve: {
                extensions: ['', '.js', '.ts']
            },
            debug: true,
            devtool: 'source-map'
        },
        'public/js',
        'resources/assets/typescript'
    );

    mix.version([
        'css/app.css',
        'js/app.js',
        'js/vendor.js',
        'js/all.js',
        'css/all.css'
    ]);

    mix.scripts([
        'vendor.js',
        'app.js'
    ], 'public/js/all.js', 'public/js');

    mix.styles([
        'app.css'
    ], 'public/css/all.css', 'public/css');

    mix.browserSync({
        files: [
            "public/js/*",
            "public/css/*"
        ],
        proxy: "127.0.0.1:8000"
    });

});
```


### tsconfig.json
tsconfig.json 는 Angular2 를 coding 할 때 사용하는 TypeScript의  compiler가 어떻게 JavaScript를 만들 것인가를 정의한 파일 이다.
```json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "ES5",
    "noImplicitAny": false,
    "sourceMap": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "noEmitHelpers": true,
    "removeComments": true
  },
  "exclude": [
    "node_modules",
    "vendor",
    "resources/assets/typescript/vendor.ts",
    "resources/assets/typescript/typings/index.d.ts",
    "resources/assets/typescript/typings/globals"
  ]
}

```


### typings.json
typings.json 은 TypeScript compiler가 기본적으로 인식하지 못하는 library들에 대한 추가적인 정의를 제공하는 파일이다.
```json
{
  "globalDependencies": {
    "core-js": "registry:dt/core-js#0.0.0+20160725163759",
    "jasmine": "registry:dt/jasmine#2.2.0+20160621224255",
    "node": "registry:dt/node#6.0.0+20160909174046"
  }
}
```



###systemjs.config.js 

```js


```

### Agnular2 Material UI

<https://material.angularjs.org/latest/>

