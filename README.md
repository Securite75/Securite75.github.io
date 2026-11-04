

<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SECURITY QUIZ</title>

    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background: #0a0d14;
            color: white;
            font-family: Arial, sans-serif;
            overflow: hidden;
        }

        .container {
            text-align: center;
            animation: appear 1s ease-out;
        }

        /* Animation de chargement */

        .loader {
            width: 100px;
            height: 100px;
            position: relative;
            margin: 0 auto 35px;
        }

        .circle {
            position: absolute;
            border-radius: 50%;
            border: 3px solid transparent;
        }

        .circle.one {
            inset: 0;
            border-top-color: #00bfff;
            border-right-color: #007bff;
            animation: rotate 1.5s linear infinite;
            box-shadow: 0 0 15px rgba(0, 174, 255, 0.2);
        }

        .circle.two {
            inset: 12px;
            border-bottom-color: #00e5ff;
            border-left-color: #008cff;
            animation: rotate-reverse 2s linear infinite;
        }

        .circle.three {
            inset: 25px;
            border: 2px solid rgba(0, 174, 255, 0.2);
            border-top-color: #ffffff;
            animation: rotate 1s linear infinite;
        }

        .center-dot {
            position: absolute;
            width: 8px;
            height: 8px;
            background: #00c8ff;
            border-radius: 50%;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            box-shadow: 0 0 15px #00c8ff;
            animation: pulse 1.5s ease-in-out infinite;
        }

        /* Texte */

        h1 {
            font-size: 32px;
            font-weight: 700;
            letter-spacing: 4px;
            text-shadow: 0 0 15px rgba(0, 174, 255, 0.4);
        }

        p {
            margin-top: 15px;
            color: #788599;
            font-size: 13px;
            letter-spacing: 2px;
            animation: pulse 2s ease-in-out infinite;
        }

        .line {
            width: 140px;
            height: 2px;
            background: linear-gradient(90deg, transparent, #00bfff, transparent);
            margin: 25px auto 0;
            animation: glow 2s ease-in-out infinite;
        }

        /* Animations */

        @keyframes rotate {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }

        @keyframes rotate-reverse {
            from { transform: rotate(360deg); }
            to { transform: rotate(0deg); }
        }

        @keyframes pulse {
            0%, 100% { opacity: 0.5; }
            50% { opacity: 1; }
        }

        @keyframes glow {
            0%, 100% { opacity: 0.4; }
            50% { opacity: 1; }
        }

        @keyframes appear {
            from {
                opacity: 0;
                transform: translateY(15px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        /* Accessibilité */

        @media (prefers-reduced-motion: reduce) {
            *, *::before, *::after {
                animation-duration: 0.01ms !important;
                animation-iteration-count: 1 !important;
            }
        }
    </style>
</head>

<body>

    <div class="container">

        <div class="loader">
            <div class="circle one"></div>
            <div class="circle two"></div>
            <div class="circle three"></div>
            <div class="center-dot"></div>
        </div>

        <h1>SECURITY QUIZ</h1>

        <p>SITE EN COURS DE PRÉPARATION...</p>

        <div class="line"></div>

    </div>

</body>
</html>


