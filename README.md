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
        .node-gradient { background: linear-gradient(135deg, #1e293b 0%, #334155 100%); }
        .special-gradient { background: linear-gradient(135deg, #B8860B 0%, #D4AF37 100%); }
        .btn-gold { background: linear-gradient(135deg, #B8860B, #D4AF37); color: white; border-radius: 12px; font-weight: 700; transition: 0.2s; text-transform: uppercase; font-size: 11px; }
        .btn-gold:active { transform: scale(0.96); }
        .screen { display: none; padding-bottom: 110px; animation: fadeIn 0.3s ease-in; }
        .active-screen { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(5px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: white; border-top: 1px solid #f1f5f9; display: flex; justify-content: space-around; padding: 15px 5px; z-index: 100; }
        .nav-item { color: #94a3b8; font-size: 10px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: #B8860B; }
        .p-input { background: #f1f5f9; border: 2px solid transparent; width: 100%; padding: 14px; border-radius: 12px; outline: none; margin-bottom: 10px; font-size: 13px; }
        .p-input:focus { border-color: #D4AF37; background: #fff; }
        .modal { display: none; position: fixed; inset: 0; background: white; z-index: 1000; padding: 20px; overflow-y: auto; }
        .badge-pending { background: #fff7ed; color: #ea580c; border: 1px solid #fed7aa; }
        .badge-success { background: #f0fdf4; color: #16a34a; border: 1px solid #bbf7d0; }
    </style>
</head>
<body>

    <div id="global-msg" class="hidden bg-slate-900 text-[#D4AF37] p-2.5 text-[10px] font-bold text-center uppercase tracking-widest border-b border-yellow-600/20"></div>

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
                <a id="tg-link" href="#" target="_blank" class="text-sky-500 text-xl"><i class="fa-brands fa-telegram"></i></a>
                <i onclick="logout()" class="fa-solid fa-power-off text-slate-300"></i>
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
            <span class="text-[8px] font-black text-green-600 uppercase">TOKYO-01</span>
        </div>
    </div>

    <div class="nova-card p-8 mb-6 bg-slate-900 text-white relative overflow-hidden shadow-2xl border-b-8 border-[#D4AF37]">
        <div class="absolute -right-10 -top-10 w-40 h-40 bg-[#D4AF37]/10 rounded-full blur-3xl"></div>
        
        <div class="relative z-10 text-center">
            <p class="text-[10px] text-slate-400 font-bold uppercase tracking-[4px] mb-2">Available Liquidity</p>
            <h1 id="main-bal" class="text-6xl font-black text-white tracking-tighter mb-8">$0.00</h1>
            
            <div class="grid grid-cols-2 gap-6 border-t border-white/10 pt-6">
                <div class="text-left">
                    <p class="text-[9px] text-slate-400 font-bold uppercase mb-1">Daily Yield</p>
                    <p id="v-daily" class="text-base font-black text-green-400">+$0.00</p>
                </div>
                <div class="text-right">
                    <p class="text-[9px] text-slate-400 font-bold uppercase mb-1">Total Profit</p>
                    <p id="total-profit" class="text-base font-black text-[#D4AF37]">$0.00</p>
                </div>
            </div>
        </div>

        <div class="mt-6 bg-white/5 rounded-2xl p-4 flex justify-between items-center border border-white/10">
            <div class="flex items-center gap-3">
                <div class="w-8 h-8 rounded-lg bg-yellow-500/10 flex items-center justify-center">
                    <i class="fa-solid fa-bolt-lightning text-yellow-500 text-xs"></i>
                </div>
                <span class="text-[9px] font-black text-slate-300 uppercase">Next Yield In:</span>
            </div>
            <span id="yield-timer" class="text-sm font-mono font-black text-yellow-500 tracking-widest">23:59:59</span>
        </div>
    </div>

    <div class="grid grid-cols-2 gap-4 mb-6">
        <button onclick="openModal('dep-modal')" class="nova-card p-5 flex flex-col items-center gap-3 active:scale-95 transition bg-white hover:bg-slate-50 border-b-4 border-slate-200">
            <i class="fa-solid fa-plus-circle text-2xl text-[#D4AF37]"></i>
            <span class="text-[10px] font-black uppercase text-slate-700">Add Funds</span>
        </button>
        <button onclick="openModal('wd-modal')" class="nova-card p-5 flex flex-col items-center gap-3 active:scale-95 transition bg-white hover:bg-slate-50 border-b-4 border-slate-200">
            <i class="fa-solid fa-money-bill-transfer text-2xl text-slate-400"></i>
            <span class="text-[10px] font-black uppercase text-slate-700">Withdraw</span>
        </button>
    </div>

    <div class="nova-card p-6 mb-6 space-y-6 bg-white">
        <div>
            <div class="flex justify-between items-center mb-3">
                <p class="text-[9px] font-black text-slate-400 uppercase">Network Referral Link</p>
                <span class="text-[8px] font-black text-green-500 bg-green-50 px-2.5 py-1 rounded-lg border border-green-100">+10% BONUS</span>
            </div>
            <div class="flex gap-2">
                <input type="text" id="ref-link" readonly class="p-input mb-0 py-3 text-[10px] font-mono bg-slate-50 border-slate-200 flex-1 rounded-xl" value="https://nova.io/ref=user">
                <button onclick="copyRef()" class="btn-gold px-5 text-[10px] rounded-xl shadow-lg shadow-yellow-600/20">Copy</button>
            </div>
        </div>

        <div class="pt-4 border-t border-slate-100">
            <p class="text-[9px] font-black text-slate-400 uppercase mb-3">Redeem System Voucher</p>
            <div class="flex gap-2">
                <input type="text" id="promo-inp" placeholder="ENTER CODE" class="p-input mb-0 py-3 text-[10px] font-bold uppercase border-slate-200 flex-1 rounded-xl">
                <button onclick="applyPromo()" class="bg-slate-900 text-white px-6 text-[10px] font-black rounded-xl uppercase transition active:scale-95">Apply</button>
            </div>
        </div>
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

    <div class="nova-card p-6 bg-slate-50 border-2 border-slate-100 mb-10 relative overflow-hidden">
        <div class="flex items-center gap-4 mb-4">
            <div class="w-12 h-12 rounded-2xl bg-white shadow-sm flex items-center justify-center p-2.5 border border-slate-100">
                <img src="https://cdn-icons-png.flaticon.com/512/2583/2583264.png" class="w-full opacity-80">
            </div>
            <div>
                <h3 class="text-[11px] font-black uppercase text-slate-800 tracking-tighter">Nova Infrastructure Ltd.</h3>
                <p class="text-[8px] text-slate-400 font-bold uppercase tracking-widest">Reg: London, UK #09921820</p>
            </div>
        </div>
        <p class="text-[10px] text-slate-500 leading-relaxed font-medium">
            Authorized ASIC provider. We deliver institutional-grade cloud hashing solutions with guaranteed 99.9% network stability across global clusters.
        </p>
    </div>
</div>
    <div class="grid grid-cols-2 gap-3 mb-4">
        <button onclick="openModal('dep-modal')" class="nova-card p-4 flex items-center gap-3 active:scale-95 transition bg-white border-b-2 border-slate-200">
            <div class="w-10 h-10 rounded-xl bg-yellow-50 flex items-center justify-center text-yellow-600 shadow-inner">
                <i class="fa-solid fa-layer-group"></i>
            </div>
            <span class="text-[10px] font-black uppercase text-slate-700">Add Funds</span>
        </button>
        <button onclick="openModal('wd-modal')" class="nova-card p-4 flex items-center gap-3 active:scale-95 transition bg-white border-b-2 border-slate-200">
            <div class="w-10 h-10 rounded-xl bg-slate-50 flex items-center justify-center text-slate-400 shadow-inner">
                <i class="fa-solid fa-vault"></i>
            </div>
            <span class="text-[10px] font-black uppercase text-slate-700">Withdraw</span>
        </button>
    </div>
    <div class="nova-card p-5 mb-4">
        <div class="flex justify-between items-center mb-4">
            <h3 class="text-[9px] font-black uppercase flex items-center gap-2">
                <i class="fa-solid fa-users-viewfinder text-yellow-600"></i> Organization Stats
            </h3>
            <i class="fa-solid fa-chevron-right text-[8px] text-slate-300"></i>
        </div>
        <div class="grid grid-cols-3 gap-2">
            <div class="bg-slate-50 p-3 rounded-2xl border border-slate-100 text-center">
                <p id="direct-count" class="text-sm font-black text-slate-800">0</p>
                <p class="text-[7px] font-bold text-slate-400 uppercase">Directs</p>
            </div>
            <div class="bg-slate-50 p-3 rounded-2xl border border-slate-100 text-center">
                <p id="team-count" class="text-sm font-black text-slate-800">0</p>
                <p class="text-[7px] font-bold text-slate-400 uppercase">Total Team</p>
            </div>
            <div class="bg-slate-50 p-3 rounded-2xl border border-slate-100 text-center">
                <p id="team-earn" class="text-sm font-black text-green-600">$0.00</p>
                <p class="text-[7px] font-bold text-slate-400 uppercase">Comm.</p>
            </div>
        </div>
    </div>

    <div class="nova-card p-5 bg-gradient-to-br from-slate-50 to-white border-2 border-slate-100 mb-6 relative overflow-hidden">
        <div class="absolute right-0 bottom-0 opacity-10">
            <i class="fa-solid fa-building-shield text-5xl translate-x-2 translate-y-2"></i>
        </div>
        <div class="flex items-center gap-3 mb-3">
            <div class="w-10 h-10 rounded-full bg-white shadow-md flex items-center justify-center p-2 border border-slate-100">
                <img src="https://cdn-icons-png.flaticon.com/512/2583/2583264.png" class="w-full">
            </div>
            <div>
                <h3 class="text-[10px] font-black uppercase text-slate-800">Nova Infrastructure Ltd.</h3>
                <p class="text-[7px] text-slate-400 font-bold uppercase tracking-wider">Certified UK Entity #09921820</p>
            </div>
        </div>
        <p class="text-[9px] text-slate-500 leading-relaxed font-medium">
            Global ASIC-cluster operator specializing in high-density liquid-cooled mining solutions for institutional partners.
        </p>
    </div>
</div>

<script>
    // TIMER LOGIC: Counts down to midnight daily
    function runDashboardTimer() {
        setInterval(() => {
            const now = new Date();
            const tomorrow = new Date(now.getFullYear(), now.getMonth(), now.getDate() + 1);
            const diff = tomorrow - now;
            
            const hours = Math.floor(diff / (1000 * 60 * 60)).toString().padStart(2, '0');
            const mins = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60)).toString().padStart(2, '0');
            const secs = Math.floor((diff % (1000 * 60)) / 1000).toString().padStart(2, '0');
            
            const timerEl = document.getElementById('yield-timer');
            if(timerEl) timerEl.innerText = `${hours}:${mins}:${secs}`;
        }, 1000);
    }
    runDashboardTimer();

    // NOTE FOR INTEGRATION:
    // In your start(id) function, add these lines:
    // document.getElementById('display-username').innerText = `@${id.toUpperCase()}`;
    // document.getElementById('ref-link').value = `https://u-s-h.github.io/Coin/?ref=${id}`;
</script>
        <div class="mt-4 bg-white/5 rounded-lg p-2 flex justify-between items-center">
            <span class="text-[8px] font-black text-slate-300 uppercase">Next Yield In:</span>
            <span id="yield-timer" class="text-[10px] font-mono font-bold text-yellow-500">23:59:59</span>
        </div>
    </div>

    <div class="grid grid-cols-2 gap-3 mb-4">
        <button onclick="openModal('dep-modal')" class="nova-card p-4 flex items-center gap-3 active:scale-95 transition">
            <div class="w-10 h-10 rounded-full bg-yellow-50 flex items-center justify-center text-[#D4AF37] shadow-inner">
                <i class="fa-solid fa-arrow-down-to-bracket"></i>
            </div>
            <span class="text-[10px] font-black uppercase text-slate-700">Deposit</span>
        </button>
        <button onclick="openModal('wd-modal')" class="nova-card p-4 flex items-center gap-3 active:scale-95 transition">
            <div class="w-10 h-10 rounded-full bg-slate-50 flex items-center justify-center text-slate-400 shadow-inner">
                <i class="fa-solid fa-paper-plane"></i>
            </div>
            <span class="text-[10px] font-black uppercase text-slate-700">Withdraw</span>
        </button>
    </div>

    <div class="space-y-3 mb-4">
        <div class="nova-card p-4">
            <div class="flex justify-between items-center mb-2">
                <p class="text-[9px] font-black text-slate-400 uppercase">Partner Referral Link</p>
                <span class="text-[8px] font-bold text-green-500">+10% Commission</span>
            </div>
            <div class="flex gap-2">
                <input type="text" id="ref-link" readonly class="p-input mb-0 py-2 text-[10px] font-mono bg-slate-50" value="https://nova.io/ref=user">
                <button onclick="copyRef()" class="btn-gold px-4 text-[10px] rounded-lg">Copy</button>
            </div>
        </div>

        <div class="nova-card p-4">
            <p class="text-[9px] font-black text-slate-400 uppercase mb-2">System Promo Code</p>
            <div class="flex gap-2">
                <input type="text" id="promo-input" placeholder="ENTER CODE" class="p-input mb-0 py-2 text-[10px] font-bold uppercase">
                <button onclick="applyPromo()" class="btn-gold px-6 rounded-lg">Redeem</button>
            </div>
        </div>
    </div>

    <div class="nova-card p-5 mb-4">
        <h3 class="text-[10px] font-black uppercase mb-4 flex items-center gap-2">
            <i class="fa-solid fa-users text-[#D4AF37]"></i> My Mining Team
        </h3>
        <div class="grid grid-cols-3 gap-2 text-center">
            <div class="bg-slate-50 p-2 rounded-xl border border-slate-100">
                <p class="text-[14px] font-black text-slate-800">0</p>
                <p class="text-[7px] font-bold text-slate-400 uppercase">Directs</p>
            </div>
            <div class="bg-slate-50 p-2 rounded-xl border border-slate-100">
                <p class="text-[14px] font-black text-slate-800">0</p>
                <p class="text-[7px] font-bold text-slate-400 uppercase">Team Size</p>
            </div>
            <div class="bg-slate-50 p-2 rounded-xl border border-slate-100">
                <p class="text-[14px] font-black text-slate-800">$0.00</p>
                <p class="text-[7px] font-bold text-slate-400 uppercase">Earnings</p>
            </div>
        </div>
    </div>

    <div class="nova-card p-6 bg-slate-50 border-dashed border-2 border-slate-200 mb-6">
        <div class="flex items-center gap-3 mb-3">
            <img src="https://cdn-icons-png.flaticon.com/512/2583/2583264.png" class="w-8 h-8 opacity-80">
            <div>
                <h3 class="text-[10px] font-black uppercase tracking-tighter">Nova Institutional Ltd.</h3>
                <p class="text-[8px] text-slate-400 font-bold">Registered Office: London, UK</p>
            </div>
        </div>
        <p class="text-[10px] text-slate-500 leading-relaxed font-medium">
            Nova is a verified cloud-mining infrastructure provider. We utilize high-density ASIC clusters and liquid-cooling technology to ensure 99.9% uptime for our global partners.
        </p>
        <div class="mt-4 pt-4 border-t border-slate-200 flex justify-between items-center">
            <span class="text-[8px] font-black text-slate-400">LICENSE #UK-09921820</span>
            <i class="fa-solid fa-certificate text-blue-500"></i>
        </div>
    </div>
</div>

<script>
    // Timer Countdown Logic
    function startYieldTimer() {
        setInterval(() => {
            const now = new Date();
            const tomorrow = new Date(now.getFullYear(), now.getMonth(), now.getDate() + 1);
            const diff = tomorrow - now;
            
            const hours = Math.floor(diff / (1000 * 60 * 60)).toString().padStart(2, '0');
            const mins = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60)).toString().padStart(2, '0');
            const secs = Math.floor((diff % (1000 * 60)) / 1000).toString().padStart(2, '0');
            
            document.getElementById('yield-timer').innerText = `${hours}:${mins}:${secs}`;
        }, 1000);
    }
    startYieldTimer();

    // Set Username on Start
    // Inside your start(id) function:
    // document.getElementById('display-username').innerText = `@${id.toUpperCase()}`;
    // document.getElementById('ref-link').value = `https://u-s-h.github.io/Coin/?ref=${id}`;
</script>
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
            <i onclick="closeModal('dep-modal')" class="fa-solid fa-circle-xmark text-slate-300 text-xl"></i>
        </div>
        <div class="grid grid-cols-3 gap-2 mb-6">
            <div onclick="selD('Binance Pay')" class="dep-m nova-card p-2 text-center border-2 border-yellow-500" id="m-bin"><i class="fa-solid fa-coins text-yellow-500 mb-1"></i><p class="text-[8px] font-bold">Binance</p></div>
            <div onclick="selD('Trust Wallet')" class="dep-m nova-card p-2 text-center border-2 border-slate-100"><i class="fa-solid fa-shield text-blue-500 mb-1"></i><p class="text-[8px] font-bold">Trust</p></div>
            <div onclick="selD('MetaMask')" class="dep-m nova-card p-2 text-center border-2 border-slate-100"><i class="fa-solid fa-fox text-orange-500 mb-1"></i><p class="text-[8px] font-bold">MetaMask</p></div>
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
            <i onclick="closeModal('wd-modal')" class="fa-solid fa-circle-xmark text-slate-300 text-xl"></i>
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
            <div>
                <h3 class="text-[10px] font-black mb-4">PENDING TRANSACTIONS</h3>
                <div id="adm-requests" class="space-y-4"></div>
            </div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const config = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(config); const db = getFirestore(app);
        let user = localStorage.getItem('nova_user'); let login = true; let currentM = "Binance Pay";

        if(user) start(user);

        window.auth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(document.getElementById('adm-key').value === "admin786") { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); return; }
            if(!id || !pw) return alert("Credentials Missing");
            const uRef = doc(db, "users", id); const s = await getDoc(uRef);
            if(!login) {
                if(s.exists()) return alert("Identity Exists");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, bonusUsed: false });
                localStorage.setItem('nova_user', id); start(id);
            } else if(s.exists() && s.data().password === pw) {
                localStorage.setItem('nova_user', id); start(id);
            } else alert("Access Error");
        };

        function start(id) {
            user = id; document.getElementById('auth-screen').style.display='none'; document.getElementById('app').classList.remove('hidden');
            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                document.getElementById('main-bal').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "Node Yield: +$" + (data.daily || 0).toFixed(2) + " / 24H";
                if(data.bonusUsed) { document.getElementById('bonus-btn').disabled = true; document.getElementById('bonus-btn').innerText = "Claimed"; }
            });
            loadNodes(); loadHistory();
        }

        function loadNodes() {
        const c = document.getElementById('nodes-container'); 
        c.innerHTML = "";
        
        const plans = [
            {p: 20, d: 1.60, n: "Nova Antminer S19", img: "https://images.unsplash.com/photo-1624555130581-1d9cca783bc0?auto=format&fit=crop&q=80&w=400", spec: "110 TH/s | Air Cooled", hot: false},
            {p: 100, d: 8.50, n: "Nova Whatsminer M30", img: "https://images.unsplash.com/photo-1518770660439-4636190af475?auto=format&fit=crop&q=80&w=400", spec: "134 TH/s | Stable Power", hot: false},
            {p: 300, d: 27.00, n: "Nova Avalon 1246", img: "https://images.unsplash.com/photo-1591405351990-4726e331f141?auto=format&fit=crop&q=80&w=400", spec: "90 TH/s | Eco Mode", hot: false},
            {p: 600, d: 58.00, n: "Nova Liquid-Cool X5", img: "https://images.unsplash.com/photo-1639762681485-074b7f938ba0?auto=format&fit=crop&q=80&w=400", spec: "200 TH/s | Water Tech", hot: false},
            {p: 1000, d: 110.00, n: "Nova Master Cluster", img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?auto=format&fit=crop&q=80&w=400", spec: "Full ASIC Rack Set", hot: false},
            {p: 2500, d: 300.00, n: "Corporate Giant (Big Deal)", img: "https://images.unsplash.com/photo-1550751827-4bd374c3f58b?auto=format&fit=crop&q=80&w=400", spec: "Tier 4 Data Center", hot: true},
            {p: 5000, d: 650.00, n: "Institutional Whale", img: "https://images.unsplash.com/photo-1451187580459-43490279c0fa?auto=format&fit=crop&q=80&w=400", spec: "Dedicated Power Grid", hot: true},
            {p: 10000, d: 1400.00, n: "Global Hash Farm", img: "https://images.unsplash.com/photo-1563986768609-322da13575f3?auto=format&fit=crop&q=80&w=400", spec: "Mega-Hash Authority", hot: true}
        ];

        plans.forEach(plan => {
            c.innerHTML += `
            <div class="nova-card overflow-hidden relative ${plan.hot ? 'border-2 border-yellow-500' : ''}">
                ${plan.hot ? '<div class="absolute top-2 right-2 bg-yellow-500 text-white text-[8px] px-2 py-1 rounded-full font-black z-10 shadow-lg">HOT DEAL</div>' : ''}
                <img src="${plan.img}" class="node-img" alt="${plan.n}">
                <div class="p-4">
                    <div class="flex justify-between items-start mb-2">
                        <div>
                            <h3 class="text-xs font-black text-slate-800 uppercase">${plan.n}</h3>
                            <p class="text-[8px] text-slate-400 font-bold">${plan.spec}</p>
                        </div>
                        <p class="text-sm font-black text-slate-900">$${plan.p}</p>
                    </div>
                    <div class="flex justify-between items-center mt-4">
                        <div class="bg-green-50 px-2 py-1 rounded">
                            <p class="text-[9px] font-black text-green-600">Daily: +$${plan.d}</p>
                        </div>
                        <button onclick="buyNode(${plan.p},${plan.d})" class="btn-gold px-5 py-2">Deploy</button>
                    </div>
                </div>
            </div>`;
        });
    }
        window.buyNode = async (p, d) => {
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(s.data().balance < p) return alert("Low Liquidity");
            await updateDoc(uR, { balance: s.data().balance - p, daily: (s.data().daily || 0) + parseFloat(d) });
            alert("Mining Session Started!");
        };

        window.selD = (m) => { 
            currentM = m; document.querySelectorAll('.dep-m').forEach(e => e.style.borderColor = "#f1f5f9");
            event.currentTarget.style.borderColor = "#D4AF37";
            const addr = document.getElementById('dep-addr');
            if(m === 'Binance Pay') addr.innerText = "TTSxm4pBK26RB4vXaa3Uo3hqGa5HdhxBDR";
            else if(m === 'Trust Wallet') addr.innerText = "0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37";
            else addr.innerText = "0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2";
        };

        window.reqFin = async (type) => {
            const amt = parseFloat(document.getElementById(type=='Deposit'?'dep-amt':'wd-amt').value);
            const ref = document.getElementById(type=='Deposit'?'dep-tid':'wd-acc').value;
            if(!amt || !ref) return alert("Fill all fields");
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(type === 'Withdrawal' && s.data().balance < amt) return alert("Insufficient Balance");
            if(type === 'Withdrawal') await updateDoc(uR, { balance: s.data().balance - amt });
            await addDoc(collection(db, "logs"), { user, type, amount: amt, status: 'Pending', time: Date.now(), method: type=='Deposit'?currentM:document.getElementById('wd-method').value, ref });
            alert("Protocol Submitted!"); closeModal(type=='Deposit'?'dep-modal':'wd-modal');
        };

                function loadHistory() {
            // Simplified query to avoid index errors
            onSnapshot(query(collection(db, "logs"), where("user", "==", user)), s => {
                const c = document.getElementById('hist-container'); 
                c.innerHTML = "";
                
                let logs = [];
                s.forEach(d => logs.push(d.data()));

                // Manual sorting in JavaScript so it works 100% without extra Firebase settings
                logs.sort((a, b) => b.time - a.time);

                logs.forEach(r => {
                    c.innerHTML += `<div class="nova-card p-4 flex justify-between items-center">
                        <div>
                            <p class="text-[8px] font-black text-slate-400 uppercase">${r.type} (${r.method})</p>
                            <p class="text-xs font-bold">$${r.amount.toFixed(2)}</p>
                        </div>
                        <span class="text-[9px] font-bold px-3 py-1 rounded-full ${r.status=='Success'?'badge-success':'badge-pending'}">${r.status}</span>
                    </div>`;
                });
            });
        }

        window.loadAdmin = () => {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const c = document.getElementById('adm-requests'); c.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    c.innerHTML += `<div class="nova-card p-4 bg-white border-l-4 border-yellow-500">
                        <p class="text-[10px] font-black">${r.user} | $${r.amount}</p>
                        <div class="flex gap-2 mt-3">
                            <button onclick="admAct('${d.id}','${r.user}',${r.amount},'${r.type}','Success')" class="bg-green-500 text-white text-[8px] flex-1 py-2 rounded">Approve</button>
                            <button onclick="admAct('${d.id}','${r.user}',${r.amount},'${r.type}','Rejected')" class="bg-red-500 text-white text-[8px] flex-1 py-2 rounded">Reject</button>
                        </div>
                    </div>`;
                });
            });
        };

        window.admAct = async (id, u, a, t, s) => {
            if(s === 'Success' && t === 'Deposit') { const uR = doc(db, "users", u); const us = await getDoc(uR); await updateDoc(uR, { balance: (us.data().balance || 0) + a }); }
            if(s === 'Rejected' && t === 'Withdrawal') { const uR = doc(db, "users", u); const us = await getDoc(uR); await updateDoc(uR, { balance: (us.data().balance || 0) + a }); }
            await updateDoc(doc(db, "logs", id), { status: s });
        };

        window.claimBonus = async () => {
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(s.data().bonusUsed) return;
            const b = (Math.random() * 0.4 + 0.1).toFixed(2);
            await updateDoc(uR, { balance: s.data().balance + parseFloat(b), bonusUsed: true });
            alert("Bonus Claimed: $" + b);
        };

        window.injectBal = async () => { 
            const uR = doc(db, "users", document.getElementById('adm-uid').value.toLowerCase().trim()); 
            const s = await getDoc(uR); if(s.exists()) { await updateDoc(uR, { balance: s.data().balance + parseFloat(document.getElementById('adm-amt').value) }); alert("Updated!"); }
        };

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
