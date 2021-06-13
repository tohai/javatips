# Chương 10: Bàn luận về comment

Câu hỏi đặt ra là có cần thiết phải comment không?

## 10.1 Comment dư thừa

Hãy xem xét ví dụ dưới đây

```java
// calculate sum of subjects point
private int sumSubjectPoint(List<Integer> points){
    // return sum of lisst with start sum = 0
    return points.stream().reduce(0, Integer::sum);
}
```

Hãy xem ví dụ trên ta có thể thấy là cá comment thực ự dư thừa . Tại sao ta lại phải comment cho nhưng điều đã được thể hiện rất rõ nghĩa của nó ở trên code?

## 10.2 Commnet khiên ta hiểu sai

`Và đôi khi các commment sẽ đưa chúng ta đi lạc`. ví dụ như tường hợp sau tôi đã gặp phải khi làm các dự án với Nhật.\
Tôi lấy ví dụ code như dưới:

```java
    // number of ship
    Ship ship =  shipService.getShipByKey(shipKey); 
```

WTF các comment chẳng liên quan gì đến code cả? Rồi ta lại tự hỏi code sai hay comment sai? Và ta lại mất thời gian đi xác nhận xem cái nào là đúng.\
Lúc này comment trở thành một comment tồi.

Ở đây tôi cũng nói luôn về vấn đề tài liệu - detail design

Tài liệu thiết kế có nhắc đến lấy ra toàn bộ tàu đang neo thả ở một bến cảng ở một chức năng.\
Tuy nhiên khi tôi vào trong source code của chính chức năng này thì tôi lại chẳng thể tìm được nó hoặc gọi ở đâu theo đúng nhưng gì mà tôi đã truy theo flow chương trình. Sau một hồi tôi tìm kiếm thì nó lại không ở trong code chính mà nó tồn lại trong comment!!! WTF..cái đéo gì đang diễn ra vậy? Khi tôi xác nhận lại phía khách hàng thì tôi có được thông tin chính thức là `tài liệu này đã tồn tại 12 năm và nó đã trải qua nhiều phase cập nhật code`. Vâng,trên một khía cạnh nào đó có thể nói nó đã không còn giá trị để xử dụng.

Ở đây tôi không chê trách rằng việc làm ra tài liệu là không tốt. Tuy nhiên vói các tài liệu một khi đã làm ra thì khi sửa đổi code hãy sửa dổi lại tài liệu này. như vậy nới mới thực sự mang ý nghĩa về mặt lưu trữ lịch sử. Và các tài liệu này có thể chỉ cho ta khái quát nhanh về tổng quan hệ thống và chứa các mặt về nghiệp vụ phức tạp mà đôi khi đọc từ code rất khó.

## 10.3 Comment để đánh dấu thời gian sửa đổi và lưu version trước

Hãy xem xet comment trong đoạn code dưới đây

```java
    // START 20210303 bill update for getInternationalProducts
    // List<Product> products = productService.getProducts();

    List<Product> products = productService.getInterProducts(Constant.INTERNATIONAL_KIND);
    // END 20210303 bill update for getInternationalProducts
```

Bạn có thấy nó cần thiết hay không?Vậy tại sao lại nó vẫn còn tồn tại?

Code đó đã từ lâu và trước khi các công cụ quản lý source nguồn ra đời và không còn nhu cầu sửa đổi. Nên họ sẽ hạn chế tối đa động vào code hiện có.

Hoặc cũng có thể là điều bắt buộc theo luật của một khách hàng khi làm outsource.

Nhưng thực chất nó không cần thiết. Tại vì chúng ta có thể ta được ai đã cập nhật , cập nhật những gì, và từ bao giờ một cách dễ dàng khi chúng ta dùng các công cụ hỗ tợ quản lý source code như git, svn ...

Tôi đã từng chửi nhau vì vấn đề này. Vì tôi không muốn code của mình trở nên rác và cũng không muốn mất thời gian phải comment cho những điều mà code đã thể hiện một cách rõ ràng.

## 10.4 tổng kết

Như vậy thì việc comment là điều không cần thiết? Không, nó thực sự cần thiêt. tuy nhiên bạn phải biết khi nào cần comment và khi nào không cần thiết.

Vậy khi nào thì ta cần phải comment? Đối với code sạch thì việc comment chỉ tồn tại ở 2 nơi:

1. Khi gặp một nghiệp vụ phức tạp và code trở nên tối nghĩa
2. Comment đánh dấu nhưng thứ mà ta cần phải xem xet hoặc làm sau với cu pháp //TODO:

Như bạn thấy đấy, Hãy để comment của mình trở nên có giá trị. Chứ không phải comment một cach vô tội vạ và dư thừa.
