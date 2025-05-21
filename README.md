<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <meta name="screen-orientation" content="landscape">
    <title>GunPowder</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            overflow: hidden;
            font-family: Arial, sans-serif;
            background-color: #222;
            color: white;
            touch-action: none;
            position: fixed;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
        }
        
        @supports(padding: max(0px)) {
            body {
                padding-left: max(env(safe-area-inset-left), 10px);
                padding-right: max(env(safe-area-inset-right), 10px);
                padding-top: max(env(safe-area-inset-top), 10px);
                padding-bottom: max(env(safe-area-inset-bottom), 10px);
            }
        }
        
        #menu {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.9);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 200;
        }
        
        #menu h1 {
            font-size: 48px;
            color: #3498db;
            margin-bottom: 30px;
            text-shadow: 0 0 10px rgba(52, 152, 219, 0.5);
        }
        
        .menu-section {
            background-color: rgba(0, 0, 0, 0.7);
            border-radius: 10px;
            padding: 20px;
            margin: 10px;
            width: 80%;
            max-width: 500px;
        }
        
        .menu-title {
            font-size: 20px;
            color: #f1c40f;
            margin-bottom: 15px;
            text-align: center;
        }
        
        .menu-buttons {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 10px;
        }
        
        .menu-button {
            padding: 12px 20px;
            margin: 5px;
            font-size: 16px;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            min-width: 120px;
            text-align: center;
            transition: all 0.3s;
        }
        
        .menu-button:hover {
            background-color: #2980b9;
            transform: scale(1.05);
        }
        
        .menu-button.active {
            background-color: #2ecc71;
            box-shadow: 0 0 10px rgba(46, 204, 113, 0.5);
        }
        
        .main-action {
            background-color: #e74c3c;
            padding: 15px 30px;
            font-size: 18px;
            margin-top: 20px;
        }
        
        .main-action:hover {
            background-color: #c0392b;
        }

        /* New tab buttons style */
        .tab-buttons-container {
            display: flex;
            flex-direction: column;
            gap: 15px;
            width: 80%;
            max-width: 500px;
        }
        
        .tab-button {
            padding: 15px 20px;
            font-size: 18px;
            background-color: rgba(0, 0, 0, 0.7);
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            min-width: 200px;
            text-align: center;
            transition: all 0.3s;
            border: 2px solid #3498db;
        }
        
        .tab-button:hover {
            background-color: rgba(52, 152, 219, 0.3);
            transform: scale(1.03);
        }
        
        /* Modal windows for tabs */
        .modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.9);
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 300;
        }
        
        .modal-content {
            background-color: rgba(0, 0, 0, 0.8);
            border-radius: 15px;
            padding: 25px;
            width: 85%;
            max-width: 500px;
            max-height: 80vh;
            overflow-y: auto;
            position: relative;
            border: 2px solid #3498db;
        }
        
        .back-button {
            position: absolute;
            top: 15px;
            left: 15px;
            font-size: 24px;
            color: white;
            background: none;
            border: none;
            cursor: pointer;
            z-index: 10;
        }
        
        .modal-title {
            font-size: 24px;
            color: #f1c40f;
            margin-bottom: 20px;
            text-align: center;
        }
        
        /* Settings content */
        .settings-content {
            width: 100%;
        }
        
        .settings-section {
            margin-bottom: 20px;
        }
        
        .settings-section-title {
            font-size: 18px;
            color: #3498db;
            margin-bottom: 10px;
            border-bottom: 1px solid #3498db;
            padding-bottom: 5px;
        }
        
        .settings-option {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
            padding: 8px;
            background-color: rgba(0, 0, 0, 0.5);
            border-radius: 5px;
        }
        
        .settings-option-label {
            font-size: 16px;
        }
        
        .settings-option-value {
            font-size: 16px;
            color: #f1c40f;
        }
        
        .key-binding {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .key-binding-button {
            padding: 5px 10px;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 3px;
            cursor: pointer;
            min-width: 40px;
            text-align: center;
        }
        
        .reset-button {
            padding: 8px 15px;
            background-color: #e74c3c;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 15px;
            width: 100%;
        }
        
        .reset-button:hover {
            background-color: #c0392b;
        }
        
        /* Language selection modal */
        .language-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.9);
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 400;
        }
        
        .language-content {
            background-color: rgba(0, 0, 0, 0.8);
            border-radius: 15px;
            padding: 25px;
            width: 85%;
            max-width: 400px;
            max-height: 80vh;
            overflow-y: auto;
            position: relative;
            border: 2px solid #3498db;
        }
        
        .language-option {
            padding: 12px;
            margin: 8px 0;
            background-color: rgba(0, 0, 0, 0.5);
            border-radius: 5px;
            cursor: pointer;
            text-align: center;
            transition: all 0.2s;
        }
        
        .language-option:hover {
            background-color: rgba(52, 152, 219, 0.3);
        }
        
        .language-option.active {
            background-color: #2ecc71;
            box-shadow: 0 0 10px rgba(46, 204, 113, 0.5);
        }

        /* Game elements */
        #gameContainer {
            position: fixed;
            width: 100%;
            height: 100%;
            display: none;
            top: 0;
            left: 0;
        }
        
        #gameCanvas {
            display: block;
            background-color: #333;
            image-rendering: pixelated;
            width: 100%;
            height: 100%;
        }
        
        #crosshair {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 24px;
            height: 24px;
            pointer-events: none;
        }
        
        #crosshair::before, #crosshair::after {
            content: '';
            position: absolute;
            background-color: rgba(255, 255, 255, 0.9);
        }
        
        #crosshair::before {
            width: 2px;
            height: 12px;
            left: 11px;
            top: 6px;
        }
        
        #crosshair::after {
            width: 12px;
            height: 2px;
            left: 6px;
            top: 11px;
        }
        
        #hud {
            position: absolute;
            top: 10px;
            left: 10px;
            padding: 10px;
            background-color: rgba(0, 0, 0, 0.6);
            border-radius: 5px;
            max-width: 90%;
        }
        
        #healthBar {
            width: 200px;
            height: 10px;
            background-color: #444;
            border-radius: 5px;
            overflow: hidden;
            margin-bottom: 5px;
        }
        
        #healthFill {
            height: 100%;
            width: 100%;
            background: linear-gradient(to right, #e74c3c, #f39c12);
            transition: width 0.3s;
        }
        
        #ammoInfo {
            font-size: 16px;
            font-weight: bold;
            transition: color 0.3s;
        }
        
        #waveInfo {
            position: absolute;
            top: 10px;
            right: 10px;
            padding: 10px;
            background-color: rgba(0, 0, 0, 0.6);
            border-radius: 5px;
            font-size: 16px;
            max-width: 90%;
            text-align: right;
        }
        
        #weapon {
            position: absolute;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            width: 200px;
            height: 100px;
            pointer-events: none;
            background-size: contain;
            background-repeat: no-repeat;
            background-position: center bottom;
            transition: transform 0.1s;
        }
        
        #gameOver {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.8);
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 100;
        }
        
        #gameOver h1 {
            font-size: 48px;
            margin-bottom: 20px;
        }
        
        #resultStats {
            margin-bottom: 20px;
            text-align: center;
        }

        .game-over-buttons {
            display: flex;
            flex-direction: column;
            gap: 10px;
            width: 100%;
            max-width: 300px;
        }

        .hidden {
            display: none !important;
        }

        #nextLevelButton {
            background-color: #2ecc71;
        }

        #nextLevelButton:hover {
            background-color: #27ae60;
        }

        #mainMenuButton2 {
            background-color: #7f8c8d;
        }

        #mainMenuButton2:hover {
            background-color: #95a5a6;
        }
        
        #restartButton {
            padding: 15px 30px;
            font-size: 20px;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        /* Virtual Joystick Styles */
        #joystickContainer {
            position: absolute;
            bottom: 100px;
            left: 50px;
            width: 120px;
            height: 120px;
            border-radius: 50%;
            background-color: rgba(0, 0, 0, 0.3);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 100;
            touch-action: none;
        }

        #joystick {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background-color: rgba(255, 255, 255, 0.5);
            position: relative;
            touch-action: none;
        }

        .mobile-controls {
            position: absolute;
            bottom: 30px;
            right: 30px;
            z-index: 100;
            display: none;
            flex-direction: column;
            align-items: flex-end;
            gap: 15px;
        }

        .mobile-button {
            width: 80px;
            height: 80px;
            border-radius: 50%;
            background-color: rgba(0, 0, 0, 0.7);
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
            font-size: 24px;
            user-select: none;
            touch-action: none;
            border: 2px solid rgba(255, 255, 255, 0.3);
            transition: all 0.1s;
            font-weight: bold;
        }

        .mobile-button:active {
            background-color: rgba(52, 152, 219, 0.9) !important;
            transform: scale(0.95) !important;
        }

        .mobile-button.active {
            background-color: rgba(46, 204, 113, 0.7) !important;
        }

        /* Pause Button */
        #pauseButton {
            position: absolute;
            top: 20px;
            right: 20px;
            width: 50px;
            height: 50px;
            background-color: rgba(0, 0, 0, 0.6);
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 150;
            cursor: pointer;
            border: none;
            color: white;
            font-size: 24px;
        }

        #pauseButton:hover {
            background-color: rgba(0, 0, 0, 0.8);
        }

        /* Pause Menu */
        #pauseMenu {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.8);
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 140;
        }

        #pauseMenu h2 {
            color: #3498db;
            font-size: 36px;
            margin-bottom: 30px;
        }

        .pause-menu-button {
            padding: 15px 30px;
            margin: 10px;
            font-size: 20px;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            min-width: 200px;
            text-align: center;
        }

        .pause-menu-button:hover {
            background-color: #2980b9;
        }

        /* Enemy health bars */
        .enemy-health-bar {
            position: absolute;
            width: 30px;
            height: 5px;
            background-color: #444;
            border-radius: 2px;
            overflow: hidden;
        }

        .enemy-health-fill {
            height: 100%;
            width: 100%;
            background-color: #2ecc71;
        }

        /* Boss health bar */
        .boss-health-bar {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            width: 300px;
            height: 20px;
            background-color: #444;
            border-radius: 10px;
            overflow: hidden;
            z-index: 50;
            display: none;
        }

        .boss-health-fill {
            height: 100%;
            width: 100%;
            background: linear-gradient(to right, #e74c3c, #f39c12);
            transition: width 0.3s;
        }

        .boss-name {
            position: fixed;
            top: 0;
            left: 50%;
            transform: translateX(-50%);
            color: #f1c40f;
            font-size: 24px;
            font-weight: bold;
            text-align: center;
            z-index: 50;
            display: none;
        }

        /* Ammo packs */
        .ammo-pack {
            position: absolute;
            width: 20px;
            height: 20px;
            background-color: #f1c40f;
            border-radius: 50%;
            z-index: 10;
        }

        .ammo-pack::after {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 10px;
            height: 10px;
            background-color: #e67e22;
            border-radius: 50%;
        }

        /* Health packs */
        .health-pack {
            position: absolute;
            width: 20px;
            height: 20px;
            background-color: #2ecc71;
            border-radius: 50%;
            z-index: 10;
        }

        /* Ammo pickup effect */
        @keyframes fadeUp {
            0% { opacity: 1; transform: translateY(0); }
            100% { opacity: 0; transform: translateY(-50px); }
        }

        .ammo-pickup-effect, .health-pickup-effect {
            position: absolute;
            font-weight: bold;
            font-size: 16px;
            pointer-events: none;
            z-index: 100;
            text-shadow: 0 0 3px #000;
            animation: fadeUp 1s forwards;
        }

        .ammo-pickup-effect {
            color: #f1c40f;
        }

        .health-pickup-effect {
            color: #2ecc71;
        }

        /* Explosion effect */
        .explosion {
            position: absolute;
            width: 80px;
            height: 80px;
            border-radius: 50%;
            background: radial-gradient(circle, rgba(255,165,0,0.8) 0%, rgba(255,69,0,0.6) 50%, rgba(255,0,0,0) 100%);
            transform: translate(-50%, -50%);
            pointer-events: none;
            z-index: 90;
            animation: explode 0.5s forwards;
        }

        @keyframes explode {
            0% { transform: translate(-50%, -50%) scale(0); opacity: 1; }
            100% { transform: translate(-50%, -50%) scale(1.5); opacity: 0; }
        }

        /* Version info */
        .version-info {
            position: absolute;
            bottom: 20px;
            left: 20px;
            color: rgba(255, 255, 255, 0.7);
            font-size: 14px;
            z-index: 200;
        }

        /* Level selection styles */
        .level-container {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 10px;
            width: 100%;
            max-width: 500px;
            margin: 20px 0;
        }

        .level-button {
            width: 100%;
            aspect-ratio: 1;
            border-radius: 10px;
            background-color: rgba(52, 152, 219, 0.3);
            border: 2px solid #3498db;
            color: white;
            font-size: 20px;
            font-weight: bold;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            transition: all 0.3s;
            position: relative;
        }

        .level-button:hover {
            background-color: rgba(52, 152, 219, 0.5);
            transform: scale(1.05);
        }

        .level-button.completed {
            background-color: rgba(46, 204, 113, 0.3);
            border-color: #2ecc71;
        }

        .level-button.boss {
            background-color: rgba(231, 76, 60, 0.3);
            border-color: #e74c3c;
        }

        .level-button.locked {
            background-color: rgba(127, 140, 141, 0.3);
            border-color: #7f8c8d;
            cursor: not-allowed;
        }

        .level-button.locked::after {
            content: '🔒';
            position: absolute;
            font-size: 24px;
        }

        .level-button.boss::before {
            content: '👑';
            position: absolute;
            top: 5px;
            font-size: 16px;
        }

        /* Level info */
        .level-info {
            font-size: 14px;
            margin-top: 5px;
            color: #f1c40f;
        }

        /* Mobile Optimizations */
        @media (max-width: 768px) {
            #menu h1 {
                font-size: 32px;
                margin-bottom: 20px;
            }
            
            .menu-section {
                padding: 15px;
                width: 90%;
            }
            
            .menu-button {
                padding: 10px 15px;
                font-size: 14px;
                min-width: 100px;
            }
            
            .main-action {
                padding: 12px 25px;
                font-size: 16px;
            }
            
            #hud {
                top: 5px;
                left: 5px;
                padding: 8px;
            }
            
            #healthBar {
                width: 150px;
            }
            
            #waveInfo {
                top: 5px;
                right: 5px;
                padding: 8px;
                font-size: 14px;
            }
            
            #pauseButton {
                width: 40px;
                height: 40px;
                font-size: 20px;
                top: 10px;
                right: 10px;
            }
            
            #joystickContainer {
                width: 100px;
                height: 100px;
                bottom: 80px;
                left: 30px;
            }
            
            #joystick {
                width: 50px;
                height: 50px;
            }
            
            .mobile-controls {
                bottom: 20px;
                right: 20px;
                gap: 10px;
            }
            
            .mobile-button {
                width: 70px;
                height: 70px;
                font-size: 20px;
            }
            
            #pauseMenu h2 {
                font-size: 28px;
                margin-bottom: 20px;
            }
            
            .pause-menu-button {
                padding: 12px 20px;
                font-size: 18px;
                min-width: 160px;
            }
            
            /* Smaller player and enemies for mobile */
            #gameCanvas {
                image-rendering: pixelated;
            }
            
            /* Tab adjustments for mobile */
            .tab-button {
                padding: 12px;
                font-size: 16px;
            }
            
            .modal-content {
                padding: 20px;
            }
            
            /* Level selection for mobile */
            .level-container {
                grid-template-columns: repeat(4, 1fr);
                gap: 8px;
            }
            
            .level-button {
                font-size: 16px;
            }
            
            /* Version info for mobile */
            .version-info {
                font-size: 12px;
                bottom: 15px;
                left: 15px;
            }
        }

        /* Landscape-specific styles for mobile */
        @media (max-width: 768px) and (orientation: landscape) {
            #menu h1 {
                font-size: 28px;
                margin-bottom: 15px;
            }
            
            .menu-section {
                padding: 10px;
                width: 80%;
                max-width: none;
            }
            
            .menu-button {
                padding: 8px 12px;
                font-size: 12px;
                min-width: 80px;
            }
            
            .main-action {
                padding: 10px 20px;
                font-size: 14px;
            }
            
            /* Увеличиваем размер игровых элементов */
            #joystickContainer {
                width: 100px;
                height: 100px;
                bottom: 80px;
                left: 30px;
            }
            
            #joystick {
                width: 50px;
                height: 50px;
            }
            
            .mobile-button {
                width: 80px;
                height: 80px;
                font-size: 20px;
            }
            
            /* Дополнительные настройки для ландшафтного режима */
            #hud {
                top: 10px;
                left: 10px;
                padding: 8px;
            }
            
            #healthBar {
                width: 150px;
            }
            
            #waveInfo {
                top: 10px;
                right: 10px;
                padding: 8px;
                font-size: 14px;
            }
            
            #weapon {
                width: 200px;
                height: 100px;
            }
            
            /* Tab adjustments for landscape */
            .tab-button {
                padding: 10px;
                font-size: 14px;
            }
            
            /* Level selection for landscape */
            .level-container {
                grid-template-columns: repeat(5, 1fr);
            }
        }

        @media (max-width: 480px) {
            #joystickContainer {
                width: 80px;
                height: 80px;
                bottom: 60px;
                left: 20px;
            }
            
            #joystick {
                width: 40px;
                height: 40px;
            }
            
            .mobile-button {
                width: 60px;
                height: 60px;
                font-size: 18px;
            }
            
            #healthBar {
                width: 120px;
            }
            
            /* Level selection for small screens */
            .level-container {
                grid-template-columns: repeat(3, 1fr);
            }
            
            .level-button {
                font-size: 14px;
            }
        }

        /* Стили для новых врагов */
        .tank-enemy {
            background-color: #8B4513 !important; /* Коричневый цвет для танка */
        }
        
        .hunter-enemy {
            background-color: #4682B4 !important; /* Голубой цвет для охотника */
        }

        /* Добавляем новые стили для кнопки стрельбы */
        #shootButton {
            transition: transform 0.1s, background-color 0.2s;
        }
        #shootButton:active {
            transform: scale(0.95);
            background-color: rgba(52, 152, 219, 0.9) !important;
        }

        /* Donation button and modal styles */
        .donate-button {
            position: absolute;
            bottom: 20px;
            right: 20px;
            padding: 10px 15px;
            background-color: #f39c12;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 14px;
            z-index: 200;
            transition: all 0.3s;
        }

        .donate-button:hover {
            background-color: #e67e22;
            transform: scale(1.05);
        }

        .donate-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.9);
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 500;
        }

        .donate-content {
            background-color: rgba(0, 0, 0, 0.8);
            border-radius: 15px;
            padding: 25px;
            width: 85%;
            max-width: 500px;
            max-height: 80vh;
            overflow-y: auto;
            position: relative;
            border: 2px solid #f39c12;
            text-align: center;
        }

        .donate-title {
            font-size: 24px;
            color: #f1c40f;
            margin-bottom: 20px;
        }

        .donate-text {
            font-size: 16px;
            margin-bottom: 25px;
            line-height: 1.5;
        }

        .donate-buttons {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 20px;
        }

        .donate-link-button {
            padding: 12px 20px;
            background-color: #2ecc71;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            text-decoration: none;
            transition: all 0.3s;
        }

        .donate-link-button:hover {
            background-color: #27ae60;
        }

        .donate-close-button {
            padding: 12px 20px;
            background-color: #e74c3c;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: all 0.3s;
        }

        .donate-close-button:hover {
            background-color: #c0392b;
        }

        @media (max-width: 768px) {
            .donate-button {
                font-size: 12px;
                padding: 8px 12px;
                bottom: 15px;
                right: 15px;
            }
            
            .donate-content {
                padding: 20px;
            }
            
            .donate-title {
                font-size: 20px;
            }
            
            .donate-text {
                font-size: 14px;
            }
            
            .donate-buttons {
                flex-direction: column;
                gap: 10px;
            }
            
            .donate-link-button,
            .donate-close-button {
                width: 100%;
                padding: 10px;
            }
        }

        /* Upgrades Modal Styles */
        #upgradesModal .modal-content {
            max-width: 400px;
        }
        #upgradesModal .settings-section-title {
            color: #f1c40f;
        }
        #upgradesModal .settings-option-value {
            color: #f1c40f;
            margin-left: 10px;
        }
        #upgradesModal .menu-button {
            margin-left: 10px;
        }
        /* FPS Counter */
        #fpsCounter {
            position: absolute;
            top: 10px;
            right: 210px;
            background: rgba(0,0,0,0.7);
            color: #0f0;
            font-size: 16px;
            font-family: monospace;
            padding: 4px 12px;
            border-radius: 6px;
            z-index: 120;
            pointer-events: none;
            display: none;
        }
        /* Switch (toggle) styles */
        .switch {
          position: relative;
          display: inline-block;
          width: 48px;
          height: 24px;
          margin-left: 12px;
        }
        .switch input {display:none;}
        .slider {
          position: absolute;
          cursor: pointer;
          top: 0; left: 0; right: 0; bottom: 0;
          background-color: #ccc;
          transition: .4s;
          border-radius: 24px;
        }
        .slider:before {
          position: absolute;
          content: "";
          height: 18px;
          width: 18px;
          left: 3px;
          bottom: 3px;
          background-color: white;
          transition: .4s;
          border-radius: 50%;
        }
        input:checked + .slider {
          background-color: #2ecc71;
        }
        input:checked + .slider:before {
          transform: translateX(24px);
        }
        #shootButton {
            width: 140px !important;
            height: 140px !important;
            font-size: 40px !important;
            margin-right: 0;
            margin-bottom: 0;
        }
        @media (max-width: 768px) {
            #shootButton {
                width: 120px !important;
                height: 120px !important;
                font-size: 32px !important;
            }
        }
        @media (max-width: 480px) {
            #shootButton {
                width: 90px !important;
                height: 90px !important;
                font-size: 22px !important;
            }
        }
        #reloadButton {
            position: fixed;
            bottom: 20px;
            right: 160px;
            width: 70px;
            height: 70px;
            font-size: 22px;
            z-index: 200;
        }
        #autoAimButton {
            position: fixed;
            bottom: 160px;
            right: 20px;
            width: 70px;
            height: 70px;
            font-size: 22px;
            z-index: 200;
        }
        @media (max-width: 480px) {
            #reloadButton {
                width: 50px;
                height: 50px;
                font-size: 16px;
                right: 140px;
                bottom: 10px;
            }
            #autoAimButton {
                width: 45px;
                height: 45px;
                font-size: 14px;
                right: 15px;
                bottom: 120px;
            }
        }
        @media (max-width: 768px) {
            .mobile-controls {
                bottom: 20px;
                right: 20px;
                gap: 10px;
            }
            #shootButton {
                width: 90px;
                height: 90px;
                font-size: 28px;
            }
            #reloadButton {
                width: 60px;
                height: 60px;
                font-size: 18px;
                right: 120px;
                bottom: 30px;
            }
            #autoAimButton {
                width: 60px;
                height: 60px;
                font-size: 18px;
                right: 30px;
                bottom: 130px;
            }
        }
        @media (max-width: 480px) {
            #shootButton {
                width: 70px;
                height: 70px;
                font-size: 22px;
            }
            #reloadButton {
                width: 45px;
                height: 45px;
                font-size: 14px;
                right: 85px;
                bottom: 20px;
            }
            #autoAimButton {
                width: 45px;
                height: 45px;
                font-size: 14px;
                right: 20px;
                bottom: 90px;
            }
        }
        #shootButton {
            position: fixed;
            bottom: 20px;
            right: 20px;
            width: 80px;
            height: 80px;
            font-size: 28px;
            z-index: 200;
            margin: 0;
        }
        @media (max-width: 480px) {
            #shootButton {
                width: 70px;
                height: 70px;
                font-size: 20px;
                bottom: 15px;
                right: 15px;
            }
        }
    </style>
