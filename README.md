<!DOCTYPE html>

<html lang="fr">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover"/>
<meta name="theme-color" content="#1a472a"/>
<meta name="color-scheme" content="light only"/>
<meta name="description" content="Jurisprudence sénégalaise avec IA · Cour Suprême · Conseil Constitutionnel"/>
<meta name="apple-mobile-web-app-capable" content="yes"/>
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent"/>
<meta name="apple-mobile-web-app-title" content="JuriSen"/>
<link rel="manifest" href="manifest.json"/>
<link rel="apple-touch-icon" href="icons/icon-192.png"/>
<title>JuriSen — Jurisprudence Sénégalaise</title>
<link rel="preconnect" href="https://fonts.googleapis.com"/>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet"/>
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2/dist/umd/supabase.js"></script>
<style>
/* ── FORCE LIGHT MODE — bloque le dark mode iOS/Safari/Koder ── */
:root{
  color-scheme:light only;
  --green:#1a472a;--green-mid:#2d6a4f;--green-light:#52b788;
  --gold:#d4a017;--gold-light:#f4c542;
  --cream:#f8f4ec;--paper:#fefcf7;--ink:#1c1a16;--ink-soft:#4a4540;
  --border:#e0d8c8;--shadow:rgba(26,71,42,.12);
  --wave-color:#0088cc;--red:#c62828;--red-bg:#fce4ec;
}
html{color-scheme:light only;background:#f8f4ec;}
@media(prefers-color-scheme:dark){
  html,body,div,section,main,article,nav,aside,header,footer{
    background-color:inherit !important;color:inherit !important;
  }
  :root{color-scheme:light;}
  html{background:#f8f4ec !important;}
  body{background:#f8f4ec !important;color:#1c1a16 !important;}
}
*{box-sizing:border-box;margin:0;padding:0;}
body{background:#f8f4ec;font-family:'DM Sans',sans-serif;color:#1c1a16;min-height:100vh;-webkit-text-size-adjust:100%;}
@keyframes spin{to{transform:rotate(360deg)}}
@keyframes pulse{0%,100%{opacity:1;transform:scale(1)}50%{opacity:.5;transform:scale(.85)}}
@keyframes fadeUp{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:translateY(0)}}
@keyframes modalIn{from{opacity:0;transform:scale(.94) translateY(8px)}to{opacity:1;transform:scale(1) translateY(0)}}
@keyframes bounce{0%,80%,100%{transform:translateY(0);opacity:.5}40%{transform:translateY(-6px);opacity:1}}

/* HEADER */
header{background:#1a472a;padding:0 1.5rem;display:flex;align-items:center;justify-content:space-between;height:64px;position:sticky;top:0;z-index:300;box-shadow:0 2px 16px rgba(0,0,0,.25);}
.logo{display:flex;align-items:center;gap:10px;cursor:pointer;}
.logo-icon{width:36px;height:36px;background:#d4a017;border-radius:8px;display:flex;align-items:center;justify-content:center;font-size:18px;}
.logo-text{font-family:‘Playfair Display’,serif;font-size:1.35rem;color:#fff;}
.logo-text span{color:#f4c542;}
.hdr-right{display:flex;align-items:center;gap:8px;}
.hdr-badge{background:rgba(255,255,255,.12);color:#fff;font-size:.7rem;padding:4px 10px;border-radius:20px;border:1px solid rgba(255,255,255,.2);}
.user-btn{display:flex;align-items:center;gap:7px;padding:7px 13px;background:rgba(255,255,255,.15);color:#fff;border:1px solid rgba(255,255,255,.25);border-radius:20px;font-family:‘DM Sans’,sans-serif;font-size:.8rem;cursor:pointer;transition:all .15s;}
.user-btn:hover{background:rgba(255,255,255,.28);}
.sub-dot{width:7px;height:7px;border-radius:50%;}
.dot-active{background:#4caf50;}.dot-pending{background:#d4a017;}.dot-none{background:#ef5350;}

/* HERO */
.hero{background:linear-gradient(135deg,#1a472a 0%,#2d6a4f 60%,#1e5c3a 100%);padding:2rem 2rem 0;text-align:center;position:relative;overflow:hidden;}
.hero::before{content:’’;position:absolute;inset:0;background:url(“data:image/svg+xml,%3Csvg width=‘60’ height=‘60’ xmlns=‘http://www.w3.org/2000/svg’%3E%3Cg fill=’%23fff’ fill-opacity=’.03’%3E%3Cpath d=‘M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0-30V0h-2v4h-4v2h4v4h2V6h4V4h-4zM6 34v-4H4v4H0v2h4v4h2v-4h4v-2H6zM6 4V0H4v4H0v2h4v4h2V6h4V4H6z’/%3E%3C/g%3E%3C/svg%3E”);}
.hero h1{font-family:‘Playfair Display’,serif;font-size:clamp(1.35rem,3.5vw,2.1rem);color:#fff;line-height:1.2;margin-bottom:.4rem;position:relative;}
.hero p{color:rgba(255,255,255,.72);font-size:.87rem;font-weight:300;margin-bottom:1.5rem;position:relative;}
.tabs-nav{display:flex;align-items:flex-end;gap:3px;justify-content:center;position:relative;z-index:1;padding:0 .5rem;flex-wrap:wrap;}
.tab-btn{display:flex;align-items:center;gap:6px;padding:9px 15px;border:none;border-radius:10px 10px 0 0;font-family:‘DM Sans’,sans-serif;font-size:.82rem;font-weight:500;cursor:pointer;transition:all .18s;background:rgba(255,255,255,.13);color:rgba(255,255,255,.7);}
.tab-btn:hover{background:rgba(255,255,255,.22);color:#fff;}
.tab-btn.active{background:#f8f4ec;color:#1a472a;font-weight:600;}
.tab-badge{background:#d4a017;color:#1c1a16;font-size:.62rem;font-weight:700;padding:1px 6px;border-radius:10px;min-width:18px;text-align:center;display:none;}
.tab-badge.visible{display:inline-block;}
.tab-panel{display:none;background:#f8f4ec;}.tab-panel.active{display:block;}

/* INSTALL BANNER */
#installBanner{display:none;align-items:center;gap:12px;background:linear-gradient(135deg,#1565c0,#0d47a1);padding:.85rem 1.5rem;color:#fff;}
#installBanner.show{display:flex;}
.inst-text{flex:1;font-size:.82rem;}.inst-text strong{display:block;font-size:.88rem;margin-bottom:1px;}
.btn-inst{padding:7px 14px;background:#fff;color:#1565c0;border:none;border-radius:8px;font-family:‘DM Sans’,sans-serif;font-size:.8rem;font-weight:600;cursor:pointer;}
.btn-inst-x{background:none;border:none;color:rgba(255,255,255,.7);cursor:pointer;font-size:1.2rem;padding:4px;}

/* PAYWALL */
.paywall-wrap{padding:1rem 2rem;}
.paywall-banner{background:linear-gradient(135deg,#1a472a,#2d6a4f);border-radius:14px;padding:1.2rem 1.4rem;display:flex;align-items:center;gap:14px;animation:fadeUp .3s;}
.pw-icon{font-size:1.9rem;flex-shrink:0;}
.pw-text h3{font-family:‘Playfair Display’,serif;color:#fff;font-size:.98rem;margin-bottom:3px;}
.pw-text p{color:rgba(255,255,255,.78);font-size:.78rem;}
.btn-unlock{padding:9px 18px;background:#d4a017;color:#1c1a16;border:none;border-radius:10px;font-family:‘DM Sans’,sans-serif;font-size:.83rem;font-weight:600;cursor:pointer;white-space:nowrap;flex-shrink:0;}
.btn-unlock:hover{background:#f4c542;}

/* OVERLAY + MODALS */
.ov{display:none;position:fixed;inset:0;background:rgba(0,0,0,.6);z-index:500;align-items:center;justify-content:center;padding:1rem;backdrop-filter:blur(3px);}
.ov.open{display:flex;}
.mbox{background:#fff;border-radius:20px;padding:2rem;max-width:460px;width:100%;max-height:90vh;overflow-y:auto;box-shadow:0 24px 80px rgba(0,0,0,.3);animation:modalIn .22s ease;}
.mbox.wide{max-width:540px;}
.mhead{display:flex;align-items:center;justify-content:space-between;margin-bottom:1.4rem;}
.mhead h2{font-family:‘Playfair Display’,serif;font-size:1.2rem;color:#1c1a16;}
.mcls{background:none;border:none;font-size:1.3rem;cursor:pointer;color:#4a4540;padding:4px 8px;border-radius:6px;}
.mcls:hover{background:#f8f4ec;}
.atabs{display:flex;background:#f8f4ec;border-radius:10px;padding:3px;margin-bottom:1.4rem;}
.atab{flex:1;padding:8px;border:none;border-radius:8px;font-family:‘DM Sans’,sans-serif;font-size:.85rem;cursor:pointer;background:transparent;color:#4a4540;}
.atab.active{background:#fff;color:#1a472a;font-weight:600;box-shadow:0 1px 4px rgba(0,0,0,.1);}
.fg{margin-bottom:1rem;}
.fl{display:block;font-size:.76rem;font-weight:500;color:#4a4540;text-transform:uppercase;letter-spacing:.4px;margin-bottom:5px;}
.fi,.fsel{width:100%;padding:11px 14px;border:1.5px solid #e0d8c8;border-radius:10px;font-family:‘DM Sans’,sans-serif;font-size:.9rem;color:#1c1a16;outline:none;transition:all .15s;background:#fff;}
.fi:focus,.fsel:focus{border-color:#52b788;box-shadow:0 0 0 3px rgba(82,183,136,.12);}
.fi::placeholder{color:#bbb;}
.btn-p{width:100%;padding:12px;background:#1a472a;color:#fff;border:none;border-radius:10px;font-family:‘DM Sans’,sans-serif;font-size:.92rem;font-weight:600;cursor:pointer;display:flex;align-items:center;justify-content:center;gap:8px;transition:all .15s;}
.btn-p:hover{background:#2d6a4f;}.btn-p:disabled{opacity:.55;cursor:not-allowed;}
.err{background:#fce4ec;border:1px solid #ef9a9a;border-radius:8px;padding:10px 14px;font-size:.82rem;color:#c62828;margin-bottom:12px;display:none;}
.err.on{display:block;}
.ok{background:#e8f5e9;border:1px solid #a5d6a7;border-radius:8px;padding:10px 14px;font-size:.82rem;color:#2e7d32;margin-bottom:12px;display:none;}
.ok.on{display:block;}
.loading-spin{width:16px;height:16px;border:2px solid rgba(255,255,255,.4);border-top-color:#fff;border-radius:50%;animation:spin .7s linear infinite;display:none;}
.loading-spin.on{display:inline-block;}

/* WAVE */
.wave-card{background:linear-gradient(135deg,#0088cc,#005f99);border-radius:16px;padding:1.4rem;color:#fff;text-align:center;margin-bottom:1.2rem;}
.wave-amount{font-size:2.4rem;font-weight:700;font-family:‘Playfair Display’,serif;line-height:1;}
.wave-period{font-size:.82rem;opacity:.85;margin-top:5px;}
.wave-feats{background:#f8f4ec;border-radius:12px;padding:1rem 1.2rem;margin-bottom:1.2rem;}
.wave-feats li{display:flex;align-items:center;gap:8px;font-size:.84rem;padding:4px 0;color:#4a4540;list-style:none;}
.wave-feats li::before{content:‘✓’;color:#52b788;font-weight:700;flex-shrink:0;}
.btn-wave{width:100%;padding:14px;background:linear-gradient(135deg,#0088cc,#005f99);color:#fff;border:none;border-radius:12px;font-family:‘DM Sans’,sans-serif;font-size:.98rem;font-weight:700;cursor:pointer;display:flex;align-items:center;justify-content:center;gap:8px;transition:all .18s;text-decoration:none;}
.btn-wave:hover{opacity:.9;transform:translateY(-1px);box-shadow:0 6px 22px rgba(0,136,204,.4);}
.verify-box{background:#f8f9fa;border:1px solid #e0d8c8;border-radius:12px;padding:1rem;margin-top:1rem;}
.verify-lbl{font-size:.76rem;font-weight:600;color:#4a4540;text-transform:uppercase;letter-spacing:.4px;margin-bottom:8px;}
.verify-row{display:flex;gap:8px;}
.vi{flex:1;padding:9px 12px;border:1.5px solid #e0d8c8;border-radius:8px;font-family:‘DM Sans’,sans-serif;font-size:.84rem;outline:none;background:#fff;color:#1c1a16;}
.vi:focus{border-color:#0088cc;}
.btn-vfy{padding:9px 16px;background:#1a472a;color:#fff;border:none;border-radius:8px;font-family:‘DM Sans’,sans-serif;font-size:.82rem;font-weight:600;cursor:pointer;}
.chip{display:inline-flex;align-items:center;gap:5px;padding:4px 12px;border-radius:20px;font-size:.78rem;font-weight:500;}
.chip-active{background:#e8f5e9;color:#2e7d32;border:1px solid #a5d6a7;}
.chip-pending{background:#fff8e1;color:#856404;border:1px solid #d4a017;}
.chip-none{background:#f5f5f5;color:#666;border:1px solid #ddd;}

/* ACCOUNT */
.acct-row{display:flex;align-items:center;justify-content:space-between;padding:.9rem 0;border-bottom:1px solid #e0d8c8;}
.acct-row:last-child{border:none;padding-bottom:0;}
.acct-lbl{font-size:.76rem;color:#4a4540;text-transform:uppercase;letter-spacing:.4px;}
.acct-val{font-size:.9rem;font-weight:500;color:#1c1a16;}
.btn-danger{padding:9px 18px;background:#fce4ec;color:#c62828;border:1px solid #ef9a9a;border-radius:10px;font-family:‘DM Sans’,sans-serif;font-size:.84rem;cursor:pointer;}

/* SEARCH */
.juris-search-bar{background:#fefcf7;border-bottom:1px solid #e0d8c8;padding:1rem 2rem;display:flex;gap:8px;align-items:center;}
.search-input{flex:1;padding:11px 18px;border:1.5px solid #e0d8c8;border-radius:10px;font-size:.9rem;font-family:‘DM Sans’,sans-serif;background:#fff;color:#1c1a16;outline:none;transition:all .2s;}
.search-input:focus{border-color:#52b788;box-shadow:0 0 0 3px rgba(82,183,136,.12);}
.search-btn{padding:11px 20px;background:#d4a017;color:#1c1a16;border:none;border-radius:10px;font-family:‘DM Sans’,sans-serif;font-weight:500;font-size:.88rem;cursor:pointer;}
.filters-bar{background:#fefcf7;border-bottom:1px solid #e0d8c8;padding:.8rem 2rem;display:flex;align-items:center;gap:8px;flex-wrap:wrap;}
.filter-label{font-size:.75rem;font-weight:500;color:#4a4540;text-transform:uppercase;letter-spacing:.5px;}
.filter-chip{padding:5px 14px;border-radius:20px;border:1.5px solid #e0d8c8;background:#fff;font-family:‘DM Sans’,sans-serif;font-size:.8rem;cursor:pointer;transition:all .15s;color:#4a4540;}
.filter-chip:hover{border-color:#52b788;color:#1a472a;}
.filter-chip.active{background:#1a472a;border-color:#1a472a;color:#fff;}

/* CARDS */
.list-view{max-width:960px;margin:0 auto;padding:1.5rem 2rem 2rem;background:#f8f4ec;}
.results-header{display:flex;align-items:center;justify-content:space-between;margin-bottom:1.2rem;gap:10px;flex-wrap:wrap;}
.results-count{font-size:.8rem;color:#4a4540;}
.results-count strong{color:#1a472a;}
.header-actions{display:flex;gap:8px;}
.load-btn{display:flex;align-items:center;gap:6px;padding:8px 14px;background:#1a472a;color:#fff;border:none;border-radius:8px;font-family:‘DM Sans’,sans-serif;font-size:.82rem;cursor:pointer;}
.import-btn{display:flex;align-items:center;gap:6px;padding:8px 14px;background:#d4a017;color:#1c1a16;border:none;border-radius:8px;font-family:‘DM Sans’,sans-serif;font-size:.82rem;font-weight:500;cursor:pointer;}
.case-card{background:#fff;border:1.5px solid #e0d8c8;border-radius:14px;padding:1.1rem 1.3rem;margin-bottom:10px;cursor:pointer;transition:all .18s;position:relative;overflow:hidden;}
.case-card::before{content:’’;position:absolute;left:0;top:0;bottom:0;width:3px;background:#e0d8c8;transition:background .18s;}
.case-card:hover{border-color:#52b788;box-shadow:0 4px 16px rgba(26,71,42,.12);}
.case-card:hover::before{background:#52b788;}
.case-card.imported-card::before{background:#d4a017;}
.card-top{display:flex;align-items:flex-start;justify-content:space-between;gap:8px;margin-bottom:6px;}
.card-title{font-family:‘Playfair Display’,serif;font-size:.95rem;line-height:1.35;color:#1c1a16;flex:1;}
.card-badge{padding:3px 9px;border-radius:20px;font-size:.68rem;font-weight:500;white-space:nowrap;flex-shrink:0;}
.badge-civile{background:#e8f4fd;color:#1565c0}.badge-sociale{background:#fef3cd;color:#856404}
.badge-administrative{background:#e8f5e9;color:#2e7d32}.badge-penale{background:#fce4ec;color:#c62828}
.badge-constitutionnelle{background:#f3e5f5;color:#6a1b9a}.badge-commerciale{background:#e3f2fd;color:#1976d2}
.badge-fonciere{background:#fff8e1;color:#f57f17}.badge-default{background:#f5f5f5;color:#555}
.card-meta{display:flex;gap:10px;font-size:.75rem;color:#4a4540;flex-wrap:wrap;margin-bottom:4px;}
.card-excerpt{font-size:.8rem;color:#4a4540;line-height:1.5;margin-top:5px;display:-webkit-box;-webkit-line-clamp:2;-webkit-box-orient:vertical;overflow:hidden;}
.src-tag{display:inline-flex;align-items:center;gap:2px;padding:2px 7px;border-radius:8px;font-size:.62rem;font-weight:600;margin-left:5px;vertical-align:middle;background:#fff3cd;color:#856404;}
.state-box{text-align:center;padding:3rem 1rem;color:#4a4540;}
.state-icon{font-size:2.5rem;margin-bottom:.8rem;}
.state-box h3{font-size:1rem;margin-bottom:.4rem;color:#1c1a16;}

/* DETAIL */
.detail-view{display:none;max-width:960px;margin:0 auto;padding:1.5rem 2rem 3rem;flex-direction:column;gap:1.2rem;background:#f8f4ec;}
.detail-view.visible{display:flex;animation:fadeUp .25s ease;}
.back-btn{display:inline-flex;align-items:center;gap:7px;padding:8px 16px;background:#fff;color:#1a472a;border:1.5px solid #1a472a;border-radius:10px;font-family:‘DM Sans’,sans-serif;font-size:.82rem;font-weight:500;cursor:pointer;align-self:flex-start;}
.back-btn:hover{background:#f0faf4;}
.detail-card,.fulltext-box{background:#fff;border:1.5px solid #e0d8c8;border-radius:14px;padding:1.3rem;}
.detail-jurisdiction{font-size:.72rem;text-transform:uppercase;letter-spacing:1px;color:#1a472a;font-weight:500;margin-bottom:6px;}
.detail-title{font-family:‘Playfair Display’,serif;font-size:1.15rem;line-height:1.4;margin-bottom:10px;color:#1c1a16;}
.detail-tags{display:flex;flex-wrap:wrap;gap:6px;}
.detail-tag{padding:3px 10px;border-radius:20px;font-size:.72rem;border:1px solid #e0d8c8;color:#4a4540;}
.box-title{font-size:.75rem;text-transform:uppercase;letter-spacing:.8px;color:#4a4540;margin-bottom:10px;}
.fulltext-content{font-size:.84rem;line-height:1.82;color:#4a4540;max-height:400px;overflow-y:auto;padding-right:6px;white-space:pre-wrap;}
.fulltext-content::-webkit-scrollbar{width:4px}.fulltext-content::-webkit-scrollbar-thumb{background:#52b788;border-radius:4px;}
.ai-actions-row{display:flex;justify-content:flex-end;gap:8px;padding-top:12px;border-top:1px solid #e0d8c8;flex-wrap:wrap;margin-top:12px;}
.ai-action-btn{display:inline-flex;align-items:center;gap:6px;padding:9px 18px;border:none;border-radius:10px;font-family:‘DM Sans’,sans-serif;font-size:.82rem;font-weight:500;cursor:pointer;transition:all .18s;}
.btn-explain{background:#fff;color:#1a472a;border:1.5px solid #1a472a;}.btn-explain:hover{background:#f0faf4;}
.btn-summarize{background:#1a472a;color:#fff;}.btn-summarize:hover{background:#2d6a4f;}
.btn-favorite{background:#fff;color:#d4a017;border:1.5px solid #d4a017;}.btn-favorite.is-fav{background:#d4a017;color:#1c1a16;}
.ai-action-btn:disabled{opacity:.55;cursor:not-allowed;}
.ai-action-btn .bsp{width:13px;height:13px;border:2px solid rgba(255,255,255,.4);border-top-color:#fff;border-radius:50%;animation:spin .7s linear infinite;display:none;}
.btn-explain .bsp{border-color:rgba(26,71,42,.25);border-top-color:#1a472a;}
.ai-action-btn.loading .bsp{display:inline-block;}.ai-action-btn.loading .blbl{display:none;}
.ai-box{background:linear-gradient(135deg,#f0faf4,#e8f5ef);border:1.5px solid #52b788;border-radius:14px;padding:1.2rem;}
.ai-box-header{display:flex;align-items:center;gap:8px;margin-bottom:10px;}
.ai-dot{width:8px;height:8px;background:#52b788;border-radius:50%;animation:pulse 2s ease-in-out infinite;}
.ai-label{font-size:.7rem;font-weight:500;text-transform:uppercase;letter-spacing:.8px;color:#1a472a;}
.ai-content{font-size:.85rem;line-height:1.7;color:#1c1a16;}
.ai-content strong{color:#1a472a;font-weight:600;}
.disclaimer-box{display:flex;align-items:flex-start;gap:10px;background:#fffbec;border:1.5px solid #d4a017;border-radius:12px;padding:.9rem 1.1rem;font-size:.78rem;line-height:1.55;color:#4a4540;}
.dsc-icon{font-size:1.1rem;flex-shrink:0;}
.disclaimer-box strong{display:block;color:#856404;margin-bottom:3px;font-size:.75rem;text-transform:uppercase;letter-spacing:.4px;}

/* ASSISTANT TAB */
.ai-chat-tab{max-width:760px;margin:0 auto;padding:2rem 2rem 3rem;display:flex;flex-direction:column;gap:1.2rem;background:#f8f4ec;}
.ai-chat-header{background:linear-gradient(135deg,#f0faf4,#e8f5ef);border:1.5px solid #52b788;border-radius:14px;padding:1.2rem 1.4rem;display:flex;align-items:center;gap:12px;}
.ai-chat-icon{width:44px;height:44px;background:#1a472a;border-radius:12px;display:flex;align-items:center;justify-content:center;font-size:1.3rem;flex-shrink:0;}
.ai-chat-header h3{font-family:‘Playfair Display’,serif;font-size:1rem;margin-bottom:3px;color:#1c1a16;}
.ai-chat-header p{font-size:.78rem;color:#4a4540;}
.free-chat-window{background:#fff;border:1.5px solid #e0d8c8;border-radius:14px;padding:1.2rem;display:flex;flex-direction:column;gap:12px;min-height:420px;}
.free-msgs{flex:1;min-height:300px;max-height:55vh;overflow-y:auto;display:flex;flex-direction:column;gap:10px;padding-right:4px;}
.free-msgs::-webkit-scrollbar{width:4px}.free-msgs::-webkit-scrollbar-thumb{background:#52b788;border-radius:4px;}
.chat-welcome{text-align:center;padding:2rem 1rem;color:#4a4540;}
.chat-welcome .wico{font-size:2.2rem;margin-bottom:.7rem;}
.chat-welcome h3{font-size:.95rem;margin-bottom:.4rem;color:#1c1a16;}
.chat-welcome p{font-size:.82rem;}
.suggestions{display:flex;flex-wrap:wrap;gap:6px;margin-top:1rem;justify-content:center;}
.sug-chip{padding:6px 14px;background:#f0faf4;border:1px solid #52b788;border-radius:20px;font-size:.76rem;color:#1a472a;cursor:pointer;transition:all .14s;}
.sug-chip:hover{background:#1a472a;color:#fff;}
.chat-msg{padding:9px 13px;border-radius:12px;line-height:1.55;max-width:88%;word-break:break-word;font-size:.83rem;}
.chat-msg.user{background:#1a472a;color:#fff;align-self:flex-end;border-bottom-right-radius:3px;}
.chat-msg.assistant{background:#f4f1ea;color:#1c1a16;align-self:flex-start;border-bottom-left-radius:3px;}
.free-input-row{display:flex;gap:8px;padding-top:8px;border-top:1px solid #e0d8c8;}
.free-chat-input{flex:1;padding:11px 16px;border:1.5px solid #e0d8c8;border-radius:12px;font-family:‘DM Sans’,sans-serif;font-size:.88rem;outline:none;background:#fff;color:#1c1a16;}
.free-chat-input:focus{border-color:#52b788;}
.free-send-btn{padding:11px 18px;background:#1a472a;color:#fff;border:none;border-radius:12px;cursor:pointer;display:flex;align-items:center;gap:6px;font-family:‘DM Sans’,sans-serif;font-size:.85rem;font-weight:500;}
.free-send-btn:disabled{opacity:.5;cursor:not-allowed;}
.free-send-btn .fbsp{width:14px;height:14px;border:2px solid rgba(255,255,255,.35);border-top-color:#fff;border-radius:50%;animation:spin .7s linear infinite;display:none;}
.free-send-btn.loading .fbsp{display:inline-block;}.free-send-btn.loading .fblbl{display:none;}

/* FAVORIS */
.favorites-tab{max-width:960px;margin:0 auto;padding:2rem 2rem 3rem;background:#f8f4ec;min-height:100vh;}
.favorites-tab h2{font-family:‘Playfair Display’,serif;font-size:1.3rem;margin-bottom:1.4rem;display:flex;align-items:center;gap:10px;color:#1c1a16;}
.fav-count{background:#d4a017;color:#1c1a16;font-size:.72rem;font-weight:700;padding:2px 9px;border-radius:20px;}
.fav-card{background:#fff;border:1.5px solid #e0d8c8;border-radius:14px;padding:1.1rem 1.3rem;margin-bottom:10px;display:flex;align-items:flex-start;gap:12px;transition:all .15s;position:relative;overflow:hidden;cursor:pointer;}
.fav-card::before{content:’’;position:absolute;left:0;top:0;bottom:0;width:3px;background:#d4a017;}
.fav-card:hover{border-color:#d4a017;box-shadow:0 4px 16px rgba(212,160,23,.15);}
.fav-body{flex:1;}
.fav-rm{background:none;border:none;cursor:pointer;color:#ccc;font-size:1rem;padding:4px;flex-shrink:0;}
.fav-rm:hover{color:#c62828;}
.fav-empty{text-align:center;padding:4rem 1rem;color:#4a4540;}
.fav-empty .fav-icon{font-size:3rem;margin-bottom:1rem;}
.fav-empty h3{font-size:1.05rem;margin-bottom:.4rem;color:#1c1a16;}

/* IMPORT MODAL */
.hint-box{background:linear-gradient(135deg,#f0faf4,#e8f5ef);border:1px solid #52b788;border-radius:10px;padding:.9rem 1.1rem;font-size:.78rem;color:#4a4540;margin-bottom:1.2rem;line-height:1.6;}
.hint-box strong{color:#1a472a;}
.form-row{display:grid;grid-template-columns:1fr 1fr;gap:12px;}
.form-textarea{width:100%;padding:10px 14px;border:1.5px solid #e0d8c8;border-radius:10px;font-family:‘DM Sans’,sans-serif;font-size:.88rem;color:#1c1a16;outline:none;min-height:180px;resize:vertical;line-height:1.65;background:#fff;}
.form-textarea:focus{border-color:#52b788;}
.modal-actions{display:flex;justify-content:flex-end;gap:10px;margin-top:1.5rem;padding-top:1.2rem;border-top:1px solid #e0d8c8;}
.btn-cancel{padding:10px 20px;background:#fff;color:#4a4540;border:1.5px solid #e0d8c8;border-radius:10px;font-family:‘DM Sans’,sans-serif;font-size:.85rem;cursor:pointer;}
.btn-save{padding:10px 24px;background:#1a472a;color:#fff;border:none;border-radius:10px;font-family:‘DM Sans’,sans-serif;font-size:.85rem;font-weight:500;cursor:pointer;}

/* UTILS */
.highlight{background:#fff3cd;border-radius:2px;padding:0 1px;font-weight:600;}
.tdots{display:flex;gap:4px;align-items:center;padding:4px 0;}
.tdots span{display:inline-block;width:6px;height:6px;background:#52b788;border-radius:50%;animation:bounce 1.2s ease-in-out infinite;}
.tdots span:nth-child(2){animation-delay:.2s}.tdots span:nth-child(3){animation-delay:.4s}

@media(max-width:700px){
.list-view,.detail-view,.ai-chat-tab,.favorites-tab{padding:1rem;}
.juris-search-bar{padding:.8rem 1rem;}.filters-bar{padding:.6rem 1rem;}
header{padding:0 1rem;}.hero{padding:1.6rem 1rem 0;}
.tabs-nav{padding:0 .3rem;gap:2px;}.tab-btn{padding:8px 10px;font-size:.76rem;}
.hdr-badge{display:none;}.form-row{grid-template-columns:1fr;}
.ai-action-btn{flex:1;justify-content:center;}
.paywall-banner{flex-direction:column;text-align:center;}
}
</style>

</head>
<body>

<header>
  <div class="logo" onclick="goHome()">
    <div class="logo-icon">⚖️</div>
    <div class="logo-text">Juri<span>Sen</span></div>
  </div>
  <div class="hdr-right">
    <div class="hdr-badge">🇸🇳 Jurisprudence · IA</div>
    <button class="user-btn" id="userBtn" onclick="onUserBtnClick()">
      <span class="sub-dot dot-none" id="subDot"></span>
      <span id="userBtnLabel">Connexion</span>
    </button>
  </div>
</header>

<div id="installBanner">
  <div class="inst-text"><strong>📲 Installer JuriSen</strong>Accès hors-ligne · Icône sur l'écran d'accueil</div>
  <button class="btn-inst" onclick="doInstall()">Installer</button>
  <button class="btn-inst-x" onclick="dismissInstall()">✕</button>
</div>

<div class="hero">
  <h1>La jurisprudence sénégalaise,<br/><em>enfin accessible à tous</em></h1>
  <p>Cour Suprême · Conseil Constitutionnel · 31 décisions · Analyse par IA</p>
  <nav class="tabs-nav">
    <button class="tab-btn active" data-tab="juris" onclick="switchTab('juris',this)">📚 Jurisprudence</button>
    <button class="tab-btn" data-tab="assistant" onclick="switchTab('assistant',this)">🤖 Assistant IA</button>
    <button class="tab-btn" data-tab="favoris" onclick="switchTab('favoris',this)">⭐ Favoris <span class="tab-badge" id="favBadge">0</span></button>
  </nav>
</div>

<!-- TAB 1 -->

<div class="tab-panel active" id="tab-juris">
  <div id="paywallWrap"></div>
  <div class="juris-search-bar" id="searchBarEl">
    <input class="search-input" id="searchInput" type="text" placeholder="Rechercher : licenciement, cassation, divorce, OHADA…" oninput="applyFilters()"/>
    <button class="search-btn" onclick="applyFilters()">🔍</button>
  </div>
  <div class="filters-bar" id="filtersBarEl">
    <span class="filter-label">Matière</span>
    <button class="filter-chip active" onclick="filterChip(this,'')">Toutes</button>
    <button class="filter-chip" onclick="filterChip(this,'Sociale')">Sociale</button>
    <button class="filter-chip" onclick="filterChip(this,'Civile')">Civile</button>
    <button class="filter-chip" onclick="filterChip(this,'Administrative')">Administrative</button>
    <button class="filter-chip" onclick="filterChip(this,'Pénale')">Pénale</button>
    <button class="filter-chip" onclick="filterChip(this,'Commerciale')">Commerciale</button>
    <button class="filter-chip" onclick="filterChip(this,'Constitutionnelle')">Constitutionnelle</button>
    <button class="filter-chip" onclick="filterChip(this,'Foncière')">Foncière</button>
  </div>
  <div class="list-view" id="listView">
    <div class="results-header">
      <div class="results-count" id="resultsCount">Chargement…</div>
      <div class="header-actions">
        <button class="import-btn" onclick="openImportModal()">➕ Ajouter</button>
        <button class="load-btn" id="loadMoreBtn" onclick="loadMore()" style="display:none">⬇️ Plus</button>
      </div>
    </div>
    <div id="cardList"></div>
    <div id="listState" class="state-box" style="display:none">
      <div class="state-icon">🔍</div><h3>Aucun résultat</h3><p>Essayez un autre terme</p>
    </div>
  </div>
  <div class="detail-view" id="detailView"></div>
</div>

<!-- TAB 2 -->

<div class="tab-panel" id="tab-assistant">
  <div class="ai-chat-tab">
    <div class="ai-chat-header">
      <div class="ai-chat-icon">🤖</div>
      <div>
        <h3>Assistant Juridique Sénégalais</h3>
        <p>Questions sur le droit sénégalais : procédures, textes, jurisprudence…</p>
      </div>
    </div>
    <div id="assistantPaywall"></div>
    <div class="disclaimer-box">
      <span class="dsc-icon">⚠️</span>
      <div><strong>Avertissement</strong>Outil d'assistance basé sur l'IA. Ne remplace pas un avocat.</div>
    </div>
    <div class="free-chat-window">
      <div class="free-msgs" id="freeChatMessages">
        <div class="chat-welcome" id="chatWelcome">
          <div class="wico">⚖️</div>
          <h3>Bienvenue sur l'Assistant Juridique JuriSen</h3>
          <p>Je suis spécialisé en droit sénégalais. Connectez-vous et abonnez-vous pour débloquer l'accès.</p>
          <div class="suggestions">
            <span class="sug-chip" onclick="useSug(this)">Procédures de divorce au Sénégal</span>
            <span class="sug-chip" onclick="useSug(this)">Droits en cas de licenciement abusif</span>
            <span class="sug-chip" onclick="useSug(this)">Comment saisir la Cour Suprême ?</span>
            <span class="sug-chip" onclick="useSug(this)">Droit OHADA au Sénégal</span>
            <span class="sug-chip" onclick="useSug(this)">Délais de prescription civile</span>
          </div>
        </div>
      </div>
      <div class="free-input-row">
        <input class="free-chat-input" id="freeChatInput" type="text" placeholder="Posez votre question juridique…" onkeydown="if(event.key==='Enter'){event.preventDefault();sendFreeChat()}"/>
        <button class="free-send-btn" id="freeSendBtn" onclick="sendFreeChat()">
          <span class="fbsp"></span><span class="fblbl">Envoyer ➤</span>
        </button>
      </div>
    </div>
  </div>
</div>

<!-- TAB 3 -->

<div class="tab-panel" id="tab-favoris">
  <div class="favorites-tab">
    <h2>⭐ Mes Favoris <span class="fav-count" id="favCountLabel">0</span></h2>
    <div id="favList">
      <div class="fav-empty"><div class="fav-icon">📌</div><h3>Aucun favori</h3><p>Consultez un arrêt et cliquez sur ☆ Ajouter aux favoris</p></div>
    </div>
  </div>
</div>

<!-- MODAL AUTH -->

<div class="ov" id="authModal">
  <div class="mbox">
    <div class="mhead"><h2>🔐 Accès JuriSen</h2><button class="mcls" onclick="closeModal('authModal')">✕</button></div>
    <div class="atabs">
      <button class="atab active" onclick="switchAuth('login',this)">Se connecter</button>
      <button class="atab" onclick="switchAuth('register',this)">S'inscrire</button>
    </div>
    <div id="authErr" class="err"></div>
    <div id="authOk" class="ok"></div>
    <div id="loginForm">
      <div class="fg"><label class="fl">Email</label><input class="fi" id="loginEmail" type="email" placeholder="votre@email.com"/></div>
      <div class="fg"><label class="fl">Mot de passe</label><input class="fi" id="loginPwd" type="password" placeholder="••••••••" onkeydown="if(event.key==='Enter')doLogin()"/></div>
      <button class="btn-p" onclick="doLogin()" id="loginBtn"><span class="loading-spin" id="loginSpin"></span>Se connecter</button>
    </div>
    <div id="registerForm" style="display:none">
      <div class="fg"><label class="fl">Nom complet</label><input class="fi" id="regName" type="text" placeholder="Prénom Nom"/></div>
      <div class="fg"><label class="fl">Email</label><input class="fi" id="regEmail" type="email" placeholder="votre@email.com"/></div>
      <div class="fg"><label class="fl">Mot de passe</label><input class="fi" id="regPwd" type="password" placeholder="8 caractères minimum" onkeydown="if(event.key==='Enter')doRegister()"/></div>
      <button class="btn-p" onclick="doRegister()" id="regBtn"><span class="loading-spin" id="regSpin"></span>Créer mon compte</button>
    </div>
  </div>
</div>

<!-- MODAL WAVE -->

<div class="ov" id="waveModal">
  <div class="mbox wide">
    <div class="mhead"><h2>💳 Abonnement mensuel</h2><button class="mcls" onclick="closeModal('waveModal')">✕</button></div>
    <div class="wave-card">
      <div style="font-size:1.5rem;margin-bottom:6px;">🌊 Wave Mobile Money</div>
      <div class="wave-amount">1 000 <small style="font-size:1rem">FCFA</small></div>
      <div class="wave-period">par mois · Renouvellement mensuel</div>
    </div>
    <ul class="wave-feats">
      <li>Analyses IA illimitées (résumé + analyse approfondie)</li>
      <li>Assistant juridique IA en conversation libre</li>
      <li>Import et stockage d'arrêts personnalisés</li>
      <li>Accès à toute la base de jurisprudence</li>
    </ul>
    <a class="btn-wave" href="https://pay.wave.com/m/M_sn_BVRx0-J7owDv/c/sn/?amount=1000" target="_blank" rel="noopener" onclick="onWaveClick()">🌊 Payer avec Wave</a>
    <div class="verify-box">
      <div class="verify-lbl">📋 Confirmer votre paiement</div>
      <p style="font-size:.78rem;color:#4a4540;margin-bottom:8px;">Après paiement, saisissez votre numéro Wave :</p>
      <div id="waveErr" class="err" style="margin-bottom:8px;"></div>
      <div id="waveOk" class="ok" style="margin-bottom:8px;"></div>
      <div class="verify-row">
        <input class="vi" id="wavePhone" type="tel" placeholder="77X XXX XX XX" maxlength="10"/>
        <button class="btn-vfy" onclick="submitWavePayment()">Confirmer</button>
      </div>
      <p style="font-size:.74rem;color:#4a4540;margin-top:8px;">✅ Activation sous 24h · Support : support@jurisen.sn</p>
    </div>
  </div>
</div>

<!-- MODAL COMPTE -->

<div class="ov" id="accountModal">
  <div class="mbox">
    <div class="mhead"><h2>👤 Mon Compte</h2><button class="mcls" onclick="closeModal('accountModal')">✕</button></div>
    <div id="acctContent"></div>
  </div>
</div>

<!-- MODAL IMPORT -->

<div class="ov" id="importModal" onclick="if(event.target===this)closeModal('importModal')">
  <div class="mbox" style="max-width:640px;">
    <div class="mhead"><h2>➕ Ajouter un arrêt</h2><button class="mcls" onclick="closeModal('importModal')">✕</button></div>
    <div class="hint-box"><strong>Comment utiliser :</strong> Copiez le texte d'un arrêt depuis Juricaf et collez-le dans « Texte intégral ». Sauvegardé localement dans votre navigateur.</div>
    <div class="fg"><label class="fl">Titre *</label><input class="fi" id="imp-title" type="text" placeholder="Ex: Arrêt N°12 — Cour Suprême — X c/ État du Sénégal"/></div>
    <div class="form-row">
      <div class="fg"><label class="fl">Date *</label><input class="fi" id="imp-date" type="date"/></div>
      <div class="fg"><label class="fl">Matière *</label>
        <select class="fsel" id="imp-matiere"><option value="">Choisir…</option><option>Sociale</option><option>Civile</option><option>Administrative</option><option>Pénale</option><option>Commerciale</option><option>Constitutionnelle</option><option>Foncière</option><option>Autre</option></select>
      </div>
    </div>
    <div class="form-row">
      <div class="fg"><label class="fl">Juridiction</label><input class="fi" id="imp-jurisdiction" type="text" value="Cour Suprême"/></div>
      <div class="fg"><label class="fl">N° affaire</label><input class="fi" id="imp-numero" type="text" placeholder="J/123/RG/24"/></div>
    </div>
    <div class="fg"><label class="fl">Résumé court</label><input class="fi" id="imp-excerpt" type="text" placeholder="En 1-2 phrases…"/></div>
    <div class="fg"><label class="fl">Texte intégral *</label><textarea class="form-textarea" id="imp-texte" placeholder="Collez ici le texte complet de l'arrêt…"></textarea></div>
    <div class="fg"><label class="fl">Lien source (optionnel)</label><input class="fi" id="imp-url" type="url" placeholder="https://juricaf.org/arret/…"/></div>
    <div class="modal-actions">
      <button class="btn-cancel" onclick="closeModal('importModal')">Annuler</button>
      <button class="btn-save" onclick="saveImport()">💾 Enregistrer</button>
    </div>
  </div>
</div>

<script>
/* CONFIG */
const SUPABASE_URL  = 'https://csgpjrjfpbjumwqmnxyw.supabase.co';
const SUPABASE_ANON = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImNzZ3BqcmpmcGJqdW13cW1ueHl3Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzE5NjM2ODYsImV4cCI6MjA4NzUzOTY4Nn0.cta4zoicDYr0-nxHtDmVWm6NR7b0ApkI3jBh2XdS_jI';
const OPENAI_KEY    = 'REMPLACEZ_PAR_VOTRE_CLE_OPENAI';
const PAGE_SIZE     = 25;

/* SUPABASE — init défensif (fonctionne hors-ligne) */
let sb = null;
let supabaseAvailable = false;
try {
  if (typeof supabase !== 'undefined' && supabase.createClient) {
    sb = supabase.createClient(SUPABASE_URL, SUPABASE_ANON);
    supabaseAvailable = true;
  }
} catch(e) { console.warn('Supabase indisponible', e); }

if (!sb) {
  const noNet = async () => ({ data: null, error: { message: 'Connexion réseau indisponible.' } });
  sb = {
    auth: {
      onAuthStateChange: (cb) => { cb('SIGNED_OUT', null); return { data:{ subscription:{ unsubscribe:()=>{} } } }; },
      signInWithPassword: async () => ({ error:{ message:'Connexion réseau indisponible.' } }),
      signUp: async () => ({ error:{ message:'Connexion réseau indisponible.' } }),
      signOut: async () => {}
    },
    from: () => {
      const chain = { select:()=>chain, eq:()=>chain, gt:()=>chain, order:()=>chain, limit: async ()=>({ data:[], error:null }), insert: async ()=>({ error:{ message:'Connexion réseau indisponible.' } }) };
      return chain;
    }
  };
}

/* STATE */
let currentUser = null, currentSubscription = null;
let allCases = [], filteredCases = [], displayedCount = 0;
let activeFilter = '', currentCase = null, freeHistory = [];
let favorites = [], importedCases = [], deferredInstall = null;

try { favorites     = JSON.parse(localStorage.getItem('jurisen_favorites') || '[]'); } catch(e){ favorites=[]; }
try { importedCases = JSON.parse(localStorage.getItem('jurisen_imports')   || '[]'); } catch(e){ importedCases=[]; }

/* BASE DE DONNÉES */
const BASE_CASES=[{id:"cs-soc-2024-40",title:"Arrêt N°40/2024 — Droits collectifs des travailleurs",date:"2024-11-27",jurisdiction:"Cour Suprême — Ch. Sociale",matiere:"Sociale",numero:"J/401/RG/23",excerpt:"Violation du Code du Travail relatif aux droits collectifs des travailleurs. Cassation et renvoi devant la Cour d'Appel de Dakar.",texte:`ARRÊT N°40 — 27 novembre 2024 — CHAMBRE SOCIALE\nAttendu que les demandeurs font grief à l'arrêt attaqué d'avoir violé les dispositions du Code du Travail relatives aux droits collectifs des travailleurs, notamment les articles 269 et suivants sur les délégués du personnel ;\nAttendu que la Cour d'Appel, en statuant sur la validité d'un accord collectif sans respecter les formes légales de consultation, a méconnu le principe de la représentation collective ;\nPAR CES MOTIFS : CASSE ET ANNULE l'arrêt attaqué. Renvoie devant la Cour d'Appel de Dakar, autrement composée.`},{id:"cs-soc-2023-47",title:"Arrêt N°47/2023 — Rupture abusive CDI — Art. L.53 Code du Travail",date:"2023-12-27",jurisdiction:"Cour Suprême — Ch. Sociale",matiere:"Sociale",numero:"J/149/RG/23",excerpt:"Rupture abusive CDI. Non-respect de la procédure légale de licenciement. Cassation.",texte:`ARRÊT N°47 — 27 décembre 2023 — CHAMBRE SOCIALE\nAttendu que l'employeur n'a pas notifié les motifs du licenciement par lettre recommandée avec accusé de réception, comme l'exige l'article L.53 du Code du Travail ;\nPAR CES MOTIFS : CASSE ET ANNULE l'arrêt attaqué. Renvoie devant la Cour d'Appel de Thiès.`},{id:"cs-soc-2023-44",title:"Arrêt N°44/2023 — DP World Dakar — Proportionnalité sanction disciplinaire",date:"2023-11-08",jurisdiction:"Cour Suprême — Ch. Sociale",matiere:"Sociale",numero:"J/038/RG/23",excerpt:"DP World Dakar. Licenciement disciplinaire disproportionné. Principe de proportionnalité. Rejet du pourvoi.",texte:`ARRÊT N°44 — 8 novembre 2023 — CHAMBRE SOCIALE\nAttendu que le principe de proportionnalité impose à l'employeur de graduer les sanctions avant de prononcer le licenciement ;\nPAR CES MOTIFS : REJETTE le pourvoi. Condamne DP World Dakar aux entiers dépens.`},{id:"cs-soc-2022-greve",title:"Arrêt 2022 — Droit de grève — Retenue sur salaire — Art. L.138",date:"2022-06-15",jurisdiction:"Cour Suprême — Ch. Sociale",matiere:"Sociale",numero:"J/187/RG/21",excerpt:"Retenue sur salaire excédant la durée réelle de la grève. Sanction pécuniaire prohibée. Cassation.",texte:`ARRÊT — 15 juin 2022 — CHAMBRE SOCIALE\nAttendu que toute retenue excédant la proportionnalité au temps d'arrêt effectif constitue une sanction pécuniaire prohibée par l'article L.138 du Code du Travail ;\nPAR CES MOTIFS : CASSE l'arrêt. Condamne la société à rembourser le trop-retenu avec intérêts légaux.`},{id:"cs-soc-2021-transport",title:"Arrêt 2021 — Faute grave non prouvée — Charge de la preuve employeur",date:"2021-05-12",jurisdiction:"Cour Suprême — Ch. Sociale",matiere:"Sociale",numero:"J/203/RG/20",excerpt:"Licenciement pour faute grave non prouvé. La preuve incombe à l'employeur. Cassation.",texte:`ARRÊT — 12 mai 2021 — CHAMBRE SOCIALE\nAttendu que la faute grave doit être prouvée par l'employeur avec certitude ; que le doute profite au salarié ;\nPAR CES MOTIFS : CASSE l'arrêt attaqué. Dit le licenciement abusif.`},{id:"cs-soc-2020-harcelement",title:"Arrêt 2020 — Harcèlement moral — Résiliation judiciaire torts employeur",date:"2020-09-10",jurisdiction:"Cour Suprême — Ch. Sociale",matiere:"Sociale",numero:"J/055/RG/19",excerpt:"Harcèlement moral prouvé. Résiliation judiciaire aux torts de l'employeur. Dommages-intérêts majorés.",texte:`ARRÊT — 10 septembre 2020 — CHAMBRE SOCIALE\nAttendu que les attestations médicales, témoignages et correspondances établissent une pression psychologique systématique ;\nPAR CES MOTIFS : CONFIRME la résiliation judiciaire aux torts de l'employeur.`},{id:"cs-civ-2024-56",title:"Arrêt N°56/2024 — Obligations contractuelles — Inexécution fautive COCC",date:"2024-06-05",jurisdiction:"Cour Suprême — Ch. Civile",matiere:"Civile",numero:"Arrêt n°56",excerpt:"Inexécution fautive d'obligations contractuelles. Art. 103 COCC. Rejet du pourvoi.",texte:`ARRÊT N°56 — 5 juin 2024 — CHAMBRE CIVILE\nAttendu que la Cour d'Appel a souverainement constaté l'inexécution fautive des obligations contractuelles ;\nPAR CES MOTIFS : REJETTE le pourvoi. Condamne le demandeur aux dépens.`},{id:"cs-civ-2023-divorce",title:"Arrêt 2023 — Divorce pour faute — Violences conjugales — Code Famille art.163",date:"2023-08-22",jurisdiction:"Cour Suprême — Ch. Civile",matiere:"Civile",numero:"J/345/RG/22",excerpt:"Divorce pour faute aux torts exclusifs du mari. Violences conjugales et abandon. Partage des acquêts par moitié.",texte:`ARRÊT — 22 août 2023 — CHAMBRE CIVILE\nAttendu que les violations de l'obligation de cohabitation constituent des fautes causant des torts exclusifs au mari ;\nPAR CES MOTIFS : PRONONCE le divorce aux torts exclusifs du mari. Condamne au paiement de 120 000 FCFA/mois.`},{id:"cs-civ-2022-succession",title:"Arrêt 2022 — Succession — Droits de la veuve — Code Famille art.401",date:"2022-03-17",jurisdiction:"Cour Suprême — Ch. Civile",matiere:"Civile",numero:"J/312/RG/21",excerpt:"Partage successoral excluant la veuve. Droits successoraux du conjoint survivant. Code de la Famille art.401.",texte:`ARRÊT — 17 mars 2022 — CHAMBRE CIVILE\nAttendu que le Code de la Famille (art.401) prévoit que le conjoint survivant a vocation héréditaire ;\nPAR CES MOTIFS : INFIRME le partage. Ordonne la reconstitution de la masse successorale.`},{id:"cs-civ-2022-filiation",title:"Arrêt 2022 — Filiation naturelle — Expertise ADN — Code Famille art.198",date:"2022-04-05",jurisdiction:"Cour Suprême — Ch. Civile",matiere:"Civile",numero:"J/189/RG/21",excerpt:"Recherche de paternité. Admissibilité du test ADN. Présomption légale en cas de refus.",texte:`ARRÊT — 5 avril 2022 — CHAMBRE CIVILE\nAttendu que l'expertise génétique (ADN) est admissible et probante en droit sénégalais ;\nAttendu que le refus de s'y soumettre crée une présomption légale de paternité ;\nPAR CES MOTIFS : CONFIRME l'ordonnance d'expertise génétique.`},{id:"cs-civ-2021-bail",title:"Arrêt 2021 — Bail commercial 12 ans — Indemnité d'éviction OHADA",date:"2021-11-23",jurisdiction:"Cour Suprême — Ch. Civile",matiere:"Civile",numero:"J/098/RG/20",excerpt:"Résiliation bail commercial pour travaux. Droit au renouvellement ou indemnité d'éviction.",texte:`ARRÊT — 23 novembre 2021 — CHAMBRE CIVILE\nAttendu que l'Acte Uniforme OHADA confère au locataire un droit au renouvellement ou une indemnité d'éviction ;\nPAR CES MOTIFS : CONFIRME la condamnation du bailleur.`},{id:"cs-civ-2023-responsabilite",title:"Arrêt 2023 — Responsabilité civile — Accident de route — Art.1384 COCC",date:"2023-04-20",jurisdiction:"Cour Suprême — Ch. Civile",matiere:"Civile",numero:"J/089/RG/22",excerpt:"Accident de la route. Responsabilité du fait des choses. Faute de la victime exonératoire à 1/3.",texte:`ARRÊT — 20 avril 2023 — CHAMBRE CIVILE\nAttendu que le gardien est présumé responsable du dommage causé par la chose, conformément à l'article 1384 du COCC ;\nPAR CES MOTIFS : CONFIRME le partage 2/3 conducteur, 1/3 victime.`},{id:"cs-adm-2024-57",title:"Arrêt N°57/2024 — APIX c/ État du Sénégal — Responsabilité administrative",date:"2024-11-14",jurisdiction:"Cour Suprême — Ch. Administrative",matiere:"Administrative",numero:"J/446/RG/23",excerpt:"Dommages-intérêts sans base légale suffisante. Lien de causalité non établi. Cassation partielle.",texte:`ARRÊT N°57 — 14 novembre 2024 — CHAMBRE ADMINISTRATIVE\nAttendu que l'APIX n'a pas établi le lien de causalité direct entre l'activité en cause et les préjudices allégués ;\nPAR CES MOTIFS : CASSE partiellement l'arrêt sur le quantum.`},{id:"cs-adm-2024-walfadjri",title:"Arrêt N°27/2024 — Groupe Walfadjri c/ État — Liberté de la presse",date:"2024-04-11",jurisdiction:"Cour Suprême — Ch. Administrative",matiere:"Administrative",numero:"J/274/RG/23",excerpt:"Annulation de la suspension des activités audiovisuelles de Walfadjri. Décision non motivée.",texte:`ARRÊT N°27 — 11 avril 2024 — CHAMBRE ADMINISTRATIVE\nAttendu que la liberté de la presse ne peut être restreinte que dans des conditions strictement nécessaires ;\nAttendu que la décision n'est pas motivée et que la suspension totale est disproportionnée ;\nPAR CES MOTIFS : ANNULE la décision administrative.`},{id:"cs-adm-2022-marche",title:"Arrêt 2022 — Marché public 8 Mds FCFA — Annulation ARMP",date:"2022-09-28",jurisdiction:"Cour Suprême — Ch. Administrative",matiere:"Administrative",numero:"J/201/RG/21",excerpt:"Marché public annulé. Critères d'évaluation absents de l'AAO. Violation du principe de transparence.",texte:`ARRÊT — 28 septembre 2022 — CHAMBRE ADMINISTRATIVE\nAttendu que les critères d'évaluation des offres doivent être fixés dans l'Avis d'Appel d'Offres ;\nPAR CES MOTIFS : ANNULE la procédure d'attribution.`},{id:"cs-adm-2023-fonctionnaire",title:"Arrêt 2023 — Révocation fonctionnaire — Violation du contradictoire",date:"2023-07-06",jurisdiction:"Cour Suprême — Ch. Administrative",matiere:"Administrative",numero:"J/156/RG/22",excerpt:"Révocation d'un inspecteur des impôts sans audition préalable. Réintégration avec rappel de traitement.",texte:`ARRÊT — 6 juillet 2023 — CHAMBRE ADMINISTRATIVE\nAttendu que le Statut Général de la Fonction Publique garantit au fonctionnaire le droit d'être entendu ;\nPAR CES MOTIFS : ANNULE l'arrêté de révocation. Ordonne la réintégration.`},{id:"cs-adm-2020-retractation",title:"Ordonnance N°09/2020 — Rétractation — Conditions d'admissibilité",date:"2020-06-11",jurisdiction:"Cour Suprême — Ch. Administrative",matiere:"Administrative",numero:"J/391/RG/19",excerpt:"Demande en rétractation d'ordonnance. Voie de recours exceptionnelle. Conditions non remplies. Irrecevabilité.",texte:`ORDONNANCE N°09 — 11 juin 2020 — CHAMBRE ADMINISTRATIVE\nAttendu que la rétractation est une voie de recours exceptionnelle ;\nPAR CES MOTIFS : DÉCLARE la demande irrecevable.`},{id:"cs-pen-2021-ultrapetita",title:"Arrêt N°29/CJ-P/2021 — Ultra petita — Personnalisation de la peine",date:"2021-06-11",jurisdiction:"Cour Suprême — Ch. Pénale",matiere:"Pénale",numero:"2019-33/CJ-P",excerpt:"Ultra petita en matière pénale. Peine ne pouvant excéder les réquisitions du Parquet.",texte:`ARRÊT N°29/CJ-P — 11 juin 2021 — CHAMBRE PÉNALE\nAttendu que le principe ultra petita interdit en matière pénale de prononcer une peine dépassant les réquisitions du Parquet ;\nPAR CES MOTIFS : REJETTE le pourvoi.`},{id:"cs-pen-2022-escroquerie",title:"Arrêt 2022 — Escroquerie — Faux diplômes — Art.379 Code Pénal",date:"2022-04-14",jurisdiction:"Cour Suprême — Ch. Pénale",matiere:"Pénale",numero:"2021-18/CJ-P",excerpt:"Escroquerie par manœuvres frauduleuses. Présentation de faux diplômes. Condamnation à 2 ans.",texte:`ARRÊT — 14 avril 2022 — CHAMBRE PÉNALE\nAttendu que l'escroquerie est constituée par l'emploi de manœuvres frauduleuses ;\nPAR CES MOTIFS : REJETTE le pourvoi. Confirme 2 ans d'emprisonnement.`},{id:"cs-pen-2023-cybercrime",title:"Arrêt 2023 — Cybercriminalité — Phishing — Loi n°2008-11",date:"2023-10-05",jurisdiction:"Cour Suprême — Ch. Pénale",matiere:"Pénale",numero:"2022-27/CJ-P",excerpt:"Fraude informatique par phishing. Application de la Loi n°2008-11. Condamnation à 3 ans dont 18 mois ferme.",texte:`ARRÊT — 5 octobre 2023 — CHAMBRE PÉNALE\nAttendu que le phishing entre dans les prévisions de l'article 431-11 du Code Pénal modifié par la Loi n°2008-11 ;\nPAR CES MOTIFS : REJETTE le pourvoi. Condamne à 3 ans dont 18 mois ferme.`},{id:"cs-com-2022-ohada",title:"Arrêt 2022 — OHADA — Saisie-attribution — Banque UBA",date:"2022-11-09",jurisdiction:"Cour Suprême — Ch. Commerciale",matiere:"Commerciale",numero:"J/445/RG/21",excerpt:"Saisie-attribution OHADA. Secret bancaire non opposable. Obligation de déclaration du tiers saisi.",texte:`ARRÊT — 9 novembre 2022 — CHAMBRE COMMERCIALE\nAttendu que le secret bancaire n'est pas opposable dans le cadre de l'AUPSRVE OHADA ;\nPAR CES MOTIFS : REJETTE la mainlevée.`},{id:"cs-com-2023-societe",title:"Arrêt 2023 — Abus de biens sociaux — Gérant SARL — OHADA",date:"2023-05-18",jurisdiction:"Cour Suprême — Ch. Commerciale",matiere:"Commerciale",numero:"J/298/RG/22",excerpt:"Gérant SARL utilisant la trésorerie à des fins personnelles. Abus de biens sociaux.",texte:`ARRÊT — 18 mai 2023 — CHAMBRE COMMERCIALE\nAttendu que les dirigeants sont responsables de l'abus des biens sociaux à des fins personnelles ;\nPAR CES MOTIFS : CONFIRME la condamnation civile à restitution.`},{id:"cs-com-2021-faillite",title:"Arrêt 2021 — Règlement préventif OHADA — COVID-19 — AUPCAP",date:"2021-08-26",jurisdiction:"Cour Suprême — Ch. Commerciale",matiere:"Commerciale",numero:"J/167/RG/20",excerpt:"Règlement préventif ouvert pour difficultés post-COVID. Passif 4,2 Mds FCFA, actif 6,8 Mds.",texte:`ARRÊT — 26 août 2021 — CHAMBRE COMMERCIALE\nAttendu que l'AUPCAP OHADA prévoit le règlement préventif pour des difficultés sérieuses mais non irrémédiables ;\nPAR CES MOTIFS : ORDONNE l'ouverture. Suspend les poursuites individuelles pour 3 mois.`},{id:"cs-com-2023-cheque",title:"Arrêt 2023 — Chèque sans provision — Prescription UEMOA",date:"2023-02-16",jurisdiction:"Cour Suprême — Ch. Commerciale",matiere:"Commerciale",numero:"J/201/RG/22",excerpt:"Chèque présenté après 6 mois. Non-extinction de l'obligation civile. Prescription rejetée.",texte:`ARRÊT — 16 février 2023 — CHAMBRE COMMERCIALE\nAttendu que le dépassement du délai de présentation n'éteint pas le droit du porteur contre le tireur ;\nPAR CES MOTIFS : REJETTE l'exception de prescription civile.`},{id:"cc-2025-1C",title:"Décision N°1/C/2025 — Loi d'amnistie n°2024-09 — Conformité sous réserve",date:"2025-04-23",jurisdiction:"Conseil Constitutionnel",matiere:"Constitutionnelle",numero:"1/C/25",excerpt:"Loi d'amnistie. Conformité sous réserve. L'amnistie ne s'applique pas aux violations graves des droits de l'homme.",texte:`DÉCISION N°1/C/2025 — 23 avril 2025\nAttendu que l'exclusion des crimes contre l'humanité est conforme aux engagements internationaux du Sénégal ;\nPAR CES MOTIFS : DÉCLARE la loi conforme SOUS RÉSERVE que l'amnistie ne s'applique pas aux violations graves des droits de l'homme.`},{id:"cc-2024-garde-a-vue",title:"Décision 2024 — Garde à vue — Accès avocat — Inconstitutionnel",date:"2024-03-06",jurisdiction:"Conseil Constitutionnel",matiere:"Constitutionnelle",numero:"2/C/24",excerpt:"Dispositions du CPP reportant l'accès à l'avocat au-delà de 24h. Contraires à la Constitution.",texte:`DÉCISION — 6 mars 2024 — CONSEIL CONSTITUTIONNEL\nAttendu que le droit à l'assistance d'un avocat dès les premières heures constitue une garantie essentielle du procès équitable ;\nPAR CES MOTIFS : DÉCLARE contraires à la Constitution les dispositions reportant l'accès à l'avocat au-delà de 24 heures.`},{id:"cc-2023-propriete",title:"Décision 2023 — Nationalisation — Droit de propriété — Art.15 Constitution",date:"2023-11-10",jurisdiction:"Conseil Constitutionnel",matiere:"Constitutionnelle",numero:"3/C/23",excerpt:"Nationalisation sans modalités d'indemnisation. Conformité sous réserve d'un décret dans les 6 mois.",texte:`DÉCISION N°3/C/2023 — 10 novembre 2023\nAttendu que l'article 15 de la Constitution dispose que la nationalisation ne peut être effectuée que sous la condition d'une juste et préalable indemnité ;\nPAR CES MOTIFS : Déclare conforme sous réserve qu'un décret d'application soit soumis au Parlement dans les 6 mois.`},{id:"cs-fonc-2022-expropriation",title:"Arrêt 2022 — Expropriation — Autoroute Dakar-Thiès — Indemnisation préalable",date:"2022-05-19",jurisdiction:"Cour Suprême — Ch. Administrative",matiere:"Foncière",numero:"J/234/RG/21",excerpt:"Expropriation sans indemnisation préalable juste. Loi n°76-67. Expertise foncière ordonnée.",texte:`ARRÊT — 19 mai 2022 — CHAMBRE ADMINISTRATIVE\nAttendu que la Loi n°76-67 impose une indemnisation préalable calculée sur la valeur vénale des biens expropriés ;\nPAR CES MOTIFS : ANNULE la prise de possession. Ordonne une expertise foncière contradictoire.`},{id:"cs-fonc-2021-domaine",title:"Arrêt 2021 — Domaine national — Zone de terroirs — Conseil Rural",date:"2021-03-08",jurisdiction:"Cour Suprême — Ch. Administrative",matiere:"Foncière",numero:"J/078/RG/20",excerpt:"Occupation de 40 ans sur le domaine national. Aucun droit de propriété créé.",texte:`ARRÊT — 8 mars 2021 — CHAMBRE ADMINISTRATIVE\nAttendu que la Loi n°64-46 classe les terres non immatriculées en zones de terroirs gérées par les Conseils Ruraux ;\nAttendu que la détention de fait ne crée aucun droit de propriété sur le domaine national ;\nPAR CES MOTIFS : REJETTE la demande.`},{id:"cs-fonc-2023-titre",title:"Arrêt 2023 — Titre foncier — Opposabilité absolue — Droits coutumiers éteints",date:"2023-09-14",jurisdiction:"Cour Suprême — Ch. Civile",matiere:"Foncière",numero:"J/389/RG/22",excerpt:"Conflit titre foncier immatriculé vs droits coutumiers. Titre inattaquable après purge.",texte:`ARRÊT — 14 septembre 2023 — CHAMBRE CIVILE\nAttendu que le titre foncier, une fois définitivement établi après purge, est inattaquable, intangible et définitif selon le système Torrens ;\nPAR CES MOTIFS : CONFIRME l'opposabilité du titre foncier. Dit toute prétention coutumière éteinte.`}];

/* HELPERS */
function esc(t){return String(t||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');}
function hl(text,q){if(!q||!text)return esc(text||'');const r=q.replace(/[.*+?^${}()|[\]\\]/g,'\\$&');return esc(text).replace(new RegExp('('+r+')','gi'),'<span class="highlight">$1</span>');}
function fmtDate(d){if(!d)return'—';try{const dt=new Date(d);return isNaN(dt.getTime())?String(d):dt.toLocaleDateString('fr-FR',{day:'numeric',month:'long',year:'numeric'});}catch(e){return String(d);}}
function badgeClass(m){if(!m)return'badge-default';const v=m.toLowerCase();if(v.includes('civil'))return'badge-civile';if(v.includes('social'))return'badge-sociale';if(v.includes('admin'))return'badge-administrative';if(v.includes('pénal')||v.includes('penal'))return'badge-penale';if(v.includes('constitu'))return'badge-constitutionnelle';if(v.includes('commer'))return'badge-commerciale';if(v.includes('foncièr')||v.includes('foncier'))return'badge-fonciere';return'badge-default';}
function md2html(t){if(!t)return'';return t.replace(/\*\*(.+?)\*\*/g,'<strong>$1</strong>').replace(/\n\n+/g,'<br/><br/>').replace(/\n/g,'<br/>');}
function appendMsg(cont,text,role){const el=document.createElement('div');el.className='chat-msg '+role;el.innerHTML=role==='assistant'?md2html(text):esc(text);cont.appendChild(el);cont.scrollTop=cont.scrollHeight;return el;}
function typing(cont){const el=document.createElement('div');el.className='chat-msg assistant';el.innerHTML='<div class="tdots"><span></span><span></span><span></span></div>';cont.appendChild(el);cont.scrollTop=cont.scrollHeight;return el;}
function isSubscribed(){if(!currentSubscription)return false;if(currentSubscription.status!=='active')return false;if(!currentSubscription.expires_at)return false;return new Date(currentSubscription.expires_at)>new Date();}

/* AUTH */
sb.auth.onAuthStateChange(async(event,session)=>{
  currentUser=session?.user||null;
  if(currentUser){await loadSubscription();}else{currentSubscription=null;}
  updateHeader();renderPaywalls();
});

async function loadSubscription(){
  if(!currentUser||!supabaseAvailable)return;
  try{
    const{data}=await sb.from('subscriptions').select('*').eq('user_id',currentUser.id).eq('status','active').gt('expires_at',new Date().toISOString()).order('expires_at',{ascending:false}).limit(1);
    currentSubscription=(data&&data.length>0)?data[0]:null;
  }catch(e){currentSubscription=null;}
}

function updateHeader(){
  const dot=document.getElementById('subDot'),lbl=document.getElementById('userBtnLabel');
  if(!currentUser){dot.className='sub-dot dot-none';lbl.textContent='Connexion';}
  else if(isSubscribed()){dot.className='sub-dot dot-active';lbl.textContent=currentUser.email.split('@')[0];}
  else{dot.className='sub-dot dot-pending';lbl.textContent=currentUser.email.split('@')[0];}
}
function onUserBtnClick(){if(!currentUser)openModal('authModal');else openAccountModal();}
function goHome(){switchTabByName('juris');}

/* PAYWALLS */
function renderPaywalls(){
  renderPaywall('paywallWrap','Accédez aux analyses IA de chaque arrêt');
  renderPaywall('assistantPaywall',"Débloquez l'assistant IA en conversation libre");
}
function renderPaywall(id,desc){
  const el=document.getElementById(id);if(!el)return;
  if(!currentUser){
    el.innerHTML=`<div class="paywall-wrap"><div class="paywall-banner"><div class="pw-icon">🔒</div><div class="pw-text"><h3>Connexion requise</h3><p>${esc(desc)}</p></div><button class="btn-unlock" onclick="openModal('authModal')">Se connecter</button></div></div>`;
  }else if(!isSubscribed()){
    el.innerHTML=`<div class="paywall-wrap"><div class="paywall-banner"><div class="pw-icon">🌊</div><div class="pw-text"><h3>Abonnement requis — 1 000 FCFA/mois</h3><p>${esc(desc)} · Paiement sécurisé via Wave</p></div><button class="btn-unlock" onclick="openModal('waveModal')">S'abonner via Wave</button></div></div>`;
  }else{el.innerHTML='';}
}
function requireSub(){if(!currentUser){openModal('authModal');return false;}if(!isSubscribed()){openModal('waveModal');return false;}return true;}

/* AUTH FORMS */
function switchAuth(mode,btn){
  document.querySelectorAll('.atab').forEach(b=>b.classList.remove('active'));btn.classList.add('active');
  document.getElementById('loginForm').style.display=mode==='login'?'block':'none';
  document.getElementById('registerForm').style.display=mode==='register'?'block':'none';
  clearAuthMsgs();
}
function clearAuthMsgs(){const e=document.getElementById('authErr'),o=document.getElementById('authOk');if(e){e.className='err';e.textContent='';}if(o){o.className='ok';o.textContent='';}}
function showAuthErr(msg){const e=document.getElementById('authErr');e.className='err on';e.textContent=msg;}
function showAuthOk(msg){const o=document.getElementById('authOk');o.className='ok on';o.textContent=msg;}

async function doLogin(){
  const email=document.getElementById('loginEmail').value.trim(),pwd=document.getElementById('loginPwd').value;
  if(!email||!pwd){showAuthErr('Remplissez tous les champs.');return;}
  const btn=document.getElementById('loginBtn');btn.disabled=true;document.getElementById('loginSpin').className='loading-spin on';clearAuthMsgs();
  const{error}=await sb.auth.signInWithPassword({email,password:pwd});
  btn.disabled=false;document.getElementById('loginSpin').className='loading-spin';
  if(error){showAuthErr(error.message.includes('Invalid')?'Email ou mot de passe incorrect.':error.message);}else{closeModal('authModal');}
}
async function doRegister(){
  const name=document.getElementById('regName').value.trim(),email=document.getElementById('regEmail').value.trim(),pwd=document.getElementById('regPwd').value;
  if(!name||!email||!pwd){showAuthErr('Remplissez tous les champs.');return;}
  if(pwd.length<8){showAuthErr('Le mot de passe doit contenir au moins 8 caractères.');return;}
  const btn=document.getElementById('regBtn');btn.disabled=true;document.getElementById('regSpin').className='loading-spin on';clearAuthMsgs();
  const{error}=await sb.auth.signUp({email,password:pwd,options:{data:{full_name:name}}});
  btn.disabled=false;document.getElementById('regSpin').className='loading-spin';
  if(error){showAuthErr(error.message);}else{showAuthOk('✅ Compte créé ! Vérifiez votre email.');}
}

/* WAVE */
function onWaveClick(){setTimeout(()=>document.getElementById('wavePhone').focus(),300);const o=document.getElementById('waveOk');o.className='ok on';o.textContent='✅ Après le paiement Wave, saisissez votre numéro ci-dessous.';}
async function submitWavePayment(){
  if(!currentUser){openModal('authModal');return;}
  const phone=document.getElementById('wavePhone').value.trim().replace(/\s/g,'');
  const errEl=document.getElementById('waveErr'),okEl=document.getElementById('waveOk');
  if(!phone||phone.length<9){errEl.className='err on';errEl.textContent='Saisissez votre numéro Wave valide (ex: 771234567).';return;}
  errEl.className='err';okEl.className='ok';
  const btn=document.querySelector('.btn-vfy');btn.textContent='⏳…';btn.disabled=true;
  const{error}=await sb.from('payment_verifications').insert({user_id:currentUser.id,wave_ref:'PENDING_'+Date.now(),wave_phone:phone,status:'pending'});
  btn.textContent='Confirmer';btn.disabled=false;
  if(error){errEl.className='err on';errEl.textContent="Erreur. Contactez support@jurisen.sn";}
  else{okEl.className='ok on';okEl.textContent='✅ Demande enregistrée ! Activation sous 24h.';document.getElementById('wavePhone').value='';}
}

/* ACCOUNT */
function openAccountModal(){
  const sub=currentSubscription;
  let subHtml=isSubscribed()?`<span class="chip chip-active">✅ Actif</span><small style="display:block;color:#4a4540;font-size:.76rem;margin-top:3px;">Expire le ${new Date(sub.expires_at).toLocaleDateString('fr-FR',{day:'numeric',month:'long',year:'numeric'})}</small>`:
    sub&&sub.status==='pending'?`<span class="chip chip-pending">⏳ En attente</span>`:`<span class="chip chip-none">Aucun abonnement</span>`;
  document.getElementById('acctContent').innerHTML=`
    <div class="acct-row"><div class="acct-lbl">Email</div><div class="acct-val">${esc(currentUser.email)}</div></div>
    <div class="acct-row"><div class="acct-lbl">Abonnement</div><div class="acct-val">${subHtml}</div></div>
    <div class="acct-row"><div class="acct-lbl">Arrêts importés</div><div class="acct-val">${importedCases.length}</div></div>
    <div class="acct-row"><div class="acct-lbl">Favoris</div><div class="acct-val">${favorites.length}</div></div>
    ${!isSubscribed()?`<div class="acct-row"><button class="btn-wave" style="padding:11px" onclick="closeModal('accountModal');openModal('waveModal')">🌊 S'abonner — 1 000 FCFA/mois</button></div>`:''}
    <div class="acct-row"><button class="btn-danger" onclick="doLogout()">🚪 Se déconnecter</button></div>`;
  openModal('accountModal');
}
async function doLogout(){await sb.auth.signOut();closeModal('accountModal');}

/* MODALS */
function openModal(id){document.getElementById(id).classList.add('open');}
function closeModal(id){document.getElementById(id).classList.remove('open');}
document.addEventListener('click',e=>{if(e.target.classList.contains('ov'))e.target.classList.remove('open');});

/* TABS */
function switchTab(name,btn){
  document.querySelectorAll('.tab-panel').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('.tab-btn').forEach(b=>b.classList.remove('active'));
  document.getElementById('tab-'+name).classList.add('active');btn.classList.add('active');
}
function switchTabByName(name){const btn=document.querySelector(`.tab-btn[data-tab="${name}"]`);if(btn)btn.click();}

/* INIT */
function initApp(){
  allCases=[...BASE_CASES,...importedCases];
  applyFilters();renderFavorites();updateHeader();renderPaywalls();
}
function filterChip(btn,matiere){
  document.querySelectorAll('.filter-chip').forEach(b=>b.classList.remove('active'));btn.classList.add('active');
  activeFilter=matiere;applyFilters();
}
function applyFilters(){
  const q=(document.getElementById('searchInput').value||'').toLowerCase();
  filteredCases=allCases.filter(c=>{
    const matchM=activeFilter?c.matiere===activeFilter:true;
    const matchT=(c.title||'').toLowerCase().includes(q)||(c.excerpt||'').toLowerCase().includes(q)||(c.texte||'').toLowerCase().includes(q);
    return matchM&&matchT;
  });
  displayedCount=0;document.getElementById('cardList').innerHTML='';loadMore();
}
function loadMore(){
  const frag=document.createDocumentFragment(),limit=Math.min(displayedCount+PAGE_SIZE,filteredCases.length);
  for(let i=displayedCount;i<limit;i++)frag.appendChild(createCard(filteredCases[i]));
  document.getElementById('cardList').appendChild(frag);displayedCount=limit;
  document.getElementById('resultsCount').innerHTML=`<strong>${filteredCases.length}</strong> décision(s) trouvée(s)`;
  document.getElementById('listState').style.display=filteredCases.length===0?'block':'none';
  document.getElementById('loadMoreBtn').style.display=displayedCount<filteredCases.length?'inline-flex':'none';
}
function createCard(c){
  const div=document.createElement('div');
  const isImported=!BASE_CASES.find(bc=>bc.id===c.id);
  div.className='case-card'+(isImported?' imported-card':'');div.onclick=()=>openCase(c.id);
  const q=document.getElementById('searchInput').value;
  div.innerHTML=`<div class="card-top"><div class="card-title">${hl(c.title,q)}</div><div class="card-badge ${badgeClass(c.matiere)}">${esc(c.matiere)}</div></div>
    <div class="card-meta"><span>📅 ${fmtDate(c.date)}</span><span>🏛️ ${esc(c.jurisdiction)}</span>${isImported?'<span class="src-tag">Importé</span>':''}</div>
    <div class="card-excerpt">${hl(c.excerpt,q)}</div>`;
  return div;
}

/* DETAIL */
function openCase(id){
  currentCase=allCases.find(c=>c.id===id);if(!currentCase)return;
  document.getElementById('listView').style.display='none';
  document.getElementById('searchBarEl').style.display='none';
  document.getElementById('filtersBarEl').style.display='none';
  document.getElementById('paywallWrap').style.display='none';
  const dv=document.getElementById('detailView');
  const isFav=favorites.some(f=>f.id===currentCase.id);
  dv.innerHTML=`
    <button class="back-btn" onclick="backToList()">← Retour à la liste</button>
    <div class="detail-card">
      <div class="detail-jurisdiction">${esc(currentCase.jurisdiction)}</div>
      <h2 class="detail-title">${esc(currentCase.title)}</h2>
      <div class="detail-tags">
        <span class="detail-tag">📅 ${fmtDate(currentCase.date)}</span>
        <span class="detail-tag">📁 ${esc(currentCase.numero||'N/A')}</span>
        <span class="detail-tag">⚖️ ${esc(currentCase.matiere)}</span>
      </div>
      <div class="ai-actions-row">
        <button class="ai-action-btn btn-summarize" onclick="callAI('summarize',this)"><span class="bsp"></span><span class="blbl">🤖 Résumer</span></button>
        <button class="ai-action-btn btn-explain" onclick="callAI('explain',this)"><span class="bsp"></span><span class="blbl">💡 Expliquer simplement</span></button>
        <button class="ai-action-btn ${isFav?'btn-favorite is-fav':'btn-favorite'}" id="btnFavDetail" onclick="toggleFavorite('${esc(currentCase.id)}')">${isFav?'⭐ Favori':'☆ Ajouter aux favoris'}</button>
      </div>
    </div>
    <div id="aiResultBox" style="display:none"></div>
    <div class="fulltext-box"><div class="box-title">📄 Texte Intégral</div><div class="fulltext-content">${esc(currentCase.texte)}</div></div>`;
  dv.classList.add('visible');
}
function backToList(){
  const dv=document.getElementById('detailView');dv.classList.remove('visible');dv.innerHTML='';
  document.getElementById('listView').style.display='block';
  document.getElementById('searchBarEl').style.display='flex';
  document.getElementById('filtersBarEl').style.display='flex';
  document.getElementById('paywallWrap').style.display='';
  renderPaywalls();
}

/* IA */
async function fetchOpenAI(messages){
  if(OPENAI_KEY==='REMPLACEZ_PAR_VOTRE_CLE_OPENAI')return'⚠️ Clé OpenAI non configurée.';
  try{
    const res=await fetch('https://api.openai.com/v1/chat/completions',{method:'POST',headers:{'Content-Type':'application/json','Authorization':`Bearer ${OPENAI_KEY}`},body:JSON.stringify({model:'gpt-4o-mini',messages,temperature:0.3})});
    if(!res.ok){const err=await res.json().catch(()=>({}));return'Erreur API ('+res.status+') : '+(err.error?.message||'Réessayez.');}
    const data=await res.json();return data.choices[0].message.content;
  }catch(e){return"Erreur de connexion à l'IA.";}
}
async function callAI(action,btn){
  if(!requireSub())return;
  btn.classList.add('loading');btn.disabled=true;
  const aiBox=document.getElementById('aiResultBox');aiBox.style.display='block';
  aiBox.innerHTML=`<div class="ai-box"><div class="ai-box-header"><div class="ai-dot"></div><div class="ai-label">Analyse en cours…</div></div><div class="ai-content">L'assistant lit l'arrêt…</div></div>`;
  const prompt=action==='summarize'
    ?`Résume cet arrêt sénégalais. Structure : **Faits**, **Procédure**, **Problème de droit**, **Solution**.\n\n${currentCase.texte}`
    :`Explique cet arrêt à quelqu'un sans formation juridique. Utilise des mots simples.\n\n${currentCase.texte}`;
  const response=await fetchOpenAI([{role:'system',content:'Tu es un avocat sénégalais expert. Réponds en français.'},{role:'user',content:prompt}]);
  aiBox.innerHTML=`<div class="ai-box"><div class="ai-box-header"><div class="ai-label">🤖 Analyse IA</div></div><div class="ai-content">${md2html(response)}</div></div>`;
  btn.classList.remove('loading');btn.disabled=false;
}

/* CHAT LIBRE */
function useSug(el){document.getElementById('freeChatInput').value=el.textContent.trim();sendFreeChat();}
async function sendFreeChat(){
  if(!requireSub())return;
  const input=document.getElementById('freeChatInput'),text=input.value.trim();if(!text)return;
  const cont=document.getElementById('freeChatMessages');
  const welcome=document.getElementById('chatWelcome');if(welcome)welcome.style.display='none';
  appendMsg(cont,text,'user');input.value='';
  const btn=document.getElementById('freeSendBtn');btn.classList.add('loading');btn.disabled=true;
  const typingEl=typing(cont);
  freeHistory.push({role:'user',content:text});
  const response=await fetchOpenAI([{role:'system',content:'Tu es un expert en droit sénégalais. Sois précis, cite les articles si possible, et rappelle que tu ne remplaces pas un avocat.'},...freeHistory]);
  typingEl.remove();appendMsg(cont,response,'assistant');freeHistory.push({role:'assistant',content:response});
  btn.classList.remove('loading');btn.disabled=false;
}

/* FAVORIS */
function toggleFavorite(id){
  const caseObj=allCases.find(c=>c.id===id);if(!caseObj)return;
  const idx=favorites.findIndex(f=>f.id===id);
  if(idx>-1)favorites.splice(idx,1);else favorites.push({id:caseObj.id,title:caseObj.title,jurisdiction:caseObj.jurisdiction,date:caseObj.date});
  localStorage.setItem('jurisen_favorites',JSON.stringify(favorites));renderFavorites();
  const btn=document.getElementById('btnFavDetail');
  if(btn){const nowFav=idx===-1;btn.className=nowFav?'ai-action-btn btn-favorite is-fav':'ai-action-btn btn-favorite';btn.innerHTML=nowFav?'⭐ Favori':'☆ Ajouter aux favoris';}
}
function renderFavorites(){
  const count=favorites.length;
  document.getElementById('favBadge').textContent=count;document.getElementById('favBadge').classList.toggle('visible',count>0);
  document.getElementById('favCountLabel').textContent=count;
  const list=document.getElementById('favList');
  if(count===0){list.innerHTML=`<div class="fav-empty"><div class="fav-icon">📌</div><h3>Aucun favori</h3><p>Consultez un arrêt et cliquez sur ☆ Ajouter aux favoris</p></div>`;return;}
  list.innerHTML=favorites.map(f=>`
    <div class="fav-card" onclick="switchTabByName('juris');setTimeout(()=>openCase('${esc(f.id)}'),50)">
      <div class="fav-body"><div class="card-title" style="margin-bottom:4px;">${esc(f.title)}</div><div style="font-size:.75rem;color:#4a4540">${esc(f.jurisdiction||'')} · ${fmtDate(f.date)}</div></div>
      <button class="fav-rm" onclick="event.stopPropagation();toggleFavorite('${esc(f.id)}')">✕</button>
    </div>`).join('');
}

/* IMPORT */
function openImportModal(){if(!requireSub())return;openModal('importModal');}
function saveImport(){
  const title=document.getElementById('imp-title').value.trim(),texte=document.getElementById('imp-texte').value.trim();
  if(!title||!texte){alert('Le titre et le texte intégral sont obligatoires.');return;}
  const dateVal=document.getElementById('imp-date').value||new Date().toISOString().slice(0,10);
  const newCase={id:'import-'+Date.now(),title,date:dateVal,matiere:document.getElementById('imp-matiere').value||'Autre',jurisdiction:document.getElementById('imp-jurisdiction').value||'Inconnue',numero:document.getElementById('imp-numero').value||'',excerpt:document.getElementById('imp-excerpt').value.trim()||texte.substring(0,150)+'…',texte,url:document.getElementById('imp-url').value||''};
  importedCases.unshift(newCase);localStorage.setItem('jurisen_imports',JSON.stringify(importedCases));
  allCases=[...BASE_CASES,...importedCases];applyFilters();closeModal('importModal');
  ['imp-title','imp-date','imp-excerpt','imp-texte','imp-url','imp-numero'].forEach(id=>document.getElementById(id).value='');
  document.getElementById('imp-matiere').value='';document.getElementById('imp-jurisdiction').value='Cour Suprême';
}

/* PWA */
window.addEventListener('beforeinstallprompt',e=>{
  e.preventDefault();deferredInstall=e;
  const isStandalone=window.matchMedia('(display-mode: standalone)').matches||window.navigator.standalone===true;
  if(!isStandalone&&!localStorage.getItem('jurisen_dismiss_install'))document.getElementById('installBanner').classList.add('show');
});
async function doInstall(){
  if(!deferredInstall)return;
  document.getElementById('installBanner').classList.remove('show');deferredInstall.prompt();
  const{outcome}=await deferredInstall.userChoice;deferredInstall=null;
  if(outcome==='accepted')localStorage.setItem('jurisen_dismiss_install','true');
}
function dismissInstall(){document.getElementById('installBanner').classList.remove('show');localStorage.setItem('jurisen_dismiss_install','true');}

window.addEventListener('load',initApp);
</script>

</body>
</html>
