# **Chủ đề tuần 3**

- HttpClient
- Interceptor

## **a. HttpClient**
- Http Client là một `Service Module(*)` được cung cấp bởi Angular giúp chúng ta thực hiện những yêu cầu Http, dễ dàng custom các request option và handle error một cách dễ dàng.
- Bản chất nó được gọi là `Service Module` vì nó chỉ init các services (http client, http backend, etc) và không export bất cứ component hay directive nào.

<div align="center">
    <img src="https://res.cloudinary.com/tuananh-asia/image/upload/v1591952900/HTTP%20CLIENT%20MODULE/http-client-module-core_rbhuy6.png">
</div>

- Đây là nơi khai báo `HttpClientModule` trong core của Angular.

## **a.1 Sử dụng Http Client Module trong Angular project**
- Đầu tiên, Mình sẽ tạo một service ở level `AppModule` nhé. Service này có method `getListPosts` để lấy danh sách bài viết từ 1 API endpoint.

```typescript
import { Injectable } from "@angular/core";
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { PostEntityModel } from '../models/post.entity.model';

@Injectable({
    providedIn: 'root'
})
export class PostService {
    constructor(private httpClient: HttpClient) { }

    getListPosts(): Observable<PostEntityModel[]> {
        return this.httpClient.get<PostEntityModel[]>('https://jsonplaceholder.typicode.com/posts');
    }
}
```

- Tạo model cho post:

```typescript
export interface PostEntityModel {
    userId: number;
    id: number;
    title: string;
    body: string;
}
```

- Inject vào compoent để gọi lấy danh sách bài viết

```typescript
import { Component, OnInit } from '@angular/core';
import { PostService } from '../../services/post.service';

@Component({
    selector: 'app-post',
    templateUrl: './post.component.html',
    styleUrls: ['./post.component.scss']
})
export class PostComponent implements OnInit {
    constructor(
        private postService: PostService
    ) { }

    ngOnInit(): void {
        this.postService.getListPosts().subscribe(console.log);
    }
}
```

- Và kết quả là:

<div align="center">
    <img src="https://res.cloudinary.com/tuananh-asia/image/upload/v1592072438/HTTP%20CLIENT%20MODULE/no-import-http-client-module_zzorzz.png">
</div>

- Uầy. Lỗi rồi. Lý do gì đây ??
- Lí do là chưa import `HttpClientModule` haha. -> Mình sẽ import `HttpClientModule` vào `AppModule` nhé. (Còn import vào `AppModule` hay import vào Feature Module thì mình sẽ nói thật chi tiết trong bài sau nhé)

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { HttpClientModule } from '@angular/common/http';

@NgModule({
    declarations: [
        AppComponent
    ],
    imports: [
        BrowserModule,
        AppRoutingModule,
        HttpClientModule // --> import here
    ],
    providers: [],
    bootstrap: [AppComponent]
})
export class AppModule { }
```

- Kết quả đây:
<div align="center">
    <img src="https://res.cloudinary.com/tuananh-asia/image/upload/v1592041530/HTTP%20CLIENT%20MODULE/result-after-import-http-client-module_bvektu.png">
</div>

- Mình xin giải thích chút xíu về lỗi phía trên. Khi import `HttpClientModule`. Nó provide các dịch vụ có trong module như là `HttpClient`, `HttpBackend`,… Nên nãy quên import `HttpClientModule` thì chưa có instance của các service nên DI của Angular không biết lấy `HttpClient` ở đâu cả. Nên báo lỗi `NullInjector` ngay.
Đây là danh sách các service được provide trong `HttpClientModule`:

<div align="center">
    <img src="https://res.cloudinary.com/tuananh-asia/image/upload/v1592042305/HTTP%20CLIENT%20MODULE/providers-of-http-client-module_wetzpm.png">
</div>

## **a.2 Implement các method của HttpClient**
- Chúng ta cùng điểm qua một số method của `HttpClient` mà mình thường xài nhé.

```typescript
@Injectable({
    providedIn: 'root'
})
export class PostService {
    constructor(private httpClient: HttpClient) { }

    getListPosts(): Observable<PostEntityModel[]> {
        return this.httpClient.get<PostEntityModel[]>('https://jsonplaceholder.typicode.com/posts');
    }

    createPost(post: PostEntityModel): Observable<PostEntityModel> {
        return this.httpClient.post<PostEntityModel>('https://jsonplaceholder.typicode.com/posts', post);
    }

    updatePost(postId: number, post: PostEntityModel): Observable<PostEntityModel> {
        return this.httpClient.put<PostEntityModel>(`https://jsonplaceholder.typicode.com/posts/${ postId }`, post);
    }

