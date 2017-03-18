##gulp-browser-sync
这个插件可以让页面`自动刷新`可谓刷新神器
kindly check the code as follows:

```javascript
var gulp=require("gulp");
var browserSync=require("browser-sync");
var sass=require("gulp-sass");
var reload=browserSync.reload;
//服务器，运行的时候顺带运行“搬运工任务”
gulp.task("server",['sass'],function(){
    browserSync.init({
		// server:{
		// 	baseDir:"./",
		// }
		proxy:"http://localhost/gulps",
		port:1871
	});
	//监听scss文件，如其发生变化，则执行“搬运工任务”
	gulp.watch("src/scss/*.scss",['sass'])
	//监听html文件，如其发生变化，则执行browser-sync的reload方法
	gulp.watch("src/*.html").on("change",reload)
});

//这个任务仅仅只是将scss注入到css文件中去
gulp.task("sass",function(){
	return gulp.src("src/scss/*.scss")
	.pipe(sass({outputStyle:"compact"}))
	.pipe(gulp.dest("src/css"))
	.pipe(reload({stream:true}))
});
//自动运行服务器
gulp.task("default",["server"]);
