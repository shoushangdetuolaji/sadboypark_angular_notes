---
typora-root-url: img
---

## angular学习笔记

angular是一个完整框架，而react是一个类库，其对应angular的各种特性，需要寻找各种开源社区类库。

```
1.数据绑定、依赖注入		@angular/core
2.动态属性				rxjs（内部提供）
3.路由				@angular/router
4.组件库			    @angular/material
5.样式绑定，样式隔离		@angular/core
6.表单验证			   @angular/forms
7.命令行			    @angular/cli
```

### vscode的设置

```
angular 8 snippets
angular files
angular language
angular2-switcher
.net core project manager

推荐拓展
debugger for chrome
path intellisense
angular files
angular language service
angualr 8 snippets
```



### 谷歌扩展

augury 开发者工具 类似vue-dev-tool



### angular cli

- 新建一个工程 ng new project
- would you like to add angular routing? --yes/no
- which stylesheet format would you like to use? css

命令行式的创建一个项目：

```
ng new ${project} --skip-install --style css --routing false 

cd ${prject}

yarn / npm install
```



- angular.json

  项目的配置文件

- package.json

  npm包 开发依赖、库

### 编译打包

> 目录有个environments目录 打包配置用的、设置api接口

- 开发环境 ng build
- 生存环境 ng build --prod

### 项目目录结构

![](/01-项目目录结构.png)

### angular组件

```
- 模板
	- 视图布局
	- html
	- 绑定和指令
- 样式
	- 视图样式
	- css/scss/..
- 类
	- 属性
	-方法
    - 视图的控制
    - typescript
    - 属性：数据
    - 方法：逻辑
- 元数据
	- angular需要的数据或配置
	- 使用注解进行定义
= 组件
```

### typescript中的Interface

> 类型
>
> 很重要的作用之一 类型命名

可选属性，只读属性

函数类型、索引类型，类的类型

```js
interface Person {
    firstName: numbe;
    lastName: string;
    kids: number
  }

  var p: Person ={
    firstName:"bart",
    lastName:"wullems",
    kids: 1,
    age: 33 // 这里会报错 因为不是原有的属性
  }
```

### flex布局

```
看阮一峰的介绍吧
```

### 手机真机调试

确保手机和电脑处于同一 wifi

启动命令 ng serve --host 0.0.0.0

手机浏览器打开 http://<电脑IP>:4200

### 重要指令

- ngFor

  索引的获取 let i = index

  第一个和最后一个 let first = first； let last = last

  奇和偶：let odd = odd；let even = even

  提升性能：trackBy:trackElement

### 组件封装的意义和方法

- 需要复用，或者简化逻辑
- ng generate component components/组件名 (驼峰形式)
- 使用index.ts方便导入以及隔离内部变化对外部的影响

### 组件的输入输出

父组件---- 属性绑定@Input----->子组件

父组件<-----------事件绑定@Output----子组件

### 样式绑定的几种方式

```
<div [class.className]="条件表达式">...</div>
	class.className对于单个样式的条件绑定最为合适

<div [ngClass]="{'One':true,'Two':true,'Three':false}">
...</div>
	ngClass是自由度和拓展性最强的样式绑定方式

<div [ngStyle]="{'color':someColor,'font-size':fontSize}">
...</div>
	ngStyle由于是嵌入式样式，他会覆盖掉其他样式，使用时要谨慎
```





### 组件的声明周期

- `constructor` 构造函数永远首先被调用
- `ngOnChanges` 输入属性变化时被调用
- `ngOnInit`组件初始化时被调用
- `ngDoCheck`脏值检测时被调用
  - `ngAfterContentInit` 当内容投影ng-content完成时调用
  - `ngAfterContentChecked`Angular检测投影内容时调用（多次）
  - `ngAfterViewInit` 当组件视图(子视图)初始化完成时
  - `ngAfterViewChecked`当检测视图变化时(多次)
  - `ngAfterViewChecked`当检测视图变化时(多次)
- `ngOnDestroy`当组件销毁时



```
接口是可选的，也就说只要有类似 ngOnInit这样的方法
生命周期的钩子函数还是正常执行
但建议实现接口，好处一个是不会由于误删除某个钩子函数
另一个是对组件涉及到哪些生命周期一目了然
```





