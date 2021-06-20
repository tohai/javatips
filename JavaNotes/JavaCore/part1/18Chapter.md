# Chương 18 Reflection

Reflection là một trong những khía cạnh ít được hiểu nhất của Java, nhưng cũng là một trong những khía cạnh mạnh mẽ nhất. Phản ánh được sử dụng trong hầu hết mọi sản phẩm sản xuất lớn trên thị trường, nhưng nó thường phải được học hỏi từ các nhà phát triển đã là chuyên gia, và các nhà phát triển này thường rất ít và khá xa.

Lý do sử dụng Reflection là vì những khó khăn trong việc xây dựng các dự án phức tạp trong thời gian ngắn. Khi các dự án của bạn trở nên nâng cao hơn, bạn có thể thấy rằng bạn có ít thời gian hơn để hoàn thành chúng.

Ví dụ, hãy xem xét mô hình dữ liệu Ngân hàng Trực tuyến từ Chương 8. Nó tương đối nhỏ và dễ hiểu. Khi thiết kế GUI để thao tác với mô hình dữ liệu này, bạn có thể muốn kích hoạt một công cụ xây dựng GUI và bắt đầu tạo biểu mẫu cho từng đối tượng. Cuối cùng, bạn sẽ có càng nhiều biểu mẫu cũng như có các đối tượng mô hình dữ liệu. Trên thực tế, đây là cách hầu hết các GUI được xây dựng ngày nay.

Tuy nhiên, hãy xem xét nếu bạn đã sử dụng kỹ thuật tương tự trên mô hình dữ liệu với hơn 200 đối tượng mô hình dữ liệu và hàng nghìn mối quan hệ giữa các mục. Trong tình huống này, ý tưởng sử dụng công cụ xây dựng GUI kém hấp dẫn hơn nhiều — bạn sẽ cần ngân sách hàng tháng giờ làm việc để hoàn thành dự án. Hơn nữa, bất cứ khi nào bất kỳ đối tượng nào trong số này thay đổi, dù chỉ là một chút, bạn sẽ cần phải quay lại và sửa tất cả các bảng của đối tượng bị ảnh hưởng. Điều khó khăn trên chiếc bánh đặc biệt đắng này là bạn sẽ phải tạo các bảng mới mỗi khi một đối tượng mới được đưa vào hệ thống. Rõ ràng, đây không phải là một phương pháp hiệu quả để cung cấp phần mềm một cách kịp thời.

Vì sự phức tạp này, cơn thịnh nộ hiện nay trong ngành lập trình Java là biến mọi thứ thành một ứng dụng web. Trên thực tế, nhiều ứng dụng lẽ ra dựa trên GUI đã được chuyển thành ứng dụng web vì lý do này. Tuy nhiên, ngay cả khi bạn quyết định đi theo con đường ứng dụng web, vấn đề xây dựng GUI hiệu quả vẫn còn. Bây giờ, thay vì phải xây dựng các bảng GUI, bạn phải xây dựng các trang JSP cho từng đối tượng mô hình dữ liệu của mình. Một lần nữa, 200 đối tượng mô hình dữ liệu đòi hỏi nhiều thời gian hơn để phát triển mà bạn có thể dành. Rõ ràng, bạn cần một giải pháp cho vấn đề này và sự phản ánh có thể cung cấp giải pháp đó.

Reflection cho phép bạn xây dựng các công cụ hơn là các bảng điều khiển. Trên thực tế, bạn có thể sử dụng Reflection để xây dựng các bảng tự xây dựng một cách nhanh chóng theo cấu trúc của đối tượng mà chúng đang hiển thị. Thay vì thiết kế 200 tấm, bạn có thể thiết kế một công cụ xây dựng các tấm và hoạt động cho bất kỳ đối tượng nào. Bất chấp sự phức tạp của mã được giới thiệu bằng cách sử dụng Reflection, việc tạo hệ thống với các công cụ dựa trên Reflection nhanh hơn đáng kể và bảo trì rẻ hơn nhiều.

## 18.1 Nền tảng cơ bản

Giả sử bạn đang đi bộ trên phố và một người lạ đến gần và đưa cho bạn một đồ vật. Bạn sẽ làm gì? Đương nhiên, bạn sẽ nhìn vào bất cứ thứ gì được giao cho bạn. Bạn kiểm tra vật phẩm, và ngay lập tức xác định rằng vật thể đó là một tờ giấy. Ngay lập tức, bạn biết nhiều điều về đối tượng này: người ta có thể viết trên đó; nó có thể có các thông điệp như quảng cáo, cảnh báo, tin tức và thông tin hữu ích khác được in trên đó; và bạn thậm chí có thể nghiền nó thành một quả bóng và chơi bóng rổ khi sếp của bạn không nhìn.

Mặc dù đây là một ví dụ ngớ ngẩn, nhưng nó minh họa ý tưởng cơ bản về reflection, cho phép bạn tiến hành kiểm tra tương tự các đối tượng và lớp Java. Sử dụng kết hợp giữa suy tư và xem xét nội tâm, bạn có thể xác định bản chất và chức năng có thể có của một đối tượng mà bạn chưa biết về nó tại thời điểm biên dịch. Hơn nữa, bạn có thể sử dụng thông tin này để thực thi các phương thức hoặc đặt các giá trị trường trên đối tượng.

