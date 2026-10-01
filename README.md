<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1, viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="default">
<meta name="apple-mobile-web-app-title" content="我們的錢包">
<meta name="theme-color" content="#FAF3EE">
<title>我們的錢包｜共用記帳本</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;500;700;900&display=swap" rel="stylesheet">
<style>
:root{
  --pink:#C9938A; --soft-pink:#E8C4BC; --light-pink:#F5E6E3;
  --blue:#7A9BAD; --soft-blue:#B8CFDA; --light-blue:#E3EDF3;
  --sage:#9AAE9B; --light-sage:#E8F0E8;
  --cream:#FAF3EE; --bg:#FAFAF8; --dark:#37352F; --medium:#5A5248; --gray:#9B9A97;
  --radius:18px;
  --shadow:0 4px 16px rgba(55,53,47,0.06);
}
*{box-sizing:border-box;margin:0;padding:0;}
html,body{height:100%;}
body{
  font-family:"Noto Sans TC",-apple-system,BlinkMacSystemFont,"PingFang TC","Microsoft JhengHei",sans-serif;
  background:var(--bg);
  color:var(--dark);
  -webkit-font-smoothing:antialiased;
  padding-bottom:84px;
}
button, input, select, textarea{ font-family:inherit; }

/* ---------- top bar ---------- */
.topbar{
  position:sticky; top:0; z-index:20;
  background:var(--cream);
  padding:18px 20px 14px;
  border-bottom:1px solid var(--soft-pink);
}
.topbar-row{display:flex; align-items:center; justify-content:space-between;}
.brand{display:flex; align-items:center; gap:10px;}
.brand .dot{
  width:14px;height:14px;border-radius:50%;
  background:linear-gradient(135deg,var(--pink),var(--sage));
  flex-shrink:0;
}
.brand h1{font-size:19px; font-weight:700; letter-spacing:.5px;}
.brand span{display:block; font-size:11px; color:var(--gray); font-weight:400; margin-top:1px;}
.icon-btn{
  width:38px;height:38px;border-radius:12px;
  border:none; background:var(--light-pink); color:var(--pink);
  display:flex; align-items:center; justify-content:center;
  font-size:18px; cursor:pointer;
}
.icon-btn:active{ transform:scale(0.94); }

/* ---------- container ---------- */
.wrap{ max-width:640px; margin:0 auto; padding:18px 16px 8px; }
.page{ display:none; }
.page.active{ display:block; animation:fadeIn .25s ease; }
@keyframes fadeIn{ from{opacity:0; transform:translateY(6px);} to{opacity:1; transform:translateY(0);} }

/* ---------- alert banner ---------- */
.alert{
  display:flex; align-items:center; gap:10px;
  background:var(--light-pink); border:1px solid var(--soft-pink);
  color:var(--pink); padding:13px 16px; border-radius:var(--radius);
  font-size:13.5px; font-weight:700; margin-bottom:16px;
}
.alert .emoji{ font-size:18px; }
.alert.hidden{ display:none; }

