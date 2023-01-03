# **Chủ đề tuần 9**

- Animation
- NgZone

## **a. Animation**
- Animations trong Angular liên quan đến nhiều kiểu chuyển đổi theo thời gian. Nó bao gồm các thành phần di chuyển, thay đổi màu sắc, các thành phần phóng toa hoặc thu nhỏ, mờ dần hoặc trượt khỏi trang.Những thay đổi này có thể xảy ra đồng thời hoặc tuần tự. Bạn có thể kiểm soát thời gian của mọi chuyển đổi.

- Angular Animation cạnh có thể cải thiện trải nghiệm người dùng và ứng dụng của bạn theo nhiều cách:
    - Nếu không sử dụng hoạt ảnh, quá trình chuyển đổi trang web có vẻ đột ngột và chói mắt.
    - Chuyển động nâng cao đáng kể trải nghiệm người dùng, do đó Animation mang đến cho người dùng cơ hội phát hiện phản ứng của ứng dụng đối với hành động của họ.
    - Hoạt ảnh tốt thu hút sự chú ý của người dùng một cách trực quan đến nơi cần thiết.

### **a.1 Hiểu về Angular Animation State**
- Animation trong Angular liên quan đến việc chuyển từ trạng thái này của phần tử sang trạng thái khác. Angular xác định ba trạng thái khác nhau cho một thành phần:
    + Void State(state rỗng): Trạng thái void biểu thị trạng thái của phần tử không phải là một phần của DOM. Trạng thái này xảy ra khi phần tử được tạo nhưng chưa được đặt trong DOM hoặc bị xóa khỏi DOM.
    + Wildcard State: Còn được gọi là trạng thái mặc định của phần tử. Các kiểu được xác định cho trạng thái này áp dụng cho thành phần bất kể trạng thái hoạt hình hiện tại của nó. Để xác định trạng thái này trong mã của chúng tôi, chúng tôi sử dụng ký hiệu `*`.
    + Custom State: Trạng thái tùy chỉnh cần được xác định rõ ràng trong mã. Để xác định trạng thái tùy chỉnh, chúng ta có thể sử dụng bất kỳ tên tùy chỉnh nào mà chúng ta chọn.

### **a.2 Angular Animation Transition Timing**
- Để hiển thị quá trình chuyển đổi hoạt ảnh trong Angular từ trạng thái này sang trạng thái khác, chúng tôi xác định thời gian chuyển đổi hoạt ảnh trong ứng dụng của mình.

- Angular cung cấp các thuộc tính ba thời gian như sau.

#### Duration
- Thời lượng trong Angular mô tả thời gian hoạt hình của chúng ta hoàn thành (trạng thái ban đầu) để kết thúc (trạng thái cuối cùng).
- VD:
    - Using an integer value to describe the time in milliseconds. E.g., 1000.
    - Using the string value to describe the time in milliseconds. E.g.: ‘1000ms’.
    - Using the string value to describe the time in seconds. E.g.: ‘1s’.

#### Easing
- Thuộc tính easing trong Angular thể hiện cách hoạt ảnh tăng tốc hoặc giảm tốc trong quá trình thực thi.
- Để xác định độ easing trong Angular, hãy thêm biến thứ ba trong chuỗi sau thuộc tính thời lượng và độ trễ.

### **a.3 Create Demo**
#### Step 1: Creating the Angular application

```typescript
ng new anganimation
npm install bootstrap
```

- Mở tệp Angular.json và thêm đường dẫn đến Bootstrap CSS Framework.

```typescript
"styles": [
    "src/styles.css",
    "./node_modules/bootstrap/dist/css/bootstrap.min.css"
],
```

#### Step 2: Enabling the animations module
- Import BrowserAnimationsModule  vào root Angular của bạn.

```typescript
// app.module.ts

import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { BrowserAnimationsModule } from '@angular/platform-browser/animations';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    BrowserAnimationsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

#### Step 3: Importing animation functions
- Nếu bạn dự định sử dụng các chức năng hoạt hình cụ thể trong các tệp thành phần, hãy nhập các chức năng đó từ **@angular/animations**. Trong trường hợp của chúng tôi, đó là tệp **app.component.ts**.

```typescript
// app.component.ts

