<!DOCTYPE html>
<html lang="en">
<head>
    <!-- Add this first to prevent FOUC (Flash of Unstyled Content) -->
    <style>
        html { 
            background-color: #000;
        }
        body {
            visibility: hidden;
            opacity: 0;
            margin: 0;
            padding: 0;
            height: 100%;
            overflow: hidden;
            background: #000;
            font-family: 'Times New Roman', serif;
        }
    </style>
    
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <title>dread.services</title>
    <style>
        .youtube-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 145%;
            opacity: 0;
            transition: opacity 1s ease-in-out;
            z-index: 1;
            pointer-events: none;
        }
        .youtube-container.playing {
            opacity: 1;
        }
        .youtube-iframe {
            width: 100%;
            height: 100%;
            border: none;
            transform: translateY(-15%);
        }
        .overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: #000;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 2;
        }
        .overlay.hidden {
            opacity: 0;
            pointer-events: none;
            transition: opacity 1s ease-in-out;
        }
        .title-text {
            color: #ffffff;
            font-size: min(5vw, 36px);
            text-transform: uppercase;
            letter-spacing: 4px;
            margin-bottom: 20px;
            text-shadow: 0 0 8px rgba(255, 255, 255, 0.5);
        }
        .subtitle-text {
            color: #FFFF00;
            font-size: min(3vw, 24px);
            letter-spacing: 2px;
            margin-bottom: 40px;
            text-shadow: 0 0 8px rgba(255, 255, 0, 0.7);
        }
        .enter-button {
            padding: 20px 40px;
            font-size: min(5vw, 28px);
            font-weight: bold;
            color: #ffffff;
            background: linear-gradient(145deg, #1a1a1a, #333333);
            border: 2px solid #444;
            border-top: 2px solid #666;
            border-bottom: 2px solid #222;
            border-radius: 0;
            text-transform: uppercase;
            letter-spacing: 4px;
            cursor: pointer;
            position: relative;
            overflow: hidden;
            box-shadow: 0 0 20px rgba(255, 0, 0, 0.7);
            transition: all 0.3s ease;
            text-shadow: 0 0 8px rgba(255, 255, 255, 0.5);
            min-width: 250px;
            text-align: center;
        }
        .enter-button:hover {
            box-shadow: 0 0 30px rgba(255, 0, 0, 0.9);
            transform: scale(1.05);
            text-shadow: 0 0 12px rgba(255, 255, 255, 0.8);
        }
        .click-animation {
            position: absolute;
            width: 100px;
            height: 100px;
            background: radial-gradient(circle, 
                rgba(255, 0, 0, 0.8) 0%, 
                rgba(255, 0, 0, 0) 70%);
            border-radius: 50%;
            transform: scale(0);
            opacity: 0;
            pointer-events: none;
            mix-blend-mode: screen;
        }
        .animate {
            animation: ripple 1s ease-out;
        }
        @keyframes ripple {
            to {
                transform: scale(3);
                opacity: 0;
            }
        }
        .login-button {
            position: fixed;
            bottom: 50px;
            right: 20px;
            padding: 10px 20px;
            font-size: 16px;
            font-weight: bold;
            color: #ffffff;
            background: linear-gradient(145deg, #1a1a1a, #333333);
            border: 2px solid #444;
            border-top: 2px solid #666;
            border-bottom: 2px solid #222;
            border-radius: 0;
            text-transform: uppercase;
            letter-spacing: 2px;
            cursor: pointer;
            z-index: 100;
            box-shadow: 0 0 10px rgba(255, 0, 0, 0.5);
            transition: all 0.3s ease;
            text-shadow: 0 0 5px rgba(255, 255, 255, 0.5);
            opacity: 0;
            transition: opacity 0.5s ease;
        }
        .login-button.visible {
            opacity: 1;
        }
        .login-button:hover {
            box-shadow: 0 0 15px rgba(255, 0, 0, 0.8);
            transform: scale(1.05);
        }
        .login-page {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: #000;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.5s ease;
        }
        .login-page.visible {
            opacity: 1;
            pointer-events: all;
        }
        .login-title {
            color: #ffffff;
            font-size: min(5vw, 36px);
            text-transform: uppercase;
            letter-spacing: 4px;
            margin-bottom: 40px;
            text-shadow: 0 0 8px rgba(255, 255, 255, 0.5);
        }
        .input-container {
            margin-bottom: 20px;
            text-align: center;
        }
        .input-label {
            display: block;
            color: #FFFF00;
            font-size: min(3vw, 18px);
            margin-bottom: 8px;
            text-shadow: 0 0 8px rgba(255, 255, 0, 0.7);
        }
        .input-field {
            padding: 15px 25px;
            font-size: min(4vw, 20px);
            color: #ffffff;
            background: linear-gradient(145deg, #1a1a1a, #333333);
            border: 2px solid #444;
            border-top: 2px solid #666;
            border-bottom: 2px solid #222;
            border-radius: 0;
            text-transform: uppercase;
            letter-spacing: 2px;
            min-width: 250px;
            text-align: center;
            box-shadow: 0 0 10px rgba(255, 0, 0, 0.5);
            transition: all 0.3s ease;
        }
        .input-field:focus {
            outline: none;
            box-shadow: 0 0 15px rgba(255, 0, 0, 0.8);
        }
        .login-submit {
            margin-top: 30px;
            padding: 15px 30px;
            font-size: min(4vw, 20px);
            font-weight: bold;
            color: #ffffff;
            background: linear-gradient(145deg, #1a1a1a, #333333);
            border: 2px solid #444;
            border-top: 2px solid #666;
            border-bottom: 2px solid #222;
            border-radius: 0;
            text-transform: uppercase;
            letter-spacing: 2px;
            cursor: pointer;
            box-shadow: 0 0 10px rgba(255, 0, 0, 0.5);
            transition: all 0.3s ease;
        }
        .login-submit:hover {
            box-shadow: 0 0 20px rgba(255, 0, 0, 0.9);
            transform: scale(1.05);
        }
        .login-message {
            margin-top: 20px;
            color: #FF0000;
            font-size: min(3vw, 16px);
            text-shadow: 0 0 8px rgba(255, 0, 0, 0.7);
            height: 20px;
        }
        .close-login {
            position: absolute;
            top: 20px;
            right: 20px;
            color: #ffffff;
            font-size: 24px;
            cursor: pointer;
            text-shadow: 0 0 8px rgba(255, 0, 0, 0.7);
        }
        .logged-in-page {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: #000;
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            color: #ffffff;
            font-size: min(10vw, 72px);
            text-transform: uppercase;
            letter-spacing: 8px;
            text-shadow: 0 0 20px rgba(255, 0, 0, 0.9);
            opacity: 0;
            pointer-events: none;
            transition: opacity 1s ease;
        }
        .logged-in-page.visible {
            opacity: 1;
            pointer-events: all;
        }
    </style>
</head>
<body>
    <!-- Preloader (hidden by default, shown only if needed) -->
    <div id="preloader" style="position:fixed;top:0;left:0;width:100%;height:100%;background:#000;z-index:9999;display:none;"></div>

    <div id="youtubeContainer" class="youtube-container"></div>

    <audio id="backgroundMusic" loop>
        <source src="music.mp3" type="audio/mpeg">
    </audio>

    <div class="overlay" id="overlay">
        <div class="title-text">dread.services</div>
        <div class="subtitle-text">Work In Progress</div>
        <button class="enter-button" id="enterButton">CLICK TO ENTER</button>
    </div>

    <button class="login-button" id="loginButton">LOGIN</button>
    
    <div class="login-page" id="loginPage">
        <span class="close-login" id="closeLogin">&times;</span>
        <div class="login-title">LOGIN</div>
        
        <div class="input-container">
            <label class="input-label" for="username">ENTER USERNAME</label>
            <input type="text" id="username" class="input-field">
        </div>
        
        <div class="input-container">
            <label class="input-label" for="password">ENTER PASSWORD</label>
            <input type="password" id="password" class="input-field">
        </div>
        
        <button class="login-submit" id="loginSubmit">LOGIN</button>
        <div class="login-message" id="loginMessage"></div>
    </div>

    <div class="logged-in-page" id="loggedInPage">
        LOGGED IN
    </div>

    <script>
        // Secure credential storage
        const SECURE = {
            user: '1',
            pass: '1',
            token: 'dread_' + btoa('r00do:admin').split('').reverse().join('') + '_' 
                 + Math.random().toString(36).substring(2, 15)
        };

        // Authentication functions
        function setAuth() {
            const now = new Date();
            now.setTime(now.getTime() + (60 * 60 * 1000)); // 1 hour expiration
            document.cookie = `dread_auth=${SECURE.token}; expires=${now.toUTCString()}; path=/; SameSite=Strict`;
            localStorage.setItem('dread_auth', SECURE.token);
        }

        function checkAuth() {
            const cookieAuth = document.cookie.split(';').some(item => 
                item.trim().startsWith('dread_auth=' + SECURE.token));
            const storageAuth = localStorage.getItem('dread_auth') === SECURE.token;
            return cookieAuth || storageAuth;
        }

        function clearAuth() {
            document.cookie = 'dread_auth=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/';
            localStorage.removeItem('dread_auth');
        }

        // DOM Ready
        document.addEventListener("DOMContentLoaded", () => {
            // Show the page content smoothly
            setTimeout(() => {
                document.body.style.visibility = "visible";
                document.body.style.opacity = "1";
                document.getElementById('preloader').style.display = 'none';
            }, 50);
            
            // Hide login button initially
            document.getElementById('loginButton').style.display = 'none';
            
            // If already logged in, show logged in page
            if (checkAuth()) {
                document.getElementById('loggedInPage').classList.add('visible');
                document.getElementById('overlay').style.display = 'none';
                // Start video and music if already logged in
                loadYouTubeVideo();
                music.play();
                // Show login button immediately if already logged in
                loginButton.style.display = 'block';
                loginButton.classList.add('visible');
            }
        });

        // Element references
        const youtubeContainer = document.getElementById('youtubeContainer');
        const music = document.getElementById('backgroundMusic');
        const overlay = document.getElementById('overlay');
        const enterButton = document.getElementById('enterButton');
        const loginButton = document.getElementById('loginButton');
        const loginPage = document.getElementById('loginPage');
        const closeLogin = document.getElementById('closeLogin');
        const loginSubmit = document.getElementById('loginSubmit');
        const usernameInput = document.getElementById('username');
        const passwordInput = document.getElementById('password');
        const loginMessage = document.getElementById('loginMessage');
        const loggedInPage = document.getElementById('loggedInPage');

        // YouTube video loader function
        function loadYouTubeVideo() {
            youtubeContainer.innerHTML = `
                <iframe class="youtube-iframe" 
                    src="https://www.youtube.com/embed/sKu8Zg7Hplc?autoplay=1&controls=0&disablekb=1&fs=0&loop=1&modestbranding=1&playsinline=1&rel=0&showinfo=0&mute=1&iv_load_policy=3&cc_load_policy=0&playlist=sKu8Zg7Hplc&enablejsapi=1" 
                    frameborder="0" 
                    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
                    allowfullscreen>
                </iframe>`;
            
            // Add slight delay before showing to ensure it's loaded
            setTimeout(() => {
                youtubeContainer.classList.add('playing');
            }, 300);
        }

        // Enter button click
        enterButton.addEventListener('click', (e) => {
            // Click animation
            const ripple = document.createElement('div');
            ripple.classList.add('click-animation');
            const rect = enterButton.getBoundingClientRect();
            ripple.style.left = (e.clientX - rect.left - 50) + 'px';
            ripple.style.top = (e.clientY - rect.top - 50) + 'px';
            enterButton.appendChild(ripple);
            
            setTimeout(() => {
                ripple.classList.add('animate');
            }, 10);
            
            setTimeout(() => {
                ripple.remove();
            }, 1000);
            
            // Load YouTube video and play music immediately
            loadYouTubeVideo();
            music.play();
            
            // Hide the enter button and show the login button
            enterButton.style.display = 'none';
            loginButton.style.display = 'block';
            setTimeout(() => {
                loginButton.classList.add('visible');
            }, 500);
            
            // Optionally fade out the overlay (but keep it in DOM for potential login)
            overlay.classList.add('hidden');
        });

        // Login button click
        loginButton.addEventListener('click', () => {
            loginPage.classList.add('visible');
        });
        
        // Close login page
        closeLogin.addEventListener('click', () => {
            loginPage.classList.remove('visible');
            loginMessage.textContent = '';
        });
        
        // Login submit
        loginSubmit.addEventListener('click', () => {
            const username = usernameInput.value.trim();
            const password = passwordInput.value.trim();
            
            if (!username || !password) {
                loginMessage.textContent = 'Please enter both username and password';
                return;
            }
            
            if (username === SECURE.user && password === SECURE.pass) {
                loginMessage.textContent = 'Login successful!';
                loginMessage.style.color = '#00FF00';
                setAuth();
                setTimeout(() => {
                    loginPage.classList.remove('visible');
                    loginButton.classList.remove('visible');
                    
                    // Show logged in page after a delay
                    setTimeout(() => {
                        loggedInPage.classList.add('visible');
                    }, 1000);
                    
                    usernameInput.value = '';
                    passwordInput.value = '';
                }, 1000);
            } else {
                loginMessage.textContent = 'Invalid credentials';
                loginMessage.style.color = '#FF0000';
            }
        });
        
        // Enter key for password field
        passwordInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                loginSubmit.click();
            }
        });

        // Responsive font sizing
        window.addEventListener('resize', () => {
            const width = window.innerWidth;
            enterButton.style.fontSize = `${Math.min(width * 0.05, 28)}px`;
        });

        window.dispatchEvent(new Event('resize'));
        
        // Anti-tampering measures
        document.addEventListener('contextmenu', e => e.preventDefault());
        
        document.addEventListener('keydown', e => {
            // Block F12, Ctrl+Shift+I, Ctrl+Shift+J, Ctrl+Shift+C
            if (e.key === 'F12' || 
                (e.ctrlKey && e.shiftKey && ['I','J','C'].includes(e.key))) {
                e.preventDefault();
                alert('Access restricted');
                return false;
            }
            
            // Block Ctrl+U (View Source)
            if (e.ctrlKey && e.key === 'u') {
                e.preventDefault();
                return false;
            }
        });

        // Extra protection against console access
        setInterval(() => {
            if (window.console && console.log) {
                console.log = function() {};
                console.warn = function() {};
                console.error = function() {};
            }
        }, 1000);
    </script>
</body>
</html>
