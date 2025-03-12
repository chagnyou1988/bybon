<!DOCTYPE html>
<html lang="zh">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>故障排查系统</title>
    <style>
        /* 样式部分保持不变 */
    </style>
</head>

<body>
    <!-- 登录表单 -->
    <div id="loginForm" class="container">
        <h1>故障排查系统</h1>
        <label for="userType">用户类型:</label>
        <select id="userType">
            <option value="admin">管理员</option>
            <option value="user" selected>普通用户</option>
        </select>
        <label for="password">密码:</label>
        <input type="password" id="password">
        <button onclick="login()">登录</button>
    </div>
    <!-- 管理员更改密码表单，初始隐藏 -->
    <div id="changePasswordForm" class="container" style="display: none;">
        <h1>更改用户密码</h1>
        <label for="newPassword">新密码:</label>
        <input type="password" id="newPassword">
        <button onclick="changePassword()">更改密码</button>
        <div id="menu">
            <button onclick="logout()">退出登录</button>
        </div>
    </div>
    <!-- 原故障排查系统内容，初始隐藏 -->
    <div id="troubleshootingSystem" class="container" style="display: none;">
        <h1>故障排查系统</h1>
        <p>请选择您遇到的手机故障类型：</p>
        <select id="problemType">
            <option value="powerOn">不开机问题</option>
            <option value="shortBattery">待机时间短</option>
            <option value="poorSignal">信号差</option>
            <option value="stillProblem">维修后仍旧有问题</option>
            <option value="shuaihuai">摔坏了</option>
        </select>
        <button onclick="startTroubleshooting()">开始排查</button>
        <div id="steps" style="display: none;">
            <p id="stepText"></p>
            <button onclick="nextStep()">下一步</button>
            <button onclick="restartTroubleshooting()">重新选择问题</button>
        </div>
        <div id="menu">
            <button onclick="goBackToInitialPage()">返回主菜单</button>
            <button onclick="logout()">退出登录</button>
        </div>
    </div>

    <script>
        const adminPassword = 'bybon123';
        let userPassword = localStorage.getItem('userPassword');

        if (!userPassword) {
            alert("请管理员设置初始密码！");
            document.getElementById('loginForm').style.display = 'none';
            document.getElementById('changePasswordForm').style.display = 'block';
        } else {
            document.getElementById('loginForm').style.display = 'block';
        }

        function login() {
            const userType = document.getElementById('userType').value;
            const password = document.getElementById('password').value;

            if (userType === 'admin' && password === adminPassword) {
                document.getElementById('loginForm').style.display = 'none';
                document.getElementById('changePasswordForm').style.display = 'block';
            } else if (userType === 'user' && password === userPassword) {
                document.getElementById('loginForm').style.display = 'none';
                document.getElementById('troubleshootingSystem').style.display = 'block';
            } else {
                alert('密码错误，请重试。');
            }
        }

        function changePassword() {
            const newPassword = document.getElementById('newPassword').value;
            if (newPassword.length < 6) {
                alert('密码长度至少为6个字符');
                return;
            }
            userPassword = newPassword;
            localStorage.setItem('userPassword', newPassword);
            alert('用户密码已更改。');
            document.getElementById('changePasswordForm').style.display = 'none';
            document.getElementById('loginForm').style.display = 'block';
        }

        // 其他故障排查相关函数保持不变

        document.getElementById('password').addEventListener('keydown', function (event) {
            if (event.key === 'Enter') {
                login();
            }
        });

        function goBackToInitialPage() {
            document.getElementById('steps').style.display = 'none';
            document.getElementById('troubleshootingSystem').querySelector('#menu').style.display = 'none';
        }

        function logout() {
            document.getElementById('changePasswordForm').style.display = 'none';
            document.getElementById('troubleshootingSystem').style.display = 'none';
            document.getElementById('loginForm').style.display = 'block';
        }
    </script>
</body>

</html>
