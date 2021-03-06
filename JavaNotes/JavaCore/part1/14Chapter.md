# Chương 14 Bàn luận về Exception

Trong chương này tôi sẽ không nói quá sau về Exception.

## 14.1 Hai loại exeption

có 2 kiểu ngoại lệ trong Java là Exception và RuntimeException.

kiểu Exception thì bắt buộc phải bắt ngoại lệ hoặc ném ngoại lệ ra ngoài trong khi đó RuntimeException thì không

Ở đây tôi sẽ nói về 2 kĩ thuật mà chúng ta hay gặp phải
một là kĩ thuật mặt nạ exeption 2 là kĩ thuật bỏ qua exception

đầu tiên xem xét ví dụ như sau

```java
public void reflectionMethod2( ) throws MyCustomException {

    try {
      Class.forName("oreilly.hcj.bankdata.Gender");
      //  . . . some code
      Method meth = Gender.class.getDeclaredMethod("getName", null);

      meth.invoke(Gender.MALE, null);

    } catch (final NoSuchMethodException ex) {
      throw new RuntimeException(ex);
    } catch (final IllegalAccessException ex) {
      throw new RuntimeException(ex);
    } catch (final InvocationTargetException ex) {
      throw new RuntimeException(ex);
    } catch (final ClassNotFoundException ex) {
      throw new RuntimeException(ex);
    }
  }
```

`kĩ thuật mặt nạ exceptions:`

như bạn đã thấy thì nó có quá nhiều loại exception. và hay lưu ý rằng các exception này là các loại ít gặp và liên quan đến cấu hình. vậy kĩ thuật mặt nạ của nó sẽ được xử lý như sau:

```java
public void reflectionMethod3( ) throws MyCustomException {
    try {
      Class.forName("oreilly.hcj.bankdata.Gender");
      //  . . . some code
      Method meth = Gender.class.getDeclaredMethod("getName", null);
      meth.invoke(Gender.MALE, null);
    } catch (final Exception ex) {
      if (ex instanceof MyCustomException) {
        throw (MyCustomException)ex;
      }
      throw new RuntimeException(ex);
    } 
  }
```

Về cơ bản là code sẽ gọn hơn.tuy nhiên theo quan điểm về mặt bảo trì thì nó sẽ không quá khủng khiếp. Vì nó che giấu đi hiệu quả của việc ném tất cả các loại exception. Tối nghĩ giải pháp này cũng là một ý giapr pháp hay tuy nhiên cần phải làm rõ thêm với các loại runtime exception. tôi lấy ví dụ như sau. ParseException là một ngoại lệ khi mà người dùng nhạp sai định dạng thời gian. Đáng lẽ ra nó sẽ là một ngoại lệ logic (ngoại lệ liên quan đến logic nghiệp vụ), vì cần phải có để báo cho nguwoif gọi phương thức biết đã xảy ra lỗi format thời gian. nhưng nếu áp dụng một cách máy móc giapr pháp này thì vô tình ta đã biến logic nghiệp vụ thành RuntimeException.

`Kĩ thuật bỏ qua exceptions:`

xem ví dụ dưới

```java
public void reflectionMethod3( ) throws MyCustomException {
    try {
      Class.forName("oreilly.hcj.bankdata.Gender");
      //  . . . some code
      Method meth = Gender.class.getDeclaredMethod("getName", null);
      meth.invoke(Gender.MALE, null);
    } catch (final Exception ex) {
      if (ex instanceof MyCustomException) {
        throw (MyCustomException)ex;
      }
    } 
  }
```

Ở đây rõ ràng tất cả các runtime exception đã bị bỏ qua. tôi thấy đây là điều tồi tệ. nó bỏ qu tất cả các lỗi phát sinh kể cả lỗi cấu hình. theo quan điểm maintain thì nó là một điều cực kì tồi tệ và có thể nói là khủng khiếp.

Tôi vẫn thấy được vẫn có nhiều người có vẻ như không quan tâm đến nó. hoặc họ mở khối try-catch như lại để trống không xử lý gì trong khối catch. Theo tôi nó là nợ kĩ thuật và không sớm thì muộn bạn cũng phải trả món nợ này nếu như làm lâu dài.

ví dụ cho điều đó :

