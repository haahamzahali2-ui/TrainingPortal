/* ── CREDENTIALS ── */
let SHEET_ID=‘1DNEHBjPYQIcxwWCYdjty7M6Dp2-zdLmjkSpcVLG0wNw’;
let API_KEY=‘AIzaSyCv9F_LlqtyLP8m-Xtv7GBRxSi6jSa_20k’;
let CLIENT_ID=’’; // ← paste your OAuth2 Client ID here

const SH_BP=‘BP Visits’,SH_A1C=‘A1C Visits’,SH_LABS=‘Labs & Meds’,SH_INFO=‘Patient Info’;
const SCOPES=‘https://www.googleapis.com/auth/spreadsheets’;
const DISC=‘https://sheets.googleapis.com/$discovery/rest?version=v4’;
const HDR={
‘BP Visits’:  [‘Date’,‘Patient_ID’,‘Systolic’,‘Diastolic’,‘Heart_Rate’,‘Weight_lbs’,‘O2_Sat’,‘Pain_Score’,‘BP_Reading’,‘Classification’,‘Controlled’,‘Crisis’],
‘A1C Visits’: [‘Date’,‘Patient_ID’,‘HbA1c_pct’,‘Fasting_Glucose_mgdL’,‘Goal_Status’,‘Notes’],
‘Labs & Meds’:[‘Date’,‘Patient_ID’,‘Record_Type’,‘Name’,‘Value_Dose’,‘Unit’,‘Category_Frequency’,‘Route’,‘Ref_Low’,‘Ref_High’,‘Flag’],
‘Patient Info’:[‘Patient_ID’,‘Condition_Tag’,‘Date_Added’,‘Notes’],
};
let allBP=[],allA1C=[],allLabs=[],allInfo=[];
let patients=[];
let activePtId=null;
let isSignedIn=false,tokenClient=null,headersWritten={};
let anCharts={},detCharts={};
const TK=‘ct_tok_v5’;

/* ── AUTH — persistent via sessionStorage ── */
function gapiLoaded(){
gapi.load(‘client’,async()=>{
if(!API_KEY)return;
await gapi.client.init({apiKey:API_KEY,discoveryDocs:[DISC]});
const saved=tryLoad();
if(saved){gapi.client.setToken(saved);isSignedIn=true;setAuthUI(true);}
loadAll();
});
}
function gisLoaded(){if(CLIENT_ID)initTC();}
function initTC(){
if(!CLIENT_ID||typeof google===‘undefined’)return;
tokenClient=google.accounts.oauth2.initTokenClient({
client_id:CLIENT_ID,scope:SCOPES,
callback:r=>{if(!r.error){isSignedIn=true;trySave(gapi.client.getToken());setAuthUI(true);toast(‘Signed in — write enabled’);}}
});
}
function handleAuth(){
if(!CLIENT_ID){openDrawer();toast(‘Add OAuth2 Client ID in Settings’,‘err’);return;}
if(isSignedIn){
try{google.accounts.oauth2.revoke(gapi.client.getToken()?.access_token,()=>{});}catch(e){}
gapi.client.setToken(null);clearTok();isSignedIn=false;setAuthUI(false);toast(‘Signed out’);
}else{if(!tokenClient)initTC();tokenClient.requestAccessToken({prompt:’’});}
}
function setAuthUI(on){
const b=document.getElementById(‘auth-btn’),d=document.getElementById(‘cdot’),l=document.getElementById(‘clbl’);
if(on){b.textContent=‘Sign out’;b.classList.add(‘live’);d.classList.add(‘live’);l.textContent=‘write enabled’;}
else{b.textContent=‘Sign in to save’;b.classList.remove(‘live’);d.classList.remove(‘live’);l.textContent=SHEET_ID?‘read-only’:‘not connected’;}
}
function trySave(t){try{sessionStorage.setItem(TK,JSON.stringify(t));}catch(e){}}
function tryLoad(){try{const t=sessionStorage.getItem(TK);return t?JSON.parse(t):null;}catch(e){return null;}}
function clearTok(){try{sessionStorage.removeItem(TK);}catch(e){}}

/* ── DATA ── */
async function fetchSheet(tab,range){
if(!SHEET_ID||!API_KEY)return[];
const url=`https://sheets.googleapis.com/v4/spreadsheets/${SHEET_ID}/values/${encodeURIComponent(tab+'!'+range)}?key=${API_KEY}`;
const r=await fetch(url);const d=await r.json();
if(d.error)throw new Error(d.error.message+’ — sheet: “’+tab+’”’);
const rows=d.values||[];if(!rows.length)return[];
const h=rows[0];
return rows.slice(1).filter(r=>r.some(c=>(c||’’).trim())).map(r=>{const o={};h.forEach((k,i)=>o[(k||’’).trim()]=(r[i]||’’));return o;});
}
async function loadAll(){
if(!SHEET_ID||!API_KEY)return;
try{
[allBP,allA1C,allLabs,allInfo]=await Promise.all([
fetchSheet(SH_BP,‘A1:L2000’),fetchSheet(SH_A1C,‘A1:F2000’),
fetchSheet(SH_LABS,‘A1:K2000’),fetchSheet(SH_INFO,‘A1:D2000’),
]);
buildPtList();renderPtList();
document.getElementById(‘cdot’).classList.add(‘live’);
document.getElementById(‘clbl’).textContent=isSignedIn?‘write enabled’:‘read-only’;
if(activePtId)selectPt(activePtId);
if(document.getElementById(‘page-analytics’).classList.contains(‘active’))renderAnalytics();
}catch(e){toast(e.message,‘err’);}
}

/* ── PATIENT LIST ── */
function buildPtList(){
const seen=new Map();
allInfo.forEach(r=>{
const pid=(r[‘Patient_ID’]||’’).trim();if(!pid)return;
seen.set(pid,{ptId:pid,condTag:r[‘Condition_Tag’]||’’,dateAdded:r[‘Date_Added’]||’’,notes:r[‘Notes’]||’’,lastVisit:’’});
});
const touch=(arr,dk,pk)=>arr.forEach(r=>{
const pid=(r[pk]||’’).trim(),dt=pd(r[dk]||’’);if(!pid)return;
if(!seen.has(pid))seen.set(pid,{ptId:pid,condTag:’’,dateAdded:dt,notes:’’,lastVisit:dt});
else{const e=seen.get(pid);if(dt>(e.lastVisit||’’))e.lastVisit=dt;if(!e.dateAdded)e.dateAdded=dt;}
});
touch(allBP,‘Date’,‘Patient_ID’);touch(allA1C,‘Date’,‘Patient_ID’);touch(allLabs,‘Date’,‘Patient_ID’);
patients.forEach(p=>{if(!seen.has(p.ptId))seen.set(p.ptId,p);});
patients=[…seen.values()].sort((a,b)=>{
const da=a.lastVisit||a.dateAdded||’’,db=b.lastVisit||b.dateAdded||’’;return db.localeCompare(da);
});
}
function renderPtList(){
const q=(document.getElementById(‘pt-search’)?.value||’’).toLowerCase().trim();
let list=patients.filter(p=>!q||p.ptId.toLowerCase().includes(q));
if(!list.length){
document.getElementById(‘pt-list-body’).innerHTML=`<div class="pt-empty"><div class="pt-empty-t">${q?'No results':'No patients yet'}</div><div style="font-size:12px;color:var(--ink3)">${q?'Try a different ID':'Click "Add New Patient" to get started'}</div></div>`;
return;
}
document.getElementById(‘pt-list-body’).innerHTML=
`<div class="pt-list-lbl">${list.length} patient${list.length!==1?'s':''}</div>`+
list.map(p=>{
const lb=latest(allBP,‘Patient_ID’,p.ptId),la=latest(allA1C,‘Patient_ID’,p.ptId);
const hasData=lb||la;
const dot=!hasData?‘new’:bpDotCls(lb);
const meta=[];
if(lb)meta.push(lb[‘BP_Reading’]||lb[‘Systolic’]+’/’+lb[‘Diastolic’]);
if(la)meta.push(‘A1C ‘+la[‘HbA1c_pct’]+’%’);
if(p.condTag)meta.push(p.condTag);
return `<div class="pt-row${activePtId===p.ptId?' active':''}" onclick="selectPt('${p.ptId}')"> <div class="pt-dot ${dot}"></div> <div style="flex:1;min-width:0"><div class="pt-row-id">${p.ptId}</div><div class="pt-row-meta">${meta.join(' · ')||'No visits logged'}</div></div> <div class="pt-row-right">${!hasData?'<span class="new-badge">New</span>':''}</div> </div>`;
}).join(’’);
}
function bpDotCls(r){if(!r)return’new’;if((r[‘Crisis’]||’’).toLowerCase().includes(‘y’))return’crit’;const s=parseInt(r[‘Systolic’]||0);return s>=140?‘warn’:s>0?‘ok’:‘new’;}

