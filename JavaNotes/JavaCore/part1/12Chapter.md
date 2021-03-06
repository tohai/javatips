# Chương 12 : bàn luận về từ khóa final trong java

## 12.1 Từ khóa final trong constant

điều này khá cơ bản ta hay gặp người ta khai báo một hằng số như ví dụ dưới đây:

```java
    public final static double PI = 3.141;
```

Nó chả có gì gọi là đặc biệt phải không ? ta sẽ đặt thêm 1 câu hỏi như sau khi nào ta cần một biến constant?

Đó là khi có nhiều chỗ sử dụng cùng một biến cụ thể và ta cần sự linh động trong code nó.

Ta lấy ví dụ như sau:

```java
public class FinalConstants {
    public static class CircleTools {
        public double getCircleArea(final double radius) {
            return (Math.pow(radius, 2) * 3.141);
        }
        public double getCircleCircumference(final double radius) {
            return ((radius * 2) * 3.141);
        }
        
    }
}
```

Ở đây tôi lấy ví dụ về bài toán tính chuvi, diện tích hình tròn

Nhìn trên ta thừa sức để hiểu con số 3.141 là số PI, tuy nhiên ở đây tôi lại có một ý định như thế này. tôi không muốn số pi của tôi là 3 con số sau thập phân nữa . mà giờ tôi muốn nó là 5 con số sau thập phân chẳng hạn 3.14159. Khi đó bạn sẽ phải đổi tất cả các số 3.141 thành 3.14159 vâng và hay cảm thấy may mắn là ở đây chỉ có 2 vị trí cần sửa bạn có thể sửa một cách dễ dàng. Tuy nhiên hay thử hình dung một chương trình có khoảng 100 vị trí như vậy thì sao ? tai họa rồi nhỉ ?
và khi đó ta cần một biến constant, và khi tôi cần sửa đổi tôi chỉ cần sửa đúng 1 nơi như ví dụ dưới

```java
public class FinalConstants {
    public static class CircleTools {

        private final static double PI = 3.141;

        public double getCircleArea(final double radius) {
            return (Math.pow(radius, 2) * PI);
        }
        public double getCircleCircumference(final double radius) {
            return ((radius * 2) * PI);
        }
        
    }
}
```

Rồi khi muốn sửa dổi ta chỉ cần thay trực tiếp `PI = 3.141` thành `PI = 3.14159` thật dễ dàng và nhanh gọn phải khổng.

Tôi có thêm một lưu ý về nhưng người dùng biên dịch ctrinhf java bằng tay. khi bạn thay đổi một biến final thì bạn phải biên dịch toàn bộ code có sử dụng tới nó

ví dụ như sau

```java
public class PictConstants {
      public final static double PI = 3.141;
}

public static class CircleTools {

    public double getCircleArea(final double radius) {
        return (Math.pow(radius, 2) * PictConstants.PI);
    }
    public double getCircleCircumference(final double radius) {
        return ((radius * 2) * PictConstants.PI);
    }
    
}

```

khi class PictConstants thay đổi PI thì ta cũng buộc phải biên dịch lại class `CircleTools` nếu không PI ở CircleTools sẽ mang giá trị trước khi nó thay đổi. tôi sẽ lấy lí dụ thế này cho dễ hiểu

ban đầu PI trong PictConstants có giá trin là 3.141 khi biên dịch và tính nó cũng bình thường thôi.chả có vấn đề phát sinh ở đây cả. Sau đó tôi thay giá trị này bằng 3.14159. và biên dịch lớp PictConstants mà không biên dịch lớp CircleTools.

khi đó nếu tôi dùng lớp CircleTools để tính bán kính, diện tích bạn sẽ thấy Pi nó vẫn mang giá trị là 3.141. hay ho phải không. Bởi vì cơ chế compile của java nó sẽ thay toàn bộ các hằng số có giá trị vào lớp ngay khi biên dịch.

Trong chính phàn common này tôi sẽ đặt ra thêm một câu hỏi nên dùng hằng số thong file constant common hay hằng số đặc trưng trong từ lớp?

tùy thuộc vào từng trường hợp ta nên đưa các hằng số này vào từng vị trí cụ thể