```java
try {
    Class.forName(packageList[idx] + "MyClass");
} catch (final ClassNotFoundException ex) {
    // not an error. 
}
```

tôi nghĩ điều này không thực sự tốt. nên trả ra một RuntimeException.

## 14.2 Khối Finally

Tại sao tôi lại nhắc đến khối finally này. Bình thường ta có cấu trúc try-catch-finally. Tuy nhiên nhiều người đã quên mất chức năng của nó và có vể như cũng chẳng bao giờ dùng.

xet ví dụ dưới

```java
public int getValue(final Connection conn, final String sql) throws SQLException {
    int result = 0;
    Statement stmt = null;
    ResultSet rs = null;

    try {
      stmt = conn.createStatement( );
      rs = stmt.executeQuery(sql);
      if (rs.next( )) {
        result = rs.getInt(1);
      }

      if (rs != null) {
        rs.close( );
      }
      if (stmt != null) {
        stmt.close( );
      }

    } catch (final SQLException ex) {
      throw ex;
    } 
    return result;
  }
```

xet ví dụ trên tôi thấy có 2 vấn đề và nó cũng là các vấn đề mà nhiều nguời mắc phải.

Vấn đề thứ nhất: khi đóng ResultSet và Statement: bạn đã tính đến việc trước khi lệnh đóng các kết nối thì nó phát sinh exception và khi đó các kết nối này vẫn bị giữ lại và chúng sẽ không bị đóng trong một thời gian dài. Điều này làm tốn tài ngyên hệ thống.

Vấn đề thứ hai: đó là khối catch bạn có thấy là vừa bắt vừa ném nó ra trong khi có cần thiết phải làm như vật không vì nó đã có `throws SQLException` ở đầu hàm.

theo như vấn đề thứ 2 thì ta sẽ bỏ catch. vậy khối try này có cần không? câu trả lời là không vì nó sẽ gặp vấn đề thứ nhất. Vậy ta có một đoạn code hoàn thiện như dưới đây

```java
public int getValue(final Connection conn, final String sql) throws SQLException {
    int result = 0;
    Statement stmt = null;
    ResultSet rs = null;

    try {
      stmt = conn.createStatement( );
      rs = stmt.executeQuery(sql);
      if (rs.next( )) {
        result = rs.getInt(1);
      }

    } finally {
      if (rs != null) {
        rs.close( );
      }
      if (stmt != null) {
        stmt.close( );
      }
    } 
    return result;
  }
```

## 14.3 Bẫy Execption

Các ngoại lệ cung cấp cho bạn sức mạnh đáng kể trong khả năng gỡ lỗi và xử lý lỗi của bạn. Tuy nhiên chúng cũng có thể tọa ra nhưng bug mà bạn có thể mất cả giờ để fix nó.

Xét vi dụ dưới đây:

```java
public class ExceptionalTraps implements TableModel {
  private ArrayList data; 

  private String sql;

  public int loadData(final Connection conn, final String sql) throws SQLException {
    int result = 0;
    Statement stmt = null;
    ResultSet rs = null;
    Object[] record = null;
    this.data = new ArrayList( );

    try {
      this.sql = sql;
      stmt = conn.createStatement( );
      rs = stmt.executeQuery(sql);
      int idx = 0;
      int columnCount = rs.getMetaData( ).getColumnCount( );

      while (rs.next( )) {
        record = new Object[columnCount];
        for (idx = 0; idx < columnCount; idx++) {
          record[idx] = rs.getObject(idx);          
        }
        data.add(record);
      }
    } finally {
      if (rs != null) {
        rs.close( );
      }
      if (stmt != null) {
        stmt.close( );
      }
    }
    return result;
  }

  public void refresh(final Connection conn) throws SQLException {
    loadData(conn, this.sql);
  }
}
```

vấn đề đặt ra ở đây là `data.add(record)` trong th bình thường thì nó hoàn toàn đúng, tuy nhiên trong một trường hợp nào đó mà khi add đến phân fthuwr thứ 100 thì mới phát sinh exception. Như vậy, thì vừa phát sinh exception vừa có dữ liệu thiếu. và nó  thực sự tai hại khi nguwoif gọi dùng dữ liệu thiếu này để sử dụng như là dữ liệu đầy đủ. trong th này tôi nghĩ nên dùng một biến result temp. nó sẽ được gán lại vào data trong trường hợp bình thường.

