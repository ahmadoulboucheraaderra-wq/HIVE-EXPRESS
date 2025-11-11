<!DOCTYPE html><html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>HIVE EXPRESS - Service de Conciergerie et CV</title>
  <style>
    body { font-family: Arial, sans-serif; margin:0; padding:0; background-color:#f5f5f5; }
    header { background-color:#4CAF50; color:white; padding:20px; text-align:center; }
    nav { background-color:#333; }
    nav a { color:white; padding:14px 20px; display:inline-block; text-decoration:none; }
    nav a:hover { background-color:#ddd; color:black; }
    section { padding:20px; margin:20px; background-color:white; border-radius:10px; box-shadow:0 2px 5px rgba(0,0,0,0.2); }
    h2 { color:#4CAF50; }
    form { display:flex; flex-direction:column; }
    input, textarea, select { margin:5px 0 15px 0; padding:10px; border-radius:5px; border:1px solid #ccc; }
    button { padding:10px; border:none; border-radius:5px; background-color:#4CAF50; color:white; cursor:pointer; }
    button:hover { background-color:#45a049; }
    .hidden { display:none; }
    ul { list-style:none; padding:0; }
    li { background:#eee; margin:5px 0; padding:10px; border-radius:5px; }
    .chatbox { border:1px solid #ccc; padding:10px; border-radius:5px; max-width:300px; }
  </style>
</head>
<body><header>
  <h1>HelperApp</h1>
  <p>Conciergerie & Création CV / Lettre de motivation</p>
</header><nav>
  <a href="#home" onclick="showSection('home')">Accueil</a>
  <a href="#orders" onclick="showSection('orders')">Commandes</a>
  <a href="#cv" onclick="showSection('cv')">CV / Lettre</a>
  <a href="#chat" onclick="showSection('chat')">Chatbot</a>
  <a href="#admin" id="adminNav" onclick="showSection('admin')">Admin</a>
</nav><section id="home">
  <h2>Bienvenue sur HelperApp</h2>
  <p>Choisissez une fonctionnalité dans le menu ci-dessus.</p>
</section><section id="orders" class="hidden">
  <h2>Passer une commande</h2>
  <form id="orderForm">
    <label for="username">Nom :</label>
    <input type="text" id="username" required>
    <label for="service">Service :</label>
    <select id="service">
      <option value="courses">Courses L'Express</option>
      <option value="livraison">Livraison</option>
      <option value="autre">Autre</option>
    </select>
    <label for="details">Détails :</label>
    <textarea id="details" rows="3" required></textarea>
    <button type="submit">Envoyer la commande</button>
  </form>
  <h3>Liste des commandes</h3>
  <ul id="orderList"></ul>
</section><section id="cv" class="hidden">
  <h2>Création CV & Lettre de motivation</h2>
  <form id="cvForm">
    <label for="cvName">Nom :</label>
    <input type="text" id="cvName" required>
    <label for="cvEmail">Email :</label>
    <input type="email" id="cvEmail" required>
    <label for="cvExp">Expérience :</label>
    <textarea id="cvExp" rows="3"></textarea>
    <button type="submit">Générer CV</button>
  </form>
  <h3>Aperçu CV :</h3>
  <div id="cvPreview"></div>
</section><section id="admin">
  <h2>Interface Admin</h2>
  <p>Liste des commandes passées :</p>
  <ul id="adminOrders"></ul>
</section><section id="chat" class="hidden">
  <h2>Chatbot d'assistance</h2>
  <div class="chatbox" id="chatbox">
    <div id="chatMessages"></div>
    <input type="text" id="chatInput" placeholder="Écrivez votre question...">
    <button onclick="sendMessage()">Envoyer</button>
  </div>
</section><script>
// ====== Gestion des sections ======
function showSection(sectionId) {
  document.querySelectorAll('section').forEach(sec => sec.classList.add('hidden'));
  document.getElementById(sectionId).classList.remove('hidden');
}

// ====== Rôle administrateur activé ======
let userRole = 'admin'; // Tu es administrateur donc menu et section admin visibles

// Aucun besoin de cacher Admin car tu es admin

// ====== Gestion des commandes ======
let orders = [];
const orderForm = document.getElementById('orderForm');
orderForm.addEventListener('submit', function(e){
  e.preventDefault();
  const order = {
    username: document.getElementById('username').value,
    service: document.getElementById('service').value,
    details: document.getElementById('details').value
  };
  orders.push(order);
  updateOrderLists();
  orderForm.reset();
});

function updateOrderLists() {
  const orderList = document.getElementById('orderList');
  const adminOrders = document.getElementById('adminOrders');
  orderList.innerHTML = '';
  adminOrders.innerHTML = '';
  orders.forEach((o, i) => {
    const li = document.createElement('li');
    li.textContent = `${o.username} - ${o.service} : ${o.details}`;
    orderList.appendChild(li);

    const liAdmin = document.createElement('li');
    liAdmin.textContent = `Commande #${i+1} | ${o.username} - ${o.service} : ${o.details}`;
    adminOrders.appendChild(liAdmin);
  });
}

// ====== Création CV ======
const cvForm = document.getElementById('cvForm');
cvForm.addEventListener('submit', function(e){
  e.preventDefault();
  const name = document.getElementById('cvName').value;
  const email = document.getElementById('cvEmail').value;
  const exp = document.getElementById('cvExp').value;
  document.getElementById('cvPreview').innerHTML = `
    <h3>${name}</h3>
    <p>Email: ${email}</p>
    <p>Expérience: ${exp}</p>
  `;
});

// ====== Chatbot simple ======
function sendMessage() {
  const input = document.getElementById('chatInput').value;
  const chatMessages = document.getElementById('chatMessages');
  if(!input) return;
  const userMsg = document.createElement('p');
  userMsg.textContent = `Vous: ${input}`;
  chatMessages.appendChild(userMsg);

  const botMsg = document.createElement('p');
  botMsg.textContent = `HelperApp: Merci pour votre message. Nous traiterons votre demande rapidement.`;
  chatMessages.appendChild(botMsg);

  document.getElementById('chatInput').value = '';
  chatMessages.scrollTop = chatMessages.scrollHeight;
}
</script></body>
</html>