lấy ví dụ là hằng số pi như trên

Trường hợp tôi chỉ dùng PI cho một class CircleTools thì việc đưa hằng số Pi vào class này là hợp lý vì nó chẳng cần gọi ở đâu khác. Nhưng nếu như ngoài CircleTools mà cũng có vài calsss khác cungxsuwr dụng hằng số PI này thì việc đưa vào common chung là hợp lý hơn như vậy sẽ dẽ dàng cho việc maintain về sau.

## 12.2 Biến final

Nghe có vẻ vô lí phải không? cơ mà nó không phải vô lí đâu nó có lí là đằng khác. bạn chỉ có thể gán cho final trong lần đầu tiên từ đó về sau biến final này sẽ không thể gán được tiếp.

xem ví dụ dưới:

```java
public static String someMethod(final String environmentKey) {

    final String key = "env_" + environmentKey;

    System.out.println("Key is: " + key);

    return (System.getProperty(key));

  }
```

Ta thấy biến `key` được gá một giá trị động khôn gphair là một hằng số mà ta vẫn thường dùng nữa . và hãy nhớ rằng nó sẽ không được phép gán lại trong phạm vi lần sử dụng này.

ví dụ sau tôi sẽ đưa ra việc không gán giá trị ngay vào một biến final

```java
public static String someMethod(final String environmentKey) {

    final String key ;
    
    key = "env_" + environmentKey;

    System.out.println("Key is: " + key);

    return (System.getProperty(key));

  }
```

Nó cũng có tác dụng như giá trị trên. ở đây tôi cũng nói thêm về một vấn đề các biến static nối liên hoàn với nhau:

```java
public static String someMethod(final String environmentKey) {

    final String key ;

    final int sizeofKey = key.length();
    
    key = "env_" + environmentKey;

    System.out.println("Key is: " + key);

    return (System.getProperty(key));

  }
```

Hãy nhìn khởi tạo `sizeOFKey` nó phụ thuộc vào một biến final khác là `key` tuy nhiên ở đây key lại chưa được gán giá trị => dòng này sẽ bị lỗi compile thứ tự đúng phải là:

```java
public static String someMethod(final String environmentKey) {

    final String key ;
    
    key = "env_" + environmentKey;

    final int sizeofKey = key.length();

    System.out.println("Key is: " + key);

    return (System.getProperty(key));

  }
```

## 12.2 final collection

tôi lấy ví dụ dưới đây:

```java
public class Rainbow  {

  public final static Set<Color> VALID_COLORS;

  static {
    VALID_COLORS = new HashSet<Color>();
    VALID_COLORS.add(Color.red);
    VALID_COLORS.add(Color.orange);
    VALID_COLORS.add(Color.yellow);
    VALID_COLORS.add(Color.green);
    VALID_COLORS.add(Color.blue);
    VALID_COLORS.add(Color.decode("#4B0082")); // indigo
    VALID_COLORS.add(Color.decode("#8A2BE2")); // violet
  }
}
```

Mục tiêu của mã này là tạo một `final static Set` để lưu trữ màu cầu vồng. Và ta nuốn dùng nó mà không làm thay đổi nó. Tuy nhiên ở đây nó có thể bị phá vỡ bởi code dưới đây:

```java
public final static void addBlackColor( ) {
    Set<Color> colors = Rainbow.VALID_COLORS;
    colors.add(Color.black); 
}
```

`colors.add(Color.black)` ở đây sai về mặt logic vì chẳng có cái cầu vồng nào mà có màu đen cả. tuy nhiên về mặt trương trình thì nó hoàn toàn không có lỗi gì cả. chúng có thể được sửa dổi xem thêm chương Mutable & Immutable. Để khắc phục lỗi mặt logic này ta làm như sau:

```java
public class Rainbow {

  public final static Set<Color> VALID_RAINBOW_COLORS;

  static {
    Set<Color> rainbowColors  = new HashSet<Color>();
    rainbowColors.add(Color.red);
    rainbowColors.add(Color.orange);
    rainbowColors.add(Color.yellow);
    rainbowColors.add(Color.green);
    rainbowColors.add(Color.blue);
    rainbowColors.add(Color.decode("#4B0082")); // indigo
    rainbowColors.add(Color.decode("#8A2BE2")); // violet

        VALID_RAINBOW_COLORS = Collections.unmodifiableSet(rainbowColors);

  }
}
```

