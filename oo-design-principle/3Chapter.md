# LISKOV SUBSTITUTION PRINCIPLE

**Nguyên lý này nói rằng các lớp dẫn xuất phải có thể được thay thế bởi lớp cha.**

Nguyên lý Thay thế Liskov phát biểu như sau:\
**"Lớp D được gọi là kế thừa từ lớp B khi và chỉ khi với mọi hàm F thao tác trên các đối tượng của B, cách cư xử (behavior) của F không đổi khi thay thế các đối tượng của B bằng các đối tượng của D".**

Để hiểu được nguyên lý trên, ta cần xem xét lại khái niệm kế thừa trong lập trình hướng đối tượng. Kế thừa (inheritance) là một trong 3 tính chất cơ bản nhất của lập trình hướng đối tượng (bên cạnh tính đóng gói (encapsulation) và tính đa hình (polymorphism)). Đó là tính chất một lớp có khả năng “tái sử dụng” các thuộc tính và phương thức của lớp khác và quan trọng hơn là nó có thể cư xử (behavior) như lớp đó. Quan hệ “IS A” thường được dùng để xác định kế thừa. A kế thừa B khi A quan hệ “IS A” với B. Có nghĩa A là trường hợp đặc biệt của B

Nghe thì có vẻ phức tạp, lằng nhằng, nhưng khi nhìn vào thực tế thì nguyên lý này khá đơn giản. Hãy xem xét 1 ví dụ đơn giản sau đây.
Chúng ta biết “hình vuông là hình chữ nhật”. Nhưng hình chữ nhật có thay thế được hình vuông? Hãy xét mối quan hệ kế thừa đơn giản trong hướng đối tượng giữa hình chữ nhật và hình vuông.

```java
class Rectangle
{
    protected int m_width;
    protected int m_height;
 
    public void setWidth(int width){
        m_width = width;
    }
 
    public void setHeight(int height){
        m_height = height;
    }
 
    public int getWidth(){
        return m_width;
    }
 
    public int getHeight(){
        return m_height;
    }
 
    public int getArea(){
        return m_width * m_height;
    }
}
 
class Square extends Rectangle
{
    public void setWidth(int width){
        m_width = width;
        m_height = width;
    }
 
    public void setHeight(int height){
        m_width = height;
        m_height = height;
    }
 
}
 
class LspTest
{
    private static Rectangle getNewRectangle()
    {
        // it can be an object returned by some factory ...
        return new Square();
    }
 
    public static void main (String args[])
    {
        Rectangle r = LspTest.getNewRectangle();
 
        r.setWidth(5);
        r.setHeight(10);
        // user knows that r it's a rectangle.
        // It assumes that he's able to set the width and height as for the base class
 
        System.out.println(r.getArea());
        // now he's surprised to see that the area is 100 instead of 50.
    }
}
```

Ở ví dụ trên, chúng ta đã vi phạm nguyên lý thay thế Liskov bởi vì behavior của Width và Heigh đã bị thay đổi ở lớp Square. Vì vậy, khi thay thế Square bằng Rectangle sẽ dẫn đến kết quả không đúng. (Rectangle phải ra 5×10=50 chứ không phải 10×10=100).

Chúng ta phải bảo đảm rằng, khi một lớp con kế thừa từ một lớp khác, nó sẽ không làm thay đổi hành vi của lớp đó. Hay trong ví dụ này là lớp Square không được phép thay đổi hành vi của lớp Rectangle (Width và Height)

Bây giờ hãy tạo ra một thiết kế mới bằng cách áp dụng nguyên lý thay thế Liskov. Chúng ta có 1 lớp Shape sẽ là lớp cơ sở cho cả 2 lớp Rectangle và Square.

```java
public abstract class Shape {
 
    /** mHeight */
    protected int mHeight;
 
    /** mWidth */
    protected int mWidth;

    public abstract int getWidth();

    public abstract void setWidth(int inWidth);
 
    public abstract int getHeight();

    public abstract void setHeight(int inHeight);

    public int getArea() {
        return mHeight * mWidth;
    }
}

public class Rectangle extends Shape
{
    @Override
    public int getWidth() {
        return mWidth;
    }
 
    @Override
    public int getHeight() {
        return mHeight;
    }
 
    @Override
    public void setWidth(int inWidth) {
        mWidth = inWidth;
    }
 
    @Override
    public void setHeight(int inHeight) {
        mHeight = inHeight;
    }
 
}

public class Square extends Shape {
 
    @Override
    public int getWidth() {
        return mWidth;
    }
 
    @Override
    public void setWidth(int inWidth) {
        SetWidthAndHeight(inWidth);
    }
 
    @Override
    public int getHeight() {
        return mHeight;
    }
 
    @Override
    public void setHeight(int inHeight) {
        SetWidthAndHeight(inHeight);
    }
 
    private void setWidthAndHeight(int inValue) {
        mHeight = inValue;
        mWidth = inValue;
    }
 
}

public class Main {
 
    static final String SQUARE = "Square";
    static final String RECTANGLE = "Rectangle";
 
    /**
     * Main method
     */
    public static void main(String[] args) {
        Shape shape1 = getShape(SQUARE);
        shape1.setHeight(10);
        shape1.setWidth(20);
        System.out.println(SQUARE + "'s area: " + shape1.getArea());
 
        Shape shape2 = getShape(RECTANGLE);
        shape2.setHeight(10);
        shape2.setWidth(20);
        System.out.println(RECTANGLE + "'s area: " + shape2.getArea());
    }
 
    /**
     * Simple factory method
     */
    static Shape getShape(String inShapeType) {
        if(inShapeType.equals(SQUARE)) {
            return new Square();
        }
        if(inShapeType.equals(RECTANGLE)) {
            return new Rectangle();
        }
        return null;
    }
 
}

```

****

**Hãy chắc rằng bạn chỉ nên override lại các thuộc tính hay hành vi trừu tượng ở lớp cha vả đảm bảo việc override đó không ảnh hưởng đến các hành vi và phương thức còn lại của lớp cha như ví dụ trên.**
