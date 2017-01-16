<template>
    <view class="page__bd page__bd_spacing">
        <view class="weui-cells__title">录入方式</view>
        <view class="weui-cells weui-cells_after-title">
            <view class="weui-cell weui-cell_switch">
                <view class="weui-cell__bd">扫码录入:</view>
                <view class="weui-cell__ft">
                    <switch checked bindchange="screenInput" />
                </view>
            </view>
            <view class="weui-cell weui-cell_switch">
                <view class="weui-cell__bd">连续录入:</view>
                <view class="weui-cell__ft">
                    <switch checked  bindchange="batchAddBook"/>
                </view>
            </view>
        </view>
        <form wx:if="{{isShowInputForm}}">
            <view class="weui-cells__title">图书基本信息:</view>
            <view class="weui-cells weui-cells_after-title">
                <view class="weui-cell weui-cell_input">
                    <view class="weui-cell__hd">
                        <view class="weui-label">书名:</view>
                    </view>
                    <view class="weui-cell__bd">
                        <input class="weui-input" placeholder="请输入书名" />
                    </view>
                </view>

                <view class="weui-cell weui-cell_input">
                    <view class="weui-cell__hd">
                        <view class="weui-label">作者:</view>
                    </view>
                    <view class="weui-cell__bd">
                        <input class="weui-input" placeholder="请输入作者" />
                    </view>
                </view>

                <view class="weui-cell weui-cell_input">
                    <view class="weui-cell__hd">
                        <view class="weui-label">价格:</view>
                    </view>
                    <view class="weui-cell__bd">
                        <input class="weui-input" placeholder="请输入价格" />
                    </view>
                </view>

                <view class="weui-cell weui-cell_input">
                    <view class="weui-cell__hd">
                        <view class="weui-label">出版社:</view>
                    </view>
                    <view class="weui-cell__bd">
                        <input class="weui-input" placeholder="请输入出版社" />
                    </view>
                </view>

                <view class="weui-cell weui-cell_input">
                    <view class="weui-cell__hd">
                        <view class="weui-label">ISBN:</view>
                    </view>
                    <view class="weui-cell__bd">
                        <input class="weui-input" placeholder="请输入ISBN" type="number"/>
                    </view>
                </view>
            </view>
        </form>

        <view class="weui-btn-area">
            <button class="weui-btn" type="primary" bindtap="addBook">开始录入</button>
        </view>
    </view>
</template>

<script>
import wepy from 'wepy';
import Config from '../config/config';

export default class AddBook extends wepy.page {
    config = {
        "navigationBarTitleText": "图书管理系统"
    };

    components = {
    };

    data = {
        isShowInputForm:false, // 是否显示手动输入书本信息的表单
        isBatchAddBook:true, // 是否连续录入书本，如果是，录入完一本后会继续扫描
        book:{},
    };
    methods = {
        //根据扫码录入控制手动输入的form是否显示
        screenInput(e){
            this.isShowInputForm=!e.detail.value;
        },
        batchAddBook(e){
            this.isBatchAddBook=e.detail.value;
        }
    };

    events = {};

    onLoad(){
    }

    async addBook(){
            // 1.调用摄像头扫描获得ISBN13
            let scanRes = await wx.scanCode();
            let isbn13=scanRes.result;
            // 2.根据ISBN检查数据库是否存在这本书，有则数量+1，否则进行第三步
            let op1 = {
                url: Config.clubApi.get,
                data: {
                    appkey: Config.appKey,
                    key: isbn13,
                    type: 'bookLibrary'
                },
            };
            let result = await wx.request(op1);
            let bookHasAlreadyExist =false;
            try{
                this.book = result.data.result.value;
                this.book.qty+=1;
                bookHasAlreadyExist=true;
                console.log(this.book);
            }
            catch(err){
                this.book={};
                bookHasAlreadyExist=false;
            }

            // 3.从豆瓣获取书籍信息，并增加qty属性，值为1
            if(!bookHasAlreadyExist){
                let option = {
                    url:Config.doubanUrl+isbn13,
                };
                let res = await wx.request(option);
                let bookMsg=res.data;
                bookMsg.qty=1;
                this.book=bookMsg;
            }
            // 4.把this.book覆盖到数据库
            let options = {
                url: Config.clubApi.put,
                method: 'POST',
                header: {
                    'content-type': 'application/x-www-form-urlencoded'
                },
                data: {
                    appkey: Config.appKey,
                    key: isbn13,
                    type: 'bookLibrary',
                    value: JSON.stringify(this.book),
                },
            };
            let books = await wx.request(options);
            // 5.弹出提示框，返回上一页 或者继续录入
            wx.showToast({
                title: '已成功录入书籍信息',
                icon: 'success',
                duration: 1000
            });
            setTimeout(()=>{
                console.log(this.isBatchAddBook);
                if(this.isBatchAddBook){
                    this.addBook();
                }else{
                    wx.switchTab({
                    url: '/pages/index'
                    });
                }

            },1000);


        }


}
</script>

<style lang="less">
</style>