/* ── SELECT PATIENT ── */
function selectPt(pid){
activePtId=pid;renderPtList();
const p=patients.find(x=>x.ptId===pid)||{ptId:pid,condTag:’’,dateAdded:’’,notes:’’};
const lb=latest(allBP,‘Patient_ID’,pid);
const sys=lb?parseInt(lb[‘Systolic’]||0):null;
const cr=lb&&(lb[‘Crisis’]||’’).toLowerCase().includes(‘y’);
let abCls=‘ab-nodata’,abIcon=icoInfo(),abMsg=‘No visits recorded yet’;
if(lb){
if(cr){abCls=‘ab-crisis’;abIcon=icoWarn();abMsg=‘⚠ BP Crisis — ‘+lb[‘BP_Reading’]+’ — Immediate attention required’;}
else if(sys>=140){abCls=‘ab-stage2’;abIcon=icoWarn();abMsg=‘Stage 2 Hypertension — ‘+lb[‘BP_Reading’]+’ — Uncontrolled’;}
else if(sys>=130){abCls=‘ab-stage1’;abIcon=icoWarn();abMsg=‘Stage 1 Hypertension — ‘+lb[‘BP_Reading’]+’ — Monitor closely’;}
else if(sys>=120){abCls=‘ab-stage1’;abIcon=icoInfo();abMsg=’Elevated BP — ’+lb[‘BP_Reading’];}
else if(sys>0){abCls=‘ab-controlled’;abIcon=icoCheck();abMsg=’Blood pressure controlled — ’+lb[‘BP_Reading’];}
}
document.getElementById(‘pt-detail’).innerHTML=` <div class="pt-hd"> <div class="pt-hd-av">${pid.toString().slice(-4)}</div> <div><div class="pt-hd-id">Patient ${pid}</div><div class="pt-hd-sub">${p.condTag||'Primary care'}${p.dateAdded?' · Added '+p.dateAdded:''}</div></div> <div class="pt-hd-right"> <button class="tab-action active" id="ta-dash" onclick="swTab('dash')"> <svg viewBox="0 0 24 24"><rect x="3" y="3" width="7" height="7" rx="1"/><rect x="14" y="3" width="7" height="7" rx="1"/><rect x="3" y="14" width="7" height="7" rx="1"/><rect x="14" y="14" width="7" height="7" rx="1"/></svg>Dashboard </button> <button class="tab-action" id="ta-log" onclick="swTab('log')"> <svg viewBox="0 0 24 24"><path d="M12 20h9"/><path d="M16.5 3.5a2.121 2.121 0 0 1 3 3L7 19l-4 1 1-4L16.5 3.5z"/></svg>Log Visit </button> </div> </div> <div class="alert-bar ${abCls}">${abIcon} ${abMsg}</div> <div class="det-panel active" id="dp-dash">${dashHTML(pid)}</div> <div class="det-panel" id="dp-log">${logHTML(pid)}</div>`;
const lbT=allLabs.filter(r=>pd(r[‘Date’])===todayISO()&&r[‘Patient_ID’]===pid&&r[‘Record_Type’]===‘Lab’);
const mdT=allLabs.filter(r=>pd(r[‘Date’])===todayISO()&&r[‘Patient_ID’]===pid&&r[‘Record_Type’]===‘Medication’);
lbT.length?lbT.forEach(r=>addLabRow(r[‘Name’],r[‘Value_Dose’],r[‘Unit’],r[‘Category_Frequency’],r[‘Ref_Low’],r[‘Ref_High’])):addLabRow();
mdT.length?mdT.forEach(r=>addMedRow(r[‘Name’],r[‘Value_Dose’],r[‘Unit’],r[‘Category_Frequency’],r[‘Route’])):addMedRow();
livePreview();
requestAnimationFrame(()=>drawDetCharts(pid));
}

