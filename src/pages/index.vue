<style lang="less">
.main-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    height: 100%;
}

.search-bar-wrapper {
    height: 90rpx;
    width: 100%;
    background-color: #FBF9FE;
    position: fixed;
}

.book-list-wrapper {
    margin-top: 90rpx;
}
</style>

<template>
<view class="main-wrapper">
    <view class="search-bar-wrapper">
        <component id="searchBar"></component>
    </view>

    <view class="book-list-wrapper">
        <component id="bookList" :books.sync="books"></component>
    </view>
</view>

</template>

<script>
import wepy from 'wepy';
import SearchBar from '../components/searchbar';
import BookList from '../components/booklist';
import Config from '../config/config';

export default class Index extends wepy.page {

    config = {
        "navigationBarTitleText": "图书管理系统"
    };
    components = {
        searchBar: SearchBar,
        bookList: BookList,
    };

    data = {
        books: [],
        booksCopy: [],
        searchFilter: '',
    };
    methods = {};

    events = {
        // 监听searchbar emit出来的事件，从而获得searchbar的输入
        'inputChange': ($event, ...args) => {
            this.searchFilter = args[0];
            let filterBooks = [];
            if (this.searchFilter.trim().length === 0) {
                filterBooks = this.booksCopy;
            } else {
                this.books.forEach((book) => {
                    if (this.searchFilter.trim() &&
                        book.title.toUpperCase().indexOf(this.searchFilter.toUpperCase()) >= 0) {
                        filterBooks.push(book);
                    }
                });
            }
            this.books = filterBooks;
            this.$apply();
        },
        // 点击book list组件时候转跳到明细页面，book list组件把被点击的book传递出来
        'goToDetailPage': ($event, ...args) => {
            let book = args[0];
            wx.navigateTo({
                url: 'bookdetail?book=' + JSON.stringify(book),
            });
        },
    };
    async onShow() {
        var op1 = {
            url: Config.clubApi.list,
            data: {
                appkey: Config.appKey,
                type: 'bookLibrary'
            }
        };
        let res = await wx.request(op1);
        let clubBooks = res.data.result;
        let clubBooksArray = [];
        if (clubBooks.length > 0) {
            try{
                clubBooks.forEach((clubBook) => {
                    clubBooksArray.push(clubBook.value);
                });
                this.books = clubBooksArray;
                this.booksCopy = this.books;
            }catch(err){
                this.books=[];
                this.booksCopy=[];
            }
        }
        this.$apply();
    }
}

</script>