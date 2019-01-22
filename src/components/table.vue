<template>
  <div>
    <div class="rightNow">
      <el-popover placement="bottom" width="100" trigger="click">
          <el-checkbox-group v-model="checkList" @change="change">
              <el-checkbox v-for="(item,index) in col" :label="item.label" :key="index"></el-checkbox>
          </el-checkbox-group>
          <el-button slot="reference">可筛选</el-button>
      </el-popover> 
    </div>    
    <div class="loading">  
      <el-table :data="tableData" border stripe  style="width: 100%" :row-style="tableRowStyle"  ref="multipleTable"
      :cell-style="cellStyle"   @cell-click="cellClick"
        row-key="id"  @header-click="headerClick" @selection-change="handleSelectionChange">
           <el-table-column type="selection" width="55" align="center" fixed="left"></el-table-column>
          <el-table-column v-for="(item, index) in dropCol1" 
            :key="`col_${index}`"  v-show="item.first" 
            :fixed="item.fixed"   align="left" :width="item.width"
            :prop="dropCol[index].prop" :label="item.label"
            :render-header="renderHeader" 
            > 
            <!-- <template slot-scope="scope">                
                <div v-if="item.label==='测试'">
                    <slot name="xxxx"></slot>
                    <span>{{scope.row.xzz}}</span>
                </div> 
                <div v-else>
                    {{scope.row[dropCol[index].prop]}}
                </div> 
            </template> -->
          </el-table-column>
           <el-table-column  label="操作" width="120"  fixed="right" align="left">
                <template slot-scope="scope">
                    <el-button v-for="(item,index) in handle" :key="index" @click="handleClick(scope.row)" type="text" size="small">{{item.name}}</el-button>
                </template>
          </el-table-column>  
      </el-table>

    </div>

  </div>
