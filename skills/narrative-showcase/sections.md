# Section Type Catalog

Pick section types for your narrative showcase. Each type includes the HTML to place inside a `<section>` tag and any JS to append before `</script>`.

Every section starts with the standard header:

```html
<div class="section-header">
  <div class="section-number-row">
    <span class="section-number">01</span>
    <div class="section-divider"></div>
  </div>
  <h2 class="section-title">Section Title</h2>
  <p class="section-desc">One-line description.</p>
</div>
```

---

## 1. Accordion Cards

Expandable cards with color-coded borders. Click to reveal detail content. Only one open at a time.

**Use for:** Problem lists, feature lists, pain points, FAQ-style content.

### HTML

```html
<div class="card-list" id="my-list">
  <div class="expand-card" style="background:rgba(193,123,94,0.06);border-color:rgba(193,123,94,0.2)">
    <div class="expand-card-header">
      <span class="expand-card-icon" style="color:var(--c-red)">▸</span>
      <span class="expand-card-title" style="color:var(--text)">Card Title</span>
    </div>
    <div class="expand-card-detail">
      <p>Description text explaining the detail.</p>
      <div class="file-badge">path/to/file.ts:42-60</div>
      <div class="code-block"><span class="comment">// Code snippet here</span>
<span class="kw">const</span> x = <span class="fn">doSomething</span>();</div>
    </div>
  </div>
  <!-- repeat cards, change colors per category -->
</div>
```

### JS

```javascript
setupAccordion('#my-list', '.expand-card');
```

---

## 2. Clickable Diagram

Boxes connected by arrows with a shared detail panel that populates on click.

**Use for:** Interface contracts, component relationships, API flows, system overviews.

### HTML

```html
<div class="interface-flow">
  <div class="interface-box" onclick="selectNode(this, 'node1')">
    <div class="interface-name">Node One</div>
    <div class="interface-role">Brief role</div>
  </div>
  <span class="arch-arrow" style="font-size:20px">→</span>
  <div class="interface-box" onclick="selectNode(this, 'node2')">
    <div class="interface-name">Node Two</div>
    <div class="interface-role">Brief role</div>
  </div>
</div>

<div class="detail-panel" id="diagram-detail" style="border-color:rgba(212,168,83,0.2)">
  <button class="detail-panel-close" onclick="this.parentElement.classList.remove('visible')">&times;</button>
  <h3 id="diagram-detail-title"></h3>
  <div id="diagram-detail-content"></div>
</div>
```

### JS

```javascript
const diagramData = {
  'node1': {
    title: 'Node One',
    content: '<p class="label">Purpose</p><p style="font-size:12px;color:var(--text-muted)">Description here.</p><div class="code-block">code here</div>'
  },
  'node2': {
    title: 'Node Two',
    content: '<p class="label">Purpose</p><p style="font-size:12px;color:var(--text-muted)">Description here.</p>'
  }
};

function selectNode(el, key) {
  document.querySelectorAll('.interface-box').forEach(b => b.classList.remove('selected'));
  el.classList.add('selected');
  const data = diagramData[key];
  document.getElementById('diagram-detail-title').textContent = data.title;
  document.getElementById('diagram-detail-content').innerHTML = data.content;
  document.getElementById('diagram-detail').classList.add('visible');
}
```

---

## 3. Toggle Comparison

A Before/After (or any A/B) toggle that swaps between two content blocks.

**Use for:** Refactor stories, migration comparisons, version comparisons.

### HTML

```html
<div class="toggle-bar">
  <button class="toggle-btn active" onclick="toggleView('before', this)">Before</button>
  <button class="toggle-btn" onclick="toggleView('after', this)">After</button>
</div>

<div id="view-before">
  <!-- "before" content: arch-diagram, cards, any HTML -->
</div>

<div id="view-after" style="display:none">
  <!-- "after" content -->
</div>
```

### JS

```javascript
function toggleView(view, btn) {
  document.querySelectorAll('.toggle-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  document.getElementById('view-before').style.display = view === 'before' ? 'block' : 'none';
  document.getElementById('view-after').style.display = view === 'after' ? 'block' : 'none';
}
```

**Note:** If you have multiple toggles on the same page, scope the selectors (e.g., `#s03 .toggle-btn`) and use unique IDs for the content divs.

---

## 4. Animated Step Trace

Numbered steps with play/pause, progress bar, and expandable code detail per step.

**Use for:** Data flows, request lifecycles, pipeline walkthroughs.

### HTML

```html
<div class="step-trace" id="trace-main"></div>
<div class="play-controls">
  <button class="play-btn" onclick="toggleStepPlay()" id="play-btn-main">▶</button>
  <div class="progress-bar"><div class="progress-fill" id="progress-main"></div></div>
  <span class="step-counter" id="counter-main">0 / 0</span>
</div>
```

### JS

