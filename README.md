<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Institutional Mining Protocol</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Syncopate:wght@400;700&family=Inter:wght@300;500;900&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --accent: #0088FF; --bg: #030303; }
        body { background: var(--bg); color: white; font-family: 'Inter', sans-serif; overflow-x: hidden; }
        .prime-font { font-family: 'Syncopate', sans-serif; }
        
        /* Premium Background */
        .bg-mesh { position: fixed; inset: 0; background: radial-gradient(circle at 50% 50%, #111 0%, #030303 100%); z-index: -1; }
        .glow-sphere { position: absolute; width: 300px; height: 300px; background: var(--gold); filter: blur(150px); opacity: 0.05; border-radius: 50%; }

        /* Modern Components */
        .glass-panel { background: rgba(20, 20, 20, 0.6); backdrop-filter: blur(20px); border: 1px solid rgba(255,255,255,0.05); border-radius: 32px; }
        .btn-prime { background: white; color: black; font-weight: 900; letter-spacing: 1px; border-radius: 16px; transition: 0.3s; text-transform: uppercase; font-size: 11px; }
        .btn-prime:active { transform: scale(0.95); background: var(--gold); }
        
        /* Stats Ticker */
        .ticker-wrap { background: rgba(212, 175, 55, 0.1); border-top: 1px solid rgba(212, 175, 55, 0.2); border-bottom: 1px solid rgba(212, 175, 55, 0.2); }
        @keyframes scroll { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }
        .ticker-move { animation: scroll 20s linear infinite; white-space: nowrap; }

        /* Mining Rig Cards */
        .rig-card { position: relative; overflow: hidden; border: 1px solid rgba(255,255,255,0.05); transition: 0.4s; }
        .rig-card:hover { border-color: var(--gold); background: rgba(212, 175, 55, 0.02); }
        .status-dot { width: 8px; height: 8px; border-radius: 50%; background: #00FF88; box-shadow: 0 0 10px #00FF88; }
        
        .nav-bar { position: fixed; bottom: 0; left: 0; right: 0; background: rgba(0,0,0,0.9); backdrop-filter: blur(25px); border-top: 1px solid #111; display: flex; justify-content: space-around; padding: 20px; z-index: 5000; }
        .nav-item { color: #555; transition: 0.3s; font-size: 10px; font-weight: 900; }
        .nav-item.active { color: white; }
    </style>
</head>
<body>
    <div class="bg-mesh"></div>
    <div class="glow-sphere" style="top: -100px; right: -100px;"></div>

    <div class="ticker-wrap py-2 overflow-hidden sticky top-0 z-[100]">
        <div class="ticker-move flex gap-8">
            <span class="text-[9px] font-black uppercase text-gray-400">System Status: <span class="text-green-500">Operational</span></span>
            <span class="text-[9px] font-black uppercase text-gray-400">Latest Deposit: <span class="text-white">$1,450.00 via USDT</span></span>
            <span class="text-[9px] font-black uppercase text-gray-400">Withdrawal: <span class="text-white">ID 48210 processed $45.00</span></span>
            <span class="text-[9px] font-black uppercase text-gray-400">Active Miners: <span class="text-white">12,402 Units</span></span>
        </div>
    </div>

    <div id="app" class="hidden pb-32">
        <header class="p-6 flex justify-between items-center">
            <h1 class="prime-font text-xl font-bold tracking-tighter">NOVA<span class="text-[#D4AF37]">.</span>PRO</h1>
            <div onclick="logout()" class="glass-panel px-4 py-2 border-white/10"><i class="fa-solid fa-user-shield text-xs"></i></div>
        </header>

        <div id="dash-screen" class="screen active-screen px-6">
            <div class="glass-panel p-10 mb-8 relative overflow-hidden bg-gradient-to-br from-[#111] to-black">
                <p class="text-[10px] text-gray-500 font-black tracking-[5px] uppercase mb-4">Core Valuation</p>
                <h1 id="main-bal" class="text-6xl font-black tracking-tighter mb-4">$0.00</h1>
                <div class="flex items-center gap-3">
                    <div class="status-dot animate-pulse"></div>
                    <p id="v-daily" class="text-[10px] font-black text-gray-400 uppercase tracking-widest"></p>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-10">
                <button onclick="openModal('dep-modal')" class="glass-panel p-6 flex flex-col gap-2 items-center border-[#D4AF37]/20">
                    <i class="fa-solid fa-bolt-lightning text-[#D4AF37]"></i>
                    <span class="text-[10px] font-black uppercase">Initialize</span>
                </button>
                <button onclick="openModal('wd-modal')" class="glass-panel p-6 flex flex-col gap-2 items-center">
                    <i class="fa-solid fa-paper-plane text-white"></i>
                    <span class="text-[10px] font-black uppercase">Extract</span>
                </button>
            </div>

            <div class="glass-panel p-8">
                <h2 class="text-xs font-black uppercase tracking-widest mb-6 border-l-4 border-[#D4AF37] pl-4">Company Protocol</h2>
                <p class="text-[11px] text-gray-500 leading-relaxed font-medium">NOVA is a high-frequency algorithmic mining venture. We leverage institutional liquidity to optimize SHA-256 cloud distribution across global clusters.</p>
                <div class="mt-8 grid grid-cols-2 gap-4">
                    <div class="p-4 bg-white/5 rounded-2xl"><p class="text-[8px] text-gray-600 font-black uppercase">Global Rank</p><p class="text-sm font-black">#Tier-1</p></div>
                    <div class="p-4 bg-white/5 rounded-2xl"><p class="text-[8px] text-gray-600 font-black uppercase">Liquidity</p><p class="text-sm font-black text-green-500">Secured</p></div>
                </div>
            </div>
        </div>

        <div id="mine-screen" class="screen px-6">
            <h2 class="prime-font text-3xl font-bold mb-2">MINING HUB</h2>
            <p class="text-[9px] text-gray-500 font-black tracking-widest mb-10 uppercase">Quantum Hardware Fleet</p>
            <div id="nodes-container" class="space-y-6"></div>
        </div>
    </div>

    <div id="auth-screen" class="min-h-screen flex flex-col p-10 justify-center">
        <h1 class="prime-font text-6xl font-black mb-2 tracking-tighter text-white">NOVA</h1>
        <p class="text-[10px] text-gray-600 font-black tracking-[6px] uppercase mb-16">The Future of Capital</p>
        <div class="space-y-4">
            <input type="text" id="u-email" placeholder="Terminal ID" class="nova-input">
            <input type="password" id="u-pass" placeholder="Authorization Key" class="nova-input">
            <input type="text" id="adm-key" placeholder="System Override" class="hidden nova-input border-[#D4AF37]/50">
            <button onclick="auth()" class="w-full btn-prime py-5 mt-4">Initialize Terminal</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[10px] mt-10 text-gray-600 font-black uppercase cursor-pointer">Register Identity</p>
        </div>
    </div>

    <nav class="nav-bar">
        <div onclick="nav('dash-screen')" class="nav-item active" id="nav-dash"><i class="fa-solid fa-home text-lg"></i></div>
        <div onclick="nav('mine-screen')" class="nav-item" id="nav-mine"><i class="fa-solid fa-microchip text-lg"></i></div>
        <div onclick="openModal('dep-modal')" class="nav-item"><i class="fa-solid fa-wallet text-lg"></i></div>
    </nav>

    <script type="module">
        // [Standard High-Performance Script Logic]
        function renderNodes() {
            const container = document.getElementById('nodes-container');
            container.innerHTML = "";
            for(let i=1; i<=20; i++) {
                let p = i===1?15:(i*100); let y = (p*0.09).toFixed(2);
                container.innerHTML += `
                    <div class="glass-panel rig-card p-6 flex flex-col gap-6">
                        <div class="flex justify-between items-start">
                            <div class="flex items-center gap-3">
                                <div class="w-10 h-10 rounded-xl bg-[#D4AF37]/10 flex items-center justify-center text-[#D4AF37]"><i class="fa-solid fa-server"></i></div>
                                <div><p class="text-[9px] font-black text-gray-600 uppercase">Unit Model</p><h4 class="text-sm font-black uppercase">AX-NODE ${i}</h4></div>
                            </div>
                            <span class="text-[8px] bg-green-500/10 text-green-500 px-3 py-1 rounded-full font-black uppercase">Active</span>
                        </div>
                        <div class="grid grid-cols-2 border-y border-white/5 py-4">
                            <div><p class="text-[8px] text-gray-600 font-black uppercase">Yield Rate</p><p class="text-xs font-black text-green-500">+$${y} / Day</p></div>
                            <div class="text-right"><p class="text-[8px] text-gray-600 font-black uppercase">Power Cost</p><p class="text-xs font-black uppercase">0.02% Fee</p></div>
                        </div>
                        <div class="flex justify-between items-center">
                            <p class="text-2xl font-black">$${p}</p>
                            <button onclick="buyNode(${p},${y})" class="btn-prime px-8 py-3">Deploy Unit</button>
                        </div>
                    </div>`;
            }
        }
        // [Plus all the finance and admin control logic from previous versions...]
    </script>
</body>
</html>
