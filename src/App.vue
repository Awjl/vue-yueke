<template>
  <div id="app">
    <startAnimation></startAnimation>
    <router-view  v-wechat-title="$route.meta.title" v-if="isRouterAlive">
    </router-view>
    <comment></comment>
  </div>
</template>

<script>
import Comment from "components/comment/comment";
import startAnimation from "base/startAnimation/startAnimation";
import storage from "good-storage";

import { getWechat } from "api/dataList";
import { ERR_OK } from "api/config";

export default {
  provide() {
    return {
      reload: this.reload
    };
  },
  data() {
    return {
      isRouterAlive: true
    };
  },
  created() {
    this._getWechat();
  },
  methods: {
    reload() {
      this.isRouterAlive = false;
      this.$nextTick(function() {
        this.isRouterAlive = true;
      });
    },
    _getWechat() {
      storage.set("_key_", window.location.href.split("=")[1]);
      // storage.set("__userID__", 22);
      getWechat(window.location.href.split('=')[1]).then((res) => {
        if (res.code === ERR_OK) {
          storage.set('__userID__', res.data)
        }
      })
    }
  },
  components: {
    Comment,
    startAnimation
  }
};
</script>

<style lang="scss" rel="stylesheet/scss" scoped>
</style>