Để xem phản xạ hoạt động như thế nào, hãy bắt đầu với một lớp từ mô hình dữ liệu online bank của bạn. Trong Ví dụ dưới, lớp Person từ mô hình dữ liệu được hiển thị mà không có bất kỳ commnents hoặc nội dung nào từ các phương thức của nó.

```java
public class Person extends MutableObject {

    public static final ObjectConstraint GENDER_CONSTRAINT = new ObjectConstraint("gender", false, Gender.class);

    public static final StringConstraint FIRST_NAME_CONSTRAINT = new StringConstraint("firstName", false, 20);

    public static final StringConstraint LAST_NAME_CONSTRAINT = new StringConstraint("lastName", false, 40);

    public static final DateConstraint BIRTH_DATE_CONSTRAINT = new DateConstraint("birthDate", false, "01/01/1900",
            "12/31/3000", Locale.US);

    public static final StringConstraint TAX_ID_CONSTRAINT = new StringConstraint("taxID", false, 40);

    private Date birthDate = Calendar.getInstance().getTime();

    private Gender gender = Gender.MALE;

    /** The first name of the person. */

    private String firstName = "<<NEW PERSON>>";

    private String lastName = "<<NEW PERSON>>";

    private String taxID = new String();

    public void setBirthDate( final Date birthDate ) {
    }

    public Date getBirthDate() {
    }

    public void setFirstName( final String firstName ) {
    }

    public String getFirstName() {
    }

    public void setGender( final Gender gender ) {
    }

    public Gender getGender() {
    }

    public void setLastName( final String lastName ) {
    }

    public String getLastName() {
    }

    public void setTaxID( final String taxID ) {
    }

    public String getTaxID() {
    }

    public boolean equals( final Object obj ) {
    }

    public int hashCode() {
    }

}
```

Tất cả các lớp mô hình dữ liệu Online Only Bank của bạn gần giống như thế này. Tất cả chúng đều là JavaBeans — nghĩa là chúng tuân theo tiêu chuẩn đặt tên được đặt cho JavaBeans. Vì tất cả các lớp được xây dựng trong Java cuối cùng đều kế thừa từ java.lang.Object, bạn có thể chuyển một thể hiện của lớp này như một đối tượng cho một phương thức và sau đó tìm ra các thuộc tính, phương thức, giao diện bên trong, v.v., lớp chứa, như Ví dụ dưới đây sẽ chứng minh.

```java
public class MethodInfoDemo {

    public static void printMethodInfo( final Object obj ) {
        Class type = obj.getClass();
        final Method[] methods = type.getMethods();
        for( int idx = 0; idx < methods.length; idx++ ) {
            System.out.println(methods[idx]);
        }
    }

    public static void main( final String[] args ) {

        Person p = new Person();
        p.setFirstName("Robert");
        p.setLastName("Simmons");
        p.setGender(Gender.MALE);
        p.setTaxID("123abc456");

        printMethodInfo(p);

    }

}
```

Bên trong printMethodInfo (), kiểm tra đối tượng được chuyển cho bạn. Sau khi bạn xác định lớp của đối tượng, hãy kiểm tra nội dung của lớp và in phiên bản chuỗi của các phương thức:

>ant -Dexample=oreilly.hcj.reflection.MethodInfoDemo run_example


run_example:

