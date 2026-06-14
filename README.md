<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="default">
<meta name="apple-mobile-web-app-title" content="NutriTrack">
<title>NutriTrack</title>
<style>
:root{
  --bg:#F7F4ED;
  --card:#FFFFFF;
  --text:#2B2A27;
  --muted:#8C8779;
  --accent:#3E6B52;
  --accent-soft:#E2EEE7;
  --warn:#D9824A;
  --warn-soft:#FBEADE;
  --border:#E6E2D8;
  --protein:#3E6B52;
  --carbs:#D9824A;
  --fat:#7C7AA8;
  --radius:14px;
}
*{box-sizing:border-box;margin:0;padding:0;}
html,body{font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,sans-serif;}
body{
  background:var(--bg);
  color:var(--text);
  min-height:100vh;
  padding-bottom:78px;
}
.container{
  max-width:480px;
  margin:0 auto;
  padding:1.25rem 1rem 1rem;
}
h1{
  font-size:20px;
  font-weight:600;
  margin-bottom:2px;
}
.subtitle{
  font-size:13px;
  color:var(--muted);
  margin-bottom:1.25rem;
}
.view{display:none;}
.view.active{display:block;}

/* Cards */
.card{
  background:var(--card);
  border:1px solid var(--border);
  border-radius:var(--radius);
  padding:1rem 1.1rem;
  margin-bottom:0.9rem;
}

/* Ring + macros */
.summary-card{
  display:flex;
  align-items:center;
  gap:1.25rem;
}
.ring-wrap{position:relative; flex-shrink:0;}
.ring-text{
  position:absolute;
  top:0; left:0; right:0; bottom:0;
  display:flex; flex-direction:column; align-items:center; justify-content:center;
}
.ring-text .num{font-size:20px; font-weight:600;}
.ring-text .lbl{font-size:11px; color:var(--muted);}
.macros{flex:1; display:flex; flex-direction:column; gap:10px;}
.macro-row{display:flex; flex-direction:column; gap:4px;}
.macro-row .top{display:flex; justify-content:space-between; font-size:12px; color:var(--muted);}
.bar-bg{height:6px; border-radius:3px; background:var(--bg); border:1px solid var(--border); overflow:hidden;}
.bar-fill{height:100%; border-radius:3px;}
.bar-protein{background:var(--protein);}
.bar-carbs{background:var(--carbs);}
.bar-fat{background:var(--fat);}

/* Header row with settings */
.section-header{
  display:flex; justify-content:space-between; align-items:center; margin-bottom:0.6rem;
}
.icon-btn{
  background:none; border:1px solid var(--border); border-radius:10px;
  width:34px; height:34px; display:flex; align-items:center; justify-content:center;
  color:var(--muted); cursor:pointer;
}
.icon-btn:active{transform:scale(0.96);}

/* Goals form */
#goals-form{display:none;}
#goals-form.open{display:block;}
.goals-grid{display:grid; grid-template-columns:1fr 1fr; gap:10px; margin-bottom:10px;}
.field label{font-size:11px; color:var(--muted); display:block; margin-bottom:3px;}
input[type=number], input[type=text]{
  width:100%; border:1px solid var(--border); border-radius:10px;
  padding:9px 10px; font-size:14px; background:var(--bg); color:var(--text);
}
input[type=number]:focus, input[type=text]:focus{outline:2px solid var(--accent-soft); border-color:var(--accent);}

/* Buttons */
.btn{
  display:inline-flex; align-items:center; justify-content:center; gap:6px;
  border:1px solid var(--accent); background:var(--accent); color:#fff;
  border-radius:10px; padding:10px 16px; font-size:14px; font-weight:500;
  cursor:pointer; width:100%;
}
.btn:active{transform:scale(0.98);}
.btn-secondary{
  background:transparent; color:var(--accent); border:1px solid var(--accent);
}

/* Meal list */
.meal-item{
  display:flex; justify-content:space-between; align-items:center;
  padding:10px 0; border-bottom:1px solid var(--border);
}
.meal-item:last-child{border-bottom:none;}
.meal-info .name{font-size:14px; font-weight:500;}
.meal-info .detail{font-size:12px; color:var(--muted); margin-top:2px;}
.meal-kcal{font-size:14px; font-weight:600; margin-right:10px; white-space:nowrap;}
.delete-btn{
  background:none; border:none; color:var(--muted); cursor:pointer; padding:4px;
  display:flex; align-items:center;
}
.empty-msg{font-size:13px; color:var(--muted); text-align:center; padding:1.2rem 0;}

