<template>
  <div>
    <div>
      <van-nav-bar
        title="xxx"
        left-text=""
        right-text="新增"
        left-arrow
        @click-left="onClickLeft"
        @click-right="onClickRight"
      />
    </div>
    <div>
      <van-pull-refresh v-model="refreshing" @refresh="onRefresh">
        <van-list
          v-model="loading"
          :finished="finished"
          finished-text="没有更多了"
          @load="onLoad"
        >
          <van-swipe-cell v-for="item in list" :key="item.index">
            <van-cell
              is-link
              :key="item.index"
              icon="location-o"
              :value="item.value"
              :label="item.label"
              :class="{
                'van-cell__value1': item.type == 1,
                'van-cell__value2': item.type == 2,
              }"
            >
              <template slot="title">
                <span class="custom-title">{{ item.title }}</span>
                <van-tag type="danger">标签</van-tag>
              </template>
              <van-icon
                slot="right-icon"
                name="chat-o"
                :dot="bool"
                style="line-height: inherit"
                size="30px"
                :badge="item.type"
              >
              </van-icon>
            </van-cell>
            <template #right>
              <van-button square type="primary" text="修改" />
              <van-button square type="danger" text="删除" />
            </template>
            <template #left>
              <van-button square type="primary" text="修改" />
              <van-button square type="danger" text="删除" />
            </template>
          </van-swipe-cell>
        </van-list>
      </van-pull-refresh>
    </div>
  </div>
</template>
<script>
import { Toast } from 'vant'
import VueEvent from '../model/VueEvent.js'
export default {
  data () {
    return {
      checked: true,
      radio: '1',
      list: [],
      loading: false,
      finished: false,
      refreshing: false,
      type: 0,
      bool: false
    }
  },
  methods: {
    onClickLeft () {
      Toast('返回')
    },
    onClickRight () {
      Toast('按钮')
    },
    onLoad () {
      setTimeout(() => {
        if (this.refreshing) {
          // this.list = [];
          this.refreshing = false
        }

        for (let i = 0; i < 10; i++) {
          // eslint-disable-next-line eqeqeq
          if (i % 4 == 1) {
            this.list.push({'title': '明智' + i, 'label': '2019-03-19~2019-03-19', 'value': '完成', 'index': Math.random(), 'type': '1'})
          // eslint-disable-next-line eqeqeq
          } else if (i % 4 == 2) {
            this.list.push({'title': '明智' + i, 'label': '2019-03-19~2019-03-19', 'value': '1/30', 'index': Math.random(), 'type': '2'})
          }
        }
        this.loading = false

        if (this.list.length >= 40) {
          this.finished = true
        }
      }, 1000)
    },
    onRefresh () {
      // 清空列表数据
      this.finished = false

      // 重新加载数据
      // 将 loading 设置为 true，表示处于加载状态
      this.loading = true
      this.onLoad()
    }
  },
  mounted () {
    VueEvent.$on('home', function (data) {
      console.log('msg: ' + data)
    })
  }
}
</script>
<style>
.van-nav-bar__text {
  margin: 0 auto;
  font-weight: 500;
  font-size: 16px;
}

.van-cell__value1 .van-cell__value {
  color: #abed4b;
  font-size: 30px;
}
.van-cell__value2 .van-cell__value {
  color: red;
  font-size: 30px;
}
.custom-title {
  font-size: 30px;
}
</style>
