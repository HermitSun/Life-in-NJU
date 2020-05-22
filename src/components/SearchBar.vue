<template>
  <v-row justify="center" align="center" class="flex-column flex-nowrap">
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
      v-model="keyword"
      append-icon="mdi-magnify"
      @click:append="search"
      @keyup.enter="search"
      style="width: 80%; flex-grow: 0"
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
          <i class="iconfont">&#xe6b6;</i>
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
          <i class="iconfont">&#xe63f;</i>
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
          <i class="iconfont">&#xe719;</i>
        </v-btn>
      </div>
    </v-row>
  </v-row>
</template>

<script>
const dataUrl = "https://image.idealclover.cn/projects/Life-in-NJU/";

export default {
  name: "SearchBar",
  data() {
    return {
      keyword: "",
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
    open(link) {
      let tab = window.open(link);
      tab.opener = null;
      tab.location = link;
    },
    // 搜索
    search() {
      this.open(this.engineList[this.engineIndex].url + this.keyword);
      this.keyword = "";
    },
    changeEngine(num) {
      this.engineIndex = num;
      localStorage.setItem("engineIndex", this.engineIndex);
    },
    nextEngine() {
      this.changeEngine((this.engineIndex + 1) % this.engineList.length);
    }
  }
};
</script>

<style scoped src="@/assets/style/search.css"></style>