chốt cứng không cho sửa đổi collection. Khi này ta có tác động thay đổi giá trin vào `VALID_RAINBOW_COLORS` thì nó sẽ bắn ra 1 exception.

## 12.3 Final Classes

Final class được định nghĩa là một lớp mà **tự nó ngăn cản bất cứ class khác kế thừa nó**.\
Có 2 cách để tạo một Final Classes:\
Cách 1: đặt từ khóa final vào class:

```java
public final class SomeClass {
  //  . . . Class contents
}
```

Cách 2: khai báo tất cả các hàm tạo của nó là private.

```java
public class SomeClass {

  public final static SOME_INSTANCE = new SomeClass(5);

  private SomeClass(final int value) {
      // TODO: 
  }

}
```

Khi bạn cung cấp cho tất cả các hàm private, bạn đang ngầm khai báo lớp là cuối cùng. Thường, đây không phải là kết quả dự kiến. Trên thực tế, việc bỏ sót từ khóa final trên khai báo lớp sẽ cảnh báo cho bạn biết rằng có điều gì đó không ổn. Lớp ở trên rất có thể cần phải là cuối cùng, trong trường hợp đó, bạn nên sử dụng cụ thể từ khóa final trong khai báo lớp. Nếu bạn không tuân theo quy tắc này, bạn có thể sẽ gây ra một số vấn đề quanh co.

Hầu hết các lớp singleton không nên được khai báo là cuối cùng. Bạn không bao giờ biết những tính năng khác mà người dùng trong lớp của bạn sẽ mơ ước. Tuy nhiên, vì các lớp singleton cần được bảo vệ khỏi sự khởi tạo bên ngoài, bạn không thể đặt hàm tạo ở chế độ public. giải pháp ở đây là dùng **`protected`**

```java
public class Singleton {

  private static final Logger LOGGER = Logger.getLogger(Singleton.class);

  public static Singleton instance = null;

  private final String[] params;

  public static void init(final String[] params) {
    //  . . . do some initialization . . . 
    instance = new Singleton(params);
  }

  protected Singleton(final String[] params) {

    this.params = params;
    if (LOGGER.isDebugEnabled( )) {
      LOGGER.debug(Arrays.asList(this.params).toString( ));
    }
  }
}
```

Vì lớp singleton này có một constructor protected thay vì một constructor private, bạn có thể mở rộng chức năng của nó trong khi bảo vệ nó khỏi việc xây dựng bởi người dùng. Điều này sẽ cho phép bạn mở rộng singleton, như được hiển thị trong Ví dụ dưới

```java
public class ExtendedSingleton extends Singleton {

  private final static int DEFAULT_VALUE = 5;
  private final int value;

  public static void init(final String[] params) {
    instance = new ExtendedSingleton(params, DEFAULT_VALUE);
  }

  public static void init(final String[] params, final int value) {
    instance = new ExtendedSingleton(params, value);
  }

  protected ExtendedSingleton(final String[] params, final int value) {
    super(params);
    this.value = value;
  }
}
```

Kỹ thuật protected constructor không giới hạn ở các lớp có các biến và phương thức thể hiện. Một protected constructor nên được khai báo cho các lớp có bản chất hoàn toàn tĩnh. Mặc dù các lớp này không có thành viên cá thể nào khác, nhưng nếu hơi vô nghĩa, có thể khởi tạo chúng. Việc ngăn chặn sự khởi tạo này trong khi cung cấp khả năng mở rộng chắc chắn là một tài sản cho quá trình phát triển.

## 12.3 Final method

Final method phép bạn tạo một lớp cuối cùng một phần mà không ngăn cản sự kế thừa của nó bởi một lớp khác. ví dụ dưới đây :

```java
public class FinalMethod {
  public final void someMethod() {
      //TODO:
  }

  //other method
}
```

