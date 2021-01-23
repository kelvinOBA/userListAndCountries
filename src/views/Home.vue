<template>
  <div class="home">

    <div class="parent-cover" >

<div>
  <a href="https://tix.africa/" target="_blank">
  <p><span class="tix-offblue">tix.</span><span class="tix-main">africa</span></p>
  </a>
</div>
      <div class="user-detail animate__animated animate__fadeInUp" v-if="showUserObject">

          <p class="user-email"><b> {{userObject.name}}</b></p>
          <p>Country(s)</p>
          <template v-if="userObject.country.length">

          <p v-for="country in userObject.country" :key="country.country_id"> {{country.country_id}}</p>



          </template>
          <p  class="user-email" v-else>Sorry no country for this user</p>
          <button @click="showUserObject = false" class="guess-btn">Close</button>
      </div>

  <template v-if="!showUserObject">
      <div class="user-component animate__animated animate__fadeIn" v-for="data in returnUserData(userData)" :key="data.id"> 
        <div>
        <img src="../assets/profile_pic.png" height="60" alt="">

        </div>
        <div>
          <h4>{{data.name}}</h4>
          <p class="user-email">{{data.email}}</p>
        </div>
        <div>
       <button class="guess-btn" @click="checkUser(data)">Guess <i class="fa fa-spinner fa-spin" v-if="userId == data.id"></i> </button>
        </div>
      </div>
  </template>
    </div>
 <template v-if="!showUserObject">
  <a href="#" class="btn-default animate__animated animate__fadeInUp" :class="{'paginate-active': index+1 == currentPage || index == 0 && currentPage == 0 }" v-for="(count, index) in paginate_total" :key="count.index" @click="updateCurrentPage(count)">
    {{ count }}
  </a>
    </template>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";
import fakeData from "../../fakeData.json"
import axios from 'axios'
export default {
  name: "Home",
  data(){
return{
userData:[],
userId:'',
startSlice:0,
endSlice:4,
paginate_total:[],
paginate:4,
currentPage: 0,
userObject:{
  name:''
},
showUserObject : false
}

  },
  components: {
    HelloWorld,
  },

  methods: {

    returnUserData(data){

      return data.slice(this.startSlice, this.endSlice)
    },

    updateCurrentPage(index){

      this.currentPage = index
      this.setPaginate(index)

    },
       setPaginate(pageNumber) {
       if (pageNumber == 1) {
       this.startSlice = 0 
       this.endSlice = 4
       }
       else {
       this.startSlice = this.paginate * (pageNumber - 1)
       this.endSlice = pageNumber *this.paginate

       }
     },

     checkUser(user){
       this.userId = user.id
 axios.get(`https://api.nationalize.io/?name=${user.name}`)
        .then(response => {
        

        this.userObject = response.data
        this.showUserObject = true
        this.userId = ''
        })
        .catch(error => {
          if (error.response) {
           
          }
        });
     }
  },

  mounted(){
    
    this.userData = fakeData
     this.paginate_total = this.userData.length/this.paginate;
  }
};
</script>


<style lang="scss" scoped>

.parent-cover{
font-family: 'Roboto', sans-serif;
max-width: 500px;
margin: 0 auto;
}

.user-detail{
  border-radius: 10px;
        border: 2px solid rgba(0,0,0,0.078);
          background: #fff;
          min-height: 200px;
          padding: 20px;
}
.user-component{

  background: #fff;
  display: flex;
  margin-bottom:30px;
  padding: 10px;
      border:2px solid rgba(0,0,0,0.078);
    box-shadow: 0px 1px 6px 0px rgba(0,0,0,0.071);
  border-radius: 10px;
  transition: .4s;
    align-items: center;
  div{
    flex-basis: 30%;
  }
  h4,p{
    text-align: left;
    margin: 0; 
  }
 .user-email{
   color: #858796;
   font-size: 13px;
     text-align: left;
 }
}

.user-component:hover{
    cursor: pointer;
        box-shadow: 0px 1px 6px 2px rgba(0,0,0,0.091);
    border: 2px solid  #0385956b;
    transition: .4s;
}

.guess-btn{
  background: #fc6435;
  border: #fc6435;
  color: #fff;
  border-radius: 3px;
  padding: 10px;
  cursor: pointer;
}
.btn-default {
    color: #fff;
    background-color: #fc6435;
    border-color: #fc6435;
        display: inline-block;
    padding: 6px 12px;
    margin-bottom: 0;
    font-size: 14px;
    font-weight: normal;
    line-height: 1.42857143;
    text-align: center;
    text-decoration: none;
    margin-right: 2px;
    margin-bottom:4px;
    white-space: nowrap;
    vertical-align: middle;
    -ms-touch-action: manipulation;
    touch-action: manipulation;
    cursor: pointer;
  
    user-select: none;
 
    border: 1px solid #f2f2f2;
    border-radius: 4px;
}
.paginate-active{
  background-color: #fff;
  color: #fc6435;
    border-color: #fc6435;
}


.tix-offblue{


color:   #038595;
}

.tix-main{

color:   #fc6435;
}
a{
text-decoration: none;

}
</style>