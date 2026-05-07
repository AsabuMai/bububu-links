<script setup>
import { computed, onBeforeUnmount, onMounted, ref } from "vue";
import profileImage from "./assets/profile/avatar.webp";
import kotoneImage from "./assets/idolmaster/KTN.png";
import fuyuImage from "./assets/idolmaster/fuyu.jpg";
import mikiImage from "./assets/idolmaster/miki.jpg";
import rinhaImage from "./assets/idolmaster/0ha.jpg";
import kazuImage from "./assets/lovelive/kz.jpg";
import maiImage from "./assets/lovelive/mai.jpg";
import kohaneImage from "./assets/miku-stage/khn.png";
import ioriImage from "./assets/equal-love/iori.jpeg";
import yoasobiImage from "./assets/j-pop/yoasobi.jpeg";
import higedanImage from "./assets/j-pop/higedan.jpg";
import vaundyImage from "./assets/j-pop/vaundy.png";
import zutomayoImage from "./assets/j-pop/zutomayo.jpg";
import hachiImage from "./assets/j-pop/hachi.jpg";

const baseUrl = import.meta.env.BASE_URL.endsWith("/")
  ? import.meta.env.BASE_URL
  : `${import.meta.env.BASE_URL}/`;

const localeStorageKey = "bububu-links-locale";

const messages = {
  zh: {
    nav: {
      home: "首页",
      about: "关于",
    },
    localeLabel: "语言",
    localeOptions: {
      zh: "中文",
      jp: "日本語",
    },
    home: {
      eyebrow: "Personal Link Hub",
      heroLines: [
        { text: "偶像大师" },
        { text: "LoveLive!" },
        { text: "初音未来 缤纷舞台！", long: true },
        { text: "=LOVE", accent: true },
        { text: "J-POP" },
      ],
      text: "这里是 ブブブＰ 的个人主页。你可以从这里快速找到我的社交账号，也可以直接复制常用联系方式。",
      more: "了解更多",
      profileLabel: "ブブブＰ",
      profileTitle: "社交主页",
      profileText: "记录跑活日常，分享二偶相关内容。如果有共同兴趣，欢迎关注我的社交账号！",
      copyItems: [
        { label: "复制 QQ", value: "QQ: 526530439" },
        { label: "复制 P-ID", value: "P-ID: C4DTWCWB" },
      ],
      copyHint: "点击按钮即可复制联系方式。",
      copied: "已复制",
      copyFailed: "复制失败，请手动复制",
      highlightCards: [
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
      ],
    },
    about: {
      eyebrow: "About Me",
      title: "自我介绍",
      introLabel: "简介",
      introTitle: "ブブブＰ",
      introParagraphs: [
        "日本在读修士，CV方向。现在沉迷跑LIVE和女声优个活。日常生活比较无聊，所以在这里记录下自己的兴趣和喜欢的内容，也欢迎有共同兴趣的朋友来交流！",
        "这个网站是一个轻量的个人主页，下面会陈列出我推的角色。",
      ],
      focusLabel: "兴趣",
      focusTitle: "我在关注什么",
      focusPoints: ["Live", "新番", "女声优个活", "单机游戏"],
      topicsLabel: "首页展示",
      topicsTitle: "我会关注的内容",
      topicSwitcherLabel: "兴趣分类切换",
    },
    footer: "© 2026 ブブブＰ. 欢迎通过社交平台联系我。",
  },
  jp: {
    nav: {
      home: "ホーム",
      about: "プロフィール",
    },
    localeLabel: "言語",
    localeOptions: {
      zh: "中文",
      jp: "日本語",
    },
    home: {
      eyebrow: "Personal Link Hub",
      heroLines: [
        { text: "アイドルマスター" },
        { text: "ラブライブ！" },
        { text: "プロセカ" },
        { text: "=LOVE", accent: true },
        { text: "J-POP" },
      ],
      text: "ここは ブブブＰ の個人ページです。SNSアカウントをまとめて見られて、よく使う連絡先もすぐコピーできます。",
      more: "もっと見る",
      profileLabel: "ブブブＰ",
      profileTitle: "SNSプロフィール",
      profileText: "現地参加の記録や、二次元アイドル関連の好きなものをまとめています。共通の趣味があれば、ぜひフォローしてください。",
      copyItems: [
        { label: "QQをコピー", value: "QQ: 526530439" },
        { label: "P-IDをコピー", value: "P-ID: C4DTWCWB" },
      ],
      copyHint: "ボタンを押すと連絡先をコピーできます。",
      copied: "コピーしました",
      copyFailed: "コピーできませんでした。手動でコピーしてください",
      highlightCards: [
        {
          label: "よくいる場所",
          title: "SNSリンク",
          text: "X、REDNOTE、QQ で私を見つけられます。",
        },
        {
          label: "主な内容",
          title: "推し活と日常",
          text: "現場の記録や聖地巡礼、それから好きな作品に関することを載せています。",
        },
        {
          label: "気軽にどうぞ",
          title: "相互も歓迎",
          text: "同じものが好きなら、ぜひ気軽に話しかけたり相互になったりしてください。",
        },
      ],
    },
    about: {
      eyebrow: "About Me",
      title: "自己紹介",
      introLabel: "プロフィール",
      introTitle: "ブブブＰ",
      introParagraphs: [
        "日本で修士課程に在学中で、分野はCV寄りです。今はLIVE現場と女性声優の個人活動にかなりハマっています。日常はわりと地味なので、ここでは自分の趣味や好きなものをまとめています。共通の趣味がある方は、ぜひ気軽に話しかけてください！",
        "このサイトは軽めの個人ページで、下には自分が好きなキャラやアーティストを並べています。",
      ],
      focusLabel: "興味",
      focusTitle: "今気になっているもの",
      focusPoints: ["Live", "アニメ", "女性声優の個人活動", "ゲーム"],
      topicsLabel: "ホーム表示",
      topicsTitle: "よく見ているもの",
      topicSwitcherLabel: "興味カテゴリ切り替え",
    },
    footer: "© 2026 ブブブＰ. SNSから気軽に連絡してください。",
  },
};