/* Search */
.search-row{display:flex; gap:8px; margin-bottom:1rem;}
.search-row input{flex:1;}
.search-row .btn{width:auto; padding:10px 14px;}
.result-card{
  display:flex; align-items:center; gap:10px;
}
.result-info{flex:1; min-width:0;}
.result-info .name{font-size:14px; font-weight:500; overflow:hidden; text-overflow:ellipsis; white-space:nowrap;}
.result-info .detail{font-size:12px; color:var(--muted); margin-top:2px;}
.qty-input{width:64px; text-align:center;}
.add-btn{
  border:1px solid var(--accent); background:var(--accent-soft); color:var(--accent);
  border-radius:10px; padding:9px 12px; font-size:13px; font-weight:500; cursor:pointer; white-space:nowrap;
}

/* Scanner */
#scanner-view{
  width:100%; border-radius:var(--radius); overflow:hidden; margin-bottom:0.9rem;
  background:#111; min-height:0;
}
.status-text{font-size:13px; color:var(--muted); text-align:center; padding:0.5rem 0;}
.error-text{font-size:13px; color:var(--warn); text-align:center; padding:0.5rem 0;}

/* History */
.history-card{display:flex; justify-content:space-between; align-items:center;}
.history-date{font-size:14px; font-weight:500; text-transform:capitalize;}
.history-detail{font-size:12px; color:var(--muted); margin-top:2px;}
.history-kcal{font-size:15px; font-weight:600; text-align:right;}
.history-kcal .goal{font-size:11px; color:var(--muted); font-weight:400;}

/* Menu types */
.menu-card-header{display:flex; justify-content:space-between; align-items:flex-start; margin-bottom:6px; gap:8px;}
.menu-name{font-size:15px; font-weight:600;}
.menu-kcal{font-size:13px; color:var(--muted); white-space:nowrap;}
.menu-items-list{font-size:12px; color:var(--muted); margin:0 0 10px;}
.menu-actions{display:flex; gap:8px;}
.menu-actions .btn{flex:1;}
.builder-items{margin:10px 0;}
.builder-item{display:flex; justify-content:space-between; align-items:center; padding:6px 0; border-bottom:1px solid var(--border); font-size:13px;}
.builder-item:last-child{border-bottom:none;}

