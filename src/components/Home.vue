// 视频4
<template>
  <div class="fillcontain">
    <mt-switch v-model="value"  @change="turn">开关</mt-switch>
    <br />
    <mt-button type="default" @click.native="handleClick($event, '123')" id="1"
      >default</mt-button>
    <br />
    <mt-button type="primary" @click.native="handleClick($event, '555')" id="2"
      >primary</mt-button
    >
    <br />
    <mt-button type="danger">danger</mt-button>
    <br />
    <ul>
      <li
        v-for="(item, key) in list"
        :class="{ red: key == 0, blue: key == 1 }"
        :key="key"
      >
        <div class="itme" v-bind:style="{ width: boxWidth + 'px' }">
          {{ key }} --- {{ item }}
        </div>
      </li>
    </ul>
    <ul>
      <p v-for="(item, i) in list1" :key="i">
        {{ item.title }}
      </p>
    </ul>

    <br />
    <li v-for="(item, i) in list2" :key="i">
      {{ item.cate }}
      <ol>
        <li v-for="(news, i2) in item.list" :key="i2">
          {{ news.title }}
        </li>
      </ol>
    </li>
    <div v-bind:title="title">鼠标瞄上去看一下</div>
    <div v-html="h"></div>
    <div v-text="msg"></div>
    <div :class="{ red: flag, blue: !flag }">我是一个变色div</div>
    <div class="box" v-bind:style="{ width: boxWidth + 'px' }">
      我是style的div
    </div>
    <input type="text" v-model="msg" />
    <button v-on:click="getMsg()">获取表单里面的数据</button>
  </div>
</template>
<script>
import { Toast } from 'mint-ui'
import VueEvent from '../model/VueEvent.js'
export default {
  data () {
    return {
      title: '我是一个title',
      msg: '你好',
      h: '<h2>我是h2</h2>',
      i: '1',
      flag: false,
      value: true,
      boxWidth: 300,
      obj: {
        name: '张三'
      },
      list: ['111', '222', '333'],
      list1: [
        { title: '1111' },
        { title: '2222' },
        { title: '3333' },
        { title: '4444' }
      ],
      list2: [
        {
          cate: '国内新闻',
          list: [{ title: '国内新闻11111' }, { title: '国内新闻2222' }]
        },
        {
          cate: '国际新闻',
          list: [{ title: '国际新闻11111' }, { title: '国际新闻2222' }]
        }
      ]
    }
  },
  methods: {
    getListData () {
      var url =
        'http://www.phonegap100.com/appapi.php?a=getPortalList&catid=20&page=' + this.i
      // jsonp请求，需要后台接口支持jsonp
      // this.$http.jsonp(api).then((response)=>{
      // get请求
      this.$http.get(url).then(
        (response) => {
          this.list = response.body.result
          this.list[0].aid = '0'
          console.log('请求到的数据12:' + JSON.stringify(this.list))
        },
        (error) => {
          console.log('请求错误:' + error)
        }
      )
    },
    handleClick: function (e, num) {
      // 获取当前点击的按钮的id值
      var currentId = e.currentTarget.id
      console.log('id值' + currentId)
      // alert("hahaha");
      // eslint-disable-next-line eqeqeq
      if (currentId == '1') {
        this.$router.push('/list')
        VueEvent.$emit('home', this.obj.name)
        Toast({
          position: 'top',
          message: num,
          iconClass: 'icon logo'
        })
      }
      // eslint-disable-next-line eqeqeq
      if (currentId == '2') {
        Toast({
          position: 'top',
          message: num,
          iconClass: 'icon logo'
        })
      }
    },
    turn () {
      console.log(this.value)
    }
  },
  mounted () {
    // 模板已经编译 -- 执行请求数据的操作
    this.getListData()
  }
}
</script>
<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 100px;
}

a {
  color: #42b983;
}
.red {
  color: red;
}

.blue {
  color: blue;
}

.box {
  width: 100px;
  height: 100px;
  background: red;
  margin-left: 100px;
}
.item {
  width: 100px;
  height: 50px;
  background: red;
  margin: 100px;
  border-radius: 10px;
  outline: 10px solid #00f;
}
</style>
