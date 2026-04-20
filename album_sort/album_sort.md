---
layout: none
title: Album Sort
description: My creation for the AP CSP exam
permalink: /album-sort
---

<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rainbow Album Sort</title>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #0a0a0a;
    --surface: #141414;
    --border: rgba(255,255,255,0.08);
    --text: #f0f0f0;
    --text-dim: rgba(240,240,240,0.35);
    --radius: 6px;
  }

  html, body {
    width: 100%;
    height: 100%;
  }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: ui-monospace, 'Cascadia Code', 'Source Code Pro', Menlo, monospace;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 2rem 1rem;
    min-height: 100%;
    overflow-x: hidden;
  }

  header {
    text-align: center;
    margin-bottom: 2.5rem;
  }

  header h1 {
    font-size: clamp(1rem, 3vw, 1.4rem);
    font-weight: 700;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    color: var(--text);
    margin-bottom: 0.4rem;
  }

  header p {
    font-size: 0.68rem;
    color: var(--text-dim);
    letter-spacing: 0.1em;
    text-transform: uppercase;
  }

  .spectrum-bar {
    width: 100%;
    max-width: 760px;
    height: 3px;
    background: linear-gradient(to right,
      #C0392B, #E05530, #E8821A, #D4C200,
      #2E7D32, #1565C0, #0D3B6E, #6A1B9A, #3E0066, #C2185B);
    border-radius: 2px;
    margin-bottom: 0.75rem;
    opacity: 0.55;
  }

  .spectrum-labels {
    width: 100%;
    max-width: 760px;
    display: flex;
    justify-content: space-between;
    margin-bottom: 1.5rem;
  }

  .spectrum-labels span {
    font-size: 0.58rem;
    color: var(--text-dim);
    letter-spacing: 0.08em;
    text-transform: uppercase;
  }

  #rank-list {
    display: flex;
    flex-direction: row;
    gap: 8px;
    width: 100%;
    max-width: 760px;
    align-items: flex-start;
    position: relative;
  }

  .rank-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 5px;
    flex: 1;
    min-width: 0;
    cursor: grab;
    user-select: none;
    touch-action: none;
    transition: transform 0.28s cubic-bezier(0.25, 0.46, 0.45, 0.94);
  }

  .rank-item.no-transition {
    transition: none;
  }

  .rank-item.ghost {
    opacity: 0.12;
  }

  .album-cover {
    width: 100%;
    aspect-ratio: 1 / 1;
    border-radius: var(--radius);
    overflow: hidden;
    border: 1px solid var(--border);
    position: relative;
    background: var(--surface);
    transition: border-color 0.15s, box-shadow 0.15s;
    pointer-events: none;
  }

  .rank-item:hover:not(.ghost) .album-cover {
    border-color: rgba(255,255,255,0.18);
  }

  .album-cover img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
  }

  .album-cover .fallback {
    width: 100%;
    height: 100%;
  }

  .rank-num {
    font-size: 0.58rem;
    color: var(--text-dim);
    letter-spacing: 0.05em;
    pointer-events: none;
  }

  .rank-item.correct-item .album-cover {
    border-color: #1D9E75;
    box-shadow: 0 0 0 1px #1D9E75;
  }
  .rank-item.wrong-item .album-cover {
    border-color: #E24B4A;
    box-shadow: 0 0 0 1px #E24B4A;
  }
  .rank-item.correct-item .rank-num { color: #1D9E75; }
  .rank-item.wrong-item .rank-num   { color: #E24B4A; }

  /* Floating drag clone */
  #drag-clone {
    position: fixed;
    pointer-events: none;
    z-index: 999;
    display: none;
    flex-direction: column;
    align-items: center;
    gap: 5px;
  }

  #drag-clone .album-cover {
    transform: scale(1.1) translateY(-6px);
    border-color: rgba(255,255,255,0.4) !important;
    box-shadow: 0 16px 48px rgba(0,0,0,0.75);
    transition: none;
  }

  /* Insert indicator */
  #insert-indicator {
    position: fixed;
    width: 2px;
    background: #ffffff;
    border-radius: 1px;
    pointer-events: none;
    opacity: 0;
    transition: opacity 0.1s, left 0.12s cubic-bezier(0.25, 0.46, 0.45, 0.94);
    z-index: 998;
  }

  #insert-indicator.visible {
    opacity: 1;
  }

  .controls {
    display: flex;
    gap: 12px;
    margin-top: 2rem;
    align-items: center;
  }

  button {
    font-family: inherit;
    font-size: 0.68rem;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    padding: 9px 20px;
    border-radius: var(--radius);
    cursor: pointer;
    transition: all 0.15s;
  }

  #check-btn {
    background: var(--text);
    color: var(--bg);
    border: none;
    font-weight: 700;
  }
  #check-btn:hover  { background: #ccc; }
  #check-btn:active { transform: scale(0.97); }

  #reset-btn {
    background: transparent;
    color: var(--text-dim);
    border: 1px solid var(--border);
  }
  #reset-btn:hover { color: var(--text); border-color: rgba(255,255,255,0.22); }

  #result {
    margin-top: 1.25rem;
    font-size: 0.7rem;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    min-height: 1.2em;
    text-align: center;
  }
  #result.correct { color: #1D9E75; }
  #result.wrong   { color: #E24B4A; }
