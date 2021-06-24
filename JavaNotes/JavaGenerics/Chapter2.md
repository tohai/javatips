# Chapter 2. Kiểu con và ký tự đại diện

Định kiểu con là một tính năng chính của các ngôn ngữ hướng đối tượng như Java. Trong Java, một kiểu là một kiểu con của kiểu khác nếu chúng có liên quan với nhau bởi một mệnh đề extends hoặc implements

||||
|-------|----------------|--------|
|Integer|is a subtype of|Number|
|Double|is a subtype of|Number|
|`ArrayList<E>`|is a subtype of|`List<E>`|
|`List<E>`|is a subtype of|`Collection<E>`|
|`Collection<E>`|is a subtype of|`Iterable<E>`|

Kiểu con có tính bắc cầu, có nghĩa là nếu một kiểu là kiểu con của kiểu thứ hai và kiểu thứ hai là kiểu con của kiểu thứ ba, thì kiểu đầu tiên là kiểu con của kiểu thứ ba.

Mọi kiểu tham chiếu là một kiểu con của Object và Object là một supertype của mọi kiểu tham chiếu.

**`Nguyên tắc thay thế: một biến của một kiểu nhất định có thể được gán một giá trị của bất kỳ kiểu con nào của kiểu đó và một phương thức có tham số của kiểu đã cho có thể được gọi với một đối số của bất kỳ kiểu con nào của kiểu đó.`**

Xem xét interface `Collection<E>`. Một trong những phương thức của nó là add, nhận tham số kiểu `E`:

```java
interface Collections<E> {
  public boolean add(E elt);
  ...
}
```

Theo Nguyên tắc Thay thế, nếu chúng ta có một tập hợp các số, chúng ta có thể thêm một số nguyên hoặc một nhân đôi vào nó, vì Integer và Double là các kiểu con của Number.

```java
List<Number> nums = new ArrayList<Number>();
nums.add(2);
nums.add(3.14);
assert nums.toString().equals("[2, 3.14]");
```

ta có thể nhận thấy `Integer` là con của `Number`. khi vận dụng nguyên tắc thay thế có thể bạn sẽ gặp rắc rối:

```java
List<Integer> ints = Arrays.asList(1,2);
List<Number> nums = ints;  // compile-time error
nums.add(3.14);
assert ints.toString().equals("[1, 2, 3.14]");  // uh oh!
```

Ở đây có ý nghĩa `List<Integer>` không là kiểu con của `List<Number>`

Tuy nhiên nếu nó là mảng thì khác Integer[] là con của Number[]

## 2.2. Wildcards (ký tự đại diện) với extends

```java
interface Collection<E> {

  public boolean addAll(Collection<? extends E> c);

}
```

`? extends E` có thể hiểu là kiểu con của E. Dấu ? được gọi là kí tự đại diện. vì nó là viết tắt cho một số kiểu con của E.

## 2.3. Wildcards(ký tự đại diện) với super

ví dụ sau:

```java
public static <T> void copy(List<? super T> dst, List<? extends T> src) {
  for (int i = 0; i < src.size(); i++) {
    dst.set(i, src.get(i));
  }
}
```

Cụm từ kỳ quặc`? super T` có nghĩa là danh sách đích có thể có các phần tử thuộc bất kỳ kiểu nào là siêu kiểu của T, cũng như danh sách nguồn có thể có các phần tử thuộc bất kỳ kiểu nào là kiểu con của T.

Ví dụ cho áp dụng hàm trên:

```java
List<Object> objs = Arrays.<Object>asList(2, 3.14, "four");
List<Integer> ints = Arrays.asList(5, 6);
Collections.copy(objs, ints);
assert objs.toString().equals("[5, 6, four]");
```

## 2.4. Nguyên tắc Get và Put

Có thể là một phương pháp hay để chèn các ký tự đại diện bất cứ khi nào có thể, nhưng làm cách nào để bạn quyết định sử dụng ký tự đại diện nào? Nơi nào bạn nên sử dụng extension, nơi nào bạn nên sử dụng super, và nơi nào là không thích hợp khi sử dụng ký tự đại diện?

có một nguyên tắc chỉ rõ cho bạn các thắc mắc trên.

