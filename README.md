<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Subscription Price Calculator</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react/18.2.0/umd/react.production.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/18.2.0/umd/react-dom.production.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/7.23.2/babel.min.js"></script>
<style>
  *{box-sizing:border-box;margin:0;padding:0}
  body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;background:linear-gradient(135deg,#e0e7ff 0%,#f0fdf4 100%);min-height:100vh;padding:24px 16px}
  .page{max-width:700px;margin:0 auto}
  /* Header */
  .header{text-align:center;margin-bottom:32px}
  .logo{width:56px;height:56px;background:linear-gradient(135deg,#2563eb,#7c3aed);border-radius:16px;display:flex;align-items:center;justify-content:center;margin:0 auto 14px;box-shadow:0 8px 20px rgba(37,99,235,0.3)}
  .logo svg{width:28px;height:28px;stroke:white;fill:none}
  .header h1{font-size:26px;font-weight:800;color:#1e293b;letter-spacing:-0.5px}
  .header p{font-size:14px;color:#94a3b8;margin-top:5px}
  /* Cards */
  .card{background:white;border-radius:20px;box-shadow:0 2px 20px rgba(0,0,0,0.07);padding:24px;margin-bottom:16px}
  .card-title{font-size:12px;font-weight:700;text-transform:uppercase;letter-spacing:0.07em;color:#94a3b8;margin-bottom:16px;display:flex;align-items:center;gap:6px}
  .card-title span{font-size:15px}
  /* Form */
  .field{margin-bottom:14px}
  .field label{display:block;font-size:11px;font-weight:700;color:#64748b;text-transform:uppercase;letter-spacing:0.05em;margin-bottom:6px}
  select,input[type=number]{width:100%;border:1.5px solid #e2e8f0;border-radius:10px;padding:10px 14px;font-size:14px;color:#1e293b;background:white;outline:none;transition:border-color 0.2s,box-shadow 0.2s;appearance:none;-webkit-appearance:none;background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' viewBox='0 0 12 12'%3E%3Cpath fill='%2394a3b8' d='M6 8L1 3h10z'/%3E%3C/svg%3E");background-repeat:no-repeat;background-position:right 12px center}
  input[type=number]{background-image:none}
  select:focus,input:focus{border-color:#2563eb;box-shadow:0 0 0 3px rgba(37,99,235,0.1)}
  select:disabled{background-color:#f8fafc;color:#cbd5e1;cursor:not-allowed}
  .grid2{display:grid;grid-template-columns:1fr 1fr;gap:12px}
  .grid3{display:grid;grid-template-columns:2fr 1fr 1fr;gap:10px;align-items:end}
  /* Service rows */
  .service-row{background:#f8fafc;border:1.5px solid #e2e8f0;border-radius:14px;padding:16px;margin-bottom:12px;position:relative;transition:border-color 0.2s}
  .service-row:hover{border-color:#c7d2fe}
  .service-row-header{display:flex;align-items:center;justify-content:space-between;margin-bottom:12px}
  .service-num{font-size:11px;font-weight:700;color:#6366f1;background:#eef2ff;padding:3px 9px;border-radius:999px}
  .remove-btn{background:none;border:none;cursor:pointer;color:#cbd5e1;font-size:18px;line-height:1;padding:2px 6px;border-radius:6px;transition:color 0.2s,background 0.2s}
  .remove-btn:hover{color:#ef4444;background:#fef2f2}
  /* Add button */
  .add-btn{width:100%;border:2px dashed #c7d2fe;background:transparent;border-radius:14px;padding:13px;font-size:14px;font-weight:600;color:#6366f1;cursor:pointer;transition:all 0.2s;display:flex;align-items:center;justify-content:center;gap:8px}
  .add-btn:hover{background:#eef2ff;border-color:#6366f1}
  /* Calculate button */
  .calc-btn{width:100%;background:linear-gradient(135deg,#2563eb,#7c3aed);color:white;border:none;border-radius:14px;padding:15px;font-size:15px;font-weight:700;cursor:pointer;transition:opacity 0.2s,transform 0.1s;margin-top:4px;letter-spacing:0.02em}
  .calc-btn:hover{opacity:0.92;transform:translateY(-1px)}
  .calc-btn:active{transform:translateY(0)}
  /* Results */
  .results{margin-top:8px}
  .result-service{border-radius:18px;overflow:hidden;margin-bottom:20px;box-shadow:0 2px 16px rgba(0,0,0,0.06)}
  .result-service-header{background:linear-gradient(135deg,#1e40af,#4f46e5);padding:16px 20px;color:white}
  .result-service-name{font-size:15px;font-weight:700}
  .result-service-meta{font-size:12px;opacity:0.75;margin-top:2px}
  .result-body{background:white;padding:16px 20px}
  /* Per license block */
  .per-license{background:#f8fafc;border-radius:12px;padding:14px;margin-bottom:12px}
  .per-license-title{font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:0.07em;color:#94a3b8;margin-bottom:10px}
  .pl-row{display:flex;justify-content:space-between;align-items:center;padding:3px 0}
  .pl-label{font-size:13px;color:#64748b}
  .pl-value{font-size:13px;font-weight:600;color:#334155}
  /* Total block */
  .total-block{background:linear-gradient(135deg,#2563eb,#4f46e5);border-radius:12px;padding:14px;margin-bottom:12px;color:white}
  .total-title{font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:0.07em;opacity:0.7;margin-bottom:10px}
  .total-row{display:flex;justify-content:space-between;align-items:center;padding:3px 0}
  .total-label{font-size:13px;opacity:0.85}
  .total-value{font-size:16px;font-weight:800}
  /* Savings block */
  .savings-block{background:#f0fdf4;border:1.5px solid #bbf7d0;border-radius:12px;padding:14px;margin-bottom:12px}
  .savings-title{font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:0.07em;color:#15803d;margin-bottom:10px;display:flex;justify-content:space-between;align-items:center}
  .badge{font-size:11px;font-weight:700;padding:2px 8px;border-radius:999px;background:#bbf7d0;color:#15803d}
  .savings-row{display:flex;justify-content:space-between;padding:3px 0}
  .savings-label{font-size:13px;color:#166534}
  .savings-value{font-size:13px;font-weight:700;color:#14532d}
  /* Commission block */
  .commission-block{background:#faf5ff;border:1.5px solid #e9d5ff;border-radius:12px;padding:14px}
  .commission-title{font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:0.07em;color:#7e22ce;margin-bottom:10px;display:flex;justify-content:space-between;align-items:center}
  .badge-purple{background:#e9d5ff;color:#7e22ce}
  .commission-row{display:flex;justify-content:space-between;padding:3px 0}
  .commission-label{font-size:13px;color:#6b21a8}
  .commission-value{font-size:13px;font-weight:700;color:#581c87}
  /* Grand total */
  .grand-total{background:linear-gradient(135deg,#0f172a,#1e293b);border-radius:18px;padding:20px 24px;color:white;margin-top:4px}
  .gt-title{font-size:11px;font-weight:700;text-transform:uppercase;letter-spacing:0.08em;opacity:0.5;margin-bottom:14px}
  .gt-row{display:flex;justify-content:space-between;align-items:center;padding:5px 0;border-bottom:1px solid rgba(255,255,255,0.08)}
  .gt-row:last-child{border-bottom:none;padding-top:12px;margin-top:4px}
  .gt-label{font-size:13px;opacity:0.7}
  .gt-value{font-size:18px;font-weight:800}
  .gt-row:last-child .gt-label{opacity:1;font-weight:700;font-size:14px}
  .gt-row:last-child .gt-value{font-size:22px;color:#a5f3fc}
  .footer{text-align:center;font-size:11px;color:#94a3b8;margin-top:20px;padding-bottom:8px}
  .divider{border:none;border-top:1px solid #f1f5f9;margin:14px 0}
</style>
</head>
<body>
<div id="root"></div>
<script type="text/babel">
const { useState, useMemo } = React;

const DATA = [
  { name:"Microsoft 365 Apps for business", commitment:"Annually", cycle:"Annually", monthly:null, annual:89.60, savings:17.95 },
  { name:"Microsoft 365 Apps for business", commitment:"Annually", cycle:"Monthly", monthly:7.84, annual:94.08, savings:18 },
  { name:"Microsoft 365 Apps for business", commitment:"Monthly", cycle:"Monthly", monthly:8.96, annual:107.52, savings:17.95 },
  { name:"Microsoft 365 Business Basic", commitment:"Annually", cycle:"Annually", monthly:null, annual:51.20, savings:18 },
  { name:"Microsoft 365 Business Basic", commitment:"Annually", cycle:"Monthly", monthly:4.84, annual:58.08, savings:18 },
  { name:"Microsoft 365 Business Basic", commitment:"Monthly", cycle:"Monthly", monthly:5.12, annual:61.44, savings:18 },
  { name:"Microsoft 365 Business Standard", commitment:"Annually", cycle:"Annually", monthly:null, annual:106.34, savings:18 },
  { name:"Microsoft 365 Business Standard", commitment:"Annually", cycle:"Monthly", monthly:9.34, annual:112.08, savings:18 },
  { name:"Microsoft 365 Business Standard", commitment:"Monthly", cycle:"Monthly", monthly:10.64, annual:127.68, savings:18 },
  { name:"Exchange Online (Plan 2)", commitment:"Annually", cycle:"Annually", monthly:null, annual:67.94, savings:18 },
  { name:"Exchange Online (Plan 2)", commitment:"Annually", cycle:"Monthly", monthly:5.95, annual:71.40, savings:18 },
  { name:"Exchange Online (Plan 2)", commitment:"Monthly", cycle:"Monthly", monthly:6.79, annual:81.48, savings:18 },
  { name:"Exchange Online (Plan 1)", commitment:"Annually", cycle:"Annually", monthly:null, annual:34.46, savings:18 },
  { name:"Exchange Online (Plan 1)", commitment:"Annually", cycle:"Monthly", monthly:3.02, annual:36.24, savings:18 },
  { name:"Exchange Online (Plan 1)", commitment:"Monthly", cycle:"Monthly", monthly:3.45, annual:41.40, savings:18 },
  { name:"Power BI Pro", commitment:"Annually", cycle:"Annually", monthly:null, annual:122.27, savings:16 },
  { name:"Power BI Pro", commitment:"Annually", cycle:"Monthly", monthly:10.69, annual:128.28, savings:16 },
  { name:"Power BI Pro", commitment:"Monthly", cycle:"Monthly", monthly:12.32, annual:147.84, savings:16 },
  { name:"Power Bi Premium", commitment:"Annually", cycle:"Annually", monthly:null, annual:210.19, savings:16 },
  { name:"Power Bi Premium", commitment:"Annually", cycle:"Monthly", monthly:18.39, annual:220.68, savings:16 },
  { name:"Power Bi Premium", commitment:"Monthly", cycle:"Monthly", monthly:21.02, annual:252.24, savings:16 },
  { name:"Microsoft E3", commitment:"Annually", cycle:"Annually", monthly:null, annual:343.63, savings:18 },
  { name:"Microsoft E3", commitment:"Annually", cycle:"Monthly", monthly:30.07, annual:360.84, savings:18 },
  { name:"Microsoft E3", commitment:"Monthly", cycle:"Monthly", monthly:34.36, annual:412.32, savings:18 },
  { name:"Power Automate Premium", commitment:"Annually", cycle:"Annually", monthly:null, annual:123.16, savings:21 },
  { name:"Power Automate Premium", commitment:"Annually", cycle:"Monthly", monthly:10.78, annual:129.36, savings:21 },
  { name:"Power Automate Premium", commitment:"Monthly", cycle:"Monthly", monthly:12.32, annual:147.84, savings:21 },
  { name:"Microsoft 365 Copilot", commitment:"Annually", cycle:"Annually", monthly:null, annual:288.00, savings:8 },
  { name:"Microsoft 365 Copilot", commitment:"Annually", cycle:"Monthly", monthly:14.00, annual:167.97, savings:8 },
  { name:"Microsoft 365 Copilot", commitment:"Monthly", cycle:"Monthly", monthly:16.00, annual:191.96, savings:8 },
];

const SERVICE_NAMES = [...new Set(DATA.map(d => d.name))];
const fmt = v => v != null ? `€${v.toFixed(2)}` : "—";
const uid = () => Math.random().toString(36).slice(2);

const emptyRow = () => ({ id: uid(), name:"", commitment:"", cycle:"", licenses:1 });

const lookup = (name, commitment, cycle) =>
  DATA.find(d => d.name===name && d.commitment===commitment && d.cycle===cycle) || null;

const commitmentOpts = name => !name ? [] : [...new Set(DATA.filter(d=>d.name===name).map(d=>d.commitment))];
const cycleOpts = (name, commitment) => (!name||!commitment) ? [] : [...new Set(DATA.filter(d=>d.name===name&&d.commitment===commitment).map(d=>d.cycle))];

function ServiceRow({ row, idx, onChange, onRemove, canRemove }) {
  const copts = commitmentOpts(row.name);
  const cyopts = cycleOpts(row.name, row.commitment);

  const set = (k, v) => {
    const updated = { ...row, [k]: v };
    if (k === "name") { updated.commitment = ""; updated.cycle = ""; }
    if (k === "commitment") { updated.cycle = ""; }
    onChange(updated);
  };

  return (
    <div className="service-row">
      <div className="service-row-header">
        <span className="service-num">Service {idx + 1}</span>
        {canRemove && <button className="remove-btn" onClick={onRemove} title="Remove">✕</button>}
      </div>
      <div className="field">
        <label>Subscription Name</label>
        <select value={row.name} onChange={e => set("name", e.target.value)}>
          <option value="">— Select a service —</option>
          {SERVICE_NAMES.map(s => <option key={s} value={s}>{s}</option>)}
        </select>
      </div>
      <div className="grid2">
        <div className="field">
          <label>Billing Commitment</label>
          <select value={row.commitment} onChange={e => set("commitment", e.target.value)} disabled={!row.name}>
            <option value="">— Select —</option>
            {copts.map(c => <option key={c} value={c}>{c}</option>)}
          </select>
        </div>
        <div className="field">
          <label>Billing Cycle</label>
          <select value={row.cycle} onChange={e => set("cycle", e.target.value)} disabled={!row.commitment}>
            <option value="">— Select —</option>
            {cyopts.map(c => <option key={c} value={c}>{c}</option>)}
          </select>
        </div>
      </div>
      <div className="field" style={{marginBottom:0}}>
        <label>Number of Licenses</label>
        <input type="number" min="1" value={row.licenses} onChange={e => set("licenses", Math.max(1, parseInt(e.target.value)||1))} />
      </div>
    </div>
  );
}

function ResultCard({ row, idx }) {
  const rec = lookup(row.name, row.commitment, row.cycle);
  if (!rec) return null;
  const qty = row.licenses;
  const pct = rec.savings / 100;
  const monthlyTotal = rec.monthly != null ? rec.monthly * qty : null;
  const annualTotal = rec.annual * qty;
  const monthlyDiscount = monthlyTotal != null ? monthlyTotal * pct : null;
  const yearlyDiscount = annualTotal * pct;
  const monthlyCommission = monthlyDiscount != null ? monthlyDiscount * 0.25 : null;
  const annualCommission = yearlyDiscount * 0.25;

  return (
    <div className="result-service">
      <div className="result-service-header">
        <div className="result-service-name">{row.name}</div>
        <div className="result-service-meta">{row.commitment} commitment · {row.cycle} billing · {qty} license{qty>1?"s":""}</div>
      </div>
      <div className="result-body">
        {/* Per License */}
        <div className="per-license">
          <div className="per-license-title">Per License</div>
          <div className="pl-row"><span className="pl-label">Monthly Price</span><span className="pl-value">{fmt(rec.monthly)}</span></div>
          <div className="pl-row"><span className="pl-label">Annual Price</span><span className="pl-value">{fmt(rec.annual)}</span></div>
        </div>
        {/* Total */}
        <div className="total-block">
          <div className="total-title">Total for {qty} License{qty>1?"s":""}</div>
          <div className="total-row"><span className="total-label">Monthly Total</span><span className="total-value">{monthlyTotal!=null?fmt(monthlyTotal):"—"}</span></div>
          <div className="total-row"><span className="total-label">Annual Total</span><span className="total-value">{fmt(annualTotal)}</span></div>
        </div>
        {/* Savings */}
        <div className="savings-block">
          <div className="savings-title">💰 Discount Savings <span className="badge">{rec.savings}% off</span></div>
          <div className="savings-row"><span className="savings-label">Monthly Discount Saving</span><span className="savings-value">{monthlyDiscount!=null?fmt(monthlyDiscount):"—"}</span></div>
          <div className="savings-row"><span className="savings-label">Yearly Discount Saving</span><span className="savings-value">{fmt(yearlyDiscount)}</span></div>
        </div>
        {/* Commission */}
        <div className="commission-block">
          <div className="commission-title">⚡ Spendbase Commission <span className="badge badge-purple">25% of savings</span></div>
          <div className="commission-row"><span className="commission-label">Monthly Commission</span><span className="commission-value">{monthlyCommission!=null?fmt(monthlyCommission):"—"}</span></div>
          <div className="commission-row"><span className="commission-label">Annual Commission</span><span className="commission-value">{fmt(annualCommission)}</span></div>
        </div>
      </div>
    </div>
  );
}

function App() {
  const [rows, setRows] = useState([emptyRow()]);
  const [results, setResults] = useState(null);

  const updateRow = (id, updated) => setRows(rows.map(r => r.id===id ? updated : r));
  const removeRow = id => setRows(rows.filter(r => r.id!==id));
  const addRow = () => setRows([...rows, emptyRow()]);

  const validRows = rows.filter(r => r.name && r.commitment && r.cycle);

  const calculate = () => {
    if (!validRows.length) return;
    setResults(validRows);
  };

  // Grand totals
  const grandTotals = useMemo(() => {
    if (!results) return null;
    let gMonthly = 0, gAnnual = 0, gSavingsM = 0, gSavingsY = 0, gCommM = 0, gCommY = 0;
    let hasMonthly = false;
    results.forEach(r => {
      const rec = lookup(r.name, r.commitment, r.cycle);
      if (!rec) return;
      const qty = r.licenses;
      const pct = rec.savings / 100;
      const mt = rec.monthly != null ? rec.monthly * qty : null;
      const at = rec.annual * qty;
      if (mt != null) { gMonthly += mt; hasMonthly = true; }
      gAnnual += at;
      if (mt != null) gSavingsM += mt * pct;
      gSavingsY += at * pct;
      if (mt != null) gCommM += mt * pct * 0.25;
      gCommY += at * pct * 0.25;
    });
    return { gMonthly: hasMonthly ? gMonthly : null, gAnnual, gSavingsM: hasMonthly ? gSavingsM : null, gSavingsY, gCommM: hasMonthly ? gCommM : null, gCommY };
  }, [results]);

  return (
    <div className="page">
      <div className="header">
        <div className="logo">
          <svg viewBox="0 0 24 24" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
            <rect x="2" y="5" width="20" height="14" rx="2"/><line x1="2" y1="10" x2="22" y2="10"/>
          </svg>
        </div>
        <h1>Subscription Calculator</h1>
        <p>Add multiple services with different licenses & get full pricing breakdown</p>
      </div>

      {/* Input Card */}
      <div className="card">
        <div className="card-title"><span>📋</span> Services</div>
        {rows.map((row, i) => (
          <ServiceRow
            key={row.id}
            row={row}
            idx={i}
            onChange={updated => updateRow(row.id, updated)}
            onRemove={() => removeRow(row.id)}
            canRemove={rows.length > 1}
          />
        ))}
        <button className="add-btn" onClick={addRow}>
          <svg width="16" height="16" fill="none" stroke="currentColor" strokeWidth="2.5" viewBox="0 0 24 24"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>
          Add Another Service
        </button>
        <div style={{marginTop:14}}>
          <button className="calc-btn" onClick={calculate} disabled={!validRows.length}>
            {validRows.length === 0 ? "Fill in at least one service" : `Calculate ${validRows.length} Service${validRows.length>1?"s":""}`}
          </button>
        </div>
      </div>

      {/* Results */}
      {results && (
        <div className="results">
          <div className="card-title" style={{padding:"0 4px",marginBottom:12}}><span>📊</span> Results</div>
          {results.map((r, i) => <ResultCard key={r.id} row={r} idx={i} />)}

          {/* Grand Total */}
          {results.length > 1 && grandTotals && (
            <div className="grand-total">
              <div className="gt-title">🧾 Grand Total — All {results.length} Services</div>
              <div className="gt-row"><span className="gt-label">Monthly Total</span><span className="gt-value">{grandTotals.gMonthly!=null?fmt(grandTotals.gMonthly):"—"}</span></div>
              <div className="gt-row"><span className="gt-label">Annual Total</span><span className="gt-value">{fmt(grandTotals.gAnnual)}</span></div>
              <div className="gt-row"><span className="gt-label">Total Yearly Savings</span><span className="gt-value" style={{color:"#86efac"}}>{fmt(grandTotals.gSavingsY)}</span></div>
              <div className="gt-row"><span className="gt-label">Total Annual Commission</span><span className="gt-value" style={{color:"#c4b5fd"}}>{fmt(grandTotals.gCommY)}</span></div>
              <div className="gt-row"><span className="gt-label">💎 Total Annual (incl. commission)</span><span className="gt-value">{fmt(grandTotals.gAnnual + grandTotals.gCommY)}</span></div>
            </div>
          )}
        </div>
      )}
      <p className="footer">All prices in EUR · Spendbase commission = 25% of discount savings</p>
    </div>
  );
}

ReactDOM.render(<App />, document.getElementById("root"));
</script>
</body>
</html>
