<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>The Ultimate Stock Market Simulator</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <!-- Chart.js for company graphs -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    /* Global Styles */
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: #121212;
      color: #e0e0e0;
      line-height: 1.6;
    }
    .slide-container {
      width: 100%;
      min-height: 100vh;
      position: relative;
    }
    .slide {
      width: 100%;
      min-height: 100vh;
      padding: 40px 20px;
      box-sizing: border-box;
      text-align: center;
      display: none;
    }
    .slide.active {
      display: block;
    }
    h1, h2, h3 {
      margin: 20px 0;
      font-weight: 300;
    }
    p, ul {
      max-width: 800px;
      margin: 0 auto 20px;
    }
    ul {
      list-style: none;
      padding: 0;
    }
    li::before {
      content: "• ";
      color: #007bff;
    }
    /* Company Grid */
    .company-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 20px;
      width: 90%;
      max-width: 1100px;
      margin: 20px auto;
    }
    .company-card {
      background: #1e1e1e;
      padding: 20px;
      border-radius: 10px;
      text-align: center;
      cursor: pointer;
      transition: transform 0.25s ease, box-shadow 0.25s ease;
    }
    .company-card:hover {
      transform: scale(1.05);
      box-shadow: 0 0 15px rgba(0, 255, 136, 0.3);
    }
    .company-card h3 {
      margin: 10px 0;
      color: #00ff88;
    }
    .company-card p {
      font-size: 14px;
      margin: 8px 0;
    }
    .company-card button {
      background: #007bff;
      border: none;
      color: #fff;
      padding: 6px 12px;
      margin: 5px;
      border-radius: 4px;
      cursor: pointer;
      font-size: 13px;
    }
    /* News Feed */
    .news-feed {
      max-width: 800px;
      margin: 20px auto;
      text-align: left;
    }
    .news-feed h3 {
      margin-bottom: 10px;
      border-bottom: 2px solid #007bff;
      padding-bottom: 5px;
    }
    .news-item {
      background: #1e1e1e;
      padding: 12px;
      margin-bottom: 10px;
      border-radius: 6px;
      font-size: 14px;
    }
    /* Navigation Buttons */
    .nav-buttons {
      position: fixed;
      bottom: 20px;
      width: 100%;
      text-align: center;
      z-index: 100;
    }
    .nav-buttons button {
      background: #007bff;
      border: none;
      color: #fff;
      padding: 12px 24px;
      margin: 0 12px;
      border-radius: 6px;
      cursor: pointer;
      font-size: 16px;
      transition: background 0.2s ease;
    }
    .nav-buttons button:hover {
      background: #0056b3;
    }
    /* Portfolio Info */
    .sim-portfolio {
      margin: 20px 0;
      font-size: 18px;
    }
    /* Create Company Form */
    .create-company-form {
      background: #1e1e1e;
      padding: 20px;
      border-radius: 8px;
      width: 90%;
      max-width: 600px;
      margin: 20px auto;
      text-align: left;
    }
    .create-company-form p.info {
      font-size: 14px;
      color: #b0b0b0;
      margin-bottom: 10px;
    }
    .create-company-form input {
      padding: 10px;
      margin: 8px;
      width: calc(100% - 26px);
      border: none;
      border-radius: 4px;
    }
    .create-company-form button {
      margin-top: 10px;
      padding: 12px 24px;
      background: #28a745;
      border: none;
      color: #fff;
      border-radius: 6px;
      cursor: pointer;
      font-size: 16px;
    }
    /* Modal Styles */
    .modal {
      display: none;
      position: fixed;
      top: 0; left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.9);
      z-index: 200;
      overflow-y: auto;
      padding: 30px;
      box-sizing: border-box;
    }
    .modal-content {
      background: #1e1e1e;
      padding: 30px;
      border-radius: 10px;
      max-width: 800px;
      margin: 40px auto;
      position: relative;
      text-align: left;
    }
    .close-modal {
      position: absolute;
      top: 15px;
      right: 15px;
      background: #007bff;
      border: none;
      color: #fff;
      padding: 8px 16px;
      cursor: pointer;
      border-radius: 4px;
      font-size: 14px;
    }
    #modalChartContainer {
      width: 100%;
      height: 300px;
      margin: 20px 0;
    }
    /* Portfolio Table */
    .portfolio-table {
      width: 90%;
      max-width: 1000px;
      margin: 20px auto;
      border-collapse: collapse;
    }
    .portfolio-table th, .portfolio-table td {
      padding: 12px;
      border: 1px solid #444;
      text-align: center;
    }
    .portfolio-table th {
      background: #007bff;
      color: #fff;
    }
    .portfolio-table tr:nth-child(even) {
      background: #1e1e1e;
    }
    .portfolio-table tr:hover {
      background: #2e2e2e;
    }
  </style>
