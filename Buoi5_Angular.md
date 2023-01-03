# **Chủ đề tuần 3**

- RxJS

## **a. Reactive programming**
- Điều đầu tiên trước khi các bạn muốn sử dụng **rxjs** một cách hiệu quả đó là các bạn phải hiểu được **reactive programming** là gì trước đã. **Reactive programming** là một thuật ngữ để chỉ một phương pháp phân tích logic mới. Các bạn hãy cùng xem hình bên dưới để dẽ hiểu hơn nhé.

<div align="center">
    <img src="https://images.viblo.asia/49561ecf-0f2e-447f-8e3e-95786d659519.png">
</div>

- Data trong **reactive programming** sẽ được chứa trong một **stream**, các bạn có thể tưởng tượng một **stream** giống như một cái băng truyền ở trong ảnh trên vậy. Khi **stream** nhận vào một gói data mới, gói data đó sẽ được **stream** vận chuyển đến các **modifier**. **Modifier** là các **function**, nghiệm vụ của các **modifier** là chúng sẽ react(hay phản ứng) với các gói data được stream đưa vào, thay đổi các gói data đó và trả lại cho **stream** các gói data mà chúng vừa thay đổi.

- Như các bạn có thể thấy, các **modifier** không hề chạy một cách bị động, mà chúng sẽ tự động chạy, **react**(phản ứng) với mỗi gói data được **stream** truyền vào và đây chính là lý do vì sao mô hình này được đặt tên là **reactive programming**.

- Nhờ vào việc các **modifier** tự động react với các gói data được stream truyền vào mà ta không phải tự tay chạy các **modifier** nên luồng hoạt động của mô hình này rất dễ nắm bắt, đọc hiểu. Một đặc điểm cực kỳ hữu ích của mô hình này là ta có thể tự định nghĩa thứ tự của các **modifier** trước khi một **stream** chạy, tùy vào thứ tự của các modifier mà các gói data stream trả về cho chúng ta sẽ khác nhau. Điều này giúp cho luồng hoạt động của **stream** trở nên rất rõ ràng, mạch lạc và dễ **debug**.

- Khi được áp dụng vào thực tế, **reactive programming** có thể được áp dụng trong rất nhiều trường hợp, ví dụ method **setInterval**, chúng ta **có thể coi** rằng mỗi khi **setInterval** được chạy, nó sẽ tạo ra một **stream**. Như trong ví dụ dưới đây, **stream** mà method **setInterval** tạo ra sẽ nhận vào một gói data có giá trị là undefined sau mỗi 1s và callback function mà ta truyền vào trong setInterval sẽ có vai trò như là một **modifier**.

```typescript
setInterval(data => {
    console.log('gói data nhận vào có giá trị là ', data);
}, 1000);
```

- Tương tự như vậy, chúng ta cũng **có thể coi như** rằng method **setTimeout** sẽ tạo ra một **stream**, những stream này sẽ dừng lại ngay sau khi **callback function**(<code>modifier</code>) ta truyền vào trong <code>setTimeout</code> chạy xong.

```typescript
setTimeout(data => {
    console.log('gói data nhận vào có giá trị là ', data);
}, 1000);
```

- **Event** cũng có thể được coi là các **stream**. Ví dụ như **event click**, mỗi khi chúng ta click, một gói data chứa các thông tin của lần click đó sẽ được truyền vào trong stream và callback của event click sẽ là các **modifier** xử lý các gói data đó.

```typescript
document.onclick = function(evt) {
    console.log('gói data nhận vào có giá trị là ', evt);
}
```

## **b. Rxjs**

### **b.1 Rxjs là gì**
- Reactive programming chỉ là một khái niệm, một cách nghĩ, suy luận về vòng đời của các gói data và rxjs sẽ giúp chúng ta mô hình hoá nó để chúng ta có thể áp dụng nó vào thực tế một cách dễ dàng.

### **b.2 Các thuật ngữ trong Rxjs**
- Trước khi chúng ta đi sâu hơn về Rxjs, các bạn hãy đọc qua một số thuật ngữ chính trong thư viện này nhé.

    - <code>Observable</code>: là một constructor giúp chúng ta tạo ra các object <code>observable</code> tạm thời thì các bạn có thể coi <code>observable</code> là một **class**, và các <code>instance</code> của class này chính là các <code>stream</code>.

    - <code>executor</code>: nếu như **observable** là một **class** thì <code>executor</code> chính là phần logic hay <code>constructor</code> method của **class** đó, nó sẽ giúp chúng ta định nghĩa cách mà một **stream** sẽ hoạt động (lát nữa mình sẽ giải thích sau).

    - <code>observer</code>: là một object có 3 <code>method</code>, 3 <code>method</code> này chính là 3 <code>modifier</code>, nhưng từng <code>modifier</code> sẽ chạy trong một trường hợp khác nhau:

        - <code>next</code>: modifier này sẽ chạy(<code>react</code>) mỗi khi nó nhận được một gói data.

        - <code>error</code>: modifier này sẽ chạy(<code>react</code>) khi <code>stream</code> đi theo nó bị lỗi, modifier này sẽ nhận vào một tham số trả về lỗi mà <code>stream</code> gặp phải.

        - <code>complete</code>: modifier này sẽ chạy(<code>react</code>) khi logic của <code>stream</code>> đi theo nó ngừng chạy.

    - <code>subscribe</code>: Nếu <code>observable</code> là một <code>class</code> thì **subscribe** chính là keyword <code>new</code>. Khi được gọi, <code>method</code> này sẽ tạo ra một <code>stream</code> dựa trên <code>observable</code> mà nó được gọi và chạy <code>stream</code> đó.

    - <code>subscription</code>: là một object được method <code>subscribe</code> trả về, object có một số method được dùng để điều khiển quá trình hoạt động của <code>executor</code>.

    - <code>operator</code>: chính là các <code>modifier</code>, nhưng chúng cũng đóng vai trò là người vận chuyển các gói data đến <code>stream</code>.

