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

## 1.2. Boxing and Unboxing

Mọi kiểu trong java đề là kiểu tham chiếu hoặc nguyên thủy. Bảng dưới đây thể hiện các kiểu nguyên thủy và kiểu tham chiếu tương ứng của nó trong java.lang:

|Primitive|Reference|
|---------|---------
|byte|Byte|
|short|Short|
|int|Integer|
|long|Long|
|float|Float|
|double|Double|
|bool| Boolean|
|char|Character|

Ỏ đây kiểu nguyên thủy được gọi là Unboxing, còn kiểu tham chiếu là Boxing.

Trong java có tính năng truyền cưỡng chế Boxing và Unboxing. như ví dụ dưới đây.

```java
List<Integer> ints = new ArrayList<Integer>();
ints.add(1);
int n = ints.get(0);
```

Nó tương đương với đoạn code dưới đây

```java
List<Integer> ints = new ArrayList<Integer>();
ints.add(new Integer(1));
int n = ints.get(0).intValue();
```

Tôi có thêm nột lưu ý với các bạn hãy phân biệt rõ rnagf kiểu nguyên thủy và kiểu tham chiếu của nó

ví dụ hàm tính tổng

```java
// sum of Primitive int
public static int sum(List<Integer> ints) {
  int s = 0;
  for (int n : ints) { s += n; }
  return s;
}

// sum of Reference Integer
public static Integer sumInteger(List<Integer> ints) {
  Integer s = 0;
  for (Integer n : ints) { s += n; }
  return s;
}
```

Cách vận dụng 2 hàm này hãy cản thận với nó:

```java
List<Integer> bigs = Arrays.asList(100,200,300);
assert sumInteger(bigs) == sum(bigs);
assert sumInteger(bigs) != sumInteger(bigs);  // not recommended
```

hãy để ý sumInteger nó trả ra một đối tượng Integer. Nên hãy cẩn thận với dấu `==` vì nó sẽ so sánh hashcode của đối tượng. Nó thực sự nên viết sumInteger nếu và chỉ nếu nó cần đến giá trị trả về là null, nòn nếu không tôi khuyên bạn nên dùng các hàm trả về kiểu nguyên thủy.

## 1.3. Foreach

Lần nữa tôi đề cấp đến tính tổng

```java
List<Integer> ints = Arrays.asList(1,2,3);
int s = 0;
for (int n : ints) { s += n; }
assert s == 6;
```

Nó tương đương với :

```java
List<Integer> ints = Arrays.asList(1,2,3);
int s = 0;
for (Iterator<Integer> it = ints.iterator(); it.hasNext(); ) {
  int n = it.next();
  s += n;
}
assert s == 6;
```

Vòng lặp foreach cũng có thể được áp dụng cho mảng:

```java
public static int sumArray(int[] a) {
  int s = 0;
  for (int n : a) { s += n; }
  return s;
}
```

Bình thường chúng ta không nên remove một phần tử trong quá trình duyệt mảng, tuy nhiên vẫn có cách để đảm bảo nhiều công việc một lúc, như ví dụ xóa phần tử âm trong list dưới đây:

```java
public static void removeNegative(List<Double> v) {
  for (Iterator<Double> it = v.iterator(); it.hasNext();) {
    if (it.next() < 0) it.remove();
  }
}
```

Có một lưu ý ở đây bạn chỉ nên dùng for i khi cần thao tác với chỉ số. bởi nó cũng có lỗi tiềm ẩn. ví dụ như biên trên v.size() - nó sẽ gặp vấn đề nếu v là null

## 1.4 hàm tổng quát và Varargs

Varargs: kiểu truyền nhiều tham số cùng kiểu nhưng không rõ về mặt số lượng sẽ được tổng quát thành một list.

Đầu tiên tôi sẽ lấy một ví dụ về hàm tổng quát

```java
public static <T> List<T> toList(T[] arr) {
    List<T> list = new ArrayList<T>();
    for (T elt : arr) list.add(elt);
    return list;
  }
```

kiểu T là kiểu tông quát có thể hiểu kiểu t kế thừa từ Object.

Ta có thể dùng nó như sau:

```java
List<Integer> ints = Lists.toList(new Integer[] { 1, 2, 3 });
List<String> words = Lists.toList(new String[] { "hello", "world" });
```

việc đóng gói các giá trị vào mảng đôi khi là phức tạp ta có thể dùng kiểu Varargs để thay thế cho mảng T[] như sau :

```java
public static <T> List<T> toList(T... arr) {
    List<T> list = new ArrayList<T>();
    for (T elt : arr) list.add(elt);
    return list;
}
```

Ta có thể dùng nó như sau:

```java
List<Integer> ints = Lists.toList( 1, 2, 3 );
List<String> words = Lists.toList( "hello", "world" );
```

Varargs `T...` có thể coi là một kiểu viết tắt của `T[]` , khi biên dịch nó cũng sẽ biên dịch thnahr mảng thôi. và kiểu `T...` nó cũng chấp nhạn một mảng như bình thường :D.

Tuy nhiên nếu bạn dùng Varargs, bao giờ cũng sẽ có một cảnh báo dành cho bạn trong quá trình biên dịch.

Vậy khi nào nên dùng nó? `Varargs chỉ nên được sử dụng khi đối số không có kiểu chung`.

```java
List<Object> objs = Lists.<Object>toList(1, "two");
```

Ta cũng không thể rút gọn được cú pháp trên:

```java
List<Object> objs = <Object>toList(1, "two"); // compile-time error
```

Nó sẽ không được chấp nhận vì sẽ gây nhầm lẫn cho trình phân tích cú pháp.

## 1.5. Assertions

Mỗi lần xuất hiện của Assertions được theo sau bởi một biểu thức boolean được mong đợi để đánh giá thành true. Nếu các xác nhận được bật và biểu thức được đánh giá là false, thì một AssertionError sẽ được đưa ra, bao gồm cả dấu hiệu về vị trí xảy ra lỗi. Các xác nhận được kích hoạt bằng cách gọi JVM với cờ -ea hoặc -enableassertions.
