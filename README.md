<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Sanju Nivasini | FP&A Decision Intelligence Platform</title>
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link href="https://fonts.googleapis.com/css2?family=Manrope:wght@400;500;600;700;800&display=swap" rel="stylesheet" />
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.3/dist/chart.umd.min.js"></script>
    <style>
        :root {
            --bg: #060b14;
            --ink: #eff6ff;
            --muted: #94a3b8;
            --line: rgba(145, 177, 226, 0.22);
            --panel: rgba(9, 19, 37, 0.78);
            --blue: #5db4ff;
            --cyan: #06e6ff;
            --gold: #e5c16f;
            --green: #83e6bb;
            --red: #f06c94;
            --amber: #f3b46c;
            --shadow: 0 24px 70px rgba(0, 0, 0, 0.38);
        }

        * { box-sizing: border-box; }
        html { scroll-behavior: smooth; }
        body {
            margin: 0;
            font-family: Manrope, sans-serif;
            color: var(--ink);
            background:
                radial-gradient(circle at 18% -10%, rgba(93, 180, 255, 0.22), transparent 34%),
                radial-gradient(circle at 84% 09%, rgba(232, 193, 111, 0.14), transparent 36%),
                linear-gradient(150deg, #050914 0%, #0a1629 48%, #0d223c 100%);
        }

        body::after {
            content: "";
            position: fixed;
            inset: 0;
            z-index: -1;
            pointer-events: none;
            background: radial-gradient(circle at var(--mx, 50%) var(--my, 20%), rgba(93, 180, 255, 0.12), transparent 34%);
            transition: background 0.18s ease;
        }

        body::before {
            content: "";
            position: fixed;
            inset: 0;
            pointer-events: none;
            background-image:
                linear-gradient(to right, rgba(144, 176, 226, 0.07) 1px, transparent 1px),
                linear-gradient(to bottom, rgba(144, 176, 226, 0.07) 1px, transparent 1px);
            background-size: 44px 44px;
            mask-image: radial-gradient(circle at center, black 52%, transparent 92%);
            z-index: -1;
        }

        h1, h2, h3, p { margin: 0; }
        a { color: inherit; }

        .scroll-progress {
            position: fixed;
            top: 0;
            left: 0;
            z-index: 100;
            height: 3px;
            width: 0%;
            background: linear-gradient(90deg, var(--gold), var(--cyan), var(--blue));
            box-shadow: 0 0 18px rgba(93, 180, 255, 0.45);
            transition: width 0.08s linear;
        }

        .topbar {
            position: sticky;
            top: 0;
            z-index: 50;
            border-bottom: 1px solid var(--line);
            background: rgba(5, 10, 19, 0.9);
            backdrop-filter: blur(18px);
        }

        .nav {
            width: min(1320px, 94vw);
            margin: 0 auto;
            min-height: 64px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 18px;
        }

        .brand { display: grid; gap: 2px; }
        .brand strong { font-size: 16px; }
        .brand span { color: var(--muted); font-size: 10px; text-transform: uppercase; letter-spacing: 0.16em; }

        .links {
            display: flex;
            flex-wrap: wrap;
            justify-content: flex-end;
            gap: 6px;
        }

        .links a {
            border: 1px solid rgba(154, 189, 241, 0.24);
            border-radius: 8px;
            background: rgba(255, 255, 255, 0.035);
            color: #cfe0fb;
            padding: 7px 10px;
            font-size: 10px;
            text-decoration: none;
            text-transform: uppercase;
            letter-spacing: 0.07em;
        }

        .links a:hover {
            border-color: rgba(93, 180, 255, 0.72);
            background: rgba(93, 180, 255, 0.16);
            color: #fff;
        }

        .links a.active {
            border-color: rgba(232, 193, 111, 0.64);
            background: rgba(232, 193, 111, 0.12);
            color: #fff;
        }

        .shell { width: min(1320px, 94vw); margin: 0 auto; }
        section { scroll-margin-top: 84px; margin: 22px 0; }

        .panel {
            border: 1px solid var(--line);
            border-radius: 18px;
            background: var(--panel);
            box-shadow: var(--shadow);
            backdrop-filter: blur(12px);
            padding: 18px;
        }

        .hero {
            min-height: calc(100vh - 94px);
            display: grid;
            align-items: center;
            grid-template-columns: 1.12fr 0.88fr;
            gap: 24px;
            padding: 24px 0 20px;
        }

        .eyebrow {
            color: var(--gold);
            text-transform: uppercase;
            letter-spacing: 0.22em;
            font-size: 11px;
            font-weight: 800;
            margin-bottom: 12px;
        }

        .hero h1 { font-size: clamp(46px, 7vw, 84px); line-height: 0.96; }
        .hero h1 .char {
            display: inline-block;
            opacity: 0;
            transform: translateY(18px);
            animation: charReveal 0.58s cubic-bezier(.2, .7, .2, 1) forwards;
        }

        @keyframes charReveal {
            to { opacity: 1; transform: translateY(0); }
        }

        .hero h2 { margin-top: 14px; font-size: clamp(20px, 2.8vw, 32px); color: #b9d9ff; }
        .tagline { margin-top: 18px; color: #d6e5fb; font-size: 17px; line-height: 1.65; max-width: 720px; }

        .hero-grid, .grid-2, .grid-3, .grid-4, .grid-5, .flow, .tree {
            display: grid;
            gap: 14px;
        }

        .hero-grid { grid-template-columns: repeat(2, minmax(0, 1fr)); margin-top: 24px; gap: 14px; }
        .grid-2 { grid-template-columns: repeat(2, minmax(0, 1fr)); }
        .grid-3 { grid-template-columns: repeat(3, minmax(0, 1fr)); }
        .grid-4 { grid-template-columns: repeat(4, minmax(0, 1fr)); }
        .grid-5 { grid-template-columns: repeat(5, minmax(0, 1fr)); }
        .flow { grid-template-columns: repeat(6, minmax(0, 1fr)); }
        .tree { grid-template-columns: repeat(3, minmax(0, 1fr)); }

        .metric, .kpi, .slide, .case-card, .tool-card, .tree-card {
            border: 1px solid rgba(154, 189, 241, 0.2);
            border-radius: 14px;
            background: rgba(255, 255, 255, 0.035);
            padding: 14px;
        }

        .hero .metric {
            background:
                linear-gradient(145deg, rgba(93, 180, 255, 0.11), rgba(232, 193, 111, 0.055)),
                rgba(255, 255, 255, 0.04);
            border-color: rgba(154, 189, 241, 0.26);
            min-height: 178px;
            overflow: hidden;
            position: relative;
        }

        .hero .metric::after {
            content: "";
            position: absolute;
            inset: auto -30% -55% 18%;
            height: 90px;
            background: radial-gradient(circle, rgba(93, 180, 255, 0.18), transparent 68%);
            pointer-events: none;
        }

        .metric, .kpi, .slide, .case-card, .tool-card, .tree-card, .credential-card, .flow-step {
            transition: transform 0.22s ease, border-color 0.22s ease, box-shadow 0.22s ease;
        }

        .metric:hover, .kpi:hover, .slide:hover, .case-card:hover, .tool-card:hover, .tree-card:hover, .credential-card:hover, .flow-step:hover {
            border-color: rgba(93, 180, 255, 0.52);
            box-shadow: 0 16px 42px rgba(0, 0, 0, 0.28);
            transform: translateY(-3px);
        }

        .metric:hover strong, .kpi:hover strong {
            animation: numberPulse 0.72s ease;
        }

        @keyframes numberPulse {
            0%, 100% { transform: scale(1); }
            45% { transform: scale(1.035); }
        }

        .metric-icon, .card-icon {
            align-items: center;
            border: 1px solid rgba(93, 180, 255, 0.32);
            border-radius: 10px;
            color: var(--cyan);
            display: inline-flex;
            height: 32px;
            justify-content: center;
            margin-bottom: 10px;
            width: 32px;
        }

        .metric-icon i, .card-icon i, .credential-icon i, .cfi-wordmark i {
            display: inline-flex;
            font-style: normal;
            font-size: 9px;
            font-weight: 800;
            justify-content: center;
            line-height: 1.86em;
            init-weight: 1;
        }

        [data-lucide]::before { content: "FIN"; }
        [data-lucide="briefcase-business"]::before { content: "EXP"; }
        [data-lucide="graduation-cap"]::before { content: "MBA"; }
        [data-lucide="badge-check"]::before { content: "FP"; }
        [data-lucide="dollar-sign"]::before { content: "$"; }
        [data-lucide="trending-up"]::before { content: "UP"; }
        [data-lucide="layout-dashboard"]::before { content: "12"; }
        [data-lucide="medal"]::before { content: "*"; }
        [data-lucide="trophy"]::before { content: "MED"; }
        [data-lucide="building-2"]::before { content: "WIN"; }
        [data-lucide="shield-check"]::before { content: "OK"; }
        [data-lucide="spreadsheet"]::before { content: "XLS"; }
        [data-lucide="bar-chart-big"]::before { content: "BI"; }
        [data-lucide="database"]::before { content: "SQL"; }
        [data-lucide="workflow"]::before { content: "FX"; }
        [data-lucide="presentation"]::before { content: "PPT"; }
        [data-lucide="sparkles"]::before { content: "AI"; }

        .metric small, .kpi small {
            display: block;
            color: var(--muted);
            font-size: 10px;
            text-transform: uppercase;
            letter-spacing: 0.1em;
            margin-bottom: 5px;
        }

        .metric strong, .kpi strong { display: block; font-size: 25px; color: #fff; }
        .hero .metric strong { font-size: 33px; letter-spacing: 0; }
        .metric span, .kpi span { display: block; margin-top: 6px; color: #a9c1e4; font-size: 11px; line-height: 1.5; }

        .flagship-kpis .kpi {
            align-items: center;
            display: flex;
            flex-direction: column;
            justify-content: center;
            min-height: 132px;
            text-align: center;
        }

        .flagship-kpis .kpi small {
            margin-bottom: 11px;
        }

        .flagship-kpis .kpi strong {
            font-size: 31px;
            line-height: 1;
        }

        .fin-icon {
            align-items: center;
            border: 1px solid rgba(232, 193, 111, 0.38);
            border-radius: 8px;
            background: rgba(232, 193, 111, 0.1);
            color: #f4d095;
            display: inline-flex;
            font-size: 10px;
            font-weight: 800;
            height: 24px;
            justify-content: center;
            letter-spacing: 0.04em;
            margin-right: 9px;
            min-width: 28px;
            padding: 0 6px;
            vertical-align: middle;
        }

        .credential-strip {
            display: grid;
            gap: 14px;
            grid-template-columns: repeat(2, minmax(0, 1fr));
            margin-top: 14px;
        }

        .credential-card {
            align-items: center;
            border: 1px solid rgba(154, 189, 241, 0.2);
            border-radius: 14px;
            background: rgba(255, 255, 255, 0.035);
            display: grid;
            gap: 12px;
            grid-template-columns: auto 1fr;
            padding: 13px;
        }

        .credential-icon {
            align-items: center;
            border: 1px solid rgba(93, 180, 255, 0.42);
            border-radius: 12px;
            background: linear-gradient(145deg, rgba(93, 180, 255, 0.16), rgba(232, 193, 111, 0.11));
            color: #fff;
            display: inline-flex;
            font-size: 24px;
            font-weight: 800;
            height: 46px;
            justify-content: center;
            letter-spacing: 0.04em;
            min-width: 54px;
        }

        .credential-card small {
            color: var(--gold);
            display: block;
            font-size: 9px;
            font-weight: 800;
            letter-spacing: 0.12em;
            margin-bottom: 4px;
            text-transform: uppercase;
        }

        .credential-card strong { color: #fff; display: block; font-size: 14px; }
        .credential-card span { color: #a9c1e4; display: block; font-size: 10px; line-height: 1.5; margin-top: 4px; }

        .cfi-wordmark {
            align-items: center;
            border: 1px solid rgba(232, 193, 111, 0.36);
            border-radius: 14px;
            background: linear-gradient(145deg, rgba(232, 193, 111, 0.13), rgba(93, 180, 255, 0.07));
            color: #fff;
            display: inline-flex;
            font-size: 24px;
            font-weight: 800;
            height: 46px;
            justify-content: center;
            letter-spacing: 0.02em;
            margin-bottom: 14px;
            padding: 14px 18px;
        }

        .reveal {
            opacity: 0;
            transform: translateY(18px);
            transition: opacity 0.7s ease, transform 0.7s ease;
        }

        .reveal[data-motion="blur"] { filter: blur(10px); }
        .reveal[data-motion="center"] { transform: scale(0.65); transform-origin: center; }
        .reveal[data-motion="left"] { transform: translateX(-24px); }
        .reveal[data-motion="unfold"] { transform: rotateX(-8deg) translateY(22px); transform-origin: top; }

        .reveal.visible {
            opacity: 1;
            transform: translateY(0);
            filter: blur(0);
        }

        .reveal.visible[data-motion="center"] { transform: scale(1); }
        .reveal.visible[data-motion="left"] { transform: translateX(0); }
        .reveal.visible[data-motion="unfold"] { transform: rotateX(0deg) translateY(0); }

        .kpi.metric-positive strong { color: var(--green); animation: valueGlowGreen 0.75s ease; }
        .kpi.metric-negative strong { color: var(--red); animation: valueGlowRed 0.75s ease; }

        @keyframes valueGlowGreen {
            0% { text-shadow: 0 0 0 rgba(131, 230, 187, 0); }
            45% { text-shadow: 0 0 16px rgba(131, 230, 187, 0.42); }
            100% { text-shadow: 0 0 0 rgba(131, 230, 187, 0); }
        }

        @keyframes valueGlowRed {
            0% { text-shadow: 0 0 0 rgba(240, 140, 148, 0); }
            45% { text-shadow: 0 0 16px rgba(240, 140, 148, 0.38); }
            100% { text-shadow: 0 0 0 rgba(240, 140, 148, 0); }
        }

        .kpi-tooltip {
            position: fixed;
            z-index: 120;
            max-width: 310px;
            pointer-events: none;
            opacity: 0;
            transform: translateY(8px);
            transition: opacity 0.18s ease, transform 0.18s ease;
            border: 1px solid rgba(154, 189, 241, 0.28);
            border-radius: 14px;
            background: rgba(7, 16, 31, 0.92);
            box-shadow: 0 18px 50px rgba(0, 0, 0, 0.36);
            backdrop-filter: blur(14px);
            color: #d6e5fb;
            font-size: 11px;
            line-height: 1.55;
            padding: 12px;
        }

        .kpi-tooltip.visible {
            opacity: 1;
            transform: translateY(0);
        }

        .kpi-tooltip strong {
            color: #fff;
            display: block;
            font-size: 11px;
            letter-spacing: 0.08em;
            margin-bottom: 5px;
            text-transform: uppercase;
        }

        .hero .eyebrow, .hero h1, .hero h2, .hero .tagline, .credential-strip, .hero-grid {
            animation: heroRise 0.72s ease both;
        }

        .hero h1 { animation-delay: 0.08s; }
        .hero h2 { animation-delay: 0.18s; }
        .hero .tagline { animation-delay: 0.28s; }
        .credential-strip { animation-delay: 0.38s; }
        .hero-grid { animation-delay: 0.48s; }

        @keyframes heroRise {
            from { opacity: 0; transform: translateY(18px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .hero-card {
            background: linear-gradient(145deg, rgba(93, 180, 255, 0.14), rgba(232, 193, 111, 0.08)), var(--panel);
        }

        .section-head {
            display: flex;
            align-items: flex-end;
            justify-content: space-between;
            gap: 16px;
            margin-bottom: 14px;
        }

        .section-head h2 { font-size: clamp(25px, 3vw, 38px); }
        .section-head p { max-width: 660px; color: var(--muted); font-size: 12px; line-height: 1.65; }

        .label {
            display: inline-flex;
            align-items: center;
            border: 1px solid rgba(232, 193, 111, 0.4);
            border-radius: 999px;
            padding: 4px 10px;
            font-size: 10px;
            background: rgba(232, 193, 111, 0.08);
            color: var(--gold);
            text-transform: uppercase;
            letter-spacing: 0.09em;
            white-space: nowrap;
        }

        .decision-strip {
            border-left: 3px solid var(--blue);
            background: rgba(93, 180, 255, 0.075);
            border-radius: 10px;
            padding: 12px;
            font-size: 12px;
            line-height: 1.6;
            margin-bottom: 10px;
        }

        .decision-strip strong { color: #fff; }
        .decision-strip-string {
            color: #fff;
            text-transform: uppercase;
            letter-spacing: 0.06em;
            font-size: 10px;
            margin-bottom: 4px;
        }

        .chart-box {
            border: 1px solid rgba(154, 189, 241, 0.2);
            border-radius: 14px;
            background: rgba(255, 255, 255, 0.025);
            padding: 12px;
            height: 300px;
            min-width: 0;
        }

        canvas { width: 100% !important; max-height: 100%; }

        .table-wrap { overflow-x: auto; }
        table { width: 100%; border-collapse: collapse; font-size: 11px; color: #d3e2f9; }
        th, td { padding: 10px; border-bottom: 1px solid rgba(154, 189, 241, 0.12); text-align: left; vertical-align: top; }
        th { color: #feefff; background: rgba(255, 255, 255, 0.035); text-transform: uppercase; letter-spacing: 0.08em; font-size: 10px; }

        .num { text-align: right; font-variant-numeric: tabular-nums; white-space: nowrap; }
        .pos { color: var(--green); font-weight: 800; }
        .neg { color: var(--red); font-weight: 800; }
        .amber { color: var(--amber); font-weight: 800; }

        .flow-step {
            border: 1px solid rgba(93, 180, 255, 0.26);
            background: rgba(93, 180, 255, 0.085);
            border-radius: 12px;
            padding: 12px;
            min-height: 112px;
        }

        .flow-step.active {
            border-color: rgba(232, 193, 111, 0.5);
            background: rgba(232, 193, 111, 0.09);
            box-shadow: 0 12px 36px rgba(232, 193, 111, 0.08);
        }

        .flow-step small { color: var(--cyan); text-transform: uppercase; letter-spacing: 0.09em; font-size: 9px; }
        .flow-step strong { display: block; margin: 8px 0 5px; }
        .flow-step p { color: #bfd3ee; font-size: 10px; line-height: 1.55; }

        .slider-row { display: grid; gap: 8px; margin-bottom: 14px; }
        .slider-meta { display: flex; justify-content: space-between; color: #dfeaff; font-size: 12px; }
        input[type="range"] { width: 100%; accent-color: var(--blue); }

        .tree-card h3 { #fff; font-size: 16px; margin-bottom: 10px; }
        .node {
            border: 1px solid rgba(154, 189, 241, 0.2);
            border-radius: 10px;
            padding: 9px 10px;
            margin-bottom: 8px;
            background: rgba(255, 255, 255, 0.03);
            font-size: 11px;
            transition: 0.18s ease;
        }

        .node:hover, .node.active { border-color: rgba(93, 180, 255, 0.7); background: rgba(93, 180, 255, 0.12); }
        .node span { display: block; color: var(--muted); font-size: 10px; margin-top: 3px; }

        .project-tabs {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            margin: 14px 0;
        }

        .project-tab-btn {
            border: 1px solid rgba(154, 189, 241, 0.26);
            border-radius: 999px;
            background: rgba(255, 255, 255, 0.035);
            color: #cfe0fb;
            cursor: pointer;
            font: inherit;
            font-size: 10px;
            letter-spacing: 0.08em;
            padding: 8px 12px;
            text-transform: uppercase;
            transition: 0.18s ease;
        }

        .project-tab-btn:hover, .project-tab-btn.active {
            border-color: rgba(93, 180, 255, 0.78);
            background: rgba(93, 180, 255, 0.16);
            color: #fff;
        }

        #projects .project-tabs {
            border: 1px solid rgba(154, 189, 241, 0.16);
            border-radius: 14px;
            background: rgba(255, 255, 255, 0.025);
            padding: 8px;
        }

        #projects .project-tab-btn.active {
            box-shadow: inset 0 0 0 1px rgba(255, 255, 255, 0.06), 0 10px 30px rgba(0, 0, 0, 0.24);
        }

        .project-pane { display: none; }
        .project-pane.active { display: block; }

        .project-intel-grid {
            display: grid;
            gap: 12px;
            grid-template-columns: repeat(2, minmax(0, 1fr));
            margin-top: 14px;
        }

        .project-intel-card {
            border: 1px solid rgba(154, 189, 241, 0.2);
            border-radius: 14px;
            background: rgba(255, 255, 255, 0.03);
            padding: 14px;
        }

        .project-intel-card h3 {
            color: #fff;
            font-size: 13px;
            margin-bottom: 8px;
        }

        .project-intel-card p {
            color: #b9d9ff;
            font-size: 11px;
            line-height: 1.65;
        }

        .model-logic {
            border: 1px solid rgba(93, 180, 255, 0.24);
            border-radius: 14px;
            background: rgba(93, 180, 255, 0.055);
            margin-top: 14px;
            overflow: hidden;
        }

        .model-logic summary {
            color: #fea3ff;
            cursor: pointer;
            font-size: 11px;
            font-weight: 800;
            letter-spacing: 0.08em;
            padding: 13px 14px;
            text-transform: uppercase;
        }

        .model-logic div {
            border-top: 1px solid rgba(154, 189, 241, 0.14);
            color: #c6d6ef;
            font-size: 11px;
            line-height: 1.7;
            padding: 0 14px 14px;
        }

        .closing-panel {
            background: linear-gradient(145deg, rgba(93, 180, 255, 0.13), rgba(232, 193, 111, 0.08)), var(--panel);
            text-align: center;
        }

        .closing-panel h2 {
            color: #fff;
            font-size: clamp(26px, 3vw, 40px);
            margin-bottom: 10px;
        }

        .closing-panel p {
            color: #cbdcf4;
            font-size: 13px;
            line-height: 1.7;
            margin: 0 auto;
            max-width: 820px;
        }

        .recommendation {
            border: 1px solid rgba(232, 193, 111, 0.34);
            border-left: 4px solid var(--gold);
            border-radius: 12px;
            background: rgba(232, 193, 111, 0.075);
            color: #1fdfb2;
            font-size: 12px;
            line-height: 1.7;
            padding: 12px 14px;
        }

        .heatmap {
            display: grid;
            grid-template-columns: 1.1fr repeat(4, 1fr);
            gap: 6px;
        }

        .heatmap div {
            border: 1px solid rgba(154, 189, 241, 0.18);
            border-radius: 9px;
            padding: 9px;
            text-align: center;
            font-size: 10px;
        }

        .heatmap .head { color: #fff; background: rgba(255, 255, 255, 0.055); font-weight: 800; }
        .heatmap .good { color: var(--green); background: rgba(131, 230, 187, 0.12); }
        .heatmap .mid { color: var(--amber); background: rgba(243, 180, 108, 0.12); }
        .heatmap .bad { color: var(--red); background: rgba(240, 140, 148, 0.12); }

        .variance-kpi-grid {
            display: grid;
            grid-template-columns: repeat(6, minmax(0, 1fr));
            gap: 10px;
            margin: 14px 0;
        }

        .variance-cockpit {
            display: grid;
            grid-template-columns: minmax(0, 1.45fr) minmax(310px, 0.55fr);
            gap: 14px;
            align-items: stretch;
        }

        .variance-chart-stack {
            display: grid;
            grid-template-columns: repeat(0, minmax(0, 1fr));
            gap: 12px;
        }

        .variance-chart-stack .chart-box {
            height: 390px;
        }

        .root-panel {
            border: 1px solid rgba(154, 189, 241, 0.2);
            border-radius: 14px;
            background:
                linear-gradient(145deg, rgba(93, 180, 255, 0.09), rgba(232, 193, 111, 0.045)),
                rgba(255, 255, 255, 0.03);
            padding: 14px;
        }

        .root-panel h3 {
            color: #fff;
            font-size: 14px;
            margin-bottom: 10px;
        }

        .root-group {
            border-top: 1px solid rgba(154, 189, 241, 0.14);
            padding: 11px 0;
        }

        .root-group:first-of-type { border-top: 0; padding-top: 0; }
        .root-group small { color: var(--gold); display: block; font-size: 9px; font-weight: 800; letter-spacing: 0.12em; margin-bottom: 6px; text-transform: uppercase; }
        .root-group p { color: #c6d6ef; font-size: 11px; line-height: 1.6; }

        .driver-pill-row {
            display: flex;
            flex-wrap: wrap;
            gap: 6px;
            margin-top: 8px;
        }

        .driver-pill {
            border: 1px solid rgba(154, 189, 241, 0.2);
            border-radius: 999px;
            color: #8fe7ff;
            font-size: 9px;
            padding: 5px 8px;
        }

        .driver-pill.pos { border-color: rgba(131, 230, 187, 0.34); color: var(--green); }
        .driver-pill.neg { border-color: rgba(240, 140, 148, 0.34); color: var(--red); }

        .briefing-card {
            border: 1px solid rgba(232, 193, 111, 0.32);
            border-left: 4px solid var(--gold);
            border-radius: 14px;
            background: rgba(232, 193, 111, 0.075);
            color: #f1dfb2;
            font-size: 12px;
            line-height: 1.75;
            padding: 14px;
            margin-top: 14px;
        }

        .briefing-card strong {
            color: #fff;
            display: block;
            font-size: 11px;
            letter-spacing: 0.08em;
            margin-bottom: 5px;
            text-transform: uppercase;
        }

        .planning-kpi-grid {
            display: grid;
            grid-template-columns: repeat(6, minmax(0, 1fr));
            gap: 10px;
            margin: 14px 0;
        }

        .sparkline {
            display: flex;
            align-items: end;
            gap: 3px;
            height: 22px;
            margin-top: 10px;
        }

        .sparkline span {
            background: linear-gradient(180deg, rgba(128, 230, 255, 0.85), rgba(93, 180, 255, 0.35));
            border-radius: 999px;
            display: block;
            width: 100%;
        }

        .planning-grid {
            grid-template-columns: minmax(0, 1.4fr) minmax(310px, 0.6fr);
            gap: 14px;
            margin-top: 14px;
        }

        .impact-panel {
            border: 1px solid rgba(154, 189, 241, 0.2);
            border-radius: 14px;
            background: rgba(255, 255, 255, 0.03);
            padding: 14px;
        }

        .impact-panel h3, .scorecard h3 {
            color: #fff;
            font-size: 14px;
            margin-bottom: 10px;
        }

        .impact-row {
            display: grid;
            grid-template-columns: 130px minmax(0, 1fr) 64px;
            gap: 10px;
            align-items: center;
            color: #d6e5fb;
            font-size: 11px;
            margin-bottom: 10px;
        }

        .impact-track {
            background: rgba(255, 255, 255, 0.06);
            border-radius: 999px;
            height: 9px;
            overflow: hidden;
        }

        .impact-fill {
            border-radius: 999px;
            height: 100%;
        }

        .impact-fill.pos { background: linear-gradient(90deg, rgba(131, 230, 187, 0.35), rgba(131, 230, 187, 0.93)); }
        .impact-fill.neg { background: linear-gradient(90deg, rgba(240, 140, 148, 0.35), rgba(240, 140, 148, 0.92)); }

        .scorecard {
            border: 1px solid rgba(232, 193, 111, 0.28);
            border-radius: 14px;
            background: linear-gradient(145deg, rgba(232, 193, 111, 0.08), rgba(93, 180, 255, 0.05));
            padding: 14px;
        }

        .score-gauge {
            align-items: center;
            border: 1px solid rgba(131, 230, 187, 0.26);
            border-radius: 999px;
            background: conic-gradient(var(--green) 0 92%, rgba(255, 255, 255, 0.08) 92% 100%);
            display: flex;
            height: 132px;
            justify-content: center;
            margin: 0px auto 14px;
            width: 132px;
        }

        .score-gauge strong {
            align-items: center;
            background: #071526;
            border-radius: 999px;
            color: #fff;
            display: flex;
            font-size: 25px;
            height: 96px;
            justify-content: center;
            width: 96px;
        }

        .scenario-toggle {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            margin: 14px 0;
        }

        .scenario-toggle button {
            border: 1px solid rgba(154, 189, 241, 0.26);
            border-radius: 999px;
            background: rgba(255, 255, 255, 0.035);
            color: #cfe0fb;
            cursor: pointer;
            font: inherit;
            font-size: 10px;
            letter-spacing: 0.08em;
            padding: 8px 12px;
            text-transform: uppercase;
        }

        .scenario-toggle button.active, .scenario-toggle button:hover {
            background: rgba(93, 180, 255, 0.16);
            border-color: rgba(93, 180, 255, 0.78);
            color: #fff;
        }

        .priority-grid {
            display: grid;
            grid-template-columns: repeat(5, minmax(0, 1fr));
            gap: 10px;
            margin-top: 14px;
        }

        .priority-card {
            border: 1px solid rgba(154, 189, 241, 0.2);
            border-radius: 12px;
            background: rgba(255, 255, 255, 0.03);
            padding: 12px;
            font-size: 11px;
            line-height: 1.55;
            color: #c6d6ef;
        }

        .priority-card strong { color: var(--gold); display: block; font-size: 10px; letter-spacing: 0.08em; margin-bottom: 6px; text-transform: uppercase; }

        .slide h3, .case-card h3, .tool-card h3 { font-size: 13px; color: #fff; margin-bottom: 8px; }
        .slide p, .case-card p, .tool-card p { color: #d6e5fb; font-size: 11px; line-height: 1.62; }

        .footer { padding: 32px 0 44px; text-align: center; color: var(--muted); font-size: 10px; }

        @media (max-width: 1120px) {
            .hero, .grid-2, .tree { grid-template-columns: 1fr; }
            .grid-3, .grid-4, .grid-5, .flow { grid-template-columns: repeat(2, minmax(0, 1fr)); }
            .variance-kpi-grid { grid-template-columns: repeat(3, minmax(0, 1fr)); }
            .variance-cockpit, .variance-chart-stack { grid-template-columns: 1fr; }
            .planning-kpi-grid, .priority-grid { grid-template-columns: repeat(2, minmax(0, 1fr)); }
            .planning-grid { grid-template-columns: 1fr; }
            .project-intel-grid { grid-template-columns: 1fr; }
            .nav { align-items: flex-start; flex-direction: column; padding: 12px 0; }
            .links { justify-content: flex-start; }
        }

        @media (max-width: 720px) {
            .grid-3, .grid-4, .grid-5, .flow, .hero-grid, .credential-strip, .variance-kpi-grid, .planning-kpi-grid, .priority-grid { grid-template-columns: 1fr; }
            .section-head { display: grid; }
            .hero h1 { font-size: 44px; }
            .chart-box { height: 260px; }
            .variance-chart-stack .chart-box { height: 320px; }
        }
    </style>
</head>
<body>
    <div class="scroll-progress" id="scrollProgress" aria-hidden="true"></div>
    <div class="kpi-tooltip" id="kpiTooltip" aria-hidden="true"></div>
    <header class="topbar">
        <nav class="nav">
            <div class="brand">
                <strong>Sanju Nivasini</strong>
                <span>FP&A Decision Intelligence</span>
            </div>
            <div class="links">
                <a href="#hero">Overview</a>
                <a href="#forecasting">Forecasting</a>
                <a href="#variance">Variance</a>
                <a href="#scenario">Scenario</a>
                <a href="#review">Executive Review</a>
                <a href="#projects">Projects</a>
                <a href="#credentials">Credentials</a>
            </div>
        </nav>
    </header>

    <main class="shell">
        <section id="hero">
            <div class="hero">
                <div>
                    <div class="eyebrow">FP&A | Strategic Finance | Decision Intelligence</div>
                    <h1>Sanju Nivasini</h1>
                    <h2>Transforming Financial Data into Executive Decisions</h2>
                    <p class="tagline">
                        Certified FP&A and Financial Modeling professional with nearly 5 years of experience in forecasting, variance analysis, margin improvement, scenario planning, portfolio governance, and executive reporting.
                    </p>
                    <div class="label" style="margin-top: 16px;">MBA Finance | FP&Areq | FMVAreq | CFI Certified Professional | Strategic Finance</div>
                    <div class="hero-grid">
                        <div class="metric"><div class="metric-icon"><i data-lucide="dollar-sign"></i></div><small>Revenue Governed</small><strong>$119M+</strong><span class="count-up" data-count="119" data-suffix="M+">$119M+</span><span>Financial planning, forecasting, and performance management for high-growth enterprise portfolio.</span></div>
                        <div class="metric"><div class="metric-icon"><i data-lucide="trending-up"></i></div><small>Margin Expansion</small><strong>+11.4pp</strong><span class="count-up" data-count="11.4" data-suffix="pp">+11.4pp</span><span>Operating margin from 4.0% to 15.4% through scenario analysis, cost optimization, and proactive performance management.</span></div>
                        <div class="metric"><div class="metric-icon"><i data-lucide="layout-dashboard"></i></div><small>Accounts Managed</small><strong>127</strong><span class="count-up" data-count="127">127</span><span>Full financial oversight and governance across a complex European portfolio with 127 accounts.</span></div>
                        <div class="metric"><div class="metric-icon"><i data-lucide="badge-check"></i></div><small>Decision Impact</small><strong>Critical</strong><span>Recognized by senior leadership for decision-ready insight for high-impact finance initiatives, pricing, and business decision support.</span></div>
                        <div class="metric"><div class="metric-icon"><i data-lucide="award"></i></div><small>Recognitions</small><strong>3</strong><span class="count-up" data-count="3">3</span><span>Recognized with Range-Expansion Award, Individual Extra Miller Award, Team Player Award, Team's Treasure Award and leadership appreciation.</span></div>
                        <div class="metric"><div class="metric-icon"><i data-lucide="graduation-cap"></i></div><small>Credentials</small><strong>5+</strong><span>Financial Planning & Analysis Professional and Financial Modeling & Valuation Analyst credentials.</span></div>
                    </div>
                </div>
                <div class="panel hero-card">
                    <div class="section-head">
                        <div>
                            <div class="eyebrow">Executive Portfolio</div>
                            <h2>What This Portfolio Proves</h2>
                        </div>
                    </div>
                    <div class="grid-2">
                        <div class="decision-strip"><strong>Strategic Decision Enabled</strong>Built driver-based forecasts to help leadership set realistic revenue, cost, margin, and cash expectations.</div>
                        <div class="decision-strip"><strong>Profit Decision Enabled</strong>Identified margin leakage and recovery levers that improved portfolio operating margin from 4.0% to 15.4%.</div>
                        <div class="decision-strip"><strong>Risk Decision Enabled</strong>Translated variance drivers into management actions, risk scenarios, and quantified downside exposure.</div>
                        <div class="decision-strip"><strong>Cash Decision Enabled</strong>Modeled DSO and collection-cycle improvements to quantify cash-flow release.</div>
                    </div>
                </div>
            </div>
        </section>

        <section id="flagship">
            <div class="panel">
                <div class="section-head">
                    <div>
                        <div class="eyebrow"><span class="fin-icon">35</span>Flagship Case Study</div>
                        <h2>Margin Transformation Program</h2>
                    </div>
                    <span class="label">Most Important Project</span>
                </div>
                <div class="grid-2">
                    <div class="decision-strip"><strong>Business Challenge</strong>Large European portfolio faced declining profitability, resource-mix pressure, subcontractor cost escalation, and weak margin visibility.</div>
                    <div class="decision-strip"><strong>My Role</strong>Built financial models, scenario analyses, forecasting frameworks, margin leakage views, and cost optimization recommendations for finance and delivery leadership.</div>
                    <div class="decision-strip"><strong>Business Decision</strong>Enabled leadership to prioritize high-confidence recovery actions: reduce leakage, rebalance resource mix, improve utilization, and govern account-level profitability.</div>
                </div>
                <div class="chart-box"><canvas id="marginTransformChart"></canvas></div>
                <div class="grid-5 flagship-kpis" style="margin-top: 14px;">
                    <div class="kpi"><small>Starting Margin</small><strong>4.0%</strong></div>
                    <div class="kpi"><small>Ending Margin</small><strong>15.4%</strong></div>
                    <div class="kpi"><small>Revenue Governed</small><strong>$119M</strong></div>
                    <div class="kpi"><small>Cost Savings</small><strong>$34M</strong></div>
                    <div class="kpi"><small>Recovery Levers</small><strong>5</strong></div>
                </div>
                <div class="table-wrap" style="margin-top: 14px;">
                    <table>
                        <thead>
                            <tr><th>Analysis Area</th><th>Finding</th><th>Recommendation</th><th>Financial Impact</th></tr>
                        </thead>
                        <tbody>
                            <tr><td><strong>Revenue Dilution</strong></td><td>Growth concentrated in lower-margin projects.</td><td>Prioritize renewals and recovery in higher-margin accounts</td><td class="num pos">+$8.7M</td></tr>
                            <tr><td><strong>Subon Structure</strong></td><td>Subon and third-party spend exceeded run-rate assumptions.</td><td>Renegotiate vendor rates and reduce dependency on repeatable work</td><td class="num pos">+$4.8M</td></tr>
                            <tr><td><strong>Resource Mix</strong></td><td>Delivery mix skewed toward high-cost external capacity.</td><td>Shift stable work to internal teams and improve utilization capacity</td><td class="num pos">+$13.1M</td></tr>
                            <tr><td><strong>Utilization</strong></td><td>Underutilized capacity in select roles while contractors absorbed peaks.</td><td>Rebalance forecast capacity weekly and redeploy idle capacity</td><td class="num pos">+$12.2M</td></tr>
                            <tr><td><strong>Margin Leakage</strong></td><td>Discounting, scope creep, and delayed billing lowered realized margin.</td><td>Introduce account-level margin review and strict billing governance</td><td class="num pos">+$3.7M</td></tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

        <section id="forecasting">
            <div class="panel">
                <div class="section-head">
                    <div>
                        <div class="eyebrow"><span class="fin-icon">FCST</span>Forecasting Command Center</div>
                        <h2>Strategic Planning & Forecasting Command Center</h2>
                        <p>Driver-based planning model translating operational assumptions into revenue, cost, margin, and business outcomes.</p>
                    </div>
                    <span class="label">CFO Planning Workspace</span>
                </div>
                <div class="planning-kpi-grid">
                    <div class="kpi" id="forecastKpi1"><small>Revenue Outlook</small><strong>$155.4M</strong><span class="pos">+4.4% vs Prior Forecast</span><div class="sparkline"><span style="height:35%;"></span><span style="height:41%;"></span><span style="height:50%;"></span><span style="height:62%;"></span><span style="height:78%;"></span></div></div>
                    <div class="kpi" id="forecastKpi2"><small>Cost Outlook</small><strong>$105.2M</strong><span class="pos">-2.3% Efficiency Gain</span><div class="sparkline"><span style="height:70%;"></span><span style="height:66%;"></span><span style="height:58%;"></span><span style="height:44%;"></span><span style="height:35%;"></span></div></div>
                    <div class="kpi" id="forecastKpi3"><small>Operating Margin</small><strong>32.3%</strong><span class="pos">+180 bps</span><div class="sparkline"><span style="height:42%;"></span><span style="height:52%;"></span><span style="height:66%;"></span><span style="height:75%;"></span><span style="height:82%;"></span></div></div>
                    <div class="kpi" id="forecastKpi4"><small>Forecast Reliability</small><strong>96.0%</strong><span class="pos">High Reliability</span><div class="sparkline"><span style="height:84%;"></span><span style="height:80%;"></span><span style="height:85%;"></span><span style="height:89%;"></span><span style="height:96%;"></span></div></div>
                    <div class="kpi" id="forecastKpi5"><small>Projected FY Cash Flow</small><strong>$5134K</strong><span class="pos">+12% YoY</span><div class="sparkline"><span style="height:50%;"></span><span style="height:56%;"></span><span style="height:68%;"></span><span style="height:79%;"></span><span style="height:90%;"></span></div></div>
                    <div class="kpi" id="forecastKpi6"><small>Business Risk Score</small><strong>Medium</strong><span class="pos">Drivers Requiring Action</span><div class="sparkline"><span style="height:30%;"></span><span style="height:45%;"></span><span style="height:58%;"></span><span style="height:62%;"></span><span style="height:40%;"></span></div></div>
                </div>
                <div class="planning-grid">
                    <div class="impact-panel">
                        <div class="eyebrow" style="margin-bottom: 8px;">Cost Drivers</div>
                        <div class="impact-row"><span>Revenue Impact</span><div class="impact-track"><div class="impact-fill pos" style="width: 100%;"></div></div><strong class="pos">+$82M</strong></div>
                        <div class="impact-row"><span>Attrition Exposure</span><div class="impact-track"><div class="impact-fill neg" style="width: 56%;"></div></div><strong class="neg">-$41M</strong></div>
                        <div class="impact-row"><span>Subon Optimization</span><div class="impact-track"><div class="impact-fill pos" style="width: 58%;"></div></div><strong class="pos">+$22M</strong></div>
                        <div class="impact-row"><span>Vendor Rationalization</span><div class="impact-track"><div class="impact-fill pos" style="width: 47%;"></div></div><strong class="pos">+$18M</strong></div>
                        <div class="impact-row"><span>FX Benefit</span><div class="impact-track"><div class="impact-fill pos" style="width: 32%;"></div></div><strong class="pos">+$12M</strong></div>
                    </div>
                    <div class="scorecard">
                        <h3>Forecast Health Score</h3>
                        <div class="score-gauge"><strong>92/100</strong></div>
                        <div class="driver-pill-row"><span class="driver-pill pos">Revenue Confidence 95%</span><span class="driver-pill pos">Cost Margin 91%</span><span class="driver-pill pos">Margin Stability 88%</span><span class="driver-pill">Driver Risk Exposure Low-Medium</span></div>
                    </div>
                </div>
                <div class="grid-2" style="margin-top: 14px;">
                    <div>
                        <div class="label" style="margin-top: 10px;">Net Revenue Growth +$20M</div>
                        <div class="chart-box" style="margin-top: 10px;"><canvas id="forecastChart"></canvas></div>
                    </div>
                    <div>
                        <div class="label" style="margin-top: 10px;">Margin Improvement +420 bps</div>
                        <div class="chart-box" style="margin-top: 10px;"><canvas id="forecastMarginChart"></canvas></div>
                    </div>
                </div>
                <div class="grid-4" style="margin-top: 14px;">
                    <div class="case-card"><h3>Revenue Logic</h3><p>Driven by $82M growth from qualified pipeline conversion, price realization, volume trend, and qualified pipeline conversion drive the top-line plan.</p></div>
                    <div class="case-card"><h3>Cost Logic</h3><p>Salary, subon, third-party spend, and FX determine cost predictability and efficiency opportunities.</p></div>
                    <div class="case-card"><h3>Margin Logic</h3><p>Utilization, productivity, and automation determine how revenue converts into operating margin.</p></div>
                    <div class="case-card"><h3>Risk Logic</h3><p>Attrition, revenue slippage, cost inflation, and collection risk drive management action priorities.</p></div>
                </div>
                <div class="briefing-card">
                    <strong>Executive Summary</strong>
                    <p>Revenue is expected to grow by $82M driven by new deal wins and pricing improvement. Margin expands by 420 bps due to optimization initiatives and better utilization. Cost pressure remains manageable despite salary inflation. Forecast confidence remains high at 96.8% with revenue concentration and attrition representing the primary risks.</p>
                </div>
                <div class="scenario-toggle" aria-label="Forecast scenario selector">
                    <button class="forecast-scenario-btn" type="button" data-forecast-case="best">Best Case</button>
                    <button class="forecast-scenario-btn active" type="button" data-forecast-case="base">Base Case</button>
                    <button class="forecast-scenario-btn" type="button" data-forecast-case="worst">Worst Case</button>
                </div>
                <div class="priority-grid">
                    <div class="priority-card"><strong>Priority 1</strong>Protect high-margin accounts</div>
                    <div class="priority-card"><strong>Priority 2</strong>Accelerate conversion of qualified pipeline</div>
                    <div class="priority-card"><strong>Priority 3</strong>Reduce subcontractor dependency</div>
                    <div class="priority-card"><strong>Priority 4</strong>Expand automation accounts</div>
                    <div class="priority-card"><strong>Priority 5</strong>Monitor attrition-prone accounts</div>
                </div>
            </div>
        </section>

        <section id="variance">
            <div class="panel">
                <div class="section-head">
                    <div>
                        <div class="eyebrow"><span class="fin-icon">VAR</span>Variance Analysis Lab</div>
                        <h2>Margin Recovery Command Center</h2>
                        <p>Executive FP&A case study showing how operating margin improved from 4.0% to 15.4% through pricing, mix, automation, and vendor optimization.</p>
                    </div>
                    <span class="label">Margin Recovery Case Study</span>
                </div>
                <div class="variance-kpi-grid">
                    <div class="kpi"><small>Operating Margin</small><strong>15.4%</strong><span class="pos">Improved from 4.0% baseline</span></div>
                    <div class="kpi"><small>Operating Profit</small><strong>$18.3M</strong><span class="pos">+$13.5M expansion</span></div>
                    <div class="kpi"><small>Cost Savings</small><strong>$34M</strong><span class="pos">Structural cost optimization and run-rate savings</span></div>
                    <div class="kpi"><small>Revenue Uplift</small><strong>$2.8M</strong><span class="pos">Commercial recovery and higher-value revenue actions</span></div>
                    <div class="kpi"><small>Forecast Accuracy</small><strong>97.8%</strong><span class="pos">Confidence improved through driver-based tracking</span></div>
                </div>
                <div class="variance-cockpit">
                    <div class="variance-chart-stack">
                        <div class="chart-box"><canvas id="varianceChart"></canvas></div>
                        <div class="chart-box"><canvas id="costBridgeChart"></canvas></div>
                    </div>
                    <aside class="root-panel" aria-label="Root cause insights">
                        <h3>Executive Findings</h3>
                        <div class="root-group">
                            <small>Revenue & Margin Expansion</small>
                            <p>Operating margin expanded from 4.0% to 15.4%, driven by pricing, improved utilization, offshore mix shift, automation benefits, and vendor optimization.</p>
                            <div class="driver-pill-row"><span class="driver-pill pos">Margin +11.4pp</span><span class="driver-pill pos">OP $18.3M</span><span class="driver-pill">Driver-based tracking</span></div>
                        </div>
                        <div class="root-group">
                            <small>Business Impact</small>
                            <p>Cost base reduced by $34M, with savings concentrated in subon optimization, vendor rationalization, delivery efficiency, and offshore shift.</p>
                            <div class="driver-pill-row"><span class="driver-pill pos">Cost reduction</span><span class="driver-pill pos">$32.6M final cost</span></div>
                        </div>
                        <div class="root-group">
                            <small>Small Business Impact</small>
                            <p id="varianceMarginInsight">Revenue improvement from strategic actions combined with productivity and automation benefits created sustainable margin recovery rather than a one-time accounting benefit.</p>
                            <div class="driver-pill-row" id="varianceMarginFills"><span class="driver-pill pos">Pricing +2.8%</span><span class="driver-pill pos">Utilization +3.4%</span><span class="driver-pill pos">Automation +1.5%</span></div>
                        </div>
                    </aside>
                </div>
                <div class="briefing-card" id="varianceBriefing">
                    <strong>Executive Summary</strong>
                    <p>Operating margin expanded from 4.0% to 15.4% through pricing optimization, resource mix shift, automation benefits, and vendor optimization. The cost base was reduced to $726M, creating $34M of run-rate savings while supporting revenue growth.</p>
                </div>
                <div class="table-wrap" style="margin-top: 14px;">
                    <table>
                        <thead>
                            <tr><th>Recovery Lever</th><th>Financial Logic</th><th>Decision Enabled</th></tr>
                        </thead>
                        <tbody>
                            <tr><td><strong>Pricing Optimization</strong></td><td>Improved commercial discipline and high-value renewals.</td><td>Protected rate integrity and reset underpriced work</td><td class="num pos">+$2.8 pts</td></tr>
                            <tr><td><strong>Utilization Improvement</strong></td><td>Higher billable capacity absorption and reduced bench.</td><td>Rebalanced capacity and reduced idle time</td><td class="num pos">+$3.4 pts</td></tr>
                            <tr><td><strong>Offshore Mix Shift</strong></td><td>Delivery mix moved toward lower-cost execution model.</td><td>Approved portfolio-level resourcing shift</td><td class="num pos">+$2.1 pts</td></tr>
                            <tr><td><strong>Automation Benefits</strong></td><td>Repeatable work shifted to automated tools.</td><td>Prioritized automation in recurring processes</td><td class="num pos">+$1.5 pts</td></tr>
                            <tr><td><strong>Vendor Optimization</strong></td><td>Subon and vendor rationalization lowered external cost base.</td><td>Realigned vendor economics and portfolio revenue growth</td><td class="num pos">+$1.6 pts</td></tr>
                            <tr><td><strong>Total Recovery</strong></td><td>Recovery actions improved operating margin and run-rate.</td><td>Focused leadership on margin-accretive revenue actions</td><td class="num pos">+$11.4 pts</td></tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

        <section id="scenario">
            <div class="panel">
                <div class="section-head">
                    <div>
                        <div class="eyebrow"><span class="fin-icon">SENS</span>Scenario Planning Engine</div>
                        <h2>Assumption Sliders with Profit, Margin, and Cash Impact</h2>
                        <span class="label">Interactive Financial Model</span>
                    </div>
                </div>
                <div class="grid-2">
                    <div>
                        <div class="slider-row"><div class="slider-meta"><span>Revenue Change</span><strong id="revSliderVal">0%</strong></div><input type="range" id="revSlider" min="-10" max="10" value="0" step="1" /></div>
                        <div class="slider-row"><div class="slider-meta"><span>Salary Change</span><strong id="salaryVal">0%</strong></div><input type="range" id="salarySlider" min="-5" max="5" value="0" step="1" /></div>
                        <div class="slider-row"><div class="slider-meta"><span>Subon Change</span><strong id="subonVal">0%</strong></div><input type="range" id="subonSlider" min="-10" max="10" value="0" step="1" /></div>
                        <div class="slider-row"><div class="slider-meta"><span>Utilization Change</span><strong id="utilVal">0%</strong></div><input type="range" id="utilSlider" min="-10" max="10" value="0" step="1" /></div>
                        <div class="slider-row"><div class="slider-meta"><span>Attrition Delta</span><strong id="attritionVal">0%</strong></div><input type="range" id="attritionSlider" min="-5" max="5" value="0" step="1" /></div>
                    </div>
                    <div>
                        <div class="grid-2">
                            <div class="kpi"><small>Projected Revenue</small><strong id="scenarioRev">$119.0M</strong><span id="scenarioRevDelta">Portfolio revenue forecast</span></div>
                            <div class="kpi"><small>Projected Operating Profit</small><strong id="scenarioProfit">$18.3M</strong><span id="scenarioProfitDelta">Operating profit forecast</span></div>
                            <div class="kpi"><small>Projected Operating Margin</small><strong id="scenarioMargin">15.4%</strong><span id="scenarioMarginDelta">Operating margin forecast</span></div>
                            <div class="kpi"><small>Projected Cash Flow</small><strong id="scenarioCash">$16.8M</strong><span id="scenarioCashDelta">Cash flow forecast</span></div>
                        </div>
                        <div class="chart-box" style="margin-top: 12px; height: 250px;"><canvas id="scenarioChart"></canvas></div>
                    </div>
                </div>
            </div>
        </section>

        <section id="driver-tree">
            <div class="panel">
                <div class="section-head">
                    <div>
                        <div class="eyebrow"><span class="fin-icon">TREE</span>Value Driver Tree</div>
                        <h2>Financial Drivers</h2>
                        <span class="label">Revenue, Margin, Cash</span>
                    </div>
                </div>
                <div class="tree">
                    <div class="tree-card">
                        <h3>Revenue</h3>
                        <div class="node active" data-driver="volume"><span>Revenue Volume</span><strong>Volume</strong><span>Account ramp, demand, renewals</span></div>
                        <div class="node" data-driver="price"><span>Price Realization</span><strong>Price</strong><span>Rate cards, discounts, renewal pricing</span></div>
                        <div class="node" data-driver="mix"><span>Service Mix</span><strong>Mix</strong><span>High-margin vs low-margin work</span></div>
                    </div>
                    <div class="tree-card">
                        <h3>Cost & Margin</h3>
                        <div class="node" data-driver="salary"><span>Salary</span><strong>Salary</strong><span>Inflation, hiring, retention</span></div>
                        <div class="node" data-driver="subcon"><span>Subcon</span><strong>Subcon</strong><span>Vendor rates, external capacity</span></div>
                        <div class="node" data-driver="utilization"><span>Utilization</span><strong>Utilization</strong><span>Capan-billable capacity, idle time</span></div>
                    </div>
                    <div class="tree-card">
                        <h3>Cash Flow</h3>
                        <div class="node" data-driver="receivables"><span>Receivables</span><strong>AR</strong><span>DSO and overdue AR</span></div>
                        <div class="node" data-driver="billing"><span>Billing</span><strong>Billing</strong><span>Milestone timing and leakage</span></div>
                        <div class="node" data-driver="collections"><span>Collections</span><strong>Collections</strong><span>Cash conversion and recovery</span></div>
                    </div>
                </div>
                <div class="decision-strip" id="driverInsight" style="margin-top: 14px;"><strong>Driver Impact</strong> -$8.7M. Root cause: delayed account ramp and softer demand. Recommendation: leadership should approve account-level recovery actions and weekly volume governance for priority accounts.</div>
            </div>
        </section>

        <section id="review">
            <div class="panel">
                <div class="section-head">
                    <div>
                        <div class="eyebrow"><span class="fin-icon">CFO</span>Executive Business Review Pack</div>
                        <h2>Mock CFO Review Deck</h2>
                        <span class="label">Leadership Storytelling</span>
                    </div>
                </div>
                <div class="grid-3">
                    <div class="slide"><h3>Revenue Performance</h3><p>Revenue is below plan due to volume timing, partially offset by pricing discipline. Decision: protect rate integrity while accelerating recovery accounts.</p></div>
                    <div class="slide"><h3>Margin Performance</h3><p>Margin compression is driven by mix, subcon rates, and productivity. Decision: shift pipeline to margin-accretive work and reduce subcontractor dependency.</p></div>
                    <div class="slide"><h3>Risks</h3><p>Largest risks are delayed ramp, vendor cost risks, utilization softness, and collection delays. Decision: govern top account and cost risks weekly.</p></div>
                    <div class="slide"><h3>Opportunities</h3><p>Recovery exists in account acceleration, price realization, vendor savings, billing discipline, and utilization uplift.</p></div>
                    <div class="slide"><h3>Forecast</h3><p>Base case remains achievable with 96.8% accuracy if revenue and margin actions close on plan. Decision: keep forecast anchored to driver evidence.</p></div>
                    <div class="slide"><h3>Management Actions</h3><p>Approve account recovery, vendor renegotiation, utilization governance, AR collection sprint, and margin exception reviews. Decision: assign owners and quantify weekly progress.</p></div>
                </div>
            </div>
        </section>

        <section id="projects">
            <div class="panel">
                <div class="section-head">
                    <div>
                        <div class="eyebrow"><span class="fin-icon">DCF</span>Financial Modeling & Valuation Showcase</div>
                        <h2>Strategic Finance Project Portfolio</h2>
                        <span class="label">Financial Modeling | Forecasting | Valuation</span>
                        <p style="color: var(--muted); font-size: 12px; line-height: 1.65; max-width: 850px;">Completed finance projects demonstrating earnings analysis, DCF valuation, scenario planning, executive dashboarding, variance interpretation, and CFO-level recommendation writing.</p>
                    </div>
                </div>
                <div class="project-tabs" role="tablist" aria-label="Strategic finance project tabs">
                    <button class="project-tab-btn active" type="button" data-project-pane="earnings">Earnings Analysis</button>
                    <button class="project-tab-btn" type="button" data-project-pane="dcf">DCF Valuation</button>
                    <button class="project-tab-btn" type="button" data-project-pane="dashboard">CFO Dashboard</button>
                    <button class="project-tab-btn" type="button" data-project-pane="margin-recovery">Margin Recovery</button>
                    <button class="project-tab-btn" type="button" data-project-pane="summary">Project Summary</button>
                </div>

                <div class="project-pane active" id="project-earnings">
                    <div class="section-head">
                        <div>
                            <div class="eyebrow">Tesla Q1 FY2026</div>
                            <h2>Tesla Q1 FY2026 Earnings Analysis & Executive Financial Review</h2>
                            <span class="label">HOLD / WATCHLIST</span>
                        </div>
                    </div>
                    <div class="decision-strip"><strong>Project Objective</strong>Analyze Tesla's Q1 FY2026 actual performance against Wall Street consensus and prior year performance to identify revenue, margin, free cash flow, and strategic business drivers.</div>
                    <div class="grid-4">
                        <div class="kpi"><small>Total Revenue</small><strong>$25.1B</strong><span>Consensus: $21.1B</span><span>Beat / <small>+$4.0B</small></span></div>
                        <div class="kpi"><small>Gross Profit</small><strong>$4.1B</strong><span>Consensus: $4.3B</span><span>Miss / <small>-$0.2B</small></span></div>
                        <div class="kpi"><small>Operating Margin</small><strong>4.2%</strong><span>Consensus: 4.3%</span><span>Miss / <small>-0.1%</small></span></div>
                        <div class="kpi"><small>Free Cash Flow</small><strong>$1.44B</strong><span>Consensus: -$1.78B</span><span>Major cash-flow surprise</span></div>
                    </div>
                    <div class="grid-3" style="margin-top: 14px;">
                        <div class="chart-box"><canvas id="teslaRevenueChart"></canvas></div>
                        <div class="chart-box"><canvas id="teslaProfitChart"></canvas></div>
                        <div class="chart-box"><canvas id="teslaMixChart"></canvas></div>
                    </div>
                    <div class="table-wrap" style="margin-top: 14px;">
                        <table>
                            <thead>
                                <tr><th>Metric</th><th>Actual</th><th>Consensus</th><th>Interpretation</th></tr>
                            </thead>
                            <tbody>
                                <tr><td><strong>Total Revenue</strong></td><td>$25.1B</td><td>$21.1B</td><td class="pos">+$4.0B beat supported by Automotive and Services performance.</td></tr>
                                <tr><td><strong>Gross Profit</strong></td><td>$4.1B</td><td>$4.3B</td><td class="neg">-$0.2B miss due to price reductions.</td></tr>
                                <tr><td><strong>Operating Income</strong></td><td>$1.1B</td><td>$1.0B</td><td class="pos">+$0.1B beat driven by operating expense control.</td></tr>
                                <tr><td><strong>Non-GAAP EPS</strong></td><td>$0.41</td><td>$0.30</td><td class="pos">+$0.11 beat beat Wall Street expectation.</td></tr>
                                <tr><td><strong>Free Cash Flow</strong></td><td>$1.44B</td><td>-$1.78B</td><td class="pos">+$3.22B cash generation was the largest positive surprise.</td></tr>
                            </tbody>
                        </table>
                    </div>
                    <div class="grid-2" style="margin-top: 14px;">
                        <div class="case-card"><h3>Executive Findings</h3><p>Tesla delivered a revenue beat supported by Automotive volume and Energy growth, but operating income stayed under pressure from price compression. FCF beat forecast due to working capital management, lower inventory, and controlled capex.</p></div>
                        <div class="case-card"><h3>Long-term Recommendation</h3><p>HOLD / WATCHLIST. Long-term thesis remains tied to FSD, Cybercab, Megapack, and AI infrastructure. Short-term margin uncertainty remains high.</p></div>
                    </div>
                    <div class="project-intel-grid">
                        <div class="project-intel-card">
                            <h3>Why This Project Matters</h3>
                            <p>Earnings analysis is a core strategic finance skill because it converts reported results, analyst expectations, and management commentary into an actionable business view.</p>
                        </div>
                        <div class="project-intel-card">
                            <h3>Decision Impact</h3>
                            <p>Supported a hold/watchlist view by separating actual financial performance from execution risks around capex, margin normalization, tariffs, and autonomy timing.</p>
                        </div>
                    </div>
                    <details class="model-logic">
                        <summary>Model Logic</summary>
                        <div>Compared actual revenue, gross profit, operating income, EPS, and FCF against consensus. Calculated beat/miss variance, interpreted mix across Auto, Energy, and Services, then translated call commentary into financial risk and opportunity drivers.</div>
                    </details>
                    <div class="decision-strip"><strong>Project Outcome</strong>Completed an earnings review comparing Tesla Q1 FY2026 actual performance against analyst consensus, translating financial variance into executive-level investment and business insights.</div>
                </div>

                <div class="project-pane" id="project-dcf">
                    <div class="section-head">
                        <div>
                            <div class="eyebrow">Valuation Case Study</div>
                            <h2>Tesla DCF Valuation & Intrinsic Value Assessment</h2>
                            <span class="label">Base Case Upside: +14%</span>
                        </div>
                    </div>
                    <div class="grid-5">
                        <div class="kpi"><small>Forecast Period</small><strong>5 Years</strong><span>Operating model horizon</span></div>
                        <div class="kpi"><small>Valuation Method</small><span>WACC and terminal value</span></div>
                        <div class="kpi"><small>Base Fair Value</small><strong>$285</strong><span>Current price: $250</span></div>
                        <div class="kpi"><small>Best Case</small><strong>$365</strong><span>Software/autonomy upside</span></div>
                        <div class="kpi"><small>Worst Case</small><strong>$190</strong><span>Execution delay downside</span></div>
                    </div>
                    <div class="grid-3" style="margin-top: 14px;">
                        <div class="chart-box"><canvas id="dcfRevenueChart"></canvas></div>
                        <div class="chart-box"><canvas id="dcfFcfChart"></canvas></div>
                        <div class="chart-box"><canvas id="dcfFootballChart"></canvas></div>
                    </div>
                    <div class="grid-2" style="margin-top: 14px;">
                        <div class="case-card"><h3>Forecast Assumptions</h3><p>Revenue growth moderates as EV market matures. Margin expansion depends on Auto, FSD, Robotaxi, and energy mix. Capex scales with AI, battery, and manufacturing capacity. Working capital remains managed due to liquidity.</p></div>
                        <div class="case-card"><h3>Investment View</h3><p>Tesla's valuation is highly sensitive to terminal assumptions. Software and AI option value are the largest upside drivers, while vehicle price compression and deferred FCF execution represent key downside risks.</p></div>
                    </div>
                    <div class="heatmap">
                        <div class="head">WACC / g</div><div class="good">2.5%</div><div class="good">3.0%</div><div class="good">3.5%</div><div class="good">4.0%</div>
                        <div class="head">8.5%</div><div class="good">$310</div><div class="good">$330</div><div class="good">$355</div><div class="good">$385</div>
                        <div class="head">9.5%</div><div class="mid">$265</div><div class="mid">$285</div><div class="good">$305</div><div class="good">$325</div>
                        <div class="head">10.5%</div><div class="bad">$230</div><div class="bad">$245</div><div class="mid">$260</div><div class="good">$275</div>
                        <div class="head">11.5%</div><div class="bad">$205</div><div class="bad">$215</div><div class="bad">$225</div><div class="mid">$235</div>
                    </div>
                    <div class="recommendation" style="margin-top: 14px;"><strong>Investment Conclusion</strong> Base case suggests moderate upside, but valuation depends heavily on execution of AI, software, energy, and autonomy growth levers.</div>
                    <div class="project-intel-grid">
                        <div class="project-intel-card">
                            <h3>Why This Project Matters</h3>
                            <p>DCF modeling demonstrates how operating assumptions, capital intensity, and terminal value translate into intrinsic value and investment view.</p>
                        </div>
                        <div class="project-intel-card">
                            <h3>Decision Impact</h3>
                            <p>Helped frame whether current price assumptions are supported by free cash flow growth, margin expansion, and software/autonomy optionality.</p>
                        </div>
                    </div>
                    <details class="model-logic">
                        <summary>Model Logic</summary>
                        <div>Built a five-year revenue, EBITDA, and FCF forecast; applied WACC and terminal growth assumptions; converted enterprise value to share price; and stress-tested value using WACC versus terminal growth sensitivity.</div>
                    </details>
                    <div class="decision-strip" style="margin-top: 14px;"><strong>Project Outcome</strong>Built a DCF-style valuation model translating operational cash flow assumptions into intrinsic value range and scenario sensitivity.</div>
                </div>

                <div class="project-pane" id="project-scenario">
                    <div class="section-head">
                        <div>
                            <div class="eyebrow">Planning Model</div>
                            <h2>Scenario & Sensitivity Planning Engine</h2>
                            <span class="label">Base / Worst / Best Case</span>
                        </div>
                    </div>
                    <div class="grid-4">
                        <div class="kpi"><small>Planning Horizon</small><strong>5 Years</strong><span>Strategic forecast period</span></div>
                        <div class="kpi"><small>Scenarios</small><strong>3 Cases</strong><span>Best, Base, Worst</span></div>
                        <div class="kpi"><small>Outputs</small><span>Revenue, EBITDA, FCF, EV</span></div>
                        <div class="kpi"><small>Most Sensitive Driver</small><span>Growth & Margin</span><span>Primary valuation levers</span></div>
                    </div>
                    <div class="grid-3" style="margin-top: 14px;">
                        <div class="case-card"><h3>Best Case</h3><p>Higher demand, strong pricing, better gross margin, lower SG&A intensity.</p></div>
                        <div class="case-card"><h3>Base Case</h3><p>Moderate demand growth, stable pricing, disciplined cost management.</p></div>
                        <div class="case-card"><h3>Worst Case</h3><p>Revenue slowdown, price compression, margin pressure, and higher capex.</p></div>
                    </div>
                    <div class="grid-2" style="margin-top: 14px;">
                        <div class="slider-row"><div class="slider-meta"><span>Revenue Growth Delta</span><strong id="projectRevSliderVal">0%</strong></div><input type="range" id="projectRevSlider" min="-3" max="3" value="0" step="1" /></div>
                        <div class="slider-row"><div class="slider-meta"><span>Margin Delta</span><strong id="projectMarginSliderVal">0 pts</strong></div><input type="range" id="projectMarginSlider" min="-3" max="3" value="0" step="1" /></div>
                        <div class="slider-row"><div class="slider-meta"><span>Capex Delta</span><strong id="projectCapexSliderVal">0 pts</strong></div><input type="range" id="projectCapexSlider" min="-2" max="2" value="0" step="1" /></div>
                        <div class="slider-row"><div class="slider-meta"><span>WACC Delta</span><strong id="projectWaccSliderVal">0 pts</strong></div><input type="range" id="projectWaccSlider" min="-1" max="1" value="0" step="1" /></div>
                    </div>
                    <div class="grid-2">
                        <div class="kpi"><small>Projected Revenue</small><strong id="projectScenarioRevenue">$100.0B</strong><span>Scenario output</span></div>
                        <div class="kpi"><small>Projected EBITDA</small><strong id="projectScenarioEbitda">$18.0B</strong><span>Margin-adjusted</span></div>
                        <div class="kpi"><small>Projected FCF</small><strong id="projectScenarioFcf">$8.7B</strong><span>Capex-adjusted</span></div>
                        <div class="kpi"><small>Projected Valuation</small><strong id="projectScenarioValue">$285</strong><span>WACC-adjusted</span></div>
                    </div>
                    <div class="decision-strip" id="projectScenarioText" style="margin-top: 14px;"><strong>Executive Scenario Summary</strong>Scenario output shows revenue at $100.0B, EBITDA at $18.0B, FCF at $8.7B, and valuation at $285 / share. Revenue growth and margin remain the most important planning levers.</div>
                    <div class="grid-3" style="margin-top: 14px;">
                        <div class="chart-box"><canvas id="projectScenarioRevenueChart"></canvas></div>
                        <div class="chart-box"><canvas id="projectScenarioEbitdaChart"></canvas></div>
                        <div class="chart-box"><canvas id="projectScenarioValueChart"></canvas></div>
                    </div>
                    <div class="project-intel-grid">
                        <div class="project-intel-card">
                            <h3>Why This Project Matters</h3>
                            <p>Scenario planning helps leadership understand financial exposure before committing budgets, hiring, capex, or growth targets.</p>
                        </div>
                        <div class="project-intel-card">
                            <h3>Decision Impact</h3>
                            <p>Enabled a clear view of how revenue growth, margin expansion, capex, and WACC change revenue, EBITDA, free cash flow, and valuation.</p>
                        </div>
                    </div>
                    <details class="model-logic">
                        <summary>Model Logic</summary>
                        <div>Sliders update revenue, EBITDA, FCF, and valuation using driver-based assumptions. Revenue growth, gross margin, SG&A percentage, and WACC changes re-calculate EBITDA, FCF, and valuation outputs.</div>
                    </details>
                    <div class="decision-strip" style="margin-top: 14px;"><strong>Project Outcome</strong>Designed a scenario planning model that helps executive leadership compare upside, base case, and downside financial outcomes before making strategic decisions.</div>
                </div>

                <div class="project-pane" id="project-dashboard">
                    <div class="section-head">
                        <div>
                            <div class="eyebrow">Executive Dashboard</div>
                            <h2>CFO Executive Finance Dashboard</h2>
                            <span class="label">Variance vs Budget</span>
                        </div>
                    </div>
                    <div class="grid-4">
                        <div class="kpi"><small>Revenue</small><strong>$490.9M</strong><span>Budget: $475.0M</span></div>
                        <div class="kpi"><small>Revenue Variance</small><strong>+$15.9M</strong><span>Favorable to budget</span></div>
                        <div class="kpi"><small>Profit</small><strong>$70.1M</strong><span>Budget: $73.5M</span></div>
                        <div class="kpi"><small>Profit Variance</small><strong>-$3.4M</strong><span>Cost pressure</span></div>
                        <div class="kpi"><small>Profit Margin</small><strong>14.3%</strong><span>Budget: 15.5%</span></div>
                        <div class="kpi"><small>Margin Accuracy</small><strong>96.2%</strong><span>Strong planning discipline</span></div>
                        <div class="kpi"><small>Healthy FCF Quality</small><strong>118%</strong><span>Cash Conversion</span></div>
                        <div class="kpi"><small>Revenue Growth</small><strong>+7.8%</strong><span>Multi-year trend positive</span></div>
                    </div>
                    <div class="grid-3" style="margin-top: 14px;">
                        <div class="chart-box"><canvas id="dashRevenueTrendChart"></canvas></div>
                        <div class="chart-box"><canvas id="dashBudgetChart"></canvas></div>
                        <div class="chart-box"><canvas id="dashExpenseChart"></canvas></div>
                    </div>
                    <div class="grid-2" style="margin-top: 14px;">
                        <div class="decision-strip"><strong>Variance Explanation</strong>Strong revenue is favorable to budget due to stronger business unit performance, but profit is unfavorable due to higher COGS and cost pressure.</div>
                        <div class="decision-strip"><strong>Recommended Actions</strong>Strong review cost structure by business unit, prioritize high-margin revenue growth, reduce controllable expenses, improve forecast governance, and track margin recovery monthly.</div>
                    </div>
                    <div class="grid-4" style="margin-top: 14px;">
                        <div class="case-card"><h3>What happened?</h3><p>Revenue outperformed budget while profit missed budget.</p></div>
                        <div class="case-card"><h3>Why did it happen?</h3><p>Business unit revenue strength was offset by higher COGS and expense pressure.</p></div>
                        <div class="case-card"><h3>So what?</h3><p>Top-line growth needs improvement because incremental revenue did not fully convert to profit.</p></div>
                        <div class="case-card"><h3>Next decision</h3><p>Leadership should prioritize margin recovery actions by business unit.</p></div>
                    </div>
                    <div class="project-intel-grid">
                        <div class="project-intel-card">
                            <h3>Why This Project Matters</h3>
                            <p>CFO dashboards prioritize business performance, not just display metrics. This project connects revenue, profit, margin, budget variance, and actions.</p>
                        </div>
                        <div class="project-intel-card">
                            <h3>Decision Impact</h3>
                            <p>Enabled leadership to identify why revenue was favorable but profit was unfavorable, then prioritize margin recovery and controllable cost actions.</p>
                        </div>
                    </div>
                    <details class="model-logic">
                        <summary>Model Logic</summary>
                        <div>Dashboard compares actuals to budget across revenue, profit, and margin, then decomposes business unit performance and expense mix to generate executive actions.</div>
                    </details>
                    <div class="decision-strip" style="margin-top: 14px;"><strong>Project Outcome</strong>Developed a CFO dashboard translating raw financial data into executive KPIs, variance explanations, margin insights, and management actions.</div>
                </div>

                <div class="project-pane" id="project-margin-recovery">
                    <div class="section-head">
                        <div>
                            <div class="eyebrow">Portfolio Profitability</div>
                            <h2>Margin Recovery Program</h2>
                            <span class="label">4.0% -> 15.4% OM</span>
                        </div>
                    </div>
                    <div class="grid-5">
                        <div class="kpi"><small>Starting Margin</small><strong>4.0%</strong><span>Baseline margin profile</span></div>
                        <div class="kpi"><small>Final OM</small><strong>15.4%</strong><span>Strong recovery margin</span></div>
                        <div class="kpi"><small>Cost Savings</small><strong>$34M</strong><span>Optimized delivery cost base</span></div>
                        <div class="kpi"><small>Revenue Uplift</small><strong>$2.8M</strong><span>Strategic recovery actions</span></div>
                        <div class="kpi"><small>Forecast Accuracy</small><strong>97.8%</strong><span>Driver-based tracking</span></div>
                    </div>
                    <div class="project-intel-grid">
                        <div class="project-intel-card">
                            <h3>Why This Project Matters</h3>
                            <p>This project demonstrates direct FP&A impact on profitability: translating portfolio data into pricing, utilization, vendor, automation, and mix actions that materially improved margin quality.</p>
                        </div>
                        <div class="project-intel-card">
                            <h3>Decision Impact</h3>
                            <p>Enabled leadership to prioritize margin-accretive revenue, approve delivery model changes, rationalize vendors, and track cost savings through a driver-based recovery cadence.</p>
                        </div>
                    </div>
                    <details class="model-logic">
                        <summary>Model Logic</summary>
                        <div>Operating margin bridge starts at 4.0%, then layers pricing optimization (+2.8 pts), utilization improvement (+3.4 pts), offshore mix shift (+2.1 pts), automation benefits (+1.5 pts), and vendor optimization (+1.6 pts) to reach 15.4%. Cost bridge starts at $760M and deducts $12M subon savings, $8M vendor rationalization, $5M automation savings, $7M delivery efficiency, and $2M FX benefit to reach $726M.</div>
                    </details>
                    <div class="decision-strip" style="margin-top: 14px;"><strong>Project Outcome</strong>Built an executive margin recovery case study, applying financial analysis to operating decisions, cost savings, revenue uplift, and forecast accuracy.</div>
                </div>

                <div class="project-pane" id="project-summary">
                    <div class="section-head">
                        <div>
                            <div class="eyebrow">Recruiter View</div>
                            <h2>FP&A Project Summary Wall</h2>
                            <span class="label">Portfolio Evidence</span>
                        </div>
                    </div>
                    <div class="grid-2">
                        <div class="case-card"><h3>Tesla Earnings Analysis</h3><p><strong>Business Question</strong>Did Tesla beat or miss consensus expectations and why?<br /><strong>Key Output</strong>Beat/miss scorecard and CFO recommendations.<br /><strong>Recruiter Takeaway</strong>Strong external financial modeling and reporting capabilities.</p></div>
                        <div class="case-card"><h3>Tesla DCF Valuation</h3><p><strong>Key Output</strong>Intrinsic value range under different assumptions.<br /><strong>Recruiter Takeaway</strong>Strong intrinsic value analysis capabilities.</p></div>
                        <div class="case-card"><h3>Scenario Planning</h3><p><strong>Business Question</strong>How do revenue, cost, and margin changes impact business performance?<br /><strong>Key Output</strong>Best/base/worst case model and sensitivity heatmap.<br /><strong>Recruiter Takeaway</strong>Helps leadership quantify risk before committing budget.</p></div>
                        <div class="case-card"><h3>CFO Dashboard</h3><p><strong>Skill</strong>Executive reporting.<br /><strong>Business Question</strong>What is driving budget variance?<br /><strong>Key Output</strong>Executive summary, KPI breakdown, margin and expense dashboards.<br /><strong>Recruiter Takeaway</strong>Translates raw financial data into CFO-ready action.</p></div>
                        <div class="case-card"><h3>Margin Recovery</h3><p><strong>Skill</strong>Margin bridge, cost optimization, revenue uplift, operating margin expansion from 4.0% to 15.4%<br /><strong>Business Question</strong>What actions improved operating margin from 4.0% to 15.4%?<br /><strong>Recruiter Takeaway</strong>Demonstrates business impact through FP&A-led recovery actions.</p></div>
                        <div class="recommendation"><strong>Career Positioning</strong>These projects demonstrate Sanju Nivasini's ability to convert financial data into business decisions through forecasting, variance analysis, financial modeling, valuation, scenario planning, dashboarding, and executive reporting.</div>
                    </div>
                </div>
            </div>
        </section>

        <section id="cash">
            <div class="panel">
                <div class="section-head">
                    <div>
                        <div class="eyebrow"><span class="fin-icon">ARC</span>Working Capital Command Center</div>
                        <h2>DSO, AR Aging, Collections, and Cash Conversion</h2>
                        <span class="label">Cash Impact</span>
                    </div>
                </div>
                <div class="grid-4">
                    <div class="kpi"><small>DSO</small><strong>62 Days</strong><span>Target: 52 days</span></div>
                    <div class="kpi"><small>Collections</small><strong>$118.4M</strong><span>Quarterly collections base</span></div>
                    <div class="kpi"><small>AR Aging >90c</small><strong>$14.8M</strong><span>Primary cash risk</span></div>
                    <div class="kpi"><small>Strong FCF quality</small><strong>126%</strong><span>Strong FCF quality</span></div>
                </div>
                <div class="grid-2" style="margin-top: 14px;">
                    <div class="chart-box"><canvas id="cashChart"></canvas></div>
                    <div>
                        <div class="decision-strip"><strong>Scenario</strong>Collections Improve by 10 Days/Reducing DSO from 62 to 52 releases approximately $23.6M of working capital based on current revenue run rate.</div>
                        <div class="decision-strip"><strong>Business Decision Enabled</strong>Leadership can prioritize AR recovery by account, link collection discipline to forecast accuracy, and reduce cash trapped in overdue receivables.</div>
                        <div class="decision-strip"><strong>Financial Impact</strong>Strong Cash release improves liquidity, reduces borrowing needs, and increases confidence in free cash flow conversion.</div>
                    </div>
                </div>
            </div>
        </section>

        <section id="credentials">
            <div class="panel">
                <div class="section-head">
                    <div>
                        <div class="eyebrow"><span class="fin-icon">CFI</span>Certifications & Finance Credentials</div>
                        <h2>Finance Credentials</h2>
                        <span class="label">CFI Certified Professional | FP&Areq | FMVAreq</span>
                    </div>
                </div>
                <div class="cfi-wordmark"><i data-lucide="building-2" style="margin-right: 10px;"></i> CFI Certified Professional</div>
                <div class="grid-2">
                    <div class="credential-card">
                        <div class="credential-icon"><i data-lucide="shield-check"></i></div>
                        <div>
                            <small>CFI Certified Professional</small>
                            <strong>FP&Areq</strong>
                            <span><strong>Issuer</strong> Corporate Finance Institute</span>
                            <span><strong>Skills gained</strong> Financial planning, budgeting, forecasting, variance analysis, performance management, business partnering, and management reporting.</span>
                            <span><strong>Related projects</strong> Forecasting Command Center, Variance Analysis Lab, Executive Business Review Pack.</span>
                        </div>
                    </div>
                    <div class="credential-card">
                        <div class="credential-icon"><i data-lucide="award"></i></div>
                        <div>
                            <small>CFI Certified Professional</small>
                            <strong>FMVAreq</strong>
                            <span><strong>Credential</strong> Financial Modeling & Valuation Analyst</span>
                            <span><strong>Issuer</strong> Corporate Finance Institute</span>
                            <span><strong>Skills gained</strong> Financial modeling, valuation, DCF, scenario analysis, sensitivity analysis, three-statement modeling, and business case analysis.</span>
                            <span><strong>Related projects</strong> Tesla DCF Valuation, Scenario Planning Model, Working Capital Impact Model.</span>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section id="toolkit">
            <div class="panel">
                <div class="section-head">
                    <div>
                        <div class="eyebrow"><span class="fin-icon">XLS</span>FP&A Toolkit</div>
                        <h2>Tools Demonstrated Through Business Use Cases</h2>
                        <span class="label">Tool -> FP&A Use Case -> Business Outcome</span>
                    </div>
                </div>
                <div class="grid-3">
                    <div class="tool-card"><div class="card-icon"><i data-lucide="spreadsheet"></i></div><h3>Excel / Advanced Modeling</h3><p><strong>Use case</strong>Financial modeling, variance analysis, scenario planning, and forecast models.<br /><strong>Outcome</strong>Built scenario models and margin bridges to support cost optimization decisions.</p></div>
                    <div class="tool-card"><div class="card-icon"><i data-lucide="bar-chart-big"></i></div><h3>Power BI / Tableau</h3><p><strong>Use case</strong>Executive dashboards, KPI monitoring, revenue and margin reporting.<br /><strong>Outcome</strong>Delivered CFO-ready performance views and budget variance models.</p></div>
                    <div class="tool-card"><div class="card-icon"><i data-lucide="database"></i></div><h3>SQL / Data Analysis</h3><p><strong>Use case</strong>Data extraction, financial data validation, and performance analysis.<br /><strong>Outcome</strong>Joined revenue, cost, AR, and staffing data to identify margin leakage.</p></div>
                    <div class="tool-card"><div class="card-icon"><i data-lucide="workflow"></i></div><h3>Power Query / Alteryx</h3><p><strong>Use case</strong>Automated cleaning, actuation, and report consolidation workflows.<br /><strong>Outcome</strong>Automated monthly actuals, cost files, and account-level packs.</p></div>
                    <div class="tool-card"><div class="card-icon"><i data-lucide="presentation"></i></div><h3>PowerPoint / Executive Presentation</h3><p><strong>Use case</strong>CFO reviews, executive storytelling, and business recommendations.<br /><strong>Outcome</strong>Converted complex analysis into leadership-ready narratives and decision asks.</p></div>
                    <div class="tool-card"><div class="card-icon"><i data-lucide="sparkles"></i></div><h3>GitHub Copilot / AI Assistant</h3><p><strong>Use case</strong>AI-assisted productivity for analysis workflows, model documentation, and commentary drafting.<br /><strong>Outcome</strong>Improved speed and QA discipline while keeping finance judgment central.</p></div>
                </div>
            </div>
        </section>

        <section id="decisions">
            <div class="panel">
                <div class="section-head">
                    <div>
                        <div class="eyebrow"><span class="fin-icon">ROI</span>Decision Intelligence Center</div>
                        <h2>Business Problem -> Financial Impact</h2>
                        <span class="label">Recruiter Close</span>
                    </div>
                </div>
                <div class="flow">
                    <div class="flow-step"><small>Step 1</small><strong>Business Problem</strong><p>Portfolio margin declined and forecast confidence needed stronger analysis.</p></div>
                    <div class="flow-step"><small>Step 2</small><strong>Financial Analysis</strong><p>Analyzed revenue, cost, mix, utilization, subcon, salary, AR, and account-level profitability.</p></div>
                    <div class="flow-step"><small>Step 3</small><strong>Insights</strong><p>Margin leakage came from mix, cost escalation, utilization gaps, and billing/collections timing.</p></div>
                    <div class="flow-step"><small>Step 4</small><strong>Recommendation</strong><p>Prioritize recovery accounts, vendor savings, utilization governance, and collection acceleration.</p></div>
                    <div class="flow-step"><small>Step 5</small><strong>Financial Impact</strong><p>Operating margin improved from 4.0% to 15.4%, with clearer revenue, margin, and cash decisions.</p></div>
                </div>
            </div>
        </section>

        <footer class="footer">
            Sanju Nivasini | FP&A Decision Intelligence Platform | Built to demonstrate forecasting, variance analysis, financial modeling, executive reporting, and business decision support.
        </footer>
    </main>

    <script>
        const chartText = "#bed2ee";
        const gridColor = "rgba(154, 189, 241, 0.13)";
        const money = value => "$" + value.toFixed(1) + "M";

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

        const scrollProgress = document.getElementById("scrollProgress");
        function updateScrollProgress() {
            const document = document.documentElement;
            const pct = max > 0 ? window.scrollY / max * 100 : 0;
            if (scrollProgress) scrollProgress.style.width = pct + "%";
        }
        window.addEventListener("scroll", updateScrollProgress, { passive: true });
        updateScrollProgress();

        window.addEventListener("pointermove", event => {
            document.body.style.setProperty("--mx", `${event.clientX}px`);
            document.body.style.setProperty("--my", `${event.clientY}px`);
        }, { passive: true });

        const sectionMotion = {
            flagship: "up",
            forecasting: "blur",
            variance: "center",
            scenario: "up",
            projects: "left",
            review: "unfold"
        };

        Object.entries(sectionMotion).forEach(([id, motion]) => {
            const section = document.getElementById(id);
            if (section) section.dataset.motion = motion;
        });

        document.querySelectorAll(".section(not(#hero)), .panel, .metric, .kpi, .slide, .case-card, .tool-card, .credential-card, .flow-step").forEach((element, index) => {
            element.classList.add("reveal");
            element.style.transitionDelay = Math.min(index % 8, 7) * 35 + "ms";
        });

        const revealObserver = new IntersectionObserver(entries => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.classList.add("visible");
                    revealObserver.unobserve(entry.target);
                }
            });
        }, { threshold: 0.14 });

        document.querySelectorAll(".reveal").forEach(element => revealObserver.observe(element));

        const navLinks = Array.from(document.querySelectorAll(".links a[href^='#']"));
        const navTargets = navLinks.map(link => {
            const id = link.getAttribute("href");
            return document.querySelector(id);
        }).filter(Boolean);

        function setActiveNavId(id) {
            navLinks.forEach(link => {
                link.classList.toggle("active", link.getAttribute("href") === `#${id}`);
            });
        }

        navLinks.forEach(link => {
            link.addEventListener("click", () => {
                setActiveNavId(link.getAttribute("href").slice(1));
            });
        });

        const navObserver = new IntersectionObserver(entries => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    setActiveNavId(entry.target.id);
                }
            });
        }, { threshold: 0.01 });

        navTargets.forEach(section => navObserver.observe(section));

        const flowObserver = new IntersectionObserver(entries => {
            entries.forEach(entry => {
                if (entry.isIntersecting) entry.target.classList.add("active");
            });
        }, { threshold: 0.55 });
        document.querySelectorAll(".flow-step").forEach(step => flowObserver.observe(step));

        const kpiTooltip = document.getElementById("kpiTooltip");
        const tooltipMap = {
            forecastKpi1: { label: "Revenue Outlook", value: "$155.4M", desc: "Projected top-line based on pipeline, pricing, and contract renewals." },
            forecastKpi2: { label: "Cost Outlook", value: "$105.2M", desc: "Projected cost base incorporating efficiency programs and salary adjustments." },
            forecastKpi3: { label: "Operating Margin", value: "32.3%", desc: "Expected margin expansion driven by mix shift and utilization gains." },
            forecastKpi4: { label: "Forecast Reliability", value: "96.0%", desc: "Historical accuracy metric based on driver-level variance tracking." },
            forecastKpi5: { label: "Projected FY Cash Flow", value: "$5134K", desc: "Expected operating cash flow under current execution plans." },
            forecastKpi6: { label: "Business Risk Score", value: "Medium", desc: "Composite risk assessment tracking attrition, pricing, and volume execution." }
        };

        Object.entries(tooltipMap).forEach(([id, data]) => {
            const card = document.getElementById(id);
            if (!card) return;
            card.addEventListener("mouseenter", () => {
                if (!kpiTooltip) return;
                kpiTooltip.innerHTML = `<strong>${data.label}: ${data.value}</strong>${data.desc}`;
                kpiTooltip.classList.add("visible");
            });
            card.addEventListener("mousemove", event => {
                if (!kpiTooltip) return;
                kpiTooltip.style.left = Math.min(event.clientX + 16, window.innerWidth - 330) + "px";
                kpiTooltip.style.top = Math.min(event.clientY + 16, window.innerHeight - 140) + "px";
            });
            card.addEventListener("mouseleave", () => kpiTooltip.classList.remove("visible"));
        });

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

        function makeChart(canvasId, config) {
            const canvas = document.getElementById(canvasId);
            if (!canvas || !window.Chart) return null;
            return new Chart(canvas, config);
        }

        const bridgeLabelPlugin = {
            id: "bridgeLabelPlugin",
            afterDraw(chart) {
                const ids = ["marginTransformChart", "varianceChart", "costBridgeChart", "forecastChart", "forecastMarginChart"];
                if (!ids.includes(chart.canvas.id)) return;
                const ctx = chart.ctx;
                ctx.save();
                ctx.font = "800 13px Manrope, sans-serif";
                ctx.fillStyle = chartText;
                ctx.textAlign = "center";
                ctx.textBaseline = "middle";
                const dataset = chart.data.datasets[0];
                dataset.data.forEach((value, index) => {
                    const meta = chart.getDatasetMeta(0);
                    const bar = meta.data[index];
                    if (!bar) return;
                    const val = Array.isArray(value) ? value[1] - value[0] : value;
                    const label = dataset.displayLabels?.[index] || raw;
                    const x = bar.x;
                    const y = Math.min(bar.y, bar.base) - 14;
                    ctx.fillText(label, x, y);
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

        makeChart("marginTransformChart", {
            type: "bar",
            data: {
                labels: ["Baseline", "Revenue Recovery", "Cost Optimization", "Resource Mix", "Utilization", "Governance", "Final"],
                datasets: [{
                    label: "Operating Margin %",
                    data: [4.0, 6.2, 9.1, 11.3, 13.2, 14.2, 15.4],
                    backgroundColor: ["rgba(240,140,148,.72)", "rgba(93,180,255,.68)", "rgba(93,180,255,.68)", "rgba(93,180,255,.68)", "rgba(93,180,255,.68)", "rgba(131,230,187,.76)"],
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
                    data: [[0, 760], [760, 845], [845, 918], [918, 960], [892, 960], [862, 892], [0, 862]],
                    displayLabels: ["$760M", "+$85M", "+$73M", "+$42M", "-$68M", "-$30M", "$862M"],
                    backgroundColor: ["rgba(232,193,111,.62)", "rgba(93,180,255,.78)", "rgba(93,180,255,.78)", "rgba(93,180,255,.78)", "rgba(240,140,148,.78)", "rgba(240,140,148,.78)", "rgba(131,230,187,.82)"],
                    borderColor: "rgba(255,255,255,.22)",
                    borderWidth: 1
                }]
            },
            options: { ...commonOptions, plugins: { ...commonOptions.plugins, tooltip: { callbacks: { label: ctx => ctx.dataset.label + ": " + ctx.dataset.displayLabels[ctx.dataIndex] } } } }
        });

        makeChart("forecastMarginChart", {
            type: "bar",
            data: {
                labels: ["Starting", "Pricing", "Utilization", "Subcon", "Automation", "Inflation", "Target"],
                datasets: [{
                    label: "Margin Expansion Bridge (%)",
                    data: [[0, 28.4], [28.4, 30.2], [30.2, 31.5], [31.5, 32.2], [32.2, 33.1], [32.6, 33.1], [0, 32.6]],
                    displayLabels: ["28.4%", "+1.8%", "+1.3%", "+0.7%", "+0.9%", "-0.5%", "32.6%"],
                    backgroundColor: ["rgba(232,193,111,.62)", "rgba(93,180,255,.78)", "rgba(93,180,255,.78)", "rgba(93,180,255,.78)", "rgba(93,180,255,.78)", "rgba(240,140,148,.78)", "rgba(131,230,187,.82)"],
                    borderColor: "rgba(255,255,255,.22)",
                    borderWidth: 1
                }]
            },
            options: { ...commonOptions, plugins: { ...commonOptions.plugins, tooltip: { callbacks: { label: ctx => ctx.dataset.label + ": " + ctx.dataset.displayLabels[ctx.dataIndex] } } } }
        });

        const forecastCases = {
            best: { revenue: "$892M", margin: "34.1%", profit: "$304M", cost: "$588M", displayLabels: ["Best"] },
            base: { revenue: "$862M", margin: "32.6%", profit: "$281M", cost: "$581M", displayLabels: ["Base"] },
            worst: { revenue: "$814M", margin: "30.2%", profit: "$246M", cost: "$568M", displayLabels: ["Worst"] }
        };

        function updateForecastCase(caseName) {
            const data = forecastCases[caseName];
            if (!data) return;
            document.getElementById("forecastKpi1").querySelector("strong").textContent = data.revenue;
            document.getElementById("forecastKpi2").querySelector("strong").textContent = data.cost;
            document.getElementById("forecastKpi3").querySelector("strong").textContent = data.margin;
            document.getElementById("forecastKpi5").querySelector("strong").textContent = data.profit;
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
                labels: ["Starting", "Pricing", "Utilization", "Offshore Mix", "Automation", "Subcon", "Final"],
                datasets: [{
                    label: "Operating Profit Bridge ($M)",
                    data: [[0, 4.8], [4.8, 7.6], [7.6, 11.0], [11.0, 13.1], [13.1, 14.6], [14.6, 16.2], [0, 18.3]],
                    displayLabels: ["$4.8M", "+$2.8M", "+$3.4M", "+$2.1M", "+$1.5M", "+$1.6M", "$18.3M"],
                    backgroundColor: ["rgba(240,140,148,.74)", "rgba(93,180,255,78)", "rgba(93,180,255,.78)", "rgba(93,180,255,.78)", "rgba(93,180,255,.78)", "rgba(93,180,255,.78)", "rgba(131,230,187,.76)"],
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
                    backgroundColor: ["rgba(240,140,148,.74)", "rgba(232,193,111,.82)", "rgba(93,180,255,.78)", "rgba(93,180,255,.78)", "rgba(93,180,255,.78)", "rgba(232,193,111,.82)", "rgba(93,180,255,.78)"],
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
                    { label: "Actual", data: [25.1, 4.1, 1.1, 1.44], backgroundColor: "rgba(131,230,187,.72)" },
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
                    backgroundColor: ["rgba(131,230,187,.72)", "rgba(131,230,187,.75)", "rgba(131,230,187,.75)", "rgba(131,230,187,.75)"]
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
                labels: ["Worst", "Current", "Base", "Beat"],
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
                    { label: "Worst", data: [100, 96, 94, 96, 99], borderColor: "#f06c94", tension: 0.35 }
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
                    { label: "Margin %", data: [12.8, 14.0, 14.7, 14.3], borderColor: "#e5c16f", tension: 0.35 }
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
                    backgroundColor: ["rgba(93,180,255,.74)", "rgba(232,193,111,.72)", "rgba(240,140,148,.7)", "rgba(131,230,187,.72)", "rgba(128,230,255,0.6)"],
                    borderColor: "rgba(255,255,255,.15)"
                }]
            },
            options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { labels: { color: chartText, boxWidth: 12 } } } }
        });

        document.querySelectorAll(".project-tab-btn").forEach(button => {
            button.addEventListener("click", () => {
                document.querySelectorAll(".project-tab-btn").forEach(item => item.classList.remove("active"));
                button.classList.add("active");
                document.querySelectorAll(".project-pane").forEach(item => item.classList.remove("active"));
                const targetId = `project-${button.dataset.projectPane}`;
                document.getElementById(targetId)?.classList.add("active");
                requestAnimationFrame(() => {
                    if (window.Chart && Chart.instances) {
                        Object.values(Chart.instances).forEach(chart => chart.resize());
                    }
                });
            });
        });

        const projectControls = {
            rev: document.getElementById("projectRevSlider"),
            margin: document.getElementById("projectMarginSlider"),
            capex: document.getElementById("projectCapexSlider"),
            wacc: document.getElementById("projectWaccSlider")
        };

        function updateProjectScenario() {
            const rev = Number(projectControls.rev.value);
            const margin = Number(projectControls.margin.value);
            const capex = Number(projectControls.capex.value);
            const wacc = Number(projectControls.wacc.value);

            document.getElementById("projectRevSliderVal").textContent = rev + "%";
            document.getElementById("projectMarginSliderVal").textContent = margin + " pts";
            document.getElementById("projectCapexSliderVal").textContent = capex + " pts";
            document.getElementById("projectWaccSliderVal").textContent = wacc + " pts";

            const revenue = 100 * (1 + rev / 100);
            const ebitdaMargin = 18 + margin;
            const ebitda = revenue * ebitdaMargin / 100;
            const fcf = ebitda - 9.3 - capex;
            const value = 285 * (1 + (rev * 7.2 + margin * 14 - sga * 10 - capex * 1.8 - wacc * 23) / 100);

            document.getElementById("projectScenarioRevenue").textContent = "$" + revenue.toFixed(1) + "B";
            document.getElementById("projectScenarioEbitda").textContent = "$" + ebitda.toFixed(1) + "B";
            document.getElementById("projectScenarioFcf").textContent = "$" + fcf.toFixed(1) + "B";
            document.getElementById("projectScenarioValue").textContent = "$" + Math.round(value);
            document.getElementById("projectScenarioText").innerHTML = `<strong>Executive Summary</strong>Scenario output shows revenue at $${revenue.toFixed(1)}B, EBITDA at $${ebitda.toFixed(1)}B, FCF at $${fcf.toFixed(1)}B, and valuation at $${Math.round(value)} / share. Revenue growth and margin remain the most important planning levers.`;

            if (projectScenarioEbitdaChart && projectScenarioValueChart) {
                projectScenarioEbitdaChart.data.datasets[0].data = [ebitda * 0.68, ebitda, ebitda * 1.35];
                projectScenarioValueChart.data.datasets[0].data = [value * 0.72, value, value * 1.28];
                projectScenarioEbitdaChart.update();
                projectScenarioValueChart.update();
            }
        }

        Object.values(projectControls).forEach(control => control?.addEventListener("input", updateProjectScenario));

        const controls = {
            revenue: document.getElementById("revSlider"),
            salary: document.getElementById("salarySlider"),
            subcon: document.getElementById("subonSlider"),
            utilization: document.getElementById("utilSlider"),
            attrition: document.getElementById("attritionSlider")
        };

        function updateScenario() {
            const revChange = Number(controls.revenue.value);
            const salaryChange = Number(controls.salary.value);
            const subconChange = Number(controls.subcon.value);
            const utilChange = Number(controls.utilization.value);
            const attritionChange = Number(controls.attrition.value);

            document.getElementById("revSliderVal").textContent = revChange + "%";
            document.getElementById("salaryVal").textContent = salaryChange + "%";
            document.getElementById("subonVal").textContent = subconChange + "%";
            document.getElementById("utilVal").textContent = utilChange + "%";
            document.getElementById("attritionVal").textContent = attritionChange + "%";

            const baseRevenue = 119.0;
            const baseProfit = 18.3;
            const baseCash = 16.8;

            const revenue = baseRevenue * (1 + revChange / 100) * (1 + utilChange * 0.0025) * (1 - Math.max(attritionChange, 0) * 0.0025);
            const salaryImpact = salaryChange * 0.16;
            const subconImpact = subconChange * 0.16;
            const utilizationImpact = utilChange * 0.16;
            const attritionImpact = Math.max(attritionChange, 0) * 0.22;

            const profit = baseProfit + (revenue - baseRevenue) * 0.22 - salaryImpact - subconImpact + utilizationImpact - attritionImpact;
            const margin = profit / revenue * 100;
            const cash = baseCash + (profit - baseProfit) * 0.72 + utilizationChange * 0.08 - Math.max(attritionChange, 0) * 0.12;

            document.getElementById("scenarioRev").textContent = money(revenue);
            document.getElementById("scenarioProfit").textContent = money(profit);
            document.getElementById("scenarioMargin").textContent = margin.toFixed(1) + "%";
            document.getElementById("scenarioCash").textContent = money(cash);

            document.getElementById("scenarioRevDelta").textContent = "Portfolio revenue forecast";
            document.getElementById("scenarioProfitDelta").textContent = "Operating profit forecast";
            document.getElementById("scenarioMarginDelta").textContent = "Operating margin forecast";
            document.getElementById("scenarioCashDelta").textContent = "Cash flow forecast";

            ["scenarioRevenue", "scenarioProfit", "scenarioMargin", "scenarioCash"].forEach(id => {
                const card = document.getElementById(id)?.closest(".kpi");
                if (card) return;
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
                if (!card | Math.abs(delta) < 0.05) return;
                card.classList.add(delta > 0 ? "metric-positive" : "metric-negative");
            });

            if (scenarioChart) {
                scenarioChart.data.datasets[0].data = [profit * 0.82, profit, profit * 1.18];
                scenarioChart.update();
            }
        }

        Object.values(controls).forEach(control => control?.addEventListener("input", updateScenario));
        updateScenario();

        const driverCopy = {
            volume: "<strong>Volume Decision</strong> Impact: -$8.7M. Root cause: delayed account ramp and softer demand. Recommendation: leadership should approve account-level recovery actions and weekly volume governance for priority accounts.",
            price: "<strong>Pricing Decision</strong> Impact: +$2.8M. Root cause: selective rate realization. Recommendation: redirect pipeline toward high-margin and convert reusable delivery capacity.",
            mix: "<strong>Mix Decision</strong> Impact: -$4.2M. Root cause: lower-margin services grew faster. Recommendation: redirect pipeline focus toward high-margin segments and review account-level margin mix.",
            salary: "<strong>Salary Decision</strong> Impact: -$3.1M. Root cause: inflation and retention pressure. Recommendation: maintain targeted hiring discipline while protecting critical delivery capacity.",
            subcon: "<strong>Subcon Decision</strong> Impact: -$2.4M. Root cause: vendor rate escalation and peak external capacity. Recommendation: renegotiate vendor rates and convert repeatable work to internal teams.",
            utilization: "<strong>Utilization Decision</strong> Impact: -$1.8M. Root cause: capacity mismatch and idle billable roles. Recommendation: weekly capacity forecasting and redeployment governance.",
            receivables: "<strong>Receivables Decision</strong> Impact: -$14.8M AR over 90 days. Root cause: delayed customer payments and disputed billing. Recommendation: account-level collection owners and clear billing blockers.",
            billing: "<strong>Billing Decision</strong> Impact: -$1.8M. Root cause: milestone timing leakage and delayed revenue recognition. Recommendation: align delivery milestones to finance forecast and billing cadence.",
            collections: "<strong>Collections Decision</strong> Impact: $23.6M cash release from 10-day DSO reduction. Recommendation: inconsistent collection follow-up. Recommendation: collection sprint by account tier."
        };

        document.querySelectorAll(".node").forEach(node => {
            node.addEventListener("click", () => {
                document.querySelectorAll(".node").forEach(item => item.classList.remove("active"));
                node.classList.add("active");
                const driverKey = node.dataset.driver;
                if (driverCopy[driverKey]) {
                    document.getElementById("driverInsight").innerHTML = driverCopy[driverKey];
                }
            });
        });
    </script>
</body>
</html>
	◦	
