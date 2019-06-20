<template>
  <div id="app">
    
    <div id="content">
      <h1 id="welcome">Hi! Barb! You have</h1>
      <h1 id="countdown"> {{days}}:{{hours}}:{{minutes}}:{{seconds}}</h1>
      <h2 id="countText">days    :    hours    :    mins    :    secs</h2>
      <h1>until the trip!!</h1>
      <div id="field" v-if="showField">
        <input id="dateForm" type="date" v-model="date" name="date" />
        <div class="spacer"></div>
        <button v-on:click="setDate">Submit</button>      
      </div>
      <div id="edit" v-if="showEdit">
        <button v-on:click="hideEdit">Edit</button>
      </div>
      <div id="citation">
        <a href="https://unsplash.com/photos/qE1Y8GQKhEk">Photo by Ishan</a>  
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data(){
    return{
      now: 0,
      count:0,
      countdownDate: localStorage.getItem('myDate'),
      date:"",
      timezone: 0,
      showEdit:false,
      showField: true
    }
  },
  methods: {
    //countdown function 
    countDown(){
      this.count++
      this.timezone = new Date().getTimezoneOffset() * 60000
      this.now = Math.trunc((new Date().getTime() +this.timezone - 3600000 *4) /1000 )
      this.count < 200 && setTimeout(this.countDown, 1000)
    },
    setDate(){
      var inputDate = this.date;
      this.updateDate(inputDate);
      localStorage.removeItem('myDate')
      localStorage.setItem('myDate', inputDate)
      console.log(inputDate);
      console.log(this.now)
      console.log(this.timezone)
      console.log(Math.trunc(new Date().getTime()/1000));
      this.hideFields();
    },
    updateDate(countdownDate){
      this.countdownDate = countdownDate;
      console.log(this.countdownDate)
    },
    //show and hide when pressing buttons
    hideFields(){
      this.showEdit = true;
      this.showField = false;
     
    },
    hideEdit(){
      this.showEdit = false;
      this.showField = true;
    } 
  },
  mounted(){
    this.countDown();
    console.log("hello")
  },
  computed: {
    seconds(){
      return Math.trunc(this.modifiedDate - this.now)%60
    },
    minutes(){
      return Math.trunc((this.modifiedDate - this.now)/60)%60
    },
    hours(){
      return (Math.trunc((this.modifiedDate - this.now)/60/60)%24+4)
    },
    days(){
      return Math.trunc((this.modifiedDate-this.now)/60/60/24)
    },
    modifiedDate:function(){
      return Math.trunc(Date.parse(this.countdownDate)/1000)
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #000000;
  margin-top: 60px;
  height:100%;
  width:100%
  
}
#citation{
  margin-top: 1em;
}
#content{
  position:absolute;
  margin:auto;
  top:0;
  bottom:0;
  right:0;
  left:0;
  height:50%;
  width:50%;
}
body{
  background-image: url("assets/ishan-seefromthesky-403804-unsplash.jpg");
  /*https://unsplash.com/photos/qE1Y8GQKhEk*/
  background-repeat: no-repeat;
  background-size: cover;
  height:100%;
  color: #000000;
}
a:visited {
    color:black;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    font-weight:bold;
    background-color: transparent;
    text-decoration: none;
}
a:link {
    color:black;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    background-color: transparent;
    font-weight:bold;
    text-decoration: none;
}
#countdown{
  text-align: center;
  font-size: 600%;
  margin-top: 0;
  margin-bottom: 0;
}
#countText{
  margin-top: 0;
}
input{
  border: 2px solid #ccc;
  height:2em;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
}
button{
  height:2.5em;
  border: none;
  background-color:rgb(240, 168, 204);
  text-align:center;
  width:10em;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  border-radius:12px;
}
button.hover{
  background-color: #ccc;
}
.spacer{
  height:1em;
}
@media(max-width: 768px){
  #countdown{
    font-size:300%;
  }
  h1{
    font-size: 1.5em;
  }
  #countText{
    font-size: 1em;
  }
  #welcome{
    display: none;
  }
  body{
    background-size:auto;
  }
}
</style>
