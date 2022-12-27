# **Chủ đề tuần 3**

- Lazy loading
- Eager Loading

## **a. Lazy loading**
- **Lazy Loading** là một design pattern thường được sử dụng trong lập trình máy tính để trì hoãn lại việc khởi tạo một đối tượng cho đến khi nào nó thực sự cần đến. Nó góp phần giúp cho hoạt động của chương trình được hiệu quả hơn nếu như được sử dụng một cách hợp lý. Nói đơn giản là: "<i>**Không load bất kỳ thứ gì nếu như bạn không cần đến**</i>"

- Một ứng dụng Angular quy mô lớn sẽ chứa rất nhiều feature modules, nếu chúng được load cùng một lúc khi ứng dụng được khởi động thì sẽ phải mất rất nhiều thời gian. Hãy thử tưởng tượng, người dùng vào website của các bạn mà phải ngồi nhìn icon loading cứ quay vòng vòng đến hàng chục giây thì liệu họ có còn muốn vào lần thứ 2 không 😃). Các feature modules đó có thể được load bất đồng bộ sau khi ứng dụng được load theo yêu cầu hoặc sử dụng các chiến lược khác nhau. Giảm kích thước của bundle khi ứng dụng được load lần đầu sẽ cải thiện được thời gian load của ứng dụng, do đó sẽ nâng cao trải nghiệm của người dùng đối với ứng dụng web của bạn.

- Lazy Loading có nhiều lợi ích:
    - Bạn có thể load các feature modules chỉ khi được yêu cầu bởi người dùng
    - Bạn có thể tăng tốc thời gian load cho người dùng chỉ ghé thăm một số trang nhất định của ứng dụng
    - Bạn có thể tiếp tục mở rộng các modules được lazy load mà không tăng kích thước của bundle load lần đầu

- Để cho các bạn có cái nhìn chi tiết hơn, ở đây tôi xây dựng một clone đơn giản của Gmail sử dụng những dữ liệu ngẫu nhiên. Ban đầu, code sẽ chưa được lazy load mà sử dụng eager load các feature modules để có thể làm tăng tổng kích thước bundle load ban đầu phục vụ cho việc giải thích ý tưởng của bài viết.
<div align="center">
    <img src="https://images.viblo.asia/f16adb4a-9ffe-4a19-86cb-c51ccf8eb3ad.gif">
</div>

- Đây là một ứng dụng đơn giản, nó có một file routing chung cho toàn bộ ứng dụng <code>app-routing.module.ts</code> và tất cả các features được chia ra thành các modules.

- Dưới đây là router tree cho ứng dụng này:

<div align="center">
    <img src="https://images.viblo.asia/f7ac7d31-9f23-41bd-aff1-3f67af3cc266.png">
</div>

- Ở đây bạn có thể thấy rằng, chúng ta có 2 tầng routing lồng nhau, và mỗi một tầng sẽ có nhiều routes. Trên các ứng dụng thực tế, chúng có thể có nhiều hơn thế và mỗi module sẽ chứa rất nhiều components, templates,... Hiện tại, tất cả các Components trên đều được load cùng một lúc khi mà người dùng mở trang web của bạn. Điều đó là không cần thiết, và nó tốn quá nhiều thời gian chờ đợi của người dùng. Hãy cùng tôi refactor lại codes giúp cho ứng dụng load nhanh hơn nhé ^_^.

### **a.1 Refactoring ứng dụng sử dụng lazy load routes**
- Tôi sẽ tập trung vào lazy load <code>Settings</code> module và các routes liên quan.
- Hiện tại tất cả các routes đều được đăng ký ở trong file <code>app-routing.module.ts</code> và sau đó sẽ được import vào trong root module <code>app.module.ts</code>. Dưới đây là file routing hiện tại của ứng dụng:

```typescript
# app.routing.ts

const routes: Routes = [
  {
    path: '',
    redirectTo: '/inbox/primary',
    pathMatch: 'full'
  },
  {
    path: 'inbox',
    component: InboxComponent,
    children: [
      {
        path: '',
        redirectTo: 'primary',
        pathMatch: 'full'
      },
      {
        path: 'primary',
        component: PrimaryComponent
      },
      {
        path: 'social',
        component: SocialComponent
      }
      ...
    ]
  },
  {
    path: 'settings',
    component: SettingsComponent,
    children: [
      {
        path: '',
        redirectTo: 'general',
        pathMatch: 'full'
      },
      {
        path: 'general',
        component: GeneralComponent
      },
      {
        path: 'inbox',
        component: SettingsInboxComponent
      },
      ...
    ]
  },
  {
    path: 'important',
    component: ImportantComponent
  },
  {
    path: 'sent',
    component: SentComponent
  },
  ...
];
```

**Bước 1:**
- Di chuyển routing cho <code>Settings</code> vào trong module của nó <code>gm-settings.module.ts</code>. Khi đăng ký các routes với <code>RouterModule</code>, chúng ta cần sử dụng <code>forChild</code> thay vì <code>forRoot</code> giống như đây là routing con của ứng dụng. Do đó, <code>gm-settings.module.ts</code> sẽ trông như sau:

