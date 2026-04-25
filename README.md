<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | ULTRA TERMINAL</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0F172A; }
        body { background: #F8FAFC; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; -webkit-tap-highlight-color: transparent; }
        .premium-card { background: white; border: 1px solid #e2e8f0; border-radius: 28px; box-shadow: 0 4px 20px rgba(0,0,0,0.01); overflow: hidden; }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 18px; font-weight: 800; cursor: pointer; border: none; transition: 0.3s; }
        .screen { display: none; min-height: 100vh; width: 100%; padding-bottom: 120px; }
        .active-screen { display: block; animation: slideUp 0.4s ease; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255,255,255,0.95); backdrop-filter: blur(15px); border-top: 1px solid #e2e8f0; display: flex; justify-content: space-around; padding: 12px 0; z-index: 1000; border-radius: 25px 25px 0 0; }
        .nav-item { color: #94a3b8; font-size: 8px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: var(--gold); }
        .ticker-wrap { background: #0F172A; color: #D4AF37; padding: 6px 0; font-size: 10px; font-weight: 800; overflow: hidden; }
        .ticker-move { display: inline-block; white-space: nowrap; animation: ticker 20s linear infinite; }
        @keyframes ticker { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }
    </style>
</head>
<body>

    <div id="auth-screen" class="active-screen min-h-screen bg-white flex flex-col justify-center p-8">
        <div class="text-center mb-8">
            <h1 class="text-5xl font-black italic tracking-tighter">NO<span class="text-gold">VA</span></h1>
            <p class="text-[10px] font-black uppercase text-slate-400 tracking-[0.3em] mt-2">Ultra Final Core</p>
        </div>
        <div class="space-y-4 max-w-xs mx-auto w-full">
            <input type="text" id="u-id" placeholder="Username" class="w-full p-5 rounded-2xl border bg-slate-50 outline-none font-bold">
            <input type="password" id="u-pw" placeholder="Password" class="w-full p-5 rounded-2xl border bg-slate-50 outline-none font-bold">
            <input type="text" id="u-ref" placeholder="Referral Code" class="w-full p-5 rounded-2xl border bg-slate-50 outline-none font-bold text-xs">
            <button onclick="handleAuth()" class="w-full btn-gold py-5 uppercase text-xs tracking-widest shadow-xl">Initialize Terminal</button>
            <p onclick="toggleAuth()" id="auth-btn-text" class="text-[10px] text-center text-slate-400 font-black uppercase cursor-pointer underline">Create Account</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <div class="ticker-wrap"><div class="ticker-move" id="live-ticker">Welcome to Nova Ultra Core • Global Servers Online • Deploying V21 Nodes...</div></div>
        
        <header class="p-5 flex justify-between items-center bg-white border-b sticky top-0 z-[100]">
            <h2 id="logo-main" class="font-black text-2xl italic tracking-tighter cursor-pointer">NO<span class="text-gold">VA</span></h2>
            <div class="text-right">
                <p id="user-tag" class="text-[9px] font-black uppercase text-slate-400">@USER</p>
                <p id="b-header" class="text-lg font-black text-slate-950">$0.00</p>
            </div>
        </header>

        <div id="scr-dash" class="screen active-screen px-4 pt-6">
            <div class="bg-slate-950 rounded-[35px] p-8 text-white mb-6 relative shadow-2xl overflow-hidden border-b-4 border-gold">
                <p class="text-[8px] text-slate-500 uppercase font-black mb-2">Total Assets</p>
                <h1 id="b-main" class="text-5xl font-black mb-10 tracking-tighter">$0.00</h1>
                <div class="flex justify-between border-t border-white/10 pt-6">
                    <div><p class="text-[7px] text-slate-500 uppercase font-black">Daily Mining</p><p id="b-daily" class="text-xl font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[7px] text-slate-500 uppercase font-black">Ref Earnings</p><p id="b-team" class="text-xl font-black text-gold">$0.00</p></div>
                </div>
            </div>

            <div class="grid grid-cols-3 gap-3 mb-6 text-center">
                <div onclick="openModal('mod-dep')" class="premium-card p-4"><i class="fa-solid fa-bolt text-gold mb-2"></i><p class="text-[8px] font-black uppercase">Deposit</p></div>
                <div onclick="openModal('mod-wd')" class="premium-card p-4"><i class="fa-solid fa-wallet text-slate-300 mb-2"></i><p class="text-[8px] font-black uppercase">Withdraw</p></div>
                <div onclick="claimDaily()" class="premium-card p-4"><i class="fa-solid fa-gift text-red-400 mb-2"></i><p class="text-[8px] font-black uppercase">Daily Reward</p></div>
            </div>

            <div class="premium-card p-5 mb-4 flex gap-2">
                <input type="text" id="promo-input" placeholder="Promo Code" class="flex-1 bg-slate-50 p-3 rounded-xl border-none outline-none font-bold text-xs uppercase">
                <button onclick="applyPromo()" class="btn-gold px-5 text-[9px] uppercase">Redeem</button>
            </div>

            <div class="premium-card p-6 border-dashed border-2 border-slate-200">
                <div class="flex items-center justify-between">
                    <div><h4 class="text-[9px] font-black text-slate-400 uppercase">Your Network Link</h4><p id="ref-link" class="text-[10px] font-bold text-slate-600 mt-1">nova.com/?ref=user</p></div>
                    <i onclick="copyRef()" class="fa-solid fa-copy text-gold text-lg cursor-pointer"></i>
                </div>
            </div>
        </div>

        <div id="scr-nodes" class="screen px-4 pt-6">
            <h3 class="font-black text-2xl mb-8 italic px-2">Mining Nodes</h3>
            <div id="nodes-grid" class="space-y-6"></div>
        </div>

        <div id="scr-team" class="screen px-4 pt-6">
            <h3 class="font-black text-2xl mb-8 italic px-2">Partnerships</h3>
            <div class="grid grid-cols-2 gap-4 mb-8">
                <div class="premium-card p-6 text-center bg-slate-900 text-white"><h5 id="team-count" class="text-3xl font-black">0</h5><p class="text-[7px] font-black uppercase text-gold">Direct Lvl 1</p></div>
                <div class="premium-card p-6 text-center"><h5 id="team-comm" class="text-3xl font-black text-gold">$0</h5><p class="text-[7px] font-black uppercase text-slate-400">Total Profit</p></div>
            </div>
            <div id="team-list" class="space-y-3 px-2"></div>
        </div>

        <div id="scr-logs" class="screen px-4 pt-6">
            <h3 class="font-black text-2xl mb-8 italic px-2">Transactions</h3>
            <div id="logs-list" class="space-y-4"></div>
        </div>

        <div id="scr-admin" class="screen px-4 pt-6">
            <h2 class="text-2xl font-black mb-8 text-red-600 italic px-2">Master Config</h2>
            <div class="premium-card p-6 bg-slate-900 text-white mb-6">
                <input type="text" id="adm-ep" placeholder="EasyPaisa" class="w-full p-3 rounded-xl text-black mb-2 font-bold">
                <input type="text" id="adm-jc" placeholder="JazzCash" class="w-full p-3 rounded-xl text-black mb-4 font-bold">
                <input type="number" id="adm-tax" placeholder="Withdraw Tax %" class="w-full p-3 rounded-xl text-black mb-4 font-bold">
                <button onclick="saveAdminSettings()" class="w-full btn-gold py-3 text-[10px] uppercase">Update Global Settings</button>
            </div>
            <div id="adm-pending" class="space-y-4"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('scr-dash', this)" class="nav-item active"><i class="fa-solid fa-home text-xl"></i><span>HOME</span></div>
            <div onclick="nav('scr-nodes', this)" class="nav-item"><i class="fa-solid fa-server text-xl"></i><span>NODES</span></div>
            <div onclick="nav('scr-team', this)" class="nav-item"><i class="fa-solid fa-user-group text-xl"></i><span>TEAM</span></div>
            <div onclick="nav('scr-logs', this)" class="nav-item"><i class="fa-solid fa-clock-rotate-left text-xl"></i><span>LOGS</span></div>
            <div id="nav-adm" onclick="nav('scr-admin', this)" class="nav-item text-red-600" style="display:none;"><i class="fa-solid fa-fingerprint text-xl"></i><span>ADMIN</span></div>
        </nav>
    </div>

    <div id="mod-dep" class="fixed inset-0 bg-white z-[2000] p-8 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-10"><h2 class="font-black text-2xl uppercase italic">Deposit</h2><i onclick="closeModal('mod-dep')" class="fa-solid fa-times text-3xl text-slate-200"></i></div>
        <div class="grid grid-cols-2 gap-3 mb-8">
            <button onclick="showWay('ep')" class="border-2 p-4 rounded-2xl font-black text-[10px]">EasyPaisa</button>
            <button onclick="showWay('jc')" class="border-2 p-4 rounded-2xl font-black text-[10px]">JazzCash</button>
        </div>
        <div id="way-info" class="bg-slate-950 text-gold p-8 rounded-[30px] mb-8 hidden text-center shadow-2xl border-b-4 border-gold">
            <p id="way-num" class="text-xl font-black tracking-widest">0000000000</p>
        </div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="w-full p-5 bg-slate-50 rounded-2xl mb-4 font-bold">
        <input type="text" id="dep-tx" placeholder="TRX ID" class="w-full p-5 bg-slate-50 rounded-2xl mb-10 font-bold uppercase">
        <button onclick="handleFinance('Deposit')" class="w-full btn-gold py-6 uppercase tracking-widest">Submit Request</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, deleteDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const fbCfg = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(fbCfg); const db = getFirestore(app);

        let user = localStorage.getItem('nova_user');
        let mode = 'login'; let taps = 0;
        let config = { ep: "03000000000", jc: "03000000000", tax: 0 };

        const nodes = [];
        for(let i=1; i<=21; i++) { nodes.push({ id: i, n: `NOVA-ULTRA V${i}`, c: 10 + (i * 20), y: 1 + (i * 1.8) }); }

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            const ref = document.getElementById('u-ref').value.toLowerCase().trim() || 'none';
            if(!id || !pw) return alert("Required fields missing");
            
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(mode === 'login') {
                if(snap.exists() && snap.data().pw === pw) {
                    if(snap.data().status === 'Banned') return alert("Account Banned!");
                    localStorage.setItem('nova_user', id); location.reload();
                } else alert("Invalid Access");
            } else {
                if(snap.exists()) return alert("Username Taken");
                await setDoc(uRef, { bal: 0, pw: pw, daily: 0, team_bal: 0, refBy: ref, active: [], lastClaim: 0, status: 'Active' });
                localStorage.setItem('nova_user', id); location.reload();
            }
        };

        function initApp(id) {
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('ref-link').innerText = `${window.location.origin}/?ref=${id}`;

            onSnapshot(doc(db, "users", id), d => {
                const u = d.data(); if(!u || u.status === 'Banned') { localStorage.clear(); location.reload(); }
                document.getElementById('user-tag').innerText = `@${id}`;
                document.getElementById('b-main').innerText = `$${u.bal.toFixed(2)}`;
                document.getElementById('b-header').innerText = `$${u.bal.toFixed(2)}`;
                document.getElementById('b-daily').innerText = `+$${u.daily.toFixed(2)}`;
                document.getElementById('b-team').innerText = `$${(u.team_bal || 0).toFixed(2)}`;
                renderNodes(u.active || []);
            });

            onSnapshot(query(collection(db, "users"), where("refBy", "==", id)), s => {
                document.getElementById('team-count').innerText = s.size;
                const list = document.getElementById('team-list'); list.innerHTML = "";
                s.forEach(doc => {
                    list.innerHTML += `<div class="premium-card p-4 flex justify-between items-center text-[10px]"><b>@${doc.id}</b><span class="text-green-500 font-black">LEVEL 1</span></div>`;
                });
            });

            onSnapshot(query(collection(db, "logs"), where("user", "==", id)), s => {
                const list = document.getElementById('logs-list'); list.innerHTML = "";
                s.forEach(d => {
                    const l = d.data();
                    list.innerHTML += `<div class="premium-card p-5 flex justify-between items-center">
                        <div><p class="text-[9px] font-black text-slate-400 uppercase">${l.type}</p><p class="text-xs font-bold">${l.ref}</p></div>
                        <div class="text-right"><p class="font-black">$${l.amount.toFixed(2)}</p><span class="text-[8px] font-black text-gold">${l.status}</span></div>
                    </div>`;
                });
            });

            onSnapshot(doc(db, "config", "master"), d => { if(d.exists()) config = d.data(); });
        }

        function renderNodes(active) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            nodes.forEach(n => {
                const isRun = active.includes(n.id);
                grid.innerHTML += `<div class="premium-card p-6 flex justify-between items-center">
                    <div><h4 class="font-black text-xs uppercase italic">${n.n}</h4><p class="text-[9px] font-bold text-green-500">Yield: $${n.y.toFixed(2)}/day</p></div>
                    ${isRun ? `<span class="text-green-500 font-black text-[9px] animate-pulse">MINING ●</span>` : `<button onclick="buyNode(${n.id}, ${n.c}, ${n.y})" class="btn-gold px-6 py-3 text-[9px] uppercase">$${n.c}</button>`}
                </div>`;
            });
        }

        window.claimDaily = async () => {
            const uRef = doc(db, "users", user); const s = await getDoc(uRef);
            if(Date.now() - (s.data().lastClaim || 0) < 86400000) return alert("Available every 24h");
            await updateDoc(uRef, { bal: s.data().bal + 0.10, lastClaim: Date.now() });
            alert("Reward Claimed!");
        };

        window.buyNode = async (id, c, y) => {
            const uRef = doc(db, "users", user); const s = await getDoc(uRef);
            if(s.data().bal < c) return alert("Insufficient Assets");
            await updateDoc(uRef, { bal: s.data().bal - c, active: [...s.data().active, id], daily: s.data().daily + y });
            
            if(s.data().refBy && s.data().refBy !== 'none') {
                const rRef = doc(db, "users", s.data().refBy); const rs = await getDoc(rRef);
                if(rs.exists()) await updateDoc(rRef, { bal: rs.data().bal + (c*0.1), team_bal: (rs.data().team_bal || 0) + (c*0.1) });
            }
            alert("Mining Started!");
        };

        window.handleFinance = async (type) => {
            const amt = parseFloat(document.getElementById('dep-amt').value);
            const ref = document.getElementById('dep-tx').value;
            if(!amt || !ref) return alert("Enter details");
            await addDoc(collection(db, "logs"), { user, type, amount: amt, ref, status: 'Pending' });
            alert("Request Sent!"); closeModal('mod-dep');
        };

        document.getElementById('logo-main').onclick = () => {
            taps++; if(taps >= 5) {
                if(prompt("System Key:") === "nov786") { document.getElementById('nav-adm').style.display="flex"; loadAdm(); }
                taps = 0;
            }
        };

        function loadAdm() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const list = document.getElementById('adm-pending'); list.innerHTML = "";
                s.forEach(d => {
                    list.innerHTML += `<div class="premium-card p-5 text-[10px] flex justify-between items-center">
                        <div><b>${d.data().user}</b> ($${d.data().amount})<br>${d.data().ref}</div>
                        <div class="flex gap-2">
                            <button onclick="approve('${d.id}', '${d.data().user}', ${d.data().amount})" class="bg-green-500 text-white px-4 py-2 rounded-xl">OK</button>
                            <button onclick="banU('${d.data().user}')" class="bg-red-600 text-white px-4 py-2 rounded-xl">BAN</button>
                        </div>
                    </div>`;
                });
            });
        }

        window.approve = async (lid, uid, amt) => {
            const uRef = doc(db, "users", uid); const us = await getDoc(uRef);
            await updateDoc(uRef, { bal: us.data().bal + amt });
            await updateDoc(doc(db, "logs", lid), { status: 'Approved' }); alert("Success!");
        };

        window.banU = async (uid) => { if(confirm(`Ban ${uid}?`)) await updateDoc(doc(db, "users", uid), { status: 'Banned' }); };
        
        window.saveAdminSettings = async () => {
            const up = { ep: document.getElementById('adm-ep').value, jc: document.getElementById('adm-jc').value, tax: parseFloat(document.getElementById('adm-tax').value) };
            await setDoc(doc(db, "config", "master"), up, { merge: true }); alert("Config Saved!");
        };

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); el.classList.add('active');
        };
        window.showWay = (t) => { document.getElementById('way-info').classList.remove('hidden'); document.getElementById('way-num').innerText = config[t]; };
        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.toggleAuth = () => { mode = mode === 'login' ? 'reg' : 'login'; document.getElementById('auth-btn-text').innerText = mode === 'login' ? "Create Account" : "Login Instead"; };
        window.copyRef = () => { navigator.clipboard.writeText(document.getElementById('ref-link').innerText); alert("Referral Copied!"); };

        if(user) initApp(user);
    </script>
</body>
</html>
