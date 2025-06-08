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
            justify-content: center;
            align-items: center;
            z-index: 2;
        }
        .overlay.hidden {
            opacity: 0;
            pointer-events: none;
            transition: opacity 1s ease-in-out;
        }
        .enter-button {
            padding: 20px 40px;
            font-size: min(5vw, 28px);
            font-weight: bold;
            color: #fff;
            background: linear-gradient(145deg, #2a2a2a, #1a1a1a);
            border: 2px solid #555;
            border-radius: 0;
            text-transform: uppercase;
            letter-spacing: 4px;
            cursor: pointer;
            position: relative;
            overflow: hidden;
            box-shadow: 0 0 20px rgba(255, 215, 0, 0.6);
            transition: all 0.3s ease;
            text-shadow: 0 0 8px #fff;
            min-width: 250px;
            text-align: center;
        }
        .enter-button:hover {
            box-shadow: 0 0 30px rgba(255, 215, 0, 0.9);
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
            transform: scale(0.98);
        }
        @keyframes glowing {
            0% { background-position: 0 0; }
            50% { background-position: 400% 0; }
            100% { background-position: 0 0; }
        }
        .click-animation {
            position: absolute;
            width: 100px;
            height: 100px;
            background: radial-gradient(circle, rgba(255,255,255,0.8) 0%, rgba(255,255,255,0) 70%);
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
    <!-- Video (loop, not autoplaying) -->
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
        <button class="enter-button" id="enterButton">CLICK TO ENTER</button>
    </div>

    <script>
        const video = document.getElementById('videoPlayer');
        const music = document.getElementById('backgroundMusic');
        const overlay = document.getElementById('overlay');
        const enterButton = document.getElementById('enterButton');
        
        // Ensure video is muted initially and not playing
        video.muted = false;
        video.pause();
        
        // Handle enter button click
        enterButton.addEventListener('click', (e) => {
            // Create ripple effect at click position
            const ripple = document.createElement('div');
            ripple.classList.add('click-animation');
            const rect = enterButton.getBoundingClientRect();
            ripple.style.left = (e.clientX - rect.left - 50) + 'px';
            ripple.style.top = (e.clientY - rect.top - 50) + 'px';
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
                    music.play();
                    overlay.classList.add('hidden');
                })
                .catch(e => {
                    console.log('Playback failed:', e);
                    // Fallback for browsers that block audio without interaction
                    video.muted = true;
                    video.play()
                        .then(() => {
                            video.classList.add('playing');
                            overlay.classList.add('hidden');
                        });
                });
        });

        // Handle window resize to maintain proper scaling
        window.addEventListener('resize', () => {
            // For video scaling
            const width = window.innerWidth;
            const height = window.innerHeight;
            
            // Adjust button size if needed
            enterButton.style.fontSize = `${Math.min(width * 0.05, 28)}px`;
        });

        // Initial sizing
        window.dispatchEvent(new Event('resize'));
    </script>
</body>
</html>
