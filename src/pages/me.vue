<template>
    <view class="me-wrapper">
    <view  bindtap="bindViewTap" class="userinfo">
        <image class="userinfo-avatar" src="{{userInfo.avatarUrl}}" background-size="cover"></image>
        <text class="userinfo-nickname">{{userInfo.nickName}}</text>
    </view>
    </view>

    <view class="space-line"></view>

    <view class="weui-cells weui-cells_after-title">
    <navigator wx:for="{{meList}}" wx:key="text" url="{{item.url}}" class="weui-cell weui-cell_access" hover-class="weui-cell_active" >
        <view class="weui-cell__hd">
            <image src="{{item.icon}}" style="margin-right: 5px;vertical-align: middle;width:20px; height: 20px;"></image>
        </view>
        <view class="weui-cell__bd" style="padding-top:10rpx;">{{item.text}}</view>
        <view  class="badge" wx:if="{{showBadge}}">1</view>
        <view class="weui-cell__ft weui-cell__ft_in-access"></view>
    </navigator>
    </view>
</template>

<script>
    import wepy from 'wepy';

  export default class Me extends wepy.component {
    config = {
      "navigationBarTitleText": "我"
    };

    components = {};

    data = {
      userInfo: {},
      showBadge: false,
      meList: [{
        text: '借入的图书',
        icon: '../images/iconfont-dingdan.png',
        url: '/index'
      }, {
        text: '借出的图书',
        icon: '../images/iconfont-help.png',
        url: ''
      }, {
        text: '预约的图书',
        icon: '../images/iconfont-icontuan.png',
        url: ''
      }, {
        text: '飘流的图书',
        icon: '../images/iconfont-kefu.png',
        url: ''
      }, {
        text: '曾经拥有的图书',
        icon: '../images/iconfont-tuihuo.png',
        url: ''
      } ]
    };
    methods = {};

    globalData = {
      userInfo: null
    };

    async onLoad() {
      if (this.globalData.userInfo) {
        this.userInfo = this.globalData.userInfo;
      }
      let x = await wx.login();
      let res = await wx.getUserInfo();
      this.globalData.userInfo = res.userInfo;
      this.userInfo = res.userInfo;
      this.$apply();
    }

  }
</script>

<style lang="less">
.userinfo {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.userinfo-avatar {
  width: 128rpx;
  height: 128rpx;
  margin: 20rpx;
  border-radius: 50%;
}

.userinfo-nickname {
  color: #aaa;
}

.usermotto {
  margin-top: 200px;
}

.space-line{
    display: block;
    height: 70rpx;
    width: 100%;
}

.badge{
  position: absolute;
  top: 36rpx;
  right: 80rpx;
  width: 30rpx;
  height: 30rpx;
  line-height: 30rpx;
  background: #ff0000;
  color: #fff;
  border-radius: 50%;
  text-align: center;
  font-size: 25rpx;
}
</style>