### **b.3 Cách Observable hoạt động**
- Đến đây, các bạn có thể thắc mắc tại sao mình lại đi giải thích cách observable hoạt động mà không phải cách <code>rxjs</code> hoạt động thì đó là vì rxjs là một hệ sinh thái bao gồm một số công cụ chính giúp chúng ta mô phỏng lại mô hình **reactive programming** và một trong số chúng là <code>observable</code>.

- Để giúp mọi người hiểu được cách <code>observable</code> hoạt động, mình đã tạo một observable mô phỏng lại một method <code>setInterval</code> có thời gian chạy cố định là 1s.

```typescript
import { Observable } from "rxjs";

const interval$ = new Observable(observer => {
    let intervalCounter = 0;
    const intervalInstance = setInterval(() => {
        intervalCounter++;
        observer.next(intervalCounter);

        if(intervalCounter === 3) {
            clearInterval(intervalInstance);
            observer.error('error');
            observer.complete();
        }

    }, 1000);
});

const observer = {
    next: intervalCounter => {
        console.log(intervalCounter);
    },
    error: error => {
        console.error(error);
    },
    complete: () => {
        console.log('complete');
    }
}

interval$.subscribe(observer);

// kết quả:
//
// 1
// 2
// 3
// error
```

- Đầu tiên là dấu `$` nằm ở đuôi của biến <code>interval$</code>, dấu này chỉ là một <code>convention</code> khi các bạn đặt tên một biến chứa một <code>observable</code> thôi nhé.

- Tiếp đến là constructor <code>Observable</code>, constructor này được dùng để tạo ra các object <code>observable</code> và như mình có nói ở trên, các bạn có thể tạm coi các <code>observable</code> hay các <code>instance</code> của constructor <code>Observable</code> là các <code>class</code> của các <code>stream</code>, và mỗi khi method <code>subscribe</code> của một <code>observable</code> được chạy thì một <code>stream</code> sẽ được tạo ra và chạy ngay lập tức cho đến khi <code>stream</code> này kết thúc quá trình chạy.

- Constructor này nhận vào một function(function này chính là một executor) và truyền vào trong function đó một <code>observer</code>, 3 method của object này là: <code>next, complete, error</code> và chúng chính là 3 method <code>next, complete, error</code> mà ta định nghĩa trong object <code>observer</code> và truyền vào trong method <code>subscribe</code>.

- Đi sâu hơn về <code>executor</code> thì một <code>executor</code> chính là cách mà một stream hoạt động. Hơi khó hiểu phải không. Nhưng nếu các bạn coi cách hoạt động của một method <code>setInterval</code> là một <code>stream</code> như ví dụ đầu tiên ở phần giới thiệu về <code>reacive programming</code> thì các ban có thể thấy <code>executer</code> chỉ là một function được dùng để chạy method <code>setInterval</code>, điều duy nhất ngoài việc gọi method <code>setInterval</code> mà <code>executor</code> làm đó là chạy các <code>method: next, complete, error</code> của object <code>observer</code> mà nó nhận được (đây là khi các <code>operator</code> này đóng vai trò là những người vận chuyển data). Mỗi khi method <code>subscribe</code> của một <code>observable</code> được chạy thì <code>executor</code> của <code>observable</code> đó sẽ được chạy và mỗi khi một <code>executor</code> được chạy thì được nhiên là ... một <code>stream</code> sẽ được tạo ra. Okay, bây giờ thì các bạn có thể từ bỏ việc coi <code>observable</code> như là một <code>class</code> của các <code>stream</code> được rồi đấy.

