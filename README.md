<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NOVA CORP | Digital Terminal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body { background-color: #0a0a0a; color: #e5e5e5; font-family: 'Inter', sans-serif; }
        .glass-card { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(10px); border: 1px solid rgba(255, 255, 255, 0.1); border-radius: 15px; }
        .neon-text { text-shadow: 0 0 10px #00f2fe; color: #00f2fe; }
        .node-status { animation: pulse 2s infinite; }
        @keyframes pulse { 0% { opacity: 1; } 50% { opacity: 0.5; } 100% { opacity: 1; } }
    </style>
</head>
<body>

    <header class="p-6 text-center">
        <h1 class="text-3xl font-bold neon-text">WELCOME TO NOVA</h1>
        <p class="text-gray-400 mt-2 text-sm">Empowering Your Digital Assets Worldwide</p>
    </header>

    <main class="max-w-md mx-auto p-4 space-y-6">
        
        <section class="glass-card p-5">
            <h2 class="text-lg font-semibold mb-4 border-b border-gray-700 pb-2">Active Hardware Nodes</h2>
            <div class="space-y-4">
                <div class="flex justify-between items-center bg-black/40 p-3 rounded-lg">
                    <span>Node-Alpha (Global)</span>
                    <span class="text-green-400 text-xs node-status">● Online</span>
                </div>
                <div class="flex justify-between items-center bg-black/40 p-3 rounded-lg">
                    <span>Yield Optimizer v2</span>
                    <span class="text-blue-400 text-xs">Processing...</span>
                </div>
            </div>
        </section>

        <section class="glass-card flex flex-col h-[400px]">
            <div class="p-3 border-b border-gray-700 flex justify-between items-center">
                <div class="flex items-center gap-2">
                    <div class="w-3 h-3 bg-green-500 rounded-full"></div>
                    <span id="agent-name" class="font-medium text-sm text-blue-300 italic">Agent 786 is online</span>
                </div>
                <button onclick="clearChat()" class="text-xs text-red-400 hover:underline">Clear History</button>
            </div>
            
            <div id="chat-box" class="flex-1 overflow-y-auto p-4 space-y-3 text-sm">
                <div class="bg-blue-900/20 p-2 rounded-lg self-start max-w-[80%]">
                    Welcome to Nova Corp. How can we assist your growth today?
                </div>
            </div>

            <div class="p-3 border-t border-gray-700 bg-black/20">
                <div class="flex gap-2 items-center">
                    <label class="cursor-pointer bg-gray-800 p-2 rounded-full hover:bg-gray-700 transition">
                        <input type="file" id="imageInput" class="hidden" accept="image/*" onchange="handleImage(this)">
                        📷
                    </label>
                    <input type="text" id="userInput" placeholder="Type a message..." class="bg-transparent border border-gray-600 rounded-full px-4 py-2 flex-1 focus:outline-none focus:border-blue-400 text-sm">
                    <button onclick="sendMessage()" class="bg-blue-600 px-4 py-2 rounded-full text-xs font-bold">SEND</button>
                </div>
            </div>
        </section>

    </main>

    <script>
        // Dynamic Agent Logic
        const agents = ["Agent 786", "Agent 99", "Nova-Support", "Tech-Alpha"];
        document.getElementById('agent-name').innerText = agents[Math.floor(Math.random() * agents.length)] + " is online";

        const chatBox = document.getElementById('chat-box');

        function sendMessage() {
            const input = document.getElementById('userInput');
            if (input.value.trim() === "") return;

            addMessage(input.value, 'user');
            input.value = "";
        }

        function handleImage(input) {
            if (input.files && input.files[0]) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    const base64Image = e.target.result;
                    addImageMessage(base64Image, 'user');
                };
                reader.readAsDataURL(input.files[0]);
            }
        }

        function addMessage(text, sender) {
            const msgDiv = document.createElement('div');
            msgDiv.className = `p-2 rounded-lg max-w-[80%] ${sender === 'user' ? 'bg-gray-700 self-end ml-auto' : 'bg-blue-900/20'}`;
            msgDiv.innerText = text;
            chatBox.appendChild(msgDiv);
            chatBox.scrollTop = chatBox.scrollHeight;
        }

        function addImageMessage(src, sender) {
            const msgDiv = document.createElement('div');
            msgDiv.className = `p-2 rounded-lg max-w-[80%] ${sender === 'user' ? 'bg-gray-700 self-end ml-auto' : 'bg-blue-900/20'}`;
            const img = document.createElement('img');
            img.src = src;
            img.className = "rounded-lg w-full h-auto";
            msgDiv.appendChild(img);
            chatBox.appendChild(msgDiv);
            chatBox.scrollTop = chatBox.scrollHeight;
        }

        function clearChat() {
            chatBox.innerHTML = '<div class="text-center text-gray-500 text-xs py-2 italic">Chat history cleared by admin.</div>';
        }
    </script>
</body>
</html>