</style>
</head>
<body>

<header>
  <h1>Rainbow Album Sort</h1>
  <p>Drag into rainbow order — reddest to pinkest</p>
</header>

<div class="spectrum-bar"></div>
<div class="spectrum-labels"><span>Red</span><span>Pink</span></div>

<div id="rank-list"></div>
<div id="drag-clone"><div class="album-cover"></div></div>
<div id="insert-indicator"></div>

<div class="controls">
  <button id="check-btn" onclick="checkOrder()">Check</button>
  <button id="reset-btn" onclick="resetGame()">Shuffle</button>
</div>
<div id="result"></div>

<script>
// ── Data ──────────────────────────────────────────────────────────────────────
const ALBUMS = [
  { id:0, title:"Wish",                artist:"The Cure",          src:"{{site.baseurl}}/album_sort/albums/wish_thecure.png", color:"#C0392B", correctPos:0 },
  { id:1, title:"Short Bus",           artist:"Filter",            src:"{{site.baseurl}}/album_sort/albums/shortbus_filter.png", color:"#E05530", correctPos:1 },
  { id:2, title:"Louder Than Bombs",   artist:"The Smiths",        src:"{{site.baseurl}}/album_sort/albums/ltb_thesmiths.png", color:"#E8821A", correctPos:2 },
  { id:3, title:"The Downward Spiral", artist:"NIN",               src:"{{site.baseurl}}/album_sort/albums/tds_nin.png", color:"#D4C200", correctPos:3 },
  { id:4, title:"Life Is Killing Me",  artist:"Type O Negative",   src:"{{site.baseurl}}/album_sort/albums/likm_typeoneg.png", color:"#2E7D32", correctPos:4 },
  { id:5, title:"Weezer (Blue Album)", artist:"Weezer",            src:"{{site.baseurl}}/album_sort/albums/weezer_blue.png", color:"#1565C0", correctPos:5 },
  { id:6, title:"Mellon Collie",       artist:"Smashing Pumpkins", src:"{{site.baseurl}}/album_sort/albums/melloncollie_tsp.png", color:"#0D3B6E", correctPos:6 },
  { id:7, title:"Godlike Snake",       artist:"Ufomammut",         src:"{{site.baseurl}}/album_sort/albums/glsnake_ufomammut.png", color:"#6A1B9A", correctPos:7 },
  { id:8, title:"So Tonight...",       artist:"Mazzy Star",        src:"{{site.baseurl}}/album_sort/albums/sttims_mazzy.png", color:"#3E0066", correctPos:8 },
  { id:9, title:"Pottymouth",          artist:"Bratmobile",        src:"{{site.baseurl}}/album_sort/albums/pottymouth_bratmobile.png", color:"#C2185B", correctPos:9 },
];

