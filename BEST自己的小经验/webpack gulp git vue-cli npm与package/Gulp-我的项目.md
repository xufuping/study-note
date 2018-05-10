gulp-转码项目中的ES6-ES5并压缩js
---
在这之前，请确保你的 `node` 和 `npm` 已经安装无误，可以用`node -v` 和 `npm -v`查看 `node` 和 `npm` 的版本。

好了，接下来，gulp-babel，转码压缩，七步到位，走你！

##### 1. 全局安装 gulp
终端执行 `cnpm install gulp -g`。(这里默认大家已经安装了[淘宝镜像](http://npm.taobao.org/)，没有安装也可以使用 `npm`)。

安装完毕后可以使用 `gulp -v`检测版本。

##### 2. 在项目中安装 babel-cli
在项目根目录执行 `cnpm install babel-cli --save -dev`，等待安装好项目所依赖的`babel`。

安装完毕后可以使用 `.\node_modules\.bin\babel --version` 检测版本。这时候你可以在你的项目中看到一个node_modules文件夹。

##### 3. 在项目中安装 gulp
在项目根目录执行 `cnpm install gulp --save -dev`，安装项目中的 `gulp`。

##### 4. 在项目中安装 gulp-babel
在项目根目录执行 `cnpm install --save-dev gulp-babel babel-preset-env`。

安装完毕后，你可以看到你的项目出现了 `package.json`(如果你没有预先 `npm init` 或者安装依赖的话一般是不会出现这个文件的)，你可以检测文件里会出现了以下依赖：

    {
      "devDependencies": {
        "babel-preset-env": "^1.6.1",
        "gulp-babel": "^7.0.0"
      }
    }

##### 5. 在项目中安装 gulp-uglify
在项目根目录执行 `cnpm install gulp-uglify --save-dev`, 于是 `package.json` 变成了这样：

    {
      "devDependencies": {
        "babel-preset-env": "^1.6.1",
        "gulp-babel": "^7.0.0",
        "gulp-uglify": "^3.0.0"
      }
    }
    
##### 6.根目录创建 gulpfile.js
在项目根目录创建 `gulpfile.js`，里面的代码是这样的：

    const gulp = require('gulp'); // 引入gulp
    const babel = require('gulp-babel'); // 引入gulp-babel
    const uglify = require('gulp-uglify'); // 引入gulp-uglify
    
    // ES6代码转码为ES5
    gulp.task('toes5', () =>
        gulp.src('src/*.js') // 需要转码的es6文件，这里代表src文件夹下的所有js文件
            /*
    		最新的presets: ['env']，是通用版，可以转码es2015、react、es2017, 
    		这里只用于转码es2015，你可以下载相关转码依赖
            */
            .pipe(babel({
                presets: ['env']
            }))
            .pipe(gulp.dest('dist')) // 转码后的文件输出位置
    );
    
    // JS代码压缩
    gulp.task('jsmin', () =>
        gulp.src('dist/*.js') // 需要压缩的js文件
            .pipe(uglify())
            .pipe(gulp.dest('min')) // 压缩的js文件输出位置
    );
    
    // 使用监听，但是需要预先使用命令 gulp auto，文件如果修改了，就一步到位
    gulp.task('auto', () => {
    gulp.watch('src/*.js', ['toes5']) // 需要转码的js文件位置 + 口令
    gulp.watch('dist/*.js', ['jsmin']) // 需要压缩的js文件位置 + 口令
});

##### 7. 流水线的骚操作
在项目根目录终端运行 `gulp toes5`，就将 `es6` 文件转为 `es5` 文件了。运行 `gulp jsmin`，就将 `js` 文件稳稳当当地压缩了。

如果我们需要一步到位，而且不用随时去执行这繁琐的命令，我们还可以使用监听。在项目根目录终端使用 `gulp auto`，然后每次修改开发文件( `src` 下的 `js` )，就会自动同步转码和压缩。【==监听还可以监听更多的 gulp task，这点非常重要==】

附录
----
##### 8. gulp-htmlmin 压缩html
`cnpm install gulp-htmlmin --save-dev`

    const htmlmin = require('gulp-htmlmin');
    
    // 压缩html和html页面内的css/js
    gulp.task('htmlmin', function () {
        var options = {
            removeComments: true,//清除HTML注释
            collapseWhitespace: true,//压缩HTML
            collapseBooleanAttributes: true,//省略布尔属性的值 <input checked="true"/> ==> <input />
            removeEmptyAttributes: true,//删除所有空格作属性值 <input id="" /> ==> <input />
            removeScriptTypeAttributes: true,//删除<script>的type="text/javascript"
            removeStyleLinkTypeAttributes: true,//删除<style>和<link>的type="text/css"
            minifyJS: true,//压缩页面JS
            minifyCSS: true//压缩页面CSS
        };
        gulp.src('src/*.html')
            .pipe(htmlmin(options))
            .pipe(gulp.dest('min'));
    });

##### 9. gulp-clean-css 压缩css
`cnpm install gulp-clean-css --save-dev`

    
    const cleanCSS = require('gulp-clean-css');

    // 压缩css文件, 如果有外部引用，为了保持项目结构，需要使用插件 gulp-sourcemaps
    gulp.task('cssmin', () => {
        gulp.src('src/*.css')
            .pipe(cleanCSS({compatibility: 'ie8'})) // 兼容IE8及以下浏览器
            .pipe(gulp.dest('min'));
    });

##### 10. gulp-concat 合并 js 文件
`cnpm install gulp-concat --save-dev`

    const concat = require('gulp-concat');
    
    // 合并 js 文件, 但不会转码和压缩
    gulp.task('jsconcat', function () {
        gulp.src('src/*.js')
            .pipe(concat('index.js'))//合并后的文件名
            .pipe(gulp.dest('dist'));
    });
    
##### 11. gulp-less 将less文件编译成css
`cnpm install gulp-less --save-dev`

    const less = require('gulp-less');
    
    // 编译less文件，如果有外部引用，为了保持项目结构，需要使用插件 gulp-sourcemaps
    gulp.task('lesstocss', function () {
        //编译src目录下的所有less文件
        //除了reset.less和test.less（**匹配src/less的0个或多个子文件夹）
        gulp.src(['src/*.less', '!src/less/**/{reset,test}.less']) 
            .pipe(less())
            // 编译后压缩css文件输出到min
            .pipe(cleanCSS({compatibility: 'ie8'})) // 兼容IE8及以下浏览器
            .pipe(gulp.dest('min'));
    });

##### 12. gulp-autoprefixer 给 css 项处理浏览器前缀
`cnpm install --save-dev gulp-autoprefixer`

    const autoprefixer = require('gulp-autoprefixer');
    
    // 自动补齐css前缀
    gulp.task('autoprefixer', () => {
        gulp.src('src/index.css')
            .pipe(autoprefixer({
                browsers: ['last 2 versions'], // 主流浏览器的最新两个版本
                cascade: true, // 是否美化属性值 默认：true
                remove:true // 是否去掉不必要的前缀 默认：true 
            }))
            .pipe(gulp.dest('dist'))
    });
    
    
    
    // 需要适配的页面
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>
    <body>
        @@include('include/header.html')
    
        <p> 这是 layout 的内容 </p>
    
        @@include('include/footer.html')
    </body>
    </html>

##### 13. gulp-file-include 头尾共用、文件合并
`cnpm install gulp-file-include --save-dev`

    const fileinclude  = require('gulp-file-include');

    // 头尾共用
    gulp.task('fileinclude', () => {
        // 适配page中所有文件夹下的所有html，排除page下的include文件夹中html
            gulp.src(['src/*.html','!src/include/**.html'])
            .pipe(fileinclude({
              prefix: '@@',
              basepath: '@file'
            }))
        .pipe(gulp.dest('dist'))
    });

##### 14. gulp-livereload 监听文件变化局部刷新

##### 15. gulp-imagemin 压缩图片

示例
---

    const gulp = require('gulp');
    const babel = require('gulp-babel');
    const uglify = require('gulp-uglify');
    const htmlmin = require('gulp-htmlmin');
    const cleanCSS = require('gulp-clean-css');
    const concat = require('gulp-concat');
    const less = require('gulp-less');
    const autoprefixer = require('gulp-autoprefixer');
    const fileinclude  = require('gulp-file-include');
    
    // 头尾共用
    gulp.task('fileinclude', () => {
        // 适配page中所有文件夹下的所有html，排除page下的include文件夹中html
            gulp.src(['src/*.html','!src/include/**.html'])
            .pipe(fileinclude({
              prefix: '@@',
              basepath: '@file'
            }))
        .pipe(gulp.dest('dist'))
    });
    
    // 压缩html和html页面内的css/js
    gulp.task('htmlmin', function () {
        var options = {
            removeComments: true,//清除HTML注释
            collapseWhitespace: true,//压缩HTML
            collapseBooleanAttributes: true,//省略布尔属性的值 <input checked="true"/> ==> <input />
            removeEmptyAttributes: true,//删除所有空格作属性值 <input id="" /> ==> <input />
            removeScriptTypeAttributes: true,//删除<script>的type="text/javascript"
            removeStyleLinkTypeAttributes: true,//删除<style>和<link>的type="text/css"
            minifyJS: true,//压缩页面JS
            minifyCSS: true//压缩页面CSS
        };
        gulp.src('dist/*.html')
            .pipe(htmlmin(options))
            .pipe(gulp.dest('min'));
    });
    
    // 编译less文件，如果有外部引用，为了保持项目结构，需要使用插件 gulp-sourcemaps
    gulp.task('lesstocss', function () {
        //编译src目录下的所有less文件
        //除了reset.less和test.less（**匹配src的0个或多个子文件夹）
        gulp.src(['src/*.less', '!src/**/{reset,test}.less']) 
            .pipe(less())
            // 编译后自动补齐css前缀
            .pipe(autoprefixer({
                browsers: ['last 2 versions'], // 主流浏览器的最新两个版本
                cascade: true, // 是否美化属性值 默认：true
                remove:true // 是否去掉不必要的前缀 默认：true 
            }))
            // 然后压缩css文件输出到min
            .pipe(cleanCSS({compatibility: 'ie8'})) // 兼容IE8及以下浏览器
            .pipe(gulp.dest('min'));
    });
    
    // ES6代码转码为ES5
    gulp.task('toes5', () =>
        gulp.src('src/*.js') // 需要转码的es6文件，这里代表src文件夹下的所有js文件
            /*
    		最新的presets: ['env']，是通用版，可以转码es2015、react、es2017, 
    		这里只用于转码es2015，你可以下载相关转码依赖
            */
            .pipe(babel({
                presets: ['env']
            }))
            .pipe(gulp.dest('dist')) // 转码后的文件输出位置
    );
    
    // 合并 js 文件, 但不会转码和压缩
    gulp.task('jsconcat', () => {
        gulp.src('src/*.js')
            .pipe(concat('index.js')) // 合并后的文件名
            .pipe(gulp.dest('dist'))
    });
    
    // JS代码压缩
    gulp.task('jsmin', () =>
        gulp.src('dist/*.js') // 需要压缩的js文件
            .pipe(uglify())
            .pipe(gulp.dest('min')) // 压缩的js文件输出位置
    );
    
    
    
    // // 使用监听，但是需要预先使用命令 gulp auto，文件如果修改了，就一步到位
    // gulp.task('auto', () => {
    //     gulp.watch('src/*.js', ['toes5']) // 需要转码的js文件位置 + 口令
    //     gulp.watch('dist/*.js', ['jsmin']) // 需要压缩的js文件位置 + 口令
    //     // gulp.watch...
    // });