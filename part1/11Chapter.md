# Chương 11: Bàn luận về nên chọn công cụ nào để code

## 11.1 Sơ lược

Đây là một công cụ khó trả lời tuy nhiên ta phải đưa ra các quan điểm rõ ràng dựa trên các điều kiện cụ thể và được gì và mất gì

Đầu tiên tôi sẽ nói về các công cụ hiện đại.

1. Ưu điểm
    - Dễ dùng
    - Có các thành phần hỗ trợ như hỗ trợ hỗ trợ chạy, debug
    - Tích hợp nhiều thành phần vào cùng một tool như vậy khiến cho IDE trở nên đa năng
    - Hỗ trợ rất mạnh cho người dùng, đặc biệt là những người mới
2. Nhược điểm
    - Sự tiện lợi được đánh đổi bởi tốc độ chạy cũng như chiếm dụng bộ nhớ
    - Người dùng được hỗ trợ quá cao làm mất đi nền tảng của các thành phần ví dụ như biên dịch một chương trình hoặc debug một ctrinhf bằng tay
    - Nó không thể hỗ trợ với các môi trường không có GUI Desktop và sẽ tạo thành yếu điểm của người mới khi tiếp cận việc code mà không có giao diện dồ họa cũng như làm việc trong môi trường tùy biến cao.

Thời gian gần đây các công cụ hiện đại càng gia tăng thì tương đương với việc các công cụ này cũng trở nên nặng hơn. ví dụ vào năm 2015 tôi có 1 con máy tính cà tàng ram 4gb nó vẫn có thể chạy được các project java một cách bình thường. tuy nhiên những năm này ram 4gb đã không đủ để có thể làm việc một cách bình thường.
và nó hoàn toàn làm việc bình thường với các công cụ cổ điển.

Gần đây nổi cộm lên vscode, eclipse, spring tool suite, inteji...
ai thích dùng gì thì dùng cái đó

Tiếp theo tôi sẽ nói về các công cụ cổ điển.

1. Ưu điểm
    - Nhẹ, tốc độ nhanh, chiếm bộ nhứ ít
    - Mức độ tùy biến cực cao
2. Nhược điểm
    - Đòi hỏi cao từ người dùng nói một cách khác là kén người dùng
    - Mức tuỳ biến cao luôn đi cùng với việc phải tự thân tùy chỉnh
    - Yêu cầu có cơ sở vững
    - Thời gian ban đầu cho tìm hiểu công cụ

Nói chung thì ưu điểm của thằng này lại là nhược điểm của thằng kia và ngược lại.

## 11.2 Các cao thủ về code đánh giá

Câu hỏi đặt ra các cao thủ code họ dùng gì?\
Các cao thủ code thường hay khuyên nên dùng Vim hoặc emacs. Với lý do khá ra gì và này nọ.

Lí do mà hộ đưa ra như sau:

1. Khi mở mã nguồn bằng Vim/emacs họ gần như không cảm nhận được độ trễ của mã nguồn và có thể nói nó là ngay lập tức.
2. Họ quan điểm rằng dùng chuột sẽ làm tiêu tốn thời gian của họ vì họ phải di chuyển tay từ bàn phím đến chuột và ngược lại.
3. Khi dùng các công cụ cổ điển này kiến tốc độ và năng suất của họ tăng mạnh
4. Họ có thể tùy chỉnh những gì họ thích trên chính tool của họ
5. Họ có hiểu rõ hơn về nền tảng cũng như việc build, debug
6. Họ có thể làm việc trên tất cả các môi trường

## 11.3 Tổng kết

Dùng gì là quyền của bạn, miễn sao bạn cảm thấy thoải mái và hiệu quả với mình.
