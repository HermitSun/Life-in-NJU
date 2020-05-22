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
      @click:append="search"
      @keyup.enter="search"
      style="width: 80%; flex-grow: 0;"
    >
      <template #append>
        <img
          src="@/assets/icon/search.svg"
          alt="search-icon"
          style="zoom: 125%"
        />
      </template>
    </v-text-field>
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
    }
  }
};
</script>
