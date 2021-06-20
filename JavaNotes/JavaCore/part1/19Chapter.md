# Chapter 19 Proxies

## 19.1 Proxy là cái gì?

Proxy là một đối tượng thay thế cho một đối tượng khác và dường như thực hiện các chức năng của đối tượng đầu tiên. Đối tượng mà proxy bắt chước được gọi là đối tượng thực thi. Thay vì sử dụng triển khai trực tiếp, các lớp muốn truy cập các tính năng của triển khai sử dụng proxy.

Để hiểu rõ nó xem ví dụ 19-1:

```java
// ví dụ 19-1

public class SomeClassImpl {

    private String userName;

    public SomeClassImpl( final String userName ) {
        this.userName = userName;
    }

    public void someMethod() {
        System.out.println(this.userName);
    }

    public void someOtherMethod( final String text ) {
        System.out.println(text);
    }

}
```

Vì bạn muốn người dùng có thể sử dụng lớp này mà không cần có quyền truy cập trực tiếp vào nó, bạn cần tạo một proxy. Hãy bắt đầu với một proxy đơn giản tỏng ví dụ 19-2:

```java
// ví dụ 19-2
public class SomeClassProxy {

    private final SomeClassImpl impl;

    public SomeClassProxy( final SomeClassImpl impl ) {
        this.impl = impl;
    }

    public void someMethod() {
        this.impl.someMethod();
    }

    public void someOtherMethod( String text ) {
        this.impl.someOtherMethod(text);
    }

}
```

Mã này tạo một proxy cho SomeClassImpl trông giống hệt như chính SomeClassImpl. Tuy nhiên, thay vì cung cấp chức năng của lớp, SomeClassProxy chuyển tiếp các yêu cầu tới SomeClassProxy để triển khai (được lưu trữ trong biến thành viên impl). Bây giờ bạn đã có proxy của mình, bạn có thể làm việc với nó như thể bạn đang làm việc với chính quá trình triển khai.

Bạn có thể thắc mắc tại sao mã đặc biệt này không được chèn vào chính đối tượng triển khai. Có nhiều lý do tại sao bạn có thể không muốn làm điều này:

- Mã nguồn của lớp triển khai có thể không truy cập được. Đây có thể là trường hợp nếu thư viện của bên thứ ba được triển khai bởi công ty của bạn hoặc được cung cấp bởi một bộ phận riêng biệt.
- Chức năng mới được thêm vào thường không phù hợp với chức năng của đối tượng triển khai. Thường thì nó sẽ vi phạm nguyên tắc rằng một đối tượng chỉ nên triển khai một khái niệm.
- Việc triển khai có thể làm lộ các tính năng vẫn bị ẩn với nhiều người dùng khác nhau.
- Bạn có thể muốn ẩn tên của các phương pháp triển khai với người dùng của việc triển khai vì lý do bảo mật. Điều này sẽ yêu cầu một proxy dạng xem đặc biệt, trong đó các phương thức trong proxy có tên khác với tên trong quá trình triển khai.
- Chức năng có thể phụ thuộc vào ngữ cảnh mà đối tượng đang được sử dụng. Ví dụ, một máy tính được kết nối trực tiếp với cánh tay robot trong nhà máy không cần mã mạng để truy cập cánh tay đó, nhưng trung tâm điều khiển ở phía bên kia của nhà máy lại cần mã này.
- Chức năng có thể phụ thuộc vào giai đoạn phát triển. Ví dụ: bạn có thể sử dụng proxy để thực hiện hành vi theo dõi trong một chương trình đếm số lần đối tượng được gọi. Mã này có thể không cần thiết trong bản phát hành đã triển khai của phần mềm.
- Vị trí của đối tượng triển khai có thể thay đổi, giống như trong lập trình doanh nghiệp. Các đối tượng thực hiện các hoạt động thực tế trong mạng doanh nghiệp thường thay đổi vị trí tùy thuộc vào mã cân bằng tải và khả năng chịu lỗi. Bạn sẽ cần một proxy thông minh để định vị đối tượng để cung cấp các dịch vụ của đối tượng đó cho người dùng.

