<!doctype html>
<html lang="zh-CN">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>即时兑换汇率对比（多链版）</title>
<style>
  body { font-family: -apple-system, "Helvetica Neue", Arial; margin: 12px; }
  input, button, select { font-size: 15px; padding: 4px; border-radius: 4px; border: 1px solid #ccc; }
  table { width: 100%; border-collapse: collapse; margin-top: 10px; }
  th, td { border-bottom: 1px solid #ddd; padding: 8px; text-align:left; }
  .ok { color: green; } .err { color: #c00; }
  .small { font-size: 13px; color: #666; }
  .adapter-item { border: 1px solid #eee; padding: 8px; margin: 6px 0; border-radius: 4px; background: #f9f9f9; }
  .section-title { margin-top: 20px; border-bottom: 2px solid #555; padding-bottom: 5px; }
  .config-area { border:1px solid #aaa;padding:8px;margin-top:16px;background:#f5f5f5;border-radius:4px; }
  .summary { margin-top: 15px; padding: 10px; border: 1px solid #eee; border-radius: 4px; background: #f0f8ff; }
  .summary b { font-size: 1.1em; }
</style>
</head>
<body>
<h2>即时兑换汇率对比（多链版）</h2>

<div>
  <label>币种From:<input id="from" value="BTC" style="width:80px"></label>
  <label>链From:<input id="fromNetwork" value="ETH" style="width:80px"></label>
  <label>币种To:<input id="to" value="USDT" style="width:80px"></label>
  <label>链To:<input id="toNetwork" value="SOL" style="width:80px"></label>
  <button id="compare">一键比价</button>
</div>

<div id="status" class="small" style="margin:10px 0;">尚未查询</div>

<h4 style="color:#007bff;">浮动汇率</h4>
<table id="floatingTable"><thead><tr><th>交易所</th><th>汇率</th><th>预估输出量</th><th>操作</th></tr></thead><tbody></tbody></table>

<div id="floatingSummary" class="summary" style="display:none;"></div>

<h4 style="color:#28a745;">锁定汇率</h4>
<table id="lockedTable"><thead><tr><th>交易所</th><th>汇率</th><th>预估输出量</th><th>操作</th></tr></thead><tbody></tbody></table>

<div id="lockedSummary" class="summary" style="display:none;"></div>

<div class="config-area">
  <h3 class="section-title">交易所适配器管理（需密码）</h3>
  <div>
    <input id="adminPass" type="password" placeholder="输入管理密码" style="width:140px;">
    <button id="loadFromKV">从后端加载配置</button>
    <button id="saveToKV">保存到后端</button>
    <button id="addAdapter">新增交易所</button>
  </div>
  <div class="small">可添加或删除自定义交易所 API。配置将保存到 Cloudflare Worker KV 中。</div>
  <div id="adapterList"></div>
</div>

<script>
const WORKER_URL = 'YOUR_WORKER_URL_HERE'; 
let adapters = [];
let queryPair = {}; // 用于存储当前的查询币对和链名

function escapeHtml(s){ 
  return s ? s.replace(/&/g, '&amp;')
             .replace(/"/g,'&quot;')
             .replace(/</g,'&lt;')
             .replace(/>/g,'&gt;')
             .replace(/'/g, '&#39;') 
           : ''; 
}

function renderAdapters() {
  const container = document.getElementById('adapterList');
  container.innerHTML = '';
  adapters.forEach((a, i) => {
    const mappingsJson = JSON.stringify(a.mappings || {}, null, 2);
    
    const div = document.createElement('div');
    div.className = 'adapter-item';
    div.innerHTML = `
      <b>${escapeHtml(a.name || ('适配器 ' + (i+1)))}</b><br>
      <div class="small">URL模板: <input data-i="${i}" class="url" value="${escapeHtml(a.urlTemplate||'')}" style="width:90%"></div>
      <div class="small">链接模板: <input data-i="${i}" class="linkTemplate" value="${escapeHtml(a.linkTemplate||'')}" style="width:90%"></div>
      <div class="small">
        <label>类型:
          <select data-i="${i}" class="rateType">
            <option value="floating" ${a.rateType==='floating'?'selected':''}>浮动</option>
            <option value="locked" ${a.rateType==='locked'?'selected':''}>锁定</option>
          </select>
        </label>
      </div>
      <label>解析类型:
        <select data-i="${i}" class="parseType">
          <option value="json" ${a.parseType==='json'?'selected':''}>json</option>
          <option value="html_regex" ${a.parseType==='html_regex'?'selected':''}>html_regex</option>
        </select>
      </label>
      <label>路径/Regex: <input data-i="${i}" class="parsePath" value="${escapeHtml(a.parsePath||'')}" style="width:60%"></label><br>
      <div class="small">
        <label>币种/链名映射 (Mappings): <textarea data-i="${i}" class="mappings" style="width:90%; height: 80px;" placeholder='{"BTC": "XBT", "ETH": "ETHEREUM"}'></textarea></label>
      </div>
      <button data-i="${i}" class="del">删除</button>
    `;
    div.querySelector('.mappings').value = mappingsJson;
    container.appendChild(div);
  });

  const update = (e,p)=>{
    const i = Number(e.target.dataset.i);
    if (p === 'mappings') {
      try {
        adapters[i][p] = JSON.parse(e.target.value);
      } catch(err) {
        alert('映射配置格式错误，必须是有效的 JSON 对象: ' + err.message);
        return;
      }
    } else {
      adapters[i][p] = e.target.value;
    }
  };
  
  container.querySelectorAll('.url').forEach(inp=>inp.onchange=e=>update(e,'urlTemplate'));
  container.querySelectorAll('.linkTemplate').forEach(inp=>inp.onchange=e=>update(e,'linkTemplate'));
  container.querySelectorAll('.rateType').forEach(sel=>sel.onchange=e=>update(e,'rateType'));
  container.querySelectorAll('.parseType').forEach(sel=>sel.onchange=e=>update(e,'parseType'));
  container.querySelectorAll('.parsePath').forEach(inp=>inp.onchange=e=>update(e,'parsePath'));
  container.querySelectorAll('.mappings').forEach(ta=>ta.onchange=e=>update(e,'mappings'));
  container.querySelectorAll('.del').forEach(btn=>btn.onclick=e=>{adapters.splice(Number(e.target.dataset.i),1);renderAdapters();});
}

document.getElementById('addAdapter').onclick = () => {
  adapters.push({ 
    name:'新交易所', 
    urlTemplate:'https://api.example.com?from={{from}}&to={{to}}&fromChain={{fromNetwork}}&toChain={{toNetwork}}', 
    method:'GET', 
    parseType:'json', 
    parsePath:'data.rate', 
    linkTemplate:'https://example.com/{{from}}-{{to}}/{{fromNetwork}}-{{toNetwork}}', 
    rateType:'floating',
    mappings: {}
  });
  renderAdapters();
};

document.getElementById('loadFromKV').onclick = async () => {
  const pass = document.getElementById('adminPass').value.trim();
  if (!pass) return alert('请输入管理密码');
  try {
    // 改为 POST，避免将密码放在 URL
    const res = await fetch(WORKER_URL + '/getConfig', {
      method: 'POST',
      headers: { 'content-type': 'application/json' },
      body: JSON.stringify({ pass })
    });
    if (!res.ok) {
      const txt = await res.text();
      throw new Error(txt || ('HTTP ' + res.status));
    }
    const json = await res.json();
    adapters = json.adapters || [];
    alert('已从后端加载配置');
    renderAdapters();
  } catch (err) {
    alert('加载失败：' + err.message);
  } finally {
    // 成功或失败后立即清空密码输入框，降低泄露风险
    try { document.getElementById('adminPass').value = ''; } catch(e){}
  }
};

document.getElementById('saveToKV').onclick = async () => {
  const pass = document.getElementById('adminPass').value.trim();
  if (!pass) return alert('请输入管理密码');
  try {
    const res = await fetch(WORKER_URL + '/saveConfig', {
      method: 'POST',
      headers: { 'content-type': 'application/json' },
      body: JSON.stringify({ pass, adapters })
    });
    const txt = await res.text();
    if (!res.ok) throw new Error(txt || ('HTTP ' + res.status));
    alert(txt);
  } catch (err) {
    alert('保存失败：' + err.message);
  } finally {
    // 成功或失败后立即清空密码输入框，降低泄露风险
    try { document.getElementById('adminPass').value = ''; } catch(e){}
  }
};

document.getElementById('compare').onclick = async ()=>{
  const from=document.getElementById('from').value.trim().toUpperCase();
  const to=document.getElementById('to').value.trim().toUpperCase();
  const fromNetwork=document.getElementById('fromNetwork').value.trim().toUpperCase();
  const toNetwork=document.getElementById('toNetwork').value.trim().toUpperCase();
  if(!from||!to)return alert('请输入币种');
  
  queryPair = { from, to, fromNetwork, toNetwork }; 

  document.getElementById('status').textContent='查询中...';
  document.querySelector('#floatingTable tbody').innerHTML='';
  document.querySelector('#lockedTable tbody').innerHTML='';
  document.getElementById('floatingSummary').style.display='none';
  document.getElementById('lockedSummary').style.display='none';

  try{
    const res=await fetch(WORKER_URL, {
      method:'POST', headers:{'content-type':'application/json'},
      body: JSON.stringify({ pair: queryPair, adapters })
    });
    if (!res.ok) throw new Error(`HTTP 错误: ${res.status}`);
    const results=await res.json();
    showResults(results);
    document.getElementById('status').textContent=`查询完成，共 ${results.length} 条结果`;
  }catch(e){
    document.getElementById('status').textContent='失败: '+e.message;
  }
};

function showResults(results){
  const validResults = results.filter(r => r.ok && r.rate != null);

  const floating = results.filter(r=>r.rateType==='floating');
  const locked = results.filter(r=>r.rateType==='locked');
  
  const sortFn=(a,b)=>{
    if(a.rate===null&&b.rate===null)return 0;
    if(a.rate===null)return 1;if(b.rate===null)return -1;return b.rate-a.rate;
  };
  
  floating.sort(sortFn);
  locked.sort(sortFn);
  
  renderTable(floating,'#floatingTable tbody');
  renderTable(locked,'#lockedTable tbody');
  
  renderSummary(floating.filter(r => r.ok && r.rate != null), '#floatingSummary');
  renderSummary(locked.filter(r => r.ok && r.rate != null), '#lockedSummary');
}

// 修复1：简化 outputDisplay，移除非法语法
// 修复2：添加 const tr 声明
// 修复4：统一使用后端 outputAmount
// 同时修复 XSS/链接注入：不使用 innerHTML 拼接，使用 DOM API 并校验 URL 协议，仅允许 http/https
function renderTable(data,selector){
  const tbody=document.querySelector(selector);tbody.innerHTML='';
  if(!data.length){tbody.innerHTML='<tr><td colspan=4 style="text-align:center;color:#666;">无数据</td></tr>';return;}
  data.forEach((r,i)=>{
    const best=i===0&&r.ok&&r.rate!=null;
    // 输出量（保持原逻辑，使用后端 outputAmount）
    const outputDisplay = r.ok && r.outputAmount != null 
                          ? Number(r.outputAmount).toFixed(6) + ' ' + queryPair.to
                          : '-';

    const tr = document.createElement('tr');

    // 名称单元（安全）
    const tdName = document.createElement('td');
    tdName.textContent = r.name || '-';

    // 汇率单元（高亮最优）
    const tdRate = document.createElement('td');
    if (r.ok && r.rate != null) {
      if (best) {
        const b = document.createElement('b');
        b.style.color = 'red';
        b.textContent = String(r.rate);
        tdRate.appendChild(b);
      } else {
        tdRate.textContent = String(r.rate);
      }
    } else {
      const spanErr = document.createElement('span');
      spanErr.className = 'err';
      spanErr.textContent = r.error || '无';
      tdRate.appendChild(spanErr);
    }

    // 输出量单元
    const tdOutput = document.createElement('td');
    tdOutput.textContent = outputDisplay;

    // 操作单元（跳转），对 URL 进行严格校验（仅允许 http/https），避免 javascript: 等注入
    const tdOp = document.createElement('td');
    if (r.ok && r.link) {
      let safeHref = '#';
      try {
        const u = new URL(r.link);
        if (u.protocol === 'http:' || u.protocol === 'https:') {
          safeHref = u.toString();
        } else {
          safeHref = '#';
        }
      } catch (e) {
        safeHref = '#';
      }
      if (safeHref !== '#') {
        const a = document.createElement('a');
        a.href = safeHref;
        a.target = '_blank';
        a.rel = 'noopener noreferrer';
        a.textContent = '前往';
        tdOp.appendChild(a);
      } else {
        tdOp.textContent = '-';
      }
    } else {
      tdOp.textContent = '-';
    }

    tr.appendChild(tdName);
    tr.appendChild(tdRate);
    tr.appendChild(tdOutput);
    tr.appendChild(tdOp);
    tbody.appendChild(tr);
  });
}

// 修复3：data 已在前方 sortFn 排序，data[0] 即最优
function renderSummary(data, selector) {
    const summaryDiv = document.querySelector(selector);
    
    if (data.length === 0) {
        summaryDiv.style.display = 'none';
        return;
    }
    
    summaryDiv.style.display = 'block';
    
    const rates = data.map(r => r.rate);
    const sumRate = rates.reduce((a, b) => a + b, 0);
    const avgRate = sumRate / rates.length;
    
    const bestRateResult = data[0]; 
    const bestRate = bestRateResult.rate;
    const bestOutput = bestRateResult.outputAmount.toFixed(6);

    const fromCoin = queryPair.from;
    const toCoin = queryPair.to;
    
    summaryDiv.innerHTML = `
        <p>输入量: <b>1 ${fromCoin}-${queryPair.fromNetwork}</b></p>
        <p>最优汇率 (${bestRateResult.name}): <b>${bestRate}</b> 
           (预估输出: <b>${bestOutput} ${toCoin}</b>)</p>
        <p>平均汇率: <b>${avgRate.toFixed(6)}</b> 
           (预估输出: <b>${(avgRate * 1).toFixed(6)} ${toCoin}</b>)</p>
    `;
}
</script>
</body>
</html>