/* Bottom nav */
nav{
  position:fixed; bottom:0; left:0; right:0;
  background:var(--card); border-top:1px solid var(--border);
  display:flex; justify-content:center;
  padding-bottom:env(safe-area-inset-bottom);
}
.nav-inner{
  max-width:480px; width:100%; display:flex;
}
.nav-btn{
  flex:1; display:flex; flex-direction:column; align-items:center; gap:3px;
  padding:10px 0 8px; background:none; border:none; color:var(--muted); cursor:pointer; font-size:10px;
  white-space:nowrap;
}
.nav-btn.active{color:var(--accent);}
.nav-btn svg{width:20px; height:20px;}
</style>
</head>
<body>
<div class="container">

  <!-- JOURNAL VIEW -->
  <section class="view active" id="view-journal">
    <h1>Aujourd'hui</h1>
    <p class="subtitle" id="today-date"></p>

    <div class="card summary-card">
      <div class="ring-wrap">
        <svg width="120" height="120" viewBox="0 0 120 120">
          <circle cx="60" cy="60" r="52" fill="none" stroke="var(--border)" stroke-width="10"/>
          <circle id="cal-ring" cx="60" cy="60" r="52" fill="none" stroke="var(--accent)" stroke-width="10"
            stroke-linecap="round" transform="rotate(-90 60 60)" />
        </svg>
        <div class="ring-text">
          <span class="num" id="cal-num">0</span>
          <span class="lbl" id="cal-goal-lbl">/ 2000 kcal</span>
        </div>
      </div>
      <div class="macros">
        <div class="macro-row">
          <div class="top"><span>Protéines</span><span id="protein-lbl">0 / 0 g</span></div>
          <div class="bar-bg"><div class="bar-fill bar-protein" id="protein-bar"></div></div>
        </div>
        <div class="macro-row">
          <div class="top"><span>Glucides</span><span id="carbs-lbl">0 / 0 g</span></div>
          <div class="bar-bg"><div class="bar-fill bar-carbs" id="carbs-bar"></div></div>
        </div>
        <div class="macro-row">
          <div class="top"><span>Lipides</span><span id="fat-lbl">0 / 0 g</span></div>
          <div class="bar-bg"><div class="bar-fill bar-fat" id="fat-bar"></div></div>
        </div>
      </div>
    </div>

    <div class="section-header">
      <span style="font-size:14px; font-weight:500;">Repas du jour</span>
      <button class="icon-btn" id="goals-toggle" aria-label="Objectifs">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <circle cx="12" cy="12" r="3"/>
          <path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 1 1-2.83 2.83l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-4 0v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 1 1-2.83-2.83l.06-.06A1.65 1.65 0 0 0 4.6 15a1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1 0-4h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06A2 2 0 1 1 7.04 4.29l.06.06A1.65 1.65 0 0 0 8.92 4.6a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 4 0v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 1 1 2.83 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 0 4h-.09a1.65 1.65 0 0 0-1.51 1z"/>
        </svg>
      </button>
    </div>

    <div class="card" id="goals-form">
      <div class="goals-grid">
        <div class="field"><label>Calories (kcal)</label><input type="number" id="goal-cal" min="0"></div>
        <div class="field"><label>Protéines (g)</label><input type="number" id="goal-protein" min="0"></div>
        <div class="field"><label>Glucides (g)</label><input type="number" id="goal-carbs" min="0"></div>
        <div class="field"><label>Lipides (g)</label><input type="number" id="goal-fat" min="0"></div>
      </div>
      <button class="btn" id="save-goals-btn">Enregistrer les objectifs</button>
    </div>

    <div class="card" id="meal-list">
      <p class="empty-msg">Aucun repas ajouté pour le moment.</p>
    </div>
  </section>

  <!-- SEARCH VIEW -->
  <section class="view" id="view-search">
    <h1>Rechercher un aliment</h1>
    <p class="subtitle">Base de données Open Food Facts</p>
    <div class="search-row">
      <input type="text" id="search-input" placeholder="Ex: yaourt nature, pomme...">
      <button class="btn" id="search-btn">Chercher</button>
    </div>
    <div id="search-results"></div>
  </section>

  <!-- SCANNER VIEW -->
  <section class="view" id="view-scanner">
    <h1>Scanner un produit</h1>
    <p class="subtitle">Pointe la caméra vers le code-barres</p>
    <div id="scanner-view"></div>
    <button class="btn" id="start-scan-btn">Démarrer le scanner</button>
    <p class="status-text" id="scan-status"></p>
    <div id="scan-result"></div>
  </section>

  <!-- HISTORY VIEW -->
  <section class="view" id="view-historique">
    <h1>Historique</h1>
    <p class="subtitle">Tes apports des jours précédents</p>
    <div id="history-list"></div>
  </section>

  <!-- MENU TYPES VIEW -->
  <section class="view" id="view-menus">
    <h1>Menus types</h1>
    <p class="subtitle">Enregistre des repas pour les ajouter en un clic</p>
    <button class="btn" id="new-menu-btn" style="margin-bottom:0.9rem;">+ Créer un menu type</button>

    <div class="card" id="menu-builder" style="display:none;">
      <div class="field" style="margin-bottom:10px;">
        <label>Nom du menu</label>
        <input type="text" id="menu-name-input" placeholder="Ex: Petit-déjeuner habituel">
      </div>
      <div class="search-row">
        <input type="text" id="menu-search-input" placeholder="Ajouter un aliment...">
        <button class="btn" id="menu-search-btn">Chercher</button>
      </div>
      <div id="menu-search-results"></div>
      <div class="builder-items" id="builder-items"></div>
      <p class="status-text" id="builder-totals"></p>
      <button class="btn" id="save-menu-btn">Enregistrer le menu</button>
    </div>

    <div id="menu-list"></div>
  </section>

</div>

