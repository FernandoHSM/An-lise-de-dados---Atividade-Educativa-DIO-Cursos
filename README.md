Olá! Bom te ter por aqui.

Esta é uma atividade para conclusão do bootCamp presente no curso.

Esse repositório é uma atividade educativa de Análise de Dados com IA.

A atividade foi dada por meio do curso: AI Reports com Excel, GPT Agents e Claude Code na plataforma da DIO:
[
https:/web.dio](https://web.dio.me/home)

A atividade teve a orientação de Felipe Aguiar

GitHub: https://github.com/felipeAguiarCode
Linkedin: https://web.dio.me/project/criando-agentes-de-criacao-de-dashboards-com-excel-e-claude-code/learning/www.linkedin.com/in/felipeaguiar-exe

-------------------

A atividade consistiu no uso de IA (ChatGPT, Claude) para automatizar respectivas tarefas presentes no processo de análise, compillance, filtros, entre outros, através de um script feito em python, sendo totalmente reutilizavel para qualquer outro tipo de banco de Dados desde que as regras no markDown sejam definidas.
Com a IA, foi possivel agilizar processos manuais, tornando o trabalho mais ágil e entregando um resultado visual elegante com a criação de um site (HTML) para 
a apresentação de uma DashBoard, capaz de filtrar a receita por Vendedor em diferentes Meses do Ano.

O arquivo MarkDown (.md) foi usado para definir as regras de Negócio do script para que a IA cumprisse todas as exigências de forma exata, sem alucinar.

Os dados no banco foram salvos em um JSON, sem nenhum tipo de dado sensível; é uma atividade totalmente voltada para educação.


  <!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Porsche — Painel de Vendas</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@500;600;700&family=Inter:wght@400;500;600&family=Roboto+Mono:wght@500;700&display=swap" rel="stylesheet">
<style>
  :root{
    --black: #0a0a0a;
    --panel: #131313;
    --panel-2: #1a1a1a;
    --line: #2b2b2b;
    --white: #f4f3ef;
    --grey: #8a8a86;
    --red: #d5001c;
    --gold: #c9a24b;
  }
  *{ box-sizing: border-box; }
  html,body{ margin:0; padding:0; }
  body{
    background: var(--black);
    color: var(--white);
    font-family: 'Inter', sans-serif;
    -webkit-font-smoothing: antialiased;
  }
  .display{
    font-family: 'Barlow Condensed', sans-serif;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.04em;
  }
  .mono{ font-family: 'Roboto Mono', monospace; }

  /* ---------- Header ---------- */
  header{
    padding: 28px clamp(16px, 4vw, 56px) 0;
  }
  .crest-row{
    display:flex; align-items:center; justify-content:space-between; gap: 16px; flex-wrap: wrap;
  }
  .brand{ display:flex; align-items:center; gap:14px; }
  .crest{ width:40px; height:40px; flex:none; }
  .brand-text .eyebrow{
    font-family:'Roboto Mono', monospace; font-size:11px; letter-spacing:0.28em;
    color: var(--grey); text-transform:uppercase;
  }
  .brand-text h1{
    margin: 2px 0 0; font-size: clamp(26px, 4vw, 38px); line-height: 1;
  }
  .brand-text h1 span{ color: var(--red); }
  .meta{
    font-family:'Roboto Mono', monospace; font-size:12px; color: var(--grey); text-align:right;
  }
  .stripe{
    height: 6px; margin-top: 22px;
    background: repeating-linear-gradient(
      45deg, var(--white) 0 10px, var(--black) 10px 20px
    );
    opacity: 0.9;
  }
  .stripe.thin{ height:3px; opacity:0.5; }

  /* ---------- Layout ---------- */
  main{
    padding: 28px clamp(16px, 4vw, 56px) 64px;
    max-width: 1360px; margin: 0 auto;
  }

  /* ---------- Filters ---------- */
  .filters{
    display:flex; flex-wrap:wrap; align-items:flex-end; gap:22px;
    background: var(--panel);
    border: 1px solid var(--line);
    padding: 18px 22px;
    margin-top: 26px;
  }
  .field{ display:flex; flex-direction:column; gap:6px; }
  .field label{
    font-family:'Roboto Mono', monospace; font-size:10.5px; letter-spacing:0.18em;
    text-transform:uppercase; color: var(--grey);
  }
  select{
    background: var(--black);
    color: var(--white);
    border: 1px solid var(--line);
    padding: 9px 12px;
    font-family:'Inter', sans-serif;
    font-size: 14px;
    min-width: 200px;
    appearance:none;
    -webkit-appearance:none;
    background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='10' height='6'><path d='M0 0L5 6L10 0Z' fill='%238a8a86'/></svg>");
    background-repeat:no-repeat;
    background-position: right 12px center;
  }
  select:focus{ outline: 1.5px solid var(--red); }
  .reset-btn{
    background: transparent; color: var(--grey); border: 1px solid var(--line);
    font-family:'Roboto Mono', monospace; font-size: 11px; letter-spacing:0.14em; text-transform:uppercase;
    padding: 10px 16px; cursor:pointer; transition: all .15s ease;
  }
  .reset-btn:hover{ color: var(--white); border-color: var(--grey); }
  .reset-btn:focus-visible, select:focus-visible, button:focus-visible{ outline: 2px solid var(--red); outline-offset: 2px; }
  .filter-count{
    margin-left:auto; font-family:'Roboto Mono', monospace; font-size:12px; color: var(--grey);
    align-self:center;
  }
  .filter-count b{ color: var(--white); }

  /* ---------- KPI cards ---------- */
  .kpis{
    display:grid; grid-template-columns: repeat(4, 1fr); gap: 1px;
    background: var(--line);
    margin-top: 22px;
    border: 1px solid var(--line);
  }
  .kpi{
    background: var(--panel);
    padding: 20px 22px;
    position: relative;
  }
  .kpi::before{
    content:""; position:absolute; top:0; left:0; width:3px; height:100%;
    background: var(--red);
  }
  .kpi .label{
    font-family:'Roboto Mono', monospace; font-size: 10.5px; letter-spacing:0.16em;
    text-transform:uppercase; color: var(--grey);
  }
  .kpi .value{
    font-family:'Roboto Mono', monospace; font-weight:700; font-size: clamp(22px, 2.6vw, 30px);
    margin-top: 10px; color: var(--white);
  }
  .kpi .sub{ font-size: 12px; color: var(--grey); margin-top: 4px; }
  .kpi .value.gold{ color: var(--gold); }

  /* ---------- Sections ---------- */
  .section-title{
    display:flex; align-items:center; gap:10px;
    margin: 46px 0 18px;
  }
  .section-title .bar{ width:4px; height:20px; background: var(--red); }
  .section-title h2{
    margin:0; font-size: clamp(18px, 2.4vw, 24px); font-weight:600; letter-spacing:0.02em;
  }
  .section-title .count{
    margin-left:auto; font-family:'Roboto Mono', monospace; font-size:11px; color: var(--grey);
  }

  .charts-grid{
    display:grid; grid-template-columns: 1.4fr 1fr; gap: 1px;
    background: var(--line); border:1px solid var(--line);
  }
  .charts-grid-2{
    display:grid; grid-template-columns: 1fr 1fr; gap: 1px;
    background: var(--line); border:1px solid var(--line); margin-top:1px;
  }
  .chart-card{
    background: var(--panel); padding: 20px 22px 14px;
  }
  .chart-card h3{
    margin:0 0 4px; font-size:14px; font-weight:600; color: var(--white);
  }
  .chart-card .cap{
    font-family:'Roboto Mono', monospace; font-size:11px; color: var(--grey); margin-bottom: 12px;
  }
  canvas{ width:100%; display:block; }
  .empty-note{
    font-family:'Roboto Mono', monospace; font-size:12px; color: var(--grey); padding: 30px 0; text-align:center;
  }

  /* ---------- Insights (perguntas) ---------- */
  .insights{
    display:grid; grid-template-columns: repeat(3, 1fr); gap: 1px;
    background: var(--line); border: 1px solid var(--line);
  }
  .insight{
    background: var(--panel-2); padding: 22px;
    display:flex; flex-direction:column; gap: 10px;
  }
  .insight .q-num{
    font-family:'Roboto Mono', monospace; color: var(--red); font-size:12px; letter-spacing:0.14em;
  }
  .insight .q{
    font-size: 15px; font-weight:600; color: var(--white); line-height:1.35;
  }
  .insight .a{
    font-family:'Barlow Condensed', sans-serif; font-weight:700; font-size: 26px;
    color: var(--gold); letter-spacing: 0.01em; text-transform:none;
  }
  .insight .a-sub{ font-size:12.5px; color: var(--grey); line-height:1.4; }

  footer{
    padding: 30px clamp(16px,4vw,56px) 40px; text-align:center;
    font-family:'Roboto Mono', monospace; font-size:11px; color:#4a4a4a;
    letter-spacing:0.08em;
  }

  @media (max-width: 900px){
    .kpis{ grid-template-columns: repeat(2, 1fr); }
    .charts-grid, .charts-grid-2{ grid-template-columns: 1fr; }
    .insights{ grid-template-columns: 1fr; }
    select{ min-width: 150px; }
  }
</style>
</head>
<body>

<header>
  <div class="crest-row">
    <div class="brand">
      <svg class="crest" viewBox="0 0 64 64" fill="none" xmlns="http://www.w3.org/2000/svg">
        <circle cx="32" cy="32" r="30" stroke="#c9a24b" stroke-width="2"/>
        <rect x="14" y="10" width="36" height="14" fill="#d5001c"/>
        <path d="M32 14 L36 24 L28 24 Z" fill="#f4f3ef"/>
        <path d="M20 40 Q32 26 44 40 Q32 54 20 40 Z" fill="#1a1a1a" stroke="#c9a24b" stroke-width="1"/>
      </svg>
      <div class="brand-text">
        <div class="eyebrow">Relatório interno · sales operations</div>
        <h1>PAINEL DE VENDAS <span>PORSCHE</span></h1>
      </div>
    </div>
    <div class="meta">
      BASE SANITIZADA<br>
      <span id="meta-range"></span>
    </div>
  </div>
  <div class="stripe"></div>
</header>

<main>

  <div class="filters">
    <div class="field">
      <label for="f-month">Mês da venda</label>
      <select id="f-month"><option value="">Todos os meses</option></select>
    </div>
    <div class="field">
      <label for="f-seller">Vendedor</label>
      <select id="f-seller"><option value="">Todos os vendedores</option></select>
    </div>
    <button class="reset-btn" id="reset-filters">Limpar filtros</button>
    <div class="filter-count"><b id="filtered-count">0</b>&nbsp;de <span id="total-count">0</span> vendas exibidas</div>
  </div>

  <div class="kpis">
    <div class="kpi">
      <div class="label">Vendas no filtro</div>
      <div class="value mono" id="kpi-count">—</div>
      <div class="sub" id="kpi-count-sub">&nbsp;</div>
    </div>
    <div class="kpi">
      <div class="label">Receita total</div>
      <div class="value mono gold" id="kpi-revenue">—</div>
      <div class="sub" id="kpi-revenue-sub">&nbsp;</div>
    </div>
    <div class="kpi">
      <div class="label">Ticket médio</div>
      <div class="value mono" id="kpi-avg">—</div>
      <div class="sub">por veículo vendido</div>
    </div>
    <div class="kpi">
      <div class="label">Modelo em destaque</div>
      <div class="value" id="kpi-model" style="font-size:20px;">—</div>
      <div class="sub" id="kpi-model-sub">&nbsp;</div>
    </div>
  </div>

  <div class="section-title">
    <div class="bar"></div>
    <h2>Desempenho</h2>
    <div class="count">gráficos atualizam conforme os filtros</div>
  </div>

  <div class="charts-grid">
    <div class="chart-card">
      <h3>Receita por mês</h3>
      <div class="cap">soma de SalesPriceSanitized, agrupada por mês da venda</div>
      <canvas id="chart-revenue" height="260"></canvas>
    </div>
    <div class="chart-card">
      <h3>Top vendedores</h3>
      <div class="cap">receita gerada, top 8</div>
      <canvas id="chart-sellers" height="260"></canvas>
    </div>
  </div>

  <div class="charts-grid-2">
    <div class="chart-card">
      <h3>Modelos mais vendidos</h3>
      <div class="cap">unidades por modelo, top 8</div>
      <canvas id="chart-models" height="230"></canvas>
    </div>
    <div class="chart-card">
      <h3>Forma de pagamento</h3>
      <div class="cap">participação por método</div>
      <canvas id="chart-payment" height="230"></canvas>
    </div>
  </div>

  <div class="section-title">
    <div class="bar"></div>
    <h2>Perguntas que os dados respondem</h2>
    <div class="count">recalculado a cada filtro</div>
  </div>

  <div class="insights">
    <div class="insight">
      <div class="q-num">01</div>
      <div class="q">Qual vendedor gerou mais receita?</div>
      <div class="a" id="ins-seller">—</div>
      <div class="a-sub" id="ins-seller-sub">&nbsp;</div>
    </div>
    <div class="insight">
      <div class="q-num">02</div>
      <div class="q">Qual modelo Porsche mais vendido?</div>
      <div class="a" id="ins-model">—</div>
      <div class="a-sub" id="ins-model-sub">&nbsp;</div>
    </div>
    <div class="insight">
      <div class="q-num">03</div>
      <div class="q">Qual estado concentra mais receita?</div>
      <div class="a" id="ins-state">—</div>
      <div class="a-sub" id="ins-state-sub">&nbsp;</div>
    </div>
  </div>

</main>

<footer>PORSCHE SALES DASHBOARD · DADOS SANITIZADOS · GERADO PARA USO INTERNO</footer>

<script>
const DATA = [{"sale_id": 7, "SaleDateSanitized": "2024-03-14", "month": "2024-03", "salesperson": "Amanda Scott", "PorscheModelSanitized": "911 Turbo S", "SalesPriceSanitized": 235000.0, "VehicleMileageSanitized": 1200, "PayMethodSanitized": "Wire Transfer", "CitySanitized": "Seattle", "StateSanitized": "WA", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 8, "SaleDateSanitized": "2024-04-18", "month": "2024-04", "salesperson": "Brian Hall", "PorscheModelSanitized": "Cayenne Coupe", "SalesPriceSanitized": 112750.0, "VehicleMileageSanitized": 6400, "PayMethodSanitized": "Financing", "CitySanitized": "Austin", "StateSanitized": "TX", "DeliveryStatusSanitized": "In Transit"}, {"sale_id": 10, "SaleDateSanitized": "2024-05-22", "month": "2024-05", "salesperson": "Thomas King", "PorscheModelSanitized": "Taycan 4S", "SalesPriceSanitized": 121000.0, "VehicleMileageSanitized": 0, "PayMethodSanitized": "Bank Transfer", "CitySanitized": "Los Angeles", "StateSanitized": "CA", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 11, "SaleDateSanitized": "2024-08-06", "month": "2024-08", "salesperson": "Lisa Ray", "PorscheModelSanitized": "Panamera 4", "SalesPriceSanitized": 104500.0, "VehicleMileageSanitized": 14500, "PayMethodSanitized": "Credit Card", "CitySanitized": "Miami", "StateSanitized": "FL", "DeliveryStatusSanitized": "Cancelled"}, {"sale_id": 12, "SaleDateSanitized": "2024-07-11", "month": "2024-07", "salesperson": "Mark Evans", "PorscheModelSanitized": "911 Carrera S", "SalesPriceSanitized": 96300.0, "VehicleMileageSanitized": 41000, "PayMethodSanitized": "Lease", "CitySanitized": "New York", "StateSanitized": "NY", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 14, "SaleDateSanitized": "2024-08-19", "month": "2024-08", "salesperson": "George Bell", "PorscheModelSanitized": "718 Boxster", "SalesPriceSanitized": 73500.0, "VehicleMileageSanitized": 22300, "PayMethodSanitized": "Debit Card", "CitySanitized": "Chicago", "StateSanitized": "IL", "DeliveryStatusSanitized": "Shipped"}, {"sale_id": 15, "SaleDateSanitized": "2024-09-02", "month": "2024-09", "salesperson": "Helen Brooks", "PorscheModelSanitized": "Macan GTS", "SalesPriceSanitized": 95000.0, "VehicleMileageSanitized": 3500, "PayMethodSanitized": "Financing", "CitySanitized": "Phoenix", "StateSanitized": "AZ", "DeliveryStatusSanitized": "In Transit"}, {"sale_id": 16, "SaleDateSanitized": "2024-09-17", "month": "2024-09", "salesperson": "Samuel Price", "PorscheModelSanitized": "Taycan Turbo", "SalesPriceSanitized": 153200.5, "VehicleMileageSanitized": 11, "PayMethodSanitized": "ACH Payment", "CitySanitized": "Dallas", "StateSanitized": "TX", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 18, "SaleDateSanitized": "2024-05-11", "month": "2024-05", "salesperson": "Angela Green", "PorscheModelSanitized": "Panamera Turbo S", "SalesPriceSanitized": 132000.0, "VehicleMileageSanitized": 19250, "PayMethodSanitized": "Cash", "CitySanitized": "San Jose", "StateSanitized": "CA", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 19, "SaleDateSanitized": "2024-12-12", "month": "2024-12", "salesperson": "Victor Stone", "PorscheModelSanitized": "Cayenne Turbo GT", "SalesPriceSanitized": 188000.0, "VehicleMileageSanitized": 2100, "PayMethodSanitized": "Crypto Payment", "CitySanitized": "Houston", "StateSanitized": "TX", "DeliveryStatusSanitized": "Awaiting Delivery"}, {"sale_id": 20, "SaleDateSanitized": "2024-12-25", "month": "2024-12", "salesperson": "Rachel Turner", "PorscheModelSanitized": "911 Carrera Cabriolet", "SalesPriceSanitized": 127800.0, "VehicleMileageSanitized": 12000, "PayMethodSanitized": "Credit Card", "CitySanitized": "Atlanta", "StateSanitized": "GA", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 21, "SaleDateSanitized": "2025-01-06", "month": "2025-01", "salesperson": "Chris Moore", "PorscheModelSanitized": "Macan", "SalesPriceSanitized": 58900.0, "VehicleMileageSanitized": 33700, "PayMethodSanitized": "Bank Transfer", "CitySanitized": "Orlando", "StateSanitized": "FL", "DeliveryStatusSanitized": "Pending"}, {"sale_id": 23, "SaleDateSanitized": "2025-02-14", "month": "2025-02", "salesperson": "Martin Young", "PorscheModelSanitized": "Taycan Cross Turismo", "SalesPriceSanitized": 118500.0, "VehicleMileageSanitized": 7800, "PayMethodSanitized": "Wire Transfer", "CitySanitized": "Charlotte", "StateSanitized": "NC", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 25, "SaleDateSanitized": "2025-03-21", "month": "2025-03", "salesperson": "Steve Gray", "PorscheModelSanitized": "911 Targa 4S", "SalesPriceSanitized": 158750.0, "VehicleMileageSanitized": 2500, "PayMethodSanitized": "Lease", "CitySanitized": "Minneapolis", "StateSanitized": "MN", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 26, "SaleDateSanitized": "2025-03-28", "month": "2025-03", "salesperson": "Olivia Ross", "PorscheModelSanitized": "Panamera", "SalesPriceSanitized": 72000.0, "VehicleMileageSanitized": 49000, "PayMethodSanitized": "Bank Transfer", "CitySanitized": "Philadelphia", "StateSanitized": "PA", "DeliveryStatusSanitized": "Cancelled"}, {"sale_id": 27, "SaleDateSanitized": "2025-04-09", "month": "2025-04", "salesperson": "Patrick Wood", "PorscheModelSanitized": "Macan Electric", "SalesPriceSanitized": 86500.0, "VehicleMileageSanitized": 0, "PayMethodSanitized": "Wire Transfer", "CitySanitized": "San Antonio", "StateSanitized": "TX", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 29, "SaleDateSanitized": "2025-05-12", "month": "2025-05", "salesperson": "Justin Cox", "PorscheModelSanitized": "Taycan GTS", "SalesPriceSanitized": 139000.0, "VehicleMileageSanitized": 6250, "PayMethodSanitized": "Financing", "CitySanitized": "Raleigh", "StateSanitized": "NC", "DeliveryStatusSanitized": "In Transit"}, {"sale_id": 30, "SaleDateSanitized": "2025-06-18", "month": "2025-06", "salesperson": "Betty Ward", "PorscheModelSanitized": "Cayenne", "SalesPriceSanitized": 76800.0, "VehicleMileageSanitized": 38400, "PayMethodSanitized": "Credit Card", "CitySanitized": "Detroit", "StateSanitized": "MI", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 32, "SaleDateSanitized": "2025-07-07", "month": "2025-07", "salesperson": "Maria Foster", "PorscheModelSanitized": "911 Carrera GTS", "SalesPriceSanitized": 119900.0, "VehicleMileageSanitized": 13600, "PayMethodSanitized": "Cash", "CitySanitized": "Indianapolis", "StateSanitized": "IN", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 33, "SaleDateSanitized": "2025-07-22", "month": "2025-07", "salesperson": "Harold Bennett", "PorscheModelSanitized": "Panamera 4 E-Hybrid", "SalesPriceSanitized": 109250.0, "VehicleMileageSanitized": 8900, "PayMethodSanitized": "Lease", "CitySanitized": "Fort Worth", "StateSanitized": "TX", "DeliveryStatusSanitized": "In Transit"}, {"sale_id": 34, "SaleDateSanitized": "2025-08-14", "month": "2025-08", "salesperson": "Teresa Cook", "PorscheModelSanitized": "Macan T", "SalesPriceSanitized": 82000.0, "VehicleMileageSanitized": 21750, "PayMethodSanitized": "Wire Transfer", "CitySanitized": "Jacksonville", "StateSanitized": "FL", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 35, "SaleDateSanitized": "2025-09-01", "month": "2025-09", "salesperson": "Frank Morgan", "PorscheModelSanitized": "Taycan Turbo S", "SalesPriceSanitized": 214000.0, "VehicleMileageSanitized": 0, "PayMethodSanitized": "Crypto Payment", "CitySanitized": "San Diego", "StateSanitized": "CA", "DeliveryStatusSanitized": "Pending Review"}, {"sale_id": 37, "SaleDateSanitized": "2025-09-18", "month": "2025-09", "salesperson": "Eric Woods", "PorscheModelSanitized": "Cayenne S", "SalesPriceSanitized": 98200.0, "VehicleMileageSanitized": 12200, "PayMethodSanitized": "Bank Transfer", "CitySanitized": "Sacramento", "StateSanitized": "CA", "DeliveryStatusSanitized": "Pending"}, {"sale_id": 38, "SaleDateSanitized": "2025-10-04", "month": "2025-10", "salesperson": "Patricia Fox", "PorscheModelSanitized": "Macan", "SalesPriceSanitized": 67500.0, "VehicleMileageSanitized": 24100, "PayMethodSanitized": "Financing", "CitySanitized": "Cleveland", "StateSanitized": "OH", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 39, "SaleDateSanitized": "2025-10-12", "month": "2025-10", "salesperson": "Alan Price", "PorscheModelSanitized": "Taycan", "SalesPriceSanitized": 116900.0, "VehicleMileageSanitized": 0, "PayMethodSanitized": "Wire Transfer", "CitySanitized": "Milwaukee", "StateSanitized": "WI", "DeliveryStatusSanitized": "In Transit"}, {"sale_id": 40, "SaleDateSanitized": "2025-10-29", "month": "2025-10", "salesperson": "Sandra Hill", "PorscheModelSanitized": "Panamera 4S", "SalesPriceSanitized": 112000.0, "VehicleMileageSanitized": 10, "PayMethodSanitized": "Cash", "CitySanitized": "Kansas City", "StateSanitized": "MO", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 41, "SaleDateSanitized": "2025-02-11", "month": "2025-02", "salesperson": "Victor Lee", "PorscheModelSanitized": "718 Boxster", "SalesPriceSanitized": 74000.0, "VehicleMileageSanitized": 31000, "PayMethodSanitized": "Debit Card", "CitySanitized": "Omaha", "StateSanitized": "NE", "DeliveryStatusSanitized": "Cancelled"}, {"sale_id": 42, "SaleDateSanitized": "2025-11-16", "month": "2025-11", "salesperson": "Monica", "PorscheModelSanitized": "911 Turbo", "SalesPriceSanitized": 198300.0, "VehicleMileageSanitized": 2400, "PayMethodSanitized": "Wire Transfer", "CitySanitized": "Albuquerque", "StateSanitized": "NM", "DeliveryStatusSanitized": "Awaiting Delivery"}, {"sale_id": 45, "SaleDateSanitized": "2025-12-07", "month": "2025-12", "salesperson": "Henry King", "PorscheModelSanitized": "Taycan 4S", "SalesPriceSanitized": 129000.0, "VehicleMileageSanitized": 0, "PayMethodSanitized": "ACH Payment", "CitySanitized": "Virginia Beach", "StateSanitized": "VA", "DeliveryStatusSanitized": "In Transit"}, {"sale_id": 46, "SaleDateSanitized": "2025-12-22", "month": "2025-12", "salesperson": "Amber Clark", "PorscheModelSanitized": "Panamera Turbo", "SalesPriceSanitized": 136000.0, "VehicleMileageSanitized": 18400, "PayMethodSanitized": "Credit Card", "CitySanitized": "Colorado Springs", "StateSanitized": "CO", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 48, "SaleDateSanitized": "2026-01-08", "month": "2026-01", "salesperson": "Grace Morris", "PorscheModelSanitized": "Cayenne E-Hybrid", "SalesPriceSanitized": 92800.0, "VehicleMileageSanitized": 15000, "PayMethodSanitized": "Lease", "CitySanitized": "Bakersfield", "StateSanitized": "CA", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 49, "SaleDateSanitized": "2026-01-15", "month": "2026-01", "salesperson": "Kyle Perry", "PorscheModelSanitized": "Macan T", "SalesPriceSanitized": 72400.0, "VehicleMileageSanitized": 19750, "PayMethodSanitized": "Cash", "CitySanitized": "Mesa", "StateSanitized": "AZ", "DeliveryStatusSanitized": "Awaiting Pickup"}, {"sale_id": 50, "SaleDateSanitized": "2026-01-28", "month": "2026-01", "salesperson": "Megan Reed", "PorscheModelSanitized": "Taycan Turbo", "SalesPriceSanitized": 158500.0, "VehicleMileageSanitized": 3200, "PayMethodSanitized": "Crypto Payment", "CitySanitized": "Atlanta", "StateSanitized": "GA", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 51, "SaleDateSanitized": "2026-02-03", "month": "2026-02", "salesperson": "Carl Simmons", "PorscheModelSanitized": "718 Cayman", "SalesPriceSanitized": 69900.0, "VehicleMileageSanitized": 27600, "PayMethodSanitized": "Bank Transfer", "CitySanitized": "Long Beach", "StateSanitized": "CA", "DeliveryStatusSanitized": "Pending"}, {"sale_id": 53, "SaleDateSanitized": "2026-02-19", "month": "2026-02", "salesperson": "Martin Brooks", "PorscheModelSanitized": "Panamera", "SalesPriceSanitized": 71500.0, "VehicleMileageSanitized": 52000, "PayMethodSanitized": "Wire Transfer", "CitySanitized": "Tulsa", "StateSanitized": "OK", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 54, "SaleDateSanitized": "2026-02-25", "month": "2026-02", "salesperson": "Diana Long", "PorscheModelSanitized": "Cayenne Turbo", "SalesPriceSanitized": 146800.0, "VehicleMileageSanitized": 11300, "PayMethodSanitized": "Credit Card", "CitySanitized": "Wichita", "StateSanitized": "KS", "DeliveryStatusSanitized": "Pending"}, {"sale_id": 55, "SaleDateSanitized": "2026-03-01", "month": "2026-03", "salesperson": "Steve Ross", "PorscheModelSanitized": "Macan Electric", "SalesPriceSanitized": 89700.0, "VehicleMileageSanitized": 0, "PayMethodSanitized": "Lease", "CitySanitized": "New Orleans", "StateSanitized": "LA", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 56, "SaleDateSanitized": "2026-03-14", "month": "2026-03", "salesperson": "Olivia Cruz", "PorscheModelSanitized": "911 Carrera S", "SalesPriceSanitized": 104600.0, "VehicleMileageSanitized": 15900, "PayMethodSanitized": "Bank Transfer", "CitySanitized": "Honolulu", "StateSanitized": "HI", "DeliveryStatusSanitized": "Cancelled"}, {"sale_id": 58, "SaleDateSanitized": "2026-04-08", "month": "2026-04", "salesperson": "Karen Stone", "PorscheModelSanitized": "Cayenne", "SalesPriceSanitized": 78400.0, "VehicleMileageSanitized": 40250, "PayMethodSanitized": "Cash", "CitySanitized": "Henderson", "StateSanitized": "NV", "DeliveryStatusSanitized": "Awaiting Review"}, {"sale_id": 60, "SaleDateSanitized": "2026-04-21", "month": "2026-04", "salesperson": "Betty James", "PorscheModelSanitized": "911 Dakar", "SalesPriceSanitized": 268900.0, "VehicleMileageSanitized": 1400, "PayMethodSanitized": "Credit Card", "CitySanitized": "Riverside", "StateSanitized": "CA", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 61, "SaleDateSanitized": "2026-04-29", "month": "2026-04", "salesperson": "Gary Price", "PorscheModelSanitized": "Panamera 4", "SalesPriceSanitized": 101300.0, "VehicleMileageSanitized": 12700, "PayMethodSanitized": "Wire Transfer", "CitySanitized": "Corpus Christi", "StateSanitized": "TX", "DeliveryStatusSanitized": "Pending"}, {"sale_id": 62, "SaleDateSanitized": "2026-05-05", "month": "2026-05", "salesperson": "Maria Green", "PorscheModelSanitized": "Macan S", "SalesPriceSanitized": 66750.0, "VehicleMileageSanitized": 29800, "PayMethodSanitized": "Cash", "CitySanitized": "St. Louis", "StateSanitized": "MO", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 63, "SaleDateSanitized": "2026-05-14", "month": "2026-05", "salesperson": "Harold Young", "PorscheModelSanitized": "Taycan Cross Turismo", "SalesPriceSanitized": 127900.0, "VehicleMileageSanitized": 7500, "PayMethodSanitized": "Lease", "CitySanitized": "Pittsburgh", "StateSanitized": "PA", "DeliveryStatusSanitized": "In Transit"}, {"sale_id": 64, "SaleDateSanitized": "2026-05-23", "month": "2026-05", "salesperson": "Teresa Ward", "PorscheModelSanitized": "Cayenne Turbo GT", "SalesPriceSanitized": 200000.0, "VehicleMileageSanitized": 1950, "PayMethodSanitized": "Wire Transfer", "CitySanitized": "Cincinnati", "StateSanitized": "OH", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 65, "SaleDateSanitized": "2026-06-02", "month": "2026-06", "salesperson": "Frank Reed", "PorscheModelSanitized": "911 Carrera Cabriolet", "SalesPriceSanitized": 132000.0, "VehicleMileageSanitized": 8800, "PayMethodSanitized": "Crypto Payment", "CitySanitized": "Anchorage", "StateSanitized": "AK", "DeliveryStatusSanitized": "Pending Review"}, {"sale_id": 66, "SaleDateSanitized": "2026-06-15", "month": "2026-06", "salesperson": "Angela Moore", "PorscheModelSanitized": "718 Cayman GT4 RS", "SalesPriceSanitized": 176400.0, "VehicleMileageSanitized": 600, "PayMethodSanitized": "Credit Card", "CitySanitized": "Plano", "StateSanitized": "TX", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 68, "SaleDateSanitized": "2026-07-07", "month": "2026-07", "salesperson": "Thomas Fox", "PorscheModelSanitized": "Macan", "SalesPriceSanitized": 59000.0, "VehicleMileageSanitized": 36000, "PayMethodSanitized": "Financing", "CitySanitized": "Greensboro", "StateSanitized": "NC", "DeliveryStatusSanitized": "Awaiting Delivery"}, {"sale_id": 69, "SaleDateSanitized": "2026-07-20", "month": "2026-07", "salesperson": "Lisa Dunn", "PorscheModelSanitized": "Taycan Turbo S", "SalesPriceSanitized": 218000.0, "VehicleMileageSanitized": 0, "PayMethodSanitized": "Wire Transfer", "CitySanitized": "Lincoln", "StateSanitized": "NE", "DeliveryStatusSanitized": "Pending"}, {"sale_id": 71, "SaleDateSanitized": "2026-04-08", "month": "2026-04", "salesperson": "George Hall", "PorscheModelSanitized": "911 Carrera GTS", "SalesPriceSanitized": 121750.0, "VehicleMileageSanitized": 5872, "PayMethodSanitized": "Credit Card", "CitySanitized": "Chandler", "StateSanitized": "AZ", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 72, "SaleDateSanitized": "2026-08-18", "month": "2026-08", "salesperson": "Helen King", "PorscheModelSanitized": "718 Boxster GTS", "SalesPriceSanitized": 91500.0, "VehicleMileageSanitized": 13300, "PayMethodSanitized": "Lease", "CitySanitized": "Reno", "StateSanitized": "NV", "DeliveryStatusSanitized": "Shipped"}, {"sale_id": 73, "SaleDateSanitized": "2026-08-31", "month": "2026-08", "salesperson": "Samuel Price", "PorscheModelSanitized": "Panamera Turbo S", "SalesPriceSanitized": 134000.0, "VehicleMileageSanitized": 20100, "PayMethodSanitized": "Wire Transfer", "CitySanitized": "Buffalo", "StateSanitized": "NY", "DeliveryStatusSanitized": "In Transit"}, {"sale_id": 74, "SaleDateSanitized": "2026-09-09", "month": "2026-09", "salesperson": "Peter Ford", "PorscheModelSanitized": "Macan GTS", "SalesPriceSanitized": 96800.0, "VehicleMileageSanitized": 0, "PayMethodSanitized": "ACH Payment", "CitySanitized": "Durham", "StateSanitized": "NC", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 75, "SaleDateSanitized": "2026-09-17", "month": "2026-09", "salesperson": "Angela Green", "PorscheModelSanitized": "Taycan 4S", "SalesPriceSanitized": 131600.0, "VehicleMileageSanitized": 2900, "PayMethodSanitized": "Wire Transfer", "CitySanitized": "Laredo", "StateSanitized": "TX", "DeliveryStatusSanitized": "Pending Approval"}, {"sale_id": 76, "SaleDateSanitized": "2026-09-28", "month": "2026-09", "salesperson": "Victor Stone", "PorscheModelSanitized": "Cayenne E-Hybrid", "SalesPriceSanitized": 94300.0, "VehicleMileageSanitized": 13100, "PayMethodSanitized": "Cash", "CitySanitized": "Madison", "StateSanitized": "WI", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 77, "SaleDateSanitized": "2026-10-06", "month": "2026-10", "salesperson": "Rachel Turner", "PorscheModelSanitized": "911 Turbo S", "SalesPriceSanitized": 242000.0, "VehicleMileageSanitized": 1100, "PayMethodSanitized": "Crypto Payment", "CitySanitized": "Lubbock", "StateSanitized": "TX", "DeliveryStatusSanitized": "Awaiting Pickup"}, {"sale_id": 78, "SaleDateSanitized": "2026-10-16", "month": "2026-10", "salesperson": "Chris Moore", "PorscheModelSanitized": "718 Cayman S", "SalesPriceSanitized": 82750.0, "VehicleMileageSanitized": 22500, "PayMethodSanitized": "Credit Card", "CitySanitized": "Toledo", "StateSanitized": "OH", "DeliveryStatusSanitized": "Cancelled"}, {"sale_id": 79, "SaleDateSanitized": "2026-10-29", "month": "2026-10", "salesperson": "Laura Hill", "PorscheModelSanitized": "Macan Electric", "SalesPriceSanitized": 91300.0, "VehicleMileageSanitized": 0, "PayMethodSanitized": "Wire Transfer", "CitySanitized": "Irvine", "StateSanitized": "CA", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 80, "SaleDateSanitized": "2026-11-03", "month": "2026-11", "salesperson": "Martin Young", "PorscheModelSanitized": "Panamera", "SalesPriceSanitized": 79900.0, "VehicleMileageSanitized": 44800, "PayMethodSanitized": "Financing", "CitySanitized": "Garland", "StateSanitized": "TX", "DeliveryStatusSanitized": "Pending"}, {"sale_id": 82, "SaleDateSanitized": "2026-12-11", "month": "2026-12", "salesperson": "Steve Gray", "PorscheModelSanitized": "911 Targa 4S", "SalesPriceSanitized": 156500.0, "VehicleMileageSanitized": 4200, "PayMethodSanitized": "Cash", "CitySanitized": "Chesapeake", "StateSanitized": "VA", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 83, "SaleDateSanitized": "2026-12-24", "month": "2026-12", "salesperson": "Olivia Ross", "PorscheModelSanitized": "Taycan", "SalesPriceSanitized": 119900.0, "VehicleMileageSanitized": 1250, "PayMethodSanitized": "Lease", "CitySanitized": "Scottsdale", "StateSanitized": "AZ", "DeliveryStatusSanitized": "Pending"}, {"sale_id": 85, "SaleDateSanitized": "2026-12-28", "month": "2026-12", "salesperson": "Karen Diaz", "PorscheModelSanitized": "911 GT3", "SalesPriceSanitized": 224000.0, "VehicleMileageSanitized": 3000, "PayMethodSanitized": "Wire Transfer", "CitySanitized": "Boise", "StateSanitized": "ID", "DeliveryStatusSanitized": "Awaiting Delivery"}, {"sale_id": 86, "SaleDateSanitized": "2027-01-15", "month": "2027-01", "salesperson": "Kevin Brown", "PorscheModelSanitized": "911 Carrera", "SalesPriceSanitized": 126900.0, "VehicleMileageSanitized": 7200, "PayMethodSanitized": "Credit Card", "CitySanitized": "Orlando", "StateSanitized": "FL", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 87, "SaleDateSanitized": "2027-01-29", "month": "2027-01", "salesperson": "Amanda Lee", "PorscheModelSanitized": "Cayenne", "SalesPriceSanitized": 84500.0, "VehicleMileageSanitized": 21400, "PayMethodSanitized": "Bank Transfer", "CitySanitized": "San Jose", "StateSanitized": "CA", "DeliveryStatusSanitized": "Pending"}, {"sale_id": 88, "SaleDateSanitized": "2027-02-11", "month": "2027-02", "salesperson": "Brian Hall", "PorscheModelSanitized": "Macan S", "SalesPriceSanitized": 69800.0, "VehicleMileageSanitized": 26300, "PayMethodSanitized": "Financing", "CitySanitized": "Tampa", "StateSanitized": "FL", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 90, "SaleDateSanitized": "2027-03-05", "month": "2027-03", "salesperson": "Thomas King", "PorscheModelSanitized": "Panamera", "SalesPriceSanitized": 81000.0, "VehicleMileageSanitized": 42, "PayMethodSanitized": "Cash", "CitySanitized": "Austin", "StateSanitized": "TX", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 91, "SaleDateSanitized": "2027-03-18", "month": "2027-03", "salesperson": "Victor Lee", "PorscheModelSanitized": "718 Cayman", "SalesPriceSanitized": 78900.0, "VehicleMileageSanitized": 17500, "PayMethodSanitized": "Debit Card", "CitySanitized": "Seattle", "StateSanitized": "WA", "DeliveryStatusSanitized": "Cancelled"}, {"sale_id": 92, "SaleDateSanitized": "2027-04-02", "month": "2027-04", "salesperson": "Monica Green", "PorscheModelSanitized": "911 Turbo S", "SalesPriceSanitized": 249300.0, "VehicleMileageSanitized": 900, "PayMethodSanitized": "Wire Transfer", "CitySanitized": "Boston", "StateSanitized": "MA", "DeliveryStatusSanitized": "Awaiting Delivery"}, {"sale_id": 95, "SaleDateSanitized": "2027-05-12", "month": "2027-05", "salesperson": "Henry King", "PorscheModelSanitized": "Taycan Turbo", "SalesPriceSanitized": 164000.0, "VehicleMileageSanitized": 0, "PayMethodSanitized": "ACH Payment", "CitySanitized": "Dallas", "StateSanitized": "TX", "DeliveryStatusSanitized": "In Transit"}, {"sale_id": 96, "SaleDateSanitized": "2027-05-27", "month": "2027-05", "salesperson": "Amber Clark", "PorscheModelSanitized": "Panamera 4S", "SalesPriceSanitized": 119000.0, "VehicleMileageSanitized": 13400, "PayMethodSanitized": "Credit Card", "CitySanitized": "San Francisco", "StateSanitized": "CA", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 98, "SaleDateSanitized": "2027-06-18", "month": "2027-06", "salesperson": "Grace Morris", "PorscheModelSanitized": "Cayenne E-Hybrid", "SalesPriceSanitized": 96800.0, "VehicleMileageSanitized": 14000, "PayMethodSanitized": "Lease", "CitySanitized": "Charlotte", "StateSanitized": "NC", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 99, "SaleDateSanitized": "2027-07-03", "month": "2027-07", "salesperson": "Kyle Perry", "PorscheModelSanitized": "Macan T", "SalesPriceSanitized": 74400.0, "VehicleMileageSanitized": 20750, "PayMethodSanitized": "Cash", "CitySanitized": "Mesa", "StateSanitized": "AZ", "DeliveryStatusSanitized": "Awaiting Pickup"}, {"sale_id": 100, "SaleDateSanitized": "2027-07-22", "month": "2027-07", "salesperson": "Megan Reed", "PorscheModelSanitized": "Taycan GTS", "SalesPriceSanitized": 148500.0, "VehicleMileageSanitized": 5200, "PayMethodSanitized": "Crypto Payment", "CitySanitized": "Atlanta", "StateSanitized": "GA", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 101, "SaleDateSanitized": "2027-08-08", "month": "2027-08", "salesperson": "Carl Simmons", "PorscheModelSanitized": "718 Boxster", "SalesPriceSanitized": 71900.0, "VehicleMileageSanitized": 29600, "PayMethodSanitized": "Bank Transfer", "CitySanitized": "Long Beach", "StateSanitized": "CA", "DeliveryStatusSanitized": "Pending"}, {"sale_id": 103, "SaleDateSanitized": "2027-09-19", "month": "2027-09", "salesperson": "Martin Brooks", "PorscheModelSanitized": "Panamera Turbo", "SalesPriceSanitized": 137500.0, "VehicleMileageSanitized": 48000, "PayMethodSanitized": "Wire Transfer", "CitySanitized": "Tulsa", "StateSanitized": "OK", "DeliveryStatusSanitized": "Delivered"}, {"sale_id": 104, "SaleDateSanitized": "2027-09-25", "month": "2027-09", "salesperson": "Diana Long", "PorscheModelSanitized": "Cayenne Turbo GT", "SalesPriceSanitized": 204800.0, "VehicleMileageSanitized": 3300, "PayMethodSanitized": "Credit Card", "CitySanitized": "Wichita", "StateSanitized": "KS", "DeliveryStatusSanitized": "Pending"}, {"sale_id": 105, "SaleDateSanitized": "2027-10-01", "month": "2027-10", "salesperson": "Steve Ross", "PorscheModelSanitized": "911 Dakar", "SalesPriceSanitized": 271700.0, "VehicleMileageSanitized": 1050, "PayMethodSanitized": "Lease", "CitySanitized": "New Orleans", "StateSanitized": "LA", "DeliveryStatusSanitized": "Delivered"}]
;

// ---------------- Helpers ----------------
const fmtUSD = n => '$' + Math.round(n).toLocaleString('en-US');
const fmtUSDShort = n => {
  if (n >= 1000000) return '$' + (n/1000000).toFixed(2) + 'M';
  if (n >= 1000) return '$' + (n/1000).toFixed(0) + 'K';
  return '$' + Math.round(n);
};
const MONTH_NAMES = ['Jan','Fev','Mar','Abr','Mai','Jun','Jul','Ago','Set','Out','Nov','Dez'];
const monthLabel = m => { const [y, mo] = m.split('-'); return MONTH_NAMES[parseInt(mo,10)-1] + '/' + y.slice(2); };

function groupSum(rows, keyFn, valFn){
  const map = new Map();
  for(const r of rows){
    const k = keyFn(r);
    map.set(k, (map.get(k)||0) + valFn(r));
  }
  return map;
}
function groupCount(rows, keyFn){
  const map = new Map();
  for(const r of rows){
    const k = keyFn(r);
    map.set(k, (map.get(k)||0) + 1);
  }
  return map;
}
function topN(map, n){
  return [...map.entries()].sort((a,b)=>b[1]-a[1]).slice(0,n);
}

// ---------------- Populate filters ----------------
const months = [...new Set(DATA.map(d=>d.month))].sort();
const sellers = [...new Set(DATA.map(d=>d.salesperson))].sort();

const monthSelect = document.getElementById('f-month');
months.forEach(m=>{
  const opt = document.createElement('option');
  opt.value = m; opt.textContent = monthLabel(m);
  monthSelect.appendChild(opt);
});
const sellerSelect = document.getElementById('f-seller');
sellers.forEach(s=>{
  const opt = document.createElement('option');
  opt.value = s; opt.textContent = s;
  sellerSelect.appendChild(opt);
});

document.getElementById('total-count').textContent = DATA.length;
const allDates = DATA.map(d=>d.SaleDateSanitized).sort();
document.getElementById('meta-range').textContent = allDates[0] + ' — ' + allDates[allDates.length-1];

document.getElementById('reset-filters').addEventListener('click', ()=>{
  monthSelect.value = ''; sellerSelect.value = '';
  render();
});
monthSelect.addEventListener('change', render);
sellerSelect.addEventListener('change', render);

// ---------------- Canvas chart primitives ----------------
function setupCanvas(canvas){
  const rect = canvas.getBoundingClientRect();
  const cssHeight = canvas.height; // set via HTML attribute, treated as CSS px target
  const dpr = window.devicePixelRatio || 1;
  const width = canvas.parentElement.clientWidth - 44; // minus card padding approx
  canvas.width = Math.max(width, 200) * dpr;
  canvas.height = cssHeight * dpr;
  canvas.style.width = Math.max(width,200) + 'px';
  canvas.style.height = cssHeight + 'px';
  const ctx = canvas.getContext('2d');
  ctx.setTransform(dpr,0,0,dpr,0,0);
  return { ctx, w: Math.max(width,200), h: cssHeight };
}

function drawEmpty(canvas, msg){
  const { ctx, w, h } = setupCanvas(canvas);
  ctx.clearRect(0,0,w,h);
  ctx.fillStyle = '#8a8a86';
  ctx.font = '12px Roboto Mono, monospace';
  ctx.textAlign = 'center';
  ctx.fillText(msg, w/2, h/2);
}

function drawBarChart(canvas, entries, opts={}){
  if(entries.length === 0){ drawEmpty(canvas, 'Sem dados para os filtros selecionados'); return; }
  const { ctx, w, h } = setupCanvas(canvas);
  ctx.clearRect(0,0,w,h);
  const horizontal = !!opts.horizontal;
  const valueFmt = opts.valueFmt || (v => v);
  const maxVal = Math.max(...entries.map(e=>e[1]));
  const padLeft = horizontal ? 132 : 46;
  const padBottom = horizontal ? 14 : 46;
  const padTop = 10;
  const padRight = horizontal ? 58 : 14;
  const plotW = w - padLeft - padRight;
  const plotH = h - padTop - padBottom;

  ctx.strokeStyle = '#2b2b2b';
  ctx.lineWidth = 1;
  ctx.beginPath();
  if(horizontal){ ctx.moveTo(padLeft, padTop); ctx.lineTo(padLeft, h - padBottom); }
  else { ctx.moveTo(padLeft, padTop); ctx.lineTo(padLeft, h - padBottom); ctx.lineTo(w - padRight, h - padBottom); }
  ctx.stroke();

  const n = entries.length;
  if(horizontal){
    const bandH = plotH / n;
    const barH = Math.min(bandH * 0.58, 26);
    entries.forEach(([label, val], i)=>{
      const y = padTop + i*bandH + (bandH-barH)/2;
      const barW = maxVal>0 ? (val/maxVal) * plotW : 0;
      ctx.fillStyle = i===0 ? '#d5001c' : '#8a2530';
      ctx.fillRect(padLeft, y, barW, barH);
      ctx.fillStyle = '#f4f3ef';
      ctx.font = '12px Inter, sans-serif';
      ctx.textAlign = 'right';
      const lbl = label.length > 16 ? label.slice(0,15)+'…' : label;
      ctx.fillText(lbl, padLeft - 10, y + barH/2 + 4);
      ctx.textAlign = 'left';
      ctx.fillStyle = '#c9a24b';
      ctx.font = '11px Roboto Mono, monospace';
      ctx.fillText(valueFmt(val), padLeft + barW + 8, y + barH/2 + 4);
    });
  } else {
    const bandW = plotW / n;
    const barW = Math.min(bandW * 0.55, 46);
    entries.forEach(([label, val], i)=>{
      const x = padLeft + i*bandW + (bandW-barW)/2;
      const barH = maxVal>0 ? (val/maxVal) * plotH : 0;
      const y = h - padBottom - barH;
      ctx.fillStyle = '#d5001c';
      ctx.fillRect(x, y, barW, barH);
      ctx.fillStyle = '#8a8a86';
      ctx.font = '10.5px Roboto Mono, monospace';
      ctx.textAlign = 'center';
      ctx.fillText(label, x + barW/2, h - padBottom + 16);
      if(barH > 16){
        ctx.fillStyle = '#0a0a0a';
        ctx.font = '10px Roboto Mono, monospace';
        ctx.fillText(valueFmt(val), x + barW/2, y + 14);
      }
    });
  }
}

function drawDonut(canvas, entries){
  if(entries.length === 0){ drawEmpty(canvas, 'Sem dados para os filtros selecionados'); return; }
  const { ctx, w, h } = setupCanvas(canvas);
  ctx.clearRect(0,0,w,h);
  const total = entries.reduce((s,e)=>s+e[1],0);
  const cx = Math.min(w*0.34, 120), cy = h/2, r = Math.min(cx, cy) - 12, rInner = r*0.58;
  const palette = ['#d5001c','#c9a24b','#8a8a86','#4a4a4a','#f4f3ef','#7a1420','#6e5a2c','#3a3a3a'];
  let start = -Math.PI/2;
  entries.forEach(([label,val], i)=>{
    const angle = (val/total) * Math.PI*2;
    ctx.beginPath();
    ctx.moveTo(cx,cy);
    ctx.arc(cx,cy,r,start,start+angle);
    ctx.closePath();
    ctx.fillStyle = palette[i % palette.length];
    ctx.fill();
    start += angle;
  });
  ctx.beginPath();
  ctx.arc(cx,cy,rInner,0,Math.PI*2);
  ctx.fillStyle = '#131313';
  ctx.fill();
  ctx.fillStyle = '#f4f3ef';
  ctx.font = '11px Roboto Mono, monospace';
  ctx.textAlign = 'center';
  ctx.fillText(total, cx, cy+4);
  ctx.font = '9px Roboto Mono, monospace';
  ctx.fillStyle = '#8a8a86';
  ctx.fillText('vendas', cx, cy+16);

  // legend
  const legendX = cx + r + 26;
  let legendY = cy - r + 6;
  ctx.textAlign = 'left';
  entries.forEach(([label,val], i)=>{
    ctx.fillStyle = palette[i % palette.length];
    ctx.fillRect(legendX, legendY-8, 9, 9);
    ctx.fillStyle = '#f4f3ef';
    ctx.font = '11.5px Inter, sans-serif';
    const pct = ((val/total)*100).toFixed(0);
    ctx.fillText(`${label} — ${pct}%`, legendX + 14, legendY);
    legendY += 19;
  });
}

// ---------------- Filtering ----------------
function currentFilters(){
  return { month: monthSelect.value, seller: sellerSelect.value };
}
function applyFilters(){
  const { month, seller } = currentFilters();
  return DATA.filter(d => (!month || d.month === month) && (!seller || d.salesperson === seller));
}

// ---------------- Render ----------------
function render(){
  const rows = applyFilters();
  document.getElementById('filtered-count').textContent = rows.length;

  // KPIs
  const revenue = rows.reduce((s,r)=>s+r.SalesPriceSanitized,0);
  document.getElementById('kpi-count').textContent = rows.length;
  document.getElementById('kpi-revenue').textContent = rows.length ? fmtUSDShort(revenue) : '—';
  document.getElementById('kpi-avg').textContent = rows.length ? fmtUSDShort(revenue/rows.length) : '—';

  const modelCounts = groupCount(rows, r=>r.PorscheModelSanitized);
  const topModel = topN(modelCounts, 1)[0];
  document.getElementById('kpi-model').textContent = topModel ? topModel[0] : '—';
  document.getElementById('kpi-model-sub').textContent = topModel ? `${topModel[1]} unidade(s) no filtro` : '\u00A0';
  document.getElementById('kpi-count-sub').textContent = rows.length ? `${new Set(rows.map(r=>r.salesperson)).size} vendedor(es) envolvidos` : '\u00A0';
  document.getElementById('kpi-revenue-sub').textContent = rows.length ? `${months.length ? '' : ''}soma das ${rows.length} venda(s) filtradas` : '\u00A0';

  // Revenue by month
  const byMonth = groupSum(rows, r=>r.month, r=>r.SalesPriceSanitized);
  const monthEntries = [...byMonth.entries()].sort((a,b)=>a[0]<b[0]?-1:1).map(([m,v])=>[monthLabel(m), v]);
  drawBarChart(document.getElementById('chart-revenue'), monthEntries, { valueFmt: fmtUSDShort });

  // Top sellers by revenue
  const bySeller = groupSum(rows, r=>r.salesperson, r=>r.SalesPriceSanitized);
  drawBarChart(document.getElementById('chart-sellers'), topN(bySeller, 8), { horizontal:true, valueFmt: fmtUSDShort });

  // Top models by count
  drawBarChart(document.getElementById('chart-models'), topN(modelCounts, 8), { horizontal:true, valueFmt: v=>v });

  // Payment method donut
  const byPayment = groupCount(rows, r=>r.PayMethodSanitized);
  drawDonut(document.getElementById('chart-payment'), topN(byPayment, 8));

  // Insights
  if(rows.length === 0){
    ['ins-seller','ins-model','ins-state'].forEach(id=>document.getElementById(id).textContent = '—');
    ['ins-seller-sub','ins-model-sub','ins-state-sub'].forEach(id=>document.getElementById(id).textContent = 'Sem dados no filtro atual');
  } else {
    const topSeller = topN(bySeller, 1)[0];
    document.getElementById('ins-seller').textContent = topSeller[0];
    document.getElementById('ins-seller-sub').textContent = `${fmtUSD(topSeller[1])} em receita no recorte atual`;

    document.getElementById('ins-model').textContent = topModel[0];
    document.getElementById('ins-model-sub').textContent = `${topModel[1]} unidade(s) vendida(s), o modelo mais frequente no recorte`;

    const byState = groupSum(rows, r=>r.StateSanitized, r=>r.SalesPriceSanitized);
    const topState = topN(byState, 1)[0];
    document.getElementById('ins-state').textContent = topState[0];
    document.getElementById('ins-state-sub').textContent = `${fmtUSD(topState[1])} em receita, gerados pelo estado de ${topState[0]}`;
  }
}

window.addEventListener('resize', render);
render();
</script>
</body>
</html>
