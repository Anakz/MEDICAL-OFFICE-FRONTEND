<template>
<div class="bloc-modale">
    <div class="overlay" v-on:click="toggleModal"></div>
<div  class="modale card">

    <div class="container">
        <div class="text">Add a schedule</div>
        <form @submit.prevent="submitForm">
           <div class="form-row" style="margin-left: 7rem;">
              <div class="input-data">
                 <input v-model="schedule.date" name="date" type="date" style="width: 120px;" onfocus="(this.type='date')" onblur="if(!this.value)this.type='date'">
                 <div class="underline"></div>
                 <label style="left:0px;">Date</label>
              </div>
              
           </div>
           <!-- <div class="form-row" style="margin-left: 7rem;">
              <div class="input-data">
                 <input v-model="schedule.hour" name="hour" type="date" style="width: 120px;" onfocus="(this.type='date')" onblur="if(!this.value)this.type='text'">
                 <div class="underline"></div>
                 <label style="left:0px;">Hour</label>
              </div>
              
           </div> -->
           <div class="form-row"  style="margin-left: 7rem;">
                 <label style="left:0px; color:#0099BE">Hour &nbsp;&nbsp;</label>
                 <div class="input-data">
                    <!-- <input  v-model="schedule.hour" name="hour" type="text" style="width: 120px;" onfocus="(this.type='date')" onblur="if(!this.value)this.type='text'"> -->
                     <select  v-model="schedule.hour" class="form-select" onfocus="(this.type='text')" onblur="if(!this.value)this.type='text'" >
                        <option selected>Select an hour</option>
                        <option value="09:30:00">9h30min</option>
                        <option value="10:00:00">10h00min</option>
                        <option value="13:00:00">13h00min</option>
                        <option value="14:30:00">14h00min</option>
                    </select>
                    <div class="underline mb-2"></div>
                </div>
            </div>
          
             <!-- <div class="form-row" style="margin-left:8rem;">
              
              
                <label style="left:0px; color:#0099BE">User &nbsp;&nbsp;</label>
                 <select  class="btn btn-secondary" style="background-color: #0099BE;" >
                 <div class="dropdown-menu">              
                  <option selected style="background-color: aliceblue; color: black;"> Choose a user</option>    
                    <option style="background-color: aliceblue; color: black;" >Ahmed Bouhouch</option>
                </div>
                </select>               
           </div> -->
           <!-- {{this.schedule.availibility}} -->
           <div class="form-row" style="margin-left:8rem;">
           <label style="left:0px; color:#0099BE;" >Availability &nbsp;&nbsp;</label>
            <input class="form-check-input" style="margin-top: 4px;" type="checkbox"  v-model="schedule.availibility" id="flexCheckDefault">
           </div>
           <div class="form-row submit-btn">
                    <div class="input-data">
                       <div class="inner"></div>
                       <input type="submit" style="margin: 0;width: 120px; position: absolute;top: 50%; left: 110%;
                       right:99%; -ms-transform: translate(-50%, -50%);transform: translate(-50%, -50%);
                        background-color:#0099BE;color: white;border-radius: 8px;" value="submit">                       
                    </div>
                    <div class="input-data">
                       <div class="inner"></div>
                         <button type="button" class="btn btn-danger submit-btn" style="border-radius: 8px;margin-left:30%; height: 40px; width: 120px;" @click="toggleModal">Cancel</button>
                    </div>
                 </div>
        </form>
         
     </div>
  
