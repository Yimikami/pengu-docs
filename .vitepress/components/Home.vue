<script setup lang="ts">
import { computed, ref } from "vue";
import { withBase } from "vitepress";

const menu = ref<HTMLDetailsElement>();
const selected = ref(0);
const effect = ref(0);
const showcase = ref(0);
const showcases = [
  {
    name: "Custom artwork",
    image: "/landing/community-theme.webp",
    alt: "Community League client theme with Spirit Blossom artwork extending behind transparent navigation and profile panels.",
    title: "A new background is only the beginning.",
    description:
      "Artwork across the client. Transparent panels. A completely different atmosphere.",
    credit: "Community showcase · Pengu Loader",
    source: "https://github.com/PenguLoader#showcases",
  },
  {
    name: "A different layout",
    image: "/landing/indie-theme.webp",
    alt: "Indie theme showing an expanded League lobby, compact navigation and a collapsed friends panel.",
    title: "More lobby. Less in the way.",
    description:
      "Indie theme makes room with compact navigation and a collapsible friends panel.",
    credit: "Indie theme · nomi-san",
    source: "https://github.com/nomi-san/indie-theme",
  },
];
const currentShowcase = computed(() => showcases[showcase.value]);
const tabs = ["Themes", "Plugins", "Build your own"];
const effects = [
  {
    name: "Acrylic",
    image: "/images/visual-acrylic.png",
    description: "Acrylic brings a soft, frosted finish to the client.",
  },
  {
    name: "Blur behind",
    image: "/images/visual-blurbehind.png",
    description: "Blur keeps the desktop visible behind the client.",
  },
  {
    name: "Transparent",
    image: "/images/visual-transparent.png",
    description: "Transparency lets the desktop become part of your client.",
  },
];
const currentEffect = computed(() => effects[effect.value]);
const descriptions = [
  "Change the look. Add new features. Make the client yours with community-made themes and plugins, or build your own.",
  "Small changes can make a familiar client feel more useful. Find community plugins, follow their installation guides, and choose what belongs in your setup.",
  "Know a little JavaScript or CSS? You already have a place to start. Bring your idea into the client with the tools you know.",
];

function moveTab(event: KeyboardEvent) {
  const keys = ["ArrowLeft", "ArrowRight", "Home", "End"];
  if (!keys.includes(event.key)) return;
  event.preventDefault();
  if (event.key === "Home") selected.value = 0;
  else if (event.key === "End") selected.value = tabs.length - 1;
  else
    selected.value =
      (selected.value + (event.key === "ArrowRight" ? 1 : -1) + tabs.length) %
      tabs.length;
  const buttons = (
    event.currentTarget as HTMLElement
  ).querySelectorAll<HTMLButtonElement>('[role="tab"]');
  buttons[selected.value]?.focus();
}

function closeMenu() {
  menu.value?.removeAttribute("open");
}
</script>

