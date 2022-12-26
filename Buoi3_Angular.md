# **Chủ đề tuần 3**

- Template and Data Binding
- Directive
- Pipe
- Services & Dependency Injection

## **a. Template and Data Binding**

### **a.1 Template:**

### **a.2 Data Binding**

- So, what is data binding? Hãy thử tưởng tượng rằng công ty X có anh nhân viên A, anh này có vợ là chị B, bằng một cách nào đó cứ đến kỳ chuyển lương thì chị B sẽ nhận được thông báo về điện thoại của mình rằng số tiền trong tài khoản của anh A đã được công ty X trả Y triệu. Tương tự như thế trong lập trình khi chúng ta có data D thuộc object O sẽ được ràng buộc với data G thuộc object OP, bằng một cách nào đó, mỗi khi data D thay đổi thì G cũng sẽ biết sự thay đổi và thay đổi theo. Đó là một dạng của data binding. Trong Angular chúng ta đã được Angular làm nhiệm vụ đồng bộ những dữ liệu bị ràng buộc lại với nhau, chúng ta chỉ cần update data/state cần thiết. Vậy có những dạng data binding nào?

#### **a.2.1 INTERPOLATION**

- Bây giờ hãy nhớ lại, một component trong Angular chỉ là một TypeScript (TS) class thông thường, và nó đi kèm với một decorator để gắn thêm meta-data như template mà nó định nghĩa là gì. Như vậy, TS class và template này hoàn toàn không biết đến nhau, mà nó sẽ được Angular xử lý để gắn chúng lại. Câu hỏi đặt ra là làm thế nào để tôi hiển thị một dữ liệu nào đó (tên, tuổi, ngày tháng năm sinh, một string, number bất kỳ, hay bất kỳ thứ gì (object) có thể hiển thị được? Đây chính là nơi tỏa sáng của cặp đôi hoàn cảnh (mà chúng ta gọi là interpolation) <code>{{ expression }}</code>. Nó có thể hiểu là hãy tính toán cái expression này, nếu có trả về cái gì thì phun (display) nó ra ngay vị trí chỗ dấu {{}} này cho tôi. Chỉ đơn giản thế. Giờ các bạn có thể phun data về tên tuổi của một người thành cái profile đơn giản như sau:

```typescript
import { Component } from "@angular/core";
@Component({
  selector: "app-hello",
  template: `
    <h2>Hello there!</h2>
    <h3>Your name: {{ user.name }}</h3>
    <p>Your name: {{ user.age }}</p>
  `,
})
export class HelloComponent {
  user = {
    name: "Minh Tam",
    age: 28,
  };
}
```

#### **a.2.2 PROPERTY BINDING**

- Một số tag khi sử dụng bạn cần phải truyền vào data cho một property nào đó. Đối với DOM chúng ta có 2 khái niệm khác nhau là property và attribute. À mà khoan DOM là gì? Khi browser load trang web của bạn, nó sẽ parse phần HTML và xây dựng nên một cây **Document Object Model (DOM)** từ đó để biểu diễn tương ứng những gì HTML đang được dựng, cho phép chúng ta có thể tương tác với phần HTML như đọc, sửa HTML bằng JavaScript. Giả sử khi bạn có phần HTML: Sau khi parse xong sẽ có một object (node) thuộc type <code>HTMLInputElement</code> được tạo ra. Ở đây <code>type="text"</code> hay <code>value="something"</code> là các HTML attribute. Mỗi tag HTML có thể có nhiều attribute khác nữa (xin mời bạn Search Google). Object được tạo tương ứng sẽ có dạng

```typescript
obj = {
  type: "text",
  value: "something",
  attributes: [], // thuộc type NamedNodeMap, dạng như một array
  // … các thuộc tính, method khác
};
```

- Như bạn có thể thấy, attribute được dùng để chỉ những gì các bạn đặt vào phần opening tag của một tag, còn lại là property của object (node). Các attributes sẽ được lưu trữ vào property attributes của node tương ứng. Có những attribute sẽ được map sang thành property tương ứng, ví dụ như type và value ở trên, nhưng có những attribute không được map sang ví dụ class sẽ thành className, chúng không có quan hệ 1-1 với nhau. Thực tế khi sử dụng Angular, chúng ta sẽ muốn ứng dụng trở nên linh động hơn (ví dụ: cùng là 1 template nhưng có thể thay đổi data để hiển thị). Lúc này chúng ta cần update các properties của DOM để làm cho nó đáp ứng chẳng hạn. Nhưng nếu cần phải dùng document.querySelector/jQuery để manipulate DOM thì Angular quá vô vị. Phải có cách gì đó hay ho hơn chứ nhỉ? Đó chính là đất diễn của property binding. Chúng ta chỉ cần sử dụng một cú pháp để báo cho Angular biết chúng ta cần binding một property như sau ở template.

```typescript
<input type="text" [value]="user.name" />
```

- Trong dự án Angular, bạn hoàn toàn có thể hiểu <code>type="text"</code> lúc này cũng là một property binding, thay vì <code>[type]="'text'"</code> (để ý có 1 cặp nháy đơn và 1 cặp nhảy đôi), nó đang biểu diễn binding cho một hằng giá trị (literal), lúc này bạn hoàn toàn có thể bỏ qua mấy dấu vuông vuông kia đi. Nhưng trong hầu hết các trường hợp còn lại, bạn sẽ dùng dấu vuông vuông <code>[property]="expression"</code> để thực hiện khai báo property binding. Trong ví dụ ở trên chúng ta đã binding từ TS class ra ngoài template, và khi dữ liệu ở class thay đổi, Angular sẽ tự động làm nhiệm vụ update template để hiển thị tương ứng sự thay đổi đó cho chúng ta.

> Lưu ý: ngoài property binding cho các phần tử HTML, chúng ta cũng có thể áp dụng property binding cho các component.

#### **a.2.3 EVENT BINDING**

- JavaScript sử dụng rất nhiều đến khái niệm event. Khi một điều gì đó xảy ra, chúng ta muốn thực hiện một số task nào đó. Ví dụ, khi người dùng click vào button, tôi muốn hiển thị alert cho người dùng nhìn thấy. Angular có cách nào chính thống cho việc này không đây, hay chúng ta sẽ dùng addEventListener như thường? Câu trả lời chính là Event binding. Để gắn event listener vào một phần tử nào đó ở trên template, chúng ta có thể sử dụng cú pháp ngoặc tròn tròn như sau:

```typescript
@Component({
  selector: "app-hello",
  template: `
    <h2>Hello there!</h2>
    <button (click)="showInfo()">Click me!</button>
  `,
})
export class HelloComponent {
  showInfo() {
    alert("Inside Angular Component method");
  }
}
```

- Chỉ là lúc này chúng ta sẽ trỏ đến method bên trong Component. Khá là giống như chúng ta sử dụng inline event listener đó.

```typescript
<button onclick="showInfo()">Click me!</button>
```

#### **a.2.4 TWO-WAY BINDING**

- Trong thực tế two-way binding chính là kết hợp của binding dữ liệu từ class ra template thông qua property binding, và từ template vào class thông qua event binding.
- Nó chứa cú pháp ngắn gọn dạng vuông vuông tròn tròn như sau:

```typescript
<input type="text" [(ngModel)]="user.name" >
```

- Để sử dụng ngModel bạn cần imports <code>FormsModule</code>, nhưng trong bài này chúng ta chỉ cần hiểu, nó là cách viết tắt của dạng tương ứng là:

```typescript
<input type="text" [ngModel]="user.name" (ngModelChange)="user.name = $event">
```

#### **a.2.5 CLASS BINDING**

- Trong các ứng dụng thực tế, có thể chúng ta cần thay đổi (thêm, xóa) một số class tùy thuộc vào một số điều kiện nào đó.
- Ví dụ, nếu chúng ta đang chọn một tab nào đó để hiển thị, thì tab đó sẽ có thêm class tab-active, các tab khác sẽ không có. Lúc này chúng ta sẽ sử dụng cú pháp:

```typescript
<div [class.tab-active]="isTabActive">
		some content
</div>
```

- Nhìn qua thì nó chỉ là property binding, với giá trị của <code>isTabActive</code> trả về <code>true</code> thì classList của div đó sẽ tồn tại class <code>tab-active</code>, còn nếu trả về <code>false</code> thì sẽ không tồn tại.
- Ngoài cú pháp trên chúng ta có thể dùng:

```typescript
[class]="classExpr"
```

- Với <code>classExpr</code> có thể là <code>string, array string hoặc object</code> – **nếu key nào của object là truthy thì sẽ thêm vào, nếu falsy thì sẽ xóa đi**.

- Ví dụ các dạng của classExpr:

```typescript
String: "my-class-1 my-class-2 my-class-3"
Array String: ['foo', 'bar']
Object: {foo: true, bar: false}
```

- Tương tự với class binding chúng ta có thể sử dụng <code>ngClass</code>, nhưng hiện tại cũng có thể nói rằng <code>ngClass</code> không có gì khác biệt với <code>[class]="classExpr"</code>. Và cách sử dụng Class Binding vẫn được khuyến cao sử dụng hơn ngClass.

- Ví dụ về <code>ngClass</code>

```typescript
// app.component.ts
  import { Component } from '@angular/core';

  @Component({
      selector: 'app-example',
      template: `
          <div [ngClass]='classes'>
              ...
          </div>
          <div [ngClass]='setClasses()'>
              ...
          </div>
      `
  })
  export class AppComponent {
      ...
      classes = {
          alert-danger: this.isDanger,
          inactive: !this.isActive,
          ...
      };

      setClasses() {
          let classes = {
              alert-danger: this.isDanger,
              inactive: !this.isActive,
              ...
          };

          return classes;
      }
  }
```

#### **a.2.6 STYLE BINDING**
Có thể khi cần thiết, chúng ta cần binding cho style property (inline style), lúc này chúng ta có thể sử dụng Style binding.

**Cấu trúc của style binding như sau:**

```typescript
[style.property]="expression"
```

- Với <code>expression</code> sẽ tính toán về các giá trị kiểu <code>string | undefined | null</code>
- VD

```typescript
<div [style.width]="someValue"></div>
```

**Tiếp theo là cú pháp kèm theo unit:**

```typescript
[style.property.unit]="expression"
```

- Với <code>expression</code> sẽ tính toán về các giá trị kiểu <code>number | undefined | null</code>
- VD

```typescript
[style.height.%]="containerHeight"
```

**Cuối cùng là cú pháp dạng:**

```typescript
[style]="styleExpr"
```

- Với <code>styleExpr</code> là một trong các dạng:

```typescript
String: "width: 100%; height: 100%"
Array String: ['width', '100px']
Object: {[key: string]: string | undefined | null} ex {width: '100px', height: '100px'}
```

- Có một directive tương tự là <code>ngStyle</code> với cách dùng gấn giống thế, trong hầu hết các trường hợp, chúng ta được khuyến cáo sử dụng style binding thay thế.
- **Lưu ý rằng, một style property có thể dùng cả kiểu dash-key hoặc camelCase, ví dụ font-size hoặc fontSize đều được**
- VD

```typescript
// app.component.ts
  import { Component } from '@angular/core';

  @Component({
      selector: 'app-example',
      template: `
          <button [ngStyle]='multipleStyles'>Click me!</button>
          <button [ngStyle]='getStyles()'>Cancel</button>
      `
  })
  export class AppComponent {
      multipleStyles = {
          'background-color': 'red',
          'foreground-color': 'white',
          'font-size': '20px',
          'width': '120px',
          'height': '30px'
      }

      getStyles() {
          'background-color': 'white',
          'foreground-color': 'cyan',
          'font-size': '20px',
          'width': '120px',
          'height': '30px'
      }
  }
```

- Anh mota Notion

<br>

### **b. Directive**
- Directives là một đối tượng giúp chúng ta dễ dàng thay đổi một đối tượng khác và cách áp dụng rất đơn giản và linh hoạt. Directives có thể hiểu như là các đoạn mã typescript (hoặc javascript) kèm theo cả HTML và khi gọi thì gọi như là HTML luôn. Ví dụ:

```typescript
<!-- Chỗ này là gọi directive *ngIf để kiểm tra điều kiện if ngay ở html -->
<div *ngIf="time">
  Time: {{ time }}
</div>
```

#### **b.1 Phân loại Directive**
- Từ Angular 2 trở đi, directives được chia làm các loại sau đây:
  1. **Components directives**: Không có nghi ngờ gì khi gọi component là directive cũng được, vì rõ ràng là component cho phép định nghĩa selector và gọi ra như một thẻ html tag (<code>`<component-name></component-name>`</code>)
  2. **Structural directives**: Là directive cấu trúc, dùng để vẽ html, hiển thị data lên giao diện html, và thay đổi cấu trúc DOM bằng việc thêm bớt các phần tử trong DOM. Các structural directive thường có dấu '**`*`**' ở trước của directive. Ví dụ <code>`*ngFor, *ngIf`</code>
  3. **Attribute directives**: Thay đổi giao diện, tương tác của các đối tượng hoặc thay đổi directive khác hoặc thêm các thuộc tính động cho element html. ví dụ <code>*ngStyle</code>

<br>

  <i>**b.1.1 Components directives**</i>
  
  - Components directives được sử dụng rất phổ biến , bạn có xem tại đây [Components directives](https://angular.io/start). Sau đây, mình sẽ trình bày ngắn gọn về directives này.

  - Components là một khối code trong app Angular. Nó là sự kết hợp của bộ template html (bộ khung html) và nhúng kèm code TypeScript (hoặc Javascript). Các components là độc lập với nhau và độc lập với hệ thống. Nó có thể được cài vào hoặc tháo ra khỏi hệ thống dễ dàng. Một component có thể hiểu như một control trên màn hình hiển thị, gồm giao diện html và code logic xử lý sự kiện đi kèm control đó. Một component cũng có thể to lớn như là cả 1 màn hình chứa nhiều control hoặc một nhóm nhiều màn hình. Tức là là một component cũng có thể chứa và gọi được nhiều component khác nối vào. Như vậy Angular rất linh hoạt trong việc chia nhỏ code ra các component.
  - Trong Angular chúng ta khai báo một Component với cấu trúc như sau:

  ```typescript
  import { Component } from '@angular/core';
  @Component({
      selector: 'app-hello-world',
      template: `<h1>Hello Angular world</h1>`
  })
  export class HelloWorld { 
  }
  ```

  - Như chúng ta thấy từ khóa **@Component** sẽ giúp định nghĩa ra một bộ khung html cho nó. Và bên dưới là một class HelloWorld dùng để viết code logic. Trong định nghĩa bộ khung html, chúng ta có một số thuộc tính cần chú ý sau đây:
  
    - **selector** : Là tên được đặt để gọi một component trong code html. Ở ví dụ vừa rồi, từ khóa **app-hello-world** được đặt tên cho component này. Khi cần gọi component này ra ở màn hình html cha, ta sẽ gọi bằng html tag <code>`<app-hello-world></app-hello-world>`</code>. Gọi như vậy thì component con sẽ được render ra component cha.
    - **template** : Là tự định nghĩa khung html cho component dạng string ở trong file này luôn. Ví dụ ở trên chỉ định nghĩa một thẻ html h1 đơn giản. Cách này chỉ dùng cho component đơn giản.
    - **templateUrl** : Là đường dẫn url tới file html bên ngoài để load file đó vào làm khung html cho component này. Đây là cách code hay được dùng vì cho phép tách riêng khung html ra khỏi code logic, người làm design sẽ sửa file html riêng, độc lập với người làm code.
    - **styles** : Là viết style css luôn vào file component này. Cách này chỉ dùng cho component đơn giản.
    - **styleUrls** : Là đường dẫn url đến file style css độc lập cho component này. Cách này khuyên dùng vì file css nên để dành riêng cho người designer đụng vào.
    
  <i>**b.1.2 Structural directives**</i>
  
  - Sau đây, mình sẽ trình bày một vài structural directives cơ bản và thông dụng. Ngoài ra bạn có thể tham khảo và xem chi tiết tại đây [Stuctural directives](https://angular.io/guide/structural-directives).
  
    **Ng-If .. else**
    
    - Có tác dụng kiểm tra điều kiện ngay ở html. Ví dụ:

    ```typescript
      <div *ngIf="time; else noTime">
          Time: {{time}}
      </div>
      <ng-template #noTime> No time. </ng-template>
    ```

    - Code ở trên, khi biến time có giá trị, thì chuỗi Time: [value] được show ra. Và cục #noTime template bị ẩn đi, ngược lại thì điều kiện else được chạy và #noTime được hiện ra.

    <br>
    
    **Ng-Template**

    - Nó giúp gom cục html cần ẩn hiện.
    ```typescript
      <div *ngIf="isTrue; then tmplWhenTrue else tmplWhenFalse"></div>
      <ng-template #tmplWhenTrue >I show-up when isTrue is true. </ng-template>
      <ng-template #tmplWhenFalse > I show-up when isTrue is false </ng-template>
    ```

    - Cách viết này đầy đủ hơn của **Ng-if… else**

    <br>
    
    **Ng-Container**

    - Tương tự như **Ng-Template** dùng để gom html. Nhưng điểm mạnh của **Ng-Container** là thẻ directive này không render ra tag <code>`<ng-container>`</code> html như là <code>`<ng-template>`</code> mà tag sẽ được ẩn đi, giúp cho layout css không bị vỡ nếu bạn gom html (Không sợ bị nhảy từ div cha sang div con, cấu trúc html không hề thay đổi khi gom vào tag <code>`<ng-container></ng-container>`</code>)
    - Xét ví dụ sau:

    ```typescript
    Welcome <div *ngIf="title">to <i>the</i> {{title}} world.</div>
    ```
    - Sẽ được render ra như sau:

    <div align="center">
      <img src="https://images.viblo.asia/01e013fa-2936-4ace-8b5c-bde8a502b33c.png">
    </div>

    - Khi soi html chúng ta sẽ thấy:

    <div align="center">
      <img src="https://images.viblo.asia/8c96f0ec-a122-4288-b5e6-537d3fefa3f3.png">
    </div>

    - Tự dưng dòng div có ngIf nó lại chèn một cái thuộc tính _ngcontent-c0, dẫn đến dòng đó bị xuống dòng, làm sai layout design.
    - Bây giờ hãy viết lại như sau:
    
    ```typescript
    Welcome <ng-container *ngIf="title">to <i>the</i> {{title}} world.</ng-container>

    ```

    - Kết quả sẽ thật đẹp ngay:
    <div align="center">
      <img src="https://images.viblo.asia/fdd341e2-a545-47f4-9548-f2e027d28286.png">
    </div>

    - Đó là vì html đã được dọn gọn gàng:
    <div align="center">
      <img src="https://images.viblo.asia/42435ccd-5905-4e32-9b09-c25a30d5b4d0.png">
    </div>

    <br>

    **NgSwitch and NgSwitchCase**
    - Chúng ta hoàn toàn có thể sử dụng câu lệnh điều kiện switch case trong Angular y như switch case trong Javascript vậy.

      ```typescript
        <div [ngSwitch]="isMetric">
          <div *ngSwitchCase="true">Degree Celsius</div>
          <div *ngSwitchCase="false">Fahrenheit</div>
          <div *ngSwitchDefault>Default </div>
        </div>
      ```
    
    - <code>`*ngSwitchDefault`</code> Trong trường hợp muốn dùng switch case default (nếu toàn bộ case k thỏa màn thì vào default).
  
  <i>**b.1.3 Attribute directive**</i>
  
  - Được dùng như một thuộc tính của đối tượng, cho nên khi build thì directive và các thuộc tính thông thường khác được build cùng một lúc dẫn đến dự thay đổi của directive là đồng thời khi render đối tượng đó.
  
    **Xây dựng một attributes directive đơn giản.**
    
    - Implement cho directive. Chúng ta có thể sử dụng CLI command để tạo ra đối tượng directive.

    ```typescript
      ng generate directive highlight | ng g d highlight
    ```

    - CLI sẽ tạo ra file <code>src/app/highlight.directive.ts</code> và khai báo nó trong AppModule Cấu trúc của file s<code>rc/app/highlight.directive.ts</code>.
    
    ```typescript
      import { Directive } from '@angular/core';

      @Directive({
          selector: '[appHighlight]'
      })
      export class HighlightDirective {
        constructor() { }
      }

    ```

    - Một attribute directive cần requires decorate class đối tượng Directive của angular bằng các dùng <code>@Directive</code> trước class. Ví dụ ở trên đây là <code>HighlightDirective</code> mục đích sẽ làm thay đổi màu background của đối tượng khi người dùng hover qua nó.

    - Import định danh Directive để sử dụng nó decorate cho đối tượng trong angular. Gọi <code>@Directive</code> trước class <code>HighlightDirective</code> là để sư dụng decorate, khi sử dùng chúng ta cần khai báo tên selector để sử dụng như một thuộc tính, <code>`[appHighlight]`</code> dấu <code>`([])`</code> là cách mà angular hiểu nó là một thuộc tính, Khi biên dịch mà thấy phần tử nào có thuộc tính có tên là <code>appHighlight</code> sẽ được áp dụng thay đổi bởi directive. (Sau khi sử dụng <code>@Directive</code>, đừng quên export class <code>HighlightDirective</code> để có thể import và sử dụng.)
    
    - Bây giờ chúng ta hãy implement cho cho HighlightDirective để làm thay đổi màu background:

    ```typescript
      import { Directive, ElementRef } from '@angular/core';

      @Directive({
          selector: '[appHighlight]'
      })
      export class HighlightDirective {
            constructor(el: ElementRef) {
              el.nativeElement.style.backgroundColor = 'yellow';
          }
      }
    ```

    - <code>ElementRef</code> là Class trong thư viện của angular. Chúng ta dùng ElementRef trong hàm construct để inject nó tham chiếu đến các phần tử DOM trong component hiện tại. Sau đó chi cần gọi thuộc tính ElementRef để lấy về đối tượng DOM để thay đổi style background sang mày vàng.
  
    <br>

      **Áp dụng attribute directive.**

      - Để dùng HighlightDirective, ta thêm thẻ như sau:

      ```typescript
        <p appHighlight>Highlight me!</p>
      ```

      <br>

      **Tương tác directive với người dùng.**

      - Hiện tại appHighlight chỉ set màu cố dịnh cho background, chưa hề có sự linh hoạt và tương tác nào. Chúng ta sẽ implement nó để thay đổi màu cho các sự kiện chuột và người dùng hành động. Trước tiên cần import HostListener.

      ```typescript
        import { Directive, ElementRef, HostListener } from '@angular/core';
      ```

      - Tiếp theo là thêm hàm xử lý khi sự kiện xảy ra bằng cách dử dụng Decorator HostListener.

      ```typescript
        @HostListener('mouseenter') onMouseEnter() {
          this.highlight('yellow');
        }

        @HostListener('mouseleave') onMouseLeave() {
          this.highlight(null);
        }

        private highlight(color: string) {
          this.el.nativeElement.style.backgroundColor = color;
        }
      ```

      - <code>@HostListener decorator</code> sẽ theo dõi và bắt các sự kiện của phần tử trong DOM mà có dử dụng directive appHighlight Hàm highlight sẽ thay đổi background color theo màu được truyền vào tham số, nên trong các hàm xử lý chỉ cần gọi tới highlight với tham số là màu cần hiển thị. Chạy và kiểm tra kêt quả nhé.

      <br>

      **Truyền dữ liệu vào directive thông qua Input.**
      - Hiện tại các màu cho các sự kiện vẫn là cố định, sử dụng ở đâu thì các màu vẫn vậy . Để tạo nên tính linh hoạt cho directive chúng ta sẽ truyền các màu vào cho nó. Đầu tiên cần import Input:

      ```typescript
        import { Directive, ElementRef, HostListener, Input } from '@angular/core';
      ```

      - Sử dụng decorator cho biến highlightColor.

      ```typescript
        @Input() highlightColor: string;
      ```

      - Sử dụng kết hợp appHighlight và input binding cho đối tượng.

      ```typescript
            <p appHighlight highlightColor="yellow">Highlighted in yellow</p>
            <p appHighlight [highlightColor]="'orange'">Highlighted in orange</p>
      ```

      - Đó là cách sử dụng ban đầu, nhưng directive đã được cải thiện để rút ngắn code và thuận tiện hơn bằng cách sử dụng director như một thuộc tính (đây là lý do vì sao selector của nó có dấu <code>`[]`</code>)

      ```typescript
        <p [appHighlight]="color">Highlight me!</p>
      ```

      - Thuộc tính <code>`[appHighlight]`</code> là sự kết hợp của <code>highlighting directive</code> và set value cho biến appHighlight Chúng ta cũng có thể đổi tên cho biến nếu không muốn đặt tên biến là appHighlight theo selector'

      ```typescript
        @Input('appHighlight') highlightColor: string;
      ```

      - Chúng ta cũng hoàn toàn có thể kết hợp cách trên với input binding thông thường.

      ```typescript
        <p [appHighlight]="color" defaultColor="violet">
              Highlight me too!
        </p>
      ```

      ```typescript
        @Input() defaultColor: string;
      ```

      - Angular sẽ tự hiểu bạn binding defaultColor cho HighlightDirective vì bạn đã khai báo decorator Input cho nó. binding thêm một defaultColor thông qua input để làm màu mặc định. trong hàm xử lý cho sự kiện chuột thay đổi như sau:

      ```typescript
        @HostListener('mouseenter') onMouseEnter() {
            this.highlight(this.highlightColor || this.defaultColor || 'red');
        }
      ```
  <br>

### **c. Pipe**

<div align="center">
  <img src="https://images.viblo.asia/b4304155-7312-4001-b222-bdc4797b897a.png"/>
</div>

- Pipe là một tính năng được xây dựng sẵn từ **Angular 2** với mục tiêu nhằm biến đổi dữ liệu đầu ra, hiển thị lên trên template đúng với ý tưởng thiết kế lập trình, thân thiện với người sử dụng.

- Ví dụ là định dạng kiểu hiển thị datetime, viết hoa chữ cái, hiển thị tên thành phố, định dạng lại số hay đơn vị tiền, ...

- List pipe mà angular cung cấp dùng mặc định [(xem thêm)](https://angular.io/api?type=pipe).

<br>

  <i>**c.1 Sử dụng pipe**</i>
  - Ví dụ dưới đây sẽ sử dụng <code>pipe</code> để biến đổi ngày sinh nhật từ dữ liệu thô sang định dạng dễ đọc hơn.
  
  ```typescript
    #src/app/hero-birthday1.component.ts

    import { Component } from '@angular/core';

    @Component({
        selector: 'app-hero-birthday',
        template: `<p>The hero's birthday is {{ birthday | date }}</p>`
    })
    export class HeroBirthdayComponent {
        birthday = new Date(1988, 3, 15);
    }

  //Fri Apr 15 1988 00:00:00 GMT+0700 (Indochina Time) ->  April 15, 1988
  ```

  - Bên trong biểu thức nội suy <code>{{}}</code>, bạn đã biến đổi ngày sinh nhật bằng biểu thức pipe <code>|</code> từ định dạng của js <code>Fri Apr 15 1988 00:00:00 GMT+0700 (Indochina Time)</code> sang <code>April 15, 1988</code> dễ đọc cho người dùng.

<br>

  <i>**c.2 Tham số trong <code>pipe</code>**</i>
  - Để có thể bổ sung tham số trong <code>pipe</code>, chúng ta sử dụng dấu hai chấm <code>:</code> sau tên pipe đó (ví dụ <code>currency:'EUR'</code>)
  
  - Nếu pipe đó chấp nhận nhiều tham số đúng với cú pháp, hay viết <code>:</code> nằm giữa hai tham số (ví dụ <code>slice:1:5</code>)

  - Tương tự với ví dụ trên, chúng ta sẽ truyền tham số là định dạng của [DatePipe](https://angular.io/api/common/DatePipe).

  ```typescript
    #src/app/app.component.html

    <p>The hero's birthday is {{ birthday | date:"MM/dd/yy" }} </p>
  ```

  - Tham số có thể được custom lại thông qua các thao tác thay đổi sự kiện (nhưng vẫn phải đúng với cú pháp của pipe đó)

  - Sử dụng tiếp với ví dụ trên, chúng ta sẽ không truyền tham số định dạng trực tiếp đằng sau <code>datepipe</code> mà sẽ ràng buộc thông qua một biến là <code>format</code> và chúng ta sẽ thay đổi <code>format</code> đó thông qua một button bên dưới.

  ```typescript
    #src/app/hero-birthday2.component.ts (template)

    template: `
      <p>The hero's birthday is {{ birthday | date:format }}</p>
      <button (click)="toggleFormat()">Toggle Format</button>
    `
  ```

  - Sự kiện <code>click</code> sẽ thay đổi <code>format</code> của <code>datepipe</code> giữa kiểu <code>shortDate</code> và <code>fullDate</code>.

  - Kết quả:

  <div align="center">
    <img src="https://images.viblo.asia/e288781b-95b1-4e29-ba06-cc61fa57cd4c.gif">
  </div>

<br>

  <i>**c.3 Chuỗi các <code>pipe</code>**</i>
  - Bạn có thể kết hợp sử dụng các pipe liên tiếp với nhau, pipe trước sẽ là dữ liệu đầu vào của pipe sau, đúng như nghĩa đen của nó là **ống nước**.
  
  - Ví dụ, sử dụng kết hợp cả **DatePipe** và **UpperCasePipe**

  ```typescript
    #src/app/app.component.html

    The chained hero's birthday is
    {{ birthday | date | uppercase}}

    // APR 15, 1988
  ```

  - Sử dụng tham số cùng với chuỗi pipe

  ```typescript
    #src/app/app.component.html

    The chained hero's birthday is
    {{  birthday | date:'fullDate' | uppercase}}

    // FRIDAY, APRIL 15, 1988
  ```

<br>

  <i>**c.4 Custom <code>pipe</code>**</i>
  - Ngoài những pipe mà angular tích hợp sẵn trong code, nó còn cung cấp cho chúng ta công cụ để có thể tự sáng tạo ra những pipe của riêng mình

  ```typescript
    #src/app/exponential-strength.pipe.ts

    import { Pipe, PipeTransform } from '@angular/core';
    /*
    * Raise the value exponentially
    * Takes an exponent argument that defaults to 1.
    * Usage:
    *   value | exponentialStrength:exponent
    * Example:
    *   {{ 2 | exponentialStrength:10 }}
    *   formats to: 1024
    */
    @Pipe({name: 'exponentialStrength'})
    export class ExponentialStrengthPipe implements PipeTransform {
      transform(value: number, exponent?: number): number {
        return Math.pow(value, isNaN(exponent) ? 1 : exponent);
      }
    }
  ```

  - Pipe trên là một <code>custom pipe</code>.
  
  - Có thể hiểu đầu vào là 2 tham số, tham số thứ nhất là số ngẫu nhiễn, tham số thứ 2 là số mũ. Đầu ra sẽ là phép tính lũy thừa từ hai số

  ```typescript
    #src/app/power-booster.component.ts

    import { Component } from '@angular/core';

    @Component({
        selector: 'app-power-booster',
        template: `
        <h2>Power Booster</h2>
        <p>Super power boost: {{2 | exponentialStrength: 10}}</p>
      `
    })
    export class PowerBoosterComponent { }
  ```

  - Kết quả:

  <div align="center">
    <img src="https://images.viblo.asia/a014e114-5a72-473b-94ef-2e90d5b4270b.png">
  </div>

  - Những nội dung cần làm để tạo một custom pipe
  
    - Viết một class và decorate nó với decorator <code>@Pipe</code> ( báo cho angular biết đây là pipe) và decorator này cần tối thiểu một object có property name, chính là tên của pipe để có thể gọi ra trong template.

    - Tiếp theo, chúng ta cần implement một interface là PipeTransform, và implement hàm transform của interface đó. Hàm <code>transform</code> sẽ làm biến đổi dữ liệu

    - Sau khi hoàn thiện pipe trên, ta cần import custom pipe trên vào trong NgModule mà template cần sử dụng custom pipe thuộc về ( ví dụ trên là viết cùng file )
  
  - Ví dụ trên là dữ liệu tĩnh, bây giờ chúng ta sẽ làm cho nó động hơn với <code>ngModel</code>

  ```typescript
    #src/app/power-boost-calculator.component.ts

    import { Component } from '@angular/core';

    @Component({
        selector: 'app-power-boost-calculator',
        template: `
            <h2>Power Boost Calculator</h2>
            <div>Normal power: <input [(ngModel)]="power"></div>
            <div>Boost factor: <input [(ngModel)]="factor"></div>
        <p>
            Super Hero Power: {{power | exponentialStrength: factor}}
        </p>
      `
    })
    export class PowerBoostCalculatorComponent {
        power = 5;
        factor = 1;
    }
  ```

  - Kết quả:

  <div align="center">
    <img src="https://images.viblo.asia/a6b8cef6-e76c-45df-85b9-46e25199510e.gif">
  </div>

<br>

### **d Services & Dependency Injection**

  #### **d.1 Services**

  - Chúng ta có thể gặp một tình huống mà chúng ta cần một code để sử dụng ở mọi nơi trên trang. Chẳng hạn như kết nối dữ liệu cần được chia sẻ giữa các component. Với Service, chúng ta có thể truy cập các phương thức và thuộc tính trên các component khác trong toàn bộ dự án.

  <i>**d.1.1 Tạo Service**</i>
  - Ta có thể khởi tạo service với câu lệnh Angular CLI:

  ```typescript
    ng g s <tên_service> | ng genarate service <tên_service>
  ```

  - Kết quả

  ```typescript
    C:\Angular\Angular7\my-app>ng g service myservice
    CREATE src/app/myservice.service.spec.ts (348 bytes)
    CREATE src/app/myservice.service.ts (138 bytes)
  ```

  **myservice.service.ts**

  ```typescript
    import { Injectable } from '@angular/core';
 
    @Injectable({
      providedIn: 'root'
    })
    export class MyserviceService {
    
      constructor() { }
    }
  ```
  
  - Ở đây, mô-đun Injectable được import từ <code>@angular/core</code>. Nó chứa phương thức <code>@Injectable</code> và một lớp gọi là **MyserviceService**. Chúng ta sẽ tạo chức năng service trong lớp này.
  
  <br>

  <i>**d.1.2 Khai báo service trong module  <code>app.module.ts</code>**</i>

  ```typescript
    import { BrowserModule } from '@angular/platform-browser';
    import { NgModule } from '@angular/core';
 
    import { AppRoutingModule, RoutingComponent } from './app-routing.module';
    import { AppComponent } from './app.component';
    import { NewCmpComponent } from './new-cmp/new-cmp.component';
    import { AddTextDirective } from './add-text.directive';
    import { SqrtPipe, SquarePipe } from './app.sqrt';
    import { HomeComponent } from './home/home.component';
    import { ContactusComponent } from './contactus/contactus.component';
    import { MyserviceService } from './myservice.service';
 
    @NgModule({
        declarations: [
          AppComponent,
          NewCmpComponent,
          AddTextDirective,
          SqrtPipe,
          SquarePipe,
          HomeComponent,
          ContactusComponent,
          RoutingComponent
        ],
        imports: [
          BrowserModule,
          AppRoutingModule
        ],
        providers: [MyserviceService],
        bootstrap: [AppComponent]
    })
    export class AppModule { }
  ```

  - Trong file trên, chúng ta đã nhập Service với tên lớp và lớp tương tự được khai báo trong mảng providers (<code>providers: [MyserviceService]</code>).

<br>

  <i>**d.1.3 Tạo hàm cho service trong Angular**</i>
  - Bây giờ chúng ta sẽ chuyển trở lại lớp MyserviceService và tạo một chức năng trong nó.

  - Trong lớp MyserviceService, chúng ta sẽ tạo một hàm hiển thị ngày hôm nay. Sau đó sử dụng một hàm này trong app.component.ts và trong new-cmp.component.ts mà chúng ta đã tạo trước.

  - Tạo hàm: <code>showTodayDate()</code>

  ```typescript
    import { Injectable } from '@angular/core';
 
    @Injectable({
      providedIn: 'root'
    })
    export class MyserviceService {
      constructor() { }
    
      showTodayDate() { 
          let today = new Date(); 
          return today;
      } 
    }
  ```

  <br>

  <i>**d.1.4 Sử dụng hàm showTodayDate() trong component <code>new-cmp.component.ts</code>**</i>
  - Để sử dụng hàm showTodayDate() trong component new-cmp.component.ts, đầu tiên chúng ta cần phải inject lớp MyServiceService vào constructor của lớp AppComponent trong file new-cmp.component.ts. Sau đó chúng ta có thể gọi hàm <code>showTodayDate()</code> như sau:

  **new-cmp.component.ts**

  ```typescript
    import { Component, OnInit } from '@angular/core';
    import { MyserviceService } from '../myservice.service';
 
    @Component({
        selector: 'app-new-cmp',
        templateUrl: './new-cmp.component.html',
        styleUrls: ['./new-cmp.component.css']
    })
    export class NewCmpComponent implements OnInit {
        newcomponent = "New Component";                        
        todayDate: Date; 
        constructor(private myservice: MyserviceService) { }
        ngOnInit() { 
          this.todayDate = this.myservice.showTodayDate(); 
        } 
    }
  ```
  - Hiển thị biến todayDate trong view của component new-cmp.component.ts

  **new-cmp.component.html**

  ```typescript
    <p>
      {{newcomponent}} 
    </p>
    <p>
      Today's Date : {{todayDate}} 
    </p>
  ```

  - **Nếu bạn thay đổi thuộc tính của service trong bất kỳ component nào, thì các component khác cũng thay đổi như vậy. Bây giờ chúng ta cùng tìm hiểu cách này hoạt động này qua ví dụ sau.**

  <br>

  #### **d.2 Dependency Injection**
  - Trong bài này chúng ta đã tìm hiểu và thấy rằng Dependency Injection được sử dụng trong ứng dụng Angular để tạo ra các Service, và các Service này được inject vào các class (ví dụ: component, directive, service) khác thông qua constructor injection. Vậy ngoài để tạo ra Service thì nó còn có thể sử dụng ở những đâu nữa.

  <i>**d.2.1 Inject parent component to child component**</i>
  - Angular application là một component tree có dạng như sau:

  <div align="center">
    <img src="https://github.com/angular-vietnam/100-days-of-angular/raw/master/assets/components-tree.jpg">
  </div>

  - Do Angular support DI đến tận level của từng Component, nên chúng ta hoàn toàn có thể inject parent component vào child component như ví dụ sau.

  - Giả sử chúng ta đang xây dựng Tabs Component để thao tác với các tab. Chúng ta sẽ có hai component bao gồm <code>TabGroupComponent</code> để quản lý các tab panel và <code>TabPanelComponent</code> tương ứng với mỗi panel.

  ```typescript
    <app-tab-group>
        <app-tab-panel title="Tab 1">content tab 1</app-tab-panel>
        <app-tab-panel title="Tab 2">content tab 2</app-tab-panel>
        <app-tab-panel title="Tab 3">content tab 3</app-tab-panel>
        <!-- More tab panel -->
    </app-tab-group>
  ```

  - Lúc này mỗi khi có một tab panel được thêm vào hay xóa đi thì tab group phải biết được để có thể xử lý tương ứng như: hiển thị các tab title, handle event khi select tab nào đó từ tab title.

  - Và trong mỗi component của Angular chúng ta sẽ được cung cấp sẵn hai lifecycle method quan trọng để biết khi nào một component được tạo ra và sắp bị destroy đó là <code>OnInit</code> và <code>OnDestroy</code>. Đây chính là những thời điểm thích hợp để <code>TabPanelComponent</code> có thể tương tác lại với <code>TabGroupComponent</code>.

  - Đến thời điểm hiện tại, bạn hoàn toàn có thể sử dụng <code>EventEmitter</code> để notify cho parent component biết được các thời điểm tương ứng. Nhưng cũng có một cách khác, đó chính là inject parent component vào child component và thực hiện các hành động tương ứng.

  - Giả sử bạn cài đặt <code>tab-group.component.ts</code> như sau:

  ```typescript
    @Component({
        selector: 'app-tab-group',
        templateUrl: './tab-group.component.html',
        styleUrls: ['./tab-group.component.css'],
    })

    export class TabGroupComponent implements OnInit {
        tabPanelList: TabPanelComponent[] = [];

        @Input() tabActiveIndex = 0;
        @Output() tabActiveChange = new EventEmitter<number>();
        constructor() {}

        ngOnInit() {}

        selectItem(idx: number) {
            this.tabActiveIndex = idx;
            this.tabActiveChange.emit(idx);
        }

        addTabPanel(tab: TabPanelComponent) {
            this.tabPanelList.push(tab);
        }

        removeTabPanel(tab: TabPanelComponent) {
            let index = -1;
            const tabPanelList: TabPanelComponent[] = [];

            this.tabPanelList.forEach((item, idx) => {
              if (tab === item) {
                index = idx;
                return;
              }
              tabPanelList.push(item);
            });

            this.tabPanelList = tabPanelList;
            if (index !== -1) {
              this.selectItem(0);
            }
        }
      }
  ```

  - Và đây là phần UI cho <code>tab-group.component.html</code>:

  ```typescript
    <div class="tab-header">
        <div
          class="tab-item-header"
          role="presentation"
          *ngFor="let tab of tabPanelList; index as idx"
          (click)="selectItem(idx)"
        >
          {{tab.title}}
        </div>
    </div>

<div class="tab-body">
      <ng-container *ngFor="let tab of tabPanelList; index as idx">
        <div *ngIf="idx === tabActiveIndex">
          <ng-container *ngTemplateOutlet="tab.panelBody"></ng-container>
        </div>
      </ng-container>
</div>
  ```

  - Việc của chúng ta bây giờ chỉ là inject và call các method để **register** và **remove**:

  ```typescript
    @Component({
        selector: 'app-tab-panel',
        template: `
          <ng-template>
            <ng-content></ng-content>
          </ng-template>
        `,
        styles: [''],
    })
    export class TabPanelComponent implements OnInit, OnDestroy {
        @Input() title: string;
        @ViewChild(TemplateRef, { static: true }) panelBody: TemplateRef<unknown>;
        constructor(private tabGroup: TabGroupComponent) {}

        ngOnInit() {
          this.tabGroup.addTabPanel(this);
        }
        ngOnDestroy() {
          this.tabGroup.removeTabPanel(this);
        }
    }
  ```

  <i>**d.2.2 Provide một tab group khác có cùng APIs**</i>
  - Như bạn có thể thấy, tab group của chúng ta ở trên có UI cực kỳ đơn giản, hoặc trong trường hợp bạn muốn có UI riêng, đúng như design system (bootstrap, ant design, etc) đang dùng trong application thì sao? Chả lẽ chúng ta không reuse được gì ở trên.

- Đây là nơi tỏa sáng của DI. Bạn chỉ cần đơn giản là provide một provider để override là được.

  **bs-tab-group.component.ts**

  ```typescript
    @Component({
        selector: 'app-bs-tab-group',
        templateUrl: './bs-tab-group.component.html',
        styleUrls: ['./bs-tab-group.component.css'],
        providers: [
          {
            provide: TabGroupComponent,
            useExisting: BsTabGroupComponent,
          },
        ],
    })
    export class BsTabGroupComponent extends TabGroupComponent {}
  ```

  **bs-tab-group.component.html**

  ```typescript
    <ul class="nav nav-tabs" role="tablist">
      <li
        class="nav-item"
        role="presentation"
        *ngFor="let tab of tabPanelList; index as idx"
        (click)="selectItem(idx)"
      >
          <a
            class="nav-link"
            [class.active]="idx === tabActiveIndex"
            role="tab"
            aria-selected="true"
            >{{tab.title}}</a
          >
      </li>
  </ul>

  <div class="tab-content">
    <ng-container *ngFor="let tab of tabPanelList; index as idx">
      <div class="tab-pane active" role="tabpanel" *ngIf="idx === tabActiveIndex">
        <ng-container *ngTemplateOutlet="tab.panelBody"></ng-container>
      </div>
    </ng-container>
  </div>
  ```

- Template khi sử dụng:

  ```typescript
      <app-bs-tab-group>
          <app-tab-panel title="Tab 1">content tab 1</app-tab-panel>
          <app-tab-panel title="Tab 2">content tab 2</app-tab-panel>
          <app-tab-panel title="Tab 3">content tab 3</app-tab-panel>
      </app-bs-tab-group>
  ```
- Hoàn toàn hợp lệ, không có lỗi gì xảy ra cả.

- Kỹ thuật này được sử dụng rất nhiều trong chính bản thân của Angular Framework như đối với phần Form ở đây: [ngForm](https://github.com/angular/angular/blob/9.1.x/packages/forms/src/directives/ng_form.ts), [ngModelGroup](https://github.com/angular/angular/blob/9.1.x/packages/forms/src/directives/ng_model_group.ts), etc.

<br>

<i>**d.2.3 Forward Reference**</i>
- Trong những phần code của Form ở trên bạn thấy người ta sử dụng thêm <code>forwardRef</code> vậy khi nào cần sử dụng nó.

- Như các bạn cũng biết, một class ở trong ES2015/TypeScript chỉ có thể refer đến nó khi nó đã được khai báo. Nếu chúng ta refer đến nó trước khi được khai báo thì sẽ trả về error (hoặc undefined khi code transpile sang ES5 với <code>var</code> keyword).

- Bây giờ giả sử bạn tách phần provider ở decorator ra một variable đặt ở trước khi khai báo class.

  ```typescript
    const BsTabGroupProvider = {
        provide: TabGroupComponent,
        useExisting: BsTabGroupComponent,
    };

    @Component({
        selector: 'app-bs-tab-group',
        templateUrl: './bs-tab-group.component.html',
        styleUrls: ['./bs-tab-group.component.css'],
        providers: [BsTabGroupProvider],
    })
    export class BsTabGroupComponent extends TabGroupComponent {}
  ```

- Bạn sẽ nhận được một Error như sau:
  > Error in src/app/bs-tab-group/bs-tab-group.component.ts (6:16)
  
  > Class 'BsTabGroupComponent' used before its declaration.

- Do đó chúng ta cần dùng đến closure, đó là tạo một function nó sẽ được call sau khi chúng ta tạo xong class, và nó sẽ có thể refer đến class đó ở thời điểm được gọi.

  ```typescript
    const BsTabGroupProvider = {
        provide: TabGroupComponent,
        useExisting: forwardRef(() => BsTabGroupComponent),
    };
  ```

- Bạn có thể thắc mắc là tại sao sử dụng trực tiếp trong decorator thì lại không lỗi? Câu trả lời là vì bản thân Class Decorator sẽ được call sau khi mà bạn đã tạo xong class.

- Bạn có thể tưởng tượng nó sẽ hoạt động giống như sau:

  ```typescript
    @SomeDecorator 
    class SomeClass {}
  ```

- Sẽ tương đương với call một function như sau.
  ```typescript
    let SomeClass = class SomeClass {};
    SomeClass = SomeDecorator(SomeClass);
  ```

<br>

<i>**d.2.4 Provider syntax**</i>
- Như chúng ta đã tìm hiểu qua thì chúng ta có các cách provide một provider với các dạng như sau:

  > Lưu ý: code phía dưới đây sẽ tương tự cho <code>@NgModule, @Component, @Directive</code>.

- **useClass**:

  ```typescript
    @NgModule({
        providers: [SomeClass]
    })
  ```

- Tương đương với cú pháp:

  ```typescript
    @NgModule({
    providers: [{ provide: SomeClass, useClass: SomeClass}]
    })
  ```

- **useExisting:**

  ```typescript
    @Component({
    providers: [
        {
          provide: SomeClass,
          useExisting: OtherClass
        }
      ]
    })
  ```

- **useFactory:**

  ```typescript
    @Component({
      providers: [
        {
          provide: SomeClass,
          useFactory: function() {
            return aValue;
          }
        }
      ]
    })
  ```

- **useValue:**

  ```typescript
    @Component({
      providers: [
        {
          provide: SomeToken,
          useValue: someValue
        }
      ]
    })
  ```

<br>

### **d. SUMMARY**
- [Khởi tạo một service đơn giản](https://viettuts.vn/angular7/service-trong-angular7)
- [Tìm hiểu về pipe](https://viblo.asia/p/tim-hieu-pipe-trong-angular-GrLZD332Kk0)
- [Tìm hiểu về directive](https://viblo.asia/p/tim-hieu-co-ban-ve-directives-trong-angular-ByEZk2roKQ0)
