[my-body-dashboard.html](https://github.com/user-attachments/files/27572763/my-body-dashboard.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>My Body — Val's Health Dashboard</title>
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;0,600;1,300;1,400&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --cream: #F7F3EE;
    --warm-white: #FDFAF6;
    --terracotta: #C4704A;
    --terracotta-light: #E8A882;
    --terracotta-pale: #F5E3D7;
    --sage: #7A9B7E;
    --sage-light: #B5CCBA;
    --sage-pale: #E8F0E9;
    --brown: #6B4F3A;
    --brown-light: #9B7B65;
    --charcoal: #3D3530;
    --muted: #9A8880;
    --in-range: #5C8F62;
    --in-range-bg: #E8F2EA;
    --out-range: #B85C4A;
    --out-range-bg: #F5E8E5;
    --card-shadow: 0 2px 24px rgba(61,53,48,0.07);
    --card-shadow-hover: 0 8px 40px rgba(61,53,48,0.13);
  }

  body {
    font-family: 'DM Sans', sans-serif;
    background-color: var(--cream);
    color: var(--charcoal);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* Subtle texture overlay */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.03'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 0;
    opacity: 0.4;
  }

  .app {
    position: relative;
    z-index: 1;
    max-width: 420px;
    margin: 0 auto;
    padding: 0 0 100px;
    min-height: 100vh;
  }

  /* Header */
  .header {
    padding: 52px 24px 24px;
    display: flex;
    flex-direction: column;
    gap: 4px;
    animation: fadeUp 0.6s ease both;
  }

  .header-eyebrow {
    font-family: 'DM Sans', sans-serif;
    font-size: 11px;
    font-weight: 500;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--muted);
  }

  .header-title {
    font-family: 'Cormorant Garamond', serif;
    font-size: 38px;
    font-weight: 300;
    color: var(--charcoal);
    line-height: 1.1;
    letter-spacing: -0.01em;
  }

  .header-title span {
    color: var(--terracotta);
    font-style: italic;
  }

  .header-date {
    font-size: 13px;
    color: var(--muted);
    font-weight: 300;
    margin-top: 2px;
  }

  /* Nav tabs */
  .nav {
    display: flex;
    gap: 4px;
    padding: 0 24px 28px;
    animation: fadeUp 0.6s 0.1s ease both;
  }

  .nav-tab {
    flex: 1;
    padding: 8px 4px;
    background: none;
    border: none;
    font-family: 'DM Sans', sans-serif;
    font-size: 12px;
    font-weight: 400;
    color: var(--muted);
    cursor: pointer;
    border-bottom: 1.5px solid transparent;
    transition: all 0.2s ease;
    letter-spacing: 0.04em;
  }

  .nav-tab.active {
    color: var(--terracotta);
    border-bottom-color: var(--terracotta);
    font-weight: 500;
  }

  /* Section */
  .section {
    display: none;
    padding: 0 20px;
    flex-direction: column;
    gap: 16px;
  }

  .section.active {
    display: flex;
    animation: fadeUp 0.4s ease both;
  }

  /* Cards */
  .card {
    background: var(--warm-white);
    border-radius: 20px;
    padding: 22px 24px;
    box-shadow: var(--card-shadow);
    transition: box-shadow 0.3s ease;
  }

  .card:hover {
    box-shadow: var(--card-shadow-hover);
  }

  .card-label {
    font-size: 11px;
    font-weight: 500;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .card-label-dot {
    width: 6px;
    height: 6px;
    border-radius: 50%;
    background: var(--sage);
    animation: pulse 2s infinite;
  }

  /* Glucose Hero Card */
  .glucose-card {
    background: linear-gradient(145deg, #3D3530 0%, #5C4438 100%);
    color: white;
    border-radius: 24px;
    padding: 28px 24px;
    box-shadow: 0 8px 32px rgba(61,53,48,0.25);
    position: relative;
    overflow: hidden;
  }

  .glucose-card::before {
    content: '';
    position: absolute;
    top: -40px;
    right: -40px;
    width: 180px;
    height: 180px;
    background: radial-gradient(circle, rgba(196,112,74,0.3) 0%, transparent 70%);
    border-radius: 50%;
  }

  .glucose-card::after {
    content: '';
    position: absolute;
    bottom: -60px;
    left: -30px;
    width: 200px;
    height: 200px;
    background: radial-gradient(circle, rgba(122,155,126,0.2) 0%, transparent 70%);
    border-radius: 50%;
  }

  .glucose-label {
    font-size: 11px;
    font-weight: 500;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: rgba(255,255,255,0.5);
    margin-bottom: 8px;
    position: relative;
    z-index: 1;
  }

  .glucose-value-row {
    display: flex;
    align-items: flex-end;
    gap: 12px;
    position: relative;
    z-index: 1;
    margin-bottom: 6px;
  }

  .glucose-number {
    font-family: 'Cormorant Garamond', serif;
    font-size: 76px;
    font-weight: 300;
    line-height: 1;
    color: white;
    letter-spacing: -2px;
  }

  .glucose-unit {
    font-size: 14px;
    color: rgba(255,255,255,0.5);
    padding-bottom: 12px;
    font-weight: 300;
  }

  .glucose-trend {
    font-size: 28px;
    padding-bottom: 8px;
  }

  .glucose-status {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    padding: 5px 12px;
    border-radius: 100px;
    font-size: 12px;
    font-weight: 500;
    position: relative;
    z-index: 1;
    margin-bottom: 20px;
  }

  .glucose-status.in-range {
    background: rgba(92,143,98,0.25);
    color: #98D4A0;
  }

  .glucose-status.out-range {
    background: rgba(184,92,74,0.25);
    color: #F5A898;
  }

  .glucose-status-dot {
    width: 5px;
    height: 5px;
    border-radius: 50%;
    background: currentColor;
    animation: pulse 1.5s infinite;
  }

  /* Mini sparkline */
  .sparkline-container {
    position: relative;
    z-index: 1;
  }

  .sparkline-label {
    font-size: 10px;
    color: rgba(255,255,255,0.35);
    letter-spacing: 0.08em;
    text-transform: uppercase;
    margin-bottom: 8px;
  }

  .sparkline {
    width: 100%;
    height: 48px;
    overflow: visible;
  }

  /* Range indicator */
  .range-bar {
    margin-top: 12px;
    position: relative;
    z-index: 1;
  }

  .range-track {
    height: 4px;
    background: rgba(255,255,255,0.1);
    border-radius: 4px;
    position: relative;
    overflow: hidden;
  }

  .range-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--sage-light), var(--terracotta-light));
    border-radius: 4px;
    width: 72%;
  }

  .range-labels {
    display: flex;
    justify-content: space-between;
    margin-top: 6px;
  }

  .range-label-text {
    font-size: 10px;
    color: rgba(255,255,255,0.35);
  }

  /* Stats row */
  .stats-row {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 12px;
  }

  .stat-card {
    background: var(--warm-white);
    border-radius: 16px;
    padding: 16px 14px;
    box-shadow: var(--card-shadow);
    text-align: center;
  }

  .stat-value {
    font-family: 'Cormorant Garamond', serif;
    font-size: 26px;
    font-weight: 400;
    color: var(--charcoal);
    line-height: 1;
    margin-bottom: 4px;
  }

  .stat-label {
    font-size: 10px;
    color: var(--muted);
    letter-spacing: 0.06em;
    text-transform: uppercase;
    font-weight: 400;
  }

  /* Whoop Card */
  .whoop-card {
    background: var(--warm-white);
    border-radius: 20px;
    padding: 22px 24px;
    box-shadow: var(--card-shadow);
  }

  .whoop-metrics {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
    margin-top: 16px;
  }

  .whoop-metric {
    display: flex;
    flex-direction: column;
    gap: 6px;
  }

  .whoop-metric-label {
    font-size: 11px;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 0.08em;
    font-weight: 400;
  }

  .whoop-metric-value {
    font-family: 'Cormorant Garamond', serif;
    font-size: 32px;
    font-weight: 400;
    color: var(--charcoal);
    line-height: 1;
  }

  .whoop-metric-value span {
    font-family: 'DM Sans', sans-serif;
    font-size: 13px;
    color: var(--muted);
    font-weight: 300;
  }

  .recovery-ring {
    display: flex;
    align-items: center;
    gap: 16px;
    padding: 16px 0 0;
    border-top: 1px solid rgba(61,53,48,0.06);
    margin-top: 16px;
  }

  .ring-svg {
    flex-shrink: 0;
  }

  .ring-info {
    flex: 1;
  }

  .ring-label {
    font-size: 11px;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 0.08em;
    margin-bottom: 3px;
  }

  .ring-value {
    font-family: 'Cormorant Garamond', serif;
    font-size: 28px;
    font-weight: 400;
    color: var(--sage);
    line-height: 1;
  }

  .ring-desc {
    font-size: 12px;
    color: var(--muted);
    margin-top: 2px;
  }

  /* Chart section */
  .chart-card {
    background: var(--warm-white);
    border-radius: 20px;
    padding: 22px 24px;
    box-shadow: var(--card-shadow);
  }

  .chart-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 20px;
  }

  .chart-title {
    font-family: 'Cormorant Garamond', serif;
    font-size: 20px;
    font-weight: 400;
    color: var(--charcoal);
  }

  .chart-period {
    display: flex;
    gap: 8px;
  }

  .period-btn {
    font-size: 11px;
    font-weight: 400;
    color: var(--muted);
    background: none;
    border: none;
    cursor: pointer;
    padding: 4px 8px;
    border-radius: 8px;
    transition: all 0.2s;
    font-family: 'DM Sans', sans-serif;
  }

  .period-btn.active {
    background: var(--terracotta-pale);
    color: var(--terracotta);
    font-weight: 500;
  }

  .chart-area {
    width: 100%;
    height: 140px;
    overflow: visible;
  }

  /* Patterns section */
  .insight-card {
    background: var(--warm-white);
    border-radius: 20px;
    padding: 22px 24px;
    box-shadow: var(--card-shadow);
    display: flex;
    flex-direction: column;
    gap: 14px;
  }

  .insight-item {
    display: flex;
    gap: 14px;
    align-items: flex-start;
    padding-bottom: 14px;
    border-bottom: 1px solid rgba(61,53,48,0.06);
  }

  .insight-item:last-child {
    padding-bottom: 0;
    border-bottom: none;
  }

  .insight-icon {
    width: 36px;
    height: 36px;
    border-radius: 12px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 18px;
    flex-shrink: 0;
  }

  .insight-icon.sage { background: var(--sage-pale); }
  .insight-icon.terra { background: var(--terracotta-pale); }
  .insight-icon.brown { background: #F5EDEA; }

  .insight-text {
    flex: 1;
  }

  .insight-title {
    font-size: 13px;
    font-weight: 500;
    color: var(--charcoal);
    margin-bottom: 3px;
  }

  .insight-desc {
    font-size: 12px;
    color: var(--muted);
    line-height: 1.5;
    font-weight: 300;
  }

  /* Log Section */
  .log-form {
    background: var(--warm-white);
    border-radius: 20px;
    padding: 22px 24px;
    box-shadow: var(--card-shadow);
  }

  .log-title {
    font-family: 'Cormorant Garamond', serif;
    font-size: 22px;
    font-weight: 400;
    color: var(--charcoal);
    margin-bottom: 18px;
  }

  .input-group {
    margin-bottom: 14px;
  }

  .input-label {
    font-size: 11px;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 0.08em;
    font-weight: 500;
    margin-bottom: 6px;
    display: block;
  }

  .input-field {
    width: 100%;
    padding: 12px 14px;
    border: 1.5px solid rgba(61,53,48,0.1);
    border-radius: 12px;
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    color: var(--charcoal);
    background: var(--cream);
    outline: none;
    transition: border-color 0.2s;
  }

  .input-field:focus {
    border-color: var(--terracotta-light);
  }

  .input-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
  }

  .log-btn {
    width: 100%;
    padding: 14px;
    background: var(--terracotta);
    color: white;
    border: none;
    border-radius: 14px;
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    font-weight: 500;
    cursor: pointer;
    margin-top: 6px;
    letter-spacing: 0.03em;
    transition: all 0.2s ease;
  }

  .log-btn:hover {
    background: var(--brown);
    transform: translateY(-1px);
    box-shadow: 0 4px 16px rgba(196,112,74,0.3);
  }

  .log-btn:active {
    transform: translateY(0);
  }

  /* Recent entries */
  .entries-card {
    background: var(--warm-white);
    border-radius: 20px;
    padding: 22px 24px;
    box-shadow: var(--card-shadow);
  }

  .entry-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 12px 0;
    border-bottom: 1px solid rgba(61,53,48,0.06);
  }

  .entry-item:last-child {
    border-bottom: none;
    padding-bottom: 0;
  }

  .entry-left {
    display: flex;
    flex-direction: column;
    gap: 3px;
  }

  .entry-time {
    font-size: 12px;
    color: var(--muted);
    font-weight: 300;
  }

  .entry-note {
    font-size: 13px;
    color: var(--charcoal);
  }

  .entry-glucose {
    font-family: 'Cormorant Garamond', serif;
    font-size: 24px;
    font-weight: 400;
  }

  .entry-glucose.in { color: var(--in-range); }
  .entry-glucose.out { color: var(--out-range); }

  /* Bottom nav */
  .bottom-nav {
    position: fixed;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    width: 100%;
    max-width: 420px;
    background: rgba(253,250,246,0.92);
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
    border-top: 1px solid rgba(61,53,48,0.08);
    display: flex;
    padding: 12px 0 28px;
    z-index: 100;
  }

  .bottom-tab {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 4px;
    background: none;
    border: none;
    cursor: pointer;
    padding: 6px 0;
    transition: all 0.2s;
  }

  .bottom-tab-icon {
    font-size: 20px;
    transition: transform 0.2s;
  }

  .bottom-tab.active .bottom-tab-icon {
    transform: translateY(-2px);
  }

  .bottom-tab-label {
    font-family: 'DM Sans', sans-serif;
    font-size: 10px;
    font-weight: 400;
    color: var(--muted);
    letter-spacing: 0.05em;
    text-transform: uppercase;
    transition: color 0.2s;
  }

  .bottom-tab.active .bottom-tab-label {
    color: var(--terracotta);
    font-weight: 500;
  }

  /* Section label */
  .section-label {
    font-family: 'Cormorant Garamond', serif;
    font-size: 22px;
    font-weight: 400;
    color: var(--charcoal);
    padding: 4px 4px 0;
  }

  /* Animations */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(16px); }
    to { opacity: 1; transform: translateY(0); }
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.4; }
  }

  .card-anim-1 { animation: fadeUp 0.5s 0.15s ease both; }
  .card-anim-2 { animation: fadeUp 0.5s 0.25s ease both; }
  .card-anim-3 { animation: fadeUp 0.5s 0.35s ease both; }
  .card-anim-4 { animation: fadeUp 0.5s 0.45s ease both; }

  /* ── CHECK-IN TAB ── */
  .checkin-hero {
    background: linear-gradient(145deg, #2E3D35 0%, #4A6352 100%);
    border-radius: 24px;
    padding: 28px 24px;
    box-shadow: 0 8px 32px rgba(46,61,53,0.3);
    position: relative;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    gap: 16px;
  }

  .checkin-hero::before {
    content: '';
    position: absolute;
    top: -50px; right: -50px;
    width: 200px; height: 200px;
    background: radial-gradient(circle, rgba(181,204,186,0.25) 0%, transparent 70%);
    border-radius: 50%;
  }

  .checkin-hero-label {
    font-size: 11px;
    font-weight: 500;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: rgba(255,255,255,0.5);
    position: relative; z-index: 1;
  }

  .checkin-locate-btn {
    background: rgba(255,255,255,0.12);
    border: 1.5px solid rgba(255,255,255,0.2);
    border-radius: 16px;
    padding: 16px 20px;
    display: flex;
    align-items: center;
    gap: 14px;
    cursor: pointer;
    transition: all 0.2s;
    position: relative; z-index: 1;
    width: 100%;
    text-align: left;
  }

  .checkin-locate-btn:hover {
    background: rgba(255,255,255,0.18);
  }

  .checkin-locate-icon {
    font-size: 28px;
    flex-shrink: 0;
  }

  .checkin-locate-text {
    flex: 1;
  }

  .checkin-locate-title {
    font-size: 15px;
    font-weight: 500;
    color: white;
    margin-bottom: 2px;
  }

  .checkin-locate-sub {
    font-size: 12px;
    color: rgba(255,255,255,0.5);
    font-weight: 300;
  }

  .checkin-locate-arrow {
    font-size: 18px;
    color: rgba(255,255,255,0.4);
  }

  /* Places list */
  .places-card {
    background: var(--warm-white);
    border-radius: 20px;
    padding: 20px 20px;
    box-shadow: var(--card-shadow);
  }

  .place-item {
    display: flex;
    align-items: center;
    gap: 14px;
    padding: 13px 0;
    border-bottom: 1px solid rgba(61,53,48,0.06);
    cursor: pointer;
    transition: all 0.2s;
  }

  .place-item:last-child { border-bottom: none; padding-bottom: 0; }
  .place-item:first-child { padding-top: 0; }

  .place-item:hover .place-checkin-btn {
    background: var(--terracotta);
    color: white;
  }

  .place-icon {
    width: 42px; height: 42px;
    border-radius: 14px;
    display: flex; align-items: center; justify-content: center;
    font-size: 20px;
    flex-shrink: 0;
  }

  .place-icon.gym { background: var(--sage-pale); }
  .place-icon.food { background: var(--terracotta-pale); }
  .place-icon.cafe { background: #F5EDDF; }
  .place-icon.other { background: var(--cream); }

  .place-info { flex: 1; min-width: 0; }

  .place-name {
    font-size: 14px;
    font-weight: 500;
    color: var(--charcoal);
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .place-type {
    font-size: 11px;
    color: var(--muted);
    margin-top: 1px;
    font-weight: 300;
  }

  .place-checkin-btn {
    padding: 7px 14px;
    border-radius: 100px;
    border: 1.5px solid var(--terracotta-light);
    background: none;
    color: var(--terracotta);
    font-size: 12px;
    font-weight: 500;
    cursor: pointer;
    font-family: 'DM Sans', sans-serif;
    transition: all 0.2s;
    flex-shrink: 0;
  }

  /* Check-in celebration modal */
  .checkin-modal {
    position: fixed;
    inset: 0;
    z-index: 600;
    display: flex;
    align-items: flex-end;
    pointer-events: none;
    opacity: 0;
    transition: opacity 0.3s ease;
  }

  .checkin-modal.open {
    opacity: 1;
    pointer-events: all;
  }

  .checkin-modal-sheet {
    background: var(--warm-white);
    border-radius: 28px 28px 0 0;
    padding: 32px 28px 48px;
    width: 100%;
    max-width: 420px;
    margin: 0 auto;
    box-shadow: 0 -8px 40px rgba(61,53,48,0.15);
    transform: translateY(100%);
    transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 12px;
    text-align: center;
  }

  .checkin-modal.open .checkin-modal-sheet {
    transform: translateY(0);
  }

  .checkin-modal-emoji {
    font-size: 56px;
    line-height: 1;
  }

  .checkin-modal-msg {
    font-family: 'Cormorant Garamond', serif;
    font-size: 28px;
    font-weight: 400;
    color: var(--charcoal);
    line-height: 1.2;
    font-style: italic;
  }

  .checkin-modal-place {
    font-size: 13px;
    color: var(--muted);
    font-weight: 300;
  }

  .checkin-modal-glucose {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    padding: 8px 18px;
    border-radius: 100px;
    font-size: 13px;
    font-weight: 500;
    margin-top: 4px;
  }

  .checkin-modal-glucose.good { background: var(--in-range-bg); color: var(--in-range); }
  .checkin-modal-glucose.watch { background: var(--out-range-bg); color: var(--out-range); }

  .checkin-modal-close {
    margin-top: 8px;
    padding: 13px 40px;
    background: var(--charcoal);
    color: white;
    border: none;
    border-radius: 100px;
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.2s;
  }

  /* Check-in history */
  .checkin-history-card {
    background: var(--warm-white);
    border-radius: 20px;
    padding: 20px;
    box-shadow: var(--card-shadow);
  }

  .checkin-entry {
    display: flex;
    gap: 12px;
    align-items: flex-start;
    padding: 11px 0;
    border-bottom: 1px solid rgba(61,53,48,0.06);
  }

  .checkin-entry:last-child { border-bottom: none; padding-bottom: 0; }
  .checkin-entry:first-child { padding-top: 0; }

  .checkin-entry-icon {
    width: 36px; height: 36px;
    border-radius: 12px;
    display: flex; align-items: center; justify-content: center;
    font-size: 16px; flex-shrink: 0;
  }

  .checkin-entry-info { flex: 1; }
  .checkin-entry-name { font-size: 13px; font-weight: 500; color: var(--charcoal); }
  .checkin-entry-time { font-size: 11px; color: var(--muted); font-weight: 300; margin-top: 1px; }

  .checkin-entry-glucose {
    font-family: 'Cormorant Garamond', serif;
    font-size: 22px;
    font-weight: 400;
    flex-shrink: 0;
  }

  /* ── VIBES / SPOTIFY TAB ── */
  .vibes-hero {
    background: linear-gradient(145deg, #1A1A2E 0%, #2D1B4E 100%);
    border-radius: 24px;
    padding: 28px 24px;
    box-shadow: 0 8px 32px rgba(26,26,46,0.4);
    position: relative;
    overflow: hidden;
  }

  .vibes-hero::before {
    content: '';
    position: absolute;
    top: -40px; right: -40px;
    width: 200px; height: 200px;
    background: radial-gradient(circle, rgba(29,185,84,0.2) 0%, transparent 70%);
  }

  .vibes-hero::after {
    content: '';
    position: absolute;
    bottom: -60px; left: -20px;
    width: 180px; height: 180px;
    background: radial-gradient(circle, rgba(196,112,74,0.2) 0%, transparent 70%);
  }

  .vibes-label {
    font-size: 11px;
    font-weight: 500;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: rgba(255,255,255,0.4);
    margin-bottom: 10px;
    position: relative; z-index: 1;
  }

  .vibes-greeting {
    font-family: 'Cormorant Garamond', serif;
    font-size: 28px;
    font-weight: 300;
    color: white;
    line-height: 1.2;
    margin-bottom: 6px;
    position: relative; z-index: 1;
  }

  .vibes-greeting span { color: #1DB954; font-style: italic; }

  .vibes-sub {
    font-size: 13px;
    color: rgba(255,255,255,0.45);
    font-weight: 300;
    position: relative; z-index: 1;
  }

  .mood-vibe-row {
    display: flex;
    gap: 10px;
    overflow-x: auto;
    padding-bottom: 4px;
    scrollbar-width: none;
  }

  .mood-vibe-row::-webkit-scrollbar { display: none; }

  .vibe-chip {
    flex-shrink: 0;
    padding: 10px 16px;
    border-radius: 100px;
    border: 1.5px solid rgba(61,53,48,0.1);
    background: var(--warm-white);
    font-family: 'DM Sans', sans-serif;
    font-size: 12px;
    color: var(--muted);
    cursor: pointer;
    transition: all 0.2s;
    white-space: nowrap;
    display: flex;
    align-items: center;
    gap: 6px;
    box-shadow: var(--card-shadow);
  }

  .vibe-chip:hover, .vibe-chip.active {
    background: var(--charcoal);
    color: white;
    border-color: var(--charcoal);
  }

  .playlist-card {
    background: var(--warm-white);
    border-radius: 20px;
    padding: 0;
    box-shadow: var(--card-shadow);
    overflow: hidden;
  }

  .playlist-item {
    display: flex;
    align-items: center;
    gap: 14px;
    padding: 14px 20px;
    border-bottom: 1px solid rgba(61,53,48,0.06);
    cursor: pointer;
    transition: background 0.2s;
  }

  .playlist-item:last-child { border-bottom: none; }
  .playlist-item:hover { background: var(--cream); }
  .playlist-item.featured { background: linear-gradient(135deg, rgba(29,185,84,0.05), rgba(196,112,74,0.05)); }

  .playlist-art {
    width: 52px; height: 52px;
    border-radius: 10px;
    display: flex; align-items: center; justify-content: center;
    font-size: 24px;
    flex-shrink: 0;
    position: relative;
  }

  .playlist-art-bg {
    position: absolute; inset: 0;
    border-radius: 10px;
    opacity: 0.85;
  }

  .playlist-art-emoji { position: relative; z-index: 1; }

  .playlist-info { flex: 1; min-width: 0; }

  .playlist-name {
    font-size: 14px;
    font-weight: 500;
    color: var(--charcoal);
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .playlist-desc {
    font-size: 11px;
    color: var(--muted);
    font-weight: 300;
    margin-top: 2px;
  }

  .playlist-badge {
    font-size: 10px;
    padding: 3px 8px;
    border-radius: 100px;
    background: rgba(29,185,84,0.12);
    color: #1DB954;
    font-weight: 500;
    flex-shrink: 0;
  }

  .spotify-open-btn {
    width: 100%;
    padding: 14px;
    background: #1DB954;
    color: white;
    border: none;
    border-radius: 14px;
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    font-weight: 600;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    letter-spacing: 0.02em;
    transition: all 0.2s;
  }

  .spotify-open-btn:hover {
    background: #17a349;
    transform: translateY(-1px);
    box-shadow: 0 6px 20px rgba(29,185,84,0.3);
  }

  .time-vibe-banner {
    background: var(--warm-white);
    border-radius: 16px;
    padding: 16px 20px;
    box-shadow: var(--card-shadow);
    display: flex;
    align-items: center;
    gap: 14px;
  }

  .time-vibe-icon { font-size: 32px; flex-shrink: 0; }

  .time-vibe-text { flex: 1; }
  .time-vibe-title { font-size: 14px; font-weight: 500; color: var(--charcoal); margin-bottom: 2px; }
  .time-vibe-sub { font-size: 12px; color: var(--muted); font-weight: 300; }

  /* ── SCRAPBOOK TAB ── */
  .scrapbook-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
    padding: 4px 4px 0;
  }

  .scrapbook-title {
    font-family: 'Cormorant Garamond', serif;
    font-size: 22px;
    font-weight: 400;
    color: var(--charcoal);
  }

  .scrapbook-count {
    font-size: 12px;
    color: var(--muted);
    font-weight: 300;
  }

  /* Camera capture card */
  .camera-card {
    background: var(--warm-white);
    border-radius: 20px;
    padding: 22px 24px;
    box-shadow: var(--card-shadow);
    display: flex;
    flex-direction: column;
    gap: 14px;
  }

  .camera-preview {
    width: 100%;
    aspect-ratio: 1;
    border-radius: 14px;
    background: var(--cream);
    border: 2px dashed rgba(196,112,74,0.25);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 10px;
    cursor: pointer;
    transition: all 0.2s;
    overflow: hidden;
    position: relative;
  }

  .camera-preview:hover {
    border-color: var(--terracotta-light);
    background: var(--terracotta-pale);
  }

  .camera-preview img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    border-radius: 12px;
  }

  .camera-preview-icon {
    font-size: 36px;
    opacity: 0.5;
  }

  .camera-preview-text {
    font-size: 13px;
    color: var(--muted);
    font-weight: 300;
  }

  #camera-input { display: none; }

  .camera-stats-row {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 10px;
  }

  .camera-stat {
    background: var(--cream);
    border-radius: 12px;
    padding: 10px 10px;
    text-align: center;
  }

  .camera-stat-label {
    font-size: 9px;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 0.08em;
    margin-bottom: 4px;
  }

  .camera-stat-input {
    width: 100%;
    background: none;
    border: none;
    font-family: 'Cormorant Garamond', serif;
    font-size: 20px;
    font-weight: 400;
    color: var(--charcoal);
    text-align: center;
    outline: none;
  }

  .camera-stat-input::placeholder {
    color: rgba(154,136,128,0.4);
    font-size: 18px;
  }

  .mood-row {
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
  }

  .mood-btn {
    padding: 6px 12px;
    border-radius: 100px;
    border: 1.5px solid rgba(61,53,48,0.1);
    background: none;
    font-size: 12px;
    color: var(--muted);
    cursor: pointer;
    transition: all 0.2s;
    font-family: 'DM Sans', sans-serif;
  }

  .mood-btn.selected {
    background: var(--terracotta-pale);
    border-color: var(--terracotta-light);
    color: var(--terracotta);
    font-weight: 500;
  }

  .capture-btn {
    width: 100%;
    padding: 14px;
    background: linear-gradient(135deg, var(--terracotta), #A85C38);
    color: white;
    border: none;
    border-radius: 14px;
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    font-weight: 500;
    cursor: pointer;
    letter-spacing: 0.03em;
    transition: all 0.2s ease;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
  }

  .capture-btn:hover {
    transform: translateY(-1px);
    box-shadow: 0 6px 20px rgba(196,112,74,0.35);
  }

  /* Polaroid wall */
  .polaroid-wall {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }

  .polaroid-month-group {
    display: flex;
    flex-direction: column;
    gap: 14px;
  }

  .polaroid-month-label {
    font-family: 'Cormorant Garamond', serif;
    font-size: 18px;
    font-weight: 400;
    color: var(--charcoal);
    font-style: italic;
    padding: 0 4px;
  }

  .polaroid-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 14px;
  }

  /* THE POLAROID */
  .polaroid {
    background: white;
    border-radius: 4px;
    padding: 10px 10px 14px;
    box-shadow: 0 4px 20px rgba(61,53,48,0.12), 0 1px 4px rgba(61,53,48,0.08);
    display: flex;
    flex-direction: column;
    gap: 0;
    transform: rotate(var(--tilt, 0deg));
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    cursor: pointer;
    position: relative;
  }

  .polaroid:hover {
    transform: rotate(0deg) scale(1.03);
    box-shadow: 0 12px 40px rgba(61,53,48,0.2);
    z-index: 10;
  }

  .polaroid-photo {
    width: 100%;
    aspect-ratio: 1;
    border-radius: 2px;
    overflow: hidden;
    background: var(--cream);
    position: relative;
  }

  .polaroid-photo img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
  }

  .polaroid-photo-placeholder {
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 32px;
    background: linear-gradient(145deg, var(--cream), var(--terracotta-pale));
  }

  /* Stats overlay on photo */
  .polaroid-overlay {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    background: linear-gradient(transparent, rgba(30,20,15,0.75));
    padding: 16px 8px 6px;
    display: flex;
    justify-content: space-around;
  }

  .polaroid-stat {
    text-align: center;
    color: white;
  }

  .polaroid-stat-val {
    font-family: 'Cormorant Garamond', serif;
    font-size: 16px;
    font-weight: 400;
    line-height: 1;
    color: white;
  }

  .polaroid-stat-lbl {
    font-size: 7px;
    color: rgba(255,255,255,0.7);
    text-transform: uppercase;
    letter-spacing: 0.06em;
    margin-top: 1px;
  }

  /* Bottom of polaroid */
  .polaroid-bottom {
    padding: 8px 2px 0;
    display: flex;
    flex-direction: column;
    gap: 2px;
  }

  .polaroid-date {
    font-family: 'Cormorant Garamond', serif;
    font-size: 13px;
    font-weight: 400;
    color: var(--charcoal);
    font-style: italic;
  }

  .polaroid-mood-tag {
    font-size: 10px;
    color: var(--muted);
    font-weight: 300;
  }

  /* Empty state */
  .empty-state {
    text-align: center;
    padding: 40px 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 10px;
  }

  .empty-state-icon {
    font-size: 48px;
    opacity: 0.4;
  }

  .empty-state-text {
    font-family: 'Cormorant Garamond', serif;
    font-size: 20px;
    font-weight: 300;
    color: var(--muted);
    font-style: italic;
  }

  .empty-state-sub {
    font-size: 13px;
    color: var(--muted);
    font-weight: 300;
  }

  /* Lightbox */
  .lightbox {
    position: fixed;
    inset: 0;
    background: rgba(30,20,15,0.85);
    z-index: 500;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 24px;
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.3s ease;
    backdrop-filter: blur(8px);
  }

  .lightbox.open {
    opacity: 1;
    pointer-events: all;
  }

  .lightbox-polaroid {
    background: white;
    border-radius: 4px;
    padding: 14px 14px 20px;
    max-width: 300px;
    width: 100%;
    box-shadow: 0 30px 80px rgba(0,0,0,0.4);
    transform: scale(0.9) rotate(-1deg);
    transition: transform 0.3s ease;
  }

  .lightbox.open .lightbox-polaroid {
    transform: scale(1) rotate(0deg);
  }

  .lightbox-photo {
    width: 100%;
    aspect-ratio: 1;
    border-radius: 2px;
    overflow: hidden;
    background: var(--cream);
    position: relative;
    margin-bottom: 12px;
  }

  .lightbox-photo img {
    width: 100%; height: 100%; object-fit: cover;
  }

  .lightbox-photo-placeholder {
    width: 100%; height: 100%;
    display: flex; align-items: center; justify-content: center;
    font-size: 60px;
    background: linear-gradient(145deg, var(--cream), var(--terracotta-pale));
  }

  .lightbox-stats {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 8px;
    margin-bottom: 12px;
  }

  .lightbox-stat {
    text-align: center;
    background: var(--cream);
    border-radius: 8px;
    padding: 8px 4px;
  }

  .lightbox-stat-val {
    font-family: 'Cormorant Garamond', serif;
    font-size: 22px;
    font-weight: 400;
    color: var(--charcoal);
    line-height: 1;
  }

  .lightbox-stat-lbl {
    font-size: 9px;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 0.07em;
    margin-top: 2px;
  }

  .lightbox-meta {
    text-align: center;
  }

  .lightbox-date {
    font-family: 'Cormorant Garamond', serif;
    font-size: 18px;
    font-weight: 300;
    color: var(--charcoal);
    font-style: italic;
  }

  .lightbox-mood {
    font-size: 12px;
    color: var(--muted);
    margin-top: 2px;
  }

  .lightbox-close {
    position: absolute;
    top: 20px;
    right: 20px;
    width: 36px;
    height: 36px;
    border-radius: 50%;
    background: rgba(255,255,255,0.15);
    border: none;
    color: white;
    font-size: 18px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: background 0.2s;
  }

  .lightbox-close:hover { background: rgba(255,255,255,0.25); }

  /* ── DR VISIT TAB ── */
  .dr-hero {
    background: linear-gradient(145deg, #3D3530 0%, #6B4F3A 100%);
    border-radius: 24px;
    padding: 26px 24px;
    box-shadow: 0 8px 32px rgba(61,53,48,0.25);
    position: relative;
    overflow: hidden;
  }

  .dr-hero::before {
    content: '';
    position: absolute;
    top: -40px; right: -40px;
    width: 180px; height: 180px;
    background: radial-gradient(circle, rgba(245,227,215,0.15) 0%, transparent 70%);
  }

  .dr-hero-label {
    font-size: 11px; font-weight: 500; letter-spacing: 0.12em;
    text-transform: uppercase; color: rgba(255,255,255,0.45);
    margin-bottom: 8px; position: relative; z-index: 1;
  }

  .dr-hero-title {
    font-family: 'Cormorant Garamond', serif;
    font-size: 28px; font-weight: 300; color: white;
    line-height: 1.2; margin-bottom: 4px;
    position: relative; z-index: 1;
  }

  .dr-hero-title span { color: var(--terracotta-light); font-style: italic; }

  .dr-hero-sub {
    font-size: 12px; color: rgba(255,255,255,0.4); font-weight: 300;
    position: relative; z-index: 1; margin-bottom: 18px;
  }

  .dr-next-appt {
    background: rgba(255,255,255,0.1);
    border: 1px solid rgba(255,255,255,0.15);
    border-radius: 14px;
    padding: 14px 16px;
    display: flex; align-items: center; gap: 12px;
    position: relative; z-index: 1;
  }

  .dr-appt-icon { font-size: 24px; flex-shrink: 0; }

  .dr-appt-info { flex: 1; }
  .dr-appt-label { font-size: 10px; color: rgba(255,255,255,0.4); text-transform: uppercase; letter-spacing: 0.08em; margin-bottom: 2px; }
  .dr-appt-date { font-size: 14px; font-weight: 500; color: white; }
  .dr-appt-edit { font-size: 20px; color: rgba(255,255,255,0.3); cursor: pointer; }

  /* Sub-tab nav inside Dr Visit */
  .dr-subtabs {
    display: flex;
    background: var(--warm-white);
    border-radius: 16px;
    padding: 4px;
    box-shadow: var(--card-shadow);
    gap: 2px;
  }

  .dr-subtab {
    flex: 1; padding: 9px 4px;
    border: none; background: none;
    font-family: 'DM Sans', sans-serif;
    font-size: 11px; font-weight: 400;
    color: var(--muted); cursor: pointer;
    border-radius: 12px; transition: all 0.2s;
    letter-spacing: 0.02em;
  }

  .dr-subtab.active {
    background: var(--charcoal); color: white; font-weight: 500;
  }

  .dr-panel { display: none; flex-direction: column; gap: 14px; }
  .dr-panel.active { display: flex; animation: fadeUp 0.3s ease both; }

  /* One pager summary */
  .summary-card {
    background: var(--warm-white);
    border-radius: 20px;
    padding: 22px 22px;
    box-shadow: var(--card-shadow);
  }

  .summary-header {
    display: flex; justify-content: space-between; align-items: flex-start;
    margin-bottom: 18px; padding-bottom: 14px;
    border-bottom: 1.5px solid rgba(61,53,48,0.08);
  }

  .summary-title {
    font-family: 'Cormorant Garamond', serif;
    font-size: 20px; font-weight: 400; color: var(--charcoal);
  }

  .summary-date { font-size: 11px; color: var(--muted); font-weight: 300; margin-top: 2px; }

  .summary-print-btn {
    padding: 7px 14px;
    background: var(--terracotta-pale);
    color: var(--terracotta);
    border: none; border-radius: 100px;
    font-size: 11px; font-weight: 500;
    cursor: pointer; font-family: 'DM Sans', sans-serif;
    transition: all 0.2s; flex-shrink: 0;
  }

  .summary-print-btn:hover { background: var(--terracotta); color: white; }

  .summary-section { margin-bottom: 16px; }
  .summary-section:last-child { margin-bottom: 0; }

  .summary-section-title {
    font-size: 10px; font-weight: 600; letter-spacing: 0.1em;
    text-transform: uppercase; color: var(--terracotta);
    margin-bottom: 8px; display: flex; align-items: center; gap: 6px;
  }

  .summary-section-title::after {
    content: ''; flex: 1; height: 1px;
    background: rgba(196,112,74,0.2);
  }

  .summary-stat-grid {
    display: grid; grid-template-columns: 1fr 1fr;
    gap: 8px;
  }

  .summary-stat-item {
    background: var(--cream); border-radius: 10px;
    padding: 10px 12px;
  }

  .summary-stat-item-label { font-size: 10px; color: var(--muted); margin-bottom: 3px; text-transform: uppercase; letter-spacing: 0.06em; }
  .summary-stat-item-val { font-family: 'Cormorant Garamond', serif; font-size: 22px; font-weight: 400; color: var(--charcoal); }

  .summary-text-block {
    font-size: 13px; color: var(--charcoal); line-height: 1.6;
    font-weight: 300; background: var(--cream); border-radius: 10px; padding: 12px 14px;
  }

  .summary-tag-row { display: flex; flex-wrap: wrap; gap: 6px; }

  .summary-tag {
    padding: 4px 10px; border-radius: 100px;
    font-size: 11px; font-weight: 400;
    background: var(--terracotta-pale); color: var(--terracotta);
  }

  .summary-question-item {
    display: flex; gap: 10px; align-items: flex-start;
    padding: 8px 0; border-bottom: 1px solid rgba(61,53,48,0.06);
    font-size: 13px; color: var(--charcoal); font-weight: 300; line-height: 1.5;
  }

  .summary-question-item:last-child { border-bottom: none; }
  .summary-q-num { color: var(--terracotta); font-weight: 600; flex-shrink: 0; font-size: 12px; margin-top: 1px; }

  /* Photo upload strip */
  .photo-strip { display: flex; gap: 10px; overflow-x: auto; padding-bottom: 4px; scrollbar-width: none; }
  .photo-strip::-webkit-scrollbar { display: none; }

  .photo-thumb {
    width: 80px; height: 80px; border-radius: 12px;
    flex-shrink: 0; object-fit: cover; cursor: pointer;
    transition: transform 0.2s;
  }

  .photo-thumb:hover { transform: scale(1.05); }

  .photo-add-btn {
    width: 80px; height: 80px; border-radius: 12px;
    flex-shrink: 0; border: 2px dashed rgba(196,112,74,0.3);
    background: var(--terracotta-pale);
    display: flex; flex-direction: column; align-items: center; justify-content: center;
    gap: 4px; cursor: pointer; font-size: 22px; color: var(--terracotta);
    font-family: 'DM Sans', sans-serif; font-size: 20px;
    transition: all 0.2s;
  }

  .photo-add-btn:hover { border-color: var(--terracotta); background: var(--terracotta-pale); }
  .photo-add-label { font-size: 9px; color: var(--terracotta); text-transform: uppercase; letter-spacing: 0.06em; font-weight: 500; }

  /* Post-visit card */
  .post-visit-card {
    background: var(--warm-white); border-radius: 20px;
    padding: 22px; box-shadow: var(--card-shadow);
    display: flex; flex-direction: column; gap: 14px;
  }

  .post-visit-title {
    font-family: 'Cormorant Garamond', serif;
    font-size: 20px; font-weight: 400; color: var(--charcoal);
  }

  .visit-date-badge {
    display: inline-flex; align-items: center; gap: 6px;
    padding: 5px 12px; border-radius: 100px;
    background: var(--sage-pale); color: var(--sage);
    font-size: 11px; font-weight: 500;
  }

  .med-item {
    display: flex; gap: 12px; align-items: center;
    padding: 10px 0; border-bottom: 1px solid rgba(61,53,48,0.06);
  }

  .med-item:last-child { border-bottom: none; }

  .med-icon {
    width: 36px; height: 36px; border-radius: 10px;
    background: var(--sage-pale);
    display: flex; align-items: center; justify-content: center;
    font-size: 16px; flex-shrink: 0;
  }

  .med-info { flex: 1; }
  .med-name { font-size: 13px; font-weight: 500; color: var(--charcoal); }
  .med-detail { font-size: 11px; color: var(--muted); font-weight: 300; margin-top: 1px; }

  .med-delete {
    font-size: 16px; color: var(--muted); background: none;
    border: none; cursor: pointer; padding: 4px;
    transition: color 0.2s;
  }

  .med-delete:hover { color: var(--out-range); }

  .add-med-form {
    background: var(--cream); border-radius: 14px; padding: 14px;
    display: flex; flex-direction: column; gap: 10px;
  }

  /* Notes / Questions for next visit */
  .notes-card {
    background: var(--warm-white); border-radius: 20px;
    padding: 22px; box-shadow: var(--card-shadow);
    display: flex; flex-direction: column; gap: 14px;
  }

  .note-item {
    display: flex; gap: 10px; align-items: flex-start;
    padding: 10px 12px; background: var(--cream); border-radius: 12px;
    animation: fadeUp 0.3s ease both;
  }

  .note-item-dot {
    width: 6px; height: 6px; border-radius: 50%;
    background: var(--terracotta); flex-shrink: 0; margin-top: 5px;
  }

  .note-item-text { flex: 1; font-size: 13px; color: var(--charcoal); font-weight: 300; line-height: 1.5; }

  .note-item-del {
    background: none; border: none; font-size: 14px;
    color: var(--muted); cursor: pointer; flex-shrink: 0;
    transition: color 0.2s; padding: 2px;
  }

  .note-item-del:hover { color: var(--out-range); }

  .note-add-row {
    display: flex; gap: 10px;
  }

  .note-input {
    flex: 1; padding: 11px 14px;
    border: 1.5px solid rgba(61,53,48,0.1);
    border-radius: 12px; font-family: 'DM Sans', sans-serif;
    font-size: 13px; color: var(--charcoal); background: var(--cream);
    outline: none; transition: border-color 0.2s;
  }

  .note-input:focus { border-color: var(--terracotta-light); }

  .note-add-btn {
    width: 42px; height: 42px; border-radius: 12px;
    background: var(--terracotta); color: white;
    border: none; font-size: 20px; cursor: pointer;
    display: flex; align-items: center; justify-content: center;
    flex-shrink: 0; transition: all 0.2s;
  }

  .note-add-btn:hover { background: var(--brown); }

  /* ── EMERGENCY CARD ── */
  .emergency-screen {
    position: fixed;
    inset: 0;
    z-index: 1000;
    background: #1A0A0A;
    display: none;
    flex-direction: column;
    overflow-y: auto;
    animation: fadeUp 0.4s ease both;
  }

  .emergency-screen.open { display: flex; }

  /* Pulsing red banner at top */
  .emergency-banner {
    background: #CC2200;
    padding: 18px 24px 16px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-shrink: 0;
    animation: pulseBg 1.8s ease-in-out infinite;
  }

  @keyframes pulseBg {
    0%, 100% { background: #CC2200; }
    50% { background: #FF2200; }
  }

  .emergency-banner-left {
    display: flex; align-items: center; gap: 12px;
  }

  .emergency-icon { font-size: 28px; }

  .emergency-banner-text { }
  .emergency-banner-title {
    font-family: 'DM Sans', sans-serif;
    font-size: 18px; font-weight: 700;
    color: white; letter-spacing: 0.04em;
  }

  .emergency-banner-sub {
    font-size: 11px; color: rgba(255,255,255,0.75);
    font-weight: 400; margin-top: 1px;
  }

  .emergency-close-btn {
    background: rgba(255,255,255,0.15);
    border: 1.5px solid rgba(255,255,255,0.3);
    color: white; border-radius: 100px;
    padding: 7px 14px; font-size: 12px; font-weight: 600;
    cursor: pointer; font-family: 'DM Sans', sans-serif;
    transition: background 0.2s; letter-spacing: 0.04em;
  }

  .emergency-close-btn:hover { background: rgba(255,255,255,0.25); }

  .emergency-body {
    flex: 1;
    padding: 20px 20px 40px;
    display: flex;
    flex-direction: column;
    gap: 14px;
  }

  /* Profile block */
  .emergency-profile {
    background: #2A1010;
    border: 1.5px solid rgba(204,34,0,0.3);
    border-radius: 20px;
    padding: 22px;
    display: flex;
    gap: 18px;
    align-items: center;
  }

  .emergency-avatar {
    width: 80px; height: 80px;
    border-radius: 50%;
    border: 3px solid #CC2200;
    object-fit: cover;
    flex-shrink: 0;
    background: #3D1515;
    display: flex; align-items: center; justify-content: center;
    font-size: 36px;
    cursor: pointer;
    overflow: hidden;
  }

  .emergency-avatar img { width: 100%; height: 100%; object-fit: cover; border-radius: 50%; }

  .emergency-name-block { flex: 1; }

  .emergency-name {
    font-family: 'Cormorant Garamond', serif;
    font-size: 28px; font-weight: 400; color: white;
    line-height: 1.1; margin-bottom: 4px;
  }

  .emergency-dob {
    font-size: 12px; color: rgba(255,255,255,0.5);
    font-weight: 300; margin-bottom: 8px;
  }

  .emergency-blood-type {
    display: inline-flex; align-items: center; gap: 6px;
    background: #CC2200; color: white;
    padding: 4px 12px; border-radius: 100px;
    font-size: 13px; font-weight: 700;
    letter-spacing: 0.06em;
  }

  /* Critical info card */
  .emergency-critical {
    background: #2A1010;
    border: 2px solid #CC2200;
    border-radius: 20px;
    padding: 20px;
  }

  .emergency-section-label {
    font-size: 10px; font-weight: 700;
    letter-spacing: 0.14em; text-transform: uppercase;
    color: #FF6B55; margin-bottom: 14px;
    display: flex; align-items: center; gap: 8px;
  }

  .emergency-section-label::after {
    content: ''; flex: 1; height: 1px;
    background: rgba(204,34,0,0.3);
  }

  .emergency-diab-row {
    display: grid; grid-template-columns: 1fr 1fr;
    gap: 10px; margin-bottom: 12px;
  }

  .emergency-diab-item {
    background: rgba(204,34,0,0.12);
    border: 1px solid rgba(204,34,0,0.2);
    border-radius: 12px; padding: 12px 14px;
  }

  .emergency-diab-label {
    font-size: 9px; font-weight: 600; text-transform: uppercase;
    letter-spacing: 0.1em; color: rgba(255,255,255,0.4);
    margin-bottom: 4px;
  }

  .emergency-diab-val {
    font-size: 14px; font-weight: 600; color: white;
    line-height: 1.2;
  }

  /* Live glucose block */
  .emergency-glucose-block {
    background: #1A0A0A;
    border: 1.5px solid rgba(255,255,255,0.1);
    border-radius: 14px;
    padding: 16px;
    display: flex;
    align-items: center;
    gap: 14px;
  }

  .emergency-glucose-pulse {
    width: 12px; height: 12px;
    border-radius: 50%;
    flex-shrink: 0;
    animation: pulseGlow 1.4s ease-in-out infinite;
  }

  .emergency-glucose-pulse.in { background: #4CAF50; box-shadow: 0 0 10px #4CAF50; }
  .emergency-glucose-pulse.low { background: #FF6B35; box-shadow: 0 0 10px #FF6B35; }
  .emergency-glucose-pulse.high { background: #FFB830; box-shadow: 0 0 10px #FFB830; }

  @keyframes pulseGlow {
    0%, 100% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.5; transform: scale(1.4); }
  }

  .emergency-glucose-info { flex: 1; }

  .emergency-glucose-label {
    font-size: 10px; color: rgba(255,255,255,0.4);
    text-transform: uppercase; letter-spacing: 0.1em;
    font-weight: 500; margin-bottom: 2px;
  }

  .emergency-glucose-val {
    font-family: 'Cormorant Garamond', serif;
    font-size: 38px; font-weight: 300; color: white;
    line-height: 1;
  }

  .emergency-glucose-status {
    font-size: 13px; font-weight: 600;
    margin-top: 2px;
  }

  .emergency-glucose-status.in { color: #4CAF50; }
  .emergency-glucose-status.low { color: #FF6B35; }
  .emergency-glucose-status.high { color: #FFB830; }

  .emergency-glucose-time {
    font-size: 10px; color: rgba(255,255,255,0.3);
    margin-top: 2px;
  }

  /* Instructions block */
  .emergency-instructions {
    background: rgba(255,107,53,0.1);
    border: 1.5px solid rgba(255,107,53,0.3);
    border-radius: 14px;
    padding: 14px 16px;
  }

  .emergency-instr-title {
    font-size: 12px; font-weight: 700; color: #FF6B35;
    text-transform: uppercase; letter-spacing: 0.08em;
    margin-bottom: 10px;
  }

  .emergency-instr-item {
    font-size: 13px; color: rgba(255,255,255,0.85);
    font-weight: 400; line-height: 1.6;
    padding: 4px 0;
    display: flex; gap: 8px;
  }

  .emergency-instr-num {
    color: #FF6B35; font-weight: 700; flex-shrink: 0;
  }

  /* Emergency contacts */
  .emergency-contacts {
    background: #2A1010;
    border: 1.5px solid rgba(204,34,0,0.2);
    border-radius: 20px;
    padding: 20px;
    display: flex; flex-direction: column; gap: 12px;
  }

  .emergency-contact-item {
    display: flex; align-items: center; gap: 14px;
    padding: 12px 14px;
    background: #1A0A0A;
    border-radius: 14px;
    cursor: pointer;
    transition: background 0.2s;
    text-decoration: none;
  }

  .emergency-contact-item:hover { background: rgba(204,34,0,0.15); }

  .emergency-contact-icon {
    width: 44px; height: 44px; border-radius: 50%;
    background: rgba(204,34,0,0.2);
    display: flex; align-items: center; justify-content: center;
    font-size: 20px; flex-shrink: 0;
  }

  .emergency-contact-info { flex: 1; }

  .emergency-contact-role {
    font-size: 10px; font-weight: 600; color: rgba(255,255,255,0.4);
    text-transform: uppercase; letter-spacing: 0.1em; margin-bottom: 2px;
  }

  .emergency-contact-name {
    font-size: 16px; font-weight: 600; color: white;
  }

  .emergency-contact-number {
    font-size: 13px; color: rgba(255,255,255,0.5); margin-top: 1px;
  }

  .emergency-call-btn {
    background: #CC2200; color: white;
    border: none; border-radius: 100px;
    padding: 8px 16px; font-size: 12px; font-weight: 700;
    cursor: pointer; font-family: 'DM Sans', sans-serif;
    letter-spacing: 0.04em; flex-shrink: 0;
    transition: all 0.2s;
  }

  .emergency-call-btn:hover { background: #FF2200; transform: scale(1.05); }

  /* Allergy / notes block */
  .emergency-notes-block {
    background: #2A1010;
    border: 1.5px solid rgba(204,34,0,0.2);
    border-radius: 20px;
    padding: 20px;
  }

  .emergency-note-text {
    font-size: 13px; color: rgba(255,255,255,0.7);
    line-height: 1.7; font-weight: 300;
  }

  /* Setup modal */
  .emergency-setup {
    position: fixed;
    inset: 0; z-index: 1100;
    background: rgba(0,0,0,0.8);
    display: none;
    align-items: flex-end;
    backdrop-filter: blur(6px);
  }

  .emergency-setup.open { display: flex; }

  .emergency-setup-sheet {
    background: var(--warm-white);
    border-radius: 28px 28px 0 0;
    padding: 28px 24px 48px;
    width: 100%; max-width: 420px; margin: 0 auto;
    display: flex; flex-direction: column; gap: 14px;
    max-height: 85vh; overflow-y: auto;
  }

  .setup-title {
    font-family: 'Cormorant Garamond', serif;
    font-size: 24px; font-weight: 400; color: var(--charcoal);
    margin-bottom: 4px;
  }

  .setup-sub {
    font-size: 13px; color: var(--muted); font-weight: 300;
    margin-top: -8px; margin-bottom: 4px;
  }

  /* SOS floating button on main app */
  .sos-fab {
    position: fixed;
    bottom: 90px; right: 20px;
    width: 52px; height: 52px;
    border-radius: 50%;
    background: #CC2200;
    color: white; border: none;
    font-size: 11px; font-weight: 800;
    cursor: pointer; z-index: 300;
    box-shadow: 0 4px 20px rgba(204,34,0,0.5);
    display: flex; flex-direction: column;
    align-items: center; justify-content: center;
    animation: sosPulse 2.5s ease-in-out infinite;
    font-family: 'DM Sans', sans-serif;
    letter-spacing: 0.06em;
    line-height: 1;
    gap: 1px;
  }

  @keyframes sosPulse {
    0%, 100% { box-shadow: 0 4px 20px rgba(204,34,0,0.5); transform: scale(1); }
    50% { box-shadow: 0 4px 32px rgba(204,34,0,0.8); transform: scale(1.06); }
  }

  .sos-fab-icon { font-size: 16px; }

  /* Toast */
  .toast {
    position: fixed;
    bottom: 100px;
    left: 50%;
    transform: translateX(-50%) translateY(20px);
    background: var(--charcoal);
    color: white;
    padding: 12px 20px;
    border-radius: 100px;
    font-size: 13px;
    font-weight: 400;
    opacity: 0;
    transition: all 0.3s ease;
    z-index: 200;
    white-space: nowrap;
  }

  .toast.show {
    opacity: 1;
    transform: translateX(-50%) translateY(0);
  }
</style>
</head>
<body>

<div class="app">
  <div class="header">
    <div class="header-eyebrow">Good morning</div>
    <div class="header-title">My <span>Body</span></div>
    <div class="header-date" id="current-date"></div>
  </div>

  <div class="nav">
    <button class="nav-tab active" onclick="switchSection('today', this)">Today</button>
    <button class="nav-tab" onclick="switchSection('trends', this)">Trends</button>
    <button class="nav-tab" onclick="switchSection('patterns', this)">Patterns</button>
    <button class="nav-tab" onclick="switchSection('log', this)">Log</button>
    <button class="nav-tab" onclick="switchSection('scrapbook', this)">📷</button>
    <button class="nav-tab" onclick="switchSection('checkin', this)">📍</button>
    <button class="nav-tab" onclick="switchSection('vibes', this)">🎵</button>
    <button class="nav-tab" onclick="switchSection('dr', this)">🩺</button>
  </div>

  <!-- TODAY -->
  <div class="section active" id="section-today">

    <!-- Glucose Hero -->
    <div class="glucose-card card-anim-1">
      <div class="glucose-label">Glucose · Dexcom G7</div>
      <div class="glucose-value-row">
        <div class="glucose-number" id="glucose-display">118</div>
        <div class="glucose-unit">mg/dL</div>
        <div class="glucose-trend">→</div>
      </div>
      <div class="glucose-status in-range">
        <div class="glucose-status-dot"></div>
        In Range
      </div>
      <div class="sparkline-container">
        <div class="sparkline-label">Last 3 hours</div>
        <svg class="sparkline" viewBox="0 0 320 48" preserveAspectRatio="none">
          <defs>
            <linearGradient id="sparkGrad" x1="0" y1="0" x2="0" y2="1">
              <stop offset="0%" stop-color="rgba(232,168,130,0.5)"/>
              <stop offset="100%" stop-color="rgba(232,168,130,0)"/>
            </linearGradient>
          </defs>
          <!-- Range band -->
          <rect x="0" y="12" width="320" height="26" fill="rgba(122,155,126,0.15)" rx="2"/>
          <!-- Glucose path -->
          <path d="M0,36 C30,34 50,28 80,24 C110,20 130,16 160,18 C190,20 220,22 250,16 C280,10 300,12 320,14"
                fill="url(#sparkGrad)" stroke="none"/>
          <path d="M0,36 C30,34 50,28 80,24 C110,20 130,16 160,18 C190,20 220,22 250,16 C280,10 300,12 320,14"
                fill="none" stroke="rgba(232,168,130,0.9)" stroke-width="2" stroke-linecap="round"/>
          <!-- Current dot -->
          <circle cx="320" cy="14" r="4" fill="white"/>
          <circle cx="320" cy="14" r="2" fill="#E8A882"/>
        </svg>
        <div class="range-bar">
          <div class="range-track"><div class="range-fill"></div></div>
          <div class="range-labels">
            <span class="range-label-text">72% in range today</span>
            <span class="range-label-text">Goal: 70%+</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Stats row -->
    <div class="stats-row card-anim-2">
      <div class="stat-card">
        <div class="stat-value" style="color: var(--terracotta)">7.1</div>
        <div class="stat-label">Est. A1C</div>
      </div>
      <div class="stat-card">
        <div class="stat-value">4.2</div>
        <div class="stat-label">Units today</div>
      </div>
      <div class="stat-card">
        <div class="stat-value" style="color: var(--sage)">2</div>
        <div class="stat-label">Lows today</div>
      </div>
    </div>

    <!-- Whoop Recovery -->
    <div class="whoop-card card-anim-3">
      <div class="card-label">
        Recovery · Whoop
        <div class="card-label-dot"></div>
      </div>
      <div class="whoop-metrics">
        <div class="whoop-metric">
          <div class="whoop-metric-label">Sleep</div>
          <div class="whoop-metric-value">6h 42<span>min</span></div>
        </div>
        <div class="whoop-metric">
          <div class="whoop-metric-label">HRV</div>
          <div class="whoop-metric-value">48<span>ms</span></div>
        </div>
      </div>
      <div class="recovery-ring">
        <svg class="ring-svg" width="56" height="56" viewBox="0 0 56 56">
          <circle cx="28" cy="28" r="22" fill="none" stroke="rgba(122,155,126,0.15)" stroke-width="6"/>
          <circle cx="28" cy="28" r="22" fill="none" stroke="#7A9B7E" stroke-width="6"
                  stroke-dasharray="138.2" stroke-dashoffset="41.5"
                  stroke-linecap="round" transform="rotate(-90 28 28)"/>
          <text x="28" y="33" text-anchor="middle" font-family="Cormorant Garamond, serif" font-size="16" fill="#3D3530" font-weight="400">70</text>
        </svg>
        <div class="ring-info">
          <div class="ring-label">Recovery Score</div>
          <div class="ring-value">Moderate</div>
          <div class="ring-desc">Good day for moderate activity</div>
        </div>
      </div>
    </div>

  </div>

  <!-- TRENDS -->
  <div class="section" id="section-trends">
    <div class="section-label">7-Day Trends</div>

    <div class="chart-card card-anim-1">
      <div class="chart-header">
        <div>
          <div class="chart-title">Glucose Range</div>
          <div style="font-size:12px; color:var(--muted); margin-top:2px">mg/dL over time</div>
        </div>
        <div class="chart-period">
          <button class="period-btn active">7D</button>
          <button class="period-btn" onclick="togglePeriod(this)">14D</button>
          <button class="period-btn" onclick="togglePeriod(this)">30D</button>
        </div>
      </div>
      <svg class="chart-area" viewBox="0 0 340 140" preserveAspectRatio="none">
        <defs>
          <linearGradient id="chartGrad" x1="0" y1="0" x2="0" y2="1">
            <stop offset="0%" stop-color="rgba(196,112,74,0.25)"/>
            <stop offset="100%" stop-color="rgba(196,112,74,0)"/>
          </linearGradient>
        </defs>
        <!-- Grid lines -->
        <line x1="0" y1="28" x2="340" y2="28" stroke="rgba(61,53,48,0.06)" stroke-width="1" stroke-dasharray="4,4"/>
        <line x1="0" y1="70" x2="340" y2="70" stroke="rgba(61,53,48,0.06)" stroke-width="1" stroke-dasharray="4,4"/>
        <line x1="0" y1="112" x2="340" y2="112" stroke="rgba(61,53,48,0.06)" stroke-width="1" stroke-dasharray="4,4"/>
        <!-- Labels -->
        <text x="0" y="24" font-family="DM Sans,sans-serif" font-size="9" fill="rgba(154,136,128,0.8)">180</text>
        <text x="0" y="66" font-family="DM Sans,sans-serif" font-size="9" fill="rgba(154,136,128,0.8)">120</text>
        <text x="0" y="108" font-family="DM Sans,sans-serif" font-size="9" fill="rgba(154,136,128,0.8)">70</text>
        <!-- Range band -->
        <rect x="18" y="70" width="322" height="42" fill="rgba(122,155,126,0.1)" rx="4"/>
        <!-- Area fill -->
        <path d="M18,112 L18,80 C60,70 90,50 118,60 C148,70 170,45 200,55 C230,65 260,40 290,48 C315,54 330,60 340,58 L340,112 Z"
              fill="url(#chartGrad)"/>
        <!-- Line -->
        <path d="M18,80 C60,70 90,50 118,60 C148,70 170,45 200,55 C230,65 260,40 290,48 C315,54 330,60 340,58"
              fill="none" stroke="var(--terracotta)" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/>
        <!-- Data points -->
        <circle cx="18" cy="80" r="3.5" fill="white" stroke="var(--terracotta)" stroke-width="2"/>
        <circle cx="68" cy="68" r="3.5" fill="white" stroke="var(--terracotta)" stroke-width="2"/>
        <circle cx="118" cy="60" r="3.5" fill="white" stroke="var(--terracotta)" stroke-width="2"/>
        <circle cx="168" cy="50" r="3.5" fill="white" stroke="var(--terracotta)" stroke-width="2"/>
        <circle cx="218" cy="55" r="3.5" fill="white" stroke="var(--terracotta)" stroke-width="2"/>
        <circle cx="280" cy="44" r="3.5" fill="white" stroke="var(--terracotta)" stroke-width="2"/>
        <circle cx="340" cy="58" r="4.5" fill="var(--terracotta)" stroke="white" stroke-width="2"/>
        <!-- Day labels -->
        <text x="18" y="134" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="9" fill="rgba(154,136,128,0.8)">Mon</text>
        <text x="68" y="134" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="9" fill="rgba(154,136,128,0.8)">Tue</text>
        <text x="118" y="134" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="9" fill="rgba(154,136,128,0.8)">Wed</text>
        <text x="168" y="134" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="9" fill="rgba(154,136,128,0.8)">Thu</text>
        <text x="218" y="134" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="9" fill="rgba(154,136,128,0.8)">Fri</text>
        <text x="280" y="134" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="9" fill="rgba(154,136,128,0.8)">Sat</text>
        <text x="340" y="134" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="9" fill="rgba(154,136,128,0.8)">Sun</text>
      </svg>
    </div>

    <div class="chart-card card-anim-2">
      <div class="chart-header">
        <div>
          <div class="chart-title">Time In Range</div>
          <div style="font-size:12px; color:var(--muted); margin-top:2px">Daily % 70–180 mg/dL</div>
        </div>
      </div>
      <svg class="chart-area" viewBox="0 0 340 120" preserveAspectRatio="xMidYMid meet" style="height:120px">
        <!-- Bars -->
        <rect x="18" y="40" width="32" height="70" rx="8" fill="var(--sage-pale)"/>
        <rect x="18" y="40" width="32" height="56" rx="8" fill="var(--sage-light)" opacity="0.7"/>
        <rect x="68" y="30" width="32" height="80" rx="8" fill="var(--sage-pale)"/>
        <rect x="68" y="30" width="32" height="67" rx="8" fill="var(--sage)" opacity="0.7"/>
        <rect x="118" y="50" width="32" height="60" rx="8" fill="var(--sage-pale)"/>
        <rect x="118" y="50" width="32" height="48" rx="8" fill="var(--sage-light)" opacity="0.7"/>
        <rect x="168" y="25" width="32" height="85" rx="8" fill="var(--sage-pale)"/>
        <rect x="168" y="25" width="32" height="72" rx="8" fill="var(--sage)" opacity="0.8"/>
        <rect x="218" y="35" width="32" height="75" rx="8" fill="var(--sage-pale)"/>
        <rect x="218" y="35" width="32" height="65" rx="8" fill="var(--sage-light)" opacity="0.7"/>
        <rect x="268" y="20" width="32" height="90" rx="8" fill="var(--sage-pale)"/>
        <rect x="268" y="20" width="32" height="80" rx="8" fill="var(--sage)" opacity="0.9"/>
        <rect x="318" y="28" width="22" height="82" rx="8" fill="var(--terracotta-pale)"/>
        <rect x="318" y="28" width="22" height="72" rx="8" fill="var(--terracotta-light)" opacity="0.7"/>
        <!-- Labels -->
        <text x="34" y="115" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="9" fill="rgba(154,136,128,0.8)">Mon</text>
        <text x="84" y="115" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="9" fill="rgba(154,136,128,0.8)">Tue</text>
        <text x="134" y="115" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="9" fill="rgba(154,136,128,0.8)">Wed</text>
        <text x="184" y="115" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="9" fill="rgba(154,136,128,0.8)">Thu</text>
        <text x="234" y="115" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="9" fill="rgba(154,136,128,0.8)">Fri</text>
        <text x="284" y="115" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="9" fill="rgba(154,136,128,0.8)">Sat</text>
        <text x="329" y="115" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="9" fill="rgba(154,136,128,0.8)">Sun</text>
      </svg>
    </div>

    <!-- Avg stats -->
    <div class="stats-row card-anim-3">
      <div class="stat-card">
        <div class="stat-value" style="color:var(--sage)">74%</div>
        <div class="stat-label">Avg TIR</div>
      </div>
      <div class="stat-card">
        <div class="stat-value">126</div>
        <div class="stat-label">Avg glucose</div>
      </div>
      <div class="stat-card">
        <div class="stat-value" style="color:var(--terracotta)">38</div>
        <div class="stat-label">Avg HRV</div>
      </div>
    </div>
  </div>

  <!-- PATTERNS -->
  <div class="section" id="section-patterns">
    <div class="section-label">What I'm Noticing</div>

    <div class="insight-card card-anim-1">
      <div class="insight-item">
        <div class="insight-icon sage">😴</div>
        <div class="insight-text">
          <div class="insight-title">Sleep affects your morning numbers</div>
          <div class="insight-desc">On nights with under 6 hours, your fasting glucose averages 24 mg/dL higher the next morning.</div>
        </div>
      </div>
      <div class="insight-item">
        <div class="insight-icon terra">📈</div>
        <div class="insight-text">
          <div class="insight-title">Tuesday spikes pattern</div>
          <div class="insight-desc">Your glucose tends to run higher mid-week. Could be stress, activity level, or meal timing — worth tracking.</div>
        </div>
      </div>
      <div class="insight-item">
        <div class="insight-icon brown">💪</div>
        <div class="insight-text">
          <div class="insight-title">Recovery improving</div>
          <div class="insight-desc">Your Whoop HRV has trended up +8ms over the last 14 days. Your body is adapting well.</div>
        </div>
      </div>
    </div>

    <div class="chart-card card-anim-2">
      <div class="chart-header">
        <div>
          <div class="chart-title">Sleep vs. Morning Glucose</div>
          <div style="font-size:12px; color:var(--muted); margin-top:2px">Correlation over 7 days</div>
        </div>
      </div>
      <svg class="chart-area" viewBox="0 0 340 140" style="height:130px">
        <!-- Grid -->
        <line x1="30" y1="0" x2="30" y2="110" stroke="rgba(61,53,48,0.08)" stroke-width="1"/>
        <line x1="30" y1="110" x2="340" y2="110" stroke="rgba(61,53,48,0.08)" stroke-width="1"/>
        <!-- Sleep bars (blue = hours slept) -->
        <rect x="40" y="60" width="18" height="50" rx="5" fill="var(--sage-pale)"/>
        <rect x="40" y="75" width="18" height="35" rx="5" fill="var(--sage-light)" opacity="0.6"/>
        <rect x="88" y="50" width="18" height="60" rx="5" fill="var(--sage-pale)"/>
        <rect x="88" y="60" width="18" height="50" rx="5" fill="var(--sage)" opacity="0.5"/>
        <rect x="136" y="70" width="18" height="40" rx="5" fill="var(--sage-pale)"/>
        <rect x="136" y="80" width="18" height="30" rx="5" fill="var(--sage-light)" opacity="0.6"/>
        <rect x="184" y="45" width="18" height="65" rx="5" fill="var(--sage-pale)"/>
        <rect x="184" y="52" width="18" height="58" rx="5" fill="var(--sage)" opacity="0.6"/>
        <rect x="232" y="55" width="18" height="55" rx="5" fill="var(--sage-pale)"/>
        <rect x="232" y="63" width="18" height="47" rx="5" fill="var(--sage-light)" opacity="0.6"/>
        <rect x="280" y="40" width="18" height="70" rx="5" fill="var(--sage-pale)"/>
        <rect x="280" y="46" width="18" height="64" rx="5" fill="var(--sage)" opacity="0.7"/>
        <!-- Glucose dots on top -->
        <circle cx="49" cy="52" r="5" fill="var(--terracotta)" opacity="0.8"/>
        <circle cx="97" cy="40" r="5" fill="var(--terracotta)" opacity="0.8"/>
        <circle cx="145" cy="58" r="5" fill="var(--terracotta)" opacity="0.8"/>
        <circle cx="193" cy="32" r="5" fill="var(--sage)" opacity="0.8"/>
        <circle cx="241" cy="44" r="5" fill="var(--terracotta)" opacity="0.8"/>
        <circle cx="289" cy="28" r="5" fill="var(--sage)" opacity="0.8"/>
        <!-- Day labels -->
        <text x="49" y="124" text-anchor="middle" font-family="DM Sans" font-size="9" fill="rgba(154,136,128,0.8)">M</text>
        <text x="97" y="124" text-anchor="middle" font-family="DM Sans" font-size="9" fill="rgba(154,136,128,0.8)">T</text>
        <text x="145" y="124" text-anchor="middle" font-family="DM Sans" font-size="9" fill="rgba(154,136,128,0.8)">W</text>
        <text x="193" y="124" text-anchor="middle" font-family="DM Sans" font-size="9" fill="rgba(154,136,128,0.8)">T</text>
        <text x="241" y="124" text-anchor="middle" font-family="DM Sans" font-size="9" fill="rgba(154,136,128,0.8)">F</text>
        <text x="289" y="124" text-anchor="middle" font-family="DM Sans" font-size="9" fill="rgba(154,136,128,0.8)">S</text>
        <!-- Legend -->
        <circle cx="30" cy="134" r="4" fill="var(--sage)" opacity="0.6"/>
        <text x="38" y="138" font-family="DM Sans" font-size="9" fill="rgba(154,136,128,0.9)">Sleep hrs</text>
        <circle cx="100" cy="134" r="4" fill="var(--terracotta)" opacity="0.8"/>
        <text x="108" y="138" font-family="DM Sans" font-size="9" fill="rgba(154,136,128,0.9)">AM glucose</text>
      </svg>
    </div>
  </div>

  <!-- LOG -->
  <div class="section" id="section-log">
    <div class="section-label">Log Reading</div>

    <div class="log-form card-anim-1">
      <div class="input-row">
        <div class="input-group">
          <label class="input-label">Glucose (mg/dL)</label>
          <input class="input-field" type="number" id="log-glucose" placeholder="e.g. 112">
        </div>
        <div class="input-group">
          <label class="input-label">Time</label>
          <input class="input-field" type="time" id="log-time">
        </div>
      </div>
      <div class="input-group">
        <label class="input-label">Context</label>
        <select class="input-field" id="log-context">
          <option value="">Select...</option>
          <option>Fasting / woke up</option>
          <option>Before meal</option>
          <option>After meal</option>
          <option>Before exercise</option>
          <option>After exercise</option>
          <option>Bedtime</option>
          <option>Low treatment</option>
        </select>
      </div>
      <div class="input-group">
        <label class="input-label">Note (optional)</label>
        <input class="input-field" type="text" id="log-note" placeholder="e.g. stress, skipped snack...">
      </div>
      <div class="input-row">
        <div class="input-group">
          <label class="input-label">Insulin (units)</label>
          <input class="input-field" type="number" id="log-insulin" placeholder="0">
        </div>
        <div class="input-group">
          <label class="input-label">Carbs (g)</label>
          <input class="input-field" type="number" id="log-carbs" placeholder="0">
        </div>
      </div>
      <button class="log-btn" onclick="logEntry()">Save Entry</button>
    </div>

    <div class="entries-card card-anim-2">
      <div class="card-label">Recent Entries</div>
      <div id="entries-list">
        <div class="entry-item">
          <div class="entry-left">
            <div class="entry-time">7:14 AM · Fasting</div>
            <div class="entry-note">Slept 6h, woke once</div>
          </div>
          <div class="entry-glucose in">118</div>
        </div>
        <div class="entry-item">
          <div class="entry-left">
            <div class="entry-time">11:30 AM · After meal</div>
            <div class="entry-note">Oatmeal, 45g carbs</div>
          </div>
          <div class="entry-glucose out">192</div>
        </div>
        <div class="entry-item">
          <div class="entry-left">
            <div class="entry-time">2:00 PM · Before exercise</div>
            <div class="entry-note">Walk with the girls</div>
          </div>
          <div class="entry-glucose in">134</div>
        </div>
      </div>
    </div>
  </div>

  <!-- DR VISIT -->
  <div class="section" id="section-dr">

    <!-- Hero -->
    <div class="dr-hero card-anim-1">
      <div class="dr-hero-label">🩺 Doctor Visit</div>
      <div class="dr-hero-title">Your health, <span>summarized</span></div>
      <div class="dr-hero-sub">Everything your doctor needs in one place</div>
      <div class="dr-next-appt">
        <div class="dr-appt-icon">📅</div>
        <div class="dr-appt-info">
          <div class="dr-appt-label">Next Appointment</div>
          <div class="dr-appt-date" id="appt-display">Tap to set appointment date</div>
        </div>
        <div class="dr-appt-edit" onclick="setApptDate()">✏️</div>
      </div>
    </div>

    <!-- Sub-tabs -->
    <div class="dr-subtabs card-anim-1">
      <button class="dr-subtab active" onclick="switchDrPanel('summary', this)">One Pager</button>
      <button class="dr-subtab" onclick="switchDrPanel('postvisit', this)">Post Visit</button>
      <button class="dr-subtab" onclick="switchDrPanel('nextvisit', this)">Next Visit</button>
    </div>

    <!-- PANEL: ONE PAGER SUMMARY -->
    <div class="dr-panel active" id="drpanel-summary">

      <div class="summary-card">
        <div class="summary-header">
          <div>
            <div class="summary-title">Patient Summary</div>
            <div class="summary-date" id="summary-date-label"></div>
          </div>
          <button class="summary-print-btn" onclick="printSummary()">🖨️ Print</button>
        </div>

        <!-- Glucose stats -->
        <div class="summary-section">
          <div class="summary-section-title">Glucose Data</div>
          <div class="summary-stat-grid">
            <div class="summary-stat-item">
              <div class="summary-stat-item-label">Avg Glucose</div>
              <div class="summary-stat-item-val">126 <span style="font-size:12px;color:var(--muted)">mg/dL</span></div>
            </div>
            <div class="summary-stat-item">
              <div class="summary-stat-item-label">Time In Range</div>
              <div class="summary-stat-item-val">72<span style="font-size:12px;color:var(--muted)">%</span></div>
            </div>
            <div class="summary-stat-item">
              <div class="summary-stat-item-label">Est. A1C</div>
              <div class="summary-stat-item-val">7.1<span style="font-size:12px;color:var(--muted)">%</span></div>
            </div>
            <div class="summary-stat-item">
              <div class="summary-stat-item-label">Lows (7 days)</div>
              <div class="summary-stat-item-val" style="color:var(--out-range)">4</div>
            </div>
          </div>
        </div>

        <!-- Recovery / Whoop -->
        <div class="summary-section">
          <div class="summary-section-title">Recovery & Sleep</div>
          <div class="summary-stat-grid">
            <div class="summary-stat-item">
              <div class="summary-stat-item-label">Avg Sleep</div>
              <div class="summary-stat-item-val">6h <span style="font-size:12px;color:var(--muted)">42m</span></div>
            </div>
            <div class="summary-stat-item">
              <div class="summary-stat-item-label">Avg HRV</div>
              <div class="summary-stat-item-val">48 <span style="font-size:12px;color:var(--muted)">ms</span></div>
            </div>
          </div>
        </div>

        <!-- Patterns noticed -->
        <div class="summary-section">
          <div class="summary-section-title">Patterns I've Noticed</div>
          <div class="summary-text-block" id="summary-patterns-text">
            Sleep under 6 hours correlates with fasting glucose running ~24 mg/dL higher the next morning. Mid-week glucose tends to spike, possibly stress-related. HRV trending upward +8ms over last 14 days.
          </div>
        </div>

        <!-- Current meds -->
        <div class="summary-section">
          <div class="summary-section-title">Current Medications</div>
          <div class="summary-tag-row" id="summary-meds-tags">
            <div class="summary-tag">Insulin (Omnipod 5)</div>
            <div class="summary-tag">CGM (Dexcom G7)</div>
          </div>
        </div>

        <!-- My questions for doctor -->
        <div class="summary-section" id="summary-questions-section">
          <div class="summary-section-title">My Questions for You</div>
          <div id="summary-questions-list">
            <div style="font-size:13px;color:var(--muted);font-weight:300;font-style:italic;">No questions added yet — go to Next Visit tab to add some.</div>
          </div>
        </div>

        <!-- Visit photos -->
        <div class="summary-section">
          <div class="summary-section-title">Photos to Share</div>
          <div class="photo-strip" id="dr-photo-strip">
            <div class="photo-add-btn" onclick="document.getElementById('dr-photo-input').click()">
              <span>+</span>
              <div class="photo-add-label">Add Photo</div>
            </div>
            <input type="file" id="dr-photo-input" accept="image/*" style="display:none" onchange="addDrPhoto(this)">
            <div class="photo-add-btn" onclick="document.getElementById('dr-camera-input').click()" style="font-size:18px;">
              📷
              <div class="photo-add-label">Camera</div>
            </div>
            <input type="file" id="dr-camera-input" accept="image/*" capture="environment" style="display:none" onchange="addDrPhoto(this)">
          </div>
        </div>
      </div>

    </div>

    <!-- PANEL: POST VISIT -->
    <div class="dr-panel" id="drpanel-postvisit">

      <div class="post-visit-card">
        <div class="post-visit-title">After the Appointment</div>

        <div>
          <div class="input-label" style="font-size:10px;color:var(--muted);text-transform:uppercase;letter-spacing:0.08em;margin-bottom:6px;display:block;">Visit Date</div>
          <input class="input-field" type="date" id="visit-date-input" style="width:100%;padding:11px 14px;border:1.5px solid rgba(61,53,48,0.1);border-radius:12px;font-family:'DM Sans',sans-serif;font-size:13px;color:var(--charcoal);background:var(--cream);outline:none;">
        </div>

        <!-- Dr name -->
        <div class="input-group">
          <label class="input-label" style="font-size:10px;color:var(--muted);text-transform:uppercase;letter-spacing:0.08em;margin-bottom:6px;display:block;">Doctor / Specialist</label>
          <input class="input-field" type="text" id="dr-name" placeholder="e.g. Dr. Reyes — Endocrinologist" style="width:100%;padding:11px 14px;border:1.5px solid rgba(61,53,48,0.1);border-radius:12px;font-family:'DM Sans',sans-serif;font-size:13px;color:var(--charcoal);background:var(--cream);outline:none;">
        </div>

        <!-- New medications -->
        <div>
          <div style="font-size:10px;color:var(--muted);text-transform:uppercase;letter-spacing:0.08em;margin-bottom:10px;font-weight:600;">New / Updated Medications</div>
          <div id="med-list"></div>
          <div class="add-med-form">
            <input class="input-field" type="text" id="med-name-input" placeholder="Medication name" style="padding:10px 12px;border:1.5px solid rgba(61,53,48,0.1);border-radius:10px;font-family:'DM Sans',sans-serif;font-size:13px;color:var(--charcoal);background:white;outline:none;width:100%;">
            <input class="input-field" type="text" id="med-detail-input" placeholder="Dosage / instructions (e.g. 10 units before meals)" style="padding:10px 12px;border:1.5px solid rgba(61,53,48,0.1);border-radius:10px;font-family:'DM Sans',sans-serif;font-size:13px;color:var(--charcoal);background:white;outline:none;width:100%;">
            <button class="log-btn" onclick="addMedication()" style="margin-top:0;padding:11px;">+ Add Medication</button>
          </div>
        </div>

        <!-- Dr recommendations -->
        <div>
          <label class="input-label" style="font-size:10px;color:var(--muted);text-transform:uppercase;letter-spacing:0.08em;margin-bottom:6px;display:block;">Dr Recommendations & Notes</label>
          <textarea id="dr-recs" placeholder="e.g. Increase basal rate overnight, try walking 20 min after dinner, follow up in 3 months..." style="width:100%;padding:12px 14px;border:1.5px solid rgba(61,53,48,0.1);border-radius:12px;font-family:'DM Sans',sans-serif;font-size:13px;color:var(--charcoal);background:var(--cream);outline:none;resize:none;min-height:90px;line-height:1.6;transition:border-color 0.2s;" onfocus="this.style.borderColor='var(--terracotta-light)'" onblur="this.style.borderColor='rgba(61,53,48,0.1)'"></textarea>
        </div>

        <!-- Things to try -->
        <div>
          <label class="input-label" style="font-size:10px;color:var(--muted);text-transform:uppercase;letter-spacing:0.08em;margin-bottom:6px;display:block;">Things to Try / Lifestyle Changes</label>
          <textarea id="dr-try" placeholder="e.g. Low carb breakfast for 2 weeks, track stress levels, aim for 7hr sleep..." style="width:100%;padding:12px 14px;border:1.5px solid rgba(61,53,48,0.1);border-radius:12px;font-family:'DM Sans',sans-serif;font-size:13px;color:var(--charcoal);background:var(--cream);outline:none;resize:none;min-height:80px;line-height:1.6;transition:border-color 0.2s;" onfocus="this.style.borderColor='var(--terracotta-light)'" onblur="this.style.borderColor='rgba(61,53,48,0.1)'"></textarea>
        </div>

        <button class="log-btn" onclick="savePostVisit()">Save Visit Notes</button>
      </div>

      <!-- Saved visit history -->
      <div class="notes-card" id="visit-history-card" style="display:none">
        <div style="font-family:'Cormorant Garamond',serif;font-size:20px;font-weight:400;color:var(--charcoal);">Visit History</div>
        <div id="visit-history-list"></div>
      </div>

    </div>

    <!-- PANEL: NEXT VISIT NOTES -->
    <div class="dr-panel" id="drpanel-nextvisit">

      <div class="notes-card card-anim-1">
        <div style="font-family:'Cormorant Garamond',serif;font-size:22px;font-weight:400;color:var(--charcoal);">Questions for My Doctor</div>
        <div style="font-size:12px;color:var(--muted);font-weight:300;margin-top:-6px;">These will appear on your One Pager summary</div>

        <div id="questions-list"></div>

        <div class="note-add-row">
          <input class="note-input" type="text" id="question-input" placeholder="e.g. Should I adjust my basal rate at night?" onkeydown="if(event.key==='Enter') addQuestion()">
          <button class="note-add-btn" onclick="addQuestion()">+</button>
        </div>
      </div>

      <div class="notes-card card-anim-2">
        <div style="font-family:'Cormorant Garamond',serif;font-size:22px;font-weight:400;color:var(--charcoal);">Things I Want to Mention</div>
        <div style="font-size:12px;color:var(--muted);font-weight:300;margin-top:-6px;">Symptoms, patterns, concerns — don't forget these</div>

        <div id="mentions-list"></div>

        <div class="note-add-row">
          <input class="note-input" type="text" id="mention-input" placeholder="e.g. Feeling more tired than usual after meals" onkeydown="if(event.key==='Enter') addMention()">
          <button class="note-add-btn" onclick="addMention()">+</button>
        </div>
      </div>

    </div>

  </div>

  <!-- CHECK-IN -->
  <div class="section" id="section-checkin">

    <div class="checkin-hero card-anim-1">
      <div class="checkin-hero-label">📍 Check In</div>
      <button class="checkin-locate-btn" onclick="findNearbyPlaces()">
        <div class="checkin-locate-icon">🗺️</div>
        <div class="checkin-locate-text">
          <div class="checkin-locate-title">Find places near me</div>
          <div class="checkin-locate-sub" id="locate-status">Tap to use your location</div>
        </div>
        <div class="checkin-locate-arrow">›</div>
      </button>
    </div>

    <div class="places-card card-anim-2" id="places-section" style="display:none">
      <div class="card-label">Nearby Places</div>
      <div id="places-list"></div>
    </div>

    <!-- Manual place type picker (fallback / always shown) -->
    <div class="places-card card-anim-2" id="manual-places">
      <div class="card-label">Quick Check-In</div>
      <div class="place-item" onclick="checkIn('gym','💪','Gym / Workout','gym')">
        <div class="place-icon gym">💪</div>
        <div class="place-info">
          <div class="place-name">Gym / Workout</div>
          <div class="place-type">Physical activity</div>
        </div>
        <button class="place-checkin-btn">Check In</button>
      </div>
      <div class="place-item" onclick="checkIn('food','🍽️','Restaurant','restaurant')">
        <div class="place-icon food">🍽️</div>
        <div class="place-info">
          <div class="place-name">Restaurant</div>
          <div class="place-type">Dining out</div>
        </div>
        <button class="place-checkin-btn">Check In</button>
      </div>
      <div class="place-item" onclick="checkIn('cafe','☕','Coffee Shop','cafe')">
        <div class="place-icon cafe">☕</div>
        <div class="place-info">
          <div class="place-name">Coffee Shop</div>
          <div class="place-type">Café / Work session</div>
        </div>
        <button class="place-checkin-btn">Check In</button>
      </div>
      <div class="place-item" onclick="checkIn('other','🛍️','Shopping / Errands','other')">
        <div class="place-icon other">🛍️</div>
        <div class="place-info">
          <div class="place-name">Shopping / Errands</div>
          <div class="place-type">Out and about</div>
        </div>
        <button class="place-checkin-btn">Check In</button>
      </div>
      <div class="place-item" onclick="checkIn('other','🏠','Home','home')">
        <div class="place-icon other">🏠</div>
        <div class="place-info">
          <div class="place-name">Home</div>
          <div class="place-type">Rest / Recovery</div>
        </div>
        <button class="place-checkin-btn">Check In</button>
      </div>
    </div>

    <!-- Check-in history -->
    <div class="checkin-history-card card-anim-3">
      <div class="card-label">Today's Activity</div>
      <div id="checkin-history">
        <div style="text-align:center; padding:20px 0; color:var(--muted); font-size:13px; font-weight:300;">No check-ins yet today</div>
      </div>
    </div>

  </div>

  <!-- CHECK-IN CELEBRATION MODAL -->
  <div class="checkin-modal" id="checkin-modal" onclick="closeCheckinModal(event)">
    <div class="checkin-modal-sheet" id="checkin-modal-sheet">
      <!-- filled by JS -->
    </div>
  </div>

  <!-- VIBES / SPOTIFY -->
  <div class="section" id="section-vibes">

    <div class="vibes-hero card-anim-1">
      <div class="vibes-label">🎵 Your Vibes</div>
      <div class="vibes-greeting" id="vibes-greeting">Good morning, <span>Val</span></div>
      <div class="vibes-sub" id="vibes-sub">Here's what's playing for your energy right now</div>
    </div>

    <!-- Time-based suggestion -->
    <div class="time-vibe-banner card-anim-1" id="time-vibe-banner">
      <div class="time-vibe-icon" id="time-vibe-icon">🌅</div>
      <div class="time-vibe-text">
        <div class="time-vibe-title" id="time-vibe-title">Morning Energy</div>
        <div class="time-vibe-sub" id="time-vibe-sub">Upbeat pop to start your day strong</div>
      </div>
    </div>

    <!-- Mood selector -->
    <div class="card-label" style="padding: 0 4px;">How are you feeling right now?</div>
    <div class="mood-vibe-row" id="vibe-chips">
      <div class="vibe-chip active" onclick="selectVibe(this,'energized')">⚡ Energized</div>
      <div class="vibe-chip" onclick="selectVibe(this,'calm')">🌿 Need calm</div>
      <div class="vibe-chip" onclick="selectVibe(this,'focus')">🧠 Focus mode</div>
      <div class="vibe-chip" onclick="selectVibe(this,'happy')">☀️ Happy vibes</div>
      <div class="vibe-chip" onclick="selectVibe(this,'lowsugar')">🍬 Low sugar crash</div>
      <div class="vibe-chip" onclick="selectVibe(this,'postworkout')">💪 Post-workout</div>
      <div class="vibe-chip" onclick="selectVibe(this,'sleepy')">😴 Winding down</div>
    </div>

    <!-- Playlist suggestions -->
    <div class="playlist-card card-anim-2" id="playlist-suggestions">
      <!-- filled by JS -->
    </div>

    <button class="spotify-open-btn" onclick="openSpotify()">
      <svg width="18" height="18" viewBox="0 0 24 24" fill="white"><path d="M12 0C5.4 0 0 5.4 0 12s5.4 12 12 12 12-5.4 12-12S18.66 0 12 0zm5.521 17.34c-.24.359-.66.48-1.021.24-2.82-1.74-6.36-2.101-10.561-1.141-.418.122-.779-.179-.899-.539-.12-.421.18-.78.54-.9 4.56-1.021 8.52-.6 11.64 1.32.42.18.479.659.301 1.02zm1.44-3.3c-.301.42-.841.6-1.262.3-3.239-1.98-8.159-2.58-11.939-1.38-.479.12-1.02-.12-1.14-.6-.12-.48.12-1.021.6-1.141C9.6 9.9 15 10.561 18.72 12.84c.361.181.54.78.241 1.2zm.12-3.36C15.24 8.4 8.82 8.16 5.16 9.301c-.6.179-1.2-.181-1.38-.721-.18-.601.18-1.2.72-1.381 4.26-1.26 11.28-1.02 15.721 1.621.539.3.719 1.02.419 1.56-.299.421-1.02.599-1.559.3z"/></svg>
      Open in Spotify
    </button>

  </div>
  <div class="section" id="section-scrapbook">

    <div class="scrapbook-header">
      <div class="scrapbook-title">Scrapbook</div>
      <div class="scrapbook-count" id="photo-count">0 entries</div>
    </div>

    <!-- Camera / capture card -->
    <div class="camera-card card-anim-1">
      <div class="camera-preview" onclick="document.getElementById('camera-input').click()" id="camera-preview-area">
        <div class="camera-preview-icon">📷</div>
        <div class="camera-preview-text">Tap to take today's selfie</div>
      </div>
      <input type="file" id="camera-input" accept="image/*" capture="user" onchange="previewPhoto(this)">

      <div class="camera-stats-row">
        <div class="camera-stat">
          <div class="camera-stat-label">Glucose</div>
          <input class="camera-stat-input" type="number" id="snap-glucose" placeholder="—">
        </div>
        <div class="camera-stat">
          <div class="camera-stat-label">TIR %</div>
          <input class="camera-stat-input" type="number" id="snap-tir" placeholder="—">
        </div>
        <div class="camera-stat">
          <div class="camera-stat-label">Recovery</div>
          <input class="camera-stat-input" type="number" id="snap-recovery" placeholder="—">
        </div>
      </div>

      <div>
        <div class="camera-stat-label" style="margin-bottom:8px; font-size:10px; color:var(--muted); text-transform:uppercase; letter-spacing:0.08em;">How are you feeling?</div>
        <div class="mood-row" id="mood-row">
          <button class="mood-btn" onclick="selectMood(this)">✨ Thriving</button>
          <button class="mood-btn" onclick="selectMood(this)">😊 Good</button>
          <button class="mood-btn" onclick="selectMood(this)">😐 Okay</button>
          <button class="mood-btn" onclick="selectMood(this)">😮‍💨 Rough day</button>
          <button class="mood-btn" onclick="selectMood(this)">💪 Fought hard</button>
        </div>
      </div>

      <button class="capture-btn" onclick="savePolaroid()">
        <span>📸</span> Save to Scrapbook
      </button>
    </div>

    <!-- Polaroid wall -->
    <div class="polaroid-wall" id="polaroid-wall">
      <div class="empty-state" id="empty-scrapbook">
        <div class="empty-state-icon">🌿</div>
        <div class="empty-state-text">Your story starts today</div>
        <div class="empty-state-sub">Take your first selfie to begin your diabetic scrapbook</div>
      </div>
    </div>

  </div>

</div>

<!-- Lightbox -->
<div class="lightbox" id="lightbox" onclick="closeLightbox(event)">
  <button class="lightbox-close" onclick="document.getElementById('lightbox').classList.remove('open')">✕</button>
  <div class="lightbox-polaroid" id="lightbox-content"></div>
</div>
<div class="bottom-nav">
  <button class="bottom-tab active" onclick="switchBottom('today', this)">
    <div class="bottom-tab-icon">🌿</div>
    <div class="bottom-tab-label">Today</div>
  </button>
  <button class="bottom-tab" onclick="switchBottom('trends', this)">
    <div class="bottom-tab-icon">📊</div>
    <div class="bottom-tab-label">Trends</div>
  </button>
  <button class="bottom-tab" onclick="switchBottom('patterns', this)">
    <div class="bottom-tab-icon">✨</div>
    <div class="bottom-tab-label">Patterns</div>
  </button>
  <button class="bottom-tab" onclick="switchBottom('log', this)">
    <div class="bottom-tab-icon">＋</div>
    <div class="bottom-tab-label">Log</div>
  </button>
  <button class="bottom-tab" onclick="switchBottom('scrapbook', this)">
    <div class="bottom-tab-icon">📷</div>
    <div class="bottom-tab-label">Scrapbook</div>
  </button>
  <button class="bottom-tab" onclick="switchBottom('checkin', this)">
    <div class="bottom-tab-icon">📍</div>
    <div class="bottom-tab-label">Check In</div>
  </button>
  <button class="bottom-tab" onclick="switchBottom('vibes', this)">
    <div class="bottom-tab-icon">🎵</div>
    <div class="bottom-tab-label">Vibes</div>
  </button>
  <button class="bottom-tab" onclick="switchBottom('dr', this)">
    <div class="bottom-tab-icon">🩺</div>
    <div class="bottom-tab-label">Dr Visit</div>
  </button>
</div>

<!-- SOS Floating Button -->
<button class="sos-fab" onclick="openEmergency()">
  <div class="sos-fab-icon">🚨</div>
  SOS
</button>

<!-- EMERGENCY FULL SCREEN CARD -->
<div class="emergency-screen" id="emergency-screen">

  <div class="emergency-banner">
    <div class="emergency-banner-left">
      <div class="emergency-icon">🚨</div>
      <div class="emergency-banner-text">
        <div class="emergency-banner-title">MEDICAL EMERGENCY</div>
        <div class="emergency-banner-sub">Please read carefully — call 911 if unconscious</div>
      </div>
    </div>
    <button class="emergency-close-btn" onclick="closeEmergency()">Close</button>
  </div>

  <div class="emergency-body">

    <!-- Profile -->
    <div class="emergency-profile">
      <div class="emergency-avatar" id="emergency-avatar-display" onclick="document.getElementById('emergency-avatar-input').click()">
        🙍
      </div>
      <input type="file" id="emergency-avatar-input" accept="image/*" capture="user" style="display:none" onchange="setEmergencyAvatar(this)">
      <div class="emergency-name-block">
        <div class="emergency-name" id="e-name">Valerie Ramirez</div>
        <div class="emergency-dob" id="e-dob">DOB: —</div>
        <div class="emergency-blood-type" id="e-blood">🩸 Blood Type: —</div>
      </div>
    </div>

    <!-- Critical diabetes info -->
    <div class="emergency-critical">
      <div class="emergency-section-label">⚠️ Diabetes Information</div>
      <div class="emergency-diab-row">
        <div class="emergency-diab-item">
          <div class="emergency-diab-label">Diabetes Type</div>
          <div class="emergency-diab-val" id="e-type">Type 1</div>
        </div>
        <div class="emergency-diab-item">
          <div class="emergency-diab-label">Insulin Dependent</div>
          <div class="emergency-diab-val" id="e-insulin">Yes</div>
        </div>
        <div class="emergency-diab-item">
          <div class="emergency-diab-label">CGM Device</div>
          <div class="emergency-diab-val" id="e-cgm">Dexcom G7</div>
        </div>
        <div class="emergency-diab-item">
          <div class="emergency-diab-label">Pump / Delivery</div>
          <div class="emergency-diab-val" id="e-pump">Omnipod 5</div>
        </div>
      </div>

      <!-- Live glucose -->
      <div class="emergency-glucose-block">
        <div class="emergency-glucose-pulse in" id="e-glucose-pulse"></div>
        <div class="emergency-glucose-info">
          <div class="emergency-glucose-label">Last Glucose Reading</div>
          <div class="emergency-glucose-val" id="e-glucose-val">118 mg/dL</div>
          <div class="emergency-glucose-status in" id="e-glucose-status">✓ In Range (70–180)</div>
          <div class="emergency-glucose-time" id="e-glucose-time"></div>
        </div>
      </div>

      <!-- First responder instructions -->
      <div class="emergency-instructions" style="margin-top:14px;">
        <div class="emergency-instr-title">🚑 If She Is Unconscious</div>
        <div class="emergency-instr-item"><div class="emergency-instr-num">1.</div><div>Call 911 immediately</div></div>
        <div class="emergency-instr-item"><div class="emergency-instr-num">2.</div><div>Do NOT give food or water if unconscious</div></div>
        <div class="emergency-instr-item"><div class="emergency-instr-num">3.</div><div>If glucagon kit available, administer per kit instructions</div></div>
        <div class="emergency-instr-item"><div class="emergency-instr-num">4.</div><div>Insulin pump/CGM may be on her body — do not remove</div></div>
        <div class="emergency-instr-item"><div class="emergency-instr-num">5.</div><div>Inform EMS she is insulin-dependent T1 diabetic</div></div>
      </div>
    </div>

    <!-- Emergency contacts -->
    <div class="emergency-contacts">
      <div class="emergency-section-label">📞 Emergency Contacts</div>
      <a class="emergency-contact-item" id="e-contact1-link" href="#">
        <div class="emergency-contact-icon">👤</div>
        <div class="emergency-contact-info">
          <div class="emergency-contact-role" id="e-contact1-role">Emergency Contact 1</div>
          <div class="emergency-contact-name" id="e-contact1-name">—</div>
          <div class="emergency-contact-number" id="e-contact1-phone">—</div>
        </div>
        <button class="emergency-call-btn" onclick="callContact(event,'e-contact1-phone')">CALL</button>
      </a>
      <a class="emergency-contact-item" id="e-contact2-link" href="#">
        <div class="emergency-contact-icon">👤</div>
        <div class="emergency-contact-info">
          <div class="emergency-contact-role" id="e-contact2-role">Emergency Contact 2</div>
          <div class="emergency-contact-name" id="e-contact2-name">—</div>
          <div class="emergency-contact-number" id="e-contact2-phone">—</div>
        </div>
        <button class="emergency-call-btn" onclick="callContact(event,'e-contact2-phone')">CALL</button>
      </a>
    </div>

    <!-- Allergies / other notes -->
    <div class="emergency-notes-block">
      <div class="emergency-section-label">💊 Allergies & Important Notes</div>
      <div class="emergency-note-text" id="e-allergies">No known drug allergies listed. Tap edit to add.</div>
    </div>

    <!-- Edit button -->
    <button class="log-btn" style="background:rgba(255,255,255,0.1);border:1.5px solid rgba(255,255,255,0.2);margin-top:4px;" onclick="openEmergencySetup()">
      ✏️ Edit My Emergency Info
    </button>

  </div>
</div>

<!-- EMERGENCY SETUP MODAL -->
<div class="emergency-setup" id="emergency-setup">
  <div class="emergency-setup-sheet">
    <div class="setup-title">Your Emergency Card</div>
    <div class="setup-sub">This info is shown to first responders. Be accurate.</div>

    <div class="input-group">
      <label class="input-label">Full Name</label>
      <input class="input-field" type="text" id="setup-name" placeholder="e.g. Valerie Ramirez">
    </div>
    <div class="input-row">
      <div class="input-group">
        <label class="input-label">Date of Birth</label>
        <input class="input-field" type="date" id="setup-dob">
      </div>
      <div class="input-group">
        <label class="input-label">Blood Type</label>
        <select class="input-field" id="setup-blood">
          <option value="">Unknown</option>
          <option>A+</option><option>A-</option>
          <option>B+</option><option>B-</option>
          <option>AB+</option><option>AB-</option>
          <option>O+</option><option>O-</option>
        </select>
      </div>
    </div>
    <div class="input-row">
      <div class="input-group">
        <label class="input-label">Diabetes Type</label>
        <select class="input-field" id="setup-type">
          <option>Type 1</option>
          <option>Type 2</option>
          <option>LADA</option>
          <option>Gestational</option>
          <option>Other</option>
        </select>
      </div>
      <div class="input-group">
        <label class="input-label">Insulin Dependent?</label>
        <select class="input-field" id="setup-insulin">
          <option>Yes</option>
          <option>No</option>
        </select>
      </div>
    </div>
    <div class="input-row">
      <div class="input-group">
        <label class="input-label">CGM Device</label>
        <input class="input-field" type="text" id="setup-cgm" placeholder="e.g. Dexcom G7">
      </div>
      <div class="input-group">
        <label class="input-label">Insulin Pump</label>
        <input class="input-field" type="text" id="setup-pump" placeholder="e.g. Omnipod 5">
      </div>
    </div>

    <div style="border-top:1.5px solid rgba(61,53,48,0.08);margin:4px 0;padding-top:14px;">
      <div style="font-size:11px;font-weight:600;color:var(--terracotta);letter-spacing:0.08em;text-transform:uppercase;margin-bottom:12px;">Emergency Contact 1</div>
      <div class="input-group">
        <label class="input-label">Name & Relationship</label>
        <input class="input-field" type="text" id="setup-c1-name" placeholder="e.g. Zach (Partner)">
      </div>
      <div class="input-group">
        <label class="input-label">Phone Number</label>
        <input class="input-field" type="tel" id="setup-c1-phone" placeholder="(555) 000-0000">
      </div>
    </div>

    <div style="border-top:1.5px solid rgba(61,53,48,0.08);margin:4px 0;padding-top:14px;">
      <div style="font-size:11px;font-weight:600;color:var(--terracotta);letter-spacing:0.08em;text-transform:uppercase;margin-bottom:12px;">Emergency Contact 2</div>
      <div class="input-group">
        <label class="input-label">Name & Relationship</label>
        <input class="input-field" type="text" id="setup-c2-name" placeholder="e.g. Mom (Maria)">
      </div>
      <div class="input-group">
        <label class="input-label">Phone Number</label>
        <input class="input-field" type="tel" id="setup-c2-phone" placeholder="(555) 000-0000">
      </div>
    </div>

    <div style="border-top:1.5px solid rgba(61,53,48,0.08);margin:4px 0;padding-top:14px;">
      <label class="input-label">Allergies & Important Medical Notes</label>
      <textarea id="setup-allergies" placeholder="e.g. Penicillin allergy. Glucagon kit in purse. No latex." style="width:100%;padding:12px 14px;border:1.5px solid rgba(61,53,48,0.1);border-radius:12px;font-family:'DM Sans',sans-serif;font-size:13px;color:var(--charcoal);background:var(--cream);outline:none;resize:none;min-height:80px;line-height:1.6;"></textarea>
    </div>

    <button class="log-btn" onclick="saveEmergencyInfo()">Save Emergency Card</button>
    <button style="width:100%;padding:12px;background:none;border:none;color:var(--muted);font-family:'DM Sans',sans-serif;font-size:13px;cursor:pointer;" onclick="closeEmergencySetup()">Cancel</button>
  </div>
</div>

<div class="toast" id="toast">Entry saved ✓</div>

<script>
  // Set date
  const days = ['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday'];
  const months = ['January','February','March','April','May','June','July','August','September','October','November','December'];
  const now = new Date();
  document.getElementById('current-date').textContent = `${days[now.getDay()]}, ${months[now.getMonth()]} ${now.getDate()}`;

  // Set current time in log
  const timeInput = document.getElementById('log-time');
  const h = String(now.getHours()).padStart(2,'0');
  const m = String(now.getMinutes()).padStart(2,'0');
  timeInput.value = `${h}:${m}`;

  // Navigation
  function switchSection(id, btn) {
    document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
    document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
    document.getElementById('section-' + id).classList.add('active');
    btn.classList.add('active');
  }

  function switchBottom(id, btn) {
    document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
    document.querySelectorAll('.bottom-tab').forEach(t => t.classList.remove('active'));
    document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
    document.getElementById('section-' + id).classList.add('active');
    btn.classList.add('active');
    const tabs = document.querySelectorAll('.nav-tab');
    const map = {today:0, trends:1, patterns:2, log:3, scrapbook:4, checkin:5, vibes:6, dr:7};
    if (map[id] !== undefined) tabs[map[id]].classList.add('active');
  }

  // ── DR VISIT ──
  let drQuestions   = JSON.parse(localStorage.getItem('drQuestions') || '[]');
  let drMentions    = JSON.parse(localStorage.getItem('drMentions')  || '[]');
  let drMeds        = JSON.parse(localStorage.getItem('drMeds')      || '[]');
  let drVisits      = JSON.parse(localStorage.getItem('drVisits')    || '[]');
  let drPhotos      = JSON.parse(localStorage.getItem('drPhotos')    || '[]');

  // Set summary date
  document.getElementById('summary-date-label').textContent =
    'Prepared ' + new Date().toLocaleDateString('en-US',{month:'long',day:'numeric',year:'numeric'});

  function switchDrPanel(id, btn) {
    document.querySelectorAll('.dr-panel').forEach(p => p.classList.remove('active'));
    document.querySelectorAll('.dr-subtab').forEach(b => b.classList.remove('active'));
    document.getElementById('drpanel-' + id).classList.add('active');
    btn.classList.add('active');
    if (id === 'summary') refreshSummary();
  }

  function setApptDate() {
    const d = prompt('Enter your next appointment date (e.g. June 15, 2025):');
    if (d) {
      document.getElementById('appt-display').textContent = d;
      localStorage.setItem('apptDate', d);
    }
  }

  // Load saved appt
  const savedAppt = localStorage.getItem('apptDate');
  if (savedAppt) document.getElementById('appt-display').textContent = savedAppt;

  // ── QUESTIONS FOR NEXT VISIT ──
  function addQuestion() {
    const inp = document.getElementById('question-input');
    const text = inp.value.trim();
    if (!text) return;
    drQuestions.push({ id: Date.now(), text });
    try { localStorage.setItem('drQuestions', JSON.stringify(drQuestions)); } catch(e) {}
    inp.value = '';
    renderQuestions();
    showToast('Question saved ✓');
  }

  function deleteQuestion(id) {
    drQuestions = drQuestions.filter(q => q.id !== id);
    try { localStorage.setItem('drQuestions', JSON.stringify(drQuestions)); } catch(e) {}
    renderQuestions();
  }

  function renderQuestions() {
    const el = document.getElementById('questions-list');
    if (drQuestions.length === 0) {
      el.innerHTML = '<div style="font-size:13px;color:var(--muted);font-weight:300;font-style:italic;padding:4px 0;">No questions yet — add your first one below</div>';
    } else {
      el.innerHTML = drQuestions.map(q => `
        <div class="note-item">
          <div class="note-item-dot"></div>
          <div class="note-item-text">${q.text}</div>
          <button class="note-item-del" onclick="deleteQuestion(${q.id})">✕</button>
        </div>`).join('');
    }
    refreshSummaryQuestions();
  }

  function addMention() {
    const inp = document.getElementById('mention-input');
    const text = inp.value.trim();
    if (!text) return;
    drMentions.push({ id: Date.now(), text });
    try { localStorage.setItem('drMentions', JSON.stringify(drMentions)); } catch(e) {}
    inp.value = '';
    renderMentions();
    showToast('Note saved ✓');
  }

  function deleteMention(id) {
    drMentions = drMentions.filter(m => m.id !== id);
    try { localStorage.setItem('drMentions', JSON.stringify(drMentions)); } catch(e) {}
    renderMentions();
  }

  function renderMentions() {
    const el = document.getElementById('mentions-list');
    if (drMentions.length === 0) {
      el.innerHTML = '<div style="font-size:13px;color:var(--muted);font-weight:300;font-style:italic;padding:4px 0;">Nothing added yet</div>';
    } else {
      el.innerHTML = drMentions.map(m => `
        <div class="note-item">
          <div class="note-item-dot" style="background:var(--sage)"></div>
          <div class="note-item-text">${m.text}</div>
          <button class="note-item-del" onclick="deleteMention(${m.id})">✕</button>
        </div>`).join('');
    }
  }

  // ── POST VISIT / MEDICATIONS ──
  function addMedication() {
    const name   = document.getElementById('med-name-input').value.trim();
    const detail = document.getElementById('med-detail-input').value.trim();
    if (!name) return;
    drMeds.push({ id: Date.now(), name, detail });
    try { localStorage.setItem('drMeds', JSON.stringify(drMeds)); } catch(e) {}
    document.getElementById('med-name-input').value = '';
    document.getElementById('med-detail-input').value = '';
    renderMeds();
    showToast('Medication added ✓');
  }

  function deleteMed(id) {
    drMeds = drMeds.filter(m => m.id !== id);
    try { localStorage.setItem('drMeds', JSON.stringify(drMeds)); } catch(e) {}
    renderMeds();
  }

  function renderMeds() {
    const el = document.getElementById('med-list');
    if (drMeds.length === 0) { el.innerHTML = ''; return; }
    el.innerHTML = drMeds.map(m => `
      <div class="med-item">
        <div class="med-icon">💊</div>
        <div class="med-info">
          <div class="med-name">${m.name}</div>
          <div class="med-detail">${m.detail || 'No details added'}</div>
        </div>
        <button class="med-delete" onclick="deleteMed(${m.id})">✕</button>
      </div>`).join('');
    // Also refresh summary meds
    const tags = document.getElementById('summary-meds-tags');
    const existing = ['Insulin (Omnipod 5)', 'CGM (Dexcom G7)'];
    const all = [...existing, ...drMeds.map(m => m.name)];
    tags.innerHTML = all.map(t => `<div class="summary-tag">${t}</div>`).join('');
  }

  function savePostVisit() {
    const date  = document.getElementById('visit-date-input').value;
    const dr    = document.getElementById('dr-name').value.trim();
    const recs  = document.getElementById('dr-recs').value.trim();
    const tries = document.getElementById('dr-try').value.trim();
    if (!date && !dr && !recs && !tries) { showToast('Add some visit details first'); return; }
    drVisits.unshift({ id: Date.now(), date, dr, recs, tries, meds: [...drMeds] });
    try { localStorage.setItem('drVisits', JSON.stringify(drVisits.slice(0,20))); } catch(e) {}
    renderVisitHistory();
    showToast('Visit saved ✓');
  }

  function renderVisitHistory() {
    const card = document.getElementById('visit-history-card');
    const list = document.getElementById('visit-history-list');
    if (drVisits.length === 0) { card.style.display = 'none'; return; }
    card.style.display = 'flex';
    list.innerHTML = drVisits.map(v => `
      <div style="padding:14px 0;border-bottom:1px solid rgba(61,53,48,0.06);">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:6px;">
          <div style="font-size:13px;font-weight:500;color:var(--charcoal)">${v.dr || 'Doctor visit'}</div>
          <div class="visit-date-badge">📅 ${v.date || 'No date'}</div>
        </div>
        ${v.recs ? `<div style="font-size:12px;color:var(--muted);font-weight:300;margin-top:4px;line-height:1.5"><b style="color:var(--charcoal)">Recommendations:</b> ${v.recs}</div>` : ''}
        ${v.tries ? `<div style="font-size:12px;color:var(--muted);font-weight:300;margin-top:4px;line-height:1.5"><b style="color:var(--charcoal)">Things to try:</b> ${v.tries}</div>` : ''}
      </div>`).join('');
  }

  // ── DR PHOTOS ──
  function addDrPhoto(input) {
    if (!input.files || !input.files[0]) return;
    const reader = new FileReader();
    reader.onload = (e) => {
      drPhotos.push(e.target.result);
      try { localStorage.setItem('drPhotos', JSON.stringify(drPhotos.slice(0,10))); } catch(err) {}
      renderDrPhotos();
      showToast('Photo added ✓');
    };
    reader.readAsDataURL(input.files[0]);
    input.value = '';
  }

  function renderDrPhotos() {
    const strip = document.getElementById('dr-photo-strip');
    // Keep the two add buttons, prepend photos
    const addBtns = strip.querySelectorAll('.photo-add-btn');
    strip.innerHTML = '';
    addBtns.forEach(b => strip.appendChild(b));
    // Re-attach inputs
    strip.innerHTML = `
      <div class="photo-add-btn" onclick="document.getElementById('dr-photo-input').click()">
        <span>+</span><div class="photo-add-label">Add Photo</div>
      </div>
      <input type="file" id="dr-photo-input" accept="image/*" style="display:none" onchange="addDrPhoto(this)">
      <div class="photo-add-btn" onclick="document.getElementById('dr-camera-input').click()" style="font-size:18px;">
        📷<div class="photo-add-label">Camera</div>
      </div>
      <input type="file" id="dr-camera-input" accept="image/*" capture="environment" style="display:none" onchange="addDrPhoto(this)">
      ${drPhotos.map((src, i) => `<img src="${src}" class="photo-thumb" alt="Visit photo ${i+1}" onclick="viewDrPhoto('${src}')">`).join('')}
    `;
  }

  function viewDrPhoto(src) {
    const lb = document.getElementById('lightbox');
    document.getElementById('lightbox-content').innerHTML = `
      <img src="${src}" style="width:100%;border-radius:4px;display:block;">
    `;
    lb.classList.add('open');
  }

  // ── SUMMARY REFRESH ──
  function refreshSummaryQuestions() {
    const el = document.getElementById('summary-questions-list');
    if (drQuestions.length === 0) {
      el.innerHTML = '<div style="font-size:13px;color:var(--muted);font-weight:300;font-style:italic;">No questions added yet — go to Next Visit tab to add some.</div>';
    } else {
      el.innerHTML = drQuestions.map((q, i) => `
        <div class="summary-question-item">
          <div class="summary-q-num">${i + 1}.</div>
          <div>${q.text}</div>
        </div>`).join('');
    }
  }

  function refreshSummary() {
    refreshSummaryQuestions();
    renderMeds();
  }

  function printSummary() {
    window.print();
  }

  // Init Dr Visit
  renderQuestions();
  renderMentions();
  renderMeds();
  renderVisitHistory();
  renderDrPhotos();

  // ── CHECK-IN ──
  let checkinHistory = JSON.parse(localStorage.getItem('checkins') || '[]');

  const checkinMessages = {
    gym: {
      messages: ["¡Tú puedes! Let's get it 💪", "Every rep counts, mama.", "You showed up. That's already winning.", "Stronger than yesterday. Let's go.", "This is for YOU, Val. 🔥"],
      emoji: '🏋️',
      glucoseNote: { low: "⚠️ Glucose is low — have a snack before you start!", high: "Glucose is a little high — light warm-up first.", good: "Numbers look great. Crush it! 💪" }
    },
    restaurant: {
      messages: ["¡Buen provecho! Enjoy every bite 🍽️", "You deserve this. Eat with joy.", "¡Buen apetito, hermosa!", "Savor it. Life is too short for sad meals.", "Food is medicine and pleasure. Both count."],
      emoji: '🍽️',
      glucoseNote: { low: "Glucose is low — perfect timing to eat! Enjoy.", high: "Heads up, glucose is elevated — maybe skip the chips 😅", good: "Great numbers going in. Enjoy your meal!" }
    },
    cafe: {
      messages: ["Cozy mode activated ☕", "A little calm goes a long way.", "You deserve this quiet moment.", "Sip slowly. You've earned the break.", "Coffee + you = a good idea."],
      emoji: '☕',
      glucoseNote: { low: "Glucose is low — grab a snack with that coffee!", high: "Skip the sugary drinks for now, amor.", good: "All good. Enjoy your vibe ✨" }
    },
    home: {
      messages: ["Rest is productive too 🏠", "Home is where you heal.", "The girls need a healthy mama. Rest up.", "Recharge mode: on.", "You're exactly where you need to be."],
      emoji: '🌿',
      glucoseNote: { low: "Glucose is low — snack time!", high: "Numbers are up — water and movement will help.", good: "All good at home base 💚" }
    },
    other: {
      messages: ["Out and about! You've got this.", "Errands done = adulting won.", "One thing at a time, amor.", "You're doing great. Keep going.", "Every little task counts."],
      emoji: '✨',
      glucoseNote: { low: "Glucose is low — grab something before you go!", high: "Glucose is up — stay hydrated out there.", good: "Numbers look good. Go handle it! 💪" }
    }
  };

  function getGlucoseState() {
    const val = parseInt(document.getElementById('glucose-display').textContent);
    if (isNaN(val)) return 'good';
    if (val < 70) return 'low';
    if (val > 180) return 'high';
    return 'good';
  }

  function checkIn(type, emoji, name, category) {
    const config = checkinMessages[category] || checkinMessages.other;
    const msg = config.messages[Math.floor(Math.random() * config.messages.length)];
    const glucoseState = getGlucoseState();
    const glucoseMsg = config.glucoseNote[glucoseState];
    const glucose = document.getElementById('glucose-display').textContent;

    const sheet = document.getElementById('checkin-modal-sheet');
    const isGood = glucoseState === 'good';
    const isLow = glucoseState === 'low';

    sheet.innerHTML = `
      <div class="checkin-modal-emoji">${config.emoji}</div>
      <div class="checkin-modal-msg">${msg}</div>
      <div class="checkin-modal-place">Checked in at ${name}</div>
      <div class="checkin-modal-glucose ${isGood ? 'good' : 'watch'}">
        ${isGood ? '✅' : isLow ? '⚠️' : '📈'} ${glucoseMsg}
      </div>
      <button class="checkin-modal-close" onclick="document.getElementById('checkin-modal').classList.remove('open')">Done</button>
    `;

    document.getElementById('checkin-modal').classList.add('open');

    // Log it
    const entry = {
      type, emoji, name, category,
      glucose,
      time: new Date().toLocaleTimeString([], {hour:'2-digit', minute:'2-digit'}),
      date: new Date().toLocaleDateString()
    };
    checkinHistory.unshift(entry);
    try { localStorage.setItem('checkins', JSON.stringify(checkinHistory.slice(0,50))); } catch(e) {}
    renderCheckinHistory();
  }

  function renderCheckinHistory() {
    const el = document.getElementById('checkin-history');
    const todayEntries = checkinHistory.filter(e => e.date === new Date().toLocaleDateString());
    if (todayEntries.length === 0) {
      el.innerHTML = `<div style="text-align:center;padding:20px 0;color:var(--muted);font-size:13px;font-weight:300;">No check-ins yet today</div>`;
      return;
    }
    el.innerHTML = todayEntries.map(e => {
      const val = parseInt(e.glucose);
      const inRange = !isNaN(val) && val >= 70 && val <= 180;
      const color = isNaN(val) ? 'var(--muted)' : inRange ? 'var(--in-range)' : 'var(--out-range)';
      return `
        <div class="checkin-entry">
          <div class="checkin-entry-icon" style="background:var(--cream)">${e.emoji}</div>
          <div class="checkin-entry-info">
            <div class="checkin-entry-name">${e.name}</div>
            <div class="checkin-entry-time">${e.time}</div>
          </div>
          <div class="checkin-entry-glucose" style="color:${color}">${e.glucose}</div>
        </div>`;
    }).join('');
  }

  function closeCheckinModal(e) {
    if (e.target === document.getElementById('checkin-modal')) {
      document.getElementById('checkin-modal').classList.remove('open');
    }
  }

  function findNearbyPlaces() {
    const status = document.getElementById('locate-status');
    status.textContent = 'Locating you...';
    if (!navigator.geolocation) { status.textContent = 'Location not available'; return; }
    navigator.geolocation.getCurrentPosition(
      (pos) => { status.textContent = `Found you · ${pos.coords.latitude.toFixed(3)}, ${pos.coords.longitude.toFixed(3)}`; },
      () => { status.textContent = 'Could not get location — use Quick Check-In below'; }
    );
  }

  renderCheckinHistory();

  // ── VIBES / SPOTIFY ──
  const vibeHour = new Date().getHours();

  const timePlaylists = {
    morning: { icon:'🌅', title:'Morning Energy', sub:'Upbeat pop & Latin beats to power your start', vibes:['energized','happy'] },
    midday:  { icon:'☀️', title:'Midday Flow', sub:'Feel-good mix to keep your momentum', vibes:['focus','happy','energized'] },
    afternoon: { icon:'🌤️', title:'Afternoon Reset', sub:'Smooth & motivating — keep pushing', vibes:['focus','calm','postworkout'] },
    evening: { icon:'🌆', title:'Evening Wind-Down', sub:'Softer tones, slower pace — you did good today', vibes:['calm','sleepy'] },
    night:   { icon:'🌙', title:'Night Mode', sub:'Gentle sounds to help you rest & recover', vibes:['sleepy','calm'] }
  };

  function getTimeSlot() {
    if (vibeHour < 11) return 'morning';
    if (vibeHour < 14) return 'midday';
    if (vibeHour < 18) return 'afternoon';
    if (vibeHour < 21) return 'evening';
    return 'night';
  }

  const allPlaylists = {
    energized: [
      { emoji:'⚡', bg:'#FF6B35', name:'¡Arriba! Morning Latina Pop', desc:'Bad Bunny, Karol G, Becky G — high energy Latina heat', tag:'Top Pick' },
      { emoji:'🏃', bg:'#E8A882', name:'Cardio Rush', desc:'BPM-optimized workout bangers', tag:'' },
      { emoji:'🌟', bg:'#C4704A', name:'Feel Good Friday', desc:'Pop hits that make you move', tag:'' },
    ],
    calm: [
      { emoji:'🌿', bg:'#7A9B7E', name:'Classical Calm', desc:'Debussy, Satie, Chopin — pure stress relief', tag:'Top Pick' },
      { emoji:'🛁', bg:'#B5CCBA', name:'Lo-fi Sunday', desc:'Soft beats, zero pressure', tag:'' },
      { emoji:'🌊', bg:'#6B9EA3', name:'Ocean Breathing', desc:'Ambient soundscapes for nervous system reset', tag:'' },
    ],
    focus: [
      { emoji:'🧠', bg:'#4A5568', name:'Deep Focus', desc:'Instrumental + lo-fi for laser concentration', tag:'Top Pick' },
      { emoji:'☕', bg:'#6B4F3A', name:'Café Study Session', desc:'Jazz & acoustic for productive flow', tag:'' },
      { emoji:'🎹', bg:'#8B7355', name:'Piano & Focus', desc:'Classical piano for clear thinking', tag:'' },
    ],
    happy: [
      { emoji:'☀️', bg:'#F6C90E', name:'Pure Happiness', desc:'Viral feel-good hits, pure serotonin', tag:'Top Pick' },
      { emoji:'💃', bg:'#E8558A', name:'Latina Party Starters', desc:'Reggaeton, salsa, cumbia — ¡wepa!', tag:'' },
      { emoji:'🎉', bg:'#C4704A', name:'Mom Wins Playlist', desc:'Songs for when you crushed the day', tag:'' },
    ],
    lowsugar: [
      { emoji:'🍬', bg:'#E8A882', name:'Slow & Steady Recovery', desc:'Calm tunes while you bring glucose back up', tag:'Top Pick' },
      { emoji:'😮‍💨', bg:'#B5CCBA', name:'Gentle Reset', desc:'No loud songs while your body recovers', tag:'' },
      { emoji:'🌿', bg:'#7A9B7E', name:'Healing Frequencies', desc:'432hz music for body recovery', tag:'' },
    ],
    postworkout: [
      { emoji:'💪', bg:'#5C8F62', name:'Cool Down Flow', desc:'Upbeat but slower — perfect for stretching', tag:'Top Pick' },
      { emoji:'🧘', bg:'#7A9B7E', name:'Yoga & Stretch', desc:'Mellow beats for post-gym wind down', tag:'' },
      { emoji:'🌸', bg:'#E8A882', name:'Feel Accomplished', desc:'You earned this playlist, mama', tag:'' },
    ],
    sleepy: [
      { emoji:'🌙', bg:'#2D3748', name:'Sleep Sanctuary', desc:'Binaural beats & soft classical for deep rest', tag:'Top Pick' },
      { emoji:'💤', bg:'#4A5568', name:'Night Lullabies', desc:'Gentle instrumentals as you drift off', tag:'' },
      { emoji:'🌌', bg:'#553C7B', name:'Dreamland', desc:'Ambient soundscapes for sleep', tag:'' },
    ]
  };

  const spotifySearches = {
    energized: 'Latina pop workout playlist',
    calm: 'classical relaxing stress relief playlist',
    focus: 'deep focus instrumental playlist',
    happy: 'feel good pop hits playlist',
    lowsugar: 'calm recovery music playlist',
    postworkout: 'post workout cool down playlist',
    sleepy: 'sleep music binaural beats playlist'
  };

  let currentVibe = 'energized';

  function initVibes() {
    const slot = getTimeSlot();
    const time = timePlaylists[slot];
    document.getElementById('time-vibe-icon').textContent = time.icon;
    document.getElementById('time-vibe-title').textContent = time.title;
    document.getElementById('time-vibe-sub').textContent = time.sub;

    const greetings = { morning:'Good morning,', midday:'Hey there,', afternoon:'Afternoon,', evening:'Good evening,', night:'Wind down,' };
    document.getElementById('vibes-greeting').innerHTML = `${greetings[slot]} <span>Val</span>`;

    // Auto-select vibe based on time
    currentVibe = time.vibes[0];
    document.querySelectorAll('.vibe-chip').forEach(c => c.classList.remove('active'));
    renderPlaylists(currentVibe);
  }

  function selectVibe(el, vibe) {
    document.querySelectorAll('.vibe-chip').forEach(c => c.classList.remove('active'));
    el.classList.add('active');
    currentVibe = vibe;
    renderPlaylists(vibe);
  }

  function renderPlaylists(vibe) {
    const list = allPlaylists[vibe] || allPlaylists.energized;
    const container = document.getElementById('playlist-suggestions');
    container.innerHTML = list.map((p, i) => `
      <div class="playlist-item ${i===0?'featured':''}" onclick="openSpotifySearch('${vibe}')">
        <div class="playlist-art">
          <div class="playlist-art-bg" style="background:${p.bg}"></div>
          <div class="playlist-art-emoji">${p.emoji}</div>
        </div>
        <div class="playlist-info">
          <div class="playlist-name">${p.name}</div>
          <div class="playlist-desc">${p.desc}</div>
        </div>
        ${p.tag ? `<div class="playlist-badge">${p.tag}</div>` : ''}
      </div>
    `).join('');
  }

  function openSpotify() {
    const query = encodeURIComponent(spotifySearches[currentVibe] || 'feel good playlist');
    window.open(`https://open.spotify.com/search/${query}`, '_blank');
  }

  function openSpotifySearch(vibe) {
    const query = encodeURIComponent(spotifySearches[vibe] || 'feel good playlist');
    window.open(`https://open.spotify.com/search/${query}`, '_blank');
  }

  initVibes();

  // ── SCRAPBOOK ──
  let scrapbookEntries = JSON.parse(localStorage.getItem('scrapbook') || '[]');
  let currentPhotoData = null;
  let selectedMood = '';
  const tilts = [-2.5, 1.8, -1.2, 2.1, -0.8, 1.5, -2, 0.9];

  function previewPhoto(input) {
    if (!input.files || !input.files[0]) return;
    const file = input.files[0];
    const reader = new FileReader();
    reader.onload = (e) => {
      currentPhotoData = e.target.result;
      const area = document.getElementById('camera-preview-area');
      area.innerHTML = `<img src="${currentPhotoData}" style="width:100%;height:100%;object-fit:cover;border-radius:12px;">`;
    };
    reader.readAsDataURL(file);
  }

  function selectMood(btn) {
    document.querySelectorAll('.mood-btn').forEach(b => b.classList.remove('selected'));
    btn.classList.add('selected');
    selectedMood = btn.textContent.trim();
  }

  function savePolaroid() {
    const glucose = document.getElementById('snap-glucose').value;
    const tir = document.getElementById('snap-tir').value;
    const recovery = document.getElementById('snap-recovery').value;

    const now = new Date();
    const months = ['January','February','March','April','May','June','July','August','September','October','November','December'];
    const days = ['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday'];
    const dateStr = `${months[now.getMonth()]} ${now.getDate()}, ${now.getFullYear()}`;
    const monthKey = `${months[now.getMonth()]} ${now.getFullYear()}`;

    const entry = {
      id: Date.now(),
      photo: currentPhotoData,
      glucose: glucose || '—',
      tir: tir || '—',
      recovery: recovery || '—',
      mood: selectedMood || '',
      date: dateStr,
      monthKey,
      dayName: days[now.getDay()]
    };

    scrapbookEntries.unshift(entry);
    try { localStorage.setItem('scrapbook', JSON.stringify(scrapbookEntries)); } catch(e) {}

    renderScrapbook();
    resetCaptureForm();
    showToast('Polaroid saved 📸');
  }

  function resetCaptureForm() {
    currentPhotoData = null;
    selectedMood = '';
    document.getElementById('snap-glucose').value = '';
    document.getElementById('snap-tir').value = '';
    document.getElementById('snap-recovery').value = '';
    document.querySelectorAll('.mood-btn').forEach(b => b.classList.remove('selected'));
    const area = document.getElementById('camera-preview-area');
    area.innerHTML = `<div class="camera-preview-icon">📷</div><div class="camera-preview-text">Tap to take today's selfie</div>`;
    document.getElementById('camera-input').value = '';
  }

  function renderScrapbook() {
    const wall = document.getElementById('polaroid-wall');
    const empty = document.getElementById('empty-scrapbook');
    const count = document.getElementById('photo-count');

    count.textContent = `${scrapbookEntries.length} ${scrapbookEntries.length === 1 ? 'entry' : 'entries'}`;

    if (scrapbookEntries.length === 0) {
      wall.innerHTML = '';
      wall.appendChild(empty || createEmpty());
      return;
    }

    // Group by month
    const groups = {};
    scrapbookEntries.forEach(e => {
      if (!groups[e.monthKey]) groups[e.monthKey] = [];
      groups[e.monthKey].push(e);
    });

    wall.innerHTML = '';
    Object.entries(groups).forEach(([month, entries]) => {
      const group = document.createElement('div');
      group.className = 'polaroid-month-group';
      group.innerHTML = `<div class="polaroid-month-label">${month}</div>`;
      const grid = document.createElement('div');
      grid.className = 'polaroid-grid';

      entries.forEach((entry, i) => {
        const tilt = tilts[i % tilts.length];
        const inRange = entry.glucose !== '—' && parseInt(entry.glucose) >= 70 && parseInt(entry.glucose) <= 180;
        const glucoseColor = entry.glucose === '—' ? 'white' : (inRange ? '#98D4A0' : '#F5A898');

        const pol = document.createElement('div');
        pol.className = 'polaroid';
        pol.style.setProperty('--tilt', tilt + 'deg');
        pol.onclick = () => openLightbox(entry);

        pol.innerHTML = `
          <div class="polaroid-photo">
            ${entry.photo
              ? `<img src="${entry.photo}" alt="Day photo">`
              : `<div class="polaroid-photo-placeholder">🌿</div>`}
            <div class="polaroid-overlay">
              <div class="polaroid-stat">
                <div class="polaroid-stat-val" style="color:${glucoseColor}">${entry.glucose}</div>
                <div class="polaroid-stat-lbl">glucose</div>
              </div>
              <div class="polaroid-stat">
                <div class="polaroid-stat-val">${entry.tir}${entry.tir !== '—' ? '%' : ''}</div>
                <div class="polaroid-stat-lbl">in range</div>
              </div>
              <div class="polaroid-stat">
                <div class="polaroid-stat-val">${entry.recovery}</div>
                <div class="polaroid-stat-lbl">recovery</div>
              </div>
            </div>
          </div>
          <div class="polaroid-bottom">
            <div class="polaroid-date">${entry.date}</div>
            <div class="polaroid-mood-tag">${entry.mood}</div>
          </div>
        `;
        grid.appendChild(pol);
      });

      group.appendChild(grid);
      wall.appendChild(group);
    });
  }

  function openLightbox(entry) {
    const lb = document.getElementById('lightbox');
    const content = document.getElementById('lightbox-content');
    const inRange = entry.glucose !== '—' && parseInt(entry.glucose) >= 70 && parseInt(entry.glucose) <= 180;
    const glucoseColor = entry.glucose === '—' ? 'var(--charcoal)' : (inRange ? 'var(--sage)' : 'var(--terracotta)');

    content.innerHTML = `
      <div class="lightbox-photo">
        ${entry.photo
          ? `<img src="${entry.photo}" alt="Day photo">`
          : `<div class="lightbox-photo-placeholder">🌿</div>`}
      </div>
      <div class="lightbox-stats">
        <div class="lightbox-stat">
          <div class="lightbox-stat-val" style="color:${glucoseColor}">${entry.glucose}</div>
          <div class="lightbox-stat-lbl">Glucose</div>
        </div>
        <div class="lightbox-stat">
          <div class="lightbox-stat-val">${entry.tir}${entry.tir !== '—' ? '%' : ''}</div>
          <div class="lightbox-stat-lbl">In Range</div>
        </div>
        <div class="lightbox-stat">
          <div class="lightbox-stat-val">${entry.recovery}</div>
          <div class="lightbox-stat-lbl">Recovery</div>
        </div>
      </div>
      <div class="lightbox-meta">
        <div class="lightbox-date">${entry.date}</div>
        <div class="lightbox-mood">${entry.mood}</div>
      </div>
    `;
    lb.classList.add('open');
  }

  function closeLightbox(e) {
    if (e.target === document.getElementById('lightbox')) {
      document.getElementById('lightbox').classList.remove('open');
    }
  }

  function togglePeriod(btn) {
    document.querySelectorAll('.period-btn').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
  }

  // Log entry
  function logEntry() {
    const glucose = document.getElementById('log-glucose').value;
    const time = document.getElementById('log-time').value;
    const context = document.getElementById('log-context').value;
    const note = document.getElementById('log-note').value;

    if (!glucose) {
      showToast('Enter a glucose reading first');
      return;
    }

    const val = parseInt(glucose);
    const inRange = val >= 70 && val <= 180;
    const timeStr = time || 'Now';
    const label = context || 'Manual entry';
    const noteStr = note || '';

    const entry = document.createElement('div');
    entry.className = 'entry-item';
    entry.style.animation = 'fadeUp 0.3s ease both';
    entry.innerHTML = `
      <div class="entry-left">
        <div class="entry-time">${timeStr} · ${label}</div>
        <div class="entry-note">${noteStr}</div>
      </div>
      <div class="entry-glucose ${inRange ? 'in' : 'out'}">${val}</div>
    `;

    const list = document.getElementById('entries-list');
    list.insertBefore(entry, list.firstChild);

    // Update hero display
    document.getElementById('glucose-display').textContent = val;

    // Clear
    document.getElementById('log-glucose').value = '';
    document.getElementById('log-note').value = '';
    document.getElementById('log-context').value = '';

    showToast('Entry saved ✓');
  }

  function showToast(msg) {
    const t = document.getElementById('toast');
    t.textContent = msg;
    t.classList.add('show');
    setTimeout(() => t.classList.remove('show'), 2500);
  }

  // ── EMERGENCY CARD ──
  let emergencyData = JSON.parse(localStorage.getItem('emergencyData') || '{}');

  function openEmergency() {
    loadEmergencyCard();
    document.getElementById('emergency-screen').classList.add('open');
    document.body.style.overflow = 'hidden';
  }

  function closeEmergency() {
    document.getElementById('emergency-screen').classList.remove('open');
    document.body.style.overflow = '';
  }

  function openEmergencySetup() {
    const d = emergencyData;
    if (d.name)      document.getElementById('setup-name').value     = d.name;
    if (d.dob)       document.getElementById('setup-dob').value      = d.dob;
    if (d.blood)     document.getElementById('setup-blood').value    = d.blood;
    if (d.type)      document.getElementById('setup-type').value     = d.type;
    if (d.insulin)   document.getElementById('setup-insulin').value  = d.insulin;
    if (d.cgm)       document.getElementById('setup-cgm').value      = d.cgm;
    if (d.pump)      document.getElementById('setup-pump').value     = d.pump;
    if (d.c1name)    document.getElementById('setup-c1-name').value  = d.c1name;
    if (d.c1phone)   document.getElementById('setup-c1-phone').value = d.c1phone;
    if (d.c2name)    document.getElementById('setup-c2-name').value  = d.c2name;
    if (d.c2phone)   document.getElementById('setup-c2-phone').value = d.c2phone;
    if (d.allergies) document.getElementById('setup-allergies').value = d.allergies;
    document.getElementById('emergency-setup').classList.add('open');
  }

  function closeEmergencySetup() {
    document.getElementById('emergency-setup').classList.remove('open');
  }

  function saveEmergencyInfo() {
    emergencyData = {
      name:      document.getElementById('setup-name').value.trim(),
      dob:       document.getElementById('setup-dob').value,
      blood:     document.getElementById('setup-blood').value,
      type:      document.getElementById('setup-type').value,
      insulin:   document.getElementById('setup-insulin').value,
      cgm:       document.getElementById('setup-cgm').value.trim(),
      pump:      document.getElementById('setup-pump').value.trim(),
      c1name:    document.getElementById('setup-c1-name').value.trim(),
      c1phone:   document.getElementById('setup-c1-phone').value.trim(),
      c2name:    document.getElementById('setup-c2-name').value.trim(),
      c2phone:   document.getElementById('setup-c2-phone').value.trim(),
      allergies: document.getElementById('setup-allergies').value.trim(),
    };
    try { localStorage.setItem('emergencyData', JSON.stringify(emergencyData)); } catch(e) {}
    closeEmergencySetup();
    loadEmergencyCard();
    showToast('Emergency card saved ✓');
  }

  function loadEmergencyCard() {
    const d = emergencyData;
    document.getElementById('e-name').textContent = d.name || 'Valerie Ramirez';

    if (d.dob) {
      const dob = new Date(d.dob + 'T00:00:00');
      const age = Math.floor((new Date() - dob) / (365.25*24*60*60*1000));
      document.getElementById('e-dob').textContent =
        `DOB: ${dob.toLocaleDateString('en-US',{month:'long',day:'numeric',year:'numeric'})} · Age ${age}`;
    }

    document.getElementById('e-blood').textContent = d.blood ? `🩸 Blood Type: ${d.blood}` : '🩸 Blood Type: Unknown';
    document.getElementById('e-type').textContent    = d.type    || 'Type 1';
    document.getElementById('e-insulin').textContent = d.insulin || 'Yes';
    document.getElementById('e-cgm').textContent     = d.cgm     || 'Dexcom G7';
    document.getElementById('e-pump').textContent    = d.pump    || 'Omnipod 5';

    document.getElementById('e-contact1-name').textContent  = d.c1name  || 'Not set';
    document.getElementById('e-contact1-phone').textContent = d.c1phone || 'No phone added';
    document.getElementById('e-contact1-role').textContent  = 'Emergency Contact 1';
    if (d.c1phone) document.getElementById('e-contact1-link').href = `tel:${d.c1phone}`;

    document.getElementById('e-contact2-name').textContent  = d.c2name  || 'Not set';
    document.getElementById('e-contact2-phone').textContent = d.c2phone || 'No phone added';
    document.getElementById('e-contact2-role').textContent  = 'Emergency Contact 2';
    if (d.c2phone) document.getElementById('e-contact2-link').href = `tel:${d.c2phone}`;

    document.getElementById('e-allergies').textContent = d.allergies || 'No known drug allergies listed. Tap edit to add.';

    // Live glucose
    const glucoseVal = document.getElementById('glucose-display').textContent.trim();
    const val = parseInt(glucoseVal);
    document.getElementById('e-glucose-val').textContent  = `${glucoseVal} mg/dL`;
    document.getElementById('e-glucose-time').textContent = `As of ${new Date().toLocaleTimeString([],{hour:'2-digit',minute:'2-digit'})}`;

    const pulse  = document.getElementById('e-glucose-pulse');
    const status = document.getElementById('e-glucose-status');
    pulse.className  = 'emergency-glucose-pulse';
    status.className = 'emergency-glucose-status';

    if (!isNaN(val)) {
      if (val < 70) {
        pulse.classList.add('low'); status.classList.add('low');
        status.textContent = '⚠️ LOW — Give glucose immediately if conscious';
      } else if (val > 180) {
        pulse.classList.add('high'); status.classList.add('high');
        status.textContent = '⚠️ HIGH — Monitor closely';
      } else {
        pulse.classList.add('in'); status.classList.add('in');
        status.textContent = '✓ In Range (70–180 mg/dL)';
      }
    }
  }

  function setEmergencyAvatar(input) {
    if (!input.files || !input.files[0]) return;
    const reader = new FileReader();
    reader.onload = (e) => {
      document.getElementById('emergency-avatar-display').innerHTML = `<img src="${e.target.result}" alt="Profile photo">`;
      try { localStorage.setItem('emergencyAvatar', e.target.result); } catch(err) {}
    };
    reader.readAsDataURL(input.files[0]);
  }

  function callContact(e, phoneId) {
    e.preventDefault();
    const phone = document.getElementById(phoneId).textContent;
    if (phone && phone !== '—' && phone !== 'No phone added') {
      window.location.href = `tel:${phone}`;
    }
  }

  // Load saved avatar on boot
  const savedAvatar = localStorage.getItem('emergencyAvatar');
  if (savedAvatar) {
    document.getElementById('emergency-avatar-display').innerHTML = `<img src="${savedAvatar}" alt="Profile">`;
  }

  loadEmergencyCard();
</script>
</body>
</html>
