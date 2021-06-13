# Chương 9: Các khái niêm Mutable và Immutable

## 9.1 Immutable

Immutable Object: khi khởi tạo 1 đối tượng, thì trạng thái của tối tượng đó không thể thay đổi được sau khi việc khởi tạo đối tượng thành công. (Điều này có nghĩa là, bạn chỉ có thể get mà không thể set).

Một chú ý đặc biệt ở đây: kiểu Immutable Object là một kiểu an toàn tức là bạn có thể chuyển các tham chiếu của họ ở khắp nơi mà không phải lo lắng về việc phá vỡ sự đóng gói hoặc an toàn luồng.

Đơn giản như sau:

```java
    public static void main(String args[]) {
        String hello = "hello";
        printString(hello);
        System.out.println(hello);
    }

    private static void printString(String hello) {
        hello = hello + " haitv";
        System.out.println("in function : "+hello );
    }
```

Kết quả sau cùng là `hello` không thay đổi

## 9.2 Mutable

Mutable Object: Khi khởi tạo 1 đối tượng, tức ta có 1 tham chiếu tới 1 thể hiện của 1 lớp, thì trạng thái của đối tượng có thể thay đổi được sau khi việc khởi tạo đối tượng thành công.

xet ví dụ sau:

```java
    public static void main(String args[]) {
        List<String> list = new ArrayList<String>();
        list.add("a");
        list.add("abc_abc");
        list.add("abc_def");
        list.add("def");
        list.add("haitv");
        printList(list);
        System.out.println("==========================================");
        list.forEach(System.out::println);
    }

private static void printList(List<String> list) {
    for (int i =0; i< list.size();i++) {
        if(list.get(i).equals("abc_abc")) {
            list.add("omdz");
        }
    }
    list.forEach(System.out::println);
}
```

Kết qua nhạn được là `list` bị thay đổi khi ra khỏi hàm `printList`

## 9.3 Các lưu ý về mutable và immutable trong java

1. Các loại kiểu dữ liệu Immutable trong java

    Ở đây tôi chỉ xem set các kiểu dữ liệu thường gặp\
    Kiểu String, Các kiểu dữ liệu nguyên mẫu ( int , long, double, float, ...), BigInteger.

    Hoặc các class mà bạn tạo ra mà khi khởi tạo bạn chỉ có thể lấy dữ liệu ra nhưng không thể thay đổi dữ liệu bên trong nó.

2. Các loại kiểu dữ liệu Immutable trong java

    StringBuffer, StringBuilder, Integer, Float và nhóm Collection...

3. Lưu ý về mutable và immutable trong java

    Lưu ý khi truyền params truyền vào hàm và đặc biệt phải lưu ý tới sự thay đổi của nó trong hàm.

    Hãy nhìn ví dụ dưới đây:

    ```java
        List<String> languagePrograms =  new ArrayList<String>();
        languagePrograms.add("Python");
        languagePrograms.add("C++");
        languagePrograms.add("javascript");
        
        String aNewLearnLanguage = "Java";
        String bNewLearnLanguage = "C#";
        
        List<String> ACanWorkWithLanguages = getLanguageUserWorking(aNewLearnLanguage, languagePrograms );
        
        List<String> BCanWorkWithLanguages = getLanguageUserWorking(bNewLearnLanguage, languagePrograms );
        //(1)
        ...

        private List<String> getLanguageUserWorking(String learnLanguage, List<String> languagePrograms) {

            if(languagePrograms.contains(learnLanguage)){
                return languagePrograms;
            }

            languagePrograms.add(learnLanguage);

            return languagePrograms;
        }
    ```

    Xét ngữ cảnh, 2 nhân viên A và B cùng dùng được các ngôn ngữ giống nhau. Sau một thời gian thì nhân viên A học thêm java và nhân viên B học C#. Viết hàm đưa ra các danh sách các ngôn ngữ lập trình của từng nhân viên.

    Nếu nhìn trực quan thì ta có thể thấy hàm getLanguageUserWorking có vẻ xử lý logic đúng rồi.

    Tuy nhiên nếu nhìn kĩ về mặt kiểu dữ liệu đầu vào của hàm getLanguageUserWorking ta có thể thấy rằng languagePrograms là một kiểu mutable (có thể thay đổi). Như vậy, Thì hàm getLanguageUserWorking sai. Có một lưu ý nữa trong phần này. languagePrograms là một List bản chất nó sẽ chứa địa chỉ mà nó sẽ truy cập tới cùng dữ liệu. do vậy nếu như ta trả **`return languagePrograms`** thì tại vị trí **`(1)`** `ACanWorkWithLanguages` và `BCanWorkWithLanguages` có giá trị là như nhau.

    hàm này được viết lại như sau.

    ```java
        private List<String> getLanguageUserWorking(String learnLanguage, List<String> languagePrograms) {
            List<String> clonedLanguagePrograms = new ArrayList<String>(languagePrograms);
            if (clonedLanguagePrograms.contains(learnLanguage)) {
                return clonedLanguagePrograms;
            }
            clonedLanguagePrograms.add(learnLanguage);
            return clonedLanguagePrograms;
        }
    ```

## 9.4 Phá vỡ tính bất biến (Immutable)

Ở đây tôi sẽ tạo một lớp bất biến như sau.

```java
public class ImmutablePerson {

  private String firstName;
  private String lastName;
  private int age;

  public ImmutablePerson(final String firstName, final String lastName, final int age) {
    if (firstName == null) {
      throw new NullPointerException("firstName"); 
    }
    if (lastName == null) {
      throw new NullPointerException("lastName"); 
    }
    this.age = age;
    this.firstName = firstName;
    this.lastName = lastName;
  }

  public int getAge( ) {
    return age;
  }

  public String getFirstName( ) {
    return firstName;
  }

  public String getLastName( ) {
    return lastName;
  }
}
```

Thông thường bạn sẽ để lớp như thế này. và theo một cách bình thường thì có vẻ như chúng ta sẽ không thay đổi được các giá trị của các thuộc tính sau khi đã khởi tạo. Tuy nhiên vẫn có cách để can thiệp vào việc thay dổi giá trị của nó đó là dùng kĩ thuật reflection, đầu tiên là vô hiệu hóa bảo vệ truy cập cảu các biên này, và thay đổi giá trị của chúng.

Để bảo vệ các biên này bạn có thể tọa một lớp bất biến như sau:

```java
public class ImmutablePerson {

  private final String firstName;
  private final String lastName;
  private final int age;

  public ImmutablePerson(final String firstName, final String lastName, final int age) {
    if (firstName == null) {
      throw new NullPointerException("firstName"); 
    }
    if (lastName == null) {
      throw new NullPointerException("lastName"); 
    }
    this.age = age;
    this.firstName = firstName;
    this.lastName = lastName;
  }

  public int getAge( ) {
    return age;
  }

  public String getFirstName( ) {
    return firstName;
  }

  public String getLastName( ) {
    return lastName;
  }
}
```

Dùng final sẽ hạn chế được rất nhiều các kĩ thuật reflection.

## 9.5 Tổng hợp

Trong chương này cần chú ý 2 điều:
    - Chú ý tới các biến Mutable khi truyền vào hàm.
    - Hạn chế tối đa trả ra biến là biến params Mutable.
    - Các lớp Collction thì biến đó chỉ lưu địa chỉ chứ không lưu dữ liệu.
    - Các lớp bất biến thì với các thuộc tính của nó hãy để thêm từ khóa final.