</head>
<body>

<div class="slide-container">
  <!-- Slide 1: Welcome -->
  <div class="slide active" id="slide1">
    <h1>The Ultimate Stock Market Simulator</h1>
    <h2>Build Your Virtual Trading Empire</h2>
    <p>Experience real-time trading, AI-driven insights, and an immersive ecosystem of innovative companies—all without risking real money. Start with $100,000 in virtual cash and become a market legend!</p>
  </div>

  <!-- Slide 2: Key Features -->
  <div class="slide" id="slide2">
    <h2>Key Features</h2>
    <ul>
      <li>Real-Time/Delayed Market Data</li>
      <li>Virtual Money & Live Portfolio Tracking</li>
      <li>Advanced Trading Tools: Market, Limit & Stop-Loss Orders</li>
      <li>Historical Data, Backtesting & Analytics</li>
      <li>Competitions, Leaderboards & Social Integration</li>
      <li>AI-Driven Trading & Live News Updates</li>
      <li>Custom Challenges, Gamification & Company Creation</li>
      <li>Flexible Trading: Choose number of shares to buy/sell</li>
    </ul>
  </div>

  <!-- Slide 3: Company Showcase -->
  <div class="slide" id="slide3">
    <h2>Company Showcase</h2>
    <p>Discover a diverse ecosystem of companies—from global giants to innovative startups.</p>
    <div class="company-grid">
      <div class="company-card">
        <h3>Apple Inc.</h3>
        <p>Sector: Technology</p>
        <p>Price: $<span id="price-AAPL">150.00</span></p>
      </div>
      <div class="company-card">
        <h3>Microsoft</h3>
        <p>Sector: Technology</p>
        <p>Price: $<span id="price-MSFT">300.00</span></p>
      </div>
      <div class="company-card">
        <h3>Amazon</h3>
        <p>Sector: E-commerce</p>
        <p>Price: $<span id="price-AMZN">3500.00</span></p>
      </div>
      <div class="company-card">
        <h3>Google (Alphabet)</h3>
        <p>Sector: Technology</p>
        <p>Price: $<span id="price-GOOGL">2800.00</span></p>
      </div>
      <div class="company-card">
        <h3>Tesla</h3>
        <p>Sector: Automotive</p>
        <p>Price: $<span id="price-TSLA">700.00</span></p>
      </div>
      <div class="company-card">
        <h3>Berkshire Hathaway</h3>
        <p>Sector: Conglomerate</p>
        <p>Price: $<span id="price-BRK.A">450000.00</span></p>
      </div>
      <div class="company-card">
        <h3>Meta (Facebook)</h3>
        <p>Sector: Technology</p>
        <p>Price: $<span id="price-FB">350.00</span></p>
      </div>
      <div class="company-card">
        <h3>NVIDIA</h3>
        <p>Sector: Technology</p>
        <p>Price: $<span id="price-NVDA">600.00</span></p>
      </div>
    </div>
  </div>

  <!-- Slide 4: Interactive Dashboard Overview -->
  <div class="slide" id="slide4">
    <h2>Interactive Dashboard</h2>
    <p>Monitor your portfolio, execute trades, and track performance in real time with live charts, AI-driven signals, and customizable trading parameters.</p>
  </div>

  <!-- Slide 5: Live News Feed -->
  <div class="slide" id="slide5">
    <h2>Live News Feed</h2>
    <div class="news-feed">
      <h3>Latest Market News</h3>
      <div class="news-item">Apple unveils the new iPhone 15 with groundbreaking features.</div>
      <div class="news-item">Microsoft acquires a leading AI startup for $10 billion.</div>
      <div class="news-item">Amazon announces record-breaking Prime Day sales.</div>
      <div class="news-item">Tesla unveils its fully autonomous driving system.</div>
      <div class="news-item">Google launches its next-generation AI-powered search engine.</div>
    </div>
  </div>

  <!-- Slide 6: About This Simulator -->
  <div class="slide" id="slide6">
    <h2>About This Simulator</h2>
    <p>This simulator provides a realistic, immersive virtual trading experience. Use your virtual funds to trade, track live market data, and learn from AI-driven strategies. Explore detailed company profiles complete with historical charts and live news feeds—all without any real risk.</p>
    <p><em>Developed by a passionate community of trading enthusiasts.</em></p>
  </div>

  <!-- Slide 7: Interactive Trading Simulator -->
  <div class="slide" id="slide7">
    <h2>Interactive Trading Simulator</h2>
    <!-- Portfolio Info -->
    <div class="sim-portfolio" id="simPortfolioInfo">
      <strong>Your Portfolio:</strong> Balance: <span id="simBalance">$100,000</span> | Portfolio Value: <span id="simPortfolioValue">$0.00</span> | Total Gain: <span id="simTotalGain">+0.00%</span>
    </div>
    <div class="sim-portfolio" id="aiPortfolioInfo">
      <strong>AI Portfolio:</strong> Balance: <span id="aiBalance">$100,000</span> | Portfolio Value: <span id="aiPortfolioValue">$0.00</span> | Total Gain: <span id="aiTotalGain">+0.00%</span>
    </div>
    <!-- Create Company Form with Info -->
    <div class="create-company-form">
      <p class="info">
        To start your own company, enter your company's name and the industry (sector) it operates in.
        Your company's initial price is automatically set to <strong>$100,000</strong> with 1,000,000 shares outstanding.
        This represents your company's initial valuation. Over time, AI-driven growth will increase its value.
      </p>
      <input type="text" id="newCompanyName" placeholder="Company Name">
      <input type="text" id="newCompanySector" placeholder="Sector">
      <!-- Note: Price field is ignored; your company starts at $100,000 -->
      <br>
      <button onclick="createCompany()">Create Company</button>
    </div>
    <!-- Simulation Company Grid -->
    <div class="company-grid" id="simCompanyGrid">
      <!-- Company cards are generated dynamically -->
    </div>
  </div>

  <!-- Slide 8: Full Portfolio -->
  <div class="slide" id="slide8">
    <h2>Your Portfolio</h2>
    <div class="sim-portfolio" id="portfolioSummary">
      <strong>Portfolio Summary:</strong> Balance: <span id="portfolioBalance">$100,000</span> | Total Value: <span id="portfolioTotalValue">$0.00</span> | Total Gain: <span id="portfolioTotalGain">+0.00%</span>
    </div>
    <table class="portfolio-table">
      <thead>
        <tr>
          <th>Company</th>
          <th>Shares Owned</th>
          <th>Current Price</th>
          <th>Total Value</th>
          <th>Gain/Loss</th>
        </tr>
      </thead>
      <tbody id="portfolioTableBody">
        <!-- Portfolio rows are generated dynamically -->
      </tbody>
    </table>
  </div>
