<script setup>
import { computed, onMounted, onBeforeUnmount, ref } from "vue";
import profileImage from "./assets/profile/avatar.webp";
import kotoneImage from "./assets/idolmaster/KTN.png";
import fuyuImage from "./assets/idolmaster/fuyu.jpg";
import mikiImage from "./assets/idolmaster/miki.jpg";
import rinhaImage from "./assets/idolmaster/0ha.jpg";
import kazuImage from "./assets/lovelive/kz.jpg";
import maiImage from "./assets/lovelive/mai.jpg";
import kohaneImage from "./assets/miku-stage/khn.png";
import ioriImage from "./assets/equal-love/iori.jpeg";

const pages = {
  home: {
    title: "首页",
    hash: "#/",
  },
  about: {
    title: "关于",
    hash: "#/about",
  },
};

const heroLines = [
  { text: "偶像大师" },
  { text: "LoveLive!" },
  { text: "初音未来 缤纷舞台！", long: true },
  { text: "=LOVE", accent: true },
  { text: "J-POP" },
];

const profileLinks = [
  {
    name: "X",
    value: "@YouSan233",
    tag: "Follow",
    href: "https://x.com/YouSan233",
  },
  {
    name: "REDNOTE",
    value: "423711376",
    tag: "Open",
    href: "https://xhslink.com/m/49SmGoZdWrQ",
  },
];

const copyItems = [
  { label: "复制 QQ", value: "QQ: 526530439" },
  { label: "复制 P-ID", value: "P-ID: C4DTWCWB" },
];

const highlightCards = [
  {
    label: "常出没",
    title: "平台入口",
    text: "X、REDNOTE、QQ 这些平台都可以找到我。",
  },
  {
    label: "主要内容",
    title: "推活和日常",
    text: "会分享跑活记录、圣地巡礼，还有一些喜欢的作品相关内容。",
  },
  {
    label: "欢迎交流",
    title: "互关也可以",
    text: "如果你也喜欢这些内容，欢迎来聊天、互关或者交换名片。",
  },
];

const focusPoints = [
  "Live",
  "新番",
  "女声优个活",
  "单机游戏",
];

const topics = [
  {
    id: "idolmaster",
    number: "01",
    name: "偶像大师",
    entries: [
      {
        title: "藤田 言音",
        imageLabel: "角色图片 01",
        image: kotoneImage,
        imagePosition: "50% 22%",
        text: "爱钱的小偶像，很难不喜欢。薯ki薯ki大薯ki❤。",
      },
      {
        title: "黛 冬优子",
        imageLabel: "角色图片 02",
        image: fuyuImage,
        imagePosition: "38% 42%",
        text: "带专夹子，但是非常喜欢fuyu不夹之后那种老婆感，还有宅属性，妈妈与老婆并存。想永远支持她。",
      },
      {
        title: "星井 美希",
        imageLabel: "角色图片 03",
        image: mikiImage,
        imagePosition: "40% 30%",
        text: "我一直都是Darling 啊！",
      },
      {
        title: "賀陽 燐羽",
        imageLabel: "角色图片 04",
        image: rinhaImage,
        imagePosition: "7% 20%",
        text: "快点实装！快点实装！快点实装！快点实装！快点实装！快点实装！快点实装！快点实装！快点实装！快点实装！快点实装！",
      },
    ],
  },
  {
    id: "lovelive",
    number: "02",
    name: "LoveLive!",
    entries: [
      {
        title: "乙宗 梢",
        imageLabel: "角色图片 01",
        image: kazuImage,
        imagePosition: "50% 26%",
        text: "梢先輩、、、",
      },
      {
        title: "麻布 麻衣",
        imageLabel: "角色图片 02",
        image: maiImage,
        imagePosition: "50% 22%",
        text: "程序员，喜欢❤",
      },
    ],
  },
  {
    id: "miku-stage",
    number: "03",
    name: "初音未来 缤纷舞台！",
    entries: [
      {
        title: "小豆泽 こはね",
        imageLabel: "角色图片 01",
        image: kohaneImage,
        imagePosition: "64% 30%",
        text: "很喜欢小豆的嗓音，虽然本人很小只很可爱，但是歌声充满力量。",
      },
    ],
  },
  {
    id: "equal-love",
    number: "04",
    name: "=LOVE",
    entries: [
      {
        title: "野口 衣織",
        imageLabel: "角色图片 01",
        image: ioriImage,
        imagePosition: "50% 20%",
        text: "喜欢衣织的综艺感，歌声和没有架子的感觉。（还有笑声真的非常搞笑）",
      },
    ],
  },
  {
    id: "j-pop",
    number: "05",
    name: "J-POP",
    entries: [
      {
        title: "喜欢的封面",
        imageLabel: "封面预留 01",
        text: "J-POP 更像是整体兴趣背景，会影响我平时喜欢的风格、听歌习惯和关注方向，所以也想留一个独立区块慢慢补充。",
      },
      {
        title: "循环播放",
        imageLabel: "封面预留 02",
        text: "这一块比较适合放专辑封面、歌手宣传图或者你很喜欢的单曲视觉图，不一定非要只放一张。",
      },
      {
        title: "继续补充",
        imageLabel: "封面预留 03",
        text: "如果你有特别常听的歌手或者某几张很喜欢的封面，也可以继续用这种一行一条的方式往下记。",
      },
    ],
  },
];