```typescript
export const routes: Routes = [
  {
    path: '',
    component: SettingsComponent,
    children: [
      {
        path: '',
        redirectTo: 'general',
        pathMatch: 'full'
      },
      {
        path: 'general',
        component: GeneralComponent
      },
      {
        path: 'inbox',
        component: InboxComponent
      },
      ...
    ]
  }
];


@NgModule({
  imports: [
    CommonModule,
    RouterModule.forChild(routes),
    MdTabsModule
  ],
  declarations: [
    SettingsComponent, 
    GeneralComponent, 
    InboxComponent,
    ...
  ]
})
export class GmSettingsModule { }
```

**Bước 2:**
- Cập nhật <code>app.routing.ts</code> để load các routes con sử dụng một đường dẫn tương đối đến module <code>Settings</code> và thêm cả tên lớp module đó nữa:

```typescript
{
    path: 'settings',
    loadChildren: './gm-settings/gmsettings.module#GmSettingsModule'
}
```

**Bước 3:**
- Module <code>Settings</code> hiện tại đang được import vào trong file <code>app.module.ts</code> để được eager load, bây giờ chúng ta cần xóa bỏ nó đi, Angular sẽ tự động cấu hình việc đăng ký module <code>Settings</code>.

- Như vậy là chúng ta đã tiến hành xong việc Lazy Load cho module Settings. Hãy cũng xem Router Tree để thấy kết quả thay đổi ra sao nhé:

<div align="center">
    <img src="https://images.viblo.asia/3dc235f0-b98e-47d5-8751-759670c591cd.gif ">
</div>

- Nhìn vào kết quả ở trên, ta có thể thấy rằng, khi ứng dụng được load, tất cả các components của module Settings đều chưa hề được load, mà chúng chỉ thực sự được load khi mà người dùng click vào tab settings.

- Như vậy là chúng ta đã Lazy Load thành công được module Settings và cải thiện được đáng kể tốc độ load lần đầu của ứng dụng.

- Tuy nhiên, có một điều vẫn chưa được tốt cho lắm, đó là các module chỉ được load khi mà người dùng click vào settings hoặc các email cụ thể. Điều này có thể dẫn đến sự chậm trễ khi mà người dùng phải chờ đợi cho module đó được load => làm cho trải nghiệm người dùng xấu đi.

<div align="center">
    <img src="https://images.viblo.asia/4c6175e8-d6fb-4432-9535-2560debc1f0a.gif">
</div>

Để khắc phục được vấn đề này, chúng ta sử dụng chiến lược Preload.

### **a.2 Chiến lược Preload**
- Đối với vấn đề được nêu ở trên, Angular cung cấp một phương thức để nói với router load tất cả các lazy load modules bất đồng bộ ngay lập tức sau khi ứng dụng được load và không cần phải chờ cho đến khi chúng được kích hoạt bằng cách người dùng click.

```typescript
# app.routing.ts

imports: [
    RouterModule.forRoot(
        routes, 
        { 
            preloadingStrategy: PreloadAllModules 
        }
    )
],
```

<div align="center">
    <img src="https://images.viblo.asia/8dda8989-f6d5-47d6-8f5b-a01fd7fcf045.gif">
</div>

-Nhìn ảnh gif trên ta có thể thấy rằng, tất cả các file <code>*.chunk.js</code> đều được load ngay lập tức sau khi ứng dụng được khởi tạo.

### **a.3 Tùy chỉnh chiến lược tải**
- Sử dụng chiến lược Preload ở trên giúp chúng ta giải quyết được 2 vấn đều:

  - Nó sẽ cải thiện tốc độ load của ứng dụng bằng cách giảm kích thước của lần load đầu tiên
  - Tất cả các lazy load modules sẽ được load bất đồng bộ ngay sau khi ứng dụng được load xong, do vậy sẽ không có một delay nào đối với người dụng khi chuyển hướng sang bất cứ lazy load module nào.

- Tuy nhiên, preload tất cả các lazy load module không phải lúc nào cũng là sự lựa chọn đúng đắn. Đặc biệt đối với các thiết bị di động hay những kết nối băng thông thấp. Chúng ta có thể sẽ phải tải những modules mà người dùng có thể rất ít khi chuyển hướng đến. Tìm ra sự cân bằng cả về hiệu năng và trải nghiệm người dùng là chìa khóa cho việc phát triển.

- Ví dụ, trong ứng dụng này, chúng ta có 2 lazy load modules:
  - Settings module
  - Email module

- Đây là một ứng dụng email client nên module <code>Email</code> sẽ được sử dụng rất rất thường xuyên. Tuy nhiên module <code>Setings</code> sẽ được người dùng sử dụng nhưng với tuần suất rất thấp. Do vậy mà việc preload module Email sẽ đem lại hiệu quả cao, trong khi với module Setings thì thấp.

