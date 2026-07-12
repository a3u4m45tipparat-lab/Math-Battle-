# Math-Battle-
ใช้ AI ในการสร้างสรรค์ เพื่อนำมาใช้ทางการศึกษาเท่านั้น

<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Math Battle : ศึกนักคิดพิชิตเหรียญทอง</title>
    <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@400;600;800;900&display=swap" rel="stylesheet">
    <style>
        /* ================= CSS VARIABLES & RESET ================= */
        :root {
            --primary: #FF9800;
            --primary-glow: rgba(255, 152, 0, 0.6);
            --team-a: #00BFFF;
            --team-a-glow: rgba(0, 191, 255, 0.6);
            --team-b: #FF4500;
            --team-b-glow: rgba(255, 69, 0, 0.6);
            --bg-color: #87CEEB;
            --text-dark: #333;
            --card-bg: rgba(255, 255, 255, 0.95);
        }
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Kanit', sans-serif;
            user-select: none;
        }
        body {
            background-color: var(--bg-color);
            overflow: hidden;
            width: 100vw;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            color: var(--text-dark);
        }

        /* ================= BACKGROUND ANIMATIONS ================= */
        .bg-layer {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            z-index: -2;
            background: linear-gradient(180deg, #4facfe 0%, #00f2fe 100%);
        }
        .cloud {
            position: absolute;
            background: white;
            border-radius: 50px;
            opacity: 0.8;
            animation: moveClouds linear infinite;
        }
        .cloud::before, .cloud::after {
            content: '';
            position: absolute;
            background: white;
            border-radius: 50%;
        }
        .c1 { width: 150px; height: 50px; top: 10%; left: -200px; animation-duration: 35s; }
        .c1::before { width: 70px; height: 70px; top: -30px; left: 20px; }
        .c1::after { width: 90px; height: 90px; top: -40px; left: 60px; }
        .c2 { width: 200px; height: 60px; top: 40%; left: -300px; animation-duration: 45s; animation-delay: 10s;}
        .c2::before { width: 80px; height: 80px; top: -40px; left: 30px; }
        .c2::after { width: 100px; height: 100px; top: -50px; left: 80px; }
        .star {
            position: absolute;
            color: white;
            font-size: 24px;
            animation: twinkle 2s infinite alternate;
        }
        @keyframes moveClouds {
            0% { transform: translateX(-200px); }
            100% { transform: translateX(110vw); }
        }
        @keyframes twinkle {
            0% { opacity: 0.2; transform: scale(0.8); }
            100% { opacity: 1; transform: scale(1.2); text-shadow: 0 0 10px white; }
        }

        /* ================= LAYOUT & PAGES ================= */
        #app {
            width: 100%;
            height: 100%;
            max-width: 1920px;
            max-height: 1080px;
            position: relative;
            z-index: 1;
            display: flex;
            flex-direction: column;
        }
        .page {
            display: none;
            width: 100%;
            height: 100%;
            padding: 20px;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            animation: fadeIn 0.5s ease;
            position: relative;
        }
        .page.active { display: flex; }
        
        @keyframes fadeIn { from { opacity: 0; transform: scale(0.95); } to { opacity: 1; transform: scale(1); } }
        
        /* ================= UI COMPONENTS ================= */
        .btn {
            padding: 15px 40px;
            font-size: 24px;
            font-weight: 600;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.2s;
            box-shadow: 0 10px 20px rgba(0,0,0,0.2);
            position: relative;
            overflow: hidden;
            background: linear-gradient(45deg, #FF9800, #FFC107);
            color: white;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.3);
            margin: 10px;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }
        .btn:hover {
            transform: scale(1.05) translateY(-5px);
            box-shadow: 0 15px 25px var(--primary-glow);
        }
        .btn:active { transform: scale(0.95); }
        .btn:disabled { filter: grayscale(1); opacity: 0.5; cursor: not-allowed; transform: none; box-shadow: none; }
        
        .btn-back-float {
            position: absolute;
            top: 20px;
            left: 20px;
            z-index: 110;
            background: #607D8B;
            padding: 10px 25px;
            font-size: 20px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.2);
            margin: 0;
        }
        .btn-back-float:hover { background: #455A64; }

        .ripple {
            position: absolute;
            background: rgba(255, 255, 255, 0.5);
            border-radius: 50%;
            transform: scale(0);
            animation: ripple-anim 0.6s linear;
            pointer-events: none;
        }
        @keyframes ripple-anim { to { transform: scale(4); opacity: 0; } }

        .controls {
            position: absolute;
            top: 20px; right: 20px;
            display: flex; gap: 10px;
            z-index: 100;
        }
        .btn-icon {
            width: 50px; height: 50px;
            border-radius: 50%;
            font-size: 20px;
            padding: 0;
            background: white;
            color: var(--primary);
        }

        .card {
            background: var(--card-bg);
            border-radius: 30px;
            padding: 40px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.2);
            text-align: center;
            max-width: 90%;
            max-height: 90%;
            overflow-y: auto;
            position: relative;
        }
        .card::-webkit-scrollbar { width: 10px; }
        .card::-webkit-scrollbar-thumb { background: var(--primary); border-radius: 5px; }

        /* Typography */
        h1.title {
            font-size: 110px;
            font-weight: 900;
            color: #fff;
            text-transform: uppercase;
            letter-spacing: 5px;
            text-shadow: 
                4px 4px 0px #FF4500,
                -4px -4px 0px #FF4500,
                4px -4px 0px #FF4500,
                -4px 4px 0px #FF4500,
                0px 10px 0px #C62828,
                0px 20px 30px rgba(0,0,0,0.6),
                0px 0px 50px #FFEB3B;
            margin-bottom: 30px;
            text-align: center;
            line-height: 1;
            animation: titleFloat 2s ease-in-out infinite alternate;
        }
        h1.title span { 
            display: block; 
            font-size: 50px; 
            color: #FFFDE7;
            letter-spacing: 2px;
            text-shadow: 2px 2px 0px #FF9800, 0 0 20px #FF9800;
            margin-top: 15px;
        }
        @keyframes titleFloat {
            0% { transform: translateY(0) scale(1); }
            100% { transform: translateY(-15px) scale(1.02); }
        }
        
        h2 { font-size: 40px; color: var(--primary); margin-bottom: 20px; }
        p { font-size: 24px; line-height: 1.6; }

        /* ================= GAME SCREEN ================= */
        .game-layout {
            display: grid;
            grid-template-columns: 240px 1fr 240px; 
            grid-template-rows: 90px 1fr 180px;
            width: 100%; height: 100%;
            gap: 15px;
        }
        
        .top-bar {
            grid-column: 1 / -1;
            grid-row: 1 / 2;
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: rgba(255,255,255,0.9);
            border-radius: 20px;
            padding: 0 30px;
            box-shadow: 0 10px 20px rgba(0,0,0,0.1);
        }
        
        #panel-a { grid-column: 1 / 2; grid-row: 2 / 4; }
        #question-panel { grid-column: 2 / 3; grid-row: 2 / 3; }
        #panel-b { grid-column: 3 / 4; grid-row: 2 / 4; }
        .options-grid { grid-column: 2 / 3; grid-row: 3 / 4; }

        .timer-box {
            font-size: 50px; font-weight: 800; color: #F44336;
            text-shadow: 2px 2px 5px rgba(0,0,0,0.2);
        }

        /* Side Panels (Power Bars) */
        .side-panel {
            display: flex; flex-direction: column; align-items: center;
            background: rgba(255,255,255,0.95);
            border-radius: 20px; padding: 20px 15px;
            box-shadow: 0 10px 20px rgba(0,0,0,0.15);
        }

        /* --- กล่องตั้งชื่อทีม --- */
        .team-name-container {
            display: flex;
            align-items: center;
            justify-content: center;
            background: rgba(0, 0, 0, 0.05);
            padding: 5px 10px;
            border-radius: 12px;
            margin-bottom: 5px;
            transition: background 0.3s;
            width: 100%;
        }
        .team-name-container:hover {
            background: rgba(0, 0, 0, 0.1);
        }
        .team-name-input {
            font-size: 22px; font-weight: bold; text-align: center;
            border: none; background: transparent; width: 100%;
            font-family: 'Kanit'; color: #333;
            user-select: text; 
            -webkit-user-select: text;
            cursor: text;
        }
        .team-name-input:focus {
            outline: none;
            border-bottom: 2px solid var(--primary);
        }
        .edit-icon {
            font-size: 16px;
            margin-left: 5px;
            color: #888;
        }

        .team-score-label {
            font-size: 18px; color: #666; font-weight: 600; margin-top: 5px;
        }
        .team-score { 
            font-size: 36px; font-weight: 900; line-height: 1; margin-bottom: 15px;
        }
        .power-bar-container {
            width: 60px; flex-grow: 1;
            background: repeating-linear-gradient(
                0deg, 
                #e0e0e0, 
                #e0e0e0 18%, 
                #bdbdbd 18%, 
                #bdbdbd 20%
            );
            border: 4px solid #fff; border-radius: 15px;
            position: relative; overflow: hidden; margin-bottom: 15px;
            box-shadow: 0 10px 20px rgba(0,0,0,0.2), inset 0 5px 15px rgba(0,0,0,0.1);
        }
        .power-bar {
            position: absolute; bottom: 0; left: 0; width: 100%; height: 0%;
            transition: height 0.6s cubic-bezier(0.4, 0, 0.2, 1);
            border-top: 5px solid rgba(255,255,255,0.9);
        }
        #bar-a { background: linear-gradient(0deg, #00BFFF, #00e5ff); box-shadow: 0 0 15px var(--team-a-glow); }
        #bar-b { background: linear-gradient(0deg, #FF4500, #ff9100); box-shadow: 0 0 15px var(--team-b-glow); }
        
        .team-btn { 
            width: 100%; font-size: 20px; padding: 10px 0; margin-top: auto; 
        }

        /* Center (Question) */
        .center-panel {
            display: flex; flex-direction: column;
            justify-content: center; align-items: center;
            background: rgba(255,255,255,0.95);
            border-radius: 30px; padding: 25px;
            box-shadow: 0 15px 30px rgba(0,0,0,0.15);
            position: relative;
        }
        .q-number { position: absolute; top: 15px; left: 25px; font-size: 24px; font-weight: bold; color: #888; }
        
        .question-text {
            font-size: 65px; font-weight: bold; color: #333;
            text-align: center; display: flex; flex-direction: column; align-items: center; gap: 15px;
        }
        
        .fraction {
            display: inline-flex; flex-direction: column; align-items: center;
            vertical-align: middle; margin: 0 10px; line-height: 1.2;
        }
        .fraction .num { border-bottom: 5px solid #333; padding: 0 10px; }
        .fraction .den { padding: 0 10px; }

        .fraction-visual-container {
            display: flex; gap: 15px; justify-content: center; flex-wrap: wrap; margin-top: 15px;
        }
        .fraction-visual-grid {
            display: grid; gap: 2px; background: #bbb; border: 3px solid #fff;
            box-shadow: 0 5px 10px rgba(0,0,0,0.15); padding: 3px; border-radius: 6px;
        }
        .fraction-cell {
            background: white; border-radius: 2px; width: 100%; height: 100%;
        }
        .fraction-cell.filled { background: #FF9800; box-shadow: inset 0 0 5px rgba(255,152,0,0.8); }
        
        /* Bottom (Options) */
        .options-grid {
            display: grid; grid-template-columns: 1fr 1fr; grid-template-rows: 1fr 1fr;
            gap: 15px;
        }
        .opt-btn {
            font-size: 38px; font-weight: bold; background: white; color: var(--primary);
            border: 4px solid var(--primary); border-radius: 15px;
            box-shadow: 0 6px 0 var(--primary); cursor: pointer; transition: all 0.1s;
        }
        .opt-btn:disabled { border-color: #aaa; color: #aaa; box-shadow: 0 6px 0 #aaa; cursor:not-allowed;}
        .opt-btn:not(:disabled):hover { transform: translateY(-3px); box-shadow: 0 9px 0 var(--primary); background: #FFF3E0; }
        .opt-btn:not(:disabled):active { transform: translateY(6px); box-shadow: 0 0 0 var(--primary); }

        /* ================= KNOWLEDGE & RULES ================= */
        .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 30px; text-align: left;}
        .info-box { background: #f0f8ff; padding: 20px; border-radius: 20px; border-left: 10px solid var(--primary); margin-bottom: 20px;}
        .icon-large { font-size: 50px; }

        /* ================= EFFECTS & OVERLAYS ================= */
        #fireworksCanvas { position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; pointer-events: none; z-index: 999; }
        .flash-red { animation: flashRed 0.5s ease-out; }
        @keyframes flashRed { 0% { box-shadow: inset 0 0 100px 50px rgba(255,0,0,0.8); } 100% { box-shadow: inset 0 0 0 0 rgba(255,0,0,0); } }
        .shake { animation: shake 0.5s; }
        @keyframes shake { 0%, 100% { transform: translateX(0); } 25% { transform: translateX(-15px); } 50% { transform: translateX(15px); } 75% { transform: translateX(-10px); } }
        
        .float-msg {
            position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%) scale(0);
            font-size: 70px; font-weight: 900; color: #4CAF50;
            text-shadow: 3px 3px 0 #fff, -3px -3px 0 #fff, 0 0 20px #4CAF50;
            z-index: 50; animation: popMsg 1.5s forwards;
        }
        @keyframes popMsg { 0% { transform: translate(-50%, -50%) scale(0); } 20% { transform: translate(-50%, -50%) scale(1.2); } 80% { transform: translate(-50%, -50%) scale(1); opacity: 1; } 100% { transform: translate(-50%, -50%) scale(1.5); opacity: 0; } }

        .active-team { border: 5px solid #FFEB3B; animation: pulseBorder 1s infinite alternate; }
        @keyframes pulseBorder { from { box-shadow: 0 0 10px #FFEB3B; transform: scale(1.02); } to { box-shadow: 0 0 30px #FFEB3B; transform: scale(1.04); } }

    </style>
</head>
<body>

    <!-- Canvas สำหรับพลุ -->
    <canvas id="fireworksCanvas"></canvas>

    <!-- พื้นหลังแบบ Animation -->
    <div class="bg-layer">
        <div class="cloud c1"></div>
        <div class="cloud c2"></div>
        <div class="star" style="top: 20%; left: 10%;">⭐</div>
        <div class="star" style="top: 15%; left: 80%; animation-delay: 0.5s;">⭐</div>
        <div class="star" style="top: 70%; left: 85%; animation-delay: 1s;">⭐</div>
        <div class="star" style="top: 80%; left: 15%; animation-delay: 1.5s;">⭐</div>
    </div>

    <!-- ปุ่มควบคุมมุมขวาบน -->
    <div class="controls">
        <button class="btn btn-icon" id="btn-sound" onclick="toggleSound()">🔊</button>
        <button class="btn btn-icon" id="btn-fullscreen" onclick="toggleFullScreen()">⛶</button>
    </div>

    <div id="app">
        <!-- ================= 1. หน้าแรก (Home) ================= -->
        <div id="home" class="page active">
            <h1 class="title">Math Battle<span>ศึกนักคิดพิชิตเหรียญทอง</span></h1>
            <div style="display: flex; flex-direction: column; gap: 15px; margin-top: 30px;">
                <!-- เปลี่ยนไปหน้า Setup แทนการเข้าเกมทันที -->
                <button class="btn" style="padding: 20px 60px; font-size: 36px;" onclick="showPage('setup');">🎮 เริ่มเกม</button>
                <button class="btn" style="background: linear-gradient(45deg, #2196F3, #00BCD4);" onclick="showPage('rules')">📖 วิธีเล่น</button>
                <button class="btn" style="background: linear-gradient(45deg, #4CAF50, #8BC34A);" onclick="showPage('knowledge')">📚 คลังความรู้</button>
            </div>
        </div>

        <!-- ================= 1.5. หน้าตั้งชื่อทีม (Setup) ================= -->
        <div id="setup" class="page">
            <button class="btn btn-back-float" onclick="showPage('home')">⬅️ กลับหน้าแรก</button>
            <div class="card" style="width: 700px;">
                <h2 style="font-size: 50px;">📝 ตั้งชื่อทีม</h2>
                <div style="display: flex; flex-direction: column; gap: 20px; margin: 40px 0;">
                    <div style="display: flex; align-items: center; justify-content: center; gap: 20px;">
                        <span style="font-size: 50px;">🦊</span>
                        <input type="text" id="setup-team-a" class="team-name-input" style="border-bottom: 3px solid var(--team-a); width: 300px; text-align: center; padding: 10px; font-size: 30px;" value="ทีม A" maxlength="12">
                    </div>
                    <div style="display: flex; align-items: center; justify-content: center; gap: 20px;">
                        <span style="font-size: 50px;">🐳</span>
                        <input type="text" id="setup-team-b" class="team-name-input" style="border-bottom: 3px solid var(--team-b); width: 300px; text-align: center; padding: 10px; font-size: 30px;" value="ทีม B" maxlength="12">
                    </div>
                </div>
                <button class="btn" style="background: #4CAF50; padding: 15px 50px; font-size: 32px;" onclick="startGameFromSetup()">🎮 เริ่มแข่งขัน</button>
            </div>
        </div>

        <!-- ================= 2. หน้าวิธีเล่น (Rules) ================= -->
        <div id="rules" class="page">
            <button class="btn btn-back-float" onclick="showPage('home')">⬅️ กลับหน้าแรก</button>
            <div class="card" style="width: 1000px;">
                <h2>📖 กติกาการแข่งขัน</h2>
                <div class="grid-2">
                    <div>
                        <div class="info-box">
                            <span class="icon-large">🧑‍🏫</span>
                            <p>1. แบ่งผู้เล่นเป็น 2 ทีม กดเลือกทีมก่อนตอบ ทีมที่ไม่ได้เลือกจะไม่สามารถกดปุ่มได้</p>
                        </div>
                        <div class="info-box">
                            <span class="icon-large">⏱️</span>
                            <!-- แก้ไขเป็น 45 วินาที -->
                            <p>2. มีเวลา <b>45 วินาที</b> ต่อข้อ หากหมดเวลาจะถือว่าตอบผิดทันที</p>
                        </div>
                    </div>
                    <div>
                        <div class="info-box">
                            <span class="icon-large">🪙</span>
                            <p>3. ตอบถูกรับ <b>20 คะแนน</b> พร้อมเหรียญทองสะสม! หากตอบผิด สิทธิ์การตอบข้อนั้นจะเปลี่ยนไปเป็นของอีกทีมทันที </p>
                        </div>
                        <div class="info-box">
                            <span class="icon-large">🏆</span>
                            <p>4. หากตอบผิดทั้งสองทีม จะทำการเฉลยข้อนั้นและไปยังข้อถัดไป ทีมที่หลอดคะแนนสะสมมากที่สุดตอนจบเกมเป็นผู้ชนะ!</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- ================= 3. หน้าคลังความรู้ (Knowledge Base) ================= -->
        <div id="knowledge" class="page">
            <button class="btn btn-back-float" onclick="showPage('home')">⬅️ กลับหน้าแรก</button>
            <div class="card" style="width: 1300px; text-align: left; padding-top: 60px;">
                <h2 style="text-align: center;">📚 คลังความรู้</h2>
                <div class="grid-2">
                    <div style="border-right: 2px dashed #ccc; padding-right: 20px;">
                        <h3 style="font-size: 32px; color: #E91E63;">🍕 บทที่ 1: การเขียนเศษส่วนให้อยู่ในรูปทศนิยม</h3>
                        <p>ตัวส่วนเป็น 10, 100, 1000 สามารถเขียนเป็นทศนิยมได้ทันที (1, 2, 3 ตำแหน่ง ตามลำดับ)</p>
                        
                        <div class="info-box" style="display: flex; align-items: center; justify-content: space-between;">
                            <div style="display: flex; align-items: center; gap: 15px;">
                                <div class="fraction" style="font-size: 30px;"><span class="num">8</span><span class="den">100</span></div>
                                <span style="font-size: 30px;">= 0.08</span>
                            </div>
                            <div id="demo-visual-1"></div> 
                        </div>

                        <p><b>กรณีตัวส่วนไม่ใช่ 10 100 หรือ 1000:</b> ให้หาตัวเลขมาคูณทั้งเศษและส่วนที่ทำให้เป็น 10 100 หรือ 1000 ก่อน</p>
                        <div class="info-box" style="display: flex; align-items: center; justify-content: space-between;">
                            <div style="display: flex; align-items: center; gap: 15px;">
                                <div class="fraction" style="font-size: 30px;">
                                    <span class="num">3 <span style="color:red; font-size: 18px;">× 25</span></span>
                                    <span class="den">4 <span style="color:red; font-size: 18px;">× 25</span></span>
                                </div>
                                <span style="font-size: 30px;">=</span>
                                <div class="fraction" style="font-size: 30px;"><span class="num">75</span><span class="den">100</span></div>
                            </div>
                            <div id="demo-visual-2"></div>
                        </div>
                    </div>
                    
                    <div style="padding-left: 20px;">
                        <h3 style="font-size: 32px; color: #9C27B0;">🧮 บทที่ 2: การหารทศนิยม 1 ตำแหน่ง</h3>
                        <p><b>หลักการ:</b> ให้แปลงทศนิยมเป็นเศษส่วนก่อน จากนั้น <b>เปลี่ยนหารเป็นคูณ แล้วกลับเศษเป็นส่วน</b> ของตัวหาร</p>
                        
                        <div class="info-box">
                            <p style="font-size: 30px; text-align: center; margin-bottom: 15px;">2.4 ÷ 0.6 = ?</p>
                            
                            <div style="display: flex; justify-content: center; align-items: center; font-size: 26px; gap: 10px; margin-bottom: 15px;">
                                <div class="fraction"><span class="num">24</span><span class="den">10</span></div>
                                <span>÷</span>
                                <div class="fraction"><span class="num">6</span><span class="den">10</span></div>
                                
                                <span style="margin: 0 15px; color: #888;">➡️</span>
                                
                                <div class="fraction"><span class="num">24</span><span class="den">10</span></div>
                                <span style="color: #E91E63; font-weight: bold;">×</span>
                                <div class="fraction"><span class="num" style="color: #E91E63;">10</span><span class="den" style="color: #E91E63;">6</span></div>
                            </div>
                            
                            <p style="font-size: 38px; text-align: center; color: #4CAF50; font-weight: bold; margin-top: 10px;">= 4</p>
                        </div>

                        <div class="info-box">
                            <p style="font-size: 30px; text-align: center; margin-bottom: 15px;">5.6 ÷ 0.8 = ?</p>
                            <div style="display: flex; justify-content: center; align-items: center; font-size: 24px; gap: 8px;">
                                <div class="fraction"><span class="num">56</span><span class="den">10</span></div>
                                <span style="color: #E91E63; font-weight: bold;">×</span>
                                <div class="fraction"><span class="num" style="color: #E91E63;">10</span><span class="den" style="color: #E91E63;">8</span></div>
                                <span style="margin-left: 10px;">= <span style="color: #4CAF50; font-weight: bold; font-size: 36px;">7</span></span>
                            </div>
                        </div>
                    </div>
                </div>
                <div style="text-align: center; margin-top: 30px;">
                    <!-- เปลี่ยนไปหน้า Setup เช่นกัน -->
                    <button class="btn" style="background: #4CAF50; padding: 20px 50px; font-size: 30px;" onclick="showPage('setup');">🎮 เริ่มแข่งขันเลย!</button>
                </div>
            </div>
        </div>

        <!-- ================= 4. หน้าเกม (Game Screen) ================= -->
        <div id="game" class="page" style="padding: 20px;">
            <div class="game-layout">
                <!-- ขอบบน -->
                <div class="top-bar">
                    <button class="btn" style="padding: 8px 15px; font-size: 18px; background: #E91E63;" onclick="showPage('home'); stopBGM();">🏠 ออก</button>
                    <!-- แก้ไขค่าเริ่มต้นเป็น 45 -->
                    <div class="timer-box">⏱️ <span id="time-display">45</span>s</div>
                    <button class="btn" id="btn-pause" style="padding: 8px 15px; font-size: 18px; background: #607D8B;" onclick="togglePause()">⏸️ พัก</button>
                </div>

                <!-- ฝั่งซ้าย (ทีม A) -->
                <div class="side-panel" id="panel-a">
                    <div style="font-size: 45px; margin-bottom: 5px;">🦊</div>
                    <div class="team-name-container" title="คลิกเพื่อเปลี่ยนชื่อทีม">
                        <input type="text" id="game-team-a" class="team-name-input" value="ทีม A" maxlength="12">
                        <span class="edit-icon">✏️</span>
                    </div>
                    <div class="team-score-label">คะแนนสะสม</div>
                    <div class="team-score" id="score-a" style="color: var(--team-a);">0</div>
                    <div class="power-bar-container"><div class="power-bar" id="bar-a"></div></div>
                    <button class="btn team-btn" id="btn-team-a" style="background: var(--team-a);" onclick="selectTeam('A')">เลือกให้ตอบ 👆</button>
                </div>

                <!-- ตรงกลาง (โจทย์) -->
                <div class="center-panel" id="question-panel">
                    <div class="q-number" id="q-num-display">ข้อ 1/10</div>
                    <div class="question-text" id="question-text">
                        <!-- โครงสร้างโจทย์และภาพแบ่งส่วนจะถูกสร้างด้วย JS -->
                    </div>
                </div>

                <!-- ฝั่งขวา (ทีม B) -->
                <div class="side-panel" id="panel-b">
                    <div style="font-size: 45px; margin-bottom: 5px;">🐳</div>
                    <div class="team-name-container" title="คลิกเพื่อเปลี่ยนชื่อทีม">
                        <input type="text" id="game-team-b" class="team-name-input" value="ทีม B" maxlength="12">
                        <span class="edit-icon">✏️</span>
                    </div>
                    <div class="team-score-label">คะแนนสะสม</div>
                    <div class="team-score" id="score-b" style="color: var(--team-b);">0</div>
                    <div class="power-bar-container"><div class="power-bar" id="bar-b"></div></div>
                    <button class="btn team-btn" id="btn-team-b" style="background: var(--team-b);" onclick="selectTeam('B')">เลือกให้ตอบ 👆</button>
                </div>

                <!-- ด้านล่าง (ตัวเลือก) -->
                <div class="options-grid">
                    <button class="opt-btn" onclick="checkAnswer(0)" id="opt-0">A</button>
                    <button class="opt-btn" onclick="checkAnswer(1)" id="opt-1">B</button>
                    <button class="opt-btn" onclick="checkAnswer(2)" id="opt-2">C</button>
                    <button class="opt-btn" onclick="checkAnswer(3)" id="opt-3">D</button>
                </div>
            </div>
        </div>

        <!-- ================= 5. หน้าจบเกม (Result) ================= -->
        <div id="result" class="page">
            <div class="card" style="width: 800px; background: rgba(255,255,255,0.95);">
                <div style="font-size: 130px; text-shadow: 0 0 50px gold;">🏆</div>
                <h1 style="font-size: 70px; color: #E91E63; margin-bottom: 10px;" id="winner-text">ทีม A ชนะ!</h1>
                <h2 style="font-size: 35px; color: #333; margin-bottom: 30px;">สุดยอดนักคิดพิชิตเหรียญทอง 🪙</h2>
                
                <div style="display: flex; justify-content: space-around; font-size: 35px; margin-bottom: 40px; font-weight: bold;">
                    <div style="color: var(--team-a);">🦊 <span id="res-name-a">ทีม A</span><br><span id="final-score-a" style="font-size: 55px;">0</span> 🪙</div>
                    <div style="color: var(--team-b);">🐳 <span id="res-name-b">ทีม B</span><br><span id="final-score-b" style="font-size: 55px;">0</span> 🪙</div>
                </div>

                <button class="btn" onclick="showPage('home'); stopBGM();">🏠 กลับหน้าแรก</button>
                <button class="btn" style="background: #4CAF50;" onclick="showPage('setup');">🔄 เล่นใหม่อีกครั้ง</button>
            </div>
        </div>
    </div>

    <!-- Script ควบคุมระบบทั้งหมด -->
    <script>
        /* ================= ระบบสร้างภาพตารางแบ่งส่วน (Fraction Visualizer) ================= */
        function getVisualHTML(v) {
            if (!v) return '';
            const totalCells = v.cols * v.rows;
            const gridsCount = Math.ceil(v.filled / totalCells) || 1;
            let html = '<div class="fraction-visual-container">';
            let filledCount = v.filled;
            
            let size = v.cols >= 10 ? '10px' : (v.cols === 5 ? '20px' : '30px');

            for (let g = 0; g < gridsCount; g++) {
                let cellsHTML = '';
                for(let i=0; i < totalCells; i++) {
                    cellsHTML += `<div class="fraction-cell ${filledCount > 0 ? 'filled' : ''}"></div>`;
                    filledCount--;
                }
                html += `<div class="fraction-visual-grid" style="grid-template-columns: repeat(${v.cols}, ${size}); grid-auto-rows: ${size};">
                            ${cellsHTML}
                         </div>`;
            }
            html += '</div>';
            return html;
        }

        document.addEventListener('DOMContentLoaded', () => {
            document.getElementById('demo-visual-1').innerHTML = getVisualHTML({ cols: 10, rows: 10, filled: 8 }); 
            document.getElementById('demo-visual-2').innerHTML = getVisualHTML({ cols: 4, rows: 1, filled: 3 });  
        });

        /* ================= ข้อมูลโจทย์ ================= */
        const originalQuestions = [
            { type: 'fraction', num: 8, den: 100, ans: '0.08', options: ['0.08', '0.8', '0.008', '8'], visual: {cols: 10, rows: 10, filled: 8} },
            { type: 'fraction', num: 154, den: 100, ans: '1.54', options: ['1.45', '15.4', '1.54', '0.154'], visual: {cols: 10, rows: 10, filled: 154} },
            { type: 'fraction', num: 4, den: 25, ans: '0.16', options: ['0.4', '0.25', '0.16', '1.6'], visual: {cols: 5, rows: 5, filled: 4} },
            { type: 'fraction', num: 7, den: 20, ans: '0.35', options: ['0.7', '0.35', '3.5', '0.53'], visual: {cols: 5, rows: 4, filled: 7} },
            { type: 'fraction', num: 3, den: 4, ans: '0.75', options: ['0.75', '0.34', '1.33', '7.5'], visual: {cols: 4, rows: 1, filled: 3} },
            
            { type: 'text', q: '2.4 ÷ 0.6 = ?', ans: '4', options: ['6', '4', '3', '2'] },
            { type: 'text', q: '5.6 ÷ 0.8 = ?', ans: '7', options: ['8', '6', '7', '5'] },
            { type: 'text', q: '3.6 ÷ 0.9 = ?', ans: '4', options: ['3', '4', '5', '6'] },
            { type: 'text', q: '4.5 ÷ 0.9 = ?', ans: '5', options: ['5', '0.5', '50', '4'] }, 
            { type: 'text', q: '7.2 ÷ 0.8 = ?', ans: '9', options: ['8', '9', '7', '90'] }
        ];

        let questions = [];
        let currentQIndex = 0;
        let scoreA = 0, scoreB = 0;
        let activeTeam = null; 
        let chances = 2; 
        
        let timer;
        let timeLeft = 45; // ปรับเวลาเป็น 45 วินาที
        let isPaused = false;
        let soundEnabled = true;

        /* ================= ระบบเสียง (Web Audio API) ================= */
        const AudioContext = window.AudioContext || window.webkitAudioContext;
        let audioCtx;
        function initAudio() { if(!audioCtx) audioCtx = new AudioContext(); }

        function playSynth(type) {
            if (!soundEnabled) return;
            initAudio();
            const osc = audioCtx.createOscillator();
            const gainNode = audioCtx.createGain();
            osc.connect(gainNode);
            gainNode.connect(audioCtx.destination);
            const now = audioCtx.currentTime;
            
            if (type === 'click') {
                osc.type = 'sine'; osc.frequency.setValueAtTime(600, now);
                gainNode.gain.setValueAtTime(0.3, now); gainNode.gain.exponentialRampToValueAtTime(0.01, now + 0.1);
                osc.start(now); osc.stop(now + 0.1);
            } else if (type === 'correct') {
                osc.type = 'triangle'; 
                osc.frequency.setValueAtTime(523.25, now); osc.frequency.setValueAtTime(659.25, now + 0.1); osc.frequency.setValueAtTime(783.99, now + 0.2);
                gainNode.gain.setValueAtTime(0.3, now); gainNode.gain.linearRampToValueAtTime(0, now + 0.5);
                osc.start(now); osc.stop(now + 0.5);
            } else if (type === 'wrong') {
                osc.type = 'sawtooth'; osc.frequency.setValueAtTime(150, now); osc.frequency.linearRampToValueAtTime(100, now + 0.3);
                gainNode.gain.setValueAtTime(0.3, now); gainNode.gain.linearRampToValueAtTime(0, now + 0.3);
                osc.start(now); osc.stop(now + 0.3);
            } else if (type === 'tick') {
                osc.type = 'square'; osc.frequency.setValueAtTime(800, now);
                gainNode.gain.setValueAtTime(0.1, now); gainNode.gain.exponentialRampToValueAtTime(0.01, now + 0.05);
                osc.start(now); osc.stop(now + 0.05);
            }
        }

        let bgmInterval;
        let noteIdx = 0;
        const bgmNotes = [220, 0, 220, 261.63, 0, 329.63, 0, 261.63]; 
        
        function startBGM() {
            if(!soundEnabled) return;
            initAudio();
            if(bgmInterval) clearInterval(bgmInterval);
            bgmInterval = setInterval(() => {
                if(!soundEnabled || isPaused) return;
                const freq = bgmNotes[noteIdx];
                noteIdx = (noteIdx + 1) % bgmNotes.length;
                if(freq === 0) return; 
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.type = 'square'; osc.frequency.value = freq;
                osc.connect(gain); gain.connect(audioCtx.destination);
                gain.gain.setValueAtTime(0.05, audioCtx.currentTime); 
                gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + 0.15);
                osc.start(); osc.stop(audioCtx.currentTime + 0.15);
            }, 200);
        }
        function stopBGM() { clearInterval(bgmInterval); bgmInterval = null; }

        /* ================= UI & ควบคุมปุ่มทั่วไป ================= */
        document.querySelectorAll('.btn, .opt-btn').forEach(btn => {
            btn.addEventListener('click', function(e) {
                playSynth('click');
                const x = e.clientX - e.target.getBoundingClientRect().left;
                const y = e.clientY - e.target.getBoundingClientRect().top;
                const ripple = document.createElement('span');
                ripple.classList.add('ripple');
                ripple.style.left = `${x}px`; ripple.style.top = `${y}px`;
                this.appendChild(ripple);
                setTimeout(() => ripple.remove(), 600);
            });
        });

        function toggleSound() {
            soundEnabled = !soundEnabled;
            document.getElementById('btn-sound').innerText = soundEnabled ? '🔊' : '🔇';
            if(soundEnabled && document.getElementById('game').classList.contains('active')) startBGM();
            else stopBGM();
        }

        function toggleFullScreen() {
            if (!document.fullscreenElement) document.documentElement.requestFullscreen();
            else if (document.exitFullscreen) document.exitFullscreen();
        }

        function showPage(pageId) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(pageId).classList.add('active');
        }

        /* ================= ระบบเกมลอจิก ================= */
        
        // ฟังก์ชันดึงชื่อทีมแล้วส่งไปหน้าเกม
        function startGameFromSetup() {
            const nameA = document.getElementById('setup-team-a').value || 'ทีม A';
            const nameB = document.getElementById('setup-team-b').value || 'ทีม B';
            
            document.getElementById('game-team-a').value = nameA;
            document.getElementById('game-team-b').value = nameB;

            showPage('game');
            initGame();
        }

        function initGame() {
            scoreA = 0; scoreB = 0; currentQIndex = 0;
            updateScores();
            questions = JSON.parse(JSON.stringify(originalQuestions)).sort(() => Math.random() - 0.5);
            questions.forEach(q => q.options.sort(() => Math.random() - 0.5));
            startBGM();
            loadQuestion();
        }

        function loadQuestion() {
            if (currentQIndex >= questions.length) return endGame();
            
            activeTeam = null; chances = 2; isPaused = false;
            document.getElementById('q-num-display').innerText = `ข้อ ${currentQIndex + 1}/${questions.length}`;
            
            document.getElementById('panel-a').classList.remove('active-team');
            document.getElementById('panel-b').classList.remove('active-team');
            document.getElementById('btn-team-a').disabled = false;
            document.getElementById('btn-team-b').disabled = false;

            const qData = questions[currentQIndex];
            const qContainer = document.getElementById('question-text');
            
            if (qData.type === 'fraction') {
                const visualHTML = qData.visual ? getVisualHTML(qData.visual) : '';
                qContainer.innerHTML = `
                    <div style="display:flex; align-items:center; justify-content:center;">
                        <div class="fraction">
                            <span class="num">${qData.num}</span>
                            <span class="den">${qData.den}</span>
                        </div>
                        <span style="margin-left: 10px;"> = ?</span>
                    </div>
                    ${visualHTML}
                `;
            } else {
                qContainer.innerHTML = `<span>${qData.q}</span>`;
            }

            for (let i = 0; i < 4; i++) {
                const btn = document.getElementById(`opt-${i}`);
                btn.innerText = qData.options[i];
                btn.disabled = true;
                btn.style.background = 'white';
                btn.style.color = 'var(--primary)';
                btn.style.border = '4px solid var(--primary)';
            }

            timeLeft = 45; // ปรับเวลาเริ่มต้นเป็น 45
            document.getElementById('time-display').innerText = timeLeft;
            clearInterval(timer);
            timer = setInterval(timerTick, 1000);
        }

        function selectTeam(team) {
            if(isPaused) return;
            activeTeam = team;
            document.getElementById('btn-team-a').disabled = true;
            document.getElementById('btn-team-b').disabled = true;
            
            document.getElementById('panel-a').classList.remove('active-team');
            document.getElementById('panel-b').classList.remove('active-team');
            document.getElementById(`panel-${team.toLowerCase()}`).classList.add('active-team');
            
            for (let i = 0; i < 4; i++) document.getElementById(`opt-${i}`).disabled = false;
        }

        function timerTick() {
            if (isPaused) return;
            timeLeft--;
            document.getElementById('time-display').innerText = timeLeft;
            if(timeLeft <= 5 && timeLeft > 0) playSynth('tick'); 
            if (timeLeft <= 0) { clearInterval(timer); handleWrongAnswer(null); }
        }

        function togglePause() {
            isPaused = !isPaused;
            document.getElementById('btn-pause').innerText = isPaused ? '▶️ เล่นต่อ' : '⏸️ พัก';
        }

        function checkAnswer(optIndex) {
            if (!activeTeam || isPaused) return;
            clearInterval(timer); 
            
            for (let i = 0; i < 4; i++) document.getElementById(`opt-${i}`).disabled = true;

            const selectedAns = questions[currentQIndex].options[optIndex];
            const correctAns = questions[currentQIndex].ans;
            const btn = document.getElementById(`opt-${optIndex}`);

            if (selectedAns === correctAns) {
                playSynth('correct');
                btn.style.background = '#4CAF50'; btn.style.color = 'white'; btn.style.borderColor = '#388E3C';
                if (activeTeam === 'A') scoreA += 20; else scoreB += 20;
                updateScores();
                showFloatingMessage(['เยี่ยมมาก!', 'สุดยอด!', 'เก่งมาก!', 'ยอดเยี่ยม!'][Math.floor(Math.random()*4)]);
                fireworks(); 
                setTimeout(() => { currentQIndex++; loadQuestion(); }, 2000);
            } else {
                playSynth('wrong');
                btn.style.background = '#F44336'; btn.style.color = 'white'; btn.style.borderColor = '#D32F2F';
                document.body.classList.add('flash-red');
                document.getElementById('question-panel').classList.add('shake');
                setTimeout(() => { document.body.classList.remove('flash-red'); document.getElementById('question-panel').classList.remove('shake'); }, 500);
                handleWrongAnswer(btn);
            }
        }

        function handleWrongAnswer(wrongBtn) {
            chances--;
            if (chances > 0) {
                activeTeam = activeTeam === 'A' ? 'B' : 'A';
                document.getElementById('panel-a').classList.remove('active-team');
                document.getElementById('panel-b').classList.remove('active-team');
                document.getElementById(`panel-${activeTeam.toLowerCase()}`).classList.add('active-team');
                
                for (let i = 0; i < 4; i++) {
                    const btn = document.getElementById(`opt-${i}`);
                    if(btn !== wrongBtn) btn.disabled = false;
                }
                timeLeft = 45; // รีเซ็ตเวลาสำหรับทีมที่ 2 เป็น 45
                document.getElementById('time-display').innerText = timeLeft;
                timer = setInterval(timerTick, 1000);
            } else {
                const correctAns = questions[currentQIndex].ans;
                for (let i = 0; i < 4; i++) {
                    const btn = document.getElementById(`opt-${i}`);
                    if (questions[currentQIndex].options[i] === correctAns) {
                        btn.style.background = '#4CAF50'; btn.style.color = 'white'; btn.style.border = '4px solid #fff';
                    }
                }
                setTimeout(() => { currentQIndex++; loadQuestion(); }, 3000); 
            }
        }

        function updateScores() {
            document.getElementById('score-a').innerText = scoreA;
            document.getElementById('score-b').innerText = scoreB;
            
            const maxScore = questions.length * 20 || 200;
            const pctA = Math.min(100, (scoreA / maxScore) * 100);
            const pctB = Math.min(100, (scoreB / maxScore) * 100);
            document.getElementById('bar-a').style.height = `${pctA}%`;
            document.getElementById('bar-b').style.height = `${pctB}%`;
        }

        function showFloatingMessage(text) {
            const msg = document.createElement('div');
            msg.className = 'float-msg'; msg.innerText = text;
            document.getElementById('question-panel').appendChild(msg);
            setTimeout(() => msg.remove(), 1500);
        }

        /* ================= จบเกม & เอฟเฟกต์เฮ ================= */
        function endGame() {
            stopBGM(); clearInterval(timer);
            showPage('result');
            
            document.getElementById('final-score-a').innerText = scoreA;
            document.getElementById('final-score-b').innerText = scoreB;
            
            const teamAName = document.getElementById('game-team-a').value || 'ทีม A';
            const teamBName = document.getElementById('game-team-b').value || 'ทีม B';
            document.getElementById('res-name-a').innerText = teamAName;
            document.getElementById('res-name-b').innerText = teamBName;
            
            const winText = document.getElementById('winner-text');
            if (scoreA > scoreB) winText.innerText = `🎉 ${teamAName} ชนะ!`;
            else if (scoreB > scoreA) winText.innerText = `🎉 ${teamBName} ชนะ!`;
            else winText.innerText = `🤝 เสมอกัน!`;

            if(soundEnabled) {
                initAudio();
                const bufferSize = audioCtx.sampleRate * 2.5; 
                const buffer = audioCtx.createBuffer(1, bufferSize, audioCtx.sampleRate);
                const data = buffer.getChannelData(0);
                for (let i = 0; i < bufferSize; i++) data[i] = Math.random() * 2 - 1; 
                
                const noise = audioCtx.createBufferSource(); noise.buffer = buffer;
                const filter = audioCtx.createBiquadFilter(); filter.type = 'lowpass'; filter.frequency.value = 1000;
                
                const gain = audioCtx.createGain();
                gain.gain.setValueAtTime(0, audioCtx.currentTime);
                gain.gain.linearRampToValueAtTime(0.6, audioCtx.currentTime + 0.5); 
                gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 2.5); 
                
                noise.connect(filter); filter.connect(gain); gain.connect(audioCtx.destination);
                noise.start();
                
                playSynth('correct'); setTimeout(()=>playSynth('correct'), 200); setTimeout(()=>playSynth('correct'), 400);
            }

            let fwInterval = setInterval(fireworks, 300);
            setTimeout(() => clearInterval(fwInterval), 3000);
        }

        /* ================= Canvas พลุ ================= */
        const canvas = document.getElementById('fireworksCanvas');
        const ctx = canvas.getContext('2d');
        let particles = [];
        
        function resizeCanvas() { canvas.width = window.innerWidth; canvas.height = window.innerHeight; }
        window.addEventListener('resize', resizeCanvas); resizeCanvas();

        function createParticles(x, y) {
            const colors = ['#FF1461', '#18FF92', '#5A87FF', '#FBF38C', '#FF9800'];
            for (let i = 0; i < 50; i++) {
                particles.push({
                    x: x, y: y,
                    vx: (Math.random() - 0.5) * 15, vy: (Math.random() - 0.5) * 15,
                    life: 1, color: colors[Math.floor(Math.random() * colors.length)],
                    size: Math.random() * 8 + 3
                });
            }
        }

        function fireworks() { createParticles(Math.random() * canvas.width, Math.random() * (canvas.height/2)); }

        function animateParticles() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            for (let i = particles.length - 1; i >= 0; i--) {
                let p = particles[i];
                p.x += p.vx; p.y += p.vy; p.vy += 0.2; p.life -= 0.02; p.size *= 0.95;
                ctx.globalAlpha = Math.max(0, p.life); ctx.fillStyle = p.color;
                ctx.beginPath(); ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2); ctx.fill();
                if (p.life <= 0) particles.splice(i, 1);
            }
            requestAnimationFrame(animateParticles);
        }
        animateParticles();
    </script>
</body>
</html>