>[java] public int oreilly.hcj.bankdata.Person.hashCode( )\
>[java] public boolean oreilly.hcj.bankdata.Person.equals(java.lang.Object)\
>[java] public void oreilly.hcj.bankdata.Person.setFirstName(java.lang.String)\
>[java] public void oreilly.hcj.bankdata.Person.setLastName(java.lang.String)\
>[java] public void oreilly.hcj.bankdata.Person.setGender(oreilly.hcj.bankdata.Gender)\
>[java] public void oreilly.hcj.bankdata.Person.setTaxID(java.lang.String)\
>[java] public void oreilly.hcj.bankdata.Person.setBirthDate(java.util.Date)\
>[java] public java.util.Date oreilly.hcj.bankdata.Person.getBirthDate( )\
>[java] public java.lang.String oreilly.hcj.bankdata.Person.getFirstName( )\
>[java] public oreilly.hcj.bankdata.Gender             oreilly.hcj.bankdata.Person.getGender( )\
>[java] public java.lang.String oreilly.hcj.bankdata.Person.getLastName( )\
>[java] public java.lang.String oreilly.hcj.bankdata.Person.getTaxID( )\
>[java] public java.lang.String oreilly.hcj.datamodeling.MutableObject.toString()\
>[java] public void oreilly.hcj.datamodeling.MutableObject.addPropertyChangeListener(java.lang.String,java.beans.PropertyChangeListener)\
>[java] public void oreilly.hcj.datamodeling.MutableObject.addPropertyChangeListener(java.beans.PropertyChangeListener)\
>[java] public void oreilly.hcj.datamodeling.MutableObject.removePropertyChangeListener(java.lang.String,java.beans.PropertyChangeListener)\
>[java] public void oreilly.hcj.datamodeling.MutableObject.removePropertyChangeListener(java.beans.PropertyChangeListener)\
>[java] public final native java.lang.Class java.lang.Object.getClass( )\
>[java] public final void java.lang.Object.wait(long,int) throws java.lang.InterruptedException\
>[java] public final void java.lang.Object.wait( ) throws java.lang.InterruptedException\
>[java] public final native void java.lang.Object.wait(long) throws  java.lang.InterruptedException\
>[java] public final native void java.lang.Object.notify( )\
>[java] public final native void java.lang.Object.notifyAll( )\
```

Lời gọi getMethods() trong printMethodInfo() trả về một mảng các đối tượng Phương thức từ gói java.lang.reflect; phiên bản chuỗi của các đối tượng Method này sau đó đã được ghi vào bảng điều khiển. Lời gọi getMethods() trích xuất từng thành viên công khai của lớp Person, bao gồm các thành viên mà nó được thừa kế từ các lớp khác như MutableObject. Các dòng được nhấn mạnh chứa các bộ định tuyến và bộ thu thập cho lớp `Person`. Điều thú vị về printMethodInfo() là nó sẽ hoạt động với bất kỳ đối tượng nào bạn truyền vào nó.

Ngoài việc in ra các đối tượng Method, bạn có thể sử dụng chúng để thực thi các phương thức. Điều này được chứng minh trong Ví dụ dưới với một phương thức sẽ đặt tất cả các thuộc tính chuỗi trong một lớp dưới dạng chuỗi rỗng.

```java
public class MethodInfoDemo {
    public static void emptyStrings( final Object obj ) throws IllegalAccessException, InvocationTargetException {

        final String PREFIX = "set";
        Method[] methods = obj.getClass().getMethods();

        for( int idx = 0; idx < methods.length; idx++ ) {
            if (methods[idx].getName().startsWith(PREFIX)) {
                if (methods[idx].getParameterTypes()[0] == String.class) {
                    methods[idx].invoke(obj, new Object[] { new String() });
                }
            }
        }
    }

}
```

Phương thức này tìm kiếm tất cả các phương thức trong một lớp bắt đầu bằng từ "set" và lấy một Chuỗi duy nhất làm tham số. Sau đó, nó gọi các phương thức này bằng cách truyền một mảng các đối tượng chỉ chứa một chuỗi trống duy nhất cho phương thức.

Điều thú vị về hai phương thức cuối cùng trong ví dụ là chúng sẽ hoạt động với bất kỳ đối tượng nào được truyền cho chúng, không chỉ đối tượng Person. Nếu bạn có mã phức tạp trong lớp trình diễn này, bạn có thể áp dụng nó cho nhiều đối tượng, giống như một người thợ mộc dùng cưa cho nhiều loại gỗ.

## 18.2 Reflection và Greater Reflection

Các công cụ có sẵn cho một lập trình viên reflection không giới hạn trong gói java.lang.reflect, như bạn có thể mong đợi. Trên thực tế, chúng nằm rải rác trên toàn bộ lõi của JDK. Tất cả các công cụ được sử dụng trong reflection thường được gọi là Greater Reflection. Trong phần này, bạn sẽ giải quyết từng gói có liên quan và các công cụ trong các gói đó và tập hợp một kho công cụ phản ánh sẽ cải thiện chương trình của bạn.

### 18.2.1 Package java.lang

Gói này chứa chức năng cốt lõi của reflection cũng như chính JDK. Vì tất cả các lớp đều kế thừa từ java.lang.Object, gói **`java.lang đóng một vai trò quan trọng trong việc lấy thông tin về các lớp và đối tượng`**.

#### 18.2.1.1 Class java.lang.Class

Lớp này chứa thông tin về lớp của đối tượng, chẳng hạn như các trường và phương thức mà nó chứa cũng như gói nào trong đó. Bạn đã sử dụng nó trước đây để lấy thông tin về các phương thức trong một lớp. (ví dụ 2 trong mục 18.1)

#### 18.2.1.2 Class java.lang.Object

Từ lớp này, bạn có thể nhận được cá thể Lớp cho bất kỳ đối tượng nào. Ngoài ra, vì lớp Đối tượng là cơ sở cho tất cả các kiểu được xây dựng trong Java, nó được sử dụng để truyền các tham số hoặc nhận kết quả từ các phương thức và trường được gọi theo phản xạ.

#### 18.2.1.3 Class java.lang.Package

Lớp này đóng gói biểu hiện thời gian chạy của một gói. Tuy nhiên, Package không phải là một lớp đặc biệt hữu ích vì bạn không thể yêu cầu nó cho tất cả các lớp trong một gói. Tôi hiếm khi sử dụng lớp này trong lập trình reflection của mình.

### 18.2.2 Package java.lang.reflect

Gói này chứa các tiện ích để gọi động các phương thức và gán và truy xuất động các trường. Khi bạn nhận được danh mục các trường và phương thức từ Lớp, chúng sẽ được trả lại cho bạn dưới dạng các thể hiện của các lớp trong java.lang.reflect. Gói này là một trong những công cụ quan trọng trong việc xây dựng mã động và nó thường được sử dụng trong mã reflection.

### 18.2.2.1 Class java.lang.reflect.Field

Lớp này chứa mô tả về một trường và các phương tiện để truy cập vào trường. Với lớp này, bạn có thể đặt hoặc truy xuất động các giá trị trường.

#### 18.2.2.2 Class java.lang.reflect.Method

Lớp này chứa một mô tả của một phương thức và phương tiện để gọi nó một cách động. Bạn có thể sử dụng các phiên bản của lớp này để gọi động các phương thức.

#### 18.2.2.3 Class java.lang.reflect.Modifier

Lớp này được sử dụng để giải mã các sửa đổi trường bit của một lớp, trường hoặc phương thức. Sử dụng `Modifier`, bạn có thể xác định xem một trường là public, private, static, transient, v.v. Các sửa đổi được đặt bởi mã reflection gốc trong máy ảo kiểm tra mã byte Java thực tế. Các bổ ngữ này ở chế độ chỉ đọc.

#### 18.2.2.4 Class java.lang.reflect.Proxy

Lớp này được sử dụng để tạo các lớp proxy động, sau đó được sử dụng để tạo các đối tượng triển khai các giao diện cụ thể. Thông thường, các lớp này được sử dụng để ẩn các phần triển khai của một lớp khỏi người dùng. Ví dụ, bạn có thể tạo một Proxy cho đối tượng dữ liệu để hiển thị một giao diện bất biến cho đối tượng.

#### 18.2.2.5 Class java.lang.reflect.AccessibleObject

Đây là lớp cơ sở mà Method, Field,  và các bộ mô tả kế thừa khác. Nó cho phép bạn bỏ qua các ràng buộc truy cập lớp như "private", cho phép bạn thực thi các phương thức và đặt các trường mà thông thường sẽ bị chặn bởi các bộ chỉ định khả năng hiển thị. Tuần tự hóa thường sử dụng lớp này vì nó phải có quyền truy cập vào các trường private để tuần tự hóa một đối tượng. Trong EJB, lớp này là không giới hạn.

Mặc dù lớp này có thể hữu ích, nhưng tôi thận trọng không sử dụng nó trừ khi bạn không có lựa chọn nào khác. Buộc phá vỡ bao bọc trên một đối tượng có thể dẫn đến mã cực kỳ khó hiểu và rất khó gỡ lỗi.

### 18.2.3 Package java.beans

Gói java.beans được thiết kế để cho phép các nhà phát triển xây dựng GUI bằng các công cụ trực quan. Tuy nhiên, có nhiều lớp bên trong gói này có thể được sử dụng với reflection. Gói này có các lớp cung cấp khả năng truy cập thuận tiện vào các phần phản ánh khác nhau mà nếu không bạn sẽ phải tìm kiếm ở một số nơi.

Mặc dù gói này chứa một số công cụ mạnh mẽ, nhưng nó đã khá cũ. Hầu hết các lớp trong gói này đã được giới thiệu từ lâu trước khi khung tập hợp của Java Foundation Classes  (JFC) được hoàn thiện. Do đó, gói này xử lý các mảng chứ không phải với các lớp bộ sưu tập cao cấp trong gói java.util.

#### 18.2.3.1 Class java.beans.Introspector

Đây là một lớp singleton có thể được sử dụng để lấy thông tin thuộc tính về một lớp được khai báo với đặc tả đặt tên JavaBeans. Một lợi ích quan trọng của lớp này là nó lưu vào bộ nhớ cache việc tra cứu thông tin lớp, giúp tăng tốc đáng kể quá trình reflection.

Mặc dù IntrosInspector rất hữu ích, nhưng nó có một số nhược điểm đáng kể. Đầu tiên, nó không phát hiện các thuộc tính đa hình. Hãy xem xét thuộc tính sau:

```java
public void setValue(final int value);

