<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Financial Dashboard</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body {
      font-family: 'Manrope', sans-serif;
      background-color: #0f172a;
      color: #f8fafc;
      margin: 0;
      padding: 20px;
    }
    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
      gap: 20px;
    }
    .card, .kpi, .metric {
      background: #1e293b;
      padding: 15px;
      border-radius: 8px;
      border: 1px solid rgba(255, 255, 255, 0.1);
      position: relative;
    }
    .chart-container {
      position: relative;
      height: 250px;
      width: 100%;
    }
    .flow-step {
      opacity: 0.3;
      transition: opacity 0.3s;
    }
    .flow-step.active {
      opacity: 1;
    }
    #kpiTooltip {
      position: fixed;
      background: #090d16;
      border: 1px solid rgba(255, 255, 255, 0.2);
      padding: 8px 12px;
      border-radius: 6px;
      display: none;
      z-index: 1000;
      pointer-events: none;
      width: 250px;
    }
    #kpiTooltip.visible {
      display: block;
    }
    .project-pane {
      display: none;
    }
    .project-pane.active {
      display: block;
    }
    .project-tab-btn.active, .forecast-scenario-btn.active {
      background-color: #3b82f6;
      color: #fff;
    }
    .metric-positive {
      border-color: #83e6bb;
    }
    .metric-negative {
      border-color: #f08c94;
    }
    .node {
      cursor: pointer;
      padding: 8px;
      margin: 4px 0;
      background: #334155;
      border-radius: 4px;
    }
    .node.active {
      background: #475569;
      border-left: 4px solid #3b82f6;
    }
    button {
      padding: 8px 16px;
      margin: 4px;
      background: #334155;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
  </style>
</head>
<body>

  <div id="kpiTooltip"></div>

  <!-- KPI Controls & Indicators -->
  <div class="grid">
    <div class="kpi" id="scenarioRevenueCard">
      <small>Revenue Forecast</small><br>
      <strong id="scenarioRevenue" class="count-up" data-count="119" data-suffix="M">$0M</strong><br>
      <span id="scenarioRevenueDelta">Portfolio revenue forecast</span>
    </div>
    <div class="kpi" id="scenarioProfitCard">
      <small>Operating Profit</small><br>
      <strong id="scenarioProfit" class="count-up" data-count="18.3" data-suffix="M">$0M</strong><br>
      <span id="scenarioProfitDelta">Operating profit forecast</span>
    </div>
    <div class="kpi" id="scenarioMarginCard">
      <small>Operating Margin</small><br>
      <strong id="scenarioMargin">15.4%</strong><br>
      <span id="scenarioMarginDelta">Operating margin forecast</span>
    </div>
    <div class="kpi" id="scenarioCashCard">
      <small>Cash Flow</small><br>
      <strong id="scenarioCash" class="count-up" data-count="16.8" data-suffix="M">$0M</strong><br>
      <span id="scenarioCashDelta">Cash flow forecast</span>
    </div>
  </div>

  <br>

  <!-- Forecast Scenario Buttons -->
  <div>
    <button class="forecast-scenario-btn active" data-forecast-case="base">Base Case</button>
    <button class="forecast-scenario-btn" data-forecast-case="best">Best Case</button>
    <button class="forecast-scenario-btn" data-forecast-case="worst">Worst Case</button>
  </div>

  <div class="grid" style="margin-top: 10px;">
    <div class="card">
      <small>Forecast Revenue KPI</small>: <strong id="forecastRevenueKpi">$862.4M</strong>
    </div>
    <div class="card">
      <small>Forecast Cost KPI</small>: <strong id="forecastCostKpi">$598.6M</strong>
    </div>
    <div class="card">
      <small>Forecast Margin KPI</small>: <strong id="forecastMarginKpi">32.6%</strong>
    </div>
    <div class="card">
      <small>Forecast Cash KPI</small>: <strong id="forecastCashKpi">$134M</strong>
    </div>
  </div>

  <br>

  <!-- Interactive Controls Section -->
  <div class="grid">
    <div class="card">
      <h3>Portfolio Scenario Controls</h3>
      <label>Revenue Change: <span id="revVal">0%</span></label><br>
      <input type="range" id="revSlider" min="-20" max="20" value="0"><br><br>
      
      <label>Salary Change: <span id="salaryVal">0%</span></label><br>
      <input type="range" id="salarySlider" min="-10" max="10" value="0"><br><br>
      
      <label>Subcon Change: <span id="subconVal">0%</span></label><br>
      <input type="range" id="subconSlider" min="-10" max="10" value="0"><br><br>
      
      <label>Utilization Change: <span id="utilVal">0%</span></label><br>
      <input type="range" id="utilSlider" min="-10" max="10" value="0"><br><br>
      
      <label>Attrition Change: <span id="attritionVal">0%</span></label><br>
      <input type="range" id="attritionSlider" min="-10" max="10" value="0">
    </div>

    <div class="card">
      <h3>Project Controls</h3>
      <label>Revenue: <span id="projectRevVal">0%</span></label><br>
      <input type="range" id="projectRevSlider" min="-20" max="20" value="0"><br><br>
      
      <label>Margin: <span id="projectMarginVal">0 pts</span></label><br>
      <input type="range" id="projectMarginSlider" min="-10" max="10" value="0"><br><br>
      
      <label>SG&A: <span id="projectSgaVal">0 pts</span></label><br>
      <input type="range" id="projectSgaSlider" min="-10" max="10" value="0"><br><br>
      
      <label>Capex: <span id="projectCapexVal">0%</span></label><br>
      <input type="range" id="projectCapexSlider" min="-10" max="10" value="0"><br><br>
      
      <label>WACC: <span id="projectWaccVal">0 pts</span></label><br>
      <input type="range" id="projectWaccSlider" min="-5" max="5" value="0"><br><br>

      <div style="margin-top: 15px;">
        <p>Revenue: <strong id="projectScenarioRevenue">$100.0B</strong></p>
        <p>EBITDA: <strong id="projectScenarioEbitda">$18.0B</strong></p>
        <p>FCF: <strong id="projectScenarioFcf">$9.7B</strong></p>
        <p>Valuation: <strong id="projectScenarioValue">$285/share</strong></p>
        <div id="projectScenarioText"></div>
      </div>
    </div>

    <div class="card">
      <h3>Driver Insights</h3>
      <div class="node" data-driver="volume">Volume Decision</div>
      <div class="node" data-driver="price">Pricing Decision</div>
      <div class="node" data-driver="mix">Mix Decision</div>
      <div class="node" data-driver="salary">Salary Decision</div>
      <div class="node" data-driver="subcon">Subcon Decision</div>
      <div class="node" data-driver="utilization">Utilization Decision</div>
      <div class="node" data-driver="receivables">Receivables Decision</div>
      <div class="node" data-driver="billing">Billing Decision</div>
      <div class="node" data-driver="collections">Collections Decision</div>
      <br>
      <div id="driverInsight">Select a driver to view insights.</div>
    </div>
  </div>

  <br>

  <!-- Project Tabs Section -->
  <div>
    <button class="project-tab-btn active" data-project-pane="pane1">Project View 1</button>
    <button class="project-tab-btn" data-project-pane="pane2">Project View 2</button>
  </div>

  <div id="project-pane1" class="project-pane active">
    <div class="grid">
      <div class="card"><div class="chart-container"><canvas id="marginTransformChart"></canvas></div></div>
      <div class="card"><div class="chart-container"><canvas id="forecastChart"></canvas></div></div>
      <div class="card"><div class="chart-container"><canvas id="forecastMarginChart"></canvas></div></div>
      <div class="card"><div class="chart-container"><canvas id="varianceChart"></canvas></div></div>
    </div>
  </div>

  <div id="project-pane2" class="project-pane">
    <div class="grid">
      <div class="card"><div class="chart-container"><canvas id="costBridgeChart"></canvas></div></div>
      <div class="card"><div class="chart-container"><canvas id="scenarioChart"></canvas></div></div>
      <div class="card"><div class="chart-container"><canvas id="cashChart"></canvas></div></div>
      <div class="card"><div class="chart-container"><canvas id="projectScenarioRevenueChart"></canvas></div></div>
      <div class="card"><div class="chart-container"><canvas id="projectScenarioEbitdaChart"></canvas></div></div>
      <div class="card"><div class="chart-container"><canvas id="projectScenarioValueChart"></canvas></div></div>
    </div>
  </div>

  <br>

  <!-- Benchmarks & DCF Section -->
  <h2>Benchmark & DCF Models</h2>
  <div class="grid">
    <div class="card"><div class="chart-container"><canvas id="teslaRevenueChart"></canvas></div></div>
    <div class="card"><div class="chart-container"><canvas id="teslaProfitChart"></canvas></div></div>
    <div class="card"><div class="chart-container"><canvas id="teslaMixChart"></canvas></div></div>
    <div class="card"><div class="chart-container"><canvas id="dcfRevenueChart"></canvas></div></div>
    <div class="card"><div class="chart-container"><canvas id="dcfFcfChart"></canvas></div></div>
    <div class="card"><div class="chart-container"><canvas id="dcfFootballChart"></canvas></div></div>
    <div class="card"><div class="chart-container"><canvas id="dashRevenueTrendChart"></canvas></div></div>
    <div class="card"><div class="chart-container"><canvas id="dashBudgetChart"></canvas></div></div>
    <div class="card"><div class="chart-container"><canvas id="dashExpenseChart"></canvas></div></div>
  </div>

  <!-- JavaScript Implementation -->
  <script>
    const chartText = "#f8fafc";
    const gridColor = "rgba(255, 255, 255, 0.1)";

    function money(val) {
      return "$" + val.toFixed(1) + "M";
    }

    // Scroll Observer
    const flowObserver = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) entry.target.classList.add("active");
      });
    }, { threshold: 0.55 });
    document.querySelectorAll(".flow-step").forEach(step => flowObserver.observe(step));

    // KPI Tooltip Logic
    const kpiTooltip = document.getElementById("kpiTooltip");
    document.querySelectorAll(".kpi, .metric").forEach(card => {
      card.addEventListener("mouseenter", () => {
        if (!kpiTooltip) return;
        const title = card.querySelector("small")?.textContent || "Metric";
        const value = card.querySelector("strong")?.textContent || "";
        const detail = card.querySelector("span")?.textContent || "Financial metric used for executive decision support.";
        kpiTooltip.innerHTML = `<strong>${title} ${value}</strong><br>${detail}`;
        kpiTooltip.classList.add("visible");
      });
      card.addEventListener("mousemove", event => {
        if (!kpiTooltip) return;
        kpiTooltip.style.left = Math.min(event.clientX + 16, window.innerWidth - 270) + "px";
        kpiTooltip.style.top = Math.min(event.clientY + 16, window.innerHeight - 140) + "px";
      });
      card.addEventListener("mouseleave", () => kpiTooltip?.classList.remove("visible"));
    });

    // Count Up Animation
    function animateCount(element) {
      const target = Number(element.dataset.count);
      const startValue = Number(element.dataset.start || 0);
      const suffix = element.dataset.suffix || "";
      const start = performance.now();
      const duration = 900;
      function step(now) {
        const progress = Math.min((now - start) / duration, 1);
        const eased = 1 - Math.pow(1 - progress, 3);
        const value = startValue + (target - startValue) * eased;
        element.textContent = target % 1 === 0 ? Math.round(value).toString() + suffix : value.toFixed(1) + suffix;
        if (progress < 1) requestAnimationFrame(step);
      }
      requestAnimationFrame(step);
    }

    const countObserver = new IntersectionObserver(entries => {
      entries.forEach(entry => {
        if (entry.isIntersecting && !entry.target.dataset.counted) {
          entry.target.dataset.counted = "true";
          animateCount(entry.target);
        }
      });
    }, { threshold: 0.65 });

    document.querySelectorAll(".count-up").forEach(element => countObserver.observe(element));

    // Chart.js Helper & Custom Waterfall Plugin
    function makeChart(canvasId, config) {
      const canvas = document.getElementById(canvasId);
      if (!canvas || !window.Chart) return null;
      return new Chart(canvas, config);
    }

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

    if (window.Chart) Chart.register(bridgeLabelPlugin);

    const commonOptions = {
      responsive: true,
      maintainAspectRatio: false,
      plugins: { legend: { labels: { color: chartText, boxWidth: 12 } } },
      scales: {
        x: { ticks: { color: chartText }, grid: { color: gridColor } },
        y: { ticks: { color: chartText }, grid: { color: gridColor } }
      }
    };

    // Instantiate Charts
    makeChart("marginTransformChart", {
      type: "bar",
      data: {
        labels: ["Baseline", "Revenue Recovery", "Cost Optimization", "Resource Mix", "Utilization", "Governance", "Final"],
        datasets: [{
          label: "Operating Margin %",
          data: [4.0, 6.2, 9.1, 11.3, 13.2, 14.2, 15.4],
          backgroundColor: ["rgba(240,140,148,.72)", "rgba(93,180,255,.68)", "rgba(93,180,255,.68)", "rgba(93,180,255,.68)", "rgba(93,180,255,.68)", "rgba(93,180,255,.68)", "rgba(131,230,187,.76)"],
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
          backgroundColor: ["rgba(232,193,111,.82)", "rgba(131,230,187,.78)", "rgba(131,230,187,.78)", "rgba(131,230,187,.78)", "rgba(240,140,148,.78)", "rgba(240,140,148,.78)", "rgba(232,193,111,.82)"],
          borderColor: "rgba(255,255,255,.22)",
          borderWidth: 1
        }]
      },
      options: { ...commonOptions, plugins: { ...commonOptions.plugins, tooltip: { callbacks: { label: ctx => ctx.dataset.label + ": " + ctx.dataset.displayLabels[ctx.dataIndex] } } } }
    });

    makeChart("forecastMarginChart", {
      type: "bar",
      data: {
        labels: ["Starting", "Pricing", "Utilization", "Subcon", "Automation", "Inflation", "Final"],
        datasets: [{
          label: "Margin Expansion Bridge (%)",
          data: [[0, 28.4], [28.4, 30.2], [30.2, 31.5], [31.5, 32.2], [32.2, 33.1], [32.6, 33.1], [0, 32.6]],
          displayLabels: ["28.4%", "+1.8%", "+1.3%", "+0.7%", "+0.9%", "-0.5%", "32.6%"],
          backgroundColor: ["rgba(232,193,111,.82)", "rgba(131,230,187,.78)", "rgba(131,230,187,.78)", "rgba(131,230,187,.78)", "rgba(131,230,187,.78)", "rgba(240,140,148,.78)", "rgba(232,193,111,.82)"],
          borderColor: "rgba(255,255,255,.22)",
          borderWidth: 1
        }]
      },
      options: { ...commonOptions, plugins: { ...commonOptions.plugins, tooltip: { callbacks: { label: ctx => ctx.dataset.label + ": " + ctx.dataset.displayLabels[ctx.dataIndex] } } } }
    });

    const forecastCases = {
      best: { revenue: "$910M", cost: "$604M", margin: "35.4%", cash: "$158M" },
      base: { revenue: "$862.4M", cost: "$598.6M", margin: "32.6%", cash: "$134M" },
      worst: { revenue: "$805M", cost: "$612M", margin: "28.9%", cash: "$96M" }
    };

    function updateForecastCase(caseName) {
      const selected = forecastCases[caseName];
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

    const varianceChart = makeChart("varianceChart", {
      type: "bar",
      data: {
        labels: ["Starting OM", "Pricing", "Utilization", "Offshore Mix", "Automation", "Vendor Opt.", "Final OM"],
        datasets: [{
          label: "Operating Margin Recovery (%)",
          data: [[0, 4.0], [4.0, 6.8], [6.8, 10.2], [10.2, 12.3], [12.3, 13.8], [13.8, 15.4], [0, 15.4]],
          displayLabels: ["4.0%", "+2.8%", "+3.4%", "+2.1%", "+1.5%", "+1.6%", "15.4%"],
          backgroundColor: ["rgba(232,193,111,.82)", "rgba(131,230,187,.78)", "rgba(131,230,187,.78)", "rgba(131,230,187,.78)", "rgba(131,230,187,.78)", "rgba(131,230,187,.78)", "rgba(232,193,111,.82)"],
          borderColor: "rgba(255,255,255,.22)",
          borderWidth: 1
        }]
      },
      options: { ...commonOptions, plugins: { ...commonOptions.plugins, tooltip: { callbacks: { label: ctx => ctx.dataset.label + ": " + ctx.dataset.displayLabels[ctx.dataIndex] } } } }
    });

    const costBridgeChart = makeChart("costBridgeChart", {
      type: "bar",
      data: {
        labels: ["Baseline Cost", "Subcon", "Vendor", "Automation", "Delivery", "FX", "Final Cost"],
        datasets: [{
          label: "Cost Optimization Bridge ($M)",
          data: [[0, 760], [748, 760], [740, 748], [735, 740], [728, 735], [726, 728], [0, 726]],
          displayLabels: ["$760M", "-$12M", "-$8M", "-$5M", "-$7M", "-$2M", "$726M"],
          backgroundColor: ["rgba(232,193,111,.82)", "rgba(93,180,255,.78)", "rgba(93,180,255,.78)", "rgba(93,180,255,.78)", "rgba(93,180,255,.78)", "rgba(93,180,255,.78)", "rgba(232,193,111,.82)"],
          borderColor: "rgba(255,255,255,.22)",
          borderWidth: 1
        }]
      },
      options: { ...commonOptions, plugins: { ...commonOptions.plugins, tooltip: { callbacks: { label: ctx => ctx.dataset.label + ": " + ctx.dataset.displayLabels[ctx.dataIndex] } } } }
    });

    let scenarioChart = makeChart("scenarioChart", {
      type: "bar",
      data: {
        labels: ["Worst", "Base", "Best"],
        datasets: [{ label: "Operating Profit ($M)", data: [14.1, 18.3, 22.8], backgroundColor: ["rgba(240,140,148,.74)", "rgba(93,180,255,.72)", "rgba(131,230,187,.76)"] }]
      },
      options: commonOptions
    });

    makeChart("cashChart", {
      type: "bar",
      data: {
        labels: ["Current DSO", "Target DSO", "Cash Released"],
        datasets: [{ label: "Working Capital", data: [62, 52, 23.6], backgroundColor: ["rgba(240,140,148,.72)", "rgba(93,180,255,.72)", "rgba(131,230,187,.76)"] }]
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
        datasets: [{ label: "Beat / Miss", data: [0.4, 0.416, 3.22, 0.11], backgroundColor: ["rgba(131,230,187,.75)", "rgba(131,230,187,.75)", "rgba(131,230,187,.75)", "rgba(232,193,111,.72)"] }]
      },
      options: commonOptions
    });

    makeChart("teslaMixChart", {
      type: "doughnut",
      data: {
        labels: ["Automotive", "Energy", "Services & Other"],
        datasets: [{ data: [16.2, 2.4, 3.7], backgroundColor: ["rgba(93,180,255,.78)", "rgba(232,193,111,.76)", "rgba(131,230,187,.74)"], borderColor: "rgba(255,255,255,.15)" }]
      },
      options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { labels: { color: chartText, boxWidth: 12 } } } }
    });

    makeChart("dcfRevenueChart", {
      type: "line",
      data: {
        labels: ["FY26", "FY27", "FY28", "FY29", "FY30"],
        datasets: [{ label: "Revenue Forecast ($B)", data: [101, 112, 126, 143, 162], borderColor: "#5db4ff", backgroundColor: "rgba(93,180,255,.12)", tension: .35, fill: true }]
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
      data: { labels: ["Worst", "Current", "Base", "Best"], datasets: [{ label: "Value / Share", data: [190, 250, 285, 365], backgroundColor: ["rgba(240,140,148,.72)", "rgba(232,193,111,.68)", "rgba(93,180,255,.72)", "rgba(131,230,187,.76)"] }] },
      options: { ...commonOptions, indexAxis: "y" }
    });

    makeChart("projectScenarioRevenueChart", {
      type: "line",
      data: { labels: ["Y1", "Y2", "Y3", "Y4", "Y5"], datasets: [
        { label: "Best", data: [100, 116, 134, 155, 180], borderColor: "#83e6bb", tension: .35 },
        { label: "Base", data: [100, 110, 121, 133, 146], borderColor: "#5db4ff", tension: .35 },
        { label: "Worst", data: [100, 96, 94, 96, 99], borderColor: "#f08c94", tension: .35 }
      ]},
      options: commonOptions
    });

    const projectScenarioEbitdaChart = makeChart("projectScenarioEbitdaChart", {
      type: "bar",
      data: { labels: ["Worst", "Base", "Best"], datasets: [{ label: "EBITDA ($B)", data: [11.2, 18.0, 29.4], backgroundColor: ["rgba(240,140,148,.72)", "rgba(93,180,255,.72)", "rgba(131,230,187,.76)"] }] },
      options: commonOptions
    });

    const projectScenarioValueChart = makeChart("projectScenarioValueChart", {
      type: "bar",
      data: { labels: ["Worst", "Base", "Best"], datasets: [{ label: "Enterprise Value ($B)", data: [580, 875, 1160], backgroundColor: ["rgba(240,140,148,.72)", "rgba(93,180,255,.72)", "rgba(131,230,187,.76)"] }] },
      options: commonOptions
    });

    makeChart("dashRevenueTrendChart", {
      type: "line",
      data: { labels: ["FY22", "FY23", "FY24", "FY25", "FY26"], datasets: [{ label: "Revenue ($M)", data: [386, 412, 441, 455, 490.9], borderColor: "#83e6bb", backgroundColor: "rgba(131,230,187,.1)", tension: .35, fill: true }, { label: "Margin %", data: [12.1, 13.2, 14.0, 14.7, 14.3], borderColor: "#e8c16f", tension: .35 }] },
      options: commonOptions
    });

    makeChart("dashBudgetChart", {
      type: "bar",
      data: { labels: ["Revenue", "Profit", "Margin %"], datasets: [
        { label: "Actual", data: [490.9, 70.1, 14.3], backgroundColor: "rgba(131,230,187,.72)" },
        { label: "Budget", data: [475.0, 73.5, 15.5], backgroundColor: "rgba(93,180,255,.62)" }
      ]},
      options: commonOptions
    });

    makeChart("dashExpenseChart", {
      type: "doughnut",
      data: { labels: ["COGS", "Salary", "Third Party", "Sales & Marketing", "G&A"], datasets: [{ data: [308, 54, 28, 18, 12], backgroundColor: ["rgba(93,180,255,.74)", "rgba(232,193,111,.72)", "rgba(240,140,148,.7)", "rgba(131,230,187,.72)", "rgba(128,230,255,.66)"], borderColor: "rgba(255,255,255,.15)" }] },
      options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { labels: { color: chartText, boxWidth: 12 } } } }
    });

    // Tab Switching Mechanism
    document.querySelectorAll(".project-tab-btn").forEach(button => {
      button.addEventListener("click", () => {
        document.querySelectorAll(".project-tab-btn").forEach(item => item.classList.remove("active"));
        document.querySelectorAll(".project-pane").forEach(item => item.classList.remove("active"));
        button.classList.add("active");
        document.getElementById("project-" + button.dataset.projectPane).classList.add("active");
        requestAnimationFrame(() => {
          if (window.Chart && Chart.instances) Object.values(Chart.instances).forEach(chart => chart.resize());
        });
      });
    });

    // Project Controls Interactive Logic
    const projectControls = {
      revenue: document.getElementById("projectRevSlider"),
      margin: document.getElementById("projectMarginSlider"),
      sga: document.getElementById("projectSgaSlider"),
      capex: document.getElementById("projectCapexSlider"),
      wacc: document.getElementById("projectWaccSlider")
    };

    function updateProjectScenario() {
      const rev = Number(projectControls.revenue.value);
      const margin = Number(projectControls.margin.value);
      const sga = Number(projectControls.sga.value);
      const capex = Number(projectControls.capex.value);
      const wacc = Number(projectControls.wacc.value);

      document.getElementById("projectRevVal").textContent = rev + "%";
      document.getElementById("projectMarginVal").textContent = margin + " pts";
      document.getElementById("projectSgaVal").textContent = sga + " pts";
      document.getElementById("projectCapexVal").textContent = capex + "%";
      document.getElementById("projectWaccVal").textContent = wacc + " pts";

      const revenue = 100 * (1 + rev / 100);
      const ebitdaMargin = 18 + margin - sga;
      const ebitda = revenue * ebitdaMargin / 100;
      const fcf = ebitda * 0.54 - (capex * 0.18);
      const value = 285 + rev * 7.2 + margin * 14 - sga * 10 - capex * 1.8 - wacc * 23;

      document.getElementById("projectScenarioRevenue").textContent = "$" + revenue.toFixed(1) + "B";
      document.getElementById("projectScenarioEbitda").textContent = "$" + ebitda.toFixed(1) + "B";
      document.getElementById("projectScenarioFcf").textContent = "$" + fcf.toFixed(1) + "B";
      document.getElementById("projectScenarioValue").textContent = "$" + value.toFixed(0) + "/share";
      document.getElementById("projectScenarioText").innerHTML = "<strong>Executive Summary</strong> Scenario output shows revenue at $" + revenue.toFixed(1) + "B, EBITDA at $" + ebitda.toFixed(1) + "B, FCF at $" + fcf.toFixed(1) + "B, and valuation at $" + value.toFixed(0) + "/share. Revenue growth and margin remain the most important planning levers.";

      if (projectScenarioEbitdaChart && projectScenarioValueChart) {
        projectScenarioEbitdaChart.data.datasets[0].data = [ebitda * 0.68, ebitda, ebitda * 1.35];
        projectScenarioValueChart.data.datasets[0].data = [value * 0.72, value, value * 1.28];
        projectScenarioEbitdaChart.update();
        projectScenarioValueChart.update();
      }
    }

    Object.values(projectControls).forEach(control => control.addEventListener("input", updateProjectScenario));
    updateProjectScenario();

    // General Scenario Interactive Logic
    const controls = {
      revenue: document.getElementById("revSlider"),
      salary: document.getElementById("salarySlider"),
      subcon: document.getElementById("subconSlider"),
      utilization: document.getElementById("utilSlider"),
      attrition: document.getElementById("attritionSlider")
    };

    function updateScenario() {
      const revenueChange = Number(controls.revenue.value);
      const salaryChange = Number(controls.salary.value);
      const subconChange = Number(controls.subcon.value);
      const utilizationChange = Number(controls.utilization.value);
      const attritionChange = Number(controls.attrition.value);

      document.getElementById("revVal").textContent = revenueChange + "%";
      document.getElementById("salaryVal").textContent = salaryChange + "%";
      document.getElementById("subconVal").textContent = subconChange + "%";
      document.getElementById("utilVal").textContent = utilizationChange + "%";
      document.getElementById("attritionVal").textContent = attritionChange + "%";

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

      document.getElementById("scenarioRevenue").textContent = money(revenue);
      document.getElementById("scenarioProfit").textContent = money(profit);
      document.getElementById("scenarioMargin").textContent = margin.toFixed(1) + "%";
      document.getElementById("scenarioCash").textContent = money(cash);
      document.getElementById("scenarioRevenueDelta").textContent = "Portfolio revenue forecast";
      document.getElementById("scenarioProfitDelta").textContent = "Operating profit forecast";
      document.getElementById("scenarioMarginDelta").textContent = "Operating margin forecast";
      document.getElementById("scenarioCashDelta").textContent = "Cash flow forecast";

      ["scenarioRevenueCard", "scenarioProfitCard", "scenarioMarginCard", "scenarioCashCard"].forEach(id => {
        const card = document.getElementById(id);
        if (!card) return;
        card.classList.remove("metric-positive", "metric-negative");
        void card.offsetWidth;
      });

      const metricPairs = [
        ["scenarioRevenueCard", revenue - baseRevenue],
        ["scenarioProfitCard", profit - baseProfit],
        ["scenarioMarginCard", margin - 15.4],
        ["scenarioCashCard", cash - baseCash]
      ];

      metricPairs.forEach(([id, delta]) => {
        const card = document.getElementById(id);
        if (!card || Math.abs(delta) < 0.05) return;
        card.classList.add(delta > 0 ? "metric-positive" : "metric-negative");
      });

      if (scenarioChart) {
        scenarioChart.data.datasets[0].data = [profit * 0.82, profit, profit * 1.18];
        scenarioChart.update();
      }
    }

    Object.values(controls).forEach(control => control.addEventListener("input", updateScenario));
    updateScenario();

    // Driver Insights Click Handlers
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
        const [title, body] = driverCopy[node.dataset.driver];
        document.getElementById("driverInsight").innerHTML = "<strong>" + title + "</strong><br>" + body;
      });
    });
  </script>
</body>
</html>