```java
public class ExceptionalTraps implements TableModel {
  private ArrayList data; 

  private String sql;

  public int loadData(final Connection conn, final String sql) throws SQLException {
    int result = 0;
    Statement stmt = null;
    ResultSet rs = null;
    Object[] record = null;
    ArrayList temp = new ArrayList( );

    try {
      stmt = conn.createStatement( );
      rs = stmt.executeQuery(sql);
      int idx = 0;
      int columnCount = rs.getMetaData( ).getColumnCount( );

      while (rs.next( )) {
        record = new Object[columnCount];
        for (idx = 0; idx < columnCount; idx++) {
          record[idx] = rs.getObject(idx);          
        }
        temp.add(record);
      }
    } finally {
      if (rs != null) {
        rs.close( );
      }
      if (stmt != null) {
        stmt.close( );
      }
    }

    this.data = temp;
    this.sql = sql;

    return result;
  }

  public void refresh(final Connection conn) throws SQLException {
    loadData(conn, this.sql);
  }
}
```

Hỏng dữ liệu không phải lúc nào cũng phát sinh mà nó phát sinh một cách bất thình lình và hãy luôn cảnh giác với nó.

Xét thêm ví dụ dưới đây:

```java
public class ExceptionalTraps implements TableModel {

  public void processOrder(final Object order, final Customer customer) {
    this.processedOrders.add(order);
    doOrderProcessing(); 
    billCreditCard(customer); 
  }
}
```

nhìn có vẻ như nó không ném ra exception và cũng có vẻ như chẳng có vấn đề gì phát sinh.

Tuy nhiên nó cũng có thể có lỗi tiềm tàng. hãy giả sử sau khi add đơn hàng vào biến instance thì billCreditCard lại ném ra một **NullPointerException** do customer là null. tôi lấy ví dụ là function billCreditCard như sau để dẽ hình dung.

```java
public void billCreditCard(final Customer customer) {
    if (customer == null) {
      throw new NullPointerException( );
    }
    System.err.println(customer);
  }
```

rõ ràng là đơn này đã không xử lý được như vẫn được add vào đơn đặt hàng. đó là một điều vô lí. Theo tôi nó nên được viết lại như sau thì sẽ hợp lý hơn

```java
public class ExceptionalTraps implements TableModel {

  public void processOrder(final Object order, final Customer customer) {
    doOrderProcessing(); 
    billCreditCard(customer);
    this.processedOrders.add(order);
  }
}
```

Ở đây tôi muốn nói đến một quy tắc ngầm cần được tuân thủ. **`Các biến instance nên được gán giá trị sau cùng nhất có thể trong một function`**

Tuy nhiên, không phải lúc nào nguyên tắc này cũng đúng xét cho ví dụ dưới đây:

```java
public void setSomeProperty(final String someProperty) {

    final String oldSomeProperty = this.someProperty;
    this.someProperty = someProperty;
    propertyChangeSupport.firePropertyChange("someProperty", oldSomeProperty, someProperty);

  }
```

Trong ví dụ này các biến instance phải được gán trước khi xử lý. tuy nhiên thì nó cũng sẽ có thể gặp vấn dề ở trên. người ta sẽ áp dụng kỹ thuật tường lửa exception (bản chất là cho một thăng try catch ở ngoài để tóm cái runtime exception này để ko làm hỏng trương trình hiện tại). code như dưới :

```java
public void setSomeProperty2(final String someProperty) {

    final String oldSomeProperty = this.someProperty;
    this.someProperty = someProperty;
    try {
      propertyChangeSupport.firePropertyChange("someProperty", oldSomeProperty, someProperty);
    } catch (final Exception ex) {
      LOGGER.error("Exception in Listener", ex);
    }
  }
```

tuy nhiên kĩ thuật này chỉ dành cho việc nghe thụ động như PropertyListenerSupport. còn nó sẽ thực sự không phù hợp với trường hợp các biến instance sẽ phải hủy bỏ thay đổi (rollback) khi có một runtime exception phát sinh.

## 14.4 Tổng kết

Người đọc tự ngẫm
