<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>dread.services</title>
    <style>
        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
            overflow: hidden;
            background: #000;
            font-family: 'Times New Roman', serif;
            visibility: hidden; /* Hide body initially */
        }
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
        /* Login button styles */
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
    </style>
</head>
<body>
    <div id="youtubeContainer" class="youtube-container"></div>

    <audio id="backgroundMusic" loop>
        <source src="music.mp3" type="audio/mpeg">
    </audio>

    <div class="overlay" id="overlay">
        <div class="title-text">dread.services</div>
        <div class="subtitle-text">Work In Progress</div>
        <button class="enter-button" id="enterButton">CLICK TO ENTER</button>
    </div>

    <!-- Login button -->
    <button class="login-button" id="loginButton">LOGIN</button>

    <script>
        // Wait until the DOM is fully loaded before showing content
        document.addEventListener("DOMContentLoaded", () => {
            document.body.style.visibility = "visible";
            // Show login button after a short delay
            setTimeout(() => {
                document.getElementById('loginButton').classList.add('visible');
            }, 500);
        });

        const youtubeContainer = document.getElementById('youtubeContainer');
        const music = document.getElementById('backgroundMusic');
        const overlay = document.getElementById('overlay');
        const enterButton = document.getElementById('enterButton');
        const loginButton = document.getElementById('loginButton');
        
        enterButton.addEventListener('click', (e) => {
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
            
            youtubeContainer.innerHTML = `
                <iframe class="youtube-iframe" 
                    src="https://www.youtube.com/embed/sKu8Zg7Hplc?autoplay=1&controls=0&disablekb=1&fs=0&loop=1&modestbranding=1&playsinline=1&rel=0&showinfo=0&mute=1&iv_load_policy=3&cc_load_policy=0&playlist=sKu8Zg7Hplc" 
                    frameborder="0" 
                    allow="autoplay; encrypted-media" 
                    allowfullscreen>
                </iframe>
            `;
            
            youtubeContainer.classList.add('playing');
            music.play();
            overlay.classList.add('hidden');
        });

        // Login button functionality
        loginButton.addEventListener('click', () => {
            alert('Login functionality would go here');
            // You can replace this with actual login logic
        });

        window.addEventListener('resize', () => {
            const width = window.innerWidth;
            enterButton.style.fontSize = `${Math.min(width * 0.05, 28)}px`;
        });

        window.dispatchEvent(new Event('resize'));
    </script>
</body>
</html>