/* ---------- dashboard cards ---------- */
.card-grid{
  display:grid; grid-template-columns:1fr 1fr; gap:12px; margin-bottom:16px;
}
.card{
  border-radius:var(--radius); padding:18px 16px; box-shadow:var(--shadow);
  position:relative; overflow:hidden;
}
.card.wide{ grid-column:1 / -1; }
.card .label{ font-size:12px; color:var(--medium); font-weight:500; margin-bottom:6px; }
.card .value{ font-size:26px; font-weight:900; letter-spacing:.3px; }
.card .sub{ font-size:11px; color:var(--gray); margin-top:6px; }
.card.balance{ background:linear-gradient(135deg, var(--light-blue), var(--soft-blue) 160%); }
.card.balance .label{ color:var(--blue); }
.card.balance .value{ font-size:32px; color:var(--dark); }
.card.today{ background:var(--light-sage); }
.card.today .label{ color:var(--sage); }
.card.month{ background:var(--light-pink); }
.card.month .label{ color:var(--pink); }
.card.deposit{ background:var(--light-blue); }
.card.deposit .label{ color:var(--blue); }
.card.deposit .value{ color:var(--blue); }
.card.count{ background:#fff; border:1px solid #EEEAE4; }
.card .edit-hint{ position:absolute; top:12px; right:14px; font-size:11px; color:var(--gray); }
.balance-actions{ display:flex; gap:8px; margin-top:12px; flex-wrap:wrap; }
.mini-btn{
  border:none; border-radius:10px; padding:8px 14px; font-size:12.5px; font-weight:700;
  cursor:pointer; background:rgba(255,255,255,0.6); color:var(--dark); flex:1 1 auto; white-space:nowrap;
}
.mini-btn.deposit-mini{ background:var(--sage); color:#fff; }
.mini-btn.dual-mini{ background:var(--blue); color:#fff; }
.amt-pair{ display:flex; gap:10px; }
.amt-pair .field{ flex:1; margin-bottom:0; }
.sync-row{ display:flex; align-items:center; gap:8px; margin:10px 0 0; }
.sync-row input[type="checkbox"]{ width:16px; height:16px; accent-color:var(--blue); }
.sync-row label{ font-size:12px; color:var(--medium); font-weight:500; }
.dual-total{ font-size:12.5px; color:var(--gray); margin-top:14px; text-align:center; }
.dual-total strong{ color:var(--dark); font-weight:700; }

.section-title{
  font-size:14px; font-weight:700; color:var(--medium);
  margin:22px 0 10px; display:flex; align-items:center; justify-content:space-between;
}

/* ---------- list items ---------- */
.expense-item{
  display:flex; align-items:center; gap:12px;
  background:#fff; border:1px solid #F0ECE6; border-radius:14px;
  padding:13px 14px; margin-bottom:9px; cursor:pointer;
  transition:background .15s;
}
.expense-item:active{ background:var(--light-pink); }
.expense-item .tag{
  width:42px;height:42px;border-radius:12px; flex-shrink:0;
  background:var(--light-sage); color:var(--sage);
  display:flex; align-items:center; justify-content:center;
  font-size:18px; font-weight:700;
}
.expense-item .info{ flex:1; min-width:0; }
.expense-item .item-name{ font-size:14.5px; font-weight:700; color:var(--dark); }
.expense-item .item-meta{ font-size:11.5px; color:var(--gray); margin-top:2px; }
.expense-item .amounts{ text-align:right; flex-shrink:0; }
.expense-item .amt{ font-size:15.5px; font-weight:700; color:var(--pink); }
.expense-item .bal{ font-size:11px; color:var(--gray); margin-top:2px; }
.expense-item.deposit .tag{ background:var(--light-blue); color:var(--blue); }
.expense-item.deposit.p0 .tag{ background:var(--light-pink); color:var(--pink); }
.expense-item.deposit.p1 .tag{ background:var(--light-blue); color:var(--blue); }
.expense-item.deposit .amt{ color:var(--sage); }

.empty-state{
  text-align:center; padding:50px 20px; color:var(--gray);
}
.empty-state .big{ font-size:34px; margin-bottom:10px; }
.empty-state p{ font-size:13px; }

/* ---------- filter bar ---------- */
.filter-bar{ display:flex; gap:8px; margin-bottom:14px; }
.filter-bar input[type="text"]{
  flex:1; border:1px solid #E8E3DB; border-radius:12px;
  padding:11px 14px; font-size:14px; background:#fff;
}
.filter-bar input[type="month"]{
  border:1px solid #E8E3DB; border-radius:12px;
  padding:11px 10px; font-size:13px; background:#fff; color:var(--medium);
}
.filter-bar input:focus, select:focus, textarea:focus{ outline:2px solid var(--soft-blue); outline-offset:1px; }
.chip-row{ display:flex; gap:8px; margin-bottom:14px; flex-wrap:wrap; }
.chip{
  border:1px solid #E8E3DB; background:#fff; color:var(--medium);
  padding:7px 14px; border-radius:999px; font-size:12.5px; font-weight:500; cursor:pointer;
}
.chip.active{ background:var(--sage); border-color:var(--sage); color:#fff; font-weight:700; }

/* ---------- bottom nav ---------- */
.bottom-nav{
  position:fixed; bottom:0; left:0; right:0; z-index:20;
  background:#fff; border-top:1px solid #EEEAE4;
  display:flex; padding:8px 10px calc(8px + env(safe-area-inset-bottom));
}
.nav-btn{
  flex:1; background:none; border:none; cursor:pointer;
  display:flex; flex-direction:column; align-items:center; gap:3px;
  padding:6px 0; color:var(--gray); font-size:11px; font-weight:500;
}
.nav-btn .ic{ font-size:20px; }
.nav-btn.active{ color:var(--pink); font-weight:700; }
.fab{
  position:fixed; right:18px; bottom:78px; z-index:25;
  width:56px; height:56px; border-radius:50%;
  background:linear-gradient(135deg,var(--pink),var(--soft-pink));
  color:#fff; font-size:28px; font-weight:400; border:none;
  box-shadow:0 6px 18px rgba(201,147,138,0.45);
  display:flex; align-items:center; justify-content:center; cursor:pointer;
}
.fab:active{ transform:scale(0.94); }

/* ---------- modal ---------- */
.modal-overlay{
  position:fixed; inset:0; background:rgba(55,53,47,0.4);
  display:none; align-items:flex-end; justify-content:center; z-index:50;
}
.modal-overlay.show{ display:flex; }
.modal{
  background:var(--bg); width:100%; max-width:640px;
  border-radius:22px 22px 0 0; padding:22px 20px calc(22px + env(safe-area-inset-bottom));
  max-height:88vh; overflow-y:auto;
  animation:slideUp .25s ease;
}
@keyframes slideUp{ from{ transform:translateY(30px); opacity:0; } to{ transform:translateY(0); opacity:1; } }
.modal.center-modal{ border-radius:22px; max-width:400px; margin:auto; }
.modal-header{ display:flex; align-items:center; justify-content:space-between; margin-bottom:18px; }
.modal-header h2{ font-size:17px; font-weight:700; }
.modal-close{ background:none; border:none; font-size:22px; color:var(--gray); cursor:pointer; line-height:1; }

.type-switch{
  display:flex; background:#EFEAE4; border-radius:14px; padding:4px; margin-bottom:18px;
}
.type-switch button{
  flex:1; border:none; background:none; padding:11px 0; border-radius:11px;
  font-size:14px; font-weight:700; color:var(--medium); cursor:pointer;
}
.type-switch button.active{ background:#fff; box-shadow:0 2px 6px rgba(55,53,47,0.08); }
.type-switch button[data-type="expense"].active{ color:var(--pink); }
.type-switch button[data-type="deposit"].active{ color:var(--sage); }
.field{ margin-bottom:16px; }

.person-select{ display:flex; gap:8px; margin-top:8px; }
.person-btn{
  flex:1; border:1px solid #E8E3DB; background:#fff; color:var(--medium);
  padding:11px 8px; border-radius:12px; font-size:13.5px; font-weight:700; cursor:pointer;
}
.person-btn.active{ background:var(--light-blue); border-color:var(--blue); color:var(--blue); }
.person-btn[data-person="0"].active{ background:var(--light-pink); border-color:var(--pink); color:var(--pink); }
.person-btn[data-person="1"].active{ background:var(--light-blue); border-color:var(--blue); color:var(--blue); }

.contrib-grid{ display:grid; grid-template-columns:1fr 1fr; gap:12px; margin-bottom:16px; }
.contrib-card{
  border-radius:var(--radius); padding:16px; background:#fff; border:1px solid #EEEAE4;
}
.contrib-card .name{ font-size:12.5px; font-weight:700; color:var(--medium); margin-bottom:6px; display:flex; align-items:center; gap:6px; }
.contrib-card .name .swatch{ width:9px; height:9px; border-radius:50%; }
.contrib-card .amt{ font-size:20px; font-weight:900; color:var(--dark); }
.contrib-card .cnt{ font-size:11px; color:var(--gray); margin-top:4px; }
.contrib-bar{ height:6px; border-radius:99px; background:#EFEAE4; margin-top:22px; overflow:hidden; display:flex; }
.contrib-bar .seg{ height:100%; }

.cat-stats-card{
  background:#fff; border:1px solid #EEEAE4; border-radius:var(--radius);
  padding:18px 16px; margin-bottom:16px;
}
.cat-stat-row{ margin-bottom:16px; }
.cat-stat-row:last-child{ margin-bottom:0; }
.cat-stat-top{ display:flex; align-items:baseline; justify-content:space-between; margin-bottom:6px; cursor:pointer; }
.cat-stat-arrow{ font-size:10px; color:var(--gray); margin-left:2px; }
.cat-stat-row.open .cat-stat-name{ color:var(--pink); }
.cat-stat-detail{ margin-top:12px; padding-top:12px; border-top:1px dashed #E8E3DB; }
.cat-stat-detail .expense-item:last-child{ margin-bottom:0; }
.cat-stat-name{ font-size:13.5px; font-weight:700; color:var(--dark); }
.cat-stat-amt{ font-size:12.5px; color:var(--medium); font-weight:600; white-space:nowrap; }
.cat-stat-bar{ height:8px; border-radius:99px; background:#EFEAE4; overflow:hidden; }
.cat-stat-fill{ height:100%; border-radius:99px; }
.cat-stats-empty{ text-align:center; color:var(--gray); font-size:13px; padding:14px 0; }
.field label{ display:block; font-size:12.5px; font-weight:700; color:var(--medium); margin-bottom:7px; }
.field input, .field textarea{
  width:100%; border:1px solid #E8E3DB; border-radius:12px;
  padding:12px 14px; font-size:15px; background:#fff; color:var(--dark);
}
.field input[list]{ background:#fff; }
.field textarea{ resize:none; min-height:60px; }
.quick-cats{ display:flex; gap:7px; flex-wrap:wrap; margin-top:8px; }
.quick-cat{
  border:1px solid var(--light-sage); background:var(--light-sage); color:var(--sage);
  font-size:12px; font-weight:700; padding:6px 12px; border-radius:999px; cursor:pointer;
}
.cat-grid{ display:flex; flex-wrap:wrap; gap:8px; }
.cat-chip{
  border:1px solid #E8E3DB; background:#fff; color:var(--medium);
  font-size:13px; font-weight:600; padding:9px 14px; border-radius:999px; cursor:pointer;
}
.cat-chip.active{ background:var(--light-pink); border-color:var(--pink); color:var(--pink); font-weight:700; }
.form-actions{ display:flex; gap:10px; margin-top:20px; }
.btn{
  flex:1; padding:14px; border-radius:14px; border:none;
  font-size:14.5px; font-weight:700; cursor:pointer;
}
.btn-primary{ background:var(--pink); color:#fff; }
.btn-secondary{ background:#EFEAE4; color:var(--medium); }
.btn-danger{ background:var(--light-pink); color:var(--pink); }
.btn:active{ transform:scale(0.97); }
.today-warn{
  display:none; background:var(--light-pink); color:var(--pink);
  border-radius:10px; padding:10px 12px; font-size:12px; font-weight:700; margin-top:6px;
}
.today-warn.show{ display:block; }

.settings-row{ display:flex; gap:10px; margin-top:14px; }
.settings-row .btn{ font-size:13px; padding:12px; }

.toast{
  position:fixed; left:50%; bottom:100px; transform:translateX(-50%);
  background:var(--dark); color:#fff; padding:10px 20px; border-radius:999px;
  font-size:13px; font-weight:500; z-index:60; opacity:0; pointer-events:none;
  transition:opacity .25s;
}
.toast.show{ opacity:1; }

@media (min-width:520px){
  .card-grid{ grid-template-columns:1fr 1fr 1fr; }
  .card.balance{ grid-column:1/-1; }
}
</style>
</head>
<body>

<div class="topbar">
  <div class="topbar-row">
    <div class="brand">
      <div class="dot"></div>
      <div>
        <h1>我們的錢包</h1>
        <span>共用記帳本 · 自動算餘額</span>
      </div>
    </div>
    <button class="icon-btn" id="btnSettings" title="設定">⚙︎</button>
  </div>
</div>

<div class="wrap">

  <!-- ===== 首頁 Dashboard ===== -->
  <div class="page active" id="page-home">
    <div class="alert hidden" id="alertToday">
      <span class="emoji">⚠️</span>
      <span id="alertTodayText"></span>
    </div>

    <div class="card-grid">
      <div class="card balance wide" id="balanceCard">
        <div class="label">目前餘額</div>
        <div class="value" id="curBalance">$0</div>
        <div class="sub">初始金額 $<span id="initBalanceShow">0</span> · 點卡片內容可調整初始金額</div>
        <div class="balance-actions">
          <button type="button" class="mini-btn deposit-mini" id="btnQuickDeposit">💰 儲值</button>
          <button type="button" class="mini-btn" id="btnQuickExpense">🧾 記花費</button>
        </div>
      </div>
      <div class="card today">
        <div class="label">今日花費</div>
        <div class="value" id="todayTotal">$0</div>
        <div class="sub" id="todayCountText">0 筆</div>
      </div>
      <div class="card month">
        <div class="label">本月花費</div>
        <div class="value" id="monthTotal">$0</div>
        <div class="sub" id="monthCountText">0 筆</div>
      </div>
      <div class="card deposit">
        <div class="label">本月儲值</div>
        <div class="value" id="monthDepositTotal">$0</div>
        <div class="sub" id="monthDepositCountText">0 筆</div>
      </div>
    </div>

    <div class="section-title" style="margin:22px 0 10px;">
      <span>累計儲值（各自貢獻）</span>
    </div>
    <div class="contrib-grid">
      <div class="contrib-card">
        <div class="name"><span class="swatch" style="background:var(--pink);"></span><span id="contribName1">阿蓒</span></div>
        <div class="amt" id="contribAmt1">$0</div>
        <div class="cnt" id="contribCnt1">0 筆</div>
      </div>
      <div class="contrib-card">
        <div class="name"><span class="swatch" style="background:var(--blue);"></span><span id="contribName2">威哲</span></div>
        <div class="amt" id="contribAmt2">$0</div>
        <div class="cnt" id="contribCnt2">0 筆</div>
      </div>
    </div>
    <div class="contrib-bar" id="contribBar">
      <div class="seg" id="contribSeg1" style="background:var(--pink); width:50%;"></div>
      <div class="seg" id="contribSeg2" style="background:var(--blue); width:50%;"></div>
    </div>

    <div class="section-title">
      <span>最近紀錄</span>
      <span style="color:var(--pink); font-weight:500; cursor:pointer;" onclick="goPage('list')">查看全部 ›</span>
    </div>
    <div id="recentList"></div>
  </div>

  <!-- ===== 清單頁 List ===== -->
  <div class="page" id="page-list">
    <div class="filter-bar">
      <input type="text" id="searchInput" placeholder="搜尋項目…（例如：晚餐）">
      <input type="month" id="monthFilter">
    </div>
    <div class="chip-row">
      <button class="chip active" data-range="all">全部時間</button>
      <button class="chip" data-range="thisMonth">本月</button>
      <button class="chip" data-range="last7">近 7 天</button>
    </div>
    <div class="chip-row">
      <button class="chip active" data-type="all">全部類型</button>
      <button class="chip" data-type="expense">只看花費</button>
      <button class="chip" data-type="deposit">只看儲值</button>
    </div>
    <div class="section-title" style="margin-top:0;">
      <span id="listSummary">共 0 筆</span>
    </div>
    <div id="fullList"></div>
  </div>

  <!-- ===== 分類統計頁 Stats ===== -->
  <div class="page" id="page-stats">
    <div class="filter-bar">
      <input type="month" id="statsMonthFilter" style="flex:1;">
    </div>
    <div class="card-grid" style="grid-template-columns:1fr;">
      <div class="card month">
        <div class="label" id="statsMonthLabel">本月花費</div>
        <div class="value" id="statsMonthTotal">$0</div>
        <div class="sub" id="statsMonthCountText">0 筆</div>
      </div>
    </div>
    <div class="section-title" style="margin-top:18px;">
      <span>📊 各類別花費佔比</span>
    </div>
    <div class="cat-stats-card" id="categoryBreakdown"></div>
  </div>

</div>

<button class="fab" id="btnAdd" title="新增花費">＋</button>

<div class="bottom-nav">
  <button class="nav-btn active" data-page="home"><span class="ic">🏠</span>首頁</button>
  <button class="nav-btn" data-page="list"><span class="ic">🧾</span>所有紀錄</button>
  <button class="nav-btn" data-page="stats"><span class="ic">📊</span>分類</button>
</div>

<!-- ===== 新增/編輯 Modal ===== -->
<div class="modal-overlay" id="formOverlay">
  <div class="modal">
    <div class="modal-header">
      <h2 id="formTitle">新增花費</h2>
      <button class="modal-close" id="formClose">✕</button>
    </div>
    <div class="type-switch" id="typeSwitch">
      <button type="button" class="active" data-type="expense">🧾 花費</button>
      <button type="button" data-type="deposit">💰 儲值</button>
    </div>
    <form id="expenseForm">
      <input type="hidden" id="editId">
      <input type="hidden" id="fType" value="expense">
      <div class="field">
        <label>日期</label>
        <input type="date" id="fDate" required>
      </div>
      <div class="field" id="categoryField">
        <label>類別</label>
        <div class="cat-grid" id="categorySelect">
          <button type="button" class="cat-chip active" data-cat="餐飲">🍚 餐飲</button>
          <button type="button" class="cat-chip" data-cat="交通">🚗 交通</button>
          <button type="button" class="cat-chip" data-cat="娛樂">🎬 娛樂</button>
          <button type="button" class="cat-chip" data-cat="日用品">🧴 日用品</button>
          <button type="button" class="cat-chip" data-cat="旅遊">✈️ 旅遊</button>
          <button type="button" class="cat-chip" data-cat="其他">📦 其他</button>
        </div>
      </div>
      <div class="field">
        <label id="itemLabel">細項</label>
        <input type="text" id="fItem" placeholder="輸入細項，例如：麥當勞晚餐" required>
        <div class="quick-cats" id="quickCatsDeposit" style="display:none;">
          <button type="button" class="quick-cat" data-cat="薪水轉入">💼 薪水轉入</button>
          <button type="button" class="quick-cat" data-cat="生活費轉入">🏦 生活費轉入</button>
          <button type="button" class="quick-cat" data-cat="紅包">🧧 紅包</button>
          <button type="button" class="quick-cat" data-cat="補款">🔁 補款</button>
        </div>
      </div>
      <div class="field" id="depositModeField" style="display:none;">
        <label>儲值方式</label>
        <div class="type-switch" id="depositModeSwitch" style="margin-bottom:0;">
          <button type="button" class="active" data-mode="single">單人儲值</button>
          <button type="button" data-mode="dual">👫 雙人一起儲值</button>
        </div>
      </div>
      <div class="field" id="personField" style="display:none;">
        <label>這筆是誰儲值的？</label>
        <div class="person-select" id="personSelect">
          <button type="button" class="person-btn active" data-person="0">阿蓒</button>
          <button type="button" class="person-btn" data-person="1">威哲</button>
        </div>
      </div>
      <div class="field" id="singleAmountField">
        <label id="amountLabel">金額</label>
        <input type="number" id="fAmount" placeholder="0" min="0" step="1" required inputmode="numeric">
        <div class="today-warn" id="todayWarnInline">💡 提醒：加上這筆，今天已經花超過 $1000 囉，要不要節制一下～</div>
      </div>
      <div class="field" id="dualAmountField" style="display:none;">
        <label>兩人各自的儲值金額</label>
        <div class="amt-pair">
          <div class="field">
            <label id="dualName1Label">阿蓒</label>
            <input type="number" id="fAmount1" placeholder="0" min="0" step="1" inputmode="numeric">
          </div>
          <div class="field">
            <label id="dualName2Label">威哲</label>
            <input type="number" id="fAmount2" placeholder="0" min="0" step="1" inputmode="numeric">
          </div>
        </div>
        <div class="sync-row">
          <input type="checkbox" id="dualSync" checked>
          <label for="dualSync">兩人金額相同（自動同步）</label>
        </div>
        <div class="dual-total">合計儲值 <strong id="dualTotal">$0</strong></div>
      </div>
      <div class="field">
        <label>備註（選填）</label>
        <textarea id="fNote" placeholder="想補充的小備註…"></textarea>
      </div>
      <div class="form-actions">
        <button type="button" class="btn btn-danger" id="btnDelete" style="display:none;">刪除</button>
        <button type="submit" class="btn btn-primary">儲存</button>
      </div>
    </form>
  </div>
</div>

<!-- ===== 設定 Modal ===== -->
<div class="modal-overlay" id="settingsOverlay">
  <div class="modal center-modal">
    <div class="modal-header">
      <h2>設定</h2>
      <button class="modal-close" id="settingsClose">✕</button>
    </div>
    <div class="field">
      <label>錢包初始金額</label>
      <input type="number" id="sInitBalance" min="0" step="1" inputmode="numeric">
    </div>
    <div class="field">
      <label>兩人的稱呼（用來標記各自的儲值）</label>
      <div class="person-select" style="margin-top:0;">
        <input type="text" id="sName1" maxlength="8" style="border:1px solid #E8E3DB; border-radius:12px; padding:11px 12px; font-size:14px; text-align:center;">
        <input type="text" id="sName2" maxlength="8" style="border:1px solid #E8E3DB; border-radius:12px; padding:11px 12px; font-size:14px; text-align:center;">
      </div>
    </div>
    <button class="btn btn-primary" id="btnSaveSettings" style="width:100%;">儲存設定</button>

    <div class="section-title" style="margin-top:24px;">資料備份</div>
    <div class="settings-row">
      <button class="btn btn-secondary" id="btnExport">匯出備份</button>
      <button class="btn btn-secondary" id="btnImportTrigger">匯入備份</button>
    </div>
    <input type="file" id="importFile" accept="application/json" style="display:none;">
    <p style="font-size:11.5px; color:var(--gray); margin-top:10px; line-height:1.6;">
      資料只存在這台裝置的瀏覽器裡。換手機、換瀏覽器或清除瀏覽資料前，記得先匯出備份，之後用「匯入備份」就能還原。
    </p>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
(function(){
  "use strict";
  const STORAGE_KEY = "coupleWalletData_v1";
  const DAILY_LIMIT = 1000;

  function defaultData(){
    return { settings:{ initialBalance:0, names:["阿蓒","威哲"] }, expenses:[] };
  }
  function load(){
    try{
      const raw = localStorage.getItem(STORAGE_KEY);
      if(!raw) return defaultData();
      const parsed = JSON.parse(raw);
      if(!parsed.settings) parsed.settings = { initialBalance:0, names:["阿蓒","威哲"] };
      if(!Array.isArray(parsed.settings.names) || parsed.settings.names.length<2){
        parsed.settings.names = ["阿蓒","威哲"];
      }
      if(!Array.isArray(parsed.expenses)) parsed.expenses = [];
      return parsed;
    }catch(e){ return defaultData(); }
  }
  function save(){ localStorage.setItem(STORAGE_KEY, JSON.stringify(state)); }

  let state = load();

  // ---------- helpers ----------
  function fmt(n){
    n = Math.round(Number(n)||0);
    return "$" + n.toLocaleString("zh-Hant-TW");
  }
  function todayStr(){
    const d = new Date();
    return d.getFullYear()+"-"+String(d.getMonth()+1).padStart(2,"0")+"-"+String(d.getDate()).padStart(2,"0");
  }
  function monthStr(dateStr){ return dateStr.slice(0,7); }
  function uid(){ return Date.now().toString(36)+Math.random().toString(36).slice(2,7); }

  function sortedExpenses(){
    // sort by date asc, then createdAt asc, for running balance calc
    return [...state.expenses].sort((a,b)=>{
      if(a.date !== b.date) return a.date < b.date ? -1 : 1;
      return a.createdAt - b.createdAt;
    });
  }
  function signedAmount(e){
    return e.type === "deposit" ? Number(e.amount) : -Number(e.amount);
  }
  function withBalances(){
    const list = sortedExpenses();
    let running = Number(state.settings.initialBalance)||0;
    return list.map(e=>{
      running = running + signedAmount(e);
      return {...e, balanceAfter: running};
    });
  }
  function currentBalance(){
    const total = state.expenses.reduce((s,e)=>s+signedAmount(e),0);
    return (Number(state.settings.initialBalance)||0) + total;
  }

  // ---------- toast ----------
  let toastTimer;
  function toast(msg){
    const el = document.getElementById("toast");
    el.textContent = msg;
    el.classList.add("show");
    clearTimeout(toastTimer);
    toastTimer = setTimeout(()=>el.classList.remove("show"), 1800);
  }

  // ---------- nav ----------
  function goPage(name){
    document.querySelectorAll(".page").forEach(p=>p.classList.remove("active"));
    document.getElementById("page-"+name).classList.add("active");
    document.querySelectorAll(".nav-btn").forEach(b=>{
      b.classList.toggle("active", b.dataset.page===name);
    });
    if(name==="list") renderList();
    if(name==="stats") renderStatsPage();
    window.scrollTo(0,0);
  }
  window.goPage = goPage;
  document.querySelectorAll(".nav-btn").forEach(btn=>{
    btn.addEventListener("click", ()=>goPage(btn.dataset.page));
  });

  // ---------- render dashboard ----------
  function isExpense(e){ return e.type !== "deposit"; }
  function isDeposit(e){ return e.type === "deposit"; }
  function todayTotalAmount(){
    return state.expenses.filter(e=>e.date===todayStr() && isExpense(e)).reduce((s,e)=>s+Number(e.amount),0);
  }
  function renderDashboard(){
    const bal = currentBalance();
    document.getElementById("curBalance").textContent = fmt(bal);
    document.getElementById("initBalanceShow").textContent = Math.round(Number(state.settings.initialBalance)||0).toLocaleString("zh-Hant-TW");

    const tt = todayTotalAmount();
    const todayCount = state.expenses.filter(e=>e.date===todayStr() && isExpense(e)).length;
    document.getElementById("todayTotal").textContent = fmt(tt);
    document.getElementById("todayCountText").textContent = todayCount + " 筆";

    const thisMonth = new Date().toISOString().slice(0,7);
    const monthExpenses = state.expenses.filter(e=>monthStr(e.date)===thisMonth && isExpense(e));
    const mt = monthExpenses.reduce((s,e)=>s+Number(e.amount),0);
    document.getElementById("monthTotal").textContent = fmt(mt);
    document.getElementById("monthCountText").textContent = monthExpenses.length + " 筆";

    const monthDeposits = state.expenses.filter(e=>monthStr(e.date)===thisMonth && isDeposit(e));
    const mdt = monthDeposits.reduce((s,e)=>s+Number(e.amount),0);
    document.getElementById("monthDepositTotal").textContent = fmt(mdt);
    document.getElementById("monthDepositCountText").textContent = monthDeposits.length + " 筆";

    renderContributions();

    const alertBox = document.getElementById("alertToday");
    if(tt > DAILY_LIMIT){
      alertBox.classList.remove("hidden");
      document.getElementById("alertTodayText").textContent =
        "今天已經花了 " + fmt(tt) + "，超過 " + fmt(DAILY_LIMIT) + " 囉，記得節制一下花費～";
    } else {
      alertBox.classList.add("hidden");
    }

    renderRecent();
  }

  function personIndex(e){
    const p = Number(e.person);
    return p===1 ? 1 : 0;
  }
  function renderContributions(){
    const names = state.settings.names;
    document.getElementById("contribName1").textContent = names[0];
    document.getElementById("contribName2").textContent = names[1];

    const deposits = state.expenses.filter(isDeposit);
    let sum1=0, cnt1=0, sum2=0, cnt2=0;
    deposits.forEach(e=>{
      if(personIndex(e)===1){ sum2 += Number(e.amount); cnt2++; }
      else { sum1 += Number(e.amount); cnt1++; }
    });
    document.getElementById("contribAmt1").textContent = fmt(sum1);
    document.getElementById("contribCnt1").textContent = cnt1 + " 筆";
    document.getElementById("contribAmt2").textContent = fmt(sum2);
    document.getElementById("contribCnt2").textContent = cnt2 + " 筆";

    const total = sum1 + sum2;
    const pct1 = total>0 ? Math.round((sum1/total)*100) : 50;
    document.getElementById("contribSeg1").style.width = pct1 + "%";
    document.getElementById("contribSeg2").style.width = (100-pct1) + "%";
  }

  const CATEGORY_ICONS = {"餐飲":"🍚","交通":"🚗","娛樂":"🎬","日用品":"🧴","旅遊":"✈️","其他":"📦"};
  const CATEGORY_COLORS = {"餐飲":"var(--pink)","交通":"var(--blue)","娛樂":"var(--sage)","日用品":"var(--soft-pink)","旅遊":"var(--soft-blue)","其他":"var(--gray)","未分類":"var(--gray)"};
  let expandedCategory = null;
  let lastCategoryMonthExpenses = [];
  let lastCategoryMt = 0;

  function renderCategoryBreakdown(monthExpenses, mt){
    lastCategoryMonthExpenses = monthExpenses;
    lastCategoryMt = mt;
    const container = document.getElementById("categoryBreakdown");
    if(!monthExpenses.length){
      container.innerHTML = `<div class="cat-stats-empty">這個月還沒有花費紀錄</div>`;
      return;
    }
    const totals = {};
    monthExpenses.forEach(e=>{
      const cat = e.category || "未分類";
      totals[cat] = (totals[cat]||0) + Number(e.amount);
    });
    const rows = Object.entries(totals).sort((a,b)=>b[1]-a[1]);
    container.innerHTML = rows.map(([cat, amt])=>{
      const pct = mt>0 ? Math.round((amt/mt)*100) : 0;
      const icon = CATEGORY_ICONS[cat] || "📦";
      const color = CATEGORY_COLORS[cat] || "var(--gray)";
      const isOpen = expandedCategory === cat;
      let detailHTML = "";
      if(isOpen){
        const items = monthExpenses
          .filter(e => (e.category || "未分類") === cat)
          .slice()
          .sort((a,b)=> b.date===a.date ? b.createdAt-a.createdAt : (b.date<a.date?-1:1));
        detailHTML = `<div class="cat-stat-detail">${items.map(e=>expenseRowHTML(e,false)).join("")}</div>`;
      }
      return `
        <div class="cat-stat-row${isOpen ? " open" : ""}" data-cat="${escapeHtml(cat)}">
          <div class="cat-stat-top">
            <span class="cat-stat-name">${icon} ${escapeHtml(cat)}</span>
            <span class="cat-stat-amt">${fmt(amt)} · ${pct}% <span class="cat-stat-arrow">${isOpen ? "▴" : "▾"}</span></span>
          </div>
          <div class="cat-stat-bar"><div class="cat-stat-fill" style="width:${pct}%; background:${color};"></div></div>
          ${detailHTML}
        </div>`;
    }).join("");
    container.querySelectorAll(".cat-stat-detail").forEach(bindRowClicks);
  }
  document.getElementById("categoryBreakdown").addEventListener("click", (ev)=>{
    const top = ev.target.closest(".cat-stat-top");
    if(!top) return;
    const row = top.closest(".cat-stat-row");
    if(!row) return;
    const cat = row.dataset.cat;
    expandedCategory = (expandedCategory === cat) ? null : cat;
    renderCategoryBreakdown(lastCategoryMonthExpenses, lastCategoryMt);
  });

  function renderStatsPage(){
    const monthInput = document.getElementById("statsMonthFilter");
    const currentMonth = new Date().toISOString().slice(0,7);
    const monthVal = monthInput.value || currentMonth;
    const monthExpenses = state.expenses.filter(e=>monthStr(e.date)===monthVal && isExpense(e));
    const mt = monthExpenses.reduce((s,e)=>s+Number(e.amount),0);
    document.getElementById("statsMonthLabel").textContent = monthVal===currentMonth ? "本月花費" : (monthVal + " 花費");
    document.getElementById("statsMonthTotal").textContent = fmt(mt);
    document.getElementById("statsMonthCountText").textContent = monthExpenses.length + " 筆";
    expandedCategory = null;
    renderCategoryBreakdown(monthExpenses, mt);
  }
  document.getElementById("statsMonthFilter").addEventListener("change", renderStatsPage);
  function expenseRowHTML(e, showBalance){
    const deposit = isDeposit(e);
    const cat = deposit ? "💰" : (e.category ? (CATEGORY_ICONS[e.category] || "📦") : (e.item ? e.item.slice(0,1) : "＄"));
    const sign = deposit ? "+" : "-";
    const personName = deposit ? (state.settings.names[personIndex(e)] || "") : "";
    const metaParts = [e.date];
    if(deposit && personName) metaParts.push("由 " + personName + " 儲值");
    if(!deposit && e.category) metaParts.push(e.category);
    if(e.note) metaParts.push(e.note);
    const personClass = deposit ? (" p" + personIndex(e)) : "";
    return `
      <div class="expense-item${deposit ? " deposit" : ""}${personClass}" data-id="${e.id}">
        <div class="tag">${cat}</div>
        <div class="info">
          <div class="item-name">${escapeHtml(e.item)}</div>
          <div class="item-meta">${metaParts.map(escapeHtml).join(" · ")}</div>
        </div>
        <div class="amounts">
          <div class="amt">${sign}${fmt(e.amount)}</div>
          ${showBalance ? `<div class="bal">餘額 ${fmt(e.balanceAfter)}</div>` : ""}
        </div>
      </div>`;
  }
  function escapeHtml(s){
    return String(s||"").replace(/[&<>"']/g, c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c]));
  }

  function renderRecent(){
    const withBal = withBalances().sort((a,b)=> b.date===a.date ? b.createdAt-a.createdAt : (b.date<a.date?-1:1));
    const recent = withBal.slice(0,5);
    const container = document.getElementById("recentList");
    if(recent.length===0){
      container.innerHTML = emptyStateHTML("還沒有任何紀錄", "點右下角的「＋」開始記第一筆花費吧");
      return;
    }
    container.innerHTML = recent.map(e=>expenseRowHTML(e,true)).join("");
    bindRowClicks(container);
  }

  function emptyStateHTML(title, sub){
    return `<div class="empty-state"><div class="big">📒</div><p><strong>${title}</strong></p><p>${sub}</p></div>`;
  }

  function bindRowClicks(container){
    container.querySelectorAll(".expense-item").forEach(row=>{
      row.addEventListener("click", ()=>openEditForm(row.dataset.id));
    });
  }

  // ---------- list page ----------
  let currentRange = "all";
  let currentTypeFilter = "all";
  function renderList(){
    const search = document.getElementById("searchInput").value.trim().toLowerCase();
    const monthFilterVal = document.getElementById("monthFilter").value;
    let list = withBalances().sort((a,b)=> b.date===a.date ? b.createdAt-a.createdAt : (b.date<a.date?-1:1));

    if(search){
      list = list.filter(e => (e.item||"").toLowerCase().includes(search) || (e.note||"").toLowerCase().includes(search) || (e.category||"").toLowerCase().includes(search));
    }
    if(monthFilterVal){
      list = list.filter(e => monthStr(e.date) === monthFilterVal);
    }
    if(currentRange==="thisMonth"){
      const tm = new Date().toISOString().slice(0,7);
      list = list.filter(e => monthStr(e.date)===tm);
    } else if(currentRange==="last7"){
      const cutoff = new Date();
      cutoff.setDate(cutoff.getDate()-6);
      const cutoffStr = cutoff.toISOString().slice(0,10);
      list = list.filter(e => e.date >= cutoffStr);
    }
    if(currentTypeFilter==="expense"){
      list = list.filter(isExpense);
    } else if(currentTypeFilter==="deposit"){
      list = list.filter(isDeposit);
    }

    const spent = list.filter(isExpense).reduce((s,e)=>s+Number(e.amount),0);
    const deposited = list.filter(isDeposit).reduce((s,e)=>s+Number(e.amount),0);
    const summaryParts = [`共 ${list.length} 筆`];
    if(spent>0) summaryParts.push(`花費 ${fmt(spent)}`);
    if(deposited>0) summaryParts.push(`儲值 ${fmt(deposited)}`);
    document.getElementById("listSummary").textContent = summaryParts.join(" · ");

    const container = document.getElementById("fullList");
    if(list.length===0){
      container.innerHTML = emptyStateHTML("找不到符合的紀錄", "試試看調整搜尋或篩選條件");
      return;
    }
    container.innerHTML = list.map(e=>expenseRowHTML(e,true)).join("");
    bindRowClicks(container);
  }
  document.getElementById("searchInput").addEventListener("input", renderList);
  document.getElementById("monthFilter").addEventListener("change", renderList);
  document.querySelectorAll(".chip[data-range]").forEach(chip=>{
    chip.addEventListener("click", ()=>{
      document.querySelectorAll(".chip[data-range]").forEach(c=>c.classList.remove("active"));
      chip.classList.add("active");
      currentRange = chip.dataset.range;
      renderList();
    });
  });
  document.querySelectorAll(".chip[data-type]").forEach(chip=>{
    chip.addEventListener("click", ()=>{
      document.querySelectorAll(".chip[data-type]").forEach(c=>c.classList.remove("active"));
      chip.classList.add("active");
      currentTypeFilter = chip.dataset.type;
      renderList();
    });
  });

  // ---------- form modal ----------
  const formOverlay = document.getElementById("formOverlay");
  const expenseForm = document.getElementById("expenseForm");

  function refreshPersonLabels(){
    const names = state.settings.names;
    document.querySelectorAll("#personSelect .person-btn").forEach(btn=>{
      btn.textContent = names[Number(btn.dataset.person)] || (Number(btn.dataset.person)===1 ? "威哲" : "阿蓒");
    });
  }
  document.querySelectorAll("#personSelect .person-btn").forEach(btn=>{
    btn.addEventListener("click", ()=>{
      document.querySelectorAll("#personSelect .person-btn").forEach(b=>b.classList.remove("active"));
      btn.classList.add("active");
    });
  });
  function setPersonSelection(idx){
    document.querySelectorAll("#personSelect .person-btn").forEach(b=>{
      b.classList.toggle("active", Number(b.dataset.person)===idx);
    });
  }

  document.querySelectorAll("#categorySelect .cat-chip").forEach(btn=>{
    btn.addEventListener("click", ()=>{
      document.querySelectorAll("#categorySelect .cat-chip").forEach(b=>b.classList.remove("active"));
      btn.classList.add("active");
    });
  });
  function setCategorySelection(cat){
    const chips = document.querySelectorAll("#categorySelect .cat-chip");
    let matched = false;
    chips.forEach(b=>{
      const active = b.dataset.cat===cat;
      b.classList.toggle("active", active);
      if(active) matched = true;
    });
    if(!matched && chips.length){
      chips.forEach(b=>b.classList.remove("active"));
      chips[0].classList.add("active");
    }
  }
  function getCategorySelection(){
    const active = document.querySelector("#categorySelect .cat-chip.active");
    return active ? active.dataset.cat : "其他";
  }

  function updateDualTotal(){
    const a1 = Number(document.getElementById("fAmount1").value)||0;
    const a2 = Number(document.getElementById("fAmount2").value)||0;
    document.getElementById("dualTotal").textContent = fmt(a1+a2);
  }
  document.getElementById("fAmount1").addEventListener("input", ()=>{
    if(document.getElementById("dualSync").checked){
      document.getElementById("fAmount2").value = document.getElementById("fAmount1").value;
    }
    updateDualTotal();
  });
  document.getElementById("fAmount2").addEventListener("input", updateDualTotal);
  document.getElementById("dualSync").addEventListener("change", (ev)=>{
    document.getElementById("fAmount2").disabled = ev.target.checked;
    if(ev.target.checked){
      document.getElementById("fAmount2").value = document.getElementById("fAmount1").value;
      updateDualTotal();
    }
  });

  function setDepositMode(mode){
    document.querySelectorAll("#depositModeSwitch button").forEach(b=>{
      b.classList.toggle("active", b.dataset.mode===mode);
    });
    const single = mode !== "dual";
    document.getElementById("personField").style.display = single ? "block" : "none";
    document.getElementById("singleAmountField").style.display = single ? "block" : "none";
    document.getElementById("dualAmountField").style.display = single ? "none" : "block";
    document.getElementById("fAmount").required = single;
    document.getElementById("fAmount1").required = !single;
    document.getElementById("fAmount2").required = !single;
    document.getElementById("fItem").required = single;
    if(single){
      refreshPersonLabels();
    } else {
      document.getElementById("dualName1Label").textContent = state.settings.names[0];
      document.getElementById("dualName2Label").textContent = state.settings.names[1];
      document.getElementById("dualSync").checked = true;
      document.getElementById("fAmount2").disabled = true;
      document.getElementById("fAmount2").value = document.getElementById("fAmount1").value;
      updateDualTotal();
    }
  }
  document.querySelectorAll("#depositModeSwitch button").forEach(btn=>{
    btn.addEventListener("click", ()=>setDepositMode(btn.dataset.mode));
  });

  function setFormType(type){
    document.getElementById("fType").value = type;
    document.querySelectorAll("#typeSwitch button").forEach(b=>{
      b.classList.toggle("active", b.dataset.type===type);
    });
    if(type==="deposit"){
      document.getElementById("itemLabel").textContent = "儲值來源";
      document.getElementById("fItem").placeholder = "例如：薪水轉入、紅包、補款";
      document.getElementById("amountLabel").textContent = "儲值金額";
      document.getElementById("categoryField").style.display = "none";
      document.getElementById("quickCatsDeposit").style.display = "flex";
      document.getElementById("depositModeField").style.display = "block";
      setDepositMode("single");
      document.getElementById("todayWarnInline").classList.remove("show");
    } else {
      document.getElementById("itemLabel").textContent = "細項";
      document.getElementById("fItem").placeholder = "輸入細項，例如：麥當勞晚餐";
      document.getElementById("amountLabel").textContent = "金額";
      document.getElementById("categoryField").style.display = "block";
      document.getElementById("quickCatsDeposit").style.display = "none";
      document.getElementById("depositModeField").style.display = "none";
      document.getElementById("personField").style.display = "none";
      document.getElementById("dualAmountField").style.display = "none";
      document.getElementById("singleAmountField").style.display = "block";
      document.getElementById("fAmount").required = true;
      document.getElementById("fAmount1").required = false;
      document.getElementById("fAmount2").required = false;
      document.getElementById("fItem").required = true;
      checkTodayWarnInline();
    }
  }
  document.querySelectorAll("#typeSwitch button").forEach(btn=>{
    btn.addEventListener("click", ()=>setFormType(btn.dataset.type));
  });

  function openAddForm(presetType){
    const type = presetType === "deposit" ? "deposit" : "expense";
    document.getElementById("formTitle").textContent = type==="deposit" ? "新增儲值" : "新增花費";
    document.getElementById("editId").value = "";
    document.getElementById("fDate").value = todayStr();
    document.getElementById("fItem").value = "";
    document.getElementById("fAmount").value = "";
    document.getElementById("fAmount1").value = "";
    document.getElementById("fAmount2").value = "";
    document.getElementById("fNote").value = "";
    document.getElementById("btnDelete").style.display = "none";
    document.getElementById("todayWarnInline").classList.remove("show");
    setFormType(type);
    setPersonSelection(0);
    setCategorySelection("餐飲");
    formOverlay.classList.add("show");
    setTimeout(()=>document.getElementById("fItem").focus(), 200);
  }
  function openEditForm(id){
    const e = state.expenses.find(x=>x.id===id);
    if(!e) return;
    const type = isDeposit(e) ? "deposit" : "expense";
    document.getElementById("formTitle").textContent = type==="deposit" ? "編輯儲值" : "編輯花費";
    document.getElementById("editId").value = e.id;
    document.getElementById("fDate").value = e.date;
    document.getElementById("fItem").value = e.item;
    document.getElementById("fAmount").value = e.amount;
    document.getElementById("fAmount1").value = "";
    document.getElementById("fAmount2").value = "";
    document.getElementById("fNote").value = e.note || "";
    document.getElementById("btnDelete").style.display = "block";
    setFormType(type);
    document.getElementById("depositModeField").style.display = "none";
    setPersonSelection(personIndex(e));
    setCategorySelection(e.category || "餐飲");
    formOverlay.classList.add("show");
  }
  function closeForm(){ formOverlay.classList.remove("show"); }

  document.getElementById("btnAdd").addEventListener("click", ()=>openAddForm("expense"));
  document.getElementById("formClose").addEventListener("click", closeForm);
  formOverlay.addEventListener("click", (ev)=>{ if(ev.target===formOverlay) closeForm(); });

  document.querySelectorAll(".quick-cat").forEach(btn=>{
    btn.addEventListener("click", ()=>{ document.getElementById("fItem").value = btn.dataset.cat; });
  });

  function checkTodayWarnInline(){
    const type = document.getElementById("fType").value;
    const dateVal = document.getElementById("fDate").value;
    const amtVal = Number(document.getElementById("fAmount").value)||0;
    const editId = document.getElementById("editId").value;
    if(type==="deposit" || dateVal !== todayStr()){
      document.getElementById("todayWarnInline").classList.remove("show");
      return;
    }
    const existingToday = state.expenses
      .filter(e => e.date === todayStr() && e.id !== editId && isExpense(e))
      .reduce((s,e)=>s+Number(e.amount),0);
    const projected = existingToday + amtVal;
    document.getElementById("todayWarnInline").classList.toggle("show", projected > DAILY_LIMIT && amtVal>0);
  }
  document.getElementById("fAmount").addEventListener("input", checkTodayWarnInline);
  document.getElementById("fDate").addEventListener("change", checkTodayWarnInline);

  expenseForm.addEventListener("submit", (ev)=>{
    ev.preventDefault();
    const id = document.getElementById("editId").value;
    const type = document.getElementById("fType").value === "deposit" ? "deposit" : "expense";
    const date = document.getElementById("fDate").value;
    const note = document.getElementById("fNote").value.trim();

    if(type==="deposit" && !id){
      const modeBtn = document.querySelector("#depositModeSwitch button.active");
      const mode = modeBtn ? modeBtn.dataset.mode : "single";
      if(mode==="dual"){
        const itemVal = document.getElementById("fItem").value.trim() || "雙人儲值";
        const amt1 = Number(document.getElementById("fAmount1").value)||0;
        const amt2 = Number(document.getElementById("fAmount2").value)||0;
        if(!date || !(amt1>0) || !(amt2>0)){
          toast("請確認日期跟兩人的金額都已填寫");
          return;
        }
        const now = Date.now();
        state.expenses.push({ id:uid(), date, item:itemVal, amount:amt1, note, type:"deposit", person:0, createdAt:now });
        state.expenses.push({ id:uid(), date, item:itemVal, amount:amt2, note, type:"deposit", person:1, createdAt:now+1 });
        save();
        closeForm();
        renderDashboard();
        renderList();
        renderStatsPage();
        toast(`已新增兩筆儲值：${state.settings.names[0]} ${fmt(amt1)}、${state.settings.names[1]} ${fmt(amt2)}`);
        return;
      }
    }

    const item = document.getElementById("fItem").value.trim();
    const amount = Number(document.getElementById("fAmount").value);
    const activePersonBtn = document.querySelector("#personSelect .person-btn.active");
    const person = type==="deposit" ? Number(activePersonBtn ? activePersonBtn.dataset.person : 0) : undefined;
    const category = type==="expense" ? getCategorySelection() : undefined;
    if(!date || !item || !(amount>0)){
      toast("請確認日期、項目、金額都已填寫");
      return;
    }
    if(id){
      const idx = state.expenses.findIndex(x=>x.id===id);
      if(idx>-1){
        state.expenses[idx] = {...state.expenses[idx], date, item, amount, note, type, person, category};
      }
    } else {
      state.expenses.push({ id:uid(), date, item, amount, note, type, person, category, createdAt:Date.now() });
    }
    save();
    closeForm();
    renderDashboard();
    renderList();
    renderStatsPage();
    toast(id ? "已更新這筆紀錄" : (type==="deposit" ? "已新增一筆儲值" : "已新增一筆花費"));
  });

  document.getElementById("btnDelete").addEventListener("click", ()=>{
    const id = document.getElementById("editId").value;
    if(!id) return;
    if(!confirm("確定要刪除這筆紀錄嗎？")) return;
    state.expenses = state.expenses.filter(x=>x.id!==id);
    save();
    closeForm();
    renderDashboard();
    renderList();
    renderStatsPage();
    toast("已刪除");
  });

  // ---------- settings modal ----------
  const settingsOverlay = document.getElementById("settingsOverlay");
  function openSettings(){
    document.getElementById("sInitBalance").value = state.settings.initialBalance || 0;
    document.getElementById("sName1").value = state.settings.names[0];
    document.getElementById("sName2").value = state.settings.names[1];
    settingsOverlay.classList.add("show");
  }
  document.getElementById("btnSettings").addEventListener("click", openSettings);
  document.getElementById("balanceCard").addEventListener("click", (ev)=>{
    if(ev.target.closest(".mini-btn")) return;
    openSettings();
  });
  document.getElementById("btnQuickDeposit").addEventListener("click", (ev)=>{
    ev.stopPropagation();
    openAddForm("deposit");
  });
  document.getElementById("btnQuickExpense").addEventListener("click", (ev)=>{
    ev.stopPropagation();
    openAddForm("expense");
  });
  document.getElementById("settingsClose").addEventListener("click", ()=>settingsOverlay.classList.remove("show"));
  settingsOverlay.addEventListener("click", (ev)=>{ if(ev.target===settingsOverlay) settingsOverlay.classList.remove("show"); });

  document.getElementById("btnSaveSettings").addEventListener("click", ()=>{
    const val = Number(document.getElementById("sInitBalance").value)||0;
    const n1 = document.getElementById("sName1").value.trim() || "阿蓒";
    const n2 = document.getElementById("sName2").value.trim() || "威哲";
    state.settings.initialBalance = val;
    state.settings.names = [n1, n2];
    save();
    renderDashboard();
    settingsOverlay.classList.remove("show");
    toast("設定已儲存");
  });

  // ---------- export / import ----------
  document.getElementById("btnExport").addEventListener("click", ()=>{
    const blob = new Blob([JSON.stringify(state, null, 2)], {type:"application/json"});
    const url = URL.createObjectURL(blob);
    const a = document.createElement("a");
    const d = new Date();
    const stamp = d.getFullYear()+String(d.getMonth()+1).padStart(2,"0")+String(d.getDate()).padStart(2,"0");
    a.href = url;
    a.download = `我們的錢包_備份_${stamp}.json`;
    document.body.appendChild(a);
    a.click();
    document.body.removeChild(a);
    URL.revokeObjectURL(url);
    toast("已匯出備份檔");
  });
  document.getElementById("btnImportTrigger").addEventListener("click", ()=>{
    document.getElementById("importFile").click();
  });
  document.getElementById("importFile").addEventListener("change", (ev)=>{
    const file = ev.target.files[0];
    if(!file) return;
    const reader = new FileReader();
    reader.onload = (e)=>{
      try{
        const parsed = JSON.parse(e.target.result);
        if(!parsed || !Array.isArray(parsed.expenses)){
          toast("檔案格式不正確");
          return;
        }
        if(!confirm("匯入將會覆蓋目前裝置上的所有資料，確定要匯入嗎？")) return;
        const importedNames = Array.isArray(parsed.settings?.names) && parsed.settings.names.length>=2
          ? parsed.settings.names : ["阿蓒","威哲"];
        state = {
          settings: { initialBalance: Number(parsed.settings?.initialBalance)||0, names: importedNames },
          expenses: parsed.expenses.map(x=>({
            id: x.id || uid(),
            date: x.date,
            item: x.item,
            amount: Number(x.amount)||0,
            note: x.note || "",
            type: x.type === "deposit" ? "deposit" : "expense",
            person: x.person===1 ? 1 : 0,
            category: x.category || undefined,
            createdAt: x.createdAt || Date.now()
          }))
        };
        save();
        renderDashboard();
        renderList();
        renderStatsPage();
        settingsOverlay.classList.remove("show");
        toast("匯入成功");
      }catch(err){
        toast("匯入失敗，請確認檔案內容");
      }
      document.getElementById("importFile").value = "";
    };
    reader.readAsText(file);
  });

  // ---------- init ----------
  document.getElementById("monthFilter").value = "";
  document.getElementById("statsMonthFilter").value = new Date().toISOString().slice(0,7);
  renderDashboard();
  renderList();
})();
</script>
</body>
</html>
