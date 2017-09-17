<template>
<div>
 <form class="form-inline">
  <div class="form-group">
    <label class="sr-only" for="exampleInputAmount">key for search</label>
    <div class="input-group">
            <input type="text" class="form-control" id="exampleInputAmount" placeholder="keys" v-model="searchQuery">
      <div class="input-group-addon glyphicon glyphicon-zoom-in"></div>
    </div>
  </div>
  <!-- <button type="submit" class="btn btn-primary">search</button> -->
</form>
  <table class="table table-bordered table-hover">
    <thead>
      <tr>
        <th class="text-center" v-for="key in cols">{{key}}</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(file,index) in filelists">
        <td>{{index+1}}</td>
        <td v-for="item in file">{{item}}</td>
      </tr>
    </tbody>
  </table>
</div>
</template>
<script>
export default {
name: 'main',
data () {
return {
cols:["num","name","start","end","author","isPay"],
filelists:[{1:"active",2:"2016",3:"2017",4:"nzw",5:"true"},{1:"active",2:"2016",3:"2017",4:"nzw",5:"true"}]
}
},
method:{
},
mounted:function(){
this.$nextTick(function(){
var _this=this;
fetch('./static/a.json').then(res=>res.json()).then(function(data){
_this.filelists=data;
})
})
}
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
font-weight: normal;
}
ul {
list-style-type: none;
padding: 0;
}
li {
display: inline-block;
margin: 0 10px;
}
a {
color: #42b983;
}
</style>