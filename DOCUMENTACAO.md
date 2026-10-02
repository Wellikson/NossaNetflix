# 📄 Documentação — Página NETFLIX (Loveflix Theme)

Arquivo principal: `loveflix.html`
Assets: pasta `assets/` (imagens convertidas de HEIC → JPG)

---

## 1. Visão Geral

Página estática estilo "catálogo Netflix" para apresentar a história do casal como uma série de streaming personalizada. Single-file HTML com CSS e JS embutidos — não requer build nem dependências externas (CDNs, frameworks).

## 2. Assets (`assets/`)

| Arquivo | Uso |
|---|---|
| `hero.jpg` | Fundo do banner principal (crop 16:10, casal junto) |
| `carta.jpg` | Fundo da seção "Nossa Carta" |
| `ep1.jpg` … `ep4.jpg` | Thumbnails dos 4 episódios |
| `profile.jpg` | Avatar do navbar (crop quadrado 220px) |

Origem: fotos `IMG_*.HEIC` convertidas para JPG (máx. 1200–1600px, qualidade 82–85%). O pair `IMG_*.HEIC + IMG_*.MP4` representa Live Photos do iPhone.

## 3. Seções da Página

1. **Abertura estilo Netflix** (`#intro`)
   - Logo "NETFLIX" em vermelho com animação de zoom (0 → 3.2x) em ~2,6s
   - Som "ta-dum" via Web Audio API no primeiro clique do usuário (áudio automático é bloqueado por navegadores)
   - Após o zoom, o overlay some em fade (0,9s) e é removido do DOM

2. **Navbar fixa** (`header#navbar`)
   - Logo, menu âncora (Início, Episódios, Nossa Carta, Detalhes), avatar do casal
   - Ganha fundo escurecido ao rolar (`.scrolled`); menu é escondido abaixo de 760px

3. **Hero** (`section.hero#inicio`)
   - Foto de fundo com vignette em degradê (CSS gradients sobrepostos)
   - Badge "SÉRIE ORIGINAL", título, tags (match %, classificação, temporada, gêneros), sinopse
   - `▶ Assistir` → abre a seção da carta e toca o áudio tema
   - `ℹ Mais Informações` → scroll suave até a ficha técnica

4. **Carrossel de episódios** (`#episodios`)
   - Slider horizontal com 4 cards (`.episode-card`)
   - Hover: scale 1.08 + sombra/glow vermelho (desativado no mobile)
   - No mobile: `scroll-snap` para deslizar card a card

5. **Nossa Carta** (`#carta`, oculta por padrão)
   - Exibida após clique em "Assistir"; faz scroll até ela
   - Player de áudio tema (loop) — note que o `src` atual é de demonstração
   - Texto da carta com animação de revelação caractere a caractere

6. **Ficha técnica** (`#detalhes`)
   - Grid responsivo: 4 colunas → 2 → 1

7. **Rodapé** — créditos simples

## 4. Comportamentos JS

| Ação | Detalhe |
|---|---|
| `pointerdown` (1º) | Toca o som "ta-dum" (AudioContext), esconde o hint "Clique para iniciar" |
| `setTimeout` 2600ms | Finaliza abertura com fade-out |
| `scroll` | Alterna classe `.scrolled` na navbar |
| Render da carta | Cada caractere vira um `<span>` com `animation-delay` incremental (0,03s) |
| `#btnPlay` | Abre `#carta`, scroll suave, `audio.play()` (com `catch` para bloqueio de autoplay) |

## 5. Paletas & Tokens CSS

```css
--bg:#141414; --red:#E50914; --white:#FFFFFF; --muted:#AAAAAA; --glow:rgba(229,9,20,.4)
```

## 6. Responsividade

- **≤760px**: nav escondido, hero alinhado embaixo (`align-items:flex-end`, 92vh), botões empilhados em largura total, cards de episódio em ~68vw com scroll-snap, carta com padding reduzido, detalhes em 2 colunas
- **≤380px**: cards ~78vw, detalhes em coluna única

## 7. Pontos de customização

| O que trocar | Onde |
|---|---|
| Texto da carta | `<div class="letter-text" id="letterText">` |
| Trilha sonora | atributo `src` do `<audio id="themeAudio">` |
| Episódios | blocos `.episode-card` dentro de `.carousel` |
| Ficha técnica | `#detalhes .detail-grid` |
| Fotos | substituir arquivos em `assets/` (manter os mesmos nomes) |
| Cores da marca | variáveis em `:root` no topo do `<style>` |

## 8. Checklist de Go-Live

- [ ] Trocar o `src` do áudio de demonstração pela trilha real (Cloudinary/MP3)
- [ ] Testar autoplay: o clique em "Assistir" já contorna a política dos navegadores
- [ ] Validar em 320–430px de largura (Chrome DevTools)
- [ ] Otimizar fotos via Cloudinary (WebP/AVIF) para carregamento 4G
- [ ] SEO básico: `<meta description>`, `og:image` com a foto do casal
