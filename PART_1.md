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
<p>Một <b>LifeCycle</b> của <b>Component</b> bắt đầu khi Angular khởi tạo <b>class Component</b> và hiển thị dạng xem thành phần cùng với các chế độ xem con của nó. <b>LifeCycle</b> tiếp tục với tính năng phát hiện thay đổi, khi Angular kiểm tra xem khi nào các thuộc tính ràng buộc dữ liệu thay đổi và cập nhật cả chế độ xem và phiên bản thành phần nếu cần. <b>LifeCycle</b> kết thúc khi Angular phá hủy Component và xóa mẫu đã kết xuất khỏi DOM. Các lệnh có vòng đời tương tự, vì Angular tạo, cập nhật và phá hủy các phiên bản trong quá trình thực thi.</p>
 <p>Ứng dụng của bạn có thể sử dụng các phương thức móc vòng đời(<b>LifeCycle Hook Method</b>) để khai thác các sự kiện chính trong vòng đời của một cấu phần hoặc lệnh nhằm khởi tạo các phiên bản mới, bắt đầu phát hiện thay đổi khi cần, phản hồi các bản cập nhật trong quá trình phát hiện thay đổi và dọn dẹp trước khi xóa phiên bản.</p>
 <p>Một vòng đời của Component sẽ có thứ tự như sau: OnChanges => OnInit => DoCheck => AfterContentInit => AfterContentChecked => AfterViewInit => AfterViewChecked => OnDestroy.</p>
 <div align="center">
  <img src="https://images.viblo.asia/3e8bca05-06d1-4999-98a6-628bd57a7c21.png">
</div>

#### Các Menthod và chức năng của từng Method
**1. <code>ngOnChanges()</code>**
- Phản hồi khi Angular đặt hoặc đặt lại các thuộc tính đầu vào liên kết dữ liệu. Phương thức nhận một <code>SimpleChanges</code> đối tượng có giá trị thuộc tính hiện tại và trước đó.
> NOTE: Điều này xảy ra thường xuyên, do đó, bất kỳ thao tác nào bạn thực hiện ở đây đều ảnh hưởng đáng kể đến hiệu suất.
<br>

- Được gọi trước <code>ngOnInit()</code> (nếu thành phần có đầu vào ràng buộc) và bất cứ khi nào một hoặc nhiều thuộc tính đầu vào ràng buộc dữ liệu thay đổi.
> NOTE: Nếu thành phần của bạn không có đầu vào hoặc bạn sử dụng nó mà không cung cấp bất kỳ đầu vào nào, khung sẽ không gọi ngOnChanges().

- Code Example:

```typescript
import { Component, Input, OnChanges } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
  <h3>Child Component</h3>
  <p>TICKS: {{ lifecycleTicks }}</p>
  <p>DATA: {{ data }}</p>
  `
})
export class ChildComponent implements OnChanges {
  @Input() data: string;
  lifecycleTicks: number = 0;

  ngOnChanges() {
    this.lifecycleTicks++;
  }
}

@Component({
  selector: 'app-parent',
  template: `
  <h1>ngOnChanges Example</h1>
  <app-child [data]="arbitraryData"></app-child>
  `
})
export class ParentComponent {
  arbitraryData: string = 'initial';