Có nhiều lý do khác để tạo proxy. Tuy nhiên, đây là một số phổ biến nhất. Trên thực tế, toàn bộ mô hình lập trình Enterprise JavaBeans được mô hình hóa dựa trên lý do cuối cùng.

Trước đây trong hệ phân tán họ từng xử dùng proxies cho việc kêt nối đến một bên khác thay vì các kết nối cổ điển nhất TCP/IP. Đại diện cho nó là CORBA, RMI và EJB... Tôi nghĩ các hệ thống loại này vẫn tồn tại ở đâu đó, loại này có rủi ro khác đó là đường truyền mạng. Với mô hình hiện đại họ thường xử dùng broker message queue đại diện như Kafka, rabitMG,...

### 19.1.1 Factories

Để nhận proxy đến một đối tượng, bạn thường phải sử dụng đối tượng gốc, đối tượng này kết hợp proxy và việc triển khai thành một đơn vị. Một lý do tại sao Factories được sử dụng là bởi vì bạn không đặc biệt quan tâm loại đối tượng nào thực hiện chức năng của bạn, miễn là nó thực hiện đúng cách.

Với EJB, đối tượng gốc tìm thấy việc triển khai trên mạng bằng cách sử dụng một đối tượng khác hoặc một mã định danh duy nhất. Sau đó, nhà máy liên kết proxy với việc triển khai và trả lại proxy cho bạn. Tại thời điểm này, bạn có thể làm việc với proxy như thể nó là sự triển khai của đối tượng, ngay cả khi việc triển khai là trên một máy tính khác.

Các đối tượng Factory được sử dụng để lấy proxy vì chúng thường thực hiện rất nhiều công việc thiết lập proxy. Họ có thể cần phân bổ tài nguyên mạng cho proxy hoặc cho phép proxy thực hiện các quy trình khởi tạo để tự chuẩn bị sử dụng. Ngoài ra, các đối tượng của nhà máy thường thực hiện chính sách bảo mật nhằm ngăn chặn hoạt động trái phép hoặc nguy hiểm

```java
SomeClassProxy proxy = new SomeClassProxy(new SomeClassImpl("Fred"));
```

Tuy nhiên, bạn muốn chuyển tên người dùng thực của người dùng trong hàm tạo tới SomeClassImpl. Nếu bạn cho phép người dùng proxy chuyển bất cứ thứ gì anh ta muốn, bạn không thể chắc chắn rằng anh ta sẽ chuyển bằng tên người dùng thực của mình.

```java
public class SomeClassFactory {

    public final static SomeClassProxy getProxy() {
        SomeClassImpl impl = new SomeClassImpl(System.getProperty("user.name"));
        return new SomeClassProxy(impl);
    }
}
```

Trong mã này, nhà máy proxy thiết lập đối tượng và liên kết với việc triển khai. Trong một hệ thống thực, người dùng sẽ không thể tạo các đối tượng triển khai thực tế. Ví dụ, trong hệ thống EJB, người dùng chỉ đơn giản là không có tệp lớp thực sự để triển khai. Thay vào đó, anh ta phải yêu cầu máy chủ EJB cung cấp proxy cho đối tượng đó.

```java
public class DemoProxyFactory {

  public static final void main(final String[] args) {
    SomeClassProxy proxy = SomeClassFactory.getProxy();
    proxy.someMethod();
    proxy.someOtherMethod("Our Proxy works!");
  }
}
```

Ở đây, nhà máy proxy được sử dụng để tạo proxy cho lớp triển khai mong muốn. Vì đây là cách duy nhất người dùng có thể truy cập vào việc triển khai trong hệ thống thực, nên anh ta không bao giờ có thể gửi tên người dùng sai.

