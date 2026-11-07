# COLOR-CLUB
from pathlib import Path
html = r'''<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>COLOUR WORLD — Camera Edition</title>
<style>
:root{--bg:#0b0f16;--panel:#121a27;--panel2:#182233;--line:#334156;--text:#edf2f7;--muted:#98a6ba;--accent:#7da2ff}
*{box-sizing:border-box}body{margin:0;background:var(--bg);color:var(--text);font-family:Arial,Helvetica,sans-serif}
button,input{font:inherit}button{cursor:pointer}.launcher{min-height:100vh;display:grid;place-items:center;background:radial-gradient(circle at 50% 25%,#243653 0,#0b0f16 60%)}
.tile{width:260px;padding:24px;border-radius:24px;border:1px solid var(--line);background:linear-gradient(145deg,#182235,#0f1520);color:#fff;text-align:center;box-shadow:0 22px 60px #0009}
.tileIcon{width:110px;height:110px;margin:auto;border-radius:28px;background:conic-gradient(#ff4d6d,#ffb703,#8cff98,#4cc9f0,#8b5cf6,#ff4d6d);display:grid;place-items:center;color:#101522;font-weight:900;font-size:31px}
.tile h1{font-size:20px;margin:16px 0 5px}.tile p{color:var(--muted);font-size:12px}.open{width:100%;padding:11px;border:0;border-radius:9px;background:var(--accent);font-weight:800;color:#101522}
.app{display:none;min-height:100vh}.top{height:62px;display:flex;align-items:center;justify-content:space-between;padding:0 18px;background:#101722;border-bottom:1px solid var(--line)}
.top b{letter-spacing:1px}.back{background:#202b3c;color:#e8eef8;border:1px solid #3a4960;border-radius:8px;padding:8px 11px}
.layout{display:grid;grid-template-columns:175px minmax(0,1fr);min-height:calc(100vh - 62px)}
.side{padding:14px;background:#101722;border-right:1px solid var(--line)}.side button{width:100%;text-align:left;margin:4px 0;padding:10px;border:1px solid transparent;border-radius:7px;background:transparent;color:#b4bfd0}.side button.active{background:#26334a;border-color:#41526d;color:white}
.main{padding:20px;max-width:1200px;width:100%;margin:auto}.main h2{margin:0}.sub{color:var(--muted);font-size:12px;margin:6px 0 18px}
.cameraBox{display:grid;grid-template-columns:minmax(0,1fr) 280px;gap:16px}.videoWrap{background:#05080d;border:1px solid var(--line);border-radius:14px;overflow:hidden;position:relative;min-height:280px}
video{width:100%;height:100%;max-height:520px;object-fit:cover;display:block}.target{position:absolute;left:50%;top:50%;width:48px;height:48px;transform:translate(-50%,-50%);border:3px solid white;border-radius:50%;box-shadow:0 0 0 9999px #0001}
.controls,.info{background:var(--panel);border:1px solid var(--line);border-radius:12px;padding:14px}.controls button{width:100%;padding:11px;margin:5px 0;border-radius:8px;border:1px solid #40506a;background:#202b3d;color:white;font-weight:700}.controls .primary{background:var(--accent);color:#111827;border:0}.status{font-size:11px;color:var(--muted);padding:8px 0}
.preview{height:80px;border-radius:9px;background:#273247;border:1px solid #ffffff22;margin-bottom:12px}.name{font-size:22px;font-weight:900}.label{font-size:10px;color:var(--muted);letter-spacing:1px;margin-top:12px}.fact{font-size:13px;line-height:1.55;margin-top:5px}
.library{margin-top:20px;background:var(--panel);border:1px solid var(--line);border-radius:12px;padding:14px}.grid{display:grid;grid-template-columns:repeat(10,1fr);gap:7px}.sw{aspect-ratio:1;border-radius:7px;border:1px solid #ffffff33}
.footer{text-align:center;color:#75839a;font-size:11px;padding:24px}
@media(max-width:760px){.layout{grid-template-columns:1fr}.side{display:flex;overflow:auto;padding:8px}.side button{white-space:nowrap;width:auto}.cameraBox{grid-template-columns:1fr}.grid{grid-template-columns:repeat(7,1fr)}}
</style>
</head>
<body>
<section id="launcher" class="launcher">
  <div class="tile">
    <div class="tileIcon">CW</div>
    <h1>COLOUR WORLD</h1>
    <p>CAMERA COLOUR EXPLORER</p>
    <button id="openApp" class="open">OPEN APP ↗</button>
  </div>
</section>

<section id="app" class="app">
<header class="top"><b>◈ COLOUR WORLD <span style="color:#7da2ff">/ CAMERA STUDIO</span></b><button id="desktop" class="back">← Desktop</button></header>
<div class="layout">
<nav class="side"><button class="active">📷 Camera Scan</button><button id="libraryBtn">🎨 Colour Library</button><button id="aboutBtn">ⓘ About</button></nav>
<main class="main">
<section id="cameraPage">
<h2>Camera Colour Scan</h2><div class="sub">Point the centre circle at an object, then press SCAN COLOUR.</div>
<div class="cameraBox">
<div class="videoWrap"><video id="video" autoplay playsinline muted></video><div class="target"></div><canvas id="canvas" hidden></canvas></div>
<div class="controls">
<button id="start" class="primary">▶ START CAMERA</button>
<button id="scan">◎ SCAN CENTRE COLOUR</button>
<button id="stop">■ STOP CAMERA</button>
<div id="status" class="status">Camera is waiting.</div>
<div class="info">
<div id="preview" class="preview"></div><div id="colourName" class="name">No colour scanned</div>
<div class="label">WHERE YOU CAN SEE IT</div><div id="where" class="fact">Start the camera and scan an object.</div>
<div class="label">CRAZY FACT</div><div id="fact" class="fact">Colour facts will appear here.</div>
<div class="label">TECHNICAL</div><div id="technical" class="fact">—</div>
</div>
</div></div></section>

<section id="libraryPage" hidden><h2>200-Colour Library</h2><div class="sub">A quick visual collection of shades.</div><div class="library"><div id="grid" class="grid"></div></div></section>
<section id="aboutPage" hidden><h2>About COLOUR WORLD</h2><div class="library"><p>COLOUR WORLD is an interactive colour exploration program created for discovering colours in everyday life using a visual library and camera scanning.</p><p><b>Created by Simak</b></p><p>As a part of the Technofest Project</p></div></section>
<div class="footer">Created by Simak • As a part of the Technofest Project</div>
</main></div>
</section>

<script>
const colours=[
['Red','#FF0000','strawberries, stop signs and apples','Red pigments have been used in art for thousands of years.'],
['Pink','#FFC0CB','flowers, candy and decorations','Pink is a tint made by mixing red with white.'],
['Blue','#0000FF','the sky, denim and swimming pools','The sky looks blue because of how sunlight scatters in the atmosphere.'],
['Yellow','#FFFF00','lemons, bananas and sunshine','Yellow is one of the most visible colours from far away.'],
['Green','#008000','grass, leaves and plants','Chlorophyll gives many plants their green colour.'],
['Orange','#FFA500','oranges, carrots and autumn leaves','Orange sits between red and yellow on the colour spectrum.'],
['Purple','#800080','grapes, flowers and fabrics','Historically, some purple dyes were extremely expensive.'],
['Black','#000000','ink, tyres and clothing','Black objects absorb much of the visible light hitting them.'],
['White','#FFFFFF','paper, clouds and milk','White surfaces reflect a broad mix of visible light.'],
['Brown','#A52A2A','wood, soil and chocolate','Brown is often described as a dark orange family of colours.'],
['Turquoise','#40E0D0','tropical water and jewellery','Turquoise is also the name of a famous blue-green gemstone.'],
['Burgundy','#800020','deep fabrics and grapes','Burgundy is named after the Burgundy wine region in France.'],
['Amber','#FFBF00','honey and fossil resin','Real amber can contain tiny ancient insects and plants.'],
['Beige','#F5F5DC','sand, walls and natural fabrics','Beige is popular in interiors because it is a calm neutral.'],
['Teal','#008080','water imagery and peacock feathers','Teal is named after the colourful Eurasian teal duck.'],
['Mauve','#E0B0FF','flowers and vintage fabrics','Mauve became famous after one of the first synthetic dyes was discovered.'],
['Chartreuse','#7FFF00','bright leaves and fashion','Chartreuse is named after a French liqueur.'],
['Periwinkle','#CCCCFF','flowers and stationery','Periwinkle sits between blue and violet.'],
['Vermilion','#E34234','traditional art and pottery','Vermilion has been used as a powerful red-orange pigment in art.'],
['Cerulean','#007BA7','clear-sky artwork and ceramics','Cerulean comes from a Latin word connected with the sky.'],
['Sage','#9CAF88','sage leaves and interior design','Sage green is a muted colour often used for calm spaces.'],
['Ochre','#CC7722','earth pigments and clay','Ochre pigments are among the oldest used by humans.'],
['Taupe','#483C32','stone, furniture and fabrics','Taupe is a useful neutral between brown and grey.'],
['Coral','#FF7F50','coral reefs and sunset colours','Coral colour is a mix of warm pink and orange ideas.'],
['Indigo','#4B0082','denim and textiles','Indigo dye has been used for centuries around the world.'],
['Lavender','#E6E6FA','flowers and soft decorations','Lavender is named after the fragrant flower.'],
['Mint','#98FF98','mint leaves and pastel designs','Mint colours are usually pale mixtures of green and blue.'],
['Peach','#FFE5B4','peaches and soft sunset tones','Peach colours are popular in warm pastel palettes.'],
['Crimson','#DC143C','flowers and rich fabrics','Crimson has long been associated with deep red dyes.'],
['Cyan','#00FFFF','digital screens and printing','Cyan is one of the key colours used in colour printing.'],
['Olive','#808000','olive fruit and leafy branches','Olive is a yellow-green shade with an earthy look.'],
['Ivory','#FFFFF0','ceramics and creamy fabrics','Ivory is a warm off-white rather than pure white.'],
['Lilac','#C8A2C8','lilac flowers and decorations','Lilac is a pale purple inspired by the flower.'],
['Gold','#FFD700','medals and jewellery','Gold colour is strongly connected with the reflective metal.'],
['Silver','#C0C0C0','metal and jewellery','Silver-coloured surfaces can look different depending on reflections.'],
['Emerald','#50C878','gemstones and foliage','Emerald is a green gemstone variety of beryl.'],
['Mustard','#FFDB58','spices and autumn clothing','Mustard colours are muted yellow shades.']
];
const extras=['Azure','Rose','Jade','Plum','Salmon','Slate','Copper','Bronze','Charcoal','Sand','Ice Blue','Cobalt','Ultramarine','Amethyst','Terracotta','Fuchsia','Scarlet','Ruby','Maroon','Navy','Sky Blue','Aqua','Sea Green','Forest Green','Lime','Lemon','Canary','Cream','Khaki','Tan','Chocolate','Sienna','Umber','Pearl','Smoke','Steel Blue','Midnight Blue','Orchid','Violet','Magenta','Apricot','Rust','Mahogany','Champagne','Onyx','Pearl Grey','Glaucous','Tyrian Purple','Egyptian Blue'];
const hexes=['#007FFF','#FF007F','#00A86B','#8E4585','#FA8072','#708090','#B87333','#CD7F32','#36454F','#C2B280','#D6F1FF','#0047AB','#120A8F','#9966CC','#E2725B','#FF00FF','#FF2400','#E0115F','#800000','#000080','#87CEEB','#00FFFF','#2E8B57','#228B22','#00FF00','#FFF44F','#FFFF99','#FFFDD0','#C3B091','#D2B48C','#7B3F00','#E97451','#635147','#F0EAD6','#73808C','#4682B4','#191970','#DA70D6','#8F00FF','#FF00FF','#FBCEB1','#B7410E','#C04000','#F7E7CE','#353839','#D1D1D1','#6082B6','#66023C','#1034A6'];
for(let i=0;colours.length<200;i++){let name=extras[i%extras.length]+(i>=extras.length?' '+(Math.floor(i/extras.length)+1):'');let hex=hexes[i%hexes.length];colours.push([name,hex,'art, nature, clothing, objects and design','Every colour can look different depending on nearby colours and lighting.']);}

const $=id=>document.getElementById(id), pages=['cameraPage','libraryPage','aboutPage'];
function showPage(id){pages.forEach(x=>$(x).hidden=x!==id)}
$('openApp').onclick=()=>{$('launcher').style.display='none';$('app').style.display='block'};
$('desktop').onclick=()=>{$('app').style.display='none';$('launcher').style.display='grid';stopCamera()};
$('libraryBtn').onclick=()=>showPage('libraryPage');$('aboutBtn').onclick=()=>showPage('aboutPage');

const grid=$('grid');colours.forEach(c=>{let b=document.createElement('button');b.className='sw';b.style.background=c[1];b.title=c[0];b.onclick=()=>display(c);grid.appendChild(b)});
function rgb(hex){let n=parseInt(hex.slice(1),16);return [(n>>16)&255,(n>>8)&255,n&255]}
function dist(a,b){return Math.sqrt((a[0]-b[0])**2+(a[1]-b[1])**2+(a[2]-b[2])**2)}
function nearest(r,g,b){let best=colours[0],score=Infinity;for(const c of colours){let d=dist([r,g,b],rgb(c[1]));if(d<score){score=d;best=c}}return best}
function display(c,raw){$('preview').style.background=raw||c[1];$('colourName').textContent=c[0];$('where').textContent=c[2];$('fact').textContent=c[3];$('technical').textContent=raw?`Detected colour: ${raw} • Closest library match: ${c[1]}`:`Library colour: ${c[1]}`}
let stream=null;
$('start').onclick=async()=>{try{$('status').textContent='Requesting camera permission...';stream=await navigator.mediaDevices.getUserMedia({video:{facingMode:'environment'},audio:false});$('video').srcObject=stream;$('status').textContent='Camera running. Point the centre circle at a colour.'}catch(e){$('status').textContent='Camera could not start. Use HTTPS/localhost and allow camera permission.'}};
$('stop').onclick=stopCamera;function stopCamera(){if(stream){stream.getTracks().forEach(t=>t.stop());stream=null;$('video').srcObject=null;$('status').textContent='Camera stopped.'}}
$('scan').onclick=()=>{const v=$('video');if(!v.videoWidth){$('status').textContent='Start the camera first.';return}let c=$('canvas');c.width=v.videoWidth;c.height=v.videoHeight;let ctx=c.getContext('2d',{willReadFrequently:true});ctx.drawImage(v,0,0);let x=Math.floor(c.width/2),y=Math.floor(c.height/2),s=Math.max(10,Math.floor(Math.min(c.width,c.height)*0.04));let data=ctx.getImageData(x-s,y-s,s*2,s*2).data,R=0,G=0,B=0,n=0;for(let i=0;i<data.length;i+=4){R+=data[i];G+=data[i+1];B+=data[i+2];n++}R=Math.round(R/n);G=Math.round(G/n);B=Math.round(B/n);let raw='#'+[R,G,B].map(x=>x.toString(16).padStart(2,'0')).join('').toUpperCase();display(nearest(R,G,B),raw);$('status').textContent='Colour scanned successfully.'};
</script></body></html>'''
path = Path("/mnt/data/Colour_World_Camera_HTTPS_Ready.html")
path.write_text(html, encoding="utf-8")
print(f"Created: {path}")

