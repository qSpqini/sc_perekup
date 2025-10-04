
<html lang="ru">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Инвестиционный портфель</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Sortable/1.15.0/Sortable.min.js"></script>
<style>
:root {
  --primary: #6366f1;
  --primary-dark: #4f46e5;
  --success: #10b981;
  --danger: #ef4444;
  --warning: #f59e0b;
  --info: #3b82f6;
}

[data-theme="dark"] {
  --bg-primary: #0a0a0f;
  --bg-secondary: #13131a;
  --bg-tertiary: #1a1a24;
  --bg-card: #1e1e2e;
  --text-primary: #e4e4e7;
  --text-secondary: #a1a1aa;
  --text-muted: #71717a;
  --border: rgba(255,255,255,0.08);
  --shadow: rgba(0,0,0,0.5);
  --glass: rgba(255,255,255,0.03);
}

[data-theme="light"] {
  --bg-primary: #f8fafc;
  --bg-secondary: #f1f5f9;
  --bg-tertiary: #e2e8f0;
  --bg-card: #ffffff;
  --text-primary: #0f172a;
  --text-secondary: #475569;
  --text-muted: #94a3b8;
  --border: rgba(0,0,0,0.08);
  --shadow: rgba(0,0,0,0.1);
  --glass: rgba(0,0,0,0.02);
}

* { box-sizing: border-box; margin: 0; padding: 0; }
html, body { height: 100%; }
body {
  font-family: 'Inter', system-ui, -apple-system, sans-serif;
  background: var(--bg-primary);
  color: var(--text-primary);
  -webkit-font-smoothing: antialiased;
  transition: background 0.3s, color 0.3s;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes slideIn {
  from { transform: translateX(-100%); }
  to { transform: translateX(0); }
}

@keyframes pulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.05); }
}

@keyframes shimmer {
  0% { background-position: -1000px 0; }
  100% { background-position: 1000px 0; }
}

.app {
  display: grid;
  grid-template-columns: 280px 1fr;
  min-height: 100vh;
  gap: 0;
}

/* Sidebar */
.sidebar {
  background: var(--bg-secondary);
  border-right: 1px solid var(--border);
  padding: 24px;
  display: flex;
  flex-direction: column;
  gap: 24px;
  position: sticky;
  top: 0;
  height: 100vh;
  overflow-y: auto;
  transition: transform 0.3s;
}

.sidebar.mobile-hidden {
  transform: translateX(-100%);
}

.brand {
  display: flex;
  align-items: center;
  gap: 12px;
  padding-bottom: 24px;
  border-bottom: 1px solid var(--border);
}

.logo {
  width: 42px;
  height: 42px;
  border-radius: 12px;
  background: linear-gradient(135deg, var(--primary), var(--info));
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 800;
  color: white;
  font-size: 1.2rem;
  box-shadow: 0 4px 12px rgba(99, 102, 241, 0.3);
}

.brand-text {
  font-weight: 700;
  font-size: 1.1rem;
  line-height: 1.2;
}

.nav {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.nav a {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 16px;
  border-radius: 10px;
  color: var(--text-secondary);
  text-decoration: none;
  font-weight: 500;
  transition: all 0.2s;
  position: relative;
}

.nav a:hover {
  background: var(--glass);
  color: var(--text-primary);
}

.nav a.active {
  background: linear-gradient(135deg, rgba(99, 102, 241, 0.1), rgba(59, 130, 246, 0.1));
  color: var(--primary);
  font-weight: 600;
}

.nav a.active::before {
  content: '';
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  width: 3px;
  height: 60%;
  background: var(--primary);
  border-radius: 0 3px 3px 0;
}

/* Theme Toggle */
.theme-toggle {
  background: var(--bg-tertiary);
  border: 1px solid var(--border);
  padding: 8px;
  border-radius: 10px;
  display: flex;
  gap: 4px;
  cursor: pointer;
}

.theme-btn {
  flex: 1;
  padding: 8px;
  border: none;
  background: transparent;
  color: var(--text-secondary);
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.2s;
  font-size: 1.1rem;
}

.theme-btn.active {
  background: var(--bg-card);
  color: var(--primary);
  box-shadow: 0 2px 8px var(--shadow);
}

/* Calculator Widget */
.calc-widget {
  background: linear-gradient(135deg, rgba(99, 102, 241, 0.1), rgba(59, 130, 246, 0.05));
  border: 1px solid rgba(99, 102, 241, 0.2);
  padding: 16px;
  border-radius: 12px;
}

.calc-widget h4 {
  font-size: 0.95rem;
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  gap: 8px;
}

.calc-inputs {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

/* Main Content */
.main {
  background: var(--bg-primary);
  padding: 24px;
  overflow-y: auto;
  height: 100vh;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24px;
  flex-wrap: wrap;
  gap: 16px;
}

.h-title {
  font-size: 1.75rem;
  font-weight: 700;
  background: linear-gradient(135deg, var(--primary), var(--info));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.controls {
  display: flex;
  gap: 8px;
  align-items: center;
  flex-wrap: wrap;
}

/* Buttons */
.btn {
  background: var(--bg-card);
  border: 1px solid var(--border);
  color: var(--text-primary);
  padding: 10px 16px;
  border-radius: 10px;
  cursor: pointer;
  font-weight: 500;
  font-size: 0.9rem;
  transition: all 0.2s;
  display: inline-flex;
  align-items: center;
  gap: 6px;
}

.btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px var(--shadow);
}

.btn-primary {
  background: linear-gradient(135deg, var(--primary), var(--primary-dark));
  color: white;
  border: none;
}

.btn-success {
  background: linear-gradient(135deg, var(--success), #059669);
  color: white;
  border: none;
}

.btn-danger {
  background: linear-gradient(135deg, var(--danger), #dc2626);
  color: white;
  border: none;
}

.btn-ghost {
  background: transparent;
  border: 1px solid var(--border);
}

/* Menu Toggle */
.menu-toggle {
  display: none;
  position: fixed;
  top: 16px;
  left: 16px;
  z-index: 1001;
  background: var(--bg-card);
  border: 1px solid var(--border);
  color: var(--text-primary);
  padding: 12px;
  border-radius: 10px;
  cursor: pointer;
  font-size: 1.2rem;
  box-shadow: 0 4px 12px var(--shadow);
}

/* Stats Cards */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 16px;
  margin-bottom: 24px;
}

.stat-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  padding: 20px;
  border-radius: 14px;
  box-shadow: 0 2px 8px var(--shadow);
  animation: fadeIn 0.5s ease;
  transition: all 0.3s;
  position: relative;
  overflow: hidden;
}

.stat-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 3px;
  background: linear-gradient(90deg, var(--primary), var(--info));
}

.stat-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 24px var(--shadow);
}

