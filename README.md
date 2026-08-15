<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Sanju Nivisini | FP&A Decision Intelligence Platform</title>
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link href="https://fonts.googleapis.com/css2?family=Manrope:wght@400;500;600;700;800&display=swap" rel="stylesheet" />
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.3/dist/chart.umd.min.js"></script>
    <style>
        :root {
            --bg: #060b14;
            --ink: #eff6ff;
            --muted: #9fb3d4;
            /* Added missing line parameter variable */
            --line: rgba(145, 177, 226, 0.22);
            --panel: rgba(9, 19, 37, 0.78);
            --blue: #5db4ff;
            --cyan: #80e6ff;
            --gold: #e6c16f;
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
                radial-gradient(circle at 84% 0%, rgba(232, 193, 111, 0.14), transparent 36%),
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
            background: rgba(93, 180, 255, 0.161);
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
            min-height: calc(100vh - 96px);
            display: grid;
            align-items: center;
            grid-template-columns: 1.12fr 0.88fr;
            gap: 24px;
            padding: 20px 0;
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
        .tagline { margin-top: 18px; color: #de65fb; font-size: 17px; line-height: 1.65; max-width: 720px; }

        .hero-grid, .grid-2, .grid-3, .grid-4, .grid-5, .flow, .tree {
            display: grid;
            gap: 14px;
        }

        .hero-grid { grid-template-columns: repeat(2, minmax(0, 1fr)); margin-top: 24px; gap: 16px; }
        .grid-2 { grid-template-columns: repeat(2, minmax(0, 1fr)); }
        .grid-3 { grid-template-columns: repeat(3, minmax(0, 1fr)); }
        .grid-4 { grid-template-columns: repeat(4, minmax(0, 1fr)); }
        .grid-5 { grid-template-columns: repeat(5, minmax(0, 1fr)); }
        .flow { grid-template-columns: repeat(6, minmax(0, 1fr)); }
        .tree { grid-template-columns: repeat(3, minmax(0, 1fr)); }

        .metric, .kpi, .slide, .case-card, .tool-card, .tree-card, .credential-card, .flow-step {
            border: 1px solid rgba(154, 189, 241, 0.2);
            border-radius: 14px;
            background: rgba(255, 255, 255, 0.035);
            padding: 14px;
        }

        .hero .metric {
            background: linear-gradient(145deg, rgba(93, 180, 255, 0.11), rgba(232, 193, 111, 0.055)), rgba(255, 255, 255, 0.04);
            border: 1px solid rgba(154, 189, 241, 0.26);
            min-height: 179px;
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

        .metric:hover, .kpi:hover, .slide:hover, .case-card:hover, .tool-card:hover, .tree-card:hover, .credential-card:hover, .flow-step:hover {
            border-color: rgba(93, 180, 255, 0.52);
            box-shadow: 0 16px 42px rgba(0, 0, 0, 0.28);
            transform: translateY(-3px);
            transition: all 0.22s ease;
        }

        .fin-icon {
            align-items: center;
            border: 1px solid rgba(232, 193, 111, 0.38);
            border-radius: 8px;
            background: rgba(232, 193, 111, 0.1);
            color: #fd6895;
            display: inline-flex;
            font-size: 10px;
            font-weight: 800;
            justify-content: center;
            letter-spacing: 0.04em;
            margin-right: 8px;
            min-width: 28px;
            padding: 0 6px;
            vertical-align: middle;
        }

        /* Responsive Media Queries */
        @media (max-width: 1120px) {
            .hero, .grid-2, .tree { grid-template-columns: 1fr; }
            .grid-3, .grid-4, .grid-5, .flow { grid-template-columns: repeat(2, minmax(0, 1fr)); }
        }

        @media (max-width: 720px) {
            .grid-3, .grid-4, .grid-5, .flow, .hero-grid, .credential-strip { grid-template-columns: 1fr; }
            .hero h1 { font-size: 44px; }
        }
    </style>
</head>
<body>
    <div class="scroll-progress" id="scrollProgress" aria-hidden="true"></div>
    <div class="kpi-tooltip" id="kpiTooltip" aria-hidden="true"></div>

    <header class="topbar">
        <nav class="nav">
            <div class="brand">
                <strong>Sanju Nivisini</strong>
                <span>FP&A Decision Intelligence</span>
            </div>
            <div class="links">
                <a href="#flagship">Case Study</a>
                <a href="#forecasting">Forecasting</a>
                <a href="#variance">VAR Analysis</a>
                <a href="#scenario">Scenario</a>
                <a href="#review">Executive Review</a>
                <a href="#credentials">Credentials</a>
            </div>
        </nav>
    </header>

    <main class="shell">
        <section class="hero" id="hero">
            <div>
                <div class="eyebrow"><span class="fin-icon">FP&A</span> Strategic Finance | Decision Intelligence</div>
                <h1>Sanju Nivisini</h1>
                <h2>Transforming Financial Data into Executive Decisions</h2>
                <p class="tagline">Certified FP&A and Financial Modeling professional with nearly 5 years of experience in forecasting, variance analysis, margin improvement, scenario planning, portfolio governance, and executive reporting.</p>
            </div>
            <div class="hero-grid">
                <div class="metric">
                    <div class="label"><span class="fin-icon">REVENUE</span> European Business</div>
                    <div class="value">$119M+</div>
                </div>
                <div class="metric">
                    <div class="label"><span class="fin-icon">MARGIN</span> Recovery</div>
                    <div class="value">+11.4%</div>
                </div>
            </div>
        </section>
    </main>

    <script>
        const scrollProgress = document.getElementById("scrollProgress");
        window.addEventListener("scroll", () => {
            const totalHeight = document.documentElement.scrollHeight - window.innerHeight;
            const progress = (window.scrollY / totalHeight) * 100;
            scrollProgress.style.width = progress + "%";
        });

        window.addEventListener("pointermove", (e) => {
            document.body.style.setProperty("--mx", e.clientX + "px");
            document.body.style.setProperty("--my", e.clientY + "px");
        });
    </script>
</body>
</html>