/* ── DASHBOARD HTML ── */
function dashHTML(pid){
const bpH=allBP.filter(r=>r[‘Patient_ID’]===pid).sort((a,b)=>pd(a[‘Date’]).localeCompare(pd(b[‘Date’])));
const a1H=allA1C.filter(r=>r[‘Patient_ID’]===pid).sort((a,b)=>pd(a[‘Date’]).localeCompare(pd(b[‘Date’])));
const lbH=allLabs.filter(r=>r[‘Patient_ID’]===pid&&r[‘Record_Type’]===‘Lab’);
const mdH=allLabs.filter(r=>r[‘Patient_ID’]===pid&&r[‘Record_Type’]===‘Medication’);
const lb=bpH.length?bpH[bpH.length-1]:null,la=a1H.length?a1H[a1H.length-1]:null;
const fb=bpH.length?bpH[0]:null,fa=a1H.length?a1H[0]:null;
const sys=lb?parseInt(lb[‘Systolic’]||0):null,dia=lb?parseInt(lb[‘Diastolic’]||0):null;
const a1cv=la?parseFloat(la[‘HbA1c_pct’]||0):null;
const sysd=sys&&fb?Math.round(parseInt(fb[‘Systolic’]||0)-sys):null;
const a1cd=a1cv&&fa?(parseFloat(fa[‘HbA1c_pct’]||0)-a1cv).toFixed(1):null;
const bpc=!sys?’’:sys>=140?‘vr’:sys>=130?‘va’:‘vg’;
const a1cc=!a1cv?’’:a1cv<7?‘vg’:a1cv<8?‘va’:‘vr’;
return `

  <div class="stat-row">
    <div class="stat-card"><div class="stat-lbl">Latest BP</div><div class="stat-val ${bpc}">${sys&&dia?sys+'/'+dia:'—'}</div><div class="stat-sub">${lb?pd(lb['Date']):'No data'}</div>${sysd!==null?'<div class="stat-trend '+(sysd>0?'tup':'tdn')+'">'+(sysd>0?'↓ '+sysd+' mmHg improved':'↑ '+Math.abs(sysd)+' mmHg since first')+'</div>':'<div class="stat-trend tfl">First visit</div>'}</div>
    <div class="stat-card"><div class="stat-lbl">BP Classification</div><div class="stat-val" style="font-size:14px;font-weight:700;margin-top:6px;color:${!sys?'var(--ink3)':sys>=140?'var(--red)':sys>=130?'var(--amber)':'var(--green)'}">${lb?lb['Classification']||'—':'—'}</div><div class="stat-sub">${lb?lb['Controlled']||'':'—'}</div></div>
    <div class="stat-card"><div class="stat-lbl">Latest A1C</div><div class="stat-val ${a1cc}">${a1cv?a1cv.toFixed(1)+'%':'—'}</div><div class="stat-sub">${la?pd(la['Date'])+' · '+la['Goal_Status']:'No data'}</div>${a1cd!==null?'<div class="stat-trend '+(parseFloat(a1cd)>0?'tup':'tdn')+'">'+(parseFloat(a1cd)>0?'↓ '+Math.abs(a1cd)+'% improved':'↑ '+Math.abs(a1cd)+'% increased')+'</div>':'<div class="stat-trend tfl">First visit</div>'}</div>
    <div class="stat-card"><div class="stat-lbl">Total Visits</div><div class="stat-val">${Math.max(bpH.length,a1H.length)||'0'}</div><div class="stat-sub">BP: ${bpH.length} · A1C: ${a1H.length}</div></div>
  </div>
  <div class="chart-card">
    <div class="cc-hd"><span class="cc-t">Blood Pressure — Last 5 Visits</span><span class="cc-m">${bpH.length} total readings</span></div>
    <div class="cc-s">Systolic + Diastolic · Goal lines at 130/80 mmHg</div>
    ${bpH.length>=2?`<div class="cw"><canvas id="chart-bp-det"></canvas></div><div class="cc-leg"><div class="cleg"><div class="cleg-dot" style="background:#2563eb"></div>Systolic</div><div class="cleg"><div class="cleg-dot" style="background:#16a34a"></div>Diastolic</div><div class="cleg"><div class="cleg-dot" style="background:rgba(217,119,6,.5)"></div>Goal</div></div>`
    :`<div class="no-data"><svg viewBox="0 0 24 24"><polyline points="22 20 14 12 9 17 2 10"/></svg>${bpH.length===1?'Need 2+ readings to show trend':'No BP data yet'}</div>`}
  </div>
  ${a1H.length>=2?`<div class="chart-card"><div class="cc-hd"><span class="cc-t">A1C Trend</span><span class="cc-m">${a1H.length} readings</span></div><div class="cc-s">HbA1c % · Goal &lt;7%</div><div class="cw sm"><canvas id="chart-a1c-det"></canvas></div><div class="cc-leg"><div class="cleg"><div class="cleg-dot" style="background:#d97706"></div>HbA1c %</div></div></div>`:''}
  ${bpH.length?`<div class="hist-card"><div class="hist-hd"><span class="hist-t">BP History</span><span class="hist-c">${bpH.length} records</span></div><div style="overflow-x:auto"><table class="dt"><thead><tr><th>Date</th><th>BP</th><th>Sys</th><th>Dia</th><th>HR</th><th>Weight</th><th>O2</th><th>Classification</th><th>Status</th></tr></thead><tbody>${bpH.slice().reverse().map(r=>`<tr><td class="mono">${pd(r['Date'])}</td><td class="mono" style="font-weight:600">${r['BP_Reading']||r['Systolic']+'/'+r['Diastolic']}</td><td class="mono">${r['Systolic']||'—'}</td><td class="mono">${r['Diastolic']||'—'}</td><td class="mono">${r['Heart_Rate']||'—'}</td><td class="mono">${r['Weight_lbs']?r['Weight_lbs']+' lbs':'—'}</td><td class="mono">${r['O2_Sat']?r['O2_Sat']+'%':'—'}</td><td>${r['Classification']||'—'}</td><td><span class="bdg ${(r['Crisis']||'').toLowerCase().includes('y')?'br':(r['Controlled']||'').toLowerCase().includes('uncontrolled')?'ba':'bg'}">${(r['Crisis']||'').toLowerCase().includes('y')?'Crisis':r['Controlled']||'—'}</span></td></tr>`).join('')}</tbody></table></div></div>`:''}
  ${a1H.length?`<div class="hist-card"><div class="hist-hd"><span class="hist-t">A1C History</span><span class="hist-c">${a1H.length} records</span></div><div style="overflow-x:auto"><table class="dt"><thead><tr><th>Date</th><th>HbA1c %</th><th>Fasting Glucose</th><th>Goal Status</th><th>Notes</th></tr></thead><tbody>${a1H.slice().reverse().map(r=>`<tr><td class="mono">${pd(r['Date'])}</td><td class="mono" style="font-weight:600;color:${parseFloat(r['HbA1c_pct']||99)<7?'var(--green)':parseFloat(r['HbA1c_pct']||99)<8?'var(--amber)':'var(--red)'}">${r['HbA1c_pct']?r['HbA1c_pct']+'%':'—'}</td><td class="mono">${r['Fasting_Glucose_mgdL']?r['Fasting_Glucose_mgdL']+' mg/dL':'—'}</td><td><span class="bdg ${(r['Goal_Status']||'').toLowerCase().includes('at')?'bg':'ba'}">${r['Goal_Status']||'—'}</span></td><td style="font-size:12px;color:var(--ink3)">${r['Notes']||''}</td></tr>`).join('')}</tbody></table></div></div>`:''}
  ${lbH.length?`<div class="hist-card"><div class="hist-hd"><span class="hist-t">Lab Results</span><span class="hist-c">${lbH.length} results</span></div><div style="overflow-x:auto"><table class="dt"><thead><tr><th>Date</th><th>Test</th><th>Result</th><th>Unit</th><th>Category</th><th>Flag</th></tr></thead><tbody>${lbH.slice().reverse().map(r=>`<tr><td class="mono">${pd(r['Date'])}</td><td style="font-weight:500">${r['Name']||'—'}</td><td class="mono" style="font-weight:600">${r['Value_Dose']||'—'}</td><td class="mono">${r['Unit']||'—'}</td><td>${r['Category_Frequency']||'—'}</td><td>${r['Flag']?'<span class="bdg '+(r['Flag']==='ABNORMAL'?'br':'bg')+'">'+r['Flag']+'</span>':''}</td></tr>`).join('')}</tbody></table></div></div>`:''}
  ${mdH.length?`<div class="hist-card"><div class="hist-hd"><span class="hist-t">Medications</span><span class="hist-c">${mdH.length} entries</span></div><div style="overflow-x:auto"><table class="dt"><thead><tr><th>Date</th><th>Medication</th><th>Dose</th><th>Frequency</th><th>Route</th></tr></thead><tbody>${mdH.slice().reverse().map(r=>`<tr><td class="mono">${pd(r['Date'])}</td><td style="font-weight:500">${r['Name']||'—'}</td><td class="mono">${r['Value_Dose']||'—'} ${r['Unit']||''}</td><td>${r['Category_Frequency']||'—'}</td><td><span class="bdg bgy">${r['Route']||'—'}</span></td></tr>`).join('')}</tbody></table></div></div>`:''}`;
}

