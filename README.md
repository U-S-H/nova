<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NOVA | Institutional Asset Infrastructure</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary: #0A192F;
            --accent: #64FFDA;
            --card-bg: #112240;
            --text-main: #CCD6F6;
            --text-dim: #8892B0;
        }

        body {
            margin: 0;
            padding: 0;
            font-family: 'Inter', sans-serif;
            background-color: var(--primary);
            color: var(--text-main);
            display: flex;
            min-height: 100vh;
        }

        /* Side Info Panel - Professional Details */
        .info-panel {
            flex: 1;
            padding: 60px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            background: linear-gradient(135deg, #0A192F 0%, #020c1b 100%);
            border-right: 1px solid rgba(100, 255, 218, 0.1);
        }

        .info-panel h1 {
            font-size: 3.5rem;
            font-weight: 800;
            color: var(--accent);
            margin: 0;
            letter-spacing: -2px;
        }

        .info-panel p {
            font-size: 1.1rem;
            color: var(--text-dim);
            max-width: 500px;
            line-height: 1.6;
            margin: 20px 0;
        }

        .stats {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-top: 40px;
        }

        .stat-item h3 { color: var(--accent); margin: 0; font-size: 1.5rem; }
        .stat-item p { font-size: 0.9rem; margin: 5px 0; }

        /* Login Section */
        .auth-panel {
            flex: 1;
            display: flex;
            justify-content: center;
            align-items: center;
            background-color: #020c1b;
        }

        .auth-card {
            background: var(--card-bg);
            padding: 40px;
            border-radius: 12px;
            width: 100%;
            max-width: 400px;
            box-shadow: 0 25px 50px rgba(0,0,0,0.4);
            border-top: 4px solid var(--accent);
        }

        .auth-card h2 { text-align: center; margin-bottom: 30px; letter-spacing: 1px; }

        .input-group { margin-bottom: 20px; }
        .input-group label { display: block; font-size: 0.8rem; color: var(--accent); margin-bottom: 8px; }
        
        input {
            width: 100%;
            padding: 12px;
            background: var(--primary);
            border: 1px solid #233554;
            color: white;
            border-radius: 4px;
            box-sizing: border-box;
            outline: none;
        }

        input:focus { border-color: var(--accent); }

        .btn-prime {
            width: 100%;
            padding: 14px;
            background: var(--accent);
            color: var(--primary);
            border: none;
            border-radius: 4px;
            font-weight: 800;
            cursor: pointer;
            text-transform: uppercase;
            transition: 0.3s;
        }

        .btn-prime:hover { transform: translateY(-2px); opacity: 0.9; }

        #dashboard { display: none; width: 100%; text-align: center; padding: 100px; }

        @media (max-width: 900px) {
            body { flex-direction: column; }
            .info-panel { padding: 40px; text-align: center; }
            .info-panel p { margin: 20px auto; }
        }
    </style>
</head>
<body>

    <div id="landing-page" style="display: flex; width: 100%;">
        <div class="info-panel">
            <h1>NOVA.</h1>
            <p>Empowering global investors with institutional-grade digital asset infrastructure. Secure, transparent, and built for the next generation of capital.</p>
            
            <div class="stats">
                <div class="stat-item">
                    <h3>$2.4B+</h3>
                    <p>Global Assets Managed</p>
                </div>
                <div class="stat-item">
                    <h3>24/7</h3>
                    <p>Network Uptime</p>
                </div>
                <div class="stat-item">
                    <h3>AES-256</h3>
                    <p>Military Grade Encryption</p>
                </div>
                <div class="stat-item">
                    <h3>100%</h3>
                    <p>Anonymous Protocol</p>
                </div>
            </div>
        </div>

        <div class="auth-panel">
            <div class="auth-card" id="auth-box">
                <h2 id="auth-title">TERMINAL ACCESS</h2>
                
                <div class="input-group">
                    <label>IDENTIFICATION</label>
                    <input type="text" id="username" placeholder="Enter username">
                </div>

                <div class="input-group">
                    <label>SECURITY KEY</label>
                    <input type="password" id="password" placeholder="••••••••">
                </div>

                <button class="btn-prime" onclick="attemptLogin()">Authorize Session</button>
                
                <p style="text-align: center; font-size: 0.7rem; color: var(--text-dim); margin-top: 20px;">
                    Protected by NOVA Shield Protocol &copy; 2026
                </p>
            </div>
        </div>
    </div>

    <div id="dashboard">
        <h1 style="color: var(--accent);">SESSION ACTIVE</h1>
        <p>Deposit Address for Institutional Funding:</p>
        <div style="background: #112240; padding: 20px; border-radius: 8px; display: inline-block; font-family: monospace; border: 1px dashed var(--accent);">
            0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2
        </div>
        <br><br>
        <button class="btn-prime" style="width: 200px;" onclick="logout()">Terminate Session</button>
    </div>

    <script>
        function attemptLogin() {
            const u = document.getElementById('username').value;
            const p = document.getElementById('password').value;

            // Default credentials (aap change kar sakti hain)
            if(u === "nova_global" && p === "nova786") {
                localStorage.setItem("session", "valid");
                showSecure();
            } else {
                alert("Access Denied: Invalid Credentials.");
            }
        }

        function showSecure() {
            document.getElementById('landing-page').style.display = 'none';
            document.getElementById('dashboard').style.display = 'block';
        }

        function logout() {
            localStorage.removeItem("session");
            location.reload();
        }

        if(localStorage.getItem("session") === "valid") {
            showSecure();
        }
    </script>
</body>
</html>
