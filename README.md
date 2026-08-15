<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Sanju Nivasini | FP&A Decision Intelligence</title>
  
  <!-- Chart.js Library via CDN -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  
  <style>
    /* -----------------------------------------------------------
       1. Design System & Global Styles
    ----------------------------------------------------------- */
    :root {
      /* Palette based on portfolio images */
      --bg-dark: #0b0f19;         /* Main background */
      --bg-card: #131b2e;         /* Card background */
      --bg-card-hover: #1a253e;   /* Hovered card background */
      
      --accent-blue: #2563eb;     /* Main blue brand color */
      --accent-cyan: #06b6d4;     /* For metrics and labels */
      --accent-green: #10b981;    /* For positive numbers/outcomes */
      --accent-red: #ef4444;      /* For negative numbers/risks */
      
      --text-main: #f8fafc;       /* Brightest white for text */
      --text-muted: #94a3b8;      /* Lighter gray/blue text */
      --border-subtle: #1e293b;   /* Faint separator lines */
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
      padding-bottom: 60px; /* Offset for footer */
    }

    /* Standard Utilities */
    .container {
      max-width: 1200px;
      margin: 2rem auto;
      padding: 0 1.5rem;
    }

    .section {
      margin-bottom: 4rem;
      background: var(--bg-card);
      border: 1px solid var(--border-subtle);
      border-radius: 8px;
      padding: 2.5rem;
    }

    .tag {
      background: rgba(37, 99, 235, 0.2);
      color: var(--accent-cyan);
      font-size: 0.75rem;
      padding: 0.25rem 0.6rem;
      border-radius: 4px;
      font-weight: 600;
      text-transform: uppercase;
      letter-spacing: 0.5px;
    }

    /* Common Colors */
    .text-green { color: var(--accent-green) !important; }
    .text-red { color: var(--accent-red) !important; }
    .text-cyan { color: var(--accent-cyan) !important; }

    /* Layout Grids */
    .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 1.5rem; }
    .grid-3 { display: grid; grid-template-columns: repeat(3, 1fr); gap: 1.5rem; }
    .grid-4 { display: grid; grid-template-columns: repeat(4, 1fr); gap: 1.25rem; }

    /* Text Alignment */
    .text-right { text-align: right; }

    /* -----------------------------------------------------------
       2. Header & Navigation
    ----------------------------------------------------------- */
    header {
      position: sticky;
      top: 0;
      z-index: 1000;
      background: rgba(11, 15, 25, 0.9);
      backdrop-filter: blur(10px);
      border-bottom: 1px solid var(--border-subtle);
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
      transition: all 0.2s ease;
    }

    nav a:hover {
      color: var(--text-main);
      background-color: rgba(255, 255, 255, 0.05);
    }

    /* -----------------------------------------------------------
       3. Metric Card Styling
    ----------------------------------------------------------- */
    .metric-card {
      background: rgba(255, 255, 255, 0.02);
      border: 1px solid var(--border-subtle);
      border-radius: 6px;
      padding: 1.25rem;
    }

    .metric-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 0.5rem;
    }

    .metric-title {
      font-size: 0.8rem;
      color: var(--text-muted);
      text-transform: uppercase;
      font-weight: 500;
      letter-spacing: 0.5px;
    }

    .metric-value {
      font-size: 2rem;
      font-weight: 700;
      color: var(--text-main);
    }

    .metric-subtext {
      font-size: 0.8rem;
      color: var(--text-muted);
      margin-top: 0.25rem;
    }

    /* -----------------------------------------------------------
       4. Scenario Model Sliders
    ----------------------------------------------------------- */
    .control-group {
      margin-bottom: 1.25rem;
    }

    .control-group label {
      display: flex;
      justify-content: space-between;
      font-size: 0.85rem;
      color: var(--text-muted);
      margin-bottom: 0.4rem;
    }

    .slider-container input[type=range] {
      width: 100%;
      accent-color: var(--accent-cyan);
    }

    /* -----------------------------------------------------------
       5. Process Flowchart Cards
    ----------------------------------------------------------- */
    .workflow-step {
      background: rgba(255, 255, 255, 0.015);
      border: 1px solid var(--border-subtle);
      border-left: 3px solid var(--accent-cyan);
      padding: 1.25rem;
      border-radius: 4px;
    }

    .step-number {
      font-size: 0.7rem;
      color: var(--accent-cyan);
      text-transform: uppercase;
      font-weight: 600;
      margin-bottom: 0.25rem;
    }

    .step-title {
      font-weight: 700;
      font-size: 1rem;
      margin-bottom: 0.5rem;
    }

    .step-desc {
      font-size: 0.8rem;
      color: var(--text-muted);
    }

    /* -----------------------------------------------------------
       6. Tab Navigation
    ----------------------------------------------------------- */
    .tabs-wrapper {
      margin-bottom: 1.5rem;
    }

    .tab-nav {
      display: flex;
      gap: 0.5rem;
      background: rgba(0,0,0,0.2);
      padding: 0.4rem;
      border-radius: 6px;
    }

    .tab-btn {
      background: none;
      border: none;
      color: var(--text-muted);
      padding: 0.5rem 1.2rem;
      cursor: pointer;
      font-weight: 600;
      font-size: 0.85rem;
      border-radius: 4px;
      transition: all 0.2s ease;
    }

    .tab-btn:hover {
      color: var(--text-main);
    }

    .tab-btn.active {
      background: var(--accent-blue);
      color: var(--text-main);
    }

    .tab-content {
      display: none;
    }

    .tab-content.active {
      display: block;
    }

    /* -----------------------------------------------------------
       7. General Elements
    ----------------------------------------------------------- */
    canvas {
      max-width: 100%;
    }

    /* Footer styling */
    footer {
      position: fixed;
      bottom: 0;
      left: 0;
      width: 100%;
      background: var(--bg-card);
      border-top: 1px solid var(--border-subtle);
      text-align: center;
      padding: 0.75rem;
      font-size: 0.8rem;
      color: var(--text-muted);
    }
  </style>