public int getValue();
```

Thuộc tính này được khai báo bằng cách sử dụng đặc điểm kỹ thuật đặt tên JavaBeans. Tuy nhiên, nó cũng có thể được biểu thị bằng cách sử dụng ký hiệu đa hình:

```java
public void value(final int value);

public int value();
```

Ký hiệu này sử dụng ngữ cảnh của phương thức để xác định xem phương thức là getter hay setter. Cú pháp rõ ràng hơn nhiều vì một trường không cần phải viết hoa. Các thuộc tính này thường được sử dụng trong các sản phẩm như Bộ công cụ tiện ích đơn giản (SWT), được sử dụng để xây dựng GUI Eclipse.

Để phát hiện các thuộc tính đa hình, bạn có thể thử mở rộng Introspector. Tuy nhiên, nếu bạn làm điều này, bạn gặp phải nhược điểm lớn thứ hai của lớp này. Lớp IntrosInspector hoàn toàn là riêng tư, có nghĩa là bạn không thể mở rộng chức năng của nó. Mặc dù nó không được khai báo là lớp cuối cùng, nhưng tất cả các hàm tạo của nó đều là private thay vì protected, có nghĩa là bạn không thể mở rộng nó.

#### 18.2.3.2 Class java.beans.PropertyDescriptor

Lớp này đóng gói trường, setter và getter của một thuộc tính, tất cả ở một vị trí. Về cơ bản, nó là một gói mà bạn có được tất cả thông tin về một thuộc tính ở một nơi thay vì phải tra cứu nó bằng Class.

Thật không may, cả lớp này hay bất kỳ lớp con nào của nó đều không hỗ trợ các thuộc tính bộ sưu tập với các phương thức add và remove. Lớp PropertyDescriptor được sử dụng để cung cấp một giao diện thuận tiện cho các thuộc tính của một lớp. Hầu hết các công cụ reflection sử dụng lớp này thường xuyên.

Một vấn đề với PropertyDescriptor là nó cho phép người dùng thiết lập các phương thức đọc và ghi cho thuộc tính theo cách thủ công. Mặc dù điều này có vẻ là một ý tưởng hay, nhưng nó có thể dẫn đến một số kết quả khó hiểu nếu nhiều phương thức và lớp đang sử dụng cùng một bộ mô tả thuộc tính. Ví dụ: hãy xem xét một GUI xem một đối tượng cũng đang được xem trong một bảng điều khiển khác hiển thị chế độ xem dạng cây về cấu trúc của đối tượng. Nếu một bảng điều khiển thay đổi phương thức của trình truy cập, bảng điều khiển kia sẽ truy cập vào phương thức cũ và có thể bị hỏng. Đây là một ví dụ điển hình về một đối tượng phải là bất biến.

#### 18.2.3.3 Class java.beans.IndexedPropertyDescriptor

Lớp này tương tự như PropertyDescriptor nhưng có thêm chức năng để xử lý các thuộc tính là mảng (còn được gọi là thuộc tính được lập chỉ mục). Đây là một ví dụ đơn giản về thuộc tính được lập chỉ mục:

```java
public class SomeClass {

