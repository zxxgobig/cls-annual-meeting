<template>
  <div class="main">
    <div class="titleBox">
      <h2 class="head">上海易积通电子商务有限公司<br>2016年会节目投票<div>(本页面数据实时变化，无需刷新页面)</div></h2>
    </div>
    <ul>
      <li class="bar-wrap" v-for="(program, index) in programList">
        <label>{{program.name + '（表演者：' + program.actors + '）'}}</label>
        <div class="bar" :class="program.color" v-bind:data-percentage="voteCount[index].num + '票'" v-bind:style="{ width: voteCount[index].num + '%' }"></div>
      </li>
    </ul>
    <div class="tongji">投票人数：{{ VOTE.votePeople }}， 总票数：{{ VOTE.voteCount }}</div>
    <div class="online">在线人数：{{ VOTE.onlineCount }}</div>
    <div class="btnContainer"><button v-on:click="goVote()">去投票</button></div>
    <div class="btnContainer"><button v-on:click="goChat()">去聊天</button></div>
  </div>
</template>

<script>
import VOTE from '../../api/vote.js'
import programList from '../../programList.js'

export default {
  name: 'home',
  data () {
    return {
      voteCount: VOTE.voteDetail,
      VOTE,
      programList: programList
    }
  },
  created () {
    VOTE.init(this)
    
    if(!localStorage.getItem('hasReadRule')){//未读过规则，跳转到规则页面
      this.$router.push('voteRule')
      return
    }
    
    VOTE.getVoteInfo()
  },
  methods: {
    goVote () {
      this.$router.push({name: 'vote'})
    },
    goChat () {
      this.$router.push('/')
    }
  }
}
</script>

<style scoped>
.titleBox{padding-top:60px;}
h2.head {text-align: center;color: #f43531;font-size: 35px;line-height: 40px; font-weight: 900;}
h2.head div{font-size: 25px;color:#999;margin-top: 5px;font-weight: normal;}

ul{padding-top: 10px;}
div.tongji{text-align: center;margin-top: 60px;}
div.online{color:gray;font-size: 25px;margin-top: 20px;text-align: center;}
div.btnContainer{width: 49%;text-align: center;display: inline-block;}
button{background: url(../../assets/icons.png) -51px -412px no-repeat; background-size: 300px 800px;height: 80px;width:202px;border:none;margin: 30px auto 60px auto;font-size: 32px;color: white;font-weight: bold;}

*, *:before, *:after {
  box-sizing: border-box;
}
li.bar-wrap {
  width: 100%;
  margin: 0 0 40px 0;
  overflow: hidden;
}
li.bar-wrap label {
  font-size: 28px;
  color: #8a8a8a;
  margin: 0 0 15px 0;
  display: block;
}
li.bar-wrap div.head{width: 70px;height: 97px;float: left;}
li.bar-wrap div.bar {
  height: 50px;
  width: 0;
  position: relative;
}
li.bar-wrap div.bar:after {
  content: attr(data-percentage);
  display: block;
  position: absolute;
  top: 0;
  right: 0;
  padding-right: 10px;
  line-height: 50px;
  color: #FFF;
}
li.bar-wrap div.bar.blue {
  background-color: #61a7c4;
  border-bottom: 3px solid #4290b0;
}
li.bar-wrap div.bar.green {
  background-color: #61c471;
  border-bottom: 3px solid #42b054;
}
li.bar-wrap div.bar.red {
  background-color: #c46161;
  border-bottom: 3px solid #b04242;
}
li.bar-wrap div.bar.purple {
  background-color: #a461c4;
  border-bottom: 3px solid #8d42b0;
}
li.bar-wrap div.bar.gray {
  background-color: rgb(147,147,147);
  border-bottom: 3px solid rgb(100,100,100);
}
</style>
