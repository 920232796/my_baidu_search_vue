<template>
    <div>
      <!--<swiper :options="swiperOption" ref="mySwiper" @someSwiperEvent="callback">-->
        <!--&lt;!&ndash; slides &ndash;&gt;-->
        <!--<swiper-slide>-->
          <!--<img src="@/assets/22.jpeg" width="100%"/>-->
        <!--</swiper-slide>-->
        <!--<swiper-slide>-->
          <!--<img src="@/assets/222.jpeg" width="100%" /></swiper-slide>-->
        <!--<swiper-slide>-->
          <!--<img src="@/assets/logo.png"/>-->
        <!--</swiper-slide>-->

        <!--&lt;!&ndash; Optional controls &ndash;&gt;-->
        <!--<div class="swiper-pagination"  slot="pagination"></div>-->
      <!--</swiper>-->

      <div class = "search_input_btn">
        <el-input v-model="input" placeholder="请输入内容" class="search_input"></el-input>
        <el-button @click="handleBtnClick" size="medium" type="primary" class="search_btn">搜索</el-button>
      </div>
      <div class="wrapper">
        <div class="content" v-for="each_result of resultList" :key="each_result.id" @click="handleContentClick(each_result.url)">
          <div class="title">
            {{each_result.title.substring(0, 9)}}
          </div>
          <div class="complete">
            {{each_result.title.substring(0, 30)}}
          </div>
        </div>
      </div>


      <div class="block">
        <el-pagination
          background
          @current-change="handleCurrentChange"
          :page-size="pageSize"
          layout="prev, pager, next"
          :total="totalNumber">
        </el-pagination>
      </div>


    </div>
</template>

<script>
    import axios from "axios"
    export default {
        name: '',
        data () {
            return {

              input: "",
              start: 0,
              limit: 5,
              resultList: [],
              totalNumber: 0,
              pageSize: 5
//              swiperOption: {
//                // some swiper options/callbacks
//                // 所有的参数同 swiper 官方 api 参数
//                // ...
//                pagination: '.swiper-pagination',
//                loop: true
//              }
            }
        },
        methods: {
          handleBtnClick() {
            console.log("hello world")
            this.limit = 5
            this.start = 0
            if (this.input != "") {
              axios.get("/api/search", {
                params: {
                  start: this.start,
                  limit: this.limit,
                  keyword: this.input
                }
              }).then(this.handleSearchSucc)
            }
          },
          handleSearchSucc(res) {
            res = res.data
            console.log(res)
            this.resultList = res.result_list
            this.totalNumber = res.total_number
          },
          handleContentClick(url){
            console.log(url)
            var tempwindow=window.open('_blank');
            tempwindow.location="https://pan.baidu.com/s/" + url
    },
          handleCurrentChange	(page){
            console.log(page)
            this.start = page * this.limit
            axios.get("/api/search", {
              params: {
                start: this.start,
                limit: this.limit,
                keyword: this.input
              }
            }).then(this.handleSearchSucc)
          }
        }
    }
</script>

<style scoped>
  .wrapper{
    border: 1px solid red;
    width: 600px;
    margin: 5px auto;
  }
  .content{
    width: 550px;
    /*height:250px;*/
    border-bottom: 1px solid black;
    border-top: 1px solid black;
  }
  .title {
    width: 540px;
    height: 60px;
    font-size: 25px;
    /*border:1px solid blue;*/
  }
  .complete {
    width: 540px;
    height:80px;
    font-size: inherit;
    /*border: 1px solid yellow;*/
  }
  .block {
    width: 450px;
    height: 100px;
    margin: 20px auto;
  }
  .search_input_btn{

    width: 650px;
    height: 130px;
    /*border: 1px solid red;*/
    margin: 0px auto;
    margin-top: 100px;
  }
  .search_btn {
    float: left;
    width: 120px;
    height: 40px;
    /*border: 1px solid black;*/
  }
  .search_input {
    float: left;
    width: 500px;
    height: 115px;
  }
</style>
