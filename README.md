<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Connetto - تطبيق الدردشة</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        /* ==================== 1. متغيرات الألوان والتنسيق العام ==================== */
        :root {
            --wa-green-dark: #075E54;
            --wa-green-light: #128C7E;
            --wa-background: #ECE5DD;
            --wa-chat-sent: #DCF8C6;
            --wa-chat-received: #FFFFFF;
            --wa-text-color: #333;
            --wa-border-color: #f0f0f0;
            --primary-gradient: linear-gradient(to right, #007bff, #ff6b6b);
            --error-color: #ff6b6b;
            --success-color: #4CAF50;
            --warning-color: #ff9800;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            background-color: var(--wa-border-color);
            min-height: 100vh;
            color: var(--wa-text-color);
            overflow-x: hidden;
        }

        .container {
            max-width: 100%;
            margin: 0 auto;
            padding-bottom: 60px;
        }

        button {
            cursor: pointer;
            transition: background 0.2s;
            border: none;
        }

        /* ==================== 2. شاشة تسجيل الدخول ==================== */
        .login-overlay {
            position: fixed;
            inset: 0;
            z-index: 2500;
            background: linear-gradient(135deg, #090a1b 0%, #151a3a 100%);
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .login-container {
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 20px;
            padding: 30px 20px;
            width: 100%;
            max-width: 320px;
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.5);
            text-align: center;
            backdrop-filter: blur(10px);
            color: #fff;
        }

        .login-header h1 {
            font-size: 22px;
            margin: 8px 0 4px;
            font-weight: 700;
        }

        .login-header p {
            color: #aaa;
            font-size: 12px;
        }

        .logo-placeholder {
            width: 60px;
            height: 60px;
            margin: 0 auto 10px;
            background: var(--primary-gradient);
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .logo-icon {
            color: white;
            font-size: 1.5em;
        }

        .login-input-group {
            position: relative;
            margin-bottom: 15px;
        }

        .login-input-group input {
            width: 100%;
            padding: 12px 12px 12px 40px;
            border: none;
            border-radius: 10px;
            background-color: rgba(255, 255, 255, 0.1);
            color: #fff;
            font-size: 14px;
        }

        .login-input-group i {
            position: absolute;
            top: 50%;
            left: 12px;
            transform: translateY(-50%);
            color: #aaa;
            font-size: 16px;
        }

        .login-button {
            width: 100%;
            padding: 12px;
            border: none;
            border-radius: 10px;
            background: var(--primary-gradient);
            color: #fff;
            font-size: 16px;
            font-weight: 700;
            cursor: pointer;
            margin-bottom: 20px;
        }

        .login-error {
            color: var(--error-color);
            display: none;
            margin-bottom: 10px;
            font-size: 0.9em;
        }

        .login-options {
            display: flex;
            justify-content: space-between;
            font-size: 12px;
            margin-bottom: 20px;
        }

        .forgot-password, .sign-up {
            color: #aaa;
            text-decoration: none;
        }

        .social-login {
            display: flex;
            justify-content: space-around;
        }

        .social-icon {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 18px;
            color: #fff;
            text-decoration: none;
        }

        .social-icon.facebook { background: #3b5998; }
        .social-icon.twitter { background: #1da1f2; }
        .social-icon.google { background: #db4437; }

        /* ==================== 3. هيكل التطبيق الرئيسي ==================== */
        .wa-header {
            background-color: var(--wa-green-dark);
            color: white;
            padding: 10px 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: sticky;
            top: 0;
            z-index: 1000;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.15);
        }

        .wa-header h1 {
            font-size: 1.25em;
            font-weight: 500;
        }

        .wa-header-icons {
            display: flex;
            align-items: center;
        }

        .wa-header-icons button {
            background: transparent;
            border: none;
            color: white;
            font-size: 1.2em;
            margin-left: 15px;
        }

        .wa-tabs-bar {
            display: flex;
            background-color: var(--wa-green-dark);
            color: rgba(255, 255, 255, 0.6);
            padding: 0 15px;
        }

        .wa-tab {
            flex: 1;
            padding: 10px 5px;
            text-align: center;
            background: transparent;
            border: none;
            font-weight: 500;
            font-size: 0.9em;
            color: inherit;
            border-bottom: 3px solid transparent;
            transition: all 0.3s;
        }

        .wa-tab.active {
            color: white;
            border-bottom-color: white;
        }

        /* ==================== 4. شريط البحث ==================== */
        .search-container {
            background-color: var(--wa-green-dark);
            padding: 10px 15px;
            display: none;
        }

        .search-container.active {
            display: block;
        }

        .search-input {
            width: 100%;
            padding: 10px 15px 10px 40px;
            border: none;
            border-radius: 20px;
            background-color: white;
            font-size: 0.9em;
        }

        .search-input-container {
            position: relative;
        }

        .search-input-container i {
            position: absolute;
            top: 50%;
            right: 15px;
            transform: translateY(-50%);
            color: #666;
        }

        /* ==================== 5. قائمة الدردشات ==================== */
        .friends-list {
            padding: 0;
            background-color: white;
        }

        .friend-item {
            display: flex;
            align-items: center;
            padding: 10px 15px;
            border-bottom: 1px solid var(--wa-border-color);
            transition: background 0.15s;
            cursor: pointer;
        }

        .friend-item:active {
            background: #f0f0f0;
        }

        .friend-avatar {
            width: 50px;
            height: 50px;
            min-width: 50px;
            border-radius: 50%;
            background-color: #ddd;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 1.5em;
            margin-left: 10px;
            color: #555;
            overflow: hidden;
            background-size: cover;
            background-position: center;
            position: relative;
        }

        .friend-avatar img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            border-radius: 50%;
        }

        .online-status {
            position: absolute;
            bottom: 2px;
            left: 2px;
            width: 12px;
            height: 12px;
            border-radius: 50%;
            background-color: #4CAF50;
            border: 2px solid white;
        }

        .online-status.offline {
            background-color: #ccc;
        }

        .friend-info {
            flex-grow: 1;
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        .friend-name {
            font-weight: 600;
            font-size: 1em;
            margin-bottom: 2px;
            color: #000;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .friend-last-message {
            color: #666;
            font-size: 0.9em;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .friend-meta {
            text-align: left;
            font-size: 0.75em;
            color: #999;
            display: flex;
            flex-direction: column;
            align-items: flex-start;
        }

        /* ==================== 6. شاشة الدردشة ==================== */
        #chatTab {
            position: fixed;
            inset: 0;
            background-color: var(--wa-background);
            z-index: 2000;
            flex-direction: column;
            transform: translateX(100%);
            transition: transform 0.3s ease-out;
            padding-top: 56px;
            display: flex;
        }

        #chatTab.active {
            transform: translateX(0);
        }

        .chat-header {
            position: fixed;
            top: 0;
            width: 100%;
            background-color: var(--wa-green-dark);
            color: white;
            padding: 10px;
            display: flex;
            align-items: center;
            z-index: 2001;
        }

        .chat-header button {
            background: transparent;
            border: none;
            color: white;
            font-size: 1.3em;
            margin-left: 15px;
            padding: 0;
        }

        .chat-header .friend-avatar {
            width: 35px;
            height: 35px;
            font-size: 1.2em;
            position: relative;
        }

        .friend-name-chat {
            font-size: 1.1em;
            font-weight: 600;
            margin-right: 10px;
        }

        .chat-online-status {
            font-size: 0.7em;
            color: #ccc;
            margin-top: 2px;
        }

        .chat-messages {
            flex: 1;
            overflow-y: auto;
            padding: 10px;
            display: flex;
            flex-direction: column;
            gap: 6px;
        }

        .message {
            max-width: 80%;
            padding: 8px 10px;
            border-radius: 8px;
            word-wrap: break-word;
            position: relative;
            box-shadow: 0 1px 0.5px rgba(0, 0, 0, 0.13);
            font-size: 0.9em;
        }

        .message.sent {
            background-color: var(--wa-chat-sent);
            align-self: flex-start;
            margin-right: auto;
            border-radius: 8px 0 8px 8px;
        }

        .message.received {
            background-color: var(--wa-chat-received);
            align-self: flex-end;
            margin-left: auto;
            border-radius: 0 8px 8px 8px;
        }

        .voice-message {
            display: flex;
            align-items: center;
            padding: 8px 12px;
            background-color: var(--wa-chat-sent);
            border-radius: 8px;
            max-width: 80%;
        }

        .voice-message.received {
            background-color: var(--wa-chat-received);
            align-self: flex-end;
            margin-left: auto;
        }

        .voice-message.sent {
            align-self: flex-start;
            margin-right: auto;
        }

        .voice-play-btn {
            background: var(--wa-green-light);
            color: white;
            border: none;
            border-radius: 50%;
            width: 30px;
            height: 30px;
            display: flex;
            justify-content: center;
            align-items: center;
            margin-left: 8px;
        }

        .voice-duration {
            font-size: 0.8em;
            color: #666;
            margin-right: 8px;
        }

        .voice-waveform {
            flex: 1;
            height: 20px;
            background: linear-gradient(90deg, #ddd 0%, #ddd 100%);
            border-radius: 10px;
            overflow: hidden;
            position: relative;
        }

        .voice-wave {
            height: 100%;
            background: var(--wa-green-light);
            width: 0%;
            transition: width 0.1s linear;
        }

        /* ==================== 7. الرسائل مع الصور ==================== */
        .image-message {
            max-width: 80%;
            border-radius: 8px;
            overflow: hidden;
            position: relative;
            box-shadow: 0 1px 0.5px rgba(0, 0, 0, 0.13);
        }

        .image-message.sent {
            align-self: flex-start;
            margin-right: auto;
        }

        .image-message.received {
            align-self: flex-end;
            margin-left: auto;
        }

        .image-message img {
            width: 100%;
            max-width: 250px;
            height: auto;
            display: block;
        }

        .image-caption {
            padding: 8px 10px;
            background-color: rgba(0, 0, 0, 0.7);
            color: white;
            font-size: 0.9em;
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
        }

        .chat-input-area {
            background: var(--wa-border-color);
            padding: 8px;
            display: flex;
            gap: 5px;
            align-items: center;
            position: relative;
        }

        .chat-input-area input {
            flex: 1;
            padding: 10px 15px;
            border: none;
            border-radius: 20px;
            background-color: white;
            font-size: 1em;
        }

        .send-btn {
            width: 44px;
            height: 44px;
            padding: 0;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
            font-size: 1.1em;
        }

        .chat-input-area .send-btn {
            background-color: var(--wa-green-light);
        }

        .emoji-button, .voice-button, .image-button {
            background-color: #f0f0f0 !important;
            color: #666 !important;
        }

        .emoji-picker {
            position: absolute;
            bottom: 55px;
            left: 10px;
            background: white;
            border: 1px solid #ddd;
            border-radius: 10px;
            padding: 8px;
            width: 280px;
            max-height: 200px;
            overflow-y: auto;
            display: none;
            flex-wrap: wrap;
            gap: 4px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
            z-index: 1000;
        }

        .emoji-picker.show {
            display: flex;
        }

        .emoji-item {
            font-size: 1.5em;
            cursor: pointer;
            padding: 6px;
            border-radius: 6px;
            transition: all 0.2s;
            display: flex;
            justify-content: center;
            align-items: center;
            min-width: 35px;
            min-height: 35px;
        }

        .emoji-item:hover {
            background: #f0f0f0;
            transform: scale(1.1);
        }

        .emoji-category {
            width: 100%;
            text-align: center;
            padding: 5px;
            background: #f8f9fa;
            font-weight: bold;
            color: #666;
            font-size: 0.8em;
            border-bottom: 1px solid #eee;
        }

        /* ==================== 8. تسجيل الصوت ==================== */
        .voice-recorder {
            display: none;
            flex-direction: column;
            align-items: center;
            padding: 15px;
            background: white;
            border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
            position: absolute;
            bottom: 60px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 1000;
            width: 90%;
            max-width: 300px;
        }

        .voice-recorder.active {
            display: flex;
        }

        .recording-indicator {
            display: flex;
            align-items: center;
            margin-bottom: 10px;
        }

        .recording-dot {
            width: 12px;
            height: 12px;
            background-color: #f44336;
            border-radius: 50%;
            margin-left: 8px;
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.5; }
            100% { opacity: 1; }
        }

        .recording-timer {
            font-size: 1.2em;
            font-weight: bold;
            color: #333;
        }

        .recording-controls {
            display: flex;
            gap: 15px;
        }

        .cancel-recording, .send-recording {
            padding: 8px 15px;
            border-radius: 20px;
            font-size: 0.9em;
            cursor: pointer;
        }

        .cancel-recording {
            background: #f0f0f0;
            color: #333;
        }

        .send-recording {
            background: var(--wa-green-light);
            color: white;
        }

        /* ==================== 9. تبويب الملف الشخصي ==================== */
        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
        }

        .card {
            background: white;
            border-radius: 0;
            padding: 15px;
            margin-bottom: 10px;
            box-shadow: none;
        }

        .profile-section {
            margin-bottom: 20px;
        }

        .profile-section h2 {
            margin-bottom: 15px;
            color: var(--wa-green-dark);
            font-size: 1.2em;
        }

        .input-group {
            margin-bottom: 15px;
        }

        .input-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: 600;
            color: #333;
        }

        .input-group input {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 1em;
        }

        .profile-button {
            width: 100%;
            padding: 12px;
            border-radius: 8px;
            color: white;
            font-size: 1em;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }

        .profile-button.save {
            background: var(--wa-green-light);
        }

        .profile-button.add-friend {
            background: var(--wa-green-light);
        }

        /* ==================== 10. صورة الملف الشخصي ==================== */
        .profile-picture-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            margin-bottom: 20px;
        }

        .profile-picture {
            width: 100px;
            height: 100px;
            border-radius: 50%;
            background-color: #ddd;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 2.5em;
            color: #555;
            overflow: hidden;
            background-size: cover;
            background-position: center;
            margin-bottom: 10px;
            position: relative;
        }

        .profile-picture img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            border-radius: 50%;
        }

        .change-picture-btn {
            background: var(--wa-green-light);
            color: white;
            padding: 8px 15px;
            border-radius: 20px;
            font-size: 0.9em;
            cursor: pointer;
        }

        .change-picture-btn input {
            display: none;
        }

        /* ==================== 11. قائمة النقاط الثلاث (القائمة المنبثقة) ==================== */
        .dots-menu {
            position: relative;
        }

        .dots-menu-content {
            display: none;
            position: absolute;
            top: 100%;
            left: 10px;
            background-color: white;
            min-width: 250px;
            box-shadow: 0 8px 16px rgba(0,0,0,0.1);
            border-radius: 8px;
            z-index: 1000;
            overflow: hidden;
        }

        .dots-menu-content.active {
            display: block;
        }

        .dots-menu-item {
            padding: 12px 16px;
            display: flex;
            align-items: center;
            gap: 10px;
            cursor: pointer;
            transition: background 0.2s;
            border-bottom: 1px solid #f0f0f0;
        }

        .dots-menu-item:last-child {
            border-bottom: none;
        }

        .dots-menu-item:hover {
            background: #f5f5f5;
        }

        .dots-menu-item.logout {
            color: #f44336;
        }

        .dots-menu-item.logout i {
            color: #f44336;
        }

        /* ==================== 12. إعدادات الإشعارات في القائمة المنبثقة ==================== */
        .notification-settings-menu {
            padding: 12px 16px;
            border-bottom: 1px solid #f0f0f0;
        }

        .notification-settings-menu h4 {
            margin-bottom: 10px;
            color: var(--wa-green-dark);
            font-size: 1em;
        }

        .notification-toggle {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 8px;
        }

        .notification-toggle span {
            font-size: 0.9em;
        }

        .toggle-switch {
            position: relative;
            display: inline-block;
            width: 40px;
            height: 20px;
        }

        .toggle-switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }

        .toggle-switch .slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: #ccc;
            transition: .4s;
            border-radius: 20px;
        }

        .toggle-switch input:checked + .slider {
            background-color: var(--wa-green-light);
        }

        .toggle-switch .slider:before {
            position: absolute;
            content: "";
            height: 16px;
            width: 16px;
            left: 2px;
            bottom: 2px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }

        .toggle-switch input:checked + .slider:before {
            transform: translateX(20px);
        }

        .test-notifications-btn {
            width: 100%;
            padding: 8px;
            background: #ff9800;
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 0.8em;
            margin-top: 8px;
            cursor: pointer;
        }

        /* ==================== 13. عناصر إضافية ==================== */
        .empty-state {
            text-align: center;
            color: #999;
            padding: 30px 15px;
            font-size: 0.9em;
        }

        .fab {
            position: fixed;
            bottom: 20px;
            left: 20px;
            width: 55px;
            height: 55px;
            border-radius: 50%;
            background-color: var(--wa-green-light);
            color: white;
            display: none;
            justify-content: center;
            align-items: center;
            font-size: 1.5em;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            z-index: 1500;
        }

        /* ==================== 14. الإشعارات ==================== */
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 12px 20px;
            border-radius: 8px;
            color: white;
            z-index: 3000;
            animation: slideIn 0.3s ease-out;
        }

        .notification.success {
            background-color: var(--success-color);
        }

        .notification.error {
            background-color: var(--error-color);
        }

        .notification.info {
            background-color: var(--wa-green-light);
        }

        .notification.warning {
            background-color: var(--warning-color);
        }

        @keyframes slideIn {
            from {
                transform: translateX(100%);
                opacity: 0;
            }
            to {
                transform: translateX(0);
                opacity: 1;
            }
        }

        /* ==================== 15. التجاوب مع الشاشات الصغيرة ==================== */
        @media (max-width: 768px) {
            .wa-header h1 {
                font-size: 1.1em;
            }
            
            .friend-item {
                padding: 8px 12px;
            }
            
            .friend-avatar {
                width: 45px;
                height: 45px;
                font-size: 1.3em;
            }
            
            .emoji-picker {
                width: 250px;
                max-height: 180px;
            }
            
            .profile-picture {
                width: 80px;
                height: 80px;
            }
            
            .dots-menu-content {
                min-width: 220px;
                left: 5px;
            }
            
            .image-message img {
                max-width: 200px;
            }
        }
    </style>
