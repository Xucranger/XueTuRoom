<template>
  <div class="myinfo">
      <!-- 个人介绍结束 -->
    <p class="title"><Icon type="ios-contacts-outline" /> My Makefirend:</p>
      <Tabs class="tabs" value="name1"> 
        <TabPane label="友情链接" name="name1">
        <Icon type="md-laptop" /> <a href="http://8.140.1.97/">学涂网站，学会糊涂</a>
        <br/>
        <Icon type="md-laptop" /> <a href="https://space.bilibili.com/355348366">B站账号</a>
        </TabPane>
        <TabPane label="联系方式" name="name2">
          <i class="iconfont icon-qq" style="color:skyblue;"></i>:1275244661
          <i class="iconfont icon-weixin" style="color:green;"></i>:x1275244661
          <br>
          🙆‍♂️ 有问题欢迎小伙伴来学习讨论~
          <br>
          博客文章内容主要就是我在开发中遇到的一些常见的问题，记录下来让自己有个印象，应该也可以帮到你们，欢迎你们的光临，多多支持，over~
        </TabPane>
      </Tabs>
  </div>
</template>

<script>

  export default {
    name:'myinfo',
  }

</script>
<style lang="scss" scoped>
.myinfo {
  border: 1px solid #ccc;
  width: 100%;
  border-radius: 5px;
  .title {
    background: #fff;
    line-height:20px;
    padding:0.5rem;
    border-bottom:1.5px solid lightseagreen;
    font-size: 14px;
  }
  .tabs {
    padding: .5rem .8rem;
    position: relative;
    z-index: 5;
    background: #fff;
    color: #333;
    .ivu-tabs-tabpane {
      font-size: 13px;
    }
    a {
      color: #333;
    }
    a:hover {
      color: orange;
    }
  }
}
</style>