# Budget-Plannner
WEB APP
Helps one to budget esp sallary even a companies expenses 
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Budget Planner</title>
  <style>
    :root {
      --bg: #0f172a;           /* slate-900 */
      --panel: #111827;        /* gray-900 */
      --panel-2: #0b1220;      /* deep blend */
      --text: #e5e7eb;         /* gray-200 */
      --muted: #9ca3af;        /* gray-400 */
      --primary: #22d3ee;      /* cyan-400 */
      --primary-2: #06b6d4;    /* cyan-500 */
      --danger: #ef4444;       /* red-500 */
      --success: #22c55e;      /* green-500 */
      --warning: #f59e0b;      /* amber-500 */
      --ring: rgba(34, 211, 238, 0.35);
      --radius: 16px;
      --shadow: 0 10px 25px rgba(0,0,0,.35);
      --shadow-soft: 0 6px 16px rgba(0,0,0,.25);
    }

    * { box-sizing: border-box; }
    html, body { height: 100%; }
    body {
      margin: 0;
      font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Ubuntu, Cantarell, Noto Sans, Helvetica Neue, Arial, "Apple Color Emoji", "Segoe UI Emoji";
      color: var(--text);
      background: radial-gradient(1200px 600px at 10% -10%, #0b1220 10%, transparent 40%),
                  radial-gradient(800px 500px at 90% -20%, #0b1220 10%, transparent 40%),
                  linear-gradient(180deg, #0b1220, var(--bg));
    }

    .container {
      max-width: 1100px;
      margin: 32px auto 80px;
      padding: 0 16px;
    }

    header {
      display: flex; align-items: center; justify-content: space-between;
      gap: 12px; flex-wrap: wrap;
      margin-bottom: 20px;
    }
    .title {
      display: flex; align-items: center; gap: 12px;
    }
    .logo {
      width: 44px; height: 44px; border-radius: 12px;
      background: linear-gradient(135deg, var(--primary), var(--primary-2));
      box-shadow: 0 6px 20px rgba(34,211,238,.35);
    }
    h1 { font-size: 1.4rem; margin: 0; letter-spacing: .3px; }
    .subtitle { color: var(--muted); font-size: .9rem; }

    .grid { display: grid; gap: 16px; }
    @media (min-width: 900px) {
      .grid-main { grid-template-columns: 1.1fr .9fr; align-items: start; }
    }

    .card {
      background: linear-gradient(180deg, var(--panel), var(--panel-2));
      border: 1px solid rgba(255,255,255,.06);
      border-radius: var(--radius);
      box-shadow: var(--shadow-soft);
      padding: 16px;
    }

    .summary { display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; }
    .summary .item { padding: 14px; border-radius: 14px; background: rgba(255,255,255,.03); border: 1px solid rgba(255,255,255,.06); }
    .summary .label { color: var(--muted); font-size: .85rem; }
    .summary .value { font-size: 1.25rem; font-weight: 700; margin-top: 6px; }

    .tag { font-size: .75rem; padding: 4px 8px; border-radius: 999px; border: 1px solid rgba(255,255,255,.08); color: var(--muted); }

    /* Form */
    .form-grid { display: grid; gap: 12px; grid-template-columns: 1fr 1fr; }
    .form-row { display: contents; }
    .field { display: flex; flex-direction: column; gap: 6px; }
    .field label { color: var(--muted); font-size: .85rem; }
    .input, select {
      background: #0b1220; color: var(--text); border: 1px solid rgba(255,255,255,.08);
      border-radius: 12px; padding: 12px 12px; outline: none;
    }
    .input:focus, select:focus { border-color: var(--primary); box-shadow: 0 0 0 4px var(--ring); }

    .actions { display: flex; gap: 10px; flex-wrap: wrap; }
    .btn {
      border: none; border-radius: 12px; padding: 12px 14px; cursor: pointer;
      background: linear-gradient(135deg, var(--primary), var(--primary-2));
      color: #021318; font-weight: 700; letter-spacing: .2px; box-shadow: 0 10px 20px rgba(34,211,238,.25);
      transition: transform .04s ease;
    }
    .btn:active { transform: translateY(1px); }
    .btn.secondary { background: transparent; color: var(--text); border: 1px solid rgba(255,255,255,.12); box-shadow: none; }
    .btn.danger { background: linear-gradient(135deg, #fb7185, var(--danger)); box-shadow: 0 8px 20px rgba(239,68,68,.25); }

    /* Table */
    .toolbar { display: flex; gap: 10px; flex-wrap: wrap; align-items: center; }
    .toolbar .spacer { flex: 1; }

    table { width: 100%; border-collapse: collapse; margin-top: 10px; }
    th, td { text-align: left; padding: 12px 10px; border-bottom: 1px solid rgba(255,255,255,.06); }
    th { color: var(--muted); font-weight: 600; font-size: .85rem; }

    .row { transition: background .15s ease; }
    .row:hover { background: rgba(255,255,255,.03); }

    .amount.income { color: var(--success); font-weight: 700; }
    .amount.expense { color: var(--danger); font-weight: 700; }

    .empty { color: var(--muted); text-align: center; padding: 24px; border: 1px dashed rgba(255,255,255,.1); border-radius: 12px; }

    footer { margin-top: 22px; color: var(--muted); font-size: .85rem; text-align: center; }
    a.link { color: var(--primary); text-decoration: none; }
  </style>
</head>
<body>
  <div class="container">
    <header>
      <div class="title">
        <div class="logo" aria-hidden="true"></div>
        <div>
          <h1>Budget Planner</h1>
          <div class="subtitle">Track income, expenses, and balance — locally on your device.</div>
        </div>
      </div>
      <div class="tag" id="currencyTag">Currency: <span id="currencyLabel">KES</span></div>
    </header>

    <div class="grid grid-main">
      <!-- Left: Transactions -->
      <section class="card">
        <h2 style="margin: 0 0 10px 0; font-size:1.05rem">Add Transaction</h2>
        <form id="txForm" autocomplete="off">
          <div class="form-grid">
            <div class="field">
              <label for="type">Type</label>
              <select id="type" required>
                <option value="income">Income</option>
                <option value="expense">Expense</option>
              </select>
            </div>
            <div class="field">
              <label for="amount">Amount</label>
              <input id="amount" class="input" type="number" inputmode="decimal" step="0.01" min="0" placeholder="0.00" required />
            </div>
            <div class="field">
              <label for="description">Description</label>
              <input id="description" class="input" placeholder="e.g. Salary, Groceries" />
            </div>
            <div class="field">
              <label for="category">Category</label>
              <input id="category" class="input" placeholder="e.g. Food, Rent, Transport" />
            </div>
            <div class="field">
              <label for="date">Date</label>
              <input id="date" class="input" type="date" required />
            </div>
            <div class="field">
              <label for="currency">Currency</label>
              <input id="currency" class="input" maxlength="5" value="KES" />
            </div>
          </div>
          <div class="actions" style="margin-top:12px">
            <button class="btn" type="submit">Add</button>
            <button class="btn secondary" type="button" id="exportCsv">Export CSV</button>
            <button class="btn danger" type="button" id="clearAll">Clear All</button>
          </div>
        </form>
      </section>

      <!-- Right: Summary -->
      <aside class="card">
        <h2 style="margin: 0 0 10px 0; font-size:1.05rem">Overview</h2>
        <div class="summary">
          <div class="item">
            <div class="label">Total Income</div>
            <div class="value" id="incomeTotal">0</div>
          </div>
          <div class="item">
            <div class="label">Total Expenses</div>
            <div class="value" id="expenseTotal">0</div>
          </div>
          <div class="item">
            <div class="label">Balance</div>
            <div class="value" id="balance">0</div>
          </div>
        </div>
        <div style="margin-top:12px; display:flex; gap:10px; align-items:center; flex-wrap:wrap">
          <span class="label" style="color:var(--muted); font-size:.85rem">Filters</span>
          <select id="filterType">
            <option value="all">All</option>
            <option value="income">Income</option>
            <option value="expense">Expense</option>
          </select>
          <input id="search" class="input" placeholder="Search description or category" style="max-width: 260px;" />
          <div class="spacer"></div>
        </div>
      </aside>
    </div>

    <!-- Transactions List -->
    <section class="card" style="margin-top:16px">
      <div class="toolbar">
        <h2 style="margin: 0; font-size:1.05rem">Transactions</h2>
        <div class="spacer"></div>
        <select id="sortBy">
          <option value="date-desc">Newest first</option>
          <option value="date-asc">Oldest first</option>
          <option value="amount-desc">Amount high → low</option>
          <option value="amount-asc">Amount low → high</option>
        </select>
      </div>

      <div id="emptyState" class="empty" style="display:none">No transactions yet. Add your first one above ✨</div>

      <table id="txTable" aria-label="Transactions">
        <thead>
          <tr>
            <th>Date</th>
            <th>Description</th>
            <th>Category</th>
            <th>Type</th>
            <th style="text-align:right">Amount</th>
            <th></th>
          </tr>
        </thead>
        <tbody id="txBody"></tbody>
      </table>
    </section>

    <footer>
      Your data is stored locally on this device using LocalStorage. Change currency by editing the field in the form.
    </footer>
  </div>

  <script>
    // --- Utilities ---
    const $ = (sel) => document.querySelector(sel);
    const $$ = (sel) => document.querySelectorAll(sel);

    const STORAGE_KEY = 'bp_transactions_v1';
    const CURRENCY_KEY = 'bp_currency_v1';

    function formatAmount(value, currency = 'KES') {
      const amount = Number(value || 0);
      try {
        // Use browser locale for nice formatting
        return new Intl.NumberFormat(undefined, { style: 'currency', currency }).format(amount);
      } catch {
        // Fallback if unknown currency code
        return `${currency} ${amount.toFixed(2)}`;
      }
    }

    function todayISO() {
      const d = new Date();
      const tzOffset = d.getTimezoneOffset();
      // Adjust to local date reliably (strip time)
      const local = new Date(d.getTime() - tzOffset * 60 * 1000);
      return local.toISOString().slice(0, 10);
    }

    // --- State ---
    let transactions = [];
    let currency = localStorage.getItem(CURRENCY_KEY) || 'KES';

    // --- Init form defaults ---
    $('#date').value = todayISO();
    $('#currency').value = currency;
    $('#currencyLabel').textContent = currency;

    // --- Load saved transactions ---
    try {
      const raw = localStorage.getItem(STORAGE_KEY);
      transactions = raw ? JSON.parse(raw) : [];
    } catch (e) {
      console.warn('Failed to load saved data', e);
      transactions = [];
    }

    // --- Render functions ---
    function save() {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(transactions));
    }

    function setCurrency(newCur) {
      currency = (newCur || 'KES').toUpperCase().slice(0,5);
      localStorage.setItem(CURRENCY_KEY, currency);
      $('#currencyLabel').textContent = currency;
      updateSummary();
      renderTable();
    }

    function updateSummary() {
      const income = transactions.filter(t => t.type === 'income').reduce((s, t) => s + Number(t.amount), 0);
      const expense = transactions.filter(t => t.type === 'expense').reduce((s, t) => s + Number(t.amount), 0);
      const balance = income - expense;

      $('#incomeTotal').textContent = formatAmount(income, currency);
      $('#expenseTotal').textContent = formatAmount(expense, currency);
      const balEl = $('#balance');
      balEl.textContent = formatAmount(balance, currency);
      balEl.style.color = balance >= 0 ? 'var(--success)' : 'var(--danger)';
    }

    function getFilters() {
      return {
        type: $('#filterType').value,
        query: $('#search').value.trim().toLowerCase(),
        sort: $('#sortBy').value
      };
    }

    function applyFilters(list) {
      const { type, query } = getFilters();
      let out = [...list];
      if (type !== 'all') out = out.filter(t => t.type === type);
      if (query) {
        out = out.filter(t =>
          (t.description || '').toLowerCase().includes(query) ||
          (t.category || '').toLowerCase().includes(query)
        );
      }
      return out;
    }

    function applySort(list) {
      const { sort } = getFilters();
      const out = [...list];
      if (sort === 'date-desc') out.sort((a,b) => b.date.localeCompare(a.date));
      if (sort === 'date-asc') out.sort((a,b) => a.date.localeCompare(b.date));
      if (sort === 'amount-desc') out.sort((a,b) => Number(b.amount) - Number(a.amount));
      if (sort === 'amount-asc') out.sort((a,b) => Number(a.amount) - Number(b.amount));
      return out;
    }

    function renderTable() {
      const body = $('#txBody');
      body.innerHTML = '';
      let list = applyFilters(transactions);
      list = applySort(list);

      $('#emptyState').style.display = list.length ? 'none' : 'block';

      for (const t of list) {
        const tr = document.createElement('tr');
        tr.className = 'row';
        tr.innerHTML = `
          <td>${t.date}</td>
          <td>${escapeHtml(t.description || '')}</td>
          <td>${escapeHtml(t.category || '')}</td>
          <td><span class="tag">${t.type === 'income' ? 'Income' : 'Expense'}</span></td>
          <td style="text-align:right" class="amount ${t.type}">${formatAmount(t.amount, currency)}</td>
          <td style="text-align:right">
            <button class="btn secondary" data-edit="${t.id}">Edit</button>
            <button class="btn danger" data-del="${t.id}">Delete</button>
          </td>
        `;
        body.appendChild(tr);
      }
    }

    function escapeHtml(str) {
      return str.replace(/[&<>"]/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;'}[c]));
    }

    function addTransaction(t) {
      transactions.push(t);
      save();
      updateSummary();
      renderTable();
    }

    function deleteTransaction(id) {
      const idx = transactions.findIndex(t => t.id === id);
      if (idx !== -1) {
        transactions.splice(idx, 1);
        save();
        updateSummary();
        renderTable();
      }
    }

    function editTransaction(id) {
      const t = transactions.find(x => x.id === id);
      if (!t) return;
      // Prefill form for quick edit
      $('#type').value = t.type;
      $('#amount').value = t.amount;
      $('#description').value = t.description || '';
      $('#category').value = t.category || '';
      $('#date').value = t.date;
      // Switch Add button to Save
      const submitBtn = $('#txForm button[type="submit"]');
      submitBtn.textContent = 'Save Changes';
      submitBtn.dataset.editing = id;
      submitBtn.classList.add('secondary');
    }

    function resetFormButton() {
      const submitBtn = $('#txForm button[type="submit"]');
      submitBtn.textContent = 'Add';
      submitBtn.dataset.editing = '';
      submitBtn.classList.remove('secondary');
    }

    // --- Event listeners ---
    $('#txForm').addEventListener('submit', (e) => {
      e.preventDefault();
      const type = $('#type').value;
      const amount = parseFloat($('#amount').value);
      const description = $('#description').value.trim();
      const category = $('#category').value.trim();
      const date = $('#date').value || todayISO();
      const cur = $('#currency').value.trim();

      if (!isFinite(amount) || amount < 0) {
        alert('Please enter a valid amount.');
        return;
      }

      setCurrency(cur);

      const submitBtn = e.submitter || $('#txForm button[type="submit"]');
      const editingId = submitBtn.dataset.editing;

      if (editingId) {
        // Update existing
        const t = transactions.find(x => x.id === editingId);
        if (t) {
          Object.assign(t, { type, amount, description, category, date });
        }
        save();
        resetFormButton();
      } else {
        const tx = { id: crypto.randomUUID(), type, amount, description, category, date, createdAt: Date.now() };
        addTransaction(tx);
      }

      // Reset form (keep date & currency for faster input)
      $('#amount').value = '';
      $('#description').value = '';
      $('#category').value = '';
      $('#type').value = 'expense';
      $('#amount').focus();

      updateSummary();
      renderTable();
    });

    $('#currency').addEventListener('change', (e) => setCurrency(e.target.value));
    $('#filterType').addEventListener('change', renderTable);
    $('#search').addEventListener('input', renderTable);
    $('#sortBy').addEventListener('change', renderTable);

    $('#clearAll').addEventListener('click', () => {
      if (confirm('This will delete all transactions on this device. Continue?')) {
        transactions = [];
        save();
        updateSummary();
        renderTable();
      }
    });

    $('#exportCsv').addEventListener('click', () => {
      if (!transactions.length) { alert('No data to export.'); return; }
      const headers = ['id','date','type','description','category','amount'];
      const rows = transactions.map(t => headers.map(h => (t[h] ?? '')));
      const csv = [headers.join(','), ...rows.map(r => r.map(csvEscape).join(','))].join('\n');
      const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = `budget_export_${new Date().toISOString().slice(0,10)}.csv`;
      document.body.appendChild(a);
      a.click();
      a.remove();
      URL.revokeObjectURL(url);
    });

    function csvEscape(val) {
      const s = String(val ?? '');
      if (s.includes(',') || s.includes('"') || s.includes('\n')) {
        return '"' + s.replace(/"/g, '""') + '"';
      }
      return s;
    }

    document.addEventListener('click', (e) => {
      const delBtn = e.target.closest('button[data-del]');
      if (delBtn) {
        deleteTransaction(delBtn.dataset.del);
        return;
      }
      const editBtn = e.target.closest('button[data-edit]');
      if (editBtn) {
        editTransaction(editBtn.dataset.edit);
        return;
      }
    });

    // --- Initial paint ---
    updateSummary();
    renderTable();
  </script>
</body>
</html>

