<template>
<div>
  <slot name="header"></slot>
  <form class="form-inline">
    <div class="form-group">
      
      <div class="input-group">
        <input type="text" class="form-control"  placeholder="keys" v-model="searchQuery">
        <div class="input-group-addon glyphicon glyphicon-zoom-in"></div>
      </div>
    </div>
    <!-- <button type="submit" class="btn btn-primary">search</button> -->
  </form>
  <table class="table table-bordered table-hover">
    <thead>
      <tr >
        <th class="text-center">num</th>
        <th class="text-center" v-for="key in cols" @click="sortkey(key)" :class="{dropup:order[key]==1}">
          {{key}}
          <span class="caret"></span>
        </th>
        <th class="text-center">操作</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(file,index) in search">
        <td>{{index+1}}</td>
        <td v-for="item in file">{{item}}</td>
        <td>
          <div class="btn-group">
            
          <button class="btn btn-default" data-toggle="modal" data-target="#myModal">edit</button>
          <button class="btn btn-default">down</button>
          </div>
        </td>
      </tr>
    </tbody>
  </table>
  <slot name="footer"></slot>
 
</div>
</template>
<script>
export default {
name: 'main',
data:function () {
var cols=["name","start","end","author","isPay"];
var order={}
cols.forEach(i=>order[i]=1);
return {
cols:cols,
filelists:[{"name":"active","start":"2016","end":"2017","autor":"nzw","isPay":"true"},{"name":"numberß","start":"2016","end":"2017","autor":"ty","isPay":"true"}],
searchQuery:'',
order:order
}
},
methods:{
sortkey:function(key){
var _this=this;
this.order[key]=this.order[key]*-1;
this.filelists.sort(function(a,b){
a=a[key];
b=b[key];
return (a===b?0:a>b?1:-1)*_this.order[key]
});
}
},
computed:{
search:function(){
var items=this.filelists;
var _this=this;
if(!this.searchQuery) return items;
return items.filter(function(value){
return value.name.indexOf(_this.searchQuery)!=-1|| value.author.indexOf(_this.searchQuery)!=-1;
})
}
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