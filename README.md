# Real-time-chat-application-"
Navigation Menu
Real-time-chat-application-

Code
Issues
Pull requests
 0 stars
 0 forks
 0 watching
 1 Branch
 0 Tags
 Activity
Public repository
santhiyasupramanaiyan-arch/Real-time-chat-application-
Name	

santhiyasupramanaiyan-arch
1 hour ago
README.md
1 hour ago
Repository files navigation
README
Real-time-chat-application-
step1:Install Node.js (v14 or later).

macOS / Linux: use your package manager or download from nodejs.org

Windows: download installer from nodejs.org

A text editor (VS Code, Notepad, etc.) and a terminal/command prompt.

Files to create

Create a folder, e.g. realtime- chat. Inside it create two files:

index.html — your client (use the client HTML you pasted; I’ll include a cleaned version below).

server.js — the Node/WebSocket server (pick option A or B).

Minimal ws server (fastest)

Open terminal in realtime-chat.

Initialize and install ws:

npm init -y npm install ws

Create server.js with this content:
// server.js - minimal WebSocket broadcast server using 'ws' const WebSocket = require('ws'); const wss = new WebSocket.Server({ port: 3000 }, () => { console.log('WebSocket server listening on ws://localhost:3000'); });

wss.on('connection', (ws) => { console.log('Client connected');

ws.on('message', (data) => { // Assume JSON { name, text } let msg; try { msg = JSON.parse(data); } catch (e) { return; }

// Broadcast to everyone
const out = JSON.stringify(msg);
wss.clients.forEach(client => {
  if (client.readyState === WebSocket.OPEN) client.send(out);
});
});

ws.on('close', () => console.log('Client disconnected')); });

Put index.html (client) in same folder (see client sample below).

Start server:
node server.js

Terminal will show WebSocket server listening on ws://localhost:3000.

Open index.html in your browser (double-click or File → Open). Open 2 browser tabs or 2 different browsers, set different names, then send messages. They should appear across tabs.

express + ws server (serves the HTML at http://localhost:3000)

Install packages:

npm init -y npm install express ws

server.js:
// server.js - serve static files + WebSocket server const express = require('express'); const path = require('path'); const http = require('http'); const WebSocket = require('ws');

const app = express(); app.use(express.static(path.join(__dirname))); // serves index.html

const server = http.createServer(app); const wss = new WebSocket.Server({ server });

wss.on('connection', ws => { console.log('Client connected'); ws.on('message', message => { // Broadcast received message to all clients wss.clients.forEach(c => { if (c.readyState === WebSocket.OPEN) c.send(message); }); }); ws.on('close', () => console.log('Client disconnected')); });

const PORT = 3000; server.listen(PORT, () => { console.log(App and WS running at http://localhost:${PORT}); });

Start:
node server.js

Open http://localhost:3000 in browser tabs and test.
Client HTML (cleaned single file)

Save as index.html (use this if you want a ready file):

<!doctype html>

<title>Real-Time Chat</title> <style> body{font-family:Arial,system-ui;background:#f3f4f6;display:flex;justify-content:center;align-items:center;height:100vh;margin:0} .chat-container{width:360px;background:white;border-radius:10px;box-shadow:0 0 10px rgba(0,0,0,0.08);padding:18px} .chat-box{height:300px;border:1px solid #e6e6e6;padding:10px;border-radius:6px;overflow-y:auto;background:#fafafa;margin-bottom:10px} .message{background:#e8f0ff;padding:8px;border-radius:6px;margin:6px 0} .input-row{display:flex;gap:8px} input[type="text"]{flex:1;padding:8px;border-radius:6px;border:1px solid #ccc} button{padding:8px 12px;border-radius:6px;border:none;background:#1976d2;color:#fff;cursor:pointer} button:disabled{opacity:0.6} #status{margin-top:8px;font-size:13px;color:#666} </style>
💬 Real-Time Chat
Your name:
Send
Status: Disconnected
<script> const wsUrl = (location.protocol === 'https:' ? 'wss://' : 'ws://') + (location.hostname || 'localhost') + ':3000'; // If you opened index.html directly (file://), use ws://localhost:3000 const chatBox = document.getE
