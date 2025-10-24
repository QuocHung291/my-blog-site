---
title: "JavaScript Phần 3: AJAX, API và Xử Lý Dữ Liệu"
date: 2025-08-22
draft: false
tags: ["JavaScript", "Web Development", "API", "AJAX", "Hướng dẫn"]
categories: ["Web Development"]
image: "/image/img-2.jpg"
---

Ở phần này, chúng ta sẽ tìm hiểu cách JavaScript tương tác với server thông qua AJAX và API, cũng như các kỹ thuật xử lý dữ liệu phổ biến. <!--more-->

## 1. AJAX và Fetch API

### 1.1. AJAX là gì?

AJAX (Asynchronous JavaScript and XML) cho phép trang web cập nhật dữ liệu mà không cần tải lại trang. Ngày nay, chúng ta thường sử dụng Fetch API hoặc axios để thực hiện các yêu cầu AJAX.

### 1.2. Sử Dụng Fetch API

```javascript
// GET Request cơ bản
fetch('https://api.example.com/data')
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error('Lỗi:', error));

// POST Request
const postData = {
    title: 'Bài viết mới',
    content: 'Nội dung bài viết'
};

fetch('https://api.example.com/posts', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify(postData)
})
    .then(response => response.json())
    .then(data => console.log('Success:', data))
    .catch(error => console.error('Error:', error));
```

### 1.3. Async/Await với Fetch

```javascript
async function getData() {
    try {
        const response = await fetch('https://api.example.com/data');
        const data = await response.json();
        return data;
    } catch (error) {
        console.error('Lỗi:', error);
    }
}

// Sử dụng hàm getData
async function displayData() {
    const data = await getData();
    document.getElementById('result').innerHTML = JSON.stringify(data, null, 2);
}
```

## 2. Làm Việc với REST API

### 2.1. Các Phương Thức HTTP

```javascript
// GET - Lấy dữ liệu
async function getUsers() {
    const response = await fetch('https://api.example.com/users');
    return await response.json();
}

// POST - Tạo mới
async function createUser(userData) {
    const response = await fetch('https://api.example.com/users', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify(userData)
    });
    return await response.json();
}

// PUT - Cập nhật
async function updateUser(id, userData) {
    const response = await fetch(`https://api.example.com/users/${id}`, {
        method: 'PUT',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify(userData)
    });
    return await response.json();
}

// DELETE - Xóa
async function deleteUser(id) {
    const response = await fetch(`https://api.example.com/users/${id}`, {
        method: 'DELETE'
    });
    return await response.json();
}
```

### 2.2. Xử Lý Authentication

```javascript
// Thêm token vào header
async function getProtectedData() {
    const token = localStorage.getItem('token');
    
    const response = await fetch('https://api.example.com/protected', {
        headers: {
            'Authorization': `Bearer ${token}`
        }
    });
    
    return await response.json();
}

// Login function
async function login(username, password) {
    const response = await fetch('https://api.example.com/login', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({ username, password })
    });
    
    const data = await response.json();
    if (data.token) {
        localStorage.setItem('token', data.token);
    }
    return data;
}
```

## 3. Xử Lý Dữ Liệu với JSON

### 3.1. Làm việc với JSON

```javascript
// Chuyển đổi Object thành JSON
const user = {
    name: 'Nguyễn Văn A',
    age: 25,
    email: 'nguyenvana@example.com'
};

const jsonString = JSON.stringify(user);
console.log(jsonString);

// Chuyển đổi JSON thành Object
const jsonData = '{"name":"Nguyễn Văn A","age":25}';
const userData = JSON.parse(jsonData);
console.log(userData.name); // "Nguyễn Văn A"
```

### 3.2. Xử Lý Lỗi JSON

```javascript
function safeJSONParse(jsonString) {
    try {
        return {
            data: JSON.parse(jsonString),
            error: null
        };
    } catch (error) {
        return {
            data: null,
            error: error.message
        };
    }
}
```

## 4. Ứng Dụng Thực Tế: Xây Dựng Blog API Client

```javascript
class BlogAPI {
    constructor(baseURL) {
        this.baseURL = baseURL;
        this.token = localStorage.getItem('token');
    }

