<template>
  <div>
    <!-- prize-->
    <div class="container">
      <div class="row col-sm-5 ">
        <div class="panel panel-info ">
          <div class="panel-heading  ">Prizes</div>
          <div class="panel-body ">
            <ol>
              <li>prizeFirst</li>
              <li>prizeSecond</li>
              <li>prizeThird</li>
              <li>prizeForth</li>
              <li>prizeFifth</li>
            </ol>
          </div>
        </div>
      </div>
      <!-- Winners-->
      <div class="row col-sm-5 col-sm-offset-2 ">
        <div class="panel panel-info ">
          <div class="panel-heading  ">Winners</div>
          <div class="panel-body ">
            <ol>
              <li>prizeFirst</li>
              <li>prizeSecond</li>
              <li>prizeThird</li>
              <li>prizeForth</li>
              <li>prizeFifth</li>
            </ol>
          </div>
        </div>
      </div>
    </div>
    <!-- Timeleft-->
    <div class="panel panel-info " v-if="showTimeLeft" >
      <div class="panel-heading ">Time left: <span style="color:red" > {{caltimeLeft}} </span></div>
    </div>
    <!-- Lucky Number-->
    <div class="panel panel-info"  v-else>
      <div class="panel-heading ">Lottery lucky number: <span style="color:red"> 
      <tr v-for="number in luckyNumber">
            <td>{{number.luckyNumber}}</td>
      </tr>
      </span></div>
    </div>
    <!--get start-->
    <div class="panel panel-default">
      <div class="panel-heading ">Get start</div>
      <div class="panel-body ">
        <form id="form" class="input-group  col-sm-8 col-sm-offset-2 " v-on:submit.prevent="addLottery">
          <span class="input-group-addon" id="basic-addon1">@</span>
          <div class="form-group ">
            <input type="text" class="form-control" placeholder="warframe Id" v-model="newlotteryObj.wfid">
          </div>
          <span class="input-group-btn">  <input type="submit" class="btn btn-default" value="Go!"></span>
        </form>
      </div>
    </div>
    <!--Participator list-->
    <div class="panel panel-default">
      <div class="panel-heading">Participators list</div>
      <table class="table table-hover">
        <thead>
          <tr>
            <th>warframe Id</th>
            <th>Lottery Number</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="lottery in lotteries">
            <td>{{lottery.wfid}}</td>
            <td>{{lottery.lotteryKey}}</td>
            <td><span class="glyphicon glyphicon-trash" aria-hidden="true" v-on:click="removeLottery(lottery)"></span></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
  import Firebase from 'firebase'
  import toastr from 'toastr'
  let config = {
    apiKey: "AIzaSyDxUiUJpcQ8gLAjpJWXTQa9qov712j58m0",
    authDomain: "wfucsd.firebaseapp.com",
    databaseURL: "https://wfucsd.firebaseio.com",
    projectId: "wfucsd",
    storageBucket: "wfucsd.appspot.com",
    messagingSenderId: "111487561397"
  };
  let app = Firebase.initializeApp(config)
  let db = app.database()
  let lotteryRef = db.ref('lotteries')
  let luckyNumberRef = db.ref('luckyNumber')
  export default {
    name: 'app',
    firebase: {
      lotteries: lotteryRef,
      luckyNumber:luckyNumberRef
    },
    data() {
      return {
        newlotteryObj: {
          wfid: '',
          lotteryKey: ''
        },
        timeLeft: '',
        luckyNumber: 0,
        distance: 0,
        showTimeLeft:true
      }
    },
    methods: {
      addLottery: function() {
        var submitLottery = confirm("Please confirm your warframe Id. If the warframe Id is not correct, this lottery will not count!");
        if (submitLottery == true && this.newlotteryObj.wfid) {
          this.newlotteryObj.lotteryKey = Math.floor((Math.random() * 1000) + 1);
          lotteryRef.push(this.newlotteryObj);
          toastr.success('Lottery Added successfully')
          this.newlotteryObj.wfid = '';
          this.newlotteryObj.lotteryKey = ''
        }
      },
      removeLottery: function(Lottery) {
        lotteryRef.child(Lottery['.key']).remove()
        toastr.success('Lottery removed successfully')
      }
    },
    computed: {
      caltimeLeft: function() {
        var vm = this
        var left = ''
        var countDownDate = new Date("June 29, 2017 18:00:00").getTime();
        var x = setInterval(function() {
          var now = new Date().getTime();
          this.distance = countDownDate - now;
          var days = Math.floor(distance / (1000 * 60 * 60 * 24));
          var hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
          var minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
          var seconds = Math.floor((distance % (1000 * 60)) / 1000);
        //  console.log(this.distance)
          if (this.distance >= 0){
            vm.timeLeft = (days + "d " + hours + "h " + minutes + "m " + seconds + "s ")
            vm.showTimeLeft=true
          }
          else{
          vm.showTimeLeft=false
          }
        }, 1000);    
        return vm.timeLeft;
      },
    }
  }

    //  luckyNumberRef.push({'luckyNumber':(Math.floor(Math.random() * 1000) + 1) })  
</script>

<style>
  .right-buffer {
    margin-right: 10px;
  }
</style>