### 模板在组件类中的引用

```ts
<div #helloDiv>
	你好
</div>

export class AppComponent {
	@ViewChild('helloDiv') helloDivRef: ElementRef;
}

@ViewChild 是一个选择器，用来查找要引用的DOM元素或者组件
ElementRef是DOM元素的一个包装类
因为DOM元素不是Angular中的类，所以需要一个包装类似以便在Angular中使用和标识其类型
```

### 模板在组件类中的引用

```
<app-image-slider [sliders]="imgaeSliders"></app-image-slider>

@ViewChild('ImageSliderComponent') imageSlider: ImageSliderComponent;

如果想引用模板中的angular组件
@ViewChild中，可以使用引用名
也可以使用组件类型
```

### 引用多个模板元素

```
<img #img
	 *ngFor="let slider of sliders"
	 [src]="slider.imgUrl"
	 [alt]="slider.caption"
/>

@ViewChildren('img') imgs: QueryList<ElementRef>;  // 多个elementref 所以用querylist泛型
可以使用@ViewChildren,在@ViewChildren中可以使用引用名
或者使用angular组件/指令的类型。 声明类型为QueryList<?>
```



`<ElementRef>` 当下的属性都有natvieElement 记住了吗拖拉机

直接操作dom angular不推荐 推荐rd2 防止注入攻击【场景： 用户添加html代码，防止注入恶意攻击】



用到模板元素 好像都在ngafterViewInit()获取



@ViewChild总结

- @ViewChild用来在类中引用模板中的视图节点
- 可以是angular组件，也可以是html元素
- 在afterViewInit中可以完全的使用@ViewChild引用的元素
- 推荐使用renderer2操作dom元素



### ngModule 双向绑定

`FormsModule`中提供的指令

使用`[(ngModel)]`="变量"形式e进行双向绑定

其实是一个语法糖

```
[ngModel]="username" (ngModelChang)="username = $event"
```

```
<input 
  type="text" 
  [value]="username" 
  (input)="username = $event.target.value" />

  <span>你好: {{ username }}</span>  
```

推荐:

在`app.module.ts`导入

```
import { FormsModule } from '@angular/forms';

imports: [
	...
    FormsModule
  ],
  
  <input 
  type="text" 
  [(ngModel)]="username" />

  <span>你好: {{ username }}</span> 
```



### 模块

> 模块就是提供相对独立功能的一组代码

- 模块的组成部分可以有：组件、服务、指令、管道等。
- 从某种角度说，他就像一个小型的应用



@NgModule 注解

- declarations 数组: 模块拥有的组件、指令或管道。注意每个组件/指令/管道只能在一个模块中声明。
- providers数组： 模块中需要使用的服务
- exports 数组：暴露给其他模块使用的组件、指令或管道等。
- import数组： 导入本模块需要的依赖模块，注意是模块



创建 home module

```
ng g m home --routing
ng g m recommend --routing
ng g m category --routing
ng g m chat --routing
ng g m my --routing
ng g m product --routing

ng g m shared  这里不需要路由
```



### 什么是装饰器(注解

装饰器/注解就是一个函数

但它是一个返回函数的函数

它是TypeScript的一个特性，而非angular特性

ho 蛮难的，用到

```
Object.defineProperty()
Object.getOwnPropertyDescriptor()
```



### Angular中的指令

- 组件带模板的指令
- 结构性指令--改变宿主文档结构
- 属性型指令--改变宿主行为

内建指令：

结构性指令 ngIf ngFor ngSwitch

属性型指令 ngClass ngStyle ngModel





指令模板

```TS
import {Directive } from '@angular/core';

@Directive({
  selector: 'app-Name',
})
export class NameDirective {}
```

指令所在的元素叫宿主



### 指令的样式和事件绑定

指令可以理解为没有模板的组件，他需要一个宿主元素。

推荐使用方括号[] 指定Selector， 使它变成一个属性。

指令没有模板，指令要寄宿在一个元素之上 - 宿主 host

@HostBinding 绑定宿主的属性或者样式

@HostListener 绑定宿主的事件



### 组件嵌套