<template>
  <div class="pengu-home">
    <a class="skip-link" href="#main-content">Skip to content</a>
    <header class="masthead" @keydown.esc="closeMenu">
      <div class="page-shell masthead-inner">
        <a class="brand" :href="withBase('/')" aria-label="Pengu Loader home">
          <img :src="withBase('/icon.png')" width="38" height="38" alt="" />
          <span>Pengu Loader</span>
        </a>
        <nav class="desktop-nav" aria-label="Main navigation">
          <a href="#explore">Explore</a>
          <a href="#developers">Developers</a>
          <a href="https://github.com/PenguLoader/PenguLoader">GitHub</a>
        </nav>
        <a class="nav-download" :href="withBase('/download')">Download</a>
        <details ref="menu" class="mobile-menu">
          <summary>
            Menu
            <svg viewBox="0 0 24 24" aria-hidden="true">
              <path d="m6 9 6 6 6-6" />
            </svg>
          </summary>
          <nav aria-label="Mobile navigation" @click="closeMenu">
            <a href="#explore">Explore</a>
            <a href="#developers">Developers</a>
            <a :href="withBase('/guide/welcome')">Documentation</a>
            <a href="https://github.com/PenguLoader/PenguLoader">GitHub</a>
          </nav>
        </details>
      </div>
    </header>

    <main id="main-content" tabindex="-1">
      <section class="hero" aria-labelledby="hero-title">
        <div class="page-shell hero-stage">
          <div class="hero-copy">
            <h1 id="hero-title">
              <span>Your client.</span><span>Your rules.</span>
            </h1>
            <div class="hero-intro">
              <p>
                Themes and plugins for your<br class="desktop-break" />
                League of Legends client.
              </p>
              <a class="button button-chalk" :href="withBase('/download')">
                <svg viewBox="0 0 24 24" aria-hidden="true">
                  <path d="M12 3v13m-5-5 5 5 5-5M5 21h14" />
                </svg>
                Download Pengu
              </a>
              <a class="guide-link" :href="withBase('/guide/installation')"
                >Installation guide</a
              >
              <span class="platforms">Windows + macOS</span>
            </div>
          </div>
          <figure class="community-showcase">
            <fieldset class="showcase-options">
              <legend>Made with Pengu</legend>
              <label v-for="(item, index) in showcases" :key="item.name">
                <input
                  v-model="showcase"
                  type="radio"
                  name="community-showcase"
                  :value="index"
                />
                <span>{{ item.name }}</span>
              </label>
            </fieldset>
            <div class="community-image">
              <img
                v-for="(item, index) in showcases"
                v-show="showcase === index"
                :key="item.name"
                :src="withBase(item.image)"
                width="1280"
                height="720"
                :alt="item.alt"
                :fetchpriority="index === 0 ? 'high' : 'auto'"
              />
            </div>
            <figcaption aria-live="polite">
              <div>
                <strong>{{ currentShowcase.title }}</strong>
                <p>{{ currentShowcase.description }}</p>
              </div>
              <a :href="currentShowcase.source"
                >{{ currentShowcase.credit }}
                <svg
                  class="inline-arrow"
                  viewBox="0 0 24 24"
                  aria-hidden="true"
                >
                  <path d="M6 18 18 6M6 6h12v12" /></svg
              ></a>
            </figcaption>
          </figure>
          <div class="detail-stack">
            <picture>
              <img
                :src="withBase('/landing/profile-detail.webp')"
                width="1729"
                height="910"
                alt="Pengu's mascot in a League client profile illustration."
                loading="lazy"
              />
            </picture>
            <div class="customization-note">
              <h2>Looks are just the start.</h2>
              <p>
                Themes change the interface. Plugins add the little things you
                wish were already there.
              </p>
              <a href="#explore"
                >See what you can change
                <svg class="inline-arrow" viewBox="0 0 24 24" aria-hidden="true">
                  <path d="M12 4v16m-6-6 6 6 6-6" /></svg
              ></a>
            </div>
          </div>
        </div>
      </section>

      <section
        id="explore"
        class="explore page-shell"
        aria-labelledby="explore-title"
      >
        <div class="explore-heading">
          <h2 id="explore-title">Make it look like you.</h2>
          <div class="explore-intro">
            <div
              class="explore-tabs"
              role="tablist"
              aria-label="Ways to customize"
              @keydown="moveTab"
            >
              <button
                v-for="(tab, index) in tabs"
                :id="`explore-tab-${index}`"
                :key="tab"
                role="tab"
                :aria-selected="selected === index"
                :aria-controls="`explore-panel-${index}`"
                :tabindex="selected === index ? 0 : -1"
                @click="selected = index"
              >
                {{ tab }}
              </button>
            </div>
            <p>{{ descriptions[selected] }}</p>
          </div>
        </div>

        <div
          v-show="selected === 0"
          id="explore-panel-0"
          class="explore-panel"
          role="tabpanel"
          aria-labelledby="explore-tab-0"
          tabindex="0"
        >
          <div class="effect-demo">
            <div class="effect-image">
              <Transition name="effect" mode="out-in">
                <img
                  :key="currentEffect.name"
                  :src="withBase(currentEffect.image)"
                  width="500"
                  height="255"
                  :alt="`${currentEffect.name} effect shown in the League client`"
                  loading="lazy"
                />
              </Transition>
            </div>
            <fieldset class="effect-options">
              <legend>Preview a window effect</legend>
              <label v-for="(item, index) in effects" :key="item.name">
                <input
                  v-model="effect"
                  type="radio"
                  name="window-effect"
                  :value="index"
                />
                <span>{{ item.name }}</span>
              </label>
            </fieldset>
            <p class="effect-caption" aria-live="polite">
              {{ currentEffect.description }}
            </p>
          </div>
          <div class="panel-copy">
            <h3>The same client.<br />A different feeling.</h3>
            <p>
              Your colors. Your type. Your favorite background. Themes give the
              client a different look, from small details to a whole new
              atmosphere.
            </p>
            <a class="text-link" href="https://chat.pengu.lol"
              >Find community themes
              <svg viewBox="0 0 24 24" aria-hidden="true">
                <path d="M5 12h14m-6-6 6 6-6 6" /></svg
            ></a>
            <a class="quiet-link" :href="withBase('/guide/css-theme')"
              >Learn to make a theme</a
            >
          </div>
        </div>

        <div
          v-show="selected === 1"
          id="explore-panel-1"
          class="explore-panel"
          role="tabpanel"
          aria-labelledby="explore-tab-1"
          tabindex="0"
        >
          <figure class="plugin-example">
            <img
              :src="withBase('/landing/balance-viewer.webp')"
              width="814"
              height="236"
              alt="Balance Buff Viewer adds a tooltip showing champion balance changes during champion select."
              loading="lazy"
            />
            <figcaption>
              <strong>Useful details, right where you need them.</strong>
              <p>
                Balance Buff Viewer puts game-mode buffs and nerfs into champion
                select.
              </p>
              <a href="https://github.com/nomi-san/balance-buff-viewer"
                >Balance Buff Viewer · nomi-san
                <svg
                  class="inline-arrow"
                  viewBox="0 0 24 24"
                  aria-hidden="true"
                >
                  <path d="M6 18 18 6M6 6h12v12" /></svg
              ></a>
            </figcaption>
          </figure>
          <div class="panel-copy">
            <h3>Choose what<br />belongs in your client.</h3>
            <p>
              Pengu is the loader. Plugins are where your setup becomes
              personal. Start with the community and follow each plugin’s
              installation instructions.
            </p>
            <a class="text-link" href="https://chat.pengu.lol"
              >Explore plugins on Discord
              <svg viewBox="0 0 24 24" aria-hidden="true">
                <path d="M5 12h14m-6-6 6 6-6 6" /></svg
            ></a>
            <a
              class="quiet-link"
              :href="withBase('/guide/installation#using-plugins')"
              >How to use plugins</a
            >
          </div>
        </div>

        <div
          v-show="selected === 2"
          id="explore-panel-2"
          class="explore-panel"
          role="tabpanel"
          aria-labelledby="explore-tab-2"
          tabindex="0"
        >
          <div class="first-plugin">
            <span>your-plugin / index.js</span>
            <pre><code><span class="code-comment">// Your first plugin. Really.</span>