/* ── LOG VISIT HTML ── */
function logHTML(pid){
const bpT=allBP.filter(r=>pd(r[‘Date’])===todayISO()&&r[‘Patient_ID’]===pid);
const a1T=allA1C.filter(r=>pd(r[‘Date’])===todayISO()&&r[‘Patient_ID’]===pid);
const lbT=allLabs.filter(r=>pd(r[‘Date’])===todayISO()&&r[‘Patient_ID’]===pid&&r[‘Record_Type’]===‘Lab’);
const mdT=allLabs.filter(r=>pd(r[‘Date’])===todayISO()&&r[‘Patient_ID’]===pid&&r[‘Record_Type’]===‘Medication’);
const lb=bpT.length?bpT[bpT.length-1]:null,la=a1T.length?a1T[a1T.length-1]:null;
const p=patients.find(x=>x.ptId===pid)||{condTag:’’,notes:’’};
const sv=`<svg viewBox="0 0 24 24"><path d="M19 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11l5 5v11a2 2 0 0 1-2 2z"/><polyline points="17 21 17 13 7 13 7 21"/><polyline points="7 3 7 8 15 8"/></svg>`;
return `

  <div style="font-size:12px;color:var(--ink3);padding:9px 12px;background:var(--s2);border-radius:var(--r);border:1px solid var(--border);margin-bottom:13px">📅 Logging for today — <strong style="color:var(--ink)">${todayISO()}</strong></div>
  <div class="log-sec"><div class="log-hd open" onclick="togLS(this,'lb-info')"><div class="log-icon li-ink"><svg viewBox="0 0 24 24"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/></svg></div><div class="log-info"><div class="log-title">Patient Info</div><div class="log-sub">Condition tag and notes</div></div><div class="log-chev"><svg viewBox="0 0 24 24"><polyline points="6 9 12 15 18 9"/></svg></div></div>
  <div class="log-body open" id="lb-info"><div class="fg2 mb12"><div class="f"><label>Condition Tag</label><select id="v-condtag">${['HTN','DM','HTN + DM','Other','—'].map(c=>`<option${c===p.condTag?' selected':''}>${c}</option>`).join('')}</select></div><div class="f"><label>Notes</label><input id="v-pt-notes" placeholder="General notes…" value="${p.notes||''}"/></div></div><button class="save-btn sb-dk" onclick="saveInfo('${pid}')">${sv} Save Patient Info</button></div></div>
  <div class="log-sec"><div class="log-hd${lb?' open':''}" onclick="togLS(this,'lb-bp')"><div class="log-icon li-red"><svg viewBox="0 0 24 24"><path d="M22 12h-4l-3 9L9 3l-3 9H2"/></svg></div><div class="log-info"><div class="log-title">Blood Pressure</div><div class="log-sub">${lb?'Recorded today · '+lb['BP_Reading']:'Not recorded yet'}</div></div><div class="log-chev"><svg viewBox="0 0 24 24"><polyline points="6 9 12 15 18 9"/></svg></div></div>
  <div class="log-body${lb?' open':''}" id="lb-bp"><div class="fg3 mb12"><div class="f f-xl"><label>Systolic</label><input id="v-sys" type="number" placeholder="120" value="${lb?lb['Systolic']:''}" oninput="livePreview()"/></div><div class="f f-xl"><label>Diastolic</label><input id="v-dia" type="number" placeholder="80" value="${lb?lb['Diastolic']:''}" oninput="livePreview()"/></div><div class="f f-xl"><label>Heart Rate</label><input id="v-hr" type="number" placeholder="72" value="${lb?lb['Heart_Rate']:''}"/></div></div><div class="fg4 mb12"><div class="f"><label>Weight (lbs)</label><input id="v-wt" type="number" placeholder="165" value="${lb?lb['Weight_lbs']:''}"/></div><div class="f"><label>O2 Sat (%)</label><input id="v-o2" type="number" placeholder="98" value="${lb?lb['O2_Sat']:''}"/></div><div class="f"><label>Pain (0–10)</label><input id="v-pain" type="number" min="0" max="10" placeholder="0" value="${lb?lb['Pain_Score']:''}"/></div><div class="f"><label>&nbsp;</label></div></div>
  <div class="bp-live"><div class="bpl-it"><div class="bpl-lbl">Reading</div><div class="bpl-val" id="bpl-r">${lb?lb['BP_Reading']:'—/—'}</div><div class="bpl-sub">mmHg</div></div><div class="bpl-it"><div class="bpl-lbl">Classification</div><div class="bpl-val" id="bpl-c" style="font-size:13px;font-weight:700">${lb?lb['Classification']:'—'}</div><div class="bpl-sub" id="bpl-ctrl">${lb?lb['Controlled']:''}</div></div><div class="bpl-it"><div class="bpl-lbl">Crisis Flag</div><div class="bpl-val" id="bpl-cr" style="font-size:13px">${lb?(lb['Crisis']||'No'):'—'}</div><div class="bpl-sub">status</div></div></div>
  <button class="save-btn sb-dk" id="bp-save" onclick="saveBP('${pid}')">${sv} ${lb?'Update BP':'Save BP'}</button></div></div>
  <div class="log-sec"><div class="log-hd${la?' open':''}" onclick="togLS(this,'lb-a1c')"><div class="log-icon li-amb"><svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="9"/><path d="M8 12h8M12 8v8"/></svg></div><div class="log-info"><div class="log-title">A1C / HbA1c</div><div class="log-sub">${la?'Recorded today · '+la['HbA1c_pct']+'% · '+la['Goal_Status']:'Not recorded yet'}</div></div><div class="log-chev"><svg viewBox="0 0 24 24"><polyline points="6 9 12 15 18 9"/></svg></div></div>
  <div class="log-body${la?' open':''}" id="lb-a1c"><div class="fg2 mb12"><div class="f f-xl"><label>HbA1c (%)</label><input id="v-a1c" type="number" step="0.1" placeholder="7.2" value="${la?la['HbA1c_pct']:''}"/></div><div class="f f-xl"><label>Fasting Glucose (mg/dL)</label><input id="v-glucose" type="number" placeholder="104" value="${la?la['Fasting_Glucose_mgdL']:''}"/></div></div><div class="fg2 mb12"><div class="f"><label>Goal Status</label><select id="v-a1c-goal"><option value="At Goal"${la&&la['Goal_Status']==='At Goal'?' selected':''}>At Goal</option><option value="Above Goal"${la&&la['Goal_Status']==='Above Goal'?' selected':''}>Above Goal</option></select></div><div class="f"><label>Notes</label><input id="v-a1c-notes" placeholder="Trending down…" value="${la?la['Notes']:''}"/></div></div>
  <button class="save-btn sb-amb" id="a1c-save" onclick="saveA1C('${pid}')">${sv} ${la?'Update A1C':'Save A1C'}</button></div></div>
  <div class="log-sec"><div class="log-hd${lbT.length?' open':''}" onclick="togLS(this,'lb-labs')"><div class="log-icon li-grn"><svg viewBox="0 0 24 24"><path d="M9 3H5a2 2 0 0 0-2 2v4m6-6h10a2 2 0 0 1 2 2v4M9 3v18m0 0h10a2 2 0 0 0 2-2v-4M9 21H5a2 2 0 0 1-2-2v-4m0 0h18"/></svg></div><div class="log-info"><div class="log-title">Other Labs</div><div class="log-sub">${lbT.length?lbT.length+' result(s) today':'Optional — lipids, renal, CBC, etc.'}</div></div><div class="log-chev"><svg viewBox="0 0 24 24"><polyline points="6 9 12 15 18 9"/></svg></div></div>
  <div class="log-body${lbT.length?' open':''}" id="lb-labs"><div class="col-hd ch-lab"><span>Test Name</span><span>Result</span><span>Unit</span><span>Category</span><span>Ref Lo</span><span>Ref Hi</span><span></span></div><div id="lab-rows"></div><button class="add-row-btn" onclick="addLabRow()"><svg viewBox="0 0 24 24"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>Add test row</button><button class="save-btn sb-grn" onclick="saveLabs('${pid}')">${sv} Save Labs</button></div></div>
  <div class="log-sec" style="margin-bottom:32px"><div class="log-hd${mdT.length?' open':''}" onclick="togLS(this,'lb-meds')"><div class="log-icon li-blu"><svg viewBox="0 0 24 24"><path d="M19 14c1.49-1.46 3-3.21 3-5.5A5.5 5.5 0 0 0 16.5 3c-1.76 0-3 .5-4.5 2-1.5-1.5-2.74-2-4.5-2A5.5 5.5 0 0 0 2 8.5c0 2.3 1.5 4.05 3 5.5l7 7Z"/></svg></div><div class="log-info"><div class="log-title">Medications</div><div class="log-sub">${mdT.length?mdT.length+' medication(s) today':'Optional — new or changed meds'}</div></div><div class="log-chev"><svg viewBox="0 0 24 24"><polyline points="6 9 12 15 18 9"/></svg></div></div>
  <div class="log-body${mdT.length?' open':''}" id="lb-meds"><div class="col-hd ch-med"><span>Medication</span><span>Dose</span><span>Unit</span><span>Frequency</span><span>Route</span><span></span></div><div id="med-rows"></div><button class="add-row-btn" onclick="addMedRow()"><svg viewBox="0 0 24 24"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>Add medication</button><button class="save-btn sb-blu" onclick="saveMeds('${pid}')">${sv} Save Medications</button></div></div>`;
}