Bất cứ khi nào bạn truy cập vào bất kỳ đối tượng EJB nào, bạn đều tận dụng lợi thế của một nhà máy được gọi là giao diện trang chủ. Vì giao diện trang chủ là bản chất thứ hai đối với các lập trình viên EJB, nhiều người không nhận ra rằng đây thực sự là một nhà máy, cũng như họ không nhận ra rằng đối tượng mà họ đang sử dụng ở phía máy khách thực sự là một proxy khá phức tạp.

### 19.1.2 Client and Server Objects

Với proxy và lập trình phân tán, các thuật ngữ máy khách và máy chủ mang một ý nghĩa khác với những gì mà nhiều lập trình viên máy khách / máy chủ thường dùng. Trong lập trình máy khách / máy chủ tiêu chuẩn, máy khách là giao diện người dùng và máy chủ thực hiện logic nghiệp vụ của hệ thống. Trong bối cảnh này, khách hàng không bao giờ có bất kỳ logic kinh doanh nào cả.

Trong lập trình dựa trên proxy, máy khách chỉ đơn giản là một đối tượng tận dụng các dịch vụ của một đối tượng khác, được gọi là máy chủ (đối tượng). Do đó, một đối tượng duy nhất có thể vừa là máy khách cho một đối tượng khác vừa là máy chủ cho các máy khách khác nhau, tất cả cùng một lúc.

Hiểu được sự phân biệt giữa máy khách và máy chủ là điều quan trọng khi nói về proxy vì thuật ngữ này thường xung đột với thuật ngữ từ phương pháp lập trình máy khách / máy chủ cũ.

## 19.2 Hai loại proxy

Có 2 loại proxy:  proxy tĩnh và Proxy động ( Proxy by Interface)

### 19.2.1 Proxy tĩnh

Các proxy được viết thủ công được gọi là proxy tĩnh. Các proxy tĩnh được lập trình vào hệ thống tại thời điểm biên dịch. Ví dụ 19-2 cho thấy một proxy tĩnh đơn giản.

Lập trình proxy tĩnh cũng giống như lập trình bất kỳ lớp nào khác với một vài quy tắc mới:

- Proxy không bao giờ có thể mở rộng triển khai (hoặc ngược lại). Người được ủy quyền và việc triển khai phải tạo thành một cấu trúc ủy quyền trong đó người được ủy quyền ủy quyền việc thực hiện. Hạn chế này tồn tại bởi vì nếu proxy chỉ đơn giản là mở rộng quá trình triển khai, người dùng sẽ có thể chuyển proxy đến việc triển khai và bỏ qua proxy hoàn toàn.
- Người dùng proxy không bao giờ được tạo triển khai. Nếu người dùng có thể tạo một triển khai trực tiếp, thì anh ta có thể bỏ qua proxy và chỉ cần sử dụng triển khai.

Vì không có gì ngăn cản người dùng bỏ qua proxy và đi trực tiếp vào quá trình triển khai, nên proxy luôn phải được lấy từ nhà máy hoặc thông qua các cuộc gọi tới proxy khác.

Khi viết proxy tĩnh, lập trình viên đưa mã mới vào proxy để triển khai chức năng mới. Ví dụ: bạn có thể thay đổi proxy từ Ví dụ 19-2 để kết hợp khả năng đếm số lần các phương thức triển khai được gọi. Lớp kết quả được hiển thị trong Ví dụ 19-4.

```java
//ví dụ 19-4
public class SomeClassCountingProxy {

    private final SomeClassImpl impl;
    private int invocationCount = 0;

    public SomeClassCountingProxy( final SomeClassImpl impl ) {
        this.impl = impl;
    }

    public int getInvocationCount() {
        return invocationCount;
    }

    public void someMethod() {
        this.invocationCount++;
        this.impl.someMethod();
    }

    public void someOtherMethod( String text ) {
        this.invocationCount++;
        this.impl.someOtherMethod(text);
    }

}
```

Proxy tĩnh này sẽ theo dõi số lần các phương thức của triển khai được gọi. Bạn đã thay đổi proxy ban đầu và chèn nội dung vào proxy để triển khai chức năng cần thiết. Sử dụng proxy hầu như giống với sử dụng đối tượng SomeClassProxy. Đây là một phương pháp tạo nhà máy đã sửa đổi:

```java
public class SomeClassFactory {
  public static final SomeClassCountingProxy getCountingProxy( ) {
    SomeClassImpl impl = new SomeClassImpl(System.getProperty("user.name"));
    return new SomeClassCountingProxy(impl);
  }
}
```

Cũng giống như trước đây, lấy proxy từ nhà máy và sau đó sử dụng nó như thể nó là chính lớp:

```java
public class DemoCountingProxy {

  public static final void main(final String[] args) {
    SomeClassCountingProxy proxy = SomeClassFactory.getCountingProxy();
    proxy.someMethod();
    proxy.someOtherMethod("Our Proxy works!");
    System.out.println("Method Invocation Count = " + proxy.getInvocationCount());
  }
}
```

Chức năng hai cấp này cung cấp cho bạn rất nhiều sức mạnh trong việc thiết kế các ứng dụng mạnh mẽ. Bạn có thể chèn mã vào giữa việc triển khai một đối tượng và chính đối tượng để thay đổi chức năng của việc triển khai ban đầu. Ngoài ra, bạn có thể thêm nhiều phương thức hơn vào lớp proxy không tồn tại trong lớp triển khai ban đầu. Ví dụ, phương thức getInvocationCount (), không tồn tại trong triển khai ban đầu, đã được thêm vào. Kỹ thuật này được biết đến như một mẫu trang trí (decorator pattern).

Decorators đặc biệt hữu ích khi bạn muốn sửa đổi các đối tượng có nguồn mà bạn không kiểm soát, chẳng hạn như thư viện của bên thứ ba. Tuy nhiên, chúng cũng có thể hữu ích trong việc tạo chức năng mở rộng của một đối tượng trong một ngữ cảnh cụ thể, như được thực hiện với phương thức đếm.

### 19.2.2 Proxy by Interface

Trong các ví dụ trước, proxy mà bạn đã tạo yêu cầu người dùng phải biết loại proxy mà họ muốn. Đây thường không phải là một tình huống mong muốn.

Tuy nhiên, người sử dụng triển khai không quan tâm đến những chi tiết này. Anh ta chỉ muốn sử dụng đối tượng mà không cần lo lắng về việc triển khai thực sự ở đâu. Những gì bạn cần là một cách để nhà máy cung cấp cho người dùng proxy chính xác dựa trên thông tin cụ thể, chẳng hạn như vị trí triển khai trong mạng.

Trong Ví dụ 19-4, bạn đã tạo một proxy đếm số lần các phương thức của đối tượng được gọi. Ví dụ về proxy hoạt động khác nhau dựa trên các điều kiện cụ thể, giả sử rằng proxy đếm của bạn có thể cần thiết trong quá trình gỡ lỗi nhưng không cần thiết trong quá trình triển khai ứng dụng. Những gì bạn muốn làm là cung cấp cho người dùng proxy đếm khi kích hoạt gỡ lỗi và proxy không gắn khi gỡ lỗi bị tắt. Tuy nhiên, người dùng sẽ không biết mình đang nhận được proxy nào cho đến khi chạy. Do đó, bạn cần một cách khác để người dùng tham chiếu đến proxy. Các giao diện Java là lý tưởng để giải quyết các vấn đề như vậy.

Để cô lập các chi tiết triển khai của proxy, bạn có thể sử dụng giao diện mô tả chức năng của việc triển khai. Khi bạn áp dụng kỹ thuật này cho lớp triển khai từ Ví dụ 19-2, bạn sẽ nhận được kết quả trong Ví dụ 19-5.

```java
// ex 19-5

public interface SomeClass {
  public abstract void someMethod( );
  public abstract void someOtherMethod(final String text);
}
```

Bây giờ bạn đã có giao diện để triển khai, bạn có thể thực thi giao diện này bằng cách thay đổi SomeClassImpl:

```java
public class SomeClassImpl implements SomeClass {
  // same as before
}
```

Ngoài ra, bạn có thể làm cho các lớp proxy triển khai giao diện:

