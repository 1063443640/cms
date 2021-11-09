<template>
  <view>
    <u-swiper :list="imageList" mode="none"></u-swiper>
    <div class="u-swiper-title u-line-1" style="padding-bottom: 6px">
      {{ content }}
    </div>
    <view class="form">
      <view class="inputTitle">京东联盟ID</view>
      <u-input
        border
        style="margin-top: 20rpx"
        placeholder="请输入京东联盟ID"
        v-model="unionId"
      ></u-input>
      <u-button type="primary" style="margin: 30rpx 0" @click="generated"
        >一键生成{{ cms.cms_title || " " }}推广页面链接</u-button
      >
      <view class="inputTitle">推广页面链接（点击可复制）</view>
      <u-input
        border
        v-model="url"
        style="margin-top: 20rpx"
        placeholder="推广页面链接"
        disabled
      ></u-input>
      <view class="cms">
        <u-waterfall v-model="cmsList">
          <template v-slot:left="{ leftList }">
            <view
              v-for="(item, index) in leftList"
              :key="index"
              :class="active == item.id ? 'warter-active' : 'warter'"
              @click="choose(item)"
            >
              <u-lazy-load
                threshold="-450"
                border-radius="10"
                :image="item.cms_banner"
                :index="index"
              ></u-lazy-load>
              <view class="title">
                {{ item.cms_title }}
              </view>
              <u-alert-tips
                class="tips"
                type="primary"
                :description="item.cms_Introduction"
              ></u-alert-tips>
            </view>
          </template>
          <template v-slot:right="{ rightList }">
            <view
              v-for="(item, index) in rightList"
              :key="index"
              :class="active == item.id ? 'warter-active' : 'warter'"
              @click="choose(item)"
            >
              <u-lazy-load
                threshold="-450"
                border-radius="10"
                :image="item.cms_banner"
                :index="index"
              ></u-lazy-load>
              <view class="title">
                {{ item.cms_title }}
              </view>
              <u-alert-tips
                class="tips"
                type="primary"
                :description="item.cms_Introduction"
              ></u-alert-tips>
            </view>
          </template>
        </u-waterfall>
      </view>
      <view style="height: 20rpx"></view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      active: 0,
      unionId: "",
      content: "MK 一键生成工具 公告内容",
      imageList: [
        {
          image:
            "https://cbgworkplacea0.test.huawei.com/s3/fbb0a157eb5b301ad76a373e4f851b22.png",
        },
      ],
      cms: {},
      url: "",
      cmsList: [
        {
          id: 1,
          cms_title: "京东礼金",
          cms_banner:
            "http://12345.huawei.com/unidesk/servicemgt/services/file/fileService/download?fileId=7018C46A0EA0439A9D1757F17529A5B2.png",
          cms_Introduction: "京东礼金简介哈哈哈哈哈哈嘎嘎嘎阿尔法额愤愤地说",
          cms_url: "http://zss.mkstone.club/goods/",
        },
        {
          id: 2,
          cms_title: "淘宝礼金",
          cms_banner:
            "http://12345.huawei.com/unidesk/servicemgt/services/file/fileService/download?fileId=64FDEE9FAADA4DEAB69DC26394166404.png",
          cms_Introduction: "淘宝礼金简介哈哈哈哈哈哈嘎嘎嘎阿尔法额愤愤地说",
          cms_url: "http://zss.mkstone.club/goods/",
        },
        {
          id: 3,
          cms_title: "拼多多礼金",
          cms_banner:
            "http://12345.huawei.com/unidesk/servicemgt/services/file/fileService/download?fileId=CBBCD615A2784F83B8E3EA9AB9B4BA4C.png",
          cms_Introduction: "拼多多礼金简介哈哈哈哈哈哈嘎嘎嘎阿尔法额愤愤地说",
          cms_url: "http://zss.mkstone.club/goods/",
        },
      ],
    };
  },
  created() {
    this.unionId = sessionStorage.getItem("unionId", this.unionId) || "";
  },
  methods: {
    choose(item) {
      this.active = item.id;
      this.cms = item;
      this.url = "";
    },
    generated() {
      if (!this.unionId) {
        alert("京东联盟ID不能为空");
        return false;
      }
      if (!this.cms.cms_title) {
        alert("请在下面选择礼金类型");
        return false;
      }
      let url = `${this.cms.cms_url}${this.unionId}`;
      sessionStorage.setItem("unionId", this.unionId);
      this.url = url;
    },
  },
};
</script>

<style lang="scss">
.u-alert-tips {
  padding: 8px;
}
.u-swiper-title {
  position: relative;
}
.form {
  width: 95%;
  margin: 30rpx auto;
  .inputTitle {
    font-size: 30rpx;
    font-weight: bold;
  }
}
.u-swiper-title {
  background-color: rgba(0, 0, 0, 0.3);
  bottom: 0;
  left: 0;
  width: 100%;
  font-size: 14px;
  padding: 6px 12px;
  color: rgba(255, 255, 255, 0.9);
}
.u-line-1 {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
.cms {
  margin-top: 30rpx;
  margin-bottom: 30rpx;
  background-color: rgb(240, 240, 240);
  .warter {
    border-radius: 16rpx;
    margin: 10rpx;
    background-color: #fff;
    padding: 16rpx;
    position: relative;
    &-active {
      border-radius: 16rpx;
      margin: 10rpx;
      background-color: #fff;
      padding: 16rpx;
      position: relative;
      border: 1px #3c9cff solid;
    }
  }
  .title {
    font-size: 28rpx;
    font-weight: bold;
    text-align: center;
    margin-top: 10rpx;
    color: $u-main-color;
  }
  .tips {
    margin-top: 10rpx;
    border: 0;
  }
}
</style>
