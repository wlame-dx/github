<!doctype html>
<html lang="tr">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Bio — ? Wlâme</title>
  <style>
    :root{
      --bg:#0b0f14;
      --card:#0f1720;
      --muted:#9aa6b2;
      --accent:#6ee7b7;
      --accent-2:#7dd3fc;
      --radius:14px;
      font-family: Inter, ui-sans-serif, system-ui, -apple-system;
    }

    body{
      margin:0;
      background:var(--bg);
      color:#e6eef6;
      display:flex;
      flex-direction:column;
      align-items:center;
      padding:40px 20px;
    }

    .card{
      width:min(720px,95vw);
      background:rgba(255,255,255,0.03);
      border:1px solid rgba(255,255,255,0.05);
      border-radius:var(--radius);
      padding:26px;
      box-shadow:0 10px 25px rgba(0,0,0,0.4);
      backdrop-filter:blur(6px);
    }

    .top{
      display:flex;
      gap:18px;
      align-items:center;
    }

    .avatar{
      height:90px;
      width:90px;
      border-radius:12px;
      object-fit:cover;
      background:#222;
    }

    .name{
      font-size:22px;
      font-weight:700;
    }

    .role{
      font-size:13px;
      color:var(--accent);
      font-weight:600;
      margin-top:4px;
    }

    .desc{
      margin-top:10px;
      color:var(--muted);
      font-size:14px;
      line-height:1.4;
    }

    .links{
      margin-top:12px;
      display:flex;
      gap:10px;
      flex-wrap:wrap;
    }

    .link{
      text-decoration:none;
      padding:8px 12px;
      border-radius:10px;
      background:rgba(255,255,255,0.03);
      border:1px solid rgba(255,255,255,0.05);
      color:var(--accent-2);
      font-weight:600;
      font-size:13px;
      transition:0.15s;
    }
    .link:hover{ transform:translateY(-3px); }

    .copy-btn{
      margin-top:14px;
      padding:10px 14px;
      border-radius:10px;
      border:none;
      background: linear-gradient(90deg,var(--accent),var(--accent-2));
      color:#032024;
      font-weight:700;
      cursor:pointer;
      transition:0.15s;
    }
    .copy-btn:active{ transform:translateY(1px); }

    /* PROJELER */
    .projects{
      margin-top:30px;
      background:rgba(255,255,255,0.02);
      padding:20px;
      border-radius:14px;
      border:1px solid rgba(255,255,255,0.04);
    }

    .projects h2{
      margin:0 0 16px 0;
      font-size:18px;
    }

    .project{
      margin-bottom:12px;
      padding:12px;
      background:rgba(255,255,255,0.03);
      border-radius:12px;
      border:1px solid rgba(255,255,255,0.04);
      cursor:pointer;
      transition:0.15s;
    }

    .project:hover{
      transform:translateX(4px);
      background:rgba(255,255,255,0.05);
    }

    .project-title{
      font-size:15px;
      font-weight:700;
      margin-bottom:4px;
      color:var(--accent);
    }

    .project-desc{
      font-size:13px;
      color:var(--muted);
    }
  </style>
</head>
<body>

  <div class="card">
    <div class="top">
      <img src="profile.jpg" class="avatar" alt="Profil Fotoğrafı">

      <div>
        <div class="name" id="displayName">? Wlâme</div>
        <div class="role" id="role">Fullstack • Developer</div>
        <p class="desc" id="bioText">
          Yazılım, botlar ve yapay zeka ile uğraşıyorum. DM açıktır.
        </p>
      </div>
    </div>

    <div class="links" id="links"></div>

    <button class="copy-btn" id="copyBtn">Linkleri Kopyala</button>

    <!-- PROJELER -->
    <div class="projects" id="projects">
      <h2>Projeler</h2>

      <!-- BURADA SENİN MANUEL PROJELERİN KALIYOR -->
      <div class="project">
        <div class="project-title">Rax Mod V3</div>
        <div class="project-desc">Gelişmiş Discord bot altyapısı — level, moderasyon, api destekli.</div>
      </div>

      <div class="project">
        <div class="project-title">Rax Multi V1</div>
        <div class="project-desc">Gelişmiş moderasyon botu. API destekli.</div>
      </div>

      <!-- BURAYA GITHUB REPOLARI GELECEK -->
      <div id="githubRepos"></div>

    </div>
  </div>

<script>
  const githubUsername = "wlame-dx"; // >>> BURAYI DÜZENLE <<<

  // Kullanıcı linkleri
  const profile = {
    links: [
      {label: "Discord", url: "https://discord.com"},
      {label: "Instagram", url: "https://instagram.com/itsswlame"},
      {label: "GitHub", url: "https://github.com/" + githubUsername}
    ]
  };

  // Linkleri yükle
  const linksContainer = document.getElementById("links");
  profile.links.forEach(l=>{
    const a = document.createElement("a");
    a.className = "link";
    a.href = l.url;
    a.textContent = l.label;
    a.target = "_blank";
    linksContainer.appendChild(a);
  });

  // GitHub Repo API — otomatik projeleri çek
  async function loadRepos(){
    try{
      const res = await fetch(`https://api.github.com/users/${githubUsername}/repos`);
      const data = await res.json();

      const container = document.getElementById("githubRepos");

      data.forEach(repo=>{
        const div = document.createElement("div");
        div.className = "project";
        div.onclick = ()=>window.open(repo.html_url, "_blank");

        div.innerHTML = `
          <div class="project-title">${repo.name}</div>
          <div class="project-desc">${repo.description || "Açıklama yok"}</div>
        `;

        container.appendChild(div);
      });

    }catch(err){
      console.log("Repo yüklenemedi:", err);
    }
  }

  loadRepos();

  // Kopyalama
  document.getElementById("copyBtn").addEventListener("click", async ()=>{
    const text = profile.links.map(x=>`${x.label}: ${x.url}`).join("  |  ");
    try{
      await navigator.clipboard.writeText(text);
      document.getElementById("copyBtn").textContent = "Kopyalandı ✓";
      setTimeout(()=>document.getElementById("copyBtn").textContent = "Linkleri Kopyala", 1500);
    }catch(e){
      alert("Kopyalanamadı:\n" + text);
    }
  });
</script>

</body>
</html>