</head>

<body>
    <!-- شاشة تسجيل الدخول -->
    <div id="loginOverlay" class="login-overlay">
        <div class="login-container">
            <div class="login-header">
                <div class="logo-placeholder">
                    <i class="fas fa-comments logo-icon"></i>
                </div>
                <h1>CONNETTO</h1>
                <p>Connect, Share, Discover</p>
            </div>

            <form class="login-form" id="loginForm">
                <div class="login-input-group">
                    <i class="fas fa-user"></i>
                    <input type="text" id="loginUsername" placeholder="اسم المستخدم" required>
                </div>

                <div class="login-input-group">
                    <i class="fas fa-lock"></i>
                    <input type="password" id="loginPassword" placeholder="كلمة المرور" required>
                </div>
                <div id="loginError" class="login-error">كلمة المرور خاطئة</div>

                <div class="login-options">
                    <a href="#" class="forgot-password">نسيت كلمة المرور؟</a>
                    <a href="#" class="sign-up">إنشاء حساب</a>
                </div>

                <button type="submit" class="login-button">
                    تسجيل الدخول
                </button>
            </form>

            <div class="social-login">
                <a href="#" class="social-icon facebook">
                    <i class="fab fa-facebook-f"></i>
                </a>
                <a href="#" class="social-icon twitter">
                    <i class="fab fa-twitter"></i>
                </a>
                <a href="#" class="social-icon google">
                    <i class="fab fa-google"></i>
                </a>
            </div>
        </div>
    </div>

    <!-- التطبيق الرئيسي -->
    <div class="container" id="appContainer" style="display: none;">
        <!-- رأس التطبيق -->
        <div class="wa-header">
            <h1>دردشات Connetto</h1>
            <div class="wa-header-icons">
                <button title="بحث" onclick="toggleSearch()"><i class="fas fa-search"></i></button>
                <div class="dots-menu">
                    <button title="القائمة" onclick="toggleDotsMenu()"><i class="fas fa-ellipsis-v"></i></button>
                    <div class="dots-menu-content" id="dotsMenu">
                        <div class="dots-menu-item" onclick="switchTab('profile')">
                            <i class="fas fa-user"></i>
                            <span>الملف الشخصي</span>
                        </div>
                        
                        <div class="notification-settings-menu">
                            <h4>🔔 إعدادات الإشعارات</h4>
                            
                            <div class="notification-toggle">
                                <span>إشعارات المتصفح</span>
                                <label class="toggle-switch">
                                    <input type="checkbox" id="browserNotificationsToggle" onchange="toggleBrowserNotifications()">
                                    <span class="slider"></span>
                                </label>
                            </div>

                            <div class="notification-toggle">
                                <span>إشعارات الدردشات</span>
                                <label class="toggle-switch">
                                    <input type="checkbox" id="chatNotificationsToggle" checked onchange="toggleChatNotifications()">
                                    <span class="slider"></span>
                                </label>
                            </div>

                            <div class="notification-toggle">
                                <span>إشعارات المجموعات</span>
                                <label class="toggle-switch">
                                    <input type="checkbox" id="groupNotificationsToggle" checked onchange="toggleGroupNotifications()">
                                    <span class="slider"></span>
                                </label>
                            </div>

                            <button class="test-notifications-btn" onclick="testNotifications()">
                                🔔 اختبار الإشعارات
                            </button>
                        </div>
                        
                        <div class="dots-menu-item" onclick="showSettings()">
                            <i class="fas fa-cog"></i>
                            <span>الإعدادات</span>
                        </div>
                        
                        <div class="dots-menu-item logout" onclick="logout()">
                            <i class="fas fa-sign-out-alt"></i>
                            <span>تسجيل الخروج</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- شريط البحث -->
        <div class="search-container" id="searchContainer">
            <div class="search-input-container">
                <input type="text" class="search-input" id="searchInput" placeholder="ابحث في الدردشات...">
                <i class="fas fa-search"></i>
            </div>
        </div>

        <!-- شريط التبويبات -->
        <div class="wa-tabs-bar">
            <button class="wa-tab active" onclick="switchTab('friends')">الدردشات</button>
            <button class="wa-tab" onclick="switchTab('profile')">الملف الشخصي</button>
        </div>

        <!-- تبويب الدردشات -->
        <div class="tab-content active" id="friendsTab">
            <div class="friends-list" id="friendsList">
                <div class="empty-state">
                    لا يوجد دردشات بعد. اضغط على الزر الأخضر بالأسفل لإضافة صديق.
                </div>
            </div>
        </div>

        <!-- تبويب الملف الشخصي -->
        <div class="tab-content" id="profileTab">
            <div class="card">
                <div class="profile-section">
                    <div class="profile-picture-container">
                        <div class="profile-picture" id="profilePicture">
                            <i class="fas fa-user" id="profilePictureIcon"></i>
                        </div>
                        <label class="change-picture-btn">
                            تغيير الصورة
                            <input type="file" id="profilePictureInput" accept="image/*">
                        </label>
                    </div>

                    <h2>👤 ملفك الشخصي</h2>
                    <div class="status" id="statusBar" 
                         style="background: #f0f0f0; padding: 10px; border-radius: 8px; margin-bottom: 12px; 
                                border-left: 4px solid #667eea; font-weight: 600; font-size: 0.9em;">
                        متصل
                    </div>

                    <div class="input-group">
                        <label>اسمك:</label>
                        <input type="text" id="userName" placeholder="أدخل اسمك">
                    </div>

                    <div class="input-group">
                        <label>معرفك:</label>
                        <input type="text" id="userId" placeholder="معرف فريد (مثال: ali123)">
                    </div>

                    <button class="profile-button save" onclick="saveProfile()">
                        💾 حفظ الملف
                    </button>
                </div>

                <div class="profile-section">
                    <h2>👥 إضافة صديق</h2>
                    <div class="input-group">
                        <label>معرف الصديق:</label>
                        <input type="text" id="friendId" placeholder="أدخل معرف الصديق">
                    </div>
                    <button class="profile-button add-friend" onclick="addFriend()">
                        ➕ إضافة صديق
                    </button>
                </div>
            </div>
        </div>

        <!-- تبويب الدردشة الفعلية -->
        <div class="tab-content" id="chatTab">
            <div class="chat-header">
                <button onclick="goBackToChats()"><i class="fas fa-arrow-right"></i></button>
                <div class="friend-avatar" id="currentFriendAvatar">
                    <i class="fas fa-user"></i>
                    <div class="online-status" id="currentFriendStatus"></div>
                </div>
                <div>
                    <div id="currentFriendName" class="friend-name-chat"></div>
                    <div class="chat-online-status" id="chatOnlineStatus">غير متصل</div>
                </div>
            </div>

            <div class="chat-messages" id="chatMessages">
                <div class="empty-state">
                    اختر صديقاً لبدء المحادثة
                </div>
            </div>

            <!-- مسجل الصوت -->
            <div class="voice-recorder" id="voiceRecorder">
                <div class="recording-indicator">
                    <div class="recording-dot"></div>
                    <div class="recording-timer" id="recordingTimer">00:00</div>
                </div>
                <div class="recording-controls">
                    <button class="cancel-recording" onclick="cancelRecording()">إلغاء</button>
                    <button class="send-recording" onclick="sendRecording()">إرسال</button>
                </div>
            </div>

            <div class="chat-input-area">
                <button class="emoji-button send-btn" onclick="toggleEmojiPicker()">
                    <i class="far fa-smile"></i>
                </button>
                <div class="emoji-picker" id="emojiPicker"></div>
                <button class="image-button send-btn" onclick="document.getElementById('imageInput').click()">
                    <i class="fas fa-image"></i>
                </button>
                <input type="file" id="imageInput" accept="image/*" style="display: none;">
                <button class="voice-button send-btn" onclick="toggleVoiceInput()" id="voiceButton">
                    <i class="fas fa-microphone"></i>
                </button>
                <input type="text" id="messageInput" placeholder="اكتب رسالتك..." onkeypress="handleKeyPress(event)">
                <button class="send-btn" onclick="sendMessage()">
                    <i class="fas fa-paper-plane"></i>
                </button>
            </div>
        </div>

        <!-- زر الإضافة العائم -->
        <button class="fab" onclick="switchTab('profile'); document.getElementById('friendId').focus();">
            <i class="fas fa-user-plus"></i>
        </button>
    </div>

    <script type="module">
        import { initializeApp } from 'https://www.gstatic.com/firebasejs/10.7.0/firebase-app.js';
        import { 
            getDatabase, 
            ref, 
            set, 
            push, 
            onValue, 
            onDisconnect, 
            serverTimestamp 
        } from 'https://www.gstatic.com/firebasejs/10.7.0/firebase-database.js';

        // ==================== 1. تهيئة Firebase ====================
        const firebaseConfig = {
            apiKey: "AIzaSyDmSmnCtNs1uerbN2P2oIKPrG3YcbbAVJ8",
            authDomain: "mwrx-f3818.firebaseapp.com",
            databaseURL: "https://mwrx-f3818-default-rtdb.firebaseio.com",
            projectId: "mwrx-f3818",
            storageBucket: "mwrx-f3818.firebasestorage.app",
            messagingSenderId: "505642264850",
            appId: "1:505642264850:web:23f60b42376db721d7ebcb",
            measurementId: "G-EYKM20DTTN"
        };

        let app, db;
        let currentChatWith = null;
        let currentChatName = null;
        let appData = {
            userId: '',
            userName: '',
            profilePicture: '',
            friends: [],
            messages: {},
            notificationSettings: {
                browser: false,
                chat: true,
                group: true
            }
        };

        let notificationSettings = {
            browserEnabled: false,
            chatEnabled: true,
            groupEnabled: true,
            permission: 'default'
        };

        // متغيرات لتسجيل الصوت
        let mediaRecorder = null;
        let audioChunks = [];
        let recordingTimer = null;
        let recordingSeconds = 0;
        let isRecording = false;
        let currentAudioBlob = null;

        // ==================== 2. مجموعة الإيموجي الكاملة ====================
        const emojiCategories = {
            "وجوه": ["😀", "😃", "😄", "😁", "😆", "😅", "😂", "🤣", "😊", "😇", "🙂", "🙃", "😉", "😌", "😍", "🥰", "😘", "😗", "😙", "😚", "😋", "😛", "😝", "😜", "🤪", "🤨", "🧐", "🤓", "😎", "🤩", "🥳", "😏", "😒", "😞", "😔", "😟", "😕", "🙁", "☹️", "😣", "😖", "😫", "😩", "🥺", "😢", "😭", "😤", "😠", "😡", "🤬", "🤯", "😳", "🥵", "🥶", "😱", "😨", "😰", "😥", "😓", "🤗", "🤔", "🤭", "🤫", "🤥", "😶", "😐", "😑", "😬", "🙄", "😯", "😦", "😧", "😮", "😲", "🥱", "😴", "🤤", "😪", "😵", "🤐", "🥴", "🤢", "🤮", "🤧", "😷", "🤒", "🤕"],
            "قلوب": ["❤️", "🧡", "💛", "💚", "💙", "💜", "🖤", "🤍", "🤎", "💔", "❣️", "💕", "💞", "💓", "💗", "💖", "💘", "💝", "💟"],
            "إيماءات": ["👍", "👎", "👊", "✊", "🤛", "🤜", "🤞", "✌️", "🤟", "🤘", "👌", "👈", "👉", "👆", "👇", "☝️", "✋", "🤚", "🖐️", "🖖", "👋", "🤙", "💪", "🦾", "🦿", "🦵", "🦶", "👂", "🦻", "👃", "🧠", "🦷", "🦴", "👀", "👁️", "👅", "👄", "💋", "🩸"],
            "حيوانات": ["🐵", "🐒", "🦍", "🦧", "🐶", "🐕", "🦮", "🐩", "🐺", "🦊", "🦝", "🐱", "🐈", "🦁", "🐯", "🐅", "🐆", "🐴", "🐎", "🦄", "🦓", "🦌", "🐮", "🐂", "🐃", "🐄", "🐷", "🐖", "🐗", "🐽", "🐏", "🐑", "🐐", "🐪", "🐫", "🦙", "🦒", "🐘", "🦏", "🦛", "🐭", "🐁", "🐀", "🐹", "🐰", "🐇", "🐿️", "🦔", "🦇", "🐻", "🐨", "🐼", "🦥", "🦦", "🦨", "🦘", "🦡", "🐾", "🦃", "🐔", "🐓", "🐣", "🐤", "🐥", "🐦", "🐧", "🕊️", "🦅", "🦆", "🦢", "🦉", "🦩", "🦚", "🦜", "🐸", "🐊", "🐢", "🦎", "🐍", "🐲", "🐉", "🦕", "🦖", "🐳", "🐋", "🐬", "🐟", "🐠", "🐡", "🦈", "🐙", "🐚", "🐌", "🦋", "🐛", "🐜", "🐝", "🐞", "🦗", "🕷️", "🕸️", "🦂", "🦟", "🦠"],
            "طعام": ["🍏", "🍎", "🍐", "🍊", "🍋", "🍌", "🍉", "🍇", "🍓", "🫐", "🍈", "🍒", "🍑", "🥭", "🍍", "🥥", "🥝", "🍅", "🍆", "🥑", "🥦", "🥬", "🥒", "🌶️", "🫑", "🌽", "🥕", "🫒", "🧄", "🧅", "🥔", "🍠", "🥐", "🥯", "🍞", "🥖", "🥨", "🧀", "🥚", "🍳", "🧈", "🥞", "🧇", "🥓", "🥩", "🍗", "🍖", "🦴", "🌭", "🍔", "🍟", "🍕", "🫓", "🥪", "🥙", "🧆", "🌮", "🌯", "🫔", "🥗", "🥘", "🫕", "🥫", "🍝", "🍜", "🍲", "🍛", "🍣", "🍱", "🥟", "🦪", "🍤", "🍙", "🍚", "🍘", "🍥", "🥠", "🥮", "🍢", "🍡", "🍧", "🍨", "🍦", "🥧", "🧁", "🍰", "🎂", "🍮", "🍭", "🍬", "🍫", "🍿", "🍩", "🍪", "🌰", "🥜", "🍯", "🥛", "🍼", "🫖", "☕", "🍵", "🧃", "🥤", "🍶", "🍺", "🍻", "🥂", "🍷", "🥃", "🍸", "🍹", "🧉", "🍾", "🧊", "🥄", "🍴", "🍽️", "🥣", "🥡", "🥢"],
            "أنشطة": ["⚽", "🏀", "🏈", "⚾", "🥎", "🎾", "🏐", "🏉", "🥏", "🎱", "🪀", "🏓", "🏸", "🏒", "🏑", "🥍", "🏏", "🪃", "🥅", "⛳", "🪁", "🏹", "🎣", "🤿", "🥊", "🥋", "🎽", "🛹", "🛼", "🛷", "⛸️", "🥌", "🎿", "⛷️", "🏂", "🪂", "🏋️", "🤼", "🤸", "⛹️", "🤾", "🏌️", "🏇", "🧘", "🏄", "🏊", "🤽", "🚣", "🧗", "🚵", "🚴", "🏆", "🥇", "🥈", "🥉", "🏅", "🎖️", "🏵️", "🎗️", "🎫", "🎟️", "🎪", "🤹", "🎭", "🩰", "🎨", "🎬", "🎤", "🎧", "🎼", "🎹", "🥁", "🪘", "🎷", "🎺", "🪗", "🎸", "🪕", "🎻", "🎲", "♟️", "🎯", "🎳", "🎮", "🎰"]
        };

        // ==================== 3. تهيئة التطبيق ====================
        try {
            app = initializeApp(firebaseConfig);
            db = getDatabase(app);
            updateStatus('متصل', true);

            const connectedRef = ref(db, '.info/connected');
            onValue(connectedRef, (snapshot) => {
                updateStatus(snapshot.val() === true ? 'متصل' : 'غير متصل', snapshot.val() === true);
            });
        } catch (error) {
            updateStatus('غير متصل', false);
        }

        // ==================== 4. وظائف التنقل والواجهة ====================
        window.switchTab = function(tabId) {
            // تحديث أزرار التبويبات
            document.querySelectorAll('.wa-tab').forEach(t => t.classList.remove('active'));
            const selectedTabBtn = document.querySelector(`.wa-tab[onclick="switchTab('${tabId}')"]`);
            if (selectedTabBtn) selectedTabBtn.classList.add('active');

            // تحديث محتوى التبويبات
            document.querySelectorAll('.tab-content').forEach(c => {
                c.classList.remove('active');
                if (c.id === 'chatTab') {
                    c.style.transform = 'translateX(100%)';
                }
            });

            const selectedContent = document.getElementById(`${tabId}Tab`);
            if (selectedContent) {
                selectedContent.classList.add('active');
                if (tabId === 'chat') {
                    selectedContent.style.transform = 'translateX(0)';
                }
            }

            // التحكم في زر الإضافة العائم
            const fab = document.querySelector('.fab');
            if (fab) {
                fab.style.display = (tabId === 'friends' && currentChatWith === null) ? 'flex' : 'none';
            }
            
            // إغلاق القائمة المنبثقة
            document.getElementById('dotsMenu').classList.remove('active');
            
            // إغلاق شريط البحث
            document.getElementById('searchContainer').classList.remove('active');
            
            // إغلاق منتقي الإيموجي
            document.getElementById('emojiPicker').classList.remove('show');
        };

        window.goBackToChats = function() {
            currentChatWith = null;
            currentChatName = null;
            switchTab('friends');
            updateFriendsList();
        };

        // ==================== 5. إدارة البحث ====================
        window.toggleSearch = function() {
            const searchContainer = document.getElementById('searchContainer');
            searchContainer.classList.toggle('active');
            
            if (searchContainer.classList.contains('active')) {
                document.getElementById('searchInput').focus();
            }
        };

        document.getElementById('searchInput').addEventListener('input', function(e) {
            const searchTerm = e.target.value.toLowerCase();
            const friendItems = document.querySelectorAll('.friend-item');
            
            friendItems.forEach(item => {
                const friendName = item.querySelector('.friend-name').textContent.toLowerCase();
                if (friendName.includes(searchTerm)) {
                    item.style.display = 'flex';
                } else {
                    item.style.display = 'none';
                }
            });
        });

        // ==================== 6. القائمة المنبثقة (النقاط الثلاث) ====================
        window.toggleDotsMenu = function() {
            const menu = document.getElementById('dotsMenu');
            menu.classList.toggle('active');
        };

        // إغلاق القائمة عند النقر خارجها
        document.addEventListener('click', function(event) {
            const menu = document.getElementById('dotsMenu');
            const menuButton = document.querySelector('.dots-menu button');
            
            if (!menu.contains(event.target) && event.target !== menuButton && !menuButton.contains(event.target)) {
                menu.classList.remove('active');
            }
            
            // إغلاق منتقي الإيموجي عند النقر خارجها
            const emojiPicker = document.getElementById('emojiPicker');
            const emojiButton = document.querySelector('.emoji-button');
            if (emojiPicker.classList.contains('show') && !emojiPicker.contains(event.target) && event.target !== emojiButton && !emojiButton.contains(event.target)) {
                emojiPicker.classList.remove('show');
            }
        });

        window.showSettings = function() {
            showNotification('قسم الإعدادات قيد التطوير', 'info');
            document.getElementById('dotsMenu').classList.remove('active');
        };

        // ==================== 7. إدارة المستخدم والملف الشخصي ====================
        window.logout = function() {
            if (db && appData.userId) {
                const userStatusRef = ref(db, `users/${appData.userId}/online`);
                set(userStatusRef, false);
            }

            localStorage.removeItem('appData');
            location.reload();
        };

        function updateStatus(message, isOnline) {
            const statusBar = document.getElementById('statusBar');
            if (statusBar) {
                statusBar.textContent = isOnline ? 'متصل' : 'غير متصل';
                statusBar.className = isOnline ? 'status online' : 'status';
            }
        }

        window.saveProfile = function() {
            const userName = document.getElementById('userName').value.trim();
            const userId = document.getElementById('userId').value.trim();

            if (!userName || !userId) {
                showNotification('الرجاء إدخال الاسم والمعرف', 'error');
                return;
            }

            const oldUserId = appData.userId;
            appData.userName = userName;
            appData.userId = userId;

            appData.notificationSettings = {
                browser: notificationSettings.browserEnabled,
                chat: notificationSettings.chatEnabled,
                group: notificationSettings.groupEnabled
            };

            localStorage.setItem('appData', JSON.stringify(appData));

            if (db) {
                if (oldUserId && oldUserId !== userId) {
                    location.reload();
                    return;
                }

                const userRef = ref(db, `users/${userId}`);
                set(userRef, {
                    name: userName,
                    profilePicture: appData.profilePicture,
                    lastSeen: serverTimestamp(),
                    online: true
                });
            }

            showNotification('تم حفظ الملف الشخصي', 'success');
        };

        // ==================== 8. إدارة صورة الملف الشخصي ====================
        document.getElementById('profilePictureInput').addEventListener('change', function(e) {
            const file = e.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(event) {
                    const profilePicture = document.getElementById('profilePicture');
                    const profilePictureIcon = document.getElementById('profilePictureIcon');
                    
                    // إخفاء الأيقونة وإظهار الصورة
                    profilePictureIcon.style.display = 'none';
                    profilePicture.style.backgroundImage = `url(${event.target.result})`;
                    
                    // حفظ الصورة في البيانات المحلية
                    appData.profilePicture = event.target.result;
                    localStorage.setItem('appData', JSON.stringify(appData));
                    
                    showNotification('تم تغيير صورة الملف الشخصي', 'success');
                };
                reader.readAsDataURL(file);
            }
        });

        // ==================== 9. إدارة الأصدقاء وحالة الاتصال ====================
        function updateFriendsList() {
            const friendsList = document.getElementById('friendsList');

            if (!appData.friends || appData.friends.length === 0) {
                friendsList.innerHTML = '<div class="empty-state">لا يوجد دردشات بعد. اضغط على الزر الأخضر بالأسفل لإضافة صديق.</div>';
                return;
            }

            friendsList.innerHTML = '';
            appData.friends.forEach(friend => {
                const friendId = typeof friend === 'string' ? friend : friend.id;
                const friendName = typeof friend === 'string' ? friend : (friend.name || friend.id);
                const friendPicture = friend.profilePicture || '';

                const item = document.createElement('div');
                item.className = `friend-item ${currentChatWith === friendId ? 'active' : ''}`;
                item.onclick = () => selectFriend(friendId, friendName, friendPicture);

                const avatarContent = friendPicture 
                    ? `<img src="${friendPicture}" alt="${friendName}">`
                    : `<i class="fas fa-user"></i>`;

                item.innerHTML = `
                    <div class="friend-avatar">
                        ${avatarContent}
                        <div class="online-status offline" id="status-${friendId}"></div>
                    </div>
                    <div class="friend-info">
                        <div class="friend-name">${friendName}</div>
                        <div class="friend-last-message">اضغط لبدء الدردشة...</div>
                    </div>
                    <div class="friend-meta">
                        <span>${new Date().toLocaleTimeString('ar', { hour: '2-digit', minute: '2-digit' })}</span>
                    </div>
                `;

                friendsList.appendChild(item);
                
                // مراقبة حالة الاتصال للصديق
                if (db) {
                    const friendStatusRef = ref(db, `users/${friendId}/online`);
                    onValue(friendStatusRef, (snapshot) => {
                        const isOnline = snapshot.val();
                        const statusElement = document.getElementById(`status-${friendId}`);
                        if (statusElement) {
                            statusElement.className = isOnline ? 'online-status' : 'online-status offline';
                        }
                        
                        // تحديث حالة الصديق في رأس الدردشة إذا كان محادثة حالية
                        if (currentChatWith === friendId) {
                            const chatStatusElement = document.getElementById('chatOnlineStatus');
                            const currentFriendStatusElement = document.getElementById('currentFriendStatus');
                            if (chatStatusElement && currentFriendStatusElement) {
                                chatStatusElement.textContent = isOnline ? 'متصل' : 'غير متصل';
                                currentFriendStatusElement.className = isOnline ? 'online-status' : 'online-status offline';
                            }
                        }
                    });
                }
            });
        }

        window.selectFriend = function(friendId, friendName, friendPicture) {
            currentChatWith = friendId;
            currentChatName = friendName;

            document.getElementById('currentFriendName').textContent = friendName || friendId;
            
            // تحديث صورة الصديق في رأس الدردشة
            const currentFriendAvatar = document.getElementById('currentFriendAvatar');
            if (friendPicture) {
                currentFriendAvatar.innerHTML = `<img src="${friendPicture}" alt="${friendName}"><div class="online-status offline" id="currentFriendStatus"></div>`;
            } else {
                currentFriendAvatar.innerHTML = `<i class="fas fa-user"></i><div class="online-status offline" id="currentFriendStatus"></div>`;
            }
            
            // تحديث حالة الاتصال للصديق
            if (db) {
                const friendStatusRef = ref(db, `users/${friendId}/online`);
                onValue(friendStatusRef, (snapshot) => {
                    const isOnline = snapshot.val();
                    const chatStatusElement = document.getElementById('chatOnlineStatus');
                    const currentFriendStatusElement = document.getElementById('currentFriendStatus');
                    if (chatStatusElement && currentFriendStatusElement) {
                        chatStatusElement.textContent = isOnline ? 'متصل' : 'غير متصل';
                        currentFriendStatusElement.className = isOnline ? 'online-status' : 'online-status offline';
                    }
                });
            }
            
            switchTab('chat');
            loadMessages();

            showNotification(`تتحدث الآن مع ${friendName || friendId}`, 'info');
        };

        window.addFriend = function() {
            const friendId = document.getElementById('friendId').value.trim();

            if (!friendId) {
                showNotification('الرجاء إدخال معرف الصديق', 'error');
                return;
            }

            if (friendId === appData.userId) {
                showNotification('لا يمكنك إضافة نفسك!', 'error');
                return;
            }

            const exists = appData.friends.some(f => typeof f === 'string' ? f === friendId : f.id === friendId);
            if (exists) {
                showNotification('هذا الصديق مضاف مسبقاً', 'info');
                return;
            }

            appData.friends.push({ id: friendId, name: friendId });
            localStorage.setItem('appData', JSON.stringify(appData));

            document.getElementById('friendId').value = '';
            updateFriendsList();
            showNotification('تم إضافة الصديق', 'success');

            // جلب بيانات الصديق من Firebase
            if (db) {
                const friendRef = ref(db, `users/${friendId}`);
                onValue(friendRef, (snapshot) => {
                    const data = snapshot.val();
                    if (data) {
                        const friendIndex = appData.friends.findIndex(f => f.id === friendId);
                        if (friendIndex !== -1) {
                            appData.friends[friendIndex].name = data.name || friendId;
                            appData.friends[friendIndex].profilePicture = data.profilePicture || '';
                            localStorage.setItem('appData', JSON.stringify(appData));
                            updateFriendsList();
                        }
                    }
                }, { onlyOnce: true });
            }
        };

        // ==================== 10. إدارة الرسائل ====================
        window.sendMessage = function() {
            const input = document.getElementById('messageInput');
            const text = input.value.trim();

            if (!text) return;

            if (!currentChatWith) {
                showNotification('الرجاء اختيار صديق أولاً', 'error');
                return;
            }

            const messageObj = {
                text: text,
                from: appData.userId,
                to: currentChatWith,
                timestamp: Date.now(),
                senderName: appData.userName,
                type: 'text'
            };

            if (db) {
                const chatId = [appData.userId, currentChatWith].sort().join('_');
                const messagesRef = ref(db, `chats/${chatId}`);
                push(messagesRef, messageObj);
            }

            input.value = '';
            document.getElementById('emojiPicker').classList.remove('show');
        };

        function loadMessages() {
            const chatMessages = document.getElementById('chatMessages');
            chatMessages.innerHTML = '<div class="empty-state">جاري تحميل الرسائل...</div>';

            if (!currentChatWith || !appData.userId) return;

            const chatId = [appData.userId, currentChatWith].sort().join('_');

            if (db) {
                const messagesRef = ref(db, `chats/${chatId}`);

                onValue(messagesRef, (snapshot) => {
                    const data = snapshot.val();
                    chatMessages.innerHTML = '';

                    if (!data) {
                        chatMessages.innerHTML = '<div class="empty-state">ابدأ المحادثة بإرسال رسالة 👋</div>';
                        return;
                    }

                    const messages = Object.values(data).sort((a, b) => a.timestamp - b.timestamp);

                    messages.forEach(msg => {
                        let messageElement;
                        const isSent = msg.from === appData.userId;

                        if (msg.type === 'voice') {
                            // عرض رسالة صوتية
                            messageElement = document.createElement('div');
                            messageElement.className = `voice-message ${isSent ? 'sent' : 'received'}`;
                            
                            messageElement.innerHTML = `
                                <div class="voice-duration">${msg.duration || '0:00'}</div>
                                <div class="voice-waveform">
                                    <div class="voice-wave"></div>
                                </div>
                                <button class="voice-play-btn" onclick="playVoiceMessage('${msg.audioData || ''}')">
                                    <i class="fas fa-play"></i>
                                </button>
                            `;
                        } else if (msg.type === 'image') {
                            // عرض رسالة صورة
                            messageElement = document.createElement('div');
                            messageElement.className = `image-message ${isSent ? 'sent' : 'received'}`;
                            
                            messageElement.innerHTML = `
                                <img src="${msg.imageData}" alt="صورة">
                                ${msg.caption ? `<div class="image-caption">${msg.caption}</div>` : ''}
                            `;
                        } else {
                            // عرض رسالة نصية
                            messageElement = document.createElement('div');
                            messageElement.className = `message ${isSent ? 'sent' : 'received'}`;
                            messageElement.textContent = msg.text;

                            const time = new Date(msg.timestamp).toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
                            const timeSpan = document.createElement('span');
                            timeSpan.style.fontSize = '0.7em';
                            timeSpan.style.opacity = '0.7';
                            timeSpan.style.display = 'block';
                            timeSpan.style.marginTop = '4px';
                            timeSpan.style.textAlign = isSent ? 'left' : 'right';
                            timeSpan.textContent = time;

                            messageElement.appendChild(timeSpan);
                        }

                        chatMessages.appendChild(messageElement);

                        // إشعار بالرسائل الجديدة
                        if (!isSent && Date.now() - msg.timestamp < 2000) {
                            notifyNewMessage(msg);
                        }
                    });

                    chatMessages.scrollTop = chatMessages.scrollHeight;
                });
            }
        }

        window.handleKeyPress = function(event) {
            if (event.key === 'Enter') {
                sendMessage();
            }
        };

        // ==================== 11. الإيموجي - النسخة المحسنة ====================
        function initEmojiPicker() {
            const picker = document.getElementById('emojiPicker');
            picker.innerHTML = ''; // تنظيف المحتوى القديم
            
            // إضافة الإيموجي حسب التصنيفات
            Object.keys(emojiCategories).forEach(category => {
                // إضافة عنوان التصنيف
                const categoryTitle = document.createElement('div');
                categoryTitle.className = 'emoji-category';
                categoryTitle.textContent = category;
                picker.appendChild(categoryTitle);
                
                // إضافة إيموجي التصنيف
                emojiCategories[category].forEach(emoji => {
                    const span = document.createElement('span');
                    span.className = 'emoji-item';
                    span.textContent = emoji;
                    span.title = emoji;
                    span.onclick = () => {
                        const input = document.getElementById('messageInput');
                        input.value += emoji;
                        input.focus();
                    };
                    picker.appendChild(span);
                });
            });
        }

        window.toggleEmojiPicker = function() {
            const picker = document.getElementById('emojiPicker');
            picker.classList.toggle('show');
        };

        // ==================== 12. إرسال الصور ====================
        document.getElementById('imageInput').addEventListener('change', function(e) {
            const file = e.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(event) {
                    if (!currentChatWith) {
                        showNotification('الرجاء اختيار صديق أولاً', 'error');
                        return;
                    }

                    const caption = prompt('أدخل تعليقاً للصورة (اختياري):') || '';
                    
                    const messageObj = {
                        type: 'image',
                        from: appData.userId,
                        to: currentChatWith,
                        timestamp: Date.now(),
                        senderName: appData.userName,
                        imageData: event.target.result,
                        caption: caption
                    };

                    if (db) {
                        const chatId = [appData.userId, currentChatWith].sort().join('_');
                        const messagesRef = ref(db, `chats/${chatId}`);
                        push(messagesRef, messageObj);
                    }

                    showNotification('تم إرسال الصورة', 'success');
                    
                    // إعادة تعيين حقل الإدخال
                    document.getElementById('imageInput').value = '';
                };
                reader.readAsDataURL(file);
            }
        });

        // ==================== 13. تسجيل الصوت ====================
        window.toggleVoiceInput = async function() {
            if (isRecording) {
                stopRecording();
                return;
            }

            try {
                // إيقاف أي تسجيل سابق
                if (mediaRecorder && mediaRecorder.state === 'recording') {
                    mediaRecorder.stop();
                }
                
                const stream = await navigator.mediaDevices.getUserMedia({ 
                    audio: {
                        echoCancellation: true,
                        noiseSuppression: true,
                        sampleRate: 44100
                    } 
                });
                startRecording(stream);
            } catch (error) {
                console.error('Error accessing microphone:', error);
                showNotification('لا يمكن الوصول إلى الميكروفون. يرجى التحقق من الأذونات.', 'error');
            }
        };

        function startRecording(stream) {
            // إعادة تهيئة المتغيرات
            audioChunks = [];
            recordingSeconds = 0;
            isRecording = true;
            currentAudioBlob = null;

            try {
                mediaRecorder = new MediaRecorder(stream, { mimeType: 'audio/webm' });
                
                // تحديث واجهة المستخدم
                document.getElementById('voiceRecorder').classList.add('active');
                document.getElementById('voiceButton').innerHTML = '<i class="fas fa-stop"></i>';
                document.getElementById('voiceButton').style.backgroundColor = '#f44336';

                // بدء التسجيل
                mediaRecorder.start(100); // جمع البيانات كل 100 مللي ثانية

                // بدء المؤقت
                recordingTimer = setInterval(() => {
                    recordingSeconds++;
                    const minutes = Math.floor(recordingSeconds / 60);
                    const seconds = recordingSeconds % 60;
                    document.getElementById('recordingTimer').textContent = 
                        `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
                }, 1000);

                // جمع البيانات الصوتية
                mediaRecorder.ondataavailable = (event) => {
                    if (event.data.size > 0) {
                        audioChunks.push(event.data);
                    }
                };

                mediaRecorder.onstop = () => {
                    if (audioChunks.length > 0) {
                        currentAudioBlob = new Blob(audioChunks, { type: 'audio/webm' });
                    }
                    // إيقاف جميع tracks في الـ stream
                    stream.getTracks().forEach(track => track.stop());
                };

                mediaRecorder.onerror = (event) => {
                    console.error('MediaRecorder error:', event.error);
                    showNotification('حدث خطأ في التسجيل الصوتي', 'error');
                    stopRecording();
                };

            } catch (error) {
                console.error('Error starting recording:', error);
                showNotification('لا يمكن بدء التسجيل الصوتي', 'error');
                stopRecording();
            }
        }

        function stopRecording() {
            if (mediaRecorder && isRecording) {
                try {
                    if (mediaRecorder.state === 'recording') {
                        mediaRecorder.stop();
                    }
                    clearInterval(recordingTimer);
                    isRecording = false;

                    // إعادة تعيين واجهة المستخدم
                    document.getElementById('voiceButton').innerHTML = '<i class="fas fa-microphone"></i>';
                    document.getElementById('voiceButton').style.backgroundColor = '#f0f0f0';
                } catch (error) {
                    console.error('Error stopping recording:', error);
                }
            }
        }

        window.cancelRecording = function() {
            stopRecording();
            document.getElementById('voiceRecorder').classList.remove('active');
            audioChunks = [];
            currentAudioBlob = null;
            showNotification('تم إلغاء التسجيل', 'info');
        };

        window.sendRecording = function() {
            if (!currentChatWith) {
                showNotification('الرجاء اختيار صديق أولاً', 'error');
                return;
            }

            if (!currentAudioBlob) {
                // محاولة إنشاء blob من audioChunks إذا لم يكن currentAudioBlob موجوداً
                if (audioChunks.length === 0) {
                    showNotification('لا يوجد تسجيل صوتي', 'error');
                    return;
                }
                currentAudioBlob = new Blob(audioChunks, { type: 'audio/webm' });
            }

            const reader = new FileReader();
            
            reader.onload = function() {
                const audioData = reader.result;
                
                const messageObj = {
                    type: 'voice',
                    from: appData.userId,
                    to: currentChatWith,
                    timestamp: Date.now(),
                    senderName: appData.userName,
                    audioData: audioData,
                    duration: `${Math.floor(recordingSeconds / 60)}:${(recordingSeconds % 60).toString().padStart(2, '0')}`
                };

                if (db) {
                    const chatId = [appData.userId, currentChatWith].sort().join('_');
                    const messagesRef = ref(db, `chats/${chatId}`);
                    push(messagesRef, messageObj);
                }

                document.getElementById('voiceRecorder').classList.remove('active');
                audioChunks = [];
                currentAudioBlob = null;
                
                showNotification('تم إرسال التسجيل الصوتي', 'success');
            };
            
            reader.readAsDataURL(currentAudioBlob);
        };

        // ==================== 14. تشغيل الرسائل الصوتية ====================
        window.playVoiceMessage = function(audioData) {
            try {
                const audio = new Audio(audioData);
                audio.play().catch(error => {
                    console.error('Error playing audio:', error);
                    showNotification('تعذر تشغيل التسجيل الصوتي', 'error');
                });
            } catch (error) {
                console.error('Error playing voice message:', error);
                showNotification('تعذر تشغيل التسجيل الصوتي', 'error');
            }
        };

        // ==================== 15. نظام الإشعارات ====================
        window.showNotification = function(message, type = 'info') {
            const notification = document.createElement('div');
            notification.className = `notification ${type}`;
            notification.textContent = message;
            
            document.body.appendChild(notification);
            
            setTimeout(() => {
                notification.remove();
            }, 4000);
        };

        function notifyNewMessage(message) {
            if (!notificationSettings.chatEnabled) return;
            
            if (notificationSettings.browserEnabled && Notification.permission === 'granted') {
                new Notification(`رسالة جديدة من ${message.senderName}`, {
                    body: message.type === 'text' ? message.text : 'رسالة جديدة',
                    icon: '/favicon.ico'
                });
            }
        }

        // ==================== 16. إدارة إعدادات الإشعارات ====================
        window.toggleBrowserNotifications = function() {
            notificationSettings.browserEnabled = !notificationSettings.browserEnabled;
            
            if (notificationSettings.browserEnabled) {
                if (Notification.permission === 'default') {
                    Notification.requestPermission().then(permission => {
                        notificationSettings.permission = permission;
                        if (permission !== 'granted') {
                            notificationSettings.browserEnabled = false;
                            document.getElementById('browserNotificationsToggle').checked = false;
                            showNotification('تم رفض الإذن لعرض الإشعارات', 'warning');
                        } else {
                            showNotification('تم تفعيل إشعارات المتصفح', 'success');
                        }
                    });
                } else if (Notification.permission === 'granted') {
                    showNotification('تم تفعيل إشعارات المتصفح', 'success');
                } else {
                    notificationSettings.browserEnabled = false;
                    document.getElementById('browserNotificationsToggle').checked = false;
                    showNotification('الإذن مرفوض لعرض الإشعارات', 'warning');
                }
            } else {
                showNotification('تم إيقاف إشعارات المتصفح', 'info');
            }
            
            saveNotificationSettings();
        };

        window.toggleChatNotifications = function() {
            notificationSettings.chatEnabled = !notificationSettings.chatEnabled;
            showNotification(notificationSettings.chatEnabled ? 
                'تم تفعيل إشعارات الدردشات' : 'تم إيقاف إشعارات الدردشات', 'info');
            saveNotificationSettings();
        };

        window.toggleGroupNotifications = function() {
            notificationSettings.groupEnabled = !notificationSettings.groupEnabled;
            showNotification(notificationSettings.groupEnabled ? 
                'تم تفعيل إشعارات المجموعات' : 'تم إيقاف إشعارات المجموعات', 'info');
            saveNotificationSettings();
        };

        window.testNotifications = function() {
            if (notificationSettings.browserEnabled && Notification.permission === 'granted') {
                new Notification('اختبار الإشعارات', {
                    body: 'هذا إشعار تجريبي من Connetto',
                    icon: '/favicon.ico'
                });
                showNotification('تم إرسال إشعار تجريبي', 'success');
            } else {
                showNotification('إشعارات المتصفح غير مفعلة', 'warning');
            }
        };

        function saveNotificationSettings() {
            if (appData.userId) {
                appData.notificationSettings = {
                    browser: notificationSettings.browserEnabled,
                    chat: notificationSettings.chatEnabled,
                    group: notificationSettings.groupEnabled
                };
                localStorage.setItem('appData', JSON.stringify(appData));
            }
        }

        // ==================== 17. تهيئة التطبيق عند التحميل ====================
        document.addEventListener('DOMContentLoaded', function() {
            // تحميل البيانات المحفوظة
            const savedData = localStorage.getItem('appData');
            if (savedData) {
                try {
                    appData = JSON.parse(savedData);
                    
                    // تحديث واجهة المستخدم بالبيانات المحفوظة
                    if (appData.userName) {
                        document.getElementById('userName').value = appData.userName;
                    }
                    if (appData.userId) {
                        document.getElementById('userId').value = appData.userId;
                    }
                    if (appData.profilePicture) {
                        const profilePicture = document.getElementById('profilePicture');
                        const profilePictureIcon = document.getElementById('profilePictureIcon');
                        profilePictureIcon.style.display = 'none';
                        profilePicture.style.backgroundImage = `url(${appData.profilePicture})`;
                    }
                    
                    // تحميل إعدادات الإشعارات
                    if (appData.notificationSettings) {
                        notificationSettings.browserEnabled = appData.notificationSettings.browser || false;
                        notificationSettings.chatEnabled = appData.notificationSettings.chat !== undefined ? appData.notificationSettings.chat : true;
                        notificationSettings.groupEnabled = appData.notificationSettings.group !== undefined ? appData.notificationSettings.group : true;
                        
                        document.getElementById('browserNotificationsToggle').checked = notificationSettings.browserEnabled;
                        document.getElementById('chatNotificationsToggle').checked = notificationSettings.chatEnabled;
                        document.getElementById('groupNotificationsToggle').checked = notificationSettings.groupEnabled;
                    }
                    
                    // إخفاء شاشة تسجيل الدخول
                    document.getElementById('loginOverlay').style.display = 'none';
                    document.getElementById('appContainer').style.display = 'block';
                    
                    // تحديث قائمة الأصدقاء
                    updateFriendsList();
                    
                } catch (error) {
                    console.error('Error loading saved data:', error);
                }
            }
            
            // تهيئة منتقي الإيموجي
            initEmojiPicker();
            
            // طلب إذن الإشعارات
            if ('Notification' in window && Notification.permission === 'default') {
                Notification.requestPermission();
            }
        });

        // ==================== 18. نظام تسجيل الدخول ====================
        document.getElementById('loginForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const username = document.getElementById('loginUsername').value.trim();
            const password = document.getElementById('loginPassword').value.trim();
            
            if (!username || !password) {
                document.getElementById('loginError').textContent = 'الرجاء إدخال اسم المستخدم وكلمة المرور';
                document.getElementById('loginError').style.display = 'block';
                return;
            }
            
            // محاكاة عملية تسجيل الدخول (في تطبيق حقيقي، ستكون هناك اتصال بالخادم)
            if (password.length < 3) {
                document.getElementById('loginError').textContent = 'كلمة المرور يجب أن تكون 3 أحرف على الأقل';
                document.getElementById('loginError').style.display = 'block';
                return;
            }
            
            // حفظ بيانات المستخدم الأساسية
            appData.userName = username;
            appData.userId = username.toLowerCase().replace(/\s+/g, '');
            appData.friends = [];
            
            localStorage.setItem('appData', JSON.stringify(appData));
            
            // إخفاء شاشة تسجيل الدخول وإظهار التطبيق
            document.getElementById('loginOverlay').style.display = 'none';
            document.getElementById('appContainer').style.display = 'block';
            
            // تحديث واجهة المستخدم
            document.getElementById('userName').value = username;
            document.getElementById('userId').value = appData.userId;
            
            showNotification(`مرحباً ${username}!`, 'success');
            
            // تسجيل المستخدم في Firebase إذا كان متصلاً
            if (db) {
                const userRef = ref(db, `users/${appData.userId}`);
                set(userRef, {
                    name: username,
                    profilePicture: appData.profilePicture,
                    lastSeen: serverTimestamp(),
                    online: true
                });
            }
        });

        // ==================== 19. منع الإرسال إذا لم يكن هناك محادثة نشطة ====================
        function validateChatActive() {
            if (!currentChatWith) {
                showNotification('الرجاء اختيار صديق لبدء المحادثة', 'warning');
                return false;
            }
            return true;
        }

        // ==================== 20. تحسين تجربة المستخدم ====================
        // إغلاق القوائم عند التمرير
        document.addEventListener('scroll', function() {
            document.getElementById('dotsMenu').classList.remove('active');
            document.getElementById('emojiPicker').classList.remove('show');
        });

        // تحديث حالة الاتصال عند إغلاق الصفحة
        window.addEventListener('beforeunload', function() {
            if (db && appData.userId) {
                const userStatusRef = ref(db, `users/${appData.userId}/online`);
                set(userStatusRef, false);
            }
        });

        // تحديث واجهة المستخدم عند التركيز على حقل الإدخال
        document.getElementById('messageInput').addEventListener('focus', function() {
            document.getElementById('emojiPicker').classList.remove('show');
        });

        // تحسين عرض الوقت في الرسائل
        function formatTime(timestamp) {
            const date = new Date(timestamp);
            const now = new Date();
            const diff = now - date;
            
            if (diff < 24 * 60 * 60 * 1000) {
                return date.toLocaleTimeString('ar', { hour: '2-digit', minute: '2-digit' });
            } else {
                return date.toLocaleDateString('ar') + ' ' + date.toLocaleTimeString('ar', { hour: '2-digit', minute: '2-digit' });
            }
        }

        console.log('تم تحميل تطبيق Connetto بنجاح!');
    </script>
</body>
</html>