</template>
<script>
import Sortable from 'sortablejs'
export default {
  props:{
    tableData:Array, //表格数据
    col:Array, //表头信息 必须传 label(名字) prop(值) first(判断某表头显示隐藏,默认都为true) fixed(判断某列固定 默认false/true/left/right) width 自定义
    handle:Array, //操作对应的名字 
    showCon:String,  //哪个是查看详情
   
  },
  data() {
    return {
      checkList: [], //默认选中所有表头     
      checkList1:[],  //复制默认选中所有的表头
      dropCol:[],    // 复制表头信息 循环的时候使用 
      dropCol1:[],  // 复制表头信息 拖拽使用     
     }
  },
  created(){    
   
    this.dropCol=this.deepClone(this.col);    
    this.dropCol1=this.deepClone(this.col);  
    
  },
  mounted() {         
    this.col.forEach(item=>{
        this.checkList.push(item.label);
    })

    this.checkList1=this.deepClone(this.checkList);
    this.rowDrop();
    this.columnDrop();
  },
  methods: {
    //行拖拽
    rowDrop() {
      const tbody = document.querySelector('.el-table__body-wrapper tbody')
      const _this = this
      Sortable.create(tbody, {
        onEnd({ newIndex, oldIndex }) {
          const currRow = _this.tableData.splice(oldIndex, 1)[0]
          _this.tableData.splice(newIndex, 0, currRow)
        }
      })
    },
    //列拖拽
    columnDrop() {
      const wrapperTr = document.querySelector('.el-table__header-wrapper tr')
      this.sortable = Sortable.create(wrapperTr, {
        animation: 180,
        delay: 0,
        onEnd: evt => { //拖拽完毕之后发生该事件
          const oldItem = this.dropCol[evt.oldIndex-1]
          this.dropCol.splice(evt.oldIndex-1, 1)
          this.dropCol.splice(evt.newIndex-1, 0, oldItem)
          this.dropCol1=[];
          let arr=this.deepClone(this.dropCol)
          this.$nextTick(() => {
            this.dropCol1=arr;   
          });
          console.log(this.dropCol)
        }
      })
    },
    // 点击查看 删除
    handleClick(row) {
        console.log(row);
    },
    // 点击表头
    headerClick(data){
    
        if(data.fixed){

             this.dropCol.forEach(item=>{
                if(data.label===item.label){
                    item.fixed=false;
                }
            })
        }else{
  
            let a={};
            for(var i=0;i<this.dropCol.length;i++){
                if(this.dropCol[i].label===data.label){
                    this.dropCol[i].fixed=true;
                    a=this.dropCol[i];
                }
            }
            this.removeObjWithArr(this.dropCol,a);
            this.dropCol.unshift(a)
        }
       
     
        this.dropCol1=this.deepClone(this.dropCol);    

      
    },
    // 判断2个数组的不同值
    diff(b,a){
        var c = [];        
        var tmp = a.concat(b);      
        var o = {};        
  
      for (var s = 0; s < tmp.length; s++) {       
          (tmp[s] in o) ? o[tmp[s]]++ : o[tmp[s]] = 1;      
      }       
      for (var x in o) if (o[x] == 1) c.push(x);   
        return c
    },    
    //筛选 列的显示和隐藏
    change(val){
        let arr2=this.diff(this.checkList1,val);
        let name=arr2.map(item=>{ 
            return item
        });
        console.log(val)
        if(this.checkList1.length>val.length){           
           for (var i=0; i<name.length; i++){
              for (var j=0;j<this.dropCol1.length;j++){
                  if(name[i]== this.dropCol1[j].label){
                   this.removeObjWithArr(this.dropCol1,this.dropCol1[j]);
                    this.removeObjWithArr(this.dropCol,this.dropCol[j]);
                    }
                  }
            }
            this.checkList1=val;       
            console.log(this.dropCol,this.dropCol1)     
        }else if(val.length>this.checkList1.length){
            for (var i=0; i<name.length; i++){
              for (var j=0;j<this.col.length;j++){
                  if(name[i]== this.col[j].label){
                      this.dropCol1.push(this.col[j]);
                      this.dropCol1=Array.from(new Set(this.dropCol1));
                      this.dropCol.push(this.col[j]);
                      this.dropCol=Array.from(new Set(this.dropCol));
                    }
                  }
            }
            this.checkList1=val; 
          
        }else if(val.length===this.checkList1.length){ 
              this.col.forEach(item=>{
                item.first=true;
              })
        }
    },
    // 深拷贝数据
    deepClone(obj){
          let objClone = Array.isArray(obj)?[]:{};
          if(obj && typeof obj==="object"){
              for(var key in obj){
                  if(obj.hasOwnProperty(key)){
                      //判断ojb子元素是否为对象，如果是，递归复制
                      if(obj[key]&&typeof obj[key] ==="object"){
                          objClone[key] = this.deepClone(obj[key]);
                      }else{
                          //如果不是，简单复制
                          objClone[key] = obj[key];
                      }
                  }
              }
          }
          return objClone;
    },
    //从数组中删除指定的对象
    removeObjWithArr(_arr,_obj){
         var length = _arr.length;
        for(var i = 0; i < length; i++)
        {
            if(_arr[i] == _obj)
            {
                if(i == 0)
                {
                    _arr.shift(); //删除并返回数组的第一个元素
                    return;
                }
                else if(i == length-1)
                {
                    _arr.pop();  //删除并返回数组的最后一个元素
                    return;
                }
                else
                {
                    _arr.splice(i,1); //删除下标为i的元素
                    return;
                }
            }
        }
    },
    // render 事件
    renderHeader (h,{column,$index}) { 
        return h(
          'div',
          [ 
            h('span', column.label),
            h('i', {
              class:column.fixed===false?'el-icon-edit':'el-icon-edit mystyle',
              style:'margin-left:5px;cursor: pointer;'
            }) 
          ],
        );
    },
    //给某列加上样式
    cellStyle(data){ 
      //console.log(data)
      if(data.column.label===this.showCon){
        return 'color:#409eff;cursor: pointer;'
      }      
    },
    //单元格的点击事件
    cellClick(row, column, cell, event){
        if(column.label===this.showCon){
          alert(row.id)
          //this.xxx; 点击查看详情
        }
    },
    //修改单双行样式
    tableRowStyle({row, rowIndex}){
      if(rowIndex%2===0){
          return 'background-color:#ffffff';       
      }else{
          return 'background-color:#dce5ee';
      }
    },
    //多选按钮
    handleSelectionChange(val){ 
      console.log(val)
    }
  }
}
</script>
<style lang="css">
.el-checkbox-group .el-checkbox{
  margin-left: 30px !important;
}
.rightNow{
  float: right;
  margin: 10px;
}
.mystyle{ 
  color: #409eff !important;
  font-size: 14px;
}
.el-table--enable-row-hover .el-table__body tr:hover>td{
  background-color: #f0f5fc !important;
}
.el-table__header th, .el-table__header tr {
  background-color: #ffffff !important;
  color: #333333 !important;
}
.el-table td,.building-top .el-table th.is-leaf {
    border-bottom:  1px solid #dce5ee !important;
  }
.el-table{
  color: #666666 !important;
  font-family: "Microsoft YaHei" !important;
}
.el-button--text{
  color: #3e84e9 !important;
}
</style>
