<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Institutional Mining</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --n-gold: #D4AF37; --n-bg: #F8FAFC; }
        body { background: var(--n-bg); font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; overflow-x: hidden; }
        .nova-card { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 20px; box-shadow: 0 4px 10px rgba(0,0,0,0.03); }
        .btn-gold { background: linear-gradient(135deg, #B8860B, #D4AF37); color: white; border-radius: 12px; font-weight: 700; transition: 0.2s; text-transform: uppercase; font-size: 11px; }
        .screen { display: none; padding-bottom: 110px; animation: fadeIn 0.3s ease-in; }
        .active-screen { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(5px); } to { opacity: 1; transform: translateY(0); } }
        
        /* Trust Toast Animation */
        #trust-toast {
            position: fixed; bottom: 90px; left: 15px; right: 15px;
            background: rgba(15, 23, 42, 0.98); color: white;
            padding: 15px; border-radius: 15px; font-size: 11px;
            display: none; z-index: 2000; border-left: 5px solid #D4AF37;
            box-shadow: 0 15px 30px rgba(0,0,0,0.3);
            animation: slideUp 0.5s ease-out;
        }
        @keyframes slideUp { from { transform: translateY(100px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
        
        .pulse-gold { animation: pulse 2s infinite; }
        @keyframes pulse { 0% { box-shadow: 0 0 0 0 rgba(212, 175, 55, 0.4); } 70% { box-shadow: 0 0 0 15px rgba(212, 175, 55, 0); } 100% { box-shadow: 0 0 0 0 rgba(212, 175, 55, 0); } }
        
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: white; border-top: 1px solid #f1f5f9; display: flex; justify-content: space-around; padding: 15px 5px; z-index: 100; }
        .nav-item { color: #94a3b8; font-size: 10px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: #B8860B; }
        .modal { display: none; position: fixed; inset: 0; background: white; z-index: 1000; padding: 20px; overflow-y: auto; }
        .p-input { background: #f1f5f9; border: 2px solid transparent; width: 100%; padding: 14px; border-radius: 12px; outline: none; margin-bottom: 10px; font-size: 13px; }
    </style>
</head>
<body>

    <div id="trust-toast">
        <div class="flex items-center gap-4">
            <div class="w-8 h-8 bg-green-500/20 rounded-full flex items-center justify-center">
                <i class="fa-solid fa-check text-green-500"></i>
            </div>
            <div>
                <p id="toast-user" class="font-black text-[10px] uppercase text-white"></p>
                <p id="toast-msg" class="text-slate-400 text-[9px]"></p>
            </div>
        </div>
    </div>

    <div id="auth-screen" class="min-h-screen flex flex-col p-8 justify-center active-screen bg-white">
        <div class="mb-10 text-center">
            <h1 onclick="handleTaps()" class="text-5xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[9px] text-gray-400 font-bold uppercase tracking-[5px] mt-2">Institutional Mining Core</p>
        </div>
        <div class="space-y-2">
            <input type="text" id="u-id" placeholder="Access ID" class="p-input">
            <input type="password" id="u-pw" placeholder="Security Passkey" class="p-input">
            <input type="text" id="adm-key" placeholder="Admin Secret" class="hidden p-input border-yellow-500">
            <button onclick="auth()" class="w-full btn-gold py-4 shadow-lg">Login to Node</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[10px] mt-8 text-slate-400 font-bold uppercase cursor-pointer">Register New Account</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white sticky top-0 z-50 border-b">
            <h2 class="font-black text-2xl tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="flex items-center gap-4">
                <i onclick="logout()" class="fa-solid fa-power-off text-slate-300"></i>
            </div>
        </header>

        <div id="dash-screen" class="screen active-screen px-4 pt-4">
            <div class="flex gap-2 mb-4 overflow-x-auto no-scrollbar">
                <div class="bg-white px-3 py-1.5 rounded-full border border-slate-100 flex items-center gap-2 shrink-0">
                    <i class="fa-solid fa-shield-check text-[#D4AF37] text-[10px]"></i>
                    <span class="text-[8px] font-bold text-slate-500">SECURE-NODE</span>
                </div>
                <div class="bg-white px-3 py-1.5 rounded-full border border-slate-100 flex items-center gap-2 shrink-0">
                    <i class="fa-solid fa-certificate text-[#D4AF37] text-[10px]"></i>
                    <span class="text-[8px] font-bold text-slate-500">REG-CERTIFIED</span>
                </div>
            </div>

            <div class="nova-card p-8 mb-6 relative overflow-hidden shadow-2xl border-b-8 border-[#D4AF37]" style="background-color: #0f172a !important; color: white !important;">
                <div class="absolute -right-10 -top-10 w-40 h-40 bg-[#D4AF37]/10 rounded-full blur-3xl"></div>
                <div class="relative z-10 text-center">
                    <div class="flex justify-center items-center gap-2 mb-2">
                        <span class="w-2 h-2 bg-green-500 rounded-full animate-ping"></span>
                        <p class="text-[10px] text-slate-400 font-bold uppercase tracking-[4px]">Mining Active</p>
                    </div>
                    <h1 id="main-bal" class="text-6xl font-black tracking-tighter mb-8" style="color: white !important;">$0.00</h1>
                    <div class="grid grid-cols-2 gap-6 border-t border-white/10 pt-6">
                        <div class="text-left">
                            <p class="text-[9px] text-slate-400 font-bold uppercase mb-1">Daily Yield</p>
                            <p id="v-daily" class="text-base font-black text-green-400">+$0.00</p>
                        </div>
                        <div class="text-right">
                            <p class="text-[9px] text-slate-400 font-bold uppercase mb-1">Profit</p>
                            <p id="total-profit" class="text-base font-black text-[#D4AF37]">$0.00</p>
                        </div>
                    </div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('dep-modal')" class="nova-card p-5 flex flex-col items-center gap-3">
                    <i class="fa-solid fa-plus-circle text-2xl text-[#D4AF37]"></i>
                    <span class="text-[10px] font-black uppercase">Deposit</span>
                </button>
                <button onclick="openModal('wd-modal')" class="nova-card p-5 flex flex-col items-center gap-3">
                    <i class="fa-solid fa-money-bill-transfer text-2xl text-slate-400"></i>
                    <span class="text-[10px] font-black uppercase">Withdraw</span>
                </button>
            </div>

            <div class="nova-card p-6 mb-6">
                <h3 class="text-[10px] font-black uppercase mb-4 tracking-widest text-slate-400">Legal & Transparency</h3>
                <div class="space-y-3">
                    <div onclick="alert('Viewing Official Terms of Service')" class="flex justify-between items-center p-3 bg-slate-50 rounded-xl border border-slate-100">
                        <div class="flex items-center gap-3">
                            <i class="fa-solid fa-file-contract text-slate-400"></i>
                            <span class="text-xs font-bold">Terms of Service</span>
                        </div>
                        <i class="fa-solid fa-chevron-right text-[10px] text-slate-300"></i>
                    </div>
                    <div onclick="alert('Whitepaper v2.0 - Loaded')" class="flex justify-between items-center p-3 bg-slate-50 rounded-xl border border-slate-100">
                        <div class="flex items-center gap-3">
                            <i class="fa-solid fa-book-open text-slate-400"></i>
                            <span class="text-xs font-bold">Nova Whitepaper</span>
                        </div>
                        <i class="fa-solid fa-chevron-right text-[10px] text-slate-300"></i>
                    </div>
                </div>
            </div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('dash-screen')" class="nav-item active" id="nav-dash"><i class="fa-solid fa-microchip text-lg"></i><span>DASH</span></div>
            <div onclick="nav('mine-screen')" class="nav-item" id="nav-mine"><i class="fa-solid fa-server text-lg"></i><span>NODES</span></div>
            <div onclick="nav('hist-screen')" class="nav-item" id="nav-hist"><i class="fa-solid fa-receipt text-lg"></i><span>HISTORY</span></div>
        </nav>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const config = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(config); const db = getFirestore(app);
        let user = localStorage.getItem('nova_user'); let login = true;

        if(user) start(user);

        window.auth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return alert("Required!");
            const uRef = doc(db, "users", id); const s = await getDoc(uRef);
            if(!login) {
                if(s.exists()) return alert("Exists!");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total_profit: 0 });
                localStorage.setItem('nova_user', id); start(id);
            } else if(s.exists() && s.data().password === pw) {
                localStorage.setItem('nova_user', id); start(id);
            } else alert("Error!");
        };

        function start(id) {
            user = id; document.getElementById('auth-screen').style.display='none'; document.getElementById('app').classList.remove('hidden');
            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                document.getElementById('main-bal').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "+$" + (data.daily || 0).toFixed(2);
                document.getElementById('total-profit').innerText = "$" + (data.total_profit || 0).toFixed(2);
            });
        }

        // Trust Notification Logic
        function triggerToast() {
            const names = ["Zain", "Fatima", "Ali", "Sara", "Umer", "Khan", "Malik", "Ayesha"];
            const amounts = [15.20, 45.00, 110.00, 5.50, 320.00, 8.40, 66.00];
            const name = names[Math.floor(Math.random()*names.length)];
            const amt = amounts[Math.floor(Math.random()*amounts.length)];
            
            document.getElementById('toast-user').innerText = `User @${name}***`;
            document.getElementById('toast-msg').innerText = `Withdrawal successful: $${amt.toFixed(2)}`;
            
            const toast = document.getElementById('trust-toast');
            toast.style.display = 'block';
            setTimeout(() => { toast.style.display = 'none'; }, 4000);
        }
        setInterval(triggerToast, 20000); // 20 seconds baad dikhao

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