Ở đây hơi trái khoáy một chút về tính trừu tượng. lớp con có thể ghi đè bất cứ một phương thức nào của lớp cha. tuy nhiên các phương thức final là một ngoại lệ. Không thể override.như ví dụ trên : bạn có thể ghi đè bất cứ một phuong thức nào khác ngoài `someMethod()`

Bạn không bao giờ nên tạo một Final method trừ khi nó phải là phương thức cuối cùng. Khi nghi ngờ, hãy để từ khóa cuối cùng khỏi một phương thức. Rốt cuộc, bạn không bao giờ biết các loại biến thể mà người khác dùng lớp của bạn có thể nghĩ ra.

## 12.4 Biên dịch có điều kiện

Trong Phần này tôi sẽ nói về vấn đề dùng final trong biên dịch có điều kiện.\
Biên dịch có điều kiện là một kỹ thuật trong đó các dòng mã không được biên dịch vào tệp lớp dựa trên một điều kiện cụ thể. Điều này có thể được sử dụng để loại bỏ hàng tấn mã gỡ lỗi trong bản dựng sản xuất.

Xem xét đoạn code sau:

```java
public class ConditionalCompile {

    private static final Logger LOGGER = Logger.getLogger(ConditionalCompile.class);

    public static void someMethod( ) {

        // Do some set up code. 
        LOGGER.debug("Set up complete, beginning phases.");

        // do first part. 
        LOGGER.debug("phase1 complete");

        // do second part. 
        LOGGER.debug("phase2 complete");

        // do third part. 
        LOGGER.debug("phase3 complete");

        // do finalization part. 
        LOGGER.debug("phase4 complete");

        // Operation Completed
        LOGGER.debug("All phases completed successfully");
    }
}
```

Nếu bạn giả định rằng có rất nhiều mã trong mỗi giai đoạn của phương pháp, thì việc ghi nhật ký được hiển thị trong ví dụ này có thể rất cần thiết để tìm ra lỗi logic nghiệp vụ. Tuy nhiên, khi bạn triển khai ứng dụng này trong môi trường sản xuất, bạn phải quay lại mã và loại bỏ tất cả việc ghi nhật ký, nếu không phương pháp này sẽ chạy như một con chó ba chân trong cát lún. Ngay cả khi Log4j được đặt ở mức lỗi cao hơn, mọi câu lệnh ghi nhật ký đều yêu cầu một lệnh gọi đến một phương thức khác, tra cứu trong bảng cấu hình, v.v.

Để giảm chi phí, bạn có thể thử tắt tính năng ghi nhật ký bằng một biến:

```java
public class ConditionalCompile {

    private static final Logger LOGGER = Logger.getLogger(ConditionalCompile.class);
    private static boolean doLogging = false;

    public static void someMethodBetter( ) {

        // Do some set up code. 
        if (doLogging) {
            LOGGER.debug("Set up complete, beginning phases.");
        }

        // do first part. 
        if (doLogging) {
            LOGGER.debug("phase1 complete");
        }

        // do second part. 
        if (doLogging) {
            LOGGER.debug("phase2 complete");
        }

        // do third part. 
        if (doLogging) {
            LOGGER.debug("phase3 complete");
        }

        // do finalization part. 
        if (doLogging) {
            LOGGER.debug("phase4 complete");
        }

        // Operation Completed
        if (doLogging) {
            LOGGER.debug("All phases completed successfully");
        }
    }
}
```

Ở đây tôi đang sử dụng biến `static doLogging` quả thật thì LOGGER sẽ không được gọi như vậy thì nó sẽ thay bằng câu lệnh if. tuy nhiên nếu như có vài nghìn câu lệnh if này thì nó cũng là thứ cần phải được tính toán lại.

nếu bạn từng dùng C++ tở đó có #ifdef nó sẽ bỏ qua biên dịch nếu không đạt yêu cầu đặt ra của nó. trong java không có điều này. tuy nhiên từ kháo final cũng có thể cho ra một kết quả tương tự.

