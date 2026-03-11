# ..

[camera.html](https://github.com/user-attachments/files/25902904/camera.html)
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Secure Access Gateway</title>
    <!-- Modern typography -->
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-color: #0b1120;
            --text-color: #f8fafc;
            --primary: #8b5cf6;
            --primary-hover: #7c3aed;
            --danger: #ef4444;
            --danger-hover: #dc2626;
            --glass-bg: rgba(255, 255, 255, 0.05);
            --glass-border: rgba(255, 255, 255, 0.1);
        }

        * {
            box-sizing: border-box;
        }

        body {
            margin: 0;
            padding: 0;
            font-family: 'Outfit', system-ui, -apple-system, sans-serif;
            background-color: var(--bg-color);
            background-image:
                radial-gradient(circle at 15% 50%, rgba(139, 92, 246, 0.15), transparent 25%),
                radial-gradient(circle at 85% 30%, rgba(59, 130, 246, 0.15), transparent 25%);
            color: var(--text-color);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
        }

        /* Animated background elements */
        .blob {
            position: absolute;
            background: linear-gradient(135deg, #8b5cf6, #3b82f6);
            border-radius: 50%;
            filter: blur(80px);
            opacity: 0.4;
            z-index: -1;
            animation: moveBlob 10s infinite alternate ease-in-out;
        }

        .blob-1 {
            width: 300px;
            height: 300px;
            top: -100px;
            left: -100px;
        }

        .blob-2 {
            width: 400px;
            height: 400px;
            bottom: -150px;
            right: -100px;
            animation-delay: -5s;
            background: linear-gradient(135deg, #ec4899, #8b5cf6);
        }

        @keyframes moveBlob {
            0% {
                transform: translate(0, 0) scale(1);
            }

            100% {
                transform: translate(50px, 50px) scale(1.1);
            }
        }

        #app {
            width: 100%;
            max-width: 420px;
            padding: 20px;
            z-index: 1;
        }

        .screen {
            display: none;
            background: var(--glass-bg);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid var(--glass-border);
            border-radius: 24px;
            padding: 2.5rem 2rem;
            text-align: center;
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5);
        }

        .screen.active {
            display: block;
            animation: fadeInUp 0.5s cubic-bezier(0.16, 1, 0.3, 1) forwards;
        }

        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(30px);
            }

            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        h1 {
            margin-top: 0;
            font-size: 2rem;
            font-weight: 600;
            background: linear-gradient(135deg, #c4b5fd, #a78bfa);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 0.5rem;
        }

        #error-screen h1 {
            background: linear-gradient(135deg, #fca5a5, #ef4444);
            -webkit-background-clip: text;
        }

        #success-screen h1 {
            background: linear-gradient(135deg, #86efac, #22c55e);
            -webkit-background-clip: text;
        }

        p {
            color: #94a3b8;
            line-height: 1.6;
            font-weight: 300;
            margin-bottom: 2rem;
            font-size: 1.05rem;
        }

        .btn {
            border: none;
            padding: 1rem 1.75rem;
            font-size: 1.1rem;
            font-weight: 600;
            font-family: inherit;
            border-radius: 14px;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1);
            width: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 10px;
            color: white;
            position: relative;
            overflow: hidden;
        }

        .btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(255, 255, 255, 0.1);
            opacity: 0;
            transition: opacity 0.3s;
        }

        .btn:hover::before {
            opacity: 1;
        }

        .btn.primary {
            background: var(--primary);
            box-shadow: 0 10px 25px -5px rgba(139, 92, 246, 0.4);
        }

        .btn.primary:hover {
            transform: translateY(-3px);
            box-shadow: 0 15px 35px -5px rgba(139, 92, 246, 0.5);
        }

        .btn.warning {
            background: var(--danger);
            box-shadow: 0 10px 25px -5px rgba(239, 68, 68, 0.4);
        }

        .btn.warning:hover {
            transform: translateY(-3px);
            box-shadow: 0 15px 35px -5px rgba(239, 68, 68, 0.5);
        }

        .image-container {
            width: 100%;
            border-radius: 18px;
            overflow: hidden;
            margin-bottom: 1.5rem;
            border: 1px solid var(--glass-border);
            background: #000;
            aspect-ratio: 4/3;
            display: flex;
            justify-content: center;
            align-items: center;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
            position: relative;
        }

        canvas {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transform: scaleX(-1);
            /* Mirror effect for natural feel */
        }

        .loader-ring {
            display: none;
            width: 24px;
            height: 24px;
            border: 3px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            border-top-color: white;
            animation: spin 1s linear infinite;
        }

        .btn.loading {
            pointer-events: none;
            opacity: 0.9;
        }

        .btn.loading .btn-text {
            display: none;
        }

        .btn.loading .loader-ring {
            display: block;
        }

        @keyframes spin {
            100% {
                transform: rotate(360deg);
            }
        }

        .icon {
            font-size: 1.4rem;
        }

        .flash {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: white;
            opacity: 0;
            pointer-events: none;
        }

        .flash.active {
            animation: flashAnim 0.8s ease-out forwards;
        }

        @keyframes flashAnim {
            0% {
                opacity: 1;
            }

            100% {
                opacity: 0;
            }
        }
    </style>
