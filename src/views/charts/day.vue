<template>
    <div class="container">
        <Timeline class="animated fadeIn"  v-show="!nodataFlag">
            <TimelineItem v-for="(item,index) in data" :key="index">
                <p class="time">{{item.time}}</p>
                <p class="content">{{item.name}}</p>
            </TimelineItem>
            <TimelineItem>
                <p class="time">今日还没结束</p>
                <p class="content">请继续噢！</p>
            </TimelineItem>
        </Timeline>
        <div v-show="nodataFlag">
          <Timeline class="animated fadeIn">
            <TimelineItem>
                <p class="time">加油啊</p>
                <p class="content">今日没有完成的事件</p>
            </TimelineItem>
          </Timeline>
        </div>
    </div>
</template>



<script>
export default {
  data() {
    return {
        data: [],
        nodataFlag: false,
    };
  },
  methods: {
    init (){
        this.nodataFlag = false;
        this.$axios({
        method: 'post',
        url: '/day',
        data:{'userid': localStorage.getItem('userId')}
      }).then( data => {
        if(data.data.success) {
            this.data = data.data.data;
            if(this.data.length == 0) {
              this.nodataFlag = true;
            }
        }else{
          this.nodataFlag = true;
        }
      });
    }
  },
  mounted() {
    this.init();
  },
}
</script>
<style scoped>
    .time{
        font-size: 14px;
        font-weight: bold;
    }
    .content{
        padding-left: 5px;
    }
    .container {
        width:200px;
        text-align: 0 auto;
        margin-left: 170px; 
        /* color:#c7becd */
    }
</style>