- Angular cung cấp một cách extend <code>PreloadingStrategy</code> để xác định một tùy chỉnh chiến lược Preload chỉ ra điều kiện cho việc preload các lazy load module. Chúng ta sẽ tạo một provider extend từ <code>PreloadingStrategy</code> để preload các modules mà có <code>preload: true</code> được xác định trong cấu hình route.

```typescript
# custom-preloading.ts

import 'rxjs/add/observable/of';
import { Injectable } from '@angular/core';
import { PreloadingStrategy, Route } from '@angular/router';
import { Observable } from 'rxjs/Observable';

@Injectable()
export class CustomPreloadingStrategy implements PreloadingStrategy {
  preloadedModules: string[] = [];

  preload(route: Route, load: () => Observable<any>): Observable<any> {
    if (route.data && route.data['preload']) {
      this.preloadedModules.push(route.path);
      return load();
    } else {
      return Observable.of(null);
    }
  }
}
```

- <code>CustomPreloadingStrategy</code> nên được đăng ký vào providers trong module mà <code>RouterModule.forRoot</code> được khai báo.

```typescript
# app.routing.ts

@NgModule({
  imports: [RouterModule.forRoot(routes, { preloadingStrategy: CustomPreloadingStrategy })],
  exports: [RouterModule],
  providers: [CustomPreloadingStrategy]
})
export class AppRoutingModule { }
```

- Cuối cùng, trong <code>app-routing..module.ts</code>, chúng ta thêm <code>data: { preload: true }</code> vào trong phần khai báo route của module muốn được custom preload:

```typescript
  {
    path: ':section',
    loadChildren: './gm-email/gm-email.module#GmEmailModule',
    data: { preload: true }
  }

```

<div align="center">
    <img src="https://images.viblo.asia/942c991d-3fa7-4194-b226-3c68a6d1eefe.gif">
</div>

- Nhìn vào tab nework, ta thấy <code>5.chunks.js</code> (module Email) đã được preload, còn <code>6.chunks.js</code> được load bất động bộ khi người dùng chuyển hướng (module Settings).

## **b. Eager loading**
- Các mô-đun tính năng trong Eager Loading sẽ được tải trước khi ứng dụng khởi động. Đây là chiến lược tải mô-đun mặc định.

## **c. Vậy ta nên sử dụng Lazy Loading và Eager Loading trong TH nào**

  **Đối với Eager Loading**
- **Trường hợp 1**: Ứng dụng kích thước nhỏ. Trong trường hợp này, không tốn kém khi tải tất cả các mô-đun trước khi ứng dụng khởi động và ứng dụng sẽ nhanh hơn và phản hồi nhanh hơn với các yêu cầu xử lý.

- **Trường hợp 2**: Các mô-đun cốt lõi và mô-đun tính năng được yêu cầu để khởi động ứng dụng. Các mô-đun này có thể chứa các thành phần của trang ban đầu, trình đánh chặn (để xác thực, ủy quyền và xử lý lỗi, v.v.), các thành phần phản hồi lỗi, định tuyến cấp cao nhất và bản địa hóa, v.v. Chúng ta chỉ cần háo hức tải các mô-đun này để làm cho ứng dụng hoạt động bình thường mặc dù kích thước ứng dụng.

**Đối với Lazy Loading**
- Kịch bản áp dụng Lazy Loading tương đối đơn giản và dễ hiểu. Trong một ứng dụng web có kích thước lớn, chúng ta có thể lười biếng tải tất cả các mô-đun khác không bắt buộc khi ứng dụng khởi động.

**Đối với chiến lược PreLoad**
- So với Eager Loading và Lazy Loading, Pre-Loading không được sử dụng thường xuyên trong phát triển ứng dụng web. Dựa trên sự hiểu biết của tôi về chiến lược tải này, Pre-Loading sẽ thuận lợi cho hai trường hợp.

  - **Trường hợp 1**: Ứng dụng kích thước trung bình. Trong trường hợp này, chúng ta có thể làm cho ứng dụng khởi động nhanh hơn vì nó sẽ tải tất cả các mô-đun khác sau này mà không bắt buộc phải chạy ứng dụng. Và ứng dụng sẽ đáp ứng nhiều hơn với các yêu cầu của người dùng xử lý hơn là áp dụng chiến lược Lazy Loading vì ứng dụng sẽ tải tất cả các mô-đun này sau khi ứng dụng khởi động.

  - **Trường hợp 2**: Một số mô-đun cụ thể mà người dùng rất có thể sử dụng sau khi ứng dụng khởi động. Trong trường hợp này, chúng tôi có thể tải trước các mô-đun tính năng này và vẫn lười tải các mô-đun khác. 


## **d. SUMMARY**
- [Giới thiệu về các loại loading](https://medium.com/@lifei.8886196/eager-loading-lazy-loading-and-pre-loading-in-angular-2-what-when-and-how-798bd107090c)
- [Angular - Cải thiện hiệu năng và trải nghiệm người dùng với Lazy Loading](https://viblo.asia/p/angular-cai-thien-hieu-nang-va-trai-nghiem-nguoi-dung-voi-lazy-loading-djeZ1BkRlWz)