<nav>
  <div class="nav-inner">
    <button class="nav-btn active" data-view="journal">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 11l9-8 9 8"/><path d="M5 10v9a1 1 0 0 0 1 1h4v-6h4v6h4a1 1 0 0 0 1-1v-9"/></svg>
      Journal
    </button>
    <button class="nav-btn" data-view="search">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="7"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>
      Rechercher
    </button>
    <button class="nav-btn" data-view="scanner">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="4" width="18" height="16" rx="2"/><line x1="7" y1="7" x2="7" y2="17"/><line x1="10" y1="7" x2="10" y2="17"/><line x1="13" y1="7" x2="13" y2="17"/><line x1="17" y1="7" x2="17" y2="17"/></svg>
      Scanner
    </button>
    <button class="nav-btn" data-view="historique">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>
      Historique
    </button>
    <button class="nav-btn" data-view="menus">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M19 21l-7-5-7 5V5a2 2 0 0 1 2-2h10a2 2 0 0 1 2 2z"/></svg>
      Menus
    </button>
  </div>
</nav>

<script src="https://cdn.jsdelivr.net/npm/html5-qrcode@2.3.8/html5-qrcode.min.js"></script>
<script>
const round1 = n => Math.round((n||0)*10)/10;
const todayKey = () => 'meals:' + new Date().toISOString().slice(0,10);
const RING_R = 52;
const RING_C = 2 * Math.PI * RING_R;

let state = {
  goals: { calories: 2000, protein: 90, carbs: 250, fat: 65 },
  meals: []
};
let searchResults = [];
let html5QrCode = null;

async function loadGoals(){
  try{
    const r = await window.storage.get('goals');
    if(r && r.value) state.goals = JSON.parse(r.value);
  }catch(e){}
}
async function saveGoals(){
  try{ await window.storage.set('goals', JSON.stringify(state.goals)); }catch(e){}
}
async function loadMeals(){
  try{
    const r = await window.storage.get(todayKey());
    state.meals = (r && r.value) ? JSON.parse(r.value) : [];
  }catch(e){ state.meals = []; }
}
async function saveMeals(){
  try{ await window.storage.set(todayKey(), JSON.stringify(state.meals)); }catch(e){}
}
async function loadMenuTypes(){
  try{
    const r = await window.storage.get('menutypes');
    return (r && r.value) ? JSON.parse(r.value) : [];
  }catch(e){ return []; }
}
async function saveMenuTypes(menus){
  try{ await window.storage.set('menutypes', JSON.stringify(menus)); }catch(e){}
}

function computeTotals(){
  return state.meals.reduce((acc,m)=>({
    kcal: acc.kcal + m.kcal,
    protein: acc.protein + m.protein,
    carbs: acc.carbs + m.carbs,
    fat: acc.fat + m.fat
  }), {kcal:0, protein:0, carbs:0, fat:0});
}

function renderJournal(){
  const dateEl = document.getElementById('today-date');
  dateEl.textContent = new Date().toLocaleDateString('fr-FR', {weekday:'long', day:'numeric', month:'long'});

  const totals = computeTotals();
  const g = state.goals;

  document.getElementById('cal-num').textContent = Math.round(totals.kcal);
  document.getElementById('cal-goal-lbl').textContent = `/ ${g.calories} kcal`;
  const ratio = g.calories > 0 ? Math.min(totals.kcal / g.calories, 1) : 0;
  const ring = document.getElementById('cal-ring');
  ring.setAttribute('stroke-dasharray', RING_C.toFixed(2));
  ring.setAttribute('stroke-dashoffset', (RING_C * (1 - ratio)).toFixed(2));

  setMacro('protein', totals.protein, g.protein);
  setMacro('carbs', totals.carbs, g.carbs);
  setMacro('fat', totals.fat, g.fat);

  document.getElementById('goal-cal').value = g.calories;
  document.getElementById('goal-protein').value = g.protein;
  document.getElementById('goal-carbs').value = g.carbs;
  document.getElementById('goal-fat').value = g.fat;

  renderMealList();
}

function setMacro(key, value, goal){
  document.getElementById(`${key}-lbl`).textContent = `${round1(value)} / ${goal} g`;
  const pct = goal > 0 ? Math.min((value/goal)*100, 100) : 0;
  document.getElementById(`${key}-bar`).style.width = pct + '%';
}

