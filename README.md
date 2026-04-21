<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Official Terminal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        body { background: #050505; color: white; font-family: 'Inter', sans-serif; -webkit-tap-highlight-color: transparent; }
        .nova-glass { background: rgba(15, 15, 15, 0.95); border: 1px solid #222; border-radius: 24px; }
        .gold-glow { box-shadow: 0 0 15px rgba(212, 175, 55, 0.1); border-color: #D4AF37; }
        .screen { display: none; transition: 0.3s; }
        .active { display: block; animation: fadeIn 0.4s; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .btn-prime { background: linear-gradient(135deg, #D4AF37, #F2CC60); color: black; font-weight: 800; text-transform: uppercase; letter-spacing: 1px; }
    </style>
</head>
<body>

    <div id="auth-screen" class="p-8 flex flex-col items-center justify-center min-h-screen">
        <div class="text-center mb-10">
            <h1 onclick="handleTaps()" id="main-logo" class="text-6xl font-black tracking-tighter">NOVA<span class="text-[#D4AF37]">.</span></h1>
            <p class="text-[10px] tracking-[5px] text-gray-500 uppercase mt-2">Worldwide Launch Offer Live</p>
        </div>
        
        <div class="nova-glass p-6 w-full max-w-sm">
            <input type="text" id="u-email" placeholder="Account ID" class="w-full p-4 bg-black border border-gray-800 rounded-xl mb-3 outline-none focus:border-[#D4AF37] transition-all">
            <input type="password" id="u-pass" placeholder="Security Passkey" class="w-full p-4 bg-black border border-gray-800 rounded-xl mb-3 outline-none focus:border-[#D4AF37] transition-all">
            <input type="text" id="adm-key" placeholder="Enter Master Key" class="hidden w-full p-4 bg-black border border-[#D4AF37] rounded-xl mb-3 outline-none">
            <button onclick="auth()" class="btn-prime w-full p-4 rounded-xl shadow-lg shadow-yellow-900/10">Initiate Protocol</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-xs mt-6 text-gray-500 cursor-pointer hover:text-[#D4AF37]">Don't have an ID? Create Account</p>
        </div>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-black z-[9999] p-6 overflow-y-auto">
        <div class="flex justify-between items-center mb-8 border-b border-gray-900 pb-4">
            <h2 class="text-[#D4AF37] font-black tracking-widest">MASTER OVERRIDE</h2>
            <button onclick="location.reload()" class="bg-red-600/20 text-red-500 px-4 py-2 rounded-lg text-xs font-bold">TERMINATE</button>
        </div>
        <div id="adm-list" class="space-y-4"></div>
    </div>

    <div id="app" class="hidden pb-32">
        <div class="p-6 flex justify-between items-center">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 bg-[#D4AF37] rounded-full flex items-center justify-center text-black font-black" id="avatar">N</div>
                <div>
                    <p class="text-[10px] text-gray-500">Welcome Back,</p>
                    <p id="user-id-txt" class="text-sm font-bold uppercase">User</p>
                </div>
            </div>
            <i onclick="logout()" class="fa-solid fa-power-off text-gray-800 hover:text-red-500 transition-all"></i>
        </div>

        <div id="dash-screen" class="screen active p-6">
            <div class="nova-glass p-8 mb-6 gold-glow relative overflow-hidden">
                <div class="absolute top-0 right-0 p-4 opacity-10"><i class="fa-solid fa-microchip text-6xl"></i></div>
                <p class="text-[10px] text-gray-500 tracking-widest mb-1">AVAILABLE CAPITAL</p>
                <h1 id="main-bal" class="text-5xl font-black my-2">$0.00</h1>
                <div class="flex gap-4 mt-4">
                    <div class="bg-black/40 p-3 rounded-xl border border-gray-900 flex-1">
                        <p class="text-[9px] text-gray-600">DAILY ROI</p>
                        <p class="text-green-500 font-bold text-xs">+$0.00</p>
                    </div>
                    <div class="bg-black/40 p-3 rounded-xl border border-gray-900 flex-1">
                        <p class="text-[9px] text-gray-600">NETWORK</p>
                        <p class="text-white font-bold text-xs">GLOBAL</p>
                    </div>
                </div>
            </div>

            <div class="bg-gradient-to-r from-yellow-900/20 to-black p-4 rounded-2xl border border-yellow-700/30 mb-8">
                <p class="text-[#D4AF37] text-[10px] font-bold uppercase"><i class="fa-solid fa-fire"></i> Launch Offer</p>
                <p class="text-xs text-gray-300">Get 10% bonus on your first institutional deposit today!</p>
            </div>

            <h3 class="font-black text-xs tracking-widest text-gray-500 mb-4 uppercase">Finance Hub</h3>
            <div class="nova-glass p-6">
                <input type="number" id="fin-amt" placeholder="Amount ($)" class="w-full p-4 bg-black border border-gray-800 rounded-xl mb-4 outline-none">
                <div class="flex gap-3">
                    <button onclick="requestFin('Deposit')" class="btn-prime flex-1 p-4 rounded-xl text-xs">Deposit</button>
                    <button onclick="requestFin('Withdrawal')" class="bg-gray-900 text-white flex-1 p-4 rounded-xl text-xs font-bold border border-gray-800">Withdraw</button>
                </div>
            </div>

            <h3 class="font-black text-xs tracking-widest text-gray-500 mt-8 mb-4 uppercase">Activity Logs</h3>
            <div id="hist-list" class="space-y-3"></div>
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

        window.taps = 0; window.isLogin = true; let activeUser = null;

        window.handleTaps = () => { taps++; if(taps >= 6) document.getElementById('adm-key').classList.remove('hidden'); };
        window.toggleM = () => { isLogin = !isLogin; document.getElementById('m-text').innerText = isLogin ? "Don't have an ID? Create Account" : "Already have an ID? Login"; };

        window.auth = async () => {
            const id = document.getElementById('u-email').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            const ak = document.getElementById('adm-key').value;

            if(ak === "nov786") { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); return; }
            if(!id || !pw) return alert("Fill all security fields");

            const uRef = doc(db, "users", id);
            const snap = await getDoc(uRef);

            if(!isLogin) {
                if(snap.exists()) return alert("User already in system");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total: 0 });
                startApp(id);
            } else {
                if(snap.exists() && snap.data().password === pw) startApp(id);
                else alert("Unauthorized access denied");
            }
        };

        function startApp(id) {
            activeUser = id;
            document.getElementById('auth-screen').classList.add('hidden');
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('user-id-txt').innerText = id;
            document.getElementById('avatar').innerText = id.charAt(0).toUpperCase();

            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                document.getElementById('main-bal').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "+$" + (data.daily || 0).toFixed(2);
            });
            loadHistory();
        }

        window.requestFin = async (type) => {
            const amt = document.getElementById('fin-amt').value;
            if(!amt || amt <= 0) return alert("Enter valid capital amount");
            await addDoc(collection(db, "logs"), { user: activeUser, amount: parseFloat(amt), type, status: 'Pending', time: Date.now() });
            alert(type + " protocol initiated successfully");
        };

        function loadHistory() {
            const q = query(collection(db, "logs"), where("user", "==", activeUser));
            onSnapshot(q, s => {
                const h = document.getElementById('hist-list'); h.innerHTML = "";
                let docs = [];
                s.forEach(d => docs.push(d.data()));
                docs.sort((a, b) => b.time - a.time); // Smart sorting without Indexing
                
                docs.forEach(t => {
                    const color = t.status === 'Pending' ? 'text-yellow-500' : 'text-green-500';
                    h.innerHTML += `<div class="nova-glass p-4 text-xs flex justify-between items-center">
                        <div>
                            <p class="font-bold uppercase">${t.type}</p>
                            <p class="text-[9px] text-gray-600">${new Date(t.time).toLocaleDateString()}</p>
                        </div>
                        <div class="text-right">
                            <p class="font-black">$${t.amount.toFixed(2)}</p>
                            <p class="${color} text-[9px] font-bold uppercase">${t.status}</p>
                        </div>
                    </div>`;
                });
            });
        }

        function loadAdmin() {
            const q = query(collection(db, "logs"), where("status", "==", "Pending"));
            onSnapshot(q, s => {
                const al = document.getElementById('adm-list'); al.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    al.innerHTML += `<div class="nova-glass p-5 flex flex-col gap-3">
                        <div class="flex justify-between">
                            <span class="text-xs font-bold text-gray-400">${r.user}</span>
                            <span class="text-xs font-black text-[#D4AF37]">$${r.amount}</span>
                        </div>
                        <p class="text-[10px] uppercase text-gray-600">Action: ${r.type}</p>
                        <button onclick="approve('${d.id}','${r.user}',${r.amount},'${r.type}')" class="bg-green-600 text-white font-bold p-3 rounded-xl text-xs">APPROVE REQUEST</button>
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

        window.logout = () => location.reload();
    </script>
</body>
</html>
