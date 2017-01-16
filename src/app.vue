<style lang="less">
@import 'weui.wxss';
/**app.wxss**/
.container {
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
    box-sizing: border-box;
}

page {
    height: 100%;
    background-color: #fbf9fe;
    font-size: 12px;
}
</style>

<script>
import wepy from 'wepy';
import "babel-polyfill";

export default class extends wepy.app {

    config = {
        "pages": [
            "pages/index",
            "pages/bookdetail",
            "pages/addbook",
            "pages/me"
        ],
        "tabBar": {
            "list": [{
                "iconPath": "images/search-1.png",
                "selectedIconPath": "images/search.png",
                "pagePath": "pages/index",
                "text": "图书查询"
            }, {
                "iconPath": "images/dy-1.png",
                "selectedIconPath": "images/dy.png",
                "pagePath": "pages/addbook",
                "text": "图书录入"
            }, {
                "iconPath": "images/tj-1.png",
                "selectedIconPath": "images/tj.png",
                "pagePath": "pages/me",
                "text": "我"
            }]
        },
        "window": {
            "backgroundTextStyle": "black",
            "navigationBarBackgroundColor": "#fff",
            "navigationBarTitleText": "图书管理系统",
            "navigationBarTextStyle": "black"
        }
    };

    globalData = {
        userInfo: null
    };

    onLaunch() {
        // console.log(this);
    }

    // async getUserInfo() {
    //     if (this.globalData.userInfo) {
    //         return this.globalData.userInfo;
    //     }
    //     let x = await wx.login();
    //     let res = await wx.getUserInfo();
    //     this.globalData.userInfo = res.userInfo;
    //     return res.userInfo;
    // }
}

</script>
