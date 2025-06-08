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
            background-color: black;
        }
        video {
            width: 100%;
            height: 100%;
            object-fit: contain;
        }
    </style>
</head>
<body>
    <video autoplay loop muted playsinline>
        <source src="newerme.mp4" type="video/mp4">
        Your browser does not support the video tag.
    </video>

    <script>
        // Ensure video plays with audio (unmute if needed)
        document.querySelector('video').muted = false;
    </script>
</body>
</html>
