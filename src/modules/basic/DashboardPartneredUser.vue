<template>
  <div>
    <div id="cardBody">
      <b-card id="cardWhole" no-body>
        <b-tabs active-nav-item-class="font-weight-bold text-uppercase text-default" card>
          <!-- Tab 1 -->
          <b-tab title="Online Users" active>
            <b-form-group>
              <i id="searchIcon" style="cursor: pointer" class="fa fa-search fa_custom fa-2x"></i>
              <b-form-input id="searchBar" required placeholder="Search Location" v-model="search"></b-form-input>
            </b-form-group>

            <b-container fluid>
              <b-row>
                <b-col v-for="(item, index) in filterData" :key="index" cols="12" md="4">
                  <div id="card">
                    <b-card class="text-center">
                      <b-row no-gutters>
                        <b-col md="4">
                          <b-card-img :src="item.profilePic?item.profilePic: placeholder" id="userIcon" rounded="circle" style="cursor: pointer" @click="profile('/personalinformation',index)"></b-card-img>
                        </b-col>
                        <b-col md="8">
                          <b-card-body>
                            <label id="f_name" >{{item.fullname}}</label>
                            <br>
                            <label id="address">{{item.address}}</label>
                            <br>
                            <b-button v-on:click="redirect('/authorizationForm', index)" id="connectBtn" >Connect</b-button>
                          </b-card-body>
                        </b-col>
                      </b-row>
                    </b-card>
                  </div>
                  <br>
                </b-col>
              </b-row>
            </b-container>
          </b-tab>
      <!-- Tab 2 -->
          <b-tab :title="'Activities  ' + `${notifs>0?notifs:''}`" >
            <div v-for="(item,index) in notify" :key="index">
             <vs-row  >
                <vs-col  vs-type="flex" vs-w="12">
                  <vs-card>
                    <div slot="header">
                      <h3>
                       Authorization Letter
                       <span v-if ="!item.read" style="color: red"><i>Unread</i></span>
                      </h3>
                    </div>
                    <div>
                      <b-card-text>From :  {{item.senderEmail}}.</b-card-text>
                      <b-card-text>Tracking Number is : {{item.tracknum}}</b-card-text>
                    </div>
                    <div slot="footer">
                      <vs-row vs-justify="flex-end">
                        <vs-button color="primary" type="gradient" @click="viewAuth(index, item._id)" >View</vs-button>
                        <vs-button color="danger" type="gradient"  @click="deleteNotification(index)">Delete</vs-button>
                      </vs-row>
                    </div>
                  </vs-card>
                </vs-col>
              </vs-row>
            </div>
          </b-tab>
          <!-- TAb 3 -->
          <b-tab title="Track">
            <div>
             <vs-row vs-justify="center">
                <vs-col  type="flex" vs-justify="center" vs-align="center" vs-w="6">
                  <vs-card>
                    <div slot="header">
                      <b-form-group label="Tracking No.">
                          <i id="searchIcon" class="fa fa-search fa_custom fa-2x" style="cursor: pointer" @click="searchTrackNum()"></i>
                          <b-form-input id="tracking" required placeholder="e.g 123456789012" v-model="searchTrack"></b-form-input>
                      </b-form-group>
                    </div>
                    <div id="cardTracking" scrollable >
                      <vs-card v-for="(item, index) in trackingData" :key="index">
                        <b-card-text >Receiver : {{item.sendTo}}</b-card-text>
                        <b-card-text >Receiver Location : {{item.locationTo}}</b-card-text>
                        <b-card-text >Date Receive : {{item.date}}</b-card-text>
                      </vs-card>
                    </div>
                  </vs-card>
                </vs-col>
              </vs-row>
            </div>
          </b-tab>
          <!-- Tab 4 -->
          <b-tab title="Tracking Form" @click="outputDate()">
            <b-container fluid>
              <b-row>
                <b-col cols="1">
                </b-col>
                <b-col cols="9">
                   <b-form-group label="Tracking No: ">
                      <b-form-input class="motif" required placeholder="Specific Location" v-model="trackNum"></b-form-input>
                  </b-form-group>
                  <b-form-group label="To: ">
                      <b-form-input class="motif" type="email" required placeholder="Recepient's Email"  v-model="emailTo"></b-form-input>
                  </b-form-group>
                  <b-form-group label="Location: ">
                      <b-form-input class="motif" required placeholder="Recepient's Location"  v-model="location"></b-form-input>
                  </b-form-group>
                  <b-form-group label="Date: ">
                      <b-form-input class="motif" required placeholder="Date" v-model="date" disabled></b-form-input>
                  </b-form-group>
                  <center>
                    <b-button :disabled="!inputEnable" block variant id="addBtn" @click="addTracking()">Add</b-button>
                  </center>
                 
                </b-col>
                <!-- <b-col cols="2">
                    <br>
                    <b-button :disabled="!inputEnable" block variant id="addBtn" @click="addTracking()">Add</b-button>
                </b-col> -->
              </b-row>
            </b-container>
          </b-tab>
        </b-tabs>
      </b-card>
    </div>
  </div>