    private String[] values = new String[0];

    public void setValues( String[] values ) {
        this.values = values;
    }

    public void setValues( final String value, final int index ) {
        this.values[index] = value;
    }

    public String[] getValues() {
        return values;
    }

    public String getValues( final int index ) {
        return values[index];
    }

}
```

Trong trường hợp này, thuộc tính values là thuộc tính được lập chỉ mục vì nó là một mảng và có các phương thức để đặt hoặc lấy values riêng lẻ trong mảng.

Lớp IndexedPropertyDescriptor sẽ cho phép bạn truy cập các phương thức get và set của thuộc tính và các phương thức get và set được lập chỉ mục của thuộc tính.

#### 18.2.3.4 Classes MethodDescriptor và ParameterDescriptor

Các lớp này đóng gói thông tin về các phương thức và các tham số của chúng. Lớp MethodDescriptor cho phép bạn truy xuất thông tin về một phương thức trong một lớp và ParameterDescriptor cho phép bạn lấy thông tin về các tham số của phương thức. Các lớp này hữu ích cho việc lập trình bean vì chúng chứa thông tin hiển thị về phương thức và các tham số của nó. Tuy nhiên, để phản ánh, chúng không cung cấp bất kỳ chức năng đáng kể nào trên java.lang.reflect.Method. Do đó, các lớp này hiếm khi được sử dụng trong lập trình reflection chính thống.

#### 18.2.3.5 Interface java.beans.BeanInfo

Lớp này chứa tất cả thông tin về một JavaBean cụ thể. Nó cung cấp một nơi thuận tiện mà từ đó bạn có thể tra cứu các bộ mô tả khác nhau và thông tin khác về lớp. Introspector được sử dụng để tra cứu và lưu vào bộ nhớ cache các cá thể BeanInfo.

Tuy nhiên, một vấn đề lớn với lớp BeanInfo là nó không cung cấp quyền truy cập vào các bộ mô tả thuộc tính theo tên. Thật không may, việc mở rộng lớp này là không thể vì Introspector hoàn toàn là riêng tư. Để có một Introspector tìm thấy lớp BeanInfo mở rộng của bạn, bạn sẽ phải xác định các đối tượng theo cách thủ công trên mọi lớp (điều này sẽ đánh bại điểm reflection) hoặc thực hiện lại Introspector từ đầu (đây không phải là một nhiệm vụ nhỏ).

## 18.3 Applying Reflection vào MutableObject

Trong Chương 17, chúng ta đã thảo luận về cách triển khai mô hình dữ liệu để đóng gói dữ liệu trong một ngân hàng giả định. Hầu hết các đối tượng mô hình dữ liệu trong mô hình này đều giống nhau về cấu trúc. Chúng chỉ khác nhau về số lượng, tên và loại thuộc tính mà chúng chứa. Sự phản chiếu có thể được sử dụng trên mô hình dữ liệu này để triển khai một số tính năng chính.

### 18.3.1 Reflecting on toString()

Trong mô hình dữ liệu Ngân hàng Trực tuyến của bạn, có nhiều lớp đóng gói dữ liệu và mỗi lớp có một số thuộc tính. Mặc dù bạn có thể so sánh chúng và lấy hashCode() của chúng, nhưng bạn không thể in chúng ra bảng điều khiển. Đây là công việc của phương thức toString(). Tuy nhiên, phương thức toString() mặc định được định nghĩa trên lớp Object sẽ chỉ in ra mã băm và kiểu của đối tượng. Đây không phải là đủ thông tin để thực hiện bất kỳ gỡ lỗi nghiêm trọng nào với các lớp này. Những gì bạn thực sự cần là phương thức toString() để in giá trị của các thuộc tính trong đối tượng có thể thay đổi.

Một chiến thuật bạn có thể thực hiện là tái cấu trúc phương thức toString() và làm cho mỗi lớp khai báo phương thức toString() của riêng nó. Điều này sẽ buộc các nhà phát triển phải triển khai phương thức trên mỗi lớp, điều này sẽ giải quyết được vấn đề. Tuy nhiên, việc triển khai phương thức toString() trên mỗi lớp sẽ giống nhau, ngoại trừ tên của các thuộc tính và lớp; Ngoài ra, phương pháp sẽ phải được sửa đổi theo cách thủ công nếu các thuộc tính mới được thêm vào hoặc loại bỏ. Sẽ tốt hơn nếu bạn có thể lưu những người dùng MutableObject tất cả công việc này bằng cách triển khai phương thức toString() trong lớp MutableObject để in các thuộc tính của các lớp dẫn xuất. Bạn có thể làm điều này với sự phản chiếu (xem Ví dụ dưới).

```java
public abstract class MutableObject implements Serializable {

