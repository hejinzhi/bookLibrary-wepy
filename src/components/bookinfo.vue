<style lang="less">
.book-info{
    height:100%;
    .scroll-content{
    }

    .book-intro{
    width: 95%;
    }

    .book-title{
    display: block;
    font-size: 14px;
    background-color: #E5E5E5;
    color: #323232;
    height: 100%;
    border-left: 1px solid red;
    border-bottom: 1px solid #E5E5E5;
    width: 720rpx;
    }

    .space-line-30{
    display: block;
    height: 30px;
    }

    .book-msg{
        font-size: 12px;
        line-height:18px;
        text-indent: 50rpx;
        color: #323232;
        width: 40%;
    }
}



</style>

<template>
<view class="book-info">
    <scroll-view scroll-y="true" style="height:{{height}}px;" >
        <view class="book-intro">
            <text class="book-title">内容推荐</text>
            <text class="book-msg">{{books.summary}}</text>
        </view>
        <view class="space-line-30"></view>
        <view class="book-intro">
            <text class="book-title">作者简介</text>
            <text class="book-msg">{{books.author_intro}}</text>
        </view>
        <view class="space-line-30"></view>
        <view class="book-intro">
            <text class="book-title">目录</text>
            <text class="book-msg">{{books.catalog}}</text>
        </view>
    </scroll-view>
</view>

</template>

<script>
import wepy from 'wepy';

export default class BookInfo extends wepy.component {
    props = {
        books: {},
        height:Number,
    };

    data = {
    };
    events = {};

    methods = {
    };

    onLoad() {
        // console.log(this.height,'a');
    }
}

</script>