```java
public class SomeClassProxy implements SomeClass {
  // same as before
}

public class SomeClassCountingProxy implements SomeClass {
  // same as before
}
```

Mặc dù các lớp proxy triển khai giao diện, điều này không phá vỡ quy tắc kế thừa trước đó vì người dùng sẽ chỉ có proxy, không kế thừa từ việc triển khai. Nếu người dùng nhận được proxy và chuyển nó lên giao diện, máy ảo sẽ không gặp sự cố. Tuy nhiên, nếu anh ta cố gắng truyền giao diện đến việc triển khai, cơ chế RTTI tích hợp sẵn sẽ tát anh ta bằng ClassCastException. Truyền một đối tượng không thay đổi kiểu của nó; nó chỉ đơn thuần là thay đổi cách nhìn của loại nó. Một phiên bản của SomeClassProxy vẫn là SomeClassProxy bất kể nó được truyền như thế nào.

Bây giờ cả proxy và quá trình triển khai đều đang sử dụng giao diện mới này, bạn có thể tạo factory cho phép người dùng bỏ qua việc triển khai thực tế của chính proxy. Factory được thể hiện trong Ví dụ 10-6.

```java
public class SomeClassFactory {

  public static final SomeClass getSomeClassProxy( ) {
    SomeClassImpl impl = new SomeClassImpl(System.getProperty("user.name"));
    if (LOGGER.isDebugEnabled( )) {
      return new SomeClassCountingProxy(impl);      
    } else {
      return new SomeClassProxy(impl);      
    }
  }
}
```

Phương thức factory mới chỉ đơn giản là trả lại một đối tượng kiểu SomeClass. Vì cả hai proxy đều triển khai SomeClass, bạn có thể trả lại bất kỳ proxy nào bạn muốn. Trong ví dụ này, nếu Log4J được thiết lập để gỡ lỗi, hãy trả về proxy đếm số lần gọi; nếu không, hãy trả về proxy không gắn thẻ.

Để sử dụng phương thức factory mới, người dùng chỉ cần biết rằng anh ta đang nhận được một phiên bản của SomeClass. Lược đồ proxy mới của bạn được hiển thị ở đây:

```java
public class DemoInterfaceProxy {

  public static final void main(final String[] args) {
    SomeClass proxy = SomeClassFactory.getSomeClassProxy( );
    proxy.someMethod( );
    proxy.someOtherMethod("Our Proxy works!");
  }
}
```

Người dùng không cần biết (hoặc quan tâm) nếu proxy đang đếm. Bạn có thể cung cấp cho anh ta bất kỳ proxy nào phù hợp với giao diện SomeClass. Sử dụng giao diện rõ ràng là ưu việt hơn so với sử dụng các lớp cụ thể vì nó tách máy khách khỏi đối tượng máy chủ, cho phép nhà phát triển chèn mã giữa các đối tượng máy khách và máy chủ tùy thuộc vào các tiêu chí khác nhau.

Trong ví dụ này, số lần gọi phương thức trong chương trình demo không được hiển thị. Tuy nhiên, bạn có thể kiểm tra xem proxy có phải là một phiên bản của SomeCountingProxy hay không và xuất ra số lượng:

```java
if (proxy instanceof SomeClassCountingProxy) {
  System.out.println(((SomeClassCountingProxy)proxy).getInvocationCount( ));
}
```

Mã này sẽ cho phép bạn in số lượng nếu proxy là proxy đếm hoặc chỉ cần bỏ qua nếu đó là proxy không đếm.

### 19.2.3 Dynamic Proxies

Trong khi lập trình proxy cho các đối tượng, bạn có thể nhận thấy mình đang làm điều tương tự lặp đi lặp lại trong một lớp proxy. Ví dụ: đối với mỗi triển khai mà bạn muốn có proxy đếm, bạn sẽ phải sao chép tất cả công việc mà bạn đã thực hiện cho mỗi triển khai mới. Vấn đề với điều này là tất cả các lớp proxy có mã trùng lặp này rất khó bảo trì. Nếu proxy của bạn có 1.000 dòng mã mạng và lỗi được tìm thấy chỉ trong một dòng, bạn sẽ phải nhớ thay đổi dòng đó trong mỗi bit mã trùng lặp, trong suốt hàng chục hoặc thậm chí hàng trăm proxy bổ sung.