const currentPage = ref("home");
const copyStatus = ref("点击按钮即可复制联系方式。");
const activeTopicId = ref(topics[0].id);

const activeTopic = computed(
  () => topics.find((topic) => topic.id === activeTopicId.value) ?? topics[0],
);

function syncPageWithHash() {
  currentPage.value = window.location.hash === pages.about.hash ? "about" : "home";
}

function setPage(page) {
  currentPage.value = page;
  window.location.hash = pages[page].hash;
}

async function copyText(text) {
  try {
    await navigator.clipboard.writeText(text);
    copyStatus.value = `已复制: ${text}`;
  } catch (error) {
    copyStatus.value = `复制失败，请手动复制: ${text}`;
  }
}

function setTopic(topicId) {
  activeTopicId.value = topicId;
}

onMounted(() => {
  syncPageWithHash();
  window.addEventListener("hashchange", syncPageWithHash);
});

onBeforeUnmount(() => {
  window.removeEventListener("hashchange", syncPageWithHash);
});
</script>

<template>
  <div class="page-shell">
    <nav class="navbar">
      <div class="nav-container">
        <button class="nav-logo nav-button" type="button" @click="setPage('home')">ブブブＰ</button>
        <ul class="nav-links">
          <li>
            <button
              class="nav-link-button"
              :class="{ active: currentPage === 'home' }"
              type="button"
              @click="setPage('home')"
            >
              首页
            </button>
          </li>
          <li>
            <button
              class="nav-link-button"
              :class="{ active: currentPage === 'about' }"
              type="button"
              @click="setPage('about')"
            >
              关于
            </button>
          </li>
        </ul>
      </div>
    </nav>

    <main class="main-content">
      <template v-if="currentPage === 'home'">
        <section class="hero container">
          <div class="hero-copy">
            <p class="eyebrow">Personal Link Hub</p>
            <h1 class="hero-title">
              <span
                v-for="line in heroLines"
                :key="line.text"
                class="title-line"
                :class="{
                  'title-line-long': line.long,
                  'title-line-accent': line.accent,
                }"
              >
                {{ line.text }}
              </span>
            </h1>
            <p class="hero-text">
              这里是 ブブブＰ 的个人主页。你可以从这里快速找到我的社交账号，也可以直接复制常用联系方式。
            </p>
            <div class="hero-actions">
              <a class="primary-link" href="#/about" @click.prevent="setPage('about')">了解更多</a>
            </div>
          </div>

          <div class="profile-card">
            <img class="avatar" :src="profileImage" alt="ブブブＰ 头像" />
            <div class="profile-meta">
              <p class="mini-label">ブブブＰ</p>
              <h2>社交主页</h2>
              <p class="sub">
                记录跑活日常，分享二偶相关内容。如果有共同兴趣，欢迎关注我的社交账号！
              </p>
            </div>

            <div class="links">
              <a
                v-for="link in profileLinks"
                :key="link.name"
                class="btn"
                :href="link.href"
                target="_blank"
                rel="noreferrer"
              >
                <span>
                  <strong>{{ link.name }}</strong>
                  <small>{{ link.value }}</small>
                </span>
                <span class="tag">{{ link.tag }}</span>
              </a>
            </div>

            <div class="button-group">
              <button
                v-for="item in copyItems"
                :key="item.label"
                class="copy-btn"
                type="button"
                @click="copyText(item.value)"
              >
                {{ item.label }}
              </button>
            </div>

            <p class="copy-hint" aria-live="polite">{{ copyStatus }}</p>
          </div>
        </section>

        <section class="highlights container">
          <article v-for="card in highlightCards" :key="card.title" class="info-card">
            <p class="mini-label">{{ card.label }}</p>
            <h3>{{ card.title }}</h3>
            <p>{{ card.text }}</p>
          </article>
        </section>
      </template>

      <template v-else>
        <section class="inner-hero container">
          <p class="eyebrow">About Me</p>
          <h1>自我介绍</h1>
        </section>

        <section class="content-grid container">
          <article class="content-card">
            <p class="mini-label">简介</p>
            <h2>ブブブＰ</h2>
            <p>
              日本在读修士，CV方向。现在沉迷跑LIVE和女声优个活。日常生活比较无聊，所以在这里记录下自己的兴趣和喜欢的内容，也欢迎有共同兴趣的朋友来交流！
            </p>
            <p>
              这个网站是一个轻量的个人主页，下面会陈列出我推的角色。
            </p>
          </article>

          <article class="content-card">
            <p class="mini-label">兴趣</p>
            <h2>我在关注什么</h2>
            <ul class="feature-list">
              <li v-for="point in focusPoints" :key="point">{{ point }}</li>
            </ul>
          </article>
        </section>

        <section class="topics-section container">
        <div class="section-heading">
          <p class="mini-label">首页展示</p>
          <h2>我会关注的内容</h2>
        </div>

          <div class="topic-switcher" role="tablist" aria-label="兴趣分类切换">
            <button
              v-for="topic in topics"
              :key="topic.id"
              class="topic-tab"
              :class="{ active: activeTopicId === topic.id }"
              type="button"
              role="tab"
              :aria-selected="String(activeTopicId === topic.id)"
              @click="setTopic(topic.id)"
            >
              {{ topic.name }}
            </button>
          </div>

          <div class="topic-panels">
            <article class="topic-card active" :id="activeTopic.id" role="tabpanel">
              <div class="topic-content">
                <p class="mini-label">{{ activeTopic.number }}</p>
                <h3>{{ activeTopic.name }}</h3>
                <div class="topic-stack">
                  <div
                    v-for="entry in activeTopic.entries"
                    :key="entry.imageLabel + entry.text"
                    class="topic-entry"
                  >
                    <div class="topic-entry-media">
                      <img
                        v-if="entry.image"
                        class="topic-entry-image"
                        :src="entry.image"
                        :alt="entry.title"
                        :style="{ objectPosition: entry.imagePosition || '50% 50%' }"
                      />
                      <span v-else>{{ entry.imageLabel }}</span>
                    </div>
                    <div class="topic-entry-copy">
                      <p class="topic-entry-title">{{ entry.title }}</p>
                      <p class="topic-entry-text">{{ entry.text }}</p>
                    </div>
                  </div>
                </div>
              </div>
            </article>
          </div>
        </section>
      </template>
    </main>

    <footer class="footer">
      <p>© 2026 ブブブＰ. 欢迎通过社交平台联系我。</p>
    </footer>
  </div>
</template>
