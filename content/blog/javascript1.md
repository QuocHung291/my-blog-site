---
title: "JavaScript Phần 1: Làm Quen Với Lập Trình Web"
date: 2025-08-20
draft: false
tags: ["JavaScript", "Web Development", "Hướng dẫn"]
categories: ["Web Development"]
image: "/image/img-2.jpg"
---

JavaScript là ngôn ngữ lập trình phổ biến nhất trong thế giới web, được sử dụng rộng rãi để tạo các trang web tương tác và động. <!--more--> Trong bài viết này, chúng ta sẽ bắt đầu hành trình khám phá JavaScript từ những khái niệm cơ bản nhất.

## 1. JavaScript Là Gì?

JavaScript là ngôn ngữ lập trình:
- **Dễ học**: Cú pháp đơn giản, thân thiện với người mới
- **Linh hoạt**: Có thể chạy cả phía client và server
- **Phổ biến**: Được sử dụng bởi 97% các trang web
- **Đa nền tảng**: Chạy trên mọi trình duyệt hiện đại

## 2. Chuẩn Bị Môi Trường

### 2.1. Công Cụ Cần Thiết

1. **Trình duyệt web**:
   - Google Chrome (khuyến nghị)
   - Mozilla Firefox
   - Microsoft Edge

2. **Code Editor**:
   - Visual Studio Code (khuyến nghị)
   - Sublime Text
   - Atom

### 2.2. Tạo File HTML Cơ Bản

```html
<!DOCTYPE html>
<html>
<head>
    <title>Học JavaScript</title>
</head>
<body>
    <h1>Xin chào JavaScript!</h1>
    
    <!-- Thêm JavaScript vào cuối body -->
    <script src="script.js"></script>
</body>
</html>
```

## 3. Cú Pháp Cơ Bản

### 3.1. Biến và Kiểu Dữ Liệu

```javascript
// Khai báo biến
let ten = "Nguyễn Văn A";      // String (chuỗi)
const tuoi = 25;               // Number (số)
var dangHocLapTrinh = true;   // Boolean (true/false)

// Các kiểu dữ liệu cơ bản
let chuoi = "Xin chào";       // String
let so = 100;                 // Number
let dung = true;              // Boolean
let khongGiaTri = null;       // Null
let chuaXacDinh;             // Undefined
```

### 3.2. Toán Tử

```javascript
// Toán tử số học
let tong = 5 + 3;        // Cộng
let hieu = 10 - 4;       // Trừ
let tich = 6 * 2;        // Nhân
let thuong = 15 / 3;     // Chia
let duChia = 7 % 2;      // Chia lấy dư

// Toán tử so sánh
let lonHon = 5 > 3;      // true
let nhoHon = 2 < 4;      // true
let bangNhau = 5 === 5;  // true
let khacNhau = 3 !== 4;  // true

// Toán tử logic
let va = true && false;  // AND
let hoac = true || false; // OR
let phuDinh = !true;     // NOT
```

## 4. Cấu Trúc Điều Khiển

### 4.1. Câu Lệnh Điều Kiện

```javascript
// If-else
let diem = 85;

if (diem >= 90) {
    console.log("Xuất sắc");
} else if (diem >= 80) {
    console.log("Giỏi");
} else if (diem >= 70) {
    console.log("Khá");
} else {
    console.log("Trung bình");
}

// Switch case
let ngay = 2;
switch (ngay) {
    case 1:
        console.log("Thứ Hai");
        break;
    case 2:
        console.log("Thứ Ba");
        break;
    default:
        console.log("Ngày không hợp lệ");
}
```

### 4.2. Vòng Lặp

```javascript
// For loop
for (let i = 0; i < 5; i++) {
    console.log(`Lần lặp thứ ${i + 1}`);
}

// While loop
let dem = 0;
while (dem < 3) {
    console.log(`Đếm: ${dem}`);
    dem++;
}

// For...of (với mảng)
let fruits = ["táo", "cam", "chuối"];
for (let fruit of fruits) {
    console.log(fruit);
}
```