Những gì bạn cần là một cách triển khai mã đếm phương thức ở một vị trí và một cách để sử dụng lại nó cho bất kỳ đối tượng nào.

Proxy động có thể làm điều này bằng cách sử dụng phản xạ. Proxy động khác với proxy tĩnh ở chỗ chúng không tồn tại tại thời điểm biên dịch. Thay vào đó, chúng được tạo ra trong thời gian chạy bởi JDK và sau đó được cung cấp cho người dùng. Điều này cho phép nhà máy liên kết một đối tượng với nhiều loại triển khai khác nhau mà không cần sao chép hoặc sao chép mã

#### 19.2.3.1 Invocation handlers

Khi viết một proxy động, nhiệm vụ chính của lập trình viên là viết một đối tượng được gọi là trình xử lý lệnh gọi, đối tượng này thực hiện giao diện InvocationHandler từ gói java.lang.reflect. Một trình xử lý lời gọi chặn các cuộc gọi đến việc triển khai, thực hiện một số logic lập trình, và sau đó chuyển yêu cầu đến việc thực hiện. Khi phương thức triển khai trả về, trình xử lý lời gọi trả về kết quả của nó

Ví dụ 19-7 cho thấy một trình xử lý lệnh gọi thực hiện đếm lệnh gọi phương thức của bạn.

```java
// ex 19-7

public class MethodCountingHandler implements InvocationHandler {

  private final Object impl;
  private int invocationCount = 0;

  public MethodCountingHandler(final Object impl) {
    this.impl = impl;
  }

  public int getInvocationCount() {
    return invocationCount;
  }

  public Object invoke(Object proxy, Method meth, Object[] args) throws Throwable {
    try {
      this.invocationCount++;
      Object result = meth.invoke(impl, args);
      return result;
    } catch (final InvocationTargetException ex) {
      throw ex.getTargetException( );
    }

  }
}
```

Trình xử lý lời gọi này cung cấp chức năng tương tự như proxy tĩnh. Tuy nhiên, nó sử dụng reflection  để thực hiện công việc. Khi người dùng thực thi một phương thức trên proxy, trình xử lý lời gọi được gọi thay vì thực thi. Bên trong trình xử lý lệnh gọi, hãy chèn mã để tăng biến số invocationCount và sau đó chuyển tiếp lệnh gọi tới việc triển khai bằng cách sử dụng phương thức invoke() trên đối tượng Method. Khi quá trình gọi hoàn tất, việc triển khai sẽ trả về một giá trị cho trình xử lý. Sau đó, bạn chuyển lại giá trị đó cho người gọi.

Mã bên trong phương thức invoke() có thể thực hiện nhiều việc khác nhau. Trong ví dụ này, bạn chỉ cần đếm các lệnh gọi của các phương thức. Tuy nhiên, bạn có thể viết một trình xử lý lời gọi sẽ thực hiện kiểm tra bảo mật hoặc thậm chí triển khai giao thức IIOP để gửi các cuộc gọi phương thức qua mạng.

#### 19.2.3.2 sinh proxy classes

Viết một trình xử lý lời gọi chỉ là bước đầu tiên để tạo proxy động. Khi bạn có trình xử lý lời gọi, bạn phải tạo proxy cho người dùng. Hơn nữa, theo mẫu thiết kế proxy, bạn phải đảm bảo rằng proxy trông giống như việc triển khai; người dùng không nên biết về sự khác biệt giữa proxy và việc triển khai.

Bạn có thể thực hiện việc này bằng cách sử dụng lớp java.lang.reflect.Proxy kết hợp với nhà máy proxy của bạn. Phương pháp nhà máy kết quả được hiển thị ở đây:

