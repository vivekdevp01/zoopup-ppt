# ZoopUp Omaxe – Live 3D + VR

Ye ZoopUp Omaxe game zone ka live 3D model hai. Isme bowling, laser tag, bumper cars, monster truck, arcade players, party room aur escalator sab chalte rehte hain. VR headset (Pico G2 4K, Pico 4, Quest) mein **Enter VR** se andar ghoom sakte ho.

Ye ek **static website** hai: koi build step nahi, koi server code nahi, koi CDN nahi. Page jo bhi file load karta hai, wo sab isi folder mein hai.

## Folder structure

```
zoopup-omaxe-vr/
├── index.html                  ← page (HTML + CSS)
├── vercel.json                 ← Vercel headers: cache + security
├── README.md
├── THIRD_PARTY_NOTICES.md      ← libraries aur fonts ke licence
├── .gitignore
├── assets/
│   ├── js/
│   │   ├── app.js              ← poora app ek file mein (three.js, VR, people, sab bundled)
│   │   └── app.js.LEGAL.txt    ← bundled libraries ke licence comments
│   ├── css/fonts.css           ← Cinzel + Poppins (self-hosted)
│   ├── fonts/                  ← .woff2 fonts + OFL licence
│   └── img/zoopup-logo.png
└── data/                       ← 3D model ka data (binary)
    ├── scene.json              ← materials, lights, views, files ki list
    ├── geo0.bin geo1.bin geo2.bin   ← geometry (meshopt + gzip)
    ├── tex0.bin tex1.bin       ← textures (WebP pack)
    ├── texhq0.bin              ← HD textures (computer par "High quality" ke liye)
    ├── ptex0.bin               ← logon ke textures
    ├── vr0.bin                 ← VR ke liye halki detail lists
    └── people/
        ├── people.json         ← avatars + animations ki list
        └── people.bin          ← avatars ka data
```

Sabse badi file ~9 MB hai, isliye GitHub ki browser upload limit (25 MiB per file) mein sab aa jaata hai.

## Deploy: GitHub → Vercel

### 1. GitHub par daalo

**Tareeqa A – browser se (sabse aasaan)**
1. GitHub par **New repository** banao, jaise `zoopup-omaxe-vr`.
2. Repo mein **Add file → Upload files** dabao.
3. Zip kholo. `zoopup-omaxe-vr` folder ke **andar ki sab cheezein** drag & drop karo:
   - `index.html`
   - `vercel.json`
   - `README.md`
   - `THIRD_PARTY_NOTICES.md`
   - `.gitignore`
   - `assets/`
   - `data/`
4. **Commit changes** dabao.

> Dhyan do: `index.html` repo ke root mein hona chahiye. `zoopup-omaxe-vr/index.html` jaise subfolder mein mat daalo. Agar subfolder mein daal diya, to Vercel mein **Root Directory** = `zoopup-omaxe-vr` set kar dena.
>
> `.gitignore` jaisi dot-files kuch computers par hidden hoti hain. Na dikhe to chhod sakte ho. Site uske bina bhi chalegi.

**Tareeqa B – git command line**
```bash
cd zoopup-omaxe-vr
git init
git add .
git commit -m "ZoopUp Omaxe live 3D + VR"
git branch -M main
git remote add origin https://github.com/<aapka-username>/zoopup-omaxe-vr.git
git push -u origin main
```

### 2. Vercel par host karo
1. [vercel.com](https://vercel.com) par GitHub se login karo.
2. **Add New → Project** chuno, phir apna repo **Import** karo.
3. Settings:
   - **Framework Preset:** `Other`
   - **Build Command:** khaali chhodo (override band)
   - **Output Directory:** khaali / default chhodo
   - **Root Directory:** `./` (agar index.html repo root mein hai)
4. **Deploy** dabao. ~1 minute mein `https://<project>.vercel.app` link mil jaayega.

Iske baad jab bhi GitHub par nayi files push karoge, Vercel apne aap naya version deploy kar dega.

## Headset mein VR
1. Headset ke browser mein `https://<project>.vercel.app` kholo. Vercel ka link HTTPS hota hai, aur VR ke liye HTTPS zaroori hai.
2. Model load hone do. Load ke baad 10–20 second ruko, kyunki VR ki halki copy peeche ban rahi hoti hai.
3. **VR quality** chuno:
   - **Smooth:** sabse steady frames
   - **HD:** default, Pico G2 ke liye best
   - **Ultra HD:** naye headsets ke liye
4. **Enter VR** dabao.

**Controls:**
- **Trigger daba ke chhodo:** teleport
- **Touchpad upar / neeche:** chalo
- **Touchpad left / right:** 30° mudo
- **Touchpad beech mein click:** agla zone

## Local test (optional)
Ye version alag-alag files load karta hai, isliye `index.html` ko double-click (file://) se kholne par nahi chalega. Ek chhota local server chalao:
```bash
cd zoopup-omaxe-vr
python3 -m http.server 8080
# browser mein: http://localhost:8080
```
Bina server ke chalana ho to alag single-file version use karo: `ZoopUp_Omaxe_Render.html`.

## Caching (update karte waqt)
`index.html` har baar fresh aata hai. `assets/` aur `data/` ke link mein ek version (`?v=…`) laga hai, isliye browser unko 1 saal cache karta hai.

Model update karna ho to ye teeno cheezein ek saath replace karke push karo:
- `index.html`
- `assets/`
- `data/`

Version badal jaayega aur sabko naya model milega.

## Zaroori baatein
- **Download size:** pehli baar kholne par lagbhag 29 MB data aata hai:
  - computer par HD textures ke saath ~35 MB
  - headset par VR data milake ~30 MB
  
  Doosri baar browser cache se khulta hai.
- **Vercel plan:**
  - Hobby (free) plan sirf **non-commercial / personal** use ke liye hai, aur usme 100 GB data transfer per month milta hai.
  - ZoopUp ke business use ke liye **Pro plan** lena chahiye. Dekho [Vercel Hobby plan](https://vercel.com/docs/plans/hobby).
- **Public link:** Jiske paas link hoga, wo model dekh sakta hai. Sirf apni team ko dikhana ho to Vercel ki **Deployment Protection** use kar sakte ho.
- **Licences:** Ye libraries aur cheezein use hui hain, inke licence follow karne zaroori hain. Details `THIRD_PARTY_NOTICES.md` mein hain:
  - three.js, postprocessing, N8AO, meshoptimizer, webxr-polyfill
  - Microsoft Rocketbox avatars
  - Cinzel aur Poppins fonts