- Khác với <code>stream hay observable, Observer</code> khá là dễ hiểu. Nếu các bạn đọc kỹ đoạn code ở trên và test thử thì object <code>observer</code> được truyền vào trong <code>executor</code> chính mà object <code>observer</code> mà các bạn truyền vào trong method subscribe. Thực chất thì mình đang gọi method <code>next</code> của object <code>observer</code> mà mình truyền vào trong subscribe khi mình viết: <code>observer.next(intervalCounter);</code>, đây cũng chính là cách mà các <code>operator</code> được chạy và khi chúng được chạy thì chúng sẽ đóng vai trò là các <code>operator</code>, các bạn có thể thấy mình **cố tình** gọi method <code>error: observer.error('error');</code> khi <code>setInterval</code> chạy được 3 lần.

#### **b.3.1 Dừng chạy-complete một observable**
- Như mình đã đề cập ở mục b.2, thì <code>subscription</code> có một số method được dùng để điều khiển quá trình chạy của một stream và method có lẽ là quan trọng nhất và cũng được dùng phổ biến nhất là <code>unsubscribe</code>.

- Đúng như cái tên của nó, nghiệm vụ của <code>unsubscribe</code> hoàn toàn đối lập với <code>subscribe</code>, đó là dừng chạy một <code>executor</code>.

```typescript
...
const subscription = interval$.subscribe(observer);

setTimeout(() => {
    subscription.unsubscribe();
}, 2000);
```

- Khi thay đoạn <code>subscribe</code> ví dụ trong phần b.3 bằng ví dụ này thì các bạn có thể thấy <code>executor</code> của chúng ta chỉ in log ra inspector đúng một lần. Đó là vì chúng ta đã dừng executor.

- Để có thể bắt được sự kiện khi một <code>executer</code> dừng chạy thì ta có thể return một function ngay trong <code>executor</code>, function này sẽ được class <code>Observable</code> class gọi khi hàm <code>unsubscribe</code> của một <code>observable</code> được gọi. Việc bắt sự kiện này có lẽ không phổ biến lắm, nhưng trong một số trường hợp cụ thể như ví dụ dưới đây thì nó rất quan trọng vì nó sẽ giúp chúng ta dừng hàm <code>setInterval</code> khi ta không cần dùng <code>observable interval$</code> nữa.

```typescript
import { Observable } from "rxjs";

// Object.create method sẽ gọi new Observable() ngầm giúp chúng ta
const interval$ = Observable.create(observer => {
        let intervalCounter = 0;
        const intervalInstance = setInterval(() => {
        intervalCounter++;
        observer.next(intervalCounter);
    }, 1000);
   
    // function này sẽ được chạy khi interval$ bị unsubscribe
    return () => {
        console.log('complete');
        clearInterval(intervalInstance);
    }
});

const subscription = interval$.subscribe(
   // Observable class sẽ tự động map lần lượt các function dưới đây vào các method next, error và complete
   // của object observer mà nó chuyền vào trong executor
  intervalCounter => console.log(intervalCounter),
  error => console.log(error),
);

setTimeout(() => {
  subscription.unsubscribe();
}, 2000);

// kết quả:
//
// 1
// complete
```

#### **b.3.2 Observable contract**
- <code>Observable contract</code> là một quy định trong <code>rxjs</code> trong đó:
    - Một <code>stream</code> sẽ không thể nhận các gói data sau khi <code>observer.complete</code> được gọi hoặc sau khi <code>observer.error</code> được gọi.
    - Khi <code>observer.error</code> đã được gọi thì stream sẽ không thể nhận được bất kỳ gói data nào từ hay trigger <code>observer.complete</code> nữa.
    - Khi <code>observer.complete</code> đã được gọi thì stream sẽ không thể nhận được bất kỳ gói data nào từ trigger <code>observer.error</code> nữa.

#### **2.3.3 Observable được dùng để xử lý bất đồng bộ ?**
- Mình thấy rất nhiều người cho rằng rxjs hay observable được dùng để xử lý bất đồng bộ. Nhưng thực tế thì cả rxjs và observable đều không hề liên gì đến việc xử lý bất đồng bộ, tất cả những gì rxjs làm chỉ là giúp chúng ta mô phỏng lại reactive programming. Việc một observable có được dùng để xử lý bất đồng bộ hay không hoàn toàn phụ thuộc vào cách chúng ta xử lý executor. Như ví dụ dưới đây mình có tạo một observable xử lý đồng bộ.

```typescript
import { Observable, noop } from "rxjs";

const syncObservable$ = new Observable(observer => {
  observer.next(1);
  observer.next(2);
  observer.next(3);
  observer.complete();
});

syncObservable$.subscribe(
  intervalCounter => console.log(intervalCounter),
  noop,
  () => console.log('complete')
);

console.log(4);

// kết quả:
//
// 1
// 2
// 3
// complete
// 4
```

- Như các bạn có thể thấy, mình không về gọi method observer.error trong executor ở ví dụ trên nên việc định nghĩa một function để xử lý error cho observable syncObservable$ là hoàn toàn không cần thiết. Với những trường hợp như này hoặc nếu các bạn không quan tâm đến việc xử lý một method nào đó trong 3 method của object observer thì các bạn có thể truyền noop vào vị trí của method tương ứng nhé.
