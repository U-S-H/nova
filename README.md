<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Institutional Terminal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root { --gold: #D4AF37; --bg: #050505; }
        body { background: var(--bg); color: white; font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; overflow-x: hidden; }
        .app-card { background: rgba(255, 255, 255, 0.03); border: 1px solid rgba(255, 255, 255, 0.05); border-radius: 24px; position: relative; overflow: hidden; }
        .gold-glow { box-shadow: 0 0 25px rgba(212, 175, 55, 0.15); border: 1px solid rgba(212, 175, 55, 0.3); }
        .nav-bar { position: fixed; bottom: 0; left: 0; right: 0; background: rgba(10, 10, 10, 0.98); backdrop-filter: blur(25px); border-top: 1px solid #1a1a1a; padding: 15px 10px; z-index: 1000; display: flex; justify-content: space-around; }
        .nav-item { display: flex; flex-direction: column; align-items: center; gap: 5px; color: #444; font-size: 9px; font-weight: 800; transition: 0.3s; }
        .nav-item.active { color: var(--gold); transform: translateY(-3px); }
        .icon-circle { width: 52px; height: 52px; background: rgba(255, 255, 255, 0.02); border-radius: 18px; display: flex; align-items: center; justify-content: center; font-size: 18px; border: 1px solid rgba(255, 255, 255, 0.05); transition: 0.2s; }
        .icon-circle:active { transform: scale(0.9); background: var(--gold); color: black; }
        .screen { display: none; opacity: 0; transform: translateY(15px); }
        .active-screen { display: block; opacity: 1; transform: translateY(0); transition: 0.4s ease-out; }
        .nova-input { background: #0c0c0c; border: 1px solid #1a1a1a; width: 100%; padding: 18px; border-radius: 20px; outline: none; color: white; font-size: 14px; }
        .modal { display: none; position: fixed; inset: 0; background: black; z-index: 2000; padding: 25px; padding-top: 50px; overflow-y: auto; }
        .node-img { height: 110px; width: 100%; object-fit: cover; opacity: 0.4; filter: grayscale(30%); }
    </style>
</head>
<body>

    <div id="auth-screen" class="min-h-screen flex flex-col items-center justify-center p-8 active-screen">
        <h1 onclick="handleTaps()" class="text-6xl font-black tracking-tighter mb-2">NOVA<span class="text-[#D4AF37]">.</span></h1>
        <p class="text-[8px] tracking-[5px] text-gray-600 uppercase mb-12">Global Asset Protocol</p>
        <div class="app-card p-7 w-full max-w-sm">
            <input type="text" id="u-email" placeholder="Terminal ID" class="nova-input mb-4">
            <input type="password" id="u-pass" placeholder="Passkey" class="nova-input mb-6">
            <input type="text" id="adm-key" placeholder="Admin Override" class="hidden nova-input border-[#D4AF37] mb-4">
            <button onclick="auth()" class="w-full bg-[#D4AF37] text-black py-4 rounded-2xl font-black text-xs tracking-widest">INITIALIZE SESSION</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[10px] mt-8 text-gray-500 uppercase font-bold cursor-pointer">Register New Protocol</p>
        </div>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-black z-[9999] p-6 overflow-y-auto">
        <div class="flex justify-between items-center mb-8 border-b border-gray-900 pb-4"><h2 class="text-[#D4AF37] font-black uppercase">Admin Terminal</h2><button onclick="location.reload()" class="text-red-500 font-black">EXIT</button></div>
        <div id="adm-list" class="space-y-4"></div>
    </div>

    <div id="app" class="hidden">
        <header class="p-6 flex justify-between items-center">
            <div class="flex items-center gap-3">
                <div class="w-11 h-11 app-card flex items-center justify-center text-[#D4AF37] font-black border-[#D4AF37]/20">N</div>
                <div><p class="text-[8px] text-gray-600 font-bold uppercase">Authorized Access</p><p id="user-id-txt" class="text-xs font-black text-gray-300"></p></div>
            </div>
            <i onclick="logout()" class="fa-solid fa-power-off text-gray-800 text-lg"></i>
        </header>

        <div id="dash-screen" class="screen active-screen p-6 pb-32">
            <div class="app-card p-10 mb-10 gold-glow relative text-center">
                <p class="text-[9px] text-gray-500 font-black tracking-[3px] mb-3 uppercase">Total Liquidity</p>
                <h1 id="main-bal" class="text-5xl font-black tracking-tighter mb-4">$0.00</h1>
                <div class="inline-flex items-center gap-2 bg-green-500/10 px-4 py-1.5 rounded-full"><span class="w-1.5 h-1.5 bg-green-500 rounded-full animate-pulse"></span><p id="v-daily" class="text-green-500 text-[9px] font-black">+$0.00 Daily</p></div>
            </div>

            <div class="flex justify-between mb-12 gap-2">
                <div onclick="openModal('dep-modal')" class="flex flex-col items-center gap-3 flex-1">
                    <div class="icon-circle text-green-500"><i class="fa-solid fa-plus"></i></div>
                    <span class="text-[9px] font-black text-gray-500">DEPOSIT</span>
                </div>
                <div onclick="openModal('wd-modal')" class="flex flex-col items-center gap-3 flex-1">
                    <div class="icon-circle text-red-500"><i class="fa-solid fa-minus"></i></div>
                    <span class="text-[9px] font-black text-gray-500">WITHDRAW</span>
                </div>
                <div onclick="openModal('hist-modal')" class="flex flex-col items-center gap-3 flex-1">
                    <div class="icon-circle text-[#D4AF37]"><i class="fa-solid fa-bolt-lightning"></i></div>
                    <span class="text-[9px] font-black text-gray-500">HISTORY</span>
                </div>
                <div onclick="openModal('support-modal')" class="flex flex-col items-center gap-3 flex-1">
                    <div class="icon-circle text-blue-500"><i class="fa-solid fa-headset"></i></div>
                    <span class="text-[9px] font-black text-gray-500">SUPPORT</span>
                </div>
            </div>

            <h3 class="text-[10px] font-black text-gray-700 mb-5 tracking-[2px] uppercase">Live Ledger</h3>
            <div id="hist-list-mini" class="space-y-3"></div>
        </div>

        <div id="mine-screen" class="screen p-6 pb-32">
            <h2 class="text-2xl font-black mb-1 gold-text uppercase">Mining Core</h2>
            <p class="text-[9px] text-gray-600 mb-8 uppercase tracking-widest">Active High-Frequency Nodes</p>
            
            <div class="space-y-6">
                <div class="app-card border-l-2 border-blue-500">
                    <img src="https://images.unsplash.com/photo-1639762681485-074b7f938ba0?q=80&w=400" class="node-img">
                    <div class="p-5 flex justify-between items-center">
                        <div><p class="text-[10px] font-black text-blue-400 uppercase">Genesis Node</p><p class="text-xl font-black">$15.00</p><p class="text-[8px] text-gray-500">Profit: $0.80/day</p></div>
                        <button onclick="buyNode(15, 0.80)" class="bg-white text-black px-5 py-2.5 rounded-xl text-[9px] font-black">ACTIVATE</button>
                    </div>
                </div>
                <div class="app-card border-l-2 border-[#D4AF37]">
                    <img src="https://images.unsplash.com/photo-1644088379091-d574269d422f?q=80&w=400" class="node-img">
                    <div class="p-5 flex justify-between items-center">
                        <div><p class="text-[10px] font-black gold-text uppercase">Vortex Node</p><p class="text-xl font-black">$100.00</p><p class="text-[8px] text-gray-500">Profit: $6.00/day</p></div>
                        <button onclick="buyNode(100, 6.00)" class="bg-white text-black px-5 py-2.5 rounded-xl text-[9px] font-black">ACTIVATE</button>
                    </div>
                </div>
                <div class="app-card border-l-2 border-purple-500">
                    <img src="https://images.unsplash.com/photo-1639322537228-f710d846310a?q=80&w=400" class="node-img">
                    <div class="p-5 flex justify-between items-center">
                        <div><p class="text-[10px] font-black text-purple-400 uppercase">Quantum Rig</p><p class="text-xl font-black">$500.00</p><p class="text-[8px] text-gray-500">Profit: $32.00/day</p></div>
                        <button onclick="buyNode(500, 32.00)" class="bg-white text-black px-5 py-2.5 rounded-xl text-[9px] font-black">ACTIVATE</button>
                    </div>
                </div>
                <div class="app-card border-l-2 border-red-500">
                    <img src="https://images.unsplash.com/photo-1518546305927-5a555bb7020d?q=80&w=400" class="node-img">
                    <div class="p-5 flex justify-between items-center">
                        <div><p class="text-[10px] font-black text-red-500 uppercase">Titan Server</p><p class="text-xl font-black">$5,000.00</p><p class="text-[8px] text-gray-500">Profit: $350.00/day</p></div>
                        <button onclick="buyNode(5000, 350.0)" class="bg-white text-black px-5 py-2.5 rounded-xl text-[9px] font-black">ACTIVATE</button>
                    </div>
                </div>
                <div class="app-card gold-glow border-l-2 border-green-500">
                    <img src="https://images.unsplash.com/photo-1624365169192-6bc825313bc5?q=80&w=400" class="node-img opacity-70">
                    <div class="p-5 flex justify-between items-center">
                        <div><p class="text-[10px] font-black text-green-400 uppercase">Supreme Master</p><p class="text-xl font-black">$20,000.00</p><p class="text-[8px] text-gray-300">Profit: $1,500.00/day</p></div>
                        <button onclick="buyNode(20000, 1500)" class="bg-[#D4AF37] text-black px-5 py-2.5 rounded-xl text-[9px] font-black">ACTIVATE</button>
                    </div>
                </div>
            </div>
        </div>

        <div id="office-screen" class="screen p-6 pb-32">
            <div class="app-card p-10 text-center mb-8">
                <div class="w-16 h-16 bg-[#D4AF37]/10 rounded-full flex items-center justify-center mx-auto mb-6"><i class="fa-solid fa-users-rays text-2xl text-[#D4AF37]"></i></div>
                <h2 class="font-black text-lg mb-2">Referral Network</h2>
                <p class="text-[9px] text-gray-500 uppercase tracking-widest mb-6">Expand the protocol & earn 10%</p>
                <p id="ref-link" class="text-[9px] font-mono bg-black/50 p-4 rounded-xl border border-white/5 select-all text-gray-400"></p>
            </div>
        </div>

        <div id="dep-modal" class="modal">
            <div onclick="closeModal('dep-modal')" class="mb-10 text-gray-500 font-bold text-xs"><i class="fa-solid fa-arrow-left mr-2"></i> BACK</div>
            <h2 class="text-3xl font-black mb-8 gold-text uppercase">Deposit Assets</h2>
            <div class="space-y-4 mb-8">
                <div class="app-card p-4 flex justify-between items-center"><span>TRUST (BEP20)</span><button onclick="copyAddr('0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37',this)" class="text-[#D4AF37] font-black text-[10px]">COPY</button></div>
                <div class="app-card p-4 flex justify-between items-center"><span>META (ERC20)</span><button onclick="copyAddr('0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2',this)" class="text-[#D4AF37] font-black text-[10px]">COPY</button></div>
            </div>
            <input type="number" id="dep-amt" placeholder="Deposit Amount ($)" class="nova-input mb-4">
            <input type="text" id="dep-tid" placeholder="Transaction Hash / TID" class="nova-input mb-4">
            <input type="file" id="proof-img" class="hidden" onchange="processImg(this)">
            <button onclick="document.getElementById('proof-img').click()" class="w-full py-5 border border-dashed border-gray-800 rounded-2xl text-[10px] text-gray-600 mb-6 font-black uppercase tracking-widest">Attach Proof</button>
            <button onclick="requestFin('Deposit')" class="w-full bg-[#D4AF37] text-black py-5 rounded-2xl font-black text-xs">CONFIRM DEPOSIT</button>
        </div>

        <div id="wd-modal" class="modal">
            <div onclick="closeModal('wd-modal')" class="mb-10 text-gray-500 font-bold text-xs"><i class="fa-solid fa-arrow-left mr-2"></i> BACK</div>
            <h2 class="text-3xl font-black mb-8 text-red-500 uppercase">Withdrawal</h2>
            <input type="number" id="wd-amt" placeholder="Amount ($)" class="nova-input mb-4">
            <select id="wd-method" class="nova-input mb-4 bg-black text-gray-500 uppercase font-black text-[10px]">
                <option value="Trust Wallet">Trust Wallet (BEP20)</option>
                <option value="MetaMask">MetaMask (ERC20)</option>
                <option value="Binance">Binance (TRC20)</option>
            </select>
            <input type="text" id="wd-addr" placeholder="Recipient Wallet Address" class="nova-input mb-8">
            <button onclick="requestFin('Withdrawal')" class="w-full bg-white text-black py-5 rounded-2xl font-black text-xs uppercase tracking-[3px]">Send Assets</button>
        </div>

        <div id="hist-modal" class="modal"><div onclick="closeModal('hist-modal')" class="mb-10 text-gray-500 font-bold text-xs"><i class="fa-solid fa-arrow-left mr-2"></i> BACK</div><h2 class="text-2xl font-black mb-8 gold-text uppercase">Activity History</h2><div id="hist-list-full" class="space-y-4"></div></div>

        <nav class="nav-bar">
            <div onclick="nav('dash-screen')" class="nav-item active" id="nav-dash"><i class="fa-solid fa-house-chimney text-xl"></i><span>HOME</span></div>
            <div onclick="nav('mine-screen')" class="nav-item" id="nav-mine"><i class="fa-solid fa-microchip text-xl"></i><span>MINING</span></div>
            <div onclick="nav('office-screen')" class="nav-item" id="nav-office"><i class="fa-solid fa-network-wired text-xl"></i><span>OFFICE</span></div>
        </nav>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);
        let activeUser = null; let base64Img = ""; window.taps = 0; window.isLogin = true;

        // CHECK PERSISTENCE ON LOAD
        window.onload = () => {
            const saved = localStorage.getItem('nova_session');
            if(saved) start(saved);
        };

        window.handleTaps = () => { taps++; if(taps >= 6) document.getElementById('adm-key').classList.remove('hidden'); };
        window.toggleM = () => { isLogin = !isLogin; document.getElementById('m-text').innerText = isLogin ? "Register New Protocol" : "Back to Login"; };

        window.auth = async () => {
            const id = document.getElementById('u-email').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            if(document.getElementById('adm-key').value === "nov786") { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); return; }
            if(!id || !pw) return alert("Required fields missing");
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(!isLogin) {
                if(snap.exists()) return alert("Identity already exists");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, time: Date.now() }); 
                localStorage.setItem('nova_session', id); start(id);
            } else {
                if(snap.exists() && snap.data().password === pw) { localStorage.setItem('nova_session', id); start(id); }
                else alert("Authentication Failed");
            }
        };

        function start(id) {
            activeUser = id; document.getElementById('auth-screen').classList.remove('active-screen');
            setTimeout(() => { document.getElementById('auth-screen').classList.add('hidden'); document.getElementById('app').classList.remove('hidden'); }, 400);
            document.getElementById('user-id-txt').innerText = id; document.getElementById('ref-link').innerText = window.location.origin + window.location.pathname + "?ref=" + id;
            onSnapshot(doc(db, "users", id), d => {
                document.getElementById('main-bal').innerText = "$" + (d.data().balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "+ $" + (d.data().daily || 0).toFixed(2) + " Daily";
            });
            loadHistory();
            setInterval(() => { let el = document.getElementById('main-bal'); let val = parseFloat(el.innerText.replace('$','')); el.innerText = "$" + (val + 0.0001).toFixed(4); }, 3000);
        }

        window.processImg = (el) => { const reader = new FileReader(); reader.onload = () => base64Img = reader.result; reader.readAsDataURL(el.files[0]); };

        window.requestFin = async (type) => {
            const amtId = type === 'Deposit' ? 'dep-amt' : 'wd-amt';
            const amt = parseFloat(document.getElementById(amtId).value);
            const uRef = doc(db, "users", activeUser); const snap = await getDoc(uRef);
            if(type === 'Withdrawal') {
                const method = document.getElementById('wd-method').value; const addr = document.getElementById('wd-addr').value;
                if(!amt || !addr || snap.data().balance < amt) return alert("Insufficient Liquidity");
                await addDoc(collection(db, "logs"), { user: activeUser, amount: amt, type, method, addr, status: 'Pending', time: Date.now() });
                await updateDoc(uRef, { balance: snap.data().balance - amt });
                closeModal('wd-modal');
            } else {
                const tid = document.getElementById('dep-tid').value;
                if(!amt || !tid) return alert("TID Verification Required");
                await addDoc(collection(db, "logs"), { user: activeUser, amount: amt, type, tid, status: 'Pending', proof: base64Img, time: Date.now() });
                closeModal('dep-modal');
            }
            alert("Transmission Successful"); base64Img = "";
        };

        window.buyNode = async (p, d) => {
            const r = doc(db, "users", activeUser); const s = await getDoc(r);
            if(s.data().balance < p) return alert("Liquidity Error");
            await updateDoc(r, { balance: s.data().balance - p, daily: (s.data().daily || 0) + d });
            alert("Node Synchronized!");
        };

        function loadHistory() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", activeUser)), s => {
                const hFull = document.getElementById('hist-list-full'); const hMini = document.getElementById('hist-list-mini');
                hFull.innerHTML = ""; hMini.innerHTML = "";
                let arr = []; s.forEach(d => arr.push(d.data()));
                arr.sort((a,b) => b.time - a.time).forEach((t, i) => {
                    const card = `<div class="app-card p-5 flex justify-between items-center text-[10px]">
                        <div><p class="font-black uppercase">${t.type}</p><p class="text-gray-600 text-[8px]">${new Date(t.time).toLocaleDateString()}</p></div>
                        <div class="text-right"><p class="font-black">$${t.amount.toFixed(2)}</p><p class="${t.status=='Pending'?'text-yellow-600':'text-green-600'} font-bold uppercase text-[8px]">${t.status}</p></div>
                    </div>`;
                    hFull.innerHTML += card; if(i < 3) hMini.innerHTML += card;
                });
            });
        }

        function loadAdmin() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const al = document.getElementById('adm-list'); al.innerHTML = "";
                s.forEach(d => {
                    const r = d.data(); const isWD = r.type === 'Withdrawal';
                    al.innerHTML += `<div class="app-card p-5 border-l-4 ${isWD ? 'border-red-500' : 'border-green-500'}">
                        <div class="flex justify-between mb-2"><span class="text-[10px] font-bold text-gray-400">${r.user}</span><span class="font-black text-lg">$${r.amount}</span></div>
                        <div class="text-[8px] bg-black/40 p-3 rounded-xl mb-4 font-mono">
                            <p>TYPE: ${r.type} | METHOD: ${r.method || 'N/A'}</p>
                            ${isWD ? `<p class="text-orange-400">TARGET: ${r.addr}</p>` : `<p class="text-blue-400">HASH: ${r.tid}</p>`}
                        </div>
                        ${!isWD && r.proof ? `<button onclick="window.open('${r.proof}')" class="w-full py-2 mb-3 bg-white/5 rounded-xl text-[8px] uppercase font-bold">Inspect Proof</button>` : ''}
                        <button onclick="approve('${d.id}','${r.user}',${r.amount},'${r.type}')" class="w-full bg-[#D4AF37] text-black font-black p-4 rounded-xl text-[10px] uppercase">Verify & Release</button>
                    </div>`;
                });
            });
        }

        window.approve = async (id, u, a, t) => {
            const uR = doc(db, "users", u); const us = await getDoc(uR);
            if(t === 'Deposit') await updateDoc(uR, { balance: (us.data().balance || 0) + a });
            await updateDoc(doc(db, "logs", id), { status: 'Verified' });
        };

        window.openModal = (id) => { document.getElementById(id).style.display = 'block'; document.body.style.overflow = 'hidden'; };
        window.closeModal = (id) => { document.getElementById(id).style.display = 'none'; document.body.style.overflow = 'auto'; };
        window.copyAddr = (a, b) => { navigator.clipboard.writeText(a); b.innerText = "DONE"; setTimeout(() => b.innerText = "COPY", 2000); };
        window.nav = (id) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            setTimeout(() => {
                document.querySelectorAll('.screen').forEach(s => s.style.display = 'none');
                document.getElementById(id).style.display = 'block';
                setTimeout(() => document.getElementById(id).classList.add('active-screen'), 50);
                const map = {'dash-screen':'nav-dash','mine-screen':'nav-mine','office-screen':'nav-office'};
                document.getElementById(map[id]).classList.add('active');
            }, 200);
        };
        window.logout = () => { localStorage.removeItem('nova_session'); location.reload(); };
    </script>
</body>
</html>
