# **Chủ đề tuần 2**
- Angular Route
- CanActive
- CanDeactivate

## **a. Angular Route**
Angular Router là module được tích hợp sâu vào Angular, giúp bạn dễ dàng tạo các route cho ứng dụng.
<br>

Thực hiện nhiệm vụ chính là chuyển trang, thay đổi một số thành phần mà không cần phải tải lại trang

### **a.1 Sử dụng Router trong Angular ta cần:**
-  Khi khởi tạo một ứng dụng có hỗ trợ router với CLI, ta chạy câu lệnh dưới đây:
```typescript
    ng new <tên_app> --routing
```
- Tại <code>index.html</code> ta cần thêm <code>`<base href="/">`</code>.
- Cần có một khu vực khai báo Directive:<code>`<router-outlet></router-outlet>`</code> nơi này là phần nội dung cần thay đổi.
- Cần phải import <code>RouterModule, Routes</code> từ <code>@angular/router</code> vào <code>app.module.ts</code>.
- Khai báo router root cho ứng dụng: <code>RouterModule.forRoot(array_routes: Routes)</code>
- Phần tử trong <code>array_routes</code> sẽ là một object bao gồm tối thiểu:

    - <b>path</b>: Khai báo đường dẫn đến một component: VD: 'index', 'home' ...

        - Nếu khai báo <b>path</b>: <code>`'**'`</code> thì nếu không tìm thấy router thì sẽ load ra component tương ứng mà mình định nghĩa cho <b>path</b> này.
    - **component**: Khai báo component.

<i><b>Ví dụ:</b></i>

- Mình sẽ tạo ra một project mới và đồng thời sẽ tạo ra hai component có tên là <b>HomeComponent, AboutComponent</b>

Đầu tiên ta vào sau để tạo phần header và body (Phần các Component được render ra tương ứng với router của nó)

<b>File app.component.html</b>

```typescript
<nav class="navbar navbar-default" role="navigation">
    <div class="container-fluid">
    <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header">
            <button
                type="button"
                class="navbar-toggle"
                data-toggle="collapse"
                data-target=".navbar-ex1-collapse"
            >
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
        </div>

        <!-- Collect the nav links, forms, and other content for toggling -->
        <div class="collapse navbar-collapse navbar-ex1-collapse">
            <ul class="nav navbar-nav">
                <li class="active"><a href="/index">Home</a></li> <!-- Gọi đến HomeComponent -->
                <li><a href="/about">About</a></li>  <!-- Gọi đến AboutComponent -->
            </ul>
        </div><!-- /.navbar-collapse -->
    </div>
</nav>

<div class="container">
    <div class="row">
        <div class="col-xs-12 col-sm-12 col-md-12 col-lg-12">
            <div class="panel panel-primary">
                <div class="panel-heading">
                    <h3 class="panel-title"></h3>
                </div>
                <div class="panel-body">
                    <!-- Phần này là phần các component được gọi ra tương ứng với router của nó -->
                    <router-outlet></router-outlet>
                </div>
            </div>
        </div>
    </div>
</div>

```

Tiếp theo ta vào file **app.module.ts** khai báo và định nghĩa router cho project.

**File app.module.ts**

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { HomeComponent } from './components/home/home.component';
import { AboutComponent } from './components/about/about.component';
import { NotFoundComponent } from './not-found/not-found.component';

const appRouter : Routes = [
    {
        path : 'index',
        component: HomeComponent
    },
    {
        path : 'about',
        component: AboutComponent
    },
    {
        // khi một router nào được gọi mà không có trong phần appRouter thì NotFoundComponent được gọi ra
        path : '**',  
        component: NotFoundComponent
    }
];

@NgModule({
    declarations: [
        AppComponent,
        HomeComponent,
        AboutComponent,
        NotFoundComponent
    ],
    imports: [
        BrowserModule,
        AppRoutingModule,
        RouterModule.forRoot(appRouter)
    ],
    providers: [],
    bootstrap: [AppComponent]
})
export class AppModule { }
```

Ở version hiện tại <b>Angular 15.0.2</b> ta sẽ có thể cấu hình router trong file <b>app/app-routing.module.ts</b>.

Giả sử bạn muốn tự động chuyển trang khi vào một router nào đó thì ta có thể khai báo như sau:

- <b>path</b>: '<tên_router>'
- <b>redirectTo</b>: '/tên_router_khác'
- <b>pathMath</b>: 'full' => cho router biết làm thế nào để khớp một URL đến đường dẫn của router và sẽ xảy ra lỗi nếu không khai báo.

<b>File app.module.ts</b>

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { HomeComponent } from './components/home/home.component';
import { AboutComponent } from './components/about/about.component';
import { NotFoundComponent } from './not-found/not-found.component';

const appRouter : Routes = [
    {
        // khi đường dẫn là '' thì nó sẽ được tự động gọi đến đường dẫn là '/index'
        path : '',
        redirectTo : '/index',
        pathMatch : 'full'
    },
    {
        path : 'index',
        component: HomeComponent
    },
    {
        path : 'about',
        component: AboutComponent
    },
    {
        path : '**',
        component: NotFoundComponent
    }
];

@NgModule({
    declarations: [
        AppComponent,
        HomeComponent,
        AboutComponent,
        NotFoundComponent
    ],
    imports: [
        BrowserModule,
        AppRoutingModule,
        RouterModule.forRoot(appRouter)
    ],
    providers: [],
    bootstrap: [AppComponent]
})
export class AppModule { }
```