</template>
<style lang='scss' scoped>
@import "~assets/color.scss";
#cardBody {
  margin-top: 5%;
  margin-left: 1%;
  margin-right: 1%;
  margin-bottom: 1%;
  height: 100%;
}
#card{
  border-color: $motif;
  box-shadow: 3px 5px 15px 3px #888888;
  border-radius: 7px;
}
.card{
  text-align: center;
  border: 2px solid $motif;
}
#searchBar{
  width: 30%;
  float: right;
  border-color: $motif;
}
#searchIcon{
  float: right;
  margin-left: 5px;
}
#f_name,#address{
  border: 2px solid;
  border-radius: 5px;
  border-color: $motif;
  width: 100%;
}
#tracking{
  width: 90%;
  float: right;
  border-color: $motif;
}
#connectBtn{
  background-color: $motif;
}
#addBtn{
 background-color: $motif;
 width: 20%;
}
.motif{
    border-color: $motif;
}
#userIcon{
  height:130px;
  margin-top: 18%;
  margin-left: 3%;
  width: auto;
}
.red{
  color: red;
  font-style: italic;
}
</style>


<script>
import ROUTER from "router"
const Swal = require('sweetalert2')
const axios = require('axios');
export default {
  mounted(){
    this.getAllNotification();

    this.managePusher()
    setTimeout( () => {
      this.retrieve( response => {
        if(response.data.partner.length > 0){
          this.data = response.data.partner
        }else{
          this.data = []
        }
      })
    }, 100)
  },
  data() {
    return {
      placeholder: require('assets/user.png'),
      data: [],
      search: "",
      notify: [],
      trackingData: [],
      date: "",
      emailTo: "",
      location: "",
      trackNum: "",
      notifs:0,
      searchTrack: "",
      inputEnable: true,
    };
  },
  component: {
  },
  computed: {
    filterData () {
        return this.data.filter( data => {
          console.log(data.email)
            return data.email !== localStorage.getItem("email") && (!this.search || data.address.toLowerCase().includes(this.search.toLowerCase()))
    })
    }
  },
  // updated(){
  //   this.getAllNotification();
  // },

  methods: {
    retrieve(callback){
      axios.post('http://localhost:3000/allPartners')
      .then(function (response) {
        console.log(response.data.partner)
        callback(response)
      })
      .catch(function (error) {
        console.log(error);
      });
    },

    searchTrackNum(){
      if(this.searchTrack == ""){
        Swal.fire({
          position: 'center',
          icon: 'error',
          title: 'Tracking number is required',
          showConfirmButton: false,
          timer: 1500
        })
      }else{
        axios.post('http://localhost:3000/validateTrackingNum/' + this.searchTrack)
        .then(res => {
          console.log(res)
          axios.post('http://localhost:3000/searchTrack/'+ this.searchTrack)
          .then(response => {
            console.log(response)
            if(res.data.track.length == 0){
              // alert("")
              Swal.fire({
                position: 'center',
                icon: 'error',
                title: 'invalid tracking number',
                showConfirmButton: false,
                timer: 1500
              })
            }else if(response.data.track.length > 0){
              this.trackingData = response.data.track
            }else{
              this.trackingData = []
              Swal.fire({
                position: 'center',
                icon: 'error',
                title: 'No update yet',
                showConfirmButton: false,
                timer: 1500
              })
            }
          })
          .catch(err => {
            console.log(err)  
          })
        })
        .catch(err => {
          console.log(err)
        })
      }
    },

    managePusher(){
      var channel = this.$pusher.subscribe("my-channel");
      channel.bind("my-event", (notify) => {
        console.log("Channel --------------------" , notify)
          var data = {
            tracknum: notify.notify.trackingNum,
            email: notify.notify.recemail,
            senderEmail: notify.notify.sendemail
          }
          console.log(data)
          axios.post('http://localhost:3000/notification',data)
            .then(res => {
              console.log(res)
            })
            .catch(err => {
              console.log(err)
            })
          console.log("I am MJ. I'm in")
      })
    },

    getAllNotification(){
      axios.get('http://localhost:3000/notify/' + localStorage.getItem("email"))
        .then(res => {
          if(res.data.pusher.length > 0){
            this.notify = res.data.pusher;
            this.notify.map(item=>{
              if (!item.read) {
                this.notifs+=1
              } 
            })
          }else{
            this.notify = []
          }
        })
        .catch(err => {
          console.log(err)
        })
    },

    redirect(route, index){
      localStorage.setItem("receiverEmail", this.filterData[index].email)
      ROUTER.push(route);
    },
    
    addTracking(){
      this.inputEnable = false
      if(this.trackNum == "" || this.emailTo == "" || this.location == ""){
        Swal.fire({
          position: 'center',
          icon: 'error',
          title: 'all fields are required',
          showConfirmButton: false,
          timer: 1500
        })
        this.inputEnable = true
      }else{
        var data = {
          trackingNo: this.trackNum,
          sendTo: this.emailTo,
          locationTo: this.location,
          date: this.date
        }
        axios.post('http://localhost:3000/validateTrackingNum/' + this.trackNum)
          .then(response => {
            console.log("asj;ldfjlasjdlfj")
            console.log(response.data)
            if(response.data.track.length == 0){
              Swal.fire({
                position: 'center',
                icon: 'error',
                title: 'invalid tracking number',
                showConfirmButton: false,
                timer: 1500
              })
              this.inputEnable = true
            }else{
              axios.post('http://localhost:3000/trackingInput', data)
              .then(res => {
                console.log(res.data)
                Swal.fire({
                  position: 'center',
                  icon: 'success',
                  title: 'Successfully Sent',
                  showConfirmButton: false,
                  timer: 1500
                })
                var notify = {
                  trackingNum: this.trackNum,
                  sendemail: localStorage.getItem("email"),
                  recemail: this.emailTo
                }
                axios.post("http://localhost:3000/pusher", notify)
                  .then(response => {
                    console.log(response)
                  })
                  .catch(err => {
                    console.log(err)
                  })
                  this.inputEnable = true
              })
              .catch(err => {
                console.log(err)
                this.inputEnable = true
              })
            }
          })
          .catch(err => {
            console.log(err)
            Swal.fire({
              position: 'center',
              icon: 'error',
              title: 'invalid tracking number',
              showConfirmButton: false,
              timer: 1500
            })
            this.inputEnable = true
          })
      }
      
    },
    outputDate(){
      var d = new Date()
      this.date = ("Date: " + d.getMonth() + "/" + d.getDate() + "/" + d.getFullYear() + "     Time: " + d.getHours() + ":" + d.getMinutes() + ":" + d.getSeconds());
    },
    viewAuth(index, item){
      console.log(item)
      axios.get("http://localhost:3000/updateNotif/"+item).then(res=>{
        console.log(res)
        this.getAllNotification();
      }).catch(err=>{
        console.log(err)
      })
      console.log(this.notify[index].tracknum)
      localStorage.setItem("track", this.notify[index].tracknum)
      ROUTER.push('/viewAuth')
    },
    deleteNotification(index){
      console.log(this.notify[index].tracknum)
      axios.delete('http://localhost:3000/deletNotification/' + this.notify[index].tracknum)
        .then(res => {
          console.log(res)
        })
        .catch(err => {
          console.log(err)
        })
      this.notify.splice(index, 1)
    },
    profile(route, index){
      localStorage.setItem("profEmail", this.filterData[index].email)
      ROUTER.push(route);
    }
  }
};
</script>