.stat-label {
  font-size: 0.85rem;
  color: var(--text-secondary);
  font-weight: 500;
  margin-bottom: 8px;
}

.stat-value {
  font-size: 1.5rem;
  font-weight: 700;
  margin-bottom: 4px;
}

.stat-change {
  font-size: 0.8rem;
  display: flex;
  align-items: center;
  gap: 4px;
}

/* Search & Filters */
.filters-bar {
  display: flex;
  gap: 12px;
  margin-bottom: 20px;
  flex-wrap: wrap;
}

.input, select {
  background: var(--bg-card);
  border: 1px solid var(--border);
  padding: 10px 14px;
  border-radius: 10px;
  color: var(--text-primary);
  font-size: 0.9rem;
  transition: all 0.2s;
  font-family: inherit;
}

.input:focus, select:focus {
  outline: none;
  border-color: var(--primary);
  box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.1);
}

select option {
  background: var(--bg-card);
  color: var(--text-primary);
}

.search-input {
  flex: 1;
  min-width: 200px;
  padding-left: 40px;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='20' height='20' viewBox='0 0 24 24' fill='none' stroke='%239ca3af' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3E%3Ccircle cx='11' cy='11' r='8'%3E%3C/circle%3E%3Cpath d='m21 21-4.35-4.35'%3E%3C/path%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: 12px center;
}

/* Card Container */
.card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: 14px;
  padding: 20px;
  box-shadow: 0 2px 8px var(--shadow);
  animation: fadeIn 0.5s ease;
}

/* Table */
.table-wrapper {
  overflow-x: auto;
  border-radius: 12px;
}

.table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0;
}

.table thead {
  background: var(--bg-tertiary);
}

.table th {
  padding: 14px 12px;
  text-align: left;
  font-weight: 600;
  font-size: 0.85rem;
  color: var(--text-secondary);
  text-transform: uppercase;
  letter-spacing: 0.5px;
  border-bottom: 1px solid var(--border);
  position: sticky;
  top: 0;
  background: var(--bg-tertiary);
  z-index: 10;
}

.table th:first-child {
  border-top-left-radius: 12px;
}

.table th:last-child {
  border-top-right-radius: 12px;
}

.table td {
  padding: 14px 12px;
  border-bottom: 1px solid var(--border);
  font-size: 0.9rem;
}

.table tbody tr {
  transition: all 0.2s;
  cursor: move;
}

.table tbody tr:hover {
  background: var(--glass);
}

.table tbody tr.dragging {
  opacity: 0.5;
}

/* Item Row */
.item-name {
  font-weight: 600;
  color: var(--text-primary);
}

.thumb {
  width: 48px;
  height: 48px;
  object-fit: cover;
  border-radius: 8px;
  border: 1px solid var(--border);
}

.profit-positive { color: var(--success); font-weight: 600; }
.profit-negative { color: var(--danger); font-weight: 600; }
.profit-neutral { color: var(--text-muted); }

/* Checkbox */
.checkbox {
  width: 20px;
  height: 20px;
  cursor: pointer;
  accent-color: var(--primary);
}

/* Actions */
.row-actions {
  display: flex;
  gap: 6px;
}

.icon-btn {
  background: transparent;
  border: 1px solid var(--border);
  padding: 8px;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s;
  color: var(--text-secondary);
}

.icon-btn:hover {
  background: var(--glass);
  border-color: var(--primary);
  color: var(--primary);
}

/* Tooltip */
.tooltip {
  position: relative;
}

.tooltip::before {
  content: attr(data-tooltip);
  position: absolute;
  bottom: 100%;
  left: 50%;
  transform: translateX(-50%);
  background: var(--bg-secondary);
  color: var(--text-primary);
  padding: 8px 12px;
  border-radius: 8px;
  font-size: 0.8rem;
  white-space: nowrap;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.2s;
  margin-bottom: 8px;
  box-shadow: 0 4px 12px var(--shadow);
  border: 1px solid var(--border);
  z-index: 1000;
}

.tooltip:hover::before {
  opacity: 1;
}

/* Modal */
.modal-overlay {
  display: none;
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.7);
  backdrop-filter: blur(8px);
  align-items: center;
  justify-content: center;
  z-index: 2000;
  animation: fadeIn 0.2s ease;
  padding: 20px;
}

.modal-overlay.active {
  display: flex;
}

.modal-content {
  background: var(--bg-card);
  padding: 28px;
  border-radius: 16px;
  max-width: 520px;
  width: 100%;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
  animation: fadeIn 0.3s ease;
  max-height: 90vh;
  overflow-y: auto;
  border: 1px solid var(--border);
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  padding-bottom: 16px;
  border-bottom: 1px solid var(--border);
}

.modal-header h3 {
  font-size: 1.3rem;
  font-weight: 700;
}

.modal-close {
  background: transparent;
  border: none;
  color: var(--text-secondary);
  font-size: 1.8rem;
  cursor: pointer;
  transition: all 0.2s;
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 8px;
}

.modal-close:hover {
  background: var(--glass);
  color: var(--text-primary);
}

