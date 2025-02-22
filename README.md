# budget
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Gestion du Budget Familial</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      margin: 20px;
      background-color: #f4f4f9;
      color: #333;
      transition: background-color 0.3s, color 0.3s;
    }
    body.dark-mode {
      background-color: #333;
      color: #f4f4f9;
    }
    h1 {
      color: #4CAF50;
      text-align: center;
      margin-bottom: 20px;
    }
    .container {
      max-width: 800px;
      margin: 0 auto;
      background: white;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
      transition: background-color 0.3s, color 0.3s;
    }
    body.dark-mode .container {
      background-color: #444;
      color: #f4f4f9;
    }
    label {
      display: block;
      margin: 10px 0 5px;
      font-weight: bold;
      color: #555;
    }
    body.dark-mode label {
      color: #f4f4f9;
    }
    input, select, textarea {
      width: 100%;
      padding: 10px;
      margin-bottom: 15px;
      border: 1px solid #ddd;
      border-radius: 6px;
      font-size: 14px;
      transition: background-color 0.3s, color 0.3s;
    }
    body.dark-mode input, body.dark-mode select, body.dark-mode textarea {
      background-color: #555;
      color: #f4f4f9;
      border-color: #777;
    }
    textarea {
      resize: vertical;
      height: 80px;
    }
    button {
      background-color: #4CAF50;
      color: white;
      padding: 10px 20px;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-size: 14px;
      transition: background-color 0.3s ease;
    }
    button:hover {
      background-color: #45a049;
    }
    .results {
      margin-top: 20px;
      padding: 15px;
      background-color: #f9f9f9;
      border-radius: 6px;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
      transition: background-color 0.3s, color 0.3s;
    }
    body.dark-mode .results {
      background-color: #555;
      color: #f4f4f9;
    }
    .results div {
      margin: 10px 0;
      font-size: 16px;
    }
    .transactions {
      margin-top: 20px;
    }
    .transaction {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 15px;
      margin-bottom: 10px;
      background-color: #f9f9f9;
      border-radius: 6px;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
      transition: background-color 0.3s, color 0.3s;
    }
    body.dark-mode .transaction {
      background-color: #555;
      color: #f4f4f9;
    }
    .transaction span {
      flex: 1;
      font-size: 14px;
    }
    .transaction button {
      background-color: #ff4444;
      color: white;
      border: none;
      border-radius: 4px;
      padding: 8px 12px;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }
    .transaction button:hover {
      background-color: #cc0000;
    }
    canvas {
      margin-top: 20px;
      max-width: 100%;
    }
    .navigation {
      display: flex;
      justify-content: space-between;
      margin-top: 20px;
    }
    .navigation button {
      background-color: #2196F3;
    }
    .navigation button:hover {
      background-color: #1e88e5;
    }
    .page {
      display: none;
    }
    .page.active {
      display: block;
    }
    .filters {
      display: flex;
      gap: 10px;
      margin-bottom: 20px;
    }
    .filters input, .filters select {
      flex: 1;
      margin-bottom: 0;
    }
    .budget-alert {
      color: #ff4444;
      font-weight: bold;
    }
  </style>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
  <div class="container">
    <h1>Gestion du Budget Familial</h1>

    <!-- Navigation -->
    <div class="navigation">
      <button onclick="showPage('home')">Accueil</button>
      <button onclick="showPage('transactions')">Transactions</button>
      <button onclick="toggleDarkMode()">Mode sombre</button>
    </div>

    <!-- Page d'accueil -->
    <div id="home" class="page active">
      <!-- Formulaire pour ajouter une transaction -->
      <div>
        <label for="type">Type de transaction :</label>
        <select id="type">
          <option value="income">Rentrée d'argent</option>
          <option value="expense">Dépense</option>
        </select>
        <label for="category">Catégorie :</label>
        <input type="text" id="category" placeholder="Entrez une catégorie (ex: Alimentation, Transport)">
        <label for="amount">Montant (€) :</label>
        <input type="number" id="amount" placeholder="Entrez le montant">
        <label for="notes">Notes :</label>
        <textarea id="notes" placeholder="Ajoutez une note ou une description"></textarea>
        <button onclick="addTransaction()">Ajouter</button>
        <button onclick="saveTransactions()">Sauvegarder</button>
      </div>

      <!-- Aperçu financier -->
      <div class="results">
        <h2>Aperçu Financier :</h2>
        <div><strong>Solde total :</strong> <span id="balance">0 €</span></div>
        <div><strong>Rentrées d'argent :</strong> <span id="totalIncome">0 €</span></div>
        <div><strong>Dépenses :</strong> <span id="totalExpenses">0 €</span></div>
      </div>

      <!-- Répartition 50/30/20 -->
      <div class="results">
        <h2>Répartition du budget :</h2>
        <div><strong>Besoins essentiels (50%) :</strong> <span id="needs">0 €</span></div>
        <div><strong>Loisirs et envies (30%) :</strong> <span id="wants">0 €</span></div>
        <div><strong>Épargne et dettes (20%) :</strong> <span id="savings">0 €</span></div>
      </div>

      <!-- Graphiques -->
      <div>
        <h2>Graphiques :</h2>
        <canvas id="overviewChart"></canvas>
        <canvas id="expensesChart"></canvas>
        <canvas id="balanceChart"></canvas>
      </div>
    </div>

    <!-- Page des transactions -->
    <div id="transactions" class="page">
      <h2>Liste des Transactions :</h2>
      <!-- Filtres et recherche -->
      <div class="filters">
        <input type="text" id="search" placeholder="Rechercher une transaction" oninput="filterTransactions()">
        <select id="filterType" onchange="filterTransactions()">
          <option value="all">Toutes</option>
          <option value="income">Rentrées</option>
          <option value="expense">Dépenses</option>
        </select>
        <select id="filterCategory" onchange="filterTransactions()">
          <option value="all">Toutes les catégories</option>
          <!-- Les options seront ajoutées dynamiquement -->
        </select>
      </div>
      <!-- Bouton d'export -->
      <button onclick="exportToCSV()">Exporter en CSV</button>
      <!-- Liste des transactions -->
      <div id="transactionList"></div>
    </div>
  </div>

  <script>
    let transactions = [];

    // Charger les transactions sauvegardées au démarrage
    function loadTransactions() {
      const savedTransactions = localStorage.getItem('transactions');
      if (savedTransactions) {
        transactions = JSON.parse(savedTransactions);
        updateOverview();
        renderTransactions(transactions);
        updateCharts();
        updateCategoryFilters();
      }
    }

    // Sauvegarder les transactions
    function saveTransactions() {
      localStorage.setItem('transactions', JSON.stringify(transactions));
      alert("Transactions sauvegardées !");
    }

    // Ajouter une transaction
    function addTransaction() {
      const type = document.getElementById('type').value;
      const category = document.getElementById('category').value.trim();
      const amount = parseFloat(document.getElementById('amount').value);
      const notes = document.getElementById('notes').value.trim();

      if (isNaN(amount) || amount <= 0) {
        alert("Veuillez entrer un montant valide.");
        return;
      }
      if (!category) {
        alert("Veuillez entrer une catégorie.");
        return;
      }

      const transaction = {
        id: Date.now(),
        type,
        category,
        amount,
        notes,
      };

      // Vérifier les doublons
      const isDuplicate = transactions.some(t => t.id === transaction.id);
      if (!isDuplicate) {
        transactions.push(transaction);
        updateOverview();
        renderTransactions(transactions);
        updateCharts();
        updateCategoryFilters();

        // Réinitialiser le formulaire
        document.getElementById('category').value = '';
        document.getElementById('amount').value = '';
        document.getElementById('notes').value = '';
      } else {
        alert("Cette transaction existe déjà.");
      }
    }

    // Supprimer une transaction
    function deleteTransaction(id) {
      transactions = transactions.filter(transaction => transaction.id !== id);
      updateOverview();
      renderTransactions(transactions);
      updateCharts();
      updateCategoryFilters();
    }

    // Mettre à jour l'aperçu financier
    function updateOverview() {
      const totalIncome = transactions
        .filter(t => t.type === 'income')
        .reduce((sum, t) => sum + t.amount, 0);

      const totalExpenses = transactions
        .filter(t => t.type === 'expense')
        .reduce((sum, t) => sum + t.amount, 0);

      const balance = totalIncome - totalExpenses;

      document.getElementById('balance').textContent = `${balance.toFixed(2)} €`;
      document.getElementById('totalIncome').textContent = `${totalIncome.toFixed(2)} €`;
      document.getElementById('totalExpenses').textContent = `${totalExpenses.toFixed(2)} €`;

      // Répartition 50/30/20
      const needs = totalIncome * 0.5;
      const wants = totalIncome * 0.3;
      const savings = totalIncome * 0.2;

      document.getElementById('needs').textContent = `${needs.toFixed(2)} €`;
      document.getElementById('wants').textContent = `${wants.toFixed(2)} €`;
      document.getElementById('savings').textContent = `${savings.toFixed(2)} €`;
    }

    // Afficher les transactions
    function renderTransactions(transactionsToRender) {
      const transactionList = document.getElementById('transactionList');
      transactionList.innerHTML = transactionsToRender
        .map(
          t => `
            <div class="transaction">
              <span>
                <strong>${t.type === 'income' ? 'Rentrée' : 'Dépense'}</strong> (${t.category}) : ${t.amount.toFixed(2)} €
                ${t.notes ? `<br><em>${t.notes}</em>` : ''}
              </span>
              <button onclick="deleteTransaction(${t.id})">🗑️</button>
            </div>
          `
        )
        .join('');
    }

    // Mettre à jour les filtres de catégorie
    function updateCategoryFilters() {
      const categories = [...new Set(transactions.map(t => t.category))];
      const filterCategory = document.getElementById('filterCategory');
      filterCategory.innerHTML = `
        <option value="all">Toutes les catégories</option>
        ${categories.map(cat => `<option value="${cat}">${cat}</option>`).join('')}
      `;
    }

    // Filtrer les transactions
    function filterTransactions() {
      const searchText = document.getElementById('search').value.toLowerCase();
      const filterType = document.getElementById('filterType').value;
      const filterCategory = document.getElementById('filterCategory').value;

      const filteredTransactions = transactions.filter(t => {
        const matchesSearch = t.notes.toLowerCase().includes(searchText) || t.category.toLowerCase().includes(searchText);
        const matchesType = filterType === 'all' || t.type === filterType;
        const matchesCategory = filterCategory === 'all' || t.category === filterCategory;
        return matchesSearch && matchesType && matchesCategory;
      });

      renderTransactions(filteredTransactions);
    }

    // Exporter en CSV
    function exportToCSV() {
      const headers = ["ID", "Type", "Catégorie", "Montant (€)", "Notes"];
      const rows = transactions.map(t => [t.id, t.type, t.category, t.amount.toFixed(2), t.notes]);

      const csvContent = [
        headers.join(","),
        ...rows.map(row => row.join(","))
      ].join("\n");

      const blob = new Blob([csvContent], { type: "text/csv;charset=utf-8;" });
      const link = document.createElement("a");
      link.href = URL.createObjectURL(blob);
      link.download = "transactions.csv";
      link.click();
    }

    // Mettre à jour les graphiques
    let overviewChart, expensesChart, balanceChart;
    function updateCharts() {
      const totalIncome = transactions
        .filter(t => t.type === 'income')
        .reduce((sum, t) => sum + t.amount, 0);

      const totalExpenses = transactions
        .filter(t => t.type === 'expense')
        .reduce((sum, t) => sum + t.amount, 0);

      const expensesByCategory = transactions
        .filter(t => t.type === 'expense')
        .reduce((acc, t) => {
          acc[t.category] = (acc[t.category] || 0) + t.amount;
          return acc;
        }, {});

      const balanceOverTime = transactions
        .sort((a, b) => new Date(a.id) - new Date(b.id))
        .reduce((acc, t) => {
          const lastBalance = acc.length > 0 ? acc[acc.length - 1].balance : 0;
          const newBalance = t.type === 'income' ? lastBalance + t.amount : lastBalance - t.amount;
          acc.push({ date: new Date(t.id).toLocaleDateString(), balance: newBalance });
          return acc;
        }, []);

      const ctx1 = document.getElementById('overviewChart').getContext('2d');
      const ctx2 = document.getElementById('expensesChart').getContext('2d');
      const ctx3 = document.getElementById('balanceChart').getContext('2d');

      if (overviewChart) overviewChart.destroy();
      if (expensesChart) expensesChart.destroy();
      if (balanceChart) balanceChart.destroy();

      overviewChart = new Chart(ctx1, {
        type: 'pie',
        data: {
          labels: ['Rentrées', 'Dépenses'],
          datasets: [{
            data: [totalIncome, totalExpenses],
            backgroundColor: ['#4CAF50', '#FF4444'],
          }]
        },
        options: {
          responsive: true,
          plugins: {
            legend: { position: 'top' },
            tooltip: {
              callbacks: {
                label: (context) => `${context.label}: ${context.raw.toFixed(2)} €`,
              }
            }
          }
        }
      });

      expensesChart = new Chart(ctx2, {
                type: 'bar',
        data: {
          labels: Object.keys(expensesByCategory),
          datasets: [{
            label: 'Dépenses par catégorie',
            data: Object.values(expensesByCategory),
            backgroundColor: '#FF4444',
          }]
        },
        options: {
          responsive: true,
          plugins: {
            legend: { display: false },
            tooltip: {
              callbacks: {
                label: (context) => `${context.raw.toFixed(2)} €`,
              }
            }
          }
        }
      });

      balanceChart = new Chart(ctx3, {
        type: 'line',
        data: {
          labels: balanceOverTime.map(b => b.date),
          datasets: [{
            label: 'Solde au fil du temps',
            data: balanceOverTime.map(b => b.balance),
            borderColor: '#2196F3',
            fill: false,
          }]
        },
        options: {
          responsive: true,
          plugins: {
            legend: { position: 'top' },
            tooltip: {
              callbacks: {
                label: (context) => `${context.raw.toFixed(2)} €`,
              }
            }
          }
        }
      });
    }

    // Navigation entre les pages
    function showPage(pageId) {
      document.querySelectorAll('.page').forEach(page => page.classList.remove('active'));
      document.getElementById(pageId).classList.add('active');
    }

    // Basculer en mode sombre
    function toggleDarkMode() {
      document.body.classList.toggle('dark-mode');
    }

    // Charger les transactions au démarrage
    loadTransactions();
  </script>
</body>
</html>
        