  constructor() {
    setTimeout(() => {
      this.arbitraryData = 'final';
    }, 5000);
  }
}
```

- Tóm tắt: ParentComponent liên kết dữ liệu đầu vào với ChildComponent. Thành phần nhận dữ liệu này thông qua thuộc tính của nó <code>@Input</code>.<code>ngOnChanges</code> được thực thi. Sau năm giây, <code>setTimeout</code> gọi lại sẽ kích hoạt. ParentComponent thay đổi nguồn dữ liệu của thuộc tính đầu vào của ChildComponent. Dữ liệu mới chảy qua thuộc tính đầu vào.<code>ngOnChanges</code> được gọi 1 lần nữa.

**2. <code>ngOnInit()</code>**
- Khởi tạo <code>Directive</code> hoặc <code>Component</code> sau khi Angular lần đầu tiên hiển thị các thuộc tính liên kết dữ liệu và đặt các thuộc tính đầu vào của <code>Directive</code> hoặc <code>Component</code>.
- Được gọi một lần, sau lần gọi <code>ngOnChanges()</code> đầu tiên. <code>ngOnInit()</code> vẫn được gọi ngay cả khi <code>ngOnChanges()</code> không được gọi tới (đó là trường hợp không có đầu vào giới hạn mẫu).
- Code Example:

```typescript
import { Component, Input, OnInit } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
  <h3>Child Component</h3>
  <p>TICKS: {{ lifecycleTicks }}</p>
  <p>DATA: {{ data }}</p>
  `
})
export class ChildComponent implements OnInit {
  @Input() data: string;
  lifecycleTicks: number = 0;

  ngOnInit() {
    this.lifecycleTicks++;
  }
}

@Component({
  selector: 'app-parent',
  template: `
  <h1>ngOnInit Example</h1>
  <app-child [data]="arbitraryData"></app-child>
  `
})
export class ParentComponent {
  arbitraryData: string = 'initial';

  constructor() {
    setTimeout(() => {
      this.arbitraryData = 'final';
    }, 5000);
  }
}
```

- Tóm tắt: ParentComponent liên kết dữ liệu đầu vào với ChildComponent. Thành phần nhận dữ liệu này thông qua thuộc tính của nó <code>@Input</code>.<code>ngOnInit</code> được thực thi. Sau năm giây, <code>setTimeout</code> gọi lại sẽ kích hoạt. ParentComponent thay đổi nguồn dữ liệu của thuộc tính đầu vào của ChildComponent. Dữ liệu mới chảy qua thuộc tính đầu vào.<code>ngOnChanges</code> không được gọi lại nữa.

**3. <code>ngDoCheck()</code>**
- Phát hiện và hành động theo những thay đổi mà Angular không thể hoặc sẽ không tự phát hiện.
- Được gọi ngay sau <code>ngOnChanges()</code> mỗi lần chạy phát hiện thay đổi và ngay sau lần chạy <code>ngOnInit()</code> đầu tiên.
- Code Example:

```typescript
import { Component, DoCheck, ChangeDetectorRef } from '@angular/core';

@Component({
  selector: 'app-example',
  template: `
  <h1>ngDoCheck Example</h1>
  <p>DATA: {{ data[data.length - 1] }}</p>
  `
})
export class ExampleComponent implements DoCheck {
  lifecycleTicks: number = 0;
  oldTheData: string;
  data: string[] = ['initial'];

  constructor(private changeDetector: ChangeDetectorRef) {
    this.changeDetector.detach(); // lets the class perform its own change detection

    setTimeout(() => {
      this.oldTheData = 'final'; // intentional error
      this.data.push('intermediate');
    }, 3000);

    setTimeout(() => {
      this.data.push('final');
      this.changeDetector.markForCheck();
    }, 6000);
  }

  ngDoCheck() {
    console.log(++this.lifecycleTicks);

    if (this.data[this.data.length - 1] !== this.oldTheData) {
      this.changeDetector.detectChanges();
    }
  }
}
```

- Tóm tắt: Lớp khởi tạo sau hai vòng phát hiện thay đổi. Trình tạo lớp bắt đầu <code>setTimeout</code> hai lần. Sau ba giây,<code>setTimeout</code> phát hiện thay đổi trình kích hoạt đầu tiên. <code>ngDoCheck</code> đánh dấu màn hình để cập nhật. Ba giây sau, <code>setTimeout</code> phát hiện thay đổi kích hoạt lần thứ hai. Không cần cập nhật chế độ xem theo đánh giá của <code>ngDoCheck</code>.

**4. <code>ngAfterContentInit()</code>**
- Phản hồi sau khi Angular chiếu nội dung bên ngoài vào <code>View</code> của <code>Component</code>hoặc vào <code>View</code> của <code>Directive</code>.
- Được gọi một lần sau lần đầu tiên <code>ngDoCheck()</code> thực thi.
- <code>ngAfterContentInit</code> kích hoạt sau khi DOM content của <code>Component</code> khởi tạo(tải lần đầu tiên). Chờ <code>@ContentChild(ren)</code> truy vấn là trường hợp sử dụng chính của hook. <code>@ContentChild(ren)</code> truy vấn mang lại tham chiếu phần tử cho nội dung DOM. Như vậy, chúng không khả dụng cho đến sau khi tải nội dung DOM. Do đó tại sao <code>ngAfterContentInit</code> và đối tác của nó <code>ngAfterContentChecked</code> được sử dụng.

