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
        <v-row
          justify="center"
          align="center"
          class="flex-column flex-nowrap"
          style="height: 100vh"
        >
          <!--切换搜索引擎的图标-->
          <img
            class="logo-item"
            :src="engineList[engineIndex].src"
            @click="nextEngine"
            alt="logo"
          />
          <!--搜索输入框-->
          <v-text-field
            flat
            solo
            class="mt-10"
            label="Search"
            v-model="value"
            append-icon="search"
            @click:append="search"
            @keyup.enter="search"
            style="width: 80%; flex-grow: 0;"
          />
          <!--切换搜索引擎的按钮-->
          <v-row class="mt-10 mb-10" style="flex-grow: 0;">
            <!--TODO: 消除重复代码-->
            <div class="text-center mx-4">
              <v-btn
                depressed
                rounded
                color="rgba(0,0,0,.2)"
                dark
                class="searchOpt"
                @click="changeEngine(0)"
              >
                <i class="iconfont" id="iconBaidu">&#xe6b6;</i>
              </v-btn>
            </div>
            <div class="text-center mx-4">
              <v-btn
                depressed
                rounded
                color="rgba(0,0,0,.2)"
                dark
                class="searchOpt"
                @click="changeEngine(1)"
              >
                <i class="iconfont" id="iconBing">&#xe63f;</i>
              </v-btn>
            </div>
            <div class="text-center mx-4">
              <v-btn
                depressed
                rounded
                color="rgba(0,0,0,.2)"
                dark
                class="searchOpt"
                @click="changeEngine(2)"
              >
                <i class="iconfont" id="iconGoogle">&#xe719;</i>
              </v-btn>
            </div>
          </v-row>
        </v-row>
        <!--其他功能-->
        <v-expansion-panels multiple>
          <v-expansion-panel
            v-for="(category, i) in data"
            :key="i"
            class="panels"
          >
            <v-expansion-panel-header>
              {{ category.title }}
            </v-expansion-panel-header>
            <v-expansion-panel-content>
              <v-row>
                <v-col
                  class="pa-1 d-flex flex-column"
                  cols="12"
                  lg="3"
                  v-for="(item, i) in category.items"
                  :key="i"
                >
                  <v-card
                    outlined
                    tile
                    @click="item.link === '' ? null : open(item.link)"
                    class="panel flex d-flex flex-column"
                    :title="item.description"
                  >
                    <v-card-text>
                      <p class="ma-0 font-weight-black">
                        {{ item.name }}
                      </p>
                      <p class="ma-0 flex">
                        {{ item.description }}
                      </p>
                    </v-card-text>
                  </v-card>
                </v-col>
              </v-row>
            </v-expansion-panel-content>
          </v-expansion-panel>
        </v-expansion-panels>
      </v-container>
      <!--分享提示-->
      <v-snackbar v-model="showSnackbar">
        {{ snackText }}
        <v-btn color="pink" text @click="showSnackbar = false">
          Close
        </v-btn>
      </v-snackbar>
    </v-content>
  </v-app>
</template>

<script>
import NativeShare from "nativeshare";
import Clipboard from "clipboard";
import data from "@/assets/data.json";

const dataUrl = "https://image.idealclover.cn/projects/Life-in-NJU/";
const imgUrl =
  dataUrl + "background/bg" + Math.floor(Math.random() * 10) + ".jpg";

new Clipboard(".shareLink");
let nativeShare = new NativeShare();

export default {
  name: "App",
  data() {
    return {
      data,
      value: "",
      dataUrl: dataUrl,
      imgUrl: imgUrl,
      showSnackbar: false,
      snackText: "",
      engineIndex: localStorage.getItem("engineIndex") || 0,
      engineList: [
        {
          name: "baidu",
          url: "https://www.baidu.com/s?wd=",
          src: dataUrl + "search/baidu-white.png"
        },
        {
          name: "bing",
          url: "https://cn.bing.com/search?q=",
          src: dataUrl + "search/bing-white.png"
        },
        {
          name: "google",
          url: "https://www.google.com/search?q=",
          src: dataUrl + "search/google-white.png"
        }
      ]
    };
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
      nativeShare.setShareData({
        icon: "https://nju.today/img/icons/android-chrome-192x192.png",
        link: "https://nju.today",
        title: "南哪指南",
        desc: "南哪人的专属导航页！",
        from: "@idealclover"
      });
      // 唤起浏览器原生分享组件(如果在微信中不会唤起，此时call方法只会设置文案。类似setShareData)
      try {
        nativeShare.call();
      } catch (err) {
        this.showToast("链接已复制，快分享给小伙伴吧！");
      }
    },
    // 搜索
    search() {
      let value = this.value;
      this.value = "";
      this.open(this.engineList[this.engineIndex].url + value);
    },
    changeEngine(num) {
      this.engineIndex = num;
      localStorage.setItem("engineIndex", this.engineIndex);
    },
    nextEngine() {
      this.changeEngine((this.engineIndex + 1) % this.engineList.length);
    },
    // 弹出提示
    showToast(text) {
      this.snackText = text;
      this.showSnackbar = true;
    }
  }
};
</script>

<style src="./assets/style/main.css"></style>