- 组件嵌套是不可避免的
  - 过度嵌套会陷入复杂和冗余
- 组件本身和外界的交互
  - 通过@Input和@Output
- 避免组件嵌套导致冗余数据和事件传递
  - 内容投影
  - 路由
  - 指令
  - 服务

#### 投影组件

- ng-content是什么
  - 动态内容
- 表现形式
  - `<ng-content select="样式类/HTML标签/指令"></ng-content>`
- 使用场景
  - 动态内容
  - 容器组件

### 路由

- 路由(导航)本质上是切换视图的一种机制。

- 路由的导航的url是否真实存在
  - angular的路由借鉴了大家熟知的浏览器url变化导致页面切换的机制
  - angular是单页程序，路由显示的路径不过是一种保存路由状态的机制，这个路径在web服务器上不存在



定义路由数组

- 路径
- 组件
- 子路由

导入RouterModule

- forRoot
- forChild



路由模板

```ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes} from '@angular/router';

const routes: Routes = [

];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})

export class AppRoutingModule {}
```

```
Cconstructor(private router:Router; private route: ActivatedRoute) {}
```



### 子路由

在路由模板下又加个`<router-outlet></router-outlet>`

#### 路径参数

- 配置

  - ```
    {path: ':tabLink', components: HomeDetailComponent}
    ```

- 激活

  - ```
    <a [routerLink]="['/home', tab.link]">..</a>
    this.router.navigate(['home'], tab.link);
    ```

- url

  - ```
    http://localhost:4200/home/sports
    ```

- 读取

  - ```
    this.route.paramMap.subscribe(params => {...})
    ```

#### 路径对象参数

- 配置

  - ```
    {path: ':tabLink', component: HomeDetailComponent}
    ```

- 激活

  - ```
    <a [routerLink]="['/home', tab.link, {name: 'val1'}]">...</a>
    ```

- url

  - ```
    http://localhost:4200/home/sports; name=va1
    ```

- 读取

  - ```
    this.route.paramMap.subscribe(params => {...})
    ```

#### 查询参数

- 配置

  - ```
    {path:'home', component: HomeDetailComponent}
    ```

- 激活

  - ```
    <a [routerLink]="[/home]" [queryParams]={name:'val1'}>...</a>
    
    this.router.navigate(['home'], {queryParams:{name: 'val1'}});
    ```

- url

  - ```
    http://localhost:4200/home?name=val1
    ```

- 读取

  - ```
    this.route.queryParamMap.subscribe(params => {...});
    ```





栗子

```TS
const routes: Routes = [
  {
    path: 'home',
    component: HomeContainerComponent,
    children: [
      {
        path: '',
        redirectTo: 'hot',
        pathMatch: 'full'
      },
      {
        path: ':tabLink',
        component: HomeDetailComponent,
        children: [
          {
            path: 'aux',
            component: HomeAuxComponent,
            outlet: 'second'
          },
          {
            path: 'grand',
            component: HomeGrandComponent
          }
        ]
      }
    ]
  }
];
```



```
http://localhost:4200/home/hot/grand?name=zhangsan&gender=male

<a 
  [routerLink]="['grand']" 
  routerLinkActive="active"
  [queryParams]="{name:'zhangsan',gender: 'male'}">link to grand</a>
<router-outlet></router-outlet>  

###
带名字的路由 辅助插住？ 很复杂 算了把
http://localhost:4200/home/hot/(second:aux)
<a 
  [routerLink]="[{outlets: {second:['aux']}}]" 
  >link to second</a>
<router-outlet name="second"></router-outlet>
```



### 管道的概念

管道的作用就是在视图上提供便利的值变换的方法

Date -> 2天前, 1234.23 -> ￥1,234.23

- AsyncPipe 处理异步的管道
- DecimalPipe 处理数字
- l18nSelectPipe 国际化
- CurrencyPipe 货币管道
- LowerCasePipe、TitleCasePipe  UpperCasePipe大小写管道
- JsonPipe 调试用的 
- PercentPipe 百分比
- DatePipe 日期处理
- KeyValuePipe 字典对象



有angular内置的，还可以自定义的！

管道涉及到中文注册

- 要在app.module.ts

