<template>
  <div style="width:100%;display:flex;flex-direction: column;">
    <Tabs ref="tabs" @tabSelect="tabSelect" @tabClose="tabClose" style="height:100%;" :border="false" :scrollable="true">
      <TabPanel v-for="(item,index) in tabs" :key="index" :title="item.title" :closable="item.title!='+'" :bodyStyle="{display:'flex', flexDirection:'column'}">
        <div v-if="item.title!='+'" style="position:absolute;width:100%;height:100%;display:flex;flex-direction:column;">
          <div class="dialog-toolbar">
            <TextBox :ref="'url'+index" placeholder="请输入url" v-model="item.url" @keyup.enter.native="formatUrl(index)" :inputStyle="{borderRadius:0}" style="width:100%;padding:0;border:none;background:none;box-shadow: none;">
              <Addon align="left" class="center">
                <LinkButton :disabled="true" :plain="true">地址：</LinkButton>
              </Addon>
              <Addon align="right">
                <LinkButton @click="formatUrl(index)" iconCls="fa fa-fw fa-search" :plain="true"></LinkButton>
              </Addon>
            </TextBox>
          </div>
          <iframe @load="iframeLoad(index)" :class="'iframe'+index" :src="item.formatUrl" style="flex:1 0 auto;border:none;"></iframe>
        </div>
      </TabPanel>
    </Tabs>
  </div>
</template>

<script>
export default {
  name: 'browser',
  props:['close','menu'],
  data() {
    return {
      url:'',
      tabs:[{
        title:'窗口1',
        url:'www.baidu.com',
        formatUrl:'http://www.baidu.com'
      },{
        title:'+',
      }]
    }
  },
  computed:{
    
  },
  methods: {
    tabSelect(panel){
      if(panel.title == '+'){
        this.tabs.splice(this.tabs.length-1,1)
        this.tabs.push({
          title:'窗口'+(this.tabs.length+1),
          url:'',
          formatUrl:''
        },{
          title:'+',
        })
        this.$nextTick(()=>{
          this.$refs.tabs.select(this.tabs.length-2)
          this.$refs['url'+(this.tabs.length-2)][0].focus()
        })
      }
    },
    tabClose(panel){
      let title = panel.title
      let index = parseInt(title.replace('窗口',''), 10) - 1
      this.tabs.splice(index, 1)
      console.log(this.tabs)
      if(this.tabs.length == 0){
        this.close(this.menu)
      }
    },
    formatUrl(index){
      let url = this.tabs[index].url
      if(url && url.substr(0,4)!='http'){
        url = 'http://'+url
        this.tabs[index].formatUrl = url
      }
    },
    iframeLoad(index){
      let iframe = document.querySelector('.iframe'+index)
      if(this.tabs[index].formatUrl){

      }
    }
  },
  watch:{
    
  },
  mounted(){
    this.$refs.tabs.select(this.tabs.length)
  }
}
</script>