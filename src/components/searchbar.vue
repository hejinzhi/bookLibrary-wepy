<style lang="less">
.search-bar {
    z-index: 1;
}

.search-bar button {
    height: 70rpx;
    line-height: 70rpx;
    width: 140rpx;
    position: fixed;
    top: 20rpx;
    left: 585rpx;
    font-size: 30rpx;
    color: #FBF9FE;
}

.search-bar input {
    background-color: #fff;
    border-radius: 5px;
    width: 550rpx;
    height: 70rpx;
    margin-top: 20rpx;
    // text-indent: 70rpx;
    margin-bottom: 15rpx;
    position: fixed;
    top: 0;
    left: 25rpx;

}

.input-placeholder {
    font-size: 30rpx;
    // background: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAYAAADgdz34AAAEpElEQVRIiZVWX2hTdxT+zk1aahvKtU3TNWuLQoky4h9GRSyma3UkVAbWYclKIwqZIzjYyzCCTHwIOvoknRQf5iJTxPhQp1Nah67tsJNGlnbWje5hVEgiITa2uTE1Tdr7O3tYU2JMOvfBfbnfOd93fpfzO+cS1oAQQgbQSUR7AZgB6FeoGIA/mPlnADclSYoX06BCL5m5CsApAJ8KIcqfP38eDoVCwfn5+QQAVFdXy42NjY16vd5IRK8AXATgIaK5tQrOiu9h5meLi4upwcHB27t27XID+KzQY7VaT46MjPy0tLSUFkI8Y+Y9a56AmT9h5u+fPn36d3d396VHjx4l/rMiADabTe/1ep1Go7EewGEi8r1hsFL50MTExG8Wi+VKKpUSbyOehV6vLx0bGztqMpk2E1EHEQ2vGqx88yczMzNxs9l8Pl9clmWtTqfThsPhxbVMampqSqempo7X1tZqiWgLEc1JK9ypdDpd1d3dfSlX/PTp01uCweDxFy9enA8Gg33z8/Nnbty4YausrNQWMpidnc0cO3bsOyGEAf82CYiZZQChoaGh0X379t3OBg8MDNg6Ozs7A4HAwzt37ky8fPky09bWZrJarR9Go9Fgc3NzfywWyxQyGh8f79m5c+f7ABoghDiiqqqa2y0ul6tXVVW1r6/vW+R1TkdHx1cLCwvK/fv3h/K57ONwODxCCGbmIySEuBKNRlvr6urOZCt4/Pixs6ysrGzTpk39hSq8fPnyB3a7/WODwfCloijL+bwkSYjH41/rdLpBiYjMoVAomBtgNBrrA4HAX4XEAeDatWvTpaWlZTabrbYQL4RAJBIJE5FZAqDP3tD8KopBo9GAmcHMRWMURUkws14qFBQOh8M7duwwF0t2OBxbMpnMq7t370aLxRCRBAASEcWqq6vlXLK/v394w4YNm71eryU/0W63v7t///6P0un0YktLi5zPZyHLciURxYiZr8zOzrYZDAZPboDP59vb1dV1cHp6+vd79+5NJJPJTEtLS5PFYmllZpSUlJSl0+nk2bNnvR6P58/cXK1WK8Xj8d6KioofwcxHhBCq1Wo9ibx2c7vd52ZmZqYzmUxaVVU1FotFfD7f9YaGhi8CgcCvQghWVVW9devWDxqNZjXP5XL1rrZp9qKNjo4+bG9vHyh03HXr1kklJSVSIpF4rSWvXr3abrfbD2o0Gu2DBw+GW1tbrwPA5OSkc/v27e8BaJCIKA7g4u7du9tsNpu+kEEqlRL54gDQ09Mz4na7v1EUJXbhwoWHAOB0Ojdu3bq1mZkvElF8ddgx85NIJLK4bdu2c8VGQDFUVFRICwsLoqmpqdzv959Yv369eG3YEdEcER0yGo31Y2NjR2tqakr/j0FWfHh4+POqqqoqIjqU3W6rt2llfh82mUybp6amjh84cOCdtzVwOp0b/X7/ifr6+kZmPpzdBQXBzHuEEM+Wl5eXxsfHf3E4HB5Jkt4YaFqt1uVyuXonJyf9qqqqb7Uyc0xWlz4z65LJ5FwkEgkripIgIkmW5cq6urrG8vLySiJKYo2lX9Agx0gG0AlgLwAzM+uJCMj5bSGimyudWBD/AN/8UMg8sK+tAAAAAElFTkSuQmCC') no-repeat 8rpx center;

}
</style>

<template>
<view class="search-bar">
    <input placeholder="请输入书名" bindinput="inputChange" />
    <button type="primary" bindtap="queryBooks">查詢</button>
</view>

</template>

<script>
import wepy from 'wepy';

export default class SearchBar extends wepy.component {
    props = {};

    data = {};
    events = {
        'index-broadcast': ($event, ...args) => {
            console.log(`${this.name} receive ${$event.name} from ${$event.source.name}`);
        }
    };

    methods = {
        inputChange(e) {
            this.$emit('inputChange', e.detail.value);
        },
    };
}

</script>
