<template>
<view class="main-wrapper">
    <view class="book-list-wrapper">
        <component id="bookList" :books.sync="book"></component>
    </view>

    <view class="book-detail-wrapper">
        <component id="bookInfo" :books.sync="bookInfo" :height.sync="windowHeight"></component>
    </view>

    <view class="control-btn-wrapper">
        <button type="primary" wx:if="{{bookInfo.qty > 0}}" bindtap="borrowBook"> 借阅 </button>
        <button type="primary" wx:if="{{bookInfo.qty === 0}}"> 预约 </button>
        <button type="primary" wx:if="{{showAddBook}}"> 录入 </button>
    </view>
</view>

</template>

<script>
import wepy from 'wepy';
import BookList from '../components/booklist';
import BookInfo from '../components/bookinfo';
import Config from '../config/config';

export default class BookDetail extends wepy.page {
    config = {
        "navigationBarTitleText": "图书管理系统"
    };
    components = {
        bookList: BookList,
        bookInfo: BookInfo,
    };

    data = {
        book: [],
        bookInfo: {},
        windowHeight: 0,
        showBorrowBtn: true, //是否显示 借阅 按钮
        showBookBtn: false, //是否显示 预约 按钮
        showAddBook: false, //是否显示 录入 按钮
    };
    methods = {
        async borrowBook() {
            // 1.检查这个user是否曾经借过书
            let empno = 'FE717';
            var option1 = {
                url: Config.clubApi.get,
                data: {
                    appkey: Config.appKey,
                    key: empno,
                    type: 'bookBorrow'
                }
            };
            let borrowList;
            let res = await wx.request(option1);
            try {
                borrowList = res.data.result.value;
            } catch (err) {
                borrowList = [];
            }
            let book = {
                isbn13: this.bookInfo.isbn13,
                borrowDate: new Date().getTime()
            }
            borrowList.push(book);
            // 2.把借书的列表保存到数据库
            let borrowOptions = {
                url: Config.clubApi.put,
                data: {
                    appkey: Config.appKey,
                    type: 'bookBorrow',
                    key: empno,
                    value: borrowList
                }
            }
            let borrowRes = await wx.request(borrowOptions);
            // 3.如果数据库正常保存，则更新该书本的qty=qty-1
            let resEmpno;
            try {
                resEmpno = borrowRes.data.result;
            } catch (err) {
                resEmpno = '';
            }
            if (resEmpno === empno) {
                this.bookInfo.qty-=1;
                let options = {
                    url: Config.clubApi.put,
                    method: 'POST',
                    header: {
                        'content-type': 'application/x-www-form-urlencoded' //'application/json'
                    },
                    data: {
                        appkey: Config.appKey,
                        type: 'bookLibrary',
                        key: this.bookInfo.isbn13,
                        value: JSON.stringify(this.bookInfo),
                        columns: ['id', 'isbn13', 'title']
                    }
                };
                let saveRes = await wx.request(options);
                if (saveRes.data.success) {
                    wx.showToast({
                        title: '已成功借阅书本',
                        icon: 'success',
                        duration: 1000
                    });
                    setTimeout(() => {
                        // wx.switchTab({
                        //     url: '/pages/index'
                        // });
                        wx.navigateBack();
                    }, 1000);
                }

            }


        },
    };

    events = {};

    async onLoad(options) {
        this.book = [];
        this.book.push(JSON.parse(options.book));
        this.bookInfo = JSON.parse(options.book);
        if (this.windowHeight === 0) {
            let res = await wx.getSystemInfo();
            this.windowHeight = res.windowHeight - 66 - 136 - 10;
            this.$apply();
        }
    }
};
</script>

<style lang="less">
.main-wrapper
{
    display:flex;
    flex-direction:column;
    flex-flow: column;
    align-items: center;
    margin-left:10px;
    margin-right:10px;
    height:100%;
}

.book-list-wrapper{
    height:136px;

}

.book-detail-wrapper{
    position:fixed;
    left: 15rpx;
    top: 300rpx;
    flex:1;
}

.control-btn-wrapper{
    position: fixed;
    bottom: 0;
    width: 95%;
    height:66px;
    button{
        margin-top: 20rpx;
        margin-bottom: 20rpx;
        width: 95%;
    }
}


</style>