import { Component, OnInit } from '@angular/core';
import {
  trigger,
  state,
  style,
  animate,
  transition,
  // ...
} from '@angular/animations';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {

  ngOnInit(): void {
  }
}
```

#### Step 4: Adding the animation metadata property
- Trong tệp `app.component.ts`, hãy thêm thuộc tính siêu dữ liệu hoạt ảnh: bên trong trình trang trí `@Component()`. Sau đó, bạn đặt trình kích hoạt xác định hoạt ảnh trong thuộc tính siêu dữ liệu hoạt ảnh.

```typescript
// app.component.ts

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
  animations: [
    // animation triggers go here
  ]
})
```

- Vd thêm animate

```typescript
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
  animations: [
    trigger('triggerName'), [
      state('stateName', style())
      transition('stateChangeExpression', [Animation Steps])
    ]
  ]
})
```

## **b. NgZone**
- Trong Tìm hiểu về Zones, chúng tôi đã khám phá sức mạnh của các Vùng bằng cách xây dựng một vùng định hình cấu hình các hoạt động không đồng bộ trong code. 

### **b.1 Zones are a perfect fit for Angular**
- Hóa ra, vấn đề mà Zones giải quyết, hoạt động rất tốt với những gì Angular cần để thực hiện phát hiện thay đổi trong ứng dụng.

- Bạn đã bao giờ tự hỏi khi nào và tại sao Angular thực hiện phát hiện thay đổi chưa? Điều gì nói với Angular "Anh bạn, một thay đổi có thể xảy ra trong ứng dụng của tôi. Bạn có thể vui lòng kiểm tra không?"

- Trước khi đi sâu vào những câu hỏi này, trước tiên chúng ta hãy nghĩ về điều gì thực sự gây ra sự thay đổi này trong các ứng dụng của chúng ta. Hay đúng hơn, cái gì có thể thay đổi trạng thái trong các ứng dụng của chúng ta. Thay đổi trạng thái ứng dụng được gây ra bởi ba điều
    - **Events** - User events like `click`, `change`, `input`, `submit`, …
    - **XMLHttpRequests** - E.g. when fetching data from a remote service
    - **Timers** - `setTimeout()`, `setInterval()`, because JavaScript

- Hóa ra ba điều này có điểm chung. Bạn có thể đặt tên cho nó? … Chính xác! **Chúng đều không đồng bộ.**

```typescript
@Component({
  selector: 'my-component',
  template: `
    <h3>We love {{name}}</h3>
    <button (click)="changeName()">Change name</button>
  `
})
class MyComponent {

  name:string = 'thoughtram';

  changeName() {
    this.name = 'Angular';
  }
}
```

- Khi nút của Component được nhấp, changeName() được thực thi, do đó sẽ thay đổi thuộc tính `name` của thành phần.
- Vì chúng tôi cũng muốn thay đổi này được phản ánh trong DOM, nên Angular sẽ cập nhật ràng buộc chế độ xem `{% raw %}{{name}}{% enraw %}` cho phù hợp. Tốt, điều đó dường như hoạt động một cách kỳ diệu.
- Một ví dụ khác là cập nhật thuộc tính name bằng cách sử dụng `setTimeout()`. Lưu ý rằng chúng tôi đã loại bỏ nút.

```typescript
@Component({
  selector: 'my-component',
  template: `
    <h3>We love {{name}}</h3>
  `
})
class MyComponent implements OnInit {

  name:string = 'thoughtram';

