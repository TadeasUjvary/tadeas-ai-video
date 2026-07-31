# Tadeáš Ujváry — AI video & reklama

Osobní landing page prezentující tvorbu **komerčních reklam a videí pomocí AI**.
Celá stránka stojí na ukázkách „před → po": velký výsledek, v jeho rohu původní
packshot, který přišel od klienta.

> **Stack:** čisté HTML + CSS + vanilla JS. Žádný build, žádné závislosti. Jeden soubor `index.html`.

---

## ✨ Co stránka umí

- **Hero koláž** — tři výstupy (vizuál, spot 9:16, vizuál) přeložené přes sebe, s jednou
  bílou destičkou „VSTUP", která ukazuje, z čeho se vycházelo.
- **Sekce „Před → po"** — sedm ukázek v masonry mřížce (CSS `columns`). Každá karta =
  velké médium s výstupem + vsazený náhled vstupní fotky vlevo dole (na hover se zvětší).
- **Líné video** — všech osm videí má `preload="none"` + poster; zdroj se načte a spustí
  teprve když karta doscrolluje do viewportu, a při odscrollování se pauzuje.
- **Služby, proces, statistiky** v editorial layoutu.
- **Kontakt** — e-mail, telefon a formulář (odešle se přes `mailto:`).
- Plně **responzivní**, podpora `prefers-reduced-motion`, progress bar, scroll-reveal animace.
  Reveal se aktivuje až po `html.js`, takže při chybě JS se obsah pořád zobrazí.

---

## 🎨 Design

| | |
|---|---|
| Akcentová zelená | `#2F9550` |
| Ink / text | `#0c1410` |
| Papír / pozadí | `#f6f7f4` |
| Fonty | Inter (UI/text) + Instrument Serif (italic akcenty) |

---

## 📁 Struktura

```
tadeas-web/
├── index.html              # celá stránka (HTML + CSS + JS)
├── work/                   # ukázky před → po
│   ├── protein-in.jpg      │ protein-out.jpg        (fotka → vizuál)
│   ├── monaco-in.jpg       │ monaco-out.jpg         (fotka → vizuál)
│   ├── lampa-in.jpg        │ lampa-out.mp4          (fotka → video)
│   ├── duolife-in.jpg      │ duolife-out.mp4        (packshot → animovaný spot)
│   ├── vitestin-in.jpg     │ vitestin-out.mp4       (fotka → video)
│   └── *-poster.jpg        # postery k videím
├── video/                  # starší ukázky
│   ├── airpods-photo.jpg   │ airpods.mp4 + poster.jpg
│   ├── rituals-photo.jpg   │ rituals.mp4 + rituals-poster.jpg
│   └── ugc-ref.mp4 + ugc-poster.jpg
└── README.md
```

Vstupní packshoty se ukládají jako JPEG se **zploštělou bílou** — v mřížce sedí na bílé
destičce, takže průhlednost není potřeba a soubor je řádově menší.

---

## 🚀 Spuštění lokálně

```bash
python3 -m http.server 8785
```

→ http://localhost:8785 (otevření přes `file://` funguje taky)

## ☁️ Nasazení

Statický web → Vercel projekt `ujvary-studio`, bez buildu.

---

## 🔧 Jak přidat další ukázku

Potřebuješ dvojici: vstupní packshot + výstup (obrázek nebo video).

```bash
# vstupní packshot: zploštit průhlednost na bílou, zmenšit
ffmpeg -i pred.png -vf "scale=-2:1100:force_original_aspect_ratio=decrease,\
format=rgba,split[a][b];[a]drawbox=c=white@1:t=fill[bg];[bg][b]overlay=format=auto,\
format=yuv420p" -q:v 4 work/nazev-in.jpg

# výstupní vizuál
ffmpeg -i po.png -vf "scale=1500:-2" -q:v 4 work/nazev-out.jpg

# výstupní video: bez zvuku, faststart, poster z 1. sekundy
ffmpeg -i po.mp4 -an -c:v libx264 -preset slow -crf 28 -vf "scale=980:-2" \
  -pix_fmt yuv420p -movflags +faststart -profile:v main work/nazev-out.mp4
ffmpeg -ss 1 -i work/nazev-out.mp4 -frames:v 1 -q:v 5 work/nazev-poster.jpg
```

Pak zkopíruj v `index.html` blok `<article class="case">`, přepiš cesty, popisek a
`aspect-ratio` na `.case-media` (poměr **výstupního** média).

> Má-li zdroj bílé pruhy (letterbox), ořízni je před škálováním: `crop=Š:V:0:Y`.

---

## 📬 Kontakt

- **E-mail:** taujvyk@gmail.com
- **Telefon:** +420 723 065 427

---

© Tadeáš Ujváry — AI video & reklama
