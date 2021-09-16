# THE INTERFACE SERGREGATION PRINCIPLE (ISP)

Nguyên lý được phát biểu như sau:\
**Không nên buộc các thực thể phần mềm phụ thuộc vào những interface mà chúng không sử dụng đến.**

Khi xây dựng một lớp đối tượng, đặc biệt là những lớp trừu tượng (abstract class), nhiều người thường có xu hướng để cho lớp đối tượng thực hiện càng nghiều chức năng càng tốt, đưa thật nhiều thuộc tính và phương thức vào lớp đối tượng đó. Những lớp đối tượng như vậy được gọi là những lớp đối tượng có interface bị “ô nhiễm” (polluted interface).
Khi một lớp đối tượng có interface bị “ô nhiễm”, nó sẽ trở nên cồng kềnh. Một thực thể phần mềm nào đó chỉ cần thực hiện một công việc đơn giản mà lớp đối tượng này hỗ trợ buộc phải làm việc với toàn bộ interface của lớp đối tượng đó. Đặc biệt đối với lớp trừu tượng có interface bị “ô nhiễm”, một số lớp kế thừa chỉ quan tâm đến một phần interface của nó nhưng bị buộc phải thực hiện việc cài đặt cho cả phần interface không hề có ý nghĩa đối với chúng. Điều dẫn đến sự dư thừa không cần thiết trong các thực thể phần mềm. Quan trọng hơn nữa, việc buộc các lớp kế thừa phụ thuộc vào phần interface mà chúng không sử dụng đến sẽ làm tăng sự kết dính (coupling) giữa các thực thể phần mềm. Một khi sự nâng cấp, mở rộng diễn ra, đòi hỏi phần interface đó thay đổi, các lớp kế thừa này bị buộc phải chỉnh sửa. Điều này làm cho chúng vi phạm nguyên lý Open-Close.

Dưới đây minh họa cho một thiết kế sai khi không sử dụng nguyên lý phân tách interface. Các lớp kế thừa interface phải thực thi các phương thức không cần thiết của interface đó.

```java
public interface Animal {
    void fly();
    void run();
    void bark();
}
public class Bird implements Animal {
    public void bark() { /* do nothing */ }
    public void run() {
        // write code about running of the bird
    }
    public void fly() {
        // write code about flying of the bird
    }
}
public class Cat implements Animal {
    public void fly() { throw new Exception("Undefined cat property"); }
    public void bark() { throw new Exception("Undefined cat property"); }
    public void run() {
        // write code about running of the cat
    }
}
public class Dog implements Animal {
    public void fly() { }
    public void bark() {
        // write code about barking of the dog
    }
    public void run() {
        // write code about running of the dog
    }
}
```

Hãy tưởng tượng khi interface Animal ngày càng mở rộng, thì các lớp kế thừa phải tiếp tục implement các phương thức không cần thiết. Đã đến lúc nghĩ đến việc phân tách interface thành các interface nhỏ hơn. Mỗi interface chỉ nên làm những nhiệm vụ có mối liên hệ chặt chẽ với nhau.

```java
public interface Flyable {
    void fly();
}
public interface Runnable {
    void run();
}
public interface Barkable {
    void bark();
}
public class Bird implements Flyable, Runnable {
    public void run() {
        // write code about running of the bird
    }
    public void fly() {
        // write code about flying of the bird
    }
}
public class Cat implements Runnable{
    public void run() {
        // write code about running of the cat
    }
}
public class Dog implements Runnable, Barkable {
    public void bark() {
        // write code about barking of the dog
    }
    public void run() {
        // write code about running of the dog
    }
}
```

Như vậy, việc mở rộng các interface sẽ không còn là vấn đề.

Bàn thêm về nguyên lí này

Nguyên lý Phân tách interface có mối liên hệ (nhưng không mật thiết lắm) với nguyên lý Open-Close. Sự vi phạm nguyên lý Phân tách interface có khả năng dẫn đến sự vi phạm nguyên lý Open-Close (xem phân tích ở trên).

Interface bị "ô nhiễm" của lớp đối tượng nên được phân tách ngay khi có thể để tránh khả năng dẫn đến sự vi phạm nguyên lý Open-Close. Việc phân tách interface có thể được thể hiện thông qua việc truyền từng tham số riêng, cụ thể vào một hàm hơn là truyền một tham số chung, tổng quát trong khi hàm đó chỉ sử dụng một phần công việc được hỗ trợ bởi tham số chung, tổng quát này. Nó cũng có thể được thể hiện thông qua việc tăng thêm mức độ trừu tượng trong cây kế thừa. Interface chung của một bộ phận lớp kế thừa được tổng hợp lại trong một lớp cơ sở. Và lớp cơ sở này lại kế thừa từ lớp cơ sở ban đầu. Như vậy những lớp kế thừa khác không bị phụ thuộc vào phần interface mà chúng không sử dụng đến của những lớp kế thừa kia.
Trong trường hợp sau khi phân tách interface, một số lớp kế thừa muốn sử dụng những phần interface đã phân tách, chúng có thể thực hiện việc đa kế thừa từ những lớp hỗ trợ những phần interface này hoặc thực hiện "composition" những đối tượng thuộc những các lớp đó.