function renderMealList(){
  const list = document.getElementById('meal-list');
  if(state.meals.length === 0){
    list.innerHTML = '<p class="empty-msg">Aucun repas ajouté pour le moment.</p>';
    return;
  }
  list.innerHTML = state.meals.map(m => `
    <div class="meal-item">
      <div class="meal-info">
        <p class="name">${escapeHtml(m.name)}</p>
        <p class="detail">${m.qty} g · P ${round1(m.protein)}g · G ${round1(m.carbs)}g · L ${round1(m.fat)}g</p>
      </div>
      <span class="meal-kcal">${Math.round(m.kcal)} kcal</span>
      <button class="delete-btn" data-id="${m.id}" aria-label="Supprimer">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 6h18"/><path d="M8 6V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/><path d="M19 6l-1 14a2 2 0 0 1-2 2H8a2 2 0 0 1-2-2L5 6"/></svg>
      </button>
    </div>
  `).join('');
  list.querySelectorAll('.delete-btn').forEach(btn=>{
    btn.addEventListener('click', () => deleteMeal(parseFloat(btn.dataset.id)));
  });
}

function escapeHtml(str){
  const div = document.createElement('div');
  div.textContent = str || '';
  return div.innerHTML;
}

async function deleteMeal(id){
  state.meals = state.meals.filter(m => m.id !== id);
  await saveMeals();
  renderJournal();
}

function addProductToJournal(product, qty){
  const factor = qty / 100;
  const n = product.nutriments || {};
  const meal = {
    id: Date.now() + Math.random(),
    name: product.product_name || 'Produit sans nom',
    qty: qty,
    kcal: (n['energy-kcal_100g'] || 0) * factor,
    protein: (n.proteins_100g || 0) * factor,
    carbs: (n.carbohydrates_100g || 0) * factor,
    fat: (n.fat_100g || 0) * factor
  };
  state.meals.push(meal);
  saveMeals();
  renderJournal();
}

/* ---- Navigation ---- */
document.querySelectorAll('.nav-btn').forEach(btn=>{
  btn.addEventListener('click', () => {
    document.querySelectorAll('.nav-btn').forEach(b=>b.classList.remove('active'));
    document.querySelectorAll('.view').forEach(v=>v.classList.remove('active'));
    btn.classList.add('active');
    document.getElementById('view-' + btn.dataset.view).classList.add('active');
    if(btn.dataset.view !== 'scanner' && html5QrCode){
      stopScanner();
    }
    if(btn.dataset.view === 'historique') renderHistorique();
    if(btn.dataset.view === 'menus') renderMenuList();
  });
});

/* ---- Goals ---- */
document.getElementById('goals-toggle').addEventListener('click', () => {
  document.getElementById('goals-form').classList.toggle('open');
});
document.getElementById('save-goals-btn').addEventListener('click', async () => {
  state.goals = {
    calories: parseFloat(document.getElementById('goal-cal').value) || 0,
    protein: parseFloat(document.getElementById('goal-protein').value) || 0,
    carbs: parseFloat(document.getElementById('goal-carbs').value) || 0,
    fat: parseFloat(document.getElementById('goal-fat').value) || 0
  };
  await saveGoals();
  document.getElementById('goals-form').classList.remove('open');
  renderJournal();
});

/* ---- Search ---- */
document.getElementById('search-btn').addEventListener('click', doSearch);
document.getElementById('search-input').addEventListener('keydown', (e) => {
  if(e.key === 'Enter') doSearch();
});

async function doSearch(){
  const query = document.getElementById('search-input').value.trim();
  const results = document.getElementById('search-results');
  if(!query){ return; }
  results.innerHTML = '<p class="status-text">Recherche en cours...</p>';
  try{
    const url = `https://world.openfoodfacts.org/cgi/search.pl?search_terms=${encodeURIComponent(query)}&search_simple=1&action=process&json=1&page_size=15&fields=product_name,brands,nutriments,code`;
    const res = await fetch(url);
    const data = await res.json();
    searchResults = (data.products || []).filter(p => p.product_name && p.nutriments);
    if(searchResults.length === 0){
      results.innerHTML = '<p class="empty-msg">Aucun résultat trouvé.</p>';
      return;
    }
    results.innerHTML = searchResults.map((p,i) => productCardHtml(p, i)).join('');
    attachAddListeners(results, searchResults);
  }catch(e){
    results.innerHTML = '<p class="error-text">Impossible de contacter Open Food Facts. Vérifie ta connexion.</p>';
  }
}

