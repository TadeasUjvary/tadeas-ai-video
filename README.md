# Tadeáš Ujváry — AI video & reklama

Osobní landing page prezentující tvorbu **komerčních reklam a videí pomocí AI**.
Pohlcující hero se scroll-řízeným videem (AirPods), čistý light-minimal editorial styl
a reálné ukázky „z fotky → hotové video".

> **Stack:** čisté HTML + CSS + vanilla JS. Žádný build, žádné závislosti. Jeden soubor `index.html`.

---

## ✨ Co stránka umí

- **Scroll-driven hero** — video se „přehrává" podle scrollu. Technicky jde o **canvas
  image-sequence** (snímky z videa se vykreslují na `<canvas>` podle pozice scrollu).
  Je to spolehlivé napříč všemi prohlížeči i přes `file://` — na rozdíl od seekování
  `<video>.currentTime`, které v některých prohlížečích zlobí.
- **Textové panely** v hero se plynule prolínají podle progressu scrollu.
- **Sekce „Jak to funguje — na příkladech"** — dvě reálné before/after dvojice
  (AirPods a Rituals: *fotka → AI video*) + bonusové UGC video.
- **Služby, proces, statistiky** v editorial layoutu.
- **Kontakt** — e-mail, telefon a formulář (odešle se přes `mailto:`).
- Plně **responzivní**, podpora `prefers-reduced-motion`, progress bar, scroll-reveal animace.

---

## 🎨 Design

| | |
|---|---|
| Akcentová zelená | `#2F9550` (odpipetovaná přímo z videa) |
| Ink / text | `#0c1410` |
| Papír / pozadí | `#f6f7f4` |
| Fonty | Inter (UI/text) + Instrument Serif (italic akcenty) |

---

## 📁 Struktura

```
tadeas-web/
├── index.html              # celá stránka (HTML + CSS + JS)
├── video/
│   ├── frames/             # 121 snímků hero videa (f_001.jpg … f_121.jpg) – canvas scrub
│   ├── airpods.mp4         # hero video (all-intra) + výstup v ukázce
│   ├── airpods-photo.jpg   # vstupní fotka AirPods (before)
│   ├── rituals.mp4         # AI video šamponu Rituals (výstup)
│   ├── rituals-photo.jpg   # vstupní fotka Rituals (before)
│   ├── ugc-ref.mp4         # UGC referenční video
│   ├── poster.jpg          # OG/náhledový obrázek
│   └── *-poster.jpg        # postery k videím
└── README.md
```

---

## 🚀 Spuštění lokálně

Stačí jakýkoliv statický server (kvůli načítání snímků hero videa):

```bash
# Python
python3 -m http.server 8785
# → http://localhost:8785

# nebo npx
npx serve .
```

> Pozn.: otevření `index.html` napřímo (`file://`) funguje taky, ale přes server je to korektnější
> (caching, range requesty).

## ☁️ Nasazení

Statický web → ideální pro **Vercel / Netlify / GitHub Pages**.
Na Vercelu stačí `Import Project` z tohoto repa, bez nastavování (žádný build).

---

## 🔧 Jak přidat / vyměnit média

**Nová before/after dvojice** = produktová fotka (`*-photo.jpg`) + AI video (`*.mp4`).
Video pro web odlehči (bez zvuku, faststart):

```bash
ffmpeg -i zdroj.mp4 -an -c:v libx264 -crf 28 -preset slow \
  -movflags +faststart -pix_fmt yuv420p video/nove.mp4
```

**Regenerace snímků hero videa** (canvas scrub) — každý 2. snímek, 1280×720:

```bash
ffmpeg -i video/airpods.mp4 \
  -vf "select='not(mod(n\,2))',scale=1280:720" -fps_mode vfr -q:v 5 \
  video/frames/f_%03d.jpg
```

Pokud změníš počet snímků, uprav `FRAME_COUNT` v `index.html`.

---

## 📬 Kontakt

- **E-mail:** taujvyk@gmail.com
- **Telefon:** +420 723 065 427

---

© Tadeáš Ujváry — AI video & reklama
