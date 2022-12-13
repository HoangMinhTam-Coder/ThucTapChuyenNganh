# Chủ đề tuần 1:
- Angular Module
- Angular Component
- Angular lifecyle hook

## a. Angular Module:
<p>Các ứng dụng Angular là mô-đun và Angular có hệ thống mô-đun riêng được gọi là <b>NgModules</b>. <b>NgModules</b> là các vùng chứa cho một khối mã gắn 
kết dành riêng cho miền ứng dụng, quy trình làm việc hoặc một tập hợp các khả năng liên quan chặt chẽ. Chúng có thể chứa các Component, Service và các tệp mã khác có phạm vi được xác định bởi <b>NgModule</b> chứa. Họ có thể nhập chức năng được xuất từ các <b>NgModules</b> khác và xuất chức năng đã chọn để sử dụng bởi các <b>NgModules</b> khác.</p>
 
<p>Mỗi ứng dụng Angular có ít nhất một lớp NgModule, <b>the <i>root module</i></b>, được đặt tên theo quy ước và nằm trong một tệp có tên . Bạn khởi chạy ứng dụng của mình bằng cách khởi động root NgModule.<b><code>AppModule app.module.ts</code></b>
<br>
Mặc dù một ứng dụng nhỏ có thể chỉ có một NgModule, nhưng hầu hết các ứng dụng đều có nhiều mô-đun tính năng hơn. NgModule gốc cho một ứng dụng được đặt tên như vậy vì nó có thể bao gồm NgModules con trong một hệ thống phân cấp có độ sâu bất kỳ.</p>

<p>Ngay khi bắt đầu khởi tạo một dự án Angular, chúng ta đã thấy ngay một module mặc định đó là AppModule, hãy cùng xem nó có gì nhé</p>

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
@NgModule({
  imports:      [ BrowserModule ],
  providers:    [ Logger ],
  declarations: [ AppComponent ],
  exports:      [ AppComponent ],
  bootstrap:    [ AppComponent ]
})
export class AppModule { }
```

- <b>declarations</b>: Dùng để khai báo những thành phần chúng ta sẽ dùng ở trên template (thường chủ yếu là các component, directive và pipe).
- <b>providers</b>: Dùng để khai báo các service dùng trong toàn bộ các module của con (dù có lazy loading module hay không vẫn available).
- <b>imports</b>: Nó là một mảng các module cần thiết để được sử dụng trong ứng dụng. Nó cũng có thể được sử dụng bởi các Component trong mảng Declarations. Ví dụ: trong @NgModule, chúng ta thấy BrowserModule và AppRoutingModule được import
- <b>bootstrap</b>: Định nghĩa component gốc của module

> <b>NOTE</b>: Kể từ version của Angular 6, các service không cần đăng kí ở trong module mà chúng ta có thể sử dụng từ khóa providedIn: ‘root’ để xác định tầm ảnh hưởng của service, khi sử dụng cú pháp này mặc định service có thể sử dụng bất cứ đâu trong app, nó tương ứng với việc service đó được import ngay ở AppModule.
- Khởi tạo module như thế nào Để tạo ra một module chúng ta có thể tạo thủ công bằng tay, hoặc sử dụng Angular CLI bằng cú pháp:

```typescript
ng generate module <ModuleName> | ng g m <ModuleName>
EX: ng generate module Teacher
```

- <b>Feature Module</b>: Gom các component hoặc service có liên quan đến nhau hoặc cùng nằm trong một feature nào đó thành một nhóm.

<br>Có những loại module nào?
- modules of pages.
- modules of global services.
- modules of reusable components.
<div align="center">
  <img src="https://images.viblo.asia/2149bd31-9b53-40bc-b404-26b49dab7224.jpg">
</div>

- Việc tách ra các module có một tầm ảnh hưởng rất lớn đến việc phát triển một dự án angular nếu phân chia nó hợp lý và khoa học, một dự án sẽ phát triển dễ dàng, dễ bảo trì, dễ tiếp cận. Khi khỏi tạo một dự án hãy xem xét kĩ, và nên tạo ra một rule thống nhất trong quá trình phát triển để khi mỗi người tạo một module hay thêm những component sẽ không làm phá vỡ các quy tắc mọi người đang làm.

## b. Angular Component
<p>Một thành phần điều khiển một mảng màn hình được gọi là chế độ xem. Nó bao gồm của lớp TypeScript, mẫu HTML và biểu định kiểu CSS. Lớp TypeScript định nghĩa tương tác của mẫu HTML và cấu trúc DOM được kết xuất, trong khi biểu định kiểu mô tả giao diện của nó.</p>
- Khởi tạo 1 component bằng Angular CLI:

```typescript
ng generate component <ComponentName> | ng g c <ComponentName>
EX: ng g c DetailPage
```

Chú ý bạn có thể thêm:
- Option <b>-t</b> nếu không muốn tạo ra file html. Sử dụng template ngay trong component
- Option <b>--inline-style</b> nếu không muốn tạo ra file style. Sử dụng style ngay trong component
- Option <b>--skipTests</b> nếu không muốn tạo ra file Test(spec.ts) trong component
<br>
- Mặc định ngay khi khởi tạo 1 ứng dụng Angular bằng Angular CLI thì ta sẽ có sắn 1 Component là <b>App Component</b> trong thư mục <b>src/app</b>

```typescript
import { Component } from '@angular/core';

@Component({
    // Khai báo selector cho component, có thể gọi đến selector này giống như thẻ html (<app-root></app-root>)
    selector: 'app-root',
    // Khai báo file html mà component "đại diện" (hay còn gọi là view/template của Component)
    templateUrl: './app.component.html',
    // Khai báo file style mà component này sẽ sử dụng
    styleUrls: ['./app.component.css']
})
export class AppComponent {
    title = 'app';
}

```

- AppComponent là thành phần cha của ứng dụng. Tất cả các thành phần mới tạo ra sau này đều là thành phần con của AppComponent.

## c. Angular lifecyle hook