function productCardHtml(p, index){
  const kcal = Math.round(p.nutriments['energy-kcal_100g'] || 0);
  const brand = p.brands ? p.brands.split(',')[0] : '';
  return `
    <div class="card result-card">
      <div class="result-info">
        <p class="name">${escapeHtml(p.product_name)}</p>
        <p class="detail">${escapeHtml(brand)}${brand ? ' · ' : ''}${kcal} kcal / 100g</p>
      </div>
      <input type="number" class="qty-input" value="100" min="1" data-index="${index}">
      <button class="add-btn" data-index="${index}">Ajouter</button>
    </div>
  `;
}

function attachAddListeners(container, productsArray){
  container.querySelectorAll('.add-btn').forEach(btn=>{
    btn.addEventListener('click', () => {
      const idx = parseInt(btn.dataset.index);
      const qtyInput = container.querySelector(`.qty-input[data-index="${idx}"]`);
      const qty = parseFloat(qtyInput.value) || 100;
      addProductToJournal(productsArray[idx], qty);
      btn.textContent = 'Ajouté ✓';
      btn.disabled = true;
    });
  });
}

/* ---- Scanner ---- */
document.getElementById('start-scan-btn').addEventListener('click', startScanner);

function startScanner(){
  const status = document.getElementById('scan-status');
  const resultDiv = document.getElementById('scan-result');
  resultDiv.innerHTML = '';
  status.textContent = 'Demande d\\'accès à la caméra...';
  status.classList.remove('error-text');

  html5QrCode = new Html5Qrcode('scanner-view');
  const config = {
    fps: 10,
    qrbox: { width: 250, height: 150 },
    formatsToSupport: [
      Html5QrcodeSupportedFormats.EAN_13,
      Html5QrcodeSupportedFormats.EAN_8,
      Html5QrcodeSupportedFormats.UPC_A,
      Html5QrcodeSupportedFormats.UPC_E
    ]
  };
  html5QrCode.start({ facingMode: 'environment' }, config, onScanSuccess, () => {})
    .then(() => { status.textContent = 'Cadre le code-barres dans la zone.'; })
    .catch(err => {
      status.textContent = 'Caméra indisponible ici. Cette fonction nécessite un hébergement HTTPS avec accès caméra autorisé.';
      status.classList.add('error-text');
    });
}

function stopScanner(){
  if(html5QrCode){
    html5QrCode.stop().then(() => html5QrCode.clear()).catch(()=>{});
    html5QrCode = null;
  }
}

async function onScanSuccess(decodedText){
  stopScanner();
  const status = document.getElementById('scan-status');
  const resultDiv = document.getElementById('scan-result');
  status.textContent = 'Code détecté : ' + decodedText + ' — recherche du produit...';
  try{
    const url = `https://world.openfoodfacts.org/api/v2/product/${decodedText}.json?fields=product_name,brands,nutriments,code`;
    const res = await fetch(url);
    const data = await res.json();
    if(data.status === 1 && data.product && data.product.nutriments){
      status.textContent = 'Produit trouvé :';
      resultDiv.innerHTML = productCardHtml(data.product, 0);
      attachAddListeners(resultDiv, [data.product]);
    }else{
      status.textContent = 'Produit non trouvé dans Open Food Facts pour ce code-barres.';
      status.classList.add('error-text');
    }
  }catch(e){
    status.textContent = 'Erreur lors de la recherche du produit.';
    status.classList.add('error-text');
  }
}

