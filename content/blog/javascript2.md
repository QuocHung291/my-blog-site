---
title: "JavaScript Phần 2: DOM và Sự Kiện Trong JavaScript"
date: 2025-08-21
draft: false
tags: ["JavaScript", "Web Development", "DOM", "Hướng dẫn"]
categories: ["Web Development"]
image: "/image/img-2.jpg"
---

Sau khi đã nắm vững các kiến thức cơ bản về JavaScript, chúng ta sẽ tìm hiểu về DOM (Document Object Model) và cách xử lý sự kiện - hai khía cạnh quan trọng trong lập trình web động. <!--more--> 

## 1. DOM (Document Object Model)

### 1.1. DOM Là Gì?

DOM là một giao diện lập trình cho phép JavaScript tương tác với HTML và CSS. DOM biểu diễn trang web dưới dạng cấu trúc cây, với mỗi phần tử HTML là một nút (node).

### 1.2. Truy Cập Phần Tử DOM

```javascript
// Truy cập theo ID
const element = document.getElementById('myId');

// Truy cập theo class
const elements = document.getElementsByClassName('myClass');

// Truy cập theo tên thẻ
const divs = document.getElementsByTagName('div');

// Truy cập bằng CSS selector
const firstElement = document.querySelector('.myClass');
const allElements = document.querySelectorAll('.myClass');
```

### 1.3. Thao Tác với DOM

```javascript
// Tạo phần tử mới
const newDiv = document.createElement('div');
const newText = document.createTextNode('Nội dung mới');

// Thêm phần tử vào DOM
newDiv.appendChild(newText);
document.body.appendChild(newDiv);

// Xóa phần tử
const elementToRemove = document.getElementById('oldElement');
elementToRemove.parentNode.removeChild(elementToRemove);

// Thay thế phần tử
const oldElement = document.getElementById('old');
const newElement = document.createElement('div');
oldElement.parentNode.replaceChild(newElement, oldElement);
```

## 2. Thao Tác với Thuộc Tính và Style

### 2.1. Thuộc Tính HTML

```javascript
// Đọc thuộc tính
const link = document.getElementById('myLink');
console.log(link.href);

// Thay đổi thuộc tính
link.href = 'https://example.com';
link.setAttribute('target', '_blank');

// Kiểm tra thuộc tính tồn tại
console.log(link.hasAttribute('href'));

// Xóa thuộc tính
link.removeAttribute('target');
```

### 2.2. Style và CSS

```javascript
const element = document.getElementById('myElement');

// Thay đổi style trực tiếp
element.style.backgroundColor = 'red';
element.style.fontSize = '20px';
element.style.margin = '10px';

// Thêm/xóa class
element.classList.add('highlight');
element.classList.remove('old-class');
element.classList.toggle('active');
element.classList.contains('highlight');
```

## 3. Xử Lý Sự Kiện (Events)

### 3.1. Đăng Ký Sự Kiện

```javascript
// Cách 1: Sử dụng addEventListener
const button = document.getElementById('myButton');
button.addEventListener('click', function(event) {
    console.log('Nút đã được click!');
});

// Cách 2: Gán trực tiếp
button.onclick = function(event) {
    console.log('Nút đã được click!');
};

// Xóa sự kiện
const handleClick = (event) => {
    console.log('Click!');
};
button.addEventListener('click', handleClick);
button.removeEventListener('click', handleClick);
```

### 3.2. Các Loại Sự Kiện Phổ Biến

```javascript
// Sự kiện chuột
element.addEventListener('click', handleClick);
element.addEventListener('dblclick', handleDoubleClick);
element.addEventListener('mouseover', handleMouseOver);
element.addEventListener('mouseout', handleMouseOut);

// Sự kiện bàn phím
document.addEventListener('keydown', handleKeyDown);
document.addEventListener('keyup', handleKeyUp);
document.addEventListener('keypress', handleKeyPress);

// Sự kiện form
form.addEventListener('submit', handleSubmit);
input.addEventListener('change', handleChange);
input.addEventListener('input', handleInput);
```

## 4. Thực Hành: Tạo Ứng Dụng Todo List

### 4.1. HTML Cơ Bản

```html
<div id="todo-app">
    <input type="text" id="todo-input" placeholder="Nhập công việc...">
    <button id="add-button">Thêm</button>
    <ul id="todo-list"></ul>
</div>
```

### 4.2. JavaScript Implementation

