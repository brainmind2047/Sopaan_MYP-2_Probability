<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Sopaan · Probability</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,500;9..144,700&family=Source+Sans+3:wght@400;600;700&family=IBM+Plex+Mono:wght@500;600&display=swap">
<style>
  html,body{margin:0;} img{max-width:100%;} [hidden]{display:none !important;}
  :root{
    --navy:#16264A; --navy-2:#1F3B6B; --gold:#C79A3E; --gold-soft:#F3E7C9;
    --paper:#FBF9F4; --paper-2:#F1EAD8; --card:#FFFFFF;
    --ink:#211E1A; --ink-soft:#655D4C; --rule:#E4DCC8;
    --success:#2E8B57; --success-soft:#E1F2E7;
    --danger:#BF4B45; --danger-soft:#FAE3E1;
    --locked:#C7BEA9;
    --focus:#1F3B6B;
    --accent-text:#1F3B6B; --retry-text:#8A661F;
    color-scheme:light;
  }
  @media (prefers-color-scheme: dark){
    :root:not([data-theme="light"]){
      --navy:#0E1830; --navy-2:#16264A; --gold:#E0B75B; --gold-soft:#3A2F16;
      --paper:#141210; --paper-2:#1C1914; --card:#1C1914;
      --ink:#EDE7DA; --ink-soft:#B2A996; --rule:#3A342A;
      --success:#7BC79A; --success-soft:#1B2E22;
      --danger:#E38884; --danger-soft:#331D1B;
      --locked:#4A4436;
      --focus:#E0B75B;
      --accent-text:#E0B75B; --retry-text:#E0B75B;
      color-scheme:dark;
    }
  }
  :root[data-theme="dark"]{
    --navy:#0E1830; --navy-2:#16264A; --gold:#E0B75B; --gold-soft:#3A2F16;
    --paper:#141210; --paper-2:#1C1914; --card:#1C1914;
    --ink:#EDE7DA; --ink-soft:#B2A996; --rule:#3A342A;
    --success:#7BC79A; --success-soft:#1B2E22;
    --danger:#E38884; --danger-soft:#331D1B;
    --locked:#4A4436;
    --focus:#E0B75B;
    --accent-text:#E0B75B; --retry-text:#E0B75B;
    color-scheme:dark;
  }

  *{box-sizing:border-box;}
  body{background:var(--paper); color:var(--ink); font-family:'Source Sans 3',-apple-system,'Segoe UI',sans-serif; padding-inline:0; padding-block:0;}
  h1,h2,h3{font-family:'Fraunces',Georgia,serif; text-wrap:balance; margin:0;}
  .num{font-family:'IBM Plex Mono',ui-monospace,monospace; font-variant-numeric:tabular-nums;}

  header.brand{
    background:linear-gradient(155deg,var(--navy) 0%, var(--navy-2) 100%);
    color:#F4EFDF; padding-block:calc(20px + env(safe-area-inset-top,0px)) 18px; padding-inline:20px;
  }
  .brand-row{display:flex; align-items:center; gap:12px; max-width:900px; margin:0 auto;}
  .crest{
    width:42px; height:42px; border-radius:10px; background:var(--gold); color:var(--navy);
    display:flex; align-items:center; justify-content:center; font-family:'Fraunces',serif; font-weight:700; font-size:18px; flex-shrink:0;
  }
  .brand-name{font-size:12px; letter-spacing:.08em; text-transform:uppercase; color:var(--gold); font-weight:700;}
  .series-name{font-size:19px; font-weight:700; font-family:'Fraunces',serif;}
  .chapter-eyebrow{max-width:900px; margin:14px auto 0; font-size:12px; letter-spacing:.06em; text-transform:uppercase; color:#AEB9D6;}
  .chapter-title{font-size:28px; max-width:900px; margin:2px auto 0;}
  .chapter-sub{font-size:14.5px; color:var(--gold); font-weight:700; max-width:900px; margin:3px auto 0;}

  .chapter-progress{max-width:900px; margin:14px auto 0; display:flex; align-items:center; gap:10px;}
  .cp-track{flex:1; height:6px; border-radius:99px; background:rgba(255,255,255,.18); overflow:hidden;}
  .cp-fill{height:100%; background:var(--gold); border-radius:99px; transition:width .3s ease;}
  .cp-label{font-size:11.5px; color:#CFD7EA; white-space:nowrap;}

  .tabbar{position:sticky; top:env(safe-area-inset-top,0px); z-index:15; background:var(--paper); border-bottom:1px solid var(--rule); overflow-x:auto; white-space:nowrap;}
  .tabbar-inner{max-width:900px; margin:0 auto; display:flex; padding-inline:16px;}
  .tab-btn{font-family:inherit; font-size:14px; font-weight:600; color:var(--ink-soft); background:none; border:none; padding:13px 16px; cursor:pointer; position:relative; flex-shrink:0;}
  .tab-btn.active{color:var(--accent-text);}
  .tab-btn.active::after{content:''; position:absolute; left:12px; right:12px; bottom:0; height:3px; background:var(--gold); border-radius:3px 3px 0 0;}
  .tab-count{font-size:11px; color:var(--ink-soft); margin-left:5px;}

  .slide-progress{max-width:900px; margin:0 auto; padding:10px 16px 0; display:flex; align-items:center; gap:10px;}
  .sp-track{flex:1; height:5px; border-radius:99px; background:var(--rule); overflow:hidden;}
  .sp-fill{height:100%; background:var(--accent-text); border-radius:99px; transition:width .3s ease;}
  .sp-label{font-size:12px; color:var(--ink-soft); white-space:nowrap; font-weight:600;}

  .wrap{max-width:900px; margin:0 auto; padding:16px 16px calc(110px + env(safe-area-inset-bottom,0px));}

  .qcard{background:var(--card); border:1px solid var(--rule); border-radius:14px; padding:18px;}
  .qhead{display:flex; align-items:flex-start; gap:10px; margin-bottom:10px;}
  .qnum{width:32px; height:32px; border-radius:8px; background:var(--gold-soft); color:var(--accent-text); display:flex; align-items:center; justify-content:center; font-weight:700; font-family:'Fraunces',serif; flex-shrink:0; font-size:14px;}
  .qtext{font-size:16px; line-height:1.55; flex:1;}
  .qtag{font-size:10.5px; color:var(--gold); background:var(--gold-soft); padding:2px 7px; border-radius:99px; font-weight:700; margin-left:6px; white-space:nowrap;}
  .marks-pill{font-size:11px; font-weight:700; color:var(--ink-soft); border:1px solid var(--rule); padding:3px 9px; border-radius:99px; margin-left:auto; flex-shrink:0; white-space:nowrap;}
  .step-badge{font-size:11px; font-weight:700; color:var(--accent-text); background:var(--paper-2); border:1px solid var(--rule); padding:3px 9px; border-radius:99px; display:inline-block; margin-bottom:12px;}

  .options{display:flex; flex-direction:column; gap:8px; margin-top:10px;}
  .opt{display:flex; align-items:center; gap:10px; padding:11px 12px; border:1.5px solid var(--rule); border-radius:10px; cursor:pointer; font-size:15px; background:var(--paper);}
  .opt input{accent-color:var(--navy-2);}
  .opt.is-correct{border-color:var(--success); background:var(--success-soft);}
  .opt.is-wrong{border-color:var(--danger); background:var(--danger-soft);}
  .opt.locked{cursor:not-allowed;}

  .subpart-label{font-weight:700; font-size:12.5px; color:var(--accent-text); text-transform:uppercase; letter-spacing:.03em; margin:14px 0 6px;}
  .or-divider{text-align:center; font-size:11px; font-weight:700; letter-spacing:.08em; color:var(--ink-soft); margin:8px 0;}

  .steps{display:flex; flex-direction:column; gap:10px;}
  .step-line{font-size:14.5px; line-height:1.9; background:var(--paper); border:1px dashed var(--rule); border-radius:10px; padding:9px 12px;}
  .step-line.resolved{opacity:.9;}
  .blank-input{font-family:'IBM Plex Mono',monospace; font-size:14px; border:1.5px solid var(--rule); border-radius:6px; padding:3px 8px; width:120px; background:var(--card); color:var(--ink); margin:0 2px;}
  .blank-input:focus{outline:none; border-color:var(--focus); box-shadow:0 0 0 3px var(--gold-soft);}
  .blank-input.is-correct{border-color:var(--success); background:var(--success-soft);}
  .blank-input.is-wrong{border-color:var(--danger); background:var(--danger-soft);}
  .blank-input[disabled]{opacity:.85;}
  .reveal-note{font-size:12px; color:var(--danger); padding-left:2px; margin-top:-2px;}

  .feedback{font-size:13.5px; padding:10px 12px; border-radius:9px; margin-top:14px; display:none;}
  .feedback.show{display:block;}
  .feedback.ok{background:var(--success-soft); color:var(--success);}
  .feedback.retry{background:var(--gold-soft); color:var(--retry-text);}
  .feedback.reveal{background:var(--danger-soft); color:var(--danger);}
  .feedback.err{background:var(--danger-soft); color:var(--danger);}
  .attempts-dots{display:inline-flex; gap:4px; margin-left:2px;}
  .attempts-dots span{width:6px; height:6px; border-radius:50%; background:var(--ink-soft); opacity:.3; display:inline-block;}
  .attempts-dots span.used{opacity:1; background:var(--danger);}

  .ar-box{background:var(--paper-2); border-radius:10px; padding:10px 12px; margin-bottom:12px; font-size:13px; color:var(--ink-soft);}

  .navbar{
    position:fixed; left:0; right:0; bottom:0; z-index:30; background:var(--paper);
    border-top:1px solid var(--rule); padding:10px 16px calc(10px + env(safe-area-inset-bottom,0px));
  }
  .navbar-inner{max-width:900px; margin:0 auto; display:flex; gap:8px; flex-wrap:wrap; align-items:center;}
  .btn{font-family:inherit; font-size:13.5px; font-weight:700; padding:10px 14px; border-radius:9px; border:1.5px solid var(--rule); background:var(--card); color:var(--ink); cursor:pointer;}
  .btn:hover{border-color:var(--navy-2);}
  .btn:disabled{opacity:.4; cursor:not-allowed;}
  .btn-primary{background:var(--navy-2); color:#fff; border-color:var(--navy-2);}
  .navbar .spacer{flex:1;}

  .fab{position:fixed; right:16px; bottom:calc(74px + env(safe-area-inset-bottom,0px)); background:var(--gold); color:var(--navy); border:none; border-radius:999px; padding:11px 16px; font-family:inherit; font-weight:700; font-size:13px; display:flex; align-items:center; gap:7px; cursor:pointer; box-shadow:0 6px 16px rgba(0,0,0,.18); z-index:35;}
  .fab-badge{background:rgba(255,255,255,.55); border-radius:99px; padding:1px 7px; font-size:11px;}

  .overlay{position:fixed; inset:0; background:rgba(20,18,14,.45); z-index:50; display:none; align-items:flex-end; justify-content:center;}
  .overlay.show{display:flex;}
  .palette{background:var(--card); width:min(480px,100%); max-height:78vh; overflow-y:auto; border-radius:18px 18px 0 0; padding:18px 18px calc(18px + env(safe-area-inset-bottom,0px)); border:1px solid var(--rule); border-bottom:none;}
  @media (min-width:640px){ .overlay{align-items:center;} .palette{border-radius:18px; border-bottom:1px solid var(--rule); max-height:74vh;} }
  .palette-head{display:flex; align-items:center; justify-content:space-between; margin-bottom:6px;}
  .palette-head h2{font-size:16px;}
  .palette-close{background:none; border:none; font-size:20px; color:var(--ink-soft); cursor:pointer; padding:4px;}
  .palette-legend{display:flex; gap:12px; flex-wrap:wrap; font-size:11px; color:var(--ink-soft); margin:10px 0 14px;}
  .palette-legend span{display:inline-flex; align-items:center; gap:5px;}
  .dot{width:9px; height:9px; border-radius:50%; display:inline-block;}
  .dot.correct{background:var(--success);} .dot.revealed{background:var(--danger);}
  .dot.skipped{background:var(--gold);} .dot.locked{background:var(--locked);}
  .dot.current{background:var(--navy-2);}
  .chip-grid{display:grid; grid-template-columns:repeat(auto-fill,minmax(48px,1fr)); gap:8px;}
  .chip{border:1.5px solid var(--rule); border-radius:9px; padding:8px 4px; text-align:center; font-family:'IBM Plex Mono',monospace; font-size:12.5px; font-weight:600; cursor:pointer; background:var(--paper); color:var(--ink);}
  .chip[data-status="correct"]{border-color:var(--success); background:var(--success-soft); color:var(--success);}
  .chip[data-status="revealed"]{border-color:var(--danger); background:var(--danger-soft); color:var(--danger);}
  .chip[data-status="skipped"]{border-color:var(--gold); background:var(--gold-soft); color:var(--retry-text);}
  .chip[data-status="locked"]{border-color:var(--rule); color:var(--locked); cursor:not-allowed; background:var(--paper-2);}
  .chip[data-status="current"]{outline:2px solid var(--navy-2); outline-offset:1px;}

  .toast{position:fixed; left:50%; bottom:calc(140px + env(safe-area-inset-bottom,0px)); transform:translateX(-50%); background:var(--ink); color:var(--paper); padding:9px 16px; border-radius:99px; font-size:13px; opacity:0; pointer-events:none; transition:opacity .25s ease; z-index:60;}
  .toast.show{opacity:1;}

  .done-card{text-align:center; padding:36px 20px;}
  .done-card .big{font-size:38px; margin-bottom:6px;}
  .score-pills{display:flex; gap:10px; justify-content:center; flex-wrap:wrap; margin:16px 0;}
  .score-pill{padding:8px 16px; border-radius:99px; font-size:13px; font-weight:700;}

  footer.brandfoot{max-width:900px; margin:14px auto 0; padding:0 16px calc(110px + env(safe-area-inset-bottom,0px)); text-align:center; font-size:12px; color:var(--ink-soft);}

  .mode-pill{font-family:inherit; font-size:12.5px; font-weight:700; color:var(--navy); background:var(--gold); border:none; border-radius:99px; padding:6px 14px; cursor:pointer; margin-top:12px; display:inline-flex; align-items:center; gap:6px;}
  .mode-pick{display:flex; flex-direction:column; gap:14px; padding:8px 0 24px;}
  .mode-card{background:var(--card); border:1.5px solid var(--rule); border-radius:16px; padding:22px; cursor:pointer; text-align:left;}
  .mode-card:hover{border-color:var(--navy-2);}
  .mode-card .mode-icon{font-size:26px; margin-bottom:8px;}
  .mode-card h3{font-size:18px; margin-bottom:6px;}
  .mode-card p{font-size:13.5px; color:var(--ink-soft); line-height:1.5; margin:0;}
  .quiz-hint{font-size:12.5px; color:var(--ink-soft); background:var(--paper-2); border-radius:9px; padding:8px 12px; margin-bottom:14px;}
  .review-row{border:1px solid var(--rule); border-radius:10px; padding:11px 13px; margin-bottom:10px;}
  .review-tag{font-size:11.5px; font-weight:700; margin-bottom:4px; display:block;}
  .review-tag.ok{color:var(--success);} .review-tag.bad{color:var(--danger);} .review-tag.na{color:var(--ink-soft);}
  .review-q{font-size:13.5px; margin-bottom:6px; color:var(--ink-soft);}
  .review-ans{font-size:13px; margin-bottom:2px;}

  .who-row{max-width:900px; margin:12px auto 0; display:flex; flex-wrap:wrap; gap:8px; align-items:center;}
  .who-row .mode-pill{margin-top:0;}
  .who{display:inline-flex; flex-wrap:wrap; align-items:center; gap:6px; font-size:12.5px; color:#CFD7EA;}
  .who b{color:#F4EFDF;}
  .who button{font-family:inherit; font-size:12px; font-weight:700; color:#F4EFDF; background:rgba(255,255,255,.12); border:1px solid rgba(255,255,255,.22); border-radius:99px; padding:5px 11px; cursor:pointer;}
  .who button:hover{background:rgba(255,255,255,.2);}
  .login-card{max-width:460px; margin:8px auto 0; background:var(--card); border:1px solid var(--rule); border-radius:16px; padding:22px;}
  .login-card h2{font-size:21px; margin-bottom:4px;}
  .login-card p.lead{font-size:13.5px; color:var(--ink-soft); margin:0 0 16px; line-height:1.5;}
  .fld{display:flex; flex-direction:column; gap:5px; margin-bottom:12px;}
  .fld label{font-size:12px; font-weight:700; letter-spacing:.04em; text-transform:uppercase; color:var(--ink-soft);}
  .fld input{font-family:inherit; font-size:15px; padding:10px 12px; border:1.5px solid var(--rule); border-radius:9px; background:var(--paper); color:var(--ink);}
  .fld input:focus{outline:none; border-color:var(--focus); box-shadow:0 0 0 3px var(--gold-soft);}
  .fld-row{display:grid; grid-template-columns:1fr 1fr; gap:10px;}
  .login-err{font-size:13px; color:var(--danger); background:var(--danger-soft); border-radius:8px; padding:8px 10px; margin-bottom:12px;}
  .login-note{font-size:12px; color:var(--ink-soft); margin-top:12px; line-height:1.5;}
  .known{margin:0 0 16px; display:flex; flex-direction:column; gap:6px;}
  .known-label{font-size:12px; font-weight:700; letter-spacing:.04em; text-transform:uppercase; color:var(--ink-soft);}
  .known-btn{display:flex; justify-content:space-between; align-items:center; gap:10px; text-align:left; font-family:inherit; font-size:14.5px; padding:10px 12px; border:1.5px solid var(--rule); border-radius:10px; background:var(--paper); color:var(--ink); cursor:pointer;}
  .known-btn:hover{border-color:var(--navy-2);}
  .known-btn small{font-size:12px; color:var(--ink-soft);}
  .rec-card{background:var(--card); border:1px solid var(--rule); border-radius:14px; padding:18px; margin-bottom:14px;}
  .rec-card h2{font-size:19px; margin-bottom:10px;}
  .rec-card h3{font-size:15px; margin:4px 0 8px;}
  .rec-table-wrap{overflow-x:auto;}
  .rec-table{width:100%; border-collapse:collapse; font-size:13.5px; font-variant-numeric:tabular-nums;}
  .rec-table th{text-align:left; font-size:11.5px; text-transform:uppercase; letter-spacing:.04em; color:var(--ink-soft); padding:6px 8px; border-bottom:1px solid var(--rule); white-space:nowrap;}
  .rec-table td{padding:8px; border-bottom:1px solid var(--rule); white-space:nowrap;}
  .rec-bar{display:inline-block; width:70px; height:6px; border-radius:99px; background:var(--rule); overflow:hidden; vertical-align:middle; margin-right:6px;}
  .rec-bar i{display:block; height:100%; background:var(--success);}
  .rec-empty{font-size:13.5px; color:var(--ink-soft);}
  .sec-sub{max-width:900px; margin:0 auto; padding:10px 16px 0; font-size:12.5px; font-weight:700; color:var(--accent-text); letter-spacing:.02em;}
  .opt{flex-wrap:wrap;}
  .opt-l{font-family:'IBM Plex Mono',monospace; font-size:13px; font-weight:600; color:var(--ink-soft);}
  .opt-t{flex:1; min-width:0;}
  .opt.is-chosen:not(.is-correct):not(.is-wrong){border-color:var(--navy-2); background:var(--gold-soft);}
  .opt-tag{font-size:11px; font-weight:700; padding:2px 8px; border-radius:99px; background:var(--card); border:1px solid currentColor; white-space:nowrap;}
  .opt.is-correct .opt-tag{color:var(--success);} .opt.is-wrong .opt-tag{color:var(--danger);} .opt.is-chosen:not(.is-correct):not(.is-wrong) .opt-tag{color:var(--accent-text);}
  .chosen{font-size:13.5px; margin-top:10px; padding:8px 12px; border-radius:9px;}
  .chosen.ok{background:var(--success-soft); color:var(--success);} .chosen.bad{background:var(--danger-soft); color:var(--danger);}
  .chosen + .chosen{margin-top:6px;}
  .solution{margin-top:14px; border:1px solid var(--rule); border-left:4px solid var(--gold); background:var(--paper-2); border-radius:10px; padding:12px 14px;}
  .sol-h{font-family:'Fraunces',Georgia,serif; font-weight:700; font-size:14px; color:var(--accent-text); margin-bottom:6px;}
  .sol-line{font-size:14.5px; line-height:1.7;}
  .rev-sol summary{cursor:pointer; font-size:12.5px; font-weight:700; color:var(--accent-text); margin-top:6px;}
  .rev-sol .solution{margin-top:8px;}
  .chapter-credit{max-width:900px; margin:4px auto 0; font-size:12px; color:#CFD7EA;}
  footer.brandfoot a{color:inherit;}
  .blank-input.wide{width:260px; max-width:100%; font-family:'Source Sans 3',sans-serif;}
  .blank-input.expr{width:170px; max-width:100%;}
  /*fix-layout*/ .cp-fill,.sp-fill{width:0;} .fld{min-width:0;} .fld input{width:100%; min-width:0; box-sizing:border-box;}
  .fig{margin:6px 0 10px; display:flex; justify-content:center;}
  .figsvg{width:100%; max-width:340px; height:auto; overflow:visible;}
  .figsvg .ln,.figsvg .arm{stroke:var(--ink); stroke-width:1.6; fill:none;}
  .figsvg .arm{stroke:var(--accent-text); stroke-width:2;}
  .figsvg .pt{fill:var(--ink);}
  .figsvg .lb{fill:var(--ink); font:600 13px 'Source Sans 3',sans-serif;}
  .figsvg .al{fill:var(--accent-text); font:700 12px 'Source Sans 3',sans-serif;}
  .figsvg .wg{fill:var(--gold-soft); stroke:var(--gold); stroke-width:1.2;}
  .figsvg .wg2{fill:rgba(199,154,62,.35); stroke:var(--gold); stroke-width:1.2;}
  .figsvg .ra{fill:none; stroke:var(--ink); stroke-width:1.2;}
  .figsvg .ahd{fill:var(--ink);}
  .figsvg .pr{fill:var(--paper-2); stroke:var(--ink-soft); stroke-width:1.2;}
  .figsvg .tk{stroke:var(--ink-soft); stroke-width:.8;}
  .figsvg .po{fill:var(--ink); font:600 8.5px 'IBM Plex Mono',monospace;}
  .figsvg .pi{fill:var(--danger); font:600 7.5px 'IBM Plex Mono',monospace;}
  .figsvg .sh{fill:var(--gold-soft); stroke:var(--ink); stroke-width:1.5; stroke-linejoin:round;}
  .figsvg .sh2{fill:rgba(199,154,62,.38); stroke:var(--ink); stroke-width:1.5; stroke-linejoin:round;}
  .figsvg .sh3{fill:rgba(199,154,62,.22); stroke:var(--ink); stroke-width:1.5; stroke-linejoin:round;}
  .figsvg .none{fill:none;}
  .figsvg .hid{stroke-dasharray:5 4; fill:none;}
  .figsvg .tick{stroke:var(--ink); stroke-width:1.4; fill:none;}
  .figsvg .shd{fill:var(--accent-text); fill-opacity:.55;}
  .figsvg .cell{fill:var(--card); stroke:var(--ink); stroke-width:1.1;}
  .figsvg .dr{fill:var(--danger);} .figsvg .dw{fill:var(--card); stroke:var(--ink); stroke-width:1.2;}
  .figsvg .dotfill{fill:var(--danger);}
  .tt{border-collapse:collapse; font-size:13.5px; margin:4px auto; font-variant-numeric:tabular-nums;} .tt th,.tt td{border:1px solid var(--rule); padding:4px 9px; text-align:left;} .tt th{background:var(--paper-2); font-weight:700;} .ttw{overflow-x:auto; max-width:100%;}
  .fq{display:inline-flex; flex-direction:column; vertical-align:middle; text-align:center; font-size:.82em; line-height:1.12; margin:0 2px;}
  .fq>span:first-child{border-bottom:1.5px solid currentColor; padding:0 2px;}
  .fq>span:last-child{padding:0 2px;}
  .mx{white-space:nowrap;}
  .blank-input.fr{width:110px;}
  @media (prefers-reduced-motion:reduce){ *{transition:none !important;} }
.datline{display:block;margin-top:8px;padding:8px 10px;border-radius:8px;background:var(--paper-2);font:500 14px/1.7 'IBM Plex Mono',monospace;word-spacing:2px;overflow-wrap:anywhere;}

  .theory{display:flex;flex-direction:column;gap:16px;padding-bottom:40px;}
  .hub{display:grid;grid-template-columns:1fr;gap:12px;}
  @media (min-width:640px){ .hub{grid-template-columns:repeat(3,1fr);} }
  .hub-card{background:var(--card);border:1px solid var(--rule);border-radius:14px;padding:16px;display:flex;flex-direction:column;gap:8px;}
  .hub-card h3{font-family:'Fraunces',serif;font-size:18px;margin:0;color:var(--accent-text);}
  .hub-card p{margin:0;font-size:14px;color:var(--ink-soft);line-height:1.5;}
  .hub-btns{display:flex;flex-wrap:wrap;gap:8px;margin-top:auto;}
  .hub-btn{min-height:40px;border:1.5px solid var(--rule);background:var(--paper-2);color:var(--ink);border-radius:10px;padding:8px 12px;font:600 14px 'Source Sans 3',sans-serif;cursor:pointer;text-align:left;}
  .hub-btn:hover{border-color:var(--gold);}
  .hub-btn.primary{background:var(--navy);color:#F4EFDF;border-color:var(--navy);}
  .note{background:var(--card);border:1px solid var(--rule);border-radius:14px;padding:18px;scroll-margin-top:120px;}
  .note h2{font-family:'Fraunces',serif;font-size:22px;margin:0 0 4px;color:var(--ink);}
  .note .lt{font-size:13.5px;color:var(--ink-soft);margin:0 0 12px;}
  .note .lt b{color:var(--accent-text);}
  .note h4{font:700 15px 'Source Sans 3',sans-serif;margin:16px 0 6px;color:var(--accent-text);}
  .note p,.note li{font-size:15.5px;line-height:1.6;}
  .note ul{padding-left:20px;margin:6px 0;}
  .keybox{background:var(--gold-soft);border-left:4px solid var(--gold);border-radius:8px;padding:10px 12px;margin:10px 0;font-size:15px;line-height:1.6;}
  .keybox b{color:var(--ink);}
  .ex{background:var(--paper-2);border-radius:10px;padding:12px;margin:10px 0;}
  .ex .exh{font-family:'Fraunces',serif;font-weight:700;color:var(--accent-text);margin-bottom:4px;}
  .ex .exl{font-size:15px;line-height:1.7;}
  .ttab{width:100%;border-collapse:collapse;font-size:14.5px;margin:8px 0;}
  .ttab th,.ttab td{border:1px solid var(--rule);padding:7px 8px;text-align:left;vertical-align:top;}
  .ttab th{background:var(--paper-2);}
  .tscroll{overflow-x:auto;}
  .mono{font-family:'IBM Plex Mono',monospace;font-size:.92em;}
  @media (max-width:480px){ .ttab{font-size:13.5px;} .ttab th,.ttab td{padding:6px;} }
  .note .figsvg{max-width:100%;height:auto;display:block;margin:8px auto;}


  .rep-top{display:flex;flex-wrap:wrap;gap:18px;align-items:center;justify-content:center;margin-top:8px;}
  .rep-legend{flex:1;min-width:220px;display:flex;flex-direction:column;gap:8px;}
  .rep-cap{font:700 13px 'Source Sans 3',sans-serif;color:var(--ink-soft);text-transform:uppercase;letter-spacing:.05em;}
  .rep-li{display:flex;align-items:center;gap:6px;font-size:15px;}
  .rep-n{margin-left:auto;white-space:nowrap;padding-left:8px;font-family:'IBM Plex Mono',monospace;font-weight:600;}
  .sw{display:inline-block;width:12px;height:12px;border-radius:3px;flex:none;}
  .rep-grid{display:grid;grid-template-columns:1fr;gap:12px;}
  @media (min-width:640px){ .rep-grid{grid-template-columns:1fr 1fr;} }
  .rep-card{background:var(--card);border:1px solid var(--rule);border-radius:14px;padding:14px;display:flex;flex-direction:column;gap:10px;}
  .rep-h{display:flex;align-items:center;gap:8px;font:700 16px 'Source Sans 3',sans-serif;}
  .crit{display:inline-grid;place-items:center;width:28px;height:28px;border-radius:50%;color:var(--card);font:700 15px Fraunces,serif;flex:none;}
  .rep-row{display:flex;gap:14px;align-items:center;}
  .rep-stats{display:flex;flex-direction:column;gap:5px;font-size:14.5px;}
  .rep-stats div{display:flex;align-items:center;gap:6px;}
  .rep-lvl{margin-top:4px;color:var(--accent-text);} .rep-lvl span{color:var(--ink-soft);font-size:13px;}
  .rep-note{font-size:13px;color:var(--ink-soft);}


  .skip-chips{display:flex;flex-wrap:wrap;gap:8px;justify-content:center;margin:12px 0;}
  .skip-chips .chip{min-width:48px;min-height:40px;cursor:pointer;border:1.5px solid var(--gold);background:var(--gold-soft);color:var(--retry-text);border-radius:10px;font:600 14px 'IBM Plex Mono',monospace;}
  .skip-btns{display:flex;flex-wrap:wrap;gap:10px;justify-content:center;margin-top:12px;}

</style>
</head>
<body>
<header class="brand">
  <div class="brand-row">
    <div class="crest">BM</div>
    <div>
      <div class="brand-name">Brain &amp; Mind Academy</div>
      <div class="series-name">Sopaan <span style="font-weight:400;font-size:14px;color:#CFD7EA;">Practice Series</span></div>
    </div>
  </div>
  <div class="chapter-eyebrow">MYP Mathematics 2 · Unit 2</div>
  <div class="chapter-title">Probability</div>
  <div class="chapter-sub">Theory Notes · Objective-wise Practice · Criterion Tests A–D</div><div class="chapter-credit">Follows the unit structure of MYP Mathematics 2 (Oxford), Unit 2</div>
  <div class="chapter-progress">
    <div class="cp-track"><div class="cp-fill" id="cpFill"></div></div>
    <div class="cp-label" id="cpLabel">0% complete</div>
  </div>
  <div class="who-row"><button class="mode-pill" id="modePill" hidden>Choose a mode</button><span class="who" id="whoBar" hidden></span></div>
</header>

<nav class="tabbar"><div class="tabbar-inner" id="tabbar"></div></nav>
<div class="sec-sub" id="secSub"></div><div class="slide-progress"><div class="sp-track"><div class="sp-fill" id="spFill"></div></div><div class="sp-label" id="spLabel">Question 1 of 49</div></div>
<div class="wrap" id="wrap"></div>
<footer class="brandfoot">Brain &amp; Mind Academy · Sopaan Practice Series · MYP Mathematics 2 · Unit 2<br>Unit objectives and structure follow <i>MYP Mathematics 2: A concept-based approach</i> (Oxford University Press), Unit 2. Theory notes, questions, tests and worked solutions are written by Brain &amp; Mind Academy.</footer>

<div class="navbar"><div class="navbar-inner" id="navbarInner"></div></div>
<button class="fab" id="paletteFab"><span>Questions</span><span class="fab-badge" id="fabBadge">0/49</span></button>

<div class="overlay" id="overlay">
  <div class="palette" role="dialog" aria-label="Question palette">
    <div class="palette-head"><h2 id="paletteTitle">Question palette</h2><button class="palette-close" id="paletteClose">&times;</button></div>
    <div class="palette-legend">
      <span><i class="dot current"></i>Current</span><span><i class="dot correct"></i>Correct</span>
      <span><i class="dot revealed"></i>Revealed</span><span><i class="dot skipped"></i>Skipped</span>
      <span><i class="dot locked"></i>Locked</span>
    </div>
    <div class="chip-grid" id="paletteBody"></div>
  </div>
</div>
<div class="toast" id="toast"></div>

<script>
(function(){
"use strict";

/* ================= audio ================= */
var audioCtx=null;
function ensureAudio(){ if(!audioCtx){ try{ audioCtx=new (window.AudioContext||window.webkitAudioContext)(); }catch(e){} } return audioCtx; }
function playTone(freqs,dur,type){
  var ctx=ensureAudio(); if(!ctx) return;
  try{
    var t0=ctx.currentTime;
    freqs.forEach(function(f,i){
      var o=ctx.createOscillator(), g=ctx.createGain();
      o.type=type||'sine'; o.frequency.value=f;
      var start=t0+i*dur;
      g.gain.setValueAtTime(0.0001,start);
      g.gain.exponentialRampToValueAtTime(0.16,start+0.02);
      g.gain.exponentialRampToValueAtTime(0.0001,start+dur);
      o.connect(g); g.connect(ctx.destination);
      o.start(start); o.stop(start+dur+0.02);
    });
  }catch(e){}
}
function playSuccess(){ playTone([523.25,659.25,783.99],0.11,'sine'); }
function playWrong(){ playTone([220,185],0.14,'square'); }
function playReveal(){ playTone([300,220],0.16,'triangle'); }

/* ================= helpers ================= */
function norm(s){
  return String(s===undefined||s===null?'':s).trim().toLowerCase().replace(/\s+/g,'')
    .replace(/[×∗*·]/g,'x').replace(/÷/g,'/').replace(/–|—|−/g,'-')
    .replace(/[²]/g,'^2').replace(/[³]/g,'^3').replace(/[⁴]/g,'^4').replace(/[⁵]/g,'^5')
    .replace(/[⁶]/g,'^6').replace(/[⁷]/g,'^7').replace(/[⁸]/g,'^8').replace(/[⁹]/g,'^9')
    .replace(/\^/g,'');
}
function parseNum(s){
  if(s===undefined||s===null) return null;
  var t=String(s).trim().replace(/,/g,'').replace(/[−–—]/g,'-').replace(/\s+/g,''); if(t==='') return null;
  var neg=false; if(t[0]==='-'){neg=true;t=t.slice(1);}
  var m=t.match(/^(\d+)\/(\d+)$/);
  if(m){ var v=parseInt(m[1],10)/parseInt(m[2],10); return neg?-v:v; }
  if(/^\d+(\.\d+)?$/.test(t)){ var v2=parseFloat(t); return neg?-v2:v2; }
  return null;
}
/* ---- expression checker: compares answers like (P-2w)/2 and P/2-w at random values ---- */
function exprPrep(s){
  return String(s).toLowerCase().replace(/\s+/g,'').replace(/^[a-z]\(x\)=/i,'').replace(/^y=/i,'').replace(/[×∗·]/g,'*').replace(/÷/g,'/').replace(/[−–—]/g,'-')
    .replace(/²/g,'^2').replace(/³/g,'^3').replace(/⁴/g,'^4').replace(/⁵/g,'^5').replace(/⁶/g,'^6').replace(/⁷/g,'^7').replace(/⁸/g,'^8').replace(/⁹/g,'^9').replace(/π/g,'#').replace(/pi/gi,'#').replace(/½/g,'(1/2)');
}
function exprCompile(src){
  var s=exprPrep(src), i=0, toks=[], absDepth=0;
  while(i<s.length){
    var ch=s[i];
    if(/[0-9.]/.test(ch)){ var j=i; while(j<s.length && /[0-9.]/.test(s[j])) j++; toks.push({k:'n',v:parseFloat(s.slice(i,j))}); i=j; continue; }
    if(/[a-zA-Z#]/.test(ch)){ toks.push({k:'v',v:ch}); i++; continue; }
    if(ch==='|'){ var pv=toks[toks.length-1]; var opn=(absDepth===0)||!pv||('+-*/^(['.indexOf(pv.k)>=0); if(opn){ absDepth++; toks.push({k:'['}); } else { absDepth--; toks.push({k:']'}); } i++; continue; }
    if('+-*/^()'.indexOf(ch)>=0){ toks.push({k:ch}); i++; continue; }
    return null;
  }
  var out=[];
  for(var t=0;t<toks.length;t++){
    var a=toks[t], b=out[out.length-1];
    if(b && (b.k==='n'||b.k==='v'||b.k===')'||b.k===']') && (a.k==='n'||a.k==='v'||a.k==='('||a.k==='[')) out.push({k:'&'});
    out.push(a);
  }
  var p=0;
  function peek(){ return out[p]; }
  function parseE(){ var n=parseT(); while(peek() && (peek().k==='+'||peek().k==='-')){ var o=out[p++].k, r=parseT(); n=(function(l,r,o){return function(e){ return o==='+'?l(e)+r(e):l(e)-r(e); };})(n,r,o); } return n; }
  function parseI(){ var n=parseU(); while(peek() && peek().k==='&'){ p++; var r=parseP(); n=(function(l,r){return function(e){ return l(e)*r(e); };})(n,r); } return n; }
  function parseT(){ var n=parseI(); while(peek() && (peek().k==='*'||peek().k==='/')){ var o=out[p++].k, r=parseI(); n=(function(l,r,o){return function(e){ return o==='*'?l(e)*r(e):l(e)/r(e); };})(n,r,o); } return n; }
  function parseU(){ if(peek() && peek().k==='-'){ p++; var u=parseU(); return function(e){ return -u(e); }; } if(peek() && peek().k==='+'){ p++; return parseU(); } return parseP(); }
  function parseP(){ var b=parseA(); if(peek() && peek().k==='^'){ p++; var x=parseU(); return function(e){ return Math.pow(b(e),x(e)); }; } return b; }
  function parseA(){
    var t=out[p++]; if(!t) throw 0;
    if(t.k==='n') return function(){ return t.v; };
    if(t.k==='v') return t.v==='#' ? function(){ return Math.PI; } : function(e){ return e[t.v]; };
    if(t.k==='('){ var n=parseE(); if(!peek()||peek().k!==')') throw 0; p++; return n; }
    if(t.k==='['){ var m=parseE(); if(!peek()||peek().k!==']') throw 0; p++; return function(e){ return Math.abs(m(e)); }; }
    throw 0;
  }
  try{ var f=parseE(); if(p!==out.length) return null; return f; }catch(e){ return null; }
}
function exprEqual(input,answer){
  var f=exprCompile(input), g=exprCompile(answer); if(!f||!g) return false;
  var hits=0;
  for(var trial=0;trial<8;trial++){
    var env={}; 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'.split('').forEach(function(c){ env[c]=1.3+Math.random()*4.1; });
    var x=f(env), y=g(env);
    if(!isFinite(x)||!isFinite(y)) continue;
    if(Math.abs(x-y)>1e-7*Math.max(1,Math.abs(x),Math.abs(y))) return false;
    hits++;
  }
  return hits>=3;
}
function wordsNorm(s){ return String(s||'').toLowerCase().replace(/\band\b/g,' ').replace(/[^a-z]/g,''); }
function listNorm(s){ return (String(s||'').replace(/[−–—]/g,'-').replace(/(\d) (?=\d{3}\b)/g,'$1').match(/-?\d+(\.\d+)?/g)||[]).join(','); }
function powNorm(s){ return String(s||'').toLowerCase().replace(/\s+/g,'').replace(/[×∗*·.]/g,'x').replace(/[⁰¹²³⁴⁵⁶⁷⁸⁹]+/g,function(m){ return '^'+m.split('').map(function(c){ return '⁰¹²³⁴⁵⁶⁷⁸⁹'.indexOf(c); }).join(''); }).replace(/\^1(?!\d)/g,''); }
function fr(s){ return String(s).replace(/\{(\d+) (\d+)\/(\d+)\}/g,'<span class="mx">$1<span class="fq"><span>$2</span><span>$3</span></span></span>').replace(/\{(\d+)\/(\d+)\}/g,'<span class="fq"><span>$1</span><span>$2</span></span>'); }
function gcdI(a,b){ a=Math.abs(a); b=Math.abs(b); while(b){ var t=a%b; a=b; b=t; } return a; }
function fracParse(s){ s=String(s===undefined||s===null?'':s).trim().replace(/[\u2044\u2215\u00f7]/g,'/').replace(/\s+/g,' ').replace(/\s*\/\s*/g,'/'); var m;
  if((m=s.match(/^(-?\d+)$/))) return {v:+m[1],form:'int',n:+m[1],d:1};
  if((m=s.match(/^(-?\d+)\/(\d+)$/))){ if(+m[2]===0) return null; return {v:m[1]/m[2],form:'frac',n:+m[1],d:+m[2]}; }
  if((m=s.match(/^(\d+)(?: |\+|-)(\d+)\/(\d+)$/))){ if(+m[3]===0) return null; return {v:+m[1]+m[2]/m[3],form:'mixed',w:+m[1],n:+m[2],d:+m[3]}; }
  if((m=s.match(/^-?\d*\.\d+$/))) return {v:parseFloat(s),form:'dec'};
  return null; }
function fracOK(mode,input,answer){
  if(mode==='flist'){ var A=String(input).split(/[,;]/).map(function(x){return x.trim();}).filter(Boolean), K=String(answer).split(/[,;]/).map(function(x){return x.trim();});
    if(A.length!==K.length) return false; return A.every(function(x,i){ var a=fracParse(x), k=fracParse(K[i]); return a&&k&&Math.abs(a.v-k.v)<1e-9; }); }
  var a=fracParse(input), k=fracParse(answer); if(!a||!k) return false;
  var same=Math.abs(a.v-k.v)<1e-9; if(!same) return false;
  if(mode==='fv') return true;
  if(mode==='dec') return a.form==='dec'||a.form==='int';
  if(mode==='fe') return (a.form==='frac'&&k.form==='frac'&&a.n===k.n&&a.d===k.d)||(a.form==='int'&&k.form==='int');
  if(mode==='fi') return a.form==='frac'||(a.form==='int'&&k.form==='int');
  if(mode==='fl') return a.form==='int'||(a.form==='frac'&&gcdI(a.n,a.d)===1)||(a.form==='mixed'&&a.n<a.d&&gcdI(a.n,a.d)===1);
  if(mode==='fm') return a.form==='int'||(a.form==='mixed'&&a.n>0&&a.n<a.d&&gcdI(a.n,a.d)===1);
  return false; }
function answerMatches(input,answer,accept,expr){
  if(expr==='dec'||expr==='fv'||expr==='fe'||expr==='fl'||expr==='fm'||expr==='fi'||expr==='flist'){ if(input===undefined||input===null||String(input).trim()==='') return false; return fracOK(expr,input,answer); }
  if(input===undefined||input===null||String(input).trim()==='') return false;
  if(expr==='words'){ return [answer].concat(accept||[]).some(function(a){ if(/\d/.test(a)) return norm(a)===norm(input); var w=wordsNorm(a); return w!=='' && w===wordsNorm(input); }); }
  if(expr==='list'){ return [answer].concat(accept||[]).some(function(a){ return listNorm(a)===listNorm(input); }); }
  if(expr==='pow'){ return [answer].concat(accept||[]).some(function(a){ return powNorm(a)===powNorm(input); }); }
  if(expr==='time'){ var tn=function(x){ var t=String(x).toLowerCase().replace(/[\s.]/g,'').replace(/[:h]/g,''); if(/^\d{3}(am|pm)?$/.test(t)) t='0'+t; return t; }; return [answer].concat(accept||[]).some(function(a){ return tn(a)===tn(input); }); }
  if(expr==='glist'){ var gn=function(x){ return String(x).toUpperCase().split(/[\s,;]+/).filter(Boolean).sort().join(','); }; return [answer].concat(accept||[]).some(function(a){ return gn(a)===gn(input); }); }
  if(expr==='coord'){ var cn=function(x){ return String(x).replace(/[\u2212\u2013\u2014]/g,'-').replace(/[\s()\[\]]/g,''); }; return [answer].concat(accept||[]).some(function(a){ return cn(a)===cn(input); }); }
  if(expr==='dlist'){ var dn=function(x){ return (String(x).replace(/[−–—]/g,'-').match(/-?\d*\.?\d+/g)||[]).map(Number).join(','); }; return [answer].concat(accept||[]).some(function(a){ return dn(a)===dn(input); }); }
  if(expr==='set'){ var sn=function(x){ return (String(x).match(/-?\d+/g)||[]).map(Number).sort(function(a,b){return a-b;}).join(','); }; return [answer].concat(accept||[]).some(function(a){ return sn(a)===sn(input); }); }
  if(expr==='primes'){ var pn=function(x){ var t=powNorm(x).replace(/[^0-9x^]/g,''); var out=[]; t.split('x').forEach(function(tok){ if(!tok) return; var m=tok.split('^'); var b=+m[0], e=m[1]?+m[1]:1; for(var i=0;i<e;i++) out.push(b); }); return out.sort(function(a,b){return a-b;}).join(','); }; return [answer].concat(accept||[]).some(function(a){ return pn(a)===pn(input); }); }
  if(expr==='angle'){ var an=function(x){ return String(x).toUpperCase().replace(/[^A-Z]/g,''); }; var k=an(answer), v=an(input); return v===k || v===k.split('').reverse().join(''); }
  if(expr==='trans'){ var tv=function(x){ var s=String(x).toLowerCase().replace(/[−–—]/g,'-').replace(/\b(units?|squares?|and|then|steps?|to|the|a)\b/g,' ').replace(/[,;.]/g,' '); var re=/(\d+)\s*(right|left|up|down|r|l|u|d)\b/g, m, dx=0, dy=0, n=0; while((m=re.exec(s))){ var v=+m[1], d=m[2][0]; if(d==='r')dx+=v; else if(d==='l')dx-=v; else if(d==='u')dy+=v; else dy-=v; n++; } if(!n||s.replace(re,'').replace(/\s+/g,'')!=='') return null; return dx+','+dy; }; var ti=tv(input); return ti!==null && [answer].concat(accept||[]).some(function(a){ return tv(a)===ti; }); }
  if(expr==='rot'){ var rv=function(x){ var s=String(x).toLowerCase().replace(/quarter[\s-]*turn/g,'90').replace(/half[\s-]*turn/g,'180').replace(/three[\s-]*quarter[\s-]*turn/g,'270'); var m=s.match(/\b(90|180|270)\b/); if(!m) return null; var a=+m[1]; if(a===180) return '180'; var dir=/anti|counter|acw|ccw/.test(s)?-1:(/clockwise|\bcw\b/.test(s)?1:0); if(!dir) return null; return String(((a*dir)%360+360)%360); }; var ri=rv(input); return ri!==null && ri===rv(answer); }
  if(expr==='expanded'){ var f=function(x){ return String(x).replace(/\s+/g,'').replace(/,/g,''); }; return [answer].concat(accept||[]).some(function(a){ return f(a)===f(input); }); }
  if(expr===true && input!==undefined && input!==null && String(input).trim()!==''){
    if(exprEqual(input,answer)) return true;
    var alt=String(input).replace(/\/\s*(\d+(?:\.\d+)?)\s*([a-zA-Z(])/g,'/$1*$2'); if(alt!==String(input) && exprEqual(alt,answer)) return true;
    return (accept||[]).some(function(a){ return exprEqual(input,a); });
  }
  if(input===undefined||input===null||String(input).trim()==='') return false;
  var n1=parseNum(input), n2=parseNum(answer);
  if(n1!==null && n2!==null){ if(Math.abs(n1-n2)<1e-3) return true; return (accept||[]).some(function(a){ var n3=parseNum(a); return n3!==null && Math.abs(n1-n3)<1e-3; }); }
  var cands=[answer].concat(accept||[]); var ni=norm(input);
  for(var i=0;i<cands.length;i++){ if(norm(cands[i])===ni) return true; }
  return false;
}
function esc(s){ return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;'); }

/* ================= DATA ================= */
var AR_OPTIONS = ["Both A and R are true, and R is the correct explanation of A","Both A and R are true, but R is NOT the correct explanation of A","A is true but R is false","A is false but R is true"];
var THEORY = "<div class=\"hub\"><div class=\"hub-card\"><h3>📖 Theory notes</h3><p>Explained notes for each part of the unit, with rules, diagrams and worked examples.</p><div class=\"hub-btns\"><button class=\"hub-btn\" data-jump=\"n21\">2.1 notes</button><button class=\"hub-btn\" data-jump=\"n22\">2.2 notes</button><button class=\"hub-btn\" data-jump=\"n23\">2.3 notes</button><button class=\"hub-btn\" data-jump=\"n24\">2.4 notes</button><button class=\"hub-btn\" data-jump=\"n25\">2.5 notes</button></div></div><div class=\"hub-card\"><h3>🎯 Objective-wise practice</h3><p>One practice sheet for each unit objective: multiple-choice questions first, then step-by-step fill-in-the-blanks.</p><div class=\"hub-btns\"><button class=\"hub-btn\" data-go=\"s1\">2.1 · Events and sample spaces</button><button class=\"hub-btn\" data-go=\"s2\">2.2 · Representing likelihood</button><button class=\"hub-btn\" data-go=\"s3\">2.3 · Theoretical probability</button><button class=\"hub-btn\" data-go=\"s4\">2.4 · Complementary events</button><button class=\"hub-btn\" data-go=\"s5\">2.5 · Experimental probability and simulations</button></div></div><div class=\"hub-card\"><h3>📝 Chapter test</h3><p>Four tests, one for each MYP criterion. Take them in Quiz mode, then open the report to see your results as pie charts.</p><div class=\"hub-btns\"><button class=\"hub-btn\" data-go=\"s6\">Test A · Knowing and understanding</button><button class=\"hub-btn\" data-go=\"s7\">Test B · Investigating patterns</button><button class=\"hub-btn\" data-go=\"s8\">Test C · Communicating</button><button class=\"hub-btn\" data-go=\"s9\">Test D · Applying mathematics in real-life contexts</button><button class=\"hub-btn primary\" data-go=\"report\">📊 View my report</button></div></div></div><section class=\"note\" id=\"nintro\"><h2>About this unit</h2><p><b>Key concept:</b> Logic &nbsp;·&nbsp; <b>Related concepts:</b> Representation, Systems, Justification &nbsp;·&nbsp; <b>Global context:</b> Personal and cultural expression (games and play).</p><p>Every culture plays games: ludo, carrom, kabaddi, snakes and ladders, card games, cricket tosses. Many games depend on chance. Probability is the logical system that lets us describe how likely something is, compare choices and decide whether a game is fair or whether winning is just luck.</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Test</th><th>MYP criterion</th><th>What it checks</th></tr><tr><td>Test A</td><td>Knowing and understanding</td><td>Using the skills of the unit correctly in familiar and unfamiliar questions.</td></tr><tr><td>Test B</td><td>Investigating patterns</td><td>Spotting patterns in sample spaces and probabilities, describing them as rules and testing the rules.</td></tr><tr><td>Test C</td><td>Communicating</td><td>Correct notation and vocabulary, clear working and choosing the clearest representation.</td></tr><tr><td>Test D</td><td>Applying mathematics in real-life contexts</td><td>Using probability to make and justify decisions in games and everyday situations.</td></tr></table></div></section><section class=\"note\" id=\"n21\"><h2>2.1 Events and sample spaces</h2><p class=\"lt\"><b>Objective:</b> Identify events and outcomes, and represent a sample space with an organised list, a table and a tree diagram.</p><h4>Key words</h4><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Word</th><th>Meaning</th><th>Example (rolling a six-sided die)</th></tr><tr><td><b>Trial</b></td><td>one go of an experiment</td><td>one roll of the die</td></tr><tr><td><b>Outcome</b></td><td>one possible result of a trial</td><td>rolling a 4</td></tr><tr><td><b>Sample space</b></td><td>the set of <b>all</b> possible outcomes</td><td>{1, 2, 3, 4, 5, 6}</td></tr><tr><td><b>Event</b></td><td>one or more outcomes we are interested in</td><td>“rolling an even number” = {2, 4, 6}</td></tr><tr><td><b>Compound event</b></td><td>two or more things happening together</td><td>rolling a die <i>and</i> tossing a coin</td></tr></table></div><h4>Three ways to show a sample space</h4><p><b>1 · Organised list.</b> Write outcomes in a fixed order so none are missed. A coin and a spinner labelled 1, 2, 3: H1, H2, H3, T1, T2, T3.</p><p><b>2 · Tree diagram.</b> Each stage branches from the one before. Read along each path to get one outcome. The number of end branches is the number of outcomes.</p><svg class=\"figsvg\" viewBox=\"0 0 260 140\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><circle cx=\"16\" cy=\"77.0\" r=\"3\" style=\"fill:var(--ink)\"/><text class=\"po\" x=\"84.3\" y=\"12.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">coin</text><text class=\"po\" x=\"152.6\" y=\"12.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">spinner</text><text class=\"po\" x=\"220.8\" y=\"12.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">outcome</text><line x1=\"16.0\" y1=\"77.0\" x2=\"73.3\" y2=\"48.5\" style=\"stroke:var(--ink-soft);stroke-width:1.3\"/><text class=\"lb\" x=\"84.3\" y=\"48.5\" text-anchor=\"middle\" dominant-baseline=\"middle\">H</text><line x1=\"93.3\" y1=\"48.5\" x2=\"141.6\" y2=\"29.5\" style=\"stroke:var(--ink-soft);stroke-width:1.3\"/><text class=\"lb\" x=\"152.6\" y=\"29.5\" text-anchor=\"middle\" dominant-baseline=\"middle\">1</text><text class=\"al\" x=\"195.7\" y=\"29.5\" text-anchor=\"start\" dominant-baseline=\"middle\">H1</text><line x1=\"93.3\" y1=\"48.5\" x2=\"141.6\" y2=\"48.5\" style=\"stroke:var(--ink-soft);stroke-width:1.3\"/><text class=\"lb\" x=\"152.6\" y=\"48.5\" text-anchor=\"middle\" dominant-baseline=\"middle\">2</text><text class=\"al\" x=\"195.7\" y=\"48.5\" text-anchor=\"start\" dominant-baseline=\"middle\">H2</text><line x1=\"93.3\" y1=\"48.5\" x2=\"141.6\" y2=\"67.5\" style=\"stroke:var(--ink-soft);stroke-width:1.3\"/><text class=\"lb\" x=\"152.6\" y=\"67.5\" text-anchor=\"middle\" dominant-baseline=\"middle\">3</text><text class=\"al\" x=\"195.7\" y=\"67.5\" text-anchor=\"start\" dominant-baseline=\"middle\">H3</text><line x1=\"16.0\" y1=\"77.0\" x2=\"73.3\" y2=\"105.5\" style=\"stroke:var(--ink-soft);stroke-width:1.3\"/><text class=\"lb\" x=\"84.3\" y=\"105.5\" text-anchor=\"middle\" dominant-baseline=\"middle\">T</text><line x1=\"93.3\" y1=\"105.5\" x2=\"141.6\" y2=\"86.5\" style=\"stroke:var(--ink-soft);stroke-width:1.3\"/><text class=\"lb\" x=\"152.6\" y=\"86.5\" text-anchor=\"middle\" dominant-baseline=\"middle\">1</text><text class=\"al\" x=\"195.7\" y=\"86.5\" text-anchor=\"start\" dominant-baseline=\"middle\">T1</text><line x1=\"93.3\" y1=\"105.5\" x2=\"141.6\" y2=\"105.5\" style=\"stroke:var(--ink-soft);stroke-width:1.3\"/><text class=\"lb\" x=\"152.6\" y=\"105.5\" text-anchor=\"middle\" dominant-baseline=\"middle\">2</text><text class=\"al\" x=\"195.7\" y=\"105.5\" text-anchor=\"start\" dominant-baseline=\"middle\">T2</text><line x1=\"93.3\" y1=\"105.5\" x2=\"141.6\" y2=\"124.5\" style=\"stroke:var(--ink-soft);stroke-width:1.3\"/><text class=\"lb\" x=\"152.6\" y=\"124.5\" text-anchor=\"middle\" dominant-baseline=\"middle\">3</text><text class=\"al\" x=\"195.7\" y=\"124.5\" text-anchor=\"start\" dominant-baseline=\"middle\">T3</text></svg><p><b>3 · Table (two-way grid).</b> Best for exactly two stages, especially when there are many outcomes. The table below shows the totals for two six-sided dice: 6 × 6 = 36 cells, one for each outcome.</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>+</th><th>1</th><th>2</th><th>3</th><th>4</th><th>5</th><th>6</th></tr><tr><th>1</th><td class=\"mono\">2</td><td class=\"mono\">3</td><td class=\"mono\">4</td><td class=\"mono\">5</td><td class=\"mono\">6</td><td class=\"mono\">7</td></tr><tr><th>2</th><td class=\"mono\">3</td><td class=\"mono\">4</td><td class=\"mono\">5</td><td class=\"mono\">6</td><td class=\"mono\">7</td><td class=\"mono\">8</td></tr><tr><th>3</th><td class=\"mono\">4</td><td class=\"mono\">5</td><td class=\"mono\">6</td><td class=\"mono\">7</td><td class=\"mono\">8</td><td class=\"mono\">9</td></tr><tr><th>4</th><td class=\"mono\">5</td><td class=\"mono\">6</td><td class=\"mono\">7</td><td class=\"mono\">8</td><td class=\"mono\">9</td><td class=\"mono\">10</td></tr><tr><th>5</th><td class=\"mono\">6</td><td class=\"mono\">7</td><td class=\"mono\">8</td><td class=\"mono\">9</td><td class=\"mono\">10</td><td class=\"mono\">11</td></tr><tr><th>6</th><td class=\"mono\">7</td><td class=\"mono\">8</td><td class=\"mono\">9</td><td class=\"mono\">10</td><td class=\"mono\">11</td><td class=\"mono\">12</td></tr></table></div><div class=\"keybox\"><b>Order can matter.</b> With two coins, “head then tail” (HT) and “tail then head” (TH) are different outcomes. The sample space is HH, HT, TH, TT: <b>4</b> outcomes, not 3.</div><h4>The counting principle</h4><p>If one stage has <i>m</i> outcomes and a second stage has <i>n</i> outcomes, the compound event has <b>m × n</b> outcomes. This extends to more stages: 3 stages with 2, 4 and 3 choices give 2 × 4 × 3 = 24 outcomes.</p><div class=\"ex\"><div class=\"exh\">Worked example 1 · Organised list</div><div class=\"exl\">A thali comes with one sabzi (paneer, aloo, bhindi) and one bread (roti, naan).<br>List by sabzi: paneer-roti, paneer-naan, aloo-roti, aloo-naan, bhindi-roti, bhindi-naan.<br>There are <b>3 × 2 = 6</b> possible thalis.</div></div><div class=\"ex\"><div class=\"exh\">Worked example 2 · Tree diagram for three stages</div><div class=\"exl\">A team plays three matches; each is a win (W) or loss (L).<br>The tree has 2 branches, then 2 from each, then 2 from each: 2 × 2 × 2 = <b>8</b> outcomes.<br>WWW, WWL, WLW, WLL, LWW, LWL, LLW, LLL. Exactly two wins: WWL, WLW, LWW (3 outcomes).</div></div><div class=\"ex\"><div class=\"exh\">Worked example 3 · Choosing the representation</div><div class=\"exl\">Two four-sided dice are rolled and the numbers multiplied.<br>Two stages with 4 × 4 = 16 outcomes: a <b>table</b> is clearest, with one die along the top and one down the side.<br>A tree would need 16 end branches, which is harder to read.</div></div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s1\">Practise 2.1 →</button></div></section><section class=\"note\" id=\"n22\"><h2>2.2 Representing likelihood</h2><p class=\"lt\"><b>Objective:</b> Describe the likelihood of an event in words and on the probability scale, and write a probability as a fraction, decimal and percentage.</p><p>The <b>probability</b> of an event is a number from <b>0 to 1</b> that describes how likely it is to happen. We write P(event), for example P(rain) = 0.3.</p><svg class=\"figsvg\" viewBox=\"0 0 330 92\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><rect x=\"22\" y=\"42\" width=\"286\" height=\"8\" rx=\"4\" style=\"fill:var(--gold-soft);stroke:var(--gold)\"/><line x1=\"22.0\" y1=\"37\" x2=\"22.0\" y2=\"55\" style=\"stroke:var(--ink);stroke-width:1.4\"/><text class=\"lb\" x=\"22.0\" y=\"64.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">0</text><text class=\"po\" x=\"22.0\" y=\"80.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">Impossible</text><line x1=\"93.5\" y1=\"37\" x2=\"93.5\" y2=\"55\" style=\"stroke:var(--ink);stroke-width:1.4\"/><text class=\"lb\" x=\"93.5\" y=\"64.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">¼</text><text class=\"po\" x=\"93.5\" y=\"80.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">Unlikely</text><line x1=\"165.0\" y1=\"37\" x2=\"165.0\" y2=\"55\" style=\"stroke:var(--ink);stroke-width:1.4\"/><text class=\"lb\" x=\"165.0\" y=\"64.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">½</text><text class=\"po\" x=\"165.0\" y=\"80.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">Even chance</text><line x1=\"236.5\" y1=\"37\" x2=\"236.5\" y2=\"55\" style=\"stroke:var(--ink);stroke-width:1.4\"/><text class=\"lb\" x=\"236.5\" y=\"64.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">¾</text><text class=\"po\" x=\"236.5\" y=\"80.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">Likely</text><line x1=\"308.0\" y1=\"37\" x2=\"308.0\" y2=\"55\" style=\"stroke:var(--ink);stroke-width:1.4\"/><text class=\"lb\" x=\"308.0\" y=\"64.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">1</text><text class=\"po\" x=\"308.0\" y=\"80.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">Certain</text><path d=\"M50.6,39 l-5,-9 h10 z\" style=\"fill:var(--danger)\"/><text class=\"al\" x=\"50.6\" y=\"22.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">A</text><path d=\"M165.0,39 l-5,-9 h10 z\" style=\"fill:var(--danger)\"/><text class=\"al\" x=\"165.0\" y=\"22.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">B</text><path d=\"M265.1,39 l-5,-9 h10 z\" style=\"fill:var(--danger)\"/><text class=\"al\" x=\"265.1\" y=\"22.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">C</text></svg><p>On the scale above, A (0.1) is unlikely, B (0.5) is an even chance and C (0.85) is likely.</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Word</th><th>Probability</th><th>Example</th></tr><tr><td>Impossible</td><td class=\"mono\">0</td><td>rolling a 7 on a six-sided die</td></tr><tr><td>Unlikely</td><td>between 0 and ½</td><td>rolling a 6</td></tr><tr><td>Equally likely as unlikely (even chance)</td><td class=\"mono\">½ = 0.5 = 50%</td><td>a fair coin landing heads</td></tr><tr><td>Likely</td><td>between ½ and 1</td><td>rolling a number greater than 1</td></tr><tr><td>Certain</td><td class=\"mono\">1</td><td>rolling a number less than 7</td></tr></table></div><h4>Fraction, decimal, percentage</h4><p>A probability can be written in any of the three forms. They are the same number.</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Fraction</th><th>Decimal</th><th>Percentage</th><th>In words</th></tr><tr><td><span class=\"fq\"><span>1</span><span>4</span></span></td><td class=\"mono\">0.25</td><td class=\"mono\">25%</td><td>“1 in 4” chance</td></tr><tr><td><span class=\"fq\"><span>3</span><span>5</span></span></td><td class=\"mono\">0.6</td><td class=\"mono\">60%</td><td>likely</td></tr><tr><td><span class=\"fq\"><span>7</span><span>8</span></span></td><td class=\"mono\">0.875</td><td class=\"mono\">87.5%</td><td>very likely</td></tr></table></div><div class=\"keybox\"><b>A probability can never be negative or greater than 1 (100%).</b> If you get P = 1.3 or P = <span class=\"fq\"><span>9</span><span>7</span></span>, there is a mistake in your working.</div><div class=\"ex\"><div class=\"exh\">Worked example 1 · Converting</div><div class=\"exl\">Write P = <span class=\"fq\"><span>9</span><span>20</span></span> as a decimal and a percentage.<br>9 ÷ 20 = <b>0.45</b>; 0.45 × 100 = <b>45%</b>.<br>0.45 is a little less than 0.5, so the event is (slightly) unlikely.</div></div><div class=\"ex\"><div class=\"exh\">Worked example 2 · Percentage to fraction</div><div class=\"exl\">A forecast gives a 35% chance of rain.<br>35% = <span class=\"fq\"><span>35</span><span>100</span></span> = <b><span class=\"fq\"><span>7</span><span>20</span></span></b> (÷ 5) = 0.35.</div></div><div class=\"ex\"><div class=\"exh\">Worked example 3 · Comparing</div><div class=\"exl\">Which is most likely: A with P = <span class=\"fq\"><span>2</span><span>3</span></span>, B with P = 0.62, C with P = 65%?<br>As decimals: A = 0.666…, B = 0.62, C = 0.65.<br>Order from least to most likely: B, C, A. <b>A is most likely.</b></div></div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s2\">Practise 2.2 →</button></div></section><section class=\"note\" id=\"n23\"><h2>2.3 Theoretical probability</h2><p class=\"lt\"><b>Objective:</b> Calculate the theoretical probability of single and compound events with equally likely outcomes.</p><p>When all the outcomes in the sample space are <b>equally likely</b> (a fair coin, a fair die, a spinner with equal sectors, a name drawn at random), the <b>theoretical probability</b> of an event is</p><p style=\"text-align:center\"><b>P(event) = <span class=\"fq\"><span>number of favourable outcomes</span><span>total number of outcomes</span></span></b></p><p>A <b>favourable</b> outcome is one that makes the event happen. Always simplify the fraction.</p><svg class=\"figsvg\" style=\"max-width:220px\" viewBox=\"0 0 170 142\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><path d=\"M85.0,72.0 L85.0,8.0 A64,64 0 0 1 130.3,26.7 Z\" style=\"fill:var(--gold-soft);stroke:var(--ink);stroke-width:1.3\"/><text class=\"lb\" x=\"100.7\" y=\"34.2\" text-anchor=\"middle\" dominant-baseline=\"middle\">1</text><path d=\"M85.0,72.0 L130.3,26.7 A64,64 0 0 1 149.0,72.0 Z\" style=\"fill:var(--paper-2);stroke:var(--ink);stroke-width:1.3\"/><text class=\"lb\" x=\"122.8\" y=\"56.3\" text-anchor=\"middle\" dominant-baseline=\"middle\">2</text><path d=\"M85.0,72.0 L149.0,72.0 A64,64 0 0 1 130.3,117.3 Z\" style=\"fill:var(--card);stroke:var(--ink);stroke-width:1.3\"/><text class=\"lb\" x=\"122.8\" y=\"87.7\" text-anchor=\"middle\" dominant-baseline=\"middle\">3</text><path d=\"M85.0,72.0 L130.3,117.3 A64,64 0 0 1 85.0,136.0 Z\" style=\"fill:var(--gold-soft);stroke:var(--ink);stroke-width:1.3\"/><text class=\"lb\" x=\"100.7\" y=\"109.8\" text-anchor=\"middle\" dominant-baseline=\"middle\">4</text><path d=\"M85.0,72.0 L85.0,136.0 A64,64 0 0 1 39.7,117.3 Z\" style=\"fill:var(--paper-2);stroke:var(--ink);stroke-width:1.3\"/><text class=\"lb\" x=\"69.3\" y=\"109.8\" text-anchor=\"middle\" dominant-baseline=\"middle\">5</text><path d=\"M85.0,72.0 L39.7,117.3 A64,64 0 0 1 21.0,72.0 Z\" style=\"fill:var(--card);stroke:var(--ink);stroke-width:1.3\"/><text class=\"lb\" x=\"47.2\" y=\"87.7\" text-anchor=\"middle\" dominant-baseline=\"middle\">6</text><path d=\"M85.0,72.0 L21.0,72.0 A64,64 0 0 1 39.7,26.7 Z\" style=\"fill:var(--gold-soft);stroke:var(--ink);stroke-width:1.3\"/><text class=\"lb\" x=\"47.2\" y=\"56.3\" text-anchor=\"middle\" dominant-baseline=\"middle\">7</text><path d=\"M85.0,72.0 L39.7,26.7 A64,64 0 0 1 85.0,8.0 Z\" style=\"fill:var(--paper-2);stroke:var(--ink);stroke-width:1.3\"/><text class=\"lb\" x=\"69.3\" y=\"34.2\" text-anchor=\"middle\" dominant-baseline=\"middle\">8</text><line class=\"ln\" x1=\"85.0\" y1=\"72.0\" x2=\"97.6\" y2=\"48.3\" marker-end=\"url(#ah)\"/><circle cx=\"85.0\" cy=\"72.0\" r=\"3.5\" style=\"fill:var(--ink)\"/></svg><div class=\"ex\"><div class=\"exh\">Worked example 1 · Spinner</div><div class=\"exl\">The spinner has 8 equal sectors numbered 1 to 8. Find P(prime).<br>Primes on the spinner: 2, 3, 5, 7, so 4 favourable outcomes out of 8.<br>P(prime) = <span class=\"fq\"><span>4</span><span>8</span></span> = <b><span class=\"fq\"><span>1</span><span>2</span></span></b>.</div></div><div class=\"ex\"><div class=\"exh\">Worked example 2 · Names from a bag</div><div class=\"exl\">A bag holds tokens with the names Aarav, Ishaan, Kabir, Diya, Meera, Anaya, Sara and Zoya. One is drawn at random.<br>P(name starts with a vowel) = Aarav, Ishaan, Anaya = <b><span class=\"fq\"><span>3</span><span>8</span></span></b>.<br>P(name has exactly 4 letters) = Diya, Sara, Zoya = <b><span class=\"fq\"><span>3</span><span>8</span></span></b>.</div></div><h4>Compound events</h4><p>For two or more stages, first draw the whole sample space (list, tree or table), then count the favourable outcomes.</p><div class=\"ex\"><div class=\"exh\">Worked example 3 · Two dice</div><div class=\"exl\">Two fair six-sided dice are rolled. Find P(total = 9).<br>From the table, the cells with total 9 are (3, 6), (4, 5), (5, 4), (6, 3): 4 of 36.<br>P(total 9) = <span class=\"fq\"><span>4</span><span>36</span></span> = <b><span class=\"fq\"><span>1</span><span>9</span></span></b>.</div></div><div class=\"ex\"><div class=\"exh\">Worked example 4 · Coin and spinner</div><div class=\"exl\">A coin is tossed and the 1-2-3 spinner is spun (6 outcomes from the tree in 2.1).<br>P(head and an odd number) = H1, H3 = <span class=\"fq\"><span>2</span><span>6</span></span> = <b><span class=\"fq\"><span>1</span><span>3</span></span></b>.</div></div><div class=\"keybox\"><b>Only use the formula when outcomes are equally likely.</b> Totals of two dice (2 to 12) are <i>not</i> equally likely: a total of 7 can happen 6 ways but a total of 2 only 1 way. Count the 36 cells, not the 11 totals.</div><h4>Fair games</h4><p>A game is <b>fair</b> if every player has the same probability of winning. “Player A wins on an even number, B on an odd number” with one die is fair: each has P = <span class=\"fq\"><span>1</span><span>2</span></span>.</p><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s3\">Practise 2.3 →</button></div></section><section class=\"note\" id=\"n24\"><h2>2.4 Complementary events</h2><p class=\"lt\"><b>Objective:</b> Recognise complementary events and use P(not A) = 1 − P(A).</p><p>The <b>complement</b> of an event A is the event “A does <b>not</b> happen”, written <span class=\"mono\">not A</span> (or A′). An event and its complement do not overlap and together they cover the whole sample space, so</p><p style=\"text-align:center\"><b>P(A) + P(not A) = 1 &nbsp; so &nbsp; P(not A) = 1 − P(A)</b></p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Event A</th><th>Complement (not A)</th><th>Check</th></tr><tr><td>rolling a 6</td><td>rolling 1, 2, 3, 4 or 5</td><td><span class=\"fq\"><span>1</span><span>6</span></span> + <span class=\"fq\"><span>5</span><span>6</span></span> = 1</td></tr><tr><td>even number</td><td>odd number</td><td><span class=\"fq\"><span>1</span><span>2</span></span> + <span class=\"fq\"><span>1</span><span>2</span></span> = 1</td></tr><tr><td>P(rain) = 0.35</td><td>P(no rain) = 0.65</td><td>0.35 + 0.65 = 1</td></tr><tr><td>winning 12%</td><td>not winning 88%</td><td>12% + 88% = 100%</td></tr></table></div><div class=\"keybox\"><b>Not every pair of events is complementary.</b> “Rolling a number less than 3” and “rolling a number greater than 3” miss the outcome 3. “Prime” and “composite” miss 1. Check that <i>every</i> outcome is in exactly one of the two events.</div><div class=\"ex\"><div class=\"exh\">Worked example 1 · Using the rule</div><div class=\"exl\">The probability that a train is late is <span class=\"fq\"><span>2</span><span>15</span></span>. Find the probability it is not late.<br>P(not late) = 1 − <span class=\"fq\"><span>2</span><span>15</span></span> = <b><span class=\"fq\"><span>13</span><span>15</span></span></b>.</div></div><div class=\"ex\"><div class=\"exh\">Worked example 2 · “At least one”</div><div class=\"exl\">Three fair coins are tossed. Find P(at least one head).<br>The only outcome with no heads is TTT: P(TTT) = <span class=\"fq\"><span>1</span><span>8</span></span>.<br>P(at least one head) = 1 − <span class=\"fq\"><span>1</span><span>8</span></span> = <b><span class=\"fq\"><span>7</span><span>8</span></span></b>. This is much faster than counting the 7 other outcomes.</div></div><div class=\"ex\"><div class=\"exh\">Worked example 3 · Missing probability</div><div class=\"exl\">A bag has only red, blue and green counters. P(red) = 0.25 and P(blue) = 0.4.<br>P(green) = 1 − (0.25 + 0.4) = <b>0.35</b>.<br>If the bag has 40 counters, 0.35 × 40 = <b>14</b> are green.</div></div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s4\">Practise 2.4 →</button></div></section><section class=\"note\" id=\"n25\"><h2>2.5 Experimental probability and simulations</h2><p class=\"lt\"><b>Objective:</b> Calculate experimental probability from data, design simulations, compare results with theory and make predictions.</p><p><b>Theoretical probability</b> is what we <i>expect</i> from the sample space. <b>Experimental probability</b> (relative frequency) is what actually happened in trials:</p><p style=\"text-align:center\"><b>Experimental P(event) = <span class=\"fq\"><span>number of times the event happened</span><span>total number of trials</span></span></b></p><h4>More trials, better estimate</h4><p>With few trials the experimental probability can be far from the theoretical value. As the number of trials grows, it usually settles closer and closer to the theoretical probability. The graph shows one class tossing a coin.</p><svg class=\"figsvg\" viewBox=\"0 0 320 200\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><line x1=\"38\" y1=\"166.0\" x2=\"306\" y2=\"166.0\" style=\"stroke:var(--rule)\"/><text class=\"po\" x=\"33.0\" y=\"166.0\" text-anchor=\"end\" dominant-baseline=\"middle\">0.3</text><line x1=\"38\" y1=\"137.2\" x2=\"306\" y2=\"137.2\" style=\"stroke:var(--rule)\"/><text class=\"po\" x=\"33.0\" y=\"137.2\" text-anchor=\"end\" dominant-baseline=\"middle\">0.4</text><line x1=\"38\" y1=\"108.4\" x2=\"306\" y2=\"108.4\" style=\"stroke:var(--rule)\"/><text class=\"po\" x=\"33.0\" y=\"108.4\" text-anchor=\"end\" dominant-baseline=\"middle\">0.5</text><line x1=\"38\" y1=\"79.6\" x2=\"306\" y2=\"79.6\" style=\"stroke:var(--rule)\"/><text class=\"po\" x=\"33.0\" y=\"79.6\" text-anchor=\"end\" dominant-baseline=\"middle\">0.6</text><line x1=\"38\" y1=\"50.8\" x2=\"306\" y2=\"50.8\" style=\"stroke:var(--rule)\"/><text class=\"po\" x=\"33.0\" y=\"50.8\" text-anchor=\"end\" dominant-baseline=\"middle\">0.7</text><line x1=\"38\" y1=\"22.0\" x2=\"306\" y2=\"22.0\" style=\"stroke:var(--rule)\"/><text class=\"po\" x=\"33.0\" y=\"22.0\" text-anchor=\"end\" dominant-baseline=\"middle\">0.8</text><line x1=\"38\" y1=\"108.4\" x2=\"306\" y2=\"108.4\" style=\"stroke:var(--danger);stroke-width:1.6;stroke-dasharray:6 4\"/><polyline points=\"38.0,50.8 82.7,79.6 127.3,91.1 172.0,117.0 216.7,99.8 261.3,106.1 306.0,109.3\" style=\"fill:none;stroke:var(--accent-text);stroke-width:2\"/><circle cx=\"38.0\" cy=\"50.8\" r=\"3.4\" style=\"fill:var(--accent-text)\"/><text class=\"po\" x=\"38.0\" y=\"177.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">10</text><circle cx=\"82.7\" cy=\"79.6\" r=\"3.4\" style=\"fill:var(--accent-text)\"/><text class=\"po\" x=\"82.7\" y=\"177.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">20</text><circle cx=\"127.3\" cy=\"91.1\" r=\"3.4\" style=\"fill:var(--accent-text)\"/><text class=\"po\" x=\"127.3\" y=\"177.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">50</text><circle cx=\"172.0\" cy=\"117.0\" r=\"3.4\" style=\"fill:var(--accent-text)\"/><text class=\"po\" x=\"172.0\" y=\"177.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">100</text><circle cx=\"216.7\" cy=\"99.8\" r=\"3.4\" style=\"fill:var(--accent-text)\"/><text class=\"po\" x=\"216.7\" y=\"177.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">200</text><circle cx=\"261.3\" cy=\"106.1\" r=\"3.4\" style=\"fill:var(--accent-text)\"/><text class=\"po\" x=\"261.3\" y=\"177.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">500</text><circle cx=\"306.0\" cy=\"109.3\" r=\"3.4\" style=\"fill:var(--accent-text)\"/><text class=\"po\" x=\"306.0\" y=\"177.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">1000</text><text class=\"po\" x=\"172.0\" y=\"191.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">number of tosses</text><text class=\"po\" x=\"8.0\" y=\"8.0\" text-anchor=\"start\" dominant-baseline=\"middle\">relative frequency of heads</text><text class=\"al\" x=\"78.0\" y=\"119.4\" text-anchor=\"start\" dominant-baseline=\"middle\">theory 0.5</text></svg><div class=\"ex\"><div class=\"exh\">Worked example 1 · From a frequency table</div><div class=\"exl\">A spinner with 4 equal colours was spun 200 times: red 46, blue 58, green 49, yellow 47.<br>Experimental P(blue) = <span class=\"fq\"><span>58</span><span>200</span></span> = <b>0.29</b>. Theoretical P(blue) = <span class=\"fq\"><span>1</span><span>4</span></span> = 0.25.<br>0.29 is fairly close to 0.25; with more spins we would expect it to get closer. It is not strong evidence that the spinner is biased.</div></div><h4>Using probability to predict</h4><p><b>Expected number</b> = probability × number of trials. It is what we expect on average, not a guarantee.</p><div class=\"ex\"><div class=\"exh\">Worked example 2 · Prediction</div><div class=\"exl\">A fair die is rolled 90 times. How many 5s would you expect?<br><span class=\"fq\"><span>1</span><span>6</span></span> × 90 = <b>15</b>.</div></div><div class=\"ex\"><div class=\"exh\">Worked example 3 · Prediction from data</div><div class=\"exl\">In a survey, 27 of 120 students chose kho-kho as their favourite sport.<br>Experimental P = <span class=\"fq\"><span>27</span><span>120</span></span> = 0.225. For a school of 800 students, expect about 0.225 × 800 = <b>180</b>.</div></div><h4>Simulations</h4><p>A <b>simulation</b> copies a real situation with a simple device (coin, die, spinner, cards, random numbers) that has the <b>same probabilities</b>. Then you run many trials quickly and cheaply.</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Real event</th><th>Probability</th><th>Simulation</th></tr><tr><td>a baby is a girl</td><td>about <span class=\"fq\"><span>1</span><span>2</span></span></td><td>toss a coin: heads = girl</td></tr><tr><td>guessing a 4-option MCQ</td><td><span class=\"fq\"><span>1</span><span>4</span></span></td><td>spinner with 4 equal sectors, 1 marked “correct”</td></tr><tr><td>a bowler takes a wicket with 1 ball in 6</td><td><span class=\"fq\"><span>1</span><span>6</span></span></td><td>roll a die: 6 = wicket</td></tr><tr><td>a batter scores 30% of the time</td><td>0.3</td><td>random digit 0–9: 0, 1, 2 = score</td></tr><tr><td>birthday on a given weekday</td><td><span class=\"fq\"><span>1</span><span>7</span></span></td><td>spinner with 7 equal sectors</td></tr></table></div><div class=\"keybox\"><b>Design checklist:</b> (1) state the question, (2) choose a device with matching probabilities, (3) say what counts as one trial and what counts as “success”, (4) run many trials and record them in a table, (5) calculate the experimental probability and compare it with theory.</div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s5\">Practise 2.5 →</button></div></section><section class=\"note\" id=\"nsum\"><h2>Unit checklist</h2><ul><li>I can name trials, outcomes, events and the sample space.</li><li>I can show a sample space with an organised list, a tree diagram and a table, and count outcomes with m × n.</li><li>I can place a probability on the 0 to 1 scale, describe it in words and write it as a fraction, decimal and percentage.</li><li>I can calculate theoretical probability for single and compound events and decide whether a game is fair.</li><li>I can use P(not A) = 1 − P(A), including for “at least one” questions.</li><li>I can calculate experimental probability, compare it with theory, predict expected numbers and design a simulation.</li></ul><div class=\"hub-btns\" style=\"margin-top:10px\"><button class=\"hub-btn\" data-go=\"s6\">Test A</button><button class=\"hub-btn\" data-go=\"s7\">Test B</button><button class=\"hub-btn\" data-go=\"s8\">Test C</button><button class=\"hub-btn\" data-go=\"s9\">Test D</button><button class=\"hub-btn primary\" data-go=\"report\">📊 Report</button></div></section>";
var REPORT = [["s6", "A", "Knowing and understanding"], ["s7", "B", "Investigating patterns"], ["s8", "C", "Communicating"], ["s9", "D", "Applying mathematics in real-life contexts"]];
var SECTIONS = [{"id": "s1", "label": "2.1 Sample spaces", "sub": "Events and sample spaces — lists, tables and tree diagrams", "slides": [{"kind": "mcq", "text": "A coin is tossed and a fair six-sided die is rolled. How many outcomes are in the sample space?", "opts": ["36", "6", "8", "12"], "correct": 3, "tag": "", "sol": "2 outcomes for the coin × 6 for the die = 12 (H1, …, H6, T1, …, T6)."}, {"kind": "mcq", "text": "Which is the sample space for rolling a four-sided die numbered 1 to 4?", "opts": ["{1, 2, 3, 4}", "{1, 4}", "{0, 1, 2, 3, 4}", "{4}"], "correct": 0, "tag": "", "sol": "The sample space lists every possible outcome once: 1, 2, 3 and 4. A 0 cannot be rolled."}, {"kind": "mcq", "text": "Two coins are tossed. How many different outcomes are there?", "opts": ["4", "8", "2", "3"], "correct": 0, "tag": "", "sol": "HH, HT, TH, TT. HT and TH are different outcomes, so there are 4, not 3."}, {"kind": "mcq", "text": "A café offers 3 drinks (chai, coffee, lassi) and 4 snacks. How many drink-and-snack combinations are possible?", "opts": ["12", "4", "7", "3"], "correct": 0, "tag": "", "sol": "Counting principle: 3 × 4 = 12. Adding (3 + 4 = 7) counts items, not combinations."}, {"kind": "mcq", "text": "Three coins are tossed. How many end branches does the full tree diagram have?", "opts": ["9", "6", "3", "8"], "correct": 3, "tag": "", "sol": "Each coin doubles the branches: 2 × 2 × 2 = 8."}, {"kind": "mcq", "text": "Which of these is a compound event?", "opts": ["Drawing a red card", "Tossing a head", "Rolling a 5", "Rolling a die and spinning a spinner"], "correct": 3, "tag": "", "sol": "A compound event involves two or more things happening together. The others are single events."}, {"kind": "mcq", "text": "In the table of totals for two six-sided dice, how many cells show a total of 7?", "opts": ["1", "6", "5", "7"], "correct": 1, "tag": "", "sol": "(1,6), (2,5), (3,4), (4,3), (5,2), (6,1): 6 cells out of 36."}, {"kind": "mcq", "text": "The tree diagram shows a coin tossed and then a 1-2-3 spinner spun. Which outcome belongs in the place marked “?”", "opts": ["T3", "T2", "H2", "2T"], "correct": 1, "tag": "", "sol": "Follow the path: the first branch is T, the second is 2, so the outcome is T2.", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 250 140\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><circle cx=\"16\" cy=\"77.0\" r=\"3\" style=\"fill:var(--ink)\"/><text class=\"po\" x=\"81.3\" y=\"12.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">coin</text><text class=\"po\" x=\"146.6\" y=\"12.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">spinner</text><text class=\"po\" x=\"211.9\" y=\"12.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">outcome</text><line x1=\"16.0\" y1=\"77.0\" x2=\"70.3\" y2=\"48.5\" style=\"stroke:var(--ink-soft);stroke-width:1.3\"/><text class=\"lb\" x=\"81.3\" y=\"48.5\" text-anchor=\"middle\" dominant-baseline=\"middle\">H</text><line x1=\"90.3\" y1=\"48.5\" x2=\"135.6\" y2=\"29.5\" style=\"stroke:var(--ink-soft);stroke-width:1.3\"/><text class=\"lb\" x=\"146.6\" y=\"29.5\" text-anchor=\"middle\" dominant-baseline=\"middle\">1</text><text class=\"al\" x=\"187.9\" y=\"29.5\" text-anchor=\"start\" dominant-baseline=\"middle\">H1</text><line x1=\"90.3\" y1=\"48.5\" x2=\"135.6\" y2=\"48.5\" style=\"stroke:var(--ink-soft);stroke-width:1.3\"/><text class=\"lb\" x=\"146.6\" y=\"48.5\" text-anchor=\"middle\" dominant-baseline=\"middle\">2</text><text class=\"al\" x=\"187.9\" y=\"48.5\" text-anchor=\"start\" dominant-baseline=\"middle\">H2</text><line x1=\"90.3\" y1=\"48.5\" x2=\"135.6\" y2=\"67.5\" style=\"stroke:var(--ink-soft);stroke-width:1.3\"/><text class=\"lb\" x=\"146.6\" y=\"67.5\" text-anchor=\"middle\" dominant-baseline=\"middle\">3</text><text class=\"al\" x=\"187.9\" y=\"67.5\" text-anchor=\"start\" dominant-baseline=\"middle\">H3</text><line x1=\"16.0\" y1=\"77.0\" x2=\"70.3\" y2=\"105.5\" style=\"stroke:var(--ink-soft);stroke-width:1.3\"/><text class=\"lb\" x=\"81.3\" y=\"105.5\" text-anchor=\"middle\" dominant-baseline=\"middle\">T</text><line x1=\"90.3\" y1=\"105.5\" x2=\"135.6\" y2=\"86.5\" style=\"stroke:var(--ink-soft);stroke-width:1.3\"/><text class=\"lb\" x=\"146.6\" y=\"86.5\" text-anchor=\"middle\" dominant-baseline=\"middle\">1</text><text class=\"al\" x=\"187.9\" y=\"86.5\" text-anchor=\"start\" dominant-baseline=\"middle\">T1</text><line x1=\"90.3\" y1=\"105.5\" x2=\"135.6\" y2=\"105.5\" style=\"stroke:var(--ink-soft);stroke-width:1.3\"/><text class=\"lb\" x=\"146.6\" y=\"105.5\" text-anchor=\"middle\" dominant-baseline=\"middle\">2</text><text class=\"al\" x=\"187.9\" y=\"105.5\" text-anchor=\"start\" dominant-baseline=\"middle\">?</text><line x1=\"90.3\" y1=\"105.5\" x2=\"135.6\" y2=\"124.5\" style=\"stroke:var(--ink-soft);stroke-width:1.3\"/><text class=\"lb\" x=\"146.6\" y=\"124.5\" text-anchor=\"middle\" dominant-baseline=\"middle\">3</text><text class=\"al\" x=\"187.9\" y=\"124.5\" text-anchor=\"start\" dominant-baseline=\"middle\">T3</text></svg>"}, {"kind": "blank", "p": "A coin is tossed and a spinner with three equal sections A, B and C is spun.", "tag": "", "marks": "", "flat": [{"t": "Number of outcomes = __B1__", "a": {"B1": "6"}}, {"t": "Number of outcomes that include heads = __B1__", "a": {"B1": "3"}}, {"t": "The outcome that starts with T and ends with a vowel is __B1__", "a": {"B1": "TA"}, "expr": "words", "accept": ["t a", "t-a"]}], "sol": "2 × 3 = 6: HA, HB, HC, TA, TB, TC.\nHA, HB, HC.\nA is the only vowel: TA."}, {"kind": "blank", "p": "Two four-sided dice (numbered 1 to 4) are rolled and the two numbers are added. Draw a 4 × 4 table.", "tag": "", "marks": "", "flat": [{"t": "Number of cells in the table = __B1__", "a": {"B1": "16"}}, {"t": "Number of cells with a total of 5 = __B1__", "a": {"B1": "4"}}, {"t": "The smallest total is __B1__ and the largest is __B2__.", "a": {"B1": "2", "B2": "8"}}, {"t": "Number of different totals possible = __B1__", "a": {"B1": "7"}}], "sol": "4 × 4 = 16.\n(1,4), (2,3), (3,2), (4,1).\n1 + 1 = 2 and 4 + 4 = 8.\n2, 3, 4, 5, 6, 7, 8: seven totals."}, {"kind": "blank", "p": "A kabaddi team plays three matches. Each match is a win (W) or a loss (L). Use a tree diagram.", "tag": "", "marks": "", "flat": [{"t": "Number of outcomes = __B1__", "a": {"B1": "8"}}, {"t": "Outcomes with exactly two wins = __B1__", "a": {"B1": "3"}}, {"t": "Outcomes with at least one loss = __B1__", "a": {"B1": "7"}}], "sol": "2 × 2 × 2 = 8.\nWWL, WLW, LWW.\nEvery outcome except WWW: 8 − 1 = 7."}, {"kind": "blank", "p": "A school canteen offers 3 mains (rajma-chawal, chole-bhature, dosa) and 2 desserts (kheer, fruit).", "tag": "", "marks": "", "flat": [{"t": "Main-and-dessert combinations = __B1__", "a": {"B1": "6"}}, {"t": "If “no dessert” is also allowed, the number of meals = __B1__", "a": {"B1": "9"}}], "sol": "3 × 2 = 6.\nNow there are 3 dessert choices: 3 × 3 = 9."}, {"kind": "blank", "p": "A sports kit has one shirt (4 colours), one pair of shorts (3 colours) and one pair of socks (2 colours).", "tag": "", "marks": "", "flat": [{"t": "Shirt-and-shorts combinations = __B1__", "a": {"B1": "12"}}, {"t": "Complete kits = __B1__", "a": {"B1": "24"}}], "sol": "4 × 3 = 12.\n12 × 2 = 24 (counting principle for three stages)."}]}, {"id": "s2", "label": "2.2 Likelihood", "sub": "Representing likelihood — words, the probability scale, fractions, decimals and percentages", "slides": [{"kind": "mcq", "text": "Which probability means an event is impossible?", "opts": ["0.5", "0", "−1", "1"], "correct": 1, "tag": "", "sol": "Impossible events have probability 0. Probabilities are never negative."}, {"kind": "mcq", "text": "P(event) = 0.85. How is the event best described?", "opts": ["Likely", "Certain", "Unlikely", "Equally likely as unlikely"], "correct": 0, "tag": "", "sol": "0.85 is between 0.5 and 1 (but not 1), so it is likely."}, {"kind": "mcq", "text": "Write {3/8} as a percentage.", "opts": ["3.8%", "37.5%", "62.5%", "38%"], "correct": 1, "tag": "", "sol": "3 ÷ 8 = 0.375 = 37.5%."}, {"kind": "mcq", "text": "Which value cannot be a probability?", "opts": ["0.02", "99%", "{7/7}", "1.2"], "correct": 3, "tag": "", "sol": "Probabilities lie from 0 to 1. {7/7} = 1 is allowed (certain); 1.2 is greater than 1."}, {"kind": "mcq", "text": "Order these probabilities from least to most likely: 0.3, {1/4}, 28%.", "opts": ["28%, {1/4}, 0.3", "0.3, 28%, {1/4}", "{1/4}, 0.3, 28%", "{1/4}, 28%, 0.3"], "correct": 3, "tag": "", "sol": "As decimals: 0.25, 0.28, 0.3."}, {"kind": "mcq", "text": "A “1 in 5 chance” written as a decimal is", "opts": ["1.5", "0.2", "0.5", "0.15"], "correct": 1, "tag": "", "sol": "1 ÷ 5 = 0.2."}, {"kind": "mcq", "text": "A weather app says there is a 50% chance of rain. How is this described?", "opts": ["Equally likely as unlikely", "Impossible", "Certain", "Unlikely"], "correct": 0, "tag": "", "sol": "50% = 0.5, the middle of the scale: an even chance."}, {"kind": "mcq", "text": "The arrow on the probability scale marks an event. Which event could it be?", "opts": ["Rolling a 6 on a six-sided die", "A fair coin landing heads", "Rolling a number greater than 1 on a six-sided die", "Rolling a 7 on a six-sided die"], "correct": 2, "tag": "", "sol": "The arrow is at about 0.83, which is {5/6}. P(greater than 1) = {5/6} (outcomes 2 to 6), which is likely. The others are 0, 0.5 and {1/6} ≈ 0.17.", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 330 92\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><rect x=\"22\" y=\"42\" width=\"286\" height=\"8\" rx=\"4\" style=\"fill:var(--gold-soft);stroke:var(--gold)\"/><line x1=\"22.0\" y1=\"37\" x2=\"22.0\" y2=\"55\" style=\"stroke:var(--ink);stroke-width:1.4\"/><text class=\"lb\" x=\"22.0\" y=\"64.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">0</text><text class=\"po\" x=\"22.0\" y=\"80.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">Impossible</text><line x1=\"93.5\" y1=\"37\" x2=\"93.5\" y2=\"55\" style=\"stroke:var(--ink);stroke-width:1.4\"/><text class=\"lb\" x=\"93.5\" y=\"64.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">¼</text><text class=\"po\" x=\"93.5\" y=\"80.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">Unlikely</text><line x1=\"165.0\" y1=\"37\" x2=\"165.0\" y2=\"55\" style=\"stroke:var(--ink);stroke-width:1.4\"/><text class=\"lb\" x=\"165.0\" y=\"64.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">½</text><text class=\"po\" x=\"165.0\" y=\"80.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">Even chance</text><line x1=\"236.5\" y1=\"37\" x2=\"236.5\" y2=\"55\" style=\"stroke:var(--ink);stroke-width:1.4\"/><text class=\"lb\" x=\"236.5\" y=\"64.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">¾</text><text class=\"po\" x=\"236.5\" y=\"80.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">Likely</text><line x1=\"308.0\" y1=\"37\" x2=\"308.0\" y2=\"55\" style=\"stroke:var(--ink);stroke-width:1.4\"/><text class=\"lb\" x=\"308.0\" y=\"64.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">1</text><text class=\"po\" x=\"308.0\" y=\"80.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">Certain</text><path d=\"M259.4,39 l-5,-9 h10 z\" style=\"fill:var(--danger)\"/><text class=\"al\" x=\"259.4\" y=\"22.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">?</text></svg>"}, {"kind": "blank", "p": "The probability that a school bus is late is {7/20}.", "tag": "", "marks": "", "flat": [{"t": "As a decimal: __B1__", "a": {"B1": "0.35"}}, {"t": "As a percentage: __B1__%", "a": {"B1": "35"}}, {"t": "In words, the bus being late is __B1__ (unlikely / likely).", "a": {"B1": "unlikely"}, "expr": "words"}], "sol": "7 ÷ 20 = 0.35.\n0.35 × 100 = 35%.\n0.35 is less than 0.5, so unlikely."}, {"kind": "blank", "p": "The probability that a batter scores at least one run in an over is 0.64.", "tag": "", "marks": "", "flat": [{"t": "As a fraction in lowest terms: __B1__", "a": {"B1": "16/25"}, "expr": "fl"}, {"t": "As a percentage: __B1__%", "a": {"B1": "64"}}], "sol": "0.64 = {64/100}; divide by 4: {16/25}.\n0.64 × 100 = 64%, which is likely."}, {"kind": "blank", "p": "An event has probability 12.5%.", "tag": "", "marks": "", "flat": [{"t": "As a decimal: __B1__", "a": {"B1": "0.125"}}, {"t": "As a fraction in lowest terms: __B1__", "a": {"B1": "1/8"}, "expr": "fl"}], "sol": "12.5 ÷ 100 = 0.125.\n0.125 = {125/1000} = {1/8}."}, {"kind": "blank", "p": "Three events have P(A) = {2/5}, P(B) = 0.45 and P(C) = 38%.", "tag": "", "marks": "", "flat": [{"t": "The least likely event is __B1__.", "a": {"B1": "C"}, "expr": "words", "accept": ["event c"]}, {"t": "The most likely event is __B1__.", "a": {"B1": "B"}, "expr": "words", "accept": ["event b"]}], "sol": "As decimals: A = 0.4, B = 0.45, C = 0.38. Smallest: C.\nLargest: B."}, {"kind": "blank", "p": "A game show says a contestant has a “1 in 4” chance of winning a prize.", "tag": "", "marks": "", "flat": [{"t": "As a decimal: __B1__", "a": {"B1": "0.25"}}, {"t": "As a percentage: __B1__%", "a": {"B1": "25"}}, {"t": "The chance of not winning, as a percentage: __B1__%", "a": {"B1": "75"}}], "sol": "1 ÷ 4 = 0.25.\n25%.\n100% − 25% = 75%."}]}, {"id": "s3", "label": "2.3 Theoretical", "sub": "Theoretical probability — single and compound events with equally likely outcomes", "slides": [{"kind": "mcq", "text": "A fair six-sided die is rolled. What is P(prime number)?", "opts": ["{5/6}", "{1/3}", "{1/2}", "{2/3}"], "correct": 2, "tag": "", "sol": "Primes are 2, 3, 5: 3 of 6 outcomes = {1/2}. (1 is not prime.)"}, {"kind": "mcq", "text": "A bag has 5 red, 3 blue and 2 green marbles. One is taken at random. What is P(blue)?", "opts": ["{3/5}", "{1/3}", "{3/7}", "{3/10}"], "correct": 3, "tag": "", "sol": "3 blue out of 5 + 3 + 2 = 10 marbles."}, {"kind": "mcq", "text": "A letter is chosen at random from the word PROBABILITY. What is P(B)?", "opts": ["{2/11}", "{1/11}", "{2/9}", "{1/5}"], "correct": 0, "tag": "", "sol": "PROBABILITY has 11 letters and B appears twice."}, {"kind": "mcq", "text": "A spinner has 8 equal sectors numbered 1 to 8. What is P(multiple of 3)?", "opts": ["{1/3}", "{3/8}", "{1/8}", "{1/4}"], "correct": 3, "tag": "", "sol": "Multiples of 3: 3 and 6. {2/8} = {1/4}."}, {"kind": "mcq", "text": "Two fair coins are tossed. What is P(one head and one tail)?", "opts": ["{1/4}", "{1/3}", "{1/2}", "{3/4}"], "correct": 2, "tag": "", "sol": "HT and TH are favourable, 2 of 4 outcomes = {1/2}."}, {"kind": "mcq", "text": "Two fair six-sided dice are rolled. What is P(total = 10)?", "opts": ["{1/12}", "{3/11}", "{1/6}", "{1/11}"], "correct": 0, "tag": "", "sol": "(4,6), (5,5), (6,4): 3 of 36 = {1/12}. There are 36 equally likely outcomes, not 11 totals."}, {"kind": "mcq", "text": "A card is drawn from cards numbered 1 to 20. What is P(the number is a multiple of 4 or a multiple of 5)?", "opts": ["{2/5}", "{1/2}", "{1/4}", "{9/20}"], "correct": 0, "tag": "", "sol": "Multiples of 4: 4, 8, 12, 16, 20. Multiples of 5: 5, 10, 15, 20. 20 is in both, so there are 8 different numbers: {8/20} = {2/5}."}, {"kind": "mcq", "text": "Two players use one fair die. Which rule makes the game fair?", "opts": ["A wins on a prime, B on a 1", "A wins on a 6, B on 1 to 5", "A wins on 1 or 2, B on 3, 4, 5 or 6", "A wins on an even number, B on an odd number"], "correct": 3, "tag": "", "sol": "A fair game gives both players the same probability: even and odd each have {3/6} = {1/2}."}, {"kind": "blank", "p": "A bag contains 4 red, 6 yellow and 2 white marbles. One marble is taken at random.", "tag": "", "marks": "", "flat": [{"t": "Total number of marbles = __B1__", "a": {"B1": "12"}}, {"t": "P(yellow) = __B1__", "a": {"B1": "1/2"}}, {"t": "P(white) = __B1__", "a": {"B1": "1/6"}}, {"t": "P(red or white) = __B1__", "a": {"B1": "1/2"}}], "sol": "4 + 6 + 2 = 12.\n{6/12} = {1/2}.\n{2/12} = {1/6}.\n4 + 2 = 6 favourable: {6/12} = {1/2}."}, {"kind": "blank", "p": "Two fair six-sided dice are rolled. Use the 6 × 6 table of totals.", "tag": "", "marks": "", "flat": [{"t": "P(total = 8) = __B1__", "a": {"B1": "5/36"}}, {"t": "P(a double) = __B1__", "a": {"B1": "1/6"}}, {"t": "P(total is 10 or more) = __B1__", "a": {"B1": "1/6"}}], "sol": "(2,6), (3,5), (4,4), (5,3), (6,2): {5/36}.\n(1,1) to (6,6): {6/36} = {1/6}.\nTotal 10: 3 ways, 11: 2 ways, 12: 1 way; {6/36} = {1/6}."}, {"kind": "blank", "p": "A coin is tossed and a fair six-sided die is rolled.", "tag": "", "marks": "", "flat": [{"t": "Number of outcomes = __B1__", "a": {"B1": "12"}}, {"t": "P(head and an even number) = __B1__", "a": {"B1": "1/4"}}, {"t": "P(tail and a number greater than 4) = __B1__", "a": {"B1": "1/6"}}], "sol": "2 × 6 = 12.\nH2, H4, H6: {3/12} = {1/4}.\nT5, T6: {2/12} = {1/6}."}, {"kind": "blank", "p": "A class has 14 girls and 16 boys. One student is chosen at random to be captain.", "tag": "", "marks": "", "flat": [{"t": "P(girl) as a fraction in lowest terms = __B1__", "a": {"B1": "7/15"}, "expr": "fl"}, {"t": "P(girl) as a percentage (1 decimal place) = __B1__%", "a": {"B1": "46.7"}}], "sol": "{14/30} = {7/15}.\n7 ÷ 15 = 0.4666… ≈ 46.7%."}, {"kind": "blank", "p": "Three fair coins are tossed (8 outcomes).", "tag": "", "marks": "", "flat": [{"t": "P(exactly two heads) = __B1__", "a": {"B1": "3/8"}}, {"t": "P(all three the same) = __B1__", "a": {"B1": "1/4"}}], "sol": "HHT, HTH, THH: {3/8}.\nHHH and TTT: {2/8} = {1/4}."}]}, {"id": "s4", "label": "2.4 Complements", "sub": "Complementary events — P(not A) = 1 − P(A)", "slides": [{"kind": "mcq", "text": "P(rain tomorrow) = 0.35. What is P(no rain tomorrow)?", "opts": ["0.35", "0.65", "1.35", "0.75"], "correct": 1, "tag": "", "sol": "1 − 0.35 = 0.65."}, {"kind": "mcq", "text": "On a six-sided die, what is the complement of “rolling a 6”?", "opts": ["Rolling 1, 2, 3, 4 or 5", "Rolling an even number", "Rolling a 6 again", "Rolling a 1"], "correct": 0, "tag": "", "sol": "The complement contains every outcome that is not a 6."}, {"kind": "mcq", "text": "P(win) = {3/8}. What is P(not win)?", "opts": ["{5/8}", "{8/3}", "{3/8}", "{1/8}"], "correct": 0, "tag": "", "sol": "1 − {3/8} = {5/8}."}, {"kind": "mcq", "text": "Which pair of events on a six-sided die is complementary?", "opts": ["rolling a 1 and rolling a 6", "even number and odd number", "less than 3 and greater than 3", "prime number and composite number"], "correct": 1, "tag": "", "sol": "Even and odd have no outcome in common and together cover 1 to 6. The prime/composite pair misses 1, and less than 3/greater than 3 misses 3."}, {"kind": "mcq", "text": "P(A) = 27%. What is P(not A)?", "opts": ["83%", "73%", "63%", "27%"], "correct": 1, "tag": "", "sol": "100% − 27% = 73%."}, {"kind": "mcq", "text": "A bag has only red, blue and green balls. P(red) = 0.2 and P(blue) = 0.5. What is P(green)?", "opts": ["0.7", "0.3", "0.8", "0.5"], "correct": 1, "tag": "", "sol": "The three probabilities add to 1: 1 − 0.2 − 0.5 = 0.3."}, {"kind": "mcq", "text": "Two fair coins are tossed. What is P(at least one head)?", "opts": ["{1/2}", "{1/4}", "{2/3}", "{3/4}"], "correct": 3, "tag": "", "sol": "Only TT has no head: P(TT) = {1/4}, so P(at least one head) = 1 − {1/4} = {3/4}."}, {"kind": "mcq", "text": "Leela writes P(A) = 0.4 and P(not A) = 0.5. What is wrong?", "opts": ["P(not A) must be smaller than P(A)", "The two probabilities must add up to 1", "Nothing is wrong", "Probabilities cannot be decimals"], "correct": 1, "tag": "", "sol": "An event and its complement cover the whole sample space, so their probabilities add to 1. If P(A) = 0.4 then P(not A) = 0.6."}, {"kind": "blank", "p": "The probability that the 8:10 local train is late is 0.12.", "tag": "", "marks": "", "flat": [{"t": "P(not late) = __B1__", "a": {"B1": "0.88"}}, {"t": "As a percentage: __B1__%", "a": {"B1": "88"}}], "sol": "1 − 0.12 = 0.88.\n88%."}, {"kind": "blank", "p": "A spinner has 10 equal sections and 3 are red.", "tag": "", "marks": "", "flat": [{"t": "P(red) = __B1__", "a": {"B1": "3/10"}}, {"t": "P(not red) = __B1__", "a": {"B1": "7/10"}}], "sol": "3 of 10.\n1 − {3/10} = {7/10}."}, {"kind": "blank", "p": "Two fair six-sided dice are rolled.", "tag": "", "marks": "", "flat": [{"t": "P(double) = __B1__", "a": {"B1": "1/6"}}, {"t": "P(not a double) = __B1__", "a": {"B1": "5/6"}}, {"t": "Number of the 36 cells that are not doubles = __B1__", "a": {"B1": "30"}}], "sol": "6 doubles out of 36 = {1/6}.\n1 − {1/6} = {5/6}.\n36 − 6 = 30."}, {"kind": "blank", "p": "Three fair coins are tossed.", "tag": "", "marks": "", "flat": [{"t": "P(no heads) = __B1__", "a": {"B1": "1/8"}}, {"t": "P(at least one head) = __B1__", "a": {"B1": "7/8"}}], "sol": "Only TTT: {1/8}.\n1 − {1/8} = {7/8}."}, {"kind": "blank", "p": "A bag has only red, blue and green counters. P(red) = {1/4} and P(blue) = {1/3}.", "tag": "", "marks": "", "flat": [{"t": "P(green) = __B1__", "a": {"B1": "5/12"}}, {"t": "If there are 24 counters, the number of green counters = __B1__", "a": {"B1": "10"}}], "sol": "1 − {1/4} − {1/3} = {12/12} − {3/12} − {4/12} = {5/12}.\n{5/12} × 24 = 10."}]}, {"id": "s5", "label": "2.5 Experiments", "sub": "Experimental probability and simulations — data, predictions and simulations", "slides": [{"kind": "mcq", "text": "A coin was tossed 50 times and landed heads 28 times. What is the experimental probability of heads?", "opts": ["0.5", "0.28", "0.44", "0.56"], "correct": 3, "tag": "", "sol": "{28/50} = 0.56."}, {"kind": "mcq", "text": "As the number of trials increases, the experimental probability usually", "opts": ["stays exactly the same", "becomes 0", "gets closer to the theoretical probability", "gets further from the theoretical probability"], "correct": 2, "tag": "", "sol": "With many trials, results settle towards the theoretical value."}, {"kind": "mcq", "text": "A fair die is rolled 120 times. How many 4s would you expect?", "opts": ["20", "4", "30", "24"], "correct": 0, "tag": "", "sol": "{1/6} × 120 = 20."}, {"kind": "mcq", "text": "A goalkeeper saves 1 penalty in every 4. Which device best simulates one penalty?", "opts": ["A ten-sided die, 1 to 4 = save", "A six-sided die, 1 = save", "A spinner with 4 equal sectors, 1 marked “save”", "A coin, heads = save"], "correct": 2, "tag": "", "sol": "The device must give P(save) = {1/4}. A coin gives {1/2}, a die {1/6}, and 4 faces of 10 give {2/5}."}, {"kind": "mcq", "text": "A spinner was spun 80 times: 1 came up 22 times, 2 came up 31 times, 3 came up 27 times. What is the experimental P(2), to 2 decimal places?", "opts": ["0.31", "0.33", "0.39", "0.34"], "correct": 2, "tag": "", "sol": "{31/80} = 0.3875 ≈ 0.39."}, {"kind": "mcq", "text": "A fair die was rolled 12 times and no 6 appeared. What is the best conclusion?", "opts": ["P(6) is 0 for this die", "This can happen by chance; 12 rolls are too few to judge", "The next roll must be a 6", "The die must be biased"], "correct": 1, "tag": "", "sol": "With so few trials, experimental results can be far from theory. Dice have no memory, so the next roll still has P(6) = {1/6}."}, {"kind": "mcq", "text": "A stall found that 18 of its first 60 customers ordered masala dosa. About how many of the next 500 customers would you expect to order it?", "opts": ["150", "90", "300", "18"], "correct": 0, "tag": "", "sol": "Experimental P = {18/60} = 0.3; 0.3 × 500 = 150."}, {"kind": "mcq", "text": "Which device can simulate choosing a random day of the week?", "opts": ["A six-sided die", "A coin", "A spinner with 4 equal sectors", "A spinner with 7 equal sectors"], "correct": 3, "tag": "", "sol": "There are 7 equally likely days, so the device needs 7 equally likely outcomes."}, {"kind": "blank", "p": "A die was rolled 60 times. The frequencies were:\n1 → 8,  2 → 11,  3 → 9,  4 → 12,  5 → 7,  6 → 13", "tag": "", "marks": "", "flat": [{"t": "Experimental P(6), to 2 decimal places = __B1__", "a": {"B1": "0.22"}}, {"t": "Theoretical P(6), to 2 decimal places = __B1__", "a": {"B1": "0.17"}}, {"t": "Expected number of 6s in 60 rolls = __B1__", "a": {"B1": "10"}}], "sol": "{13/60} = 0.2166… ≈ 0.22.\n{1/6} = 0.1666… ≈ 0.17.\n{1/6} × 60 = 10. Getting 13 instead of 10 is normal variation for 60 rolls."}, {"kind": "blank", "p": "Rohan tosses a coin. After 20 tosses he has 13 heads. After 200 tosses he has 104 heads.", "tag": "", "marks": "", "flat": [{"t": "Experimental P(heads) after 20 tosses = __B1__", "a": {"B1": "0.65"}}, {"t": "Experimental P(heads) after 200 tosses = __B1__", "a": {"B1": "0.52"}}, {"t": "The result closer to the theoretical 0.5 is after __B1__ tosses.", "a": {"B1": "200"}}], "sol": "{13/20} = 0.65.\n{104/200} = 0.52.\n0.52 is closer to 0.5: more trials usually give a better estimate."}, {"kind": "blank", "p": "A batter hits a boundary on 30% of balls. You simulate each ball with a random digit 0–9.", "tag": "", "marks": "", "flat": [{"t": "How many of the 10 digits should stand for “boundary”? __B1__", "a": {"B1": "3"}}, {"t": "In 40 simulated balls, the expected number of boundaries = __B1__", "a": {"B1": "12"}}], "sol": "30% of 10 digits = 3 digits (for example 0, 1, 2).\n0.3 × 40 = 12."}, {"kind": "blank", "p": "At a busy junction, 150 of 400 cars counted turned left.", "tag": "", "marks": "", "flat": [{"t": "Experimental P(turn left) = __B1__", "a": {"B1": "0.375"}}, {"t": "Predicted number turning left out of 2000 cars = __B1__", "a": {"B1": "750"}}], "sol": "{150/400} = 0.375.\n0.375 × 2000 = 750."}]}, {"id": "s6", "label": "Test A", "sub": "Criterion A — Knowing and understanding", "slides": [{"kind": "mcq", "text": "The spinner has 5 equal sectors. What is P(2)?", "opts": ["{2/3}", "{2/5}", "{1/5}", "{1/4}"], "correct": 1, "tag": "", "sol": "Two of the five equal sectors show 2.", "fig": "<svg class=\"figsvg\" style=\"max-width:220px\" viewBox=\"0 0 170 142\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><path d=\"M85.0,72.0 L85.0,8.0 A64,64 0 0 1 145.9,52.2 Z\" style=\"fill:var(--gold-soft);stroke:var(--ink);stroke-width:1.3\"/><text class=\"lb\" x=\"109.1\" y=\"38.9\" text-anchor=\"middle\" dominant-baseline=\"middle\">1</text><path d=\"M85.0,72.0 L145.9,52.2 A64,64 0 0 1 122.6,123.8 Z\" style=\"fill:var(--paper-2);stroke:var(--ink);stroke-width:1.3\"/><text class=\"lb\" x=\"124.0\" y=\"84.7\" text-anchor=\"middle\" dominant-baseline=\"middle\">2</text><path d=\"M85.0,72.0 L122.6,123.8 A64,64 0 0 1 47.4,123.8 Z\" style=\"fill:var(--card);stroke:var(--ink);stroke-width:1.3\"/><text class=\"lb\" x=\"85.0\" y=\"113.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">2</text><path d=\"M85.0,72.0 L47.4,123.8 A64,64 0 0 1 24.1,52.2 Z\" style=\"fill:var(--gold-soft);stroke:var(--ink);stroke-width:1.3\"/><text class=\"lb\" x=\"46.0\" y=\"84.7\" text-anchor=\"middle\" dominant-baseline=\"middle\">3</text><path d=\"M85.0,72.0 L24.1,52.2 A64,64 0 0 1 85.0,8.0 Z\" style=\"fill:var(--paper-2);stroke:var(--ink);stroke-width:1.3\"/><text class=\"lb\" x=\"60.9\" y=\"38.9\" text-anchor=\"middle\" dominant-baseline=\"middle\">4</text><line class=\"ln\" x1=\"85.0\" y1=\"72.0\" x2=\"97.6\" y2=\"48.3\" marker-end=\"url(#ah)\"/><circle cx=\"85.0\" cy=\"72.0\" r=\"3.5\" style=\"fill:var(--ink)\"/></svg>"}, {"kind": "mcq", "text": "A bag has 7 green and 5 yellow beads. One is taken at random. What is P(yellow)?", "opts": ["{5/7}", "{1/5}", "{5/12}", "{7/12}"], "correct": 2, "tag": "", "sol": "5 yellow out of 12 beads."}, {"kind": "mcq", "text": "P(A) = 0.07. What is P(not A)?", "opts": ["0.3", "0.93", "0.97", "0.07"], "correct": 1, "tag": "", "sol": "1 − 0.07 = 0.93."}, {"kind": "mcq", "text": "A six-sided die and an eight-sided die are rolled together. How many outcomes are there?", "opts": ["64", "36", "14", "48"], "correct": 3, "tag": "", "sol": "6 × 8 = 48."}, {"kind": "mcq", "text": "Write the probability 45% as a fraction in simplest form.", "opts": ["{4/5}", "{9/10}", "{45/100}", "{9/20}"], "correct": 3, "tag": "", "sol": "{45/100} ÷ 5 = {9/20}."}, {"kind": "mcq", "text": "A player won 36 of 90 games of carrom. What is the experimental probability that she wins?", "opts": ["0.36", "0.4", "0.25", "0.6"], "correct": 1, "tag": "", "sol": "{36/90} = 0.4."}, {"kind": "mcq", "text": "Two fair six-sided dice are rolled. What is P(total = 4)?", "opts": ["{1/9}", "{1/11}", "{1/12}", "{1/6}"], "correct": 2, "tag": "", "sol": "(1,3), (2,2), (3,1): {3/36} = {1/12}."}, {"kind": "mcq", "text": "A letter is picked at random from the word MUMBAI. What is P(M)?", "opts": ["{1/3}", "{2/5}", "{1/2}", "{1/6}"], "correct": 0, "tag": "", "sol": "M appears 2 times out of 6 letters: {2/6} = {1/3}."}, {"kind": "blank", "p": "A coin is tossed and a four-sided die (1 to 4) is rolled.", "tag": "", "marks": "", "flat": [{"t": "Number of outcomes = __B1__", "a": {"B1": "8"}}, {"t": "P(head and 4) = __B1__", "a": {"B1": "1/8"}}, {"t": "P(tail and an odd number) = __B1__", "a": {"B1": "1/4"}}], "sol": "2 × 4 = 8.\nOnly H4: {1/8}.\nT1, T3: {2/8} = {1/4}."}, {"kind": "blank", "p": "A card is drawn at random from 30 cards numbered 1 to 30.", "tag": "", "marks": "", "flat": [{"t": "P(multiple of 6) = __B1__", "a": {"B1": "1/6"}}, {"t": "P(not a multiple of 6) = __B1__", "a": {"B1": "5/6"}}], "sol": "6, 12, 18, 24, 30: {5/30} = {1/6}.\n1 − {1/6} = {5/6}."}, {"kind": "blank", "p": "A spinner landed on blue 70 times in 250 spins.", "tag": "", "marks": "", "flat": [{"t": "Experimental P(blue) = __B1__", "a": {"B1": "0.28"}}, {"t": "Expected number of blues in 1000 spins = __B1__", "a": {"B1": "280"}}], "sol": "{70/250} = 0.28.\n0.28 × 1000 = 280."}, {"kind": "blank", "p": "Two fair six-sided dice are rolled and the two numbers are multiplied.", "tag": "", "marks": "", "flat": [{"t": "P(product = 12) = __B1__", "a": {"B1": "1/9"}}, {"t": "P(product is odd) = __B1__", "a": {"B1": "1/4"}}], "sol": "(2,6), (3,4), (4,3), (6,2): {4/36} = {1/9}.\nBoth numbers must be odd: 3 × 3 = 9 cells, {9/36} = {1/4}."}]}, {"id": "s7", "label": "Test B", "sub": "Criterion B — Investigating patterns", "slides": [{"kind": "mcq", "text": "A spinner with n equal sectors is spun twice. With 2 sectors there are 4 outcomes, with 3 sectors 9, with 4 sectors 16. Which rule gives the number of outcomes?", "opts": ["2n", "n²", "2ⁿ", "n + n"], "correct": 1, "tag": "", "sol": "Each spin has n outcomes, so two spins give n × n = n². Check: 2ⁿ also gives 4 at n = 2, but gives 8 (not 9) at n = 3."}, {"kind": "mcq", "text": "With two dice, the number of ways to get a total of t is 1, 2, 3, 4, 5, 6 for t = 2, 3, 4, 5, 6, 7. Which rule fits for t from 2 to 7?", "opts": ["t − 1", "t", "t + 1", "2t − 3"], "correct": 0, "tag": "", "sol": "t = 2 gives 1, t = 7 gives 6: the number of ways is t − 1."}, {"kind": "mcq", "text": "P(at least one head) for 1, 2, 3, 4 coins is {1/2}, {3/4}, {7/8}, {15/16}. What is it for 5 coins?", "opts": ["{15/32}", "{31/32}", "{31/16}", "{16/17}"], "correct": 1, "tag": "", "sol": "The pattern is 1 − 1/2ⁿ (the only outcome with no head is all tails): 1 − {1/32} = {31/32}."}, {"kind": "blank", "p": "Investigation: a spinner has n equal sectors and exactly one of them is red.", "tag": "", "marks": "", "flat": [{"t": "n = 4: P(not red) = __B1__", "a": {"B1": "3/4"}}, {"t": "n = 5: P(not red) = __B1__", "a": {"B1": "4/5"}}, {"t": "n = 10: P(not red) = __B1__", "a": {"B1": "9/10"}}, {"t": "General rule: P(not red) = __B1__", "a": {"B1": "(n-1)/n"}, "expr": true, "accept": ["1-1/n"]}, {"t": "Use the rule: with 25 sectors, P(not red) = __B1__", "a": {"B1": "24/25"}}], "sol": "3 of the 4 sectors are not red: {3/4}.\n{4/5}.\n{9/10}.\nThe numerator is always one less than the number of sectors: (n − 1) ÷ n, which is the same as 1 − {1/n}.\n(25 − 1) ÷ 25 = {24/25}."}, {"kind": "blank", "p": "Investigation: a coin is tossed and a fair die with d faces is rolled. Count the outcomes.", "tag": "", "marks": "", "flat": [{"t": "d = 4: __B1__ outcomes; d = 6: __B2__ outcomes", "a": {"B1": "8", "B2": "12"}}, {"t": "d = 10: __B1__ outcomes", "a": {"B1": "20"}}, {"t": "General rule: number of outcomes = __B1__", "a": {"B1": "2d"}, "expr": true}, {"t": "Test the rule backwards: 40 outcomes needs a die with __B1__ faces.", "a": {"B1": "20"}}], "sol": "H1…H4, T1…T4 = 8; with 6 faces, 12.\n10 heads outcomes + 10 tails outcomes = 20.\nEvery face pairs with H and with T: 2 × d = 2d.\n2d = 40, so d = 20."}, {"kind": "blank", "p": "Investigation: how many outcomes are there when n coins are tossed?", "tag": "", "marks": "", "flat": [{"t": "1 coin: __B1__ outcomes; 2 coins: __B2__ outcomes", "a": {"B1": "2", "B2": "4"}}, {"t": "3 coins: __B1__ outcomes; 4 coins: __B2__ outcomes", "a": {"B1": "8", "B2": "16"}}, {"t": "General rule for n coins (type powers with ^, e.g. 2^n): __B1__ outcomes", "a": {"B1": "2^n"}, "expr": true}, {"t": "Use the rule: P(all heads) with 6 coins = __B1__", "a": {"B1": "1/64"}}], "sol": "H, T; HH, HT, TH, TT.\nEach extra coin doubles the branches: 8 and 16.\n2 × 2 × … × 2 (n times) = 2ⁿ.\nOnly one of the 2⁶ = 64 outcomes is all heads: {1/64}."}, {"kind": "blank", "p": "Investigation: P(double) when two fair dice with n faces each are rolled.", "tag": "", "marks": "", "flat": [{"t": "Two four-sided dice: P(double) = __B1__", "a": {"B1": "1/4"}}, {"t": "Two six-sided dice: P(double) = __B1__", "a": {"B1": "1/6"}}, {"t": "Two eight-sided dice: P(double) = __B1__", "a": {"B1": "1/8"}}, {"t": "General rule: P(double) = __B1__", "a": {"B1": "1/n"}, "expr": true}], "sol": "4 doubles of 16 = {1/4}.\n6 of 36 = {1/6}.\n8 of 64 = {1/8}.\nn doubles out of n × n outcomes: {n/n²} = {1/n}."}, {"kind": "blank", "p": "Investigation: two six-sided dice, number of ways W to get a total t, for t from 7 to 12.", "tag": "", "marks": "", "flat": [{"t": "t = 8: W = __B1__; t = 10: W = __B2__", "a": {"B1": "5", "B2": "3"}}, {"t": "t = 12: W = __B1__", "a": {"B1": "1"}}, {"t": "Rule for t from 7 to 12: W = __B1__", "a": {"B1": "13-t"}, "expr": true}, {"t": "Test the rule at t = 11: W = __B1__", "a": {"B1": "2"}}], "sol": "Total 8: (2,6)…(6,2), 5 ways. Total 10: (4,6), (5,5), (6,4).\nOnly (6,6).\nW goes down by 1 each time: 7→6, 8→5, …, 12→1, so W = 13 − t.\n13 − 11 = 2: (5,6) and (6,5) ✓."}, {"kind": "blank", "p": "A class records the number of heads as the number of tosses grows.\n10 tosses → 7 heads;  100 tosses → 56 heads;  1000 tosses → 508 heads", "tag": "", "marks": "", "flat": [{"t": "Experimental P after 10 tosses = __B1__", "a": {"B1": "0.7"}}, {"t": "After 100 tosses = __B1__; after 1000 tosses = __B2__", "a": {"B1": "0.56", "B2": "0.508"}}, {"t": "The values are getting closer to __B1__.", "a": {"B1": "0.5"}}], "sol": "{7/10} = 0.7.\n{56/100} = 0.56 and {508/1000} = 0.508.\nThey approach the theoretical probability 0.5 as the number of trials grows."}]}, {"id": "s8", "label": "Test C", "sub": "Criterion C — Communicating", "slides": [{"kind": "mcq", "text": "Which is the correct way to write “the probability of rolling a 5 is one sixth”?", "opts": ["P({1/6}) = 5", "P(5) = {1/6}", "5 = P({1/6})", "P = {5/6}"], "correct": 1, "tag": "", "sol": "The event goes inside the brackets and the probability goes after the equals sign."}, {"kind": "mcq", "text": "What is the name for the set of all possible outcomes of an experiment?", "opts": ["A trial", "The sample space", "An event", "The frequency"], "correct": 1, "tag": "", "sol": "The sample space lists every possible outcome."}, {"kind": "mcq", "text": "Arjun tosses two coins, one after the other. He says: “The results can be HH, HT (head then tail) or TT, so P(HT) = {1/3}.” What is his mistake?", "opts": ["He missed TH, so there are 4 outcomes and P(HT) = {1/4}", "There is no mistake", "P(HT) should be {2/3}", "He should have written {1/2}"], "correct": 0, "tag": "", "sol": "The sample space is HH, HT, TH, TT. HT (head then tail) is only one of these 4 equally likely outcomes, so P(HT) = {1/4}. ({1/2} would be P(one head and one tail in either order).)"}, {"kind": "mcq", "text": "Which representation shows all 36 outcomes of rolling two dice most clearly?", "opts": ["A 6 × 6 table", "A tree diagram with 36 end branches", "A single probability scale", "A list written in one line"], "correct": 0, "tag": "", "sol": "For two stages with many outcomes, a two-way table is compact and easy to count from."}, {"kind": "mcq", "text": "A student writes P(win) = {5/3}. Why must this be wrong?", "opts": ["A probability cannot be greater than 1", "The fraction is not simplified", "The numerator must be even", "A probability cannot be a fraction"], "correct": 0, "tag": "", "sol": "{5/3} ≈ 1.67 is more than 1, so it cannot be a probability. The numerator and denominator are probably swapped."}, {"kind": "mcq", "text": "Which statement best explains why “even” and “odd” on a six-sided die are complementary?", "opts": ["They share no outcomes and together include every outcome", "They are both events", "They both have probability {1/2}", "Even numbers are bigger than odd numbers"], "correct": 0, "tag": "", "sol": "Complementary events do not overlap and together make up the whole sample space. Having equal probabilities is not the reason."}, {"kind": "mcq", "text": "A coin landed heads 17 times in 30 tosses. Which is the clearest way to state the experimental probability?", "opts": ["P(heads) ≈ 0.57 (2 decimal places)", "P(heads) = 0.56", "P(heads) = 0.5666", "P(heads) = 57"], "correct": 0, "tag": "", "sol": "{17/30} = 0.5666…; rounded to 2 decimal places it is 0.57, and ≈ shows it is rounded. 57 without % is not a probability."}, {"kind": "mcq", "text": "A TV host says “there is a 120% chance our team wins”. What is the best mathematical response?", "opts": ["That means the team wins 120 times", "That means the team is certain to win", "That is the same as 0.12", "That is impossible: a probability cannot be more than 100%"], "correct": 3, "tag": "", "sol": "Probabilities lie from 0% to 100%. 100% would already mean certain."}, {"kind": "blank", "p": "Neha wrote this answer: “A bag has 3 red and 5 blue counters. P(red) = {3/5}, because there are 3 red and 5 blue.” Find and correct her error.", "tag": "", "marks": "", "flat": [{"t": "The total number of counters is __B1__.", "a": {"B1": "8"}}, {"t": "Neha compared the red counters with the __B1__ counters instead of with all the counters.", "a": {"B1": "blue"}, "expr": "words", "accept": ["blue ones"]}, {"t": "The correct probability is P(red) = __B1__", "a": {"B1": "3/8"}}, {"t": "{3/5} is the ratio red : blue, not a __B1__.", "a": {"B1": "probability"}, "expr": "words", "accept": ["chance"]}], "sol": "3 + 5 = 8 counters.\nShe wrote red over blue (a part : part comparison).\nP(red) = favourable ÷ total = {3/8}.\nA probability compares the favourable outcomes with all the outcomes."}, {"kind": "blank", "p": "Riya tossed a coin 10 times and got 7 heads. She says the coin is unfair. Complete the reply.", "tag": "", "marks": "", "flat": [{"t": "Her experimental probability of heads is __B1__.", "a": {"B1": "0.7"}}, {"t": "That comes from only __B1__ trials, which is not strong evidence.", "a": {"B1": "10"}}, {"t": "To test the coin fairly she should do __B1__ trials (more / fewer).", "a": {"B1": "more"}, "expr": "words", "accept": ["many more"]}], "sol": "{7/10} = 0.7.\n10 tosses.\nWith more trials, the experimental probability becomes a more reliable estimate."}]}, {"id": "s9", "label": "Test D", "sub": "Criterion D — Applying mathematics in real-life contexts", "slides": [{"kind": "mcq", "text": "At a Diwali mela lucky draw, 500 tickets are sold and you buy 4. One winning ticket is drawn. What is your chance of winning?", "opts": ["0.4%", "8%", "0.8%", "4%"], "correct": 2, "tag": "", "sol": "{4/500} = 0.008 = 0.8%."}, {"kind": "mcq", "text": "A captain won the toss in 23 of her team's last 40 matches. She claims the coin is biased in her favour. Is the claim reasonable?", "opts": ["No: {23/40} = 0.575 is close to 0.5 for only 40 tosses", "Yes: 23 is a prime number", "Yes: she won more than half", "No: she should have won exactly 20"], "correct": 0, "tag": "", "sol": "Small differences from 0.5 are normal with 40 tosses. Exactly 20 is not expected every time either."}, {"kind": "mcq", "text": "In ludo you need a 6 to start. What is the probability of not getting a 6 in either of your first two rolls?", "opts": ["{5/6}", "{25/36}", "{11/36}", "{1/36}"], "correct": 1, "tag": "", "sol": "In the 6 × 6 table, 5 × 5 = 25 cells have no 6: {25/36}."}, {"kind": "mcq", "text": "In July, the probability of rain on any day in a coastal city is 0.8. About how many rainy days would you expect in July (31 days)?", "opts": ["About 25", "About 8", "80", "31"], "correct": 0, "tag": "", "sol": "0.8 × 31 = 24.8 ≈ 25 days."}, {"kind": "mcq", "text": "A mela stall charges ₹10 a spin. The spinner has 10 equal sectors and 1 sector wins ₹50. Over 100 spins, what does the stall owner expect to gain?", "opts": ["Nothing, the game is fair", "₹50", "₹500", "₹1000"], "correct": 2, "tag": "", "sol": "Takes 100 × ₹10 = ₹1000. Expected wins = {1/10} × 100 = 10, paying 10 × ₹50 = ₹500. Gain ₹1000 − ₹500 = ₹500."}, {"kind": "mcq", "text": "A quiz has 5 questions, each with 4 options. A student who guesses every answer claims she will get about 3 right. Is this reasonable?", "opts": ["Yes: every question has a correct answer", "No: she can expect about {1/4} × 5 ≈ 1 correct", "Yes: 3 is just over half of 5", "No: she will get 0 right"], "correct": 1, "tag": "", "sol": "P(correct) = {1/4} per question, so the expected number is 1.25, about 1. Getting 3 is possible but not expected."}, {"kind": "mcq", "text": "35% of blood donors at a camp have blood group B+. About how many B+ donors would you expect among 240 donors?", "opts": ["84", "35", "156", "68"], "correct": 0, "tag": "", "sol": "0.35 × 240 = 84."}, {"kind": "blank", "p": "In a board game you need a total of exactly 10 with two dice to reach the finish.", "tag": "", "marks": "", "flat": [{"t": "P(total 10) = __B1__", "a": {"B1": "1/12"}}, {"t": "P(not reaching the finish on this turn) = __B1__", "a": {"B1": "11/12"}}, {"t": "P(total 10) as a percentage (1 decimal place) = __B1__%", "a": {"B1": "8.3"}}], "sol": "(4,6), (5,5), (6,4): {3/36} = {1/12}.\n1 − {1/12} = {11/12}.\n1 ÷ 12 = 0.0833… ≈ 8.3%."}, {"kind": "blank", "p": "A tea stall prints 1200 scratch cards. 60 cards win a free chai and 12 cards win ₹100.", "tag": "", "marks": "", "flat": [{"t": "P(a card wins a prize) = __B1__", "a": {"B1": "0.06"}}, {"t": "P(a card wins nothing) = __B1__", "a": {"B1": "0.94"}}, {"t": "If Arjun scratches 50 cards, the expected number of winning cards = __B1__", "a": {"B1": "3"}}], "sol": "60 + 12 = 72 winners; {72/1200} = 0.06.\n1 − 0.06 = 0.94.\n0.06 × 50 = 3."}, {"kind": "blank", "p": "The school bus arrives on time on 85% of days. There are 20 school days this month.", "tag": "", "marks": "", "flat": [{"t": "Expected number of on-time days = __B1__", "a": {"B1": "17"}}, {"t": "Expected number of late days = __B1__", "a": {"B1": "3"}}], "sol": "0.85 × 20 = 17.\n20 − 17 = 3 (or 0.15 × 20 = 3)."}]}];
/* ================= build slide lists ================= */
var SLIDES = {}; SECTIONS.forEach(function(sec){ SLIDES[sec.id]=sec.slides; });
var TAB_DEFS = SECTIONS.map(function(sec){ return {id:sec.id, label:sec.label, sub:sec.sub}; });

/* ================= state ================= */
function blankItem(slide){
  if(slide.kind==='mcq'||slide.kind==='ar'){
    return {status:'unanswered', attempts:0, choice:undefined};
  }
  return {status:'unanswered', curStep:0, stepStates: slide.flat.map(function(){ return {status:'unanswered', attempts:0, inputs:{}}; })};
}
function defaultState(){
  var s={tabs:{}};
  TAB_DEFS.forEach(function(td){
    s.tabs[td.id] = { idx:0, maxReached:0, items: SLIDES[td.id].map(function(sl){ return blankItem(sl); }) };
  });
  return s;
}
var appState = {mode:null, learning:null, quiz:null};
var state = null;      // alias for appState[appState.mode]
var MODE = null;
var activeTab=TAB_DEFS[0].id;

function setMode(m){
  appState.mode = m;
  if(!appState[m]) appState[m] = defaultState();
  state = appState[m];
  MODE = m;
}
/* ================= student accounts (saved on this device) ================= */
var SHEET_KEY='sopaan-myp2-u2';
var ACC_KEY='sopaan-students-v1';
var storageOK=true;
var MEM={};
function lsGet(k){ var r=null; try{ r=localStorage.getItem(k); }catch(e){ storageOK=false; }
  if(r===null||r===undefined) r=MEM[k]||null; try{ return r?JSON.parse(r):null; }catch(e){ return null; } }
function lsSet(k,v){ var j=JSON.stringify(v); MEM[k]=j; try{ localStorage.setItem(k,j); return true; }catch(e){ storageOK=false; return false; } }
try{ localStorage.setItem('sopaan-probe','1'); localStorage.removeItem('sopaan-probe'); }catch(e){ storageOK=false; }
function hashPin(pin,salt){ var h=2166136261, str=salt+'|'+pin; for(var i=0;i<str.length;i++){ h^=str.charCodeAt(i); h=Math.imul(h,16777619)>>>0; } return h.toString(36); }
function accKey(name,roll){ return (name.trim().toLowerCase().replace(/\s+/g,' ')+'#'+roll.trim().toLowerCase()); }
function accounts(){ return lsGet(ACC_KEY)||{students:{}}; }
var student=null; // {key,name,roll}
var records=[];   // finished-tab history for this student on this sheet
function progKey(){ return SHEET_KEY+'::'+student.key; }

function loadState(){
  appState = {mode:null, learning:null, quiz:null}; records=[]; activeTab=TAB_DEFS[0].id;
  var saved=lsGet(progKey());
  if(!saved) return;
  try{
    ['learning','quiz'].forEach(function(m){
      if(saved[m] && saved[m].tabs){
        var fresh = defaultState();
        TAB_DEFS.forEach(function(td){
          if(saved[m].tabs[td.id] && saved[m].tabs[td.id].items && saved[m].tabs[td.id].items.length===SLIDES[td.id].length){
            fresh.tabs[td.id] = saved[m].tabs[td.id];
          }
        });
        appState[m] = fresh;
      }
    });
    if(saved.mode==='learning' || saved.mode==='quiz') appState.mode = saved.mode;
    if(saved.activeTab && TAB_DEFS.some(function(t){ return t.id===saved.activeTab; })) activeTab = saved.activeTab;
    if(Array.isArray(saved.records)) records = saved.records;
  }catch(e){}
}
function saveState(){
  if(!student) return;
  lsSet(progKey(), {mode:appState.mode, learning:appState.learning, quiz:appState.quiz, activeTab:activeTab, records:records, updated:Date.now()});
  var acc=accounts(); if(acc.students[student.key]){ acc.students[student.key].last=Date.now(); lsSet(ACC_KEY,acc); }
}
function addRecord(mode, tabId, correct, revealed, skipped, total){
  records.unshift({at:Date.now(), mode:mode, tab:tabId, correct:correct, revealed:revealed, skipped:skipped, total:total});
  if(records.length>60) records.length=60;
  saveState();
}
function fmtDate(ms){ try{ return new Date(ms).toLocaleString(undefined,{day:'numeric',month:'short',hour:'2-digit',minute:'2-digit'}); }catch(e){ return ''; } }

function renderWho(){
  var el=document.getElementById('whoBar');
  if(!student){ el.hidden=true; el.innerHTML=''; return; }
  el.hidden=false;
  el.innerHTML='Signed in as <b>'+esc(student.name)+'</b> · Roll '+esc(student.roll)+
    ' <button id="btnRecord">My record</button> <button id="btnSignOut">Sign out</button>';
  document.getElementById('btnRecord').addEventListener('click', renderRecord);
  document.getElementById('btnSignOut').addEventListener('click', signOut);
}
function signOut(){
  saveState(); student=null; state=null; MODE=null;
  var acc=accounts(); delete acc.current; lsSet(ACC_KEY,acc);
  document.getElementById('modePill').hidden=true;
  renderWho(); renderLogin();
}
function signIn(key){
  var acc=accounts(); var st=acc.students[key]; if(!st) return;
  student={key:key, name:st.name, roll:st.roll};
  acc.current=key; st.last=Date.now(); lsSet(ACC_KEY,acc);
  loadState(); renderWho();
  if(appState.mode==='learning' || appState.mode==='quiz'){
    setMode(appState.mode); document.getElementById('modePill').hidden=false; updateModePill();
    showChrome(true); buildTabbar(); renderSlide();
    showToast('Welcome back, '+st.name.split(' ')[0]+'. Picking up where you left off.');
  } else {
    renderModePicker();
  }
}
function renderLogin(msg){
  showChrome(false);
  var acc=accounts(); var keys=Object.keys(acc.students).sort(function(a,b){ return (acc.students[b].last||0)-(acc.students[a].last||0); });
  var h='<div class="login-card"><h2>Student sign-in</h2>'+
    '<p class="lead">Sign in to save your answers, see your record and continue where you left off next time.</p>';
  if(!storageOK) h+='<div class="login-err">This browser is blocking saved data (for example, a private window). You can still practise, but progress will not be kept after you close the page.</div>';
  if(msg) h+='<div class="login-err">'+esc(msg)+'</div>';
  if(keys.length){
    h+='<div class="known"><span class="known-label">Continue as</span>';
    keys.slice(0,6).forEach(function(k){ var st=acc.students[k];
      h+='<button class="known-btn" data-k="'+esc(k)+'"><span>'+esc(st.name)+' <small>· Roll '+esc(st.roll)+'</small></span><small>'+(st.last?'Last active '+fmtDate(st.last):'')+'</small></button>'; });
    h+='</div>';
  }
  h+='<form id="loginForm" novalidate>'+
    '<div class="fld"><label for="lgName">Full name</label><input id="lgName" autocomplete="name" required></div>'+
    '<div class="fld-row"><div class="fld"><label for="lgRoll">Roll no.</label><input id="lgRoll" required></div>'+
    '<div class="fld"><label for="lgPin">4-digit PIN</label><input id="lgPin" type="password" inputmode="numeric" maxlength="4" autocomplete="off" required></div></div>'+
    '<button class="btn btn-primary" type="submit" style="width:100%;">Sign in</button>'+
    '<p class="login-note">New here? Enter your name, roll no. and a PIN you will remember, and your profile is created. Progress is saved on this device, so use the same device and browser to continue.</p>'+
    '</form></div>';
  var wrap=document.getElementById('wrap'); wrap.innerHTML=h;
  wrap.querySelectorAll('.known-btn').forEach(function(b){
    b.addEventListener('click', function(){
      var st=accounts().students[b.dataset.k];
      document.getElementById('lgName').value=st.name; document.getElementById('lgRoll').value=st.roll;
      document.getElementById('lgPin').focus();
    });
  });
  document.getElementById('loginForm').addEventListener('submit', function(e){
    e.preventDefault();
    var name=document.getElementById('lgName').value.trim(), roll=document.getElementById('lgRoll').value.trim(), pin=document.getElementById('lgPin').value.trim();
    if(!name||!roll){ renderLoginErr('Enter your name and roll no.'); return; }
    if(!/^\d{4}$/.test(pin)){ renderLoginErr('Your PIN must be exactly 4 digits.'); return; }
    var k=accKey(name,roll); var acc=accounts();
    if(acc.students[k]){
      if(acc.students[k].pin!==hashPin(pin,k)){ renderLoginErr('That PIN does not match this name and roll no. Try again.'); return; }
    } else {
      acc.students[k]={name:name.replace(/\s+/g,' '), roll:roll, pin:hashPin(pin,k), created:Date.now()};
      lsSet(ACC_KEY,acc);
    }
    signIn(k);
  });
}
function renderLoginErr(m){
  var n=document.getElementById('lgName').value, r=document.getElementById('lgRoll').value;
  renderLogin(m); document.getElementById('lgName').value=n; document.getElementById('lgRoll').value=r; document.getElementById('lgPin').focus();
}
function tabSummary(m){
  var st=appState[m]; if(!st) return null;
  return TAB_DEFS.map(function(td){
    var items=st.tabs[td.id].items, n=items.length;
    var c=items.filter(function(i){return i.status==='correct';}).length;
    var done=items.filter(function(i){return i.status!=='unanswered';}).length;
    return {label:td.label, n:n, done:done, correct:c};
  });
}
function renderRecord(){
  showChrome(false);
  var tabLabel=function(id){ var t=TAB_DEFS.find(function(x){return x.id===id;}); return t?t.label:id; };
  var h='<div class="rec-card"><h2>'+esc(student.name)+'’s record</h2><p class="rec-empty" style="margin:0 0 12px;">Roll '+esc(student.roll)+' · Probability</p>';
  ['learning','quiz'].forEach(function(m){
    var sum=tabSummary(m);
    h+='<h3>'+(m==='learning'?'📘 Learning Sheet':'📝 Quiz Mode')+'</h3>';
    if(!sum){ h+='<p class="rec-empty">Not started yet.</p>'; return; }
    h+='<div class="rec-table-wrap"><table class="rec-table"><thead><tr><th>Section</th><th>Progress</th><th>Correct first time</th></tr></thead><tbody>';
    sum.forEach(function(r){ var pct=Math.round(r.done/r.n*100);
      h+='<tr><td>'+r.label+'</td><td><span class="rec-bar"><i style="width:'+pct+'%"></i></span>'+r.done+' / '+r.n+'</td><td>'+(m==='quiz'?'—':r.correct+' / '+r.n)+'</td></tr>'; });
    h+='</tbody></table></div>';
  });
  h+='</div><div class="rec-card"><h3>Finished sections</h3>';
  if(!records.length){ h+='<p class="rec-empty">No sections finished yet. Each time you finish a tab, your score is recorded here.</p>'; }
  else {
    h+='<div class="rec-table-wrap"><table class="rec-table"><thead><tr><th>When</th><th>Mode</th><th>Section</th><th>Score</th></tr></thead><tbody>';
    records.forEach(function(r){ h+='<tr><td>'+fmtDate(r.at)+'</td><td>'+(r.mode==='quiz'?'Quiz':'Learning')+'</td><td>'+tabLabel(r.tab)+'</td><td>'+r.correct+' / '+r.total+(r.mode==='learning'?' <span class="rec-empty">('+r.revealed+' revealed, '+r.skipped+' skipped)</span>':'')+'</td></tr>'; });
    h+='</tbody></table></div>';
  }
  h+='</div><button class="btn btn-primary" id="btnBack">'+(MODE?'Back to practice':'Choose a mode')+'</button>';
  document.getElementById('wrap').innerHTML=h;
  document.getElementById('btnBack').addEventListener('click', function(){
    if(MODE){ showChrome(true); buildTabbar(); renderSlide(); } else renderModePicker();
  });
  window.scrollTo({top:0});
}

/* ================= tab bar ================= */
function buildTabbar(){
  var el=document.getElementById('tabbar');
  el.innerHTML = '<button class="tab-btn'+(activeTab==='theory'?' active':'')+'" data-tab="theory">Theory<span class="tab-count">notes</span></button>' + TAB_DEFS.map(function(t){
    return '<button class="tab-btn'+(t.id===activeTab?' active':'')+'" data-tab="'+t.id+'">'+t.label+'<span class="tab-count">'+SLIDES[t.id].length+'</span></button>';
  }).join('') + '<button class="tab-btn'+(activeTab==='report'?' active':'')+'" data-tab="report">📊 Report<span class="tab-count">pie</span></button>';
  el.querySelectorAll('.tab-btn').forEach(function(btn){
    btn.addEventListener('click', function(){ activeTab=btn.dataset.tab; buildTabbar(); renderSlide(); window.scrollTo({top:0,behavior:'smooth'}); });
  });
}

/* ================= rendering ================= */
function canProceed(item){ return item.status==='correct'||item.status==='revealed'||item.status==='skipped'; }

function renderSlide(){
  if(!state.tabs[activeTab]){ activeTab=TAB_DEFS[0].id; }
  var ts = state.tabs[activeTab];
  var slides = SLIDES[activeTab];
  if(ts.idx>=slides.length||ts.idx<0) ts.idx=0;
  var idx = ts.idx;
  var slide = slides[idx];
  var item = ts.items[idx];

  var tdef=TAB_DEFS.find(function(t){return t.id===activeTab;});
  document.getElementById('spLabel').textContent = tdef.label+' · Question '+(idx+1)+' of '+slides.length;
  document.getElementById('secSub').textContent = tdef.label+' — '+tdef.sub;
  document.getElementById('spFill').style.width = Math.round(((idx)/(Math.max(slides.length-1,1)))*100)+'%';

  var h='<div class="qcard">';
  if(slide.kind==='mcq' || slide.kind==='ar'){
    h += renderChoiceBody(slide, item, idx);
  } else {
    h += renderBlankBody(slide, item, idx);
  }
  h += '<div class="feedback" id="feedbackBox"></div>';
  if(MODE==='learning' && (item.status==='correct'||item.status==='revealed')) h += solutionHTML(slide);
  h += '</div>';

  document.getElementById('wrap').innerHTML = h;
  wireSlideEvents(slide, item, idx);
  restoreFeedback(item);
  renderNavbar(item);
  updateFab();
  saveState();
}

function solutionHTML(slide){
  if(!slide.sol) return '';
  return '<div class="solution"><div class="sol-h">Solution</div>'+slide.sol.split('\n').map(function(l){ return '<div class="sol-line">'+fr(esc(l))+'</div>'; }).join('')+'</div>';
}
var LETTERS=['a','b','c','d','e','f'];
function chosenHTML(slide,item){
  var opts = slide.kind==='mcq' ? slide.opts : AR_OPTIONS;
  if(item.choice===undefined) return '';
  var ok=item.choice===slide.correct;
  var h='<div class="chosen '+(ok?'ok':'bad')+'"><b>Your answer:</b> ('+LETTERS[item.choice]+') '+fr(esc(opts[item.choice]))+(ok?' ✓':' ✗')+'</div>';
  if(!ok) h+='<div class="chosen ok"><b>Correct answer:</b> ('+LETTERS[slide.correct]+') '+fr(esc(opts[slide.correct]))+'</div>';
  return h;
}
function renderChoiceBody(slide, item, idx){
  var h='<div class="qhead"><div class="qnum">'+(idx+1)+'</div>';
  if(slide.kind==='mcq'){
    h += '<div class="qtext">'+fr(esc(slide.text))+(slide.tag?'<span class="qtag">'+esc(slide.tag)+'</span>':'')+'</div>';
  } else {
    h += '<div class="qtext"><span class="qtag" style="margin-left:0;margin-right:6px;">A / R</span>Assertion (A): '+fr(esc(slide.a))+'<br>Reason (R): '+fr(esc(slide.r))+(slide.tag?'<span class="qtag">'+esc(slide.tag)+'</span>':'')+'</div>';
  }
  h += '</div>';
  if(slide.fig) h += '<div class="fig">'+slide.fig+'</div>';
  var opts = slide.kind==='mcq' ? slide.opts : AR_OPTIONS;
  if(MODE==='quiz') h += '<div class="quiz-hint">Quiz mode — pick an option. The correct answer is revealed once you finish this tab.</div>';
  var locked = MODE==='learning' && (item.status==='correct' || item.status==='revealed');
  h += '<div class="options">';
  opts.forEach(function(opt,i){
    var cls='opt';
    if(locked){
      if(i===slide.correct) cls+=' is-correct';
      else if(item.choice===i) cls+=' is-wrong';
      cls+=' locked';
    }
    if(item.choice===i) cls+=' is-chosen';
    h += '<label class="'+cls+'"><input type="radio" name="choice" value="'+i+'" '+(item.choice===i?'checked':'')+' '+(locked?'disabled':'')+'> <span class="opt-l">('+LETTERS[i]+')</span> <span class="opt-t">'+fr(esc(opt))+'</span>'+(item.choice===i?'<span class="opt-tag">'+(locked?(i===slide.correct?'Your answer ✓':'Your answer ✗'):'Selected')+'</span>':'')+'</label>';
  });
  h += '</div>';
  if(locked) h += chosenHTML(slide,item);
  return h;
}

function renderBlankBody(slide, item, idx){
  var h='<div class="qhead"><div class="qnum">'+(idx+1)+'</div><div class="qtext">';
  if(slide.kind==='case'){ h += '<b>'+esc(slide.title)+'.</b> '+esc(slide.body); }
  else { var pp=fr(esc(slide.p)).split('\n'); h += pp[0]+pp.slice(1).map(function(x){return '<span class="datline">'+x+'</span>';}).join(''); }
  h += (slide.tag?'<span class="qtag">'+esc(slide.tag)+'</span>':'')+'</div>';
  if(slide.marks) h += '<div class="marks-pill">'+slide.marks+'</div>';
  h += '</div>';
  if(slide.fig) h += '<div class="fig">'+slide.fig+'</div>';

  var total = slide.flat.length;
  var hasExpr = slide.flat.some(function(s){ return s.expr===true; });
  var hasFrac = slide.flat.some(function(s){ return ['fv','fe','fl','fm','fi','flist'].indexOf(s.expr)>=0; });
  var blankCount=0; slide.flat.forEach(function(s){ blankCount+=Object.keys(s.a).length; });

  if(MODE==='quiz'){
    h += '<div class="step-badge">'+blankCount+' blank'+(blankCount===1?'':'s')+' in this question</div>';
    h += '<div class="quiz-hint">Quiz mode — fill in what you can. Correct answers are revealed once you finish this tab.</div>';
    if(hasFrac) h += '<div class="quiz-hint">Type fractions like <b>3/4</b> and mixed numbers like <b>2 3/4</b> (whole number, space, fraction).</div>';
    if(hasExpr) h += '<div class="quiz-hint">Type answers like <b>2x-5</b>, <b>x^2-4</b> or <b>3+2i</b>. Use ^ for powers, | | for absolute value and pi for π. Any equivalent form is accepted.</div>';
    h += '<div class="steps">';
    for(var qi=0; qi<total; qi++){
      var qstep = slide.flat[qi];
      var qss = item.stepStates[qi];
      if(qstep.partLabel) h += '<div class="subpart-label">'+esc(qstep.partLabel)+'</div>';
      if(qstep.orDivider) h += '<div class="or-divider">OR</div>';
      var qline = fr(qstep.t);
      Object.keys(qstep.a).forEach(function(bkey){
        var val = qss.inputs[bkey]||'';
        var inp='<input class="blank-input'+(['fv','fe','fl','fm','fi','dec'].indexOf(qstep.expr)>=0?' fr':(qstep.expr==='flist'||qstep.expr==='dlist'||qstep.expr==='glist')?' wide':qstep.expr===true?' expr':(qstep.expr==='words'||qstep.expr==='list'||qstep.expr==='expanded'||qstep.expr==='set'||qstep.expr==='primes'||qstep.expr==='trans'||qstep.expr==='rot'?' wide':''))+'" data-step="'+qi+'" data-bkey="'+bkey+'" value="'+esc(val)+'">';
        qline = qline.replace('__'+bkey+'__', inp);
      });
      if(qstep.fig) h += '<div class="fig">'+qstep.fig+'</div>';
      h += '<div class="step-line">'+qline+'</div>';
    }
    h += '</div>';
    return h;
  }

  if(hasFrac) h += '<div class="quiz-hint">Type fractions like <b>3/4</b> and mixed numbers like <b>2 3/4</b> (whole number, space, fraction).</div>';
  if(hasExpr) h += '<div class="quiz-hint">Type answers like <b>2x-5</b>, <b>x^2-4</b> or <b>3+2i</b>. Use ^ for powers, | | for absolute value and pi for π. Any equivalent form is accepted.</div>';
  var locked = item.status==='correct' || item.status==='revealed';
  var upto = locked ? total-1 : item.curStep;
  h += '<div class="step-badge">Step '+(Math.min(item.curStep,total-1)+1)+' of '+total+'</div>';
  h += '<div class="steps">';
  for(var i=0;i<=upto;i++){
    var step = slide.flat[i];
    var ss = item.stepStates[i];
    if(step.partLabel) h += '<div class="subpart-label">'+esc(step.partLabel)+'</div>';
    if(step.orDivider) h += '<div class="or-divider">OR</div>';
    var resolved = ss.status==='correct' || ss.status==='revealed';
    var line = fr(step.t);
    Object.keys(step.a).forEach(function(bkey){
      var fs = ss.inputs['fs_'+bkey];
      var cls = fs==='correct'?'is-correct':(fs==='wrong'?'is-wrong':'');
      var val = ss.inputs[bkey]||'';
      var dis = resolved ? 'disabled':'';
      var inp='<input class="blank-input '+cls+(['fv','fe','fl','fm','fi','dec'].indexOf(step.expr)>=0?' fr':(step.expr==='flist'||step.expr==='dlist'||step.expr==='glist')?' wide':step.expr===true?' expr':(step.expr==='words'||step.expr==='list'||step.expr==='expanded'||step.expr==='set'||step.expr==='primes'||step.expr==='trans'||step.expr==='rot'?' wide':''))+'" data-step="'+i+'" data-bkey="'+bkey+'" value="'+esc(val)+'" '+dis+'>';
      line = line.replace('__'+bkey+'__', inp);
    });
    if(step.fig) h += '<div class="fig">'+step.fig+'</div>';
    h += '<div class="step-line'+(resolved?' resolved':'')+'">'+line+'</div>';
    if(ss.status==='revealed'){
      var reveals=[];
      Object.keys(step.a).forEach(function(bkey){ reveals.push(bkey+' = '+esc(step.a[bkey])); });
      h += '<div class="reveal-note">correct: '+reveals.join(', ')+'</div>';
    }
  }
  h += '</div>';
  return h;
}

var __flash=null;
function restoreFeedback(item){
  var fb=document.getElementById('feedbackBox'); if(!fb) return;
  if(__flash){ fb.className='feedback show '+__flash.c; fb.innerHTML=__flash.h; __flash=null; return; }
  if(MODE==='quiz'){ fb.className='feedback'; fb.innerHTML=''; return; }
  if(item.status==='correct'){ fb.className='feedback show ok'; fb.innerHTML='<b>All done — correct!</b>'; }
  else if(item.status==='revealed'){ fb.className='feedback show reveal'; fb.innerHTML='<b>Answer revealed above.</b> Move on whenever you’re ready.'; }
  else if(item.status==='skipped'){ fb.className='feedback show retry'; fb.innerHTML='Skipped — you can answer it now: fill it in and press <b>Check</b>, or come back later from the Question Palette.'; }
  else { fb.className='feedback'; fb.innerHTML=''; }
}

function slideIsAnswered(slide,item){
  if(slide.kind==='mcq'||slide.kind==='ar') return item.choice!==undefined;
  return item.stepStates.some(function(ss){ return Object.keys(ss.inputs).some(function(k){ return k.indexOf('fs_')!==0 && ss.inputs[k] && String(ss.inputs[k]).trim()!==''; }); });
}
function renderNavbar(item){
  var ts = state.tabs[activeTab];
  var slides = SLIDES[activeTab];
  var slide = slides[ts.idx];
  var isLast = ts.idx === slides.length-1;
  var h='';
  h += '<button class="btn" id="btnPrev" '+(ts.idx===0?'disabled':'')+'>&larr; Previous</button>';

  if(MODE==='quiz'){
    h += '<button class="btn" id="btnSkip">Skip</button>';
    h += '<span class="spacer"></span>';
    h += '<button class="btn btn-primary" id="btnNext">'+(isLast?'Finish':'Next →')+'</button>';
  } else {
    h += '<button class="btn" id="btnSkip" '+(item.status!=='unanswered'?'disabled':'')+'>Skip</button>';
    h += '<span class="spacer"></span>';
    h += '<button class="btn btn-primary" id="btnCheck" '+((item.status!=='unanswered'&&item.status!=='skipped')?'disabled':'')+'>Check</button>';
    h += '<button class="btn btn-primary" id="btnNext" '+(canProceed(item)?'':'disabled')+'>'+(isLast?'Finish':'Next →')+'</button>';
  }
  document.getElementById('navbarInner').innerHTML = h;

  document.getElementById('btnPrev').addEventListener('click', function(){ if(ts.idx>0){ ts.idx--; renderSlide(); } });

  if(MODE==='quiz'){
    document.getElementById('btnSkip').addEventListener('click', function(){
      item.status='skipped';
      advanceQuiz(ts, slides);
    });
    document.getElementById('btnNext').addEventListener('click', function(){
      item.status = slideIsAnswered(slide,item) ? 'answered' : 'unanswered';
      advanceQuiz(ts, slides);
    });
  } else {
    document.getElementById('btnSkip').addEventListener('click', function(){
      if(item.status==='unanswered'){ item.status='skipped'; renderSlide(); }
    });
    document.getElementById('btnNext').addEventListener('click', function(){
      if(!canProceed(item)) return;
      if(ts.idx < slides.length-1){ ts.idx++; ts.maxReached=Math.max(ts.maxReached, ts.idx); renderSlide(); }
      else { renderFinished(); }
    });
    document.getElementById('btnCheck').addEventListener('click', function(){ handleCheck(); });
  }
}
function advanceQuiz(ts, slides){
  if(ts.idx < slides.length-1){ ts.idx++; ts.maxReached=Math.max(ts.maxReached, ts.idx); renderSlide(); }
  else { renderFinished(); }
}

function wireSlideEvents(slide, item, idx){
  document.querySelectorAll('input[type="radio"][name="choice"]').forEach(function(r){
    r.addEventListener('change', function(){ item.choice=parseInt(r.value,10); saveState();
      document.querySelectorAll('.opt').forEach(function(l){ l.classList.remove('is-chosen'); var t=l.querySelector('.opt-tag'); if(t) t.remove(); });
      var lab=r.closest('.opt'); lab.classList.add('is-chosen'); var tg=document.createElement('span'); tg.className='opt-tag'; tg.textContent='Selected'; lab.appendChild(tg); });
  });
  document.querySelectorAll('.blank-input').forEach(function(inp){
    inp.addEventListener('input', function(){
      var si=parseInt(inp.dataset.step,10), bk=inp.dataset.bkey;
      item.stepStates[si].inputs[bk]=inp.value;
      saveState();
    });
  });
}

function handleCheck(){
  var ts = state.tabs[activeTab];
  var slide = SLIDES[activeTab][ts.idx];
  var item = ts.items[ts.idx];
  if(item.status==='skipped') item.status='unanswered';
  var fb = document.getElementById('feedbackBox');

  if(slide.kind==='mcq' || slide.kind==='ar'){
    if(item.choice===undefined){ fb.className='feedback show err'; fb.innerHTML='Please choose an option first.'; return; }
    var ok = item.choice===slide.correct;
    if(ok){
      item.status='correct'; playSuccess();
      fb.className='feedback show ok'; fb.innerHTML='<b>Correct!</b>';
    } else {
      item.attempts=(item.attempts||0)+1;
      if(item.attempts>=2){ item.status='revealed'; playReveal(); }
      else { playWrong(); fb.className='feedback show retry'; fb.innerHTML='<b>Not quite — try again</b> (attempt 1 of 2) <span class="attempts-dots"><span class="used"></span><span></span></span>'; __flash={c:'retry',h:'<b>Not quite — try again</b> (attempt 1 of 2) <span class="attempts-dots"><span class="used"></span><span></span></span>'}; }
    }
    renderSlide();
    return;
  }

  // blank / case
  var step = slide.flat[item.curStep];
  var ss = item.stepStates[item.curStep];
  var blanks = Object.keys(step.a);
  var missing = blanks.some(function(k){ return !ss.inputs[k] || String(ss.inputs[k]).trim()===''; });
  if(missing){ fb.className='feedback show err'; fb.innerHTML='Fill in every blank in this step before checking.'; return; }

  var allGood=true;
  blanks.forEach(function(k){
    var good=answerMatches(ss.inputs[k], step.a[k], step.accept, step.expr);
    ss.inputs['fs_'+k]=good?'correct':'wrong';
    if(!good) allGood=false;
  });

  if(allGood){
    ss.status='correct'; playSuccess();
    advanceStepOrFinish(item, slide);
  } else {
    ss.attempts=(ss.attempts||0)+1;
    if(ss.attempts>=2){
      ss.status='revealed'; playReveal();
      advanceStepOrFinish(item, slide);
    } else {
      playWrong();
      fb.className='feedback show retry';
      fb.innerHTML='<b>Not quite — try again</b> (attempt 1 of 2) <span class="attempts-dots"><span class="used"></span><span></span></span>'; __flash={c:'retry',h:'<b>Not quite — try again</b> (attempt 1 of 2) <span class="attempts-dots"><span class="used"></span><span></span></span>'};
      renderSlide();
      return;
    }
  }
  renderSlide();
}

function advanceStepOrFinish(item, slide){
  var total = slide.flat.length;
  var hasExpr = slide.flat.some(function(s){ return s.expr===true; });
  var hasFrac = slide.flat.some(function(s){ return ['fv','fe','fl','fm','fi','flist'].indexOf(s.expr)>=0; });
  if(item.curStep < total-1){
    item.curStep++;
  } else {
    var anyRevealed = item.stepStates.some(function(ss){ return ss.status==='revealed'; });
    item.status = anyRevealed ? 'revealed' : 'correct';
  }
}

/* ================= palette ================= */
var overlay=document.getElementById('overlay');
function statusOf(tabId,i){
  var ts=state.tabs[tabId];
  if(i>ts.maxReached) return 'locked';
  if(i===ts.idx) return 'current';
  return ts.items[i].status==='unanswered' ? 'locked' : ts.items[i].status;
}
function buildPalette(){
  var slides=SLIDES[activeTab];
  document.getElementById('paletteTitle').textContent = TAB_DEFS.find(function(t){return t.id===activeTab;}).label+' · Question palette';
  var body=document.getElementById('paletteBody');
  var html='';
  for(var i=0;i<slides.length;i++){
    html += '<div class="chip" data-status="'+statusOf(activeTab,i)+'" data-idx="'+i+'">'+(i+1)+'</div>';
  }
  body.innerHTML = html;
  body.querySelectorAll('.chip').forEach(function(chip){
    chip.addEventListener('click', function(){
      var i=parseInt(chip.dataset.idx,10);
      var ts=state.tabs[activeTab];
      if(i>ts.maxReached){ showToast('Finish the earlier questions to unlock this one.'); return; }
      ts.idx=i; closePalette(); renderSlide();
    });
  });
}
function openPalette(){ buildPalette(); overlay.classList.add('show'); }
function closePalette(){ overlay.classList.remove('show'); }
var toastTimer;
function showToast(msg){
  var t=document.getElementById('toast'); t.textContent=msg; t.classList.add('show');
  clearTimeout(toastTimer); toastTimer=setTimeout(function(){ t.classList.remove('show'); },2200);
}
document.getElementById('paletteFab').addEventListener('click', openPalette);
document.getElementById('paletteClose').addEventListener('click', closePalette);
overlay.addEventListener('click', function(e){ if(e.target===overlay) closePalette(); });

function updateFab(){
  var slides=SLIDES[activeTab];
  var done=state.tabs[activeTab].items.filter(function(it){ return it.status==='correct'||it.status==='revealed'||it.status==='skipped'; }).length;
  document.getElementById('fabBadge').textContent = done+'/'+slides.length;
}

/* ================= overall progress + finish ================= */
function updateChapterProgress(){
  var total=0, done=0;
  TAB_DEFS.forEach(function(td){
    state.tabs[td.id].items.forEach(function(it){
      total++;
      if(it.status==='correct'||it.status==='revealed'||it.status==='skipped') done++;
    });
  });
  var pct = total>0 ? Math.round((done/total)*100) : 0;
  document.getElementById('cpFill').style.width=pct+'%';
  document.getElementById('cpLabel').textContent=pct+'% complete';
}
function isSlideCorrect(tabId,i){
  var slide=SLIDES[tabId][i], item=state.tabs[tabId].items[i];
  if(slide.kind==='mcq'||slide.kind==='ar') return item.choice===slide.correct;
  return slide.flat.every(function(step,si){
    var ss=item.stepStates[si];
    return Object.keys(step.a).every(function(k){ return answerMatches(ss.inputs[k], step.a[k], step.accept, step.expr); });
  });
}
function isSlideAttempted(tabId,i){
  var slide=SLIDES[tabId][i], item=state.tabs[tabId].items[i];
  if(slide.kind==='mcq'||slide.kind==='ar') return item.choice!==undefined;
  return slide.flat.some(function(step,si){
    var ss=item.stepStates[si];
    return Object.keys(step.a).some(function(k){ return ss.inputs[k] && String(ss.inputs[k]).trim()!==''; });
  });
}
function slideLabel(slide){
  var t = slide.kind==='case' ? slide.title : (slide.kind==='ar' ? slide.a : (slide.p||slide.text||''));
  t = String(t);
  return t.length>90 ? t.slice(0,90)+'…' : t;
}
function slideAnswerText(slide, item, correctSide){
  if(slide.kind==='mcq'||slide.kind==='ar'){
    var opts = slide.kind==='mcq' ? slide.opts : AR_OPTIONS;
    if(correctSide) return '('+LETTERS[slide.correct]+') '+opts[slide.correct];
    return item.choice!==undefined ? '('+LETTERS[item.choice]+') '+opts[item.choice] : '— not attempted —';
  }
  return slide.flat.map(function(step,si){
    var ss=item.stepStates[si];
    return Object.keys(step.a).map(function(k){
      return correctSide ? step.a[k] : (ss.inputs[k] || '—');
    }).join(', ');
  }).join('  |  ');
}

function renderFinished(){
  if(MODE==='quiz'){ renderQuizResults(); return; }
  var ts=state.tabs[activeTab];
  var correct=ts.items.filter(function(i){return i.status==='correct';}).length;
  var revealed=ts.items.filter(function(i){return i.status==='revealed';}).length;
  var skipped=ts.items.filter(function(i){return i.status==='skipped';}).length;
  var h='<div class="qcard done-card"><div class="big">✨</div><h2>Tab complete</h2>';
  h+='<p style="color:var(--ink-soft);font-size:14px;">You worked through all '+ts.items.length+' questions in this tab.</p>';
  if(!ts.recorded){ ts.recorded=true; addRecord('learning', activeTab, correct, revealed, skipped, ts.items.length); }
  h+='<div class="score-pills">'+
     '<span class="score-pill" style="background:var(--success-soft);color:var(--success);">'+correct+' correct</span>'+
     '<span class="score-pill" style="background:var(--danger-soft);color:var(--danger);">'+revealed+' revealed</span>'+
     '<span class="score-pill" style="background:var(--gold-soft);color:var(--retry-text);">'+skipped+' skipped</span></div>'+
     '<button class="btn btn-primary" id="btnReview">Review from question 1</button></div>';
  document.getElementById('wrap').innerHTML=h;
  document.getElementById('navbarInner').innerHTML='';
  document.getElementById('btnReview').addEventListener('click', function(){ ts.idx=0; ts.recorded=false; renderSlide(); });
  updateChapterProgress(); saveState();
}

function renderQuizResults(){
  var ts=state.tabs[activeTab];
  var slides=SLIDES[activeTab];
  var correctCount=0, attemptedCount=0;
  var rowsHtml = slides.map(function(slide,i){
    var item=ts.items[i];
    var ok=isSlideCorrect(activeTab,i);
    var att=isSlideAttempted(activeTab,i);
    if(ok) correctCount++;
    if(att) attemptedCount++;
    var tagCls = ok?'ok':(att?'bad':'na');
    var tagText = ok?'Correct':(att?'Incorrect':'Not attempted');
    var row='<div class="review-row">';
    row+='<span class="review-tag '+tagCls+'">Q'+(i+1)+' · '+tagText+'</span>';
    row+='<div class="review-q">'+fr(esc(slideLabel(slide)))+'</div>';
    row+='<div class="review-ans"><b>Your answer:</b> '+fr(esc(slideAnswerText(slide,item,false)))+'</div>';
    if(!ok) row+='<div class="review-ans" style="color:var(--success);"><b>Correct answer:</b> '+fr(esc(slideAnswerText(slide,item,true)))+'</div>';
    if(slide.sol) row+='<details class="rev-sol"><summary>Show solution</summary>'+solutionHTML(slide)+'</details>';
    row+='</div>';
    return row;
  }).join('');

  addRecord('quiz', activeTab, correctCount, 0, 0, slides.length);
  var h='<div class="qcard done-card" style="text-align:left;">';
  h+='<div style="text-align:center;"><div class="big">🏁</div><h2>Quiz complete</h2>';
  h+='<p style="color:var(--ink-soft);font-size:14px;">'+correctCount+' / '+slides.length+' correct · '+attemptedCount+' attempted</p></div>';
  h+='<div style="margin-top:16px;">'+rowsHtml+'</div>';
  h+='<div style="text-align:center;margin-top:6px;"><button class="btn btn-primary" id="btnReview">Review from question 1</button></div>';
  h+='</div>';
  document.getElementById('wrap').innerHTML=h;
  document.getElementById('navbarInner').innerHTML='';
  document.getElementById('btnReview').addEventListener('click', function(){ ts.idx=0; renderSlide(); });
  playSuccess();
  updateChapterProgress(); saveState();
}

/* ================= mode picker ================= */
function updateModePill(){
  var btn=document.getElementById('modePill');
  if(!btn) return;
  btn.textContent = MODE==='quiz' ? '📝 Quiz Mode · switch' : '📘 Learning Sheet · switch';
}
function showChrome(show){
  document.querySelector('.tabbar').style.display = show?'':'none';
  document.querySelector('.slide-progress').style.display = show?'':'none';
  document.querySelector('.navbar').style.display = show?'':'none';
  document.getElementById('paletteFab').style.display = show?'':'none';
}
function renderModePicker(){
  showChrome(false);
  var wrap=document.getElementById('wrap');
  wrap.innerHTML =
    '<div class="mode-pick">'+
      '<div class="mode-card" data-pick="learning"><div class="mode-icon">📘</div><h3>Learning Sheet</h3>'+
      '<p>Work through each question step by step with instant feedback — two tries, then the correct value is revealed right there so you can keep moving.</p></div>'+
      '<div class="mode-card" data-pick="quiz"><div class="mode-icon">📝</div><h3>Quiz Mode</h3>'+
      '<p>Attempt every question with no hints along the way. Your score and the full answer key are shown together once you finish the tab — just like the real exam.</p></div>'+
    '</div>';
  wrap.querySelectorAll('[data-pick]').forEach(function(card){
    card.addEventListener('click', function(){
      setMode(card.dataset.pick); activeTab='theory';
      document.getElementById('modePill').hidden=false;
      updateModePill();
      showChrome(true);
      buildTabbar();
      renderSlide();
    });
  });
}
document.getElementById('modePill').addEventListener('click', function(){
  if(!appState.mode){ return; }
  setMode(appState.mode==='quiz' ? 'learning' : 'quiz');
  updateModePill();
  showChrome(true);
  buildTabbar();
  renderSlide();
});

/* ================= boot ================= */
var _origRenderSlide = renderSlide;
renderSlide = function(){ _origRenderSlide(); updateChapterProgress(); updateModePill(); };


/* ================= theory tab ================= */
function renderTheory(){
  var wrap=document.getElementById('wrap');
  wrap.innerHTML='<div class="theory">'+fr(THEORY)+'</div>';
  document.querySelector('.slide-progress').style.display='none';
  document.querySelector('.navbar').style.display='none';
  document.getElementById('paletteFab').style.display='none';
  document.getElementById('secSub').textContent='Theory — notes, key ideas and worked examples';
  wrap.querySelectorAll('[data-go]').forEach(function(b){ b.addEventListener('click',function(){ activeTab=b.dataset.go; buildTabbar(); renderSlide(); window.scrollTo({top:0,behavior:'smooth'}); }); });
  wrap.querySelectorAll('[data-jump]').forEach(function(b){ b.addEventListener('click',function(){ var t=document.getElementById(b.dataset.jump); if(t) t.scrollIntoView({behavior:'smooth',block:'start'}); }); });
}
var _rsTheory = renderSlide;
renderSlide = function(){
  if(activeTab==='theory'){ renderTheory(); buildTabbar(); updateChapterProgress(); updateModePill(); saveState(); return; }
  document.querySelector('.slide-progress').style.display='';
  document.querySelector('.navbar').style.display='';
  document.getElementById('paletteFab').style.display='';
  _rsTheory();
};


/* ================= MYP criterion report ================= */
var CRIT_COL={A:'var(--accent-text)',B:'var(--gold)',C:'#8A6FC4',D:'#2F9AA0'};
function critStats(){
  return REPORT.map(function(r){
    var tabId=r[0], slides=SLIDES[tabId], ts=state&&state.tabs[tabId];
    var c=0,w=0,n=0;
    slides.forEach(function(sl,i){
      if(!ts||!ts.items[i]){ n++; return; }
      var it=ts.items[i];
      if(MODE==='learning'){
        if(it.status==='correct') c++; else if(it.status==='revealed') w++; else n++;
      } else {
        if(isSlideCorrect(tabId,i)) c++; else if(isSlideAttempted(tabId,i)) w++; else n++;
      }
    });
    var tot=slides.length, pct=tot?Math.round(c/tot*100):0;
    var lvl=Math.round(c/Math.max(tot,1)*8);
    return {tab:tabId,letter:r[1],name:r[2],c:c,w:w,n:n,tot:tot,pct:pct,lvl:lvl};
  });
}
function arcPath(cx,cy,r,a0,a1){
  if(a1-a0>=Math.PI*2-1e-6){ return 'M'+(cx-r)+','+cy+' a'+r+','+r+' 0 1,0 '+(2*r)+',0 a'+r+','+r+' 0 1,0 '+(-2*r)+',0 Z'; }
  var x0=cx+r*Math.sin(a0), y0=cy-r*Math.cos(a0), x1=cx+r*Math.sin(a1), y1=cy-r*Math.cos(a1);
  return 'M'+cx+','+cy+' L'+x0.toFixed(2)+','+y0.toFixed(2)+' A'+r+','+r+' 0 '+((a1-a0)>Math.PI?1:0)+',1 '+x1.toFixed(2)+','+y1.toFixed(2)+' Z';
}
function pieSVG(parts,size,hole,center,sub){
  var tot=parts.reduce(function(s,p){return s+p.v;},0), cx=size/2, cy=size/2, r=size/2-4, a=0, h='';
  if(!tot){ h+='<circle cx="'+cx+'" cy="'+cy+'" r="'+r+'" style="fill:var(--paper-2);stroke:var(--rule)"/>'; }
  parts.forEach(function(p){ if(!p.v) return; var b=a+p.v/tot*Math.PI*2; h+='<path d="'+arcPath(cx,cy,r,a,b)+'" style="fill:'+p.col+';stroke:var(--card);stroke-width:2"><title>'+esc(p.label)+': '+p.v+'</title></path>'; a=b; });
  if(hole) h+='<circle cx="'+cx+'" cy="'+cy+'" r="'+(r*hole)+'" style="fill:var(--card)"/>';
  if(center) h+='<text x="'+cx+'" y="'+(cy-(sub?4:0))+'" text-anchor="middle" dominant-baseline="middle" style="fill:var(--ink);font:700 '+(size>150?22:15)+'px Fraunces,serif">'+center+'</text>';
  if(sub) h+='<text x="'+cx+'" y="'+(cy+16)+'" text-anchor="middle" style="fill:var(--ink-soft);font:600 11px \'Source Sans 3\',sans-serif">'+sub+'</text>';
  return '<svg viewBox="0 0 '+size+' '+size+'" width="'+size+'" height="'+size+'" role="img">'+h+'</svg>';
}
function band(l){ return l===0?'0':(l<=2?'1–2':(l<=4?'3–4':(l<=6?'5–6':'7–8'))); }
function renderReport(){
  var st=critStats(), totC=0, totQ=0;
  st.forEach(function(s){ totC+=s.c; totQ+=s.tot; });
  var h='<div class="theory">';
  h+='<section class="note"><h2>Chapter test report</h2><p class="lt">'+(MODE==='quiz'?'<b>Quiz mode</b> results: answers are marked when you finish each test tab.':'<b>Learning mode</b> results: a question counts as correct only if you got it without the answer being revealed. Switch to Quiz mode for an exam-style score.')+'</p>';
  h+='<div class="rep-top"><div class="rep-pie">'+pieSVG(st.map(function(s){return {v:s.c,col:CRIT_COL[s.letter],label:'Criterion '+s.letter};}),200,0.55,totC+'/'+totQ,'correct')+'</div>';
  h+='<div class="rep-legend"><div class="rep-cap">Marks earned, by criterion</div>'+st.map(function(s){ return '<div class="rep-li"><span class="sw" style="background:'+CRIT_COL[s.letter]+'"></span><b>'+s.letter+'</b>&nbsp;'+esc(s.name)+'<span class="rep-n">'+s.c+' / '+s.tot+'</span></div>'; }).join('')+'</div></div></section>';
  h+='<div class="rep-grid">'+st.map(function(s){
    return '<div class="rep-card"><div class="rep-h"><span class="crit" style="background:'+CRIT_COL[s.letter]+'">'+s.letter+'</span>'+esc(s.name)+'</div>'+
      '<div class="rep-row">'+pieSVG([{v:s.c,col:'var(--success)',label:'Correct'},{v:s.w,col:'var(--danger)',label:'Incorrect'},{v:s.n,col:'var(--locked)',label:'Not attempted'}],120,0.5,s.pct+'%')+
      '<div class="rep-stats"><div><span class="sw" style="background:var(--success)"></span>Correct <b>'+s.c+'</b></div><div><span class="sw" style="background:var(--danger)"></span>Incorrect <b>'+s.w+'</b></div><div><span class="sw" style="background:var(--locked)"></span>Not attempted <b>'+s.n+'</b></div>'+
      '<div class="rep-lvl">Indicative level <b>'+s.lvl+'</b> / 8 <span>(band '+band(s.lvl)+')</span></div></div></div>'+
      '<button class="hub-btn" data-go="'+s.tab+'">Open Test '+s.letter+' →</button></div>';
  }).join('')+'</div>';
  h+='<p class="rep-note">Indicative level = fraction of questions correct × 8, rounded. It is a practice guide only; your teacher awards the real MYP criterion levels.</p></div>';
  var wrap=document.getElementById('wrap'); wrap.innerHTML=h;
  document.querySelector('.slide-progress').style.display='none';
  document.querySelector('.navbar').style.display='none';
  document.getElementById('paletteFab').style.display='none';
  document.getElementById('secSub').textContent='Report — chapter test results by MYP criterion';
  wrap.querySelectorAll('[data-go]').forEach(function(b){ b.addEventListener('click',function(){ activeTab=b.dataset.go; buildTabbar(); renderSlide(); window.scrollTo({top:0,behavior:'smooth'}); }); });
}
var _rsReport = renderSlide;
renderSlide = function(){
  if(activeTab==='report'){ renderReport(); buildTabbar(); updateChapterProgress(); updateModePill(); saveState(); return; }
  _rsReport();
};


/* ================= skipped-question return ================= */
function firstSkippedIdx(ts){
  for(var i=0;i<ts.items.length;i++){
    var it=ts.items[i];
    if(MODE==='quiz'){ if(!isSlideAttempted(activeTab,i)) return i; }
    else if(it.status==='skipped'||it.status==='unanswered') return i;
  }
  return -1;
}
function skippedList(ts){
  var out=[]; for(var i=0;i<ts.items.length;i++){ var it=ts.items[i];
    if(MODE==='quiz' ? !isSlideAttempted(activeTab,i) : (it.status==='skipped'||it.status==='unanswered')) out.push(i); }
  return out;
}
function renderSkipGate(ts){
  var list=skippedList(ts);
  var h='<div class="qcard done-card"><div class="big">⏭️</div><h2>You skipped '+list.length+' question'+(list.length>1?'s':'')+'</h2>'+
    '<p style="color:var(--ink-soft);font-size:14px;">Answer them now, or submit the tab as it is. Answers are marked only when you submit.</p>'+
    '<div class="skip-chips">'+list.map(function(i){ return '<button class="chip skip-go" data-i="'+i+'">Q'+(i+1)+'</button>'; }).join('')+'</div>'+
    '<div class="skip-btns"><button class="btn btn-primary" id="btnGoSkipped">Answer skipped questions</button><button class="btn" id="btnSubmitAnyway">Submit anyway</button></div></div>';
  document.getElementById('wrap').innerHTML=h;
  document.getElementById('navbarInner').innerHTML='';
  var go=function(i){ ts.idx=i; ts.maxReached=Math.max(ts.maxReached,i); renderSlide(); window.scrollTo({top:0,behavior:'smooth'}); };
  document.getElementById('btnGoSkipped').addEventListener('click',function(){ go(list[0]); });
  document.querySelectorAll('.skip-go').forEach(function(b){ b.addEventListener('click',function(){ go(parseInt(b.dataset.i,10)); }); });
  document.getElementById('btnSubmitAnyway').addEventListener('click',function(){ ts.submitOK=true; renderFinished(); ts.submitOK=false; });
  saveState();
}
var _rfSkip = renderFinished;
renderFinished = function(){
  var ts=state.tabs[activeTab];
  if(MODE==='quiz' && !ts.submitOK && skippedList(ts).length){ renderSkipGate(ts); return; }
  _rfSkip();
  if(MODE==='learning'){
    var list=skippedList(ts);
    if(list.length){
      var card=document.querySelector('.done-card');
      if(card){ var d=document.createElement('div'); d.className='skip-btns';
        d.innerHTML='<button class="btn btn-primary" id="btnGoSkipped">Attempt skipped questions ('+list.length+')</button>';
        card.appendChild(d);
        document.getElementById('btnGoSkipped').addEventListener('click',function(){ ts.idx=list[0]; ts.recorded=false; renderSlide(); window.scrollTo({top:0,behavior:'smooth'}); }); }
    }
  }
};

renderLogin();
})();
</script>
</body>
</html>
