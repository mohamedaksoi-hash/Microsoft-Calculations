<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Subscription Price Calculator</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react/18.2.0/umd/react.production.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/18.2.0/umd/react-dom.production.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/7.23.2/babel.min.js"></script>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; background: linear-gradient(135deg, #f0f4ff 0%, #e8f5e9 100%); min-height: 100vh; display: flex; align-items: center; justify-content: center; padding: 20px; }
  .card { background: white; border-radius: 20px; box-shadow: 0 4px 24px rgba(0,0,0,0.08); padding: 32px; width: 100%; max-width: 480px; }
  .header { text-align: center; margin-bottom: 28px; }
  .icon { width: 52px; height: 52px; background: #2563eb; border-radius: 14px; display: flex; align-items: center; justify-content: center; margin: 0 auto 12px; }
  .icon svg { width: 26px; height: 26px; stroke: white; fill: none; }
  h1 { font-size: 22px; font-weight: 700; color: #1e293b; }
  .subtitle { font-size: 13px; color: #94a3b8; margin-top: 4px; }
  .field { margin-bottom: 16px; }
  label { display: block; font-size: 11px; font-weight: 700; color: #64748b; text-transform: uppercase; letter-spacing: 0.05em; margin-bottom: 6px; }
  select, input { width: 100%; border: 1.5px solid #e2e8f0; border-radius: 10px; padding: 10px 14px; font-size: 14px; color: #1e293b; background: white; outline: none; transition: border-color 0.2s; appearance: none; -webkit-appearance: none; background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' viewBox='0 0 12 12'%3E%3Cpath fill='%2394a3b8' d='M6 8L1 3h10z'/%3E%3C/svg%3E"); background-repeat: no-repeat; background-position: right 12px center; }
  input { background-image: none; }
  select:focus, input:focus { border-color: #2563eb; }
  select:disabled { background-color: #f8fafc; color: #cbd5e1; cursor: not-allowed; }
  .grid2 { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
  .divider { border: none; border-top: 1.5px solid #f1f5f9; margin: 20px 0; }
  .empty { text-align: center; padding: 32px 0; color: #cbd5e1; }
  .empty svg { width: 40px; height: 40px; stroke: currentColor; fill: none; margin: 0 auto 8px; display: block; }
  .empty p { font-size: 13px; }
  /* Result blocks */
  .block { border-radius: 14px; padding: 16px; margin-bottom: 12px; }
  .block-blue { background: #2563eb; color: white; }
  .block-green { background: #f0fdf4; border: 1.5px solid #bbf7d0; }
  .block-purple { background: #faf5ff; border: 1.5px solid #e9d5ff; }
  .block-label { font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.07em; margin-bottom: 10px; display: flex; align-items: center; justify-content: space-between; }
  .block-blue .block-label { color: rgba(255,255,255,0.7); }
  .block-green .block-label { color: #15803d; }
  .block-purple .block-label { color: #7e22ce; }
  .badge { font-size: 11px; font-weight: 700; padding: 2px 8px; border-radius: 999px; }
  .badge-green { background: #bbf7d0; color: #15803d; }
  .badge-purple { background: #e9d5ff; color: #7e22ce; }
  .row { display: flex; justify-content: space-between; align-items: center; padding: 3px 0; }
  .row-label { font-size: 13px; }
  .block-blue .row-label { color: rgba(255,255,255,0.85); }
  .block-green .row-label { color: #166534; }
  .block-purple .row-label { color: #6b21a8; }
  .row-value { font-size: 15px; font-weight: 700; }
  .block-blue .row-value { color: white; }
  .block-green .row-value { color: #14532d; }
  .block-purple .row-value { color: #581c87; }
  .footer { text-align: center; font-size: 11px; color: #cbd5e1; margin-top: 16px; }
</style>
</head>
<body>
<div id="root"></div>
<script type="text/babel">
const { useState, useMemo } = React;

const data = [
  { name: "Microsoft 365 Apps for business", commitment: "Annually", cycle: "Annually", monthly: null, annual: 89.60, savings: 17.95 },
  { name: "Microsoft 365 Apps for business", commitment: "Annually", cycle: "Monthly", monthly: 7.84, annual: 94.08, savings: 18 },
  { name: "Microsoft 365 Apps for business", commitment: "Monthly", cycle: "Monthly", monthly: 8.96, annual: 107.52, savings: 17.95 },
  { name: "Microsoft 365 Business Basic", commitment: "Annually", cycle: "Annually", monthly: null, annual: 51.20, savings: 18 },
  { name: "Microsoft 365 Business Basic", commitment: "Annually", cycle: "Monthly", monthly: 4.84, annual: 58.08, savings: 18 },
  { name: "Microsoft 365 Business Basic", commitment: "Monthly", cycle: "Monthly", monthly: 5.12, annual: 61.44, savings: 18 },
  { name: "Microsoft 365 Business Standard", commitment: "Annually", cycle: "Annually", monthly: null, annual: 106.34, savings: 18 },
  { name: "Microsoft 365 Business Standard", commitment: "Annually", cycle: "Monthly", monthly: 9.34, annual: 112.08, savings: 18 },
  { name: "Microsoft 365 Business Standard", commitment: "Monthly", cycle: "Monthly", monthly: 10.64, annual: 127.68, savings: 18 },
  { name: "Exchange Online (Plan 2)", commitment: "Annually", cycle: "Annually", monthly: null, annual: 67.94, savings: 18 },
  { name: "Exchange Online (Plan 2)", commitment: "Annually", cycle: "Monthly", monthly: 5.95, annual: 71.40, savings: 18 },
  { name: "Exchange Online (Plan 2)", commitment: "Monthly", cycle: "Monthly", monthly: 6.79, annual: 81.48, savings: 18 },
  { name: "Exchange Online (Plan 1)", commitment: "Annually", cycle: "Annually", monthly: null, annual: 34.46, savings: 18 },
  { name: "Exchange Online (Plan 1)", commitment: "Annually", cycle: "Monthly", monthly: 3.02, annual: 36.24, savings: 18 },
  { name: "Exchange Online (Plan 1)", commitment: "Monthly", cycle: "Monthly", monthly: 3.45, annual: 41.40, savings: 18 },
  { name: "Power BI Pro", commitment: "Annually", cycle: "Annually", monthly: null, annual: 122.27, savings: 16 },
  { name: "Power BI Pro", commitment: "Annually", cycle: "Monthly", monthly: 10.69, annual: 128.28, savings: 16 },
  { name: "Power BI Pro", commitment: "Monthly", cycle: "Monthly", monthly: 12.32, annual: 147.84, savings: 16 },
  { name: "Power Bi Premium", commitment: "Annually", cycle: "Annually", monthly: null, annual: 210.19, savings: 16 },
  { name: "Power Bi Premium", commitment: "Annually", cycle: "Monthly", monthly: 18.39, annual: 220.68, savings: 16 },
  { name: "Power Bi Premium", commitment: "Monthly", cycle: "Monthly", monthly: 21.02, annual: 252.24, savings: 16 },
  { name: "Microsoft E3", commitment: "Annually", cycle: "Annually", monthly: null, annual: 343.63, savings: 18 },
  { name: "Microsoft E3", commitment: "Annually", cycle: "Monthly", monthly: 30.07, annual: 360.84, savings: 18 },
  { name: "Microsoft E3", commitment: "Monthly", cycle: "Monthly", monthly: 34.36, annual: 412.32, savings: 18 },
  { name: "Power Automate Premium", commitment: "Annually", cycle: "Annually", monthly: null, annual: 123.16, savings: 21 },
  { name: "Power Automate Premium", commitment: "Annually", cycle: "Monthly", monthly: 10.78, annual: 129.36, savings: 21 },
  { name: "Power Automate Premium", commitment: "Monthly", cycle: "Monthly", monthly: 12.32, annual: 147.84, savings: 21 },
  { name: "Microsoft 365 Copilot", commitment: "Annually", cycle: "Annually", monthly: null, annual: 288.00, savings: 8 },
  { name: "Microsoft 365 Copilot", commitment: "Annually", cycle: "Monthly", monthly: 14.00, annual: 167.97, savings: 8 },
  { name: "Microsoft 365 Copilot", commitment: "Monthly", cycle: "Monthly", monthly: 16.00, annual: 191.96, savings: 8 },
];

const serviceNames = [...new Set(data.map(d => d.name))];
const fmt = v => v != null ? `€${v.toFixed(2)}` : "—";

function App() {
  const [service, setService] = useState("");
  const [commitment, setCommitment] = useState("");
  const [cycle, setCycle] = useState("");
  const [licenses, setLicenses] = useState(1);

  const commitmentOptions = useMemo(() => !service ? [] : [...new Set(data.filter(d => d.name === service).map(d => d.commitment))], [service]);
  const cycleOptions = useMemo(() => (!service || !commitment) ? [] : [...new Set(data.filter(d => d.name === service && d.commitment === commitment).map(d => d.cycle))], [service, commitment]);
  const result = useMemo(() => (!service || !commitment || !cycle) ? null : data.find(d => d.name === service && d.commitment === commitment && d.cycle === cycle) || null, [service, commitment, cycle]);

  const qty = Math.max(1, parseInt(licenses) || 1);

  const calc = useMemo(() => {
    if (!result) return null;
    const pct = result.savings / 100;
    const monthlyTotal = result.monthly != null ? result.monthly * qty : null;
    const annualTotal = result.annual * qty;
    const monthlyDiscount = monthlyTotal != null ? monthlyTotal * pct : null;
    const yearlyDiscount = annualTotal * pct;
    return {
      monthlyTotal, annualTotal,
      monthlyDiscount, yearlyDiscount,
      monthlyCommission: monthlyDiscount != null ? monthlyDiscount * 0.25 : null,
      annualCommission: yearlyDiscount * 0.25
    };
  }, [result, qty]);

  const handleService = v => { setService(v); setCommitment(""); setCycle(""); };
  const handleCommitment = v => { setCommitment(v); setCycle(""); };

  return (
    <div className="card">
      <div className="header">
        <div className="icon">
          <svg viewBox="0 0 24 24" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
            <rect x="2" y="5" width="20" height="14" rx="2"/><line x1="2" y1="10" x2="22" y2="10"/>
          </svg>
        </div>
        <h1>Subscription Calculator</h1>
        <p className="subtitle">Select a service to calculate pricing, savings & commission</p>
      </div>

      <div className="field">
        <label>Subscription Name</label>
        <select value={service} onChange={e => handleService(e.target.value)}>
          <option value="">— Select a service —</option>
          {serviceNames.map(s => <option key={s} value={s}>{s}</option>)}
        </select>
      </div>

      <div className="grid2">
        <div className="field">
          <label>Billing Commitment</label>
          <select value={commitment} onChange={e => handleCommitment(e.target.value)} disabled={!service}>
            <option value="">— Select —</option>
            {commitmentOptions.map(c => <option key={c} value={c}>{c}</option>)}
          </select>
        </div>
        <div className="field">
          <label>Billing Cycle</label>
          <select value={cycle} onChange={e => setCycle(e.target.value)} disabled={!commitment}>
            <option value="">— Select —</option>
            {cycleOptions.map(c => <option key={c} value={c}>{c}</option>)}
          </select>
        </div>
      </div>

      <div className="field">
        <label>Number of Licenses</label>
        <input type="number" min="1" value={licenses} onChange={e => setLicenses(e.target.value)} placeholder="1" />
      </div>

      <hr className="divider" />

      {!calc ? (
        <div className="empty">
          <svg viewBox="0 0 24 24" strokeWidth="1.5" strokeLinecap="round" strokeLinejoin="round">
            <circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/>
          </svg>
          <p>Fill in the fields above to see pricing</p>
        </div>
      ) : (
        <>
          <div className="block block-blue">
            <div className="block-label">Total Price · {qty} License{qty > 1 ? "s" : ""}</div>
            <div className="row"><span className="row-label">Monthly Total</span><span className="row-value">{calc.monthlyTotal != null ? fmt(calc.monthlyTotal) : "—"}</span></div>
            <div className="row"><span className="row-label">Annual Total</span><span className="row-value">{fmt(calc.annualTotal)}</span></div>
          </div>

          <div className="block block-green">
            <div className="block-label">
              💰 Discount Savings
              <span className="badge badge-green">{result.savings}% off</span>
            </div>
            <div className="row"><span className="row-label">Monthly Discount Saving</span><span className="row-value">{calc.monthlyDiscount != null ? fmt(calc.monthlyDiscount) : "—"}</span></div>
            <div className="row"><span className="row-label">Yearly Discount Saving</span><span className="row-value">{fmt(calc.yearlyDiscount)}</span></div>
          </div>

          <div className="block block-purple">
            <div className="block-label">
              ⚡ Spendbase Commission
              <span className="badge badge-purple">25% of savings</span>
            </div>
            <div className="row"><span className="row-label">Monthly Commission</span><span className="row-value">{calc.monthlyCommission != null ? fmt(calc.monthlyCommission) : "—"}</span></div>
            <div className="row"><span className="row-label">Annual Commission</span><span className="row-value">{fmt(calc.annualCommission)}</span></div>
          </div>
        </>
      )}
      <p className="footer">All prices in EUR · Spendbase commission = 25% of discount savings</p>
    </div>
  );
}

ReactDOM.render(<App />, document.getElementById("root"));
</script>
</body>
</html>
