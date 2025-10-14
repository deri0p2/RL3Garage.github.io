index.html
<!doctype html>
<html lang="pt-BR">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover" />
<title>AutoManager — Painel RL3</title>

<!-- Estilos: tema escuro do site; PDF será branco/preto -->
<style>
  :root{
    --bg:#070607; --card:#0b0b0b; --text:#f4f6f8; --muted:#b7c1c9; --accent:#e11d23; /* vermelho RL3 */
    --glass: rgba(255,255,255,0.03); --shadow: 0 8px 30px rgba(2,8,23,0.6);
    --radius:12px; --gap:12px; --maxw:1100px;
  }
  *{box-sizing:border-box}
  body{margin:0;font-family:Inter,system-ui,Segoe UI,Roboto,Arial;background:var(--bg);color:var(--text);-webkit-font-smoothing:antialiased}
  .wrap{max-width:var(--maxw);margin:12px auto;padding:14px;display:flex;flex-direction:column;gap:12px}
  header{display:flex;align-items:center;gap:12px}
  .brand{display:flex;align-items:center;gap:12px}
  .logo{width:56px;height:56px;border-radius:8px;background:#000;display:flex;align-items:center;justify-content:center;overflow:hidden}
  .logo img{width:100%;height:100%;object-fit:contain}
  h1{font-size:18px;margin:0}
  .sub{color:var(--muted);font-size:13px}

  .top-actions{margin-left:auto;display:flex;gap:10px;align-items:center}
  .btn{padding:10px 12px;border-radius:10px;border:none;cursor:pointer;font-weight:700;background:linear-gradient(90deg,#111,#222);color:var(--text)}
  .btn.primary{background:linear-gradient(90deg,var(--accent),#c7171f);color:#fff}
  .btn.ghost{background:transparent;border:1px solid var(--glass);color:var(--muted);padding:8px 10px;border-radius:8px}
  .small{padding:8px 10px;font-size:13px}

  .layout{display:grid;grid-template-columns:340px 1fr;gap:14px;margin-top:6px}
  @media(max-width:920px){.layout{grid-template-columns:1fr}.left{order:2}}

  .panel{background:var(--card);padding:12px;border-radius:var(--radius);box-shadow:var(--shadow);border:1px solid var(--glass)}

  /* sidebar */
  .left .search{display:flex;gap:8px;margin-bottom:12px}
  input[type="text"], select, input[type="number"], input[type="date"]{width:100%;padding:10px;border-radius:10px;border:1px solid var(--glass);background:transparent;color:var(--text)}
  .sidebar-actions{display:flex;flex-direction:column;gap:8px;margin-bottom:10px}
  .car-list{max-height:62vh;overflow:auto;padding-right:6px}
  .car-item{display:flex;justify-content:space-between;align-items:center;gap:10px;padding:12px;border-radius:10px;border:1px solid transparent;cursor:pointer;margin-bottom:10px;background:linear-gradient(180deg, rgba(255,255,255,0.01), transparent)}
  .car-item:hover{transform:translateY(-4px)}
  .car-item .meta small{display:block;color:var(--muted);font-size:12px}
  .badge{padding:6px 10px;border-radius:999px;font-weight:800;font-size:12px;background:var(--glass)}

  /* main */
  .tabs{display:flex;gap:8px;flex-wrap:wrap;margin-bottom:12px}
  .tab{background:transparent;border:1px solid var(--glass);padding:8px 12px;border-radius:10px;cursor:pointer}
  .tab.active{background:linear-gradient(90deg,var(--accent),#c7171f);color:#fff;border:1px solid rgba(255,255,255,0.06)}

  .content{padding:16px;border-radius:12px;background:var(--card);box-shadow:var(--shadow);min-height:60vh}
  .grid-2{display:grid;grid-template-columns:1fr 320px;gap:14px}
  @media(max-width:920px){.grid-2{grid-template-columns:1fr}}

  .field{margin-bottom:12px}
  label{display:block;color:var(--muted);font-size:13px;margin-bottom:6px}
  .stat-row{display:flex;gap:12px;margin-bottom:12px}
  .stat{flex:1;padding:12px;border-radius:10px;background:linear-gradient(180deg,rgba(255,255,255,0.01),transparent)}
  .muted{color:var(--muted)}
  .exp-list{margin-top:10px;border-radius:8px;padding:8px;background:linear-gradient(180deg,rgba(255,255,255,0.01),transparent)}
  .exp-item{display:flex;justify-content:space-between;align-items:center;padding:8px;border-bottom:1px dashed var(--glass)}
  .exp-item:last-child{border-bottom:none}
  footer{margin-top:12px;color:var(--muted);font-size:13px;text-align:center}

  /* logo upload small */
  #logoInput{display:none}
  .logo-actions{display:flex;gap:8px;align-items:center}
  .logo-actions button{font-weight:600;padding:6px 8px;border-radius:8px;border:1px solid var(--glass);background:transparent;color:var(--muted)}
</style>
</head>
<body>
  <div class="wrap">
    <header>
      <div class="brand">
        <div class="logo panel" id="logoBox" title="Logo RL3">
          <!-- logo image goes here -->
          <img id="logoImg" src="" alt="RL3 logo" style="display:none">
          <div id="logoPlaceholder" style="display:flex;align-items:center;justify-content:center;width:56px;height:56px;color:var(--muted);font-weight:800">RL3</div>
        </div>
        <div>
          <h1>AutoManager — Controle RL3</h1>
          <div class="sub">Tema inspirado na logo • Salvo no seu celular</div>
        </div>
      </div>

      <div class="top-actions">
        <div class="logo-actions">
          <label for="logoInput" class="btn ghost small" title="Enviar logo">Trocar logo</label>
          <button id="removeLogoBtn" class="btn ghost small" title="Remover logo">Remover</button>
          <input type="file" id="logoInput" accept="image/*">
        </div>

        <button id="addCarBtn" class="btn primary" title="Novo carro">+ Novo</button>
        <button id="backupBtn" class="btn ghost small">Exportar JSON</button>
        <button id="importBtn" class="btn ghost small">Importar</button>
      </div>
    </header>

    <div class="layout">
      <!-- SIDEBAR -->
      <aside class="panel left">
        <div class="sidebar-actions">
          <input id="q" type="text" placeholder="Pesquisar veículo, placa ou marca..." />
          <div style="display:flex;gap:8px">
            <select id="filterBrand"><option value="">Todas marcas</option></select>
            <select id="filterStatus"><option value="all">Todos</option><option value="available">Disponíveis</option><option value="sold">Vendidos</option></select>
          </div>
          <div style="display:flex;gap:8px">
            <select id="sortBy"><option value="recent">Mais recentes</option><option value="brand">Marca</option><option value="totalDesc">Total (maior)</option></select>
            <button id="clearBtn" class="btn ghost small">Limpar</button>
          </div>
        </div>

        <div class="car-list panel" id="carList" aria-live="polite"></div>
      </aside>

      <!-- MAIN -->
      <main>
        <div class="tabs" id="tabs"></div>

        <section id="mainArea" class="content">
          <div style="text-align:center;color:var(--muted);padding:40px">
            <div style="font-size:18px;font-weight:800;margin-bottom:6px">Nenhum carro selecionado</div>
            <div class="muted">Use "+ Novo" ou escolha um veículo na lista</div>
          </div>
        </section>
      </main>
    </div>

    <footer>Duplo passo ao excluir — para evitar exclusões acidentais. Faça backup regular.</footer>
  </div>

  <input type="file" id="fileInput" accept="application/json" style="display:none">

  <!-- libs para gerar PDF via canvas (CDN). Se preferir, posso incorporar as libs localmente. -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<script>
/* ========== App JS ========== */
const STORAGE_KEY = 'auto_manager_rl3_v2';
let db = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]');
let activeId = null;

/* ---------- logo handling ---------- */
const logoInput = document.getElementById('logoInput');
const logoImg = document.getElementById('logoImg');
const logoPlaceholder = document.getElementById('logoPlaceholder');
const logoBox = document.getElementById('logoBox');
const removeLogoBtn = document.getElementById('removeLogoBtn');

function loadLogoFromStorage(){
  const data = localStorage.getItem('auto_manager_rl3_logo');
  if(data){
    logoImg.src = data;
    logoImg.style.display = 'block';
    logoPlaceholder.style.display = 'none';
  } else {
    logoImg.style.display = 'none';
    logoPlaceholder.style.display = 'flex';
  }
}
logoInput.addEventListener('change', ev=>{
  const f = ev.target.files[0]; if(!f) return;
  const reader = new FileReader();
  reader.onload = e=>{
    localStorage.setItem('auto_manager_rl3_logo', e.target.result);
    loadLogoFromStorage();
  };
  reader.readAsDataURL(f);
});
removeLogoBtn.addEventListener('click', ()=>{
  if(confirm('Remover logo do painel?')){ localStorage.removeItem('auto_manager_rl3_logo'); loadLogoFromStorage(); }
});
loadLogoFromStorage();

/* ---------- helpers ---------- */
const uid = ()=> 'c'+Date.now().toString(36)+Math.random().toString(36).slice(2,7);
const fmt = n => Number(n||0).toLocaleString('pt-BR',{minimumFractionDigits:2,maximumFractionDigits:2});
const qs = s => document.querySelector(s);
const qsa = s => Array.from(document.querySelectorAll(s));

function saveDB(){
  localStorage.setItem(STORAGE_KEY, JSON.stringify(db));
  buildBrandFilter();
  renderList();
  renderTabs();
  if(activeId) renderCar(activeId);
}

function calcTotals(car){
  const expSum = (car.expenses||[]).reduce((s,e)=>s+Number(e.value||0),0);
  const total = Number(car.cost||0) + expSum;
  const potential = Number(car.fipe||0);
  const profit = potential - total;
  return { expenses: expSum, total, potential, profit };
}

function escapeHtml(s){ if(!s) return ''; return String(s).replaceAll('&','&amp;').replaceAll('<','&lt;').replaceAll('>','&gt;').replaceAll('"','&quot;'); }

/* ---------- list & filters ---------- */
function buildBrandFilter(){
  const sel = qs('#filterBrand');
  const brands = Array.from(new Set(db.map(c=>c.brand||'').filter(Boolean))).sort();
  sel.innerHTML = '<option value="">Todas marcas</option>' + brands.map(b=>`<option value="${escapeHtml(b)}">${escapeHtml(b)}</option>`).join('');
}

function renderList(){
  const q = qs('#q').value.trim().toLowerCase();
  const status = qs('#filterStatus').value;
  const brandFilter = qs('#filterBrand').value;
  const sortBy = qs('#sortBy').value;
  let list = db.slice();
  if(status==='available') list = list.filter(c=>!c.sold);
  if(status==='sold') list = list.filter(c=>c.sold);
  if(brandFilter) list = list.filter(c=> (c.brand||'').toLowerCase() === brandFilter.toLowerCase());
  if(q) list = list.filter(c=> ((c.vehicle||'')+' '+(c.plate||'')+' '+(c.brand||'')).toLowerCase().includes(q));
  if(sortBy==='brand') list.sort((a,b)=>(a.brand||'').localeCompare(b.brand||''));
  if(sortBy==='totalDesc') list.sort((a,b)=>calcTotals(b).total - calcTotals(a).total);
  if(sortBy==='recent') list.sort((a,b)=>b.created - a.created);

  const el = qs('#carList'); el.innerHTML = '';
  if(list.length===0){ el.innerHTML = '<div class="muted">Nenhum carro</div>'; return; }

  list.forEach(c=>{
    const t = calcTotals(c);
    const card = document.createElement('div');
    card.className = 'car-item';
    card.dataset.id = c.id;
    card.innerHTML = `
      <div style="flex:1;display:flex;gap:10px;align-items:center">
        <div style="width:46px;height:46px;border-radius:8px;background:var(--glass);display:flex;align-items:center;justify-content:center;font-weight:800">${(c.brand||'').charAt(0) || 'C'}</div>
        <div class="meta"><strong>${escapeHtml(c.vehicle||'—')}</strong><small>${escapeHtml(c.brand||'')} • ${escapeHtml(c.plate||'')}</small></div>
      </div>
      <div style="text-align:right">
        <div style="font-weight:800">R$ ${fmt(t.total)}</div>
        <div class="badge" style="margin-top:6px">${c.sold? 'Vendido':'Disponível'}</div>
      </div>
    `;
    card.addEventListener('click', ()=> openTab(c.id));
    el.appendChild(card);
  });
}

/* ---------- tabs ---------- */
function renderTabs(){
  const tabs = qs('#tabs'); tabs.innerHTML = '';
  db.forEach(c=>{
    const t = document.createElement('div');
    t.className = 'tab' + (c.id===activeId? ' active':'');
    t.textContent = c.vehicle || 'Sem nome';
    t.dataset.id = c.id;
    t.addEventListener('click', ()=> openTab(c.id));
    tabs.appendChild(t);
  });
}

/* ---------- open car & render ---------- */
function openTab(id){
  activeId = id;
  renderTabs();
  renderCar(id);
  window.scrollTo({top:0,behavior:'smooth'});
}

function renderCar(id){
  const car = db.find(x=>x.id===id);
  const area = qs('#mainArea');
  if(!car){ area.innerHTML = '<div class="muted">Carro não encontrado</div>'; return; }
  const t = calcTotals(car);
  area.innerHTML = `
    <div class="stat-row">
      <div class="stat"><div class="muted">Total gastos</div><strong>R$ ${fmt(t.total)}</strong><div class="muted">Peças: R$ ${fmt(t.expenses)}</div></div>
      <div class="stat"><div class="muted">Valor FIPE</div><strong>R$ ${fmt(t.potential)}</strong><div class="muted">Lucro estimado: <strong>${t.profit>=0? 'R$ '+fmt(t.profit) : '- R$ '+fmt(Math.abs(t.profit))}</strong></div></div>
      <div class="stat"><div class="muted">Status</div><strong>${car.sold? 'Vendido': 'Disponível'}</strong><div class="muted">${car.sold? 'Vendido em ' + (car.soldDate ? new Date(car.soldDate).toLocaleDateString() : '') : ''}</div></div>
    </div>

    <div class="grid-2">
      <div>
        <div class="field"><label>Veículo</label><input id="vehicle_${car.id}" value="${escapeHtml(car.vehicle)}" /></div>
        <div class="field"><label>Placa</label><input id="plate_${car.id}" value="${escapeHtml(car.plate)}" /></div>
        <div class="field"><label>Marca</label><input id="brand_${car.id}" value="${escapeHtml(car.brand)}" /></div>
        <div class="field"><label>FIPE (R$)</label><input id="fipe_${car.id}" type="number" value="${Number(car.fipe)||0}" /></div>
        <div class="field"><label>Custo de compra (R$)</label><input id="cost_${car.id}" type="number" value="${Number(car.cost)||0}" /></div>

        <div style="margin-top:12px"><strong>Registrar peça / serviço</strong></div>
        <div style="display:flex;gap:8px;flex-wrap:wrap;margin-top:8px">
          <input id="expDesc_${car.id}" placeholder="Descrição (ex: óleo, pastilha...)" style="flex:2" />
          <input id="expValue_${car.id}" placeholder="Valor (R$)" type="number" style="width:100px" />
          <input id="expDate_${car.id}" type="date" style="width:150px" />
          <button id="addExp_${car.id}" class="btn primary">Adicionar</button>
        </div>

        <div id="expList_${car.id}" class="exp-list" aria-live="polite">${renderExpHTML(car)}</div>
      </div>

      <div>
        <div style="padding:12px;border-radius:10px;background:linear-gradient(180deg,rgba(255,255,255,0.02),transparent)">
          <div style="margin-bottom:8px"><strong>Resumo rápido</strong></div>
          <div class="muted">Custo compra</div><div style="font-size:20px;font-weight:800">R$ ${fmt(Number(car.cost)||0)}</div>
          <div class="muted" style="margin-top:8px">Total peças/serviços</div><div style="font-size:18px">R$ ${fmt(t.expenses)}</div>
          <hr style="margin:12px 0;border:none;border-top:1px dashed var(--glass)">
          <div class="muted">Total até agora</div><div style="font-size:22px;font-weight:900">R$ ${fmt(t.total)}</div>

          <div style="margin-top:12px;display:flex;gap:8px;flex-wrap:wrap">
            <button id="save_${car.id}" class="btn primary">Salvar</button>
            <button id="markSold_${car.id}" class="btn ghost">${car.sold? 'Desmarcar venda' : 'Marcar vendido'}</button>
            <button id="pdf_${car.id}" class="btn ghost small">Exportar PDF</button>
            <button id="delete_${car.id}" class="btn" style="background:transparent;border:1px solid var(--accent);color:var(--accent)">Excluir</button>
          </div>

          <div style="margin-top:10px" class="muted">Observação: para excluir pedimos confirmação dupla.</div>
        </div>
      </div>
    </div>
  `;

  // handlers
  qs(`#addExp_${car.id}`).addEventListener('click', ()=>{
    const desc = qs(`#expDesc_${car.id}`).value.trim();
    const value = Number(qs(`#expValue_${car.id}`).value || 0);
    const date = qs(`#expDate_${car.id}`).value || new Date().toISOString().slice(0,10);
    if(!desc || !value){ alert('Preencha descrição e valor.'); return; }
    car.expenses = car.expenses || [];
    car.expenses.push({id: uid(), desc, value, date});
    saveDB();
    qs(`#expDesc_${car.id}`).value=''; qs(`#expValue_${car.id}`).value=''; qs(`#expDate_${car.id}`).value='';
  });

  qsa(`#expList_${car.id} .exp-del`).forEach(btn=>{
    btn.addEventListener('click', ()=>{
      const eid = btn.dataset.eid;
      if(!confirm('Excluir esse gasto?')) return;
      car.expenses = (car.expenses||[]).filter(e=>e.id!==eid);
      saveDB();
    });
  });

  qs(`#save_${car.id}`).addEventListener('click', ()=>{
    const vehicle = qs(`#vehicle_${car.id}`).value.trim();
    const plate = qs(`#plate_${car.id}`).value.trim();
    const brand = qs(`#brand_${car.id}`).value.trim();
    const fipe = Number(qs(`#fipe_${car.id}`).value || 0);
    const cost = Number(qs(`#cost_${car.id}`).value || 0);
    Object.assign(car,{vehicle,plate,brand,fipe,cost});
    saveDB();
    alert('Salvo.');
  });

  qs(`#markSold_${car.id}`).addEventListener('click', ()=>{
    if(!car.sold){ if(!confirm('Marcar como vendido?')) return; car.sold=true; car.soldDate = new Date().toISOString(); }
    else{ if(!confirm('Desmarcar venda?')) return; car.sold=false; car.soldDate=null; }
    saveDB();
  });

  qs(`#delete_${car.id}`).addEventListener('click', ()=>{
    if(!confirm('Tem certeza que deseja excluir este carro?')) return;
    const ok = prompt('Para confirmar exclusão, digite: EXCLUIR');
    if(ok !== 'EXCLUIR'){ alert('Exclusão cancelada.'); return; }
    db = db.filter(x=>x.id !== car.id);
    activeId = null; saveDB();
    qs('#mainArea').innerHTML = '<div style="text-align:center;color:var(--muted);padding:40px"><div style="font-weight:800">Nenhum carro selecionado</div><div class="muted">Use "+ Novo" ou escolha um veículo</div></div>';
    alert('Carro excluído.');
  });

  qs(`#pdf_${car.id}`).addEventListener('click', ()=> generatePDFForCar(car));
}

/* ---------- render expenses HTML ---------- */
function renderExpHTML(car){
  if(!car.expenses || car.expenses.length===0) return '<div class="muted">Nenhum gasto registrado</div>';
  return car.expenses.map(e=>`
    <div class="exp-item">
      <div><strong>${escapeHtml(e.desc)}</strong><br><small class="muted">${escapeHtml(e.date)}</small></div>
      <div style="text-align:right"><div style="font-weight:700">R$ ${fmt(e.value)}</div><div style="margin-top:6px"><button class="btn small exp-del" data-eid="${e.id}">Excluir</button></div></div>
    </div>
  `).join('');
}

/* ---------- global UI setup ---------- */
function setupButtons(){
  qs('#addCarBtn').addEventListener('click', ()=>{
    const c = { id: uid(), vehicle:'Novo veículo', brand:'', plate:'', fipe:0, cost:0, expenses:[], sold:false, soldDate:null, created: Date.now() };
    db.unshift(c); saveDB(); openTab(c.id);
  });
  qs('#backupBtn').addEventListener('click', ()=>{
    const blob = new Blob([JSON.stringify(db,null,2)],{type:'application/json'});
    const url = URL.createObjectURL(blob); const a = document.createElement('a'); a.href = url; a.download = 'backup_auto_manager_'+(new Date().toISOString().slice(0,10))+'.json'; a.click(); URL.revokeObjectURL(url);
  });
  qs('#importBtn').addEventListener('click', ()=> qs('#fileInput').click());
  qs('#fileInput').addEventListener('change', ev=>{
    const f = ev.target.files[0]; if(!f) return; const reader = new FileReader();
    reader.onload = e=>{
      try{ const imported = JSON.parse(e.target.result); if(!Array.isArray(imported)) throw new Error('Formato inválido'); if(confirm('Importar adicionará os dados ao banco atual. Continuar?')){ db = imported.concat(db); saveDB(); alert('Importado.'); } } catch(err){ alert('Erro ao importar: '+err.message) }
    }; reader.readAsText(f); ev.target.value='';
  });
  qs('#clearBtn').addEventListener('click', ()=> { qs('#q').value=''; qs('#filterBrand').value=''; qs('#filterStatus').value='all'; qs('#sortBy').value='recent'; renderList(); });
  ['#q','#filterStatus','#filterBrand','#sortBy'].forEach(id=>{ qs(id).addEventListener('input', renderList); qs(id).addEventListener('change', renderList); });
}

/* ========== PDF GENERATION for a single car (A4 portrait) ========== */
/* Uses html2canvas and jsPDF (CDN loaded above) */
async function generatePDFForCar(car){
  // build printable element offscreen
  const logoData = localStorage.getItem('auto_manager_rl3_logo') || '';
  const t = calcTotals(car);
  // create container
  const wrapper = document.createElement('div');
  wrapper.style.width = '794px'; // approx A4 at 96dpi -> 794 x 1123 (we will scale)
  wrapper.style.padding = '28px';
  wrapper.style.background = '#ffffff';
  wrapper.style.color = '#000';
  wrapper.style.fontFamily = 'Arial, Helvetica, sans-serif';
  wrapper.innerHTML = `
    <div style="display:flex;align-items:center;gap:16px;border-bottom:2px solid #eee;padding-bottom:10px;margin-bottom:12px">
      <div style="width:86px;height:86px;display:flex;align-items:center;justify-content:center">
        ${ logoData ? `<img src="${logoData}" style="max-width:86px;max-height:86px;object-fit:contain" />` : `<div style="width:86px;height:86px;background:#000;color:#fff;display:flex;align-items:center;justify-content:center;font-weight:800">RL3</div>` }
      </div>
      <div>
        <div style="font-size:20px;font-weight:800">Painel de Vendas — Controle de Carros</div>
        <div style="color:#666;margin-top:6px">Ficha do veículo — Gerado em ${new Date().toLocaleDateString()}</div>
      </div>
    </div>

    <div style="display:flex;gap:18px;margin-bottom:12px">
      <div style="flex:1">
        <div style="font-size:13px;color:#333">Veículo</div>
        <div style="font-size:18px;font-weight:700;margin-bottom:8px">${escapeHtml(car.vehicle||'—')}</div>

        <div style="display:flex;gap:12px;flex-wrap:wrap">
          <div style="min-width:150px">
            <div style="font-size:12px;color:#333">Marca</div>
            <div style="font-weight:700">${escapeHtml(car.brand||'—')}</div>
          </div>
          <div style="min-width:120px">
            <div style="font-size:12px;color:#333">Placa</div>
            <div style="font-weight:700">${escapeHtml(car.plate||'—')}</div>
          </div>
          <div style="min-width:140px">
            <div style="font-size:12px;color:#333">FIPE (R$)</div>
            <div style="font-weight:700">R$ ${fmt(car.fipe||0)}</div>
          </div>
        </div>
      </div>

      <div style="width:220px;border-left:1px solid #eee;padding-left:18px">
        <div style="font-size:12px;color:#333">Custo compra</div>
        <div style="font-size:18px;font-weight:800;margin-bottom:6px">R$ ${fmt(car.cost||0)}</div>
        <div style="font-size:12px;color:#333">Total peças/serviços</div>
        <div style="font-size:16px;font-weight:700;margin-bottom:6px">R$ ${fmt(t.expenses)}</div>
        <div style="font-size:12px;color:#333">Total geral</div>
        <div style="font-size:18px;font-weight:900;color:#000">R$ ${fmt(t.total)}</div>
      </div>
    </div>

    <div style="margin-top:6px">
      <div style="font-weight:800;margin-bottom:8px">Peças / Serviços</div>
      <table style="width:100%;border-collapse:collapse;font-size:12px">
        <thead>
          <tr style="text-align:left;color:#333">
            <th style="padding:6px 8px;border-bottom:1px solid #eee">Descrição</th>
            <th style="padding:6px 8px;border-bottom:1px solid #eee;width:120px">Data</th>
            <th style="padding:6px 8px;border-bottom:1px solid #eee;width:120px;text-align:right">Valor (R$)</th>
          </tr>
        </thead>
        <tbody>
          ${ (car.expenses||[]).map(e=>`<tr><td style="padding:8px 8px;border-bottom:1px solid #fafafa">${escapeHtml(e.desc)}</td><td style="padding:8px 8px;border-bottom:1px solid #fafafa">${escapeHtml(e.date)}</td><td style="padding:8px 8px;border-bottom:1px solid #fafafa;text-align:right">R$ ${fmt(e.value)}</td></tr>`).join('') }
          ${ (car.expenses && car.expenses.length===0) ? `<tr><td colspan="3" style="padding:12px 8px;color:#777">Nenhum gasto registrado</td></tr>` : '' }
        </tbody>
      </table>
    </div>

    <div style="margin-top:18px;border-top:1px solid #eee;padding-top:12px;display:flex;justify-content:space-between;align-items:center">
      <div style="color:#444;font-size:13px">Observação: documento gerado pelo Painel RL3</div>
      <div style="font-weight:800;color:${'#'+(0xE11D23).toString(16).slice(1)}">Total: R$ ${fmt(t.total)}</div>
    </div>
  `;

  // append offscreen, render, then remove
  wrapper.style.position = 'fixed';
  wrapper.style.left = '-10000px';
  document.body.appendChild(wrapper);

  try{
    // html2canvas: render wrapper to canvas with good scale
    const scale = 2; // improve quality
    const canvas = await html2canvas(wrapper, {scale:scale, useCORS:true, backgroundColor:'#ffffff'});
    const imgData = canvas.toDataURL('image/jpeg', 0.95);

    // jsPDF: A4 size in pt (mm: 210x297) -> px mapping: we'll use jsPDF units 'pt' with internal conversion
    const { jsPDF } = window.jspdf;
    const pdf = new jsPDF({unit:'pt',format:'a4',orientation:'portrait'});

    // compute image dims to fit A4 with margins
    const pdfWidth = pdf.internal.pageSize.getWidth();
    const pdfHeight = pdf.internal.pageSize.getHeight();
    // draw image full width with proportional height
    const imgProps = { width: canvas.width/scale, height: canvas.height/scale };
    const ratio = Math.min(pdfWidth / imgProps.width, pdfHeight / imgProps.height);
    const imgW = imgProps.width * ratio;
    const imgH = imgProps.height * ratio;
    const marginX = (pdfWidth - imgW) / 2;
    const marginY = 40;

    pdf.addImage(imgData, 'JPEG', marginX, marginY, imgW, imgH, undefined, 'FAST');
    const filename = `ficha_carro_${(car.vehicle||'veiculo').replace(/\s+/g,'_')}_${(new Date().toISOString().slice(0,10))}.pdf`;
    pdf.save(filename);
  }catch(err){
    alert('Erro ao gerar PDF: ' + (err.message || err));
    console.error(err);
  } finally {
    wrapper.remove();
  }
}

/* ---------- init ---------- */
(function init(){
  // start: load brand logo (already called at top)
  loadLogoFromStorage();
  setupButtons();
  buildBrandFilter();
  renderList();
  renderTabs();
  if(db.length>0) openTab(db[0].id);
})();
</script>
</body>
</html>