```java
public class ConditionalCompile {

    private static final Logger LOGGER = Logger.getLogger(ConditionalCompile.class);
    private final static boolean doLogging = false;

    public static void someMethodBetter( ) {

        // Do some set up code. 
        if (doLogging) {
            LOGGER.debug("Set up complete, beginning phases.");
        }

        // do first part. 
        if (doLogging) {
            LOGGER.debug("phase1 complete");
        }

        // do second part. 
        if (doLogging) {
            LOGGER.debug("phase2 complete");
        }

        // do third part. 
        if (doLogging) {
            LOGGER.debug("phase3 complete");
        }

        // do finalization part. 
        if (doLogging) {
            LOGGER.debug("phase4 complete");
        }

        // Operation Completed
        if (doLogging) {
            LOGGER.debug("All phases completed successfully");
        }
    }
}
```

Hãy nhớ rằng khi biên dịch thì tất cả vị trí doLogging sẽ được thay thế bởi false và khi đó thì nó sẽ thành như thế này. ở đây tôi chỉ lấy một đoạn.

```java
// do finalization part. 
if (false) {
    LOGGER.debug("phase4 complete");
}

// Operation Completed
if (false) {
    LOGGER.debug("All phases completed successfully");
}
```

bạn nghĩ rằng nó sẽ giống như việc đặt biến static trên ? không đâu, các dòng này chúng được gọi là dead code . và chương trình sẽ bỏ qua chúng. và cuối cùng thì code còn lại sẽ như thế này

```java
public class ConditionalCompile {

  private static final Logger LOGGER = Logger.getLogger(ConditionalCompile.class);

  private final static boolean doLogging = false;

  public static void someMethodBetter( ) {

    // Do some set up code. 

    // do first part. 

    // do second part. 

    // do third part. 

    // do finalization part. 

    // Operation Completed

  }

}
```

Đến thời điểm này ta hoàn toàn có thể xác định được việc final doLogging là một biến cần thết. khi đặt là `true` thì sẽ đưa nó thành môi trường cho sản phẩm còn khi đặt nó là `false` thì nó là môi trường phát triển.

Câu hỏi đặt ra là có nên đặt chúng vào lớp constant common đã có sẵn ?\
Câu trả lời là không, bởi vì nó sẽ làm khó kiểm soát được các biên này và trong quá trình thay dổi nó sẽ dẫn đế nhiều thứ cần xem sét.

Vây nên đặt nó ở đâu là hợp lý? \
Cách tốt nhất và đẹp nhất là bạn hãy tạo ra 1 package tên là _development. dấu gạch dưới để thể hiện rằng gói này không thuộc thành phần cảu sản phẩm.

code mẫu

```java
package oreilly.hcj._development;

public final class DevelopmentMode {

  /** Development mode constant for package oreilly.hcj.bankdata. */
  public static final boolean hcj_bankdata = true;

  /** Development mode constant for package oreilly.hcj.bankdata. */
  public static final boolean hcj_collections = true;

  /** Development mode constant for package oreilly.hcj.bankdata. */
  public static final boolean hcj_constants = true;

  /** Development mode constant for package oreilly.hcj.bankdata. */
  public static final boolean hcj_datamodeling = true;

  /** Development mode constant for package oreilly.hcj.bankdata. */
  public static final boolean hcj_exceptions = true;

  /** Development mode constant for package oreilly.hcj.bankdata. */
  public static final boolean hcj_finalstory = true;

  /** Development mode constant for package oreilly.hcj.bankdata. */
  public static final boolean hcj_immutable = true;

  /** Development mode constant for package oreilly.hcj.bankdata. */
  public static final boolean hcj_nested = true;

  /** Development mode constant for package oreilly.hcj.bankdata. */
  public static final boolean hcj_proxies = true;

  /** Development mode constant for package oreilly.hcj.bankdata. */
  public static final boolean hcj_references = true;

  /** Development mode constant for package oreilly.hcj.bankdata. */
  public static final boolean hcj_review = true;

  private DevelopmentMode( ) {
    assert false: "DevelopmentMode is a Singleton.";
  }
}
```

## 12.5 Tổng kết

Bạn chú ý một số điều sau:

1. `Khi nào nên tạo biến final`
2. `Biến final có thể được khởi tạo sau`
3. `Khi nào nên đặt final vào parameter trong function`
4. `Kỹ thuật biên dịch có điều kiện với final`

ok vậy là xong chương này.