console.log(<span class="code-string">'Hello, League Client!'</span>)</code></pre>
            <p>
              Save it in your plugins folder. Open the client. Find your message
              in DevTools.
            </p>
          </div>
          <div class="panel-copy">
            <h3>One file<br />is a start.</h3>
            <p>
              Use JavaScript to add functionality, CSS to change the look, and
              the client’s built-in DevTools to see what happens.
            </p>
            <a class="text-link" :href="withBase('/guide/javascript-plugin')"
              >Write your first plugin
              <svg viewBox="0 0 24 24" aria-hidden="true">
                <path d="M5 12h14m-6-6 6 6-6 6" /></svg
            ></a>
          </div>
        </div>
      </section>

      <section
        id="developers"
        class="developers"
        aria-labelledby="developers-title"
      >
        <div class="page-shell developer-grid">
          <div class="developer-copy">
            <h2 id="developers-title">
              You know the web.<br />Meet the client.
            </h2>
            <p>
              JavaScript modules. CSS themes. Chromium DevTools. Build inside
              the League client with a familiar set of tools and direct access
              to Pengu’s APIs.
            </p>
            <a
              class="button button-chalk"
              :href="withBase('/guide/javascript-plugin')"
              >Start building
              <svg viewBox="0 0 24 24" aria-hidden="true">
                <path d="M5 12h14m-6-6 6 6-6 6" /></svg
            ></a>
          </div>
          <div class="developer-resources">
            <div class="code-sample">
              <div class="code-title">
                your-plugin / index.js <span>JavaScript</span>
              </div>
              <pre><code><span class="code-comment">// A theme starts with a stylesheet.</span>
<span class="code-keyword">import</span> <span class="code-string">'./theme.css'</span>