// ── State ─────────────────────────────────────────────────────────────────────
let currentOrder = [];
let dragState = null;

// ── Procedure: Fisher-Yates shuffle ──────────────────────────────────────────
function shuffleArray(arr) {
  const a = [...arr];
  for (let i = a.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]];
  }
  return a;
}

// ── Procedure: inner HTML for an album cover ──────────────────────────────────
function coverHTML(album) {
  if (album.src) return `<img src="${album.src}" alt="${album.title} by ${album.artist}">`;
  return `<div class="fallback" style="background:${album.color};"></div>`;
}

// ── Procedure: render list, optionally with FLIP animation ───────────────────
function renderList(animate) {
  const list = document.getElementById('rank-list');

  // FLIP – record bounding boxes before re-render
  const beforeRects = {};
  if (animate) {
    list.querySelectorAll('.rank-item').forEach(el => {
      beforeRects[el.dataset.id] = el.getBoundingClientRect();
    });
  }

  // Rebuild DOM
  list.innerHTML = '';
  for (let i = 0; i < currentOrder.length; i++) {
    const album = currentOrder[i];
    const item = document.createElement('div');
    item.className = 'rank-item no-transition';
    item.dataset.id = album.id;
    item.dataset.index = i;
    item.innerHTML = `<div class="album-cover">${coverHTML(album)}</div><span class="rank-num">${i + 1}</span>`;
    attachPointerListeners(item);
    list.appendChild(item);
  }

  // FLIP – play slide from old positions to new
  if (animate) {
    list.querySelectorAll('.rank-item').forEach(el => {
      const before = beforeRects[el.dataset.id];
      if (!before) return;
      const after = el.getBoundingClientRect();
      const dx = before.left - after.left;
      if (Math.abs(dx) < 1) return;
      // Jump to old position (no transition yet)
      el.style.transform = `translateX(${dx}px)`;
      // Force reflow so the browser registers the starting position
      el.getBoundingClientRect();
      // Enable transition and animate to natural (0) position
      el.classList.remove('no-transition');
      el.style.transform = '';
    });
  } else {
    requestAnimationFrame(() => {
      list.querySelectorAll('.rank-item').forEach(el => el.classList.remove('no-transition'));
    });
  }
}

// ── Procedure: attach pointer listeners to an item ────────────────────────────
function attachPointerListeners(item) {
  item.addEventListener('pointerdown', onPointerDown);
}

// ── Drag: start ───────────────────────────────────────────────────────────────
function onPointerDown(e) {
  if (e.button !== 0 && e.pointerType === 'mouse') return;
  e.preventDefault();

  const item  = e.currentTarget;
  const index = parseInt(item.dataset.index);
  const rect  = item.getBoundingClientRect();
  const coverRect = item.querySelector('.album-cover').getBoundingClientRect();

  dragState = {
    srcIndex: index,
    offsetX: e.clientX - rect.left,
    offsetY: e.clientY - rect.top,
  };

  // Ghost the original
  item.classList.add('ghost', 'no-transition');

  // Set up floating clone
  const clone = document.getElementById('drag-clone');
  const cloneCover = clone.querySelector('.album-cover');
  cloneCover.innerHTML = coverHTML(currentOrder[index]);
  cloneCover.style.width  = coverRect.width  + 'px';
  cloneCover.style.height = coverRect.height + 'px';
  clone.style.width   = rect.width + 'px';
  clone.style.display = 'flex';
  positionClone(e.clientX, e.clientY);

  document.addEventListener('pointermove',   onPointerMove);
  document.addEventListener('pointerup',     onPointerUp);
  document.addEventListener('pointercancel', onPointerUp);
}

function positionClone(cx, cy) {
  const clone = document.getElementById('drag-clone');
  clone.style.left = (cx - dragState.offsetX) + 'px';
  clone.style.top  = (cy - dragState.offsetY) + 'px';
}