    public String toString() {
        try {

            final BeanInfo info = Introspector.getBeanInfo(this.getClass(), Object.class);
            final PropertyDescriptor[] props = info.getPropertyDescriptors();
            final StringBuffer buf = new StringBuffer(500);

            Object value = null;

            buf.append(getClass().getName());
            buf.append("@");
            buf.append(hashCode());
            buf.append("={");

            for( int idx = 0; idx < props.length; idx++ ) {
                if (idx != 0) {
                    buf.append(", ");
                }
                buf.append(props[idx].getName());
                buf.append("=");

                if (props[idx].getReadMethod() != null) {
                    value = props[idx].getReadMethod().invoke(this, null);

                    if (value instanceof MutableObject) {
                        buf.append("@");
                        buf.append(value.hashCode());
                    } else if (value instanceof Collection) {
                        buf.append("{");
                        for( Iterator iter = ((Collection) value).iterator(); iter.hasNext(); ) {
                            Object element = iter.next();
                            if (element instanceof MutableObject) {
                                buf.append("@");
                                buf.append(element.hashCode());
                            } else {
                                buf.append(element.toString());
                            }
                        }
                        buf.append("}");

                    } else if (value instanceof Map) {
                        buf.append("{");
                        Map map = (Map) value;
                        for( Iterator iter = map.keySet().iterator(); iter.hasNext(); ) {

                            Object key = iter.next();
                            Object element = map.get(key);

                            buf.append(key.toString());
                            buf.append("=");
                            if (element instanceof MutableObject) {
                                buf.append("@");
                                buf.append(element.hashCode());
                            } else {
                                buf.append(element.toString());
                            }
                        }
                        buf.append("}");
                    } else {
                        buf.append(value);
                    }
                }
            }
            buf.append("}");
            return buf.toString();
        } catch ( Exception ex ) {
            throw new RuntimeException(ex);
        }
    }
}
```

Mặc dù phương pháp này có vẻ dài đối với một tác vụ đơn giản như vậy, nhưng nó là một phần nhỏ của mã cut-and-paste sẽ được yêu cầu để triển khai các phương thức toString() trên tất cả các loại MutableObject riêng lẻ trong một mô hình dữ liệu lớn. Hơn nữa, phương thức toString() phản xạ không yêu cầu bảo trì nếu bạn quyết định thêm một thuộc tính mới vào đối tượng mô hình dữ liệu của mình. Nó tự động cập nhật chính nó.

Hầu hết mã trong Ví dụ trên là cấu trúc string khá đơn giản. Tuy nhiên, các phần được nhấn mạnh cho thấy các cách sử dụng khác nhau của bộ công cụ reflection. Đầu tiên, yêu cầu Trình kiểm tra cho BeanInfo trên đối tượng và các lớp cha của nó lên đến, nhưng không bao gồm Object. Sau đó, lấy các đối tượng PropertyDescriptor cho lớp đó, nó sẽ cấp cho bạn quyền truy cập vào các thuộc tính. Tại thời điểm này, bạn có thể lặp qua các thuộc tính, nhưng hãy đảm bảo rằng bạn kiểm tra một phương thức null read, điều này có thể xảy ra nếu thuộc tính là chỉ ghi.

Khi phương thức toString() được biên dịch, nó sẽ hoạt động đối với bất kỳ phương thức con MutableObject nào mà bạn có thể mơ ước. Cho dù bạn có 4 đối tượng mô hình dữ liệu hay 40.000 đối tượng, bạn sẽ không phải nhấc một ngón tay khác để cung cấp cho các đối tượng của mình khả năng in nội dung của chúng. Hãy thử nó trên một số đối tượng của bạn:

```java
public class DemoToStringUsage {

    public static void main( String[] args ) {

        Person p = new Person();

        p.setFirstName("Robert");
        p.setLastName("Simmons");
        p.setGender(Gender.MALE);
        p.setTaxID("123abc456");

        Customer c = new Customer();

        c.setPerson(p);
        c.setCustomerID(new Integer(414122));
        c.setEmail("foo@bar.com");

        SavingsAccount a = new SavingsAccount();

        a.setCustomer(c);
        a.setBalance(new Float(2212.5f));
        a.setID(new Integer(412413789));
        a.setInterestRate(new Float(0.062f));

        Set accounts = new HashSet();
        accounts.add(a);

        c.setAccounts(accounts);

        System.out.println(p);
        System.out.println(c);
        System.out.println(a);
    }

}
```

### 18.3.2 Fetching Constraints

Vì các ràng buộc mà bạn đặt trên các đối tượng mô hình dữ liệu của mình được thiết kế để có thể truy cập vào bảng điều khiển, trang JSP và các lớp khác bằng cách sử dụng các đối tượng này, sẽ rất tiện lợi nếu có một cách để tìm nạp các ràng buộc từ mô hình. Thay vì yêu cầu nhà phát triển chỉ định các ràng buộc tại thời điểm biên dịch trong cấu trúc dữ liệu, bạn có thể sử dụng phản chiếu để tìm nạp các ràng buộc. Ví dụ dưới cho thấy cách bạn có thể xây dựng bản đồ các ràng buộc trên một hậu duệ MutableObject. Các dòng được nhấn mạnh sử dụng sự phản chiếu.

```java
public abstract class MutableObject implements Serializable {