```javascript
class TodoApp {
    constructor() {
        this.todoInput = document.getElementById('todo-input');
        this.addButton = document.getElementById('add-button');
        this.todoList = document.getElementById('todo-list');
        this.todos = [];
        
        this.addButton.addEventListener('click', () => this.addTodo());
    }
    
    addTodo() {
        const text = this.todoInput.value.trim();
        if (text) {
            this.todos.push({
                id: Date.now(),
                text: text,
                completed: false
            });
            this.renderTodos();
            this.todoInput.value = '';
        }
    }
    
    renderTodos() {
        this.todoList.innerHTML = '';
        this.todos.forEach(todo => {
            const li = document.createElement('li');
            li.innerHTML = `
                <input type="checkbox" 
                       ${todo.completed ? 'checked' : ''}>
                <span>${todo.text}</span>
                <button class="delete">Xóa</button>
            `;
            
            // Xử lý sự kiện checkbox
            const checkbox = li.querySelector('input');
            checkbox.addEventListener('change', () => {
                todo.completed = checkbox.checked;
                this.renderTodos();
            });
            
            // Xử lý sự kiện xóa
            const deleteButton = li.querySelector('.delete');
            deleteButton.addEventListener('click', () => {
                this.todos = this.todos.filter(t => t.id !== todo.id);
                this.renderTodos();
            });
            
            this.todoList.appendChild(li);
        });
    }
}

// Khởi tạo ứng dụng
const todoApp = new TodoApp();
```

## 5. Thao Tác với Form

### 5.1. Validation Form

```javascript
const form = document.getElementById('myForm');

form.addEventListener('submit', function(event) {
    event.preventDefault(); // Ngăn form submit mặc định
    
    const username = document.getElementById('username').value;
    const email = document.getElementById('email').value;
    const password = document.getElementById('password').value;
    
    // Kiểm tra username
    if (username.length < 3) {
        showError('username', 'Tên đăng nhập phải có ít nhất 3 ký tự');
        return;
    }
    
    // Kiểm tra email
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(email)) {
        showError('email', 'Email không hợp lệ');
        return;
    }
    
    // Kiểm tra password
    if (password.length < 6) {
        showError('password', 'Mật khẩu phải có ít nhất 6 ký tự');
        return;
    }
    
    // Nếu mọi thứ ok, submit form
    form.submit();
});

function showError(fieldId, message) {
    const errorElement = document.getElementById(`${fieldId}-error`);
    if (errorElement) {
        errorElement.textContent = message;
    }
}
```

## 6. Xử Lý Bất Đồng Bộ với DOM

### 6.1. Promises và DOM

```javascript
function loadContent() {
    return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        xhr.open('GET', 'content.html');
        xhr.onload = () => resolve(xhr.responseText);
        xhr.onerror = () => reject(xhr.statusText);
        xhr.send();
    });
}

// Sử dụng Promise
loadContent()
    .then(content => {
        document.getElementById('content').innerHTML = content;
    })
    .catch(error => {
        console.error('Lỗi:', error);
    });
```

### 6.2. Async/Await với DOM

```javascript
async function updateContent() {
    try {
        const content = await loadContent();
        document.getElementById('content').innerHTML = content;
    } catch (error) {
        console.error('Lỗi:', error);
    }
}
```

## 7. Tối Ưu Hiệu Suất

### 7.1. Best Practices

```javascript
// Sử dụng Fragment
const fragment = document.createDocumentFragment();
for (let i = 0; i < 1000; i++) {
    const element = document.createElement('div');
    element.textContent = `Item ${i}`;
    fragment.appendChild(element);
}
document.body.appendChild(fragment);

// Debounce function
function debounce(func, wait) {
    let timeout;
    return function executedFunction(...args) {
        const later = () => {
            clearTimeout(timeout);
            func(...args);
        };
        clearTimeout(timeout);
        timeout = setTimeout(later, wait);
    };
}

// Sử dụng debounce cho sự kiện scroll
const handleScroll = debounce(() => {
    console.log('Scroll event handled');
}, 250);

window.addEventListener('scroll', handleScroll);
```

## Lời Kết

DOM và xử lý sự kiện là hai khía cạnh quan trọng trong lập trình JavaScript. Với những kiến thức này, bạn có thể:
- Tạo các trang web động và tương tác
- Xây dựng các ứng dụng web phức tạp
- Tối ưu hiệu suất trang web

Trong phần tiếp theo, chúng ta sẽ tìm hiểu về AJAX, API và cách làm việc với dữ liệu từ server.

### Bài Tập Thực Hành
1. Tạo một form đăng ký với validation
2. Xây dựng ứng dụng todo list với localStorage
3. Tạo slider ảnh đơn giản
4. Xây dựng menu dropdown động

### Tài Nguyên Tham Khảo
- [MDN Web Docs](https://developer.mozilla.org/vi/)
- [JavaScript.info](https://vi.javascript.info/)
- [W3Schools DOM Tutorial](https://www.w3schools.com/js/js_htmldom.asp)