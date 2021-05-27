# Chương 2: Các vấn đề dễ nhầm lẫn trong lập trình hướng đối tượng

## 2.1 Bốn tính chất của hướng đối tượng

Bốn tính chất của hướng đối tượng:

- Tính trừu tượng (abstraction)
- Tính đóng gói (encapsulation)
- Tính kế thừa (Inheritance)
- Tính đa hình (polymorphism)

### 2.1.1 Tính trừu tượng (Abstraction)

Tôi có đọc khá nhiều quan điểm khác nhau về tính trừu tượng. Trong đó có một số định nghĩa được đưa ra như sau:

`Trừu tượng hóa là quá trình ẩn các chi tiết bên trong của một ứng dụng khỏi thế giới bên ngoài. Trừu tượng được sử dụng để mô tả mọi thứ bằng những thuật ngữ đơn giản. Nó được sử dụng để tạo ranh giới giữa ứng dụng và các chương trình khách.`

Nó thực sự khó hiểu. Còn có một định nghĩa khác dễ hiểu hơn nhiều:
`Trừu tượng hóa là quá trình phản ánh một đối tượng thực vào chương tình phẩn ánh thuộc tính ( đặc điểm) và hành vi (hành động) cửa đối tượng thực`.

Cái nào cũng đúng cả. Tuy nhiên từ các định nghĩa này tôi cá chắc rằng bạn hoàn toàn chưa nhìn ra được. Các đặc điểm của nó.

Các kiểu trừu tượng hóa:

1. Trừu tượng hóa dữ liệu.
2. Trừu tượng hóa hành vi.

Không có gì để nói về trừu tượng hóa dữ liệu. nó phản ánh hoàn toàn về `thuộc tính` trong đúng định nghĩa thứ 2. Nếu cần, quyền truy cập vào dữ liệu của Đối tượng được cung cấp thông qua một số method (get/set).

Về trừ tượng hóa hành vi. Cái này bạn có thể hiểu là phản ánh `hành vi` của nó từ đời thực theo đúng định nghĩa.

Nói chung nó chả có cái gì cả. không đâu nếu chỉ dựa vào định nghĩa thôi nó không thể hiện rõ được về khái niệm mơ hồ này đâu.

tôi sẽ lấy một ví dụ làm thưc tế như sau:

Hệ thống mà chúng tôi đang làm đó là về thương mại điện tử. Trong quá trình phát triển khách hàng họ yêu cầu là gửi mail đến người mua hàng.\
Vâng việc đó cũng đơn giản thôi. Tạo một lớp email với phung thức sendMail.\
Nhẹ nhàng quá nhỉ. mất khaongr 30-40 phút là có thể làm xong bao gồm cả test. Việc này nó chả là gì lớn lao cả. Việc không dừng lại ở đó, khách hàng lại yêu cầu chúng tôi làm chứ năng gửi tin nhắn đến khách hàng. Tại thời điểm này đã xuất hiện các thành phần phá hoại, họ đề suất ném thg send mesage vào thăng mail. Vâng hành động của mấy thằng thiểu năng. và tất nhiên là chả có ai đồng ý. Họ tạo thêm một class Class SMS thôi và với phương thức sendSMS có gì đâu ta. việc vẫn chưa dừng lại, khách hàng lại yêu cầu gửi notification vào trang cá nhân của khách mua hàng. Vâng như một thói quen ,họ tọa lớp Notification với phương thức là sendNotification. và đến thời điểm này cũng đã có thành viên nhận ra rằng đã sai ngay từ thời điểm thiết kế ban đầu.
nhưng bởi đã có quá nhiều dependencies được tạo ra và không còn cách nào khác ngoài việc tiếp tục chuỗi sai lầm tệ hại để không phá vỡ kết cấu hệ thống.

Tất cả những gì nên được tạo ra là class Sender với phương thức sendMessage, nhưng không nhiều người nhận ra ý tưởng đó ngay từ những bước thiết kế đầu tiên.

**`Sau một thời gian triển khai, chúng tôi nhận ra rằng thứ mình tạo ra hoàn toàn khác so với những tưởng tượng ban đầu về chúng.`**

**Nếu không thể biết trước những gì sẽ thay đổi, hãy trừu tượng hoá nó nhiều nhất có thể.**

### 2.1.2 Tính đóng gói (encapsulation)

Tính chất này không cho phép người sử dụng các đối tượng thay đổi trạng thái nội tại của một đối tượng. Chỉ có các phương thức nội tại của đối tượng cho phép thay đổi trạng thái của nó. Việc cho phép môi trường bên ngoài tác động lên các dữ liệu nội tại của một đối tượng theo cách nào là hoàn toàn tùy thuộc vào người viết mã. Đây là tính chất đảm bảo sự toàn vẹn của đối tượng.

Không cho phép can thiệp trực tiếp vào các thuộc tính của đối tượng. chỉ cho phép thông qua các hàm có quyền hạn trong đối tượng.

Chú ý các class liên quan đến nhau có thể để trong cùng một package.

### 2.1.3 Tính kế thừa (Inheritance)

Đặc tính này cho phép một đối tượng có thể có sẵn các đặc tính mà đối tượng khác đã có thông qua kế thừa. Điều này cho phép các đối tượng chia sẻ hay mở rộng các đặc tính sẵn có mà không phải tiến hành định nghĩa lại.

### 2.1.4 Tính đa hình (polymorphism)

Đa hình trong Java cho phép đối tượng thể hiện các hành vi khác nhau ở các giai đoạn khác nhau trong vòng đời của nó.

Tôi lấy ví dụ như sau cho dễ hiểu:
Cùng là lớp Animal nhưng lớp Duck và Dog. có hành vi chung bark, nhưng về chi tiết thì tiếng của chúng lại khác nhau.

Một ví dụ khác cùng là kế thừa từ lớp vịt thế nhưng hành vi của vịt trời và vịt nhà lại khác nhau.

## 2.2 Tổng kết

Toàn là các khái niệm mơ hồ ranh giới giữa các tính chất nó cũng rất mơ hồ :V
