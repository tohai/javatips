# Chương 8: Code tối ưu hay code sạch

## 8.1 Code sạch

Code sạch (clean code): được hiểu theo nhiều nghĩa khác nhau thường thì mỗi nhà lập trình thường có một quy ước như thế nào là code sạch. nhưng về mặt cơ bản nó phải có nhưng tính chất như sau:

- Code dễ hiểu, đơn giản.
- Code dễ mở rộng.
- Chi phí của việc bảo trì là thấp.
- Các đối tượng hạn chế các liên kết cứng
- Tuân thủ chặt ché nguyên tắc S.O.L.I.D

## 8.2 Code tối ưu

Code tôi ưu: Code đòi hỏi nâng cao về mặt hiệu năng và thường thì chúng sẽ là các lệnh thao tác trên bit. Đa phần code tối ưu đều khó hiểu.

## 8.3 Code tối ưu hay code sạch

Khi nào nên chọn code tối ưu và khi nào nên chọn code sạch? Đây là một câu hỏi khó không biết đánh dố bao nhiêu thế hệ. Ai cũng biết rằng khi mới bắt đầu nên đặt nhưng nền móng là code sạch. Tuy nhiên, không phải ở đâu cũng có thể làm được việc đó. Tôi thấy người ta đang code trên nền là code sạch và sau đó có các ý kiến trái triều nhau ghép một đoạn code tối ưu vào. Vâng nếu chỉ là một số ít thôi thì có lẽ nó sẽ là một điều tốt đẹp. Và bạn cũng biết được là `đã có lần một thì có lần hai , lần ba,...`.\
Vâng và sau đó ai cũng phá kỉ luật trong code vì ai cũng tự tin là tôi code hiệu năng cao và nó hợp lý. :D. Nó không có đơn giản như vậy . hay nhớ răng khi code hiệu năng được xen vào thì code sẽ khó bảo trì hơn. và đôi khi khiến code trở nên cứng hơn. và chi phí cho việc bảo trì trở nên to hơn. Ở đây tôi chưa để cập đến code thối mà các ông tạo ra và bảo nó là code hiệu năng cao.

Vậy Khi nào nên dùng code hiệu nang và khi nào nên dùng code sạch?

Nó không có câu trả lời cụ thể, cùng một đoạn code nhưng ở ngữ cảnh này thì nên dùng code sạch, nhưng ở một ngữ cảnh khác thì nên dùng code tối ưu.

ví dụ:

```java
for(int windex =0; windex < wUsers.size(); windex++){
    //Block A
}

for(int windex =0; windex < wUsers.size(); windex++){
    //Block B
}
```

Ở đây tôi giả định rằng cá khối A, B độc lập không phụ thuộc nhau.

Rõ ràng ở đây là ta pahir duyệt wUsers 2 lần và nhiều người đã gộp chúng lại như sau

```java
for(int windex =0; windex < wUsers.size(); windex++){
    //Block A

    //Block B
}
```

Về mặt nghiệp vụ thì chúng chẳng có gì sai. tuy nhiên để người đọc có thể nhìn vào và biết ngay được thì buộc lòng phải comment. và thật phức tạp khi A hoặc B thay dôi nghiệp vụ. bạn có thể tạo hàm và nhấn mạnh chứ năng của nó tuy nhiên hãy nhớ rằng hàm của bạn sẽ là thực hiện 2 chức năng và nó vô hình đã vi phạm cá nguyên tắc thiết kế hàm.

như vậy nói nó là sai thì cũng không phải. hay thử hình dung wUsers chứ 10 triệu dữ liệu. như vật thì chúng lại thực xự hợp lý thay vì code sạch tôi sẽ phải duyệt nhưng 20 triệu lần trong khi code tối ưu tôi chỉ cần 10 triệu lần.

Ở đây tôi nhấn mạnh điều đầu tiên: `Đôi khi ta phải chấp nhận đánh đổi giữ code sach và hiệu năng hệ thống`.

Vậy nên chọn như thế nào:

- Nên chọn code tối ưu khi bạn cực kì hiểu sâu và code này sẽ gần như không phải thay đổi nghiệp vụ một lần nào nữa.
- Ước lượng chi phí của code, nếu 2 bên chênh lệch không quá nhiều thì hãy chọn code sạch. ví dụ như wUsers có 20 phần tử thì ta thừa biết với hệ thống máy tính hiện nay. thì các doạn code này thực hiện khá là nhanh.
