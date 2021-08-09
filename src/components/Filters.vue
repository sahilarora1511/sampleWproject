<template>
    <div class="col-md-2 hidden-mobile" v-show="filters.length>0">
     <div id="sidebar">
        <div class="filter-header">
          <h4 class="filter-title">Filter by</h4>
        </div>
       <div class="sidebar_inner">
         <div class="filter-container" v-for="(filter,index) in filters" :key="index">
          <div class="filter-tab" @click="showFilterOption(filter.filter_lable)">
            <h4>{{ filter.filter_lable }}</h4>
          </div>
         <div class="filter-list-container" :id="filter.filter_lable" style="display: none">
           <ul style="list-style: none;padding:8px 5px">
             <li v-for="f in filter.options" :key="f.value_key">
               <div class="checkbox">
                 <label>
                   <input type="checkbox">
                   <span class="cr" v-if="filter.filter_lable.toLowerCase()!=='colour'"><i class="cr-icon fa fa-check"></i></span>
                   <span class="color cr" v-else style="width: 30px" :style="`background-color:${f.value}`"><i class="cr-icon fa fa-check"></i></span>
                   <div class="filter_label"> {{f.value}} <span class="count">({{f.total}})</span></div>

                 </label>
               </div>
             </li>

           </ul>
         </div>
         </div>
       </div>
     </div>
    </div>
</template>

<script>
export default {
  name: "Filters",
  props: {
    filters: Array
  },
  data(){
    return{
      visibility : true
    }
  },
  methods:{
    showFilterOption(label){
      if(document.getElementById(label).style.display === 'block')
        document.getElementById(label).style.display = 'none';
      else
        document.getElementById(label).style.display = 'block'
    }
  }
}
</script>

<style scoped>
@media screen and (max-width: 767px){
  .hidden-mobile {
    display: none;
  }
}

#sidebar{
 border: 1px solid #ededed;
}

.filter-header{
  border-bottom: 1px solid #ddd;
  padding: 8px 5px;
  position: relative;
}

.sidebar_inner{
  position: relative;
}

.filter-tab {
  padding: 8px 6px;
  border-bottom: 1px solid #ddd;
}

.filter-list-container {
  max-height: 150px;
  overflow-y: auto;
}

input[type=checkbox]{
  display: none;
}

span.cr {
  position: relative;
  display: inline-block;
  float: left;
  margin-right: 10px;
  border-radius: 0;
  width: 16px;
  height: 16px;
  margin-top: 2px;
  background-color: #f1f1f1;
}

input[type=checkbox]:checked+.cr{
  background-color: #cf181f;
}

input[type=checkbox]:checked+.color{
  border-color: #cf181f;
}

.filter_label {
  display: flex;
  flex-wrap: wrap;
}
</style>
