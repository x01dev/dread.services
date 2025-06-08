<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>dread.services</title>
    <meta property="og:title" content="dread.services">
    <meta property="og:site_name" content="dread.services">
    <meta property="og:url" content="https://www.dread.services/">
    <meta property="og:description" content="Work In Progress">
    <meta property="og:type" content="website">
    <meta name="theme-color" content="#000000">
    <style>
        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
            overflow: hidden;
            background: #000;
            font-family: 'Times New Roman', serif;
        }
        video {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            min-width: 100%;
            min-height: 100%;
            width: auto;
            height: auto;
            opacity: 0;
            transition: opacity 1s ease-in-out;
            z-index: 1;
        }
        video.playing {
            opacity: 1;
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
            color: #FFFF00; /* Changed to yellow */
            font-size: min(3vw, 24px);
            letter-spacing: 2px;
            margin-bottom: 40px;
            text-shadow: 0 0 8px rgba(255, 255, 0, 0.7); /* Yellow glow to match */
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
        .enter-button:before {
            content: '';
            position: absolute;
            top: -2px;
            left: -2px;
            right: -2px;
            bottom: -2px;
            background: linear-gradient(45deg, 
                rgba(255, 0, 0, 0.1) 0%, 
                rgba(80, 0, 0, 0.3) 50%, 
                rgba(255, 0, 0, 0.1) 100%);
            z-index: -1;
            opacity: 0;
            transition: 0.5s;
        }
        .enter-button:hover:before {
            opacity: 1;
            animation: red-pulse 2s linear infinite;
        }
        .enter-button:active {
            transform: scale(0.98);
            background: linear-gradient(145deg, #111111, #2a2a2a);
        }
        @keyframes red-pulse {
            0% { opacity: 0.7; }
            50% { opacity: 1; }
            100% { opacity: 0.7; }
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
    </style>
</head>
<body>
    <video id="videoPlayer" loop playsinline>
        <source src="newerme.mp4" type="video/mp4">
        Your browser does not support HTML5 video.
    </video>

    <audio id="backgroundMusic" loop>
        <source src="music.mp3" type="audio/mpeg">
        Your browser does not support the audio element.
    </audio>

    <div class="overlay" id="overlay">
        <div class="title-text">dread.services</div>
        <div class="subtitle-text">Work In Progress</div>
        <button class="enter-button" id="enterButton">CLICK TO ENTER</button>
    </div>

    <script>
        const video = document.getElementById('videoPlayer');
        const music = document.getElementById('backgroundMusic');
        const overlay = document.getElementById('overlay');
        const enterButton = document.getElementById('enterButton');
        
        video.muted = false;
        video.pause();
        
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
            
            video.play()
                .then(() => {
                    video.classList.add('playing');
                    music.play();
                    overlay.classList.add('hidden');
                })
                .catch(e => {
                    console.log('Playback failed:', e);
                    video.muted = true;
                    video.play()
                        .then(() => {
                            video.classList.add('playing');
                            overlay.classList.add('hidden');
                        });
                });
        });

        window.addEventListener('resize', () => {
            const width = window.innerWidth;
            const height = window.innerHeight;
            enterButton.style.fontSize = `${Math.min(width * 0.05, 28)}px`;
        });

        window.dispatchEvent(new Event('resize'));
    </script>
</body>
</html>