/* ── CHARTS ── */
function drawDetCharts(pid){
Object.values(detCharts).forEach(c=>{try{c.destroy();}catch(e){}});detCharts={};
const bpH=allBP.filter(r=>r[‘Patient_ID’]===pid).sort((a,b)=>pd(a[‘Date’]).localeCompare(pd(b[‘Date’])));
const a1H=allA1C.filter(r=>r[‘Patient_ID’]===pid).sort((a,b)=>pd(a[‘Date’]).localeCompare(pd(b[‘Date’])));
const last5=bpH.slice(-5);const co=mkOpts();
if(last5.length>=2&&document.getElementById(‘chart-bp-det’)){
detCharts[‘bp’]=new Chart(document.getElementById(‘chart-bp-det’),{type:‘line’,data:{labels:last5.map(r=>pd(r[‘Date’]).slice(5)),datasets:[
{label:‘Sys’,data:last5.map(r=>+r[‘Systolic’]||null),borderColor:’#2563eb’,backgroundColor:‘rgba(37,99,235,.07)’,tension:.4,pointRadius:4,pointHoverRadius:6,fill:true},
{label:‘Dia’,data:last5.map(r=>+r[‘Diastolic’]||null),borderColor:’#16a34a’,backgroundColor:‘rgba(22,163,74,.06)’,tension:.4,pointRadius:4,pointHoverRadius:6,fill:true,borderDash:[6,3]},
{label:‘GS’,data:last5.map(()=>130),borderColor:‘rgba(217,119,6,.3)’,borderDash:[4,4],pointRadius:0,borderWidth:1.5,fill:false},
{label:‘GD’,data:last5.map(()=>80),borderColor:‘rgba(22,163,74,.3)’,borderDash:[4,4],pointRadius:0,borderWidth:1.5,fill:false},
]},options:{…co,scales:{…co.scales,y:{…co.scales.y,min:40,max:220}}}});
}
if(a1H.length>=2&&document.getElementById(‘chart-a1c-det’)){
detCharts[‘a1c’]=new Chart(document.getElementById(‘chart-a1c-det’),{type:‘line’,data:{labels:a1H.map(r=>pd(r[‘Date’]).slice(5)),datasets:[
{label:‘A1C’,data:a1H.map(r=>+r[‘HbA1c_pct’]||null),borderColor:’#d97706’,backgroundColor:‘rgba(217,119,6,.08)’,tension:.4,pointRadius:4,fill:true},
{label:‘Goal’,data:a1H.map(()=>7),borderColor:‘rgba(22,163,74,.35)’,borderDash:[4,4],pointRadius:0,borderWidth:1.5,fill:false},
]},options:{…co,scales:{…co.scales,y:{…co.scales.y,min:4,max:14}}}});
}
}
function mkOpts(){
return{responsive:true,maintainAspectRatio:false,interaction:{mode:‘index’,intersect:false},
plugins:{legend:{display:false},tooltip:{backgroundColor:’#fff’,borderColor:’#e5e3dd’,borderWidth:1,titleColor:’#1a1916’,bodyColor:’#5a574f’,padding:10,boxPadding:4,titleFont:{family:‘JetBrains Mono’,size:11},bodyFont:{family:‘JetBrains Mono’,size:11}}},
scales:{y:{grid:{color:‘rgba(0,0,0,.05)’},ticks:{color:’#9a9589’,font:{family:‘JetBrains Mono’,size:10},maxTicksLimit:6}},
x:{grid:{display:false},ticks:{color:’#9a9589’,font:{family:‘JetBrains Mono’,size:10},maxRotation:40,autoSkip:true,maxTicksLimit:8}}}};
}

/* ── ANALYTICS ── */
function renderAnalytics(){
Object.values(anCharts).forEach(c=>{try{c.destroy();}catch(e){}});anCharts={};
if(!allBP.length&&!allA1C.length)return;
const ptIds=[…new Set([…allBP.map(r=>r[‘Patient_ID’]),…allA1C.map(r=>r[‘Patient_ID’])].filter(Boolean))];
const tot=ptIds.length;
const lbpp=ptIds.map(pid=>latest(allBP,‘Patient_ID’,pid)).filter(Boolean);
const ctrl=lbpp.filter(r=>(r[‘Controlled’]||’’).toLowerCase().includes(‘controlled’)&&!(r[‘Controlled’]||’’).toLowerCase().includes(‘uncontrolled’)).length;
const cris=lbpp.filter(r=>(r[‘Crisis’]||’’).toLowerCase().includes(‘y’)).length;
const s2=lbpp.filter(r=>(r[‘Classification’]||’’).includes(‘Stage 2’)).length;
const s1=lbpp.filter(r=>(r[‘Classification’]||’’).includes(‘Stage 1’)).length;
const elv=lbpp.filter(r=>(r[‘Classification’]||’’).includes(‘Elevated’)).length;
const norm=lbpp.filter(r=>(r[‘Classification’]||’’).includes(‘Normal’)).length;
const pctCtrl=tot?Math.round(ctrl/tot*100):0;
let imp=0,wors=0,unch=0,sf=0,sl=0,np=0;
ptIds.forEach(pid=>{
const rows=allBP.filter(r=>r[‘Patient_ID’]===pid).sort((a,b)=>pd(a[‘Date’]).localeCompare(pd(b[‘Date’])));
if(rows.length<2)return;
const d=parseInt(rows[0][‘Systolic’]||0)-parseInt(rows[rows.length-1][‘Systolic’]||0);
if(d>=5)imp++;else if(d<=-5)wors++;else unch++;
sf+=parseInt(rows[0][‘Systolic’]||0);sl+=parseInt(rows[rows.length-1][‘Systolic’]||0);np++;
});
const pctImp=tot?Math.round(imp/tot*100):0;
const avgF=np?Math.round(sf/np):null,avgL=np?Math.round(sl/np):null,avgD=avgF&&avgL?avgF-avgL:null;
const lapp=ptIds.map(pid=>latest(allA1C,‘Patient_ID’,pid)).filter(Boolean);
const a1cAtG=lapp.filter(r=>(r[‘Goal_Status’]||’’).toLowerCase().includes(‘at goal’)).length;
document.getElementById(‘an-content’).innerHTML=`

  <div class="an-g4">
    <div class="kpi"><div class="kpi-l">Total Patients</div><div class="kpi-v">${tot}</div><div class="kpi-s">Across all records</div></div>
    <div class="kpi"><div class="kpi-l">BP Controlled</div><div class="kpi-v vg">${pctCtrl}%</div><div class="kpi-s">${ctrl} of ${lbpp.length} with BP data</div><div class="kpi-bar"><div class="kpi-fill" style="width:${pctCtrl}%;background:var(--green)"></div></div></div>
    <div class="kpi"><div class="kpi-l">BP Improved ≥5 mmHg</div><div class="kpi-v vg">${pctImp}%</div><div class="kpi-s">${imp} patients (first → latest)</div><div class="kpi-bar"><div class="kpi-fill" style="width:${pctImp}%;background:var(--green)"></div></div></div>
    <div class="kpi"><div class="kpi-l">BP Crisis (active)</div><div class="kpi-v ${cris>0?'vr':''}">${cris}</div><div class="kpi-s">Most recent visit</div></div>
  </div>
  <div class="an-g3">
    <div class="kpi"><div class="kpi-l">Avg Systolic — First Visit</div><div class="kpi-v">${avgF||'—'}</div><div class="kpi-s">${np} patients with 2+ readings</div></div>
    <div class="kpi"><div class="kpi-l">Avg Systolic — Latest Visit</div><div class="kpi-v ${avgD&&avgD>0?'vg':avgD&&avgD<0?'vr':''}">${avgL||'—'}</div><div class="kpi-s">${avgD!==null?(avgD>0?'↓ '+avgD+' mmHg avg improvement':'↑ '+Math.abs(avgD)+' mmHg avg increase'):'—'}</div></div>
    <div class="kpi"><div class="kpi-l">A1C At Goal (&lt;7%)</div><div class="kpi-v vg">${lapp.length?Math.round(a1cAtG/lapp.length*100):0}%</div><div class="kpi-s">${a1cAtG} of ${lapp.length} with A1C data</div></div>
  </div>
  <div class="an-g2">
    <div class="an-chart"><div class="an-ct">BP Classification Distribution</div><div class="an-cs">Most recent visit per patient</div><div class="an-w"><canvas id="an-bpcls"></canvas></div></div>
    <div class="an-chart"><div class="an-ct">BP Outcomes: First vs Latest</div><div class="an-cs">Patients with ≥2 readings (n=${np})</div><div class="an-w"><canvas id="an-bpout"></canvas></div></div>
  </div>
  <div class="an-g2">
    <div class="an-chart"><div class="an-ct">Avg Systolic — Population Level</div><div class="an-cs">First vs most recent visit</div><div class="an-w"><canvas id="an-avgsys"></canvas></div></div>
    <div class="an-chart"><div class="an-ct">A1C Goal Status</div><div class="an-cs">Latest visit per patient</div><div class="an-w"><canvas id="an-a1cg"></canvas></div></div>
  </div>
  <div class="donor"><div class="donor-title">Donor &amp; Leadership Summary</div>
    <div class="ds"><div class="ds-num vg">${pctCtrl}%</div><div class="ds-desc">of patients have controlled blood pressure (&lt;130 mmHg) at their most recent visit</div></div>
    <div class="ds"><div class="ds-num vg">${pctImp}%</div><div class="ds-desc">of patients showed a clinically meaningful systolic BP reduction (≥5 mmHg) over the course of their care</div></div>
    ${avgD&&avgD>0?`<div class="ds"><div class="ds-num vg">↓${avgD}</div><div class="ds-desc">mmHg average systolic reduction across all patients with 2+ visits</div></div>`:''}
    <div class="ds"><div class="ds-num ${cris>0?'vr':''}">${cris}</div><div class="ds-desc">patients currently classified as hypertensive crisis at their last visit</div></div>
    ${lapp.length?`<div class="ds"><div class="ds-num vg">${lapp.length?Math.round(a1cAtG/lapp.length*100):0}%</div><div class="ds-desc">of diabetes patients are at their A1C goal at their most recent visit</div></div>`:''}
    <div class="ds"><div class="ds-num">${tot}</div><div class="ds-desc">total unique patients with data in this system</div></div>
  </div>`;
  requestAnimationFrame(()=>{
    const tt={backgroundColor:'#fff',borderColor:'#e5e3dd',borderWidth:1,titleColor:'#1a1916',bodyColor:'#5a574f',padding:10,boxPadding:4,titleFont:{family:'Inter',size:12},bodyFont:{family:'Inter',size:11}};
    const leg={position:'right',labels:{font:{family:'Inter',size:11},color:'#5a574f',padding:11,boxWidth:11}};
    anCharts['bpc']=new Chart(document.getElementById('an-bpcls'),{type:'doughnut',data:{labels:['Normal','Elevated','Stage 1 HTN','Stage 2 HTN','Crisis'],datasets:[{data:[norm,elv,s1,s2,cris],backgroundColor:['#16a34a','#d97706','#ea580c','#dc2626','#7f1d1d'],borderWidth:2,borderColor:'#fff'}]},options:{responsive:true,maintainAspectRatio:false,cutout:'60%',plugins:{legend:leg,tooltip:tt}}});
    anCharts['bpo']=new Chart(document.getElementById('an-bpout'),{type:'bar',data:{labels:['Improved (↓≥5)','Unchanged','Worsened (↑≥5)'],datasets:[{data:[imp,unch,wors],backgroundColor:['rgba(22,163,74,.75)','rgba(90,87,79,.35)','rgba(220,38,38,.65)'],borderRadius:6,borderSkipped:false}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false},tooltip:tt},scales:{y:{grid:{color:'rgba(0,0,0,.05)'},ticks:{font:{family:'Inter',size:11},color:'#9a9589'},beginAtZero:true},x:{grid:{display:false},ticks:{font:{family:'Inter',size:11},color:'#9a9589'}}}}});
    if(avgF&&avgL)anCharts['avs']=new Chart(document.getElementById('an-avgsys'),{type:'bar',data:{labels:['First Visit','Latest Visit'],datasets:[{data:[avgF,avgL],backgroundColor:[avgF>=140?'rgba(220,38,38,.6)':'rgba(217,119,6,.6)',avgL<130?'rgba(22,163,74,.7)':avgL<140?'rgba(217,119,6,.7)':'rgba(220,38,38,.7)'],borderRadius:8,borderSkipped:false}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false},tooltip:tt},scales:{y:{min:Math.max(0,Math.min(avgF,avgL)-20),grid:{color:'rgba(0,0,0,.05)'},ticks:{font:{family:'Inter',size:11},color:'#9a9589'}},x:{grid:{display:false},ticks:{font:{family:'Inter',size:11},color:'#9a9589'}}}}});
    if(lapp.length)anCharts['a1cg']=new Chart(document.getElementById('an-a1cg'),{type:'doughnut',data:{labels:['At Goal (<7%)','Above Goal'],datasets:[{data:[a1cAtG,lapp.length-a1cAtG],backgroundColor:['rgba(22,163,74,.8)','rgba(217,119,6,.7)'],borderWidth:2,borderColor:'#fff'}]},options:{responsive:true,maintainAspectRatio:false,cutout:'60%',plugins:{legend:leg,tooltip:tt}}});
  });
}

/* ── UI HELPERS ── */
function swTab(name){
[‘dash’,‘log’].forEach(t=>{
const ta=document.getElementById(‘ta-’+t),dp=document.getElementById(‘dp-’+t);
if(!ta||!dp)return;
if(t===name){ta.classList.add(‘active’);dp.classList.add(‘active’);}
else{ta.classList.remove(‘active’);dp.classList.remove(‘active’);}
});
if(name===‘dash’)requestAnimationFrame(()=>drawDetCharts(activePtId));
}
function togLS(hd,bodyId){const b=document.getElementById(bodyId);if(!b)return;const o=b.classList.toggle(‘open’);hd.classList.toggle(‘open’,o);}
function livePreview(){
const sys=parseInt(document.getElementById(‘v-sys’)?.value||0);
const dia=parseInt(document.getElementById(‘v-dia’)?.value||0);
const r=document.getElementById(‘bpl-r’),c=document.getElementById(‘bpl-c’),ct=document.getElementById(‘bpl-ctrl’),cr=document.getElementById(‘bpl-cr’);
if(!r)return;
if(!sys||!dia){r.textContent=’—/—’;r.className=‘bpl-val’;if(c){c.textContent=’—’;if(ct)ct.textContent=’’;if(cr)cr.textContent=’—’;}return;}
r.textContent=sys+’/’+dia;
let cls=‘Normal’,col=‘vg’,ctrl=‘Controlled’,crisis=‘No’;
if(sys>=180||dia>=120){cls=‘Hypertensive Crisis’;col=‘vr’;ctrl=‘Uncontrolled’;crisis=‘YES’;}
else if(sys>=140||dia>=90){cls=‘Stage 2 HTN’;col=‘vr’;ctrl=‘Uncontrolled’;}
else if(sys>=130||dia>=80){cls=‘Stage 1 HTN’;col=‘va’;ctrl=‘Uncontrolled’;}
else if(sys>=120){cls=‘Elevated’;col=‘va’;ctrl=‘Elevated’;}
r.className=’bpl-val ’+col;c.textContent=cls;c.className=‘bpl-val ‘+col;c.style.fontSize=‘13px’;c.style.fontWeight=‘700’;
if(ct)ct.textContent=ctrl;if(cr){cr.textContent=crisis;cr.className=‘bpl-val ‘+(crisis===‘YES’?‘vr’:‘vg’);cr.style.fontSize=‘13px’;}
}
function showPage(name,btn){
document.querySelectorAll(’.page’).forEach(p=>p.classList.remove(‘active’));
document.querySelectorAll(’.tnav’).forEach(b=>b.classList.remove(‘active’));
document.getElementById(‘page-’+name).classList.add(‘active’);btn.classList.add(‘active’);
if(name===‘analytics’)renderAnalytics();
}
function icoInfo(){return`<svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/></svg>`;}
function icoWarn(){return`<svg viewBox="0 0 24 24"><path d="M10.29 3.86L1.82 18a2 2 0 0 0 1.71 3h16.94a2 2 0 0 0 1.71-3L13.71 3.86a2 2 0 0 0-3.42 0z"/><line x1="12" y1="9" x2="12" y2="13"/><line x1="12" y1="17" x2="12.01" y2="17"/></svg>`;}
function icoCheck(){return`<svg viewBox="0 0 24 24"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>`;}

/* ── ADD PATIENT ── */
function openAddModal(){document.getElementById(‘add-modal’).classList.add(‘open’);setTimeout(()=>document.getElementById(‘modal-pid’).focus(),100);}
function closeAddModal(){document.getElementById(‘add-modal’).classList.remove(‘open’);document.getElementById(‘modal-pid’).value=’’;}
function confirmAdd(){
const pid=document.getElementById(‘modal-pid’).value.trim();
if(!pid){document.getElementById(‘modal-pid’).style.borderColor=‘var(–red)’;return;}
if(patients.find(p=>p.ptId===pid)){toast(‘Patient ‘+pid+’ already exists’,‘err’);closeAddModal();return;}
patients.unshift({ptId:pid,condTag:’’,dateAdded:todayISO(),notes:’’,lastVisit:’’});
renderPtList();closeAddModal();selectPt(pid);toast(‘Patient ‘+pid+’ added’);
appendRow(SH_INFO,[pid,’’,todayISO(),’’]);
}

/* ── DYNAMIC ROWS ── */
function addLabRow(name=’’,val=’’,unit=’’,cat=’’,lo=’’,hi=’’){
const d=document.createElement(‘div’);d.className=‘dyn-row dr-lab’;
d.innerHTML=`<div class="f"><input class="lr-n" placeholder="Test name" value="${name}" style="font-size:12px;padding:7px 9px"/></div><div class="f"><input class="lr-v" placeholder="Value" value="${val}" style="font-size:12px;padding:7px 9px"/></div><div class="f"><input class="lr-u" placeholder="Unit" value="${unit}" style="font-size:12px;padding:7px 9px"/></div><div class="f"><select class="lr-c" style="font-size:12px;padding:7px 9px">${['Metabolic','Lipid Panel','Renal','Hepatic','CBC','Thyroid','Cardiac','Other'].map(c=>`<option${c===cat?’ selected’:’’}>${c}</option>`).join('')}</select></div><div class="f"><input class="lr-lo" placeholder="Lo" value="${lo}" style="font-size:12px;padding:7px 9px"/></div><div class="f"><input class="lr-hi" placeholder="Hi" value="${hi}" style="font-size:12px;padding:7px 9px"/></div><button class="del-btn" onclick="this.closest('.dyn-row').remove()"><svg viewBox="0 0 24 24"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg></button>`;
document.getElementById(‘lab-rows’)?.appendChild(d);
}
function addMedRow(name=’’,dose=’’,unit=’’,freq=’’,route=‘PO’){
const d=document.createElement(‘div’);d.className=‘dyn-row dr-med’;
d.innerHTML=`<div class="f"><input class="mr-n" placeholder="Medication" value="${name}" style="font-size:12px;padding:7px 9px"/></div><div class="f"><input class="mr-d" placeholder="Dose" value="${dose}" style="font-size:12px;padding:7px 9px"/></div><div class="f"><input class="mr-u" placeholder="mg" value="${unit}" style="font-size:12px;padding:7px 9px"/></div><div class="f"><input class="mr-f" placeholder="Once daily" value="${freq}" style="font-size:12px;padding:7px 9px"/></div><div class="f"><select class="mr-r" style="font-size:12px;padding:7px 9px">${['PO','SQ','IM','IV','Inhaled','Topical','Other'].map(r=>`<option${r===route?’ selected’:’’}>${r}</option>`).join('')}</select></div><button class="del-btn" onclick="this.closest('.dyn-row').remove()"><svg viewBox="0 0 24 24"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg></button>`;
document.getElementById(‘med-rows’)?.appendChild(d);
}

/* ── WRITE ── */
async function appendRow(sheet,vals){
if(!isSignedIn){toast(‘Sign in to save data’,‘err’);return false;}
if(!SHEET_ID){toast(‘Add Spreadsheet ID in Settings’,‘err’);return false;}
try{
if(!headersWritten[sheet]){
const chk=await gapi.client.sheets.spreadsheets.values.get({spreadsheetId:SHEET_ID,range:sheet+’!A1:A1’});
if(!(chk.result.values||[])[0]?.[0]){
await gapi.client.sheets.spreadsheets.values.update({spreadsheetId:SHEET_ID,range:sheet+’!A1’,valueInputOption:‘RAW’,resource:{values:[HDR[sheet]]}});
}
headersWritten[sheet]=true;
}
await gapi.client.sheets.spreadsheets.values.append({spreadsheetId:SHEET_ID,range:sheet+’!A1’,valueInputOption:‘USER_ENTERED’,insertDataOption:‘INSERT_ROWS’,resource:{values:[vals]}});
return true;
}catch(e){toast(‘Save error: ‘+e.message,‘err’);return false;}
}
function setSaving(id,on){const b=document.getElementById(id);if(!b)return;b.classList.toggle(‘saving’,on);if(on){b.dataset.orig=b.textContent;b.textContent=‘Saving…’;}else if(b.dataset.orig)b.textContent=b.dataset.orig;}
async function saveInfo(pid){
const tag=g(‘v-condtag’),notes=g(‘v-pt-notes’);
const ok=await appendRow(SH_INFO,[pid,tag,todayISO(),notes]);
if(ok){const p=patients.find(x=>x.ptId===pid);if(p){p.condTag=tag;p.notes=notes;}toast(‘Patient info saved ✓’);renderPtList();await loadAll();}
}
async function saveBP(pid){
const sys=g(‘v-sys’),dia=g(‘v-dia’);
if(!sys||!dia){toast(‘Systolic and diastolic required’,‘err’);return;}
const s=parseInt(sys);
const cls=s>=180?‘Hypertensive Crisis’:s>=140?‘Stage 2 HTN’:s>=130?‘Stage 1 HTN’:s>=120?‘Elevated’:‘Normal’;
const ctrl=s<130?‘Controlled’:‘Uncontrolled’,crisis=s>=180?‘Yes’:‘No’;
setSaving(‘bp-save’,true);
const ok=await appendRow(SH_BP,[todayISO(),pid,sys,dia,g(‘v-hr’),g(‘v-wt’),g(‘v-o2’),g(‘v-pain’),sys+’/’+dia,cls,ctrl,crisis]);
setSaving(‘bp-save’,false);
if(ok){toast(‘BP saved ✓’);await loadAll();}
}
async function saveA1C(pid){
const a1c=g(‘v-a1c’);if(!a1c){toast(‘A1C value required’,‘err’);return;}
setSaving(‘a1c-save’,true);
const ok=await appendRow(SH_A1C,[todayISO(),pid,a1c,g(‘v-glucose’),g(‘v-a1c-goal’),g(‘v-a1c-notes’)]);
setSaving(‘a1c-save’,false);
if(ok){toast(‘A1C saved ✓’);await loadAll();}
}
async function saveLabs(pid){
let saved=0;
for(const el of document.querySelectorAll(’#lab-rows .dyn-row’)){
const name=el.querySelector(’.lr-n’).value.trim(),val=el.querySelector(’.lr-v’).value.trim();
if(!name||!val)continue;
const lo=el.querySelector(’.lr-lo’).value,hi=el.querySelector(’.lr-hi’).value;
const flag=lo&&hi?(parseFloat(val)<parseFloat(lo)||parseFloat(val)>parseFloat(hi)?‘ABNORMAL’:‘Normal’):’’;
if(await appendRow(SH_LABS,[todayISO(),pid,‘Lab’,name,val,el.querySelector(’.lr-u’).value,el.querySelector(’.lr-c’).value,’’,lo,hi,flag]))saved++;
}
if(saved){toast(saved+’ lab(s) saved ✓’);await loadAll();}else toast(‘No valid lab rows’,‘err’);
}
async function saveMeds(pid){
let saved=0;
for(const el of document.querySelectorAll(’#med-rows .dyn-row’)){
const name=el.querySelector(’.mr-n’).value.trim();if(!name)continue;
if(await appendRow(SH_LABS,[todayISO(),pid,‘Medication’,name,el.querySelector(’.mr-d’).value,el.querySelector(’.mr-u’).value,el.querySelector(’.mr-f’).value,el.querySelector(’.mr-r’).value,’’,’’,’’]))saved++;
}
if(saved){toast(saved+’ med(s) saved ✓’);await loadAll();}else toast(‘No valid medication rows’,‘err’);
}

/* ── SETTINGS ── */
function openDrawer(){
document.getElementById(‘drw-ov’).classList.add(‘open’);document.getElementById(‘drawer’).classList.add(‘open’);
document.getElementById(‘cfg-sid’).value=SHEET_ID;document.getElementById(‘cfg-key’).value=API_KEY;document.getElementById(‘cfg-client’).value=CLIENT_ID;
}
function closeDrawer(){document.getElementById(‘drw-ov’).classList.remove(‘open’);document.getElementById(‘drawer’).classList.remove(‘open’);}
function saveSettings(){
SHEET_ID=document.getElementById(‘cfg-sid’).value.trim();API_KEY=document.getElementById(‘cfg-key’).value.trim();CLIENT_ID=document.getElementById(‘cfg-client’).value.trim();
headersWritten={};if(CLIENT_ID)initTC();closeDrawer();toast(‘Settings saved’);if(SHEET_ID&&API_KEY)loadAll();
}

/* ── UTILS ── */
function pd(str){if(!str)return’’;if(str.includes(’-’))return str.slice(0,10);const p=str.split(’/’);if(p.length===3){const[m,d,y]=p;return y+’-’+m.padStart(2,‘0’)+’-’+d.padStart(2,‘0’);}return str;}
function todayISO(){return new Date().toISOString().slice(0,10);}
function g(id){return document.getElementById(id)?.value||’’;}
function latest(arr,pk,pid){const rows=arr.filter(r=>r[pk]===pid).sort((a,b)=>pd(a[‘Date’]).localeCompare(pd(b[‘Date’])));return rows.length?rows[rows.length-1]:null;}
function toast(msg,type=‘ok’){const t=document.getElementById(‘toast’);t.textContent=msg;t.className=‘toast ‘+(type===‘err’?‘err’:‘ok’)+’ show’;setTimeout(()=>t.classList.remove(‘show’),3000);}

/* ── BOOT ── */
renderPtList();
