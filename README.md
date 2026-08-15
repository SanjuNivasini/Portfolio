<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Sanju Nivasini | FP&A Decision Intelligence</title>
  <!-- Chart.js Library -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    :root {
      --bg-dark: #0b0f19;
      --bg-card: #131b2e;
      --bg-card-hover: #1a253e;
      --accent-blue: #2563eb;
      --accent-cyan: #06b6d4;
      --accent-green: #10b981;
      --accent-red: #ef4444;
      --text-main: #f8fafc;
      --text-muted: #94a3b8;
      --border-color: #1e293b;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
    }

    body {
      background-color: var(--bg-dark);
      color: var(--text-main);
      line-height: 1.5;
      padding-bottom: 60px;
    }

    /* Navigation Header */
    header {
      position: sticky;
      top: 0;
      z-index: 1000;
      background: rgba(11, 15, 25, 0.9);
      backdrop-filter: blur(10px);
      border-bottom: 1px solid var(--border-color);
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 1rem 2rem;
    }

    .brand {
      font-weight: 700;
      font-size: 1.2rem;
    }

    .brand span {
      display: block;
      font-size: 0.75rem;
      color: var(--accent-cyan);
      text-transform: uppercase;
      letter-spacing: 1px;
    }

    nav {
      display: flex;
      gap: 1rem;
    }

    nav a {
      color: var(--text-muted);
      text-decoration: none;
      font-size: 0.85rem;
      font-weight: 600;
      padding: 0.4rem 0.8rem;
      border-radius: 4px;
      transition: all 0.2s;
    }

    nav a:hover, nav a.active {
      color: var(--text-main);
      background-color: rgba(255, 255, 255, 0.05);
    }

    /* Container & Sections */
    .container {
      max-width: 1200px;
      margin: 2rem auto;
      padding: 0 1.5rem;
    }

    .section {
      margin-bottom: 4rem;
      background: var(--bg-card);
      border: 1px solid var(--border-color);
      border-radius: 8px;
      padding: 2rem;
    }

    .section-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 1.5rem;
      border-bottom: 1px solid var(--border-color);
      padding-bottom: 0.75rem;
    }

    .tag {
      background: rgba(37, 99, 235, 0.2);
      color: var(--accent-cyan);
      font-size: 0.75rem;
      padding: 0.25rem 0.6rem;
      border-radius: 4px;
      font-weight: 600;
      text-transform: uppercase;
    }

    /* Grid Layouts */
    .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 1.5rem; }
    .grid-3 { display: grid; grid-template-columns: repeat(3, 1fr); gap: 1.5rem; }
    .grid-4 { display: grid; grid-template-columns: repeat(4, 1fr); gap: 1rem; }

    /* Metric Cards */
    .metric-card {
      background: rgba(255, 255, 255, 0.02);
      border: 1px solid var(--border-color);
      border-radius: 6px;
      padding: 1.25rem;
    }

    .metric-title {
      font-size: 0.8rem;
      color: var(--text-muted);
      text-transform: uppercase;
      margin-bottom: 0.5rem;
    }

    .metric-value {
      font-size: 1.8rem;
      font-weight: 700;
      color: var(--text-main);
    }

    .metric-subtext {
      font-size: 0.8rem;
      margin-top: 0.25rem;
    }

    .text-green { color: var(--accent-green); }
    .text-red { color: var(--accent-red); }
    .text-cyan { color: var(--accent-cyan); }

    /* Slider Styling */
    .control-group {
      margin-bottom: 1rem;
    }

    .control-group label {
      display: flex;
      justify-content: space-between;
      font-size: 0.85rem;
      color: var(--text-muted);
      margin-bottom: 0.3rem;
    }

    input[type=range] {
      width: 100%;
      accent-color: var(--accent-cyan);
    }

    /* Tab Navigation */
    .tab-nav {
      display: flex;
      gap: 0.5rem;
      margin-bottom: 1.5rem;
      background: rgba(0,0,0,0.2);
      padding: 0.3rem;
      border-radius: 6px;
    }

    .tab-btn {
      background: none;
      border: none;
      color: var(--text-muted);
      padding: 0.5rem 1rem;
      cursor: pointer;
      font-weight: 600;
      font-size: 0.85rem;
      border-radius: 4px;

    }

    .tab-btn.active {
      background: var(--accent-blue);
      color: var(--text-main);
    }

    /* Tables */
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 1rem;
      font-size: 0.85rem;
    }

    th, td {
      padding: 0.75rem;
      text-align: left;
      border-bottom: 1px solid var(--border-color);
    }

    th {
      color: var(--text-muted);
      font-weight: 600;
    }

    /* Flowchart Process Step Cards */
    .process-step {
      background: rgba(255, 255, 255, 0.02);
      border: 1px solid var(--border-color);
      border-left: 3px solid var(--accent-cyan);
      padding: 1rem;
      border-radius: 4px;
    }
  </style>
