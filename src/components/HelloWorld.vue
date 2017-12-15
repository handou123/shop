<template >
<div class="container">    
        <div class="col-md-12 col-md-offset-3">
            <div id="app">
                <table class="table">
                    <thead>
                        <tr>
                            <th colspan="5">购物清单</th>
                        </tr>
                        <tr>
                            <th class="text-center"><input type="checkbox" v-model="seen1" v-on:click="checkedAll()">全选</th> 
                            <th class="text-center">商品</th>
                            <th class="text-center">数量</th>
                            <th class="text-center">单价（元）</th>
                            <th class="text-center">金额（元）</th>
                            <th class="text-center">操作</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(value,index) in products">
                            <td class="text-center">
                                <input type="checkbox" v-model="value.seen" v-if="'!value.seen ? seen=false:seen=true;'"
                                v-on:click="checked(index)"></td>
                            <td >
                                <div style="width:300px;height:100px;">
                                    <img v-bind:src="value.imgs" alt="" class="img-thumbnail">
                                    <p style="float:right;" v-html="value.name">
                                    </p>
                                </div>
                            </td>
                            <td class="text-center">
                                <input type="button" value="-" style="width:20px;" v-on:click="value.nums<=0?value.nums=0:value.nums--">
                                {{value.nums}}
                                <input type="button" value="+" style="width:20px;" v-on:click="value.nums++">
                            </td>
                            <td class="text-center">￥{{value.price}}</td>
                            <td class="text-center">￥{{Number(value.nums*value.price)}}</td>
                            <td class="text-center">
                                <button type="button" class="btn btn-primary" v-on:click="del(index)">删除</button>
                            </td>
                        </tr>
                    </tbody>
                    <tfoot>
                        <tr>
                            <td>
                                <button type="button" class="btn btn-default" @click="delAll">
                                    删除所选商品
                                </button>
                            </td>
                            <td>
                                <button type="button" class="btn btn-default">
                                    继续购物
                                </button>
                            </td>
                            <td class="text-right" colspan="3">
                                {{total[0]}}件商品总计（不含运费）：￥{{total[1]}}
                            </td>
                            <td class="text-right">
                                <button type="button" class="btn btn-warning">去结算</button>
                            </td>
                        </tr>
                    </tfoot>
                </table>
            </div>
    </div>
</div>
</template>

<script>
var len=0;
export default {
  name: 'HelloWorld',
  data () {
    return {
      seen1:false,
            products:[
                {
                seen:false,
                imgs:"../images/1.jpg",
                name:"【斯文】甘油|丙三醇 <br>品牌：产地：韩国",
                nums:1,
                price:10,
                },
                {
                seen:false,
                imgs:"../images/2.jpg",
                name:"【斯文】甘油|丙三醇 <br>品牌：产地：韩国",
                nums:3,
                price:30,
                },
                {
                seen:false,
                imgs:"../images/3.jpg",
                name:"【斯文】甘油|丙三醇 <br>品牌：产地：韩国",
                nums:2,
                price:15,
                },
                {
                seen:false,
                imgs:"../images/4.jpg",
                name:"【斯文】甘油|丙三醇 <br>品牌：产地：韩国",
                nums:1,
                price:20,
                }
            ]
    }
  },
  computed:{
            total:function(){
                var totalNum=0,totalPrice= 0;
                for(var i=0;i<this.products.length;i++)
                {
                    if(this.products[i].seen)
                    {
                        totalNum +=Number(this.products[i].seen);//true==1 
                        totalPrice+=(Number(this.products[i].price*this.products[i].nums));
                    }
                }
                return [totalNum,totalPrice];
            }
  },
  methods:{
      //单击全选
      //点击时，复选框还是false未被选中，点击完抬起 复选框才为true被选中
        checkedAll:function(){
            if(!this.seen1){
                for(var i=0;i<this.products.length;i++){
                    this.products[i].seen=true;
                } 
            }else{
              for(var i=0;i<this.products.length;i++){
                    this.products[i].seen=false;
                } 
            };
        },
        //单个选中 判断是否全选 当前选中，长度len就加1，没选中len减1。  
        //点击时，复选框还是false未被选中，点击完抬起 复选框才为true被选中
        checked:function(key){
          if(this.products[key].seen==false){
              len++;
          }else{
              len--;
          }
          if(len==this.products.length){
              this.seen1=true;
          }else{
              this.seen1=false;
          }
        },
      //   删除单个选中的
        del:function(key){
            if(this.products[key].seen)
            {
              this.products.splice(key,1);
            }
        },
        //删除所有已选中商品
        delAll:function(){

          this.products=this.products.filter(function (value) {return !value.seen});
          this.seen1=false;
        }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
*{margin: 0;padding: 0;font-size: 16px;}
img{width:140px;height:140px;float: left;}
.container{margin: 0 auto;}
.img-thumbnail{
    padding: 0.25rem;
    background-color: #fff;
    border: 1px solid #dee2e6;
    border-radius: 0.25rem;
    max-width: 100%;
    height: auto;
}
.btn-primary {
    color: #fff;
    background-color: #007bff;
    border-color: #007bff;
}
.btn-warning {
    color: #212529;
    background-color: #ffc107;
    border-color: #ffc107;
}
.btn-default{
    color: #212529;
    background-color: #ddd;
    border-color: #ddd;
}
.btn{padding: 8px 10px;}
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
