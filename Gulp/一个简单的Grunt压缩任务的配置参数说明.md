以为一个项目的原因，使用Grunt（这货用起来感觉币Gulp别扭多了，也可能是先入为主的感觉，毕竟Gulp是根据Grunt来该写的，出来的晚，而且用了node是stream操作，所以更舒服和快捷高效一些）

和Gulp一样，需要创建配置文件

必须`Gruntfile.js` 这个需要手动创建；

演示代码如下：

```javascript
module.exports=function (grunt) {
    grunt.initConfig({
        pkg:grunt.file.readJSON('package.json'),
        // 这里是uglify任务的配置信息
        uglify:{
          options:{
            banner:'/* <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
          },
            static_mappings: {
                // 由于这里的 src压缩到dest文件映射时手动一个一个指定的，比较蛋疼；适合改动不多的；
                files: [
                    {src: 'lib/a.js', dest: 'build/a.min.js'},
                    {src: 'lib/b.js', dest: 'build/b.min.js'},
                    {src: 'lib/subdir/c.js', dest: 'build/subdir/c.min.js'},
                    {src: 'lib/subdir/d.js', dest: 'build/subdir/d.min.js'},
                ],
            },
            dynamic_mappings:{
              //这里就不需要一个一个的配置了
              files:[
                  {
                      expand: true,     // expand 设置为true用于启用下面的选项：
                      cwd: 'src/',      // 所有src指定的匹配都将相对于此处指定的路径（但不包括此路径） 。
                      src: ['**/*.js'], // 相对于cwd路径的匹配模式。
                      dest:'build/',    //目标文件路径前缀。
                      ext: '.min.js',   //对于生成的dest路径中所有实际存在文件，均使用这个属性值替换扩展名。
                      extDot: 'first',  //用于指定标记扩展名的英文点号的所在位置。可以赋值 'first' （扩展名从文件名中的第一个英文点号开始） 或 'last' （扩展名从最后一个英文点号开始），默认值为 'first'
                      // flatten:true,     //从生成的dest路径中移除所有的子路径部分，注：已经有的路径不删除，不营销静态配置里的二级目录。
                      rename: function (dest, src) {
                          return dest + src.replace(/testFlatten\/hello4/, '/rename/hello44444');
                      }
                  }
              ]
          },
        }
 
    });
    //
    grunt.loadNpmTasks('grunt-contrib-uglify');
    grunt.registerTask('default',['uglify']);
};

```