Nguyên tắc Get và Put: sử dụng ký tự đại diện mở rộng khi bạn chỉ Get giá trị từ một cấu trúc, sử dụng ký tự đại diện siêu khi bạn chỉ đặt các giá trị vào một cấu trúc và không sử dụng ký tự đại diện khi bạn Get và Put.

ví dụ như trong phương thức copy trên nhìn chữ kí của nó:

```java
public static <T> void copy(List<? super T> dest, List<? extends T> src)
```

Rõ ràng src chỉ lấy ra trong khi dest vừa get và vừa put.

## 2.5. Arrays

Trong Java, kiểu mảng là `hiệp biến` tức là kiểu S[] được coi là kiểu con của T[] bất cứ khi nào S là kiểu con của T.

```java
Integer[] ints = new Integer[] {1,2,3};
Number[] nums = ints;
nums[2] = 3.14;  // array store 
```

Có vấn đề ở đây. khi ints được khỏi tạo thì nó sẽ đi kèm với kiểu sửa dổi nó. Nên ở đây num muốn thay đổi fias trị thì bắt buộc phải sử dụng kiểu int.

Ngược lại, quan hệ kiểu con đối với generic là bất biến, có nghĩa là kiểu `List<S>` không được coi là kiểu con của `List<T>`, ngoại trừ trường hợp khi S và T giống hệt nhau.

## 2.6. Các tham số kiểu ký tự đại diện

Phương thức `contains` sẽ kiểm tra xem một tập hợp có chứa một đối tượng nhất định hay không và tổng quát hóa của nó, `containsAll`, kiểm tra xem một tập hợp có chứa mọi phần tử của một tập hợp khác hay không.

```java
interface Collection<E> {
  ...
  public boolean contains(Object o);
  public boolean containsAll(Collection<?> c);
  ...
}
```

phương thức `contains` rõ ràng không sử dụng generic. phương thức `containsAll` kí tự `?`  là viết tắt cho `Collection<? extends Object>`

```java
Object obj = "one";
List<Object> objs = Arrays.<Object>asList("one", 2, 3.14, 4);
List<Integer> ints = Arrays.asList(2, 4);
assert objs.contains(obj);
assert objs.containsAll(ints);
assert !ints.contains(obj);
assert !ints.containsAll(objs);
```

Bạn có thể chọn một cách hợp lý một thiết kế thay thế cho một thiết kế bộ sưu tập trong đó bạn chỉ có thể kiểm tra khả năng ngăn chặn cho các kiểu con của kiểu phần tử:

```java
interface MyCollection<E> {  // alternative design
  
  public boolean contains(E o);
  public boolean containsAll(Collection<? extends E> c);
 
}
```

Giả sử chúng ta có một MyList lớp triển khai MyCollection. và chúng ta có một bài test để kiêm tra nó:

```java
Object obj = "one";
MyList<Object> objs = MyList.<Object>asList("one", 2, 3.14, 4);
MyList<Integer> ints = MyList.asList(2, 4);
assert objs.contains(obj);
assert objs.containsAll(ints)
assert !ints.contains(obj); // compile-time error
assert !ints.containsAll(objs); // compile-time error
```

## 2.7. Chụp ký tự đại diện

Khi một phương thức tổng quát được gọi, kiểu tham số có thể được chọn để khớp với kiểu không xác định được biểu thị bằng ký tự đại diện. cái này gọi là `Chụp ký tự đại diện` hay `Wildcard Capture`

Hãy xem xét phương thức `reverse` trong lớp tiện ích `java.util.Collections`, lớp này chấp nhận một danh sách thuộc bất kỳ kiểu nào và đảo ngược nó. Nó có thể được đưa ra một trong hai chữ ký sau, tương đương nhau:

```java
public static void reverse(List<?> list);
public static void <T> reverse(List<T> list);
```

Chữ ký ký tự đại diện ngắn hơn và rõ ràng hơn một chút và là chữ ký được sử dụng trong thư viện.

Nếu bạn sử dụng chữ ký thứ hai, có thể dễ dàng triển khai phương thức:

```java
public static void <T> reverse(List<T> list) {
  List<T> tmp = new ArrayList<T>(list);
  for (int i = 0; i < list.size(); i++) {
    list.set(i, tmp.get(list.size()-i-1));
  }
}
```