    public static final Map buildConstraintMap( final Class dataType ) throws IllegalAccessException {

        final int modifiers = Modifier.PUBLIC | Modifier.FINAL | Modifier.STATIC;
        Map constraintMap = new HashMap();
        final Field[] fields = dataType.getFields();
        Object value = null;

        for( int idx = 0; idx < fields.length; idx++ ) {
            if ((fields[idx].getModifiers() & modifiers) == modifiers) {
                value = fields[idx].get(null);
                if (value instanceof ObjectConstraint) {
                    constraintMap.put(((ObjectConstraint) value).getName(), value);
                }
            }
        }

        return Collections.unmodifiableMap(constraintMap);
    }
}
```

Lấy các trường trong lớp đích và sau đó kiểm tra từng trường public static final để xem liệu chúng có chứa các thể hiện của lớp ObjectConstraint hay không. Nếu có, hãy đặt ràng buộc vào map và khóa nó bằng tên của thuộc tính mà nó ràng buộc (bạn có thể lấy từ lớp ObjectConstraint). Phương thức buildConstraintMap() sẽ cung cấp cho bạn các tham chiếu đến các ràng buộc trên các đối tượng của bạn, bạn có thể sử dụng phương thức này trong GUI hoặc xác thực biểu mẫu trang web của mình.

Tuy nhiên, có một vấn đề với cơ chế ràng buộc. Mặc dù reflection là một công cụ mạnh mẽ, nhưng quá trình xem xét nội tâm (phân tích một lớp để xác định nội dung của nó) lại chiếm rất nhiều thời gian của CPU. Nếu người dùng MutableObject phải tìm nạp các ràng buộc theo cách này mỗi khi họ cần, thì chương trình của bạn sẽ chạy chậm. Bạn cần phải lưu vào bộ nhớ cache các ràng buộc của mình trong MutableObject. Bạn có thể tạo một danh mục đơn giản để thực hiện việc này:

```java
public abstract class MutableObject implements Serializable {

    private static final Map CONSTRAINT_CACHE = new HashMap();

    public static ObjectConstraint getConstraint( final Class dataType, final String name ) {
        Map constraintMap = getConstraintMap(dataType);
        return (ObjectConstraint) constraintMap.get(name);
    }

    public static final Map getConstraintMap( final Class dataType ) throws RuntimeException {

        try {
            Map constraintMap = (Map) CONSTRAINT_CACHE.get(dataType);
            if (constraintMap == null) {
                constraintMap = buildConstraintMap(dataType);
                CONSTRAINT_CACHE.put(dataType, constraintMap);
                return constraintMap;
            }
            return Collections.unmodifiableMap(constraintMap);

        } catch ( final IllegalAccessException ex ) {
            throw new RuntimeException(ex);
        }
    }

}
```

Mã này sẽ lưu vào bộ đệm từng bản đồ ràng buộc lần đầu tiên chúng được tìm nạp. Bây giờ tất cả những gì bạn phải làm là thay đổi khả năng hiển thị của buildConstraintMap() thành protected để buộc người dùng gọi các phương thức lưu vào bộ nhớ đệm:

```java
public class ConstraintMapDemo {

    public static final void main( final String[] args ) {

        Map constraints = MutableObject.getConstraintMap(Customer.class);
        Iterator iter = constraints.values().iterator();
        ObjectConstraint constraint = null;

        while( iter.hasNext() ) {
            constraint = (ObjectConstraint) iter.next();
            System.out.println("Property=" + constraint.getName() + " Type=" + constraint.getClass().getName());
        }

        constraint = MutableObject.getConstraint(SavingsAccount.class, "interestRate");

        System.out.println("\nSavingsAccount interestRate property");
        System.out.println("dataType = " + ((NumericConstraint) constraint).getDataType().getName());
        System.out.println("minValue = " + ((NumericConstraint) constraint).getMinValue());
        System.out.println("maxValue = " + ((NumericConstraint) constraint).getMaxValue());

    }

}
```

## 18.4 Hiệu năng của Reflection

Cuộc thảo luận của chúng tôi về bản đồ ràng buộc bộ nhớ đệm dẫn đến câu hỏi về hiệu suất của phản xạ.

Câu trả lời là reflection có thể rẻ hoặc đắt tùy thuộc vào cách bạn sử dụng nó. Không có công cụ nào trong bất kỳ ngôn ngữ lập trình nào có giá bằng không. Ví dụ dưới cho thấy phản xạ hoạt động như thế nào.

```java
public class ReflexiveInvocation {

    private String value = "some value";

    public ReflexiveInvocation() {
    }

    public static void main( final String[] args ) {

        try {

            final int CALL_AMOUNT = 1000000;
            final ReflexiveInvocation ri = new ReflexiveInvocation();
            int idx = 0;
            long millis = System.currentTimeMillis();

            for( idx = 0; idx < CALL_AMOUNT; idx++ ) {
                ri.getValue();
            }

            System.out.println("Calling method " + CALL_AMOUNT + " times programatically took "
                    + (System.currentTimeMillis() - millis) + " millis");

            Method md = null;
            millis = System.currentTimeMillis();

            for( idx = 0; idx < CALL_AMOUNT; idx++ ) {
                md = ri.getClass().getMethod("getValue", null);
                md.invoke(ri, null);
            }

            System.out.println("Calling method " + CALL_AMOUNT + " times reflexively with lookup took "
                    + (System.currentTimeMillis() - millis) + " millis");

            md = ri.getClass().getMethod("getValue", null);
            millis = System.currentTimeMillis();

            for( idx = 0; idx < CALL_AMOUNT; idx++ ) {
                md.invoke(ri, null);
            }

            System.out.println("Calling method " + CALL_AMOUNT + " times reflexively with cache took "
                    + (System.currentTimeMillis() - millis) + " millis");

        } catch ( final NoSuchMethodException ex ) {
            throw new RuntimeException(ex);
        } catch ( final InvocationTargetException ex ) {
            throw new RuntimeException(ex);
        } catch ( final IllegalAccessException ex ) {
            throw new RuntimeException(ex);
        }

    }

