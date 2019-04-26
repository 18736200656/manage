<template>
	<div class="aside">
		<el-menu
      :default-active="$route.path"
      class="el-menu-vertical-demo"
      background-color="#545c64"
      text-color="#fff"
      active-text-color="#ffd04b"
      @select="selectO"
      >
      <h3 style="padding-left:20px;line-height:50px">我 </h3>

      <el-menu-item :index="item.path" v-for="item,index in navData" :key="index">
        <!-- <i class="el-icon-delete"></i> -->

        <img :src="item.path == isActive  ? item.active :item.src" :alt="item.title" />
        <span slot="title">{{item.title}}</span>
      </el-menu-item >
    </el-menu>
 	</div>
</template>
<script>
	export default {
		name:'asidea',
		data(){
			return{
        item:'1',
        isActive:'/a',
        navData:[
        {
          // isActive:0,
          path:'/a', 
          title:'导航一', 
          src:'../../static/img/menu_icon_bz_normal.png',
          active:'../../static/img/menu_icon_bz_highlight.png'
        },
        {
          // isActive:1,
          path:'/login',
          title:'导航二',
          src:'../../static/img/menu_icon_pj_normal.png',
          active:'../../static/img/menu_icon_pj_highlight.png'
        },
        {
          // isActive:2,
          path:'/error',
          title:'导航三',
          src:'../../static/img/menu_icon_tdbz_normal.png',
          active:'../../static/img/menu_icon_tdbz_highlight.png'
        },
        {
          // isActive:3,
          path:'/',
          title:'导航四',
          src:'../../static/img/menu_icon_zb_normal.png',
          active:'../../static/img/menu_icon_zb_highlight.png'
        }
        
        ]
			}
		},
		methods:{
			selectO(key,keypath){
        this.isActive = key
				console.log(key,keypath)
				// this.$router.push({path:key})
		 	}		
		}
	}


</script>
<style lang="scss">
	.aside{
		width: 180px;
    .el-menu{
      /* box-sizing:border-box; */
    }
    .el-menu-item.is-active{
      background: #000!important;
    }
    .el-menu-item img{
      margin-right:5px;
    }
	}
</style>