Ở trên thì mình đang dùng <code>`href="index"`</code> nên là khi click vào đó thì trang sẽ bị tải lại, để khắc phục điều đó ta sẽ:

- sử dụng <code>``[routerLink]="[/tên_router', params]"</code> Vd: <code>`[routerLink]="[/home', 1]"`</code> => '/home/1'

<i><b>Ví dụ:</b></i>

Ta vào file **app.component.html** thay <code>`href="/index"`</code> bằng <code>`[routerLink]="['/index']"`</code> xem sao

<b>File app.component.html</b>

```typescript
........
        <!-- Collect the nav links, forms, and other content for toggling -->
        <div class="collapse navbar-collapse navbar-ex1-collapse">
            <ul class="nav navbar-nav">
                <li class="active"><a [routerLink]="['/index']">Home</a></li>
                <li><a [routerLink]="['/about']">About</a></li>
            </ul>
        </div><!-- /.navbar-collapse -->
    </div>
</nav>
........
```

Nếu bạn muốn thêm class **active** (hoặc thêm class_name khác) nếu router hiện tại trùng với router khai báo:

```typescript
- `routerLinkActive='active'` (có thể thay thế active bằng một tên khác hoặc sử dụng nhiều class cùng một lúc 'active active1 active2')
```

<b>File app.component.html</b>

```typescript
........
 <div class="collapse navbar-collapse navbar-ex1-collapse">
        <ul class="nav navbar-nav">
            <li routerLinkActive="active">
                <a [routerLink]="['/index']">Home</a>
            </li>
            <li routerLinkActive="active">
                <a [routerLink]="['/about']">About</a>
            </li>
        </ul>
</div>
........

```

### **a.2 Chuyển trang bằng even binding:**
Giả sử giờ mình sẽ không gọi trực tiếp đường dẫn ở các thẻ mà mình sẽ gọi nó thông qua một sự kiện nào đó

- Cần import Router từ <code>@angular/router</code>
- Sử dụng **navigate**: <code>.navigate(['tên_router', param])</code>.
- hoặc sử dụng **navigateByUrl('tên_router')**

<i><b>Ví dụ:</b></i>

Ở file **app.component.html** mình tạo ra thêm hai button và cho nó sự kiện click

<b>File app.component.html</b>

```typescript
........

<div class="container">
    <div class="row">
        <div class="col-xs-12 col-sm-12 col-md-12 col-lg-12">
            <div class="panel panel-primary">
                <div class="panel-heading">
                    <h3 class="panel-title"></h3>
                </div>
                <div class="panel-body">
                    <!-- Phần này là phần các component được gọi ra tương ứng với router của nó -->
                    <router-outlet></router-outlet>
                </div>
            </div>
        </div>
    </div>
</div>

<button type="button" class="btn btn-primary" (click)="navigate('index')">Home</button>
<button type="button" class="btn btn-success" (click)="navigate('about')">About</button>
```

<b>File app.component.ts</b>

```typescript
import { Component } from '@angular/core';
import { Router } from '@angular/router';

@Component({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.css']
})
export class AppComponent {
    title = 'Router';

    constructor(
        public routerService : Router
    ) {}

    navigate(url : string) {
        // this.routerService.navigate([url]);
        this.routerService.navigateByUrl(url);
    }
}
```

### **a.3 Lấy tham số trên router:**

- Để lấy được tham số trên router ta sử dụng ActivateRoute từ <code>@angular/router</code>.
- Cú pháp: <code>.snapshot.params['tên_param_khai_báo_trong_router']</code> Ví dụ: '/home/:id' => 'id'.

<b>Ví dụ</b> ở file <b>app.module.ts</b> mình khai báo thêm một router

<b>File app.module.ts</b>

```typescript
........
const appRouter : Routes = [
    {
        path: '',
        redirectTo: '/index',
        pathMatch: 'full'
    },
    {
        path: 'index',
        component: HomeComponent
    },
    {
        path: 'about',
        component: AboutComponent
    },
    {
        path: 'products/:id', // khai báo router này để lấy id từ trên router
        component: ProductDetailComponent
    },
    {
        path: '**',
        component: NotFoundComponent
    }
];
........
```