  ngOnInit() {
    setTimeout(() => {
      this.name = 'Angular';
    }, 1000);
  }
}
```


### **b.2 NgZone in Angular**
- NgZone về cơ bản là một vùng rẽ nhánh mở rộng API của nó và thêm một số chức năng bổ sung vào ngữ cảnh thực thi của nó. Một trong những thứ nó thêm vào API là tập hợp các sự kiện tùy chỉnh sau đây mà chúng ta có thể đăng ký, vì chúng là các luồng có thể quan sát được:
    - **onTurnStart()** - Thông báo cho người đăng ký ngay trước khi lượt sự kiện của Angular bắt đầu. Phát ra một sự kiện một lần cho mỗi tác vụ trình duyệt được xử lý bởi Angular.
    - **onTurnDone()** - Thông báo cho người đăng ký ngay lập tức sau khi khu vực của Angular xử lý xong lượt hiện tại và bất kỳ tác vụ vi mô nào được lên lịch từ lượt đó.
    - **onEventDone()** - Thông báo cho người đăng ký ngay sau lần gọi lại `onTurnDone()` cuối cùng trước khi kết thúc sự kiện VM. Hữu ích cho việc thử nghiệm để xác thực trạng thái ứng dụng.

### **c. Running code outside Angular’s zone**
- Vì NgZone thực sự chỉ là một nhánh của vùng toàn cầu, nên Angular có toàn quyền kiểm soát khi nào thì chạy thứ gì đó bên trong vùng của nó để thực hiện phát hiện thay đổi và khi nào thì không.Tại sao điều đó lại hữu ích? Chà, hóa ra không phải lúc nào chúng ta cũng muốn Angular thực hiện phát hiện thay đổi một cách kỳ diệu.

- Chúng tôi có thể không muốn thực hiện phát hiện thay đổi mỗi khi `mousemove` được kích hoạt vì nó sẽ làm chậm ứng dụng của chúng tôi và dẫn đến trải nghiệm người dùng rất tệ.
- Đó là lý do tại sao NgZone đi kèm với API runOutsideAngular() thực hiện một tác vụ nhất định trong vùng cha của NgZone, không phát ra sự kiện onTurnDone,do đó không có phát hiện thay đổi được thực hiện. Để chứng minh tính năng hữu ích này, chúng ta hãy xem đoạn mã sau:

```typescript
@Component({
  selector: 'progress-bar',
  template: `
    <h3>Progress: {{progress}}</h3>
    <button (click)="processWithinAngularZone()">
      Process within Angular zone
    </button>
  `
})
class ProgressBar {

  progress: number = 0;

  constructor(private zone: NgZone) {}

  processWithinAngularZone() {
    this.progress = 0;
    this.increaseProgress(() => console.log('Done!'));
  }
}
```

- Không có gì đặc biệt xảy ra ở đây. Chúng tôi có thành phần gọi `processWithinAngularZone()` khi nhấp vào nút trong mẫu. Tuy nhiên, phương pháp đó gọi `increaseProgress()`. Chúng ta hãy xem xét kỹ hơn cái này:

```typescript
increaseProgress(doneCallback: () => void) {
  this.progress += 1;
  console.log(`Current progress: ${this.progress}%`);

  if (this.progress < 100) {
    window.setTimeout(() => {
      this.increaseProgress(doneCallback);
    }, 10);
  } else {
    doneCallback();
  }
}
```

- `increaseProgress()` tự gọi cứ sau 10 mili giây cho đến khi `progress` bằng `100`. Sau khi thực hiện xong, `doneCallback` đã cho sẽ thực thi. Lưu ý cách chúng tôi sử dụng `setTimeout()` để tăng tiến trình.

- Chạy mã này trong trình duyệt, về cơ bản thể hiện những gì chúng ta đã biết. Sau mỗi lệnh gọi `setTimeout()`. Angular thực hiện phát hiện thay đổi và cập nhật chế độ xem, cho phép chúng tôi xem tiến trình được tăng lên như thế nào sau mỗi 10 mili giây. Sẽ thú vị hơn khi chúng tôi chạy mã này bên ngoài vùng của Angular. Hãy thêm một phương thức thực hiện chính xác điều đó.

```typescript
processOutsideAngularZone() {
  this.progress = 0;
  this.zone.runOutsideAngular(() => {
    this.increaseProgress(() => {
      this.zone.run(() => {
        console.log('Outside Done!');
      });
    });
  });
}
```
