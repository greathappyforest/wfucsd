<template>
  <div>
 <!-- prize-->
  <div class="container">
  <div class="row col-sm-6 right-buffer">
    <div class="panel panel-info ">
      <div class="panel-heading  ">Prize</div>
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
  <div class="row col-sm-6 ">
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
    <div class="panel panel-info ">
      <div class="panel-heading ">Time left: <span style="color:red"> 123456s{{leftTime}}</span></div>
    </div>
    <!--get start-->
    <div class="panel panel-default">
      <div class="panel-heading ">Get start</div>
      <div class="panel-body ">
        <div class="input-group  col-sm-8 col-sm-offset-2 ">
          <span class="input-group-addon" id="basic-addon1">@</span>
          <input type="text" class="form-control" placeholder="warframe Id">
          <span class="input-group-btn"> <button class="btn btn-default" type="button">Go!</button></span>
        </div>
        <br>
      </div>
    </div>

    <!--Participator list-->
    <div class="panel panel-default">
      <div class="panel-heading">Participators list</div>
      <ul class="list-group">
        <li class="list-group-item">Cras justo odio</li>
        <li class="list-group-item">Dapibus ac facilisis in</li>
        <li class="list-group-item">Morbi leo risus</li>
        <li class="list-group-item">Porta ac consectetur ac</li>
        <li class="list-group-item">Vestibulum at eros</li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    

  }
</script>

<style>
.right-buffer { margin-right:10px; }
</style>