</head>
<body>

  <!-- Header Nav -->
  <header>
    <div class="brand">
      Sanju Nivasini
      <span>FP&A Decision Intelligence</span>
    </div>
    <nav>
      <a href="#hero">CASE STUDY</a>
      <a href="#forecasting">FORECASTING</a>
      <a href="#variance">VARIANCE</a>
      <a href="#scenario">SCENARIO</a>
      <a href="#modeling">MODELING</a>
      <a href="#executive">EXECUTIVE PACK</a>
      <a href="#credentials">CREDENTIALS</a>
    </nav>
  </header>

  <div class="container">

    <!-- HERO SECTION -->
    <section id="hero" class="section">
      <div class="grid-2">
        <div>
          <span class="tag">FP&A | Strategic Finance | Decision Intelligence</span>
          <h1 style="font-size: 2.5rem; margin: 1rem 0; line-height: 1.2;">Transforming Financial Data into Executive Decisions</h1>
          <p style="color: var(--text-muted); margin-bottom: 1.5rem;">
            Certified FP&A and Financial Modeling professional with nearly 5 years of experience in forecasting, variance analysis, margin improvement, scenario planning, portfolio governance, and executive reporting.
          </p>
          <div style="font-size: 0.85rem; color: var(--accent-cyan); font-weight: 600;">
            MBA FINANCE | FP&A™ | FMVA® | CFI CERTIFIED PROFESSIONAL | STRATEGIC FINANCE
          </div>
        </div>
        <div class="grid-2">
          <div class="metric-card">
            <div class="metric-title">Revenue Governed</div>
            <div class="metric-value">$119M+</div>
            <div class="metric-subtext text-cyan">Financial planning & forecasting across 127 accounts</div>
          </div>
          <div class="metric-card">
            <div class="metric-title">Margin Expansion</div>
            <div class="metric-value text-green">+11.4pp</div>
            <div class="metric-subtext text-green">Improved operating margin from 4.0% to 15.4%</div>
          </div>
          <div class="metric-card">
            <div class="metric-title">Accounts Managed</div>
            <div class="metric-value">127</div>
            <div class="metric-subtext">European business portfolio</div>
          </div>
          <div class="metric-card">
            <div class="metric-title">Leadership Trust</div>
            <div class="metric-value text-green">Critical</div>
            <div class="metric-subtext">Trusted contributor for high-impact decision support</div>
          </div>
        </div>
      </div>
    </section>

    <!-- FLAGSHIP CASE STUDY -->
    <section class="section">
      <div class="section-header">
        <h2>Margin Transformation Program</h2>
        <span class="tag">Flagship Case Study</span>
      </div>
      <div class="grid-2">
        <div>
          <canvas id="marginChart" style="max-height: 250px;"></canvas>
        </div>
        <div>
          <div class="grid-2" style="margin-bottom: 1rem;">
            <div class="metric-card">
              <div class="metric-title">Starting Margin</div>
              <div class="metric-value">4.0%</div>
            </div>
            <div class="metric-card">
              <div class="metric-title">Ending Margin</div>
              <div class="metric-value text-green">15.4%</div>
            </div>
          </div>
          <table>
            <thead>
              <tr>
                <th>Analysis Area</th>
                <th>Recommendation</th>
                <th>Financial Impact</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>Revenue Drivers</td>
                <td>Prioritize high-margin accounts</td>
                <td class="text-green">+200 bps</td>
              </tr>
              <tr>
                <td>Cost Structure</td>
                <td>Renegotiate vendor rates</td>
                <td class="text-green">+$4.0M</td>
              </tr>
              <tr>
                <td>Resource Mix</td>
                <td>Shift work to internal teams</td>
                <td class="text-green">+$5.1M</td>
              </tr>
              <tr>
                <td>Utilization</td>
                <td>Reallocate capacity</td>
                <td class="text-green">+$2.2M</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </section>

    <!-- FORECASTING COMMAND CENTER -->
    <section id="forecasting" class="section">
      <div class="section-header">
        <h2>Strategic Planning & Forecasting Command Center</h2>
        <span class="tag">Forecasting Engine</span>
      </div>
      <div class="grid-4" style="margin-bottom: 1.5rem;">
        <div class="metric-card">
          <div class="metric-title">Revenue Outlook</div>
          <div class="metric-value">$862.4M</div>
          <div class="metric-subtext text-green">+$82.4M vs Prior Baseline</div>
        </div>
        <div class="metric-card">
          <div class="metric-title">Cost Outlook</div>
          <div class="metric-value">$598.6M</div>
          <div class="metric-subtext text-green">-2.3% Efficiency Gain</div>
        </div>
        <div class="metric-card">
          <div class="metric-title">Operating Margin</div>
          <div class="metric-value">32.6%</div>
          <div class="metric-subtext text-green">+180 bps</div>
        </div>
        <div class="metric-card">
          <div class="metric-title">Cash Generation</div>
          <div class="metric-value">$134M</div>
          <div class="metric-subtext">Projected Cash Flow</div>
        </div>
      </div>
      <div class="grid-2">
        <div>
          <h4 style="margin-bottom: 1rem; color: var(--text-muted);">Revenue Build-Up Waterfall ($M)</h4>
          <canvas id="waterfallChart" style="max-height: 250px;"></canvas>
        </div>
        <div>
          <h4 style="margin-bottom: 1rem; color: var(--text-muted);">Margin Expansion Bridge (+420 bps)</h4>
          <canvas id="bridgeChart" style="max-height: 250px;"></canvas>
        </div>
      </div>
    </section>

    <!-- SCENARIO ENGINE (INTERACTIVE) -->
    <section id="scenario" class="section">
      <div class="section-header">
        <h2>Assumption Sliders with Profit, Margin, and Cash Impact</h2>
        <span class="tag">Interactive Financial Model</span>
      </div>
      <div class="grid-2">
        <div>
          <div class="control-group">
            <label>Revenue Change <span id="revVal">0%</span></label>
            <input type="range" id="revSlider" min="-20" max="20" value="0">
          </div>
          <div class="control-group">
            <label>Salary Change <span id="salVal">0%</span></label>
            <input type="range" id="salSlider" min="-10" max="10" value="0">
          </div>
          <div class="control-group">
            <label>Subcon Change <span id="subVal">0%</span></label>
            <input type="range" id="subSlider" min="-20" max="20" value="0">
          </div>
          <div class="control-group">
            <label>Utilization Change <span id="utilVal">0%</span></label>
            <input type="range" id="utilSlider" min="-10" max="10" value="0">
          </div>
        </div>
        <div class="grid-2">
          <div class="metric-card">
            <div class="metric-title">Projected Revenue</div>
            <div class="metric-value" id="outRevenue">$119.0M</div>
          </div>
          <div class="metric-card">
            <div class="metric-title">Projected Operating Profit</div>
            <div class="metric-value text-green" id="outProfit">$18.3M</div>
          </div>
          <div class="metric-card">
            <div class="metric-title">Projected Margin</div>
            <div class="metric-value text-cyan" id="outMargin">15.4%</div>
          </div>
          <div class="metric-card">
            <div class="metric-title">Projected Cash Flow</div>
            <div class="metric-value text-green" id="outCash">$16.8M</div>
          </div>
        </div>
      </div>
    </section>

    <!-- MODELING & VALUATION SHOWCASE -->
    <section id="modeling" class="section">
      <div class="section-header">
        <h2>Strategic Finance Project Portfolio</h2>
        <span class="tag">Valuation & DCF</span>
      </div>
      
      <div class="tab-nav">
        <button class="tab-btn active" onclick="switchTab('earnings')">Tesla Earnings Analysis</button>
        <button class="tab-btn" onclick="switchTab('dcf')">Tesla DCF Valuation</button>
      </div>

      <!-- Tab 1: Tesla Earnings -->
      <div id="tab-earnings">
        <h3 style="margin-bottom: 0.5rem;">Tesla Q1 FY2026 Earnings Analysis & Executive Financial Review</h3>
        <p style="color: var(--text-muted); margin-bottom: 1.5rem;">Analyzed Tesla Q1 FY2026 actual performance against Wall Street consensus and prior year performance.</p>
        <div class="grid-4" style="margin-bottom: 1.5rem;">
          <div class="metric-card">
            <div class="metric-title">Total Revenue</div>
            <div class="metric-value">$22.4B</div>
            <div class="metric-subtext text-green">Consensus: $21.1B (+1.3B)</div>
          </div>
          <div class="metric-card">
            <div class="metric-title">Gross Profit</div>
            <div class="metric-value">$4.7B</div>
            <div class="metric-subtext text-green">Gross Margin: 21.1%</div>
          </div>
          <div class="metric-card">
            <div class="metric-title">Operating Income</div>
            <div class="metric-value">$941M</div>
            <div class="metric-subtext">Operating Margin: 4.2%</div>
          </div>
          <div class="metric-card">
            <div class="metric-title">Free Cash Flow</div>
            <div class="metric-value text-green">$1.44B</div>
            <div class="metric-subtext text-green">Beat Consensus by +$3.22B</div>
          </div>
        </div>
      </div>

      <!-- Tab 2: DCF Valuation -->
      <div id="tab-dcf" style="display: none;">
        <h3 style="margin-bottom: 0.5rem;">Tesla DCF Valuation & Intrinsic Value Assessment</h3>
        <p style="color: var(--text-muted); margin-bottom: 1.5rem;">5-Year Operating Model & Sensitivity Analysis.</p>
        <div class="grid-3" style="margin-bottom: 1.5rem;">
          <div class="metric-card">
            <div class="metric-title">Base Fair Value</div>
            <div class="metric-value">$285</div>
            <div class="metric-subtext">Current Price: $250</div>
          </div>
          <div class="metric-card">
            <div class="metric-title">Best Case</div>
            <div class="metric-value text-green">$365</div>
            <div class="metric-subtext">Software/Autonomy Upside</div>
          </div>
          <div class="metric-card">
            <div class="metric-title">Worst Case</div>
            <div class="metric-value text-red">$190</div>
            <div class="metric-subtext">Execution Delay Downside</div>
          </div>
        </div>
      </div>
    </section>

    <!-- CREDENTIALS & TOOLKIT -->
    <section id="credentials" class="section">
      <div class="section-header">
        <h2>Tools & Credentials</h2>
        <span class="tag">FP&A Toolkit</span>
      </div>
      <div class="grid-3">
        <div class="metric-card">
          <h4 style="color: var(--accent-cyan); margin-bottom: 0.5rem;">Excel & Financial Modeling</h4>
          <p style="font-size: 0.85rem; color: var(--text-muted);">Advanced 3-statement models, DCF, scenario planning, dynamic variance engines, and complex logic.</p>
        </div>
        <div class="metric-card">
          <h4 style="color: var(--accent-cyan); margin-bottom: 0.5rem;">Power BI & Analytics</h4>
          <p style="font-size: 0.85rem; color: var(--text-muted);">Executive dashboard design, automated DAX metrics, interactive drill-downs, and trend tracking.</p>
        </div>
        <div class="metric-card">
          <h4 style="color: var(--accent-cyan); margin-bottom: 0.5rem;">SQL & Data Analytics</h4>
          <p style="font-size: 0.85rem; color: var(--text-muted);">Data extraction, aggregation, pipeline analysis, and reconciliation of complex datasets.</p>
        </div>
      </div>
    </section>

    <!-- DECISION INTELLIGENCE CENTER (PROCESS) -->
    <section class="section">
      <div class="section-header">
        <h2>Business Problem to Financial Impact Workflow</h2>
        <span class="tag">Methodology</span>
      </div>
      <div class="grid-4">
        <div class="process-step">
          <div style="font-size: 0.75rem; color: var(--text-muted);">STEP 1</div>
          <div style="font-weight: 700; margin: 0.25rem 0;">Business Problem</div>
          <p style="font-size: 0.8rem; color: var(--text-muted);">Identify margin decline and cost leakage drivers.</p>
        </div>
        <div class="process-step">
          <div style="font-size: 0.75rem; color: var(--text-muted);">STEP 2</div>
          <div style="font-weight: 700; margin: 0.25rem 0;">Financial Analysis</div>
          <p style="font-size: 0.8rem; color: var(--text-muted);">Deconstruct variance across cost categories.</p>
        </div>
        <div class="process-step">
          <div style="font-size: 0.75rem; color: var(--text-muted);">STEP 3</div>
          <div style="font-weight: 700; margin: 0.25rem 0;">Recommendation</div>
          <p style="font-size: 0.8rem; color: var(--text-muted);">Formulate recovery initiatives & pricing fixes.</p>
        </div>
        <div class="process-step">
          <div style="font-size: 0.75rem; color: var(--text-muted);">STEP 4</div>
          <div style="font-weight: 700; margin: 0.25rem 0;">Financial Impact</div>
          <p style="font-size: 0.8rem; color: var(--text-muted);">Deliver +11.4pp margin expansion & cash growth.</p>
        </div>
      </div>
    </section>

  </div>

  <script>
    // Tab Switcher Logic
    function switchTab(tabId) {
      document.getElementById('tab-earnings').style.display = tabId === 'earnings' ? 'block' : 'none';
      document.getElementById('tab-dcf').style.display = tabId === 'dcf' ? 'block' : 'none';
      
      const buttons = document.querySelectorAll('.tab-btn');
      buttons.forEach(btn => btn.classList.remove('active'));
      event.target.classList.add('active');
    }

    // Chart.js Implementations
    window.addEventListener('DOMContentLoaded', () => {
      // 1. Flagship Margin Chart
      new Chart(document.getElementById('marginChart'), {
        type: 'bar',
        data: {
          labels: ['Baseline', 'Revenue Recovery', 'Cost Optimization', 'Resource Mix', 'Utilization', 'Governance', 'Final'],
          datasets: [{
            label: 'Operating Margin %',
            data: [4.0, 6.0, 10.0, 12.5, 14.0, 14.5, 15.4],
            backgroundColor: '#2563eb'
          }]
        },
        options: {
          responsive: true,
          plugins: { legend: { display: false } },
          scales: { y: { ticks: { color: '#94a3b8' } }, x: { ticks: { color: '#94a3b8' } } }
        }
      });

      // 2. Revenue Build-Up Waterfall Chart
      new Chart(document.getElementById('waterfallChart'), {
        type: 'bar',
        data: {
          labels: ['Run Rate', 'Renewals', 'New Wins', 'Price Impact', 'Attrition', 'Forecast'],
          datasets: [{
            label: 'Revenue ($M)',
            data: [780, 835, 908, 893, 862, 862],
            backgroundColor: ['#2563eb', '#10b981', '#10b981', '#ef4444', '#ef4444', '#06b6d4']
          }]
        },
        options: {
          responsive: true,
          plugins: { legend: { display: false } },
          scales: { y: { ticks: { color: '#94a3b8' } }, x: { ticks: { color: '#94a3b8' } } }
        }
      });

      // 3. Margin Expansion Bridge Chart
      new Chart(document.getElementById('bridgeChart'), {
        type: 'bar',
        data: {
          labels: ['Starting', 'Pricing', 'Utilization', 'Subcon', 'Automation', 'Final'],
          datasets: [{
            label: 'Margin Gap (%)',
            data: [28.4, 30.0, 31.3, 32.2, 31.7, 32.6],
            backgroundColor: '#06b6d4'
          }]
        },
        options: {
          responsive: true,
          plugins: { legend: { display: false } },
          scales: { y: { ticks: { color: '#94a3b8' } }, x: { ticks: { color: '#94a3b8' } } }
        }
      });
    });

    // Interactive Slider Logic
    const baseRev = 119.0;
    const baseProfit = 18.3;
    const baseMargin = 15.4;
    const baseCash = 16.8;

    const revSlider = document.getElementById('revSlider');
    const salSlider = document.getElementById('salSlider');
    const subSlider = document.getElementById('subSlider');
    const utilSlider = document.getElementById('utilSlider');

    function updateCalculations() {
      const rVal = parseFloat(revSlider.value);
      const sVal = parseFloat(salSlider.value);
      const subVal = parseFloat(subSlider.value);
      const uVal = parseFloat(utilSlider.value);

      document.getElementById('revVal').innerText = rVal + '%';
      document.getElementById('salVal').innerText = sVal + '%';
      document.getElementById('subVal').innerText = subVal + '%';
      document.getElementById('utilVal').innerText = uVal + '%';

      const newRev = baseRev * (1 + rVal / 100);
      const profitDelta = (rVal * 0.5) - (sVal * 0.3) - (subVal * 0.2) + (uVal * 0.4);
      const newProfit = baseProfit + profitDelta;
      const newMargin = (newProfit / newRev) * 100;
      const newCash = baseCash + (profitDelta * 0.85);

      document.getElementById('outRevenue').innerText = '$' + newRev.toFixed(1) + 'M';
      document.getElementById('outProfit').innerText = '$' + newProfit.toFixed(1) + 'M';
      document.getElementById('outMargin').innerText = newMargin.toFixed(1) + '%';
      document.getElementById('outCash').innerText = '$' + newCash.toFixed(1) + 'M';
    }

    [revSlider, salSlider, subSlider, utilSlider].forEach(slider => {
      slider.addEventListener('input', updateCalculations);
    });
  </script>
</body>
</html>
