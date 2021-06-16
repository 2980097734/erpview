<template>
  <div class="homeWrap">
    <el-container style="height:100%">
      <el-container>
        <el-aside>
          <!-- 左侧菜单-->
          <h5>菜单选项</h5>
          <el-menu
            default-active="2"
            class="el-menu-vertical-demo"
            background-color="#545c64"
            text-color="#fff"
            active-text-color="#ffd04b">
            <el-submenu :index="parentmenu.id+''" v-for="parentmenu in menutable">
              <template slot="title">
                <i :class="parentmenu.iconUrl"></i>
                <span>{{parentmenu.name}}</span>
              </template>
              <el-submenu :index="parentmenu1.id+''" v-for="parentmenu1 in parentmenu.childMenu">
                <template slot="title">
                  <i :class="parentmenu1.iconUrl"></i>
                  <span>{{parentmenu1.name}}</span>
                </template>
                <el-menu-item-group>
                  <el-menu-item  @click="addTab(childmenu.name,childmenu.linkUrl)" :index="childmenu.id+''" v-for="childmenu in parentmenu1.childMenu">
                    <i :class="childmenu.iconUrl"></i>{{childmenu.name}}
                  </el-menu-item>
                </el-menu-item-group>
              </el-submenu>
<!--              <el-menu-item-group>-->
<!--                <el-menu-item  @click="addTab(childmenu.name,childmenu.linkUrl)" :index="childmenu.id+''" v-for="childmenu in parentmenu.childMenu">-->
<!--                  <i :class="childmenu.iconUrl"></i>{{childmenu.name}}-->
<!--                </el-menu-item>-->
<!--              </el-menu-item-group>-->
            </el-submenu>
          </el-menu>
        </el-aside>
        <el-main>
          <el-tabs v-model="editableTabsValue" type="card" closable @tab-remove="removeTab">
            <el-tab-pane
              v-for="(item, index) in editableTabs"
              :key="item.name"
              :label="item.title"
              :name="item.name"
            >
              <component :is="item.content"></component>
            </el-tab-pane>
          </el-tabs>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>

  import Inventory from "./components/Inventory";
  import Welcome from "./components/Welcome";
  import Manufactrue from "./components/Manufactrue";
  import Product from "./components/Product";
  import Record from "./components/Record";

export default {
  name: 'app',
  data () {
    return {
      menutable:[],
      editableTabsValue: '1',
      editableTabs: [{   //tab选项卡显示的数据内容
        title: '首页',
        name: '1',
        content: 'Welcome'
      }],
      tabIndex: 1   // 设置到name属性的值
    };
  },
  methods:{
    getmenudata(){
      this.$axios.get("queryAllmenu").then(response=>{
        this.menutable = response.data;
      }).catch();
    },
    addTab(titlename,linkUrl){
      //判断当前页面是否存在
      var tempobj = this.editableTabs.find(item=>{return item.title == titlename});
      //存在

      if(tempobj!=undefined){
        console.log(tempobj.name)
        this.editableTabsValue = tempobj.name;
      }else{
        let newTabName = ++this.tabIndex+'';
        this.editableTabs.push({
          title: titlename,
          name: newTabName,
          content: linkUrl
        });
        this.editableTabsValue = newTabName;
      }
    },
    removeTab(targetName){
      let tabs = this.editableTabs;
      let activeName = this.editableTabsValue;
      if(activeName==targetName){
        tabs.forEach((tab,index)=>{
          if(tab.name==targetName){
            let nextTab = tabs[index+1] || tabs[index-1];
            if(nextTab){
              activeName = nextTab.name;
            }
          }
        });
      }
      this.editableTabsValue = activeName;
      this.editableTabs = tabs.filter(tab=>tab.name != targetName);
    }
  },
  created() {
    this.getmenudata();
  },
  components: {
    Inventory,Manufactrue,Product,Welcome,Record
  }
}
</script>

<style>
  .homeWrap {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }

</style>