```typescript
import { Component, ContentChild, AfterContentInit, ElementRef, Renderer2 } from '@angular/core';

@Component({
  selector: 'app-c',
  template: `
  <p>I am C.</p>
  <p>Hello World!</p>
  `
})
export class CComponent { }

@Component({
  selector: 'app-b',
  template: `
  <p>I am B.</p>
  <ng-content></ng-content>
  `
})
export class BComponent implements AfterContentInit {
  @ContentChild("BHeader", { read: ElementRef }) hRef: ElementRef;
  @ContentChild(CComponent, { read: ElementRef }) cRef: ElementRef;

  constructor(private renderer: Renderer2) { }

  ngAfterContentInit() {
    this.renderer.setStyle(this.hRef.nativeElement, 'background-color', 'yellow')

    this.renderer.setStyle(this.cRef.nativeElement.children.item(0), 'background-color', 'pink');
    this.renderer.setStyle(this.cRef.nativeElement.children.item(1), 'background-color', 'red');
  }
}

@Component({
  selector: 'app-a',
  template: `
  <h1>ngAfterContentInit Example</h1>
  <p>I am A.</p>
  <app-b>
    <h3 #BHeader>BComponent Content DOM</h3>
    <app-c></app-c>
  </app-b>
  `
})
export class AComponent { }
```

**5. <code>ngAfterContentChecked()</code>**
- Phản hồi sau khi Angular kiểm tra nội dung được chiếu vào <code>Directive</code> và <code>Component</code>.
- Được gọi sau <code>ngAfterContentInit()</code> và mỗi lần <code>ngDoCheck()</code> được gọi.

```typescript
import { Component, ContentChild, AfterContentChecked, ElementRef, Renderer2 } from '@angular/core';

@Component({
  selector: 'app-c',
  template: `
  <p>I am C.</p>
  <p>Hello World!</p>
  `
})
export class CComponent { }

@Component({
  selector: 'app-b',
  template: `
  <p>I am B.</p>
  <button (click)="$event">CLICK</button>
  <ng-content></ng-content>
  `
})
export class BComponent implements AfterContentChecked {
  @ContentChild("BHeader", { read: ElementRef }) hRef: ElementRef;
  @ContentChild(CComponent, { read: ElementRef }) cRef: ElementRef;

  constructor(private renderer: Renderer2) { }

  randomRGB(): string {
    return `rgb(${Math.floor(Math.random() * 256)},
    ${Math.floor(Math.random() * 256)},
    ${Math.floor(Math.random() * 256)})`;
  }

  ngAfterContentChecked() {
    this.renderer.setStyle(this.hRef.nativeElement, 'background-color', this.randomRGB());
    this.renderer.setStyle(this.cRef.nativeElement.children.item(0), 'background-color', this.randomRGB());
    this.renderer.setStyle(this.cRef.nativeElement.children.item(1), 'background-color', this.randomRGB());
  }
}

@Component({
  selector: 'app-a',
  template: `
  <h1>ngAfterContentChecked Example</h1>
  <p>I am A.</p>
  <app-b>
    <h3 #BHeader>BComponent Content DOM</h3>
    <app-c></app-c>
  </app-b>
  `
})
export class AComponent { }
```

- Tóm tắt: Kết xuất bắt đầu với AComponent. Để hoàn thành, AComponent phải kết xuất BComponent. BComponent dự án nội dung được lồng trong phần tử của nó thông qua<code><ng-content></ng-content></code>phần tử. CComponent là một phần của nội dung dự kiến. Nội dung được chiếu kết thúc hiển thị. BComponent kết thúc kết xuất.<code>ngAfterContentChecked</code> được gọi AComponent kết thúc kết xuất.<code>ngAfterViewChecked</code> có thể kích hoạt lại thông qua phát hiện thay đổi.
