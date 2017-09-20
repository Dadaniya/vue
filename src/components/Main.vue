<template>
<div>
  <form class="form-inline">
    <div class="form-group">
      <div class="row">
        <div class="col-md-7">
          <div class="input-group">
            <input type="text" class="form-control"  placeholder="keys" v-model="searchQuery">
            <span class="input-group-addon"><label class="glyphicon glyphicon-zoom-in"></label></span>
          </div>
          
        </div>
        <div class="col-md-5">
          <input class="control-label btn btn-default" type="button" value="new" data-toggle="modal" data-target="#myModal" :data-whatever="-1">
          
        </div>
        
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
        <td class="text-center">{{index+1}}</td>
        <td class="text-center" v-for="item in file">{{item}}</td>
        <td class="text-center">
          <div class="btn-group">
            
            <button class="btn btn-default" data-toggle="modal" data-target="#myModal" :data-whatever="index">edit</button>
            <button class="btn btn-default">down</button>
          </div>
        </td>
      </tr>
    </tbody>
  </table>
  <div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel">
    <div class="modal-dialog" role="document">
      <div class="modal-content">
        <div class="modal-header">
          <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
          <h4 class="modal-title" id="myModalLabel">{{modalItem.name}}</h4>
        </div>
        <div class="modal-body">
          <form class="form-horizontal">
            <div class="form-group" v-for="(value,key) in modalItem">
              <label class="col-sm-2 control-label">{{key}}</label>
              <div class="col-sm-10">
                <input type="text" class="form-control" v-model="modalItem[key]"/>
              </div>
            </div>
          </form>
        </div>
        <div class="modal-footer has-error">
          <label class="text-danger" v-if="">has error</label>
          <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
          <button type="button" class="btn btn-primary" @click="saveChange">Save changes</button>
        </div>
      </div>
    </div>
  </div>
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
searchList:[],
order:order,
modalItem:{},
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
},
dosomething:function(){
},
saveChange:function(){
console.log("saveChange");
}
},
computed:{
search:function(){
var items=this.filelists;
var _this=this;
if(!this.searchQuery) return this.searchList=items;
return this.searchList=items.filter(function(value){
return value.name.indexOf(_this.searchQuery)!=-1|| value.author.indexOf(_this.searchQuery)!=-1;
})
}
},
mounted:function(){
var _this=this;
this.$nextTick(function(){
fetch('./static/a.json').then(res=>res.json()).then(function(data){
_this.filelists=data;
})
});
$('#myModal').on('show.bs.modal',
function(e){
// e.preventDefault();
// e.stopPropagation();
var bar=$(e.relatedTarget);
var re=bar.data('whatever');
switch (re){
case -1:
_this.modalItem={};
_this.cols.forEach(x=>_this.modalItem[x]='');console.log(_this.modalItem);
break;
default:
_this.modalItem=JSON.parse(JSON.stringify(_this.searchList[re]));

}
}
);
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