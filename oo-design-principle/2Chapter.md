# THE OPEN CLOSED PRINCIPLE (OCP)

Nguyên lý này đề cập đến việc một đối tượng phải luôn **MỞ cho việc mở rộng, nhưng ĐÓNG trong việc thay đổi**. Nghĩa là bạn phải làm thế nào để tránh được những thay đổi trong lớp khi có những sự thay đổi ở bên ngoài và vẫn phải đảm bảo tính linh hoạt để đáp ứng sự thay đổi đó. Một cách đơn giản để đạt được điều đó là: **Hãy tách những thành phần dễ thay đổi ra khỏi những phần khó thay đổi.**

Hãy cùng xem xét 1 ví dụ sau: Yêu cầu là chúng ta hãy viết 1 lớp để đảm nhận việc kết nối tới các hệ quản trị cơ sở dữ liệu khác nhau như: SQL Server hoặc MySQL.

Thiết kế ban đầu như sau:

```java
public class SqlServerConnection {
    public void doConnect() {
        System.out.println("Connect to SQL Server");
    }
}

public class MySqlConnection {

    public void doConnect() {
        System.out.println("Connect to MySQL Server");
    }
}

public class ConnectionManager {

    void doSqlServerConnect(SqlServerConnection inConnection) {
        // Let manager class do something before Server is started
        // ...
 
        // Connect to server
        inConnection.doConnect();
 
        // The manager will do something more after Server is stopped
        // ...
    }
 
    void doMySqlConnect(MySqlConnection inConnection) {
        // Let manager class do something before Server is started
        // ...
 
        // Connect to server
        inConnection.doConnect();
 
        // The manager will do something more after Server is stopped
        // ...
    }
}

public class Main {

    public static void main(String[] args) {
        ConnectionManager connectionPool = new ConnectionManager();
 
        connectionPool.doMySqlConnect(new MySqlConnection());
        connectionPool.doSqlServerConnect(new SqlServerConnection());
    }
 
}
```

Và bây giờ là thời điểm của sự thay đổi. Bạn muốn connect tới Oracle server? Đơn giản là tạo ra lớp OracleConnection, sau đó vào lớp ConnectionManager và viết thêm hàm doOracleConnect. Vậy còn PostgreSQL?
Bạn phải thay đổi quá nhiều (thật ra là chỉ tạo thêm 1 class và 1 phương thức, nhưng vẫn là quá nhiều).

Hãy thay đổi 1 chút trong lớp ConnectionManager:

```java
public class ConnectionManager {

    void doConnect(Object inConnection) {
        // Let manager class do something before Server is started
        // ...
 
        // Connect to server
        if(inConnection instanceof SqlServerConnection) {
            ((SqlServerConnection)inConnection).doConnect();
        }
        else if(inConnection instanceof OracleConnection) {
            ((OracleConnection)inConnection).doConnect();
        }
        else if(inConnection instanceof MySqlConnection) {
            ((MySqlConnection)inConnection).doConnect();
        }
 
        // The manager will do something more after Server is stopped
        // ...
    }
}

public class Main {

    public static void main(String[] args) {
        ConnectionManager connectionPool = new ConnectionManager();
 
        connectionPool.doConnect(new MySqlConnection());
        connectionPool.doConnect(new OracleConnection());
        connectionPool.doConnect(new SqlServerConnection());
    }
 
}
```

Bây giờ chúng chỉ còn hàm doConnect trong lớp ConnectionManager (lớp Main cũng khác chút ít). Nhưng vẫn không đảm bảo được việc không phải thay đổi khi có 1 Connection mới được thêm vào.

Hãy cùng xem xét:

1. 3 lớp MySqlConnection, OracleConnection, SqlServerConnection đều có chung một việc là kết nối tới Server tương ứng, và chúng ta có thể gọi chúng là Connection.
2. Hàm doConnect trong lớp ConnectionManager (là hàm rất dễ bị thay đổi khi có một Connection được thêm vào) giúp các Connection connect đến đúng Server của chúng.
3. Bằng cách trừu tượng hóa (abstractionalization) các đối tượng Connection, chúng ta có thể giúp hàm doConnect có thể đối xử với các Connection với chung 1 phương thức.

Ta sẽ xử lý như sau:

1. Chúng ta cần có 1 lớp cơ sở (BASE) gọi là Connection, các lớp Connection cụ thể (MySqlConnection, OracleConnection, SqlServerConnection) sẽ kế thừa từ Connection.
2. Hàm doConnect sẽ làm việc với Connection.
3. Bằng cách trừu tượng hóa phương thức connect, chúng ta sẽ làm cho lớp ConnectionManager đối xử với các lớp cụ thể qua lớp BASE của nó.

```java
public abstract class Connection {
    public abstract void doConnect();
}

public class SqlServerConnection extends Connection {

    public void doConnect() {
        System.out.println("Connect to SQL Server");
    }
}

public class OracleConnection extends Connection {

    public void doConnect() {
        System.out.println("Connect to Oracle Server");
    }
}

public class MySqlConnection extends Connection {
 
    public void doConnect() {
        System.out.println("Connect to MySQL Server");
    }
}

public class ConnectionManager {
 
    /**
     * Do connect to Oracle Server
     * @param inConnection
     */
    void doConnect(Connection inConnection) {
        // Let manager class do something before Server is started
        // ...
 
        // Connect to server
        inConnection.doConnect();
 
        // The manager will do something more after Server is stopped
        // ...
    }
}

public class Main {

    public static void main(String[] args) {
        ConnectionManager connectionPool = new ConnectionManager();
 
        connectionPool.doConnect(new MySqlConnection());
        connectionPool.doConnect(new OracleConnection());
        connectionPool.doConnect(new SqlServerConnection());
    }
}
```

Lớp Connection là lớp abstract (vì nó sẽ không thực hiện 1 việc connect cụ thể đến Server nào), hàm doConnect của nó cũng là abstract và các lớp con MySqlConnection, OracleConnection, SqlServerConnection sẽ thực hiện override hàm doConnect để thực hiện việc connect đến Server tương ứng.
Lớp ConnectionManager bây giờ chỉ làm việc với lớp abstract Connection
Và khi chúng ta muốn kết nối tới PostgreSQL, chỉ cần tạo ra lớp PostgreSQL kế thừa từ Connection và override hàm doConnect để thực hiện việc connect tới PostgreSQL Server.
