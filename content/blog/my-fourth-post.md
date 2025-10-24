---
title: "Java Phần 3: Kiến Thức Cơ Bản Và Hướng Đối Tượng"
date: 2025-08-22
draft: false
tags: ["Java", "Programming", "Hướng dẫn"]
categories: ["Web Development"]
image: "/image/1-img.jpg"
---

Java là một trong những ngôn ngữ lập trình phổ biến nhất thế giới và là sự lựa chọn tuyệt vời cho người mới bắt đầu học lập trình. <!--more--> Bài viết này sẽ giúp bạn hiểu sâu hơn về các khái niệm quan trọng trong Java.

## 1. Collections Framework

### 1.1. List Interface
```java
// ArrayList - Mảng động
ArrayList<String> danhSach = new ArrayList<>();
danhSach.add("Java");
danhSach.add("Python");

// LinkedList - Danh sách liên kết
LinkedList<Integer> linkedList = new LinkedList<>();
linkedList.addFirst(1);  // Thêm vào đầu
linkedList.addLast(2);   // Thêm vào cuối
```

### 1.2. Set Interface
```java
// HashSet - Tập hợp không trùng lặp
HashSet<String> set = new HashSet<>();
set.add("Java");  // Thêm phần tử
set.contains("Java");  // Kiểm tra tồn tại

// TreeSet - Tập hợp có thứ tự
TreeSet<Integer> treeSet = new TreeSet<>();
treeSet.add(5);
treeSet.add(1);  // Tự động sắp xếp
```

### 1.3. Map Interface
```java
// HashMap - Bảng băm
HashMap<String, Integer> map = new HashMap<>();
map.put("Java", 1995);
map.put("Python", 1991);

// TreeMap - Map có thứ tự
TreeMap<String, Double> grades = new TreeMap<>();
grades.put("Alice", 9.5);
grades.put("Bob", 8.7);
```

## 2. Luồng (Threads) và Đa Luồng

### 2.1. Tạo Thread
```java
// Cách 1: Kế thừa Thread
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread đang chạy");
    }
}

// Cách 2: Implements Runnable
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("Runnable đang chạy");
    }
}
```

### 2.2. Đồng Bộ Hóa (Synchronization)
```java
public class TaiKhoan {
    private int soDu = 0;
    
    // Phương thức đồng bộ
    public synchronized void nap(int tien) {
        soDu += tien;
    }
    
    public synchronized void rut(int tien) {
        if (soDu >= tien) {
            soDu -= tien;
        }
    }
}
```

## 3. Stream API và Lambda Expressions

### 3.1. Lambda Expressions
```java
// Trước Java 8
Comparator<String> comp = new Comparator<String>() {
    @Override
    public int compare(String s1, String s2) {
        return s1.compareTo(s2);
    }
};

// Với Lambda
Comparator<String> comp = (s1, s2) -> s1.compareTo(s2);
```

### 3.2. Stream Operations
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);

// Lọc số chẵn và tính tổng
int sum = numbers.stream()
    .filter(n -> n % 2 == 0)    // Lọc số chẵn
    .mapToInt(Integer::intValue) // Chuyển đổi
    .sum();                      // Tính tổng

// Sắp xếp và chuyển đổi
List<String> sorted = numbers.stream()
    .sorted()
    .map(String::valueOf)
    .collect(Collectors.toList());
```

## 4. Java IO và NIO

### 4.1. Đọc File
```java
// Sử dụng BufferedReader
try (BufferedReader reader = new BufferedReader(new FileReader("file.txt"))) {
    String line;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }
} catch (IOException e) {
    e.printStackTrace();
}

// Sử dụng Files (NIO)
try {
    List<String> lines = Files.readAllLines(Paths.get("file.txt"));
    lines.forEach(System.out::println);
} catch (IOException e) {
    e.printStackTrace();
}
```

### 4.2. Ghi File
```java
// Sử dụng BufferedWriter
try (BufferedWriter writer = new BufferedWriter(new FileWriter("output.txt"))) {
    writer.write("Dòng văn bản mới");
    writer.newLine();
} catch (IOException e) {
    e.printStackTrace();
}

// Sử dụng Files (NIO)
try {
    Files.write(Paths.get("output.txt"), 
                Arrays.asList("Dòng 1", "Dòng 2"), 
                StandardCharsets.UTF_8);
} catch (IOException e) {
    e.printStackTrace();
}
```

## 5. Reflection API

```java
// Lấy thông tin về class
Class<?> clazz = String.class;
Method[] methods = clazz.getDeclaredMethods();

// Tạo đối tượng mới
Constructor<?> constructor = clazz.getConstructor();
Object instance = constructor.newInstance();

// Gọi phương thức
Method method = clazz.getMethod("length");
Object result = method.invoke(instance);
```

## 6. Unit Testing với JUnit

```java
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
}

@Test
public void testAdd() {
    Calculator calc = new Calculator();
    assertEquals(4, calc.add(2, 2));
    assertEquals(0, calc.add(-1, 1));
    assertEquals(-2, calc.add(-1, -1));
}
```

## 7. Design Patterns Phổ Biến

### 7.1. Singleton Pattern
```java
public class Singleton {
    private static Singleton instance;
    
    private Singleton() {}
    
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

### 7.2. Factory Pattern
```java
interface Animal {
    void makeSound();
}

class Dog implements Animal {
    public void makeSound() {
        System.out.println("Gâu gâu!");
    }
}

class Cat implements Animal {
    public void makeSound() {
        System.out.println("Meo meo!");
    }
}

class AnimalFactory {
    public Animal createAnimal(String type) {
        if ("dog".equalsIgnoreCase(type)) {
            return new Dog();
        } else if ("cat".equalsIgnoreCase(type)) {
            return new Cat();
        }
        return null;
    }
}
```

## Kết Luận

Các chủ đề nâng cao trong Java mở ra nhiều khả năng mạnh mẽ cho việc phát triển ứng dụng. Việc nắm vững những kiến thức này sẽ giúp bạn:

- Xây dựng ứng dụng đa luồng hiệu quả
- Quản lý dữ liệu tốt hơn với Collections Framework
- Viết code ngắn gọn và dễ đọc hơn với Lambda và Stream
- Xử lý file và IO một cách linh hoạt
- Kiểm thử ứng dụng chuyên nghiệp
- Áp dụng các mẫu thiết kế phù hợp

Hãy tiếp tục thực hành và áp dụng những kiến thức này vào các dự án thực tế để trở thành một lập trình viên Java chuyên nghiệp!