    public String getValue() {
        return this.value;
    }

}
```

Trong phần đầu tiên của bài kiểm tra này, bạn đo thời gian cần thiết để gọi getter của clasa thông thường là 1 triệu lần. Trong giai đoạn thứ hai, bạn đo thời gian cần thiết để gọi getter 1 triệu lần bằng cách sử dụng lời gọi dựa trên reflection, trong đó phương thức getter được tra cứu mỗi khi lặp lại vòng lặp. Cuối cùng, bạn lưu vào bộ nhớ cache việc tra cứu getter và gọi phương thức getter 1 triệu lần bằng cách sử dụng đối tượng Phương thức được lưu trong bộ nhớ cache. Kết quả của việc chạy lớp này được hiển thị ở đây:

>ant -Dexample=oreilly.hcj.reflection.ReflexiveInvocation run_example

run_example:

>[java] Calling method 1000000 times normally took 20 millis\
>[java] Calling method 1000000 times reflexively with lookup took 5618 millis\
>[java] Calling method 1000000 times reflexively with cache took 270 millis

Giai đoạn trong đó đối tượng Phương thức được lưu vào bộ nhớ cache nhanh hơn đáng kể so với giai đoạn trong đó đối tượng Phương thức được tra cứu ở mỗi lần lặp. Đương nhiên, cả hai giai đoạn đều chậm hơn so với việc gọi phương thức bình thường.

Bài kiểm tra này dạy một vài bài học. Đầu tiên, tra cứu đối tượng Method là phần đắt giá nhất của phản xạ. Nếu bạn lưu phương pháp này vào bộ nhớ cache, bạn có thể cải thiện đáng kể hiệu suất. Thứ hai, phản xạ chậm hơn so với các phương thức gọi thông thường. Tuy nhiên, sự mất hiệu suất này được bù đắp bằng lợi ích của việc sản xuất sản phẩm của bạn nhanh hơn đáng kể và ít lỗi hơn. Trong hầu hết các trường hợp, lợi ích của sự phản chiếu rất xứng đáng với chi phí 0,25 giây trên 1 triệu cuộc gọi.

## 18.5 Reflection + JUnit = Mã ổn định

Với Reflection, bạn có thể kiểm tra một đối tượng mô hình dữ liệu bất kể nó khai báo thuộc tính nào:

```java
public class TestMutableObject extends TestCase {

    private Class type = null;

    protected TestMutableObject( final Class type, final String testName ) {
        super(testName);
        assert (type != null);
        assert (MutableObject.class.isAssignableFrom(type));
        this.type = type;
    }

    public static Test suite( final Class type ) {
        if (type == null) {
            throw new NullPointerException("type");
        }
        if (!MutableObject.class.isAssignableFrom(type)) {
            throw new IllegalArgumentException("type");
        }
        TestSuite suite = new TestSuite(type.getName());
        suite.addTest(new TestMutableObject(type, "testConstraintsExist"));
        return suite;
    }

    public void testConstraintsExist() {
        try {
            final PropertyDescriptor[] props = Introspector.getBeanInfo(type, Object.class).getPropertyDescriptors();

            for( int idx = 0; idx < props.length; idx++ ) {
                ObjectConstraint constraint = MutableObject.getConstraint(type, props[idx].getName());
                assertNotNull("Property " + props[idx].getName() + " does not have a constraint.", constraint);
            }

        } catch ( final IntrospectionException ex ) {
            throw new RuntimeException();
        }
    }

}
```

Lớp này là một trường hợp thử nghiệm JUnit tiêu chuẩn với một vòng xoắn. Nó không biết nó sẽ thử nghiệm loại lớp nào cho đến khi bộ trường hợp thử nghiệm được tạo bằng phương thức suite(). Người dùng test case chuyển lớp mà anh ta muốn kiểm tra vào phương thức suite () và bài kiểm tra được xây dựng cho lớp cụ thể đó:

```java
public class BankDataTests {

    public static Test suite() {
        TestSuite suite = new TestSuite("Online-Only Bank Datamodel Tests");

        suite.addTest(TestMutableObject.suite(Account.class));
        suite.addTest(TestMutableObject.suite(AssetAccount.class));
        suite.addTest(TestMutableObject.suite(AutomaticPayment.class));
        suite.addTest(TestMutableObject.suite(BankOfficer.class));
        suite.addTest(TestMutableObject.suite(CreditCardAccount.class));
        suite.addTest(TestMutableObject.suite(Customer.class));
        suite.addTest(TestMutableObject.suite(Employee.class));
        suite.addTest(TestMutableObject.suite(ExternalTransaction.class));
        suite.addTest(TestMutableObject.suite(LiabilityAccount.class));
        suite.addTest(TestMutableObject.suite(LoanAccount.class));
        suite.addTest(TestMutableObject.suite(LoanApplication.class));
        suite.addTest(TestMutableObject.suite(OnlineCheckingAccount.class));
        suite.addTest(TestMutableObject.suite(Person.class));
        suite.addTest(TestMutableObject.suite(SavingsAccount.class));
        suite.addTest(TestMutableObject.suite(Transaction.class));

        return suite;
    }

    public static void main( final String[] args ) {
        TestRunner.run(BankDataTests.class);
    }
}
```