    async getPosts() {
        try {
            const response = await fetch(`${this.baseURL}/posts`);
            return await response.json();
        } catch (error) {
            console.error('Lỗi khi lấy bài viết:', error);
            throw error;
        }
    }

    async createPost(postData) {
        try {
            const response = await fetch(`${this.baseURL}/posts`, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': `Bearer ${this.token}`
                },
                body: JSON.stringify(postData)
            });
            return await response.json();
        } catch (error) {
            console.error('Lỗi khi tạo bài viết:', error);
            throw error;
        }
    }

    async updatePost(id, postData) {
        try {
            const response = await fetch(`${this.baseURL}/posts/${id}`, {
                method: 'PUT',
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': `Bearer ${this.token}`
                },
                body: JSON.stringify(postData)
            });
            return await response.json();
        } catch (error) {
            console.error('Lỗi khi cập nhật bài viết:', error);
            throw error;
        }
    }
}

// Sử dụng BlogAPI
const api = new BlogAPI('https://api.example.com');

// Hiển thị danh sách bài viết
async function displayPosts() {
    try {
        const posts = await api.getPosts();
        const postsContainer = document.getElementById('posts');
        
        postsContainer.innerHTML = posts.map(post => `
            <article>
                <h2>${post.title}</h2>
                <p>${post.content}</p>
                <button onclick="editPost(${post.id})">Sửa</button>
                <button onclick="deletePost(${post.id})">Xóa</button>
            </article>
        `).join('');
    } catch (error) {
        alert('Không thể tải bài viết');
    }
}
```

## 5. Xử Lý File và Upload

### 5.1. Upload File với FormData

```javascript
async function uploadFile(file) {
    const formData = new FormData();
    formData.append('file', file);

    try {
        const response = await fetch('https://api.example.com/upload', {
            method: 'POST',
            body: formData
        });
        return await response.json();
    } catch (error) {
        console.error('Lỗi khi upload file:', error);
        throw error;
    }
}

// Xử lý sự kiện upload
const fileInput = document.getElementById('fileInput');
fileInput.addEventListener('change', async (event) => {
    const file = event.target.files[0];
    try {
        const result = await uploadFile(file);
        console.log('File đã được upload:', result);
    } catch (error) {
        alert('Lỗi khi upload file');
    }
});
```

## 6. Xử Lý Lỗi và Loading States

### 6.1. Loading Component

```javascript
class LoadingManager {
    constructor() {
        this.loadingElement = document.getElementById('loading');
    }

    show() {
        this.loadingElement.style.display = 'block';
    }

    hide() {
        this.loadingElement.style.display = 'none';
    }

    async wrapPromise(promise) {
        try {
            this.show();
            const result = await promise;
            return result;
        } finally {
            this.hide();
        }
    }
}

// Sử dụng LoadingManager
const loading = new LoadingManager();

async function fetchData() {
    return loading.wrapPromise(
        fetch('https://api.example.com/data').then(r => r.json())
    );
}
```

## Lời Kết

AJAX và API là những công cụ quan trọng trong phát triển web hiện đại. Với những kiến thức này, bạn có thể:
- Tạo ứng dụng web động với dữ liệu thời gian thực
- Tích hợp các dịch vụ bên ngoài vào ứng dụng của bạn
- Xây dựng ứng dụng web một trang (SPA)

### Bài Tập Thực Hành
1. Tạo ứng dụng todo list với REST API
2. Xây dựng form upload ảnh với preview
3. Tạo trang blog đơn giản với CRUD operations
4. Thực hiện authentication với JWT

### Tài Nguyên Tham Khảo
- [MDN Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [JSON.org](https://www.json.org/json-en.html)
- [RESTful API Design](https://restfulapi.net/)