# Chapter 1. Giới thiệu

## 1.1. Generics

Một giao diện hoặc lớp có thể được khai báo để nhận một hoặc nhiều tham số kiểu, được viết trong dấu ngoặc nhọn và phải được cung cấp khi bạn khai báo một biến thuộc giao diện hoặc lớp hoặc khi bạn tạo một phiên bản mới của lớp.

ta có ví dụ sau

```java
List<String> words = new ArrayList<String>();
words.add("Hello ");
words.add("world!");
String s = words.get(0)+words.get(1);
assert s.equals("Hello world!");
```

Lớp `ArrayList<E>` triển khai Danh sách interface `<E>`. Đoạn mã này khai báo biến `words` để chứa danh sách các chuỗi, tạo một thể hiện của ArrayList, thêm hai chuỗi vào danh sách và lấy lại chúng.

Trong Java trước generic, mã tương tự sẽ được viết như sau:

```java
List words = new ArrayList();
words.add("Hello ");
words.add("world!");
String s = ((String)words.get(0))+((String)words.get(1))
assert s.equals("Hello world!");
```

Không có generics, các tham số kiểu sẽ bị bỏ qua, nhưng `bạn phải ép kiểu rõ ràng bất cứ khi nào một phần tử được trích xuất từ danh sách`.

Trên thực tế, mã bytecode được biên dịch từ hai nguồn trên sẽ giống hệt nhau. Chúng tôi nói rằng generic được thực hiện bằng cách xóa vì các loại `List <Integer>`, `List <String>` và `List <List <String>>` đều được đại diện tại thời điểm chạy bằng cùng một loại, List. Chúng tôi cũng sử dụng tính năng xóa để mô tả quá trình chuyển đổi chương trình đầu tiên sang chương trình thứ hai. Thuật ngữ tẩy xóa là một cách hiểu sai nhỏ, vì quy trình xóa các tham số kiểu nhưng thêm ép kiểu.
