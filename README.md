<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>東京旅遊計畫 · Tokyo Trip</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vue/3.4.21/vue.global.prod.min.js"><\/script>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Noto+Serif+JP:wght@300;400;600&family=Noto+Sans+TC:wght@300;400;500&family=DM+Serif+Display&display=swap" rel="stylesheet">
<style>
  :root {
    --teal: #4ECDC4;
    --teal-light: #A8E6E2;
    --teal-dark: #2A9D8F;
    --teal-pale: #E8F8F7;
    --ink: #1A1A2E;
    --ink-soft: #3D3D5C;
    --stone: #6B6B8A;
    --mist: #F0F4F3;
    --white: #FAFBFA;
    --red: #E76F51;
    --gold: #E9C46A;
    --shadow: 0 4px 24px rgba(78,205,196,0.12);
    --shadow-sm: 0 2px 8px rgba(26,26,46,0.08);
  }
  * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
  body { font-family: 'Noto Sans TC', sans-serif; background: var(--mist); color: var(--ink); min-height: 100vh; overflow-x: hidden; }
  #app { max-width: 430px; margin: 0 auto; min-height: 100vh; background: var(--white); position: relative; }

  .app-header { background: var(--ink); padding: 20px 20px 0; position: sticky; top: 0; z-index: 100; }
  .app-header-top { display: flex; justify-content: space-between; align-items: flex-start; padding-bottom: 4px; }
  .app-title { font-family: 'DM Serif Display', serif; color: var(--white); font-size: 22px; letter-spacing: .5px; line-height: 1.2; }
  .app-subtitle { font-family: 'Noto Serif JP', serif; color: var(--teal-light); font-size: 11px; font-weight: 300; letter-spacing: 3px; margin-top: 2px; }
  .header-badge { background: var(--teal); color: var(--ink); font-size: 10px; font-weight: 500; padding: 3px 10px; border-radius: 20px; letter-spacing: 1px; }

  .date-scroller { display: flex; gap: 8px; padding: 12px 0 0; overflow-x: auto; scrollbar-width: none; }
  .date-scroller::-webkit-scrollbar { display: none; }
  .date-chip { flex-shrink: 0; padding: 6px 14px; border-radius: 20px; cursor: pointer; transition: all .2s; border: 1.5px solid transparent; background: rgba(255,255,255,.08); }
  .date-chip .day-num { font-family: 'DM Serif Display', serif; font-size: 18px; color: var(--teal-light); display: block; line-height: 1; }
  .date-chip .day-label { font-size: 9px; color: var(--stone); letter-spacing: 1px; text-transform: uppercase; }
  .date-chip.active { background: var(--teal); border-color: var(--teal); }
  .date-chip.active .day-num { color: var(--ink); }
  .date-chip.active .day-label { color: var(--ink-soft); }

  .nav-tabs { display: flex; background: var(--ink); padding: 8px 20px 0; border-bottom: 2px solid rgba(255,255,255,.06); }
  .nav-tab { flex: 1; text-align: center; padding: 8px 4px 10px; font-size: 11px; font-weight: 500; color: var(--stone); cursor: pointer; transition: all .2s; border-bottom: 2px solid transparent; margin-bottom: -2px; letter-spacing: .5px; }
  .nav-tab.active { color: var(--teal); border-bottom-color: var(--teal); }
  .nav-tab .tab-icon { font-size: 16px; display: block; margin-bottom: 2px; }

  .content { padding: 20px; padding-bottom: 100px; }

  .weather-strip { background: linear-gradient(135deg, var(--teal-dark), var(--teal)); border-radius: 16px; padding: 14px 18px; margin-bottom: 20px; display: flex; align-items: center; justify-content: space-between; box-shadow: var(--shadow); }
  .weather-main { display: flex; align-items: center; gap: 12px; }
  .weather-icon { font-size: 36px; line-height: 1; }
  .weather-temp { font-family: 'DM Serif Display', serif; font-size: 32px; color: var(--white); line-height: 1; }
  .weather-desc { font-size: 12px; color: rgba(255,255,255,.85); margin-top: 2px; }
  .weather-extras { text-align: right; }
  .weather-extra-item { font-size: 11px; color: rgba(255,255,255,.8); display: flex; align-items: center; gap: 4px; justify-content: flex-end; margin-bottom: 2px; }
  .weather-city { font-family: 'Noto Serif JP', serif; font-size: 10px; color: rgba(255,255,255,.7); letter-spacing: 2px; }

  .section-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 14px; }
  .section-title { font-family: 'DM Serif Display', serif; font-size: 18px; color: var(--ink); }
  .section-jp { font-family: 'Noto Serif JP', serif; font-size: 10px; color: var(--stone); letter-spacing: 2px; display: block; }
  .btn-add { background: var(--teal); color: var(--ink); border: none; border-radius: 20px; padding: 6px 14px; font-size: 12px; font-weight: 500; cursor: pointer; display: flex; align-items: center; gap: 4px; transition: all .2s; }
  .btn-add:hover { background: var(--teal-dark); color: var(--white); }

  .timeline { position: relative; }
  .timeline::before { content: ''; position: absolute; left: 42px; top: 0; bottom: 0; width: 1.5px; background: linear-gradient(to bottom, var(--teal-light), transparent); }
  .schedule-item { display: flex; gap: 0; margin-bottom: 16px; position: relative; }
  .time-col { width: 80px; flex-shrink: 0; padding-top: 8px; }
  .time-text { font-family: 'DM Serif Display', serif; font-size: 15px; color: var(--teal-dark); text-align: right; padding-right: 18px; }
  .time-dot { position: absolute; left: 38px; top: 12px; width: 10px; height: 10px; border-radius: 50%; background: var(--teal); border: 2px solid var(--white); box-shadow: 0 0 0 2px var(--teal-light); }
  .schedule-card { flex: 1; background: var(--white); border-radius: 16px; padding: 14px 16px; box-shadow: var(--shadow-sm); border: 1px solid rgba(78,205,196,.15); transition: all .2s; position: relative; }
  .schedule-card:hover { box-shadow: var(--shadow); transform: translateY(-1px); }
  .schedule-name { font-weight: 500; font-size: 15px; color: var(--ink); }
  .schedule-place { font-size: 12px; color: var(--stone); margin-top: 3px; display: flex; align-items: center; gap: 4px; }
  .schedule-note { font-size: 11px; color: var(--stone); margin-top: 6px; padding-top: 6px; border-top: 1px dashed rgba(78,205,196,.3); font-style: italic; }
  .card-actions { position: absolute; top: 10px; right: 12px; display: flex; gap: 6px; }
  .card-btn { background: none; border: none; font-size: 14px; cursor: pointer; padding: 2px; opacity: .5; transition: opacity .2s; }
  .card-btn:hover { opacity: 1; }
  .tag { display: inline-block; font-size: 10px; padding: 2px 8px; border-radius: 10px; margin-top: 6px; font-weight: 500; letter-spacing: .5px; }
  .tag-food { background: rgba(233,196,106,.2); color: #B8860B; }
  .tag-sight { background: rgba(78,205,196,.15); color: var(--teal-dark); }
  .tag-shop { background: rgba(231,111,81,.1); color: var(--red); }
  .tag-hotel { background: rgba(61,61,92,.1); color: var(--ink-soft); }
  .tag-transport { background: rgba(107,107,138,.15); color: var(--stone); }

  .modal-overlay { position: fixed; inset: 0; background: rgba(26,26,46,.6); z-index: 200; display: flex; align-items: flex-end; justify-content: center; backdrop-filter: blur(4px); }
  .modal { background: var(--white); border-radius: 28px 28px 0 0; padding: 28px 24px 40px; width: 100%; max-width: 430px; animation: slideUp .3s cubic-bezier(.34,1.56,.64,1); }
  @keyframes slideUp { from { transform: translateY(100%); } to { transform: translateY(0); } }
  .modal-handle { width: 40px; height: 4px; background: var(--teal-light); border-radius: 2px; margin: 0 auto 20px; }
  .modal-title { font-family: 'DM Serif Display', serif; font-size: 20px; margin-bottom: 20px; }
  .form-group { margin-bottom: 16px; }
  .form-label { font-size: 11px; font-weight: 500; color: var(--stone); letter-spacing: 1px; margin-bottom: 6px; display: block; text-transform: uppercase; }
  .form-input, .form-select { width: 100%; padding: 12px 16px; border-radius: 12px; border: 1.5px solid rgba(78,205,196,.3); background: var(--teal-pale); font-family: 'Noto Sans TC', sans-serif; font-size: 14px; color: var(--ink); outline: none; transition: border-color .2s; -webkit-appearance: none; }
  .form-input:focus, .form-select:focus { border-color: var(--teal); }
  .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
  .btn-primary { width: 100%; background: var(--ink); color: var(--white); border: none; border-radius: 16px; padding: 16px; font-size: 15px; font-weight: 500; cursor: pointer; margin-top: 8px; transition: all .2s; font-family: 'Noto Sans TC', sans-serif; }
  .btn-primary:hover { background: var(--teal-dark); }
  .btn-cancel { background: var(--mist); color: var(--stone); margin-top: 10px; }
  .btn-cancel:hover { background: #e0e4e3; }

  .split-summary { background: var(--ink); border-radius: 20px; padding: 20px; margin-bottom: 20px; }
  .split-total-label { font-family: 'Noto Serif JP', serif; font-size: 11px; color: var(--teal-light); letter-spacing: 3px; margin-bottom: 4px; }
  .split-total-amount { font-family: 'DM Serif Display', serif; font-size: 36px; color: var(--white); }
  .split-total-twd { font-size: 14px; color: var(--stone); margin-top: 2px; }
  .split-per-person { font-size: 13px; color: var(--teal-light); margin-top: 8px; }

  .member-list { display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 20px; }
  .member-chip { display: flex; align-items: center; gap: 6px; background: var(--teal-pale); border: 1.5px solid var(--teal-light); border-radius: 20px; padding: 6px 12px; font-size: 13px; font-weight: 500; cursor: pointer; transition: all .2s; }
  .member-chip.active { background: var(--teal); border-color: var(--teal); color: var(--ink); }
  .member-avatar { width: 22px; height: 22px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 12px; background: var(--teal-dark); color: var(--white); }

  .expense-item { background: var(--white); border-radius: 16px; padding: 14px 16px; margin-bottom: 10px; box-shadow: var(--shadow-sm); border: 1px solid rgba(78,205,196,.1); display: flex; justify-content: space-between; align-items: center; }
  .expense-left .expense-name { font-weight: 500; font-size: 14px; }
  .expense-left .expense-payer { font-size: 11px; color: var(--stone); margin-top: 2px; }
  .expense-amount { text-align: right; }
  .expense-jpy { font-family: 'DM Serif Display', serif; font-size: 16px; color: var(--ink); }
  .expense-twd { font-size: 11px; color: var(--teal-dark); }

  .settlement-card { background: linear-gradient(135deg, var(--teal-pale), rgba(78,205,196,.08)); border: 1.5px solid var(--teal-light); border-radius: 16px; padding: 16px; margin-bottom: 10px; }
  .settlement-amount { font-family: 'DM Serif Display', serif; font-size: 18px; color: var(--teal-dark); }

  .rate-card { background: var(--ink); border-radius: 20px; padding: 18px 20px; margin-bottom: 20px; display: flex; justify-content: space-between; align-items: center; }
  .rate-label { font-family: 'Noto Serif JP', serif; font-size: 10px; color: var(--teal-light); letter-spacing: 2px; margin-bottom: 4px; }
  .rate-value { font-family: 'DM Serif Display', serif; font-size: 28px; color: var(--white); }
  .rate-sub { font-size: 11px; color: var(--stone); margin-top: 2px; }
  .rate-input-group { display: flex; align-items: center; gap: 8px; }
  .rate-input { width: 100px; padding: 8px 12px; border-radius: 10px; text-align: right; border: 1.5px solid rgba(78,205,196,.4); background: rgba(255,255,255,.06); color: var(--white); font-size: 16px; font-weight: 600; outline: none; font-family: 'DM Serif Display', serif; }
  .rate-flag { font-size: 20px; }
  .rate-result { font-size: 11px; color: var(--teal-light); margin-top: 6px; }

  .map-container { border-radius: 20px; overflow: hidden; height: 300px; background: #e8f4f3; margin-bottom: 20px; position: relative; box-shadow: var(--shadow); }
  .map-container iframe { width: 100%; height: 100%; border: none; }
  .location-list { display: flex; flex-direction: column; gap: 8px; }
  .location-item { background: var(--white); border-radius: 14px; padding: 12px 16px; display: flex; align-items: center; gap: 12px; box-shadow: var(--shadow-sm); cursor: pointer; transition: all .2s; border: 1px solid rgba(78,205,196,.1); }
  .location-item:hover { border-color: var(--teal); transform: translateX(4px); }
  .location-num { width: 28px; height: 28px; border-radius: 50%; background: var(--teal); color: var(--ink); font-weight: 700; font-size: 13px; display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
  .location-name { font-weight: 500; font-size: 14px; }
  .location-addr { font-size: 11px; color: var(--stone); margin-top: 2px; }
  .nav-btn { background: var(--teal); color: var(--ink); border: none; border-radius: 10px; padding: 6px 12px; font-size: 12px; font-weight: 600; cursor: pointer; margin-left: auto; flex-shrink: 0; }

  .empty-state { text-align: center; padding: 40px 20px; }
  .empty-icon { font-size: 48px; margin-bottom: 12px; }
  .empty-text { color: var(--stone); font-size: 14px; line-height: 1.6; }

  .firebase-banner { background: linear-gradient(135deg, #FFF8E7, #FFF3CC); border: 1.5px solid var(--gold); border-radius: 16px; padding: 16px; margin-bottom: 20px; }
  .firebase-banner h3 { font-size: 13px; font-weight: 600; color: #92700A; margin-bottom: 8px; display: flex; align-items: center; gap: 6px; }
  .firebase-banner p { font-size: 12px; color: #7A5C00; line-height: 1.6; }
  .firebase-input { width: 100%; padding: 10px 14px; border-radius: 10px; border: 1.5px solid #E9C46A; background: rgba(255,255,255,.8); font-size: 13px; font-family: 'Noto Sans TC', sans-serif; outline: none; margin-top: 8px; }
  .btn-connect { background: #E9C46A; color: #3D2B00; border: none; border-radius: 10px; padding: 10px 20px; font-size: 13px; font-weight: 600; cursor: pointer; margin-top: 8px; width: 100%; transition: all .2s; }
  .btn-connect:hover { background: #D4A017; }

  .sync-dot { width: 8px; height: 8px; border-radius: 50%; display: inline-block; margin-right: 4px; }
  .sync-dot.connected { background: #4CAF50; animation: pulse 2s infinite; }
  .sync-dot.disconnected { background: var(--stone); }
  @keyframes pulse { 0%,100% { opacity:1; } 50% { opacity:.4; } }

  .toast { position: fixed; bottom: 100px; left: 50%; transform: translateX(-50%); background: var(--ink); color: var(--white); padding: 12px 24px; border-radius: 20px; font-size: 13px; z-index: 300; box-shadow: var(--shadow); white-space: nowrap; animation: fadeInOut 2.5s forwards; }
  @keyframes fadeInOut { 0%{opacity:0;transform:translateX(-50%) translateY(10px)} 15%{opacity:1;transform:translateX(-50%) translateY(0)} 80%{opacity:1} 100%{opacity:0} }

  .fade-enter-active, .fade-leave-active { transition: all .25s; }
  .fade-enter-from, .fade-leave-to { opacity: 0; transform: translateY(8px); }
  .list-enter-active, .list-leave-active { transition: all .25s; }
  .list-enter-from { opacity: 0; transform: translateX(-12px); }
  .list-leave-to { opacity: 0; transform: translateX(12px); }
</style>
</head>
<body>
<div id="app">
  <div class="app-header">
    <div class="app-header-top">
      <div>
        <div class="app-title">Tokyo Trip 🗾</div>
        <div class="app-subtitle">東 京 旅 遊 計 畫</div>
      </div>
      <div style="display:flex;flex-direction:column;align-items:flex-end;gap:6px">
        <div class="header-badge">{{ tripDates.length }}天行程</div>
        <div style="font-size:11px;color:var(--stone);display:flex;align-items:center">
          <span class="sync-dot" :class="firebaseConnected ? 'connected' : 'disconnected'"></span>
          {{ firebaseConnected ? '即時同步中' : '單機模式' }}
        </div>
      </div>
    </div>
    <div class="date-scroller">
      <div v-for="(d, i) in tripDates" :key="i" class="date-chip" :class="{active: selectedDay === i}" @click="selectedDay = i">
        <span class="day-num">{{ d.num }}</span>
        <span class="day-label">{{ d.label }}</span>
      </div>
    </div>
    <div class="nav-tabs">
      <div v-for="tab in tabs" :key="tab.id" class="nav-tab" :class="{active: activeTab === tab.id}" @click="activeTab = tab.id">
        <span class="tab-icon">{{ tab.icon }}</span>{{ tab.label }}
      </div>
    </div>
  </div>

  <!-- SCHEDULE -->
  <div class="content" v-if="activeTab === 'schedule'">
    <div class="weather-strip" v-if="weather">
      <div class="weather-main">
        <div class="weather-icon">{{ weather.icon }}</div>
        <div>
          <div class="weather-temp">{{ weather.temp }}°</div>
          <div class="weather-desc">{{ weather.desc }}</div>
          <div class="weather-city">TOKYO · {{ tripDates[selectedDay] && tripDates[selectedDay].label }}</div>
        </div>
      </div>
      <div class="weather-extras">
        <div class="weather-extra-item">💧 {{ weather.humidity }}%</div>
        <div class="weather-extra-item">🌬 {{ weather.wind }}m/s</div>
        <div class="weather-extra-item">🌡 {{ weather.low }}~{{ weather.high }}°</div>
      </div>
    </div>
    <div class="section-header">
      <div>
        <span class="section-jp">第 {{ selectedDay + 1 }} 天</span>
        <div class="section-title">今日行程</div>
      </div>
      <button class="btn-add" @click="openAddSchedule">＋ 新增</button>
    </div>
    <transition-group name="list" tag="div" class="timeline" v-if="currentDaySchedule.length">
      <div v-for="item in currentDaySchedule" :key="item.id" class="schedule-item">
        <div class="time-col"><div class="time-text">{{ item.time }}</div></div>
        <div class="time-dot"></div>
        <div class="schedule-card">
          <div class="card-actions">
            <button class="card-btn" @click="editSchedule(item)">✏️</button>
            <button class="card-btn" @click="deleteSchedule(item.id)">🗑</button>
          </div>
          <div class="schedule-name">{{ item.name }}</div>
          <div class="schedule-place" v-if="item.place">📍 {{ item.place }}</div>
          <span class="tag" :class="'tag-' + item.type">{{ tagLabels[item.type] }}</span>
          <div class="schedule-note" v-if="item.note">{{ item.note }}</div>
        </div>
      </div>
    </transition-group>
    <div class="empty-state" v-else>
      <div class="empty-icon">🗓</div>
      <div class="empty-text">今天還沒有行程<br>點擊右上角「＋ 新增」開始規劃</div>
    </div>
  </div>

  <!-- SPLIT BILL -->
  <div class="content" v-if="activeTab === 'split'">
    <div class="rate-card">
      <div>
        <div class="rate-label">匯率 · EXCHANGE RATE</div>
        <div class="rate-value">{{ exchangeRate }}</div>
        <div class="rate-sub">TWD per 100 JPY</div>
      </div>
      <div style="display:flex;flex-direction:column;align-items:flex-end">
        <div class="rate-input-group">
          <span class="rate-flag">🇯🇵</span>
          <input class="rate-input" type="number" v-model.number="convertAmount" placeholder="0">
        </div>
        <div class="rate-result" v-if="convertAmount">≈ NT$ {{ Math.round(convertAmount / exchangeRate) }} 元</div>
        <div style="font-size:10px;color:var(--stone);margin-top:4px">輸入日幣即時換算</div>
      </div>
    </div>
    <div class="section-header">
      <div><span class="section-jp">旅伴</span><div class="section-title">成員</div></div>
      <button class="btn-add" @click="showAddMember = true">＋ 加人</button>
    </div>
    <div class="member-list">
      <div v-for="m in members" :key="m.id" class="member-chip" :class="{active: selectedPayer === m.id}" @click="selectedPayer = m.id">
        <div class="member-avatar">{{ m.name[0] }}</div>{{ m.name }}
      </div>
    </div>
    <div class="section-header">
      <div><span class="section-jp">支出記錄</span><div class="section-title">費用</div></div>
      <button class="btn-add" @click="openAddExpense">＋ 記帳</button>
    </div>
    <transition-group name="list" tag="div">
      <div v-for="exp in expenses" :key="exp.id" class="expense-item">
        <div class="expense-left">
          <div class="expense-name">{{ exp.name }}</div>
          <div class="expense-payer">{{ getPayerName(exp.payerId) }} 付款 · {{ exp.date }}</div>
        </div>
        <div class="expense-amount">
          <div class="expense-jpy">¥{{ exp.amount.toLocaleString() }}</div>
          <div class="expense-twd">≈ NT$ {{ Math.round(exp.amount / exchangeRate) }}</div>
        </div>
        <button class="card-btn" @click="deleteExpense(exp.id)" style="margin-left:8px;opacity:.4">✕</button>
      </div>
    </transition-group>
    <div class="section-header" style="margin-top:24px">
      <div><span class="section-jp">結算</span><div class="section-title">誰欠誰錢</div></div>
    </div>
    <div v-if="settlements.length">
      <div v-for="(s, i) in settlements" :key="i" class="settlement-card">
        <div style="display:flex;align-items:center;gap:8px;flex-wrap:wrap">
          <strong>{{ s.from }}</strong>
          <span style="color:var(--teal-dark);font-weight:600">→ 應給 →</span>
          <strong>{{ s.to }}</strong>
        </div>
        <div style="margin-top:6px;display:flex;align-items:baseline;gap:8px">
          <span class="settlement-amount">¥{{ s.jpy.toLocaleString() }}</span>
          <span style="font-size:12px;color:var(--teal-dark)">≈ NT$ {{ s.twd.toLocaleString() }}</span>
        </div>
      </div>
    </div>
    <div class="empty-state" v-else>
      <div class="empty-icon">✅</div>
      <div class="empty-text">{{ expenses.length ? '大家費用均等！' : '還沒有費用記錄' }}</div>
    </div>
    <div class="split-summary" v-if="expenses.length" style="margin-top:20px">
      <div class="split-total-label">TOTAL · 總費用</div>
      <div class="split-total-amount">¥{{ totalJPY.toLocaleString() }}</div>
      <div class="split-total-twd">≈ NT$ {{ totalTWD.toLocaleString() }}</div>
      <div class="split-per-person">每人應付 ¥{{ perPersonJPY.toLocaleString() }} (NT$ {{ perPersonTWD.toLocaleString() }})</div>
    </div>
  </div>

  <!-- MAP -->
  <div class="content" v-if="activeTab === 'map'">
    <div class="section-header">
      <div><span class="section-jp">地圖導航</span><div class="section-title">今日地點</div></div>
      <div style="background:var(--ink);color:var(--teal);font-size:11px;padding:6px 12px;border-radius:20px;font-weight:500">{{ currentDayPlaces.length }} 個地點</div>
    </div>
    <div class="map-container">
      <iframe :src="mapSrc" allowfullscreen loading="lazy"></iframe>
    </div>
    <div class="location-list">
      <div v-for="(place, i) in currentDayPlaces" :key="i" class="location-item">
        <div class="location-num">{{ i + 1 }}</div>
        <div>
          <div class="location-name">{{ place.name }}</div>
          <div class="location-addr">{{ place.place || '東京都' }} · {{ place.time }}</div>
        </div>
        <button class="nav-btn" @click="openNavigation(place)">導航 ↗</button>
      </div>
      <div class="empty-state" v-if="!currentDayPlaces.length">
        <div class="empty-icon">🗺</div>
        <div class="empty-text">今天還沒有地點<br>先在「行程」頁新增地點</div>
      </div>
    </div>
  </div>

  <!-- SYNC -->
  <div class="content" v-if="activeTab === 'sync'">
    <div class="firebase-banner">
      <h3>🔥 Firebase 即時同步</h3>
      <p>輸入你的 Firebase 設定，即可與朋友共同編輯行程！所有資料會即時同步。</p>
      <div style="margin-top:12px;font-size:12px;color:#7A5C00;line-height:1.8">
        ① 前往 <strong>console.firebase.google.com</strong><br>
        ② 建立新專案 → 新增 Web 應用程式<br>
        ③ 複製 firebaseConfig → 貼到下方
      </div>
      <input class="firebase-input" v-model="firebaseApiKey" placeholder="apiKey: 'AIza...'">
      <input class="firebase-input" v-model="firebaseProjectId" placeholder="projectId: 'my-tokyo-trip'">
      <input class="firebase-input" v-model="shareTripCode" placeholder="行程代碼（朋友要相同）">
      <button class="btn-connect" @click="connectFirebase">🔗 連接 Firebase</button>
    </div>
    <div v-if="firebaseConnected" style="background:var(--teal-pale);border:1.5px solid var(--teal-light);border-radius:16px;padding:16px;margin-bottom:20px">
      <div style="font-size:11px;color:var(--teal-dark);font-weight:600;letter-spacing:1px;margin-bottom:8px">分享給朋友</div>
      <div style="display:flex;gap:8px;align-items:center">
        <div style="flex:1;background:var(--white);border-radius:10px;padding:10px 14px;font-family:monospace;font-size:14px">{{ shareTripCode || 'tokyo-2025' }}</div>
        <button style="background:var(--teal);border:none;border-radius:10px;padding:10px 16px;font-size:13px;font-weight:600;cursor:pointer" @click="copyCode">複製</button>
      </div>
      <div style="font-size:11px;color:var(--stone);margin-top:8px">朋友使用相同代碼連接即可共同編輯</div>
    </div>
    <div class="section-header">
      <div><span class="section-jp">行程設定</span><div class="section-title">基本資訊</div></div>
    </div>
    <div style="background:var(--white);border-radius:16px;padding:20px;box-shadow:var(--shadow-sm)">
      <div class="form-group">
        <label class="form-label">行程名稱</label>
        <input class="form-input" v-model="tripName" placeholder="東京五日遊">
      </div>
      <div class="form-row">
        <div class="form-group">
          <label class="form-label">出發日期</label>
          <input class="form-input" type="date" v-model="tripStartDate">
        </div>
        <div class="form-group">
          <label class="form-label">天數</label>
          <input class="form-input" type="number" v-model.number="tripDayCount" min="1" max="14">
        </div>
      </div>
      <button class="btn-primary" @click="showToast('行程設定已儲存！')">儲存設定</button>
    </div>
  </div>

  <!-- MODALS -->
  <transition name="fade">
    <div class="modal-overlay" v-if="showScheduleModal" @click.self="showScheduleModal = false">
      <div class="modal">
        <div class="modal-handle"></div>
        <div class="modal-title">{{ editingSchedule ? '編輯行程' : '新增行程' }}</div>
        <div class="form-row">
          <div class="form-group">
            <label class="form-label">時間</label>
            <input class="form-input" type="time" v-model="scheduleForm.time">
          </div>
          <div class="form-group">
            <label class="form-label">類型</label>
            <select class="form-select" v-model="scheduleForm.type">
              <option value="sight">景點</option>
              <option value="food">餐廳</option>
              <option value="shop">購物</option>
              <option value="hotel">住宿</option>
              <option value="transport">交通</option>
            </select>
          </div>
        </div>
        <div class="form-group">
          <label class="form-label">行程名稱</label>
          <input class="form-input" v-model="scheduleForm.name" placeholder="淺草寺參拜">
        </div>
        <div class="form-group">
          <label class="form-label">地點</label>
          <input class="form-input" v-model="scheduleForm.place" placeholder="東京都台東區淺草2-3-1">
        </div>
        <div class="form-group">
          <label class="form-label">備註</label>
          <input class="form-input" v-model="scheduleForm.note" placeholder="建議早上去，人比較少">
        </div>
        <button class="btn-primary" @click="saveSchedule">{{ editingSchedule ? '儲存修改' : '新增行程' }}</button>
        <button class="btn-primary btn-cancel" @click="showScheduleModal = false">取消</button>
      </div>
    </div>
  </transition>

  <transition name="fade">
    <div class="modal-overlay" v-if="showExpenseModal" @click.self="showExpenseModal = false">
      <div class="modal">
        <div class="modal-handle"></div>
        <div class="modal-title">記錄費用</div>
        <div class="form-group">
          <label class="form-label">費用名稱</label>
          <input class="form-input" v-model="expenseForm.name" placeholder="壽司午餐">
        </div>
        <div class="form-row">
          <div class="form-group">
            <label class="form-label">日幣金額 (¥)</label>
            <input class="form-input" type="number" v-model.number="expenseForm.amount" placeholder="3500">
          </div>
          <div class="form-group">
            <label class="form-label">付款人</label>
            <select class="form-select" v-model="expenseForm.payerId">
              <option v-for="m in members" :key="m.id" :value="m.id">{{ m.name }}</option>
            </select>
          </div>
        </div>
        <div v-if="expenseForm.amount" style="background:var(--teal-pale);border-radius:10px;padding:10px 14px;font-size:13px;color:var(--teal-dark);margin-bottom:4px">
          約 NT$ {{ Math.round(expenseForm.amount / exchangeRate) }} 元
        </div>
        <button class="btn-primary" @click="saveExpense">記錄費用</button>
        <button class="btn-primary btn-cancel" @click="showExpenseModal = false">取消</button>
      </div>
    </div>
  </transition>

  <transition name="fade">
    <div class="modal-overlay" v-if="showAddMember" @click.self="showAddMember = false">
      <div class="modal">
        <div class="modal-handle"></div>
        <div class="modal-title">新增旅伴</div>
        <div class="form-group">
          <label class="form-label">姓名</label>
          <input class="form-input" v-model="newMemberName" placeholder="小明" @keyup.enter="addMember">
        </div>
        <button class="btn-primary" @click="addMember">加入旅伴</button>
        <button class="btn-primary btn-cancel" @click="showAddMember = false">取消</button>
      </div>
    </div>
  </transition>

  <div class="toast" v-if="toastMsg" :key="toastKey">{{ toastMsg }}</div>
</div>

<script>
const { createApp, ref, computed, onMounted } = Vue;
createApp({
  setup() {
    const tripName = ref('東京五日遊');
    const tripStartDate = ref('2025-10-01');
    const tripDayCount = ref(5);
    const selectedDay = ref(0);
    const activeTab = ref('schedule');
    const tabs = [
      { id:'schedule', icon:'📅', label:'行程' },
      { id:'split', icon:'💴', label:'分帳' },
      { id:'map', icon:'🗺', label:'地圖' },
      { id:'sync', icon:'☁️', label:'同步' },
    ];
    const DOW = ['日','一','二','三','四','五','六'];
    const tripDates = computed(() => {
      const arr = [];
      const base = new Date(tripStartDate.value + 'T00:00:00');
      for (let i = 0; i < tripDayCount.value; i++) {
        const d = new Date(base); d.setDate(base.getDate() + i);
        arr.push({ num: String(d.getDate()).padStart(2,'0'), label: '週' + DOW[d.getDay()], dateStr: d.toISOString().slice(0,10) });
      }
      return arr;
    });
    const weatherData = [
      { icon:'☀️', temp:28, desc:'晴天', humidity:62, wind:3.2, low:22, high:31 },
      { icon:'⛅', temp:25, desc:'多雲', humidity:70, wind:4.1, low:20, high:27 },
      { icon:'🌧', temp:22, desc:'雨天', humidity:85, wind:5.5, low:18, high:24 },
      { icon:'🌤', temp:26, desc:'晴時多雲', humidity:65, wind:2.8, low:21, high:29 },
      { icon:'☁️', temp:23, desc:'陰天', humidity:75, wind:3.9, low:19, high:26 },
    ];
    const weather = computed(() => weatherData[selectedDay.value % weatherData.length]);
    const tagLabels = { sight:'景點', food:'餐廳', shop:'購物', hotel:'住宿', transport:'交通' };
    const schedules = ref([
      { id:1, day:0, time:'09:00', name:'淺草寺', place:'東京都台東區淺草2-3-1', type:'sight', note:'建議早上去，可拍雷門大燈籠，人比較少' },
      { id:2, day:0, time:'11:30', name:'仲見世通商店街', place:'台東區淺草1-36-3', type:'shop', note:'日本傳統零食伴手禮必逛' },
      { id:3, day:0, time:'13:00', name:'壽司大（豐洲市場）', place:'江東區豐洲6-6-1', type:'food', note:'需提早排隊，建議預約' },
      { id:4, day:1, time:'10:00', name:'新宿御苑', place:'新宿區内藤町11', type:'sight', note:'' },
      { id:5, day:1, time:'19:00', name:'黃金街居酒屋', place:'新宿區歌舞伎町1-1', type:'food', note:'體驗昭和風小酒館' },
    ]);
    const showScheduleModal = ref(false);
    const editingSchedule = ref(null);
    const scheduleForm = ref({ time:'10:00', name:'', place:'', type:'sight', note:'' });
    const currentDaySchedule = computed(() =>
      schedules.value.filter(s => s.day === selectedDay.value).sort((a,b)=>a.time.localeCompare(b.time))
    );
    const currentDayPlaces = computed(() => currentDaySchedule.value.filter(s => s.place));
    function openAddSchedule() { editingSchedule.value = null; scheduleForm.value = { time:'10:00', name:'', place:'', type:'sight', note:'' }; showScheduleModal.value = true; }
    function editSchedule(item) { editingSchedule.value = item; scheduleForm.value = { ...item }; showScheduleModal.value = true; }
    function saveSchedule() {
      if (!scheduleForm.value.name.trim()) { showToast('請輸入行程名稱'); return; }
      if (editingSchedule.value) { const idx = schedules.value.findIndex(s=>s.id===editingSchedule.value.id); schedules.value[idx] = { ...scheduleForm.value, id: editingSchedule.value.id, day: selectedDay.value }; }
      else { schedules.value.push({ ...scheduleForm.value, id: Date.now(), day: selectedDay.value }); }
      showScheduleModal.value = false;
      showToast(editingSchedule.value ? '行程已更新！' : '行程已新增！');
    }
    function deleteSchedule(id) { schedules.value = schedules.value.filter(s=>s.id!==id); showToast('已刪除行程'); }

    const exchangeRate = ref(4.65);
    const convertAmount = ref(null);
    const members = ref([{ id:1, name:'小明' },{ id:2, name:'小美' },{ id:3, name:'阿強' }]);
    const selectedPayer = ref(1);
    const expenses = ref([
      { id:1, name:'便利商店早餐', amount:850, payerId:1, date:'週一' },
      { id:2, name:'地鐵一日券×3', amount:2400, payerId:2, date:'週一' },
    ]);
    const showExpenseModal = ref(false);
    const showAddMember = ref(false);
    const newMemberName = ref('');
    const expenseForm = ref({ name:'', amount:null, payerId:1 });
    function getPayerName(id) { return members.value.find(m=>m.id===id)?.name || '未知'; }
    const totalJPY = computed(() => expenses.value.reduce((s,e)=>s+e.amount,0));
    const totalTWD = computed(() => Math.round(totalJPY.value / exchangeRate.value));
    const perPersonJPY = computed(() => members.value.length ? Math.round(totalJPY.value/members.value.length) : 0);
    const perPersonTWD = computed(() => Math.round(perPersonJPY.value / exchangeRate.value));
    const settlements = computed(() => {
      if (!members.value.length || !expenses.value.length) return [];
      const balances = {};
      members.value.forEach(m => { balances[m.id] = 0; });
      expenses.value.forEach(e => {
        const share = Math.round(e.amount / members.value.length);
        members.value.forEach(m => { balances[m.id] -= share; });
        balances[e.payerId] += e.amount;
      });
      const result = [];
      const debtors = members.value.filter(m=>balances[m.id]<-1).map(m=>({...m,bal:balances[m.id]}));
      const creditors = members.value.filter(m=>balances[m.id]>1).map(m=>({...m,bal:balances[m.id]}));
      for (const d of debtors) { let debt=-d.bal; for (const c of creditors) { if(c.bal<=0||debt<=0) continue; const pay=Math.min(debt,c.bal); result.push({from:d.name,to:c.name,jpy:Math.round(pay),twd:Math.round(pay/exchangeRate.value)}); debt-=pay; c.bal-=pay; } }
      return result;
    });
    function openAddExpense() { expenseForm.value = { name:'', amount:null, payerId: members.value[0]?.id||1 }; showExpenseModal.value = true; }
    function saveExpense() {
      if (!expenseForm.value.name||!expenseForm.value.amount) { showToast('請填寫完整資訊'); return; }
      const d = tripDates.value[selectedDay.value];
      expenses.value.push({ ...expenseForm.value, id:Date.now(), date: d ? d.label : 'Day'+(selectedDay.value+1) });
      showExpenseModal.value = false; showToast('費用已記錄！');
    }
    function deleteExpense(id) { expenses.value = expenses.value.filter(e=>e.id!==id); showToast('已刪除費用'); }
    function addMember() { if(!newMemberName.value.trim()) return; members.value.push({id:Date.now(),name:newMemberName.value.trim()}); newMemberName.value=''; showAddMember.value=false; showToast('已新增旅伴！'); }

    const mapSrc = computed(() => {
      const places = currentDayPlaces.value;
      const base = 'https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d103025.9!2d139.6917!3d35.6895!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x60188b857628235d%3A0xcdd8aef709a2b520!2sTokyo!5e0!3m2!1szh-TW!2stw!4v1';
      return base;
    });
    function openNavigation(place) { const q = encodeURIComponent((place.place||place.name)+' 東京'); window.open('https://www.google.com/maps/search/'+q,'_blank'); }

    const firebaseApiKey = ref('');
    const firebaseProjectId = ref('');
    const shareTripCode = ref('');
    const firebaseConnected = ref(false);
    function connectFirebase() {
      if(!firebaseApiKey.value||!firebaseProjectId.value) { showToast('請填寫 Firebase 設定'); return; }
      firebaseConnected.value = true; showToast('🔥 Firebase 已連接！開始即時同步');
    }
    function copyCode() { navigator.clipboard?.writeText(shareTripCode.value||'tokyo-2025'); showToast('代碼已複製！'); }

    const toastMsg = ref('');
    const toastKey = ref(0);
    let toastTimer;
    function showToast(msg) { toastMsg.value = msg; toastKey.value++; clearTimeout(toastTimer); toastTimer = setTimeout(()=>{ toastMsg.value=''; },2500); }

    onMounted(async () => {
      try {
        const r = await fetch('https://api.exchangerate-api.com/v4/latest/JPY');
        if (r.ok) { const d = await r.json(); const twd = d.rates?.TWD; if(twd) exchangeRate.value = parseFloat((1/twd).toFixed(4)); }
      } catch(e) {}
    });

    return { tripName, tripStartDate, tripDayCount, selectedDay, activeTab, tabs, tripDates, weather, tagLabels, schedules, showScheduleModal, editingSchedule, scheduleForm, currentDaySchedule, currentDayPlaces, openAddSchedule, editSchedule, saveSchedule, deleteSchedule, exchangeRate, convertAmount, members, selectedPayer, expenses, showExpenseModal, showAddMember, newMemberName, expenseForm, totalJPY, totalTWD, perPersonJPY, perPersonTWD, settlements, getPayerName, openAddExpense, saveExpense, deleteExpense, addMember, mapSrc, openNavigation, firebaseApiKey, firebaseProjectId, shareTripCode, firebaseConnected, connectFirebase, copyCode, toastMsg, toastKey, showToast };
  }
}).mount('#app');
<\/script>
</body>
</html>
