<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Ayushi Mishra — Pinterest Gallery</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
  <style>
    :root{
      --accent:#FF6B6B; /* coral */
      --accent-2:#6BCB77; /* green */
      --muted:#64748B;
      --bg:#ffffff;
      --text:#0f1724;
      --card:#f8fafc;
      --radius:14px;
      --shadow: 0 6px 20px rgba(15,23,36,0.08);
    }
    *{box-sizing:border-box}
    body{font-family:'Poppins',system-ui,Segoe UI,Roboto,Helvetica,Arial,sans-serif;background:var(--bg);color:var(--text);margin:0;line-height:1.4}
    header{display:flex;align-items:center;justify-content:space-between;padding:24px 28px;border-bottom:1px solid #eef2f7}
    .brand{display:flex;gap:16px;align-items:center}
    .logo{width:52px;height:52px;border-radius:12px;background:linear-gradient(135deg,var(--accent),var(--accent-2));display:flex;align-items:center;justify-content:center;color:white;font-weight:700;font-size:18px;box-shadow:var(--shadow)}
    h1{margin:0;font-size:18px}
    p.lead{margin:0;color:var(--muted);font-size:13px}
    .controls{display:flex;gap:12px;align-items:center}
    .btn{background:linear-gradient(90deg,var(--accent),#FF8A7B);border:none;color:white;padding:10px 14px;border-radius:10px;cursor:pointer;font-weight:600;box-shadow:var(--shadow)}
    .btn.ghost{background:transparent;color:var(--accent);border:1px solid rgba(255,107,107,0.12)}
    main{padding:28px}

    .panel{display:grid;grid-template-columns:1fr 380px;gap:20px}
    @media (max-width:980px){.panel{grid-template-columns:1fr}}

    /* left: gallery */
    .gallery-wrap{background:var(--card);padding:18px;border-radius:16px;box-shadow:var(--shadow)}
    .toolbar{display:flex;align-items:center;justify-content:space-between;margin-bottom:12px}
    .search{flex:1;margin-right:12px}
    .search input{width:100%;padding:10px 12px;border-radius:10px;border:1px solid #e6eef6}

    .grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(190px,1fr));gap:12px}
    .card{background:white;border-radius:12px;overflow:hidden;position:relative;border:1px solid #f1f5f9}
    .card img{width:100%;height:170px;object-fit:cover;display:block}
    .card .meta{padding:10px}
    .card .meta small{color:var(--muted);display:block}
    .card .actions{position:absolute;right:10px;top:10px;display:flex;gap:8px}
    .icon-btn{background:rgba(255,255,255,0.9);border-radius:8px;padding:6px;cursor:pointer;border:1px solid rgba(15,23,36,0.04)}

    /* right: uploader */
    .uploader{background:linear-gradient(180deg,#ffffff,#fbfbfd);border-radius:16px;padding:16px;border:1px solid #f1f5f9}
    .uploader h3{margin:0 0 8px 0}
    .drop{border:2px dashed #e6eef6;border-radius:12px;padding:18px;text-align:center;color:var(--muted);cursor:pointer;background:linear-gradient(180deg,rgba(107,203,119,0.03),transparent)}
    .drop.dragover{border-color:var(--accent);box-shadow:0 6px 30px rgba(255,107,107,0.06)}
    .small{font-size:13px;color:var(--muted)}
    .inputs{display:flex;flex-direction:column;gap:10px;margin-top:10px}
    .inputs input[type="text"]{padding:10px;border-radius:10px;border:1px solid #e6eef6}
    .inputs input[type="file"]{padding:8px}
    .saved-list{margin-top:14px}
    .saved-item{display:flex;align-items:center;gap:10px;padding:8px;border-radius:10px;background:white;border:1px solid #f1f5f9}
    .saved-item img{width:44px;height:44px;object-fit:cover;border-radius:8px}

    footer{padding:18px;text-align:center;color:var(--muted);font-size:13px}

    /* modal */
    .modal{position:fixed;inset:0;background:rgba(15,23,36,0.6);display:flex;align-items:center;justify-content:center;z-index:60;}
    .modal .inner{max-width:920px;width:94%;background:white;border-radius:12px;overflow:hidden}
    .modal img{width:100%;height:auto;display:block}

    a.pinterest-link{color:var(--accent);font-weight:600;text-decoration:none}
  </style>
</head>
<body>
  <header>
    <div class="brand">
      <div class="logo">AM</div>
      <div>
        <h1>Ayushi Mishra</h1>
        <p class="lead">Pinterest collection viewer &amp; uploader — <a class="pinterest-link" href="https://www.pinterest.com/_67_Ayushi" target="_blank">@_67_Ayushi</a></p>
      </div>
    </div>
    <div class="controls">
      <a class="btn ghost" href="https://www.pinterest.com/_67_Ayushi" target="_blank">Open Pinterest</a>
      <button class="btn" id="clearAll">Clear All</button>
    </div>
  </header>

  <!-- Admin Login Button -->
  <div style="position:fixed;bottom:20px;right:20px;z-index:9999">
    <button class="btn" id="adminLoginBtn">Admin Login</button>
  </div>

  <main>
    <div class="panel">
      <div class="gallery-wrap">
        <div class="toolbar">
          <div class="search"><input id="search" placeholder="Search by filename or note..." /></div>
          <div style="display:flex;gap:8px">
            <button class="btn" id="addUrlBtn">Add by URL</button>
            <button class="btn ghost" id="filterBtn">Filter</button>
          </div>
        </div>

        <div id="grid" class="grid" aria-live="polite">
          <!-- image cards injected here -->
        </div>

        <div style="margin-top:12px;display:flex;justify-content:center">
          <small class="small">Images are stored locally in your browser (localStorage). Use "Export" to download backups.</small>
        </div>
      </div>

      <aside class="uploader" id="uploaderSection" style="display:none">
        <h3>Upload / Add (Admin Only)</h3>
        <div id="drop" class="drop">Drag & drop images here or click to select</div>
        <div class="inputs">
          <input id="fileInput" type="file" accept="image/*,video/*" multiple />
          <input id="urlInput" type="text" placeholder="Paste direct media URL" />
          <div style="display:flex;gap:8px">
            <button class="btn" id="addUrl">Add URL</button>
            <button class="btn ghost" id="exportBtn">Export JSON</button>
            <button class="btn ghost" id="importBtn">Import JSON</button>
          </div>
        </div>
        <div class="saved-list" id="savedList"></div>
        <hr style="margin:12px 0;border:none;border-top:1px solid #eef2f7" />
        <p class="small">Admin Mode: Only you can upload.</p>
      </aside>
    </div>
  </main>

  <footer>
    Made with ♥ for Ayushi Mishra • <a class="pinterest-link" href="https://www.pinterest.com/_67_Ayushi" target="_blank">Visit @_67_Ayushi on Pinterest</a>
  </footer>

  <!-- modal viewer -->
  <div id="modal" class="modal" style="display:none" aria-hidden="true">
    <div class="inner">
      <div style="display:flex;justify-content:space-between;padding:10px 12px;align-items:center">
        <div id="modalTitle" style="font-weight:600"></div>
        <div style="display:flex;gap:8px">
          <button id="downloadBtn" class="btn ghost">Download</button>
          <button id="closeModal" class="btn">Close</button>
        </div>
      </div>
      <div style="background:#000"><img id="modalImg" src="" alt="Preview" /></div>
    </div>
  </div>

  <input id="hiddenDownload" style="display:none" />

  <script>
    // Simple gallery app: stores images in localStorage as data-URLs or remote URLs.
    const STORAGE_KEY = 'ayushi_gallery_v1';
    let items = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]');

    const grid = document.getElementById('grid');
    const savedList = document.getElementById('savedList');
    const fileInput = document.getElementById('fileInput');
    const urlInput = document.getElementById('urlInput');
    const addUrlBtn = document.getElementById('addUrl');
    const addUrlQuick = document.getElementById('addUrlBtn');
    const drop = document.getElementById('drop');
    const modal = document.getElementById('modal');
    const modalImg = document.getElementById('modalImg');
    const modalTitle = document.getElementById('modalTitle');
    const closeModal = document.getElementById('closeModal');
    const downloadBtn = document.getElementById('downloadBtn');

    function save(){ localStorage.setItem(STORAGE_KEY, JSON.stringify(items)); render(); }

    function render(){
      // grid
      grid.innerHTML = '';
      if(items.length === 0){ grid.innerHTML = '<div style="padding:28px;text-align:center;color:var(--muted)">No images yet — add some from the right panel or paste Pinterest image URLs.</div>'; savedList.innerHTML=''; return; }
      items.forEach((it, idx)=>{
        const el = document.createElement('div'); el.className='card';
        el.innerHTML = `
          <div style="position:relative">
            <img src="${it.src}" alt="${it.name||'image'}" loading="lazy" />
            <div class="actions">
              <div class="icon-btn" title="Open" data-idx="${idx}">🔍</div>
              <div class="icon-btn" title="Delete" data-del="${idx}">🗑️</div>
            </div>
          </div>
          <div class="meta">
            <div style="display:flex;justify-content:space-between;align-items:center">
              <strong style="font-size:13px">${it.name || 'Untitled'}</strong>
              <small style="color:var(--muted);font-size:12px">${new Date(it.t||Date.now()).toLocaleDateString()}</small>
            </div>
            <small title="source">${it.src.length>60?it.src.substr(0,57)+'...':it.src}</small>
          </div>
        `;
        grid.appendChild(el);
      });

      // saved list
      savedList.innerHTML = items.map((it,idx)=>`<div class="saved-item"><img src="${it.src}" alt="thumb"/><div style="flex:1"><strong style="display:block;font-size:13px">${it.name||'Untitled'}</strong><small class="small">${it.src.length>60?it.src.substr(0,57)+'...':it.src}</small></div><div style="display:flex;flex-direction:column;gap:6px"><button class="btn" data-open="${idx}">Open</button><button class="btn ghost" data-delete="${idx}">Delete</button></div></div>`).join('')
    }

    // open modal
    grid.addEventListener('click', e=>{
      const openBtn = e.target.closest('[data-idx]');
      const delBtn = e.target.closest('[data-del]');
      if(openBtn){ const idx = +openBtn.dataset.idx; openViewer(idx); }
      if(delBtn){ const idx = +delBtn.dataset.del; if(confirm('Delete this item?')){ items.splice(idx,1); save(); }}
    });

    // saved list delegate
    savedList.addEventListener('click', e=>{
      const open = e.target.closest('[data-open]');
      const del = e.target.closest('[data-delete]');
      if(open){ openViewer(+open.dataset.open); }
      if(del){ if(confirm('Delete this item?')){ items.splice(+del.dataset.delete,1); save(); }}
    });

    function openViewer(idx){
      const it = items[idx]; if(!it) return;
      modalImg.src = it.src; modalTitle.textContent = it.name || 'Image preview';
      modal.style.display='flex'; modal.setAttribute('aria-hidden','false');
      downloadBtn.onclick = ()=>downloadData(it.src, it.name || 'image');
    }
    closeModal.addEventListener('click', ()=>{ modal.style.display='none'; modal.setAttribute('aria-hidden','true'); modalImg.src=''; });
    modal.addEventListener('click', e=>{ if(e.target===modal) { closeModal.click(); } });

    function downloadData(src, name){
      // if data URL -> download directly; if remote URL, attempt to fetch as blob (CORS may block)
      if(src.startsWith('data:')){
        const a = document.createElement('a'); a.href = src; a.download = name.replace(/\s+/g,'_') + '.png'; a.click();
        return;
      }
      // try to fetch
      fetch(src).then(r=>r.blob()).then(blob=>{
        const url = URL.createObjectURL(blob); const a=document.createElement('a'); a.href=url; a.download = name.replace(/\s+/g,'_'); a.click(); URL.revokeObjectURL(url);
      }).catch(()=>{ alert('Unable to download remote image due to CORS. Try saving from the source site.'); });
    }

    // file input
    fileInput.addEventListener('change', async(e)=>{
      const files = Array.from(e.target.files || []);
      for(const f of files){
        const data = await fileToDataURL(f);
        items.unshift({src:data,name:f.name,t:Date.now()});
      }
      save(); fileInput.value='';
    });

    // drag drop
    ['dragenter','dragover'].forEach(ev=>{ drop.addEventListener(ev,e=>{ e.preventDefault(); drop.classList.add('dragover'); }); });
    ['dragleave','drop'].forEach(ev=>{ drop.addEventListener(ev,e=>{ e.preventDefault(); drop.classList.remove('dragover'); }); });
    drop.addEventListener('drop', async(e)=>{
      const files = Array.from(e.dataTransfer.files || []).filter(f=>f.type.startsWith('image/'));
      for(const f of files){ const data = await fileToDataURL(f); items.unshift({src:data,name:f.name,t:Date.now()}); }
      save();
    });
    drop.addEventListener('click', ()=>fileInput.click());

    function fileToDataURL(f){ return new Promise((res,rej)=>{ const r=new FileReader(); r.onload=()=>res(r.result); r.onerror=rej; r.readAsDataURL(f); }); }

    // add by URL
    addUrlBtn.addEventListener('click', ()=>{
      const url = prompt('Paste image URL (direct image link):'); if(url) addImageUrl(url);
    });
    document.getElementById('addUrlBtn').addEventListener('click', ()=>{ /* duplicate kept for accessibility */ });
    document.getElementById('addUrl').addEventListener('click', ()=>{ const url=urlInput.value.trim(); if(!url) return alert('Paste an image URL first.'); addImageUrl(url); urlInput.value=''; });

    function addImageUrl(url){ const name = url.split('/').pop().split('?')[0] || 'remote-image'; items.unshift({src:url,name:name,t:Date.now()}); save(); }

    // export/import
    document.getElementById('exportBtn').addEventListener('click', ()=>{
      const data = JSON.stringify(items);
      const blob = new Blob([data],{type:'application/json'});
      const url = URL.createObjectURL(blob); const a=document.createElement('a'); a.href=url; a.download = 'ayushi_gallery_export.json'; a.click(); URL.revokeObjectURL(url);
    });
    document.getElementById('importBtn').addEventListener('click', ()=>{
      const f = document.createElement('input'); f.type='file'; f.accept='application/json'; f.onchange = async(e)=>{
        const file = e.target.files[0]; if(!file) return; const text = await file.text(); try{ const parsed = JSON.parse(text); if(Array.isArray(parsed)){ items = parsed.concat(items); save(); alert('Imported successfully.'); } else alert('Invalid import file.'); }catch(err){ alert('Error parsing JSON'); }
      }; f.click();
    });

    // clear all
    document.getElementById('clearAll').addEventListener('click', ()=>{ if(confirm('Clear all saved images?')){ items=[]; save(); }});

    // search
    document.getElementById('search').addEventListener('input', e=>{
      const q = e.target.value.trim().toLowerCase(); const cards = document.querySelectorAll('.card');
      cards.forEach((c,i)=>{
        const it = items[i]; const show = !q || (it.name && it.name.toLowerCase().includes(q)) || (it.src && it.src.toLowerCase().includes(q));
        c.style.display = show ? '' : 'none';
      });
    });

    // Admin login
    const uploaderSection = document.getElementById('uploaderSection');
    const adminBtn = document.getElementById('adminLoginBtn');

    adminBtn.onclick = () => {
      const pass = prompt('Enter admin password:');
      if(pass === 'ayushi123'){
        uploaderSection.style.display = 'block';
        alert('Admin mode enabled. You can now upload.');
      } else {
        alert('Incorrect password.');
      }
    };

    // initial render
    render();

    // friendly note in console
    console.log('Ayushi gallery loaded — store images locally and add Pinterest image links via URL.');
  </script>
</body>
</html>
