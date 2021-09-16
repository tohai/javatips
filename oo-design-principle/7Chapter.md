# Các nguyên tắc khác

## Chung

- [KISS]
- [YAGNI]
- [Làm điều đơn giản nhất mà có thể thành công]
- [Tách mối quan tâm] (# mối quan tâm tách biệt)
- [Giữ đồ khô]
- [Mã cho người bảo trì]
- [Tránh tối ưu hóa sớm]
- [Quy tắc hướng đạo sinh]

## Liên mô-đun / Lớp

- [Khớp nối thu nhỏ]
- [Định luật Demeter]
- [Thành phần thừa kế]
- [Trực giao]
- [Nguyên tắc mạnh mẽ]
- [Inversion of Control]

## Mô-đun / Lớp

- [Tối đa hóa sự gắn kết]
- [Nguyên tắc thay thế Liskov]
- [Nguyên tắc mở / đóng]
- [Nguyên tắc trách nhiệm duy nhất]
- [Ẩn chi tiết triển khai]
- [Định luật xoăn]
- [Encapsulate What Changes]
- [Nguyên tắc phân tách giao diện]
- [Tách truy vấn lệnh]

### KISS

**Giữ nó đơn giản, ngu ngốc**: hầu hết các hệ thống hoạt động tốt nhất nếu chúng được giữ đơn giản
hơn là làm cho phức tạp.

Tại sao:

- Ít mã hơn, mất ít thời gian để viết hơn, ít lỗi hơn và dễ sửa đổi hơn.
- Đơn giản là sự tinh tế cuối cùng.
- Có vẻ như sự hoàn hảo đạt đến không phải khi không còn gì để bổ sung, mà là
  khi không còn gì để lấy đi.

### YAGNI

**Bạn không cần phải thực hiện**: không thực hiện một cái gì đó cho đến khi nó là cần thiết.

Tại sao

- Bất kỳ tác phẩm nào chỉ được sử dụng cho một tính năng cần thiết vào ngày mai, đồng nghĩa với việc mất
  nỗ lực từ các tính năng cần thực hiện cho lần lặp hiện tại.
- Nó dẫn đến mã phình to; phần mềm trở nên lớn hơn và phức tạp hơn.

Thế nào

- Luôn thực hiện mọi thứ khi bạn thực sự cần, không bao giờ khi bạn chỉ cần
  thấy trước rằng bạn cần chúng.

### Làm điều đơn giản nhất có thể thành công

Tại sao

- Tiến bộ thực sự so với vấn đề thực sự được tối đa hóa nếu chúng ta chỉ làm việc với những gì
  vấn đề thực sự là.

Thế nào

- Tự hỏi bản thân: "Điều gì đơn giản nhất có thể hoạt động được?"

## Tách các mối quan tâm

Tách các mối quan tâm là một nguyên tắc thiết kế để tách một chương trình máy tính
thành các phần riêng biệt, sao cho mỗi phần giải quyết một mối quan tâm riêng biệt. Vì
ví dụ, logic nghiệp vụ và giao diện người dùng của một ứng dụng là
mối quan tâm riêng biệt. Thay đổi giao diện người dùng không yêu cầu thay đổi đối với
logic kinh doanh và ngược lại.

Trích dẫn [Edsger W. Dijkstra]

> Đó là điều mà đôi khi tôi gọi là "sự tách biệt của những mối quan tâm", thậm chí là
> nếu không hoàn toàn khả thi, thì đây vẫn là kỹ thuật duy nhất có hiệu quả
> thứ tự suy nghĩ của một người, mà tôi biết. Đây là điều tôi muốn nói khi "tập trung
> sự chú ý của một người trên một số khía cạnh ": không có nghĩa là phớt lờ những điều khác
> khía cạnh, nó chỉ thực hiện công bằng với thực tế là từ khía cạnh này
> xem, khác là không liên quan.

Tại sao

- Đơn giản hóa việc phát triển và bảo trì các ứng dụng phần mềm.
- Khi các mối quan tâm được tách biệt rõ ràng, các phần riêng lẻ cũng có thể được sử dụng lại
  như được phát triển và cập nhật độc lập.

Thế nào

- Chia chức năng chương trình thành các mô-đun riêng biệt chồng chéo lên nhau ít nhất
  khả thi.

## DRY

**Đừng lặp lại chính mình**: Mỗi phần kiến ​​thức phải có một,
đại diện rõ ràng, có thẩm quyền trong một hệ thống.

Mỗi phần chức năng quan trọng trong một chương trình phải được triển khai trong
chỉ một nơi trong mã nguồn. Trường hợp các chức năng tương tự được thực hiện bởi
các đoạn mã riêng biệt, nhìn chung sẽ có lợi khi kết hợp chúng thành từng đoạn mã
trừu tượng hóa các phần khác nhau.

Tại sao

- Sao chép (sao chép vô tình hoặc có mục đích) có thể dẫn đến bảo trì
  ác mộng, bao thanh toán kém và mâu thuẫn logic.
- Việc sửa đổi bất kỳ phần tử đơn lẻ nào của hệ thống không yêu cầu thay đổi
  các yếu tố không liên quan về mặt logic khác.
- Ngoài ra, các yếu tố có liên quan đến logic đều thay đổi có thể dự đoán được và
  đồng nhất, và do đó được giữ đồng bộ.

Thế nào

- Đặt các quy tắc nghiệp vụ, biểu thức dài, câu lệnh if, công thức toán học, siêu dữ liệu,
  vv ở một nơi duy nhất.
- Xác định nguồn chính xác, duy nhất của mọi phần kiến ​​thức được sử dụng trong
  hệ thống của bạn và sau đó sử dụng nguồn đó để tạo các phiên bản có thể áp dụng
  kiến thức (mã, tài liệu, bài kiểm tra, v.v.).
- Áp dụng
  [Quy tắc ba] (<https://en.wikipedia.org/wiki/Rule_of_three_(computer_programming)>).

## Mã cho Người bảo trì

Tại sao

- Bảo trì cho đến nay là giai đoạn tốn kém nhất của bất kỳ dự án nào.

Thế nào

- mã dành người bảo trì.
- Luôn viết mã như thể người cuối cùng duy trì mã của bạn là một kẻ bạo lực kẻ biến thái nhân cách biết bạn sống ở đâu.
- Luôn viết mã và bình luận theo cách mà nếu ai đó có vài khía cạnh nhỏ hơn nhận mã, họ sẽ thích thú khi đọc và học hỏi từ đó.
- [Đừng khiến tôi phải suy nghĩ] (<http://www.sensible.com/dmmt.html>).
- Sử dụng
  [Nguyên tắc ít kinh ngạc] (<https://en.wikipedia.org/wiki/Principle_of_least_astonishment>).

## Tránh tối ưu hóa sớm

Trích dẫn [Donald Knuth]:

> Các lập trình viên lãng phí rất nhiều thời gian để suy nghĩ hoặc lo lắng về
> tốc độ của các phần không quan trọng trong chương trình của họ và những nỗ lực này
> hiệu quả thực sự có tác động tiêu cực mạnh mẽ khi gỡ lỗi và
> bảo trì được xem xét. Chúng ta nên quên đi những hiệu quả nhỏ, nói
> khoảng 97% thời gian: tối ưu hóa quá sớm là gốc rễ của mọi điều xấu. Tuy nhiên, chúng tôi
> không nên bỏ qua cơ hội của chúng ta trong 3% quan trọng đó.

Hiểu được những gì được và không phải là "quá sớm" là rất quan trọng.

Tại sao

- Chưa biết trước những nút thắt sẽ nằm ở đâu.
- Sau khi tối ưu hóa, nó có thể khó đọc hơn và do đó khó duy trì.

Thế nào

- [Làm cho nó hoạt động Làm cho nó nhanh chóng] (<http://wiki.c2.com/?MakeItWorkMakeItRightMakeItFast>)
- Không tối ưu hóa cho đến khi bạn cần và chỉ sau khi lập hồ sơ, bạn mới phát hiện ra nút cổ chai để tối ưu hóa.

## Quy tắc Hướng đạo sinh

Hội Nam Hướng đạo Hoa Kỳ có một quy tắc đơn giản mà chúng ta có thể áp dụng cho
nghiệp vụ: "Hãy để khu cắm trại sạch hơn bạn tìm thấy nó". Quy tắc hướng đạo của cậu bé
nói rằng chúng ta nên để mã sạch hơn chúng ta đã tìm thấy.

Tại sao

- Khi thực hiện các thay đổi đối với cơ sở mã hiện có, chất lượng mã có xu hướng giảm,
  nợ kỹ thuật tích lũy. Tuân theo quy tắc trinh sát cậu bé, chúng ta nên lưu ý
  chất lượng với từng cam kết. Nợ kỹ thuật được chống lại bằng cách liên tục
  tái cấu trúc, không có vấn đề nhỏ như thế nào.

Thế nào

- Với mỗi cam kết, hãy đảm bảo rằng nó không làm giảm chất lượng codebase.
- Bất cứ khi nào ai đó nhìn thấy một số mã không rõ ràng như nó cần, họ
  nên tận dụng cơ hội để sửa chữa nó ngay tại đó và sau đó.

## Giảm thiểu Khớp nối

Sự kết hợp giữa các mô-đun hoặc các thành phần là mức độ tương hỗ của chúng sự phụ thuộc lẫn nhau; khớp nối thấp hơn là tốt hơn. Nói cách khác, khớp nối là xác suất rằng thành phần "B" sẽ bị "phá vỡ" sau một thay đổi không xác định cảu thành phần "A".

Tại sao

- Một thay đổi trong một mô-đun thường tạo ra hiệu ứng gợn sóng của những thay đổi trong mô-đun khác
  các mô-đun.
- Việc lắp ráp các mô-đun có thể đòi hỏi nhiều nỗ lực và / hoặc thời gian hơn do tăng
  sự phụ thuộc giữa các mô-đun.
- Một mô-đun cụ thể có thể khó sử dụng lại và / hoặc kiểm tra hơn vì phụ thuộc
  mô-đun phải được bao gồm.
- Các nhà phát triển có thể sợ thay đổi mã vì họ không chắc những gì có thể
  bị ảnh hưởng.

Thế nào

- Loại bỏ, giảm thiểu và giảm bớt sự phức tạp của các mối quan hệ cần thiết.
- Bằng cách ẩn các chi tiết thực hiện, việc ghép nối được giảm bớt.
- Áp dụng [Định luật Demeter] (# law-of-demeter).

## Định luật Demeter (Chương 6)

## Thành phần Thừa kế

Tốt hơn là bạn nên soạn những gì một đối tượng có thể làm hơn là mở rộng những gì nó đang có. Soạn, biên soạn khi có một quan hệ "has a" (hoặc "using a"), kế thừa khi "là một".

Tại sao

- Ít ghép nối giữa các lớp.
- Sử dụng tính kế thừa, các lớp con dễ dàng đưa ra các giả định và phá vỡ [LSP] (# lsp).
- Tăng tính linh hoạt: thích ứng với những thay đổi yêu cầu trong tương lai, nếu không
  yêu cầu cơ cấu lại các lớp miền kinh doanh trong mô hình kế thừa.
- Tránh các vấn đề thường liên quan đến những thay đổi tương đối nhỏ đối với một
  mô hình dựa trên kế thừa bao gồm một số thế hệ lớp.

Thế nào

- Xác định các hành vi của đối tượng hệ thống trong các giao diện riêng biệt, thay vì tạo
  mối quan hệ thứ bậc để phân phối các hành vi giữa các miền doanh nghiệp
  các lớp thông qua kế thừa.
- Kiểm tra [LSP] (# lsp) để quyết định thời điểm kế thừa.

## Trực giao

> Ý tưởng cơ bản của tính trực giao là những thứ không liên quan
> về mặt khái niệm không nên liên quan trong hệ thống.

Nguồn: [Be Orthogonal]

> Nó được liên kết với sự đơn giản; thiết kế càng trực giao thì càng ít
> ngoại lệ. Điều này làm cho việc học, đọc và viết các chương trình trở nên dễ dàng hơn trong một
> ngôn ngữ lập trình. Ý nghĩa của một đối tượng địa lý trực giao không phụ thuộc vào
> ngữ cảnh; các thông số quan trọng là đối xứng và nhất quán.

## Nguyên tắc mạnh mẽ

> Bảo thủ trong những gì bạn làm, tự do trong những gì bạn chấp nhận từ người khác

Các dịch vụ cộng tác phụ thuộc vào các giao diện khác. Thường thì các giao diện
cần phát triển khiến đầu bên kia nhận dữ liệu không xác định. Ngây thơ
triển khai từ chối cộng tác nếu dữ liệu nhận được không nghiêm ngặt
theo đặc điểm kỹ thuật. Việc triển khai phức tạp hơn sẽ vẫn hoạt động
bỏ qua dữ liệu mà nó không nhận ra.

Tại sao

- Đảm bảo rằng nhà cung cấp có thể thực hiện các thay đổi để hỗ trợ các nhu cầu mới, đồng thời gây ra
  sự cố tối thiểu cho khách hàng hiện tại của họ.

Thế nào

- Mã gửi lệnh hoặc dữ liệu đến các chương trình hoặc máy khác phải tuân theo
  hoàn toàn theo thông số kỹ thuật, nhưng mã nhận đầu vào phải chấp nhận
  đầu vào không tuân thủ miễn là ý nghĩa rõ ràng.

## Đảo ngược Kiểm soát

IoC đảo ngược dòng điều khiển. Nó còn được gọi là Nguyên tắc Hollywood,
"Đừng gọi cho chúng tôi, chúng tôi sẽ gọi cho bạn". Một nguyên tắc thiết kế trong đó tùy chỉnh được viết
các phần của chương trình máy tính nhận luồng điều khiển từ một chương trình chung
khuôn khổ. Nó mang hàm ý mạnh mẽ rằng mã có thể tái sử dụng và
mã vấn đề cụ thể được phát triển độc lập mặc dù chúng hoạt động
cùng nhau trong một ứng dụng.

Tại sao

- Tăng tính mô-đun của chương trình và làm cho nó có thể mở rộng.
- Để tách việc thực hiện một nhiệm vụ khỏi việc thực hiện.
- Để tập trung một mô-đun vào nhiệm vụ mà nó được thiết kế.
- Để giải phóng các mô-đun khỏi các giả định về các hệ thống khác và thay vào đó dựa vào
  hợp đồng.
- Để ngăn ngừa các tác dụng phụ khi thay thế một mô-đun.

Thế nào

- Sử dụng mẫu Factory
- Sử dụng mẫu Định vị dịch vụ
- Sử dụng Dependency Injection
- Sử dụng tra cứu theo ngữ cảnh
- Sử dụng mẫu Phương pháp Mẫu
- Sử dụng mẫu chiến lược

## Tối đa hóa sự gắn kết

Sự gắn kết của một mô-đun hoặc thành phần đơn lẻ là mức độ mà nó
trách nhiệm tạo thành một đơn vị có ý nghĩa. Tính liên kết càng cao càng tốt.

Tại sao

- Giảm độ phức tạp của mô-đun
- Tăng khả năng bảo trì hệ thống, vì những thay đổi logic trong miền ảnh hưởng đến
  ít mô-đun hơn và vì những thay đổi trong một mô-đun yêu cầu ít thay đổi hơn trong
  các mô-đun khác.
- Tăng khả năng tái sử dụng mô-đun, bởi vì các nhà phát triển ứng dụng sẽ tìm thấy
  thành phần họ cần dễ dàng hơn trong số các hoạt động gắn kết được cung cấp
  bởi mô-đun.

Thế nào

- Nhóm các chức năng liên quan chia sẻ một trách nhiệm duy nhất (ví dụ: trong một
  mô-đun hoặc lớp).

## Nguyên tắc thay thế Liskov (Chương 3)

## Nguyên tắc Mở / Đóng (Chương 2)

## Nguyên tắc Trách nhiệm Đơn lẻ (Chương 1)

## Ẩn Chi tiết Triển khai

Mô-đun phần mềm ẩn thông tin (tức là chi tiết triển khai) bằng cách cung cấp một giao diện và không bị rò rỉ bất kỳ thông tin không cần thiết nào.

Tại sao

- Khi việc triển khai thay đổi, các máy khách giao diện đang sử dụng không có thay đổi.

Thế nào

- Giảm thiểu khả năng tiếp cận của các lớp và thành viên.
- Không để lộ dữ liệu thành viên ở nơi công cộng.
- Tránh đưa các chi tiết triển khai riêng tư vào giao diện của một lớp.
- Giảm khớp nối để ẩn nhiều chi tiết triển khai hơn.

## Định luật Curly

Quy luật của Curly là về việc chọn một mục tiêu duy nhất, được xác định rõ ràng cho bất kỳ mục tiêu cụ thể nào bit mã: Do One Thing.

- [Luật của Curly: Do One Thing] (<https://blog.codinghorror.com/curlys-law-do-one-thing/>)
- [Quy tắc của Một hoặc Quy luật của Curly] (<http://grsmentor.com/blog/the-rule-of-one-or-curlys-law/>)

## Đóng gói Nội dung Thay đổi

Một thiết kế tốt xác định các điểm nóng có nhiều khả năng thay đổi và
đóng gói chúng đằng sau một giao diện. Khi một thay đổi dự kiến ​​xảy ra sau đó,
các sửa đổi được giữ ở địa phương.

Tại sao

- Để giảm thiểu các sửa đổi cần thiết khi xảy ra thay đổi.

Thế nào

- Đóng gói khái niệm thay đổi đằng sau một giao diện.
- Có thể tách các khái niệm khác nhau thành mô-đun riêng của nó.

## Nguyên tắc phân tách giao diện (Chương 5)

## Tách Truy vấn Lệnh

Nguyên tắc Phân tách Truy vấn Lệnh nói rằng mỗi phương thức phải là một lệnh thực hiện một hành động hoặc một truy vấn trả về dữ liệu cho người gọi nhưng không phải cả hai. Đặt một câu hỏi không nên sửa đổi câu trả lời.

Với nguyên tắc này được áp dụng, lập trình viên có thể viết mã tự tin hơn nhiều.
Các phương thức truy vấn có thể được sử dụng ở bất kỳ đâu và theo bất kỳ thứ tự nào vì chúng không thay đổi
nhà nước. Với các lệnh người ta phải cẩn thận hơn.

Tại sao

- Bằng cách phân tách rõ ràng các phương thức thành các truy vấn và lệnh, lập trình viên có thể
  mã với độ tin cậy bổ sung mà không cần biết cách triển khai của từng phương pháp
  thông tin chi tiết.

Thế nào

- Triển khai mỗi phương thức dưới dạng truy vấn hoặc lệnh
- Áp dụng quy ước đặt tên cho các tên phương thức ngụ ý rằng phương thức có phải là một
  truy vấn hoặc một lệnh
