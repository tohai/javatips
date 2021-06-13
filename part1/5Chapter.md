# Chương 5: format code

Trong trương này tôi sẽ nói về vấn đề format code, và team của bạn có cần một format chung hay không.

## 5.1 Sự cần thiết của format code

Lấy ví dụ sau:

- Code không được định dạng:

```java
    public class FitNesseServer implements SocketServer { private FitNesseContext
context; public FitNesseServer(FitNesseContext context) { this.context =
context; } public void serve(Socket s) { serve(s, 10000); } public void
serve(Socket s, long requestTimeout) { try { FitNesseExpediter sender = new
FitNesseExpediter(s, context);
sender.setRequestParsingTimeLimit(requestTimeout); sender.start(); }
catch(Exception e) { e.printStackTrace(); } } }
```

và đây là code đã được định dạng:

```java
public class FitNesseServer implements SocketServer {
    private FitNesseContext context;

    public FitNesseServer(FitNesseContext context) {
        this.context = context;
    }

    public void serve(Socket s) {
        serve(s, 10000);
    }

    public void serve(Socket s, long requestTimeout) {
        try {
            FitNesseExpediter sender = new FitNesseExpediter(s, context);
            sender.setRequestParsingTimeLimit(requestTimeout);
            sender.start();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

Vâng, Đó là 2 đoạn code giống hệt nhau về nội dung vận bạn sẽ muốn đọc đoạn code nào? và tất nhiên đa số đều muốn đọc đoạn code thứ 2 trừ nhưng kể thích khác người(thg điên). Ta thấy rằng với đoạn code 2 hoàn toàn vượt tooik so với đoạn 1:

- Nhìn nó gọn hơn dễ đọc hơn.
- Tính thẩm mỹ cao hơn.
- Phân chia và bố cục rõ ràng.

Hãy định dạng code mọi lúc

## 5.2 tại sao team bạn cần 1 định dạng code chung

Trong phần này tôi sẽ nói về việc ại sao team cần chung 1 format.

Hãy thử hình dung bạn dùng một số tool hiện đại để code như eclipse,... và chúng hoàn toàn có thể tự autoformat khi bạn save file. Và mỗi thành viên của team có một format của riêng mình. lúc đó rất dễ xảy ra conflict code dù trên thực tế nó chẳng thay đổi gì.

Và bạn sẽ mất thời gian để sửa confict này. Như thế bạn có vui không?

## 5.3 Khung nhìn màn hình code

Tại sao tôi lại gọi là khung nhìn code. Tôi gọi nó là như vậy bởi vì tất cả code được bạn nhìn trên màn hình mà bạn không cần phải kéo ngang để xem các các òng code dài dằng dặc kia.

Vậy ta thử ước lượng hiệu suất code của việc code tàn khung nhìn. Đầu tiên chúng ta phải hình dung người khác đọc code của bạn. Bắt đầu cái nhìn đàu tiên : người đó không thể nào khái quát code của bạn trong một khung nhìn và phải kéo chuột sang phải để xem code trang khung nìn của bạn. Và điều đó dẫn đến việc mất tập trung trong quá trình đọc code và review code. Và ta lại mất thời gian cho việc kéo chuột hoặc di nut [->] trên bàn phím.

Từ góc độ nào đi nữa thì nó cũng thực sự không có lợi cho ta.

Vậy câu hỏi đặt ra là một dòng nên chứa tối đa là bao nhiêu kí tự. Theo như kinh nghiệm của tôi thì nó rơi vào khoảng 80-120

## 5.4 Tổng kết

Như các quan điểm nêu ở trên ta nhận thấy rằng thực sự cần một format chung của cả bạn lấn team của bạn.
