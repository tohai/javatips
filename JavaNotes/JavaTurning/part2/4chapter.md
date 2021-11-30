# 4.1 Tránh chi phí phân loại không cần thiết

JDK cung cấp các phương thức sắp xếp trong java.util.Arrays (đối với mảng đối tượng) và trong java.util.Collections (đối với đối tượng implement Collection interfaces).Những loại này thường thích hợp cho tất cả trừ các ứng dụng chuyên biệt nhất. Để tối ưu hóa một loại, thông thường bạn có thể có đủ cải tiến bằng cách thực hiện lại một loại tiêu chuẩn (chẳng hạn như quicksort) như một phương thức trong lớp đang được sắp xếp. So sánh các phần tử sau đó có thể được thực hiện trực tiếp mà không cần gọi các phương pháp so sánh chung chung. Chỉ những ứng dụng chuyên biệt nhất mới cần tìm kiếm các thuật toán sắp xếp chuyên biệt.

Xem ví dụ dưới đây :

``` java
public class Sortable implements Comparable
{
  int order;
  public Sortable(int i){order = i;}
  public int compareTo(Object o){return order - ((Sortable) o).order;}
  public int compareToSortable(Sortable o){return order - o.order;}
}

```
