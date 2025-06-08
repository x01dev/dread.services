<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Video Player</title>
    <style>
        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
            overflow: hidden;
            background: #000;
            font-family: 'Arial', sans-serif;
        }
        video {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            object-fit: cover;
            opacity: 0;
            transition: opacity 1s ease-in-out;
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
            justify-content: center;
            align-items: center;
            transition: opacity 1s ease-in-out;
            z-index: 10;
        }
        .overlay.hidden {
            opacity: 0;
            pointer-events: none;
        }
        .enter-button {
            padding: 15px 30px;
            font-size: 24px;
            font-weight: bold;
            color: #fff;
            background: linear-gradient(145deg, #333333, #1a1a1a);
            border: 2px solid #666;
            border-radius: 0;
            text-transform: uppercase;
            letter-spacing: 3px;
            cursor: pointer;
            position: relative;
            overflow: hidden;
            box-shadow: 0 0 15px rgba(255, 215, 0, 0.5);
            transition: all 0.3s ease;
            font-family: 'Times New Roman', serif;
            text-shadow: 0 0 5px #fff;
        }
        .enter-button:hover {
            box-shadow: 0 0 25px rgba(255, 215, 0, 0.8);
            transform: scale(1.05);
        }
        .enter-button:before {
            content: '';
            position: absolute;
            top: -2px;
            left: -2px;
            right: -2px;
            bottom: -2px;
            background: linear-gradient(45deg, #ff0000, #ff7300, #fffb00, #48ff00, #00ffd5, #002bff, #7a00ff, #ff00c8, #ff0000);
            background-size: 400%;
            z-index: -1;
            opacity: 0;
            transition: 0.5s;
            animation: glowing 20s linear infinite;
        }
        .enter-button:hover:before {
            opacity: 0.3;
        }
        .enter-button:active {
            transform: scale(0.95);
        }
        @keyframes glowing {
            0% { background-position: 0 0; }
            50% { background-position: 400% 0; }
            100% { background-position: 0 0; }
        }
        .click-animation {
            position: absolute;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle, rgba(255,255,255,0.8) 0%, rgba(255,255,255,0) 70%);
            border-radius: 50%;
            transform: scale(0);
            opacity: 0;
            pointer-events: none;
        }
        .animate {
            animation: ripple 1s ease-out;
        }
        @keyframes ripple {
            to {
                transform: scale(4);
                opacity: 0;
            }
        }
    </style>
</head>
<body>
    <!-- Video (autoplay, loop, unmuted) -->
    <video id="videoPlayer" loop playsinline>
        <source src="newerme.mp4" type="video/mp4">
        Your browser does not support HTML5 video.
    </video>

    <!-- Audio element for music -->
    <audio id="backgroundMusic" loop>
        <source src="music.mp3" type="audio/mpeg">
        Your browser does not support the audio element.
    </audio>

    <!-- Overlay with enter button -->
    <div class="overlay" id="overlay">
        <button class="enter-button" id="enterButton">Click to Enter</button>
    </div>

    <script>
        const video = document.getElementById('videoPlayer');
        const music = document.getElementById('backgroundMusic');
        const overlay = document.getElementById('overlay');
        const enterButton = document.getElementById('enterButton');
        
        // Initially mute the video (we'll unmute after user interaction)
        video.muted = true;
        
        // Check if video can autoplay
        const canAutoplay = video.play().then(() => {
            // Autoplay worked, hide overlay
            video.classList.add('playing');
            overlay.classList.add('hidden');
            video.muted = false;
            return true;
        }).catch(e => {
            // Autoplay blocked, show overlay
            console.log('Autoplay blocked:', e);
            return false;
        });
        
        // Handle enter button click
        enterButton.addEventListener('click', (e) => {
            // Create ripple effect
            const ripple = document.createElement('div');
            ripple.classList.add('click-animation');
            ripple.style.left = (e.clientX - enterButton.getBoundingClientRect().left) + 'px';
            ripple.style.top = (e.clientY - enterButton.getBoundingClientRect().top) + 'px';
            enterButton.appendChild(ripple);
            
            // Start animation
            setTimeout(() => {
                ripple.classList.add('animate');
            }, 10);
            
            // Remove ripple after animation
            setTimeout(() => {
                ripple.remove();
            }, 1000);
            
            // Play video and music
            video.play()
                .then(() => {
                    video.classList.add('playing');
                    video.muted = false;
                    music.play();
                    overlay.classList.add('hidden');
                })
                .catch(e => console.log('Playback failed:', e));
        });
        
        // Some browsers require a user interaction to play audio.
        // This triggers when clicking anywhere on the page
        document.addEventListener('click', () => {
            if (video.paused) {
                video.play()
                    .then(() => {
                        video.classList.add('playing');
                        video.muted = false;
                        music.play();
                        overlay.classList.add('hidden');
                    })
                    .catch(e => console.log('Playback failed:', e));
            }
        });
    </script>
</body>
</html>