nó cũng dễ hiểu thôi, copy ra một cái list tạm thời và thực hiện gán lại cho list ban đầu với thứ tự ngược lại.

nhưng nếu lấy thân của method này cho chứ kĩ đầu tiên tôi thấy nó sẽ không hoạt động

```java
public static void reverse(List<?> list) {
  List<Object> tmp = new ArrayList<Object>(list);
  for (int i = 0; i < list.size(); i++) {
    list.set(i, tmp.get(list.size()-i-1));  // compile-time error
  }
}
```

Thay vào đó, bạn có thể triển khai phương thức với chữ ký đầu tiên bằng cách triển khai phương thức riêng với chữ ký thứ hai và gọi chữ ký thứ hai từ chữ ký đầu tiên:

```java
public static void reverse(List<?> list) { 
  rev(list); 
}

private static <T> void rev(List<T> list) {
  List<T> tmp = new ArrayList<T>(list);
  for (int i = 0; i < list.size(); i++) {
    list.set(i, tmp.get(list.size()-i-1));
  }
}
```

Ở đây chúng ta nói rằng biến kiểu T đã `Chụp ký tự đại diện`.

Nó là thứ bạn nên biết.

## 2.8. Hạn chế đối với Ký tự đại diện

Các ký tự đại diện có thể không xuất hiện ở cấp cao nhất trong các biểu thức tạo cá thể lớp (new), trong các tham số kiểu rõ ràng trong các lệnh gọi phương thức chung hoặc trong các siêu kiểu (extends và implements).

khi tạo một **Instance** thì không có tham số kiểu nào có thể là ký tự đại diện:

```java
List<?> list = new ArrayList<?>();  // compile-time error
Map<String, ? extends Number> map
  = new HashMap<String, ? extends Number>();  // compile-time error
```

Có thể nói như thê này, đoạn code trên đã vi phạm nguyên tắc get and put. nên nó sẽ bị lỗi khi compile-time. (compile-time là lỗi khi chạy mới phát hiện nên bạn hãy cẩn thận với nó vì bạn sẽ mất khá nhiều công để sửa đó)

```java
List<Number> nums = new ArrayList<Number>();
List<? super Number> sink = nums;
List<? extends Number> source = nums;
for (int i=0; i<10; i++) sink.add(i);
double sum=0; for (Number num : source) sum+=num.doubleValue();
```

Ở đây các ký tự đại diện xuất hiện ở dòng thứ hai và thứ ba, nhưng không xuất hiện ở dòng đầu tiên tạo danh sách.

Bạn cũng đã thấy rồi đó nó tuan thủ đúng theo nguyên tắc get and put.

Chỉ các tham số cấp cao nhất trong quá trình tạo phiên bản mới bị cấm chứa các ký tự đại diện. Các ký tự đại diện lồng nhau được cho phép. Do đó, những điều sau đây là hợp pháp:

```java
List<List<?>> lists = new ArrayList<List<?>>();
lists.add(Arrays.asList(1,2,3));
lists.add(Arrays.asList("four","five"));
assert lists.toString().equals("[[1, 2, 3], [four, five]]");
```

Một cách để nhớ hạn chế là mối quan hệ giữa các ký tự đại diện và các kiểu thông thường tương tự như mối quan hệ giữa các giao diện và các thẻ lớp và các giao diện là chung chung hơn, các kiểu và lớp thông thường cụ thể hơn và việc tạo cá thể yêu cầu thông tin cụ thể hơn. Hãy xem xét ba lệnh sau:

```java
List<?> list = new ArrayList<Object>();   // ok
List<?> list = new List<Object>()  // compile-time error
List<?> list = new ArrayList<?>()  // compile-time error
```

Các nhà thiết kế Java đã lưu ý rằng mọi kiểu ký tự đại diện đều là viết tắt của một số kiểu thông thường, vì vậy họ tin rằng cuối cùng mọi đối tượng nên được tạo bằng một kiểu thông thường.

Bạn đừng cố tìm thủ thuật đẻ lack qua được vấn đề này, vì nó đéo có cửa đâu :V

hết chương. :D