<span class="code-keyword">export function</span> load() {
  console.log(<span class="code-string">'Make yourself at home.'</span>)
}</code></pre>
            </div>
            <a class="resource-link" :href="withBase('/runtime-api/')"
              ><span>Runtime API</span
              ><span
                >Hooks, storage, commands and more
                <svg viewBox="0 0 24 24" aria-hidden="true">
                  <path d="M5 12h14m-6-6 6 6-6 6" /></svg></span
            ></a>
            <a class="resource-link" :href="withBase('/guide/css-theme')"
              ><span>Theme guide</span
              ><span
                >Give the client a new look
                <svg viewBox="0 0 24 24" aria-hidden="true">
                  <path d="M5 12h14m-6-6 6 6-6 6" /></svg></span
            ></a>
          </div>
        </div>
      </section>

      <section
        class="get-started page-shell"
        aria-labelledby="get-started-title"
      >
        <div>
          <h2 id="get-started-title">Make yourself at home.</h2>
          <p>Get Pengu. Find a theme. Make it yours.</p>
        </div>
        <div class="closing-actions">
          <a class="button button-blue" :href="withBase('/download')"
            >Download Pengu
            <svg viewBox="0 0 24 24" aria-hidden="true">
              <path d="M12 3v13m-5-5 5 5 5-5M5 21h14" /></svg></a
          ><a class="quiet-link" href="https://chat.pengu.lol"
            >Meet the community on Discord</a
          >
        </div>
      </section>
    </main>

    <footer class="footer page-shell">
      <a class="brand" :href="withBase('/')"
        ><img :src="withBase('/icon.png')" width="28" height="28" alt="" /><span
          >Pengu Loader</span
        ></a
      >
      <nav aria-label="Footer navigation">
        <a :href="withBase('/guide/welcome')">Docs</a
        ><a :href="withBase('/guide/faqs')">FAQs</a
        ><a :href="withBase('/policy')">Usage policy</a
        ><a href="https://github.com/PenguLoader/PenguLoader">GitHub</a
        ><a href="https://chat.pengu.lol">Discord</a>
      </nav>
      <p>MIT licensed. © 2023–present Pengu Loader.</p>
      <p>An independent community project. Not affiliated with Riot Games.</p>
    </footer>
  </div>
</template>

