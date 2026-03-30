# molly-h5<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>MOLLY 20周年：燃掉平庸</title>
    <style>
        :root {
            --bg-gray: #2c2c2c;
            --active-red: #ff4d4d;
            --active-purple: #9d4edd;
        }

        body, html {
            margin: 0; padding: 0; width: 100%; height: 100%;
            overflow: hidden; background-color: #1a1a1a;
            font-family: "PingFang SC", "Microsoft YaHei", sans-serif;
        }

        /* 场景容器 */
        #app { width: 100%; height: 100%; position: relative; }

        /* 背景弹幕层 (压抑感) */
        .barrage-layer {
            position: absolute; width: 100%; height: 100%;
            opacity: 0.3; pointer-events: none;
            transition: opacity 1s;
        }
        .text-item {
            position: absolute; color: #888; white-space: nowrap; font-size: 14px;
        }

        /* 核心 IP 形象区 */
        .character-container {
            position: absolute; left: 50%; top: 50%;
            transform: translate(-50%, -50%);
            text-align: center; z-index: 10;
        }

        .molly-img {
            width: 280px; filter: grayscale(100%);
            transition: transform 0.2s, filter 0.5s;
        }

        /* 进度条/光环 */
        .energy-circle {
            position: absolute; left: 50%; top: 50%;
            transform: translate(-50%, -50%);
            width: 320px; height: 320px;
            border: 4px solid rgba(255,255,255,0.1);
            border-radius: 50%;
        }

        /* 交互提示 */
        .hint-text {
            color: #aaa; margin-top: 50px; font-size: 14px;
            letter-spacing: 2px; transition: color 0.5s;
        }

        /* 爆发后的特效 */
        .burst-effect {
            position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            background: radial-gradient(circle, var(--active-red) 0%, transparent 70%);
            opacity: 0; pointer-events: none; z-index: 5;
        }

        /* 按钮长按态动画 */
        .pressing .molly-img { transform: translate(-50%, -50%) scale(0.95); }
        .active .molly-img { filter: grayscale(0%) drop-shadow(0 0 20px var(--active-red)); transform: scale(1.1); }
        .active .hint-text { color: var(--active-red); font-weight: bold; }

    </style>
</head>
<body>

<div id="app">
    <div class="barrage-layer" id="barrage"></div>

    <div class="burst-effect" id="burst"></div>

    <div class="character-container" id="mainTrigger">
        <div class="energy-circle"></div>
        <img src="https://via.placeholder.com/300x400/333/fff?text=Classic+Molly" alt="Molly" class="molly-img" id="mollyImg">
        <div class="hint-text" id="statusText">长按释放 20 年的炽热</div>
    </div>
</div>

<script>
    const barrageContainer = document.getElementById('barrage');
    const mollyImg = document.getElementById('mollyImg');
    const statusText = document.getElementById('statusText');
    const burstEffect = document.getElementById('burst');
    const words = ["方案再改一版", "成熟一点", "情绪稳定", "还没下班？", "合群一点", "20岁该有的样子", "KPI达成了吗"];

    // 1. 生成背景弹幕
    function createBarrage() {
        for(let i=0; i<30; i++) {
            const div = document.createElement('div');
            div.className = 'text-item';
            div.innerText = words[Math.floor(Math.random()*words.length)];
            div.style.top = Math.random() * 90 + '%';
            div.style.left = Math.random() * 80 + '%';
            barrageContainer.appendChild(div);
        }
    }
    createBarrage();

    // 2. 长按逻辑
    let timer = null;
    let progress = 0;
    const trigger = document.getElementById('mainTrigger');

    function startPress() {
        document.body.classList.add('pressing');
        timer = setInterval(() => {
            progress += 5;
            mollyImg.style.transform = `scale(${0.9 + (progress/500)})`;
            if (progress >= 100) {
                explode();
                clearInterval(timer);
            }
        }, 30);
    }

    function endPress() {
        if (progress < 100) {
            document.body.classList.remove('pressing');
            progress = 0;
            mollyImg.style.transform = `scale(1)`;
            clearInterval(timer);
        }
    }

    // 3. 爆发函数
    function explode() {
        document.body.classList.add('active');
        // 模拟替换为 Angry Molly
        mollyImg.src = "https://via.placeholder.com/300x400/ff4d4d/fff?text=Angry+Molly"; 
        
        statusText.innerText = "愤怒是炽热的生命力！";
        barrageContainer.style.opacity = '0';
        burstEffect.style.opacity = '1';
        
        // 震动反馈 (部分手机支持)
        if (window.navigator.vibrate) window.navigator.vibrate([100, 50, 200]);

        // 这里可以跳转到下一页或展示领券弹窗
        setTimeout(() => {
            alert("MOLLY 20周年：感谢你保持真实！点击确定领取限定福利。");
        }, 1000);
    }

    // 绑定事件 (兼容移动端)
    trigger.addEventListener('touchstart', (e) => { e.preventDefault(); startPress(); });
    trigger.addEventListener('touchend', endPress);
    // 桌面端调试用
    trigger.addEventListener('mousedown', startPress);
    trigger.addEventListener('mouseup', endPress);

</script>
</body>
</html>
