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
        }
        video {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            object-fit: cover; /* Ensures video covers full screen */
        }
    </style>
</head>
<body>
    <!-- Video (autoplay, loop, unmuted) -->
    <video id="videoPlayer" autoplay loop playsinline>
        <source src="newerme.mp4" type="video/mp4">
        Your browser does not support HTML5 video.
    </video>

    <script>
        const video = document.getElementById('videoPlayer');
        
        // Unmute the video (required for autoplay with sound in some browsers)
        video.muted = false;
        
        // Some browsers require a user interaction to play audio.
        // This triggers a fake click on the page to enable sound.
        document.addEventListener('click', () => {
            video.play().catch(e => console.log('Autoplay blocked:', e));
        });
        
        // Attempt to autoplay with sound (may still fail without user interaction)
        video.play().catch(e => {
            console.log('Autoplay with sound blocked. Click the page to enable.');
        });
    </script>
</body>
</html>
