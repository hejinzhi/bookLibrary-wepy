<style lang="less">
.book-content{
    .book-list {
    display: flex;
    // width: 93.3%;
    flex-direction: row;
    align-items: center;
    box-sizing: border-box;
    margin-top: 20rpx;
    background-color: #fff;
    }

    .book-image {
        flex: 0 0 32%;
        box-sizing: border-box;
        min-height: 242rpx;
    }

    .book-image image {

        height: 242rpx;
        width: 242rpx;
    }

    .book-info {
        background-color: #fff;
        flex: 0 0 66%;
        box-sizing: border-box;
        // height: 242rpx;
        margin-left: 20rpx;
    }

    .book-info-style {
        height: 240rpx;
        background-color: white;
    }

    .book-info-style view {
        display: block;
        line-height: 42rpx;
        font-size: 12px;
        margin-bottom: 8rpx;
        width: 460rpx;
        text-overflow: ellipsis;
        overflow: hidden;
        white-space: nowrap;
        margin-top: 8rpx;
    }

    .line {
        width: 100%;
        border: 1px solid #eee;
    }
}
</style>

<template>
<view class="book-content">
    <view wx:for="{{books}}" wx:key="isbn13" id="{{item.isbn13}}" data-qty="{{item.qty}}" bindtap="goToDetailPage({{item}})">
        <view class="book-list">
            <view class="book-image">
                <image src="{{item.image}}" mode="aspectFit"></image>
            </view>
            <view class="book-info">
                <view class="book-info-style">
                    <view>书名:{{item.title}}</view>
                    <view>作者:{{item.author}}\n</view>
                    <view>价格:{{item.price}}\n</view>
                    <view>出版信息:{{item.publisher}}\n</view>
                    <view>可借数量:{{item.qty}}\n</view>
                </view>
            </view>
        </view>
        <view class="line"></view>
    </view>
</view>

</template>

<script>
import wepy from 'wepy';

export default class BookList extends wepy.component {
    props = {
        books: {},
    };

    data = {};
    events = {};

    methods = {
        goToDetailPage(e, item) {
            let book = e.currentTarget.dataset.wepyParams;
            this.$emit('goToDetailPage', book);
        },
    };

    onLoad() {
    }
}

</script>
