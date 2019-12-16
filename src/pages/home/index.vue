<template>
  <div>
    <div class="top">
      <span class="top_name">豆瓣</span>
      <icon class="top_icon"
            type="search"
            color='#20c93c'></icon>
      <button class="btn"
              size="mini">打开豆瓣App</button>
    </div>
    <div class="hotmovie">
      <span>影院热映</span>
      <span class="more">更多</span>
    </div>
    <div class="swiper">
      <scroll-view class="scroll-view_H"
                   scroll-x="true"
                   bindscroll=false>
        <view v-for="item in moverList"
              :key='item.id'
              id="demo1"
              class="scroll-view-item_H demo-text-1">
          <img :src="item.images.large"
               alt="">
          <p>{{ item.title }}</p>
          <img v-for="(items,i) in newMovieDate[index]" :key='items' class="stars"
               src="../../image/star.svg"
               alt="">
          <span>{{ item.rating.average ? item.rating.average : '暂无评分' }}</span>
        </view>
      </scroll-view>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      moverList: [],
      newMovieDate: []
    }
  },
  onLoad () {
    wx.request({
      url: 'https://api.douban.com/v2/movie/in_theaters?apikey=0df993c66c0c636e29ecbb5344252a4a',
      method: 'GET',
      header: {
        'Content-Type': 'application/x-www-form-urlencoded'
      },
      success: res => {
        console.log(res.data.subjects)
        let movieData = res.data.subjects
        this.moverList = movieData
        this.newMovieDate = movieData.map((item, index) => {
          // console.log(item.rating.average)
          // console.log('============')
          console.log(parseInt((Math.ceil(item.rating.average)) / 2))
          return parseInt((Math.ceil(item.rating.average)) / 2)
        })
      }
    })
  }
  // methods: {
  //   getData () {
  //     wx.request({
  //       url: 'https://api.douban.com/v2/movie/in_theaters?apikey=0df993c66c0c636e29ecbb5344252a4a',
  //       method: 'GET',
  //       header: {
  //         'Content-Type': 'application/x-www-form-urlencoded'
  //       },
  //       success: res => {
  //         console.log(res.data.subjects)
  //         this.moverList = res.data.subjects
  //       }
  //     })
  //   }
  // },
  // created () {
  //   this.getData()
  // }
}
</script>

<style lang='less'>
.top {
  display: flex;
  height: 94rpx;
  border-bottom: 1px solid #ccc;
  align-items: center;
  padding: 0 30rpx;
  box-shadow: 0px 0px 3px 2px #eee;
  .top_name {
    font-size: 50rpx;
    color: #20c93c;
    font-weight: 900;
  }
  .top_icon {
    flex: 1;
    margin-left: 18px;
  }
  .btn {
    height: 58rpx;
    background-color: #20c93c;
    color: white;
    line-height: 58rpx;
    font-size: 14px;
  }
}
.hotmovie {
  width: 100%;
  display: flex;
  justify-content: space-between;
  padding-left: 30rpx;
  height: 88rpx;
  line-height: 88rpx;
  .more {
    color: #20c93c;
    margin-right: 30px;
  }
}
// 轮播样式
.scroll-view_H {
  display: flex;
  white-space: nowrap;
  margin-top: 12rpx;
  height: 400rpx;
}
.scroll-view-item_H {
  // background-color: #20c93c;
  display: inline-block;
  margin-right: 12rpx;
  text-align: center;
  img {
    width: 200rpx;
    height: 286rpx;
  }
  p {
    width: 200rpx;
    font-size: 30rpx;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  .stars {
    width: 20rpx;
    height: 20rpx;
  }
  span {
    font-size: 20rpx;
    }
}
</style>