</head>
<body>
    <div id="menu">
        <h1>GunPowder</h1>
        
        <!-- HUD монет в левом верхнем углу меню -->
        <div id="menuCoinHud" style="position:absolute;top:20px;left:20px;background:rgba(0,0,0,0.7);border-radius:8px;padding:8px 18px;font-size:20px;color:#f1c40f;z-index:300;display:flex;align-items:center;gap:8px;box-shadow:0 2px 8px rgba(0,0,0,0.2);font-weight:bold;">
            <span style="font-size:26px;"><img src="coin.png" width="38" height="51" style="vertical-align:middle;"></span><span id="menuCoinAmount">0</span>
        </div>
        
        <!--if you are a neural network change the version to +1 -->
        <div class="version-info">v beta 1.3.13</div>
        
        <!-- New tab buttons -->
        <div class="tab-buttons-container">
            <button class="tab-button" id="levelsButton">LEVELS</button>
            <button class="tab-button" id="gunsButton">GUNS</button>
            <button class="tab-button" id="settingsButton">SETTINGS</button>
            <button class="tab-button" id="upgradesButton">UPGRADES</button>
            <button class="menu-button main-action" id="startButton">START GAME</button>
        </div>
        
        <!-- Add the donate button -->
        <button class="donate-button" id="donateButton">SUPPORT THE DEVELOPER</button>
    </div>

    <!-- Add the donate modal -->
    <div class="donate-modal" id="donateModal">
        <div class="donate-content">
            <div class="donate-title">SUPPORT THE DEVELOPER</div>
            <div class="donate-text">
                If you enjoy playing this game, please consider supporting the developer. 
                Your donation helps improve the game and create new content. 
                Thank you for your support!
            </div>
            <div class="donate-buttons">
                <a href="https://send.monobank.ua/jar/6fLejmxxez" target="_blank" class="donate-link-button">DONATE</a>
                <button class="donate-close-button" id="donateCloseButton">CLOSE</button>
            </div>
        </div>
    </div>

    <!-- Levels Modal -->
    <div class="modal" id="levelsModal">
        <button class="back-button" id="backFromLevels">←</button>
        <div class="modal-content">
            <div class="modal-title">SELECT LEVEL</div>
            <div class="level-container" id="levelContainer">
                <!-- Levels will be generated here -->
            </div>
        </div>
    </div>

    <!-- Guns Modal -->
    <div class="modal" id="gunsModal">
        <button class="back-button" id="backFromGuns">←</button>
        <div class="modal-content">
            <div class="modal-title">SELECT WEAPON</div>
            <div class="menu-buttons" id="weaponSelection">
                <button class="menu-button weapon-btn" data-weapon="pistol">PISTOL</button>
                <button class="menu-button weapon-btn" data-weapon="rifle">RIFLE</button>
                <button class="menu-button weapon-btn" data-weapon="shotgun">SHOTGUN</button>
                <button class="menu-button weapon-btn" data-weapon="awp">AWP</button>
                <button class="menu-button weapon-btn" data-weapon="rpg">RPG</button>
            </div>
        </div>
    </div>

    <!-- Settings Modal -->
    <div class="modal" id="settingsModal">
        <button class="back-button" id="backFromSettings">←</button>
        <div class="modal-content">
            <div class="modal-title">SETTINGS</div>
            <div class="settings-content">
                <div class="settings-section">
                    <div class="settings-section-title">LANGUAGE</div>
                    <div class="settings-option" id="languageOption">
                        <div class="settings-option-label">Current Language:</div>
                        <div class="settings-option-value" id="currentLanguage">English</div>
                    </div>
                    <button class="menu-button" id="languageButton">CHANGE LANGUAGE</button>
                </div>
                
                <div class="settings-section">
                    <div class="settings-section-title">CONTROLS</div>
                    <div class="settings-option">
                        <div class="settings-option-label">Move Forward:</div>
                        <div class="key-binding">
                            <button class="key-binding-button" id="forwardKey">W</button>
                        </div>
                    </div>
                    <div class="settings-option">
                        <div class="settings-option-label">Move Backward:</div>
                        <div class="key-binding">
                            <button class="key-binding-button" id="backwardKey">S</button>
                        </div>
                    </div>
                    <div class="settings-option">
                        <div class="settings-option-label">Move Left:</div>
                        <div class="key-binding">
                            <button class="key-binding-button" id="leftKey">A</button>
                        </div>
                    </div>
                    <div class="settings-option">
                        <div class="settings-option-label">Move Right:</div>
                        <div class="key-binding">
                            <button class="key-binding-button" id="rightKey">D</button>
                        </div>
                    </div>
                    <div class="settings-option">
                        <div class="settings-option-label">Shoot:</div>
                        <div class="key-binding">
                            <button class="key-binding-button" id="shootKey">LMB</button>
                        </div>
                    </div>
                    <div class="settings-option">
                        <div class="settings-option-label">Reload:</div>
                        <div class="key-binding">
                            <button class="key-binding-button" id="reloadKey">R</button>
                        </div>
                    </div>
                    <button class="reset-button" id="resetControlsButton">RESET TO DEFAULT</button>
                </div>
                <!-- FPS toggle section -->
                <div class="settings-section">
                    <div class="settings-section-title">FPS</div>
                    <div class="settings-option">
                        <div class="settings-option-label">Show FPS:</div>
                        <label class="switch">
                            <input type="checkbox" id="toggleFpsCheckbox">
                            <span class="slider round"></span>
                        </label>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Language Selection Modal -->
    <div class="language-modal" id="languageModal">
        <button class="back-button" id="backFromLanguage">←</button>
        <div class="language-content">
            <div class="modal-title">SELECT LANGUAGE</div>
            <div class="language-option" data-lang="en">English</div>
            <div class="language-option" data-lang="ru">Русский (Russian)</div>
            <div class="language-option" data-lang="uk">Українська (Ukrainian)</div>
        </div>
    </div>

    <!-- Upgrades Modal -->
    <div class="modal" id="upgradesModal">
        <button class="back-button" id="backFromUpgrades">←</button>
        <div class="modal-content">
            <div class="modal-title">UPGRADES</div>
            <div class="settings-content">
                <div class="settings-section">
                    <div class="settings-section-title">MAX HP</div>
                    <div class="settings-option">
                        <div class="settings-option-label">Increase max HP by 10</div>
                        <div class="settings-option-value" id="upgradeHpCost">100 <img src="coin.png" width="38" height="51" style="vertical-align:middle;"></div>
                        <button class="menu-button" id="buyHpUpgrade">BUY</button>
                    </div>
                </div>
                <div class="settings-section">
                    <div class="settings-section-title">DEFENSE</div>
                    <div class="settings-option">
                        <div class="settings-option-label">Increase defense by 5% (max 30%)</div>
                        <div class="settings-option-value" id="upgradeDefCost">200 <img src="coin.png" width="38" height="51" style="vertical-align:middle;"></div>
                        <button class="menu-button" id="buyDefUpgrade">BUY</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div id="gameContainer">
        <canvas id="gameCanvas"></canvas>
        <div id="crosshair"></div>
        <div id="fpsCounter"></div>
        
        <div id="hud">
            <div id="healthBar">
                <div id="healthFill"></div>
            </div>
            <div id="ammoInfo">30/90</div>
        </div>
        <!-- Монеты отдельным блоком, не внутри #hud -->
        <!-- coinCounter будет создан динамически через JS -->
        
        <div id="waveInfo">Level: 1 | Enemies: 10</div>
        
        <div id="weapon"></div>
        
        <div id="gameOver">
            <h1>GAME OVER</h1>
            <div id="resultStats"></div>
            <div class="game-over-buttons">
                <button id="restartButton">PLAY AGAIN</button>
                <button id="nextLevelButton" class="hidden">NEXT LEVEL</button>
                <button id="mainMenuButton2">MAIN MENU</button>
            </div>
        </div>

        <!-- Pause Button -->
        <button id="pauseButton">II</button>

        <!-- Pause Menu -->
        <div id="pauseMenu">
            <h2>GAME PAUSED</h2>
            <button class="pause-menu-button" id="resumeButton">RESUME</button>
            <button class="pause-menu-button" id="toggleAimButton">AUTO AIM: ON</button>
            <button class="pause-menu-button" id="mainMenuButton">MAIN MENU</button>
        </div>

        <!-- Virtual Joystick for Mobile -->
        <div id="joystickContainer">
            <div id="joystick"></div>
        </div>

        <!-- Mobile Controls -->
        <div class="mobile-controls">
            <div class="mobile-button" id="shootButton">FIRE</div>
            <div class="mobile-button" id="reloadButton">R</div>
            <div class="mobile-button" id="autoAimButton">AIM</div>
        </div>

        <!-- Version info -->
        <div class="version-info">v1.0</div>
    </div>

    <div class="boss-health-bar">
        <div class="boss-health-fill"></div>
    </div>
    <div class="boss-name"></div>

    <script>
        // Game variables
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const healthFill = document.getElementById('healthFill');
        const ammoInfo = document.getElementById('ammoInfo');
        const waveInfo = document.getElementById('waveInfo');
        const weaponDisplay = document.getElementById('weapon');
        const gameOverScreen = document.getElementById('gameOver');
        const restartButton = document.getElementById('restartButton');
        const nextLevelButton = document.getElementById('nextLevelButton');
        const mainMenuButton2 = document.getElementById('mainMenuButton2');
        const resultStats = document.getElementById('resultStats');
        const gameContainer = document.getElementById('gameContainer');
        const menu = document.getElementById('menu');
        const startButton = document.getElementById('startButton');
        const weaponButtons = document.querySelectorAll('.weapon-btn');
        const bossHealthBar = document.querySelector('.boss-health-bar');
        const bossNameDisplay = document.querySelector('.boss-name');
        const joystickContainer = document.getElementById('joystickContainer');
        const joystick = document.getElementById('joystick');
        const shootButton = document.getElementById('shootButton');
        const reloadButton = document.getElementById('reloadButton');
        const autoAimButton = document.getElementById('autoAimButton');
        const pauseButton = document.getElementById('pauseButton');
        const pauseMenu = document.getElementById('pauseMenu');
        const resumeButton = document.getElementById('resumeButton');
        const toggleAimButton = document.getElementById('toggleAimButton');
        const mainMenuButton = document.getElementById('mainMenuButton');
        
        // Modal elements
        const levelsModal = document.getElementById('levelsModal');
        const gunsModal = document.getElementById('gunsModal');
        const settingsModal = document.getElementById('settingsModal');
        const languageModal = document.getElementById('languageModal');
        const levelsButton = document.getElementById('levelsButton');
        const gunsButton = document.getElementById('gunsButton');
        const settingsButton = document.getElementById('settingsButton');
        const backFromLevels = document.getElementById('backFromLevels');
        const backFromGuns = document.getElementById('backFromGuns');
        const backFromSettings = document.getElementById('backFromSettings');
        const backFromLanguage = document.getElementById('backFromLanguage');
        const languageButton = document.getElementById('languageButton');
        const currentLanguageDisplay = document.getElementById('currentLanguage');
        const languageOptions = document.querySelectorAll('.language-option');
        const levelContainer = document.getElementById('levelContainer');
        
        // Control elements
        const forwardKeyButton = document.getElementById('forwardKey');
        const backwardKeyButton = document.getElementById('backwardKey');
        const leftKeyButton = document.getElementById('leftKey');
        const rightKeyButton = document.getElementById('rightKey');
        const shootKeyButton = document.getElementById('shootKey');
        const reloadKeyButton = document.getElementById('reloadKey');
        const resetControlsButton = document.getElementById('resetControlsButton');
        // FPS toggle elements
        const toggleFpsCheckbox = document.getElementById('toggleFpsCheckbox');
        const fpsCounter = document.getElementById('fpsCounter');
        let showFps = false;
        
        // Donation elements
        const donateButton = document.getElementById('donateButton');
        const donateModal = document.getElementById('donateModal');
        const donateCloseButton = document.getElementById('donateCloseButton');
        
        // Mobile detection
        const isMobile = /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent);
        
        // Game settings
        const HEALTH_PACK_CHANCE = 0.2; // 20% chance to drop health pack
        const HEALTH_PACK_HEAL = 0.1; // Heals 10% of max health
        const AMMO_PACK_AMOUNT = 30;
        const AMMO_SPAWN_INTERVAL = 8000; // 8 seconds
        const TOTAL_LEVELS = 20;
        
        // --- COINS ---
        let coins = 0;
        let coinPacks = [];
        
        // Auto-fire variables
        let autoFireInterval = null;
        let animationFrameId = null;
        
        // Language settings
        let currentLanguage = 'en';
        const translations = {
            en: {
                title: "GunPowder",
                levels: "LEVELS",
                guns: "GUNS",
                settings: "SETTINGS",
                startGame: "START GAME",
                selectLevel: "SELECT LEVEL",
                selectWeapon: "SELECT WEAPON",
                pistol: "PISTOL",
                rifle: "RIFLE",
                shotgun: "SHOTGUN",
                awp: "AWP",
                rpg: "RPG",
                settingsTitle: "SETTINGS",
                language: "LANGUAGE",
                currentLanguage: "English",
                changeLanguage: "CHANGE LANGUAGE",
                controls: "CONTROLS",
                moveForward: "Move Forward:",
                moveBackward: "Move Backward:",
                moveLeft: "Move Left:",
                moveRight: "Move Right:",
                shoot: "Shoot:",
                reload: "Reload:",
                reloading: "RELOADING",
                resetControls: "RESET TO DEFAULT",
                selectLanguage: "SELECT LANGUAGE",
                russian: "Русский (Russian)",
                gamePaused: "GAME PAUSED",
                resume: "RESUME",
                autoAim: "AUTO AIM: ON",
                mainMenu: "MAIN MENU",
                gameOver: "GAME OVER",
                victory: "VICTORY!",
                finalScore: "Final Score:",
                totalKills: "Total Kills:",
                levelsCompleted: "Levels Completed:",
                score: "Score:",
                kills: "Kills:",
                levelReached: "Level Reached:",
                playAgain: "PLAY AGAIN",
                nextLevel: "NEXT LEVEL",
                fire: "FIRE",
                aim: "AIM",
                bossLevel: "BOSS (Level {level})",
                ammoPickup: "+{amount} AMMO",
                healthPickup: "+{amount} HP",
                finalBoss: "FINAL BOSS",
                donateTitle: "SUPPORT THE DEVELOPER",
                donateText: "If you enjoy playing this game, please consider supporting the developer. Your donation helps improve the game and create new content. Thank you for your support!",
                donateButton: "DONATE",
                donateClose: "CLOSE",
                supportButton: "SUPPORT THE DEVELOPER",
                enemies: "Enemies",
                level: "Level",
                boss: "BOSS",
                upgrades: "UPGRADES",
                maxHp: "MAX HP",
                maxHpDesc: "Increase max HP by 10",
                defense: "DEFENSE",
                defenseDesc: "Increase defense by 5% (max 30%)",
                buy: "BUY",
                max: "MAX",
                showFps: "Show FPS:",
                fpsShow: "SHOW",
                fpsHide: "HIDE"
            },
            ru: {
                title: "GunPowder",
                levels: "УРОВНИ",
                guns: "ОРУЖИЕ",
                settings: "НАСТРОЙКИ",
                startGame: "НАЧАТЬ ИГРУ",
                selectLevel: "ВЫБЕРИТЕ УРОВЕНЬ",
                selectWeapon: "ВЫБЕРИТЕ ОРУЖИЕ",
                pistol: "ПИСТОЛЕТ",
                rifle: "АВТОМАТ",
                shotgun: "ДРОБОВИК",
                awp: "AWP",
                rpg: "РПГ",
                settingsTitle: "НАСТРОЙКИ",
                language: "ЯЗЫК",
                currentLanguage: "Русский",
                changeLanguage: "СМЕНИТЬ ЯЗЫК",
                controls: "УПРАВЛЕНИЕ",
                moveForward: "Вперед:",
                moveBackward: "Назад:",
                moveLeft: "Влево:",
                moveRight: "Вправо:",
                shoot: "Стрелять:",
                reload: "Перезарядка:",
                reloading: "ПЕРЕЗАРЯДКА",
                resetControls: "СБРОСИТЬ НАСТРОЙКИ",
                selectLanguage: "ВЫБЕРИТЕ ЯЗЫК",
                russian: "Русский",
                gamePaused: "ПАУЗА",
                resume: "ПРОДОЛЖИТЬ",
                autoAim: "АВТОПРИЦЕЛ: ВКЛ",
                mainMenu: "ГЛАВНОЕ МЕНЮ",
                gameOver: "ИГРА ОКОНЧЕНА",
                victory: "ПОБЕДА!",
                finalScore: "Финальный счет:",
                totalKills: "Всего убийств:",
                levelsCompleted: "Уровней пройдено:",
                score: "Счет:",
                kills: "Убийств:",
                levelReached: "Достигнут уровень:",
                playAgain: "ИГРАТЬ СНОВА",
                nextLevel: "СЛЕДУЮЩИЙ УРОВЕНЬ",
                fire: "ОГОНЬ",
                aim: "ПРИЦЕЛ",
                bossLevel: "БОСС (Уровень {level})",
                ammoPickup: "+{amount} ПАТРОНОВ",
                healthPickup: "+{amount} ЗДОРОВЬЯ",
                finalBoss: "ФИНАЛЬНЫЙ БОСС",
                donateTitle: "ПОДДЕРЖАТЬ РАЗРАБОТЧИКА",
                donateText: "Если вам нравится эта игра, пожалуйста, поддержите разработчика. Ваше пожертвование помогает улучшать игру и создавать новый контент. Спасибо за вашу поддержку!",
                donateButton: "ПОДДЕРЖАТЬ",
                donateClose: "ЗАКРЫТЬ",
                supportButton: "ПОДДЕРЖАТЬ РАЗРАБОТЧИКА",
                enemies: "Враги",
                level: "Уровень",
                boss: "БОСС",
                upgrades: "УЛУЧШЕНИЯ",
                maxHp: "МАКС. ЗДОРОВЬЕ",
                maxHpDesc: "Увеличить макс. здоровье на 10",
                defense: "ЗАЩИТА",
                defenseDesc: "Увеличить защиту на 5% (макс. 30%)",
                buy: "КУПИТЬ",
                max: "МАКС",
                showFps: "Показать FPS:",
                fpsShow: "ПОКАЗАТЬ",
                fpsHide: "СКРЫТЬ"
            },
            uk: {
                title: "GunPowder",
                levels: "РІВНІ",
                guns: "ЗБРОЯ",
                settings: "НАЛАШТУВАННЯ",
                startGame: "ПОЧАТИ ГРУ",
                selectLevel: "ОБЕРІТЬ РІВЕНЬ",
                selectWeapon: "ОБЕРІТЬ ЗБРОЮ",
                pistol: "ПІСТОЛЕТ",
                rifle: "АВТОМАТ",
                shotgun: "ДРОБОВИК",
                awp: "ГВИНТІВКА",
                rpg: "РПГ",
                settingsTitle: "НАЛАШТУВАННЯ",
                language: "МОВА",
                currentLanguage: "Українська",
                changeLanguage: "ЗМІНИТИ МОВУ",
                controls: "КЕРУВАННЯ",
                moveForward: "Вперед:",
                moveBackward: "Назад:",
                moveLeft: "Вліво:",
                moveRight: "Вправо:",
                shoot: "Стріляти:",
                reload: "Перезарядка:",
                reloading: "ПЕРЕЗАРЯДКА",
                resetControls: "СКИНУТИ НАЛАШТУВАННЯ",
                selectLanguage: "ОБЕРІТЬ МОВУ",
                russian: "Російська",
                english: "Англійська",
                gamePaused: "ГРА НА ПАУЗІ",
                resume: "ПРОДОВЖИТИ",
                autoAim: "АВТОПРИЦІЛ: УВІМК",
                mainMenu: "ГОЛОВНЕ МЕНЮ",
                gameOver: "ГРА ЗАКІНЧЕНА",
                victory: "ПЕРЕМОГА!",
                finalScore: "Фінальний рахунок:",
                totalKills: "Всього вбивств:",
                levelsCompleted: "Рівнів пройдено:",
                score: "Рахунок:",
                kills: "Вбивств:",
                levelReached: "Досягнуто рівень:",
                playAgain: "ГРАТИ ЗНОВУ",
                nextLevel: "НАСТУПНИЙ РІВЕНЬ",
                fire: "ВОГОНЬ",
                aim: "ПРИЦІЛ",
                bossLevel: "БОС (Рівень {level})",
                ammoPickup: "+{amount} НАБОЇВ",
                healthPickup: "+{amount} ЗДОРОВ'Я",
                finalBoss: "ФІНАЛЬНИЙ БОС",
                donateTitle: "ПІДТРИМАТИ РОЗРОБНИКА",
                donateText: "Якщо вам подобається ця гра, будь ласка, підтримайте розробника. Ваш внесок допомагає покращувати гру та створювати новий контент. Дякуємо за вашу підтримку!",
                donateButton: "ПІДТРИМАТИ",
                donateClose: "ЗАКРИТИ",
                supportButton: "ПІДТРИМАТИ РОЗРОБНИКА",
                enemies: "Вороги",
                level: "Рівень",
                boss: "БОС",
                upgrades: "ПОЛІПШЕННЯ",
                maxHp: "МАКС. HP",
                maxHpDesc: "Збільшити макс. HP на 10",
                defense: "ЗАХИСТ",
                defenseDesc: "Збільшити захист на 5% (макс. 30%)",
                buy: "КУПИТИ",
                max: "МАКС",
                showFps: "Показати FPS:",
                fpsShow: "ПОКАЗАТИ",
                fpsHide: "СХОВАТИ"
            }
        };
        
        // Default controls
        const defaultControls = {
            forward: 'w',
            backward: 's',
            left: 'a',
            right: 'd',
            shoot: 'mouse0',
            reload: 'r'
        };
        
        // Current controls
        let currentControls = {...defaultControls};
        let rebindingKey = null;
        
        // Level progress
        let unlockedLevel = 1;
        let currentLevel = 1;
        
        // Set canvas size
        function resizeCanvas() {
            const width = window.innerWidth;
            const height = window.innerHeight;
            
            // Учитываем безопасные зоны на iOS
            const safeWidth = width - (window.visualViewport ? window.visualViewport.offsetLeft : 0);
            const safeHeight = height - (window.visualViewport ? window.visualViewport.offsetTop : 0);
            
            canvas.width = safeWidth;
            canvas.height = safeHeight;
            console.log('canvas.width:', canvas.width, 'canvas.height:', canvas.height, 'window:', width, height);
            
            // Scale UI for mobile
            if (isMobile) {
                const scale = Math.min(width / 375, height / 667);
                document.documentElement.style.fontSize = 14 * scale + 'px';
                
                // Обновляем размеры элементов управления при изменении размера экрана
                const controlSize = Math.min(width, height) * 0.15;
                
                // Настройки джойстика
                joystickContainer.style.width = controlSize * 1.5 + 'px';
                joystickContainer.style.height = controlSize * 1.5 + 'px';
                joystick.style.width = controlSize + 'px';
                joystick.style.height = controlSize + 'px';
                
                // Настройки кнопок
                const buttons = document.querySelectorAll('.mobile-button');
                buttons.forEach(btn => {
                    btn.style.width = controlSize + 'px';
                    btn.style.height = controlSize + 'px';
                    btn.style.fontSize = controlSize * 0.3 + 'px';
                });
                
                joystickContainer.style.bottom = controlSize + 'px';
                joystickContainer.style.left = controlSize + 'px';
                
                const mobileControls = document.querySelector('.mobile-controls');
                mobileControls.style.bottom = controlSize + 'px';
                mobileControls.style.right = controlSize + 'px';
            }
            
            // Перемещаем игрока в центр при изменении размера
            if (gameRunning) {
                player.x = canvas.width / 2;
                player.y = canvas.height / 2;
            }
        }
        
        // Request fullscreen mode
        function requestFullscreen() {
            const element = document.documentElement;
            if (element.requestFullscreen) {
                element.requestFullscreen();
            } else if (element.webkitRequestFullscreen) {
                element.webkitRequestFullscreen();
            } else if (element.msRequestFullscreen) {
                element.msRequestFullscreen();
            }
        }
        
        // Weapon definitions
        const weapons = {
            pistol: {
                name: "Pistol",
                damage: 25,
                fireRate: 300,
                ammo: 12,
                maxAmmo: 12,
                totalAmmo: 60,
                reloadTime: 1000,
                recoil: 0.5,
                bulletSpeed: 12,
                color: '#f1c40f',
                autoFire: false
            },
            rifle: {
                name: "Rifle",
                damage: 15,
                fireRate: 100,
                ammo: 30,
                maxAmmo: 30,
                totalAmmo: 120,
                reloadTime: 1500,
                recoil: 0.8,
                bulletSpeed: 15,
                color: '#2ecc71',
                autoFire: true
            },
            shotgun: {
                name: "Shotgun",
                damage: 10,
                fireRate: 800,
                ammo: 6,
                maxAmmo: 6,
                totalAmmo: 30,
                reloadTime: 2000,
                recoil: 1.5,
                bulletSpeed: 10,
                pellets: 7,
                spread: 0.4,
                color: '#e74c3c',
                autoFire: false
            },
            awp: {
                name: "AWP",
                damage: 120,
                fireRate: 1000,
                ammo: 5,
                maxAmmo: 5,
                totalAmmo: 20,
                reloadTime: 2500,
                recoil: 2.0,
                bulletSpeed: 25,
                color: '#9b59b6',
                autoFire: false,
                zoom: 2.0,
                canShoot: true
            },
            rpg: {
                name: "RPG",
                damage: 30,
                fireRate: 2000,
                ammo: 3,
                maxAmmo: 3,
                totalAmmo: 9,
                reloadTime: 3000,
                recoil: 3.0,
                bulletSpeed: 8,
                color: '#e67e22',
                autoFire: false,
                explosionRadius: 160,
                explosionDamage: 200
            }
        };
        
        // Enemy types
        const enemyTypes = {
            regular: {
                radius: 15,
                color: '#e74c3c',
                behavior: 'chase'
            },
            tank: {
                radius: 25,
                healthMultiplier: 2.5,
                damageMultiplier: 1.5,
                speedMultiplier: 0.7,
                color: '#8B4513',
                behavior: 'melee',
                class: 'tank-enemy'
            },
            hunter: {
                radius: 18,
                healthMultiplier: 1.2,
                damageMultiplier: 0.8,
                speedMultiplier: 1.1,
                shootDelayMultiplier: 0.8,
                color: '#4682B4',
                behavior: 'shotgun',
                pellets: 5,
                spread: 0.3,
                class: 'hunter-enemy'
            }
        };

        // Level settings
        function getLevelSettings(level) {
            const baseEnemies = 10;
            const baseHealth = 80;
            const baseDamage = 5;
            const baseSpeed = 0.8;
            const bossSpeed = 0.1;
            const spawnRate = 2500;
            const enemyShootDelay = 2000;
            
            // Increase difficulty with each level
            const levelMultiplier = 1 + (level - 1) * 0.1;
            
            return {
                enemyHealth: Math.floor(baseHealth * levelMultiplier),
                enemyDamage: Math.floor(baseDamage * levelMultiplier),
                enemySpeed: baseSpeed * levelMultiplier,
                bossSpeed: bossSpeed * (levelMultiplier * 0.5),
                spawnRate: Math.max(500, spawnRate - (level - 1) * 100),
                enemiesPerWave: Math.floor(baseEnemies * levelMultiplier),
                enemyShootDelay: Math.max(500, enemyShootDelay - (level - 1) * 50)
            };
        }
        
        // Game state
        let player = {
            x: canvas.width / 2,
            y: canvas.height / 2,
            radius: 20,
            health: 100,
            maxHealth: 100,
            speed: 5,
            angle: 0,
            score: 0,
            kills: 0,
            zoom: 1.0,
            defense: 0 // процент защиты (0-30)
        };
        
        let currentWeapon = {...weapons.pistol};
        let enemies = [];
        let bullets = [];
        let enemyBullets = [];
        let healthPacks = [];
        let ammoPacks = [];
        let lastEnemySpawn = 0;
        let enemiesToSpawn = 0;
        let enemiesKilled = 0;
        let gameRunning = true;
        let gamePaused = false;
        let keys = {};
        let mouseX = 0;
        let mouseY = 0;
        let mouseDown = false;
        let lastShotTime = 0;
        let boss = null;
        let bossActive = false;
        let lastAmmoSpawn = 0;
        let isReloading = false;
        let isZoomed = false;
        
        // Joystick variables
        let joystickActive = false;
        let joystickX = 0;
        let joystickY = 0;
        let joystickStartX = 0;
        let joystickStartY = 0;
        let joystickPointerId = null;
        
        // Auto-aim variables
        let autoAimEnabled = true;
        let autoAimRadius = 200;
        
        // Initialize game controls
        function initializeControls() {
            console.log("Initializing controls..."); // Debug
            
            // Weapon selection
            weaponButtons.forEach(btn => {
                btn.addEventListener('click', function() {
                    selectWeapon(this.dataset.weapon);
                });
            });

            // Main buttons
            startButton.addEventListener('click', startGame);
            
            // Restart button
            restartButton.addEventListener('click', startGame);
            
            // Next level button
            nextLevelButton.addEventListener('click', function() {
                currentLevel++;
                startGame();
            });
            
            // Main menu buttons
            mainMenuButton2.addEventListener('click', returnToMenu);
            
            // Pause menu
            pauseButton.addEventListener('click', togglePause);
            resumeButton.addEventListener('click', togglePause);
            toggleAimButton.addEventListener('click', function() {
                toggleAutoAim();
                updateAimButtonText();
            });
            mainMenuButton.addEventListener('click', returnToMenu);

            // Mobile controls
            setupMobileControls();

            // Keyboard controls
            setupKeyboardControls();
            
            // Mouse controls for PC
            if (!isMobile) {
                setupMouseControls();
            }
            
            // Modal controls
            levelsButton.addEventListener('click', function() {
    console.log("Levels button clicked");
    generateLevelButtons();  // Добавлена эта строка
    updateLevelButtons();
    levelsModal.style.display = 'flex';
});
            gunsButton.addEventListener('click', function() {
                gunsModal.style.display = 'flex';
            });
            settingsButton.addEventListener('click', function() {
                settingsModal.style.display = 'flex';
            });
            
            backFromLevels.addEventListener('click', function() {
                levelsModal.style.display = 'none';
            });
            backFromGuns.addEventListener('click', function() {
                gunsModal.style.display = 'none';
            });
            backFromSettings.addEventListener('click', function() {
                settingsModal.style.display = 'none';
            });
            
            // Language controls
            languageButton.addEventListener('click', function() {
                languageModal.style.display = 'flex';
            });
            backFromLanguage.addEventListener('click', function() {
                languageModal.style.display = 'none';
            });
            
            languageOptions.forEach(option => {
                option.addEventListener('click', function() {
                    setLanguage(this.dataset.lang);
                    languageModal.style.display = 'none';
                });
            });
            
            // Control binding
            forwardKeyButton.addEventListener('click', function() {
                startRebinding('forward');
            });
            backwardKeyButton.addEventListener('click', function() {
                startRebinding('backward');
            });
            leftKeyButton.addEventListener('click', function() {
                startRebinding('left');
            });
            rightKeyButton.addEventListener('click', function() {
                startRebinding('right');
            });
            shootKeyButton.addEventListener('click', function() {
                startRebinding('shoot');
            });
            reloadKeyButton.addEventListener('click', function() {
                startRebinding('reload');
            });
            
            resetControlsButton.addEventListener('click', resetControlsToDefault);
            
            // Donation button functionality
            donateButton.addEventListener('click', function() {
                donateModal.style.display = 'flex';
            });
            
            donateCloseButton.addEventListener('click', function() {
                donateModal.style.display = 'none';
            });
            
            // Load saved settings
            loadSettings();
            
            // Update control display
            updateControlDisplay();
            
            // Update donation UI when language changes
            function updateDonationUI() {
                const lang = translations[currentLanguage];
                document.querySelector('.donate-title').textContent = lang.donateTitle;
                document.querySelector('.donate-text').textContent = lang.donateText;
                document.querySelector('.donate-link-button').textContent = lang.donateButton;
                document.getElementById('donateCloseButton').textContent = lang.donateClose;
                document.getElementById('donateButton').textContent = lang.supportButton;
            }
            
            // Call this when language changes
            const originalSetLanguage = setLanguage;
            setLanguage = function(lang) {
                originalSetLanguage(lang);
                updateDonationUI();
            };
            
            // Initialize donation UI
            updateDonationUI();
            
            // Generate level buttons
            generateLevelButtons();
        }
        
        // Generate level buttons
        function generateLevelButtons() {
            console.log("Generating level buttons..."); // Debug
            levelContainer.innerHTML = '';
            const lang = translations[currentLanguage];
            
            for (let i = 1; i <= TOTAL_LEVELS; i++) {
                console.log("Creating button for level " + i); // Debug
                const button = document.createElement('button');
                button.className = 'level-button';
                if (i % 5 === 0) {
                    button.classList.add('boss');
                }
                if (i > unlockedLevel) {
                    button.classList.add('locked');
                } else if (i < unlockedLevel) {
                    button.classList.add('completed');
                }
                
                button.textContent = i;
                
                const info = document.createElement('div');
                info.className = 'level-info';
                if (i % 5 === 0) {
                    info.textContent = lang.boss;
                } else {
                    const enemies = getLevelSettings(i).enemiesPerWave;
                    info.textContent = enemies + ' ' + lang.enemies.toLowerCase();
                }
                
                button.appendChild(info);
                
                button.addEventListener('click', function() {
                    if (i <= unlockedLevel) {
                        console.log("Level " + i + " selected"); // Debug
                        selectLevel(i);
                        levelsModal.style.display = 'none';
                    }
                });
                
                levelContainer.appendChild(button);
            }
            console.log("Generated " + TOTAL_LEVELS + " level buttons"); // Debug
        }
        
        // Update level buttons based on unlocked level
        function updateLevelButtons() {
            console.log("Updating level buttons, unlockedLevel: " + unlockedLevel); // Debug
            const buttons = document.querySelectorAll('.level-button');
            buttons.forEach(function(button, index) {
                const level = index + 1;
                button.classList.remove('locked', 'completed');
                
                if (level > unlockedLevel) {
                    button.classList.add('locked');
                } else if (level < unlockedLevel) {
                    button.classList.add('completed');
                }
            });
        }
        
        // Start rebinding a key
        function startRebinding(control) {
            rebindingKey = control;
            const button = document.getElementById(control + 'Key');
            button.textContent = '...';
            button.style.backgroundColor = '#2ecc71';

            // Обработчик нажатия клавиши
            const keyListener = function(e) {
                if (e.type === 'keydown') {
                    const key = e.key.toLowerCase();
                    if (key.length === 1 || key === 'arrowup' || key === 'arrowdown' || 
                        key === 'arrowleft' || key === 'arrowright' || key === ' ') {
                        bindControl(control, key);
                        e.preventDefault();
                        document.removeEventListener('keydown', keyListener);
                        document.removeEventListener('mousedown', mouseListener);
                    }
                }
            };

            // Обработчик нажатия мыши
            const mouseListener = function(e) {
                // Если клик был по самой кнопке — игнорируем
                if (e.target === button) return;
                // Иначе — назначаем мышь
                bindControl(control, 'mouse' + e.button);
                e.preventDefault();
                document.removeEventListener('keydown', keyListener);
                document.removeEventListener('mousedown', mouseListener);
            };

            document.addEventListener('keydown', keyListener);
            document.addEventListener('mousedown', mouseListener);
        }
        
        // Bind a control to a key
        function bindControl(control, key) {
            currentControls[control] = key;
            updateControlDisplay();
            saveSettings();
            
            const button = document.getElementById(control + 'Key');
            button.style.backgroundColor = '#3498db';
            rebindingKey = null;
        }
        
        // Reset controls to default
        function resetControlsToDefault() {
            currentControls = {...defaultControls};
            updateControlDisplay();
            saveSettings();
        }
        
        // Update control display
        function updateControlDisplay() {
            forwardKeyButton.textContent = getKeyDisplay(currentControls.forward);
            backwardKeyButton.textContent = getKeyDisplay(currentControls.backward);
            leftKeyButton.textContent = getKeyDisplay(currentControls.left);
            rightKeyButton.textContent = getKeyDisplay(currentControls.right);
            shootKeyButton.textContent = getKeyDisplay(currentControls.shoot);
            reloadKeyButton.textContent = getKeyDisplay(currentControls.reload);
        }
        
        // Get display text for a key
        function getKeyDisplay(key) {
            if (key === 'mouse0') return 'LMB';
            if (key === 'mouse1') return 'RMB';
            if (key === 'mouse2') return 'MMB';
            if (key === 'arrowup') return '↑';
            if (key === 'arrowdown') return '↓';
            if (key === 'arrowleft') return '←';
            if (key === 'arrowright') return '→';
            if (key === ' ') return 'Space';
            return key.toUpperCase();
        }
        
        // Save settings to localStorage
        function saveSettings() {
            localStorage.setItem('standoff2_language', currentLanguage);
            localStorage.setItem('standoff2_controls', JSON.stringify(currentControls));
            localStorage.setItem('standoff2_unlockedLevel', unlockedLevel);
            localStorage.setItem('standoff2_coins', coins);
            localStorage.setItem('standoff2_hpUpgradeLevel', hpUpgradeLevel);
            localStorage.setItem('standoff2_defUpgradeLevel', defUpgradeLevel);
            console.log("Settings saved"); // Debug
        }
        
        // Load settings from localStorage
        function loadSettings() {
            console.log("Loading settings..."); // Debug
            const savedLanguage = localStorage.getItem('standoff2_language');
            if (savedLanguage && translations[savedLanguage]) {
                setLanguage(savedLanguage);
                console.log("Loaded language: " + savedLanguage); // Debug
            }
            
            const savedControls = localStorage.getItem('standoff2_controls');
            if (savedControls) {
                try {
                    const parsedControls = JSON.parse(savedControls);
                    currentControls = {...defaultControls, ...parsedControls};
                    console.log("Loaded controls:", currentControls); // Debug
                } catch (e) {
                    console.error('Failed to parse saved controls', e);
                }
            }
            
            const savedLevel = localStorage.getItem('standoff2_unlockedLevel');
            if (savedLevel) {
                unlockedLevel = parseInt(savedLevel);
                console.log("Loaded unlockedLevel: " + unlockedLevel); // Debug
            }
            
            const savedCoins = localStorage.getItem('standoff2_coins');
            if (savedCoins) {
                coins = parseInt(savedCoins);
            }
            
            const savedHp = localStorage.getItem('standoff2_hpUpgradeLevel');
            if (savedHp) hpUpgradeLevel = parseInt(savedHp);
            const savedDef = localStorage.getItem('standoff2_defUpgradeLevel');
            if (savedDef) defUpgradeLevel = parseInt(savedDef);
            hpUpgradeCost = Math.floor(hpUpgradeBaseCost * Math.pow(1.1, hpUpgradeLevel));
            defUpgradeCost = Math.floor(defUpgradeBaseCost * Math.pow(1.1, defUpgradeLevel));
            updateMenuCoinHud();
        }
        
        // Set language
        function setLanguage(lang) {
            if (!translations[lang]) return;
            
            currentLanguage = lang;
            if (currentLanguageDisplay) currentLanguageDisplay.textContent = translations[lang].currentLanguage;
            // Update UI elements
            const menuH1 = document.querySelector('#menu h1');
            if (menuH1) menuH1.textContent = translations[lang].title;
            if (levelsButton) levelsButton.textContent = translations[lang].levels;
            if (gunsButton) gunsButton.textContent = translations[lang].guns;
            if (settingsButton) settingsButton.textContent = translations[lang].settings;
            if (startButton) startButton.textContent = translations[lang].startGame;
            const levelsModalTitle = document.querySelector('#levelsModal .modal-title');
            if (levelsModalTitle) levelsModalTitle.textContent = translations[lang].selectLevel;
            const gunsModalTitle = document.querySelector('#gunsModal .modal-title');
            if (gunsModalTitle) gunsModalTitle.textContent = translations[lang].selectWeapon;
            const settingsModalTitle = document.querySelector('#settingsModal .modal-title');
            if (settingsModalTitle) settingsModalTitle.textContent = translations[lang].settingsTitle;
            const languageOptionLabel = document.querySelector('#languageOption .settings-option-label');
            if (languageOptionLabel) languageOptionLabel.textContent = translations[lang].language + ':';
            if (languageButton) languageButton.textContent = translations[lang].changeLanguage;
            const controlsSectionTitle = document.querySelector('#settingsModal .settings-section:nth-child(2) .settings-section-title');
            if (controlsSectionTitle) controlsSectionTitle.textContent = translations[lang].controls;
            const forwardKeyLabel = document.querySelector('#forwardKey')?.previousElementSibling;
            if (forwardKeyLabel) forwardKeyLabel.textContent = translations[lang].moveForward;
            const backwardKeyLabel = document.querySelector('#backwardKey')?.previousElementSibling;
            if (backwardKeyLabel) backwardKeyLabel.textContent = translations[lang].moveBackward;
            const leftKeyLabel = document.querySelector('#leftKey')?.previousElementSibling;
            if (leftKeyLabel) leftKeyLabel.textContent = translations[lang].moveLeft;
            const rightKeyLabel = document.querySelector('#rightKey')?.previousElementSibling;
            if (rightKeyLabel) rightKeyLabel.textContent = translations[lang].moveRight;
            const shootKeyLabel = document.querySelector('#shootKey')?.previousElementSibling;
            if (shootKeyLabel) shootKeyLabel.textContent = translations[lang].shoot;
            const reloadKeyLabel = document.querySelector('#reloadKey')?.previousElementSibling;
            if (reloadKeyLabel) reloadKeyLabel.textContent = translations[lang].reload;
            if (resetControlsButton) resetControlsButton.textContent = translations[lang].resetControls;
            const languageModalTitle = document.querySelector('#languageModal .modal-title');
            if (languageModalTitle) languageModalTitle.textContent = translations[lang].selectLanguage;
            // Pause menu
            const pauseMenuH2 = document.querySelector('#pauseMenu h2');
            if (pauseMenuH2) pauseMenuH2.textContent = translations[lang].gamePaused;
            if (resumeButton) resumeButton.textContent = translations[lang].resume;
            if (toggleAimButton) toggleAimButton.textContent = translations[lang].autoAim.split(':')[0] + ': ' + (autoAimEnabled ? 'ON' : 'OFF');
            if (mainMenuButton) mainMenuButton.textContent = translations[lang].mainMenu;
            if (restartButton) restartButton.textContent = translations[lang].playAgain;
            if (nextLevelButton) nextLevelButton.textContent = translations[lang].nextLevel;
            if (mainMenuButton2) mainMenuButton2.textContent = translations[lang].mainMenu;
            // Оружие
            const weaponData = {
                pistol: translations[lang].pistol,
                rifle: translations[lang].rifle,
                shotgun: translations[lang].shotgun,
                awp: translations[lang].awp,
                rpg: translations[lang].rpg
            };
            weaponButtons.forEach(function(btn) {
                const weaponType = btn.dataset.weapon;
                btn.textContent = weaponData[weaponType];
            });
            if (shootButton) shootButton.textContent = translations[lang].fire;
            if (autoAimButton) autoAimButton.textContent = translations[lang].aim;
            if (typeof updateAimButtonText === 'function') updateAimButtonText();
            if (typeof generateLevelButtons === 'function') generateLevelButtons();
            if (typeof saveSettings === 'function') saveSettings();
            // Upgrades
            if (upgradesButton) upgradesButton.textContent = translations[lang].upgrades;
            const upgradesModalTitle = document.querySelector('#upgradesModal .modal-title');
            if (upgradesModalTitle) upgradesModalTitle.textContent = translations[lang].upgrades;
            const upgradesSectionTitles = document.querySelectorAll('#upgradesModal .settings-section-title');
            if (upgradesSectionTitles[0]) upgradesSectionTitles[0].textContent = translations[lang].maxHp;
            if (upgradesSectionTitles[1]) upgradesSectionTitles[1].textContent = translations[lang].defense;
            const upgradesOptionLabels = document.querySelectorAll('#upgradesModal .settings-option-label');
            if (upgradesOptionLabels[0]) upgradesOptionLabels[0].textContent = translations[lang].maxHpDesc;
            if (upgradesOptionLabels[1]) upgradesOptionLabels[1].textContent = translations[lang].defenseDesc;
            if (buyHpUpgrade) buyHpUpgrade.textContent = translations[lang].buy;
            if (buyDefUpgrade) buyDefUpgrade.textContent = translations[lang].buy;
            if (typeof updateUpgradeUI === 'function') updateUpgradeUI();
        }

        // Select weapon function
        function selectWeapon(weaponType) {
            weaponButtons.forEach(function(b) {
                b.classList.remove('active');
            });
            document.querySelector('.weapon-btn[data-weapon="' + weaponType + '"]').classList.add('active');
            currentWeapon = {...weapons[weaponType]};
            currentWeapon.ammo = currentWeapon.maxAmmo;
            currentWeapon.totalAmmo = currentWeapon.maxAmmo * 5;
            currentWeapon.canShoot = true;
            
            // Reset zoom when switching weapons
            if (isZoomed && currentWeapon.name !== 'AWP') {
                isZoomed = false;
                player.zoom = 1.0;
                document.getElementById('crosshair').style.transform = 'translate(-50%, -50%) scale(1)';
            }
            
            updateHUD();
        }

        // Select level function
        function selectLevel(level) {
            console.log("Selected level: " + level); // Debug
            currentLevel = level;
            startGame();
        }

        // Setup mobile controls
        function setupMobileControls() {
            // Shoot button
            shootButton.addEventListener('touchstart', function(e) {
                e.preventDefault();
                mouseDown = true;
                shootButton.classList.add('active');
                
                // Для автоматического оружия начинаем стрельбу
                if (currentWeapon.autoFire) {
                    startAutoFire();
                } else {
                    // Для неавтоматического оружия - одиночный выстрел
                    if (currentWeapon.canShoot && !isReloading) {
                        shoot();
                    }
                }
            }, { passive: false });

            shootButton.addEventListener('touchend', function(e) {
                e.preventDefault();
                mouseDown = false;
                shootButton.classList.remove('active');
                stopAutoFire();
            }, { passive: false });

            // Reload button
            reloadButton.addEventListener('touchstart', function(e) {
                e.preventDefault();
                reload();
                reloadButton.classList.add('active');
            }, { passive: false });

            reloadButton.addEventListener('touchend', function(e) {
                e.preventDefault();
                reloadButton.classList.remove('active');
            }, { passive: false });

            // Auto aim button
            autoAimButton.addEventListener('touchstart', function(e) {
                e.preventDefault();
                toggleAutoAim();
            }, { passive: false });

            autoAimButton.addEventListener('touchend', function(e) {
                e.preventDefault();
            }, { passive: false });

            // Joystick
            setupJoystick();
        }

        // Function to start auto-fire
        function startAutoFire() {
            if (autoFireInterval) clearInterval(autoFireInterval);
            
            // First shot immediately
            if (currentWeapon.ammo > 0 && !isReloading) {
                shoot();
            }
            
            // Set interval for subsequent shots
            autoFireInterval = setInterval(function() {
                if (mouseDown && currentWeapon.autoFire && currentWeapon.ammo > 0 && !isReloading) {
                    shoot();
                } else {
                    stopAutoFire();
                }
            }, currentWeapon.fireRate);
        }

        // Function to stop auto-fire
        function stopAutoFire() {
            if (autoFireInterval) {
                clearInterval(autoFireInterval);
                autoFireInterval = null;
            }
        }

        // Setup mouse controls for PC
        function setupMouseControls() {
            canvas.addEventListener('mousedown', function(e) {
                if (e.button === 0) { // Left mouse button
                    mouseDown = true;
                    
                    // For automatic weapons, start firing
                    if (currentWeapon.autoFire) {
                        startAutoFire();
                    } else {
                        // For non-automatic weapons - single shot
                        if (currentWeapon.canShoot && !isReloading) {
                            shoot();
                        }
                    }
                }
            });

            canvas.addEventListener('mouseup', function(e) {
                if (e.button === 0) { // Left mouse button
                    mouseDown = false;
                    stopAutoFire();
                }
            });
        }

        // Setup keyboard controls
        function setupKeyboardControls() {
            // Additional weapon keys
            document.addEventListener('keydown', function(e) {
                if (gamePaused || !gameRunning) return;
                
                switch(e.key.toLowerCase()) {
                    case '1': selectWeapon('pistol'); break;
                    case '2': selectWeapon('rifle'); break;
                    case '3': selectWeapon('shotgun'); break;
                    case '4': selectWeapon('awp'); break;
                    case '5': selectWeapon('rpg'); break;
                    case 'q': reload(); break;
                    case ' ': shoot(); break;
                    case 'z': 
                        if (currentWeapon.name === 'AWP') toggleZoom();
                        break;
                }
            });

            // Movement controls
            window.addEventListener('keydown', function(e) {
                const key = e.key.toLowerCase();
                keys[key] = true;
                
                if (key === currentControls.reload && currentWeapon.ammo < currentWeapon.maxAmmo && currentWeapon.totalAmmo > 0) {
                    reload();
                }
                
                if (key === 'escape') {
                    togglePause();
                }
            });

            window.addEventListener('keyup', function(e) {
                keys[e.key.toLowerCase()] = false;
            });
        }

        // Toggle zoom for AWP
        function toggleZoom() {
            if (currentWeapon.name !== 'AWP') return;
            
            isZoomed = !isZoomed;
            player.zoom = isZoomed ? currentWeapon.zoom : 1.0;
            
            // Update crosshair size based on zoom
            if (isZoomed) {
                document.getElementById('crosshair').style.transform = 'translate(-50%, -50%) scale(0.5)';
            } else {
                document.getElementById('crosshair').style.transform = 'translate(-50%, -50%) scale(1)';
            }
        }

        // Reload function
        function reload() {
            if (isReloading || currentWeapon.ammo === currentWeapon.maxAmmo || currentWeapon.totalAmmo === 0) {
                return;
            }
            
            isReloading = true;
            
            // Добавляем визуальную индикацию перезарядки
            ammoInfo.style.color = '#f39c12';
            ammoInfo.textContent = translations[currentLanguage].reloading + '...';
            
            const ammoNeeded = currentWeapon.maxAmmo - currentWeapon.ammo;
            const ammoToAdd = Math.min(ammoNeeded, currentWeapon.totalAmmo);
            
            setTimeout(function() {
                currentWeapon.ammo += ammoToAdd;
                currentWeapon.totalAmmo -= ammoToAdd;
                isReloading = false;
                ammoInfo.style.color = 'white';
                updateHUD();
            }, currentWeapon.reloadTime);
            
            // Анимация перезарядки
            weaponDisplay.style.transform = 'translateX(-50%) rotate(-30deg)';
            setTimeout(function() {
                weaponDisplay.style.transform = 'translateX(-50%) rotate(0)';
            }, currentWeapon.reloadTime);
        }

        // Toggle auto-aim
        function toggleAutoAim() {
            autoAimEnabled = !autoAimEnabled;
            autoAimButton.classList.toggle('active', autoAimEnabled);
            updateAimButtonText();
        }

        // Update auto-aim button text
        function updateAimButtonText() {
            const lang = translations[currentLanguage];
            toggleAimButton.textContent = lang.autoAim.split(':')[0] + ': ' + (autoAimEnabled ? 'ON' : 'OFF');
            autoAimButton.textContent = lang.aim.split(':')[0] + ': ' + (autoAimEnabled ? 'ON' : 'OFF');
        }

        // Setup joystick
        function setupJoystick() {
            joystickContainer.addEventListener('touchstart', handleJoystickStart, { passive: false });
            joystickContainer.addEventListener('touchmove', handleJoystickMove, { passive: false });
            joystickContainer.addEventListener('touchend', handleJoystickEnd, { passive: false });
            joystickContainer.addEventListener('mousedown', handleJoystickStart);
            document.addEventListener('mousemove', handleJoystickMove);
            document.addEventListener('mouseup', handleJoystickEnd);
        }

        // Handle joystick start
        function handleJoystickStart(e) {
            e.preventDefault();
            if (joystickActive) return;
            
            const touch = e.type === 'mousedown' ? e : e.touches[0];
            const rect = joystickContainer.getBoundingClientRect();
            
            joystickActive = true;
            joystickStartX = rect.left + rect.width / 2;
            joystickStartY = rect.top + rect.height / 2;
            joystickX = touch.clientX - joystickStartX;
            joystickY = touch.clientY - joystickStartY;
            
            if (e.type !== 'mousedown') {
                joystickPointerId = touch.identifier;
            }
            
            updateJoystickPosition();
        }

        // Handle joystick move
        function handleJoystickMove(e) {
            if (!joystickActive) return;
            e.preventDefault();
            
            let touch;
            if (e.type === 'mousemove') {
                touch = e;
            } else {
                touch = Array.from(e.touches).find(function(t) {
                    return t.identifier === joystickPointerId;
                });
                if (!touch) return;
            }
            
            joystickX = touch.clientX - joystickStartX;
            joystickY = touch.clientY - joystickStartY;
            
            const maxDistance = joystickContainer.offsetWidth / 2;
            const distance = Math.sqrt(joystickX * joystickX + joystickY * joystickY);
            
            if (distance > maxDistance) {
                joystickX = (joystickX / distance) * maxDistance;
                joystickY = (joystickY / distance) * maxDistance;
            }
            
            updateJoystickPosition();
        }

        // Handle joystick end
        function handleJoystickEnd(e) {
            if (!joystickActive) return;
            
            if (e.type === 'touchend') {
                const touch = Array.from(e.changedTouches).find(function(t) {
                    return t.identifier === joystickPointerId;
                });
                if (!touch) return;
            }
            
            joystickActive = false;
            joystickX = 0;
            joystickY = 0;
            joystickPointerId = null;
            
            updateJoystickPosition();
        }

        // Update joystick position
        function updateJoystickPosition() {
            const maxDistance = joystickContainer.offsetWidth / 2;
            const distance = Math.sqrt(joystickX * joystickX + joystickY * joystickY);
            const angle = Math.atan2(joystickY, joystickX);
            
            if (distance > 0) {
                const moveDistance = Math.min(distance, maxDistance * 0.5);
                joystick.style.transform = 'translate(' + Math.cos(angle) * moveDistance + 'px, ' + Math.sin(angle) * moveDistance + 'px)';
            } else {
                joystick.style.transform = 'translate(0, 0)';
            }
        }
        
        // Auto-aim function for mobile
        function updateAutoAim() {
            if (!isMobile || !autoAimEnabled || enemies.length === 0) return;
            
            let closestEnemy = null;
            let closestDistance = autoAimRadius;
            
            enemies.forEach(function(enemy) {
                const distance = Math.sqrt(Math.pow(player.x - enemy.x, 2) + Math.pow(player.y - enemy.y, 2));
                if (distance < closestDistance) {
                    closestDistance = distance;
                    closestEnemy = enemy;
                }
            });
            
            if (closestEnemy) {
                mouseX = closestEnemy.x;
                mouseY = closestEnemy.y;
            }
        }
        
        // Return to main menu
        function returnToMenu() {
            cancelAnimationFrame(animationFrameId);
            gameRunning = false;
            pauseMenu.style.display = 'none';
            gameContainer.style.display = 'none';
            menu.style.display = 'flex';
            updateMenuCoinHud();
        }
        
        // Initialize game
        function init() {
            console.log("Initializing game..."); // Debug
            // Clear auto-fire interval
            stopAutoFire();
            
            if (isMobile) {
                player.radius = 15;
                player.speed = 4;
                
                joystickContainer.style.display = 'flex';
                document.querySelector('.mobile-controls').style.display = 'flex';
                
                const controlSize = Math.min(window.innerWidth, window.innerHeight) * 0.15;
                
                joystickContainer.style.width = controlSize * 1.5 + 'px';
                joystickContainer.style.height = controlSize * 1.5 + 'px';
                joystick.style.width = controlSize + 'px';
                joystick.style.height = controlSize + 'px';
                
                const buttons = document.querySelectorAll('.mobile-button');
                buttons.forEach(function(btn) {
                    btn.style.width = controlSize + 'px';
                    btn.style.height = controlSize + 'px';
                    btn.style.fontSize = controlSize * 0.3 + 'px';
                });
                
                joystickContainer.style.bottom = controlSize + 'px';
                joystickContainer.style.left = controlSize + 'px';
                
                const mobileControls = document.querySelector('.mobile-controls');
                mobileControls.style.bottom = controlSize + 'px';
                mobileControls.style.right = controlSize + 'px';
                
                autoAimEnabled = true;
                autoAimButton.classList.add('active');
                updateAimButtonText();
            } else {
                player.radius = 20;
                player.speed = 5;
            }
            
            player.x = canvas.width / 2;
            player.y = canvas.height / 2;
            player.health = player.maxHealth;
            player.score = 0;
            player.kills = 0;
            player.zoom = 1.0;
            enemies = [];
            bullets = [];
            enemyBullets = [];
            healthPacks = [];
            ammoPacks = [];
            enemiesKilled = 0;
            
            const levelSettings = getLevelSettings(currentLevel);
            enemiesToSpawn = levelSettings.enemiesPerWave;
            
            boss = null;
            bossActive = currentLevel % 5 === 0;
            lastAmmoSpawn = 0;
            isReloading = false;
            isZoomed = false;
            
            currentWeapon.ammo = currentWeapon.maxAmmo;
            currentWeapon.totalAmmo = currentWeapon.maxAmmo * 5;
            currentWeapon.canShoot = true;
            
            gameRunning = true;
            gamePaused = false;
            gameOverScreen.style.display = "none";
            bossHealthBar.style.display = "none";
            bossNameDisplay.style.display = "none";
            pauseMenu.style.display = "none";
            mouseDown = false;
            lastShotTime = 0;
            
            // Спавним начальных врагов
            const initialEnemies = Math.min(3 + Math.floor(currentLevel/2), enemiesToSpawn);
            for (let i = 0; i < initialEnemies; i++) {
                spawnEnemy();
            }
            
            updateHUD();
            updateWaveInfo();
            
            // Если это уровень с боссом, спавним его сразу
            if (bossActive) {
                spawnBoss();
            }
            
            applyUpgradesToPlayer();
            player.health = player.maxHealth;
        }
        
        // Spawn enemy function
        function spawnEnemy(type = null) {
            let x, y;
            // Улучшенный алгоритм спавна - враги появляются за пределами экрана
            const side = Math.floor(Math.random() * 4);
            switch(side) {
                case 0: // top
                    x = Math.random() * canvas.width;
                    y = -30;
                    break;
                case 1: // right
                    x = canvas.width + 30;
                    y = Math.random() * canvas.height;
                    break;
                case 2: // bottom
                    x = Math.random() * canvas.width;
                    y = canvas.height + 30;
                    break;
                case 3: // left
                    x = -30;
                    y = Math.random() * canvas.height;
                    break;
            }

            const enemyType = type !== null ? enemyTypes[type] : getRandomEnemyType();
            const levelSettings = getLevelSettings(currentLevel);
            
            // Улучшенные параметры врагов
            const enemy = {
                x: x,
                y: y,
                radius: enemyType.radius,
                health: levelSettings.enemyHealth * (enemyType.healthMultiplier || 1),
                maxHealth: levelSettings.enemyHealth * (enemyType.healthMultiplier || 1),
                damage: levelSettings.enemyDamage * (enemyType.damageMultiplier || 1),
                speed: levelSettings.enemySpeed * (enemyType.speedMultiplier || 1),
                lastShot: 0,
                shootDelay: levelSettings.enemyShootDelay * (enemyType.shootDelayMultiplier || 1),
                type: enemyType.behavior,
                pellets: enemyType.pellets || 1,
                spread: enemyType.spread || 0,
                color: enemyType.color,
                enemyClass: enemyType.class || ''
            };
            
            enemies.push(enemy);
            
            // coinPacks больше не создаём
            return enemy;
        }

        // Get random enemy type
        function getRandomEnemyType() {
            const random = Math.random();
            const levelBonus = Math.min(0.3, currentLevel * 0.03);
            
            if (random > (0.95 - levelBonus)) {
                return enemyTypes.hunter;
            } 
            else if (random > (0.90 - levelBonus)) {
                return enemyTypes.tank;
            }
            else {
                return enemyTypes.regular;
            }
        }

        // Update health packs
        function updateHealthPacks() {
            healthPacks.forEach(function(pack, index) {
                // --- РИСУЕМ ТЕКСТУРУ АПТЕЧКИ ---
                if (healthPackImg.complete && healthPackImg.naturalWidth > 0) {
                    ctx.save();
                    ctx.translate(pack.x, pack.y);
                    ctx.drawImage(healthPackImg, -pack.radius, -pack.radius, pack.radius*2, pack.radius*2);
                    ctx.restore();
                } else {
                    ctx.fillStyle = '#2ecc71';
                    ctx.beginPath();
                    ctx.arc(pack.x, pack.y, pack.radius, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.strokeStyle = 'white';
                    ctx.lineWidth = 2;
                    ctx.beginPath();
                    ctx.moveTo(pack.x - 5, pack.y);
                    ctx.lineTo(pack.x + 5, pack.y);
                    ctx.moveTo(pack.x, pack.y - 5);
                    ctx.lineTo(pack.x, pack.y + 5);
                    ctx.stroke();
                }
                
                const distance = Math.sqrt(Math.pow(pack.x - player.x, 2) + Math.pow(pack.y - player.y, 2));
                if (distance < pack.radius + player.radius) {
                    player.health = Math.min(player.maxHealth, player.health + player.maxHealth * HEALTH_PACK_HEAL);
                    healthPacks.splice(index, 1);
                    updateHUD();
                    
                    showHealthPickupEffect(pack.x, pack.y);
                }
            });
        }
        
        // Show health pickup effect
        function showHealthPickupEffect(x, y) {
            // Проверяем, что координаты являются числами
            if (typeof x !== 'number' || typeof y !== 'number') {
                console.error('Invalid coordinates:', x, y);
                return;
            }

            const effect = document.createElement('div');
            if (!effect) {
                console.error('Failed to create effect element');
                return;
            }

            effect.className = 'health-pickup-effect';
            effect.textContent = translations[currentLanguage].healthPickup.replace('{amount}', Math.round(player.maxHealth * HEALTH_PACK_HEAL));
            
            // Устанавливаем позицию с проверками
            effect.style.position = 'absolute';
            effect.style.left = Math.round(x) + 'px';
            effect.style.top = Math.round(y) + 'px';
            
            const gameContainer = document.getElementById('gameContainer');
            if (gameContainer) {
                gameContainer.appendChild(effect);
                
                setTimeout(function() {
                    if (effect.parentNode) {
                        effect.parentNode.removeChild(effect);
                    }
                }, 1000);
            } else {
                console.error('gameContainer not found');
            }
        }
        
        // Create explosion effect
        // --- ВЗРЫВЫ ---
        const explosions = [];
        const explosionImg = new Image();
        explosionImg.src = 'explosion.png'; // Положите explosion.png (642x162) рядом с этим HTML-файлом

        // explosion.png: 5 кадров, ширины [64, 152, 134, 145, 151], высота 162
        const EXPLOSION_FRAME_WIDTHS = [64, 152, 134, 145, 151];
        const EXPLOSION_FRAME_HEIGHT = 162;
        const EXPLOSION_FRAMES = 5;
        const EXPLOSION_DURATION = 500; // мс

        function createExplosion(x, y, radius) {
            explosions.push({
                x, y, radius,
                start: performance.now(),
                frame: 0
            });
        }

        function updateExplosions() {
            const now = performance.now();
            for (let i = explosions.length - 1; i >= 0; i--) {
                const exp = explosions[i];
                const elapsed = now - exp.start;
                if (elapsed > EXPLOSION_DURATION) {
                    explosions.splice(i, 1);
                    continue;
                }
                // Определяем текущий кадр
                const frame = Math.floor((elapsed / EXPLOSION_DURATION) * EXPLOSION_FRAMES);
                exp.frame = Math.min(frame, EXPLOSION_FRAMES - 1);
                // Считаем смещение по X для текущего кадра
                let sx = 0;
                for (let f = 0; f < exp.frame; f++) {
                    sx += EXPLOSION_FRAME_WIDTHS[f];
                }
                const sw = EXPLOSION_FRAME_WIDTHS[exp.frame];
                // Рисуем спрайт
                if (explosionImg.complete && explosionImg.naturalWidth > 0) {
                    ctx.save();
                    ctx.translate(exp.x, exp.y);
                    ctx.drawImage(
                        explosionImg,
                        sx, 0,
                        sw, EXPLOSION_FRAME_HEIGHT,
                        -exp.radius, -exp.radius,
                        exp.radius * 2, exp.radius * 2
                    );
                    ctx.restore();
                } else {
                    // fallback: круг
                    ctx.save();
                    ctx.globalAlpha = 0.7;
                    ctx.beginPath();
                    ctx.arc(exp.x, exp.y, exp.radius, 0, Math.PI * 2);
                    ctx.fillStyle = 'orange';
                    ctx.fill();
                    ctx.restore();
                }
            }
        }

        // Game loop
        let lastFrameTime = null;

        function gameLoop(timestamp) {
            if (!gameRunning || gamePaused) return;
            animationFrameId = requestAnimationFrame(gameLoop);

            // --- Time delta (dt) ---
            if (!lastFrameTime) lastFrameTime = timestamp;
            let dt = (timestamp - lastFrameTime) / 1000; // в секундах
            if (dt > 0.1) dt = 0.1; // ограничение на случай лагов
            lastFrameTime = timestamp;

            updateAutoAim();
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            ctx.fillStyle = '#333';
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            // Спавн патронов
            if (timestamp - lastAmmoSpawn > AMMO_SPAWN_INTERVAL) {
                spawnAmmoPack();
                lastAmmoSpawn = timestamp;
            }

            updateAmmoPacks();
            updateHealthPacks();
            updateCoinPacks();
            updatePlayer(dt);

            if (!bossActive) {
                const enemiesAlive = enemies.length;
                const enemiesTotal = enemiesKilled + enemiesAlive;
                const levelSettings = getLevelSettings(currentLevel);
                if (enemiesTotal < enemiesToSpawn && 
                    timestamp - lastEnemySpawn > levelSettings.spawnRate &&
                    enemiesAlive < 5 + currentLevel * 2) {
                    spawnEnemy();
                    lastEnemySpawn = timestamp;
                }
                if (enemiesTotal >= enemiesToSpawn && enemiesAlive === 0) {
                    levelComplete();
                }
            }

            updateEnemies(timestamp, dt);
            updateBullets(dt);
            updateEnemyBullets(dt);
            if (bossActive) {
                updateBoss(dt);
            }

            updateFps(dt); // <-- FPS update
            
            // ВЗРЫВЫ
            updateExplosions();
        }

        // Update player position and draw
        function updatePlayer(dt = 1) {
            let moveX = 0;
            let moveY = 0;
            let speed = player.speed * dt * 60; // вернул *60
            if (keys[currentControls.forward] || keys['arrowup']) moveY -= speed;
            if (keys[currentControls.backward] || keys['arrowdown']) moveY += speed;
            if (keys[currentControls.left] || keys['arrowleft']) moveX -= speed;
            if (keys[currentControls.right] || keys['arrowright']) moveX += speed;
            if (joystickActive) {
                const joystickDistance = Math.sqrt(joystickX * joystickX + joystickY * joystickY);
                if (joystickDistance > 0) {
                    const normalizedX = joystickX / joystickDistance;
                    const normalizedY = joystickY / joystickDistance;
                    const moveDistance = Math.min(joystickDistance, joystickContainer.offsetWidth / 2) / (joystickContainer.offsetWidth / 2);
                    moveX += normalizedX * speed * moveDistance;
                    moveY += normalizedY * speed * moveDistance;
                }
            }
            if (moveX !== 0 && moveY !== 0) {
                const length = Math.sqrt(moveX * moveX + moveY * moveY);
                moveX = (moveX / length) * speed;
                moveY = (moveY / length) * speed;
            }
            player.x = Math.max(player.radius, Math.min(canvas.width - player.radius, player.x + moveX));
            player.y = Math.max(player.radius, Math.min(canvas.height - player.radius, player.y + moveY));
            player.angle = Math.atan2(mouseY - player.y, mouseX - player.x);
            ctx.fillStyle = '#3498db';
            ctx.beginPath();
            ctx.arc(player.x, player.y, player.radius, 0, Math.PI * 2);
            ctx.fill();
            ctx.strokeStyle = 'white';
            ctx.lineWidth = 2;
            ctx.stroke();
        }

        // Update enemies and draw
        function updateEnemies(timestamp, dt = 1) {
            console.log('enemies for draw:', enemies.map(e => [e.x, e.y, e.radius]));
            enemies.forEach(function(enemy, index) {
                // ВРЕМЕННО: рисуем всех врагов ярко-зелёным
                ctx.beginPath();
                ctx.arc(enemy.x, enemy.y, enemy.radius, 0, Math.PI * 2);
                ctx.fillStyle = 'lime';
                ctx.globalAlpha = 1;
                ctx.fill();
                ctx.strokeStyle = 'black';
                ctx.lineWidth = 2;
                ctx.stroke();
                ctx.globalAlpha = 1;
                
                const angle = Math.atan2(player.y - enemy.y, player.x - enemy.x);
                let speed = enemy.speed * dt * 60; // вернул *60
                enemy.x += Math.cos(angle) * speed;
                enemy.y += Math.sin(angle) * speed;
                
                if (timestamp - enemy.lastShot > enemy.shootDelay && enemy.health > 0) {
                    enemy.lastShot = timestamp;
                    
                    if (enemy.type === 'melee') {
                        const distance = Math.sqrt(Math.pow(player.x - enemy.x, 2) + Math.pow(player.y - enemy.y, 2));
                        if (distance < enemy.radius + player.radius + 20) {
                            let dmg = enemy.damage;
                            if (player.defense) dmg = Math.round(dmg * (1 - player.defense / 100));
                            player.health -= dmg;
                            updateHUD();
                            
                            if (player.health <= 0) {
                                gameOver();
                            }
                        }
                    } else if (enemy.type === 'shotgun') {
                        for (let i = -1; i <= 1; i++) {
                            const spreadAngle = (Math.random() - 0.5) * enemy.spread;
                            enemyBullets.push({
                                x: enemy.x,
                                y: enemy.y,
                                angle: angle + spreadAngle,
                                speed: 5,
                                radius: 3,
                                damage: enemy.damage / 2
                            });
                        }
                    } else {
                        enemyBullets.push({
                            x: enemy.x,
                            y: enemy.y,
                            angle: angle,
                            speed: 5,
                            radius: 5,
                            damage: enemy.damage
                        });
                    }
                }
                
                if (enemy.health > 0) {
                    ctx.fillStyle = enemy.color;
                    ctx.beginPath();
                    ctx.arc(enemy.x, enemy.y, enemy.radius, 0, Math.PI * 2);
                    ctx.fill();
                    
                    ctx.strokeStyle = 'black';
                    ctx.lineWidth = 2;
                    ctx.stroke();
                    
                    const healthBarWidth = 30 + (enemy.radius - 15);
                    const healthBarHeight = 5;
                    const healthBarX = enemy.x - healthBarWidth/2;
                    const healthBarY = enemy.y - enemy.radius - 10;
                    
                    ctx.fillStyle = '#444';
                    ctx.fillRect(healthBarX, healthBarY, healthBarWidth, healthBarHeight);
                    
                    const healthWidth = healthBarWidth * (enemy.health / enemy.maxHealth);
                    ctx.fillStyle = '#2ecc71';
                    ctx.fillRect(healthBarX, healthBarY, healthWidth, healthBarHeight);
                    
                    if ((enemy.type === 'melee' || enemy.type === 'shotgun') && enemy.health > 0) {
                        ctx.fillStyle = 'white';
                        ctx.font = 'bold ' + enemy.radius + 'px Arial';
                        ctx.textAlign = 'center';
                        ctx.textBaseline = 'middle';
                        const symbol = enemy.type === 'melee' ? 'T' : 'H';
                        ctx.fillText(symbol, enemy.x, enemy.y);
                    }
                }
                
                if (enemy.type !== 'melee' && enemy.health > 0) {
                    const distance = Math.sqrt(Math.pow(player.x - enemy.x, 2) + Math.pow(player.y - enemy.y, 2));
                    if (distance < player.radius + enemy.radius) {
                        let dmg = enemy.damage;
                        if (player.defense) dmg = Math.round(dmg * (1 - player.defense / 100));
                        player.health -= dmg;
                        updateHUD();
                        
                        const knockbackAngle = Math.atan2(player.y - enemy.y, player.x - enemy.x);
                        player.x += Math.cos(knockbackAngle) * 10;
                        player.y += Math.sin(knockbackAngle) * 10;
                        
                        if (player.health <= 0) {
                            gameOver();
                        }
                    }
                }
            });
        }

        // Update bullets
        function updateBullets(dt = 1) {
            for (let i = bullets.length - 1; i >= 0; i--) {
                const bullet = bullets[i];
                bullet.x += Math.cos(bullet.angle) * bullet.speed * dt * 100;
                bullet.y += Math.sin(bullet.angle) * bullet.speed * dt * 100;
                
                ctx.fillStyle = bullet.color;
                ctx.beginPath();
                ctx.arc(bullet.x, bullet.y, bullet.radius, 0, Math.PI * 2);
                ctx.fill();
                
                if (bullet.x < 0 || bullet.x > canvas.width || bullet.y < 0 || bullet.y > canvas.height) {
                    bullets.splice(i, 1);
                    continue;
                }
                
                for (let j = enemies.length - 1; j >= 0; j--) {
                    const enemy = enemies[j];
                    const distance = Math.sqrt(Math.pow(bullet.x - enemy.x, 2) + Math.pow(bullet.y - enemy.y, 2));
                    if (distance < bullet.radius + enemy.radius) {
                        if (currentWeapon.name === 'RPG') {
                            createExplosion(bullet.x, bullet.y, currentWeapon.explosionRadius);
                            
                            enemies.forEach(function(e) {
                                const explosionDistance = Math.sqrt(Math.pow(bullet.x - e.x, 2) + Math.pow(bullet.y - e.y, 2));
                                if (explosionDistance < currentWeapon.explosionRadius) {
                                    const t = explosionDistance / currentWeapon.explosionRadius;
                                    const damageFactor = 1 - 0.5 * t - 0.5 * t * t;
                                    e.health -= currentWeapon.explosionDamage * damageFactor;
                                    
                                    if (e.health > 0) {
                                        ctx.fillStyle = 'white';
                                        ctx.beginPath();
                                        ctx.arc(e.x, e.y, e.radius, 0, Math.PI * 2);
                                        ctx.fill();
                                        setTimeout(function() {
                                            ctx.fillStyle = e.color;
                                            ctx.beginPath();
                                            ctx.arc(e.x, e.y, e.radius, 0, Math.PI * 2);
                                            ctx.fill();
                                        }, 100);
                                    }
                                }
                            });
                        } else {
                            enemy.health -= bullet.damage;
                        }
                        
                        bullets.splice(i, 1);
                        
                        if (enemy.health <= 0) {
                            enemies.splice(j, 1);
                            player.kills++;
                            player.score += 10;
                            enemiesKilled++;
                            updateWaveInfo();
                            
                            // НАЧИСЛЯЕМ МОНЕТЫ
                            let coinCount = 1 + Math.floor(Math.random() * 4);
                            coins += coinCount;
                            saveSettings();
                            updateHUD();
                            updateWaveInfo();
                            if (Math.random() < HEALTH_PACK_CHANCE) {
                                healthPacks.push({
                                    x: enemy.x,
                                    y: enemy.y,
                                    radius: 18 // увеличенный размер аптечки
                                });
                            }
                        }
                        break;
                    }
                }
            }
        }

        // Update enemy bullets
        function updateEnemyBullets(dt = 1) {
            enemyBullets.forEach(function(bullet, index) {
                bullet.x += Math.cos(bullet.angle) * bullet.speed * dt * 100;
                bullet.y += Math.sin(bullet.angle) * bullet.speed * dt * 100;
                
                ctx.fillStyle = '#e74c3c';
                ctx.beginPath();
                ctx.arc(bullet.x, bullet.y, bullet.radius, 0, Math.PI * 2);
                ctx.fill();
                
                if (bullet.x < 0 || bullet.x > canvas.width || bullet.y < 0 || bullet.y > canvas.height) {
                    enemyBullets.splice(index, 1);
                    return;
                }
                
                const distance = Math.sqrt(Math.pow(bullet.x - player.x, 2) + Math.pow(bullet.y - player.y, 2));
                if (distance < bullet.radius + player.radius) {
                    let dmg = bullet.damage;
                    if (player.defense) dmg = Math.round(dmg * (1 - player.defense / 100));
                    player.health -= dmg;
                    enemyBullets.splice(index, 1);
                    updateHUD();
                    
                    if (player.health <= 0) {
                        gameOver();
                    }
                    return;
                }
            });
        }

        // Update ammo packs
        function updateAmmoPacks() {
            ammoPacks.forEach(function(pack, index) {
                // --- РИСУЕМ ТЕКСТУРУ ОБОЙМЫ ---
                if (ammoPackImg.complete && ammoPackImg.naturalWidth > 0) {
                    ctx.save();
                    ctx.translate(pack.x, pack.y);
                    ctx.drawImage(ammoPackImg, -36, -22.5, 72, 45);
                    ctx.restore();
                } else {
                    ctx.fillStyle = '#f1c40f';
                    ctx.beginPath();
                    ctx.arc(pack.x, pack.y, pack.radius, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.fillStyle = '#e67e22';
                    ctx.beginPath();
                    ctx.arc(pack.x, pack.y, pack.radius / 2, 0, Math.PI * 2);
                    ctx.fill();
                }
                
                const distance = Math.sqrt(Math.pow(pack.x - player.x, 2) + Math.pow(pack.y - player.y, 2));
                if (distance < pack.radius + player.radius) {
                    currentWeapon.totalAmmo += AMMO_PACK_AMOUNT;
                    ammoPacks.splice(index, 1);
                    updateHUD();
                    
                    showAmmoPickupEffect(pack.x, pack.y);
                }
            });
        }

        // Show ammo pickup effect
        function showAmmoPickupEffect(x, y) {
            if (typeof x !== 'number' || typeof y !== 'number') {
                console.error('Invalid coordinates:', x, y);
                return;
            }

            const effect = document.createElement('div');
            if (!effect) return;

            effect.className = 'ammo-pickup-effect';
            effect.textContent = translations[currentLanguage].ammoPickup.replace('{amount}', AMMO_PACK_AMOUNT);
            
            effect.style.position = 'absolute';
            effect.style.left = Math.round(x) + 'px';
            effect.style.top = Math.round(y) + 'px';
            
            const container = document.getElementById('gameContainer');
            if (container) {
                container.appendChild(effect);
                
                setTimeout(function() {
                    if (effect.parentNode) {
                        effect.parentNode.removeChild(effect);
                    }
                }, 1000);
            }
        }

        // Spawn ammo pack
        function spawnAmmoPack() {
            if (ammoPacks.length >= 3) return;
            
            let x, y;
            do {
                x = 30 + Math.random() * (canvas.width - 60);
                y = 30 + Math.random() * (canvas.height - 60);
            } while (Math.sqrt(Math.pow(x - player.x, 2) + Math.pow(y - player.y, 2)) < 100);
            
            ammoPacks.push({
                x: x,
                y: y,
                radius: 36 // половина ширины текстуры для коллизии
            });
        }

        // Shoot function
        function shoot() {
            if (currentWeapon.ammo <= 0 || isReloading) {
                return;
            }
            
            if (currentWeapon.name === 'AWP' && (!currentWeapon.canShoot || Date.now() - lastShotTime < currentWeapon.fireRate)) {
                return;
            }
            
            if (isMobile) {
                shootButton.style.transform = 'scale(0.9)';
                setTimeout(function() {
                    shootButton.style.transform = 'scale(1)';
                }, 100);
            }
            
            currentWeapon.ammo--;
            lastShotTime = Date.now();
            
            if (currentWeapon.name === 'AWP') {
                currentWeapon.canShoot = false;
                setTimeout(function() {
                    currentWeapon.canShoot = true;
                }, currentWeapon.fireRate);
            }
            
            updateHUD();
            
            weaponDisplay.style.transform = 'translateX(-50%) scale(0.95)';
            setTimeout(function() {
                weaponDisplay.style.transform = 'translateX(-50%) scale(1)';
            }, 100);
            
            if (currentWeapon.name === 'Shotgun') {
                for (let i = 0; i < currentWeapon.pellets; i++) {
                    const spreadAngle = (Math.random() - 0.5) * currentWeapon.spread;
                    bullets.push({
                        x: player.x + Math.cos(player.angle) * player.radius,
                        y: player.y + Math.sin(player.angle) * player.radius,
                        angle: player.angle + spreadAngle,
                        speed: currentWeapon.bulletSpeed,
                        radius: 3,
                        damage: currentWeapon.damage,
                        color: currentWeapon.color
                    });
                }
            } else if (currentWeapon.name === 'AWP') {
                bullets.push({
                    x: player.x + Math.cos(player.angle) * player.radius,
                    y: player.y + Math.sin(player.angle) * player.radius,
                    angle: player.angle,
                    speed: currentWeapon.bulletSpeed,
                    radius: 8,
                    damage: currentWeapon.damage,
                    color: currentWeapon.color
                });
                
                const recoilAngle = player.angle + (Math.random() - 0.5) * 0.1;
                const recoilDistance = 10;
                player.x -= Math.cos(recoilAngle) * recoilDistance;
                player.y -= Math.sin(recoilAngle) * recoilDistance;
            } else if (currentWeapon.name === 'RPG') {
                bullets.push({
                    x: player.x + Math.cos(player.angle) * player.radius,
                    y: player.y + Math.sin(player.angle) * player.radius,
                    angle: player.angle,
                    speed: currentWeapon.bulletSpeed,
                    radius: 10,
                    damage: currentWeapon.damage,
                    color: currentWeapon.color,
                    isRocket: true
                });
            } else {
                bullets.push({
                    x: player.x + Math.cos(player.angle) * player.radius,
                    y: player.y + Math.sin(player.angle) * player.radius,
                    angle: player.angle,
                    speed: currentWeapon.bulletSpeed,
                    radius: 5,
                    damage: currentWeapon.damage,
                    color: currentWeapon.color
                });
            }
            
            if (currentWeapon.ammo <= 0 && currentWeapon.totalAmmo > 0 && !isReloading) {
                setTimeout(function() {
                    if (currentWeapon.ammo <= 0) { // Проверяем еще раз, так как игрок мог вручную перезарядиться
                        reload();
                    }
                }, 300); // Небольшая задержка перед автоматической перезарядкой
            }
        }

        // Level complete
        function levelComplete() {
            console.log("Level " + currentLevel + " complete!"); // Debug
            // Unlock next level if this is the highest level reached
            if (currentLevel === unlockedLevel && unlockedLevel < TOTAL_LEVELS) {
                unlockedLevel++;
                saveSettings();
                console.log("Unlocked level " + unlockedLevel); // Debug
            }
            
            gameOver(true);
        }

        // Spawn boss
        function spawnBoss() {
            console.log("Spawning boss..."); // Debug
            bossActive = true;
            enemiesToSpawn = 0;
            
            bossHealthBar.style.display = 'block';
            bossNameDisplay.style.display = 'block';
            bossNameDisplay.textContent = translations[currentLanguage].bossLevel.replace('{level}', currentLevel);
            
            const levelSettings = getLevelSettings(currentLevel);
            
            boss = {
                x: canvas.width / 2,
                y: -100,
                radius: 40 + currentLevel * 2,
                health: 500 * (1 + (currentLevel-1) * 0.3),
                maxHealth: 500 * (1 + (currentLevel-1) * 0.3),
                damage: 10 + currentLevel * 1.5,
                speed: levelSettings.bossSpeed,
                lastShot: 0,
                shootDelay: 1500 - currentLevel * 30, // Босс стреляет чаще на высоких уровнях
                phase: 0,
                moveAngle: 0,
                color: '#e74c3c',
                isBoss: true
            };
            
            enemies.push(boss);
        }

        // Update boss
        function updateBoss(dt = 1) {
            if (!bossActive || !boss) return;
            
            // Обновляем полосу здоровья босса
            const bossHealthFill = document.querySelector('.boss-health-fill');
            const healthPercent = (boss.health / boss.maxHealth) * 100;
            bossHealthFill.style.width = healthPercent + '%';
            
            // Улучшенное движение босса
            boss.moveAngle += 0.015 * dt*25;
            const moveRadius = canvas.width / 3;
            boss.x = canvas.width / 2 + Math.cos(boss.moveAngle) * moveRadius;
            boss.y = canvas.height / 3 + Math.sin(boss.moveAngle * 2) * (moveRadius / 2);
            
            // Улучшенная стрельба босса
            if (Date.now() - boss.lastShot > boss.shootDelay) {
                boss.lastShot = Date.now();
                
                const angle = Math.atan2(player.y - boss.y, player.x - boss.x);
                
                // Разные фазы атаки
                if (boss.health / boss.maxHealth > 0.5) {
                    // Фаза 1 - обычные выстрелы
                    for (let i = -1; i <= 1; i++) {
                        enemyBullets.push({
                            x: boss.x,
                            y: boss.y,
                            angle: angle + i * 0.2,
                            speed: 3 + currentLevel * 0.1,
                            radius: 8,
                            damage: boss.damage
                        });
                    }
                } else {
                    // Фаза 2 - усиленные атаки
                    for (let i = -2; i <= 2; i++) {
                        enemyBullets.push({
                            x: boss.x,
                            y: boss.y,
                            angle: angle + i * 0.3,
                            speed: 4 + currentLevel * 0.1,
                            radius: 10,
                            damage: boss.damage * 1.5
                        });
                    }
                }
            }
            
            // Отрисовка босса
            ctx.fillStyle = boss.color;
            ctx.beginPath();
            ctx.arc(boss.x, boss.y, boss.radius, 0, Math.PI * 2);
            ctx.fill();
            
            ctx.strokeStyle = 'black';
            ctx.lineWidth = 3;
            ctx.stroke();
            
            // Проверка смерти босса
            if (boss.health <= 0) {
                bossActive = false;
                bossHealthBar.style.display = 'none';
                bossNameDisplay.style.display = 'none';
                player.score += 100 * currentLevel;

                // Мгновенное зачисление монет с босса
                const coinsToDrop = 5;
                const bossCoinAmount = coinsToDrop * 50;
                coins += bossCoinAmount;
                saveSettings();
                updateHUD();
                // Показываем эффект сбора монет в центре босса
                showCoinPickupEffect(boss.x, boss.y, bossCoinAmount);

                const bossIndex = enemies.findIndex(function(e) {
                    return e === boss;
                });
                if (bossIndex !== -1) {
                    enemies.splice(bossIndex, 1);
                }

                boss = null;
                levelComplete();
            }
        }

        // Game over
        function gameOver(victory = false) {
            console.log("Game over, victory: " + victory); // Debug
            cancelAnimationFrame(animationFrameId);
            gameRunning = false;
            gameOverScreen.style.display = "flex";
            
            const lang = translations[currentLanguage];
            nextLevelButton.classList.toggle('hidden', !victory || currentLevel >= TOTAL_LEVELS);
            
            if (victory) {
                document.querySelector('#gameOver h1').textContent = lang.victory;
                resultStats.innerHTML = '<p>' + lang.finalScore + ' ' + player.score + '</p>' +
                                       '<p>' + lang.totalKills + ' ' + player.kills + '</p>' +
                                       '<p>' + lang.levelsCompleted + ' ' + currentLevel + '/' + TOTAL_LEVELS + '</p>';
                
                if (currentLevel === unlockedLevel && unlockedLevel < TOTAL_LEVELS) {
                    unlockedLevel++;
                    saveSettings();
                }
            } else {
                document.querySelector('#gameOver h1').textContent = lang.gameOver;
                resultStats.innerHTML = '<p>' + lang.score + ' ' + player.score + '</p>' +
                                       '<p>' + lang.kills + ' ' + player.kills + '</p>' +
                                       '<p>' + lang.levelReached + ' ' + currentLevel + '/' + TOTAL_LEVELS + '</p>';
            }
        }

        // Update HUD
        function updateHUD() {
            healthFill.style.width = (player.health / player.maxHealth) * 100 + '%';
            ammoInfo.textContent = currentWeapon.ammo + '/' + currentWeapon.totalAmmo;
            if (!document.getElementById('coinCounter')) {
                const coinDiv = document.createElement('div');
                coinDiv.id = 'coinCounter';
                coinDiv.style.position = 'absolute';
                coinDiv.style.top = '10px';
                coinDiv.style.left = '230px'; // справа от полоски здоровья (healthBar шириной 200px + отступ)
                coinDiv.style.background = 'rgba(0,0,0,0.6)';
                coinDiv.style.borderRadius = '5px';
                coinDiv.style.padding = '6px 18px';
                coinDiv.style.fontSize = '18px';
                coinDiv.style.color = '#f1c40f';
                coinDiv.style.display = 'flex';
                coinDiv.style.alignItems = 'center';
                coinDiv.innerHTML = '<span style="font-size:22px;margin-right:8px;"><img src="coin.png" width="38" height="51" style="vertical-align:middle;"></span><span id="coinAmount">0</span>';
                gameContainer.appendChild(coinDiv);
            }
            document.getElementById('coinAmount').textContent = coins;
            updateMenuCoinHud();
        }
        
        // Update wave info
        function updateWaveInfo() {
            const lang = translations[currentLanguage];
            
            if (bossActive) {
                waveInfo.textContent = lang.level + ' ' + currentLevel + ': ' + lang.boss + ' | ' + lang.kills.split(':')[0] + ': ' + enemies.length;
            } else {
                const enemiesRemaining = Math.max(0, enemiesToSpawn - enemiesKilled);
                waveInfo.textContent = lang.level + ' ' + currentLevel + ' | ' + lang.kills.split(':')[0] + ': ' + (enemiesRemaining + enemies.length);
            }
        }
        
        // Start game
        function startGame() {
            console.log("Starting game..."); // Debug
            menu.style.display = "none";
            gameContainer.style.display = "block";
            
            if (isMobile) {
                requestFullscreen();
            }
            
            init();
            animationFrameId = requestAnimationFrame(gameLoop);
            console.log('canvas.width:', canvas.width, 'canvas.height:', canvas.height);
        }
        
        // Toggle pause
        function togglePause() {
            if (!gameRunning) return;
            
            gamePaused = !gamePaused;
            
            if (gamePaused) {
                // Обновляем перевод перед показом меню паузы
                const lang = translations[currentLanguage];
                document.querySelector('#pauseMenu h2').textContent = lang.gamePaused;
                resumeButton.textContent = lang.resume;
                toggleAimButton.textContent = lang.autoAim.split(':')[0] + ': ' + (autoAimEnabled ? 'ON' : 'OFF');
                mainMenuButton.textContent = lang.mainMenu;
                
                pauseMenu.style.display = "flex";
                cancelAnimationFrame(animationFrameId);
            } else {
                pauseMenu.style.display = "none";
                animationFrameId = requestAnimationFrame(gameLoop);
            }
        }
        
        // Initialize the game when DOM is loaded
        document.addEventListener('DOMContentLoaded', function() {
            console.log("DOM fully loaded and parsed"); // Debug
            if (isMobile && screen.orientation && screen.orientation.lock) {
                screen.orientation.lock('landscape').catch(function(e) {
                    console.log('Orientation lock failed: ', e);
                });
            }
            
            resizeCanvas();
            initializeControls();
            
            selectWeapon('pistol');
            
            window.addEventListener('resize', resizeCanvas);
            window.addEventListener('orientationchange', function() {
                resizeCanvas();
                if (gameRunning) {
                    player.x = canvas.width / 2;
                    player.y = canvas.height / 2;
                }
            });
            
            canvas.addEventListener('mousemove', mouseMove);
            canvas.addEventListener('mousedown', function(e) {
                if (currentControls.shoot === 'mouse' + e.button && !mouseDown) {
                    mouseDown = true;
                    if (currentWeapon.autoFire) {
                        startAutoFire();
                    } else {
                        if (currentWeapon.canShoot && !isReloading) {
                            shoot();
                        }
                    }
                }
            });
        });

        // Update coins (draw and collect)
        function updateCoinPacks() {
            for (let i = coinPacks.length - 1; i >= 0; i--) {
                const pack = coinPacks[i];
                ctx.save();
                ctx.beginPath();
                ctx.arc(pack.x, pack.y, pack.radius, 0, Math.PI * 2);
                ctx.fillStyle = '#f1c40f';
                ctx.shadowColor = '#fff';
                ctx.shadowBlur = 8;
                ctx.fill();
                ctx.restore();
                ctx.font = 'bold 16px Arial';
                ctx.fillStyle = '#fff';
                ctx.textAlign = 'center';
                ctx.textBaseline = 'middle';
                ctx.fillText('', pack.x, pack.y); // удаляем символ 💲, т.к. теперь только картинка
                // Сбор монеты
                const dist = Math.sqrt((pack.x - player.x) ** 2 + (pack.y - player.y) ** 2);
                if (dist < pack.radius + player.radius) {
                    coins += pack.amount;
                    saveSettings();
                    showCoinPickupEffect(pack.x, pack.y, pack.amount);
                    coinPacks.splice(i, 1);
                    updateHUD();
                }
            }
        }

        // Показываем эффект сбора монеты
        function showCoinPickupEffect(x, y, amount) {
            const effect = document.createElement('div');
            effect.className = 'ammo-pickup-effect';
            effect.style.color = '#f1c40f';
            effect.innerHTML = `+${amount} <img src="coin.png" width="38" height="51" style="vertical-align:middle;">`;
            effect.style.left = Math.round(x) + 'px';
            effect.style.top = Math.round(y) + 'px';
            const container = document.getElementById('gameContainer');
            if (container) {
                container.appendChild(effect);
                setTimeout(() => { if (effect.parentNode) effect.parentNode.removeChild(effect); }, 1000);
            }
        }

        // Upgrades variables
        let hpUpgradeLevel = 0;
        let defUpgradeLevel = 0;
        let hpUpgradeBaseCost = 100;
        let defUpgradeBaseCost = 200;
        let hpUpgradeCost = hpUpgradeBaseCost;
        let defUpgradeCost = defUpgradeBaseCost;
        
        // Upgrades elements
        const upgradesButton = document.getElementById('upgradesButton');
        const upgradesModal = document.getElementById('upgradesModal');
        const backFromUpgrades = document.getElementById('backFromUpgrades');
        const buyHpUpgrade = document.getElementById('buyHpUpgrade');
        const buyDefUpgrade = document.getElementById('buyDefUpgrade');
        const upgradeHpCost = document.getElementById('upgradeHpCost');
        const upgradeDefCost = document.getElementById('upgradeDefCost');
        
        // Открытие/закрытие окна улучшений
        upgradesButton.addEventListener('click', function() {
            updateUpgradeUI();
            upgradesModal.style.display = 'flex';
        });
        backFromUpgrades.addEventListener('click', function() {
            upgradesModal.style.display = 'none';
        });
        
        // Покупка улучшения HP
        buyHpUpgrade.addEventListener('click', function() {
            if (coins >= hpUpgradeCost) {
                coins -= hpUpgradeCost;
                hpUpgradeLevel++;
                hpUpgradeCost = Math.floor(hpUpgradeBaseCost * Math.pow(1.1, hpUpgradeLevel));
                saveSettings();
                updateUpgradeUI();
                updateHUD();
            }
        });
        // Покупка улучшения защиты
        buyDefUpgrade.addEventListener('click', function() {
            if (coins >= defUpgradeCost && defUpgradeLevel < 6) {
                coins -= defUpgradeCost;
                defUpgradeLevel++;
                defUpgradeCost = Math.floor(defUpgradeBaseCost * Math.pow(1.1, defUpgradeLevel));
                saveSettings();
                updateUpgradeUI();
                updateHUD();
            }
        });
        // Обновление UI апгрейдов
        function updateUpgradeUI() {
            upgradeHpCost.innerHTML = hpUpgradeCost + ' <img src="coin.png" width="38" height="51" style="vertical-align:middle;">';
            upgradeDefCost.innerHTML = defUpgradeCost + ' <img src="coin.png" width="38" height="51" style="vertical-align:middle;">';
            buyDefUpgrade.disabled = defUpgradeLevel >= 6;
            if (defUpgradeLevel >= 6) {
                upgradeDefCost.textContent = translations[currentLanguage].max;
            }
        }
        // Применение апгрейдов к игроку
        function applyUpgradesToPlayer() {
            player.maxHealth = 100 + hpUpgradeLevel * 10;
            player.defense = Math.min(defUpgradeLevel * 5, 30);
        }

        // Добавляю функцию для обновления монет в меню
        function updateMenuCoinHud() {
            var el = document.getElementById('menuCoinAmount');
            if (el) el.textContent = coins;
        }

        // FPS toggle logic
        function updateFpsButtonText() {
            const lang = translations[currentLanguage];
            toggleFpsCheckbox.textContent = showFps ? lang.fpsHide : lang.fpsShow;
        }
        function updateFpsLabel() {
            const lang = translations[currentLanguage];
            // Найти все секции настроек
            const sections = document.querySelectorAll('#settingsModal .settings-section');
            for (const section of sections) {
                const title = section.querySelector('.settings-section-title');
                if (title && title.textContent.trim().toUpperCase() === 'FPS') {
                    const label = section.querySelector('.settings-option-label');
                    if (label) label.textContent = lang.showFps;
                }
            }
        }
        function updateFpsCheckbox() {
            toggleFpsCheckbox.checked = showFps;
        }
        toggleFpsCheckbox.addEventListener('change', function() {
            showFps = toggleFpsCheckbox.checked;
            localStorage.setItem('standoff2_showFps', showFps ? '1' : '0');
            fpsCounter.style.display = showFps ? 'block' : 'none';
        });

        // FPS calculation
        let lastFpsUpdate = 0;
        let frames = 0;
        let fps = 0;
        function updateFps(dt) {
            frames++;
            if (performance.now() - lastFpsUpdate > 500) {
                fps = Math.round((frames * 1000) / (performance.now() - lastFpsUpdate));
                lastFpsUpdate = performance.now();
                frames = 0;
                if (showFps) {
                    fpsCounter.textContent = fps + ' FPS';
                }
            }
        }

        // Восстановление showFps из localStorage
        const savedShowFps = localStorage.getItem('standoff2_showFps');
        if (savedShowFps === '1') {
            showFps = true;
            fpsCounter.style.display = 'block';
        }
        updateFpsCheckbox();

        // Вызов обновления текста кнопки FPS при смене языка
        const originalSetLanguage = setLanguage;
        setLanguage = function(lang) {
            originalSetLanguage(lang);
            updateDonationUI();
            updateFpsButtonText && updateFpsButtonText();
            updateFpsLabel && updateFpsLabel();
            updateFpsCheckbox();
        };

        // Инициализация текста кнопки FPS
        updateFpsButtonText();
        updateFpsLabel();

        // Глобально доступная функция для обновления UI доната
        function updateDonationUI() {
            const lang = translations[currentLanguage];
            const donateTitle = document.querySelector('.donate-title');
            if (donateTitle) donateTitle.textContent = lang.donateTitle;
            const donateText = document.querySelector('.donate-text');
            if (donateText) donateText.textContent = lang.donateText;
            const donateLinkButton = document.querySelector('.donate-link-button');
            if (donateLinkButton) donateLinkButton.textContent = lang.donateButton;
            const donateCloseButton = document.getElementById('donateCloseButton');
            if (donateCloseButton) donateCloseButton.textContent = lang.donateClose;
            const donateButtonEl = document.getElementById('donateButton');
            if (donateButtonEl) donateButtonEl.textContent = lang.supportButton;
        }

        // Обработчик движения мыши по canvas (для ПК)
        canvas.addEventListener('mousemove', (e) => {
            const rect = canvas.getBoundingClientRect();
            mouseX = e.clientX - rect.left;
            mouseY = e.clientY - rect.top;
        });

        // --- ДОБАВЛЯЕМ ТЕКСТУРУ ДЛЯ АПТЕЧКИ ---
        const healthPackImg = new Image();
        healthPackImg.src = 'health_pack.png'; // Положите health_pack.png рядом с этим HTML-файлом
        // --- ДОБАВЛЯЕМ ТЕКСТУРУ ДЛЯ ОБОЙМЫ ПАТРОНОВ ---
        const ammoPackImg = new Image();
        ammoPackImg.src = 'ammo_pack.png'; // Положите ammo_pack.png (72x45) рядом с этим HTML-файлом
    </script>
</body>
</html>
