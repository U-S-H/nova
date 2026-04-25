<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | PRO TERMINAL</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0F172A; }
        body { background: #F8FAFC; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; -webkit-tap-highlight-color: transparent; }
        .premium-card { background: white; border: 1px solid #e2e8f0; border-radius: 28px; box-shadow: 0 4px 20px rgba(0,0,0,0.02); overflow: hidden; }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 18px; font-weight: 800; cursor: pointer; border: none; transition: 0.3s; }
        .screen { display: none; min-height: 100vh; width: 100%; padding-bottom: 120px; }
        .active-screen { display: block; animation: slideUp 0.4s ease; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255,255,255,0.95); backdrop-filter: blur(15px); border-top: 1px solid #e2e8f0; display: flex; justify-content: space-around; padding: 12px 0; z-index: 1000; border-radius: 25px 25px 0 0; }
        .nav-item { color: #94a3b8; font-size: 8px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: var(--gold); }
        .node-img { width: 100%; height: 160px; object-fit: cover; }
        .progress-bar { height: 4px; background: #f1f5f9; border-radius: 10px; overflow: hidden; }
        .progress-fill { height: 100%; background: var(--gold); width: 0%; transition: 1s linear; }
    </style>
</head>
<body>

    <div id="auth-screen" class="active-screen min-h-screen bg-white flex flex-col justify-center p-8">
        <div class="text-center mb-10">
            <h1 class="text-5xl font-black italic tracking-tighter">NO<span class="text-gold">VA</span></h1>
            <p class="text-[10px] font-black uppercase text-slate-400 tracking-[0.2em] mt-2">Industrial Core</p>
        </div>
        <div class="space-y-4 max-w-xs mx-auto w-full">
            <input type="text" id="u-id" placeholder="Username" class="w-full p-5 rounded-2xl border bg-slate-50 outline-none font-bold">
            <input type="password" id="u-pw" placeholder="Password" class="w-full p-5 rounded-2xl border bg-slate-50 outline-none font-bold">
            <input type="text" id="u-ref" placeholder="Referral Code (Optional)" class="w-full p-5 rounded-2xl border bg-slate-50 outline-none font-bold text-xs">
            <button onclick="handleAuth()" class="w-full btn-gold py-5 uppercase text-xs tracking-widest shadow-xl shadow-gold/20">Initialize</button>
            <p onclick="toggleAuth()" id="auth-btn-text" class="text-[10px] text-center text-slate-400 font-black uppercase cursor-pointer underline">Create Account</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white border-b sticky top-0 z-[100]">
            <h2 id="logo-main" class="font-black text-2xl italic tracking-tighter cursor-pointer">NO<span class="text-gold">VA</span></h2>
            <div class="text-right">
                <p id="user-tag" class="text-[9px] font-black uppercase text-slate-400 tracking-widest">@USER</p>
                <p id="b-header" class="text-lg font-black text-slate-950">$0.00</p>
            </div>
        </header>

        <div id="scr-dash" class="screen active-screen px-4 pt-6">
            <div class="bg-slate-950 rounded-[35px] p-8 text-white mb-6 relative shadow-2xl overflow-hidden">
                <div class="absolute -right-10 -top-10 w-40 h-40 bg-gold/10 rounded-full blur-3xl"></div>
                <p class="text-[8px] text-slate-500 uppercase font-black tracking-widest mb-2">Total Balance</p>
                <h1 id="b-main" class="text-5xl font-black mb-10 tracking-tighter">$0.00</h1>
                <div class="flex justify-between border-t border-white/10 pt-6">
                    <div><p class="text-[7px] text-slate-500 uppercase font-black">Daily Profit</p><p id="b-daily" class="text-xl font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[7px] text-slate-500 uppercase font-black">Team Income</p><p id="b-team" class="text-xl font-black text-gold">$0.00</p></div>
                </div>
            </div>

            <div class="grid grid-cols-3 gap-3 mb-6 text-center">
                <div onclick="openModal('mod-dep')" class="premium-card p-4"><i class="fa-solid fa-plus-circle text-gold mb-2"></i><p class="text-[8px] font-black uppercase">Deposit</p></div>
                <div onclick="openModal('mod-wd')" class="premium-card p-4"><i class="fa-solid fa-wallet text-slate-400 mb-2"></i><p class="text-[8px] font-black uppercase">Withdraw</p></div>
                <div onclick="claimDaily()" class="premium-card p-4"><i class="fa-solid fa-gift text-red-400 mb-2"></i><p class="text-[8px] font-black uppercase">Daily</p></div>
            </div>

            <div class="premium-card p-5 mb-6 flex gap-2">
                <input type="text" id="promo-input" placeholder="Promo Code" class="flex-1 bg-slate-50 p-3 rounded-xl border-none outline-none font-bold text-xs uppercase">
                <button onclick="applyPromo()" class="btn-gold px-5 text-[9px] uppercase">Apply</button>
            </div>

            <div class="premium-card p-6 border-dashed border-2 border-slate-200">
                <h4 class="text-[9px] font-black text-slate-400 uppercase mb-3">Your Referral Link</h4>
                <div class="flex items-center justify-between bg-slate-50 p-3 rounded-xl">
                    <span id="ref-link" class="text-[10px] font-bold text-slate-500 overflow-hidden text-ellipsis">nova.com/?ref=user</span>
                    <i onclick="copyRef()" class="fa-solid fa-copy text-gold cursor-pointer ml-4"></i>
                </div>
            </div>
        </div>

        <div id="scr-nodes" class="screen px-4 pt-6">
            <h3 class="font-black text-2xl mb-8 italic uppercase px-2">Industrial Nodes</h3>
            <div id="nodes-grid" class="space-y-6"></div>
        </div>

        <div id="scr-team" class="screen px-4 pt-6">
            <h3 class="font-black text-2xl mb-8 italic uppercase px-2">My Network</h3>
            <div class="grid grid-cols-2 gap-4 mb-8">
                <div class="premium-card p-6 text-center"><h5 id="team-count" class="text-3xl font-black">0</h5><p class="text-[8px] font-black text-slate-400 uppercase">Total Members</p></div>
                <div class="premium-card p-6 text-center"><h5 id="team-comm" class="text-3xl font-black text-gold">$0</h5><p class="text-[8px] font-black text-slate-400 uppercase">Comm. Earned</p></div>
            </div>
            <div id="team-list" class="space-y-3"></div>
        </div>

        <div id="scr-logs" class="screen px-4 pt-6">
            <h3 class="font-black text-2xl mb-8 italic uppercase px-2">Audit Logs</h3>
            <div id="logs-list" class="space-y-4"></div>
        </div>

        <div id="scr-admin" class="screen px-4 pt-6">
            <h2 class="text-2xl font-black mb-8 text-red-600 uppercase italic px-2">Master Core</h2>
            <div class="premium-card p-6 bg-slate-900 text-white mb-6">
                <h4 class="text-[10px] font-black text-slate-500 uppercase mb-4 tracking-widest">Create Promo Code</h4>
                <input type="text" id="adm-p-code" placeholder="CODE" class="w-full p-3 rounded-xl text-black mb-2 font-bold uppercase">
                <input type="number" id="adm-p-val" placeholder="Value ($)" class="w-full p-3 rounded-xl text-black mb-4 font-bold">
                <button onclick="addPromoCode()" class="w-full btn-gold py-3 text-[10px] uppercase">Create Code</button>
            </div>
            <div id="adm-pending" class="space-y-4"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('scr-dash', this)" class="nav-item active"><i class="fa-solid fa-house-chimney text-xl"></i><span>HOME</span></div>
            <div onclick="nav('scr-nodes', this)" class="nav-item"><i class="fa-solid fa-microchip text-xl"></i><span>NODES</span></div>
            <div onclick="nav('scr-team', this)" class="nav-item"><i class="fa-solid fa-users text-xl"></i><span>TEAM</span></div>
            <div id="nav-hist" onclick="nav('scr-logs', this)" class="nav-item"><i class="fa-solid fa-receipt text-xl"></i><span>LOGS</span></div>
            <div id="nav-adm" onclick="nav('scr-admin', this)" class="nav-item text-red-600" style="display:none;"><i class="fa-solid fa-shield text-xl"></i><span>ADMIN</span></div>
        </nav>
    </div>

    <div id="mod-dep" class="fixed inset-0 bg-white z-[2000] p-8 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-10"><h2 class="font-black text-2xl uppercase">Deposit</h2><i onclick="closeModal('mod-dep')" class="fa-solid fa-xmark text-3xl text-slate-200"></i></div>
        <p class="text-xs font-bold mb-4 text-slate-400 uppercase tracking-widest">Select Gateway</p>
        <div class="grid grid-cols-2 gap-3 mb-8">
            <button onclick="showWay('EasyPaisa')" class="border-2 p-4 rounded-2xl font-black text-[10px]">EasyPaisa</button>
            <button onclick="showWay('JazzCash')" class="border-2 p-4 rounded-2xl font-black text-[10px]">JazzCash</button>
        </div>
        <div id="way-info" class="bg-slate-900 text-gold p-6 rounded-2xl mb-8 hidden text-center">
            <p id="way-name" class="text-[9px] uppercase font-black mb-1"></p>
            <p id="way-num" class="text-xl font-black tracking-widest">03000000000</p>
        </div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="w-full p-5 bg-slate-50 rounded-2xl mb-4 font-bold">
        <input type="text" id="dep-tx" placeholder="Transaction ID" class="w-full p-5 bg-slate-50 rounded-2xl mb-10 font-bold">
        <button onclick="handleFinance('Deposit')" class="w-full btn-gold py-6 uppercase text-[10px] tracking-widest">Confirm Request</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, deleteDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const fbCfg = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(fbCfg); const db = getFirestore(app);

        let user = localStorage.getItem('nova_user');
        let authMode = 'login'; let taps = 0;
        let sys = { ep: "03279177196", jc: "03279177196", refComm: 10 };

        // 21 NODES
        const nodes = [];
        for(let i=1; i<=21; i++) {
            nodes.push({ id: i, n: `NOVA CORE V${i}`, c: 10+(i*15), y: 1+(i*1.5), img: `https://images.unsplash.com/photo-1639322537228-f710d846310a?w=400` });
        }

        // AUTH
        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            const refBy = document.getElementById('u-ref').value.toLowerCase().trim();
            if(!id || !pw) return alert("Fill all fields");
            
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(authMode === 'login') {
                if(snap.exists() && snap.data().pw === pw) { localStorage.setItem('nova_user', id); location.reload(); }
                else alert("Access Denied");
            } else {
                if(snap.exists()) return alert("User exists");
                await setDoc(uRef, { bal: 0, pw: pw, daily: 0, team_bal: 0, refBy: refBy || 'none', active: [], lastClaim: 0 });
                localStorage.setItem('nova_user', id); location.reload();
            }
        };

        // DASHBOARD ENGINE
        function initApp(id) {
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('ref-link').innerText = `${window.location.origin}/?ref=${id}`;

            onSnapshot(doc(db, "users", id), d => {
                const u = d.data(); if(!u) return;
                document.getElementById('user-tag').innerText = `@${id}`;
                document.getElementById('b-main').innerText = `$${u.bal.toFixed(2)}`;
                document.getElementById('b-header').innerText = `$${u.bal.toFixed(2)}`;
                document.getElementById('b-daily').innerText = `+$${u.daily.toFixed(2)}`;
                document.getElementById('b-team').innerText = `$${(u.team_bal || 0).toFixed(2)}`;
                renderNodes(u.active || []);
            });

            // Sync Team
            onSnapshot(query(collection(db, "users"), where("refBy", "==", id)), s => {
                document.getElementById('team-count').innerText = s.size;
                const list = document.getElementById('team-list'); list.innerHTML = "";
                s.forEach(doc => {
                    list.innerHTML += `<div class="premium-card p-4 flex justify-between items-center text-[10px]">
                        <b>@${doc.id}</b><span class="text-green-500 font-black uppercase">Active</span>
                    </div>`;
                });
            });

            // Sync History (Fixed)
            onSnapshot(query(collection(db, "logs"), where("user", "==", id)), s => {
                const list = document.getElementById('logs-list'); list.innerHTML = "";
                s.forEach(d => {
                    const l = d.data();
                    list.innerHTML += `<div class="premium-card p-6 flex justify-between items-center border-l-4 ${l.status==='Approved'?'border-l-green-500':'border-l-gold'}">
                        <div><p class="text-[9px] font-black text-slate-400 uppercase mb-1">${l.type}</p><p class="text-xs font-bold">${l.ref}</p></div>
                        <div class="text-right"><p class="font-black text-sm">$${l.amount}</p><span class="text-[8px] uppercase font-black ${l.status==='Approved'?'text-green-500':'text-gold'}">${l.status}</span></div>
                    </div>`;
                });
            });
        }

        function renderNodes(active) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            nodes.forEach(n => {
                const isRun = active.includes(n.id);
                grid.innerHTML += `<div class="premium-card">
                    <img src="${n.img}" class="node-img">
                    <div class="p-6">
                        <div class="flex justify-between items-center mb-4">
                            <div><h4 class="font-black text-xs uppercase italic">${n.n}</h4><p class="text-[9px] font-bold text-green-500">Yield: $${n.y.toFixed(2)}/day</p></div>
                            ${isRun ? `<span class="text-green-500 font-black text-[9px]">● RUNNING</span>` : `<button onclick="buyNode(${n.id}, ${n.c}, ${n.y})" class="btn-gold px-6 py-3 text-[9px] uppercase">$${n.c}</button>`}
                        </div>
                        ${isRun ? `<div class="progress-bar"><div class="progress-fill" style="width: 100%; transition: width 86400s linear;"></div></div>` : ''}
                    </div>
                </div>`;
            });
        }

        window.claimDaily = async () => {
            const uRef = doc(db, "users", user); const s = await getDoc(uRef);
            const now = Date.now();
            if(now - (s.data().lastClaim || 0) < 86400000) return alert("Come back tomorrow!");
            await updateDoc(uRef, { bal: s.data().bal + 0.10, lastClaim: now });
            alert("Daily Reward: $0.10 Claimed!");
        };

        window.applyPromo = async () => {
            const code = document.getElementById('promo-input').value.toUpperCase();
            const pRef = doc(db, "promo", code); const pSnap = await getDoc(pRef);
            if(!pSnap.exists()) return alert("Invalid Code");
            const uRef = doc(db, "users", user); const uSnap = await getDoc(uRef);
            await updateDoc(uRef, { bal: uSnap.data().bal + pSnap.data().val });
            await deleteDoc(pRef); alert(`Success! $${pSnap.data().val} Added.`);
        };

        window.buyNode = async (id, c, y) => {
            const uRef = doc(db, "users", user); const s = await getDoc(uRef);
            if(s.data().bal < c) return alert("Insufficient Assets");
            await updateDoc(uRef, { bal: s.data().bal - c, active: [...s.data().active, id], daily: s.data().daily + y });
            
            // Ref Commission
            if(s.data().refBy !== 'none') {
                const rRef = doc(db, "users", s.data().refBy); const rs = await getDoc(rRef);
                if(rs.exists()) await updateDoc(rRef, { bal: rs.data().bal + (c * 0.1), team_bal: (rs.data().team_bal || 0) + (c * 0.1) });
            }
            alert("Node Deployed!");
        };

        window.handleFinance = async (type) => {
            const amt = parseFloat(document.getElementById('dep-amt').value);
            const ref = document.getElementById('dep-tx').value;
            if(!amt || !ref) return alert("Fill all fields");
            await addDoc(collection(db, "logs"), { user, type, amount: amt, ref, status: 'Pending' });
            alert("Request Dispatched!"); closeModal('mod-dep');
        };

        // ADMIN ENGINE
        window.addPromoCode = async () => {
            const code = document.getElementById('adm-p-code').value.toUpperCase();
            const val = parseFloat(document.getElementById('adm-p-val').value);
            await setDoc(doc(db, "promo", code), { val }); alert("Code Live!");
        };

        document.getElementById('logo-main').onclick = () => {
            taps++; if(taps >= 5) {
                if(prompt("Key:") === "nov786") { document.getElementById('nav-adm').style.display="flex"; loadAdm(); }
                taps = 0;
            }
        };

        function loadAdm() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const list = document.getElementById('adm-pending'); list.innerHTML = "";
                s.forEach(d => {
                    list.innerHTML += `<div class="premium-card p-5 text-[10px] flex justify-between items-center bg-white">
                        <div><b>${d.data().user}</b> ($${d.data().amount})<br>${d.data().ref}</div>
                        <button onclick="approve('${d.id}', '${d.data().user}', ${d.data().amount}, '${d.data().type}')" class="bg-green-500 text-white px-5 py-2 rounded-xl font-black">OK</button>
                    </div>`;
                });
            });
        }

        window.approve = async (lid, uid, amt, type) => {
            const uRef = doc(db, "users", uid); const us = await getDoc(uRef);
            if(type === 'Deposit') await updateDoc(uRef, { bal: us.data().bal + amt });
            await updateDoc(doc(db, "logs", lid), { status: 'Approved' }); alert("Done");
        };

        // UI HELPERS
        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); el.classList.add('active');
        };
        window.showWay = (t) => { document.getElementById('way-info').classList.remove('hidden'); document.getElementById('way-name').innerText = t; document.getElementById('way-num').innerText = sys[t === 'EasyPaisa' ? 'ep' : 'jc']; };
        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.toggleAuth = () => { authMode = authMode === 'login' ? 'reg' : 'login'; document.getElementById('auth-btn-text').innerText = authMode === 'login' ? "Create Account" : "Login Instead"; };
        window.copyRef = () => { navigator.clipboard.writeText(document.getElementById('ref-link').innerText); alert("Copied!"); };

        if(user) initApp(user);
    </script>
</body>
</html>
