---
title: "Java Phần 2: Kiến Thức Cơ Bản Và Hướng Đối Tượng"
date: 2025-08-23
draft: false
tags: ["Java", "Programming", "Hướng dẫn"]
categories: ["Web Development"]
image: "/image/1-img.jpg"
---

Java là một trong những ngôn ngữ lập trình phổ biến nhất thế giới và là sự lựa chọn tuyệt vời cho người mới bắt đầu học lập trình. <!--more--> Bài viết này sẽ hướng dẫn bạn xây dựng một ứng dụng quản lý thư viện hoàn chỉnh bằng Java.

## 1. Thiết Kế Cơ Sở Dữ Liệu

### 1.1. Mô Hình ERD
```plaintext
Sách (ID, Tên, Tác giả, NXB, Năm XB, Số lượng)
   ↑
   | 1:n
Mượn Sách (ID, Mã độc giả, Mã sách, Ngày mượn, Ngày trả)
   |
   ↓ 1:n
Độc Giả (ID, Họ tên, Địa chỉ, Email, SĐT)
```

### 1.2. Cấu Trúc Project
```plaintext
library-management/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   ├── models/
│   │   │   ├── dao/
│   │   │   ├── services/
│   │   │   └── ui/
│   │   └── resources/
│   └── test/
├── pom.xml
└── README.md
```

## 2. Xây Dựng Các Model

### 2.1. Book Class
```java
public class Book {
    private int id;
    private String title;
    private String author;
    private String publisher;
    private int publishYear;
    private int quantity;
    
    // Constructor
    public Book(int id, String title, String author, 
                String publisher, int publishYear, int quantity) {
        this.id = id;
        this.title = title;
        this.author = author;
        this.publisher = publisher;
        this.publishYear = publishYear;
        this.quantity = quantity;
    }
    
    // Getters and Setters
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }
    // ... các getter/setter khác
}
```

### 2.2. Member Class
```java
public class Member {
    private int id;
    private String fullName;
    private String address;
    private String email;
    private String phone;
    
    // Constructor
    public Member(int id, String fullName, String address,
                 String email, String phone) {
        this.id = id;
        this.fullName = fullName;
        this.address = address;
        this.email = email;
        this.phone = phone;
    }
    
    // Getters and Setters
}
```

## 3. Data Access Objects (DAO)

### 3.1. Database Connection
```java
public class DatabaseConnection {
    private static final String URL = "jdbc:mysql://localhost:3306/library";
    private static final String USER = "root";
    private static final String PASSWORD = "password";
    
    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(URL, USER, PASSWORD);
    }
}
```

### 3.2. BookDAO
```java
public class BookDAO {
    public void addBook(Book book) throws SQLException {
        String sql = "INSERT INTO books (title, author, publisher, " +
                    "publish_year, quantity) VALUES (?, ?, ?, ?, ?)";
        
        try (Connection conn = DatabaseConnection.getConnection();
             PreparedStatement pstmt = conn.prepareStatement(sql)) {
            
            pstmt.setString(1, book.getTitle());
            pstmt.setString(2, book.getAuthor());
            pstmt.setString(3, book.getPublisher());
            pstmt.setInt(4, book.getPublishYear());
            pstmt.setInt(5, book.getQuantity());
            
            pstmt.executeUpdate();
        }
    }
    
    public List<Book> searchBooks(String keyword) throws SQLException {
        List<Book> books = new ArrayList<>();
        String sql = "SELECT * FROM books WHERE title LIKE ? OR author LIKE ?";
        
        try (Connection conn = DatabaseConnection.getConnection();
             PreparedStatement pstmt = conn.prepareStatement(sql)) {
            
            pstmt.setString(1, "%" + keyword + "%");
            pstmt.setString(2, "%" + keyword + "%");
            
            ResultSet rs = pstmt.executeQuery();
            while (rs.next()) {
                books.add(new Book(
                    rs.getInt("id"),
                    rs.getString("title"),
                    rs.getString("author"),
                    rs.getString("publisher"),
                    rs.getInt("publish_year"),
                    rs.getInt("quantity")
                ));
            }
        }
        return books;
    }
}
```

## 4. Business Logic Layer