## 5. Hàm (Functions)

### 5.1. Định Nghĩa Hàm

```javascript
// Hàm cơ bản
function chaoHoi(ten) {
    return `Xin chào ${ten}!`;
}

// Arrow function
const tinhTong = (a, b) => a + b;

// Hàm có nhiều tham số
function tinhDiemTrungBinh(diem1, diem2, diem3) {
    return (diem1 + diem2 + diem3) / 3;
}
```

### 5.2. Gọi Hàm

```javascript
// Sử dụng hàm
let loiChao = chaoHoi("An");
console.log(loiChao);  // "Xin chào An!"

let tong = tinhTong(5, 3);
console.log(tong);     // 8

let diemTB = tinhDiemTrungBinh(8, 7, 9);
console.log(diemTB);   // 8
```

## 6. Mảng (Arrays)

### 6.1. Làm Việc với Mảng

```javascript
// Tạo mảng
let danhSach = ["JavaScript", "HTML", "CSS"];

// Thêm phần tử
danhSach.push("PHP");           // Thêm vào cuối
danhSach.unshift("Python");     // Thêm vào đầu

// Xóa phần tử
let phanTuCuoi = danhSach.pop();    // Xóa từ cuối
let phanTuDau = danhSach.shift();   // Xóa từ đầu

// Duyệt mảng
danhSach.forEach(item => {
    console.log(item);
});
```

### 6.2. Các Phương Thức Mảng Thông Dụng

```javascript
// Filter - Lọc mảng
let soNguyen = [1, 2, 3, 4, 5, 6];
let soChan = soNguyen.filter(so => so % 2 === 0);

// Map - Biến đổi mảng
let binhPhuong = soNguyen.map(so => so * so);

// Reduce - Tính tổng
let tong = soNguyen.reduce((acc, curr) => acc + curr, 0);
```

## 7. Xử Lý Lỗi

```javascript
try {
    // Code có thể gây lỗi
    let x = y + 1;  // y chưa được định nghĩa
} catch (error) {
    console.log("Đã xảy ra lỗi:", error.message);
} finally {
    console.log("Luôn thực hiện phần này");
}
```

## 8. Bài Tập Thực Hành

### 8.1. Bài Tập Cơ Bản
```javascript
// 1. Viết hàm kiểm tra số chẵn lẻ
function kiemTraChanLe(so) {
    return so % 2 === 0 ? "Số chẵn" : "Số lẻ";
}

// 2. Tính tổng các số từ 1 đến n
function tinhTong(n) {
    let tong = 0;
    for (let i = 1; i <= n; i++) {
        tong += i;
    }
    return tong;
}
```

## 9. Tài Nguyên Học Tập

1. **Tài Liệu Tiếng Việt**:
   - [W3Schools Tiếng Việt](https://w3schools.com/js/)
   - [JavaScript.info](https://vi.javascript.info/)
   - [F8 - Fullstack.edu.vn](https://fullstack.edu.vn)

2. **Công Cụ Thực Hành**:
   - [CodePen](https://codepen.io)
   - [JSFiddle](https://jsfiddle.net)
   - [JavaScript Playground](https://playcode.io/javascript)

## Lời Kết

JavaScript là ngôn ngữ tuyệt vời để bắt đầu hành trình lập trình web của bạn. Hãy thực hành thường xuyên và không ngại thử nghiệm với các ví dụ khác nhau. Trong các bài tiếp theo, chúng ta sẽ tìm hiểu về DOM, Events, và các khái niệm nâng cao hơn trong JavaScript.

Hãy nhớ:
- Thực hành là chìa khóa của thành công
- Đừng ngại mắc lỗi - đó là cách học tốt nhất
- Tham gia cộng đồng để học hỏi và chia sẻ
- Xây dựng các dự án nhỏ để áp dụng kiến thức