/* ---- History ---- */
async function renderHistorique(){
  const container = document.getElementById('history-list');
  container.innerHTML = '<p class="status-text">Chargement...</p>';
  let keys = [];
  try{
    const r = await window.storage.list('meals:');
    keys = (r && r.keys) ? r.keys : [];
  }catch(e){}
  if(keys.length === 0){
    container.innerHTML = '<p class="empty-msg">Pas encore d\'historique.</p>';
    return;
  }
  const todayStr = new Date().toISOString().slice(0,10);
  const dates = keys.map(k => k.replace('meals:','')).sort().reverse();
  const rows = [];
  for(const date of dates){
    try{
      const r = await window.storage.get('meals:' + date);
      const meals = (r && r.value) ? JSON.parse(r.value) : [];
      if(meals.length === 0) continue;
      const totals = meals.reduce((acc,m)=>({kcal:acc.kcal+m.kcal, protein:acc.protein+m.protein, carbs:acc.carbs+m.carbs, fat:acc.fat+m.fat}), {kcal:0,protein:0,carbs:0,fat:0});
      const label = (date === todayStr) ? "Aujourd'hui" : new Date(date + 'T12:00:00').toLocaleDateString('fr-FR', {weekday:'long', day:'numeric', month:'long'});
      rows.push(`
        <div class="card history-card">
          <div>
            <p class="history-date">${label}</p>
            <p class="history-detail">P ${round1(totals.protein)}g · G ${round1(totals.carbs)}g · L ${round1(totals.fat)}g</p>
          </div>
          <div>
            <p class="history-kcal">${Math.round(totals.kcal)}</p>
            <p class="history-kcal goal">/ ${state.goals.calories} kcal</p>
          </div>
        </div>
      `);
    }catch(e){}
  }
  container.innerHTML = rows.join('') || '<p class="empty-msg">Pas encore d\'historique.</p>';
}

/* ---- Menu types ---- */
let builderItems = [];
let menuSearchResults = [];

document.getElementById('new-menu-btn').addEventListener('click', () => {
  const builder = document.getElementById('menu-builder');
  const open = builder.style.display === 'block';
  builder.style.display = open ? 'none' : 'block';
  if(!open){
    builderItems = [];
    document.getElementById('menu-name-input').value = '';
    document.getElementById('menu-search-input').value = '';
    document.getElementById('menu-search-results').innerHTML = '';
    renderBuilderItems();
  }
});

document.getElementById('menu-search-btn').addEventListener('click', doMenuSearch);
document.getElementById('menu-search-input').addEventListener('keydown', (e) => {
  if(e.key === 'Enter') doMenuSearch();
});

async function doMenuSearch(){
  const query = document.getElementById('menu-search-input').value.trim();
  const results = document.getElementById('menu-search-results');
  if(!query) return;
  results.innerHTML = '<p class="status-text">Recherche en cours...</p>';
  try{
    const url = `https://world.openfoodfacts.org/cgi/search.pl?search_terms=${encodeURIComponent(query)}&search_simple=1&action=process&json=1&page_size=10&fields=product_name,brands,nutriments,code`;
    const res = await fetch(url);
    const data = await res.json();
    menuSearchResults = (data.products || []).filter(p => p.product_name && p.nutriments);
    if(menuSearchResults.length === 0){
      results.innerHTML = '<p class="empty-msg">Aucun résultat.</p>';
      return;
    }
    results.innerHTML = menuSearchResults.map((p,i) => productCardHtml(p, i)).join('');
    results.querySelectorAll('.add-btn').forEach(btn=>{
      btn.addEventListener('click', () => {
        const idx = parseInt(btn.dataset.index);
        const qtyInput = results.querySelector(`.qty-input[data-index="${idx}"]`);
        const qty = parseFloat(qtyInput.value) || 100;
        addItemToBuilder(menuSearchResults[idx], qty);
        btn.textContent = 'Ajouté ✓';
        btn.disabled = true;
      });
    });
  }catch(e){
    results.innerHTML = '<p class="error-text">Impossible de contacter Open Food Facts.</p>';
  }
}

function addItemToBuilder(product, qty){
  const factor = qty / 100;
  const n = product.nutriments || {};
  builderItems.push({
    name: product.product_name || 'Produit',
    qty: qty,
    kcal: (n['energy-kcal_100g'] || 0) * factor,
    protein: (n.proteins_100g || 0) * factor,
    carbs: (n.carbohydrates_100g || 0) * factor,
    fat: (n.fat_100g || 0) * factor
  });
  renderBuilderItems();
}

