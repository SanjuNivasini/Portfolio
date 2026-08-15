<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Executive Financial Dashboard & Scenario Modeling</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Manrope:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    :root {
      --bg-color: #0b1329;
      --card-bg: rgba(20, 32, 60, 0.6);
      --card-border: rgba(154, 189, 241, 0.15);
      --text-main: #bed2ee;
      --text-bright: #ffffff;
      --accent-blue: #5db4ff;
      --accent-green: #83e6bb;
      --accent-gold: #e8c16f;
      --accent-red: #f08c94;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: 'Manrope', sans-serif;
      background-color: var(--bg-color);
      color: var(--text-main);
      padding: 24px;
      line-height: 1.5;
    }

    .hero { text-align: center; margin-bottom: 32px; }
    .hero h1 { font-size: 2.2rem; color: var(--text-bright); display: inline-block; }
    .hero h1 .char { display: inline-block; animation: fadeIn 0.4s ease forwards; opacity: 0; }
    @keyframes fadeIn { to { opacity: 1; } }

    .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); gap: 20px; margin-bottom: 24px; }
    .card {
      background: var(--card-bg);
      border: 1px solid var(--card-border);
      border-radius: 12px;
      padding: 20px;
      backdrop-filter: blur(8px);
    }
    .card h3 { color: var(--text-bright); margin-bottom: 16px; font-weight: 700; font-size: 1.1rem; }

    .chart-container { position: relative; height: 260px; width: 100%; }

    .kpi-row { display: grid; grid-template-columns: repeat(auto-fit, minmax(140px, 1fr)); gap: 12px; margin-bottom: 16px; }
    .kpi {
      background: rgba(255, 255, 255, 0.03);
      border: 1px solid var(--card-border);
      border-radius: 8px;
      padding: 12px;
      text-align: center;
      transition: all 0.3s ease;
    }
    .kpi-title { font-size: 0.8rem; text-transform: uppercase; letter-spacing: 0.5px; opacity: 0.8; }
    .kpi-value { font-size: 1.4rem; font-weight: 800; color: var(--text-bright); margin-top: 4px; }
    .metric-positive { border-color: var(--accent-green); box-shadow: 0 0 10px rgba(131, 230, 187, 0.2); }
    .metric-negative { border-color: var(--accent-red); box-shadow: 0 0 10px rgba(240, 140, 148, 0.2); }

    .controls { display: flex; flex-direction: column; gap: 12px; margin-top: 16px; }
    .control-group { display: flex; flex-direction: column; gap: 4px; }
    .control-label { display: flex; justify-content: space-between; font-size: 0.85rem; font-weight: 600; }
    input[type=range] { width: 100%; accent-color: var(--accent-blue); }

    .btn-group { display: flex; gap: 8px; margin-bottom: 16px; }
    .btn {
      flex: 1;
      padding: 8px 12px;
      border: 1px solid var(--card-border);
      background: rgba(255, 255, 255, 0.05);
      color: var(--text-main);
      border-radius: 6px;
      cursor: pointer;
      font-weight: 600;
      transition: all 0.2s ease;
    }
    .btn.active, .btn:hover { background: var(--accent-blue); color: #000; border-color: var(--accent-blue); }

    .project-pane { display: none; }
    .project-pane.active { display: block; }

    .node-tree { display: flex; flex-wrap: wrap; gap: 10px; margin-bottom: 16px; }
    .node {
      padding: 8px 14px;
      border-radius: 6px;
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid var(--card-border);
      cursor: pointer;
      font-size: 0.85rem;
    }
    .node.active { background: var(--accent-gold); color: #000; font-weight: 700; }
    .insight-box { background: rgba(0, 0, 0, 0.2); padding: 12px; border-radius: 6px; border-left: 3px solid var(--accent-gold); font-size: 0.9rem; }
  </style>
</head>
<body>

  <div class="hero">
    <h1>Executive Financial Dashboard</h1>
  </div>

  <div class="grid">
    <!-- Forecast & Scenario Section -->
    <div class="card">
      <h3>Revenue & Margin Scenario Model</h3>
      <div class="btn-group">
        <button class="btn forecast-scenario-btn active" data-forecast-case="base">Base Case</button>
        <button class="btn forecast-scenario-btn" data-forecast-case="best">Best Case</button>
        <button class="btn forecast-scenario-btn" data-forecast-case="worst">Worst Case</button>
      </div>
      <div class="kpi-row">
        <div class="kpi"><div class="kpi-title">Revenue</div><div class="kpi-value" id="forecastRevenueKpi">$862.4M</div></div>
        <div class="kpi"><div class="kpi-title">Cost</div><div class="kpi-value" id="forecastCostKpi">$598.6M</div></div>
        <div class="kpi"><div class="kpi-title">Margin %</div><div class="kpi-value" id="forecastMarginKpi">32.6%</div></div>
        <div class="kpi"><div class="kpi-title">Free Cash</div><div class="kpi-value" id="forecastCashKpi">$134M</div></div>
      </div>
      <div class="chart-container">
        <canvas id="forecastChart"></canvas>
      </div>
    </div>

    <div class="card">
      <h3>Interactive Dynamic Sensitivity</h3>
      <div class="kpi-row">
        <div class="kpi"><div class="kpi-title">Revenue</div><div class="kpi-value" id="scenarioRevenue">$119.0M</div></div>
        <div class="kpi"><div class="kpi-title">Profit</div><div class="kpi-value" id="scenarioProfit">$18.3M</div></div>
        <div class="kpi"><div class="kpi-title">OM %</div><div class="kpi-value" id="scenarioMargin">15.4%</div></div>
        <div class="kpi"><div class="kpi-title">Cash</div><div class="kpi-value" id="scenarioCash">$16.8M</div></div>
      </div>
      <div class="controls">
        <div class="control-group">
          <div class="control-label"><span>Revenue Growth</span><span id="revVal">0%</span></div>
          <input type="range" id="revSlider" min="-15" max="15" value="0">
        </div>
        <div class="control-group">
          <div class="control-label"><span>Salary Inflation</span><span id="salaryVal">0%</span></div>
          <input type="range" id="salarySlider" min="-10" max="10" value="0">
        </div>
        <div class="control-group">
          <div class="control-label"><span>Subcon Rate Change</span><span id="subconVal">0%</span></div>
          <input type="range" id="subconSlider" min="-10" max="10" value="0">
        </div>
        <div class="control-group">
          <div class="control-label"><span>Billable Utilization</span><span id="utilVal">0%</span></div>
          <input type="range" id="utilSlider" min="-10" max="10" value="0">
        </div>
        <div class="control-group">
          <div class="control-label"><span>Attrition Delta</span><span id="attritionVal">0%</span></div>
          <input type="range" id="attritionSlider" min="-5" max="15" value="0">
        </div>
      </div>
      <div class="chart-container" style="margin-top:16px;">
        <canvas id="scenarioChart"></canvas>
      </div>
    </div>
  </div>

  <div class="grid">
    <!-- Margin & Cost Bridges -->
    <div class="card">
      <h3>Operating Margin Expansion Bridge</h3>
      <div class="chart-container"><canvas id="varianceChart"></canvas></div>
    </div>
    <div class="card">
      <h3>Cost Optimization Bridge ($M)</h3>
      <div class="chart-container"><canvas id="costBridgeChart"></canvas></div>
    </div>
    <div class="card">
      <h3>Margin Trajectory</h3>
      <div class="chart-container"><canvas id="marginTransformChart"></canvas></div>
    </div>
  </div>

  <div class="grid">
    <!-- Driver Decision Tree -->
    <div class="card">
      <h3>Root Cause & Operational Driver Tree</h3>
      <div class="node-tree">
        <div class="node active" data-driver="volume">Volume</div>
        <div class="node" data-driver="price">Pricing</div>
        <div class="node" data-driver="mix">Mix</div>
        <div class="node" data-driver="salary">Salary</div>
        <div class="node" data-driver="subcon">Subcontractor</div>
        <div class="node" data-driver="utilization">Utilization</div>
        <div class="node" data-driver="receivables">Receivables</div>
        <div class="node" data-driver="billing">Billing</div>
        <div class="node" data-driver="collections">Collections</div>
      </div>
      <div class="insight-box" id="driverInsight">
        <strong>Volume Decision</strong> Impact: -$8.7M. Root cause: delayed account ramp and softer demand. Recommendation: leadership should approve account-level recovery actions and weekly volume governance for priority accounts.
      </div>
    </div>
    <div class="card">
      <h3>Working Capital & Cash Release</h3>
      <div class="chart-container"><canvas id="cashChart"></canvas></div>
    </div>
  </div>

  <!-- Tabbed Project Valuation Section -->
  <div class="card" style="margin-bottom: 24px;">
    <h3>Long-Term Project Valuation & Planning</h3>
    <div class="btn-group">
      <button class="btn project-tab-btn active" data-project-pane="scenario">Valuation Inputs</button>
      <button class="btn project-tab-btn" data-project-pane="charts">Projections</button>
    </div>

    <div id="project-scenario" class="project-pane active">
      <div class="kpi-row">
        <div class="kpi"><div class="kpi-title">Revenue</div><div class="kpi-value" id="projectScenarioRevenue">$100.0B</div></div>
        <div class="kpi"><div class="kpi-title">EBITDA</div><div class="kpi-value" id="projectScenarioEbitda">$18.0B</div></div>
        <div class="kpi"><div class="kpi-title">Free Cash Flow</div><div class="kpi-value" id="projectScenarioFcf">$9.7B</div></div>
        <div class="kpi"><div class="kpi-title">Value / Share</div><div class="kpi-value" id="projectScenarioValue">$285/share</div></div>
      </div>
      <div class="controls">
        <div class="control-group">
          <div class="control-label"><span>Revenue Growth Delta</span><span id="projectRevVal">0%</span></div>
          <input type="range" id="projectRevSlider" min="-20" max="20" value="0">
        </div>
        <div class="control-group">
          <div class="control-label"><span>Operating Margin Delta</span><span id="projectMarginVal">0 pts</span></div>
          <input type="range" id="projectMarginSlider" min="-5" max="5" value="0">
        </div>
        <div class="control-group">
          <div class="control-label"><span>SG&A % of Sales Delta</span><span id="projectSgaVal">0 pts</span></div>
          <input type="range" id="projectSgaSlider" min="-3" max="5" value="0">
        </div>
        <div class="control-group">
          <div class="control-label"><span>CapEx Rate Delta</span><span id="projectCapexVal">0 %</span></div>
          <input type="range" id="projectCapexSlider" min="-5" max="10" value="0">
        </div>
        <div class="control-group">
          <div class="control-label"><span>WACC Premium Delta</span><span id="projectWaccVal">0 pts</span></div>
          <input type="range" id="projectWaccSlider" min="-2" max="4" value="0">
        </div>
      </div>
      <div class="insight-box" id="projectScenarioText" style="margin-top:16px;">
        <strong>Executive Summary</strong> Scenario output shows revenue at $100.0B, EBITDA at $18.0B, FCF at $9.7B, and valuation at $285/share.
      </div>
    </div>

    <div id="project-charts" class="project-pane">
      <div class="grid">
        <div class="chart-container"><canvas id="projectScenarioRevenueChart"></canvas></div>
        <div class="chart-container"><canvas id="projectScenarioEbitdaChart"></canvas></div>
        <div class="chart-container"><canvas id="projectScenarioValueChart"></canvas></div>
      </div>
    </div>
  </div>

  <div class="grid">
    <!-- Corporate Earnings & Valuation Multiples -->
    <div class="card">
      <h3>Earnings Performance vs Consensus</h3>
      <div class="chart-container"><canvas id="teslaRevenueChart"></canvas></div>
    </div>
    <div class="card">
      <h3>Profitability & Outperformance Variance</h3>
      <div class="chart-container"><canvas id="teslaProfitChart"></canvas></div>
    </div>
    <div class="card">
      <h3>Segment Revenue Mix</h3>
      <div class="chart-container"><canvas id="teslaMixChart"></canvas></div>
    </div>
  </div>

  <div class="grid">
    <!-- DCF Model & Football Field -->
    <div class="card">
      <h3>5-Year DCF Revenue Forecast</h3>
      <div class="chart-container"><canvas id="dcfRevenueChart"></canvas></div>
    </div>
    <div class="card">
      <h3>EBITDA to FCF Conversion</h3>
      <div class="chart-container"><canvas id="dcfFcfChart"></canvas></div>
    </div>
    <div class="card">
      <h3>DCF Valuation Range (Football Field)</h3>
      <div class="chart-container"><canvas id="dcfFootballChart"></canvas></div>
    </div>
  </div>

  <div class="grid">
    <!-- Overall Dashboard Core Metrics -->
    <div class="card">
      <h3>Historical Revenue & Margin Trend</h3>
      <div class="chart-container"><canvas id="dashRevenueTrendChart"></canvas></div>
    </div>
    <div class="card">
      <h3>Actual vs Budget Performance</h3>
      <div class="chart-container"><canvas id="dashBudgetChart"></canvas></div>
    </div>
    <div class="card">
      <h3>Expense Allocation Breakdown</h3>
      <div class="chart-container"><canvas id="dashExpenseChart"></canvas></div>
    </div>
  </div>

  <script>
    // Global Helper Options & Plugins
    const chartText = "#bed2ee";
    const gridColor = "rgba(154, 189, 241, 0.13)";
    const money = value => "$" + value.toFixed(1) + "M";

    // Hero Text Split Animation
    const heroName = document.querySelector(".hero h1");
    if (heroName && !heroName.dataset.split) {
      const text = heroName.textContent;
      heroName.textContent = "";
      [...text].forEach((char, index) => {
        const span = document.createElement("span");
        span.className = "char";
        span.style.animationDelay = `${index * 28}ms`;
        span.textContent = char === " " ? "\u00a0" : char;
        heroName.appendChild(span);
      });
      heroName.dataset.split = "true";
    }

    // Chart Bridge Label Plugin Definition
    const bridgeLabelPlugin = {
      id: "bridgeLabelPlugin",
      afterDatasetsDraw(chart) {
        const canvasId = chart.canvas?.id;
        if (!["varianceChart", "costBridgeChart", "forecastChart", "forecastMarginChart"].includes(canvasId)) return;
        const { ctx } = chart;
        const meta = chart.getDatasetMeta(0);
        const dataset = chart.data.datasets[0];
        ctx.save();
        ctx.font = "800 13px Manrope, sans-serif";
        ctx.textAlign = "center";
        ctx.textBaseline = "middle";
        dataset.data.forEach((value, index) => {
          const bar = meta.data[index];
          if (!bar) return;
          const raw = Array.isArray(value) ? value[1] - value[0] : value;
          const label = dataset.displayLabels?.[index] || raw;
          const props = bar.tooltipPosition();
          ctx.fillStyle = raw < 0 ? "#f08c94" : index === 0 || index === dataset.data.length - 1 ? "#f4d895" : "#83e6bb";
          ctx.fillText(label, props.x, props.y - 16);
        });
        ctx.restore();
      }
    };

    if (window.Chart) {
      Chart.register(bridgeLabelPlugin);
    }

    const commonOptions = {
      responsive: true,
      maintainAspectRatio: false,
      plugins: { legend: { labels: { color: chartText, boxWidth: 12 } } },
      scales: {
        x: { ticks: { color: chartText }, grid: { color: gridColor } },
        y: { ticks: { color: chartText }, grid: { color: gridColor } }
      }
    };

    function makeChart(canvasId, config) {
      const canvas = document.getElementById(canvasId);
      if (!canvas || !window.Chart) return null;
      return new Chart(canvas, config);
    }

    // -------------------------------------------------------------
    // Chart Declarations
    // -------------------------------------------------------------

    makeChart("marginTransformChart", {
      type: "bar",
      data: {
        labels: ["Baseline", "Revenue Recovery", "Cost Optimization", "Resource Mix", "Utilization", "Governance", "Final"],
        datasets: [{
          label: "Operating Margin %",
          data: [4.0, 6.2, 9.1, 11.3, 13.2, 14.2, 15.4],
          backgroundColor: [
            "rgba(240,140,148,.72)", "rgba(93,180,255,.68)", "rgba(93,180,255,.68)",
            "rgba(93,180,255,.68)", "rgba(93,180,255,.68)", "rgba(93,180,255,.68)",
            "rgba(131,230,187,.76)"
          ],
          borderColor: "rgba(255,255,255,.22)",
          borderWidth: 1
        }]
      },
      options: commonOptions
    });

    makeChart("forecastChart", {
      type: "bar",
      data: {
        labels: ["Run Rate", "Renewals", "New Wins", "Price", "Volume", "Attrition", "Forecast"],
        datasets: [{
          label: "Revenue Build-Up Waterfall ($M)",
          data: [[0, 780], [780, 845], [845, 918], [918, 960], [892, 960], [862, 892], [0, 862]],
          displayLabels: ["$780M", "+$65M", "+$73M", "+$42M", "-$68M", "-$30M", "$862M"],
          backgroundColor: [
            "rgba(232,193,111,.82)", "rgba(131,230,187,.78)", "rgba(131,230,187,.78)",
            "rgba(131,230,187,.78)", "rgba(240,140,148,.78)", "rgba(240,140,148,.78)",
            "rgba(232,193,111,.82)"
          ],
          borderColor: "rgba(255,255,255,.22)",
          borderWidth: 1
        }]
      },
      options: {
        ...commonOptions,
        plugins: {
          ...commonOptions.plugins,
          tooltip: { callbacks: { label: ctx => ctx.dataset.label + ": " + ctx.dataset.displayLabels[ctx.dataIndex] } }
        }
      }
    });

    makeChart("varianceChart", {
      type: "bar",
      data: {
        labels: ["Starting OM", "Pricing", "Utilization", "Offshore Mix", "Automation", "Vendor Opt.", "Final OM"],
        datasets: [{
          label: "Operating Margin Recovery (%)",
          data: [[0, 4.0], [4.0, 6.8], [6.8, 10.2], [10.2, 12.3], [12.3, 13.8], [13.8, 15.4], [0, 15.4]],
          displayLabels: ["4.0%", "+2.8%", "+3.4%", "+2.1%", "+1.5%", "+1.6%", "15.4%"],
          backgroundColor: [
            "rgba(232,193,111,.82)", "rgba(131,230,187,.78)", "rgba(131,230,187,.78)",
            "rgba(131,230,187,.78)", "rgba(131,230,187,.78)", "rgba(131,230,187,.78)",
            "rgba(232,193,111,.82)"
          ],
          borderColor: "rgba(255,255,255,.22)",
          borderWidth: 1
        }]
      },
      options: {
        ...commonOptions,
        plugins: {
          ...commonOptions.plugins,
          tooltip: { callbacks: { label: ctx => ctx.dataset.label + ": " + ctx.dataset.displayLabels[ctx.dataIndex] } }
        }
      }
    });

    const costBridgeChart = makeChart("costBridgeChart", {
      type: "bar",
      data: {
        labels: ["Baseline Cost", "Subcon", "Vendor", "Automation", "Delivery", "FX", "Final Cost"],
        datasets: [{
          label: "Cost Optimization Bridge ($M)",
          data: [[0, 760], [748, 760], [740, 748], [735, 740], [728, 735], [726, 728], [0, 726]],
          displayLabels: ["$760M", "-$12M", "-$8M", "-$5M", "-$7M", "-$2M", "$726M"],
          backgroundColor: [
            "rgba(232,193,111,.82)", "rgba(93,180,255,.78)", "rgba(93,180,255,.78)",
            "rgba(93,180,255,.78)", "rgba(93,180,255,.78)", "rgba(93,180,255,.78)",
            "rgba(232,193,111,.82)"
          ],
          borderColor: "rgba(255,255,255,.22)",
          borderWidth: 1
        }]
      },
      options: {
        ...commonOptions,
        plugins: {
          ...commonOptions.plugins,
          tooltip: { callbacks: { label: ctx => ctx.dataset.label + ": " + ctx.dataset.displayLabels[ctx.dataIndex] } }
        }
      }
    });

    let scenarioChart = makeChart("scenarioChart", {
      type: "bar",
      data: {
        labels: ["Worst", "Base", "Best"],
        datasets: [{
          label: "Operating Profit ($M)",
          data: [14.1, 18.3, 22.8],
          backgroundColor: ["rgba(240,140,148,.74)", "rgba(93,180,255,.72)", "rgba(131,230,187,.76)"]
        }]
      },
      options: commonOptions
    });

    makeChart("cashChart", {
      type: "bar",
      data: {
        labels: ["Current DSO", "Target DSO", "Cash Released"],
        datasets: [{
          label: "Working Capital",
          data: [62, 52, 23.6],
          backgroundColor: ["rgba(240,140,148,.72)", "rgba(93,180,255,.72)", "rgba(131,230,187,.76)"]
        }]
      },
      options: commonOptions
    });

    makeChart("teslaRevenueChart", {
      type: "bar",
      data: {
        labels: ["Revenue", "Gross Profit", "Operating Income", "Free Cash Flow"],
        datasets: [
          { label: "Actual", data: [22.4, 4.7, 0.941, 1.44], backgroundColor: "rgba(131,230,187,.72)" },
          { label: "Consensus", data: [21.1, 4.3, 0.525, -1.78], backgroundColor: "rgba(93,180,255,.62)" }
        ]
      },
      options: commonOptions
    });

    makeChart("teslaProfitChart", {
      type: "bar",
      data: {
        labels: ["Gross Profit", "Operating Income", "FCF Beat", "EPS Beat"],
        datasets: [{
          label: "Beat / Miss",
          data: [0.4, 0.416, 3.22, 0.11],
          backgroundColor: ["rgba(131,230,187,.75)", "rgba(131,230,187,.75)", "rgba(131,230,187,.75)", "rgba(232,193,111,.72)"]
        }]
      },
      options: commonOptions
    });

    makeChart("teslaMixChart", {
      type: "doughnut",
      data: {
        labels: ["Automotive", "Energy", "Services & Other"],
        datasets: [{
          data: [16.2, 2.4, 3.7],
          backgroundColor: ["rgba(93,180,255,.78)", "rgba(232,193,111,.76)", "rgba(131,230,187,.74)"],
          borderColor: "rgba(255,255,255,.15)"
        }]
      },
      options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { labels: { color: chartText, boxWidth: 12 } } } }
    });

    makeChart("dcfRevenueChart", {
      type: "line",
      data: {
        labels: ["FY26", "FY27", "FY28", "FY29", "FY30"],
        datasets: [{
          label: "Revenue Forecast ($B)",
          data: [101, 112, 126, 143, 162],
          borderColor: "#5db4ff",
          backgroundColor: "rgba(93,180,255,.12)",
          tension: 0.35,
          fill: true
        }]
      },
      options: commonOptions
    });

    makeChart("dcfFcfChart", {
      type: "bar",
      data: {
        labels: ["FY26", "FY27", "FY28", "FY29", "FY30"],
        datasets: [
          { label: "EBITDA ($B)", data: [13.8, 16.4, 20.1, 24.6, 30.2], backgroundColor: "rgba(93,180,255,.68)" },
          { label: "FCF ($B)", data: [5.4, 7.2, 9.8, 12.6, 16.1], backgroundColor: "rgba(131,230,187,.72)" }
        ]
      },
      options: commonOptions
    });

    makeChart("dcfFootballChart", {
      type: "bar",
      data: {
        labels: ["Worst", "Current", "Base", "Best"],
        datasets: [{
          label: "Value / Share",
          data: [190, 250, 285, 365],
          backgroundColor: ["rgba(240,140,148,.72)", "rgba(232,193,111,.68)", "rgba(93,180,255,.72)", "rgba(131,230,187,.76)"]
        }]
      },
      options: { ...commonOptions, indexAxis: "y" }
    });

    makeChart("projectScenarioRevenueChart", {
      type: "line",
      data: {
        labels: ["Y1", "Y2", "Y3", "Y4", "Y5"],
        datasets: [
          { label: "Best", data: [100, 116, 134, 155, 180], borderColor: "#83e6bb", tension: 0.35 },
          { label: "Base", data: [100, 110, 121, 133, 146], borderColor: "#5db4ff", tension: 0.35 },
          { label: "Worst", data: [100, 96, 94, 96, 99], borderColor: "#f08c94", tension: 0.35 }
        ]
      },
      options: commonOptions
    });

    const projectScenarioEbitdaChart = makeChart("projectScenarioEbitdaChart", {
      type: "bar",
      data: {
        labels: ["Worst", "Base", "Best"],
        datasets: [{
          label: "EBITDA ($B)",
          data: [11.2, 18.0, 29.4],
          backgroundColor: ["rgba(240,140,148,.72)", "rgba(93,180,255,.72)", "rgba(131,230,187,.76)"]
        }]
      },
      options: commonOptions
    });

    const projectScenarioValueChart = makeChart("projectScenarioValueChart", {
      type: "bar",
      data: {
        labels: ["Worst", "Base", "Best"],
        datasets: [{
          label: "Enterprise Value ($B)",
          data: [580, 875, 1160],
          backgroundColor: ["rgba(240,140,148,.72)", "rgba(93,180,255,.72)", "rgba(131,230,187,.76)"]
        }]
      },
      options: commonOptions
    });

    makeChart("dashRevenueTrendChart", {
      type: "line",
      data: {
        labels: ["FY22", "FY23", "FY24", "FY25", "FY26"],
        datasets: [
          { label: "Revenue ($M)", data: [386, 412, 441, 455, 490.9], borderColor: "#83e6bb", backgroundColor: "rgba(131,230,187,.1)", tension: 0.35, fill: true },
          { label: "Margin %", data: [12.1, 13.2, 14.0, 14.7, 14.3], borderColor: "#e8c16f", tension: 0.35 }
        ]
      },
      options: commonOptions
    });

    makeChart("dashBudgetChart", {
      type: "bar",
      data: {
        labels: ["Revenue", "Profit", "Margin %"],
        datasets: [
          { label: "Actual", data: [490.9, 70.1, 14.3], backgroundColor: "rgba(131,230,187,.72)" },
          { label: "Budget", data: [475.0, 73.5, 15.5], backgroundColor: "rgba(93,180,255,.62)" }
        ]
      },
      options: commonOptions
    });

    makeChart("dashExpenseChart", {
      type: "doughnut",
      data: {
        labels: ["COGS", "Salary", "Third Party", "Sales & Marketing", "G&A"],
        datasets: [{
          data: [308, 54, 28, 18, 12],
          backgroundColor: [
            "rgba(93,180,255,.74)", "rgba(232,193,111,.72)", "rgba(240,140,148,.7)",
            "rgba(131,230,187,.72)", "rgba(128,230,255,.66)"
          ],
          borderColor: "rgba(255,255,255,.15)"
        }]
      },
      options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { labels: { color: chartText, boxWidth: 12 } } } }
    });

    // -------------------------------------------------------------
    // Interactive Logic & Event Observers
    // -------------------------------------------------------------

    const forecastCases = {
      best: { revenue: "$910M", cost: "$604M", margin: "35.4%", cash: "$158M" },
      base: { revenue: "$862.4M", cost: "$598.6M", margin: "32.6%", cash: "$134M" },
      worst: { revenue: "$805M", cost: "$612M", margin: "28.9%", cash: "$96M" }
    };

    function updateForecastCase(caseName) {
      const selected = forecastCases[caseName];
      if (!selected) return;
      document.getElementById("forecastRevenueKpi").textContent = selected.revenue;
      document.getElementById("forecastCostKpi").textContent = selected.cost;
      document.getElementById("forecastMarginKpi").textContent = selected.margin;
      document.getElementById("forecastCashKpi").textContent = selected.cash;
    }

    document.querySelectorAll(".forecast-scenario-btn").forEach(button => {
      button.addEventListener("click", () => {
        document.querySelectorAll(".forecast-scenario-btn").forEach(item => item.classList.remove("active"));
        button.classList.add("active");
        updateForecastCase(button.dataset.forecastCase);
      });
    });

    document.querySelectorAll(".project-tab-btn").forEach(button => {
      button.addEventListener("click", () => {
        document.querySelectorAll(".project-tab-btn").forEach(item => item.classList.remove("active"));
        document.querySelectorAll(".project-pane").forEach(item => item.classList.remove("active"));
        button.classList.add("active");
        const pane = document.getElementById("project-" + button.dataset.projectPane);
        if (pane) pane.classList.add("active");
        requestAnimationFrame(() => {
          if (window.Chart && Chart.instances) {
            Object.values(Chart.instances).forEach(chart => chart.resize());
          }
        });
      });
    });

    const projectControls = {
      revenue: document.getElementById("projectRevSlider"),
      margin: document.getElementById("projectMarginSlider"),
      sga: document.getElementById("projectSgaSlider"),
      capex: document.getElementById("projectCapexSlider"),
      wacc: document.getElementById("projectWaccSlider")
    };

    function updateProjectScenario() {
      const rev = Number(projectControls.revenue?.value || 0);
      const margin = Number(projectControls.margin?.value || 0);
      const sga = Number(projectControls.sga?.value || 0);
      const capex = Number(projectControls.capex?.value || 0);
      const wacc = Number(projectControls.wacc?.value || 0);

      if (document.getElementById("projectRevVal")) document.getElementById("projectRevVal").textContent = rev + "%";
      if (document.getElementById("projectMarginVal")) document.getElementById("projectMarginVal").textContent = margin + " pts";
      if (document.getElementById("projectSgaVal")) document.getElementById("projectSgaVal").textContent = sga + " pts";
      if (document.getElementById("projectCapexVal")) document.getElementById("projectCapexVal").textContent = capex + " %";
      if (document.getElementById("projectWaccVal")) document.getElementById("projectWaccVal").textContent = wacc + " pts";

      const revenue = 100 * (1 + rev / 100);
      const ebitdaMargin = 18 + margin - sga;
      const ebitda = revenue * ebitdaMargin / 100;
      const fcf = ebitda * 0.54 - (capex * 0.18);
      const value = 285 + rev * 7.2 + margin * 14 - sga * 10 - capex * 1.8 - wacc * 23;

      if (document.getElementById("projectScenarioRevenue")) document.getElementById("projectScenarioRevenue").textContent = "$" + revenue.toFixed(1) + "B";
      if (document.getElementById("projectScenarioEbitda")) document.getElementById("projectScenarioEbitda").textContent = "$" + ebitda.toFixed(1) + "B";
      if (document.getElementById("projectScenarioFcf")) document.getElementById("projectScenarioFcf").textContent = "$" + fcf.toFixed(1) + "B";
      if (document.getElementById("projectScenarioValue")) document.getElementById("projectScenarioValue").textContent = "$" + value.toFixed(0) + "/share";
      if (document.getElementById("projectScenarioText")) {
        document.getElementById("projectScenarioText").innerHTML = `<strong>Executive Summary</strong> Scenario output shows revenue at $${revenue.toFixed(1)}B, EBITDA at $${ebitda.toFixed(1)}B, FCF at $${fcf.toFixed(1)}B, and valuation at $${value.toFixed(0)}/share. Revenue growth and margin remain the most important planning levers.`;
      }

      if (projectScenarioEbitdaChart && projectScenarioValueChart) {
        projectScenarioEbitdaChart.data.datasets[0].data = [ebitda * 0.68, ebitda, ebitda * 1.35];
        projectScenarioValueChart.data.datasets[0].data = [value * 0.72, value, value * 1.28];
        projectScenarioEbitdaChart.update();
        projectScenarioValueChart.update();
      }
    }

    Object.values(projectControls).forEach(control => {
      if (control) control.addEventListener("input", updateProjectScenario);
    });
    updateProjectScenario();

    const controls = {
      revenue: document.getElementById("revSlider"),
      salary: document.getElementById("salarySlider"),
      subcon: document.getElementById("subconSlider"),
      utilization: document.getElementById("utilSlider"),
      attrition: document.getElementById("attritionSlider")
    };

    function updateScenario() {
      const revenueChange = Number(controls.revenue?.value || 0);
      const salaryChange = Number(controls.salary?.value || 0);
      const subconChange = Number(controls.subcon?.value || 0);
      const utilizationChange = Number(controls.utilization?.value || 0);
      const attritionChange = Number(controls.attrition?.value || 0);

      if (document.getElementById("revVal")) document.getElementById("revVal").textContent = revenueChange + "%";
      if (document.getElementById("salaryVal")) document.getElementById("salaryVal").textContent = salaryChange + "%";
      if (document.getElementById("subconVal")) document.getElementById("subconVal").textContent = subconChange + "%";
      if (document.getElementById("utilVal")) document.getElementById("utilVal").textContent = utilizationChange + "%";
      if (document.getElementById("attritionVal")) document.getElementById("attritionVal").textContent = attritionChange + "%";

      const baseRevenue = 119.0;
      const baseProfit = 18.3;
      const baseCash = 16.8;

      const revenue = baseRevenue * (1 + revenueChange / 100) * (1 + utilizationChange * 0.0025) * (1 - Math.max(attritionChange, 0) * 0.0025);
      const salaryImpact = salaryChange * 0.28;
      const subconImpact = subconChange * 0.18;
      const utilizationImpact = utilizationChange * 0.16;
      const attritionImpact = Math.max(attritionChange, 0) * 0.22;
      const profit = baseProfit + (revenue - baseRevenue) * 0.22 - salaryImpact - subconImpact + utilizationImpact - attritionImpact;
      const margin = profit / revenue * 100;
      const cash = baseCash + (profit - baseProfit) * 0.72 + utilizationChange * 0.08 - Math.max(attritionChange, 0) * 0.12;

      if (document.getElementById("scenarioRevenue")) document.getElementById("scenarioRevenue").textContent = money(revenue);
      if (document.getElementById("scenarioProfit")) document.getElementById("scenarioProfit").textContent = money(profit);
      if (document.getElementById("scenarioMargin")) document.getElementById("scenarioMargin").textContent = margin.toFixed(1) + "%";
      if (document.getElementById("scenarioCash")) document.getElementById("scenarioCash").textContent = money(cash);

      ["scenarioRevenue", "scenarioProfit", "scenarioMargin", "scenarioCash"].forEach(id => {
        const card = document.getElementById(id)?.closest(".kpi");
        if (!card) return;
        card.classList.remove("metric-positive", "metric-negative");
        void card.offsetWidth;
      });

      const metricPairs = [
        ["scenarioRevenue", revenue - baseRevenue],
        ["scenarioProfit", profit - baseProfit],
        ["scenarioMargin", margin - 15.4],
        ["scenarioCash", cash - baseCash]
      ];

      metricPairs.forEach(([id, delta]) => {
        const card = document.getElementById(id)?.closest(".kpi");
        if (!card || Math.abs(delta) < 0.05) return;
        card.classList.add(delta > 0 ? "metric-positive" : "metric-negative");
      });

      if (scenarioChart) {
        scenarioChart.data.datasets[0].data = [profit * 0.82, profit, profit * 1.18];
        scenarioChart.update();
      }
    }

    Object.values(controls).forEach(control => {
      if (control) control.addEventListener("input", updateScenario);
    });
    updateScenario();

    const driverCopy = {
      volume: ["Volume Decision", "Impact: -$8.7M. Root cause: delayed account ramp and softer demand. Recommendation: leadership should approve account-level recovery actions and weekly volume governance for priority accounts."],
      price: ["Pricing Decision", "Impact: +$2.8M. Root cause: selective rate realization. Recommendation: preserve price discipline and restrict discounting unless margin-accretive."],
      mix: ["Mix Decision", "Impact: -$4.2M. Root cause: lower-margin services grew faster. Recommendation: redirect pipeline focus toward high-margin segments and review account-level margin before accepting growth."],
      salary: ["Salary Decision", "Impact: -$1.8M. Root cause: inflation and retention pressure. Recommendation: maintain targeted hiring discipline while protecting critical delivery capacity."],
      subcon: ["Subcon Decision", "Impact: -$2.4M. Root cause: vendor rate escalation and peak external capacity. Recommendation: renegotiate vendor rates and convert repeatable work to internal teams."],
      utilization: ["Utilization Decision", "Impact: -$1.9M. Root cause: capacity mismatch and idle billable roles. Recommendation: implement weekly capacity forecasting and redeployment governance."],
      receivables: ["Receivables Decision", "Impact: $14.8M AR over 90 days. Root cause: delayed customer payments and disputed billing. Recommendation: assign account-level collection owners and clear billing blockers."],
      billing: ["Billing Decision", "Impact: timing leakage and delayed revenue recognition. Root cause: milestone slippage. Recommendation: align delivery milestones to finance forecast and billing cadence."],
      collections: ["Collections Decision", "Impact: $23.6M cash release from 10-day DSO improvement. Root cause: inconsistent collection follow-up. Recommendation: collection sprint by account tier."]
    };

    document.querySelectorAll(".node").forEach(node => {
      node.addEventListener("click", () => {
        document.querySelectorAll(".node").forEach(item => item.classList.remove("active"));
        node.classList.add("active");
        const [title, body] = driverCopy[node.dataset.driver] || ["Decision Insight", "Select a node to review underlying impact and actionable takeaways."];
        const insightEl = document.getElementById("driverInsight");
        if (insightEl) insightEl.innerHTML = `<strong>${title}</strong> ${body}`;
      });
    });
  </script>
</body>
</html>