// ── Drag: move ────────────────────────────────────────────────────────────────
function onPointerMove(e) {
  if (!dragState) return;
  positionClone(e.clientX, e.clientY);
  showIndicator(getInsertIndex(e.clientX));
}

// ── Procedure: determine insert index from cursor X ──────────────────────────
function getInsertIndex(cursorX) {
  const items = document.querySelectorAll('.rank-item');
  // Iteration: scan each item to find where cursor sits
  for (let i = 0; i < items.length; i++) {
    const rect = items[i].getBoundingClientRect();
    if (cursorX < rect.left + rect.width / 2) return i;
  }
  return items.length;
}

// ── Procedure: show the white insert line ────────────────────────────────────
function showIndicator(insertIndex) {
  const indicator = document.getElementById('insert-indicator');
  const items = document.querySelectorAll('.rank-item');
  if (!items.length) { indicator.classList.remove('visible'); return; }

  let x;
  if (insertIndex === 0) {
    x = items[0].getBoundingClientRect().left - 4;
  } else if (insertIndex >= items.length) {
    x = items[items.length - 1].getBoundingClientRect().right + 2;
  } else {
    x = items[insertIndex].getBoundingClientRect().left - 4;
  }

  const ref = items[Math.min(insertIndex, items.length - 1)].getBoundingClientRect();
  const h   = ref.height * 0.82;

  indicator.style.left   = x + 'px';
  indicator.style.top    = (ref.top + (ref.height - h) / 2) + 'px';
  indicator.style.height = h + 'px';
  indicator.classList.add('visible');
}

// ── Drag: end ─────────────────────────────────────────────────────────────────
function onPointerUp(e) {
  if (!dragState) return;

  document.removeEventListener('pointermove',   onPointerMove);
  document.removeEventListener('pointerup',     onPointerUp);
  document.removeEventListener('pointercancel', onPointerUp);

  const insertAt  = getInsertIndex(e.clientX);
  const { srcIndex } = dragState;

  document.getElementById('drag-clone').style.display = 'none';
  document.getElementById('insert-indicator').classList.remove('visible');
  dragState = null;

  // Selection: did the position actually change?
  if (insertAt !== srcIndex && insertAt !== srcIndex + 1) {
    const moved = currentOrder.splice(srcIndex, 1)[0];
    const finalInsert = insertAt > srcIndex ? insertAt - 1 : insertAt;
    currentOrder.splice(finalInsert, 0, moved);
    clearFeedback();
    renderList(true); // animate the slide
  } else {
    // No change — just remove ghost
    document.querySelectorAll('.rank-item').forEach(el => {
      el.classList.remove('ghost', 'no-transition');
    });
  }
}

// ── Procedure: check order against correct positions ─────────────────────────
function checkOrder() {
  const items = document.querySelectorAll('.rank-item');
  let correctCount = 0;

  // Iteration: evaluate every slot
  for (let i = 0; i < currentOrder.length; i++) {
    items[i].classList.remove('correct-item', 'wrong-item');
    // Selection: correct position?
    if (currentOrder[i].correctPos === i) {
      items[i].classList.add('correct-item');
      correctCount++;
    } else {
      items[i].classList.add('wrong-item');
    }
  }

  const result = document.getElementById('result');
  // Selection: full marks vs partial
  if (correctCount === 10) {
    result.textContent = 'Perfect rainbow order!';
    result.className = 'correct';
  } else {
    result.textContent = `${correctCount} / 10 correct`;
    result.className = 'wrong';
  }
}

function clearFeedback() {
  const result = document.getElementById('result');
  result.textContent = '';
  result.className = '';
  document.querySelectorAll('.rank-item').forEach(el => {
    el.classList.remove('correct-item', 'wrong-item');
  });
}

// ── Procedure call: initialise ────────────────────────────────────────────────
function resetGame() {
  currentOrder = shuffleArray(ALBUMS);
  clearFeedback();
  renderList(false);
}

resetGame();
</script>
</body>
</html>