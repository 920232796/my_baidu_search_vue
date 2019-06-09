<template>
    <div>
      
      <div class = "cardWrapper">
        <el-carousel :interval="4000" type="card" height="200px" >
          <el-carousel-item v-for="item in itemList" :key="item.id">
            <img :src="item.url" alt="" width="400px" height="200px">
          </el-carousel-item>
        </el-carousel>

      </div>

      <div class = "search_input_btn">
        <el-input v-model="input" placeholder="请输入内容" class="search_input"></el-input>
        <el-button @click="handleBtnClick" size="medium" type="primary" class="search_btn" icon="el-icon-search">搜索</el-button>
      </div>
      <div class="wrapper">
        <div class="content" v-for="each_result of resultList" :key="each_result.id" @click="handleContentClick(each_result.url)">
          <div class="title">
            {{each_result.title.substring(0, 9)}}
          </div>
          <div class="complete">
            {{each_result.title.substring(0, 60)}}
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
              pageSize: 5,
              itemList: [
                {id: 1, url: require("@/assets/22.jpeg")},
                {id: 2, url: require("@/assets/222.jpeg")},
                {id: 3, url: require("@/assets/juzi.jpg")},
                {id: 4, url: require("@/assets/x.jpg")}
              ]
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
            this.start = (page-1) * this.limit
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

  
  .el-carousel__item:nth-child(2n) {
    background-color: #99a9bf;
  }
  
  .el-carousel__item:nth-child(2n+1) {
    background-color: #d3dce6;
  }

  .cardWrapper {
    width: 800px;
    height: 200px;
    margin: 0 auto;
 }

  .wrapper{
    /* border: 1px solid red; */
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
    height: 110px;
    /* border: 1px solid red; */
    margin: 0px auto;
    margin-top: 100px;
  }
  .search_btn {
    float: left;
    width: 120px;
    height: 40px;
    /* border: 1px solid black; */
  }
  .search_input {
    float: left;
    width: 500px;
    height: 115px;
  }
</style>