<style scoped>
@font-face {
  font-family: "Pengu Text";
  src: url("/fonts/barlow-regular.ttf") format("truetype");
  font-weight: 400;
  font-style: normal;
  font-display: swap;
}
@font-face {
  font-family: "Pengu Text";
  src: url("/fonts/barlow-semibold.ttf") format("truetype");
  font-weight: 600;
  font-style: normal;
  font-display: swap;
}
.pengu-home {
  --surface: #17191d;
  --surface-deep: #111317;
  --surface-raised: #202329;
  --text: #eceef2;
  --muted: #aeb4bf;
  --accent: #a5bfff;
  --accent-ink: #15213d;
  --rule: #353a43;
  background: var(--surface);
  color: var(--text);
  font-family: "Pengu Text", sans-serif;
  font-size: 18px;
  line-height: 1.55;
  color-scheme: dark;
}
.pengu-home * { box-sizing: border-box; }
.pengu-home ::selection {
  background: var(--accent);
  color: var(--accent-ink);
}
.pengu-home a,
.pengu-home button,
.pengu-home summary {
  -webkit-tap-highlight-color: transparent;
  text-underline-offset: 5px;
}
.pengu-home a { color: inherit; text-decoration: none; }
.pengu-home a:hover { text-decoration: none; }
.pengu-home :focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 5px;
}
.pengu-home svg {
  width: 22px;
  height: 22px;
  fill: none;
  stroke: currentColor;
  stroke-width: 1.7;
  stroke-linecap: square;
  stroke-linejoin: miter;
  flex-shrink: 0;
}
.pengu-home h1,
.pengu-home h2,
.pengu-home h3,
.pengu-home p,
.pengu-home figure { margin: 0; }
.pengu-home h1,
.pengu-home h2,
.pengu-home h3 {
  font-weight: 600;
  letter-spacing: -0.025em;
  text-wrap: balance;
}
.pengu-home img { display: block; max-width: 100%; }
.page-shell {
  width: calc(100% - 112px);
  max-width: 1280px;
  margin-inline: auto;
}
.skip-link {
  position: fixed;
  left: 20px;
  top: 12px;
  z-index: 20;
  background: var(--accent);
  color: var(--accent-ink) !important;
  padding: 10px 20px;
  transform: translateY(-160%);
}
.skip-link:focus { transform: translateY(0); }
#main-content:focus { outline: none; }
.masthead-inner {
  display: flex;
  align-items: center;
  gap: 32px;
  min-height: 88px;
  border-bottom: 1px solid var(--rule);
}
.brand {
  display: inline-flex;
  align-items: center;
  gap: 12px;
  font-weight: 600;
  font-size: 22px;
  white-space: nowrap;
}
.brand img {
  border-radius: 6px;
  width: 36px;
  height: 36px;
  object-fit: contain;
}
.desktop-nav {
  display: flex;
  gap: 30px;
  align-items: center;
  margin-left: auto;
  font-size: 16px;
  color: var(--muted);
}
.desktop-nav a { padding-block: 12px; }
.desktop-nav a:hover,
.guide-link:hover,
.quiet-link:hover,
.footer nav a:hover {
  color: var(--text);
  text-decoration: underline;
}
.nav-download {
  border: 1px solid #59616e;
  border-radius: 6px;
  padding: 8px 18px;
  font-size: 15px;
  font-weight: 600;
  transition: background 0.18s ease-out;
}
.nav-download:hover { background: var(--surface-raised); }
.mobile-menu { display: none; }
.hero-stage {
  display: grid;
  grid-template-columns: minmax(280px, 0.8fr) minmax(0, 1.55fr);
  column-gap: 64px;
  row-gap: 64px;
  align-items: start;
  padding-top: 72px;
}
.hero-copy { padding-top: 44px; }
.hero h1 {
  font-size: clamp(46px, 4.9vw, 70px);
  line-height: 1.05;
}
.hero h1 span { display: block; white-space: nowrap; }
.hero h1 span + span { color: var(--muted); }
.hero-intro { margin-top: 28px; }
.hero-intro p {
  font-size: 21px;
  line-height: 1.5;
  color: var(--muted);
  margin-bottom: 28px;
  max-width: 29ch;
}
.desktop-break { display: none; }
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 12px;
  min-height: 48px;
  padding: 11px 22px;
  border-radius: 6px;
  font-size: 17px;
  font-weight: 600;
  line-height: 1.4;
  transition: background 0.18s ease-out;
}
.button-chalk,
.button-blue {
  background: var(--accent);
  color: var(--accent-ink) !important;
}
.button-chalk:hover,
.button-blue:hover { background: #c3d3ff; }
.guide-link {
  display: table;
  text-decoration: underline !important;
  margin-top: 16px;
  font-size: 16px;
}
.platforms {
  display: block;
  font-size: 13px;
  margin-top: 18px;
  color: var(--muted);
}
.community-showcase { min-width: 0; }
.pengu-home .inline-arrow {
  display: inline-block;
  width: 15px;
  height: 15px;
  vertical-align: -2px;
  margin-left: 3px;
}
.showcase-options,
.effect-options {
  display: flex;
  gap: 20px;
  align-items: center;
  flex-wrap: wrap;
  border: 0;
  padding: 0;
  margin: 0;
}
.showcase-options { padding-bottom: 14px; }
.showcase-options legend {
  float: left;
  margin-right: auto;
  font-size: 14px;
  font-weight: 600;
}
.showcase-options label,
.effect-options label {
  display: flex;
  gap: 8px;
  align-items: center;
  font-size: 14px;
  min-height: 36px;
  cursor: pointer;
  color: var(--muted);
}
.showcase-options label:has(input:checked),
.effect-options label:has(input:checked) { color: var(--text); }
.showcase-options input,
.effect-options input {
  accent-color: var(--accent);
  width: 15px;
  height: 15px;
  margin: 0;
  cursor: pointer;
}
.community-image {
  background: #080b10;
  aspect-ratio: 16 / 9;
  border: 1px solid var(--rule);
  border-radius: 8px;
  overflow: hidden;
}
.community-image img { width: 100%; height: 100%; object-fit: contain; }
.community-showcase figcaption {
  padding-top: 18px;
  font-size: 15px;
  line-height: 1.55;
}
.community-showcase figcaption strong { font-size: 17px; font-weight: 600; }
.community-showcase figcaption p { margin-top: 4px; color: var(--muted); max-width: 65ch; }
.community-showcase figcaption a {
  display: inline-block;
  font-size: 13px;
  color: var(--muted);
  margin-top: 12px;
  text-decoration: underline;
}
.community-showcase figcaption a:hover { color: var(--text); }
.detail-stack {
  grid-column: 1 / -1;
  display: grid;
  grid-template-columns: 248px minmax(0, 620px);
  justify-content: center;
  align-items: center;
  gap: 48px;
  border-top: 1px solid var(--rule);
  border-bottom: 1px solid var(--rule);
  padding-block: 36px;
}
.detail-stack picture { display: block; overflow: hidden; border-radius: 6px; }
.detail-stack img { width: 100%; height: auto; }
.customization-note h2 { font-size: 27px; line-height: 1.2; }
.customization-note p { margin-top: 10px; font-size: 17px; color: var(--muted); max-width: 58ch; }
.customization-note a {
  display: inline-block;
  margin-top: 14px;
  font-size: 15px;
  color: var(--accent);
}
.customization-note a:hover { text-decoration: underline; }
.explore { padding-block: 80px 88px; scroll-margin-top: 32px; }
.explore-heading {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 80px;
  align-items: start;
}
.explore h2 { font-size: clamp(32px, 3.1vw, 44px); line-height: 1.15; }
.explore-tabs {
  display: flex;
  align-items: center;
  gap: 30px;
  border-bottom: 1px solid var(--rule);
}
.explore-tabs button {
  font: inherit;
  font-size: 16px;
  white-space: nowrap;
  min-height: 44px;
  padding: 0 0 12px;
  position: relative;
  cursor: pointer;
  background: transparent;
  border: 0;
  color: var(--muted);
}
.explore-tabs button[aria-selected="true"] { color: var(--text); }
.explore-tabs button[aria-selected="true"]::after {
  content: "";
  position: absolute;
  bottom: -1px;
  left: 0;
  right: 0;
  height: 2px;
  background: var(--accent);
}
.explore-tabs button:hover { color: var(--text); }
.explore-intro p {
  margin-top: 20px;
  font-size: 16px;
  line-height: 1.65;
  color: var(--muted);
  max-width: 60ch;
}
.explore-panel {
  display: grid;
  grid-template-columns: 1.15fr 1fr;
  gap: 80px;
  align-items: center;
  min-height: 400px;
  margin-top: 48px;
}
.explore-panel:focus-visible { outline-offset: 12px; }
.effect-image {
  aspect-ratio: 500/255;
  background: var(--surface-deep);
  border-radius: 8px;
  overflow: hidden;
}
.effect-image img { width: 100%; height: 100%; object-fit: cover; }
.effect-options { margin-top: 18px; }
.effect-options legend { font-size: 13px; color: var(--muted); margin-bottom: 8px; }
.effect-caption { margin-top: 8px !important; font-size: 14px; color: var(--muted); }
.panel-copy { max-width: 440px; }
.panel-copy h3 { font-size: 32px; line-height: 1.2; margin-bottom: 20px; }
.panel-copy p { color: var(--muted); font-size: 18px; line-height: 1.65; margin-bottom: 24px; }
.text-link {
  display: inline-flex;
  align-items: center;
  gap: 20px;
  line-height: 1.4;
  padding-block: 10px;
  color: var(--accent) !important;
  font-weight: 600;
}
.text-link:hover { text-decoration: underline !important; }
.quiet-link { display: table; font-size: 15px; margin-top: 14px; color: var(--muted) !important; }
.first-plugin {
  background: var(--surface-deep);
  border: 1px solid var(--rule);
  border-radius: 8px;
  padding: 28px;
}
.first-plugin > span { font-size: 14px; color: var(--muted); }
.first-plugin pre { margin: 28px 0; font-size: 15px; overflow-x: auto; }
.first-plugin p { color: var(--muted); font-size: 16px; max-width: 40ch; }
.plugin-example img { width: 100%; height: auto; border-radius: 6px; }
.plugin-example figcaption { margin-top: 20px; font-size: 16px; line-height: 1.6; }
.plugin-example figcaption strong { font-weight: 600; font-size: 20px; }
.plugin-example figcaption p { color: var(--muted); margin-top: 6px; }
.plugin-example figcaption a { display: inline-block; margin-top: 14px; text-decoration: underline; font-size: 14px; color: var(--muted); }
.developers {
  background: var(--surface-deep);
  padding-block: 80px;
  scroll-margin-top: 0;
}
.developer-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 80px;
  align-items: center;
}
.developer-copy h2 { font-size: clamp(32px, 3.1vw, 44px); line-height: 1.18; }
.developer-copy p { max-width: 48ch; margin-top: 24px; font-size: 18px; line-height: 1.65; color: var(--muted); }
.developer-copy .button { margin-top: 28px; background: var(--surface-raised); color: var(--text) !important; }
.developer-copy .button:hover { background: var(--rule); }
.code-sample { border: 1px solid var(--rule); border-radius: 8px; overflow: hidden; }
.code-title {
  display: flex;
  justify-content: space-between;
  gap: 12px;
  padding: 14px 20px;
  border-bottom: 1px solid var(--rule);
  font-size: 13px;
}
.code-title span { color: var(--muted); }
.code-sample pre { margin: 0; padding: 28px 20px; overflow: auto; font-size: 14px; line-height: 1.85; tab-size: 2; }
.code-sample code,
.first-plugin code { font-family: Consolas, "Liberation Mono", monospace; }
.code-comment { color: var(--muted); }
.code-keyword { color: var(--accent); }
.code-string { color: #c8d6a8; }
.resource-link {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 20px;
  padding-block: 20px;
  border-bottom: 1px solid var(--rule);
  font-size: 17px;
}
.resource-link > span:last-child { display: flex; gap: 16px; align-items: center; font-size: 13px; color: var(--muted); }
.resource-link:hover { color: var(--accent); }
.get-started { display: flex; justify-content: space-between; gap: 40px; align-items: center; padding-block: 80px; }
.get-started h2 { font-size: clamp(32px, 3.1vw, 44px); line-height: 1.2; }
.get-started p { font-size: 18px; margin-top: 14px; color: var(--muted); }
.closing-actions { text-align: center; flex-shrink: 0; }
.closing-actions .quiet-link { margin-inline: auto; }
.footer {
  border-top: 1px solid var(--rule);
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 22px;
  padding-block: 28px;
}
.footer .brand { font-size: 18px; gap: 10px; }
.footer .brand img { width: 28px; height: 28px; }
.footer nav { display: flex; gap: 22px; justify-content: flex-end; align-items: center; font-size: 14px; }
.footer p { font-size: 12px; color: var(--muted); }
.footer p:last-child { text-align: right; }
.effect-enter-active,
.effect-leave-active { transition: opacity 0.16s ease-out, clip-path 0.2s ease-out; }
.effect-enter-from { opacity: 0.2; clip-path: inset(0 8% 0 0); }
.effect-leave-to { opacity: 0.35; }

@media (max-width: 1100px) {
  .page-shell { width: calc(100% - 64px); }
  .hero-stage { column-gap: 40px; grid-template-columns: minmax(260px, 0.9fr) minmax(0, 1.4fr); }
  .hero h1 { font-size: 54px; }
  .hero-intro p { font-size: 19px; }
  .showcase-options { gap: 8px 20px; }
  .showcase-options legend { float: none; width: 100%; margin-bottom: 2px; }
  .explore-heading,
  .explore-panel,
  .developer-grid { gap: 48px; }
  .explore-tabs { gap: 22px; }
  .resource-link > span:last-child { max-width: 58%; }
}
@media (max-width: 850px) {
  .masthead-inner { gap: 24px; min-height: 76px; }
  .brand { font-size: 20px; }
  .desktop-nav { gap: 18px; font-size: 15px; }
  .hero-stage { grid-template-columns: 1fr; padding-top: 44px; row-gap: 40px; }
  .hero-copy { display: grid; grid-template-columns: 1fr 1fr; gap: 32px; padding-top: 0; align-items: center; }
  .hero h1 { font-size: 58px; }
  .hero-intro { margin-top: 0; }
  .hero-intro p { margin-bottom: 20px; }
  .platforms { margin-top: 12px; }
  .showcase-options legend { float: left; width: auto; }
  .detail-stack { grid-template-columns: 200px minmax(0, 1fr); gap: 28px; padding-block: 28px; }
  .customization-note h2 { font-size: 24px; }
  .customization-note p { font-size: 16px; }
  .explore { padding-block: 60px; }
  .explore-heading { grid-template-columns: 1fr; gap: 26px; }
  .explore-intro p { max-width: 65ch; }
  .explore-panel { gap: 32px; min-height: 370px; margin-top: 36px; }
  .panel-copy h3 { font-size: 28px; }
  .panel-copy p { font-size: 17px; }
  .effect-options { gap: 10px 18px; }
  .developers { padding-block: 60px; }
  .developer-grid { gap: 32px; }
  .developer-copy p { font-size: 17px; }
  .code-sample pre { font-size: 12px; padding: 24px 16px; }
  .code-title { font-size: 12px; padding-inline: 16px; }
  .get-started { padding-block: 60px; }
  .footer nav { gap: 16px; }
}
@media (max-width: 639px) {
  .page-shell { width: calc(100% - 40px); }
  .masthead-inner { min-height: 72px; gap: 12px; }
  .brand { font-size: 17px; gap: 9px; }
  .brand img { width: 30px; height: 30px; }
  .desktop-nav { display: none; }
  .nav-download { margin-left: auto; font-size: 13px; padding: 8px 10px; }
  .mobile-menu { display: block; font-size: 14px; position: relative; }
  .mobile-menu summary { display: flex; align-items: center; gap: 2px; cursor: pointer; list-style: none; padding-block: 12px; }
  .mobile-menu summary::-webkit-details-marker { display: none; }
  .mobile-menu summary svg { width: 16px; height: 16px; }
  .mobile-menu[open] summary svg { transform: rotate(180deg); }
  .mobile-menu nav {
    position: absolute;
    z-index: 10;
    right: 0;
    top: 48px;
    width: 200px;
    padding: 9px 18px;
    border: 1px solid var(--rule);
    border-radius: 8px;
    background: var(--surface-raised);
  }
  .mobile-menu nav a { display: block; padding-block: 11px; }
  .hero-stage { padding-top: 36px; row-gap: 32px; }
  .hero-copy { display: block; }
  .hero h1 { font-size: clamp(42px, 12vw, 64px); }
  .hero-intro { margin-top: 22px; }
  .hero-intro p { font-size: 19px; max-width: 30ch; }
  .hero-intro .button { padding-inline: 20px; }
  .guide-link { font-size: 15px; margin-top: 14px; }
  .platforms { font-size: 12px; }
  .showcase-options { gap: 4px 18px; padding-bottom: 10px; }
  .showcase-options legend { float: none; width: 100%; margin-bottom: 4px; }
  .community-showcase figcaption { font-size: 14px; padding-top: 14px; }
  .community-showcase figcaption strong { font-size: 17px; }
  .community-showcase figcaption a { font-size: 12px; }
  .detail-stack { grid-template-columns: 120px 1fr; gap: 14px 20px; padding-block: 26px; }
  .detail-stack picture { grid-column: 1; grid-row: 1; }
  .customization-note { display: contents; }
  .customization-note h2 { grid-column: 2; font-size: 24px; }
  .customization-note p { grid-column: 1 / -1; margin-top: 0; }
  .customization-note a { grid-column: 1 / -1; margin-top: 0; }
  .explore { padding-block: 48px; }
  .explore h2 { font-size: 34px; }
  .explore-tabs { gap: 24px; }
  .explore-tabs button { font-size: 15px; }
  .explore-intro p { margin-top: 16px; font-size: 16px; }
  .explore-panel { grid-template-columns: 1fr; gap: 28px; min-height: 0; margin-top: 30px; }
  .panel-copy h3 { font-size: 28px; margin-bottom: 16px; }
  .panel-copy p { margin-bottom: 16px; }
  .effect-options { gap: 10px 18px; margin-top: 14px; }
  .effect-options label { font-size: 14px; }
  .effect-caption { font-size: 13px; }
  .text-link { font-size: 17px; gap: 16px; }
  .quiet-link { margin-top: 12px; }
  .first-plugin { padding: 24px 16px; }
  .first-plugin pre { font-size: 13px; }
  .plugin-example figcaption strong { font-size: 19px; }
  .developer-grid { grid-template-columns: 1fr; gap: 32px; }
  .developers { padding-block: 48px; }
  .developer-copy h2 { font-size: 34px; }
  .developer-copy p { margin-top: 20px; }
  .developer-copy .button { margin-top: 24px; }
  .code-sample pre { font-size: 12px; }
  .resource-link { font-size: 16px; padding-block: 18px; gap: 16px; }
  .resource-link > span:last-child { font-size: 12px; gap: 10px; max-width: 58%; }
  .get-started { display: block; padding-block: 48px; }
  .get-started h2 { font-size: 34px; }
  .get-started p { font-size: 17px; margin-top: 12px; }
  .closing-actions { text-align: left; margin-top: 24px; }
  .closing-actions .quiet-link { margin-inline: 0; }
  .footer { grid-template-columns: 1fr; gap: 18px; padding-block: 24px; }
  .footer nav { justify-content: flex-start; flex-wrap: wrap; gap: 12px 22px; font-size: 14px; }
  .footer p:last-child { text-align: left; margin-top: -8px; }
}
@media (max-width: 359px) {
  .page-shell { width: calc(100% - 32px); }
  .masthead-inner { gap: 9px; }
  .brand { font-size: 15px; gap: 7px; }
  .brand img { width: 26px; height: 26px; }
  .nav-download { font-size: 12px; padding-inline: 8px; }
  .mobile-menu { font-size: 13px; }
  .explore-tabs { gap: 20px; }
}
@media (prefers-reduced-motion: reduce) {
  .pengu-home *,
  .pengu-home *::before,
  .pengu-home *::after {
    scroll-behavior: auto !important;
    transition: none !important;
    animation: none !important;
  }
}
</style>
