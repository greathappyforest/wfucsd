<template>
  <div>
    <!-- prize-->
    <div class="container">
      <div class="row col-sm-12 " v-if="showTimeLeft">
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
      <div v-else>
        <div class="row col-sm-5 ">
          <div class="panel panel-info ">
            <div class="panel-heading  ">Prizes</div>
            <div class="panel-body ">
              <ol>
                <li>{{prize1}}</li>
                <li>{{prize2}}</li>
                <li>{{prize3}}</li>
                <li>{{prize4}}</li>
                <li>{{prize5}}</li>
              </ol>
            </div>
          </div>
        </div>
        <!-- Winners-->
        <div class="row col-sm-5 col-sm-offset-2 ">
          <div class="panel panel-info ">
            <div class="panel-heading  ">Winners</div>
            <div class="panel-body">
              <ol>
                <li v-for="(winner,index) in winners" v-if="index<5">{{winner.wfid}}</li>
              </ol>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Timeleft-->
    <div class="panel panel-info " v-if="showTimeLeft">
      <div class="panel-heading ">Time left: <span style="color:red"> {{caltimeLeft}} </span></div>
    </div>
    <!-- Lucky Number-->
    <div class="panel panel-info" v-else>
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
            <th>Difference</th>
          </tr>
        </thead>
        
        <tbody>
          <tr v-for="lottery in lotteries">
            <td>{{lottery.wfid}}</td>
            <td>{{lottery.lotteryKey}}</td>
            <!--
            <td><span class="glyphicon glyphicon-trash" aria-hidden="true" v-on:click="removeLottery(lottery)"></span></td>
            -->
            <td><span >{{lottery.diff}}</span></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
  import toastr from 'toastr'
  import lotterydata from '../../static/lotterydata.json'
  import { database } from '../../static/firebase.config.js'

  let lotteryRef = database.ref('lotteries')
  let luckyNumberRef = database.ref('luckyNumber')
  let winnersRef = database.ref('winners')
  export default {
    name: 'Lottery',
    firebase: {
      lotteries: lotteryRef,
      luckyNumber: luckyNumberRef,
      winners: winnersRef
    },
    data () {
      return {
        lotterydata,
        newlotteryObj: {
          wfid: '',
          lotteryKey: ''
        },
        eventEndTime: lotterydata.eventEndTime,
        prize1: lotterydata.prize1,
        prize2: lotterydata.prize2,
        prize3: lotterydata.prize3,
        prize4: lotterydata.prize4,
        prize5: lotterydata.prize5,
        timeLeft: '',
        luckyNumber: 0,
        distance: 0,
        showTimeLeft: true
      }
    },
    methods: {
      addLottery: function () {
        if (!this.showTimeLeft) {
          alert('Lottery event has already passed. Please come back early next time.')
          return
        }
        var submitLottery = confirm('Please confirm your warframe Id. If the warframe Id is not correct, this lottery will not count!')
        if (submitLottery === true && this.newlotteryObj.wfid) {
          this.newlotteryObj.lotteryKey = Math.floor((Math.random() * 1000) + 1)
          lotteryRef.push(this.newlotteryObj)
          toastr.success('Lottery Added successfully')
          this.newlotteryObj.wfid = ''
          this.newlotteryObj.lotteryKey = ''
        }
      },
      removeLottery: function (Lottery) {
        lotteryRef.child(Lottery['.key']).remove()
        toastr.success('Lottery removed successfully')
      },
      getWinner: function () {
        var lucky = 0
        luckyNumberRef.on('value', function (a) {
          var luckyNumberObjs = a.val()
          for (var key in luckyNumberObjs) {
            if (luckyNumberObjs.hasOwnProperty(key)) {
              lucky = luckyNumberObjs[key].luckyNumber
            }
          }
        })
        var diff = 0
        lotteryRef.on('value', function (a) {
          var lotteryObjs = a.val()
          for (var key in lotteryObjs) {
            if (lotteryObjs.hasOwnProperty(key)) {
              diff = Math.abs(lotteryObjs[key].lotteryKey - lucky)
              lotteryRef.child(key).update({
                'diff': diff
              })
            }
          }
          //  get winner size
          var winnersSize = 0
          winnersRef.once('value', function (snapshot) {
            winnersSize = snapshot.numChildren()
          })
          lotteryRef.orderByChild('diff').once('value', function (winnerList) {
            winnerList.forEach(function (data) {
              if (winnersSize < 5) {
                winnersRef.push({
                  'wfid': data.val().wfid,
                  'lotteryKey': data.val().lotteryKey,
                  'diff': data.val().diff
                })
              }
            })
          })
        })
      },
      getLuckyNumber: function () {
        var num = Math.floor((Math.random() * 1000) + 1)
        // console.log(num)
        var luckyNumberdbSize = 0
        luckyNumberRef.once('value', function (snapshot) {
          luckyNumberdbSize = snapshot.numChildren()
        })
        if (luckyNumberdbSize === 0) {
          luckyNumberRef.push({'luckyNumber': num})
        }
      }
    },
    computed: {
      caltimeLeft: function () {
        var vm = this
        var countDownDate = new Date(this.eventEndTime).getTime()
        setInterval(function () {
          var now = new Date().getTime()
          this.distance = countDownDate - now
          var days = Math.floor(this.distance / (1000 * 60 * 60 * 24))
          var hours = Math.floor((this.distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60))
          var minutes = Math.floor((this.distance % (1000 * 60 * 60)) / (1000 * 60))
          var seconds = Math.floor((this.distance % (1000 * 60)) / 1000)
          //  console.log(this.distance)
          if (this.distance >= 0) {
            vm.timeLeft = (days + 'd ' + hours + 'h ' + minutes + 'm ' + seconds + 's ')
            vm.showTimeLeft = true
          } else {
            vm.showTimeLeft = false
          }
        }, 1000)
        return vm.timeLeft
      }
    },
    watch: {
      showTimeLeft: {
        handler: function () {
          this.getLuckyNumber()
          this.getWinner()
        }
      }
    }
  }
</script>

<style>
  .right-buffer {
    margin-right: 10px;
  }
</style>