const topicData = [
  {
    id: "idolmaster",
    name: { zh: "偶像大师", jp: "アイドルマスター" },
    entries: [
      {
        title: "藤田 言音",
        imageLabel: "角色图片 01",
        image: kotoneImage,
        imagePosition: "50% 22%",
        text: {
          zh: "爱钱的小偶像，很难不喜欢。薯ki薯ki大薯ki❤。",
          jp: "お金が大好きな小さなアイドルで、好きにならないほうが難しい。しゅきっしゅきっ❤",
        },
        titleJp: "藤田 ことね",
      },
      {
        title: "黛 冬优子",
        imageLabel: "角色图片 02",
        image: fuyuImage,
        imagePosition: "38% 42%",
        text: {
          zh: "带专夹子，但是非常喜欢fuyu不夹之后那种老婆感，还有宅属性，妈妈与老婆并存。想永远支持她。",
          jp: "あざといところも含めて好きだけど、素が見えた時の嫁っぽさやオタクっぽさが本当に刺さる。母性っぽさと嫁感が同時にあるのも好きで、ずっと応援したい。",
        },
      },
      {
        title: "星井 美希",
        imageLabel: "角色图片 03",
        image: mikiImage,
        imagePosition: "40% 30%",
        text: {
          zh: "我一直都是Darling 啊！",
          jp: "私はずっとDarlingだよ！",
        },
      },
      {
        title: "賀陽 燐羽",
        imageLabel: "角色图片 04",
        image: rinhaImage,
        imagePosition: "7% 20%",
        text: {
          zh: "快点实装！快点实装！快点实装！快点实装！快点实装！快点实装！快点实装！快点实装！快点实装！快点实装！快点实装！",
          jp: "早く実装して！早く実装して！早く実装して！早く実装して！早く実装して！早く実装して！早く実装して！",
        },
      },
    ],
  },
  {
    id: "lovelive",
    name: { zh: "LoveLive!", jp: "ラブライブ！" },
    entries: [
      {
        title: "乙宗 梢",
        imageLabel: "角色图片 01",
        image: kazuImage,
        imagePosition: "50% 26%",
        text: {
          zh: "梢先輩、、、",
          jp: "梢先輩、、、",
        },
      },
      {
        title: "麻布 麻衣",
        imageLabel: "角色图片 02",
        image: maiImage,
        imagePosition: "50% 22%",
        text: {
          zh: "程序员，喜欢❤",
          jp: "プログラマー、好き❤",
        },
      },
    ],
  },
  {
    id: "miku-stage",
    name: { zh: "初音未来 缤纷舞台！", jp: "プロセカ" },
    entries: [
      {
        title: "小豆泽 こはね",
        imageLabel: "角色图片 01",
        image: kohaneImage,
        imagePosition: "64% 30%",
        text: {
          zh: "很喜欢小豆的嗓音，虽然本人很小只很可爱，但是歌声充满力量。",
          jp: "こはねの声が本当に好き。本人は小さくて可愛いのに、歌声はすごく力強い。",
        },
      },
    ],
  },
  {
    id: "equal-love",
    name: { zh: "=LOVE", jp: "=LOVE" },
    entries: [
      {
        title: "野口 衣織",
        imageLabel: "角色图片 01",
        image: ioriImage,
        imagePosition: "50% 20%",
        text: {
          zh: "喜欢衣织的综艺感，歌声和没有架子的感觉。（还有笑声真的非常搞笑）",
          jp: "衣織のバラエティ感、歌声、それから気取っていない雰囲気が好き。（あと笑い声が本当に面白い）",
        },
      },
    ],
  },
  {
    id: "j-pop",
    name: { zh: "J-POP", jp: "J-POP" },
    entries: [
      {
        title: "YOASOBI",
        imageLabel: "封面预留 01",
        image: yoasobiImage,
        imagePosition: "50% 24%",
        text: {
          zh: "YOASOBI是我接触j-pop的起点，喜欢《夜に駆ける》，《群青》，《祝福》。",
          jp: "YOASOBIは私がJ-POPに触れるきっかけ。『夜に駆ける』『群青』『祝福』が特に好き。",
        },
      },
      {
        title: "Official髭男dism",
        imageLabel: "封面预留 02",
        image: higedanImage,
        imagePosition: "50% 32%",
        mediaClass: "topic-entry-media-wide",
        text: {
          zh: "被聪哥的高音折服，胡男的歌都太对我口味了,每次去ktv都会唱。喜欢《Pretender》,《115万キロのフィルム》，《Subtitle》,《らしさ》。",
          jp: "聡さんの高音に完全にやられた。ヒゲダンの曲はどれも好みに刺さるし、カラオケに行くたびに歌う。『Pretender』『115万キロのフィルム』『Subtitle』『らしさ』が好き。",
        },
      },
      {
        title: "Vaundy",
        imageLabel: "封面预留 03",
        image: vaundyImage,
        imagePosition: "50% 26%",
        text: {
          zh: "怪兽的花呗开始了解到的，被才华震撼了。喜欢《怪獣の花唄》，《裸の勇者》。",
          jp: "『怪獣の花唄』で知って、その才能にかなり衝撃を受けた。『怪獣の花唄』『裸の勇者』が好き。",
        },
      },
      {
        title: "ずっと真夜中でいいのに",
        imageLabel: "封面预留 04",
        image: zutomayoImage,
        imagePosition: "50% 24%",
        text: {
          zh: "好喜欢ACAね的嗓音，真夜中的歌有种特别的感觉。喜欢《秒針を噛む》，《あいつら全員同窓会》，《海馬成長痛》。",
          jp: "ACAねの声が本当に好きで、ずとまよの曲には独特の空気がある。『秒針を噛む』『あいつら全員同窓会』『海馬成長痛』が好き。",
        },
      },
      {
        title: "米津 玄师",
        imageLabel: "封面预留 05",
        image: hachiImage,
        imagePosition: "50% 24%",
        text: {
          zh: "八爷伟大无需多言。喜欢《Lemon》，《打上花火》，《さよーならまたいつか!》，《IRIS OUT》，《JANE DOE》。",
          jp: "ハチはやっぱり別格。『Lemon』『打上花火』『さよーならまたいつか!』『IRIS OUT』『JANE DOE』が好き。",
        },
      },
    ],
  },
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

const locale = ref("zh");
const currentPage = ref("home");
const activeTopicId = ref(topicData[0].id);
const copyStatus = ref(messages.zh.home.copyHint);

const textSet = computed(() => messages[locale.value] ?? messages.zh);

const localizedTopics = computed(() =>
  topicData.map((topic) => ({
    id: topic.id,
    name: topic.name[locale.value] ?? topic.name.zh,
    entries: topic.entries.map((entry) => ({
      ...entry,
      text: entry.text[locale.value] ?? entry.text.zh,
      title: locale.value === "jp" && entry.titleJp ? entry.titleJp : entry.title,
    })),
  })),
);

const activeTopic = computed(
  () => localizedTopics.value.find((topic) => topic.id === activeTopicId.value) ?? localizedTopics.value[0],
);

function syncPageWithPath() {
  const pathname = window.location.pathname.replace(/\/+$/, "");
  const aboutPath = `${baseUrl}about`.replace(/\/+$/, "");
  currentPage.value = pathname === aboutPath ? "about" : "home";
}

function setPage(page) {
  currentPage.value = page;
  const nextPath = page === "about" ? `${baseUrl}about` : baseUrl;
  window.history.pushState({}, "", nextPath);
}

function setLocale(nextLocale) {
  if (!messages[nextLocale]) return;
  locale.value = nextLocale;
  copyStatus.value = messages[nextLocale].home.copyHint;
  window.localStorage.setItem(localeStorageKey, nextLocale);
  document.documentElement.lang = nextLocale === "jp" ? "ja" : "zh-CN";
}

async function copyText(text) {
  try {
    await navigator.clipboard.writeText(text);
    copyStatus.value = `${textSet.value.home.copied}: ${text}`;
  } catch (error) {
    copyStatus.value = `${textSet.value.home.copyFailed}: ${text}`;
  }
}

function setTopic(topicId) {
  activeTopicId.value = topicId;
}

onMounted(() => {
  const savedLocale = window.localStorage.getItem(localeStorageKey);
  if (savedLocale && messages[savedLocale]) {
    locale.value = savedLocale;
    copyStatus.value = messages[savedLocale].home.copyHint;
  }

  document.documentElement.lang = locale.value === "jp" ? "ja" : "zh-CN";
  syncPageWithPath();
  window.addEventListener("popstate", syncPageWithPath);
});

onBeforeUnmount(() => {
  window.removeEventListener("popstate", syncPageWithPath);
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
              {{ textSet.nav.home }}
            </button>
          </li>
          <li>
            <button
              class="nav-link-button"
              :class="{ active: currentPage === 'about' }"
              type="button"
              @click="setPage('about')"
            >
              {{ textSet.nav.about }}
            </button>
          </li>
        </ul>

        <div class="locale-switcher" :aria-label="textSet.localeLabel">
          <button
            class="locale-button"
            :class="{ active: locale === 'zh' }"
            type="button"
            @click="setLocale('zh')"
          >
            {{ textSet.localeOptions.zh }}
          </button>
          <button
            class="locale-button"
            :class="{ active: locale === 'jp' }"
            type="button"
            @click="setLocale('jp')"
          >
            {{ textSet.localeOptions.jp }}
          </button>
        </div>
      </div>
    </nav>

    <main class="main-content">
      <template v-if="currentPage === 'home'">
        <section class="hero container">
          <div class="hero-copy">
            <p class="eyebrow">{{ textSet.home.eyebrow }}</p>
            <h1 class="hero-title" :class="{ 'hero-title-jp': locale === 'jp' }">
              <span
                v-for="line in textSet.home.heroLines"
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
              {{ textSet.home.text }}
            </p>
            <div class="hero-actions">
              <a class="primary-link" :href="`${baseUrl}about`" @click.prevent="setPage('about')">
                {{ textSet.home.more }}
              </a>
            </div>
          </div>

          <div class="profile-card">
            <img class="avatar" :src="profileImage" alt="ブブブＰ 头像" />
            <div class="profile-meta">
              <p class="mini-label">{{ textSet.home.profileLabel }}</p>
              <h2>{{ textSet.home.profileTitle }}</h2>
              <p class="sub">
                {{ textSet.home.profileText }}
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
                v-for="item in textSet.home.copyItems"
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
          <article v-for="card in textSet.home.highlightCards" :key="card.title" class="info-card">
            <p class="mini-label">{{ card.label }}</p>
            <h3>{{ card.title }}</h3>
            <p>{{ card.text }}</p>
          </article>
        </section>
      </template>

      <template v-else>
        <section class="inner-hero container">
          <p class="eyebrow">{{ textSet.about.eyebrow }}</p>
          <h1>{{ textSet.about.title }}</h1>
        </section>

        <section class="content-grid container">
          <article class="content-card" :class="{ 'content-card-jp': locale === 'jp' }">
            <p class="mini-label">{{ textSet.about.introLabel }}</p>
            <h2>{{ textSet.about.introTitle }}</h2>
            <p v-for="paragraph in textSet.about.introParagraphs" :key="paragraph">
              {{ paragraph }}
            </p>
          </article>

          <article class="content-card" :class="{ 'content-card-jp': locale === 'jp' }">
            <p class="mini-label">{{ textSet.about.focusLabel }}</p>
            <h2>{{ textSet.about.focusTitle }}</h2>
            <ul class="feature-list" :class="{ 'feature-list-jp': locale === 'jp' }">
              <li v-for="point in textSet.about.focusPoints" :key="point">{{ point }}</li>
            </ul>
          </article>
        </section>

        <section class="topics-section container">
          <div class="section-heading">
            <p class="mini-label">{{ textSet.about.topicsLabel }}</p>
            <h2>{{ textSet.about.topicsTitle }}</h2>
          </div>

          <div class="topic-switcher" role="tablist" :aria-label="textSet.about.topicSwitcherLabel">
            <button
              v-for="topic in localizedTopics"
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
                <h3>{{ activeTopic.name }}</h3>
                <div class="topic-stack">
                  <div
                    v-for="entry in activeTopic.entries"
                    :key="entry.imageLabel + entry.text"
                    class="topic-entry"
                  >
                    <div class="topic-entry-media" :class="entry.mediaClass">
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
      <p>{{ textSet.footer }}</p>
    </footer>
  </div>
</template>
