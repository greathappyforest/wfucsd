<template>
   <div >
    <div class="container">
      <div class="row col-sm-12 ">
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
      </div>
  <!--item input-->
   <div class="panel panel-default">
      <div class="panel-heading ">Get start</div>
      <div class="panel-body ">
        <form id="form" class="input-group col-sm-8 col-sm-offset-2  " v-on:submit.prevent="addItem">
          
          <div class="form-group col-sm-2">
            <input type="text" class="form-control" placeholder="itemName" v-model="newItemObj.itemName">
          </div>
          <div class="form-group col-sm-2">
            <input type="text" class="form-control" placeholder="itemPrice" v-model="newItemObj.itemPrice">
          </div>
          <div class="form-group col-sm-2">
            <input type="text" class="form-control" placeholder="Number" v-model="newItemObj.NumberOfItem">
          </div>
          <div class="form-group col-sm-2">
            <input type="text" class="form-control" placeholder="wfid" v-model="newItemObj.wfid">
          </div>
          <div class="form-group col-sm-2">
            <input type="text" class="form-control" placeholder="contact" v-model="newItemObj.contact">
          </div>
          <span class="input-group-btn col-sm-2">  <input type="submit" class="btn btn-default" value="Add"></span>
        </form>
      </div>
    </div>


    <!--item list-->
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
          <tr v-for="item in marketplacedb">
            <td>item.itemName</td>
            <td>item.itemPrice</td>
            <td>item.NumberOfItem</td>
            <td>item.wfid</td>
            <td>item.contact</td>
             <td><span class="glyphicon glyphicon-trash" aria-hidden="true" v-on:click="removeItem(item)"></span></td>
          </tr>
        </tbody>
      </table>
    </div>


</div>
</template>



<script>
  import Firebase from 'firebase'
  import toastr from 'toastr'
  import { database } from '../../static/firebase.config.js'

  let marketplacedbRef = database.ref('marketplacedb')
  export default {
    name: 'MarketPlace',
    firebase: {
      marketplacedb: marketplacedbRef,
    },
    data() {
      return {
        newItemObj: {
          wfid: '',
          itemName: '',
          NumberOfItem:'',
          itemPrice:'',
          contact:''
        }
      }
    },
    methods: {
      addItem: function() {
        var submitItem = confirm("Please confirm your warframe Id. If the warframe Id is not correct, this lottery will not count!");
     //   if (submitItem == true && this.newItemObj.wfid && this.newItemObj.itemName && this.newItemObj.itemPrice && this.newItemObj.contact) {
          marketplacedbRef.push(this.newItemObj);
          toastr.success('Item post successfully')
          this.newItemObj.itemName = '';
          this.newItemObj.itemPrice = '';
          this.newItemObj.NumberOfItem = '';
          this.newItemObj.wfid = '';
          this.newItemObj.contact = '';
      //  }
      },
      removeItem: function(Item) {
        marketplacedbRef.child(Item['.key']).remove()
        toastr.success('Item removed successfully')
      }
    }
  }
</script>

<style>
  .right-buffer {
    margin-right: 10px;
  }
</style>