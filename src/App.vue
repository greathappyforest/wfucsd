<template>
    <div class="container">
        <app-header></app-header>
        <div>
            <div class="row">
                <div class="col-xs-12">
                    <router-view></router-view>
                </div>
            </div>
        </div>
    </div>
</template>


<script>
import Header from './components/Header'

export default {

  name: 'app',
  components:{
    appHeader:Header
    
  }
}
</script>

<style>
    body {
        padding: 30px;
         font-family:'Avenir';
    }
</style>