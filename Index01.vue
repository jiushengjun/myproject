<template>
 <div style="margin: 10px">
  <div>
     <div align="right">
        <span> 
          <span>
            <img class="imgLogo" src="../assets/img/logo.png" style="width:100px;height:100px;position: relative; right: 35%" />
          </span>
          <el-input
            style="width: 20%; font-size: 2px"
            v-model.trim="searchStr"
            placeholder="请输入搜索内容"
            @keypress.enter="search()"
         clearable/>
       </span>

        <span>
          <el-button
            type="text"
            style="padding-right: 50px"
            v-bind:icon="Check"
            @click="search()">搜索</el-button>
       </span>
       <el-button type="text" @click="gotoLoginPage()">登录</el-button>
       <el-button
          type="text"
          style="padding-right: 10%"
          @click="gotoUserRegister02Page()"
          >注册</el-button
        >
      </div>
  </div>
</div>
<p align="center"><Banner04 /></p>
<p></p>
 <div class="cont" align="left">
      <p class="color"><span class="big" style="color: rgb(0, 153, 255); font-weight: 900; font-size: x-large">新</span >
      <span style="color: rgb(0, 153, 255)">书速递</span></p><hr size=30 color="pink">
    <p class="More"><span style="color:rgb(0, 153, 255);font-weight: 900; font-size: x-large">学</span> <span style="color:rgb(0, 153, 255);" @click=gotomorePage()>习空间</span></p>
    <hr size=30 color="pink">
    
    <div class="list" v-for="x in info" :key="x.id">
      <p align="center">
        <a @click="gotoBookPage(x.图书编号)">
          <img :src="require('../assets/img/' + x.pic)" />
        </a>
      </p>
          <!-- <img class="imgBook":src="require('../assets/img/'+ x.pic)"/> -->
         <!--<router-link to="/detila">  <img :src="require('../assets/img/'+x.pic)" style="width:110px;height:130px"/></router-link> -->
        <!--   <img :src="'../assets/img/'+x.pic" style="width:110px;height:130px"/>-->
          <div class="bookname">{{ x.书名 }}</div>
          <div class="author">{{ x.作者 }}</div>
          <div>
            <span class="press">{{ x.出版社 }}</span>
            <span class="price">¥{{ x.单价 }}</span>
          </div>
    </div>
    <br><br>
    
 <div class="service">
      <div
          id="local_service"
          class="chat_btn"
          @click="$message('请联系管理员！')">
          <img src="../assets/img/localservice.jpg" style="width:90px;height:90px"/>
          <div>
               <el-button class="service-info" type="text" size="small">
                 本地服务
               </el-button>
          </div>
      </div>

       <div class="chat_btn">
            <a
                  href="https://wpa.qq.com/msgrd?v=3&amp;uin=2194638717&amp;site=qq&amp;menu=yes"
                  target="_blank"
                  style="text-decoration:none"
            >
            <img src="../assets/img/reader.jpg" style="width:90px;height:80px">
            </a>
            <div class="service-info">
                  <el-button size="small" type="text">读者服务</el-button>
            </div>
         </div>
         <div class="chat_btn">
            <a> <img src="../assets/img/returntop.jpg" style="width: 90px; height: 60px"/></a>
            <div class="service-info" >
                  <el-button size="small" type="text" @click="toTop()">
                       返回顶部
                  </el-button>
            </div>
          </div>
       </div>  
<!--从这里切入--> 
</div>
<br>
<p align="center"><Footer /></p>
<!--div class="footer">
        <div class="foot1">
            <p v-bind:class="{moref}">
                <a :class="foot1" href="#">法律声明</a>
                <a :class="foot1" href="#">用户协议</a>
                <a :class="foot1" href="#">合作伙伴</a>
                <a :class="foot1" href="#">联系我们</a>
                <a :class="foot1" href="#">关于我们</a>
                <a :class="foot1" href="#">网站地图</a>
            </p>
    <p style="color:red">@2022-2025books.com版权所有<span>All rights reserved</span></p>
    </div>
    </div-->
</template>
<script>
import axios from "axios";
import Banner04 from "@/components/Banner04.vue";
import Footer from "@/components/Footer.vue"
import {
  Check,
  Delete,
  Edit,
  Message,
  Search,
  Star,
  CircleCheckFilled,
} from "@element-plus/icons-vue";
export default {
  name: "Index01",
  components: {
    Banner04,
   
    Footer
  },
  data() {
    return {
      info: [],
      searchStr: "",
      Check,
      Delete,
      Edit,
      Message,
      Search,
      Star,
      CircleCheckFilled,
    };
  },
  created(){
    axios
        .get("http://127.0.0.1/book/all")
        .then((res)=> {
            console.log('sadasdas',res.data);
            this.info=res.data;
        })
        .catch((err)=>{
            console.log("获取数据失败"+err);
        });
  },
  methods:{
    gotoLoginPage() {
      const jumpPath = this.$parent.jumPath;
      this.$router.push(jumpPath || "/");
    },
    gotoUserRegister02Page() {
      const jumpPath = this.$parent.jumpPath;
      this.$router.push(jumpPath || "/UserRegister02");
    },
    gotomorePage() {
      const jumpPath = this.$parent.jumpPath;
      this.$router.push(jumpPath || "/more");
    },
    toTop() {
      document.documentElement.scrollTop = 0;
    },
    gotoBookPage(bookID) {
      console.log(bookID,this.bookID,'bookId区别');
      localStorage.setItem("bookIDinfo",JSON.stringify(bookID));

      const jumPath = this.$parent.jumpPath;
      this.$router.push(jumpPath || "/bookmysql01");
    },
    search(){
        console.log(this.searchStr);
        axios
        .get("http://127.0.0.1/book/searchBookName", {
          params: {
            bookNameInput:this.searchStr,
          },
        })
        .then((res) => {
          console.log(res.data);
          this.info = res.data;
        })
        .catch((err) => {
          console.log("操作失败!" + err);
        });
    }
  }
};
</script>
<style>
@import "../assets/css/index01.css";
</style>


