<template>
  <div id="app">
    
    
    <router-link :to="{path:'clipboard'}">tiwen</router-link> 
    <el-row :gutter="8">
      <el-col :xs="{span: 24}" :sm="{span: 24}" :md="{span: 24}" :lg="{span: 24}" :xl="{span: 12}" style="padding-right:8px;margin-bottom:30px;">
        
       <bug-table />
      </el-col>
    </el-row>
    <el-row :gutter="8">
      <el-col :xs="{span: 24}" :sm="{span: 24}" :md="{span: 24}" :lg="{span: 24}" :xl="{span: 12}" style="padding-right:8px;margin-bottom:30px;">
       <li class="find"><router-link to="/clipboard">没有找到？那直接去提问吧</router-link></li>
      </el-col>  
    </el-row> 
  </div>
</template>


<script type="module">

import BugTable from './components/BugTable'

// note：标签用一个json文件搞定！
export default {

  name: 'Search',
  components: {
    BugTable,
  },

  data() {
    return {
      el:'#app',
      questions: [],
      state: '',
      seen: true,
      timeout: null,
      tableData: [{
          value: "改动原来代码后onLoad函数出错",
          reasons: "1.对函数内部的嵌套函数不清楚。2.onLoad函数没有删全，只删了上半部分，下半部分没有删掉",
          solve :"1.多去编写代码这个问题会被解决。2.检查一下onLoad函数的完整性。",
          tag :"onLoad函数" + " js"},
         { value: '图片无法显示（音频无法播放）（会出现报错：Failed to load local image resource /images/xx.png）',
          reasons: '1.图片（音频）的名称里面混有中文字符（包含中文逗号等）。2.是在代码中图片（音频）的路径不对' +
          '3.可能图片的位置被无意间改动了。4.图片命名里可能有空格。',
          solve: '1.检查一下图片（音频）的名称里面是否有中文字符，如果有，对图片（音频）文件重命名。2.检查一下代码里面所写的程序路径是否正确。' +
          '3.回想一下之前有没有把图片（音频）移动了位置，如果图片移动过位置的话，在小程序里面也要改相应的路径哟。4.注意这些路径里面的空格，可能会很坑！',
          tag: '图片' + ' 音频' + ' Failed to load'},
         { value: "显示上传成功了，但是数据库里面没有需要的东西，且没有报错（或者是数据上传得太慢了）",
          reasons: "1. wxml文件中与绑定上传相关的代码（如bindinput）拼写有误。2.有可能是data后面没有加数组下标（e.g：data[0]）。" +
          '（note: 这时会出现提示：Setting data field "xx" to undefined is invalid。因为data是数组名，无法从数组名中读取信息（具体原因将在c++课程中第四章学习））'
          + "3.数据库中的集合名和代码里面的集合名不一致。",
          solve :"1.尝试刷新，有时候这里反应很慢 2.试试检查一下wxml文件中有关的地方，这里拼写不对是不会报错的哟。3. 试试给data数组添加下标。" +
          "4 注意检查代码中的集合名与数据库中的集合名是否一致。",
          tag :"没有报错" + " js" + "上传" },
         { value: "在手写体识别时，出现签名串错误，在多次检查，重做后，问题没有解决。并且出现了新的报错：uploadFile：fail time out",
          reasons: "1.签名串本身有错误，比如说少了一个字母。2.签名串某一处（特别是结尾与开头处）出现空格。",
          solve :"1.检查一下签名串中是否出现空格，这个点很坑！2.检查一下签名串自身有没有缺少字母！",
          tag :"签名串错误" + " js" + "uploadFile" },
         { value: "读取数据库中数据时没有报错，但是就是读取不出来",
          reasons: "在云函数中读取函数中写的数据库名与js中读取函数中写的数据库名字不一样。",
          solve :"试试检查一下云函数中的数据库名与js中读取函数的数据库名是否一致。",
          tag :"读取" + " js" }
        ]
    }
  },
  mounted() {
    this.questions = this.loadAll()
  },
  methods: {
    loadAll() {
      return [
        { 'value': '页面无法正常显示，一片空白，出现报错：page is not defined',
          'reasons': '1.这往往是index.js里面Page的P没有大写导致的。2.也有可能是app.json文件里面页面路径配置不对。',
          'solve': '1.检查一下index.js文件中Page里面的P是否大写了。' + '2.检查一下app.json文件里面的路径配置' +
          '3.如果以上问题都已解决，但还是出现报错，那就说明在app.json文件里其他页面的路径有问题(如果有的话)。代码在有问题的页面卡住了，没有继续往后执行。 ' },
        { 'value': '给云函数安装依赖时怎么都不能成功',
          'reasons': '',
          'solve': ' 直接部署，不用安装，不会造成其他不良后果！如果不可行的话，那么试试检查自己的Node.JS是否安装成功，和自己的npm是否可以识别成功。' +
          '可以参考：https://blog.csdn.net/qq_36538012/article/details/85348019' },
        { 'value': '怎么改margin创造的边框的颜色（注：class=’margin’）',
          'reasons': '',
          'solve': ' 改颜色的话试试background-color属性' },
        { 'value': '已经显示正确的结果，但云函数那块（queey函数）还是有报错',
          'reasons': '1.修改云函数后忘记部署了。2. queey函数本身出现了一些小bug',
          'solve': ' 1.每次做修改后重新部署，并且刷新一下。2.仔细检查一下queey函数的内容是否正确（包含使用的逗号等）。' },
        { 'value': '什么时候用双引号什么时候用单引号？',
          'reasons': '',
          'solve': ' 都可以！有极少数情况会需要区分哟，感兴趣的话可以看看下面关于字符串部分的讲解: https://www.runoob.com/js/js-datatypes.html' },
        { 'value': 'js里注释是不能用<!--**-->但能用//吗',
          'reasons': '',
          'solve': ' <!--  -->用于 HTML；css 用 /**/；js /**/    //都可以；json文件没有注释！如果试图在json文件中添加注释，会直接标红线！(note:按ctrl+？就可以加注释了)' },
        { 'value': "js文件中xx定义为' '， 那' '代表了啥吗",
          'reasons': '',
          'solve': ' 其代表空字符串' },
        { 'value': '图片无法显示（音频无法播放）（会出现报错：Failed to load local image resource /images/xx.png）',
          'reasons': '1.图片（音频）的名称里面混有中文字符（包含中文逗号等），2.是在代码中图片（音频）的路径不对' +
          '3.可能图片的位置被无意间改动了。4.图片命名里可能有空格。',
          'solve': ' 1.检查一下图片（音频）的名称里面是否有中文字符，如果有，对图片（音频）文件重命名。2.检查一下代码里面所写的程序路径是否正确。' +
          '3.回想一下之前有没有把图片（音频）移动了位置，如果图片移动过位置的话，在小程序里面也要改相应的路径哟。4.注意这些路径里面的空格，可能会很坑！' },
        { 'value': '如何重返上一步操作？如何回退？',
          'reasons': '',
          'solve': ' ctrl+z' },
        { 'value': 'search云函数调用了数据库操作的api，为什么不直接在函数里调用api',
          'reasons': '',
          'solve': ' 这是可以的' },
        { 'value': '查找图片时往上翻文件夹是.. /，往下翻用什么？（如何用相对路径来使用名为xx的图片？）',
          'reasons': '',
          'solve': ' 往下翻用文件名就OK（e.g：../../images/gogogo.png）' },
        { 'value': ' 如何用绝对路径来使用名为xx的图片？',
          'reasons': '',
          'solve': ' 可以参考电脑文件的绝对路径，例如：C:\windows\system32\cmd.exe，是从盘开始的路径。' },
        { 'value': '为什么会提示"a"未定义呢（error：‘a’ is no defined）',
          'reasons': '1.在js文件中很可能是在没有定义变量a的情况下就使用了变量a。比如：Var b ，len   len=a+b ',
          'solve': ' 1.在js文件中尝试将a定义出来。2.可以试试在wxml文件中对变量a进行数据绑定。3.注意一下有没有与a相关的变量名打错了' },
        { 'value': '云控制台上显示了向数据库有读取记录，但是为什么hello集合里显示不出数据呢。昨天还可以，从今天开始就不行了？',
          'reasons': '',
          'solve': ' 1.试试重启，有可能是IDE问题。2.可能是在对数据进行删改的时候没有进行commit，切记要保存！' },
        { 'value': '云函数的回调结果不是预期的结果',
          'reasons': '1、有可能是在更改云函数代码后，没有重新部署 2、误改动原来的云函数',
          'solve': ' 1.重新部署云函数  2、仔细检查原来的函数是否正确' },
        { 'value': '为何在数据比较时出现报错？',
          'reasons': '1.数据绑定的方式不对。2.也有可能在比较时错误的用到了”=”。',
          'solve': ' 1.按照这样的形式进行数据绑定："{{xxx}}".2.试着检查一下是否出现了原因2中的情况，比较中有“=”号的地方替换为“==”号。' },
        { 'value': '请问读取时为何出错（提交正常，云数据库有正常显示）',
          'reasons': '1.有可能是将setData写成了诸如setData1，setData 2 这些不是自定义东西。（note: setData是原生语法）' +
          '2.函数内部嵌套时不能用this，如果这么做，会出现报错（this.setData is not a function; at api cloud. callFunction success callback function）',
          'solve': ' 1.检查是否出现类似于将setData写成setData1的情况。2.定义一个that来代替this的职责（note: var that=this）' },
        { 'value': ' 如何调整按钮大小?(字体的大小)',
          'reasons': '',
          'solve': '利用wxss中的属性size，设置size的值。E.g：size:30px;对于字体的大小，试试font-size属性。' +
          '具体可以参考：https://blog.csdn.net/wendyNo/article/details/105858153' },
        { 'value': '为何点击按钮后没有反应？',
          'reasons': '1.可能是js中与按钮反应相关的变量拼写错误。2.如果该按钮和页面跳转相关的话，有可能在app.json文件中没有对应的页面路径。' +
          '此外，在app.json文件中路径的名字不小心写错了也会出现类似的情况哟。',
          'solve': ' 1.检查js文件中与wxml文件中变量的拼写是否正确。2.检查一下对应路径有没有在app.json文件中。若没有，试试手动添加对应的路径。' +
          '其次，检查一下路径名是否正确，这个点很坑!' },
        { "value": "text和view是等效的吗？",
          "reasons": "",
          "solve" :"不是的，view有独占一行效果，（note:在这里view相当于html的div标签而text是文本标签，详情参考：https://blog.csdn.net/qq_44973159/article/details/104859304)" },
        { "value": "改动原来代码后onLoad函数出错",
          "reasons": "1.对函数内部的嵌套函数不清楚。2.onLoad函数没有删全，只删了上半部分，下半部分没有删掉",
          "solve" :"1. 多去编写代码这个问题会被解决“2.检查一下onLoad函数的完整性。"},

      ]
    },
    querySearchAsync(queryString, cb) {
      var questions = this.questions
      var results = queryString ? questions.filter(this.createStateFilter(queryString)) : questions
      cb(results)

    },
    createStateFilter(queryString){
      return (state) => {
        return (state.value.toLowerCase().indexOf(queryString.toLowerCase()) === located(state,queryString))
      }
      this.seen = false;
    },
   /*handleSelect(item) {
      console.log(this.tableData.length)
      console.log(item)
      while(this.tableData.length !=0)
      {
        this.tableData.pop()
      }
      this.seen = false
      this.tableData.push(item)
    },
    handleChange(item){
      console.log(item.split("").length)
      if(item.split("").length == 0)
      {
        this.seen = true
        var data = [
         {
          value: "改动原来代码后onLoad函数出错",
          reasons: "1.对函数内部的嵌套函数不清楚。2.onLoad函数没有删全，只删了上半部分，下半部分没有删掉",
          solve :"1.多去编写代码这个问题会被解决。2.检查一下onLoad函数的完整性。",
          tag :"onLoad函数" + " js"},
         { value: '图片无法显示（音频无法播放）（会出现报错：Failed to load local image resource /images/xx.png）',
          reasons: '1.图片（音频）的名称里面混有中文字符（包含中文逗号等）。2.是在代码中图片（音频）的路径不对' +
          '3.可能图片的位置被无意间改动了。4.图片命名里可能有空格。',
          solve: '1.检查一下图片（音频）的名称里面是否有中文字符，如果有，对图片（音频）文件重命名。2.检查一下代码里面所写的程序路径是否正确。' +
          '3.回想一下之前有没有把图片（音频）移动了位置，如果图片移动过位置的话，在小程序里面也要改相应的路径哟。4.注意这些路径里面的空格，可能会很坑！',
          tag: '图片' + ' 音频' + ' Failed to load'},
         { value: "显示上传成功了，但是数据库里面没有需要的东西，且没有报错（或者是数据上传得太慢了）",
          reasons: "1. wxml文件中与绑定上传相关的代码（如bindinput）拼写有误。2.有可能是data后面没有加数组下标（e.g：data[0]）。" +
          '（note: 这时会出现提示：Setting data field "xx" to undefined is invalid。因为data是数组名，无法从数组名中读取信息（具体原因将在c++课程中第四章学习））'
          + "3.数据库中的集合名和代码里面的集合名不一致。",
          solve :"1.尝试刷新，有时候这里反应很慢 2.试试检查一下wxml文件中有关的地方，这里拼写不对是不会报错的哟。3. 试试给data数组添加下标。" +
          "4 注意检查代码中的集合名与数据库中的集合名是否一致。",
          tag :"没有报错" + " js" + "上传" },
         { value: "在手写体识别时，出现签名串错误，在多次检查，重做后，问题没有解决。并且出现了新的报错：uploadFile：fail time out",
          reasons: "1.签名串本身有错误，比如说少了一个字母。2.签名串某一处（特别是结尾与开头处）出现空格。",
          solve :"1.检查一下签名串中是否出现空格，这个点很坑！2.检查一下签名串自身有没有缺少字母！",
          tag :"签名串错误" + " js" + "uploadFile" },
         { value: "读取数据库中数据时没有报错，但是就是读取不出来",
          reasons: "在云函数中读取函数中写的数据库名与js中读取函数中写的数据库名字不一样。",
          solve :"试试检查一下云函数中的数据库名与js中读取函数的数据库名是否一致。",
          tag :"读取" + " js" }
        ]
        this.tableData.pop();
        for(var i=0;i<data.length;i++)
        {
           this.tableData.push(data[i])
        }
      }
    }*/
  }
}
function located(state,queryString){
  var posi;
  // 如果输入的词与记录不匹配，不予显示
  if(state.value.toLowerCase().indexOf(queryString.toLowerCase())!== -1) {
    posi = state.value.toLowerCase().indexOf(queryString.toLowerCase());
  }
  return posi;
}
</script>
<style>
  .demo-table-expand {
    font-size: 0;
  }
  .demo-table-expand label {
    width: 90px;
    color: #99a9bf;
  }
  .demo-table-expand .el-form-item {
    margin-right: 0;
    margin-bottom: 0;
    width: 50%;
  }
  .find{
    font-size: 13px;
    color: rgb(102, 189, 230);
    margin-left: 1000px;
  }
</style>
