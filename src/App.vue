<template>
  <v-app style="background: rgba(0,0,0,0);">
    <!--背景图片-->
    <div id="web_bg" :style="'background-image: url(' + imgUrl + ');'"></div>
    <!--顶部标题栏-->
    <v-app-bar app color="rgba(0,0,0,.2)" dark flat fixed>
      Life In NJU - Lite
      <v-spacer></v-spacer>
      <v-icon
        @click="open('https://github.com/HermitSun/Life-in-NJU')"
        class="mx-3"
      >
        mdi-github
      </v-icon>
      <v-icon
        class="shareLink"
        data-clipboard-text="https://nju.today"
        @click="share"
      >
        mdi-open-in-new
      </v-icon>
    </v-app-bar>
    <v-content class="mt-12 pa-0">
      <v-container class="fill-height" fluid>
        <v-row justify="center" align="center">
          <v-carousel
            v-model="currentPage"
            delimiter-icon="mdi-minus"
            height="100%"
            hide-delimiters
            :show-arrows="false"
            light
          >
            <v-carousel-item>
              <SearchBar />
            </v-carousel-item>
            <v-carousel-item>
              <Menu />
            </v-carousel-item>
          </v-carousel>
        </v-row>
      </v-container>
      <!--分享提示-->
      <v-snackbar v-model="showSnackbar">
        {{ snackText }}
        <v-btn color="pink" text @click="showSnackbar = false">
          Close
        </v-btn>
      </v-snackbar>
    </v-content>
    <!--翻页-->
    <v-footer absolute dark style="background-color: rgba(0, 0, 0, 0.6);">
      <v-row justify="space-around">
        <v-icon @click="--currentPage">mdi-chevron-left</v-icon>
        <v-icon @click="++currentPage">mdi-chevron-right</v-icon>
      </v-row>
    </v-footer>
  </v-app>
</template>

<script>
const dataUrl = "https://image.idealclover.cn/projects/Life-in-NJU/";
const imgUrl =
  dataUrl + "background/bg" + Math.floor(Math.random() * 10) + ".jpg";

let nativeShare;

export default {
  name: "App",
  components: {
    Menu: () => import("./components/Menu"),
    SearchBar: () => import("./components/SearchBar")
  },
  data() {
    return {
      dataUrl: dataUrl,
      imgUrl: imgUrl,
      showSnackbar: false,
      snackText: "",

      currentPage: 0,
      isShareValid: false,
      isCopyValid: false
    };
  },
  mounted() {
    this.loadShare();
  },
  methods: {
    // 打开新页面
    open(link) {
      let tab = window.open(link);
      tab.opener = null;
      tab.location = link;
    },
    // 分享
    share() {
      // 正在加载
      if (this.isShareValid && this.isCopyValid) {
        nativeShare.setShareData({
          icon: "https://nju.today/img/icons/android-chrome-192x192.png",
          link: "https://nju.today",
          title: "南哪指南 - Lite",
          desc: "南哪人的专属导航页！",
          from: "@idealclover"
        });
        // 唤起浏览器原生分享组件(如果在微信中不会唤起，此时call方法只会设置文案。类似setShareData)
        try {
          nativeShare.call();
        } catch (err) {
          this.showToast("链接已复制，快分享给小伙伴吧！");
        }
      } else {
        this.showToast("分享功能正在加载中，请稍后重试");
      }
    },
    // 弹出提示
    showToast(text) {
      this.snackText = text;
      this.showSnackbar = true;
    },
    loadShare() {
      // async load
      const share = document.createElement("script");
      share.src =
        "https://cdn.jsdelivr.net/npm/nativeshare@2.1.3/NativeShare.min.js";
      share.async = true;
      const copy = document.createElement("script");
      copy.src =
        "https://cdn.jsdelivr.net/npm/clipboard@2.0.6/dist/clipboard.min.js";
      copy.async = true;
      share.addEventListener("load", () => {
        // eslint-disable-next-line no-undef
        nativeShare = new NativeShare();
        this.isShareValid = true;
      });
      copy.addEventListener("load", () => {
        // eslint-disable-next-line no-undef
        new ClipboardJS(".shareLink");
        this.isCopyValid = true;
      });
      // apply to html
      document.head.appendChild(share);
      document.head.appendChild(copy);
    }
  }
};
</script>

<style src="./assets/style/main.css"></style>
