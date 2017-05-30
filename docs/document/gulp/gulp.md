## 前端构建工具

## gulp常用插件
### gulp-clean
- 清除文件
### gulp-html
- 处理html
### gulp-concat
- 合并代码
### gulp-connect
- 启动服务
### gulp-cssmin
- 压缩css
### gulp-less
- 将less解析成css
### gulp-imagemin
- 压缩图片
### gulp-load-plugins
- 可以使用`$.less()`的方式加载gulp插件
### gulp-uglify
- 压缩js代码
### gulp-plumber
- 该插件可以组织gulp插件发生错误导致进程退出, 并输出错误日志

示例gulpfile.js
```js
var gulp = require('gulp');
var $ = require('gulp-load-plugins')();// 方便下面用到gulp插件的时候使用$.插件名()即可

var open = require('open');

var app = {
    srcPath: 'src/',
    devPath: 'build/',
    prdPath: 'dist/'
};

// 拷贝库
gulp.task('lib', function () {
    gulp.src('bower_components/**/*.js')
        .pipe(gulp.dest(app.devPath + 'vendor'))
        .pipe(gulp.dest(app.prdPath + 'vendor'))
        .pipe($.connect.reload());
});

gulp.task('html', function () {
    gulp.src(app.srcPath + '**/*.html')
        .pipe(gulp.dest(app.devPath))
        .pipe(gulp.dest(app.prdPath))
        .pipe($.connect.reload());
});

// 处理json
gulp.task('json', function () {
    gulp.src(app.srcPath + 'data/**/*.json')
        .pipe(gulp.dest(app.devPath + 'data'))
        .pipe(gulp.dest(app.prdPath + 'data'))
        .pipe($.connect.reload());
});

// 编译压缩less
gulp.task('less', function () {
    gulp.src(app.srcPath + 'style/index.less')
        .pipe($.less())
        .pipe(gulp.dest(app.devPath + 'css'))
        .pipe($.cssmin())
        .pipe(gulp.dest(app.prdPath + 'css'))
        .pipe($.connect.reload());
});

// 压缩合并js
gulp.task('js', function () {
    gulp.src(app.srcPath + 'script/**/*.js')
        .pipe($.concat('index.js'))
        .pipe(gulp.dest(app.devPath + 'js'))
        .pipe($.uglify())
        .pipe(gulp.dest(app.prdPath + 'js'))
        .pipe($.connect.reload());
});

// 压缩图片
gulp.task('image', function () {
    gulp.src(app.srcPath + 'image/**/*')
        .pipe(gulp.dest(app.devPath + 'image'))
        .pipe($.imagemin())
        .pipe(gulp.dest(app.prdPath + 'image'))
        .pipe($.connect.reload());
});

// 编译打包
gulp.task('build',['lib','html','json','less','js','image'],function(){
    console.log('打包完成...');
});

// 清除原来的文件
gulp.task('clean',function(){
    gulp.src([app.prdPath,app.devPath])
    .pipe($.clean());
});

// 启动一个服务
gulp.task('server',['build'],function(){
    $.connect.server({
        root:[app.devPath],// 根路径
        livereload:true,
        port:1234
    });
    open('http://localhost:1234');

    // 对文件的改动进行监听
    // 当指定类型的文件发生变化的时候, 重新启动指定的任务
    gulp.watch('bower_components/**/*.js',['lib']);
    gulp.watch(app.srcPath+'**/*.html',['html']);
    gulp.watch(app.srcPath+'data/**/*.json',['json']);
    gulp.watch(app.srcPath+'style/**/*.less',['less']);
    gulp.watch(app.srcPath+'script/**/*.js',['js']);
    gulp.watch(app.srcPath+'image/**/*',['image']);
})

// 指定gulp默认的任务, 这样直接使用gulp的时候就默认是gulp server
gulp.task('default',['server']);
```

## 处理命令行参数的包


```bash
npm install yargs
```
如
```bash
# production 就是命令的选项部分
gulp -production
```