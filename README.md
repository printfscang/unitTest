### 功能实现流程
1. 创建父类PraiseButton，使用ES6完成点赞+1功能
2. 开发子类Thumb实现点赞
3. 使用babel 编译ES6代码，并使用System.js加载编译后的文件
4. 将编译后的文件挂载为jQuery的组件
5. 使用karma进行单元测试，使用karma-coverage进行覆盖率检查
----------
### 一、es6父类与子类的实现代码
```
class PraiseButton {
    constructor(num, element) {  //构造函数
        this.num = num;
        this.element = element;
    }
    clickAction(){      //点击方法
        this.element.click(()=>{
           if(this.num<5){
               this.element.css('-webkit-filter','grayscale(0)');
               $('#animation').addClass('num');
               this.num=add(this.num);
               setTimeout(function(){
                $('#animation').removeClass('num');
               },1000);
           }else{
            this.element.css('-webkit-filter','grayscale(1)');
            this.num=0;
           }
           console.log(this.num);
        })
    }
}
class Thumb extends PraiseButton{    //子类继承父类
    constructor(num,element){         //构造方法
    super(num,element);                //super方法，当类使用继承，并实例化时， es5 的继承是先创建子类的this，然后将父类的方法添加到子类的this上去； es6 的继承是创建父类的this对象，然后再对this对象添加方法/属性。 而super方法就是用来创建父类this对象的。 
    }
}
export default Thumb      //export 不仅能导出变量，还能导出函数和类，关于es6的export和import使用百度自查
```
### 二、使用babel编译es6代码
1. 进入项目目录，npm install --save-dev babel-cli babel-preset-env
2. 在项目中创建.babelrc文件，输入一下内容 ` {"presets":["env"]} `
3. 使用命令 ` babel filename.es6 -o filename.js  `将es6文件编译为js文件

### 三、使用System.js加载编译后的文件
##### Systemjs是一个可配置模块加载器，为浏览器和NodeJs启用动态的Es模板加载器。
##### 加载ES6     
app/es6-file.js:
```
export class q {
    constructor() {
      this.es6 = 'yay';
    }
  }
<script>
  SystemJS.import('./app/es6-file.js').then(function(m) {
    console.log(new m.q().es6); // yay
  });
</script>
```
### 四、挂载为jQuery组件
```
 SystemJS.import('index.js').then(function (m) {
            console.log(m); 
            console.log(m.default);
            $.extend({          //该方法是将thumb合并到Jquery的全局对象中去，相当于为Jquery全局对象添加了一个静态方法
                thumb:m.default   //挂载到m.default中
            })
            callBack();    //设置一个回调函数，实现点击的效果
        });
        function callBack(){     
            var test=new $.thumb(0,$('#thumb'));
            test.clickAction();
        }
```
### 五、karma自动化单元测试
包安装（如果本地安装失败，就全部安装在全局）   
1. npm install karma --save-dev
2. npm install karma-cli --save-dev
3. npm install karma-jasmine jasmine-core --save-dev
4. npm install karma-phantomjs-launcher --save-dev
5. npm install phantom --save-dev
##### 安装完之后输入命令 `karma init` ，生成karma.config.js配置文件
##### 新建*.spec.js断言文件   
输入命令karma start进行测试

### 六、karma-coverage代码覆盖率检测
安装 `karma-coverage` 包   
输入命令： npm install karma-coverage --save-dev   
根据相应配置更改karma.config.js文件的相应内容    
最后会在当前目录下生成coverage文件夹，里面包含覆盖率检测的html文件