```javascript
const stepData = [
  { text: 'Step description', component: 'ComponentName', file: 'path/file.ts:10-20',
    code: '<span class="kw">const</span> x = <span class="fn">doThing</span>();' },
  // ... more steps
];

function renderSteps() {
  const container = document.getElementById('trace-main');
  container.innerHTML = stepData.map((step, i) => `
    <div class="step${i === 0 ? ' active' : ''}" onclick="activateStep(${i})">
      <div class="step-num">${i + 1}</div>
      <span class="step-text">${step.text}</span>
      <span class="step-component">${step.component}</span>
    </div>
    <div class="step-detail" id="step-detail-${i}">
      <div class="file-badge">${step.file}</div>
      <div class="code-block">${step.code}</div>
    </div>
  `).join('');
  updateProgress(0);
}

const traceState = { step: 0, playing: false, interval: null };

function activateStep(idx) {
  traceState.step = idx;
  document.querySelectorAll('#trace-main .step').forEach((s, i) => s.classList.toggle('active', i <= idx));
  document.querySelectorAll('[id^="step-detail-"]').forEach((d, i) => d.classList.toggle('expanded', i === idx));
  updateProgress(idx);
  if (traceState.playing) toggleStepPlay();
}

function updateProgress(idx) {
  const total = stepData.length;
  document.getElementById('progress-main').style.width = ((idx + 1) / total * 100) + '%';
  document.getElementById('counter-main').textContent = (idx + 1) + ' / ' + total;
}

function toggleStepPlay() {
  traceState.playing = !traceState.playing;
  document.getElementById('play-btn-main').textContent = traceState.playing ? '⏸' : '▶';
  if (traceState.playing) {
    traceState.interval = setInterval(() => {
      const next = traceState.step + 1;
      if (next >= stepData.length) { toggleStepPlay(); return; }
      traceState.step = next;
      document.querySelectorAll('#trace-main .step').forEach((s, i) => s.classList.toggle('active', i <= next));
      document.querySelectorAll('[id^="step-detail-"]').forEach((d, i) => d.classList.toggle('expanded', i === next));
      updateProgress(next);
    }, 1500);
  } else {
    clearInterval(traceState.interval);
  }
}

renderSteps();

document.addEventListener('keydown', (e) => {
  if (e.key === 'ArrowRight' || e.key === 'ArrowLeft') {
    let next = traceState.step + (e.key === 'ArrowRight' ? 1 : -1);
    next = Math.max(0, Math.min(stepData.length - 1, next));
    activateStep(next);
  }
});
```

**Note:** If combining with Toggle Comparison (before/after traces), give each trace unique IDs and state objects.

---

## 5. Card Catalog

Categorized grid of clickable cards with a shared detail panel.

**Use for:** Service inventories, tool lists, API endpoints, module catalogs.

### HTML

```html
<div id="catalog-container"></div>

<div class="detail-panel" id="catalog-detail" style="border-color:rgba(212,168,83,0.2)">
  <button class="detail-panel-close" onclick="this.parentElement.classList.remove('visible')">&times;</button>
  <h3 id="catalog-detail-title"></h3>
  <div id="catalog-detail-badge"></div>
  <div id="catalog-detail-content"></div>
</div>
```

### JS

```javascript
const categories = [
  {
    name: 'Category Name', color: '#d4a853', count: 2,
    items: [
      { name: 'ItemOne', role: 'Brief description', file: 'path/file.ts',
        detail: '<p class="label">Key Method</p><div class="code-block">code here</div>' },
      { name: 'ItemTwo', role: 'Brief description', file: 'path/other.ts',
        detail: '<p style="font-size:12px;color:var(--text-muted)">Explanation.</p>' },
    ]
  },
  // Add { removed: true } to a category for dashed/dimmed cards
];

function renderCatalog() {
  const container = document.getElementById('catalog-container');
  container.innerHTML = categories.map(cat =>
    '<div style="margin-bottom:24px">' +
      '<div class="service-category-header">' +
        '<div class="service-category-bar" style="background:' + cat.color + '"></div>' +
        '<span class="service-category-label" style="color:' + cat.color + '">' + cat.name + '</span>' +
        '<span class="service-category-count" style="background:' + cat.color + '15;color:' + cat.color + '">' + cat.count + '</span>' +
        '<div class="service-category-line" style="background:' + cat.color + '20"></div>' +
      '</div>' +
      '<div style="display:flex;flex-wrap:wrap;gap:8px">' +
        cat.items.map((item, i) =>
          '<div class="svc-card' + (cat.removed ? ' removed' : '') + '"' +
          ' style="background:' + cat.color + '0a;border-color:' + cat.color + '25"' +
          ' onclick="showCatalogDetail(\'' + cat.name + '-' + i + '\',\'' + item.name + '\',\'' + cat.color + '\',\'' + cat.name + '\',\'' + item.file + '\',this)">' +
            '<div class="svc-name" style="color:' + cat.color + '">' + item.name + '</div>' +
            '<div class="svc-role">' + item.role + '</div>' +
          '</div>'
        ).join('') +
      '</div>' +
    '</div>'
  ).join('');
}

const catalogMap = {};
categories.forEach(cat => cat.items.forEach((item, i) => { catalogMap[cat.name + '-' + i] = item; }));

function showCatalogDetail(id, name, color, catName, file, el) {
  const item = catalogMap[id];
  if (!item) return;
  document.querySelectorAll('.svc-card').forEach(c => c.classList.remove('selected'));
  el.classList.add('selected');
  const panel = document.getElementById('catalog-detail');
  document.getElementById('catalog-detail-title').textContent = name;
  document.getElementById('catalog-detail-title').style.color = color;
  document.getElementById('catalog-detail-badge').innerHTML = '<span class="badge" style="background:' + color + '20;color:' + color + '">' + catName + '</span>';
  document.getElementById('catalog-detail-content').innerHTML = '<div class="file-badge">' + file + '</div>' + item.detail;
  panel.style.borderColor = color + '33';
  panel.classList.add('visible');
}

renderCatalog();
```

