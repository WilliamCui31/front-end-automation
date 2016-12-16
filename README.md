# 前端自动化-打造自动化前端项目

## 前言
> 自动化当然是相对于手动化而言的。作为一名有点追求的前端开发者，刀耕火种的年代随着 NodeJS 等工具的出现，已经一去不复返了。如果你还停留在写着冗长的HTML代码，不断重复着复制粘贴，那么你应该好好学习一下前端自动化构建工具。


## 为什么要用自动化构建工具

##### 石器时代-刀耕火种的年代：
> * 手动压缩、合并、优化静态资源文件（css,js,img,html等）
> * 手动放置并引用资源文件
> * 手动处理各个模块的添加/删除/依赖关系
> * 手动构建测试环境
> * 手动FTP传到网站上部署
> * 手动备份各个版本的差异

##### 现代化-NodeJS带来的自动化时代：
> 使用各种前端构建工具，通过简单的配置，在命令行工具 **一行命令** 自动运行


## 自动化构建工具能做什么？

#### 一个完整的开发流程应该包括：

* 本地开发环境的初始化 (yeoman)
* 第三方依赖的管理 (npm)
* 源文件编译 (webpack)
* 自动化测试 (Mocha)
* 发布到各个环境

> 而现代的开发流程，就是要使上面的各个部分都可以自动化，一个命令就可以使这些流程都自动走完，并且快速的得到错误或通过的反馈，让我们可以方便快速的修复错误和release。


## 构建工具的种类

> 当然所有构建工具的前提是 **NodeJS**

1. 前端第三方模块，包管理工具：**NPM**, **Bower**
2. 脚手架工具： **Yeoman**, **HTML5 Boilerplate**
3. 常规构建工具：**Grunt**, **Gulp**, **FIS**
4. 模块化构建工具：**Webpack**
5. 自动化测试：**Mocha**, **Jasmine**


***

#### 1.NPM
* $ npm -v
* $ npm install <Module Name> [-g] [--save-dev]
* $ npm ls -g

>安装淘宝镜像 $ npm install -g cnpm --registry=https://registry.npm.taobao.org

>这里要介绍一下package.json,文件内容大概如下：

```json

{
  "name": "hawkeye",
  "version": "1.0.0",
  "description": "react + webpack + flux for HawkEye",
  "main": "webpack.config.js",
  "scripts": {
    "dev": "webpack-dev-server --devtool eval --progress --colors --hot --inline --content-base ./dist --history-api-fallback",
    "build": "set NODE_ENV=production && webpack -p --config ./webpack.production.config.js"
  },
  "keywords": [
    "HawkEye",
    "React"
  ],
  "author": "William Cui",
  "license": "ISC",
  "dependencies": {
    "moment": "^2.15.0",
    "rc-calendar": "^7.1.0",
    "rc-time-picker": "^2.0.0",
    "react": "^15.3.1",
    "react-dom": "^15.3.1",
    "react-masonry-component": "^4.2.1",
    "react-router": "^2.7.0"
  },
  "devDependencies": {
    "autoprefixer": "^6.4.1",
    "babel-core": "^6.14.0",
    "babel-loader": "^6.2.5",
    "babel-preset-es2015": "^6.14.0",
    "babel-preset-react": "^6.11.1",
    "classnames": "^2.2.5",
    "clean-webpack-plugin": "^0.1.10",
    "css-loader": "^0.24.0",
    "file-loader": "^0.9.0",
    "flux": "^2.1.1",
    "html-webpack-plugin": "^2.22.0",
    "imports-loader": "^0.6.5",
    "keymirror": "^0.1.1",
    "open-browser-webpack-plugin": "^0.0.2",
    "postcss-loader": "^0.11.1",
    "style-loader": "^0.13.1",
    "url-loader": "^0.5.7",
    "webpack": "^1.13.2",
    "webpack-dev-server": "^1.15.0"
  }
}

```

### 2.Yeoman

* $ npm i yo -g //全局安装yeoman
* $ npm i generator-name -g //全局安装生成器
* $ yo generator //脚手架安装

### 3.Gulp
>gulp配置文件 gulpfile.js 内容:

