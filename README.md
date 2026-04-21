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
            background: var(--bg); 
            color: white; 
            font-family: 'Plus Jakarta Sans', sans-serif;
            overflow-x: hidden;
            background-image: radial-gradient(circle at 50% -20%, #1a1a10 0%, #030303 80%);
        }
        
        .neo-card { 
            background: rgba(255, 255, 255, 0.03);
            backdrop-filter: blur(15px);
            border: 1px solid rgba(212, 175, 55, 0.15);
            border-radius: 32px;
        }

        .gold-text {
            background: linear-gradient(135deg, #FFF5D1 0%, #D4AF37 50%, #A67C00 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .btn-elite {
            background: linear-gradient(135deg, #D4AF37, #F2CC60);
            box-shadow: 0 10px 20px rgba(212, 175, 55, 0.2);
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .btn-elite:active { transform: scale(0.95); box-shadow: 0 5px 10px rgba(212, 175, 55, 0.1); }

        .nav-float {
            position: fixed;
            bottom: 25px;
            left: 50%;
            transform: translateX(-50%);
            width: 90%;
            max-width: 400px;
            background: rgba(10, 10, 10, 0.8);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 25px;
            padding: 15px;
            z-index: 1000;
        }

        .screen { display: none; opacity: 0; transform: translateY(20px); transition: all 0.5s ease; }
        .active-screen { display: block; opacity: 1; transform: translateY(0); }

        /* Custom Input */
        .nova-input {
            background: rgba(255, 255, 255, 0.02);
            border: 1px solid rgba(255, 255, 255, 0.05);
            transition: 0.3s;
        }
        .nova-input:focus { border-color: var(--gold); background: rgba(255, 255, 255, 0.05); }

        @keyframes pulse-gold {
            0% { box-shadow: 0 0 0 0 rgba(212, 175, 55, 0.4); }
            70% { box-shadow: 0 0 0 15px rgba(212, 175, 55, 0); }
            100% { box-shadow: 0 0 0 0 rgba(212, 175, 55, 0); }
        }
        .mining-active { animation: pulse-gold 2s infinite; }
    </style>
</head>
<body>

    <div id="auth-screen" class="min-h-screen flex flex-col items-center justify-center p-8 active-screen">
        <div class="text-center mb-12">
            <h1 onclick="handleTaps()" class="text-7xl font-black tracking-tighter gold-text">NOVA</h1>
            <p class="text-[9px] tracking-[8px] text-gray-500 uppercase mt-4">Future of Digital Assets</p>
        </div>
        
        <div class="neo-card p-8 w-full max-w-sm">
            <div class="space-y-4">
                <input type="text" id="u-email" placeholder="Terminal ID" class="nova-input w-full p-5 rounded-2xl outline-none text-sm">
                <input type="password" id="u-pass" placeholder="Security Key" class="nova-input w-full p-5 rounded-2xl outline-none text-sm">
                <input type="text" id="adm-key" placeholder="Admin Override" class="hidden nova-input w-full p-5 border-[#D4AF37] rounded-2xl outline-none text-sm">
                <button onclick="auth()" class="btn-elite w-full p-5 rounded-2xl text-black font-black text-sm tracking-widest mt-4">LOGIN TERMINAL</button>
            </div>
            <p onclick="toggleM()" id="m-text" class="text-center text-[10px] mt-8 text-gray-600 uppercase tracking-widest cursor-pointer">Request Access</p>
        </div>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-black z-[9999] p-8 overflow-y-auto">
        <div class="flex justify-between items-center mb-10">
            <h2 class="gold-text text-2xl font-black">COMMAND CENTER</h2>
            <button onclick="location.reload()" class="bg-white/5 p-3 rounded-full"><i class="fa-solid fa-xmark"></i></button>
        </div>
        <div id="adm-list" class="space-y-6"></div>
    </div>

    <div id="app" class="hidden pb-40">
        <header class="p-8 flex justify-between items-center">
            <div class="flex items-center gap-4">
                <div class="w-12 h-12 neo-card flex items-center justify-center mining-active">
                    <i class="fa-solid fa-bolt-lightning text-[#D4AF37]"></i>
                </div>
                <div>
                    <p class="text-[10px] text-gray-500 font-bold uppercase tracking-tighter">Verified User</p>
                    <h2 id="user-id-txt" class="font-black text-lg">---</h2>
                </div>
            </div>
            <button onclick="logout()" class="w-10 h-10 rounded-full bg-white/5 border border-white/10"><i class="fa-solid fa-power-off text-red-500 text-xs"></i></button>
        </header>

        <div id="dash-screen" class="screen active-screen p-8">
            <div class="neo-card p-10 mb-8 relative overflow-hidden">
                <div class="absolute -right-10 -top-10 w-40 h-40 bg-[#D4AF37] opacity-5 blur-[50px] rounded-full"></div>
                <p class="text-[10px] text-gray-500 font-bold tracking-[3px] mb-2 uppercase">Current Liquidity</p>
                <h1 id="main-bal" class="text-6xl font-black tracking-tighter mb-6">$0.00</h1>
                
                <div class="grid grid-cols-2 gap-4">
                    <div class="bg-white/5 p-4 rounded-2xl border border-white/5">
                        <p class="text-[9px] text-gray-500 mb-1">PROFIT 24H</p>
                        <p id="v-daily" class="text-green-400 font-black">+$0.00</p>
                    </div>
                    <div class="bg-white/5 p-4 rounded-2xl border border-white/5">
                        <p class="text-[9px] text-gray-500 mb-1">NETWORK</p>
                        <p class="text-blue-400 font-black">GLOBAL</p>
                    </div>
                </div>
            </div>

            <div class="flex items-center justify-between mb-6">
                <h3 class="text-xs font-black tracking-widest text-gray-500 uppercase">Staking Protocol</h3>
                <span class="text-[9px] bg-[#D4AF37]/10 text-[#D4AF37] px-2 py-1 rounded">LIVE</span>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-10">
                <div class="neo-card p-6 text-center">
                    <p class="text-[10px] text-gray-500 mb-1">CORE NODE</p>
                    <p class="text-xl font-black">$30.00</p>
                    <button onclick="buyNode(30, 1.5)" class="mt-4 w-full py-2 bg-white/5 rounded-xl text-[10px] font-bold border border-white/5">STAKE</button>
                </div>
                <div class="neo-card p-6 text-center">
                    <p class="text-[10px] text-gray-500 mb-1">ULTRA NODE</p>
                    <p class="text-xl font-black gold-text">$100.00</p>
                    <button onclick="buyNode(100, 6.0)" class="mt-4 w-full py-2 bg-white/5 rounded-xl text-[10px] font-bold border border-white/5">STAKE</button>
                </div>
            </div>

            <div class="neo-card p-8">
                <h3 class="text-xs font-black tracking-widest text-gray-500 mb-6 uppercase">Transact</h3>
                <input type="number" id="fin-amt" placeholder="0.00" class="nova-input w-full p-5 rounded-2xl mb-6 text-2xl font-black text-center">
                <div class="flex gap-4">
                    <button onclick="requestFin('Deposit')" class="btn-elite flex-1 p-4 rounded-2xl text-xs font-black text-black uppercase">Deposit</button>
                    <button onclick="requestFin('Withdrawal')" class="bg-white/5 border border-white/10 flex-1 p-4 rounded-2xl text-xs font-black uppercase">Withdraw</button>
                </div>
            </div>

            <h3 class="text-xs font-black tracking-widest text-gray-500 mt-12 mb-6 uppercase">Ledger Activity</h3>
            <div id="hist-list" class="space-y-4"></div>
        </div>

        <div id="office-screen" class="screen p-8">
            <div class="neo-card p-8 mb-8 text-center">
                <i class="fa-solid fa-link text-3xl text-[#D4AF37] mb-4"></i>
                <h4 class="font-black text-sm mb-2 uppercase">Referral Protocol</h4>
                <p id="ref-link" class="text-[10px] text-gray-500 break-all bg-black/40 p-3 rounded-xl border border-white/5"></p>
            </div>
            <button class="w-full neo-card p-6 flex items-center justify-between">
                <div class="flex items-center gap-4">
                    <i class="fa-brands fa-whatsapp text-green-500 text-xl"></i>
                    <span class="text-xs font-bold">24/7 Global Desk</span>
                </div>
                <i class="fa-solid fa-chevron-right text-gray-700"></i>
            </button>
        </div>

        <div class="nav-float flex justify-around items-center">
            <i onclick="nav('dash-screen')" class="fa-solid fa-house-chimney text-[#D4AF37] text-lg"></i>
            <div onclick="nav('dash-screen')" class="w-12 h-12 bg-[#D4AF37] rounded-full flex items-center justify-center -mt-12 border-[6px] border-[#030303] shadow-xl">
                <i class="fa-solid fa-plus text-black"></i>
            </div>
            <i onclick="nav('office-screen')" class="fa-solid fa-user-shield text-gray-500 text-lg"></i>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo",
            authDomain: "home-94d45.firebaseapp.com",
            projectId: "home-94d45",
            appId: "1:964390949419:web:589840cb91b7e42ecd506e"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let activeUser = null; window.taps = 0; window.isLogin = true;

        window.handleTaps = () => { taps++; if(taps >= 6) document.getElementById('adm-key').classList.remove('hidden'); };
        window.toggleM = () => { isLogin = !isLogin; document.getElementById('m-text').innerText = isLogin ? "Request Access" : "Secure Login"; };

        window.auth = async () => {
            const id = document.getElementById('u-email').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            if(document.getElementById('adm-key').value === "nov786") { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); return; }
            if(!id || !pw) return;
            const uRef = doc(db, "users", id);
            const snap = await getDoc(uRef);
            if(!isLogin) {
                if(snap.exists()) return alert("Identity Registered");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, time: Date.now() });
                start(id);
            } else {
                if(snap.exists() && snap.data().password === pw) start(id);
                else alert("Authentication Failed");
            }
        };

        function start(id) {
            activeUser = id;
            document.getElementById('auth-screen').classList.remove('active-screen');
            setTimeout(() => {
                document.getElementById('auth-screen').classList.add('hidden');
                document.getElementById('app').classList.remove('hidden');
                document.getElementById('dash-screen').classList.add('active-screen');
            }, 500);
            document.getElementById('user-id-txt').innerText = id;
            document.getElementById('ref-link').innerText = window.location.href + "?ref=" + id;

            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                document.getElementById('main-bal').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "+$" + (data.daily || 0).toFixed(2);
            });
            loadHistory();
            setInterval(() => {
                let el = document.getElementById('main-bal');
                let val = parseFloat(el.innerText.replace('$',''));
                el.innerText = "$" + (val + 0.0001).toFixed(4);
            }, 3000);
        }

        window.requestFin = async (type) => {
            const amt = document.getElementById('fin-amt').value;
            if(!amt || amt <= 0) return;
            await addDoc(collection(db, "logs"), { user: activeUser, amount: parseFloat(amt), type, status: 'Pending', time: Date.now() });
            alert("Protocol Initiated");
        };

        window.buyNode = async (p, d) => {
            const r = doc(db, "users", activeUser); const s = await getDoc(r);
            if(s.data().balance < p) return alert("Liquidity Error");
            await updateDoc(r, { balance: s.data().balance - p, daily: (s.data().daily || 0) + d });
            alert("Node Synchronized!");
        };

        function loadHistory() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", activeUser)), s => {
                const h = document.getElementById('hist-list'); h.innerHTML = "";
                let arr = []; s.forEach(d => arr.push(d.data()));
                arr.sort((a,b) => b.time - a.time).forEach(t => {
                    h.innerHTML += `<div class="neo-card p-5 flex justify-between items-center">
                        <div>
                            <p class="text-[10px] font-black uppercase text-gray-400">${t.type}</p>
                            <p class="text-[8px] text-gray-700">${new Date(t.time).toLocaleTimeString()}</p>
                        </div>
                        <div class="text-right">
                            <p class="text-xs font-black tracking-tighter">$${t.amount.toFixed(2)}</p>
                            <p class="text-[8px] font-bold ${t.status=='Pending'?'text-yellow-600':'text-green-600'}">${t.status}</p>
                        </div>
                    </div>`;
                });
            });
        }

        function loadAdmin() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const al = document.getElementById('adm-list'); al.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    al.innerHTML += `<div class="neo-card p-6">
                        <div class="flex justify-between mb-4"><span class="text-[10px] text-gray-500">${r.user}</span><span class="font-black">$${r.amount}</span></div>
                        <button onclick="approve('${d.id}','${r.user}',${r.amount},'${r.type}')" class="w-full bg-[#D4AF37] text-black font-black p-3 rounded-xl text-[10px]">VERIFY TRANSACTION</button>
                    </div>`;
                });
            });
        }

        window.approve = async (id, u, a, t) => {
            const uR = doc(db, "users", u); const us = await getDoc(uR);
            let nb = t === 'Deposit' ? (us.data().balance + a) : (us.data().balance - a);
            await updateDoc(uR, { balance: nb });
            await updateDoc(doc(db, "logs", id), { status: 'Verified' });
        };

        window.nav = (id) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            setTimeout(() => {
                document.querySelectorAll('.screen').forEach(s => s.style.display = 'none');
                document.getElementById(id).style.display = 'block';
                setTimeout(() => document.getElementById(id).classList.add('active-screen'), 50);
            }, 300);
        };

        window.logout = () => location.reload();
    </script>
</body>
</html>
