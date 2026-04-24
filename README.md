<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Enterprise Quantum Mining</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0f172a; }
        body { background: #f8fafc; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; overflow-x: hidden; }
        .nova-card { background: white; border: 1px solid #e2e8f0; border-radius: 24px; box-shadow: 0 10px 25px rgba(0,0,0,0.03); }
        .dark-card { background: var(--dark); color: white; border-radius: 28px; position: relative; overflow: hidden; }
        .btn-gold { background: linear-gradient(135deg, #B8860B, #D4AF37); color: white; border-radius: 16px; font-weight: 800; text-transform: uppercase; letter-spacing: 1px; }
        .screen { display: none; animation: slideUp 0.4s ease-out; padding-bottom: 110px; }
        .active-screen { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: white; border-top: 1px solid #f1f5f9; display: flex; justify-content: space-around; padding: 15px; z-index: 1000; }
        .nav-item { color: #94a3b8; font-size: 10px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 5px; }
        .nav-item.active { color: var(--gold); }
        .modal { display: none; position: fixed; inset: 0; background: white; z-index: 2000; padding: 25px; overflow-y: auto; }
        #toast { position: fixed; bottom: 90px; left: 15px; right: 15px; background: var(--dark); color: white; padding: 15px; border-radius: 18px; display: none; z-index: 3000; border-left: 6px solid var(--gold); }
        .support-bubble { position: fixed; bottom: 100px; right: 20px; width: 55px; height: 55px; background: var(--gold); border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-size: 24px; box-shadow: 0 10px 20px rgba(212, 175, 55, 0.4); z-index: 900; animation: bounce 3s infinite; }
        @keyframes bounce { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-10px); } }
        .hash-meter { font-family: monospace; color: #4ade80; text-shadow: 0 0 10px rgba(74, 222, 128, 0.3); }
    </style>
</head>
<body>

    <div id="toast">
        <div class="flex items-center gap-3">
            <div class="w-8 h-8 bg-green-500/20 rounded-full flex items-center justify-center"><i class="fa-solid fa-check text-green-500 text-sm"></i></div>
            <div><p id="t-user" class="font-black text-[10px] uppercase"></p><p id="t-msg" class="text-slate-400 text-[9px]"></p></div>
        </div>
    </div>

    <div class="support-bubble" onclick="alert('Live Support Agent is connecting...')"><i class="fa-solid fa-comment-dots"></i></div>

    <div id="auth-screen" class="min-h-screen flex flex-col p-8 justify-center active-screen bg-white">
        <div class="text-center mb-10">
            <h1 id="logo-main" class="text-6xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[9px] text-slate-400 font-bold uppercase tracking-[6px] mt-2">Certified Blockchain Infrastructure</p>
        </div>
        <div class="space-y-3">
            <input type="text" id="u-id" placeholder="Partner ID" class="w-full p-4 bg-slate-100 rounded-xl outline-none font-bold">
            <input type="password" id="u-pw" placeholder="Secret Key" class="w-full p-4 bg-slate-100 rounded-xl outline-none font-bold">
            <div id="adm-box" class="hidden"><input type="password" id="adm-key" placeholder="System Root Access" class="w-full p-4 bg-yellow-50 border border-yellow-200 rounded-xl outline-none font-bold"></div>
            <button onclick="auth()" class="w-full btn-gold py-5 shadow-xl shadow-yellow-600/10">Access Dashboard</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[10px] text-slate-400 font-bold uppercase mt-8 cursor-pointer">Register Legal Account</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white border-b sticky top-0 z-50">
            <h2 class="font-black text-2xl tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="flex items-center gap-3">
                <span class="text-[8px] font-black bg-green-100 text-green-600 px-3 py-1 rounded-full">ENCRYPTED</span>
                <i onclick="logout()" class="fa-solid fa-power-off text-slate-300"></i>
            </div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6">
            <div class="dark-card p-8 mb-6 shadow-2xl">
                <div class="absolute top-0 right-0 p-4 flex gap-2">
                    <span class="text-[8px] font-black text-slate-500">HASH-NET: ACTIVE</span>
                    <span class="w-2 h-2 bg-green-500 rounded-full animate-pulse"></span>
                </div>
                <div class="relative z-10 text-center">
                    <div class="hash-meter text-[10px] font-bold mb-4 tracking-widest uppercase">Power: <span id="hash-val">450.25</span> TH/s</div>
                    <p class="text-[9px] text-slate-400 font-black uppercase tracking-[4px] mb-2">Available Balance</p>
                    <h1 id="b-main" class="text-6xl font-black tracking-tighter mb-8">$0.00</h1>
                    <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-6">
                        <div class="text-left"><p class="text-[8px] text-slate-500 font-bold uppercase">Daily Revenue</p><p id="b-daily" class="text-lg font-black text-green-400">+$0.00</p></div>
                        <div class="text-right"><p class="text-[8px] text-slate-500 font-bold uppercase">Total Profit</p><p id="b-total" class="text-lg font-black text-[#D4AF37]">$0.00</p></div>
                    </div>
                </div>
            </div>

            <div class="flex gap-2 mb-6 overflow-x-auto no-scrollbar">
                <div class="bg-white border p-3 rounded-2xl flex items-center gap-2 shrink-0">
                    <i class="fa-solid fa-microchip text-[#D4AF37]"></i><span class="text-[9px] font-black">ASIC-POWER</span>
                </div>
                <div class="bg-white border p-3 rounded-2xl flex items-center gap-2 shrink-0">
                    <i class="fa-solid fa-building-columns text-[#D4AF37]"></i><span class="text-[9px] font-black">REG-UK-CORP</span>
                </div>
            </div>

            <div onclick="openModal('modal-legal')" class="nova-card p-5 mb-6 flex justify-between items-center bg-gradient-to-r from-white to-slate-50 border-l-4 border-blue-500">
                <div class="flex items-center gap-4">
                    <div class="w-10 h-10 bg-blue-50 rounded-full flex items-center justify-center text-blue-500"><i class="fa-solid fa-file-contract"></i></div>
                    <div><h4 class="text-xs font-black uppercase">Official Certifications</h4><p class="text-[9px] text-slate-400 font-bold">View Legal Documents & License</p></div>
                </div>
                <i class="fa-solid fa-chevron-right text-slate-300"></i>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-10">
                <button onclick="openModal('modal-dep')" class="nova-card p-6 flex flex-col items-center gap-3"><i class="fa-solid fa-wallet text-2xl text-[#D4AF37]"></i><span class="text-[10px] font-black">DEPOSIT</span></button>
                <button onclick="openModal('modal-wd')" class="nova-card p-6 flex flex-col items-center gap-3"><i class="fa-solid fa-arrow-up-right-from-square text-2xl text-slate-400"></i><span class="text-[10px] font-black">WITHDRAW</span></button>
            </div>
        </div>

        <div id="page-nodes" class="screen px-4 pt-6">
            <h2 class="text-2xl font-black mb-6 uppercase italic">Mining <span class="text-[#D4AF37]">Hardware</span></h2>
            <div id="nodes-grid" class="space-y-4 pb-10"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-gauge-high text-xl"></i><span>CORE</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-server text-xl"></i><span>HARDWARE</span></div>
            <div onclick="alert('Accessing Blockchain Records...')" class="nav-item"><i class="fa-solid fa-link text-xl"></i><span>EXPLORER</span></div>
        </nav>
    </div>

    <div id="modal-legal" class="modal">
        <div class="flex justify-between items-center mb-8"><h2 class="text-2xl font-black">LEGAL LICENSE</h2><i onclick="closeModal('modal-legal')" class="fa-solid fa-xmark text-2xl"></i></div>
        <div class="nova-card p-8 text-center border-4 border-double border-slate-200">
            <div class="mb-6"><h1 class="text-3xl font-serif font-black text-slate-800">NOVA CORE</h1><p class="text-[8px] uppercase tracking-widest text-slate-400">Certificate of Cloud Mining Operations</p></div>
            <div class="space-y-4 text-left border-y py-6 my-6 border-slate-100">
                <p class="text-[9px] font-bold"><span class="text-slate-400">License ID:</span> GB-8829441-N</p>
                <p class="text-[9px] font-bold"><span class="text-slate-400">Entity:</span> Nova Digital Infrastructure LTD</p>
                <p class="text-[9px] font-bold"><span class="text-slate-400">Jurisdiction:</span> London, United Kingdom</p>
                <p class="text-[9px] font-bold"><span class="text-slate-400">Verification:</span> Blockchain Encrypted (v4.0)</p>
            </div>
            <div class="flex justify-center"><img src="https://api.qrserver.com/v1/create-qr-code/?size=100x100&data=NOVA_CERTIFIED_882944" class="w-20 h-20 opacity-30"></div>
        </div>
        <p class="text-center text-[8px] text-slate-400 font-bold mt-10 uppercase">Official Document - Proprietary & Confidential</p>
    </div>

    <div id="modal-dep" class="modal">
        <div class="flex justify-between items-center mb-8"><h2 class="text-2xl font-black">FUNDING</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark"></i></div>
        <p class="text-center text-[10px] font-black text-slate-400 uppercase mb-4 tracking-widest">Network TRC20 Address</p>
        <div class="nova-card p-6 bg-slate-900 text-[#D4AF37] font-mono text-[10px] text-center break-all mb-8">TTSxm4pBK26RB4vXaa3Uo3hqGa5HdhxBDR</div>
        <input type="number" id="in-amt" placeholder="Amount (USDT)" class="w-full p-4 bg-slate-100 rounded-xl mb-3 font-bold">
        <input type="text" id="in-ref" placeholder="Blockchain TxID" class="w-full p-4 bg-slate-100 rounded-xl mb-6 font-bold">
        <button onclick="reqFin('Deposit')" class="w-full btn-gold py-5">Confirm Transaction</button>
    </div>

    <div id="modal-wd" class="modal">
        <div class="flex justify-between items-center mb-8"><h2 class="text-2xl font-black">PAYOUT</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-xmark"></i></div>
        <input type="number" id="out-amt" placeholder="Withdrawal Amount" class="w-full p-4 bg-slate-100 rounded-xl mb-3 font-bold">
        <input type="text" id="out-addr" placeholder="Recipient TRC20 Address" class="w-full p-4 bg-slate-100 rounded-xl mb-6 font-bold">
        <button onclick="reqFin('Withdrawal')" class="w-full btn-gold py-5 bg-slate-900">Request Extraction</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const config = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(config); const db = getFirestore(app);
        let user = localStorage.getItem('nova_user'); let isLogin = true;

        if(user) start(user);

        window.auth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(document.getElementById('adm-key').value === "NOVA786") { openAdmin(); return; }
            if(!id || !pw) return alert("All fields mandatory");
            const uR = doc(db, "users", id); const s = await getDoc(uR);
            if(!isLogin) {
                if(s.exists()) return alert("User already exists");
                await setDoc(uR, { balance: 0, password: pw, daily: 0, total_profit: 0 });
                localStorage.setItem('nova_user', id); start(id);
            } else if(s.exists() && s.data().password === pw) {
                localStorage.setItem('nova_user', id); start(id);
            } else alert("Invalid Access");
        };

        function start(id) {
            user = id; document.getElementById('auth-screen').style.display='none'; document.getElementById('app').classList.remove('hidden');
            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                document.getElementById('b-main').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('b-daily').innerText = "+$" + (data.daily || 0).toFixed(2);
                document.getElementById('b-total').innerText = "$" + (data.total_profit || 0).toFixed(2);
            });
            genNodes(); runPulse(); setInterval(showToast, 20000);
        }

        function genNodes() {
            const c = document.getElementById('nodes-grid'); c.innerHTML = "";
            const imgs = ["https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400&auto=format", "https://images.unsplash.com/photo-1639762681485-074b7f938ba0?q=80&w=400&auto=format"];
            for(let i=1; i<=30; i++) {
                const p = i * 45; const y = (p * 0.1).toFixed(2);
                c.innerHTML += `<div class="nova-card overflow-hidden"><img src="${imgs[i%2]}" class="w-full h-32 object-cover opacity-80 border-b">
                <div class="p-5 flex justify-between items-center"><div><h4 class="text-[11px] font-black uppercase tracking-tighter">Unit AX-${i} Enterprise</h4><p class="text-green-500 font-bold text-[9px]">+$${y}/24H Output</p></div><button onclick="buyNode(${p},${y})" class="btn-gold px-6 py-2 shadow-md">$${p}</button></div></div>`;
            }
        }

        function runPulse() {
            setInterval(() => {
                const val = (450 + Math.random() * 5).toFixed(2);
                document.getElementById('hash-val').innerText = val;
            }, 3000);
        }

        function showToast() {
            const users = ["Malik", "Sara", "Zain", "Fatima", "Arshad", "Umer", "Khan", "Irfan"];
            const u = users[Math.floor(Math.random()*users.length)];
            const val = (Math.random()*450 + 20).toFixed(2);
            document.getElementById('t-user').innerText = `Partner @${u}***`;
            document.getElementById('t-msg').innerText = `Successfully extracted $${val} USDT`;
            const t = document.getElementById('toast'); t.style.display='block';
            setTimeout(()=> t.style.display='none', 4500);
        }

        window.buyNode = async (p, d) => {
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(s.data().balance < p) return alert("Liquidity shortfall detected");
            await updateDoc(uR, { balance: s.data().balance - p, daily: (s.data().daily || 0) + parseFloat(d) });
            alert("Hardware Unit Deployed!");
        };

        window.reqFin = async (type) => {
            const a = parseFloat(document.getElementById(type=='Deposit'?'in-amt':'out-amt').value);
            const r = document.getElementById(type=='Deposit'?'in-ref':'out-addr').value;
            if(!a || !r) return alert("Data missing");
            await addDoc(collection(db, "logs"), { user, type, amount: a, status: 'Processing', time: Date.now(), ref: r });
            alert("Request submitted to blockchain network!"); closeModal(type=='Deposit'?'modal-dep':'modal-wd');
        };

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            el.classList.add('active');
        };

        let taps = 0;
        document.getElementById('logo-main').onclick = () => { taps++; if(taps >= 6) document.getElementById('adm-box').classList.remove('hidden'); };
        window.toggleM = () => { isLogin = !isLogin; document.getElementById('m-text').innerText = isLogin ? "Register Legal Account" : "Partner Login"; };
        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.logout = () => { localStorage.removeItem('nova_user'); location.reload(); };
    </script>
</body>
</html>
