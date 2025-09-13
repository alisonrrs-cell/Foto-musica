# Foto-musica[landing_photo_music.html](https://github.com/user-attachments/files/22311270/landing_photo_music.html)
<!--
landing-photo-music.html
Single-file landing page: replace `photo.jpg` and `audio.mp3` with your files.
Host on GitHub Pages / Netlify / Vercel by uploading this file + the media files.
-->

<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Foto + Música</title>
  <style>
    :root{--bg:#0f172a;--card:#0b1220;--accent:#7dd3fc}
    html,body{height:100%;margin:0;font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,'Helvetica Neue',Arial}
    body{display:flex;align-items:center;justify-content:center;background:linear-gradient(180deg,#071023 0%, #07183a 100%);color:#e6eef8}
    .card{width:min(880px,96%);max-width:920px;border-radius:16px;box-shadow:0 8px 30px rgba(2,6,23,0.6);overflow:hidden;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));}
    .media{display:grid;grid-template-columns:1fr;gap:0}
    @media(min-width:720px){.media{grid-template-columns:1fr 420px}}
    .cover{position:relative;min-height:320px;background:#0a0f1a;display:flex;align-items:center;justify-content:center}
    .cover img{width:100%;height:100%;object-fit:cover;display:block}
    .info{padding:20px}
    h1{margin:0 0 8px 0;font-size:20px}
    p{margin:0 0 14px 0;color:#a7c0d9}
    .player{display:flex;gap:12px;align-items:center}
    .playbtn{width:64px;height:64px;border-radius:12px;background:linear-gradient(180deg,var(--accent),#4fb7e0);display:grid;place-items:center;color:#012;cursor:pointer;box-shadow:0 6px 18px rgba(34,197,94,0.08);border:0}
    .playbtn svg{width:28px;height:28px}
    .progress{flex:1}
    .time{font-size:13px;color:#c7dbea}
    .controls-row{display:flex;gap:10px;align-items:center;margin-top:12px}
    .download{padding:10px 12px;border-radius:10px;background:transparent;border:1px solid rgba(255,255,255,0.06);cursor:pointer}
    footer{font-size:12px;color:#87a9c9;padding:12px;text-align:center;background:transparent}
    /* big cover play overlay on mobile */
    .overlayPlay{position:absolute;inset:0;display:flex;align-items:center;justify-content:center;pointer-events:none}
    .overlayPlay button{pointer-events:auto}
  </style>
</head>
<body>
  <div class="card">
    <div class="media">
      <div class="cover">
        <!-- Replace 'photo.jpg' with your image file in same folder -->
        <img id="coverImg" src="photo.jpg" alt="Foto" onerror="this.style.objectFit='contain';this.style.background='#071026'" />
        <div class="overlayPlay">
          <button id="overlayPlayBtn" class="playbtn" aria-label="Play/Pause" title="Tocar / Pausar">
            <svg id="overlayIcon" viewBox="0 0 24 24" fill="currentColor" xmlns="http://www.w3.org/2000/svg"><path d="M8 5v14l11-7z"/></svg>
          </button>
        </div>
      </div>

      <div class="info">
        <h1>Nome da Pessoa / Título</h1>
        <p>Uma mensagem curta ou descrição. Substitua por algo pessoal. A foto e a música são carregadas abaixo.</p>

        <div class="player">
          <button id="playBtn" class="playbtn" aria-label="Play/Pause">
            <svg id="playIcon" viewBox="0 0 24 24" fill="currentColor"><path d="M8 5v14l11-7z"/></svg>
          </button>

          <div class="progress">
            <audio id="audio" preload="metadata">
              <!-- Replace 'audio.mp3' with your audio file in same folder -->
              <source src="audio.mp3" type="audio/mpeg" />
              Seu navegador não suporta reprodução de áudio.
            </audio>
            <input id="seek" type="range" min="0" max="100" value="0" style="width:100%" />
            <div style="display:flex;justify-content:space-between;margin-top:6px">
              <span class="time" id="current">0:00</span>
              <span class="time" id="duration">0:00</span>
            </div>
          </div>
        </div>

        <div class="controls-row">
          <a id="downloadLink" class="download" href="audio.mp3" download>Baixar áudio</a>
          <button class="download" id="changeFiles" onclick="alert('Para trocar a foto/áudio, substitua os arquivos photo.jpg e audio.mp3 no servidor / pasta do site.')">Como trocar arquivos</button>
        </div>

        <p style="margin-top:14px;font-size:13px;color:#9fc1df"><strong>Dica:</strong> se for hospedar no GitHub Pages, envie este arquivo e os dois arquivos de mídia para o repositório (mesma pasta). No Netlify basta arrastar a pasta com esses arquivos.</p>
      </div>
    </div>
    <footer>Abra a página e toque para escutar. QR code apontará para esta URL.</footer>
  </div>

  <script>
    const audio = document.getElementById('audio');
    const playBtn = document.getElementById('playBtn');
    const overlayPlayBtn = document.getElementById('overlayPlayBtn');
    const playIcon = document.getElementById('playIcon');
    const overlayIcon = document.getElementById('overlayIcon');
    const seek = document.getElementById('seek');
    const current = document.getElementById('current');
    const duration = document.getElementById('duration');
    const downloadLink = document.getElementById('downloadLink');

    function formatTime(s){
      if (isNaN(s)) return '0:00';
      const m = Math.floor(s/60); const sec = Math.floor(s%60).toString().padStart(2,'0');
      return `${m}:${sec}`;
    }

    audio.addEventListener('loadedmetadata', ()=>{
      duration.textContent = formatTime(audio.duration);
      seek.max = Math.floor(audio.duration);
    });
    audio.addEventListener('timeupdate', ()=>{
      seek.value = Math.floor(audio.currentTime);
      current.textContent = formatTime(audio.currentTime);
    });

    function setPlaying(playing){
      const svgPlay = '<path d="M8 5v14l11-7z"/>';
      const svgPause = '<path d="M6 19h4V5H6v14zM14 5v14h4V5h-4z"/>';
      playIcon.innerHTML = playing ? svgPause : svgPlay;
      overlayIcon.innerHTML = playing ? svgPause : svgPlay;
    }

    playBtn.addEventListener('click', ()=>{
      if (audio.paused) { audio.play(); setPlaying(true); } else { audio.pause(); setPlaying(false); }
    });
    overlayPlayBtn.addEventListener('click', ()=>{ playBtn.click(); });

    seek.addEventListener('input', ()=>{ audio.currentTime = seek.value; });

    audio.addEventListener('ended', ()=>{ setPlaying(false); });

    // Prevent autoplay blocking confusion: show initial state
    setPlaying(false);
  </script>
</body>
</html>
![56d7a3b1-bee5-40bc-89be-20c1b0478a6d](https://github.com/user-attachments/assets/5fb1d9f4-61e9-4f36-8b87-b69174c4b323)
https://open.spotify.com/intl-pt/track/4Qbhr2meaJxyUp9MkIJTs3?si=91409e7fc9964a6f
