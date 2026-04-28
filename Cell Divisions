<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Cell Division Simulation</title>
    <style>
        :root {
            --bg: #0f172a; --cell-bg: #fdf2f8; --blue-chrom: #3b82f6;
            --red-chrom: #ef4444; --accent: #10b981; --centromere: #ffeb3b;
        }
        body { font-family: 'Inter', sans-serif; background: var(--bg); color: white; margin: 0; display: flex; flex-direction: column; align-items: center; min-height: 100vh; overflow-x: hidden; touch-action: manipulation; }
        
        .header { text-align: center; padding: 15px; width: 100%; background: #1e293b; border-bottom: 2px solid #334155; box-sizing: border-box;}
        .stats-bar { display: flex; gap: 15px; justify-content: center; margin-top: 10px; }
        .stat-card { background: #0f172a; padding: 8px 12px; border-radius: 8px; border: 1px solid #334155; min-width: 90px; text-align: center;}
        .stat-label { font-size: 9px; color: #94a3b8; text-transform: uppercase; }
        .stat-val { font-size: 16px; font-weight: bold; color: var(--accent); display: block; }

        /* CHANGED FOR RECOMMENDATION 2 */
        .main-layout { 
            display: flex; gap: 20px; padding: 20px; width: 1100px; box-sizing: border-box; 
            transform: scale(0.75); /* Shrinks layout to 75% */
            transform-origin: top center; 
            margin: 0 auto; 
        }
        
        .instruction-panel { width: 320px; background: #1e293b; padding: 20px; border-radius: 12px; display: flex; flex-direction: column; gap: 10px; border-left: 4px solid var(--accent); position: relative; box-sizing: border-box;}
        
        .simulation-space { position: relative; flex-grow: 1; height: 560px; background: #000; border-radius: 12px; display: flex; align-items: center; justify-content: center; border: 2px solid #334155; box-sizing: border-box;}
        .cell { width: 520px; height: 520px; background: var(--cell-bg); border-radius: 50%; border: 6px solid #ec4899; position: relative; transition: all 0.5s ease; }
        
        /* Division Lines */
        .equator-line { position: absolute; top: 50%; width: 100%; border-top: 2px dashed #db277788; z-index: 1; }
        .vert-equator { position: absolute; left: 50%; height: 100%; border-left: 2px dashed #3b82f688; display: none; z-index: 1; }
        .cell-wall-horiz { position: absolute; top: 49.5%; width: 100%; height: 8px; background: #ec4899; display: none; z-index: 5;}
        .cell-wall-vert { position: absolute; left: 49.5%; height: 100%; width: 8px; background: #ec4899; display: none; z-index: 5;}

        /* Chromosome Architecture */
        .chrom-complex { position: absolute; cursor: move; z-index: 10; display: flex; align-items: center; justify-content: space-between; touch-action: none; }
        .chrom-complex.horiz { flex-direction: column; height: 26px; }
        .chrom-complex.vert { flex-direction: row; width: 26px; }

        .chromatid-single { position: absolute; cursor: move; z-index: 11; display: flex; touch-action: none; }
        .chromatid { display: flex; box-sizing: border-box; border: 2px solid rgba(0,0,0,0.5); border-radius: 5px; overflow: hidden; pointer-events: none; }
        
        .horiz .chromatid { flex-direction: row; width: 100%; height: 11px; }
        .vert .chromatid { flex-direction: column; width: 11px; height: 100%; }

        .arm { transition: background-color 0.3s; }
        .horiz .arm { height: 100%; }
        .vert .arm { width: 100%; }

        .centromere-btn { position: absolute; width: 16px; height: 16px; background: var(--centromere); border-radius: 50%; border: 2px solid #000; cursor: pointer; z-index: 20; top: 50%; left: 50%; transform: translate(-50%, -50%); }
        
        .blue { background-color: var(--blue-chrom); }
        .red { background-color: var(--red-chrom); }

        /* UI */
        .btn { background: var(--accent); color: #064e3b; border: none; padding: 12px; border-radius: 6px; font-weight: bold; cursor: pointer; width: 100%; margin-top: auto;}
        .btn:disabled { background: #334155; color: #64748b; cursor: not-allowed; }
        
        .mode-btn { background: #334155; color: white; border: none; padding: 8px; border-radius: 4px; flex: 1; cursor: pointer; font-size: 12px; }
        .mode-btn.active { background: #6366f1; font-weight: bold; }
        
        .co-btn { background: #8b5cf6; color: white; border: none; padding: 6px; border-radius: 4px; cursor: pointer; font-size: 11px; width: 100%; margin-bottom: 4px;}
        .co-btn:hover { background: #7c3aed; }
        
        .feedback { font-size: 13px; color: #f87171; font-weight: bold; min-height: 40px; margin-top: 10px; line-height: 1.4;}

        /* Final Info Modal & Diagram Trees */
        #end-modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.95); z-index: 100; align-items: center; justify-content: center; }
        .end-box { background: #1e293b; padding: 30px; border-radius: 12px; width: 700px; text-align: center; border: 2px solid var(--accent); }
        .end-title { color: var(--accent); margin-top: 0; }
        .diagram-container { display: flex; flex-direction: column; align-items: center; margin: 30px 0; }
        .cell-node { width: 110px; height: 110px; border-radius: 50%; border: 4px solid #ec4899; display: flex; flex-direction: column; align-items: center; justify-content: center; background: var(--cell-bg); color: #0f172a; font-size: 13px; font-weight: bold; line-height: 1.3; box-shadow: 0 0 15px rgba(236, 72, 153, 0.4); }
        .cell-node span { font-weight: normal; font-size: 12px; color: #334155; margin-top: 4px;}
    </style>
</head>
<body>

<div class="header">
    <h2>Cell Division Simulation</h2>
    <div class="stats-bar">
        <div class="stat-card"><span class="stat-label">Phase</span><span id="label-phase" class="stat-val">Prophase</span></div>
        <div class="stat-card"><span class="stat-label">Ploidy</span><span id="label-ploidy" class="stat-val">2n</span></div>
        <div class="stat-card"><span class="stat-label">Chromosomes</span><span id="label-count" class="stat-val">6</span></div>
        <div class="stat-card"><span class="stat-label">DNA Content</span><span id="label-dna" class="stat-val">4c</span></div>
    </div>
</div>

<div class="main-layout">
    <div class="instruction-panel">
        <div style="display:flex; gap:5px; margin-bottom: 15px;">
            <button id="m-mitosis" class="mode-btn active" onclick="changeMode('mitosis')">Mitotic cell division</button>
            <button id="m-meiosis" class="mode-btn" onclick="changeMode('meiosis')">Meiotic cell division</button>
        </div>
        
        <div id="crossover-controls" style="display: none; background: #0f172a; padding: 10px; border-radius: 8px; border: 1px solid #3b82f6;">
            <div style="font-size: 11px; color: #93c5fd; margin-bottom: 5px; text-align: center;">Crossing Over Toggles</div>
            <button class="co-btn" onclick="toggleCross(1)">Cross Pair 1 (Long)</button>
            <button class="co-btn" onclick="toggleCross(2)">Cross Pair 2 (Short)</button>
            <button class="co-btn" onclick="toggleCross(3)">Cross Pair 3 (Medium)</button>
        </div>

        <h3 id="step-name" style="margin:15px 0 5px 0; color:var(--accent);">Prophase</h3>
        <p id="step-desc" style="font-size:13px; color:#94a3b8; line-height: 1.4;">Drag chromosomes into the cell.</p>
        
        <div id="feedback" class="feedback"></div>
        <button class="btn" id="next-btn" onclick="nextStep()" disabled>Next Phase</button>
    </div>

    <div class="simulation-space">
        <div class="cell" id="cell">
            <div class="equator-line" id="eq-main"></div>
            <div class="vert-equator" id="eq-vert"></div>
            <div class="cell-wall-horiz" id="wall-horiz"></div>
            <div class="cell-wall-vert" id="wall-vert"></div>
        </div>
    </div>
</div>

<div id="end-modal">
    <div class="end-box">
        <h2 class="end-title" id="end-title">Cytokinesis Complete</h2>
        <div id="end-diagram" class="diagram-container"></div>
        <button class="btn" style="width: auto; padding: 12px 30px;" onclick="document.getElementById('end-modal').style.display='none'; reset();">Restart Simulation</button>
    </div>
</div>

<script>
    let mode = 'mitosis';
    let step = 0;
    const cell = document.getElementById('cell');
    let chromosomes = []; 
    let chromatids = [];  

    const initialData = [
        {id:'B1', type:1, color:'blue', w:80, a1:40, a2:40, x:-100, y:30},
        {id:'R1', type:1, color:'red',  w:80, a1:40, a2:40, x:-100, y:90},
        {id:'B2', type:2, color:'blue', w:40, a1:20, a2:20, x:-100, y:150},
        {id:'R2', type:2, color:'red',  w:40, a1:20, a2:20, x:-100, y:210},
        {id:'B3', type:3, color:'blue', w:60, a1:20, a2:40, x:-100, y:270},
        {id:'R3', type:3, color:'red',  w:60, a1:20, a2:40, x:-100, y:330}
    ];

    const mitosisSteps = [
        { n: "Prophase", d: "Drag all 6 chromosomes into the nucleus area." },
        { n: "Metaphase", d: "Align all 6 chromosomes in a single horizontal row on the dashed equator." },
        { n: "Anaphase", d: "Click centromeres to split. Drag chromatids to top and bottom poles. Each pole must get EXACTLY one complete set (1 of each color/size)." },
        { n: "Telophase", d: "Chromosomes reach poles. The cell is ready to divide." }
    ];

    const meiosisSteps = [
        { n: "Prophase I", d: "Drag into cell. Pair up homologous chromosomes side-by-side to initiate crossing over." },
        { n: "Metaphase I", d: "Align the 3 homologous PAIRS on the horizontal equator." },
        { n: "Anaphase I", d: "Do NOT split! Drag whole chromosomes to opposite poles. Each pole must get exactly one Type 1, one Type 2, and one Type 3." },
        { n: "Telophase I", d: "First division complete. Cells are now haploid (n)." },
        { n: "Prophase II", d: "Notice the two separate cells (solid line) and the new equators (dotted line). Chromosomes have rotated vertically." },
        { n: "Metaphase II", d: "Drag chromosomes to align exactly on the NEW vertical dotted line in the center of their respective cells." },
        { n: "Anaphase II", d: "Click centromeres to split! Drag sister chromatids left and right to the poles of their respective cells." },
        { n: "Telophase II", d: "4 unique haploid (n) nuclei form." }
    ];

    function changeMode(m) {
        mode = m;
        document.getElementById('m-mitosis').className = m === 'mitosis' ? 'mode-btn active' : 'mode-btn';
        document.getElementById('m-meiosis').className = m === 'meiosis' ? 'mode-btn active' : 'mode-btn';
        reset();
    }

    function reset() {
        step = 0;
        document.getElementById('eq-main').style.display = "block";
        document.getElementById('eq-vert').style.display = "none";
        document.getElementById('wall-horiz').style.display = "none";
        document.getElementById('wall-vert').style.display = "none";
        document.getElementById('crossover-controls').style.display = "none";
        
        cell.querySelectorAll('.chrom-complex, .chromatid-single').forEach(e => e.remove());
        chromosomes = []; chromatids = [];
        
        initialData.forEach(d => {
            const el = createComplex(d, false);
            addDrag(el);
            cell.appendChild(el);
            chromosomes.push(el);
        });
        updateUI();
        validate();
    }

    function createComplex(d, isVert) {
        const wrap = document.createElement('div');
        wrap.className = `chrom-complex ${isVert ? 'vert' : 'horiz'}`;
        wrap.dataset.oid = d.id; wrap.dataset.type = d.type; wrap.dataset.color = d.color; wrap.dataset.w = d.w; wrap.dataset.a1 = d.a1; wrap.dataset.a2 = d.a2;
        
        const styleW = isVert ? `width:26px; height:${d.w}px;` : `width:${d.w}px; height:26px;`;
        wrap.style = `left:${d.x}px; top:${d.y}px; ${styleW}`;

        for(let i=0; i<2; i++) {
            const c = document.createElement('div'); c.className = 'chromatid';
            const a1 = document.createElement('div'); a1.className = `arm ${d.color}`; 
            a1.style[isVert ? 'height' : 'width'] = d.a1 + 'px';
            const a2 = document.createElement('div'); a2.className = `arm ${d.color} arm2`; 
            a2.style[isVert ? 'height' : 'width'] = d.a2 + 'px';
            c.append(a1, a2);
            wrap.appendChild(c);
        }

        const centro = document.createElement('div'); 
        centro.className = 'centromere-btn';
        centro.onclick = () => split(wrap.dataset.oid);
        wrap.appendChild(centro);

        return wrap;
    }

    function toggleCross(type) {
        const b = chromosomes.find(c => c.dataset.oid === `B${type}`);
        const r = chromosomes.find(c => c.dataset.oid === `R${type}`);
        if(!b || !r) return;
        
        const b_arm2 = b.querySelectorAll('.arm2')[1]; 
        const r_arm2 = r.querySelectorAll('.arm2')[0];

        if(b_arm2.classList.contains('blue')) {
            b_arm2.classList.replace('blue', 'red');
            r_arm2.classList.replace('red', 'blue');
        } else {
            b_arm2.classList.replace('red', 'blue');
            r_arm2.classList.replace('blue', 'red');
        }
    }

    function addDrag(el) {
        el.onpointerdown = (e) => {
            if(e.target.classList.contains('centromere-btn')) return;
            let ox = e.clientX - el.offsetLeft;
            let oy = e.clientY - el.offsetTop;
            
            const isM2 = mode === 'meiosis' && step >= 4;
            const startY = parseInt(el.style.top || 0);
            const inTopCell = startY < 260;

            document.onpointermove = (e) => {
                let nx = e.clientX - ox;
                let ny = e.clientY - oy;

                if (step > 0) {
                    if (nx < 20) nx = 20; if (nx > 480) nx = 480;
                    if (ny < 20) ny = 20; if (ny > 480) ny = 480;

                    if (isM2) { 
                        if (inTopCell && ny > 210) ny = 210; 
                        if (!inTopCell && ny < 280) ny = 280; 
                    }
                }

                el.style.left = nx + "px";
                el.style.top = ny + "px";
                validate();
            };
            document.onpointerup = () => { document.onpointermove = null; document.onpointerup = null; };
        };
    }

    function split(id) {
        const curStepName = (mode === 'mitosis' ? mitosisSteps : meiosisSteps)[step].n;
        
        if(mode === 'mitosis' && curStepName !== 'Anaphase') {
            document.getElementById('feedback').innerText = "Centromeres only split during Anaphase!"; return;
        }
        if(mode === 'meiosis' && curStepName !== 'Anaphase II') {
            document.getElementById('feedback').innerText = "Centromeres ONLY split during Anaphase II (not Anaphase I)!"; return;
        }

        const complex = chromosomes.find(c => c.dataset.oid === id);
        if(!complex) return;

        const isVert = complex.classList.contains('vert');
        const x = parseInt(complex.style.left);
        const y = parseInt(complex.style.top);
        
        const cNodes = complex.querySelectorAll('.chromatid');

        for(let i=0; i<2; i++) {
            const c = document.createElement('div');
            c.className = `chromatid-single ${isVert ? 'vert' : 'horiz'}`;
            
            c.style[isVert ? 'width' : 'height'] = '11px';
            c.style[isVert ? 'height' : 'width'] = complex.dataset.w + 'px';
            
            if(isVert) {
                c.style.left = (x + (i===0 ? -6 : 14)) + "px";
                c.style.top = y + "px";
            } else {
                c.style.left = x + "px";
                c.style.top = (y + (i===0 ? -6 : 14)) + "px";
            }
            
            c.innerHTML = `<div class="chromatid" style="${isVert?'flex-direction:column; width:11px; height:100%':'flex-direction:row; width:100%; height:11px'}">${cNodes[i].innerHTML}</div>`;
            
            c.dataset.oid = complex.dataset.oid; 
            c.dataset.type = complex.dataset.type;
            c.dataset.color = complex.dataset.color;
            
            addDrag(c);
            cell.appendChild(c);
            chromatids.push(c);
        }
        
        complex.remove();
        chromosomes = chromosomes.filter(c => c.dataset.oid !== id);
        updateStats();
        validate();
    }

    function validate() {
        const btn = document.getElementById('next-btn');
        const feedback = document.getElementById('feedback');
        feedback.innerText = "";
        feedback.style.color = "#f87171"; 
        let valid = false;
        const sName = (mode === 'mitosis' ? mitosisSteps : meiosisSteps)[step].n;

        const getInZone = (elems, axis, min, max) => elems.filter(el => {
            const val = parseInt(el.style[axis]); return val >= min && val <= max;
        });
        const hasTypes = (elems, types) => {
            const tArray = elems.map(e => parseInt(e.dataset.type));
            return types.every(t => tArray.includes(t));
        };
        const hasExactOIDs = (elems, req) => {
            if(elems.length !== req.length) return false;
            const oids = elems.map(e => e.dataset.oid); return req.every(r => oids.includes(r));
        };
        const isTouching = (a, b) => Math.hypot(parseInt(a.style.left)-parseInt(b.style.left), parseInt(a.style.top)-parseInt(b.style.top)) < 80;

        if (sName === "Telophase" || sName === "Telophase I" || sName === "Prophase II" || sName === "Telophase II") {
            valid = true;
            feedback.style.color = "#10b981";
            if(sName === "Telophase") feedback.innerText = "Mitosis complete. Click 'Finish Cytokinesis' to view resulting cells.";
            if(sName === "Telophase I") feedback.innerText = "Meiosis I complete. Click 'Prophase II' to continue to the second division.";
            if(sName === "Prophase II") feedback.innerText = "Review the newly formed cell walls and vertical alignment. Click 'Next Phase' to begin Metaphase II.";
            if(sName === "Telophase II") feedback.innerText = "Meiosis II complete. Click 'Finish Cytokinesis' to view resulting gametes.";
        }
        else if (sName === "Prophase") {
            const allItems = [...chromosomes, ...chromatids];
            if(allItems.every(c => parseInt(c.style.left) > 30)) valid = true;
        }
        else if (sName === "Prophase I") {
            const allItems = [...chromosomes, ...chromatids];
            if(allItems.every(c => parseInt(c.style.left) > 30)) {
                const b1 = chromosomes.find(c=>c.dataset.oid==='B1'), r1 = chromosomes.find(c=>c.dataset.oid==='R1');
                const b2 = chromosomes.find(c=>c.dataset.oid==='B2'), r2 = chromosomes.find(c=>c.dataset.oid==='R2');
                const b3 = chromosomes.find(c=>c.dataset.oid==='B3'), r3 = chromosomes.find(c=>c.dataset.oid==='R3');
                
                if (isTouching(b1, r1) && isTouching(b2, r2) && isTouching(b3, r3)) {
                    valid = true;
                    document.getElementById('crossover-controls').style.display = "block";
                    feedback.style.color = "#10b981";
                    feedback.innerText = "Homologous pairs formed! You may now trigger crossing over, then click Next Phase.";
                } else {
                    document.getElementById('crossover-controls').style.display = "none";
                    feedback.innerText = "Pair ALL 3 homologous sets side-by-side to unlock crossing over.";
                }
            }
        } 
        else if (sName === "Metaphase") {
            if(chromosomes.every(c => Math.abs(parseInt(c.style.top) - 247) < 25)) valid = true;
            else feedback.innerText = "Align all 6 exactly on the horizontal dashed equator.";
        }
        else if (sName === "Metaphase I") {
            if(chromosomes.every(c => Math.abs(parseInt(c.style.top) - 247) < 35)) valid = true;
            else feedback.innerText = "Align pairs near the horizontal equator.";
        }
        else if (sName === "Metaphase II") {
            if(chromosomes.every(c => Math.abs(parseInt(c.style.left) - 247) < 35)) {
                const top = getInZone(chromosomes, 'top', 0, 260);
                const bot = getInZone(chromosomes, 'top', 260, 520);
                if(top.length === 3 && bot.length === 3) valid = true;
            } else feedback.innerText = "Drag chromosomes to align vertically on the dotted line in their respective cells.";
        }
        else if (sName === "Anaphase") {
            if(chromatids.length === 12) {
                const top = getInZone(chromatids, 'top', 0, 130);
                const bot = getInZone(chromatids, 'top', 370, 500);
                const req = ['B1','R1','B2','R2','B3','R3'];
                if(hasExactOIDs(top, req) && hasExactOIDs(bot, req)) valid = true;
                else feedback.innerText = "Each pole needs an exact complete set (1 of each color and size).";
            } else feedback.innerText = "Split all centromeres first!";
        }
        else if (sName === "Anaphase I") {
            const top = getInZone(chromosomes, 'top', 0, 140);
            const bot = getInZone(chromosomes, 'top', 360, 500);
            if(top.length===3 && bot.length===3 && hasTypes(top, [1,2,3]) && hasTypes(bot, [1,2,3])) valid = true;
            else feedback.innerText = "Each pole must receive ONE Type 1, ONE Type 2, and ONE Type 3. Do not split centromeres!";
        }
        else if (sName === "Anaphase II") {
            if(chromatids.length === 12) {
                const tl = chromatids.filter(c => parseInt(c.style.top)<260 && parseInt(c.style.left)<130);
                const tr = chromatids.filter(c => parseInt(c.style.top)<260 && parseInt(c.style.left)>360);
                const bl = chromatids.filter(c => parseInt(c.style.top)>260 && parseInt(c.style.left)<130);
                const br = chromatids.filter(c => parseInt(c.style.top)>260 && parseInt(c.style.left)>360);
                
                if(hasTypes(tl,[1,2,3]) && hasTypes(tr,[1,2,3]) && hasTypes(bl,[1,2,3]) && hasTypes(br,[1,2,3])) valid = true;
                else feedback.innerText = "Drag left and right! Each of the 4 poles needs a Type 1, 2, and 3 chromatid.";
            } else feedback.innerText = "Split centromeres first!";
        }

        btn.disabled = !valid;
    }

    function updateStats() {
        const p = document.getElementById('label-ploidy'), c = document.getElementById('label-count'), d = document.getElementById('label-dna');
        const sName = (mode === 'mitosis' ? mitosisSteps : meiosisSteps)[step].n;
        
        if (mode === 'mitosis') {
            if(sName === "Anaphase") { set('2n', chromatids.length, '4c'); }
            else if(sName === "Telophase") { set('2n', '6', '2c'); }
            else { set('2n', '6', '4c'); }
        } else {
            if(step <= 2) { set('2n', '6', '4c'); }
            else if(step === 3 || step === 4 || step === 5) { set('n', '3', '2c'); }
            else if(step === 6) { set('n', '6 (temp)', '2c'); }
            else { set('n', '3', '1c'); }
        }
        function set(pp, cc, dd) { p.innerText=pp; c.innerText=cc; d.innerText=dd; }
    }

    function updateUI() {
        const stepsArr = mode === 'mitosis' ? mitosisSteps : meiosisSteps;
        const s = stepsArr[step];
        const btn = document.getElementById('next-btn');

        document.getElementById('step-name').innerText = s.n;
        document.getElementById('step-desc').innerText = s.d;
        document.getElementById('label-phase').innerText = s.n;
        
        if(s.n !== "Prophase I") document.getElementById('crossover-controls').style.display = "none";

        if(s.n === "Prophase II") {
            document.getElementById('wall-horiz').style.display = 'block';
            document.getElementById('eq-main').style.display = 'none';
            document.getElementById('eq-vert').style.display = 'block';
            
            chromosomes.forEach(c => {
                if(!c.classList.contains('vert')) {
                    c.classList.replace('horiz', 'vert');
                    c.style.width = '26px';
                    c.style.height = c.dataset.w + 'px';
                    const chr = c.querySelectorAll('.chromatid');
                    chr.forEach(ch => {
                        ch.style.flexDirection = 'column'; ch.style.width = '11px'; ch.style.height = '100%';
                        ch.children[0].style.width = '100%'; ch.children[0].style.height = c.dataset.a1 + 'px';
                        ch.children[1].style.width = '100%'; ch.children[1].style.height = c.dataset.a2 + 'px';
                    });
                }
            });
        }
        
        if(s.n === "Telophase II") {
            document.getElementById('wall-vert').style.display = 'block';
        }

        if (s.n === "Telophase" || s.n === "Telophase II") {
            btn.innerText = "Finish Cytokinesis";
        } else if (s.n === "Telophase I") {
            btn.innerText = "Prophase II";
        } else {
            btn.innerText = "Next Phase";
        }

        updateStats();
    }

    function nextStep() {
        const stepsArr = mode === 'mitosis' ? mitosisSteps : meiosisSteps;
        if(step === stepsArr.length - 1) { 
            showEndScreen();
            return; 
        }
        step++;
        updateUI();
        validate(); 
    }

    function showEndScreen() {
        const m = document.getElementById('end-modal');
        const title = document.getElementById('end-title');
        const diagram = document.getElementById('end-diagram');
        
        if(mode === 'mitosis') {
            title.innerText = "Mitosis & Cytokinesis Complete";
            diagram.innerHTML = `
                <div class="cell-node">Mother Cell<br><span>Ploidy: 2n<br>Chr: 6<br>DNA: 4c</span></div>
                <div style="width:2px; height:30px; background:white;"></div>
                <div style="display:flex; width:300px; justify-content:space-between; border-top:2px solid white; padding-top:20px;">
                    <div class="cell-node" style="margin-top:-20px;">Daughter Cell 1<br><span>Ploidy: 2n<br>Chr: 6<br>DNA: 2c</span></div>
                    <div class="cell-node" style="margin-top:-20px;">Daughter Cell 2<br><span>Ploidy: 2n<br>Chr: 6<br>DNA: 2c</span></div>
                </div>
            `;
            document.getElementById('wall-horiz').style.display = 'block';
        } else {
            title.innerText = "Meiosis & Cytokinesis Complete";
            diagram.innerHTML = `
                <div class="cell-node">Mother Cell<br><span>Ploidy: 2n<br>Chr: 6<br>DNA: 4c</span></div>
                <div style="width:2px; height:30px; background:white;"></div>
                <div style="display:flex; width:550px; justify-content:space-between; border-top:2px solid white; padding-top:20px;">
                    <div class="cell-node" style="margin-top:-20px;">Gamete 1<br><span>Ploidy: n<br>Chr: 3<br>DNA: 1c</span></div>
                    <div class="cell-node" style="margin-top:-20px;">Gamete 2<br><span>Ploidy: n<br>Chr: 3<br>DNA: 1c</span></div>
                    <div class="cell-node" style="margin-top:-20px;">Gamete 3<br><span>Ploidy: n<br>Chr: 3<br>DNA: 1c</span></div>
                    <div class="cell-node" style="margin-top:-20px;">Gamete 4<br><span>Ploidy: n<br>Chr: 3<br>DNA: 1c</span></div>
                </div>
            `;
            document.getElementById('wall-horiz').style.display = 'block';
            document.getElementById('wall-vert').style.display = 'block';
        }
        m.style.display = 'flex';
    }

    reset();
</script>
</body>
</html>