function renderBuilderItems(){
  const container = document.getElementById('builder-items');
  if(builderItems.length === 0){
    container.innerHTML = '';
  }else{
    container.innerHTML = builderItems.map((it,i) => `
      <div class="builder-item">
        <span>${escapeHtml(it.name)} — ${it.qty} g (${Math.round(it.kcal)} kcal)</span>
        <button class="delete-btn" data-i="${i}" aria-label="Retirer">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 6L6 18M6 6l12 12"/></svg>
        </button>
      </div>
    `).join('');
    container.querySelectorAll('.delete-btn').forEach(btn=>{
      btn.addEventListener('click', () => {
        builderItems.splice(parseInt(btn.dataset.i), 1);
        renderBuilderItems();
      });
    });
  }
  const totals = builderItems.reduce((acc,m)=>({kcal:acc.kcal+m.kcal, protein:acc.protein+m.protein, carbs:acc.carbs+m.carbs, fat:acc.fat+m.fat}), {kcal:0,protein:0,carbs:0,fat:0});
  document.getElementById('builder-totals').textContent = builderItems.length
    ? `Total : ${Math.round(totals.kcal)} kcal · P ${round1(totals.protein)}g · G ${round1(totals.carbs)}g · L ${round1(totals.fat)}g`
    : '';
}

document.getElementById('save-menu-btn').addEventListener('click', async () => {
  const name = document.getElementById('menu-name-input').value.trim();
  if(!name || builderItems.length === 0){
    alert('Donne un nom au menu et ajoute au moins un aliment.');
    return;
  }
  const menus = await loadMenuTypes();
  menus.push({ id: Date.now() + Math.random(), name, items: builderItems.map(it => ({...it})) });
  await saveMenuTypes(menus);
  document.getElementById('menu-builder').style.display = 'none';
  renderMenuList();
});

async function renderMenuList(){
  const container = document.getElementById('menu-list');
  const menus = await loadMenuTypes();
  if(menus.length === 0){
    container.innerHTML = '<p class="empty-msg">Aucun menu type enregistré.</p>';
    return;
  }
  container.innerHTML = menus.map(menu => {
    const totals = menu.items.reduce((acc,m)=>({kcal:acc.kcal+m.kcal, protein:acc.protein+m.protein, carbs:acc.carbs+m.carbs, fat:acc.fat+m.fat}), {kcal:0,protein:0,carbs:0,fat:0});
    const itemsStr = menu.items.map(it => `${escapeHtml(it.name)} (${it.qty}g)`).join(', ');
    return `
      <div class="card">
        <div class="menu-card-header">
          <span class="menu-name">${escapeHtml(menu.name)}</span>
          <span class="menu-kcal">${Math.round(totals.kcal)} kcal</span>
        </div>
        <p class="menu-items-list">${itemsStr}</p>
        <div class="menu-actions">
          <button class="btn add-menu-btn" data-id="${menu.id}">Ajouter au journal</button>
          <button class="icon-btn" data-del="${menu.id}" aria-label="Supprimer le menu">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 6h18"/><path d="M8 6V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/><path d="M19 6l-1 14a2 2 0 0 1-2 2H8a2 2 0 0 1-2-2L5 6"/></svg>
          </button>
        </div>
      </div>
    `;
  }).join('');
  container.querySelectorAll('.add-menu-btn').forEach(btn=>{
    btn.addEventListener('click', async () => {
      const menus = await loadMenuTypes();
      const menu = menus.find(m => m.id === parseFloat(btn.dataset.id));
      if(!menu) return;
      menu.items.forEach(it => {
        state.meals.push({ id: Date.now() + Math.random(), ...it });
      });
      await saveMeals();
      renderJournal();
      const original = btn.textContent;
      btn.textContent = 'Ajouté ✓';
      setTimeout(() => { btn.textContent = original; }, 1500);
    });
  });
  container.querySelectorAll('[data-del]').forEach(btn=>{
    btn.addEventListener('click', async () => {
      let menus = await loadMenuTypes();
      menus = menus.filter(m => m.id !== parseFloat(btn.dataset.del));
      await saveMenuTypes(menus);
      renderMenuList();
    });
  });
}

/* ---- Init ---- */
(async function init(){
  await loadGoals();
  await loadMeals();
  renderJournal();
})();
</script>
</body>
</html>
