<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>CAFF | FULL TRAINING - Dashboard con Google Drive</title>
    <!-- Font Awesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <!-- Google Fonts: Montserrat -->
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
    <!-- Google APIs para Drive -->
    <script type="text/javascript" src="https://apis.google.com/js/api.js"></script>
    <script type="text/javascript" src="https://accounts.google.com/gsi/client"></script>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            background: #050505;
            font-family: 'Montserrat', sans-serif;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 1.5rem;
        }
        body::before {
            content: "";
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 200 200" opacity="0.04"><path fill="none" stroke="%23ff3a00" stroke-width="0.5" d="M0 0 L200 200 M200 0 L0 200 M100 0 L100 200 M0 100 L200 100"/><circle cx="100" cy="100" r="40" stroke="%23ff6600" stroke-width="0.3" fill="none"/></svg>');
            background-repeat: repeat;
            pointer-events: none;
            z-index: 0;
        }
        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(25px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .app-container {
            max-width: 1400px;
            width: 100%;
            background: rgba(8, 8, 12, 0.85);
            backdrop-filter: blur(10px);
            border-radius: 1.5rem;
            border: 1px solid rgba(255, 58, 0, 0.35);
            box-shadow: 0 25px 45px rgba(0, 0, 0, 0.6);
            overflow: hidden;
            animation: fadeInUp 0.6s ease;
            position: relative;
            z-index: 1;
        }
        /* LOGIN */
        .login-screen { display: flex; justify-content: center; align-items: center; min-height: 550px; padding: 2rem; }
        .login-card {
            background: rgba(5, 5, 8, 0.9);
            backdrop-filter: blur(8px);
            border-radius: 1.5rem;
            padding: 2.5rem;
            width: 100%;
            max-width: 420px;
            text-align: center;
            border: 1px solid rgba(255, 58, 0, 0.5);
        }
        .brand-text { font-family: 'Montserrat', sans-serif; font-weight: 900; font-size: 3.2rem; letter-spacing: 6px; background: linear-gradient(135deg, #ffcc66, #ff6600, #ff3a00); -webkit-background-clip: text; background-clip: text; color: transparent; }
        .brand-sub { font-family: 'Montserrat', sans-serif; font-size: 0.7rem; letter-spacing: 4px; color: #ff884d; font-weight: 700; margin-top: -5px; margin-bottom: 1.5rem; }
        .input-group input { width: 100%; background: rgba(0, 0, 0, 0.6); border: 1px solid #4a2a1a; padding: 0.9rem; border-radius: 0.75rem; color: #ffccaa; font-family: 'Montserrat', sans-serif; margin-bottom: 1rem; }
        .login-btn { background: linear-gradient(95deg, #cc3300, #ff3a00); border: none; width: 100%; padding: 0.9rem; border-radius: 2rem; font-weight: 800; color: white; font-family: 'Montserrat', sans-serif; cursor: pointer; }
        .dashboard { display: none; animation: fadeInUp 0.5s ease; }
        /* HEADER */
        .dashboard-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 1rem 2rem;
            background: rgba(0, 0, 0, 0.5);
            border-bottom: 2px solid #ff3a00;
            flex-wrap: wrap;
        }
        /* NAVEGACIÓN */
        .nav-container { padding: 1rem 2rem 0 2rem; border-bottom: 1px solid rgba(255, 58, 0, 0.2); }
        .nav-tabs { display: flex; gap: 0.5rem; flex-wrap: wrap; }
        .nav-btn {
            background: transparent;
            border: none;
            padding: 0.8rem 1.6rem;
            font-family: 'Montserrat', sans-serif;
            font-weight: 700;
            font-size: 0.85rem;
            letter-spacing: 1px;
            color: #aa8866;
            cursor: pointer;
            border-radius: 2rem 2rem 0 0;
            display: flex;
            align-items: center;
            gap: 8px;
            transition: 0.2s;
        }
        .nav-btn.active { color: #ff3a00; background: rgba(255, 58, 0, 0.15); border-bottom: 2px solid #ff3a00; }
        .content-pane { padding: 2rem; }
        .pane { display: none; animation: fadeInUp 0.4s ease; }
        .pane.active-pane { display: block; }

        /* BLOQUE 1: RESUMEN */
        .stats-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(240px, 1fr)); gap: 1.5rem; margin-bottom: 2rem; }
        .stat-card { background: rgba(10, 10, 15, 0.8); border-radius: 1rem; padding: 1.4rem; border: 1px solid rgba(255, 58, 0, 0.3); transition: 0.2s; }
        .stat-card:hover { border-color: #ff3a00; transform: translateY(-4px); }
        .stat-title { color: #ff884d; font-size: 0.65rem; letter-spacing: 2px; font-weight: 700; }
        .stat-value { font-size: 2.2rem; font-weight: 800; color: #ffcc66; }
        .workout-item { background: rgba(0, 0, 0, 0.4); border-radius: 1rem; padding: 0.8rem 1rem; margin-bottom: 0.8rem; display: flex; justify-content: space-between; border-left: 3px solid #ff3a00; color: #f0dbb6; }
        .ring-bg { stroke: #2a221c; stroke-width: 12; }
        .ring-progress { stroke: url(#gradRing); stroke-width: 12; stroke-linecap: round; }

        /* BLOQUE 2: ENTRENOS */
        .exercise-table { width: 100%; border-collapse: collapse; }
        .exercise-table th, .exercise-table td { padding: 1rem; text-align: left; border-bottom: 1px solid rgba(255, 58, 0, 0.2); color: #e6d5b8; }
        .exercise-table th { color: #ff884d; font-weight: 800; }
        .badge-complete { background: rgba(255, 58, 0, 0.2); padding: 0.2rem 0.8rem; border-radius: 2rem; color: #ffaa66; }

        /* BLOQUE 3: CAFF LIVE */
        .meet-container { background: #060608; border-radius: 1rem; overflow: hidden; border: 1px solid #ff3a0055; }
        .video-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 1rem; padding: 1rem; background: #020205; }
        .video-card { background: #0f0f14; border-radius: 1rem; overflow: hidden; border: 1px solid #ff3a0033; aspect-ratio: 4/3; display: flex; align-items: center; justify-content: center; position: relative; }
        .video-card video { width: 100%; height: 100%; object-fit: cover; }
        .video-label { position: absolute; bottom: 10px; left: 10px; background: rgba(0,0,0,0.7); padding: 4px 10px; border-radius: 20px; font-size: 0.7rem; color: #ffaa66; font-weight: 600; }
        .meet-controls { display: flex; justify-content: center; gap: 1rem; padding: 1rem; background: #0a0a0f; border-top: 1px solid #ff3a0033; flex-wrap: wrap; }
        .control-btn { background: #1f1a15; border: 1px solid #ff3a0055; color: #ffcc99; padding: 0.7rem 1.2rem; border-radius: 2rem; cursor: pointer; display: flex; align-items: center; gap: 8px; font-weight: 700; font-family: 'Montserrat', sans-serif; transition: 0.2s; }
        .control-btn:hover { transform: scale(1.02); filter: brightness(1.1); }
        .control-btn.success { background: #cc3300; color: white; }
        .mock-participant { background: linear-gradient(145deg, #1a1512, #0a0806); display: flex; flex-direction: column; align-items: center; justify-content: center; color: #ccaa88; }
        .participants-badge { background: rgba(255, 58, 0, 0.15); padding: 0.3rem 1rem; border-radius: 2rem; font-size: 0.75rem; color: #ffaa77; font-weight: 600; }
        .drive-status { font-size: 0.7rem; color: #88aaff; margin-top: 0.5rem; text-align: center; }
        footer { text-align: center; padding: 1rem; font-size: 0.6rem; border-top: 1px solid rgba(255, 58, 0, 0.2); color: #886644; }
        @media (max-width: 700px) { .nav-container, .content-pane { padding: 1rem; } .video-grid { grid-template-columns: 1fr; } }
    </style>
</head>
<body>
<div class="app-container" id="appRoot">
    <!-- LOGIN -->
    <div id="loginScreen" class="login-screen">
        <div class="login-card">
            <div class="brand-text">CAFF</div>
            <div class="brand-sub">FULL TRAINING</div>
            <div class="input-group"><input type="email" id="loginEmail" placeholder="EMAIL" value="caff@train.com"></div>
            <div class="input-group"><input type="password" id="loginPassword" placeholder="PASSWORD" value="caff123"></div>
            <button class="login-btn" id="doLoginBtn">ACCEDER</button>
            <div id="loginErrorMsg" style="color:#ff6666; margin-top:1rem; font-size:0.8rem;"></div>
        </div>
    </div>

    <!-- DASHBOARD -->
    <div id="dashboardPanel" class="dashboard">
        <div class="dashboard-header">
            <div><h3 id="welcomeUserName" style="color:#ffcc66;">CAFF ATHLETE</h3><p style="color:#aa8866;">ENTRENAMIENTO INTELIGENTE</p></div>
            <div style="display:flex; gap:12px; align-items:center;"><i class="fas fa-crown" style="color:#ffcc66;"></i><span style="color:#ffcc99;">RACHA: 12 DÍAS</span><button id="logoutBtnHeader" style="background:none; border:none; color:#ff8855; font-size:1.2rem;"><i class="fas fa-sign-out-alt"></i></button></div>
        </div>

        <!-- NAVEGACIÓN -->
        <div class="nav-container">
            <div class="nav-tabs">
                <button class="nav-btn active" data-pane="paneResumen"><i class="fas fa-chart-line"></i> RESUMEN</button>
                <button class="nav-btn" data-pane="paneEntrenos"><i class="fas fa-heartbeat"></i> ENTRENOS</button>
                <button class="nav-btn" data-pane="paneLive"><i class="fas fa-video"></i> CAFF LIVE</button>
            </div>
        </div>

        <div class="content-pane">
            <!-- BLOQUE 1: RESUMEN -->
            <div class="pane active-pane" id="paneResumen">
                <div class="stats-grid">
                    <div class="stat-card"><div class="stat-title">CALORÍAS</div><div class="stat-value">2,845</div><div class="stat-unit">KCAL</div></div>
                    <div class="stat-card"><div class="stat-title">MINUTOS ACTIVOS</div><div class="stat-value">387</div><div class="stat-unit">MIN</div></div>
                    <div class="stat-card"><div class="stat-title">PESO LEVANTADO</div><div class="stat-value">6,240</div><div class="stat-unit">KG</div></div>
                    <div class="stat-card"><div class="stat-title">SESIONES CAFF</div><div class="stat-value">8</div><div class="stat-unit">ESTE MES</div></div>
                </div>
                <div style="background:rgba(0,0,0,0.4); border-radius:1rem; padding:1.5rem; display:flex; flex-wrap:wrap; gap:2rem; align-items:center;">
                    <div style="text-align:center;">
                        <svg width="130" height="130" viewBox="0 0 140 140">
                            <defs><linearGradient id="gradRing" x1="0%" y1="0%" x2="100%" y2="100%"><stop offset="0%" stop-color="#ff3a00"/><stop offset="100%" stop-color="#ffaa44"/></linearGradient></defs>
                            <circle cx="70" cy="70" r="58" fill="none" class="ring-bg"/>
                            <circle id="progressCircle" cx="70" cy="70" r="58" fill="none" class="ring-progress" stroke-dasharray="364.4" stroke-dashoffset="364.4"/>
                            <text x="70" y="70" fill="#ffcc99" font-size="1.3rem" font-weight="800" text-anchor="middle" dominant-baseline="middle" id="progressPercent">0%</text>
                        </svg>
                        <p style="color:#cc9966;">CUMPLIMIENTO</p>
                    </div>
                    <div style="flex:1">
                        <div class="workout-item"><span><i class="fas fa-check-circle" style="color:#ff6633;"></i> CAFF STRENGTH</span><span>COMPLETADO</span></div>
                        <div class="workout-item"><span><i class="fas fa-running"></i> HIIT EXPLOSIVO</span><span>PENDIENTE</span></div>
                        <div class="workout-item"><span><i class="fas fa-swimmer"></i> RECUPERACIÓN</span><span>COMPLETADO</span></div>
                    </div>
                </div>
            </div>

            <!-- BLOQUE 2: ENTRENOS -->
            <div class="pane" id="paneEntrenos">
                <h3 style="color:#ffaa77; margin-bottom:1rem;"><i class="fas fa-dumbbell"></i> RUTINA CAFF - POWER EDITION</h3>
                <table class="exercise-table">
                    <thead><tr><th>EJERCICIO</th><th>DURACIÓN</th><th>INTENSIDAD</th><th>ESTADO</th></tr></thead>
                    <tbody>
                        <tr><td>PRESS BANCA</td><td>4x10</td><td>85%</td><td><span class="badge-complete">✅ HECHO</span></td></tr>
                        <tr><td>SENTADILLA PESA</td><td>4x12</td><td>80%</td><td><span class="badge-complete">✅ HECHO</span></td></tr>
                        <tr><td>DOMINADAS CAFF</td><td>3x8</td><td>PESO EXTRA</td><td><span style="background:#ff33002d; padding:0.2rem 0.8rem; border-radius:2rem; color:#ffaa66;">🔥 HOY</span></td></tr>
                        <tr><td>PESO MUERTO</td><td>3x6</td><td>140KG</td><td><span style="color:#ffaa66;">📅 MAÑANA</span></td></tr>
                    </tbody>
                </table>
                <div style="margin-top:2rem; background:#ff3a0010; border-left:4px solid #ff3a00; padding:1rem;">
                    <i class="fas fa-clock" style="color:#ff6633;"></i> <strong style="color:#ffcc99;">PRÓXIMA CLASE EN VIVO:</strong> <span style="color:#fff0cc;">CAFF LIVE con Coach - Miércoles 19:00hs</span>
                </div>
            </div>

            <!-- BLOQUE 3: CAFF LIVE + GOOGLE DRIVE -->
            <div class="pane" id="paneLive">
                <div class="meet-container">
                    <div class="video-grid" id="videoGrid"></div>
                    <div class="meet-controls">
                        <button id="toggleCamBtn" class="control-btn"><i class="fas fa-video"></i> CÁMARA</button>
                        <button id="toggleMicBtn" class="control-btn"><i class="fas fa-microphone"></i> MIC</button>
                        <button id="shareScreenBtn" class="control-btn"><i class="fas fa-desktop"></i> PANTALLA</button>
                        <button id="simulateCoachBtn" class="control-btn success"><i class="fas fa-chalkboard-user"></i> COACH CAFF</button>
                        <button id="selectFromDriveBtn" class="control-btn"><i class="fab fa-google-drive"></i> CARGAR RUTINA</button>
                        <button id="saveToDriveBtn" class="control-btn success"><i class="fab fa-google-drive"></i> GUARDAR PROGRESO</button>
                        <span class="participants-badge"><i class="fas fa-users"></i> <span id="participantCount">4</span> ATLETAS</span>
                    </div>
                    <div id="driveStatus" class="drive-status"></div>
                    <div style="padding:0.8rem;text-align:center;font-size:0.65rem;color:#aa8866;">Cámara y micrófono reales | Google Drive: guarda/carga tu progreso</div>
                </div>
            </div>
        </div>
        <footer>© CAFF FULL TRAINING — HARD WORK. DEDICATION. ELITE.</footer>
    </div>
</div>

<script>
    // ==================== CONFIGURACIÓN DE GOOGLE DRIVE ====================
    // 🔴 IMPORTANTE: REEMPLAZA ESTOS VALORES CON LOS TUYOS DE GOOGLE CLOUD CONSOLE 🔴
    const CLIENT_ID = 'TU_CLIENT_ID.apps.googleusercontent.com';  // <-- CAMBIA ESTO
    const API_KEY = 'TU_API_KEY';                                 // <-- CAMBIA ESTO
    const APP_ID = 'TU_NUMERO_DE_PROYECTO';                       // <-- CAMBIA ESTO
    
    const SCOPES = 'https://www.googleapis.com/auth/drive.file';
    
    let accessToken = null;
    let tokenClient = null;
    
    // ==================== LOGIN Y DASHBOARD ====================
    const loginScreen = document.getElementById('loginScreen');
    const dashboard = document.getElementById('dashboardPanel');
    const loginBtn = document.getElementById('doLoginBtn');
    const logoutBtn = document.getElementById('logoutBtnHeader');
    const errorMsgDiv = document.getElementById('loginErrorMsg');
    const welcomeSpan = document.getElementById('welcomeUserName');
    
    let localStream = null, isCameraEnabled = true, isMicEnabled = true, screenStream = null, isScreenSharing = false, localVideoElement = null;
    
    function doLogin() {
        const email = document.getElementById('loginEmail').value.trim();
        const pwd = document.getElementById('loginPassword').value.trim();
        if((email.includes("@") && pwd.length > 0) || (email === "caff@train.com" && pwd === "caff123")) {
            welcomeSpan.innerText = "CAFF ATHLETE";
            loginScreen.style.display = 'none';
            dashboard.style.display = 'block';
            errorMsgDiv.innerText = "";
            initMeetRoom();
            initRingProgress(72);
            initDrivePicker();
        } else errorMsgDiv.innerText = "ACCESO DENEGADO // usa caff@train.com / caff123";
    }
    
    function logoutApp() { 
        if(localStream) localStream.getTracks().forEach(t=>t.stop()); 
        if(screenStream) screenStream.getTracks().forEach(t=>t.stop()); 
        loginScreen.style.display='flex'; 
        dashboard.style.display='none'; 
    }
    
    loginBtn.addEventListener('click', doLogin);
    logoutBtn.addEventListener('click', logoutApp);
    
    // ==================== CAFF LIVE (CÁMARA) ====================
    async function initMeetRoom() {
        const grid = document.getElementById('videoGrid'); if(!grid) return;
        grid.innerHTML = '';
        try {
            const stream = await navigator.mediaDevices.getUserMedia({ video: true, audio: true });
            localStream = stream;
            const card = document.createElement('div'); card.className = 'video-card';
            const video = document.createElement('video'); video.autoplay = true; video.muted = true; video.srcObject = localStream; localVideoElement = video;
            card.appendChild(video); card.innerHTML += '<div class="video-label"><i class="fas fa-user"></i> TÚ (CAFF)</div>';
            grid.appendChild(card);
        } catch(e) { grid.innerHTML = '<div class="video-card mock-participant"><i class="fas fa-video-slash"></i><span>CÁMARA NO DISPONIBLE</span></div>'; }
        const mock = [{name:"COACH CAFF", icon:"fas fa-chalkboard-user"},{name:"MIEMBRO ELITE", icon:"fas fa-dumbbell"},{name:"ATLETA", icon:"fas fa-heartbeat"},{name:"PESADO", icon:"fas fa-running"}];
        mock.forEach(u=>{ let c=document.createElement('div'); c.className='video-card mock-participant'; c.innerHTML=`<i class="${u.icon}" style="font-size:2.5rem; color:#ff6633;"></i><div class="video-label">${u.name}</div>`; grid.appendChild(c); });
        updateCount(1+mock.length);
    }
    
    function updateCount(c){ let s=document.getElementById('participantCount'); if(s) s.innerText=c; }
    
    document.getElementById('toggleCamBtn')?.addEventListener('click',()=>{ if(localStream){ let t=localStream.getVideoTracks()[0]; if(t){ isCameraEnabled=!isCameraEnabled; t.enabled=isCameraEnabled; document.getElementById('toggleCamBtn').innerHTML=isCameraEnabled?'<i class="fas fa-video"></i> CÁMARA':'<i class="fas fa-video-slash"></i> CÁMARA'; } } });
    document.getElementById('toggleMicBtn')?.addEventListener('click',()=>{ if(localStream){ let t=localStream.getAudioTracks()[0]; if(t){ isMicEnabled=!isMicEnabled; t.enabled=isMicEnabled; document.getElementById('toggleMicBtn').innerHTML=isMicEnabled?'<i class="fas fa-microphone"></i> MIC':'<i class="fas fa-microphone-slash"></i> MIC'; } } });
    document.getElementById('shareScreenBtn')?.addEventListener('click',async()=>{ if(isScreenSharing && screenStream){ screenStream.getTracks().forEach(t=>t.stop()); screenStream=null; isScreenSharing=false; document.getElementById('shareScreenBtn').innerHTML='<i class="fas fa-desktop"></i> PANTALLA'; if(localStream && localVideoElement) localVideoElement.srcObject=localStream; } else { try{ let ds=await navigator.mediaDevices.getDisplayMedia({video:true}); screenStream=ds; isScreenSharing=true; document.getElementById('shareScreenBtn').innerHTML='<i class="fas fa-stop"></i> DETENER'; if(localVideoElement) localVideoElement.srcObject=ds; ds.getVideoTracks()[0].onended=()=>{ if(isScreenSharing){ isScreenSharing=false; document.getElementById('shareScreenBtn').innerHTML='<i class="fas fa-desktop"></i> PANTALLA'; if(localStream && localVideoElement) localVideoElement.srcObject=localStream; screenStream=null; } }; }catch(e){ alert("No se pudo compartir pantalla"); } } });
    document.getElementById('simulateCoachBtn')?.addEventListener('click',()=>{ const grid=document.getElementById('videoGrid'); const badge=document.createElement('div'); badge.style.cssText='position:fixed;bottom:20px;right:20px;background:#ff3a00;color:#fff;padding:10px 18px;border-radius:30px;z-index:999;font-weight:bold;'; badge.innerHTML='<i class="fas fa-chalkboard-user"></i> MODO COACH CAFF ACTIVADO'; document.body.appendChild(badge); setTimeout(()=>badge.remove(),3000); if(localVideoElement) localVideoElement.parentElement.style.border='2px solid #ff3a00'; const extra=document.createElement('div'); extra.className='video-card mock-participant'; extra.innerHTML='<i class="fas fa-user-plus"></i><div class="video-label">NUEVO ATLETA</div>'; grid.appendChild(extra); updateCount(grid.children.length); });
    
    function initRingProgress(p=72){ let c=document.getElementById('progressCircle'); let t=document.getElementById('progressPercent'); if(c){ let r=58, circ=2*Math.PI*r; c.setAttribute('stroke-dasharray',circ); c.setAttribute('stroke-dashoffset',circ-(p/100)*circ); if(t) t.innerText=p+'%'; } }
    
    // ==================== GOOGLE DRIVE: INICIALIZACIÓN ====================
    function initDrivePicker() {
        if(!CLIENT_ID || CLIENT_ID === 'TU_CLIENT_ID.apps.googleusercontent.com') {
            document.getElementById('driveStatus').innerHTML = '⚠️ Configura CLIENT_ID, API_KEY y APP_ID en el código para usar Google Drive';
            return;
        }
        
        tokenClient = google.accounts.oauth2.initTokenClient({
            client_id: CLIENT_ID,
            scope: SCOPES,
            callback: (tokenResponse) => {
                accessToken = tokenResponse.access_token;
                document.getElementById('driveStatus').innerHTML = '✅ Conectado a Google Drive';
                setTimeout(() => {
                    if(document.getElementById('driveStatus').innerHTML === '✅ Conectado a Google Drive')
                        document.getElementById('driveStatus').innerHTML = '';
                }, 3000);
            },
        });
        
        // Cargar librería de picker
        gapi.load('picker', () => { console.log('Google Picker loaded'); });
    }
    
    function requestDriveAccess() {
        if(!tokenClient) {
            document.getElementById('driveStatus').innerHTML = '❌ Error: Configura las credenciales de Google Drive';
            return;
        }
        tokenClient.requestAccessToken({prompt: 'consent'});
    }
    
    // ==================== SUBIR ARCHIVO A DRIVE ====================
    async function uploadToDrive(fileName, jsonData) {
        if(!accessToken) {
            document.getElementById('driveStatus').innerHTML = '🔐 Conecta con Google Drive primero';
            requestDriveAccess();
            return false;
        }
        
        const metadata = { 'name': fileName, 'mimeType': 'application/json' };
        const blob = new Blob([JSON.stringify(jsonData)], {type: 'application/json'});
        const form = new FormData();
        form.append('metadata', new Blob([JSON.stringify(metadata)], {type: 'application/json'}));
        form.append('file', blob);
        
        try {
            const response = await fetch('https://www.googleapis.com/upload/drive/v3/files?uploadType=multipart', {
                method: 'POST',
                headers: new Headers({ 'Authorization': 'Bearer ' + accessToken }),
                body: form
            });
            const result = await response.json();
            document.getElementById('driveStatus').innerHTML = `✅ Progreso guardado: ${fileName}`;
            setTimeout(() => { if(document.getElementById('driveStatus').innerHTML.includes('guardado')) document.getElementById('driveStatus').innerHTML = ''; }, 3000);
            return true;
        } catch(e) {
            document.getElementById('driveStatus').innerHTML = '❌ Error al guardar';
            return false;
        }
    }
    
    // ==================== ABRIR SELECTOR DE ARCHIVOS ====================
    function openFilePicker() {
        if(!accessToken) {
            document.getElementById('driveStatus').innerHTML = '🔐 Conecta con Google Drive primero';
            requestDriveAccess();
            return;
        }
        
        const view = new google.picker.View(google.picker.ViewId.DOCS);
        view.setMimeTypes('application/json');
        const picker = new google.picker.PickerBuilder()
            .enableFeature(google.picker.Feature.NAV_HIDDEN)
            .setAppId(APP_ID)
            .setDeveloperKey(API_KEY)
            .setOAuthToken(accessToken)
            .addView(view)
            .addView(new google.picker.DocsUploadView())
            .setCallback(pickerCallback)
            .build();
        picker.setVisible(true);
    }
    
    function pickerCallback(data) {
        if (data.action === google.picker.Action.PICKED) {
            const file = data.docs[0];
            const fileId = file.id;
            const fileName = file.name;
            document.getElementById('driveStatus').innerHTML = `📁 Cargando: ${fileName}...`;
            loadFileContent(fileId);
        }
    }
    
    async function loadFileContent(fileId) {
        try {
            const response = await fetch(`https://www.googleapis.com/drive/v3/files/${fileId}?alt=media`, {
                headers: new Headers({ 'Authorization': 'Bearer ' + accessToken })
            });
            const content = await response.json();
            document.getElementById('driveStatus').innerHTML = `✅ Rutina cargada: ${content.name || 'entrenamiento'}`;
            setTimeout(() => { if(document.getElementById('driveStatus').innerHTML.includes('cargada')) document.getElementById('driveStatus').innerHTML = ''; }, 3000);
            if(content.exercises) {
                alert(`Rutina "${content.name}" cargada con ${content.exercises.length} ejercicios`);
            }
        } catch(e) {
            document.getElementById('driveStatus').innerHTML = '❌ Error al leer el archivo';
        }
    }
    
    // ==================== GUARDAR PROGRESO ACTUAL ====================
    function saveCurrentProgress() {
        const progressData = {
            name: `caff-progreso-${new Date().toISOString().slice(0,19)}`,
            date: new Date().toISOString(),
            calorias: 2845,
            minutosActivos: 387,
            pesoLevantado: 6240,
            sesiones: 8,
            ejerciciosCompletados: ['Press banca', 'Sentadilla', 'Plancha']
        };
        uploadToDrive(`${progressData.name}.json`, progressData);
    }
    
    // ==================== EVENTOS DE DRIVE ====================
    document.getElementById('saveToDriveBtn')?.addEventListener('click', saveCurrentProgress);
    document.getElementById('selectFromDriveBtn')?.addEventListener('click', openFilePicker);
    
    // ==================== NAVEGACIÓN ENTRE PESTAÑAS ====================
    const navBtns = document.querySelectorAll('.nav-btn');
    const panes = document.querySelectorAll('.pane');
    function switchPane(paneId) {
        panes.forEach(p => p.classList.remove('active-pane'));
        document.getElementById(paneId).classList.add('active-pane');
        navBtns.forEach(btn => {
            btn.classList.remove('active');
            if(btn.getAttribute('data-pane') === paneId) btn.classList.add('active');
        });
        if(paneId === 'paneLive' && document.getElementById('videoGrid')?.children.length === 0) initMeetRoom();
    }
    navBtns.forEach(btn => btn.addEventListener('click', () => switchPane(btn.getAttribute('data-pane'))));
</script>
</body>
</html>