---

## 6. Timeline

Horizontal phase nodes with connectors. Optionally paired with a signal flow diagram.

**Use for:** Workflow phases, event sequences, state machines, process stages.

### HTML

```html
<div class="timeline" id="my-timeline"></div>

<div class="detail-panel" id="timeline-detail" style="border-color:rgba(107,163,104,0.2)">
  <button class="detail-panel-close" onclick="this.parentElement.classList.remove('visible')">&times;</button>
  <h3 id="timeline-detail-title" style="color:var(--c-green)"></h3>
  <div id="timeline-detail-content"></div>
</div>

<!-- Optional: signal flow diagram -->
<div class="signal-flow">
  <div class="signal-row" data-signal="signal-name">
    <span class="signal-from" style="background:rgba(107,163,104,0.1);color:var(--c-green)">From</span>
    <div class="signal-arrow">
      <div class="signal-line"></div>
      <span class="signal-label">label</span>
      <span class="signal-arrowhead">→</span>
    </div>
    <span class="signal-to" style="background:rgba(107,163,104,0.1);color:var(--c-green)">To</span>
  </div>
</div>
```

### JS

```javascript
const phases = [
  { name: 'Phase One', signals: [],
    code: '<span class="kw">await</span> <span class="fn">doPhaseOne</span>();' },
  { name: 'Phase Two', signals: ['signal-name'],
    code: '<span class="kw">await</span> <span class="fn">doPhaseTwo</span>();' },
];

function renderTimeline() {
  document.getElementById('my-timeline').innerHTML = phases.map((p, i) =>
    '<div class="timeline-node' + (i === 0 ? ' active' : '') + '" onclick="selectPhase(' + i + ')">' +
      '<div class="timeline-circle">' + (i + 1) + '</div>' +
      '<div class="timeline-label">' + p.name + '</div>' +
      (i < phases.length - 1 ? '<div class="timeline-connector"></div>' : '') +
    '</div>'
  ).join('');
}

function selectPhase(idx) {
  const phase = phases[idx];
  document.querySelectorAll('.timeline-node').forEach((n, i) => n.classList.toggle('active', i === idx));
  document.getElementById('timeline-detail-title').textContent = phase.name;
  document.getElementById('timeline-detail-content').innerHTML = '<div class="code-block">' + phase.code + '</div>';
  document.getElementById('timeline-detail').classList.add('visible');
  document.querySelectorAll('.signal-row').forEach(row => {
    row.classList.toggle('active', phase.signals.includes(row.dataset.signal));
  });
}

renderTimeline();
```

---

## 7. Comparison Table

Styled table with category colors and check/cross markers. No interactivity.

**Use for:** Feature matrices, capability grids, before/after inventories.

### HTML

```html
<div style="overflow-x:auto">
  <table style="width:100%;border-collapse:collapse;font-family:var(--font-mono);font-size:11px">
    <thead>
      <tr style="border-bottom:1px solid var(--border)">
        <th style="text-align:left;padding:8px;color:var(--text-muted);font-weight:500">Item</th>
        <th style="text-align:center;padding:8px;color:var(--text-muted);font-weight:500">Column A</th>
        <th style="text-align:center;padding:8px;color:var(--text-muted);font-weight:500">Column B</th>
      </tr>
    </thead>
    <tbody>
      <tr style="border-bottom:1px solid rgba(42,37,32,0.5)">
        <td style="padding:8px;color:var(--text)">Row One</td>
        <td style="text-align:center;padding:8px;color:var(--c-green)">✓</td>
        <td style="text-align:center;padding:8px;color:var(--c-red)">✗</td>
      </tr>
    </tbody>
  </table>
</div>
```

### JS

None needed.

---

## 8. Callout Box

Highlighted box with colored left border for key insights or warnings.

**Use for:** Key insights, unusual patterns, warnings, TL;DRs.

### HTML

```html
<div class="callout">
  <h4>Callout Title</h4>
  <p>Main explanatory text.</p>
  <ul>
    <li>Point one</li>
    <li>Point two</li>
    <li>Point three</li>
  </ul>
</div>
```

To change the callout color (default is teal), override the inline styles:

```html
<div class="callout" style="background:rgba(193,123,94,0.06);border-color:rgba(193,123,94,0.2);border-left-color:#c17b5e">
  <h4 style="color:#c17b5e">Warning Title</h4>
  <p>Warning text.</p>
</div>
```

### JS

None needed.
