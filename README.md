<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA OMNI | Professional Institutional Edition</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --bg: #020617; --card: #0f172a; --accent: #10B981; --text: #f8fafc; --dim: #94a3b8; --gold: #fbbf24; --danger: #ef4444; }
        body { margin: 0; font-family: 'Plus Jakarta Sans', sans-serif; background: var(--bg); color: var(--text); overflow-x: hidden; padding-bottom: 100px; }
        
        /* Live Ticker */
        #ticker { background: #1e293b; padding: 12px; font-size: 11px; font-weight: 800; white-space: nowrap; overflow: hidden; border-bottom: 1px solid rgba(16, 185, 129, 0.2); color: var(--accent); }
        .ticker-move { display: inline-block; animation: move 25s linear infinite; }
        @keyframes move { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }

        .glass { background: var(--card); border: 1px solid rgba(255,255,255,0.05); border-radius: 32px; padding: 22px; margin: 15px; box-shadow: 0 15px 35px rgba(0,0,0,0.4); }
        .btn-prime { width: 100%; padding: 18px; background: var(--accent); color: white; border: none; border-radius: 22px; font-weight: 800; cursor: pointer; transition: 0.3s; text-transform: uppercase; letter-spacing: 1px; }
        .btn-prime:active { transform: scale(0.95); }
        
        input, select { width: 100%; padding: 16px; background: #020617; border: 1px solid #334155; color: #fff; border-radius: 20px; margin-bottom: 15px; outline: none; font-family: inherit; font-weight: 600; }

        .app-header { padding: 20px; background: rgba(15, 23, 42, 0.9); backdrop-filter: blur(20px); border-bottom: 1px solid rgba(255,255,255,0.05); display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 1000; }
        
        /* Mining Progress Bar */
        .progress-container { width: 100%; background: #1e293b; border-radius: 10px; height: 6px; margin: 10px 0; overflow: hidden; }
        .progress-bar { height: 100%; background: var(--accent); width: 0%; transition: width 0.5s; box-shadow: 0 0 10px var(--accent); }

        /* Payout Toast */
        #toast { position: fixed; bottom: 100px; left: 50%; transform: translateX(-50%); background: var(--card); border: 1px solid var(--accent); padding: 12px 20px; border-radius: 20px; font-size: 11px; z-index: 9999; display: none; box-shadow: 0 10px 30px rgba(0,0,0,0.5); width: 80%; text-align: center; }

        .node-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; padding: 15px; }
        .node-card { background: var(--card); border-radius: 28px; padding: 18px; border: 1px solid rgba(255,255,255,0.03); text-align: center; position: relative; overflow: hidden; transition: 0.3s; }
        .node-img { width: 100%; height: 100px; border-radius: 20px; object-fit: cover; margin-bottom: 12px; }

        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: rgba(15, 23, 42, 0.98); display: flex; justify-content: space-around; padding: 18px 0; border-top: 1px solid #1e293b; z-index: 1000; }
        .nav-item { text-align: center; color: var(--dim); font-size: 10px; font-weight: 800; cursor: pointer; }
        .nav-item.active { color: var(--accent); }
    </style>
</head>
<body>

    <div id="ticker"><div class="ticker-move">⚡ NOVA OMNI: BTC $64,231 • ETH $3,452 • SYSTEM SECURE • LIVE PAYOUTS ACTIVE • JOIN OUR TEAM FOR 10% BONUS ⚡</div></div>

    <div id="toast"></div>

    <div id="auth-screen" style="min-height:100vh; display:flex; flex-direction:column; align-items:center; justify-content:center; padding:25px;">
        <h1 style="font-size:4rem; font-weight:900; letter-spacing:-4px; margin-bottom:10px;">NOVA<span style="color:var(--accent);">.</span></h1>
        <div class="glass" style="width:100%; max-width:400px;">
            <input type="text" id="username" placeholder="Username">
            <input type="password" id="password" placeholder="Passkey">
            <button class="btn-prime" onclick="handleAuth()">Access Dashboard</button>
            <p onclick="toggleAuth()" id="toggleTxt" style="text-align:center; font-size:12px; color:var(--dim); margin-top:20px; cursor:pointer;">New Identity? <span style="color:var(--accent)">Register</span></p>
        </div>
    </div>

    <div id="app-main" style="display:none;">
        <header class="app-header">
            <div style="display:flex; align-items:center; gap:12px;">
                <div style="width:40px; height:40px; background:var(--accent); border-radius:12px; display:flex; align-items:center; justify-content:center; font-weight:900;">N</div>
                <div><small id="userDisp" style="font-weight:900; display:block;">USER</small><span id="vipStatus" style="font-size:9px; background:var(--gold); padding:2px 8px; border-radius:5px; color:#000;">BASIC</span></div>
            </div>
            <i class="fa-solid fa-power-off" onclick="logout()" style="color:var(--danger); font-size:1.3rem;"></i>
        </header>

        <div id="home" class="screen active">
            <div class="glass" style="background: linear-gradient(135deg, #10B981, #064e3b); border:none;">
                <small style="opacity:0.8;">CURRENT ASSETS</small>
                <h1 style="font-size:3.5rem; margin:10px 0;" id="mainBal">$0.00</h1>
                <div class="progress-container"><div id="miningBar" class="progress-bar"></div></div>
                <div style="display:flex; justify-content:space-between; margin-top:15px; font-size:12px;">
                    <span>DAILY: <b id="dayYield">+$0.00</b></span>
                    <span>TOTAL: <b id="totProfit">$0.00</b></span>
                </div>
            </div>
            <div id="nodeGrid" class="node-grid"></div>
        </div>
    </div>

    <nav class="bottom-nav" id="nav-bar" style="display:none;">
        <div class="nav-item active" onclick="nav('home', this)"><i class="fa-solid fa-house"></i><br>HOME</div>
        <div class="nav-item"><i class="fa-solid fa-vault"></i><br>VAULT</div>
        <div class="nav-item"><i class="fa-solid fa-users"></i><br>TEAM</div>
    </nav>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, onSnapshot } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let user = null; let bal = 0;

        window.handleAuth = async () => {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            const ref = doc(db, "users", u);
            const snap = await getDoc(ref);
            if(snap.exists() && snap.data().password === p) {
                sessionStorage.setItem("nova_user", u);
                start();
            } else alert("Error!");
        };

        function start() {
            user = sessionStorage.getItem("nova_user");
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app-main').style.display = 'block';
            document.getElementById('nav-bar').style.display = 'flex';
            document.getElementById('userDisp').innerText = user.toUpperCase();

            onSnapshot(doc(db, "users", user), (d) => {
                bal = d.data().balance;
                document.getElementById('mainBal').innerText = "$" + bal.toLocaleString();
            });
            renderNodes();
            startMiningAnim();
            startToasts();
        }

        function startMiningAnim() {
            let p = 0;
            setInterval(() => {
                p = (p + 1) % 100;
                document.getElementById('miningBar').style.width = p + "%";
            }, 100);
        }

        function startToasts() {
            const names = ["James", "Emma", "Arjun", "Sarah", "Li", "Omar"];
            setInterval(() => {
                const toast = document.getElementById('toast');
                const n = names[Math.floor(Math.random()*names.length)];
                const a = Math.floor(Math.random()*500) + 50;
                toast.innerHTML = `<i class="fa-solid fa-circle-check" style="color:var(--accent)"></i> <b>${n}</b> just withdrew <b>$${a}</b> successfully!`;
                toast.style.display = "block";
                setTimeout(() => toast.style.display = "none", 4000);
            }, 8000);
        }

        function renderNodes() {
            const grid = document.getElementById('nodeGrid'); grid.innerHTML = "";
            const prices = [30, 100, 500, 1000, 5000, 10000, 30000];
            prices.forEach((p, i) => {
                grid.innerHTML += `<div class="node-card">
                    <img src="https://images.unsplash.com/photo-1639762681485-074b7f938ba0?w=300" class="node-img">
                    <h2 style="margin:0">$${p.toLocaleString()}</h2>
                    <p style="font-size:10px; color:var(--accent);">CLUSTER V.${i+1}</p>
                    <button class="btn-prime" style="padding:10px; font-size:10px; border-radius:12px; margin-top:10px;">Activate</button>
                </div>`;
            });
        }

        if(sessionStorage.getItem("nova_user")) start();
    </script>
</body>
</html>