### 4.1. LibraryService
```java
public class LibraryService {
    private BookDAO bookDAO;
    private MemberDAO memberDAO;
    private BorrowingDAO borrowingDAO;
    
    public LibraryService() {
        this.bookDAO = new BookDAO();
        this.memberDAO = new MemberDAO();
        this.borrowingDAO = new BorrowingDAO();
    }
    
    public boolean borrowBook(int memberId, int bookId) 
            throws LibraryException {
        try {
            // Kiểm tra tồn tại sách và độc giả
            Book book = bookDAO.getBookById(bookId);
            Member member = memberDAO.getMemberById(memberId);
            
            if (book == null || member == null) {
                throw new LibraryException("Sách hoặc độc giả không tồn tại");
            }
            
            // Kiểm tra số lượng sách
            if (book.getQuantity() <= 0) {
                throw new LibraryException("Sách đã hết");
            }
            
            // Tạo phiếu mượn
            BorrowingRecord record = new BorrowingRecord(
                memberId, bookId, LocalDate.now());
            
            borrowingDAO.createBorrowingRecord(record);
            book.setQuantity(book.getQuantity() - 1);
            bookDAO.updateBook(book);
            
            return true;
        } catch (SQLException e) {
            throw new LibraryException("Lỗi khi mượn sách: " + e.getMessage());
        }
    }
}
```

## 5. Giao Diện Người Dùng với JavaFX

### 5.1. Main Application
```java
public class LibraryApp extends Application {
    @Override
    public void start(Stage primaryStage) {
        try {
            FXMLLoader loader = new FXMLLoader(
                getClass().getResource("/fxml/main.fxml"));
            Parent root = loader.load();
            
            Scene scene = new Scene(root);
            primaryStage.setTitle("Quản Lý Thư Viện");
            primaryStage.setScene(scene);
            primaryStage.show();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    
    public static void main(String[] args) {
        launch(args);
    }
}
```

### 5.2. Controller
```java
public class MainController {
    @FXML
    private TableView<Book> bookTable;
    @FXML
    private TextField searchField;
    
    private LibraryService libraryService;
    
    @FXML
    public void initialize() {
        libraryService = new LibraryService();
        setupTableColumns();
        loadBooks();
    }
    
    @FXML
    private void handleSearch() {
        String keyword = searchField.getText();
        try {
            List<Book> books = libraryService.searchBooks(keyword);
            bookTable.getItems().setAll(books);
        } catch (LibraryException e) {
            showError("Lỗi tìm kiếm", e.getMessage());
        }
    }
}
```

## 6. Xử Lý Giao Dịch và Logging

### 6.1. Transaction Management
```java
public class TransactionManager {
    public static void executeInTransaction(
            Connection conn, 
            TransactionBlock block) throws SQLException {
        
        boolean autoCommit = conn.getAutoCommit();
        try {
            conn.setAutoCommit(false);
            block.execute();
            conn.commit();
        } catch (SQLException e) {
            conn.rollback();
            throw e;
        } finally {
            conn.setAutoCommit(autoCommit);
        }
    }
}
```

### 6.2. Logging với Log4j
```java
public class LibraryLogger {
    private static final Logger logger = 
        LogManager.getLogger(LibraryLogger.class);
    
    public static void logInfo(String message) {
        logger.info(message);
    }
    
    public static void logError(String message, Throwable t) {
        logger.error(message, t);
    }
}
```

## 7. Testing

### 7.1. Unit Testing với JUnit
```java
public class BookServiceTest {
    private BookService bookService;
    private BookDAO mockBookDAO;
    
    @Before
    public void setup() {
        mockBookDAO = mock(BookDAO.class);
        bookService = new BookService(mockBookDAO);
    }
    
    @Test
    public void testAddBook() throws Exception {
        Book book = new Book(1, "Test Book", "Test Author", 
                           "Test Publisher", 2023, 5);
        
        when(mockBookDAO.addBook(book)).thenReturn(true);
        
        assertTrue(bookService.addBook(book));
        verify(mockBookDAO).addBook(book);
    }
}
```

## 8. Triển Khai và Bảo Trì

### 8.1. Đóng Gói Ứng Dụng
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-assembly-plugin</artifactId>
            <configuration>
                <archive>
                    <manifest>
                        <mainClass>com.library.LibraryApp</mainClass>
                    </manifest>
                </archive>
                <descriptorRefs>
                    <descriptorRef>jar-with-dependencies</descriptorRef>
                </descriptorRefs>
            </configuration>
        </plugin>
    </plugins>
</build>
```

## Kết Luận

Qua việc xây dựng ứng dụng quản lý thư viện, chúng ta đã thực hành nhiều khía cạnh quan trọng trong phát triển ứng dụng Java:

1. Thiết kế cơ sở dữ liệu
2. Xây dựng các lớp model
3. Thao tác với CSDL qua DAO
4. Xử lý logic nghiệp vụ
5. Phát triển giao diện với JavaFX
6. Quản lý giao dịch và logging
7. Kiểm thử ứng dụng
8. Đóng gói và triển khai

Hy vọng bài viết này giúp bạn có cái nhìn tổng quan về quy trình phát triển một ứng dụng Java hoàn chỉnh. Hãy thử áp dụng những kiến thức này vào dự án của riêng bạn!