<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Global Asset Terminal</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --accent: #00FFA3; --bg: #050505; --card: #0f0f0f; --text: #FFFFFF; --danger: #ff4d4d; }
        body { margin: 0; font-family: 'Plus Jakarta Sans', sans-serif; background: var(--bg); color: var(--text); overflow-x: hidden; -webkit-tap-highlight-color: transparent; }
        .glass { background: rgba(15, 15, 15, 0.98); border: 1px solid rgba(212, 175, 55, 0.2); border-radius: 30px; padding: 25px; backdrop-filter: blur(20px); }
        .input-nova { width: 100%; padding: 18px; background: #000; border: 1px solid #222; color: #fff; border-radius: 18px; margin-bottom: 12px; outline: none; box-sizing: border-box; font-family: inherit; }
        .btn-nova { background: linear-gradient(135deg, var(--gold), #F2CC60); color: #000; border: none; padding: 18px; border-radius: 18px; font-weight: 800; cursor: pointer; text-transform: uppercase; width: 100%; transition: 0.3s; }
        .btn-nova:active { transform: scale(0.96); }

        /* Auth & UI Screens */
        #auth-screen { height: 100vh; display: flex; flex-direction: column; align-items: center; justify-content: center; padding: 30px; background: radial-gradient(circle at top, #1a1a00, #050505); }
        .auth-toggle { display: flex; gap: 10px; margin-bottom: 25px; background: #000; padding: 5px; border-radius: 15px; width: 100%; max-width: 400px; }
        .auth-toggle button { flex: 1; border: none; padding: 10px; border-radius: 10px; font-weight: 800; color: #fff; background: transparent; cursor: pointer; }
        .auth-toggle .active { background: var(--gold); color: #000; }

        .stat-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-top: 15px; }
        .stat-box { background: #1a1a1a; padding: 15px; border-radius: 20px; border-bottom: 3px solid var(--gold); }
        .node-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; padding: 15px; }
        .node-card { background: #0f0f0f; border: 1px solid #222; border-radius: 22px; overflow: hidden; position: relative; padding-bottom: 10px; }
        .node-img { width: 100%; height: 100px; object-fit: cover; filter: brightness(0.6); background: #1a1a1a; }
        .node-timer { position: absolute; top: 8px; right: 8px; background: rgba(0,0,0,0.8); color: var(--accent); padding: 3px 8px; border-radius: 50px; font-size: 8px; font-weight: 800; border: 1px solid var(--accent); }

        /* Navigation */
        .navbar { position: fixed; bottom: 0; width: 100%; background: rgba(0,0,0,0.98); display: flex; justify-content: space-around; padding: 18px 0; border-top: 1px solid #222; z-index: 5000; }
        .nav-link { color: #555; text-align: center; font-size: 9px; font-weight: 800; text-decoration: none; }
        .nav-link.active { color: var(--gold); }

        .screen { display: none; padding-bottom: 120px; animation: slideUp 0.4s ease; }
        .active-screen { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }

        /* Admin UI */
        #admin-panel { display: none; position: fixed; inset: 0; background: #000; z-index: 9999; padding: 25px; overflow-y: auto; }
        .adm-card { background: #111; border: 1px solid var(--gold); border-radius: 20px; padding: 15px; margin-bottom: 12px; }
        #payout-toast { position: fixed; top: 20px; left: 50%; transform: translateX(-50%); background: #111; border: 1px solid var(--accent); padding: 10px 20px; border-radius: 50px; font-size: 10px; z-index: 9999; display: none; }
    </style>
</head>
<body>

    <div id="payout-toast"></div>

    <div id="auth-screen">
        <h1 id="logo-trigger" onclick="handleLogoTaps()" style="font-size:3.5rem; letter-spacing:-3px; font-weight:900;">NOVA<span style="color:var(--gold)">.</span></h1>
        <div class="auth-toggle">
            <button id="tab-login" class="active" onclick="toggleAuth('login')">LOGIN</button>
            <button id="tab-signup" onclick="toggleAuth('signup')">SIGNUP</button>
        </div>
        <div class="glass" style="width:100%; max-width:400px; box-sizing:border-box;">
            <input type="text" id="u-id" placeholder="Account ID" class="input-nova">
            <input type="password" id="u-pass" placeholder="Passkey" class="input-nova">
            <div id="reg-fields" style="display:none;">
                <input type="text" id="u-whatsapp" placeholder="WhatsApp" class="input-nova">
                <input type="text" id="u-ref" placeholder="Referral ID" class="input-nova">
            </div>
            <input type="text" id="admin-key" style="display:none; border-color:var(--gold);" placeholder="Admin Master Key" class="input-nova">
            <button class="btn-nova" onclick="processAuth()">Enter Terminal</button>
        </div>
    </div>

    <div id="admin-panel">
        <h2 style="color:var(--gold);">ADMIN OVERRIDE</h2>
        <div id="admin-requests"></div>
        <button onclick="location.reload()" class="btn-nova" style="background:var(--danger); margin-top:20px;">EXIT ADMIN</button>
    </div>

    <div id="app-main" style="display:none;">
        <header style="padding:20px; display:flex; justify-content:space-between; align-items:center;">
            <div><span id="user-display" style="font-weight:800; font-size:22px;">...</span></div>
            <div onclick="nav('office')"><i class="fa-solid fa-user-gear" style="color:var(--gold); font-size:20px;"></i></div>
        </header>

        <div id="dash" class="screen active-screen">
            <div style="padding:15px;">
                <div class="glass">
                    <p style="color:#666; font-size:10px; letter-spacing:2px;">TOTAL CAPITAL</p>
                    <h1 id="bal-main" style="font-size:3.5rem; margin:10px 0;">$0.00</h1>
                    <div class="stat-grid">
                        <div class="stat-box"><small>DAILY YIELD</small><br><b id="v-daily" style="color:var(--accent);">+$0.00</b></div>
                        <div class="stat-box"><small>EARNED</small><br><b id="v-total">$0.00</b></div>
                    </div>
                </div>
            </div>
            <div id="nodes-grid" class="node-grid"></div>
        </div>

        <div id="finance" class="screen">
            <div style="padding:15px;">
                <div class="glass">
                    <div style="display:flex; gap:10px; margin-bottom:20px;">
                        <button class="btn-nova" onclick="setFin('dep')" style="flex:1;">DEPOSIT</button>
                        <button class="btn-nova" onclick="setFin('wd')" style="flex:1; background:#222; color:#fff;">WITHDRAW</button>
                    </div>
                    <div id="box-dep">
                        <input type="number" id="dep-amt" placeholder="Amount ($)" class="input-nova">
                        <input type="text" id="dep-tid" placeholder="TID / Hash" class="input-nova">
                        <input type="file" id="proof-file" accept="image/*" class="input-nova" onchange="readBase64(this)">
                        <button class="btn-nova" onclick="submitDeposit()">Verify Payment</button>
                    </div>
                    <div id="box-wd" style="display:none;">
                        <input type="number" id="wd-amt" placeholder="Withdrawal Amount" class="input-nova">
                        <input type="text" id="wd-acc" placeholder="Account Details" class="input-nova">
                        <button class="btn-nova" onclick="submitWithdraw()">Request Payout</button>
                    </div>
                </div>
            </div>
            <div style="padding:15px;">
                <h4>HISTORY</h4>
                <div id="history-list"></div>
            </div>
        </div>

        <div id="office" class="screen">
            <div style="padding:20px;">
                <div class="glass">
                    <h3>REFERRAL SYSTEM</h3>
                    <small id="ref-link" style="color:var(--gold); font-weight:800; word-break:break-all;"></small>
                </div>
                <button onclick="secureLogout()" class="btn-nova" style="background:transparent; border:1px solid var(--danger); color:var(--danger); margin-top:30px;">SECURE LOGOUT</button>
            </div>
        </div>
    </div>

    <nav class="navbar" id="nav-bar" style="display:none;">
        <a href="#" class="nav-link active" onclick="nav('dash', this)"><i class="fa-solid fa-microchip"></i><br>MINING</a>
        <a href="#" class="nav-link" onclick="nav('finance', this)"><i class="fa-solid fa-wallet"></i><br>FINANCE</a>
        <a href="#" class="nav-link" onclick="nav('office', this)"><i class="fa-solid fa-gear"></i><br>OFFICE</a>
    </nav>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo",
            authDomain: "home-94d45.firebaseapp.com",
            projectId: "home-94d45",
            appId: "1:964390949419:web:589840cb91b7e42ecd506e"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let user = null; let base64Data = ""; let mode = "login"; let taps = 0;

        window.handleLogoTaps = () => { taps++; if(taps >= 6) { document.getElementById('admin-key').style.display='block'; }};

        window.toggleAuth = (m) => {
            mode = m;
            document.getElementById('reg-fields').style.display = m === 'signup' ? 'block' : 'none';
            document.getElementById('tab-login').className = m === 'login' ? 'active' : '';
            document.getElementById('tab-signup').className = m === 'signup' ? 'active' : '';
        };

        window.processAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            const ak = document.getElementById('admin-key').value;

            if(ak === "nov786") { document.getElementById('admin-panel').style.display='block'; loadAdmin(); return; }
            if(!id || !pw) return alert("Enter credentials.");

            const ref = doc(db, "users", id);
            const snap = await getDoc(ref);

            if(mode === 'login') {
                if(snap.exists() && snap.data().password === pw) { boot(id); }
                else alert("Wrong credentials.");
            } else {
                if(snap.exists()) return alert("User exists.");
                await setDoc(ref, { password: pw, balance: 0, daily: 0, total: 0, whatsapp: document.getElementById('u-whatsapp').value, time: Date.now() });
                boot(id);
            }
        };

        function boot(id) {
            user = id; sessionStorage.setItem("nova_u", id);
            document.getElementById('auth-screen').style.display='none';
            document.getElementById('app-main').style.display='block';
            document.getElementById('nav-bar').style.display='flex';
            document.getElementById('user-display').innerText = id.toUpperCase();
            document.getElementById('ref-link').innerText = window.location.href + "?ref=" + id;

            onSnapshot(doc(db, "users", id), (d) => {
                const data = d.data();
                document.getElementById('bal-main').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "+$" + (data.daily || 0).toFixed(2);
                document.getElementById('v-total').innerText = "$" + (data.total || 0).toFixed(2);
            });
            renderNodes(); loadHistory(); startVisualMining();
        }

        function renderNodes() {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            const tiers = [30, 50, 100, 500, 1000, 5000];
            tiers.forEach(p => {
                grid.innerHTML += `
                    <div class="node-card">
                        <div class="node-timer">29d 23h</div>
                        <img src="https://images.unsplash.com/photo-1639762681485-074b7f938ba0?w=400" class="node-img">
                        <div style="text-align:center; padding:10px;">
                            <b style="color:var(--gold);">$${p}</b><br>
                            <button onclick="buyNode(${p}, ${p*0.05})" class="btn-nova" style="padding:8px; font-size:10px; margin-top:5px;">STAKE</button>
                        </div>
                    </div>`;
            });
        }

        window.buyNode = async (p, d) => {
            const r = doc(db, "users", user); const s = await getDoc(r);
            if(s.data().balance < p) return alert("Low balance.");
            await updateDoc(r, { balance: s.data().balance - p, daily: (s.data().daily || 0) + d });
            alert("Node Activated!");
        };

        window.readBase64 = (el) => {
            const reader = new FileReader();
            reader.onload = () => base64Data = reader.result;
            reader.readAsDataURL(el.files[0]);
        };

        window.setFin = (t) => {
            document.getElementById('box-dep').style.display = t === 'dep' ? 'block' : 'none';
            document.getElementById('box-wd').style.display = t === 'wd' ? 'block' : 'none';
        };

        window.submitDeposit = async () => {
            const amt = document.getElementById('dep-amt').value;
            await addDoc(collection(db, "logs"), { user, amount: parseFloat(amt), type: 'Deposit', proof: base64Data, status: 'Pending', time: Date.now() });
            alert("Request sent!");
        };

        window.submitWithdraw = async () => {
            const amt = document.getElementById('wd-amt').value;
            await addDoc(collection(db, "logs"), { user, amount: parseFloat(amt), type: 'Withdrawal', status: 'Pending', time: Date.now() });
            alert("Request sent!");
        };

        function loadHistory() {
            const q = query(collection(db, "logs"), where("user", "==", user), orderBy("time", "desc"));
            onSnapshot(q, (snap) => {
                const l = document.getElementById('history-list'); l.innerHTML = "";
                snap.forEach(d => {
                    const t = d.data();
                    let color = t.status === 'Verified' ? 'var(--accent)' : (t.status === 'Rejected' ? 'var(--danger)' : 'var(--gold)');
                    l.innerHTML += `<div class="glass" style="margin-bottom:10px; padding:12px; border-left:3px solid ${color};">
                        <div style="display:flex; justify-content:space-between; font-size:12px;">
                            <b>${t.type}</b><span style="color:${color}">${t.status}</span>
                        </div>
                        <small>$${t.amount}</small>
                    </div>`;
                });
            });
        }

        function loadAdmin() {
            const q = query(collection(db, "logs"), where("status", "==", "Pending"), orderBy("time", "asc"));
            onSnapshot(q, (snap) => {
                const l = document.getElementById('admin-requests'); l.innerHTML = "";
                snap.forEach(d => {
                    const r = d.data();
                    l.innerHTML += `<div class="adm-card">
                        <p>${r.user} | ${r.type}: $${r.amount}</p>
                        ${r.proof ? `<button onclick="window.open('${r.proof}')" style="background:#333; color:#fff; border:none; padding:5px; border-radius:5px;">View Proof</button>` : ''}
                        <div style="display:flex; gap:10px; margin-top:10px;">
                            <button onclick="adminAction('${d.id}','${r.user}',${r.amount},'${r.type}','Verified')" style="flex:1; background:var(--accent); border:none; padding:10px; border-radius:10px;">APPROVE</button>
                            <button onclick="adminAction('${d.id}','${r.user}',${r.amount},'${r.type}','Rejected')" style="flex:1; background:var(--danger); border:none; padding:10px; border-radius:10px;">REJECT</button>
                        </div>
                    </div>`;
                });
            });
        }

        window.adminAction = async (id, u, amt, t, st) => {
            const r = doc(db, "users", u); const s = await getDoc(r);
            if(st === 'Verified') {
                let nb = t === 'Deposit' ? (s.data().balance + amt) : (s.data().balance - amt);
                await updateDoc(r, { balance: nb });
            }
            await updateDoc(doc(db, "logs", id), { status: st });
            alert("Done!");
        };

        window.secureLogout = () => { sessionStorage.clear(); location.reload(); };

        function startVisualMining() {
            setInterval(() => {
                const el = document.getElementById('bal-main');
                let v = parseFloat(el.innerText.replace('$',''));
                el.innerText = "$" + (v + 0.0001).toFixed(4);
            }, 3000);
        }

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.style.display='none');
            document.getElementById(id).style.display='block';
            if(el) {
                document.querySelectorAll('.nav-link').forEach(l => l.classList.remove('active'));
                el.classList.add('active');
            }
        };

        if(sessionStorage.getItem("nova_u")) boot(sessionStorage.getItem("nova_u"));
    </script>
</body>
</html>
