<template>
  <div class="wrapper">
    <tables :tableData="tableData" :col="col" :handle="handle" :showCon="showCon">  
  
    </tables>
  </div>
</template>

<script>
import tables from './table'
export default {
    components:{
        tables
    },
    data(){
        return {
            tableData: [
                {
                    id: '1',
                    date: '2016-05-02',
                    name: '王小虎1',
                    address: '上海市普陀区金沙江路 100 弄',
                    abcc:"上海市普陀区金沙江路",
                    xzz:"xzz"
                },
                {
                    id: '2',
                    date: '2016-05-04',
                    name: '王小虎2',
                    address: '上海市普陀区金沙江路 200 弄',
                    abcc:"上海市普陀区金沙江路",
                    xzz:"xzz"
                },
                {
                    id: '3',
                    date: '2016-05-01',
                    name: '王小虎3',
                    address: '上海市普陀区金沙江路 300 弄',
                    abcc:"上海市普陀区金沙江路",
                    xzz:"xzz"
                },
                {
                    id: '4',
                    date: '2016-05-03',
                    name: '王小虎4',
                    address: '上海市普陀区金沙江路 400 弄',
                    abcc:"上海市普陀区金沙江路",
                    xzz:"xzz"
                },
                {
                    id: '5',
                    date: '2016-05-03',
                    name: '王小虎4',
                    address: '上海市普陀区金沙江路 400 弄',
                    abcc:"上海市普陀区金沙江路",
                    xzz:"xzz"
                }
                ,
                {
                    id: '6',
                    date: '2016-05-03',
                    name: '王小虎4',
                    address: '上海市普陀区金沙江路 400 弄',
                    abcc:"上海市普陀区金沙江路",
                    xzz:"xzz"
                }
                ,
                {
                    id: '7',
                    date: '2016-05-03',
                    name: '王小虎4',
                    address: '上海市普陀区金沙江路 400 弄',
                    abcc:"上海市普陀区金沙江路",
                    xzz:"xzz"
                }
                ,
                {
                    id: '8',
                    date: '2016-05-03',
                    name: '王小虎4',
                    address: '上海市普陀区金沙江路 400 弄',
                    abcc:"上海市普陀区金沙江路",
                    xzz:"xzz"
                }
                               
            ],
            col: [
                {
                    label: 'id',
                    prop: 'id',
                    first:true,
                    fixed:false,
                    width:80
                },
                {
                    label: '日期',
                    prop: 'date',
                    first:true,
                    fixed:false,
                    width:100
                },
                {
                    label: '姓名',
                    prop: 'name',
                    first:true,
                    fixed:false,
                     width:100
                },
                {
                    label: '地址',
                    prop: 'address',
                    first:true,
                    fixed:false,
                     width:200
                },
                {
                    label: '嗯嗯',
                    prop: 'abcc',
                    first:true,
                    fixed:false,
                     width:150
                } 
                ,
                {
                    label: '测试',
                    prop: 'xzz',
                    first:true,
                    fixed:false,
                    width:150
                }    
            ],
            handle:[
                {
                    name:"删除"
                },
                {
                    name:"修改"
                }
            ],
            showCon:'id',
           
               
            
        }
    }
}


 


</script>
<style lang="css">

</style>