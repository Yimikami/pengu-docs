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
          <h1 id="hero-title">
            <span>Your client.</span><span>Your rules.</span>
          </h1>
          <div class="hero-rail">
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

            <div class="detail-stack">
              <div class="customization-note">
                <h2>Looks are<br />just the start.</h2>
                <p>
                  Themes change the interface. Plugins add the little things you
                  wish were already there.
                </p>
                <a href="#explore"
                  >See what you can change
                  <svg
                    class="inline-arrow"
                    viewBox="0 0 24 24"
                    aria-hidden="true"
                  >
                    <path d="M12 4v16m-6-6 6 6 6-6" /></svg
                ></a>
              </div>
              <picture>
                <img
                  :src="withBase('/landing/profile-detail.webp')"
                  width="1729"
                  height="910"
                  alt="Pengu's mascot in a League client profile illustration."
                />
              </picture>
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
  font-family: "Pengu Display";
  src: url("/fonts/anton-regular.ttf") format("truetype");
  font-weight: 400;
  font-style: normal;
  font-display: swap;
}
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
  font-weight: 600 900;
  font-style: normal;
  font-display: swap;
}
.pengu-home {
  --blue: #2d52eb;
  --chalk: #f5f6f1;
  --ink: #101c40;
  --muted: #536078;
  --rule: #c9cfd8;
  background: var(--chalk);
  color: var(--ink);
  font-family: "Pengu Text", sans-serif;
  font-size: 18px;
  line-height: 1.5;
  overflow-x: clip;
  color-scheme: light;
}
.pengu-home * {
  box-sizing: border-box;
}
.pengu-home ::selection {
  background: var(--ink);
  color: var(--chalk);
}
.pengu-home a,
.pengu-home button,
.pengu-home summary {
  -webkit-tap-highlight-color: transparent;
  text-underline-offset: 5px;
}
.pengu-home a {
  color: inherit;
  text-decoration: none;
}
.pengu-home a:hover {
  text-decoration: none;
}
.pengu-home :focus-visible {
  outline: 3px solid currentColor;
  outline-offset: 5px;
}
.pengu-home svg {
  width: 24px;
  height: 24px;
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
.pengu-home figure {
  margin: 0;
}
.pengu-home h1,
.pengu-home h2 {
  font-family: "Pengu Display", sans-serif;
  font-weight: 400;
  letter-spacing: -0.025em;
}
.pengu-home img {
  display: block;
  max-width: 100%;
}
.page-shell {
  width: calc(100% - 8.333333%);
  max-width: 1648px;
  margin-inline: auto;
}
.skip-link {
  position: fixed;
  left: 20px;
  top: 12px;
  z-index: 20;
  background: var(--chalk);
  color: var(--ink) !important;
  padding: 10px 20px;
  transform: translateY(-160%);
}
.skip-link:focus {
  transform: translateY(0);
}
#main-content:focus {
  outline: none;
}
.masthead {
  background: var(--blue);
  color: var(--chalk);
}
.masthead-inner {
  display: flex;
  align-items: center;
  gap: 32px;
  min-height: 68px;
  border-bottom: 1px solid #ffffff65;
}
.brand {
  display: inline-flex;
  align-items: center;
  gap: 17px;
  font-weight: 600;
  font-size: 24px;
  white-space: nowrap;
}
.brand img {
  border-radius: 0;
  width: 38px;
  height: 38px;
  object-fit: contain;
}
.desktop-nav {
  display: flex;
  gap: 36px;
  align-items: center;
  margin-left: 22px;
  font-size: 17px;
}
.desktop-nav a {
  padding-block: 12px;
}
.desktop-nav a:hover,
.guide-link:hover,
.quiet-link:hover,
.footer nav a:hover {
  text-decoration: underline;
}
.nav-download {
  margin-left: auto;
  border: 1px solid var(--chalk);
  padding: 6px 22px;
  font-size: 16px;
  font-weight: 600;
  transition:
    background 0.18s,
    color 0.18s;
}
.nav-download:hover {
  background: var(--chalk);
  color: var(--ink);
}
.mobile-menu {
  display: none;
}
.hero {
  background: var(--blue);
  color: var(--chalk);
}
.hero-stage {
  position: relative;
  padding-top: 27px;
  display: grid;
  grid-template-columns: 71.31% 23.58%;
  column-gap: 5.11%;
  align-items: end;
}
.hero h1 {
  grid-column: 1 / -1;
  grid-row: 1;
  font-size: min(12.76vw, 230px);
  line-height: 0.82;
  text-transform: uppercase;
  padding-bottom: 28px;
  pointer-events: none;
}
.hero h1 span {
  display: block;
  white-space: nowrap;
}
.hero h1 span + span {
  margin-top: 12px;
}
.hero-rail {
  grid-column: 2;
  grid-row: 1 / 3;
  align-self: stretch;
  padding-top: min(11.34vw, 200px);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  gap: 32px;
}
.hero-intro {
  align-self: end;
  width: 81.85%;
  z-index: 1;
}
.hero-intro p {
  font-size: 22px;
  line-height: 1.3;
  margin-bottom: 17px;
}
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 16px;
  min-height: 50px;
  padding: 10px 26px;
  font-size: 20px;
  font-weight: 600;
  line-height: 1.3;
  border: 1px solid transparent;
  transition:
    background 0.18s,
    color 0.18s;
}
.button-chalk {
  background: var(--chalk);
  color: var(--ink) !important;
}
.button-chalk:hover {
  background: var(--ink);
  color: var(--chalk) !important;
  border-color: var(--chalk);
}
.hero-intro .button {
  width: 100%;
  padding-inline: 12px;
  white-space: nowrap;
}
.guide-link {
  display: table;
  text-decoration: underline !important;
  margin-top: 12px;
  font-size: 17px;
}
.platforms {
  display: block;
  font-size: 13px;
  line-height: 1.5;
  margin-top: 20px;
  color: #e0e6ff;
}
.detail-stack {
  grid-column: 2;
  display: grid;
  gap: 13px;
  padding-bottom: 30px;
}
.detail-stack picture {
  display: block;
  overflow: hidden;
}
.detail-stack img {
  width: 100%;
  height: auto;
}
.explore {
  padding-block: 48px 90px;
  scroll-margin-top: 24px;
}
.explore-heading {
  display: grid;
  grid-template-columns: 50% 24% 18.4%;
  gap: 3.8%;
  align-items: start;
}
.explore h2 {
  font-size: clamp(40px, 5.9vw, 104px);
  line-height: 1.04;
  white-space: nowrap;
}
.explore-intro {
  border-left: 1px solid #727d99;
  padding-left: 39px;
  min-height: 133px;
}
.explore-tabs {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 12px;
  border-bottom: 1px solid #a6b2d3;
}
.explore-tabs button {
  font: inherit;
  font-size: 16px;
  white-space: nowrap;
  padding: 0 0 16px;
  position: relative;
  cursor: pointer;
  background: transparent;
  border: 0;
  color: var(--ink);
}
.explore-tabs button[aria-selected="true"]::after {
  content: "";
  position: absolute;
  bottom: -1px;
  left: 0;
  right: 0;
  height: 2px;
  background: var(--blue);
}
.explore-tabs button:hover {
  color: var(--blue);
}
.explore-intro p {
  margin-top: 16px;
  font-size: 14px;
  line-height: 1.6;
}
.explore-panel {
  display: grid;
  grid-template-columns: 1.1fr 1fr;
  gap: 9%;
  align-items: center;
  min-height: 430px;
  margin-top: 62px;
}
.explore-panel:focus-visible {
  outline-offset: 12px;
}
.effect-image {
  aspect-ratio: 500/255;
  background: #dfe6eb;
  overflow: hidden;
}
.effect-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.effect-options {
  border: 0;
  padding: 0;
  margin: 18px 0 0;
  display: flex;
  gap: 23px;
  align-items: center;
  flex-wrap: wrap;
}
.effect-options legend {
  font-size: 13px;
  color: var(--muted);
  margin-bottom: 10px;
}
.effect-options label {
  display: flex;
  gap: 8px;
  align-items: center;
  font-size: 16px;
  cursor: pointer;
  min-height: 30px;
}
.effect-options input {
  width: 15px;
  height: 15px;
  margin: 0;
  accent-color: var(--blue);
  cursor: pointer;
}
.effect-caption {
  margin-top: 10px !important;
  font-size: 14px;
  color: var(--muted);
}
.panel-copy {
  max-width: 450px;
  padding-bottom: 30px;
}
.panel-copy h3 {
  font-size: 40px;
  line-height: 1.12;
  font-weight: 600;
  letter-spacing: -0.03em;
  margin-bottom: 20px;
}
.panel-copy p {
  color: var(--muted);
  font-size: 19px;
  line-height: 1.65;
  margin-bottom: 28px;
}
.text-link {
  display: inline-flex;
  align-items: center;
  justify-content: space-between;
  gap: 24px;
  line-height: 1.4;
  padding-block: 10px;
  border-bottom: 1px solid currentColor;
  font-weight: 600;
}
.text-link:hover {
  color: var(--blue);
}
.text-link svg {
  transition: transform 0.2s;
}
.text-link:hover svg {
  transform: translateX(4px);
}
.quiet-link {
  display: table;
  font-size: 16px;
  margin-top: 17px;
  color: var(--muted) !important;
}
.first-plugin {
  background: var(--ink);
  color: var(--chalk);
  padding: 30px;
}
.first-plugin > span {
  font-size: 14px;
  color: #c2ccdf;
}
.first-plugin pre {
  margin: 30px 0;
  font-size: 16px;
  overflow-x: auto;
}
.first-plugin p {
  color: #c2ccdf;
  font-size: 17px;
  max-width: 38ch;
}
.developers {
  background: var(--ink);
  color: var(--chalk);
  padding-block: 88px;
  scroll-margin-top: 0;
}
.developer-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10%;
  align-items: center;
}
.developer-copy h2 {
  font-size: clamp(44px, 4.6vw, 78px);
  line-height: 1.13;
}
.developer-copy p {
  max-width: 47ch;
  margin-top: 28px;
  font-size: 19px;
  line-height: 1.6;
  color: #ced5e5;
}
.developer-copy .button {
  margin-top: 32px;
}
.code-sample {
  background: #18274d;
}
.code-title {
  display: flex;
  justify-content: space-between;
  gap: 12px;
  padding: 15px 23px;
  border-bottom: 1px solid #435172;
  font-size: 14px;
}
.code-title span {
  color: #bec9e2;
}
.code-sample pre {
  margin: 0;
  padding: 32px 24px;
  overflow: auto;
  font-size: 16px;
  line-height: 1.85;
  color: var(--chalk);
  tab-size: 2;
}
.code-sample code,
.first-plugin code {
  font-family: Consolas, "Liberation Mono", monospace;
}
.code-comment {
  color: #adbcd8;
}
.code-keyword {
  color: #93b7ff;
}
.code-string {
  color: #deebaa;
}
.resource-link {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 20px;
  padding-block: 20px;
  border-bottom: 1px solid #53607e;
  font-size: 18px;
}
.resource-link > span:last-child {
  display: flex;
  gap: 16px;
  align-items: center;
  font-size: 14px;
  color: #ced5e5;
}
.resource-link:hover {
  color: #b4ccff;
}
.get-started {
  display: flex;
  justify-content: space-between;
  gap: 40px;
  align-items: center;
  padding-block: 75px;
}
.get-started h2 {
  font-size: clamp(38px, 4.5vw, 77px);
  line-height: 1.1;
}
.get-started p {
  font-size: 20px;
  margin-top: 17px;
  color: var(--muted);
}
.closing-actions {
  text-align: center;
  flex-shrink: 0;
}
.closing-actions .quiet-link {
  margin-inline: auto;
}
.button-blue {
  background: var(--blue);
  color: var(--chalk) !important;
}
.button-blue:hover {
  background: var(--ink);
}
.footer {
  border-top: 1px solid var(--rule);
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 22px;
  padding-block: 27px 30px;
}
.footer .brand {
  font-size: 19px;
  gap: 12px;
}
.footer .brand img {
  width: 28px;
  height: 28px;
}
.footer nav {
  display: flex;
  gap: 24px;
  justify-content: flex-end;
  align-items: center;
  font-size: 15px;
}
.footer p {
  font-size: 13px;
  color: var(--muted);
}
.footer p:last-child {
  text-align: right;
}
.effect-enter-active,
.effect-leave-active {
  transition:
    opacity 0.16s ease-out,
    clip-path 0.2s ease-out;
}
.effect-enter-from {
  opacity: 0.2;
  clip-path: inset(0 8% 0 0);
}
.effect-leave-to {
  opacity: 0.35;
}
@media (min-width: 1000px) {
  .hero-stage {
    padding-top: 22px;
  }
  .hero h1 {
    font-size: min(10.95vw, 197px);
    line-height: 0.88;
    padding-bottom: 18px;
  }
  .hero h1 span {
    width: max-content;
    transform: scaleX(1.45);
    transform-origin: left center;
  }
}
@media (min-width: 1800px) {
  .hero-intro p {
    font-size: 24px;
  }
  .hero-stage {
    padding-top: 36px;
  }
}
@media (min-width: 1000px) and (max-width: 1350px) {
  .hero-intro {
    width: 100%;
  }
  .hero-intro p {
    font-size: 19px;
  }
  .hero-intro .button {
    font-size: 17px;
    gap: 10px;
    min-height: 45px;
  }
  .guide-link {
    font-size: 15px;
  }
  .platforms {
    margin-top: 14px;
    font-size: 12px;
  }
  .hero-stage {
    padding-top: 26px;
  }
  .hero h1 {
    padding-bottom: 26px;
  }
  .explore-intro {
    padding-left: 20px;
  }
  .explore-tabs {
    gap: 8px;
  }
  .explore-tabs button {
    font-size: 14px;
  }
  .explore-heading {
    grid-template-columns: 49% 28% 16%;
    gap: 3.5%;
  }
  .explore h2 {
    font-size: 5.8vw;
  }
  .explore-panel {
    gap: 7%;
  }
  .panel-copy h3 {
    font-size: 34px;
  }
}
@media (max-width: 999px) {
  .hero-rail {
    display: contents;
  }
  .page-shell {
    width: calc(100% - 48px);
  }
  .masthead-inner {
    min-height: 72px;
    gap: 22px;
  }
  .brand {
    font-size: 21px;
    gap: 12px;
  }
  .brand img {
    width: 32px;
    height: 32px;
  }
  .desktop-nav {
    gap: 20px;
    margin-left: 0;
    font-size: 16px;
  }
  .nav-download {
    padding: 6px 14px;
  }
  .hero-stage {
    grid-template-columns: 1fr 32%;
    gap: 25px;
    padding-top: 35px;
  }
  .hero h1 {
    grid-column: 1/-1;
    font-size: 16vw;
    line-height: 0.86;
    padding-bottom: 5px;
  }
  .hero h1 span + span {
    margin-top: 7px;
  }
  .hero-intro {
    position: static;
    align-self: stretch;
    grid-column: 1/-1;
    grid-row: 2;
    width: auto;
    display: grid;
    grid-template-columns: 1fr 1fr;
    column-gap: 40px;
    align-items: start;
    margin-bottom: 10px;
  }
  .hero-intro p {
    font-size: 21px;
    margin: 0;
  }
  .hero-intro .button {
    grid-column: 2;
    grid-row: 1/3;
    width: auto;
  }
  .guide-link {
    grid-column: 2;
    grid-row: 3;
    margin-top: 12px;
  }
  .platforms {
    grid-column: 1;
    grid-row: 2;
    margin-top: 12px;
  }
  .desktop-break {
    display: none;
  }
  .detail-stack {
    grid-row: 3;
    padding-bottom: 25px;
    gap: 12px;
  }
  .explore {
    padding-block: 43px 60px;
  }
  .explore-heading {
    grid-template-columns: 1.3fr 1fr;
    gap: 35px;
  }
  .explore h2 {
    font-size: 6.3vw;
  }
  .explore-intro {
    padding-left: 25px;
  }
  .explore-panel {
    gap: 6%;
    margin-top: 40px;
    min-height: 420px;
  }
  .panel-copy h3 {
    font-size: 31px;
  }
  .panel-copy p {
    font-size: 17px;
  }
  .effect-options {
    gap: 12px;
  }
  .effect-options label {
    font-size: 14px;
  }
  .effect-caption {
    font-size: 13px;
  }
  .developer-grid {
    gap: 6%;
  }
  .developers {
    padding-block: 65px;
  }
  .developer-copy p {
    font-size: 17px;
  }
  .code-sample pre {
    font-size: 13px;
    padding: 24px 18px;
  }
  .resource-link {
    font-size: 17px;
  }
  .resource-link > span:last-child {
    font-size: 12px;
    gap: 9px;
  }
  .get-started {
    padding-block: 60px;
  }
  .get-started h2 {
    font-size: 5vw;
  }
  .get-started p {
    font-size: 18px;
  }
  .footer nav {
    gap: 16px;
  }
}
@media (max-width: 639px) {
  .page-shell {
    width: calc(100% - 36px);
  }
  .masthead-inner {
    min-height: 66px;
    gap: 12px;
  }
  .brand {
    font-size: 17px;
    gap: 9px;
  }
  .brand img {
    width: 29px;
    height: 29px;
  }
  .desktop-nav {
    display: none;
  }
  .nav-download {
    font-size: 13px;
    padding: 6px 10px;
  }
  .mobile-menu {
    display: block;
    font-size: 14px;
    position: relative;
  }
  .mobile-menu summary {
    display: flex;
    align-items: center;
    gap: 2px;
    cursor: pointer;
    list-style: none;
    padding-block: 12px;
  }
  .mobile-menu summary::-webkit-details-marker {
    display: none;
  }
  .mobile-menu summary svg {
    width: 16px;
    height: 16px;
  }
  .mobile-menu[open] summary svg {
    transform: rotate(180deg);
  }
  .mobile-menu nav {
    position: absolute;
    z-index: 10;
    right: 0;
    top: 48px;
    width: 200px;
    padding: 9px 18px;
    border: 1px solid #ffffff75;
    background: var(--blue);
    box-shadow: 0 12px 22px #13214925;
  }
  .mobile-menu nav a {
    display: block;
    padding-block: 11px;
  }
  .hero-stage {
    display: flex;
    flex-direction: column;
    align-items: stretch;
    padding-top: 29px;
    gap: 0;
  }
  .hero h1 {
    font-size: 17.5vw;
    line-height: 0.95;
    letter-spacing: -0.02em;
    padding-bottom: 25px;
  }
  .hero h1 span + span {
    margin-top: 0;
  }
  .hero-intro {
    display: grid;
    grid-template-columns: 1fr;
    gap: 0;
    margin-bottom: 29px;
  }
  .hero-intro p {
    font-size: 20px;
    max-width: 28ch;
    line-height: 1.4;
    margin-bottom: 19px;
  }
  .hero-intro .button {
    grid-column: auto;
    grid-row: auto;
    justify-content: flex-start;
    width: max-content;
    padding: 12px 21px;
    font-size: 18px;
    min-height: 48px;
  }
  .guide-link {
    grid-column: auto;
    grid-row: auto;
    margin-top: 15px;
    font-size: 15px;
  }
  .platforms {
    grid-column: auto;
    grid-row: auto;
    font-size: 12px;
    margin-top: 17px;
  }
  .detail-stack {
    display: none;
  }
  .explore {
    padding-block: 35px 45px;
  }
  .explore-heading {
    display: block;
  }
  .explore h2 {
    font-size: 10.5vw;
    white-space: normal;
    line-height: 1.16;
  }
  .explore-intro {
    border-left: 0;
    padding: 0;
    margin-top: 25px;
    min-height: 0;
  }
  .explore-tabs {
    justify-content: flex-start;
    gap: 25px;
  }
  .explore-tabs button {
    font-size: 16px;
    padding-bottom: 13px;
    min-height: 42px;
  }
  .explore-intro p {
    font-size: 16px;
    margin-top: 16px;
    line-height: 1.6;
  }
  .explore-panel {
    grid-template-columns: 1fr;
    gap: 33px;
    min-height: 0;
    margin-top: 31px;
  }
  .panel-copy {
    padding-bottom: 0;
    max-width: none;
  }
  .panel-copy h3 {
    font-size: 32px;
    margin-bottom: 17px;
  }
  .panel-copy p {
    font-size: 17px;
    line-height: 1.6;
    margin-bottom: 15px;
  }
  .effect-options {
    gap: 16px;
    margin-top: 12px;
  }
  .effect-options label {
    font-size: 14px;
  }
  .effect-options legend {
    font-size: 12px;
  }
  .effect-caption {
    font-size: 13px;
  }
  .text-link {
    font-size: 17px;
    gap: 16px;
  }
  .quiet-link {
    font-size: 15px;
    margin-top: 14px;
  }
  .first-plugin {
    padding: 23px 18px;
  }
  .first-plugin pre {
    font-size: 13px;
  }
  .developer-grid {
    grid-template-columns: 1fr;
    gap: 35px;
  }
  .developers {
    padding-block: 43px;
  }
  .developer-copy h2 {
    font-size: 11.4vw;
    line-height: 1.16;
  }
  .developer-copy p {
    margin-top: 23px;
    font-size: 17px;
  }
  .developer-copy .button {
    font-size: 18px;
    margin-top: 24px;
  }
  .code-title {
    font-size: 12px;
    padding: 13px 15px;
  }
  .code-sample pre {
    font-size: 12px;
    padding: 23px 15px;
  }
  .resource-link {
    font-size: 17px;
    padding-block: 18px;
  }
  .resource-link > span:last-child {
    font-size: 12px;
    max-width: 54%;
  }
  .resource-link svg {
    width: 20px;
    height: 20px;
  }
  .get-started {
    display: block;
    padding-block: 43px;
  }
  .get-started h2 {
    font-size: 11.3vw;
    line-height: 1.15;
  }
  .get-started p {
    font-size: 17px;
    margin-top: 15px;
  }
  .closing-actions {
    text-align: left;
    margin-top: 25px;
  }
  .closing-actions .button {
    font-size: 18px;
  }
  .closing-actions .quiet-link {
    margin-inline: 0;
  }
  .footer {
    grid-template-columns: 1fr;
    gap: 20px;
    padding-block: 24px;
  }
  .footer nav {
    justify-content: flex-start;
    flex-wrap: wrap;
    gap: 13px 23px;
    font-size: 15px;
  }
  .footer p {
    font-size: 12px;
    line-height: 1.6;
  }
  .footer p:last-child {
    text-align: left;
    margin-top: -12px;
  }
}
.community-showcase {
  grid-column: 1;
  grid-row: 2;
  min-width: 0;
}
.pengu-home .inline-arrow {
  display: inline-block;
  width: 15px;
  height: 15px;
  vertical-align: -2px;
  margin-left: 3px;
}
.showcase-options {
  display: flex;
  gap: 22px;
  align-items: center;
  flex-wrap: wrap;
  border: 0;
  padding: 0 0 16px;
  margin: 0;
}
.showcase-options legend {
  float: left;
  margin-right: auto;
  font-size: 18px;
  font-weight: 600;
}
.showcase-options label {
  display: flex;
  gap: 8px;
  align-items: center;
  font-size: 15px;
  min-height: 32px;
  cursor: pointer;
}
.showcase-options input {
  accent-color: var(--chalk);
  width: 16px;
  height: 16px;
  margin: 0;
}
.community-image {
  background: var(--ink);
  aspect-ratio: 16 / 9;
  border: 1px solid #ffffff50;
}
.community-image img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}
.community-showcase figcaption {
  display: flex;
  align-items: start;
  justify-content: space-between;
  gap: 24px;
  padding: 18px 0 25px;
  font-size: 15px;
  line-height: 1.5;
}
.community-showcase figcaption strong {
  font-size: 20px;
  font-weight: 600;
}
.community-showcase figcaption p {
  margin-top: 3px;
  max-width: 51ch;
}
.community-showcase figcaption a {
  max-width: 23ch;
  text-decoration: underline;
  text-underline-offset: 4px;
}
.customization-note {
  border-top: 1px solid #ffffff70;
  padding-top: 24px;
  margin-bottom: 22px;
}
.customization-note h2 {
  font-family: "Pengu Text", sans-serif;
  font-size: clamp(25px, 2.4vw, 38px);
  font-weight: 600;
  line-height: 1.05;
  letter-spacing: -0.025em;
}
.customization-note p {
  margin-top: 18px;
  font-size: 17px;
  line-height: 1.5;
}
.customization-note a {
  display: inline-block;
  margin-top: 18px;
  font-size: 15px;
  text-decoration: underline;
  text-underline-offset: 4px;
}
.explore-heading {
  grid-template-columns: 52% 40%;
  gap: 8%;
}
.explore-intro p {
  max-width: 55ch;
}
.plugin-example img {
  display: block;
  width: 100%;
  height: auto;
  border: 1px solid var(--rule);
}
.plugin-example figcaption {
  margin-top: 20px;
  font-size: 16px;
  line-height: 1.6;
}
.plugin-example figcaption strong {
  font-weight: 600;
  font-size: 22px;
}
.plugin-example figcaption p {
  color: var(--muted);
  margin-top: 6px;
}
.plugin-example figcaption a {
  display: inline-block;
  margin-top: 14px;
  text-decoration: underline;
  text-underline-offset: 4px;
}
@media (max-width: 999px) {
  .community-showcase {
    grid-row: 3;
    grid-column: 1 / -1;
  }
  .detail-stack {
    display: none;
  }
  .community-showcase figcaption {
    padding-bottom: 30px;
  }
  .explore-heading {
    grid-template-columns: 1fr 1fr;
    gap: 6%;
  }
}
@media (max-width: 640px) {
  .showcase-options {
    gap: 7px 19px;
    padding-bottom: 13px;
  }
  .showcase-options legend {
    float: none;
    width: 100%;
    font-size: 17px;
    margin-bottom: 7px;
  }
  .showcase-options label {
    font-size: 14px;
  }
  .community-showcase figcaption {
    display: block;
    font-size: 14px;
    padding: 15px 0 26px;
  }
  .community-showcase figcaption strong {
    font-size: 19px;
  }
  .community-showcase figcaption a {
    display: inline-block;
    max-width: none;
    margin-top: 13px;
    font-size: 12px;
  }
  .plugin-example figcaption strong {
    font-size: 20px;
  }
}
@media (prefers-reduced-motion: reduce) {
  .pengu-home *,
  .pengu-home *::before,
  .pengu-home *::after {
    scroll-behavior: auto !important;
    transition: none !important;
    animation: none !important;
  }
  .text-link:hover svg {
    transform: none;
  }
}
</style>