```TS
import localZh from '@angular/common/locales/zh-Hans';
import { registerLocaleData } from '@angular/common';

@NgModule({
  ...
  providers: [
    {
      provide: LOCALE_ID,
      useValue: 'zh-Hans'
    }
  ],
  ...
})
export class AppModule { 
  constructor() {
    registerLocaleData(localZh, 'zh');
  }
}
```



管道的模板

```TS
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'selector-name'
})

export class NamePipe implements PipeTransform {
  transform(value: any, ...args: any[]): any {
    
  }
}
```



### 依赖注入

- 提供服务
  - @injectable()
  - 标记为可供注入的服务
- 模块中声明
  - providers数组
  - 或者import对应模块
- 再组件中使用
  - 构造函数中直接声明
  - angular框架帮你完成注入



### 脏值检测

- 什么事脏值检测?
  - 当数据改变时更新视图dom
- 什么时候会触发脏值检测?
  - 浏览器事件、click、mouseover、keyup
  - setTimeout()和setInterval()
  - http请求
- 如何进行检测
  - 检查两个状态值：当前状态和新状态
  - onPush策略

好像按道理不能在`ngAfterViewChecked` 进行脏值检测

​	但是呢 有解决爆发 构造器里导入 避免绑定消耗脏值检测

```ts
import { formatDate } from '@angular/common';
import { AfterViewChecked, Component, ElementRef, NgZone, OnInit, Renderer2, ViewChild } from '@angular/core';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html',
  styleUrls: ['./child.component.css']
})
export class ChildComponent implements OnInit, AfterViewChecked {
  _title;
  _time;
  @ViewChild('timeRef', {static: true}) timeRef: ElementRef;
  public get title(): string {
    console.log('脏值检测');
    return this._title;
  }
  public get time(): number {
    return this._time;
  }
  constructor(
    private ngZone: NgZone,
    private rd2: Renderer2
  ) { 
    this._title = 'hi';
  }

  ngOnInit(): void {
  }
  ngAfterViewChecked(): void {
    this.ngZone.runOutsideAngular(() => {
      setInterval(() => {
        this.rd2.setProperty(
          this.timeRef.nativeElement, 
          'innerText', 
          formatDate(Date.now(), 'HH:mm:ss','zh-hans')
        );
      }, 100)
    })
  }
  handleClick() {

  }

}
```

```HTML
<!-- <span [textContent]="time | date: 'HH:mm:ss'"></span> -->
<span #timeRef></span>
<p>child works!</p>
<button (click)="handleClick()">触发脏值检测</button>
```





`changeDetection: ChangeDetectionStrategy.OnPush`

只看@input属性变化

但是，涉及到路由组件返回不会出发onInit 

​	需要手动小技巧

```
constructor(
	private cd: ChangeDetectorRef
)

....
this.cd.markForCheck() 我发生变化麻烦请求下我
```

### http概览

vscode插件 - `rest client`

### Restful API

动词决定操作

- get - 查询
- post - 新增
- put - 更改
- delete 删除

名词代表资源

- /products - 复数代表集合
- /products/{id} - 路径参数取得特定条目

响应状态码

- 2xx 成功
- 3xx 重定向
- 4xx 客户端错误
- 5xx 服务端错误



#### 慕课网api调用方式

api基准地址

- http://apis.imooc.com/api

icode参数

- 需要携带一个icode参数

如何调用

- 比如课程中需要访问/banners
- api url就是 http://apis.imooc.com/api/banners?iCode=你的iCode



#### angular HttpClient

导入`HttpClientModule`

- 只在根模块中导入
- 整个应用只需导入一次，不要在其他模块导入

在构造中注入`HttpClient`

- get/post/put/delete 方法对应于http方法
- 这些方法是泛型的，可以直接把放回到JSON转换成对应类型
- 不规范的请求，使用request方法

返回的值是`Observable`

- 必须订阅，才会发送请求，否则不会发送



#### http拦截器

模板

```ts
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpEvent, HttpHandler, HttpRequest } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable()
export class YourInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    return next.handle(req);
  }
}
```

### rxjs - 响应式编程类库

rxjs - reactive extensions for js



流的种类： 无限、有限、单个、空

流的状态：next，error，complete

所有的操作都是异步的

 