.modal-body {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.form-label {
  font-size: 0.85rem;
  font-weight: 600;
  color: var(--text-secondary);
}

.checkbox-wrapper {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 12px;
  background: var(--bg-tertiary);
  border-radius: 10px;
  cursor: pointer;
  transition: all 0.2s;
}

.checkbox-wrapper:hover {
  background: var(--glass);
}

.img-preview {
  max-width: 100%;
  max-height: 200px;
  border-radius: 10px;
  margin-top: 8px;
}

.modal-footer {
  display: flex;
  gap: 10px;
  justify-content: flex-end;
  margin-top: 20px;
  padding-top: 16px;
  border-top: 1px solid var(--border);
}

/* Toast */
.toast {
  position: fixed;
  bottom: 24px;
  right: 24px;
  background: var(--bg-card);
  padding: 16px 20px;
  border-radius: 12px;
  box-shadow: 0 8px 24px var(--shadow);
  z-index: 3000;
  min-width: 280px;
  animation: slideIn 0.3s ease;
  display: none;
  border: 1px solid var(--border);
  border-left: 4px solid var(--primary);
}

.toast.show { display: block; }
.toast.success { border-left-color: var(--success); }
.toast.error { border-left-color: var(--danger); }
.toast.warning { border-left-color: var(--warning); }

/* Pagination */
.pagination {
  display: flex;
  gap: 6px;
  justify-content: center;
  margin-top: 20px;
  flex-wrap: wrap;
}

.pagination button {
  background: var(--bg-tertiary);
  border: 1px solid var(--border);
  color: var(--text-primary);
  padding: 8px 14px;
  border-radius: 8px;
  cursor: pointer;
  font-weight: 500;
  transition: all 0.2s;
}

.pagination button:hover {
  background: var(--glass);
  border-color: var(--primary);
}

.pagination button.active {
  background: var(--primary);
  border-color: var(--primary);
  color: white;
}

/* Empty State */
.empty {
  padding: 60px 24px;
  text-align: center;
  color: var(--text-muted);
}

.empty-icon {
  font-size: 3rem;
  margin-bottom: 16px;
  opacity: 0.5;
}

/* Loading */
.loading {
  display: inline-block;
  width: 20px;
  height: 20px;
  border: 3px solid var(--border);
  border-top-color: var(--primary);
  border-radius: 50%;
  animation: spin 0.8s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

/* Responsive */
@media (max-width: 980px) {
  .app {
    grid-template-columns: 1fr;
  }
  
  .sidebar {
    position: fixed;
    left: 0;
    top: 0;
    bottom: 0;
    width: 280px;
    z-index: 1000;
    box-shadow: 4px 0 12px var(--shadow);
  }
  
  .menu-toggle {
    display: block;
  }
  
  .main {
    padding: 80px 16px 16px;
  }
  
  .stats-grid {
    grid-template-columns: 1fr;
  }
  
  .filters-bar {
    flex-direction: column;
  }
}
</style>
</head>
<body data-theme="dark">

<button class="menu-toggle" onclick="toggleMobileSidebar()">☰</button>

<div class="app">
  <aside class="sidebar" id="sidebar">
    <div class="brand">
      <div class="logo">IP</div>
      <div class="brand-text">Инвестиционный<br>портфель</div>
    </div>
    
    <nav class="nav">
      <a href="#items" data-route="items" class="active">
        <span>📦</span>
        <span>Список предметов</span>
      </a>
      <a href="#chart" data-route="chart">
        <span>📈</span>
        <span>Графики прибыли</span>
      </a>
      <a href="#settings" data-route="settings">
        <span>⚙️</span>
        <span>Настройки</span>
      </a>
    </nav>
    
    <div class="theme-toggle">
      <button class="theme-btn" onclick="setTheme('light')" id="lightBtn">☀️</button>
      <button class="theme-btn active" onclick="setTheme('dark')" id="darkBtn">🌙</button>
    </div>
    
    <div class="calc-widget">
      <h4>🧮 Быстрый калькулятор</h4>
      <div class="calc-inputs">
        <input id="quickPrice" class="input" type="number" placeholder="Цена за 1 шт">
        <input id="quickQty" class="input" type="number" placeholder="Количество">
        <div class="checkbox-wrapper">
          <input type="checkbox" id="quickAuction" class="checkbox">
          <label for="quickAuction">Аукцион (-5%)</label>
        </div>
        <div style="margin-top:8px;font-size:0.85rem;color:var(--text-secondary)">
          Итого: <strong id="quickResult" style="font-size:1.1rem;color:var(--text-primary)">—</strong>
        </div>
        <button class="btn btn-primary" onclick="quickCalc()">Посчитать</button>
      </div>
    </div>
  </aside>

  <main class="main">
    <!-- ITEMS PAGE -->
    <section id="page-items" class="page" data-route="items">
      <div class="header">
        <div class="h-title">Список предметов</div>
        <div class="controls">
          <button class="btn btn-primary" onclick="openAddItemModal()">+ Добавить</button>
          <button class="btn btn-danger" onclick="confirmClearAll()">Очистить</button>
        </div>
      </div>

      <div class="stats-grid">
        <div class="stat-card">
          <div class="stat-label">Сумма покупки</div>
          <div class="stat-value" id="totalBuy">0</div>
          <div class="stat-change">💰 Всего вложено</div>
        </div>
        <div class="stat-card">
          <div class="stat-label">Сумма продажи</div>
          <div class="stat-value" id="totalSell">0</div>
          <div class="stat-change">💵 Потенциальный доход</div>
        </div>
        <div class="stat-card">
          <div class="stat-label">Итоговая прибыль</div>
          <div class="stat-value" id="totalProfit">0</div>
          <div class="stat-change" id="profitChange">📊 ROI: <span id="roiPercent">0%</span></div>
        </div>
      </div>

      <div class="filters-bar">
        <input id="search" class="input search-input" placeholder="Поиск по названию..." oninput="renderTable()">
        <select id="filterProfit" class="input" onchange="renderTable()">
          <option value="all">Все предметы</option>
          <option value="profit-pos">Прибыльные</option>
          <option value="profit-neg">Убыточные</option>
        </select>
        <select id="sortBy" class="input" onchange="renderTable()">
          <option value="">Сортировка</option>
          <option value="name">По названию</option>
          <option value="buy">По цене покупки</option>
          <option value="sell">По цене продажи</option>
          <option value="profit">По прибыли</option>
        </select>
        <button class="btn" onclick="exportCSV()">📥 Экспорт CSV</button>
      </div>

      <div class="card">
        <div class="table-wrapper">
          <table class="table" id="itemsTable">
            <thead>
              <tr>
                <th>Название</th>
                <th>Фото</th>
                <th>Цена покуп.</th>
                <th>Цена прод.</th>
                <th>Кол-во</th>
                <th>Аукцион</th>
                <th>Сум. покуп.</th>
                <th>Сум. прод.</th>
                <th>Прибыль</th>
                <th>Действия</th>
              </tr>
            </thead>
            <tbody id="itemsTableBody"></tbody>
          </table>
        </div>
        <div class="empty" id="itemsEmpty" style="display:none">
          <div class="empty-icon">📦</div>
          <div>Нет предметов — добавьте первый</div>
        </div>
        <div class="pagination" id="pagination"></div>
      </div>
    </section>

    <!-- CHART PAGE -->
    <section id="page-chart" class="page" data-route="chart" style="display:none">
      <div class="header">
        <div class="h-title">Графики прибыли</div>
        <div class="controls">
          <button class="btn" onclick="refreshCharts()">🔄 Обновить</button>
          <button class="btn btn-ghost" onclick="toggleChartType()">📊 Тип графика</button>
        </div>
      </div>

      <div class="card" style="margin-bottom:20px">
        <h3 style="margin-bottom:16px;font-size:1.1rem">Накопленная прибыль</h3>
        <canvas id="profitChart" height="100"></canvas>
      </div>

      <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(300px,1fr));gap:20px">
        <div class="card">
          <h3 style="margin-bottom:16px;font-size:1.1rem">Топ-10 по прибыли</h3>
          <canvas id="topChart" height="200"></canvas>
        </div>
        <div class="card">
          <h3 style="margin-bottom:16px;font-size:1.1rem">Статистика</h3>
          <div style="display:flex;flex-direction:column;gap:12px">
            <div style="display:flex;justify-content:space-between">
              <span style="color:var(--text-secondary)">Покупка:</span>
              <strong id="chartBuySum">0</strong>
            </div>
            <div style="display:flex;justify-content:space-between">
              <span style="color:var(--text-secondary)">Продажа:</span>
              <strong id="chartSellSum">0</strong>
            </div>
            <div style="display:flex;justify-content:space-between">
              <span style="color:var(--text-secondary)">Прибыль:</span>
              <strong id="chartProfitSum" class="profit-positive">0</strong>
            </div>
            <div style="display:flex;justify-content:space-between">
              <span style="color:var(--text-secondary)">Предметов:</span>
              <strong id="chartItemCount">0</strong>
            </div>
            <div style="display:flex;justify-content:space-between">
              <span style="color:var(--text-secondary)">Средняя прибыль:</span>
              <strong id="chartAvgProfit">0</strong>
            </div>
            <div style="display:flex;justify-content:space-between">
              <span style="color:var(--text-secondary)">ROI:</span>
              <strong id="chartROI">0%</strong>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- SETTINGS PAGE -->
    <section id="page-settings" class="page" data-route="settings" style="display:none">
      <div class="header">
        <div class="h-title">Настройки</div>
        <div class="controls">
          <button class="btn btn-danger" onclick="confirmClearStorage()">🗑️ Сбросить всё</button>
        </div>
      </div>

      <div class="card" style="margin-bottom:20px">
        <h3 style="margin-bottom:16px;font-size:1.1rem">Экспорт / Импорт</h3>
        <div style="display:flex;gap:12px;flex-wrap:wrap;margin-bottom:16px">
          <button class="btn btn-primary" onclick="exportAllJSON()">📥 Экспорт JSON</button>
          <button class="btn" onclick="exportCSV()">📥 Экспорт CSV</button>
        </div>
        <div class="form-group">
          <label class="form-label">Импорт данных</label>
          <input type="file" id="importAll" onchange="importAll(this)" accept="application/json,text/csv" class="input"/>
        </div>
      </div>

      <div class="card">
        <h3 style="margin-bottom:16px;font-size:1.1rem">Информация</h3>
        <div style="display:flex;flex-direction:column;gap:10px;font-size:0.9rem">
          <div style="display:flex;justify-content:space-between">
            <span style="color:var(--text-secondary)">Предметов:</span>
            <strong id="settingsItemCount">0</strong>
          </div>
          <div style="display:flex;justify-content:space-between">
            <span style="color:var(--text-secondary)">Размер данных:</span>
            <strong id="settingsDataSize">0 КБ</strong>
          </div>
          <div style="display:flex;justify-content:space-between">
            <span style="color:var(--text-secondary)">Тема:</span>
            <strong id="currentTheme">Тёмная</strong>
          </div>
        </div>
      </div>
    </section>
  </main>
</div>

<!-- Add Item Modal -->
<div id="addItemModal" class="modal-overlay">
  <div class="modal-content">
    <div class="modal-header">
      <h3>Добавить предмет</h3>
      <button class="modal-close" onclick="closeAddItemModal()">×</button>
    </div>
    <div class="modal-body">
      <div class="form-group">
        <label class="form-label">Название</label>
        <input id="newItemName" class="input" placeholder="Название предмета">
      </div>
      <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px">
        <div class="form-group">
          <label class="form-label">Цена покупки</label>
          <input id="newItemBuy" class="input" type="number" placeholder="0">
        </div>
        <div class="form-group">
          <label class="form-label">Цена продажи</label>
          <input id="newItemSell" class="input" type="number" placeholder="0">
        </div>
      </div>
      <div class="form-group">
        <label class="form-label">Количество</label>
        <input id="newItemQty" class="input" type="number" placeholder="0">
      </div>
      <div class="checkbox-wrapper">
        <input type="checkbox" id="newItemAuction" class="checkbox">
        <label for="newItemAuction">Продажа через аукцион (-5% комиссия)</label>
      </div>
      <div class="form-group">
        <label class="form-label">Изображение (необязательно)</label>
        <input id="newItemImg" class="input" type="file" accept="image/*" onchange="previewImage(this)">
      </div>
      <img id="imgPreview" class="img-preview" style="display:none">
    </div>
    <div class="modal-footer">
      <button class="btn btn-ghost" onclick="closeAddItemModal()">Отмена</button>
      <button class="btn btn-primary" onclick="saveNewItem()">Добавить</button>
    </div>
  </div>
</div>

<!-- Confirm Modal -->
<div id="confirmModal" class="modal-overlay">
  <div class="modal-content" style="max-width:400px">
    <div class="modal-header">
      <h3 id="confirmTitle">Подтверждение</h3>
      <button class="modal-close" onclick="closeConfirmModal()">×</button>
    </div>
    <div class="modal-body">
      <p id="confirmMessage" style="color:var(--text-secondary)"></p>
    </div>
    <div class="modal-footer">
      <button class="btn btn-ghost" onclick="closeConfirmModal()">Отмена</button>
      <button class="btn btn-danger" id="confirmBtn" onclick="confirmAction()">Подтвердить</button>
    </div>
  </div>
</div>

<div id="toast" class="toast"></div>

<script>
const SAMPLE_ITEMS = [
  "Кофе","Дрожжи","Ноотроп","Мультитул","Набор болтов","Копыто Сильного кабана",
  "Прочный металл","Защитное покрытие","Сверло","Аммиак","Смазочный материал",
  "Полиэтиленовая основа","Газовый балон","Нейродегенерант","Азотная кислота",
  "Набор специй","Тушенка","Банка с пшеном","Помидор","Любое мясо","Красная икра",
  "Железо","Аномальная плазма","Глицерин","Порох","Бутылка с водой","Чеснок"
];

let items = JSON.parse(localStorage.getItem('pf_items')||'[]');
let chartType = 'line';
let currentPage = 1;
const itemsPerPage = 20;
let confirmCallback = null;
let sortable = null;

function saveItems(){ 
  localStorage.setItem('pf_items', JSON.stringify(items)); 
  updateSettingsInfo(); 
}

function uid(){ 
  return Math.random().toString(36).slice(2,9); 
}

function formatNumber(num) {
  return num.toFixed(2).replace(/\B(?=(\d{3})+(?!\d))/g, ".");
}

function showToast(message, type = 'success') {
  const toast = document.getElementById('toast');
  toast.textContent = message;
  toast.className = `toast ${type} show`;
  setTimeout(() => toast.classList.remove('show'), 3000);
}

// Theme Management
function setTheme(theme) {
  document.body.setAttribute('data-theme', theme);
  localStorage.setItem('pf_theme', theme);
  document.getElementById('lightBtn').classList.toggle('active', theme === 'light');
  document.getElementById('darkBtn').classList.toggle('active', theme === 'dark');
  document.getElementById('currentTheme').textContent = theme === 'dark' ? 'Тёмная' : 'Светлая';
  if (window.profitChart) renderCharts();
}

function loadTheme() {
  const savedTheme = localStorage.getItem('pf_theme') || 'dark';
  setTheme(savedTheme);
}

function toggleMobileSidebar() {
  const sidebar = document.getElementById('sidebar');
  sidebar.classList.toggle('mobile-hidden');
}

function route(){
  const hash = location.hash.replace('#','') || 'items';
  document.querySelectorAll('.nav a').forEach(a=>{
    const isActive = a.getAttribute('data-route')===hash;
    a.classList.toggle('active', isActive);
    if(isActive && window.innerWidth <= 980) toggleMobileSidebar();
  });
  document.querySelectorAll('.page').forEach(p=>p.style.display = (p.getAttribute('data-route')===hash)?'block':'none');
  if(hash==='items') renderTable();
  if(hash==='chart') renderCharts();
  if(hash==='settings') updateSettingsInfo();
}

window.addEventListener('hashchange', route);

function compressImage(file, maxWidth = 200, quality = 0.7) {
  return new Promise((resolve) => {
    const reader = new FileReader();
    reader.onload = (e) => {
      const img = new Image();
      img.onload = () => {
        const canvas = document.createElement('canvas');
        let width = img.width;
        let height = img.height;
        
        if (width > maxWidth) {
          height = (height * maxWidth) / width;
          width = maxWidth;
        }
        
        canvas.width = width;
        canvas.height = height;
        const ctx = canvas.getContext('2d');
        ctx.drawImage(img, 0, 0, width, height);
        resolve(canvas.toDataURL('image/jpeg', quality));
      };
      img.src = e.target.result;
    };
    reader.readAsDataURL(file);
  });
}

function calculateProfit(item) {
  const buy = item.buy || 0;
  let sell = item.sell || 0;
  const qty = item.qty || 0;
  
  if (item.auction) {
    sell = sell * 0.95;
  }
  
  return (sell - buy) * qty;
}

function openAddItemModal() {
  document.getElementById('addItemModal').classList.add('active');
  document.getElementById('newItemName').value = '';
  document.getElementById('newItemBuy').value = '';
  document.getElementById('newItemSell').value = '';
  document.getElementById('newItemQty').value = '';
  document.getElementById('newItemAuction').checked = false;
  document.getElementById('newItemImg').value = '';
  document.getElementById('imgPreview').style.display = 'none';
}

function closeAddItemModal() {
  document.getElementById('addItemModal').classList.remove('active');
}

function previewImage(input) {
  const preview = document.getElementById('imgPreview');
  const file = input.files[0];
  if (file) {
    const reader = new FileReader();
    reader.onload = (e) => {
      preview.src = e.target.result;
      preview.style.display = 'block';
    };
    reader.readAsDataURL(file);
  } else {
    preview.style.display = 'none';
  }
}

async function saveNewItem() {
  const name = document.getElementById('newItemName').value.trim();
  const buy = parseFloat(document.getElementById('newItemBuy').value) || 0;
  const sell = parseFloat(document.getElementById('newItemSell').value) || 0;
  const qty = parseFloat(document.getElementById('newItemQty').value) || 0;
  const auction = document.getElementById('newItemAuction').checked;
  
  if (!name) {
    showToast('Введите название предмета', 'error');
    return;
  }
  
  if (buy < 0 || sell < 0 || qty < 0) {
    showToast('Значения не могут быть отрицательными', 'error');
    return;
  }
  
  let img = '';
  const fileInput = document.getElementById('newItemImg');
  if (fileInput.files[0]) {
    img = await compressImage(fileInput.files[0]);
  }
  
  const item = { id: uid(), name, buy, sell, qty, auction, img };
  items.push(item);
  saveItems();
  renderTable();
  closeAddItemModal();
  showToast('Предмет добавлен', 'success');
}

function createRowElement(item){
  const tr = document.createElement('tr');
  tr.dataset.id = item.id;
  const profit = calculateProfit(item);
  const profitClass = profit > 0 ? 'profit-positive' : profit < 0 ? 'profit-negative' : 'profit-neutral';
  
  const sumBuy = (item.buy||0)*(item.qty||0);
  let sumSell = (item.sell||0)*(item.qty||0);
  if (item.auction) sumSell = sumSell * 0.95;
  
  tr.innerHTML = `
    <td><div class="item-name tooltip" data-tooltip="Прибыль: ${formatNumber(profit)}">${escapeHtml(item.name)}</div></td>
    <td>${item.img ? `<img class='thumb' src='${item.img}' onerror="this.style.display='none'"/>` : '—'}</td>
    <td><input class="input" value="${item.buy||0}" oninput="onItemEdit(this,'buy')" type="number" min="0" style="width:90px"></td>
    <td><input class="input" value="${item.sell||0}" oninput="onItemEdit(this,'sell')" type="number" min="0" style="width:90px"></td>
    <td><input class="input" value="${item.qty||0}" oninput="onItemEdit(this,'qty')" type="number" min="0" style="width:80px"></td>
    <td style="text-align:center"><input type="checkbox" ${item.auction?'checked':''} onchange="onItemEdit(this,'auction')" class="checkbox"></td>
    <td>${formatNumber(sumBuy)}</td>
    <td>${formatNumber(sumSell)}</td>
    <td class="${profitClass}">${formatNumber(profit)}</td>
    <td>
      <div class='row-actions'>
        <button class='icon-btn tooltip' data-tooltip="Дублировать" onclick='duplicateItem(this)'>⎘</button>
        <button class='icon-btn tooltip' data-tooltip="Удалить" onclick='deleteItem(this)' style="color:var(--danger)">🗑️</button>
      </div>
    </td>
  `;
  return tr;
}

function renderTable(){
  const tbody = document.getElementById('itemsTableBody'); 
  tbody.innerHTML='';
  const q = document.getElementById('search').value.toLowerCase();
  const filter = document.getElementById('filterProfit').value;
  const sortBy = document.getElementById('sortBy').value;
  let list = items.slice();
  
  if(q) list = list.filter(i=> (i.name||'').toLowerCase().includes(q));
  if(filter==='profit-pos') list = list.filter(i=> calculateProfit(i) > 0);
  if(filter==='profit-neg') list = list.filter(i=> calculateProfit(i) < 0);
  
  if(sortBy){
    const map = {
      name: a=>(a.name||'').toLowerCase(), 
      buy:a=>a.buy||0, 
      sell:a=>a.sell||0, 
      profit:a=>calculateProfit(a)
    };
    list.sort((x,y)=>{ 
      const A=map[sortBy](x), B=map[sortBy](y); 
      if(A<B) return -1; 
      if(A>B) return 1; 
      return 0;
    });
  }
  
  const totalPages = Math.ceil(list.length / itemsPerPage);
  const startIdx = (currentPage - 1) * itemsPerPage;
  const endIdx = startIdx + itemsPerPage;
  const pageItems = list.slice(startIdx, endIdx);
  
  pageItems.forEach(i=>tbody.appendChild(createRowElement(i)));
  
  document.getElementById('itemsEmpty').style.display = list.length? 'none':'block';
  renderPagination(totalPages);
  updateTotals();
  
  // Init drag and drop
  if (sortable) sortable.destroy();
  sortable = Sortable.create(tbody, {
    animation: 150,
    handle: 'tr',
    ghostClass: 'dragging',
    onEnd: function(evt) {
      const movedItem = list[evt.oldIndex];
      list.splice(evt.oldIndex, 1);
      list.splice(evt.newIndex, 0, movedItem);
      items = list;
      saveItems();
      showToast('Порядок изменён', 'success');
    }
  });
}

function renderPagination(totalPages) {
  const pagination = document.getElementById('pagination');
  pagination.innerHTML = '';
  
  if (totalPages <= 1) return;
  
  for (let i = 1; i <= totalPages; i++) {
    const btn = document.createElement('button');
    btn.textContent = i;
    btn.className = i === currentPage ? 'active' : '';
    btn.onclick = () => {
      currentPage = i;
      renderTable();
    };
    pagination.appendChild(btn);
  }
}

function onItemEdit(input, field){
  const id = input.closest('tr').dataset.id; 
  let v;
  
  if (field === 'auction') {
    v = input.checked;
  } else {
    v = parseFloat(input.value)||0;
    if (v < 0) {
      showToast('Значение не может быть отрицательным', 'warning');
      v = 0;
      input.value = 0;
    }
  }
  
  const it = items.find(x=>x.id===id); 
  if(!it) return; 
  it[field]=v; 
  saveItems(); 
  renderTable(); 
  renderCharts();
}

function deleteItem(btn){ 
  const tr=btn.closest('tr'); 
  const id=tr.dataset.id; 
  items = items.filter(i=>i.id!==id); 
  saveItems(); 
  renderTable(); 
  renderCharts();
  showToast('Предмет удалён', 'success');
}

function duplicateItem(btn){ 
  const id=btn.closest('tr').dataset.id; 
  const it = items.find(i=>i.id===id); 
  if(!it) return; 
  const copy = {...it, id:uid(), name:it.name+' (копия)'}; 
  items.push(copy); 
  saveItems(); 
  renderTable();
  showToast('Предмет продублирован', 'success');
}

function showConfirm(title, message, callback) {
  document.getElementById('confirmTitle').textContent = title;
  document.getElementById('confirmMessage').textContent = message;
  document.getElementById('confirmModal').classList.add('active');
  confirmCallback = callback;
}

function closeConfirmModal() {
  document.getElementById('confirmModal').classList.remove('active');
  confirmCallback = null;
}

function confirmAction() {
  if (confirmCallback) confirmCallback();
  closeConfirmModal();
}

function confirmClearAll() {
  showConfirm('Очистить все предметы?', 'Это действие необратимо. Все предметы будут удалены.', () => {
    items = [];
    saveItems();
    currentPage = 1;
    renderTable();
    showToast('Все предметы удалены', 'success');
  });
}

function updateTotals(){
  let tb=0, ts=0, tp=0;
  items.forEach(i=>{ 
    tb += (i.buy||0)*(i.qty||0);
    let sellSum = (i.sell||0)*(i.qty||0);
    if (i.auction) sellSum *= 0.95;
    ts += sellSum;
    tp += calculateProfit(i);
  });
  
  document.getElementById('totalBuy').textContent = formatNumber(tb);
  document.getElementById('totalSell').textContent = formatNumber(ts);
  document.getElementById('totalProfit').textContent = formatNumber(tp);
  
  const roi = tb > 0 ? ((tp / tb) * 100) : 0;
  document.getElementById('roiPercent').textContent = roi.toFixed(2) + '%';
  
  const profitEl = document.getElementById('profitChange');
  profitEl.className = `stat-change ${tp > 0 ? 'profit-positive' : tp < 0 ? 'profit-neutral' : 'profit-negative'}`;
}

function escapeHtml(text) {
  const div = document.createElement('div');
  div.textContent = text;
  return div.innerHTML;
}

// Chart functions
let profitChart, topChart;

function renderCharts() {
  renderProfitChart();
  renderTopChart();
  updateChartStats();
}

function renderProfitChart() {
  const ctx = document.getElementById('profitChart').getContext('2d');
  
  if (profitChart) profitChart.destroy();
  
  // Calculate cumulative profit over time (simple progression)
  const cumulativeData = [];
  let cumulativeProfit = 0;
  
  items.forEach((item, index) => {
    cumulativeProfit += calculateProfit(item);
    cumulativeData.push(cumulativeProfit);
  });
  
  // If no items, show empty chart
  const labels = items.length > 0 
    ? items.map((_, i) => `Поз. ${i + 1}`)
    : ['Нет данных'];
  
  const data = items.length > 0 ? cumulativeData : [0];
  
  profitChart = new Chart(ctx, {
    type: 'line',
    data: {
      labels: labels,
      datasets: [{
        label: 'Накопленная прибыль',
        data: data,
        borderColor: '#10b981',
        backgroundColor: 'rgba(16, 185, 129, 0.1)',
        borderWidth: 3,
        fill: true,
        tension: 0.4,
        pointBackgroundColor: '#10b981',
        pointBorderColor: '#ffffff',
        pointBorderWidth: 2,
        pointRadius: 6,
        pointHoverRadius: 8
      }]
    },
    options: {
      responsive: true,
      plugins: {
        legend: {
          display: true,
          position: 'top',
        },
        tooltip: {
          callbacks: {
            label: function(context) {
              return `Прибыль: ${formatNumber(context.raw)}`;
            }
          }
        }
      },
      scales: {
        y: {
          beginAtZero: true,
          grid: {
            color: 'rgba(255,255,255,0.1)'
          },
          ticks: {
            callback: function(value) {
              return formatNumber(value);
            }
          },
          title: {
            display: true,
            text: 'Сумма прибыли'
          }
        },
        x: {
          grid: {
            color: 'rgba(255,255,255,0.1)'
          },
          title: {
            display: true,
            text: 'Позиции в портфеле'
          }
        }
      }
    }
  });
}

function renderTopChart() {
  const ctx = document.getElementById('topChart').getContext('2d');
  
  if (topChart) topChart.destroy();
  
  // Get all items sorted by profit
  const sortedItems = items
    .map(item => ({
      name: item.name,
      profit: calculateProfit(item),
      buy: (item.buy || 0) * (item.qty || 0),
      sell: (item.sell || 0) * (item.qty || 0) * (item.auction ? 0.95 : 1)
    }))
    .sort((a, b) => b.profit - a.profit)
    .slice(0, 8);
  
  topChart = new Chart(ctx, {
    type: 'bar',
    data: {
      labels: sortedItems.map(item => item.name.length > 12 ? item.name.substring(0, 12) + '...' : item.name),
      datasets: [
        {
          label: 'Выручка',
          data: sortedItems.map(item => item.sell),
          backgroundColor: 'rgba(59, 130, 246, 0.7)',
          borderColor: 'rgba(59, 130, 246, 1)',
          borderWidth: 1,
          borderRadius: 4,
        },
        {
          label: 'Прибыль',
          data: sortedItems.map(item => item.profit),
          backgroundColor: sortedItems.map(item => 
            item.profit > 0 ? 'rgba(16, 185, 129, 0.7)' : 'rgba(239, 68, 68, 0.7)'
          ),
          borderColor: sortedItems.map(item => 
            item.profit > 0 ? 'rgba(16, 185, 129, 1)' : 'rgba(239, 68, 68, 1)'
          ),
          borderWidth: 1,
          borderRadius: 4,
        }
      ]
    },
    options: {
      responsive: true,
      plugins: {
        legend: {
          display: true,
          position: 'top',
        }
      },
      scales: {
        y: {
          beginAtZero: true,
          grid: {
            color: 'rgba(255,255,255,0.1)'
          },
          ticks: {
            callback: function(value) {
              return formatNumber(value);
            }
          }
        },
        x: {
          grid: {
            display: false
          }
        }
      }
    }
  });
}

// Function to generate chart image for Telegram
function generateChartImage() {
  const canvas = document.getElementById('profitChart');
  return canvas.toDataURL('image/png');
}

// Function to get portfolio stats for Telegram
function getPortfolioStats() {
  let totalBuy = 0, totalSell = 0, totalProfit = 0;
  
  items.forEach(item => {
    totalBuy += (item.buy || 0) * (item.qty || 0);
    let sellSum = (item.sell || 0) * (item.qty || 0);
    if (item.auction) sellSum *= 0.95;
    totalSell += sellSum;
    totalProfit += calculateProfit(item);
  });
  
  const roi = totalBuy > 0 ? (totalProfit / totalBuy) * 100 : 0;
  
  return {
    totalBuy: formatNumber(totalBuy),
    totalSell: formatNumber(totalSell),
    totalProfit: formatNumber(totalProfit),
    itemCount: items.length,
    roi: roi.toFixed(2) + '%'
  };
}

function updateChartStats() {
  const stats = getPortfolioStats();
  
  document.getElementById('chartBuySum').textContent = stats.totalBuy;
  document.getElementById('chartSellSum').textContent = stats.totalSell;
  document.getElementById('chartProfitSum').textContent = stats.totalProfit;
  document.getElementById('chartProfitSum').className = parseFloat(stats.totalProfit.replace(/\./g, '')) > 0 ? 'profit-positive' : 'profit-negative';
  document.getElementById('chartItemCount').textContent = stats.itemCount;
  
  const avgProfit = items.length > 0 ? parseFloat(stats.totalProfit.replace(/\./g, '')) / items.length : 0;
  document.getElementById('chartAvgProfit').textContent = formatNumber(avgProfit);
  document.getElementById('chartROI').textContent = stats.roi;
  document.getElementById('chartROI').className = parseFloat(stats.roi) > 0 ? 'profit-positive' : 'profit-negative';
}

function toggleChartType() {
  renderCharts();
  showToast('Графики обновлены', 'success');
}

function refreshCharts() {
  renderCharts();
  showToast('Графики обновлены', 'success');
}

// Export/Import functions
function exportCSV() {
  if (items.length === 0) {
    showToast('Нет данных для экспорта', 'warning');
    return;
  }
  
  const headers = ['Название', 'Цена покупки', 'Цена продажи', 'Количество', 'Аукцион', 'Сумма покупки', 'Сумма продажи', 'Прибыль'];
  
  let csv = headers.join(';') + '\n';
  
  items.forEach(item => {
    const sumBuy = (item.buy || 0) * (item.qty || 0);
    let sumSell = (item.sell || 0) * (item.qty || 0);
    if (item.auction) sumSell *= 0.95;
    const profit = calculateProfit(item);
    
    const row = [
      `"${item.name}"`,
      item.buy || 0,
      item.sell || 0,
      item.qty || 0,
      item.auction ? 'Да' : 'Нет',
      sumBuy,
      sumSell,
      profit
    ];
    
    csv += row.join(';') + '\n';
  });
  
  const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
  const link = document.createElement('a');
  const url = URL.createObjectURL(blob);
  link.setAttribute('href', url);
  link.setAttribute('download', `portfolio_${new Date().toISOString().split('T')[0]}.csv`);
  link.style.visibility = 'hidden';
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
  
  showToast('CSV файл экспортирован', 'success');
}

function exportAllJSON() {
  if (items.length === 0) {
    showToast('Нет данных для экспорта', 'warning');
    return;
  }
  
  const data = {
    items: items,
    exportDate: new Date().toISOString(),
    version: '1.0'
  };
  
  const blob = new Blob([JSON.stringify(data, null, 2)], { type: 'application/json' });
  const link = document.createElement('a');
  const url = URL.createObjectURL(blob);
  link.setAttribute('href', url);
  link.setAttribute('download', `portfolio_backup_${new Date().toISOString().split('T')[0]}.json`);
  link.style.visibility = 'hidden';
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
  
  showToast('JSON файл экспортирован', 'success');
}

function importAll(input) {
  const file = input.files[0];
  if (!file) return;
  
  const reader = new FileReader();
  reader.onload = function(e) {
    try {
      const content = e.target.result;
      
      if (file.type === 'application/json' || file.name.endsWith('.json')) {
        // JSON import
        const data = JSON.parse(content);
        
        if (data.items && Array.isArray(data.items)) {
          showConfirm('Импорт данных?', `Будет добавлено ${data.items.length} предметов. Текущие данные будут сохранены.`, () => {
            items = [...items, ...data.items.map(item => ({ ...item, id: uid() }))];
            saveItems();
            renderTable();
            showToast(`Импортировано ${data.items.length} предметов`, 'success');
            input.value = '';
          });
        } else {
          showToast('Неверный формат JSON файла', 'error');
        }
      } else {
        // CSV import
        const lines = content.split('\n').filter(line => line.trim());
        const headers = lines[0].split(';').map(h => h.replace(/"/g, '').trim());
        
        const importedItems = [];
        
        for (let i = 1; i < lines.length; i++) {
          const values = lines[i].split(';').map(v => v.replace(/"/g, '').trim());
          if (values.length >= 4) {
            const item = {
              id: uid(),
              name: values[0] || 'Без названия',
              buy: parseFloat(values[1]) || 0,
              sell: parseFloat(values[2]) || 0,
              qty: parseFloat(values[3]) || 0,
              auction: values[4] ? values[4].toLowerCase().includes('да') : false
            };
            importedItems.push(item);
          }
        }
        
        if (importedItems.length > 0) {
          showConfirm('Импорт данных?', `Будет добавлено ${importedItems.length} предметов из CSV. Текущие данные будут сохранены.`, () => {
            items = [...items, ...importedItems];
            saveItems();
            renderTable();
            showToast(`Импортировано ${importedItems.length} предметов из CSV`, 'success');
            input.value = '';
          });
        } else {
          showToast('Не удалось прочитать данные из CSV', 'error');
        }
      }
    } catch (error) {
      showToast('Ошибка при импорте файла: ' + error.message, 'error');
    }
  };
  reader.readAsText(file);
}

function confirmClearStorage() {
  showConfirm('Сбросить все данные?', 'Будут удалены все предметы и настройки. Это действие необратимо.', () => {
    localStorage.clear();
    items = [];
    saveItems();
    loadTheme();
    renderTable();
    showToast('Все данные сброшены', 'success');
  });
}

function updateSettingsInfo() {
  document.getElementById('settingsItemCount').textContent = items.length;
  
  const dataSize = JSON.stringify(items).length;
  document.getElementById('settingsDataSize').textContent = (dataSize / 1024).toFixed(2) + ' КБ';
}

// Quick Calculator
function quickCalc() {
  const price = parseFloat(document.getElementById('quickPrice').value) || 0;
  const qty = parseFloat(document.getElementById('quickQty').value) || 0;
  const auction = document.getElementById('quickAuction').checked;
  
  if (price <= 0 || qty <= 0) {
    document.getElementById('quickResult').textContent = '—';
    return;
  }
  
  let total = price * qty;
  if (auction) total *= 0.95;
  
  document.getElementById('quickResult').textContent = formatNumber(total);
}

// Add sample data for testing
function addSampleData() {
  if (items.length > 0) {
    showConfirm('Добавить тестовые данные?', 'Будут добавлены случайные предметы для демонстрации. Текущие данные сохранятся.', () => {
      addSamples();
    });
  } else {
    addSamples();
  }
}

function addSamples() {
  const samples = [];
  
  for (let i = 0; i < 8; i++) {
    const name = SAMPLE_ITEMS[Math.floor(Math.random() * SAMPLE_ITEMS.length)];
    const buy = Math.floor(Math.random() * 1000) + 100;
    const sell = buy * (0.8 + Math.random() * 0.6); // -20% to +40%
    const qty = Math.floor(Math.random() * 10) + 1;
    const auction = Math.random() > 0.5;
    
    samples.push({
      id: uid(),
      name: name + (i > 0 ? ` ${i}` : ''),
      buy: buy,
      sell: Math.round(sell),
      qty: qty,
      auction: auction
    });
  }
  
  items = [...items, ...samples];
  saveItems();
  renderTable();
  showToast('Добавлены тестовые данные', 'success');
}

// Initialize application
function initApp() {
  loadTheme();
  route();
  renderTable();
  
  // Add sample data button for demo
  if (items.length === 0) {
    const controls = document.querySelector('.controls');
    const sampleBtn = document.createElement('button');
    sampleBtn.className = 'btn btn-success';
    sampleBtn.textContent = '🎲 Добавить тестовые данные';
    sampleBtn.onclick = addSampleData;
    controls.appendChild(sampleBtn);
  }
}

// Start the app when DOM is loaded
document.addEventListener('DOMContentLoaded', initApp);
</script>
</body>
</html>