    updateOptionPost(postId: number, post: Partial<PostEntityModel>): Observable<PostEntityModel> {
        return this.httpClient.patch<PostEntityModel>(`https://jsonplaceholder.typicode.com/posts/${ postId }`, post);
    }

    deletePost(postId: number): Observable<any> {
        return this.httpClient.delete(`https://jsonplaceholder.typicode.com/posts/${ postId }`);
    }
}
```

- Như các bạn đã thấy rồi đấy. Nó cung cấp đủ cho chúng ta những method cần thiết để làm việc với API (get, post, put, patch, delete, jsonp). Hơn nữa, các method của HttpClient đều trả về Observable, các Observable này thường chỉ emit 1 lần rồi complete (trừ 1 số options đặc biệt mình sẽ giới thiệu trong các bài sau).
- Đây là toàn bộ method của HttpClient.

## **b. Interceptor**
- Interceptor trong Angular đc biết đến nhiều nhất là `$http`.Đây là service giúp ta có thể thao tác với backend và tạo ra các HTTP request.Có những trường hợp mà ta muốn nắm bắt mọi yêu cầu và vận dụng nó trước khi gửi nó đến server hoặc nắm bắt các response từ server và xử lý nó trước khi hoàn tất các cuộc gọi.Interceptor sẽ giúp t thực hiện việc này .

### **b.1 Giới thiệu về interceptor trong AngularJS**

- Các `$httpProvider` chứa một loạt các `interceptor`. Một interceptor đơn giản chỉ là một service thường xuyên được đăng ký vào mảng. Đây là cách chúng ta tạo ra một interceptor:

```typescript
module.factory('myInterceptor', ['$log', function($log) {
    $log.debug('$log is here to show you that this is a regular factory with injection');

    var myInterceptor = {
        ....
        ....
        ....
    };

    return myInterceptor;
}]);
```

và add interceptor vào `$httpProvider.interceptors`:

```typescript
module.config(['$httpProvider', function($httpProvider) {
    $httpProvider.interceptors.push('myInterceptor');
}]);
```

- Các interceptions

    - `request` : Method này được chạy trước khi gửi 1 request tới backend, developer có thể thay đổi các tham số config của request đó
    - `requestError` : Đôi khi các request có thể gặp lỗi trong quá trình gửi đi, RequestError interceptor sẽ bắt các lỗi này để show ra lý do cho developer để có thể sửa .
    - `response` : Method được chạy ngay sau khi nhận được kết quả thành công từ request method.
    - `responseError` : Khi gặp lỗi trong quá trình nhận responce , ResponseError interceptor sẽ bắt và hiển thị các lỗi cho developer.

- ví dụ cụ thể các interceptors:

```typescript
$provide.factory('myHttpInterceptor', function($q, dependency1, dependency2) {
  return {
    // optional method
    'request': function(config) {
      // do something on success
      return config;
    },

    // optional method
   'requestError': function(rejection) {
      // do something on error
      if (canRecover(rejection)) {
        return responseOrNewPromise
      }
      return $q.reject(rejection);
    },

    // optional method
    'response': function(response) {
      // do something on success
      return response;
    },

    // optional method
   'responseError': function(rejection) {
      // do something on error
      if (canRecover(rejection)) {
        return responseOrNewPromise
      }
      return $q.reject(rejection);
    }
  };
});

$httpProvider.interceptors.push('myHttpInterceptor');
```

### **b.2 Sử lí không đồng bộ trong interceptor**
- Sử lý không đồng bộ trong request interceptor

```typescript
module.factory('myInterceptor', ['$q', 'someAsyncService', function($q, someAsyncService) {
    var requestInterceptor = {
        request: function(config) {
            var deferred = $q.defer();
            someAsyncService.doAsyncOperation().then(function() {
                // Asynchronous operation succeeded, modify config accordingly
                ...
                deferred.resolve(config);
            }, function() {
                // Asynchronous operation failed, modify config accordingly
                ...
                deferred.resolve(config);
            });
            return deferred.promise;
        }
    };

    return requestInterceptor;
}]);
```

- Sử lý không đồng bộ trong responce interceptor

```typescript
module.factory('myInterceptor', ['$q', 'someAsyncService', function($q, someAsyncService) {
    var responseInterceptor = {
        response: function(response) {
            var deferred = $q.defer();
            someAsyncService.doAsyncOperation().then(function() {
                // Asynchronous operation succeeded, modify response accordingly
                ...
                deferred.resolve(response);
            }, function() {
                // Asynchronous operation failed, modify response accordingly
                ...
                deferred.resolve(response);
            });
            return deferred.promise;
        }
    };

    return responseInterceptor;
}]);
```