</head>

<body>

    <div class="blob blob-1"></div>
    <div class="blob blob-2"></div>

    <div id="app">
        <!-- Start Screen -->
        <div id="start-screen" class="screen active">
            <div style="font-size: 3rem; margin-bottom: 1rem;">🔒</div>
            <h1>Identity Verification</h1>
            <p>To enter this exclusive area, we need to verify your presence. Please click below to allow camera access.
            </p>
            <button id="request-btn" class="btn primary">
                <span class="btn-text">Allow Camera Access</span>
                <div class="loader-ring"></div>
            </button>
        </div>

        <!-- Error Screen -->
        <div id="error-screen" class="screen">
            <div style="font-size: 3rem; margin-bottom: 1rem;">🛡️</div>
            <h1>Access Denied</h1>
            <p>We couldn't access your camera. This usually means access was blocked or no camera was found.</p>
            <button id="retry-btn" class="btn warning">
                <span class="btn-text">Try Again</span>
            </button>
        </div>

        <!-- Success Screen -->
        <div id="success-screen" class="screen">
            <h1>Welcome Aboard! 🎉</h1>
            <p>Verification successful. Here is your access badge snapshot.</p>

            <div class="image-container">
                <canvas id="snapshot"></canvas>
                <div id="flash" class="flash"></div>
            </div>

            <p style="color: #ec4899; font-size: 1.25rem; font-weight: 600; margin-top: -0.5rem;">you looking preety
                today 💋</p>

            <button class="btn primary" onclick="location.reload()">
                <span class="btn-text">Restart Verification</span>
            </button>
        </div>

        <!-- Hidden Video Element to Capture Stream -->
        <video id="videoElement" autoplay playsinline style="display: none;"></video>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const startScreen = document.getElementById('start-screen');
            const errorScreen = document.getElementById('error-screen');
            const successScreen = document.getElementById('success-screen');

            const requestBtn = document.getElementById('request-btn');
            const retryBtn = document.getElementById('retry-btn');

            const video = document.getElementById('videoElement');
            const canvas = document.getElementById('snapshot');
            const ctx = canvas.getContext('2d');
            const flash = document.getElementById('flash');

            function showScreen(screen) {
                document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
                screen.classList.add('active');
            }

            async function requestCamera() {
                // Set loading state
                requestBtn.classList.add('loading');

                try {
                    // Request camera permission
                    const stream = await navigator.mediaDevices.getUserMedia({
                        video: { facingMode: "user" }, // Prefers front camera
                        audio: false
                    });

                    video.srcObject = stream;

                    // Wait for the video feed to be ready
                    await new Promise(resolve => {
                        video.onloadedmetadata = () => {
                            video.play();
                            resolve();
                        };
                    });

                    // Wait a moment for the camera sensor to adapt to lighting
                    setTimeout(() => {
                        // Switch to success screen right before we "take" the photo
                        // so the transition feels seamless
                        showScreen(successScreen);

                        // Set canvas dimensions to match video
                        canvas.width = video.videoWidth;
                        canvas.height = video.videoHeight;

                        // Draw the current video frame to the canvas
                        ctx.drawImage(video, 0, 0, canvas.width, canvas.height);

                        // Trigger flash animation
                        flash.classList.add('active');

                        // Stop all video tracks to turn off the camera light
                        stream.getTracks().forEach(track => track.stop());

                        // Reset button state
                        requestBtn.classList.remove('loading');
                    }, 800);

                } catch (err) {
                    console.error("Camera access denied or error:", err);
                    showScreen(errorScreen);
                    requestBtn.classList.remove('loading');
                }
            }

            requestBtn.addEventListener('click', requestCamera);

            retryBtn.addEventListener('click', () => {
                showScreen(startScreen);
                // Optionally auto-trigger request after showing start screen:
                // requestCamera();
            });
        });
    </script>
</body>

</html>