</head>
<body>

  <!-- 1. HEADER NAV -->
  <header>
    <div class="brand">
      Sanju Nivasini
      <span>FP&A Decision Intelligence</span>
    </div>
    <nav>
      <a href="#hero">HOME</a>
      <a href="#flagship">FLAGSHIP</a>
      <a href="#scenario">SCENARIO ENGINE</a>
      <a href="#portfolio">PROJECTS</a>
      <a href="#modeling">MODELING</a>
    </nav>
  </header>

  <div class="container">

    <!-- 2. HERO SECTION -->
    <section id="hero" class="section">
      <div class="grid-2">
        <div>
          <span class="tag">MBA FINANCE | FP&A™ | FMVA®</span>
          <h1 style="font-size: 2.8rem; margin: 1rem 0; line-height: 1.1;">Transforming Financial Data into Executive Decisions</h1>
          <p style="color: var(--text-muted); margin-bottom: 1.5rem; max-width: 500px;">
            Nearly 5 years of specialized experience across financial modeling, forecasting, scenario analysis, margin improvement, and executive reporting for multi-currency portfolios.
          </p>
          <p style="font-size: 0.85rem; color: var(--accent-cyan); font-weight: 600;">
            CFI CERTIFIED PROFESSIONAL | FINANCIAL PLANNING & ANALYSIS (FP&A)
          </p>
        </div>
        <div class="grid-2">
          <div class="metric-card">
            <div class="metric-title">Revenue Governed</div>
            <div class="metric-value">$119M+</div>
            <div class="metric-subtext">Portfolio financial planning & performance management</div>
          </div>
          <div class="metric-card">
            <div class="metric-title">Margin Transformation</div>
            <div class="metric-value text-green">+11.4pp</div>
            <div class="metric-subtext text-green">Operating margin increased from 4.0% to 15.4%</div>
          </div>
          <div class="metric-card">
            <div class="metric-title">Accounts Governed</div>
            <div class="metric-value">127</div>
            <div class="metric-subtext">European business account management</div>
          </div>
          <div class="metric-card">
            <div class="metric-title">Leadership Trust</div>
            <div class="metric-value text-green">High</div>
            <div class="metric-subtext">Contributor for high-impact support</div>
          </div>
        </div>
      </div>
    </section>

    <!-- 3. FLAGSHIP CASE STUDY SECTION -->
    <section id="flagship" class="section">
      <div class="section-header grid-2" style="margin-bottom: 2rem;">
        <h2>Flagship Project: Margin Transformation</h2>
        <span class="tag text-right" style="align-self: flex-start; justify-self: flex-end;">Case Study</span>
      </div>
      <div class="grid-2">
        <div>
          <h4 style="margin-bottom: 1rem; color: var(--text-muted);">Margin Transformation Waterfall Chart</h4>
          <canvas id="flagshipMarginChart" style="height: 250px;"></canvas>
        </div>
        <div>
          <div class="grid-2" style="margin-bottom: 1.5rem;">
            <div class="metric-card">
              <div class="metric-title">Starting Margin</div>
              <div class="metric-value">4.0%</div>
              <div class="metric-subtext">Initial baseline performance</div>
            </div>
            <div class="metric-card">
              <div class="metric-title">Final Margin</div>
              <div class="metric-value text-green">15.4%</div>
              <div class="metric-subtext text-green">+11.4 percentage points total expansion</div>
            </div>
          </div>
          <div style="background: rgba(0,0,0,0.1); padding: 1.25rem; border-radius: 4px; border: 1px solid var(--border-subtle); font-size: 0.85rem; color: var(--text-muted);">
            <p><strong>Impact of Initiatives:</strong> Prioritizing high-margin accounts (+200 bps), renegotiating vendor rates (+$4.0M), shifting to internal teams (+$5.1M), and resource reallocation (+$2.2M) were critical drivers.</p>
          </div>
        </div>
      </div>
    </section>

    <!-- 4. SCENARIO ENGINE (INTERACTIVE) SECTION -->
    <section id="scenario" class="section">
      <div class="section-header grid-2" style="margin-bottom: 2rem;">
        <h2>Scenario Planning & Sensitivity Analysis Engine</h2>
        <span class="tag text-right" style="align-self: flex-start; justify-self: flex-end;">Interactive Model</span>
      </div>
      <div class="grid-2" style="gap: 2.5rem;">
        <div class="slider-container">
          <h4 style="margin-bottom: 1rem; color: var(--text-muted);">Set Assumption Changes</h4>
          <div class="control-group">
            <label>Revenue Variance <span id="val_rev">0%</span></label>
            <input type="range" id="sl_rev" min="-25" max="25" value="0">
          </div>
          <div class="control-group">
            <label>Subcon Rate Variance <span id="val_sub">0%</span></label>
            <input type="range" id="sl_sub" min="-15" max="15" value="0">
          </div>
          <div class="control-group">
            <label>Utilization Variance <span id="val_util">0%</span></label>
            <input type="range" id="sl_util" min="-10" max="10" value="0">
          </div>
          <div class="control-group">
            <label>Attrition Change <span id="val_att">0 pts</span></label>
            <input type="range" id="sl_att" min="-5" max="5" value="0">
          </div>
        </div>
        <div>
          <h4 style="margin-bottom: 1rem; color: var(--text-muted);">Real-Time Projected Financial Impact</h4>
          <div class="grid-2">
            <div class="metric-card">
              <div class="metric-title">Projected Revenue</div>
              <div class="metric-value" id="proj_rev">$119.0M</div>
            </div>
            <div class="metric-card">
              <div class="metric-title">Projected Profit</div>
              <div class="metric-value text-green" id="proj_prof">$18.3M</div>
            </div>
            <div class="metric-card">
              <div class="metric-title">Projected Margin</div>
              <div class="metric-value text-cyan" id="proj_marg">15.4%</div>
            </div>
            <div class="metric-card">
              <div class="metric-title">Projected Cash Flow</div>
              <div class="metric-value text-green" id="proj_cash">$16.8M</div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- 5. PORTFOLIO & MODELING SECTIONS (Tabs) -->
    <section id="portfolio" class="sectionTabs tabs-wrapper">
      <div class="tab-nav">
        <button class="tab-btn active" data-tab="forecasting_tab">Forecasting Command Center</button>
        <button class="tab-btn" data-tab="variance_tab">Variance Analysis Lab</button>
        <button class="tab-btn" data-tab="credentials_tab">Tools & Credentials</button>
      </div>

      <!-- A. Forecasting Tab Content -->
      <div id="forecasting_tab" class="tab-content active">
        <div class="section">
          <h2>Forecasting Command Center Overview</h2>
          <p style="color: var(--text-muted); margin: 1rem 0 2rem 0; max-width: 600px;">Driver-based forecasting models providing clear projections for strategic revenue, cost, and margin expectations.</p>
          <div class="grid-4" style="margin-bottom: 2rem;">
            <div class="metric-card">
              <div class="metric-title">Revenue Outlook</div>
              <div class="metric-value">$862.4M</div>
              <div class="metric-subtext text-green">+$39M favorable (Forecast accuracy: 96.8%)</div>
            </div>
            <div class="metric-card">
              <div class="metric-title">Cost Outlook</div>
              <div class="metric-value">$598.6M</div>
              <div class="metric-subtext text-green">-$2.3% Efficiency gain</div>
            </div>
            <div class="metric-card">
              <div class="metric-title">Operating Margin</div>
              <div class="metric-value text-green">32.6%</div>
              <div class="metric-subtext text-green">+180 bps total expansion</div>
            </div>
            <div class="metric-card">
              <div class="metric-title">Forecast Confidence</div>
              <div class="metric-value text-green">High</div>
              <div class="metric-subtext">96.8% revenue accuracy anchored on drivers</div>
            </div>
          </div>
          <div class="grid-2">
            <div>
              <h4 style="margin-bottom: 1rem; color: var(--text-muted);">Revenue Build-Up Waterfall ($M)</h4>
              <canvas id="revWaterfall" style="height: 250px;"></canvas>
            </div>
            <div>
              <h4 style="margin-bottom: 1rem; color: var(--text-muted);">Margin Expansion Bridge (%)</h4>
              <canvas id="margBridge" style="height: 250px;"></canvas>
            </div>
          </div>
        </div>
      </div>

      <!-- B. Variance Analysis Tab Content -->
      <div id="variance_tab" class="tab-content">
        <div class="section">
          <h2>Variance Analysis Lab</h2>
          <p style="color: var(--text-muted); margin-top: 1rem;">Tracking detailed Actual vs. Budget performance to identify cost leakage and profit opportunities.</p>
          <div class="grid-2" style="margin-top: 2rem;">
            <div>
              <h4 style="margin-bottom: 1rem; color: var(--text-muted);">Budget vs. Actual Variance ($M)</h4>
              <canvas id="budgetActualBar" style="height: 300px;"></canvas>
            </div>
            <div class="metric-card" style="margin-top: 2rem;">
              <div class="metric-title">Profit Variance</div>
              <div class="metric-value text-red">-$3.4M</div>
              <div class="metric-subtext text-red">Unfavorable to budget, driven by higher COGS and cost pressure</div>
              <p style="margin-top: 1rem; color: var(--text-muted); font-size: 0.85rem;"><strong>Analysis Breakdown:</strong> Significant unfavorable variance in COGS and contractor spend due to mix shift. The analysis points to the need for stricter subcon discipline and pricing realignment.</p>
            </div>
          </div>
        </div>
      </div>

      <!-- C. Credentials Tab Content -->
      <div id="credentials_tab" class="tab-content">
        <div class="section">
          <h2>FP&A Toolkit, Credentials, & Education</h2>
          <div class="grid-3" style="margin-top: 2rem;">
            <div class="metric-card">
              <h4 style="color: var(--accent-cyan);">MBA FINANCE</h4>
              <p style="font-size: 0.9rem; margin-top: 0.5rem;">Corporate Finance | Decision Making</p>
            </div>
            <div class="metric-card">
              <h4 style="color: var(--accent-cyan);">CFI FP&A™ Professional</h4>
              <p style="font-size: 0.9rem; margin-top: 0.5rem;">Financial Planning & Analysis Certified Professional</p>
            </div>
            <div class="metric-card">
              <h4 style="color: var(--accent-cyan);">CFI FMVA® Analyst</h4>
              <p style="font-size: 0.9rem; margin-top: 0.5rem;">Financial Modeling & Valuation Analyst Credential</p>
            </div>
          </div>
          <h4 style="margin: 2rem 0 1rem 0; color: var(--text-muted);">Core Technical Skills Demonstrated</h4>
          <div class="grid-4">
            <div style="background: rgba(255,255,255,0.015); padding: 1rem; border-radius: 4px; border: 1px solid var(--border-subtle);">Excel & Financial Modeling</div>
            <div style="background: rgba(255,255,255,0.015); padding: 1rem; border-radius: 4px; border: 1px solid var(--border-subtle);">Power BI & Executive Dashboards</div>
            <div style="background: rgba(255,255,255,0.015); padding: 1rem; border-radius: 4px; border: 1px solid var(--border-subtle);">SQL & Data Validation</div>
            <div style="background: rgba(255,255,255,0.015); padding: 1rem; border-radius: 4px; border: 1px solid var(--border-subtle);">Power Query & Automation</div>
          </div>
        </div>
      </div>
    </section>

    <!-- 6. DECISION INTELLIGENCE WORKFLOW SECTION -->
    <section class="section">
      <div class="section-header grid-2" style="margin-bottom: 2rem;">
        <h2>Financial Impact Workflow (Decision Intelligence Cycle)</h2>
        <span class="tag text-right" style="align-self: flex-start; justify-self: flex-end;">Core Methodology</span>
      </div>
      <div class="grid-3" style="gap: 1.5rem;">
        <div class="workflow-step">
          <div class="step-number">Step 1</div>
          <div class="step-title">Business Problem</div>
          <p class="step-desc">Identifying margin decline & forecasting confidence needs stronger evidence.</p>
        </div>
        <div class="workflow-step">
          <div class="step-number">Step 2</div>
          <div class="step-title">Financial Analysis</div>
          <p class="step-desc">Deconstructing variance across revenue, cost categories, mix, and accounts.</p>
        </div>
        <div class="workflow-step">
          <div class="step-number">Step 3</div>
          <div class="step-title">Insights Generation</div>
          <p class="step-desc">Isolating root causes: subcon mix, cost escalation, utilization gaps, and billing leakage.</p>
        </div>
        <div class="workflow-step">
          <div class="step-number">Step 4</div>
          <div class="step-title">Recommendation</div>
          <p class="step-desc">Prioritizing high-margin segments, vendor renegotiations, utilization governance, and accelerated collections.</p>
        </div>
        <div class="workflow-step">
          <div class="step-number">Step 5</div>
          <div class="step-title">Decision Alignment</div>
          <p class="step-desc">Securing leadership approval for quantified recovery actions and forecast governance routines.</p>
        </div>
        <div class="workflow-step">
          <div class="step-number">Step 6</div>
          <div class="step-title">Financial Impact</div>
          <p class="step-desc">Achieving +11.4 percentage point operating margin improvement with clear visibility.</p>
        </div>
      </div>
    </section>

  </div>

  <!-- 7. FIXED FOOTER -->
  <footer>
    Sanju Nivasini Portfolio © <span id="currentYear"></span> | FP&A Decision Intelligence | Single-File HTML Replica
  </footer>

  <script>
    // Set Footer Year
    document.getElementById('currentYear').textContent = new Date().getFullYear();

    // -----------------------------------------------------------
    // Tabs Functionality
    // -----------------------------------------------------------
    const tabButtons = document.querySelectorAll('.tab-btn');
    const tabContents = document.querySelectorAll('.tab-content');

    tabButtons.forEach(button => {
      button.addEventListener('click', () => {
        // Remove active class from all buttons and contents
        tabButtons.forEach(btn => btn.classList.remove('active'));
        tabContents.forEach(content => content.classList.remove('active'));

        // Add active class to current button and target content
        button.classList.add('active');
        document.getElementById(button.dataset.tab).classList.add('active');
      });
    });

    // -----------------------------------------------------------
    // Charts.js Initialization
    // -----------------------------------------------------------
    
    // 1. Flagship Margin Waterfall (Simplified as a Bar with different colors for this replica)
    new Chart(document.getElementById('flagshipMarginChart'), {
      type: 'bar',
      data: {
        labels: ['Baseline', 'Revenue Recovery', 'Cost Optimization', 'Resource Mix', 'Utilization', 'Governance', 'Final'],
        datasets: [{
          data: [4.0, 6.0, 10.0, 12.5, 14.0, 14.5, 15.4],
          backgroundColor: ['#94a3b8', '#10b981', '#10b981', '#10b981', '#10b981', '#10b981', '#2563eb'],
          borderRadius: 4
        }]
      },
      options: {
        responsive: true,
        plugins: { legend: { display: false } },
        scales: { y: { beginAtZero: false, ticks: { color: '#94a3b8', callback: function(value) { return value + '%'; } } }, x: { ticks: { color: '#94a3b8' } } }
      }
    });

    // 2. Revenue Waterfall
    new Chart(document.getElementById('revWaterfall'), {
      type: 'bar',
      data: {
        labels: ['Baseline ($780M)', 'New Deal wins (+$73M)', 'Volume Expansion (+$42M)', 'Attrition (-$68M)', 'Price adjustment (-$41M)', 'Forecast ($862M)'],
        datasets: [{
          data: [780, 853, 895, 827, 786, 862], // Placeholder simplified stack
          backgroundColor: ['#94a3b8', '#10b981', '#10b981', '#ef4444', '#ef4444', '#2563eb'],
          borderRadius: 4
        }]
      },
      options: {
        responsive: true,
        plugins: { legend: { display: false } },
        scales: { y: { beginAtZero: true, ticks: { color: '#94a3b8', callback: function(value) { return '$' + value + 'M'; } } }, x: { ticks: { color: '#94a3b8', display: false } } } // display: false, label can be long
      }
    });

    // 3. Margin Expansion Bridge Chart
    new Chart(document.getElementById('margBridge'), {
      type: 'bar',
      data: {
        labels: ['Starting (28.4%)', 'Utilization (+1.3%)', 'Subcon mix (+0.7%)', 'Inflation (-0.9%)', 'Automation (+0.5%)', 'Final (32.6%)'],
        datasets: [{
          data: [28.4, 29.7, 30.4, 29.5, 30.0, 32.6], // Simplified linear bridge
          backgroundColor: ['#94a3b8', '#10b981', '#10b981', '#ef4444', '#10b981', '#06b6d4'],
          borderRadius: 4
        }]
      },
      options: {
        responsive: true,
        plugins: { legend: { display: false } },
        scales: { y: { beginAtZero: false, ticks: { color: '#94a3b8', callback: function(value) { return value + '%'; } } }, x: { ticks: { color: '#94a3b8', display: false } } }
      }
    });

    // 4. Budget vs Actual Variance
    new Chart(document.getElementById('budgetActualBar'), {
      type: 'bar',
      data: {
        labels: ['Total Revenue', 'Cost of Services (COGS)', 'Project Profit', 'Margin %'],
        datasets: [
          { label: 'Budget', data: [811, 622, 189, 23.3], backgroundColor: '#94a3b8', borderRadius: 2 },
          { label: 'Actual', data: [862, 598, 264, 30.6], backgroundColor: '#2563eb', borderRadius: 2 }
        ]
      },
      options: {
        responsive: true,
        plugins: { legend: { labels: { color: '#94a3b8' } } },
        scales: { y: { beginAtZero: true, ticks: { color: '#94a3b8' } }, x: { ticks: { color: '#94a3b8' } } }
      }
    });

    // -----------------------------------------------------------
    // Interactive Scenario Engine Logic
    // -----------------------------------------------------------
    const baselines = { revenue: 119.0, salary: 65.2, subcon: 15.6, profit: 18.3, margin: 15.4, cash: 16.8 };

    // Get input and output elements
    const sRev = document.getElementById('sl_rev'); const sSub = document.getElementById('sl_sub'); const sUtil = document.getElementById('sl_util'); const sAtt = document.getElementById('sl_att');
    const vRev = document.getElementById('val_rev'); const vSub = document.getElementById('val_sub'); const vUtil = document.getElementById('val_util'); const vAtt = document.getElementById('val_att');
    const oRev = document.getElementById('proj_rev'); const oProf = document.getElementById('proj_prof'); const oMarg = document.getElementById('proj_marg'); const oCash = document.getElementById('proj_cash');

    function updateScenario() {
      // 1. Get input values
      const varRev = parseInt(sRev.value); const varSub = parseInt(sSub.value); const varUtil = parseInt(sUtil.value); const varAtt = parseInt(sAtt.value);

      // 2. Update UI values labels
      vRev.innerText = (varRev >= 0 ? '+' : '') + varRev + '%';
      vSub.innerText = (varSub >= 0 ? '+' : '') + varSub + '%';
      vUtil.innerText = (varUtil >= 0 ? '+' : '') + varUtil + '%';
      vAtt.innerText = (varAtt >= 0 ? '+' : '') + varAtt + ' pts';

      // 3. Simplified Financial Calculations
      
      // Calculate Revenue Impact
      const projRevenue = baselines.revenue * (1 + (varRev / 100));
      
      // Calculate Simplified Profit Deltas
      const profitDelta_Revenue = projRevenue - baselines.revenue;
      const costDelta_Subcon = baselines.subcon * (varSub / 100);
      const profitDelta_Utilization = baselines.salary * (varUtil / 100);
      
      const attritionPenaltyConstant = 0.6; // Penalty simplified per pt of increase
      const profitDelta_Attrition = -varAtt * attritionPenaltyConstant;
      
      // Calculate Projected Profit & Margin
      const projProfit = baselines.profit + (profitDelta_Revenue * 0.4) - costDelta_Subcon + profitDelta_Utilization + profitDelta_Attrition; // profit contribution constant 0.4
      const projMargin = (projProfit / projRevenue) * 100;
      
      // Calculate Cash Flow (simplified 80% contribution of profit delta)
      const cashDelta = (projProfit - baselines.profit) * 0.8;
      const projCash = baselines.cash + cashDelta;

      // 4. Update UI outputs
      oRev.innerText = '$' + projRevenue.toFixed(1) + 'M';
      oProf.innerText = '$' + projProfit.toFixed(1) + 'M';
      oMarg.innerText = projMargin.toFixed(1) + '%';
      oCash.innerText = '$' + projCash.toFixed(1) + 'M';

      // 5. Dynamic Color Handling for Projected Profit/Margin
      oProf.className = projProfit >= baselines.profit ? 'metric-value text-green' : 'metric-value text-red';
      oMarg.className = projMargin >= baselines.margin ? 'metric-value text-cyan' : 'metric-value text-red';
      oCash.className = projCash >= baselines.cash ? 'metric-value text-green' : 'metric-value text-red';
    }

    // Attach listeners to all sliders
    [sRev, sSub, sUtil, sAtt].forEach(slider => {
      slider.addEventListener('input', updateScenario);
    });
  </script>
</body>
</html>