Ở file <code>product-detail.component.html</code> mình sẽ hiển thị id mà mình lấy được.
```typescript
<h1>Param {{ id ? id : ''}}</h1>
```

Ở file <code>product-detail.component.ts</code> mình sẽ lấy id từ trên router sau đó mình sẽ truyền qua cho <code>product-detail.component.html</code> để hiển thị lên.

```typescript
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({
    selector: 'app-product-detail',
    templateUrl: './product-detail.component.html',
    styleUrls: ['./product-detail.component.css']
})
export class ProductDetailComponent implements OnInit {
    public id : number = 0;

    constructor(
        public activatedRoute : ActivatedRoute
    ) { }

    ngOnInit(): void {
        this.id = this.activatedRoute.snapshot.params['id'];
    }
}
```

<br>

## **b. CanActive**
- Là 1 trong 1 loại Angular Guard được sử dụng để bảo vệ router của mình.
- Trong một ứng dụng web, chúng ta thường đối mặt với kịch bản một số page cho phép tất cả mọi người truy cập, ngược lại 1 soos khác chỉ dành cho user login or admin chẳng hạn
- **Quyết định việc một router được kích hoạt**
- Để được cấu trúc khai báo và cách hoạt động đầu tiên ta cần tìm hiểu Guard là gì:

### **b.1 Định nghĩa Guard:**
- Guard có thể được triển khai trong những cách khác nhau, nhưng sau tất cả nó sẽ là function trả về một trong những kiểu sau <code>Observable<boolean>, Promise<boolean> </code> hoặc <code>boolean</code>.
- Guard có 4 type:

    - <b>CanActivate:</b> Quyết định việc một route được kích hoạt.
    - <b>CanActivateChild:</b> Quyết định việc children routes được kích hoạt.
    - <b>CanDeactivate:</b> Quyết định việc route hủy kích hoạt.
    - <b>CanLoad:</b> Quyết định một module được Lazy Loading
    
- Phụ thuộc ta muốn làm gì mà triển khai 1 số Guard nhất định.

#### **b.1.1 Kiểu function**
- Để đăng ký một guard chúng ta cần định nghĩa một token và guard function. Dưới dây là một định nghĩa cực kì đơn giản cho guard:

**File app.module.ts**

```typescript
@NgModule({
    ...
    providers: [
        provide: 'CanActiveGuard',
        useValue: () => {
            return true;
        }
    ],
    ...
})
export class AppModule {}
```

- Ở trên là định nghĩa một guard mà luôn trả về true, vì vậy chúng ta có thể include nó tới providers của AppModule luôn. Khi đã cấu hình nó trong AppModule chúng ta có thể sử dụng guard trong route config. Cấu hình route bên dưới có CanActivateGuard được đính kèm:

**File app-routing.module.ts**

```typescript
export const AppRoutes:RouterConfig = [
  { 
    path: '',
    component: SomeComponent,
    canActivate: ['CanActivateGuard']
  }
];
```

- Như chúng ta có thể thấy, việc chúng ta cần làm là định nghĩa các guard tokens được gọi đến. Điều này cũng có nghĩa là chúng có thể triển khai nhiều guards để bảo vệ một route. Guards được thực thi theo thứ tự mà chúng được khai báo trên route.

[**Link Tham Khảo**](https://viblo.asia/p/bao-ve-routes-su-dung-guards-trong-angular-3Q75wWX35Wb)


## **b. CanDeactivate**
- Bây giờ chúng ta có thể thấy làm thế nào để <code>CanActivate</code> làm việc trong những kịch bản khác nhau, nhưng như đã đề cập trước đó, chúng ta có thêm một số guard interfaces có thể tận dụng. <code>CanDeactivate</code> cho chúng ta lựa chọn để quyết định rời route hay không. Điều này rất hữa dụng nếu chúng ta muốn ngăn chặn người dùng làm mất những thay đổi chưa được save khi điền dữ liệu vào form và không may click trên button cancel.

- Việc implement một <code>CanDeactivate</code> guard là rất tương tụ với <code>CanActivate</code>. Tất cả những thứ cần làm là chúng ta tạo lại hoặc một function, hoặc một class cái mà implement <code>CanDeactive</code> interface. Chúng ta có thể implement đơn giản như bên dưới:

**File confimdeactivateguard.guard.ts**
```typescript
import { CanDeactivate } from '@angular/router';
import { CanDeactivateComponent } from './app/can-deactivate';

export class ConfirmDeactivateGuard implements CanDeactivate<CanDeactivateComponent> {

  canDeactivate(target: CanDeactivateComponent) {
    if(target.hasChanges()){
        return window.confirm('Do you really want to cancel?');
    }
    return true;
  }
}
```

- Khai báo nó trong AppModule

```typescript
@NgModule({
  ...
  providers: [
    ...
    ConfirmDeactivateGuard
  ]
})
export class AppModule {}
```