</div>

<!-- Modal for Company Details, Chart & Live News -->
<div class="modal" id="companyModal">
  <div class="modal-content">
    <button class="close-modal" onclick="closeModal()">Close</button>
    <div id="modalCompanyInfo"></div>
    <div id="modalChartContainer">
      <canvas id="companyChart"></canvas>
    </div>
    <div class="news-feed" id="modalNewsFeed">
      <h3>Latest News</h3>
      <!-- Live news items for the selected company -->
    </div>
  </div>
</div>

<!-- Navigation Buttons -->
<div class="nav-buttons">
  <button onclick="prevSlide()">Previous</button>
  <button onclick="nextSlide()">Next</button>
</div>

<script>
  // -----------------------------
  // Slide Navigation
  // -----------------------------
  let currentSlide = 0;
  const slides = document.querySelectorAll('.slide');
  function showSlide(index) {
    slides.forEach((slide, i) => {
      slide.classList.toggle("active", i === index);
    });
  }
  function nextSlide() {
    if (currentSlide < slides.length - 1) {
      currentSlide++;
      showSlide(currentSlide);
      checkSimulationSlide();
    }
  }
  function prevSlide() {
    if (currentSlide > 0) {
      currentSlide--;
      showSlide(currentSlide);
      checkSimulationSlide();
    }
  }
  showSlide(currentSlide);

  // -----------------------------
  // Simulation Data & Functions
  // -----------------------------
  // Predefined companies with improved names
  let stockData = {
    AAPL: { name: "Apple Inc.", price: 150, sector: "Technology" },
    MSFT: { name: "Microsoft", price: 300, sector: "Technology" },
    AMZN: { name: "Amazon", price: 3500, sector: "E-commerce" },
    GOOGL: { name: "Google (Alphabet)", price: 2800, sector: "Technology" },
    TSLA: { name: "Tesla", price: 700, sector: "Automotive" },
    BRK_A: { name: "Berkshire Hathaway", price: 450000, sector: "Conglomerate" },
    FB: { name: "Meta (Facebook)", price: 350, sector: "Technology" },
    NVDA: { name: "NVIDIA", price: 600, sector: "Technology" },
  };

  // Generate 100 additional random companies with creative names
  function generateRandomCompanies(n) {
    const adjectives = ["Quantum", "Zenith", "Pinnacle", "Infinite", "Global", "Epic", "Vanguard", "Nexus", "Synergy", "Dynamic"];
    const nouns = ["Solutions", "Industries", "Corporation", "Enterprises", "Systems", "Holdings", "Networks", "Technologies", "Ventures", "Group"];
    const sectors = ["Technology", "Healthcare", "Finance", "Energy", "Retail", "Entertainment", "Biotech", "Real Estate", "Utilities", "Automotive", "Aerospace", "Agriculture", "Logistics", "Construction", "Software", "Telecommunications"];
    for (let i = 1; i <= n; i++) {
      const key = "RC" + i;
      const name = adjectives[Math.floor(Math.random() * adjectives.length)] + " " + nouns[Math.floor(Math.random() * nouns.length)];
      const sector = sectors[Math.floor(Math.random() * sectors.length)];
      const price = parseFloat((Math.random() * 490 + 10).toFixed(2));
      stockData[key] = { name, price, sector };
    }
  }
  generateRandomCompanies(100);

  // Portfolios for player and AI
  let portfolio = { balance: 100000, stocks: {} };
  let aiPortfolio = { balance: 100000, stocks: {} };

  // Player trading functions with quantity prompt
  function buyStock(symbol, quantity) {
    const price = stockData[symbol].price;
    if (portfolio.balance >= price * quantity) {
      portfolio.balance -= price * quantity;
      portfolio.stocks[symbol] = (portfolio.stocks[symbol] || 0) + quantity;
      updatePortfolioUI();
    } else {
      alert('Not enough balance!');
    }
  }
  function sellStock(symbol, quantity) {
    const price = stockData[symbol].price;
    if (portfolio.stocks[symbol] >= quantity) {
      portfolio.balance += price * quantity;
      portfolio.stocks[symbol] -= quantity;
      updatePortfolioUI();
    } else {
      alert('Not enough shares to sell!');
    }
  }
  function updatePortfolioUI() {
    let totalValue = portfolio.balance;
    for (let symbol in portfolio.stocks) {
      totalValue += stockData[symbol].price * portfolio.stocks[symbol];
    }
    let totalGain = totalValue - 100000;
    let gainPercentage = ((totalGain / 100000) * 100).toFixed(2);
    document.getElementById('simBalance').textContent = `$${portfolio.balance.toFixed(2)}`;
    document.getElementById('simPortfolioValue').textContent = `$${totalValue.toFixed(2)}`;
    document.getElementById('simTotalGain').textContent = `${totalGain >= 0 ? '+' : ''}${totalGain.toFixed(2)} (${gainPercentage}%)`;
  }

  // AI trading functions
  function aiBuyStock(symbol, quantity) {
    const price = stockData[symbol].price;
    if (aiPortfolio.balance >= price * quantity) {
      aiPortfolio.balance -= price * quantity;
      aiPortfolio.stocks[symbol] = (aiPortfolio.stocks[symbol] || 0) + quantity;
    }
  }
  function aiSellStock(symbol, quantity) {
    const price = stockData[symbol].price;
    if (aiPortfolio.stocks[symbol] >= quantity) {
      aiPortfolio.balance += price * quantity;
      aiPortfolio.stocks[symbol] -= quantity;
    }
  }
  function updateAIPortfolioUI() {
    let totalValue = aiPortfolio.balance;
    for (let symbol in aiPortfolio.stocks) {
      totalValue += stockData[symbol].price * aiPortfolio.stocks[symbol];
    }
    let totalGain = totalValue - 100000;
    let gainPercentage = ((totalGain / 100000) * 100).toFixed(2);
    document.getElementById('aiBalance').textContent = `$${aiPortfolio.balance.toFixed(2)}`;
    document.getElementById('aiPortfolioValue').textContent = `$${totalValue.toFixed(2)}`;
    document.getElementById('aiTotalGain').textContent = `${totalGain >= 0 ? '+' : ''}${totalGain.toFixed(2)} (${gainPercentage}%)`;
  }
  function aiTrade() {
    const symbols = Object.keys(stockData);
    const symbol = symbols[Math.floor(Math.random() * symbols.length)];
    const action = Math.random() < 0.5 ? "buy" : "sell";
    const quantity = 10;
    if (action === "buy") {
      aiBuyStock(symbol, quantity);
    } else {
      aiSellStock(symbol, quantity);
    }
    updateAIPortfolioUI();
  }
  setInterval(aiTrade, 5000);

  // Render simulation company cards
  function renderSimulationCompanies() {
    const grid = document.getElementById('simCompanyGrid');
    grid.innerHTML = "";
    for (let symbol in stockData) {
      const company = stockData[symbol];
      const card = document.createElement('div');
      card.className = "company-card";
      // Open modal on card click
      card.onclick = () => { openCompanyModal(symbol); };
      card.innerHTML = `
        <h3>${company.name}</h3>
        <p>Sector: ${company.sector}</p>
        <p>Price: $<span id="price-${symbol}">${company.price.toFixed(2)}</span></p>
        <button onclick="event.stopPropagation(); promptBuyStock('${symbol}');">Buy</button>
        <button onclick="event.stopPropagation(); promptSellStock('${symbol}');">Sell</button>
      `;
      grid.appendChild(card);
    }
  }
  function simulatePrices() {
    for (let symbol in stockData) {
      let change = (Math.random() - 0.5) * 0.05;
      // If company is user-created, add extra growth from AI
      if (stockData[symbol].userCreated) {
        change += 0.01;
      }
      stockData[symbol].price = Math.max(1, stockData[symbol].price * (1 + change));
      const priceEl = document.getElementById(`price-${symbol}`);
      if (priceEl) {
        priceEl.textContent = stockData[symbol].price.toFixed(2);
      }
    }
    updatePortfolioUI();
    updateAIPortfolioUI();
  }
  setInterval(simulatePrices, 3000);

  // Create Your Own Company
  let userCompanyCounter = 1;
  function createCompany() {
    const name = document.getElementById('newCompanyName').value.trim();
    const sector = document.getElementById('newCompanySector').value.trim();
    // For user companies, we override the initial price to 100,000 regardless of input.
    const price = 100000;
    if (!name || !sector) {
      alert("Please enter valid company details.");
      return;
    }
    const symbol = "UC" + userCompanyCounter++;
    // Set sharesOutstanding for valuation purposes (e.g., 1,000,000 shares)
    stockData[symbol] = { name, sector, price, userCreated: true, sharesOutstanding: 1000000 };
    renderSimulationCompanies();
    document.getElementById('newCompanyName').value = "";
    document.getElementById('newCompanySector').value = "";
    document.getElementById('newCompanyPrice').value = "";
  }

  // -----------------------------
  // Buy/Sell Quantity Prompts
  // -----------------------------
  function promptBuyStock(symbol) {
    const qty = prompt("Enter number of shares to buy:", "10");
    const quantity = parseInt(qty);
    if (!isNaN(quantity) && quantity > 0) {
      buyStock(symbol, quantity);
    } else {
      alert("Invalid quantity.");
    }
  }
  function promptSellStock(symbol) {
    const qty = prompt("Enter number of shares to sell:", "10");
    const quantity = parseInt(qty);
    if (!isNaN(quantity) && quantity > 0) {
      sellStock(symbol, quantity);
    } else {
      alert("Invalid quantity.");
    }
  }

  // -----------------------------
  // Modal for Company Details, Chart & Live News
  // -----------------------------
  const modal = document.getElementById('companyModal');
  const modalInfo = document.getElementById('modalCompanyInfo');
  const modalNews = document.getElementById('modalNewsFeed');
  let modalNewsInterval;
  let companyChartInstance;

  // Random extra details generators
  function getRandomYear() {
    return Math.floor(Math.random() * 121) + 1900;
  }
  function getRandomCEO() {
    const names = ["Alex Morgan", "Jordan Blake", "Taylor Reed", "Morgan Lee", "Casey Quinn", "Riley Brooks"];
    return names[Math.floor(Math.random() * names.length)];
  }
  function getRandomHeadquarters() {
    const cities = ["New York", "San Francisco", "London", "Tokyo", "Berlin", "Sydney", "Singapore"];
    return cities[Math.floor(Math.random() * cities.length)];
  }
  function getRandomDescription(name, sector) {
    return `${name} is a leader in the ${sector} sector, renowned for its innovative solutions and dynamic market strategies.`;
  }

  function openCompanyModal(symbol) {
    const company = stockData[symbol];
    const founded = getRandomYear();
    const ceo = getRandomCEO();
    const hq = getRandomHeadquarters();
    const description = getRandomDescription(company.name, company.sector);
    const marketCap = company.userCreated && company.sharesOutstanding 
                      ? (company.price * company.sharesOutstanding).toLocaleString()
                      : (company.price * 1000000).toLocaleString();
    const peRatio = (Math.random() * 30 + 5).toFixed(2);
    const earnings = (Math.random() * 50000000).toLocaleString();
    modalInfo.innerHTML = `
      <h2>${company.name}</h2>
      <p><strong>Sector:</strong> ${company.sector}</p>
      <p><strong>Price:</strong> $${company.price.toFixed(2)}</p>
      <p><strong>Market Cap:</strong> $${marketCap}</p>
      <p><strong>P/E Ratio:</strong> ${peRatio}</p>
      <p><strong>Quarterly Earnings:</strong> $${earnings}</p>
      <p><strong>Founded:</strong> ${founded}</p>
      <p><strong>CEO:</strong> ${ceo}</p>
      <p><strong>Headquarters:</strong> ${hq}</p>
      <p><strong>Description:</strong> ${description}</p>
      ${company.userCreated && company.sharesOutstanding ? `<p><strong>Company Valuation:</strong> $${(company.price * company.sharesOutstanding).toLocaleString()}</p>` : ''}
    `;
    createCompanyChart(symbol);
    modalNews.innerHTML = `<h3>Latest News for ${company.name}</h3>`;
    if (modalNewsInterval) clearInterval(modalNewsInterval);
    modalNewsInterval = setInterval(() => {
      const newsItem = document.createElement('div');
      newsItem.className = 'news-item';
      const headlines = [
        `${company.name} unveils a breakthrough product line.`,
        `${company.name} reports impressive quarterly growth.`,
        `Analysts upgrade ${company.name} amid robust market trends.`,
        `${company.name} faces regulatory challenges in key markets.`,
        `${company.name} announces a strategic partnership.`,
        `${company.name} experiences surge in investor confidence.`,
      ];
      newsItem.textContent = headlines[Math.floor(Math.random() * headlines.length)];
      modalNews.insertBefore(newsItem, modalNews.children[1]);
      if (modalNews.children.length > 6) {
        modalNews.removeChild(modalNews.lastChild);
      }
    }, 4000);
    modal.style.display = "block";
  }

  function closeModal() {
    modal.style.display = "none";
    if (modalNewsInterval) clearInterval(modalNewsInterval);
    if (companyChartInstance) {
      companyChartInstance.destroy();
      companyChartInstance = null;
    }
  }

  function createCompanyChart(symbol) {
    const ctx = document.getElementById('companyChart').getContext('2d');
    let labels = [];
    let data = [];
    for (let i = 29; i >= 0; i--) {
      let d = new Date();
      d.setDate(d.getDate() - i);
      labels.push(d.toLocaleDateString());
      let base = stockData[symbol].price;
      let variation = base * (Math.random() * 0.2 - 0.1);
      data.push(parseFloat((base + variation).toFixed(2)));
    }
    if (companyChartInstance) {
      companyChartInstance.destroy();
    }
    companyChartInstance = new Chart(ctx, {
      type: 'line',
      data: {
        labels: labels,
        datasets: [{
          label: `${stockData[symbol].name} Price (Last 30 Days)`,
          data: data,
          backgroundColor: 'rgba(0, 255, 136, 0.2)',
          borderColor: '#00ff88',
          borderWidth: 2,
          fill: true,
        }]
      },
      options: {
        responsive: true,
        maintainAspectRatio: false,
      }
    });
  }

  // -----------------------------
  // Portfolio Table
  // -----------------------------
  function updatePortfolioTable() {
    const tableBody = document.getElementById('portfolioTableBody');
    tableBody.innerHTML = "";
    let totalValue = portfolio.balance;
    for (let symbol in portfolio.stocks) {
      const company = stockData[symbol];
      const shares = portfolio.stocks[symbol];
      const value = shares * company.price;
      totalValue += value;
      const row = document.createElement('tr');
      row.innerHTML = `
        <td>${company.name}</td>
        <td>${shares}</td>
        <td>$${company.price.toFixed(2)}</td>
        <td>$${value.toFixed(2)}</td>
        <td>+0.00%</td>
      `;
      tableBody.appendChild(row);
    }
    document.getElementById('portfolioBalance').textContent = `$${portfolio.balance.toFixed(2)}`;
    document.getElementById('portfolioTotalValue').textContent = `$${totalValue.toFixed(2)}`;
    const totalGain = totalValue - 100000;
    const gainPercentage = ((totalGain / 100000) * 100).toFixed(2);
    document.getElementById('portfolioTotalGain').textContent = `${totalGain >= 0 ? '+' : ''}${totalGain.toFixed(2)} (${gainPercentage}%)`;
  }

  // -----------------------------
  // Simulation Slide Initialization
  // -----------------------------
  function initSimulation() {
    renderSimulationCompanies();
    updatePortfolioUI();
    updateAIPortfolioUI();
    updatePortfolioTable();
  }
  function checkSimulationSlide() {
    if (currentSlide === 6) { // Slide 7 (0-indexed)
      initSimulation();
    }
  }
  document.querySelector('.nav-buttons button:nth-child(1)').onclick = () => { prevSlide(); checkSimulationSlide(); };
  document.querySelector('.nav-buttons button:nth-child(2)').onclick = () => { nextSlide(); checkSimulationSlide(); };
  if (currentSlide === slides.length - 1) initSimulation();
</script>

<!-- Modal -->
<div class="modal" id="companyModal">
  <div class="modal-content">
    <button class="close-modal" onclick="closeModal()">Close</button>
    <div id="modalCompanyInfo"></div>
    <div id="modalChartContainer">
      <canvas id="companyChart"></canvas>
    </div>
    <div class="news-feed" id="modalNewsFeed">
      <h3>Latest News</h3>
    </div>
  </div>
</div>

<!-- Navigation Buttons -->
<div class="nav-buttons">
  <button onclick="prevSlide()">Previous</button>
  <button onclick="nextSlide()">Next</button>
</div>

</body>
</html>
