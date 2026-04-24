<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | Quantum Mining Enterprise</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --bg: #F8FAFC; --card: #FFFFFF; }
        body { background: var(--bg); font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; overflow-x: hidden; scroll-behavior: smooth; }
        
        .premium-card { background: var(--card); border: 1px solid #e2e8f0; border-radius: 28px; transition: 0.3s; overflow: hidden; box-shadow: 0 4px 20px rgba(0,0,0,0.02); }
        .premium-card:hover { transform: translateY(-5px); box-shadow: 0 10px 40px rgba(0,0,0,0.05); }
        
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 16px; font-weight: 800; transition: 0.3s; box-shadow: 0 4px 15px rgba(212, 175, 55, 0.3); text-align: center; }
        .btn-gold:active { transform: scale(0.96); }

        .ticker-wrap { background: #0f172a; color: white; padding: 12px 0; overflow: hidden; }
        .ticker { display: inline-block; white-space: nowrap; animation: ticker 40s linear infinite; font-size: 10px; font-weight: 800; letter-spacing: 1px; }
        @keyframes ticker { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }

        .screen { display: none; }
        .active-screen { display: block; animation: slideUp 0.6s cubic-bezier(0.16, 1, 0.3, 1); }
        @keyframes slideUp { from { opacity: 0; transform: translateY(40px); } to { opacity: 1; transform: translateY(0); } }

        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(20px); border-top: 1px solid #e2e8f0; display: flex; justify-content: space-around; padding: 18px 10px; z-index: 1000; }
        .nav-item { color: #94a3b8; font-size: 9px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 6px; cursor: pointer; transition: 0.2s; }
        .nav-item.active { color: var(--gold); }

        .no-scrollbar::-webkit-scrollbar { display: none; }
    </style>
</head>
<body>

    <div class="ticker-wrap">
        <div class="ticker uppercase">
            ⚡ Network Status: Operational • 🚀 New Node Activated by @A***2 • 💎 BTC/USD: $64,281.00 • ✅ Security Audit: PASSED • 🔥 Hashrate: 214.5 EH/s • ⚡ User @M***9 withdrew $450.00
        </div>
    </div>

    <div id="auth-screen" class="active-screen">
        <section class="p-10 text-center bg-white min-h-[50vh] flex flex-col justify-center">
            <h1 class="text-6xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[10px] text-slate-400 font-bold uppercase tracking-[12px] mt-2 mb-12">Institutional Mining</p>
            <div class="space-y-4 max-w-sm mx-auto w-full">
                <input type="text" id="u-id" placeholder="Access ID" class="w-full p-5 border-2 border-slate-100 rounded-3xl outline-none focus:border-[#D4AF37] transition">
                <input type="password" id="u-pw" placeholder="Security Key" class="w-full p-5 border-2 border-slate-100 rounded-3xl outline-none focus:border-[#D4AF37] transition">
                <button onclick="handleAuth()" class="w-full btn-gold py-5 uppercase tracking-widest text-sm">Initialize Session</button>
                <p onclick="toggleMode()" id="mode-text" class="text-[10px] text-slate-400 font-black uppercase mt-6 cursor-pointer hover:text-slate-900 transition underline underline-offset-4">Create Master Account</p>
            </div>
        </section>

        <section class="p-8 space-y-6 bg-slate-50 pb-24">
            <div class="premium-card p-6 border-l-4 border-[#D4AF37]">
                <div class="flex items-center gap-4 mb-3">
                    <i class="fa-solid fa-shield-check text-xl text-[#D4AF37]"></i>
                    <h4 class="font-black text-sm uppercase">End-to-End Encryption</h4>
                </div>
                <p class="text-[11px] text-slate-500 leading-relaxed">Your assets are protected by military-grade cold storage protocols and 2FA authentication.</p>
            </div>
        </section>
    </div>

    <div id="app" class="hidden">
        <header class="p-6 flex justify-between items-center bg-white/80 backdrop-blur-md border-b border-slate-100 sticky top-0 z-[100]">
            <h2 class="font-black text-3xl tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="flex items-center gap-4">
                <div class="flex flex-col items-end">
                    <span id="user-tag" class="text-[10px] font-black text-slate-900 uppercase">@USER</span>
                    <span class="text-[8px] font-bold text-green-500 uppercase">Pro Trader</span>
                </div>
                <div class="w-10 h-10 rounded-2xl bg-slate-100 border border-slate-200 flex items-center justify-center">
                    <i class="fa-solid fa-user text-slate-400"></i>
                </div>
            </div>
        </header>

        <div id="page-dash" class="screen active-screen px-5 pt-8 pb-32">
            <div class="bg-slate-900 rounded-[40px] p-10 mb-8 text-white shadow-2xl relative overflow-hidden">
                <div class="absolute -right-20 -top-20 w-64 h-64 bg-white/5 rounded-full blur-3xl"></div>
                <div class="relative z-10">
                    <p class="text-[10px] text-slate-400 font-black uppercase tracking-[5px] mb-4">Portfolio Balance</p>
                    <h1 id="b-main" class="text-6xl font-black tracking-tighter mb-12">$0.00</h1>
                    <div class="grid grid-cols-2 gap-6 border-t border-white/10 pt-8">
                        <div>
                            <p class="text-[9px] text-slate-500 uppercase font-bold mb-1">Daily Yield</p>
                            <p id="b-daily" class="text-2xl font-black text-green-400">+$0.00</p>
                        </div>
                        <div class="text-right">
                            <p class="text-[9px] text-slate-500 uppercase font-bold mb-1">Total Profits</p>
                            <p id="b-total" class="text-2xl font-black text-[#D4AF37]">$0.00</p>
                        </div>
                    </div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-5 mb-8">
                <button onclick="openModal('modal-dep')" class="premium-card p-8 flex flex-col items-center gap-4 hover:border-[#D4AF37]">
                    <div class="w-12 h-12 rounded-2xl bg-amber-50 flex items-center justify-center">
                        <i class="fa-solid fa-arrow-down text-xl text-[#D4AF37]"></i>
                    </div>
                    <span class="text-[11px] font-black uppercase">Deposit</span>
                </button>
                <button onclick="openModal('modal-wd')" class="premium-card p-8 flex flex-col items-center gap-4 hover:border-slate-900">
                    <div class="w-12 h-12 rounded-2xl bg-slate-50 flex items-center justify-center">
                        <i class="fa-solid fa-arrow-up text-xl text-slate-400"></i>
                    </div>
                    <span class="text-[11px] font-black uppercase">Withdraw</span>
                </button>
            </div>

            <div class="premium-card p-8 mb-8">
                <h4 class="text-[11px] font-black uppercase mb-6 text-slate-400 tracking-widest text-center">Cloud Performance (7D)</h4>
                <canvas id="profitChart" height="180"></canvas>
            </div>
        </div>

        <div id="page-nodes" class="screen px-5 pt-8 pb-32">
            <div class="mb-10 text-center">
                <h2 class="text-3xl font-black uppercase tracking-tighter">Mining <span class="text-[#D4AF37]">Infrastructure</span></h2>
                <p class="text-[9px] text-slate-400 font-bold uppercase mt-2">Active Hardware Nodes Worldwide</p>
            </div>
            <div id="nodes-grid" class="grid grid-cols-1 gap-8"></div>
        </div>

        <div id="page-logs" class="screen px-5 pt-8 pb-32">
            <h2 class="text-3xl font-black mb-10 uppercase tracking-tighter">Network <span class="text-[#D4AF37]">Ledger</span></h2>
            <div id="logs-list" class="space-y-4"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-grid-2 text-xl"></i><span>TERMINAL</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-microchip text-xl"></i><span>HARDWARE</span></div>
            <div onclick="nav('page-logs', this)" class="nav-item"><i class="fa-solid fa-list-ul text-xl"></i><span>RECORDS</span></div>
            <div onclick="logout()" class="nav-item"><i class="fa-solid fa-power-off text-xl text-red-400"></i><span>SHUTDOWN</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-white z-[2000] p-10 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-12"><h2 class="text-4xl font-black">FUNDING</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark text-3xl cursor-pointer"></i></div>
        <div class="premium-card p-8 border-l-8 border-blue-600 bg-blue-50/30 mb-8">
            <p class="text-xs font-black text-blue-600 uppercase mb-4">Official USDT (BEP20) Portal</p>
            <p class="text-sm font-mono font-bold break-all bg-white p-4 rounded-xl border border-blue-100">0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37</p>
            <button onclick="copy('0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37')" class="mt-6 text-[11px] font-black underline text-blue-800">COPY WALLET ADDRESS</button>
        </div>
        <div class="space-y-4">
            <input type="number" id="in-amt" placeholder="Deposit Amount" class="w-full p-5 border-2 border-slate-100 rounded-3xl outline-none focus:border-[#D4AF37]">
            <input type="text" id="in-ref" placeholder="Transaction TXID" class="w-full p-5 border-2 border-slate-100 rounded-3xl outline-none focus:border-[#D4AF37]">
            <button onclick="submitFin('Deposit')" class="w-full btn-gold py-6 text-sm uppercase mt-4">Confirm Deposit</button>
        </div>
    </div>

    <div id="modal-wd" class="fixed inset-0 bg-white z-[2000] p-10 hidden">
        <div class="flex justify-between items-center mb-12"><h2 class="text-4xl font-black">EXTRACT</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-xmark text-3xl cursor-pointer"></i></div>
        <div class="space-y-4">
            <input type="number" id="out-amt" placeholder="Payout Amount" class="w-full p-5 border-2 border-slate-100 rounded-3xl outline-none focus:border-[#D4AF37]">
            <input type="text" id="out-addr" placeholder="Recipient Wallet" class="w-full p-5 border-2 border-slate-100 rounded-3xl outline-none focus:border-[#D4AF37]">
            <button onclick="submitFin('Withdrawal')" class="w-full btn-gold py-6 bg-slate-900 !text-white text-sm uppercase mt-4">Initialize Payout</button>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);
        
        let currentUser = localStorage.getItem('nova_session');
        let isLoginMode = true;

        const nodesData = [
            { id: 1, name: "Nano Core V1", cost: 20, yield: 1.5, hashrate: "1.2 TH/s", algo: "SHA-256", img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?q=80&w=400" },
            { id: 2, name: "Neon Bit 2.0", cost: 50, yield: 4.2, hashrate: "3.5 TH/s", algo: "Scrypt", img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?q=80&w=400" },
            { id: 3, name: "Bronze Rig X", cost: 100, yield: 9.0, hashrate: "8.2 TH/s", algo: "SHA-256", img: "https://images.unsplash.com/photo-1639322537228-f710d846310a?q=80&w=400" },
            { id: 4, name: "Cobalt Miner", cost: 200, yield: 18.5, hashrate: "15.0 TH/s", algo: "EtHash", img: "https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400" },
            { id: 5, name: "Silver Pro 50", cost: 500, yield: 48.0, hashrate: "42.5 TH/s", algo: "SHA-256", img: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?q=80&w=400" },
            { id: 6, name: "Viper Blade", cost: 750, yield: 72.0, hashrate: "68.0 TH/s", algo: "Scrypt", img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?q=80&w=400" },
            { id: 7, name: "Gold CPU Master", cost: 1000, yield: 110.0, hashrate: "95.0 TH/s", algo: "SHA-256", img: "https://images.unsplash.com/photo-1639322537523-abaea1ec960c?q=80&w=400" },
            { id: 8, name: "Titan X-500", cost: 1500, yield: 165.0, hashrate: "140.0 TH/s", algo: "EtHash", img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?q=80&w=400" },
            { id: 9, name: "Apex Master Node", cost: 2000, yield: 220.0, hashrate: "195.0 TH/s", algo: "Multi-Algo", img: "https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400" },
            { id: 10, name: "Nova Storm 250", cost: 2500, yield: 280.0, hashrate: "250.0 TH/s", algo: "SHA-256", img: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?q=80&w=400" },
            { id: 11, name: "Zenith Node", cost: 3000, yield: 340.0, hashrate: "310.0 TH/s", algo: "Quantum-S", img: "https://images.unsplash.com/photo-1639322537228-f710d846310a?q=80&w=400" },
            { id: 12, name: "Raptor Cluster", cost: 4000, yield: 460.0, hashrate: "420.0 TH/s", algo: "SHA-256", img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?q=80&w=400" },
            { id: 13, name: "Orion Server S3", cost: 5000, yield: 580.0, hashrate: "510.0 TH/s", algo: "EtHash", img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?q=80&w=400" },
            { id: 14, name: "Kraken Mega Rig", cost: 7000, yield: 820.0, hashrate: "750.0 TH/s", algo: "Scrypt", img: "https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400" },
            { id: 15, name: "Empire Blade 8", cost: 8500, yield: 1050.0, hashrate: "920.0 TH/s", algo: "SHA-256", img: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?q=80&w=400" },
            { id: 16, name: "Nebula Core PH", cost: 10000, yield: 1250.0, hashrate: "1.2 PH/s", algo: "Quantum-X", img: "https://images.unsplash.com/photo-1639322537523-abaea1ec960c?q=80&w=400" },
            { id: 17, name: "Galaxy Miner Ultra", cost: 12500, yield: 1600.0, hashrate: "1.8 PH/s", algo: "Multi-Chain", img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?q=80&w=400" },
            { id: 18, name: "Infinity Node PH", cost: 15000, yield: 1950.0, hashrate: "2.5 PH/s", algo: "SHA-256", img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?q=80&w=400" },
            { id: 19, name: "Omni Pro PH", cost: 20000, yield: 2700.0, hashrate: "3.2 PH/s", algo: "EtHash", img: "https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400" },
            { id: 20, name: "Quantum Nexus PH", cost: 25000, yield: 3500.0, hashrate: "5.0 PH/s", algo: "Quantum-Max", img: "https://images.unsplash.com/photo-1639322537228-f710d846310a?q=80&w=400" },
            { id: 21, name: "Nova Sovereign PH", cost: 30000, yield: 4300.0, hashrate: "8.5 PH/s", algo: "Sovereign-X", img: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?q=80&w=400" }
        ];

        window.toggleMode = () => {
            isLoginMode = !isLoginMode;
            document.getElementById('mode-text').innerText = isLoginMode ? "Create Master Account" : "Access Registered Portal";
        };

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return alert("Fill Security Credentials");

            const uRef = doc(db, "users", id);
            const snap = await getDoc(uRef);

            if(isLoginMode) {
                if(snap.exists() && snap.data().password === pw) {
                    localStorage.setItem('nova_session', id);
                    location.reload();
                } else { alert("Unauthorized Access Detected"); }
            } else {
                if(snap.exists()) return alert("Access ID already taken");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total_profit: 0, active_nodes: [] });
                localStorage.setItem('nova_session', id);
                location.reload();
            }
        };

        function initApp(id) {
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('user-tag').innerText = `@${id}`;

            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                document.getElementById('b-main').innerText = "$" + (data.balance || 0).toLocaleString(undefined, {minimumFractionDigits: 2});
                document.getElementById('b-daily').innerText = "+$" + (data.daily || 0).toLocaleString();
                document.getElementById('b-total').innerText = "$" + (data.total_profit || 0).toLocaleString();
                renderNodes(data.active_nodes || []);
            });

            loadRecords(id);
            setupChart();
            setInterval(syncTimers, 1000);
        }

        function renderNodes(activeList) {
            const container = document.getElementById('nodes-grid');
            container.innerHTML = "";
            nodesData.forEach(node => {
                const isActive = activeList.find(a => a.nodeId === node.id);
                container.innerHTML += `
                <div class="premium-card">
                    <div class="h-44 relative">
                        <img src="${node.img}" class="w-full h-full object-cover">
                        <div class="absolute inset-0 bg-gradient-to-t from-black/60 to-transparent"></div>
                        <div class="absolute bottom-4 left-4"><h4 class="text-white font-black text-xl">${node.name}</h4></div>
                    </div>
                    <div class="p-6">
                        <div class="flex justify-between items-center mb-6">
                            <span class="text-[9px] font-black bg-slate-100 px-3 py-1 rounded-full uppercase text-slate-500">${node.algo}</span>
                            <span class="text-sm font-black text-green-500">+$${node.yield}/Day</span>
                        </div>
                        <div class="grid grid-cols-2 gap-4 mb-6">
                            <div class="bg-slate-50 p-3 rounded-2xl">
                                <p class="text-[8px] font-bold text-slate-400 uppercase">Hashrate</p>
                                <p class="text-[11px] font-black">${node.hashrate}</p>
                            </div>
                            <div class="bg-slate-50 p-3 rounded-2xl">
                                <p class="text-[8px] font-bold text-slate-400 uppercase">Status</p>
                                <p id="timer-${node.id}" class="text-[11px] font-black text-[#D4AF37]">READY</p>
                            </div>
                        </div>
                        ${isActive ? `<button class="w-full py-4 bg-slate-100 text-slate-400 rounded-2xl text-[11px] font-black cursor-not-allowed">MINING ACTIVE</button>` : 
                        `<button onclick="buyNode(${node.id},${node.cost},${node.yield})" class="w-full btn-gold py-4 text-[11px] uppercase tracking-widest">Activate • $${node.cost}</button>`}
                    </div>
                </div>`;
            });
        }

        window.buyNode = async (id, cost, y) => {
            const uid = localStorage.getItem('nova_session');
            const uRef = doc(db, "users", uid);
            const snap = await getDoc(uRef);
            if(snap.data().balance < cost) return alert("Insufficient Liquidity");

            await updateDoc(uRef, {
                balance: snap.data().balance - cost,
                active_nodes: [...(snap.data().active_nodes || []), { nodeId: id, yield: y, lastClaim: Date.now() }],
                daily: (snap.data().daily || 0) + y
            });
            alert("Hardware Activated Successfully!");
        };

        function syncTimers() {
            const uid = localStorage.getItem('nova_session');
            if(!uid) return;
            getDoc(doc(db, "users", uid)).then(s => {
                const active = s.data().active_nodes || [];
                active.forEach(n => {
                    const elapsed = Date.now() - n.lastClaim;
                    const remaining = (24 * 60 * 60 * 1000) - elapsed;
                    const el = document.getElementById(`timer-${n.nodeId}`);
                    if(el && remaining > 0) {
                        const h = Math.floor(remaining / 3600000).toString().padStart(2, '0');
                        const m = Math.floor((remaining % 3600000) / 60000).toString().padStart(2, '0');
                        const sc = Math.floor((remaining % 60000) / 1000).toString().padStart(2, '0');
                        el.innerText = `${h}:${m}:${sc}`;
                    } else if(el) { el.innerText = "CLAIM READY"; }
                });
            });
        }

        window.submitFin = async (type) => {
            const amt = parseFloat(document.getElementById(type=='Deposit'?'in-amt':'out-amt').value);
            const ref = document.getElementById(type=='Deposit'?'in-ref':'out-addr').value;
            if(!amt || !ref) return alert("Fill Required Data");
            
            await addDoc(collection(db, "logs"), { user: localStorage.getItem('nova_session'), type, amount: amt, status: 'Pending Approval', ref, time: Date.now() });
            alert("Encrypted Transmission Received. Processing...");
            closeModal(type=='Deposit'?'modal-dep':'modal-wd');
        };

        function loadRecords(id) {
            onSnapshot(query(collection(db, "logs"), where("user", "==", id)), s => {
                const list = document.getElementById('logs-list'); list.innerHTML = "";
                s.forEach(doc => {
                    const d = doc.data();
                    list.innerHTML += `<div class="premium-card p-6 flex justify-between items-center border-r-4 border-amber-400"><div><p class="text-[10px] font-black text-slate-400 uppercase">${d.type}</p><p class="text-lg font-black tracking-tight">$${d.amount}</p></div><div class="text-right"><p class="text-[9px] font-bold text-orange-500 uppercase">${d.status}</p><p class="text-[8px] text-slate-300 uppercase">${new Date(d.time).toLocaleDateString()}</p></div></div>`;
                });
            });
        }

        function setupChart() {
            const ctx = document.getElementById('profitChart').getContext('2d');
            new Chart(ctx, { type: 'line', data: { labels: ['M', 'T', 'W', 'T', 'F', 'S', 'S'], datasets: [{ data: [12, 19, 15, 25, 32, 28, 45], borderColor: '#D4AF37', borderWidth: 4, tension: 0.4, fill: true, backgroundColor: 'rgba(212, 175, 55, 0.05)', pointRadius: 0 }] }, options: { plugins: { legend: { display: false } }, scales: { y: { display: false }, x: { grid: { display: false }, ticks: { font: { weight: '800', size: 9 } } } } } });
        }

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            el.classList.add('active');
        };

        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.logout = () => { localStorage.removeItem('nova_session'); location.reload(); };
        window.copy = (txt) => { navigator.clipboard.writeText(txt); alert("Secure Link Copied!"); };

        if(currentUser) initApp(currentUser);
    </script>
</body>
</html>