</div>
</div>
</template>
<script>
import Axios from 'axios'
export default {
  data() {
        return {
            
            schedules: [],
            staffs: [],
            schedule:{},
        };
    },
  methods : {
     submitForm(){
this.createSchedule();
     if (this.schedule) {
    const day = this.schedule.date+'T'+this.schedule.hour+'Z'
    console.log("day")
    console.log(day)
    console.log(new Date().toISOString().slice(0, 10))
    Axios.post("http://127.0.0.1:8000/schedule/", {
      date: "2022-05-30T09:00:00Z",
      user: 1,
      availability: false,
      isDelete: false,
  
    })
    .then(res =>{
      console.log
    })
    console.log("this.schedule")
    console.log(this.schedule)
  }
   },
   async createSchedule(){
  var response = await fetch('http://localhost:8000/schedule/', {
    method: 'post',
    headers: {
      'Content-Type':'application/json'
    },
    body: JSON.stringify(this.schedule)
  });
  this.schedules.push(await response.json());
},
 async getStaff(){
  var response = await fetch('http://localhost:8000/user/');
  this.staffs = await response.json();
          /*const data = await response.json();  
            console.log(data, data.slice()); // <-- These are the same
            this.patients = data.slice();*/
},
   
  },
  created() {
        this.getStaff();
    },
    props:['show','toggleModal']
}
</script>
<style scoped>
/* Modal css*/
.bloc-modale{
    position: fixed;
    top:0;
    bottom:0;
    left:0;
    right:0;
    display: flex;
    justify-content: center;
    align-items: center;
}
.overlay{
    background: rgba(0,0,0,0.5);
    position: fixed;
    top:0;
    bottom:0;
    left:0;
    right:0;
}
.modale{
    background: #f1f1f1;
    color: #333;
    padding: 15px;
    position: fixed;
    top: 10% ;
    left:30%;
    margin-left: 130px;
    
}
.btn-modale {
    position: absolute;
    top:10px;
    right:10px;
}
/*Form css*/
*{
  margin: 0;
  padding: 0;
  outline: none;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}
body{
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  padding: 40px;
  background: linear-gradient(115deg, #56d8e4 10%);
}
.container{
  max-width: 800px;
  background: #fff;
  width: 500px;
  padding: 25px 40px 10px 40px;
  box-shadow: 0px 0px 10px rgba(0,0,0,0.1);
}
.container .text{
  text-align: center;
  font-size: 35px;
  font-weight: 600;
  background: -webkit-linear-gradient(right, #0099BE, #0099BE, #0099BE, #0099BE);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
.container form{
  padding: 15px 0 0 0;
}
.container form .form-row{
  display: flex;
  margin: 32px 0;
}
form .form-row .input-data{
  width: 30%;
  height: 40px;
  margin: 0 20px;
  position: relative;
}
form .form-row .textarea{
  height: 70px;
}
.input-data input,
.textarea textarea{
  display: block;
  width: 70%;
  height: 100%;
  border: none;
  font-size: 17px;
  border-bottom: 2px solid rgba(0,0,0, 0.12);
}
.input-data input:focus ~ label, .textarea textarea:focus ~ label,
.input-data input:valid ~ label, .textarea textarea:valid ~ label{
  transform: translateY(-20px);
  font-size: 14px;
  color: #3498db;
}
.textarea textarea{
  resize: none;
  padding-top: 10px;
}
.input-data label{
  position: absolute;
  pointer-events: none;
  bottom: 10px;
  font-size: 16px;
  transition: all 0.3s ease;
}
.textarea label{
  width: 100%;
  bottom: 40px;
  background: #fff;
}
.input-data .underline{
  position: fixed;
  bottom: 0;
  height: 2px;
  width: 100%;
}
.input-data .underline:before{
  position: absolute;
  content: "";
  height: 2px;
  width: 100%;
  background: #3498db;
  transform: scaleX(0);
  transform-origin: center;
  transition: transform 0.3s ease;
}
.input-data input:focus ~ .underline:before,
.input-data input:valid ~ .underline:before,
.textarea textarea:focus ~ .underline:before,
.textarea textarea:valid ~ .underline:before{
  transform: scale(1);
}
@media (max-width: 700px) {
  .container .text{
    font-size: 30px;
  }
  .container form{
    padding: 10px 0 0 0;
  }
  .container form .form-row{
    display: block;
  }
  form .form-row .input-data{
    margin: 35px 0!important;
  }
}
</style>