```javascript

// 载入插件
var gulp = require('gulp'),
    less = require('gulp-less'),
    autoprefixer = require('gulp-autoprefixer'),
    cssmin = require('gulp-clean-css'),
    uglify = require('gulp-uglify'),
    imagemin = require('gulp-imagemin'),
    rename = require('gulp-rename'),
    clean = require('gulp-clean'),
    concat = require('gulp-concat'),
    cache = require('gulp-cache'),
    htmlmin = require('gulp-htmlmin');

    var browserSync = require('browser-sync').create();
    var reload = browserSync.reload;

// less
gulp.task('less', function() {
  return gulp.src('src/css/*.less')
    .pipe(less())
    .pipe(autoprefixer('last 2 version', 'safari 5', 'ie 8', 'ie 9', 'opera 12.1', 'ios 6', 'android 4'))
    .pipe(cssmin({
      advanced: true,
      compatibility: '*',
      keepBreaks: false,
      keepSpecialComments: '*'
    }))
    .pipe(gulp.dest('dist/css'))
    .pipe(reload({stream: true}));
});

// less
gulp.task('css', function() {
  return gulp.src('src/css/*.css')
    .pipe(autoprefixer('last 2 version', 'safari 5', 'ie 8', 'ie 9', 'opera 12.1', 'ios 6', 'android 4'))
    .pipe(cssmin({
      advanced: true,
      compatibility: '*',
      keepBreaks: false,
      keepSpecialComments: '*'
    }))
    .pipe(gulp.dest('dist/css'))
    .pipe(reload({stream: true}));
});

// 脚本
gulp.task('scripts', function() {
  return gulp.src('src/js/**/*.js')
    .pipe(uglify())
    .pipe(gulp.dest('dist/js'))
    .pipe(reload({stream: true}));
});

// 图片
gulp.task('images', function() {
  return gulp.src('src/img/**/*')
    .pipe(cache(imagemin({ optimizationLevel: 3, progressive: true, interlaced: true })))
    .pipe(gulp.dest('dist/img'))
    .pipe(reload({stream: true}));
});

gulp.task('pages',function(){
  return gulp.src('src/page/**/*.html')
      .pipe(htmlmin({
        removeComments: true,
        collapseWhitespace: true,
        collapseBooleanAttributes: true,
        removeEmptyAttributes: true,
        removeScriptTypeAttributes: true,
        removeStyleLinkTypeAttributes: true,
        minifyJS: true,
        minifyCSS: true
      }))
      .pipe(gulp.dest('dist/page'))
      .pipe(reload({stream: true}));
});

// 清理
gulp.task('clean',function(){
  return gulp.src(['dist/css','dist/img','dist/js','dist/page'],{ read: false })
      .pipe(clean());
})

gulp.task('serve',['clean'],function(){

  browserSync.init({
    server: {
      baseDir: './'
    }
  });

  gulp.run('less','css','scripts','images','pages');

  gulp.watch('src/css/*.less',['less']);
  gulp.watch('src/css/*.css',['css']);
  gulp.watch('src/js/**/*.js',['scripts']);
  gulp.watch('src/img/**/*.{png,jpg,gif,ico}',['images']);
  gulp.watch('src/page/**/*.html',['pages']);
});

gulp.task('default',['serve']);

```

### 4.Webpack

> webpack 配置文件 webpack.config.js 内容：

```javascript

var path = require('path');
var openBrowserPlugin = require('open-browser-webpack-plugin');
var htmlWebpackplugin = require('html-webpack-plugin');
var Dashboard = require('webpack-dashboard');
var DashboardPlugin = require('webpack-dashboard/plugin');
var dashboard = new Dashboard();

module.exports = {
	entry: {
		app: [
			'webpack/hot/dev-server',
			'webpack-dev-server/client?http://localhost:9000',
			path.resolve(__dirname, './src/app.js')
		]
	},
	output: {
		path: path.resolve(__dirname, './dist'),
		filename: '[name].[hash].js'
	},
	resolve: {
		extentions: ['', '.js', '.jsx'],
		alias: {
			'styles': path.resolve(__dirname, './assets/styles'),
			'images': path.resolve(__dirname, './assets/images')
		}
	},
	postcss: [require('autoprefixer')],
	module: {
		loaders: [{
			test: /\.jsx?$/,
			loader: 'babel',
			exclude: 'node_modules',
			query: {
				presets: ['react', 'es2015']
			}
		},{
			test: /\.css$/,
			loader: 'style!css!postcss'
		},{
			test: /\.(png|jpg|gif|eot|svg|ttf|woff)\??.*$/,
			loader: 'url?limit=25000&name=[path][name].[ext]'
		},{
			test: /masonry|imagesloaded|fizzy\-ui\-utils|desandro\-|outlayer|get\-size|doc\-ready|eventie|eventemitter/,
			loader: 'imports?define=>false&this=>window'
		}]
	},
	plugins: [
		new openBrowserPlugin({url: 'http://localhost:9000'}),
		new htmlWebpackplugin({
			template: path.resolve(__dirname, './src/index.html'),
			filename: 'index.html',
			inject: 'body'
		}),
		new DashboardPlugin(dashboard.setData)
	]
}

```