```java
public class SomeClassFactory {

  public static final SomeClass getDynamicSomeClassProxy() {
    SomeClassImpl impl = new SomeClassImpl(System.getProperty("user.name"));
    InvocationHandler handler = new MethodCountingHandler(impl);
    Class[] interfaces = new Class[] { SomeClass.class };
    ClassLoader loader = SomeClassFactory.class.getClassLoader();
    SomeClass proxy = (SomeClass)Proxy.newProxyInstance(loader, interfaces, handler);
    return proxy;
  }
}
```

SomeClass là một giao diện được triển khai bởi quá trình triển khai thực tế, có tên là SomeClassImpl. Điều này cho phép bạn yêu cầu lớp Proxy tạo một proxy mới thực hiện giao diện SomeClass và sử dụng trình xử lý lệnh gọi.

Lớp Proxy đóng một vai trò quan trọng trong việc tạo và quản lý các lớp proxy mới trong máy ảo. Proxy có được sức mạnh to lớn chỉ từ bốn methods.

```java
getInvocationHandler()
```

Phương thức này cung cấp cho người gọi một tham chiếu đến trình xử lý lệnh gọi được sử dụng trong proxy được cung cấp dưới dạng tham số

```java
getProxyClass()
```

Đây là phương thức chính trong việc tạo lớp proxy thực tế. Nó lấy một mảng các giao diện và một trình nạp lớp làm đối số và tạo mã byte cho lớp proxy trong thời gian chạy. Sau khi lớp được tạo, nó có thể được khởi tạo và sử dụng. Sau đó, nó được lưu vào bộ nhớ đệm và trả về vào lần sau khi phương thức được gọi với cùng các tham số. Vì việc tạo lớp proxy tương đối chậm, điều này cải thiện đáng kể hiệu suất của proxy.

```java
isProxyClass()
```

Phương thức này cho bạn biết liệu lớp proxy có được tạo động hay không. Nó được sử dụng bởi công cụ bảo mật để xác minh rằng lớp đó là một lớp proxy.

```java
newProxyInstance()
```

Phương thức này là một lối tắt để gọi getProxyClass() và sau đó gọi newInstance() trên lớp kết quả.

Các lớp proxy đã tạo bắt buộc phải tuân theo các quy tắc sau:

- Tất cả các lớp proxy được khai báo là public và final.
- Tất cả các lớp proxy đều mở rộng lớp java.lang.reflect.Proxy.
- Khi một lớp proxy được định nghĩa, nó sẽ triển khai các giao diện được nêu theo thứ tự giống như chúng trong mảng

#### 19.2.3.3 Sử dụng dynamic proxies

```java
public class DemoDynamicProxy {

  public static final void main(final String[] args) {
    SomeClass proxy = SomeClassFactory.getDynamicSomeClassProxy( );
    proxy.someMethod( );
    proxy.someOtherMethod("Our Proxy works!");
  }  
}
```

Sử dụng lớp proxy hầu như giống với việc sử dụng các lớp proxy tĩnh. Tuy nhiên, có một vài điểm khác biệt quan trọng. Đầu tiên, việc triển khai proxy được tạo ra bởi lớp Proxy. Thứ hai, để có được số lượng lệnh gọi phương thức của proxy, bạn sẽ phải tìm nạp bộ xử lý lệnh gọi và sau đó yêu cầu bộ xử lý lệnh gọi về số lượng:

```java
InvocationHandler handler = Proxy.getInvocationHandler(proxy);
if (handler instanceof MethodCountingHandler) {
  System.out.println(((MethodCountingHandler)handler).getInvocationCount( ));
}
```

Phương thức Proxy.getInvocationHandler() được sử dụng để tìm nạp trình xử lý lời gọi từ proxy. Sau đó, bạn đảm bảo rằng trình xử lý trên thực tế là MethodCountingHandler; hãy nhớ rằng có thể sử dụng bất kỳ trình xử lý lời kêu gọi nào. Cuối cùng, xuất ra số lượng lệnh gọi.
