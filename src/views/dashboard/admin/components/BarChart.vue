<template>
  <el-table :data="tableData.filter(data => !search || data.name.toLowerCase().includes(search.toLowerCase()))" height="400" style="width: 100%" highlight-current-row @current-change="handleCurrentChange">
    <el-table-column type="expand">
      <template slot-scope="props">
        <el-form label-position="left" inline class="demo-table-expand">
          <el-form-item label="问题">
            <span>{{ props.row.name }}</span>
          </el-form-item>
          <el-form-item label="回答">
            <span>{{ props.row.reason }}</span>
          </el-form-item>          
        </el-form>
      </template>
    </el-table-column>
    <el-table-column
      type="index"
      width="50">
    </el-table-column>
    <el-table-column
      label="问题"
      prop="name">
    </el-table-column>
    <el-table-column
      prop="tag"
      label="标签"
      :filters="[{ text: 'json', value: 'json' }, { text: 'wxss', value: 'wxss' },{text: 'wxml', value: 'wxml'},{text: 'js',value: 'js'},{text: '云函数',value: '云函数'},{text: '数据库',value: '数据库'},{text: '报错',value: '报错'},{text: '路径',value: '路径'},{text: '预览',value: '预览'}]"
      :filter-method="filterTag"
      filter-placement="bottom-end">
      <template slot-scope="scope">
        <el-tag
          :type="scope.row.tag"
          disable-transitions>{{scope.row.tag}}</el-tag>
      </template>
    </el-table-column>
    <el-table-column prop="warehouseName" label="库房" :show-overflow-tooltip="true" header-align="left" align="left">
        <template slot-scope="scope">
            {{scope.row.warehouseName}}
        </template>
        <template slot="header" slot-scope="scope">
            <el-row :span="24">
                <el-col :span="6" class="custom-title"
                    style="padding-top: 5px;color:rgb(12, 123, 164); font-size:12px;line-height: 2;">{{scope.row}}</el-col>
                <el-col :span="18" class="search-before-content">
                    <el-input v-model="search" size="mini" placeholder="输入关键字搜索" />
                </el-col>
            </el-row>
            <hr class="hr-custom-style" />
        </template>
    </el-table-column>
  </el-table>
  
</template>

<script>
import tableRouter from '../../../../router/modules/table'
  export default {
    data() {
      return {
        tableData: [{
          
          name: '报错类型：page is not defined',
          reason: '原因：这往往是index.js里面Page的P没有大写导致的，也有可能是app.json文件里面页面路径配置不对；解决方法：1.检查一下index.js文件中Page里面的P是否大写了。2.检查一下app.json文件里面的路径配置',
          tag: '报错'
        }, {
          
          name: '给云函数安装依赖时怎么都不能成功',
          reason: '直接部署，不用安装，不会造成其他不良后果',
          tag: '云函数'
        },{
          
          name: '改动原来代码后onLoad函数出错',
          reason: '1：对函数内部的嵌套函数不清楚（多去编写代码这个问题会被解决）;2：onLoad函数没有删全，只删了上半部分，下半部分没有删掉',
          tag: 'js'
        },{
          
          name:'怎么改margin创造的边框的颜色',
          reason: '这是错误的认识，  margin不创造边框，边框用border。',
          tag: 'wxss'
        },{
          
          name: '数据库不显示上传的内容',
          reason: '解决方案：1.尝试刷新 2.注意：集合名里面的内容和代码里面的要一致。',
          tag: '数据库', 
        },{
          
          name: '已经显示正确的结果，但云函数那块（queey函数）还是有报错',
          reason: '解决方案：1.每次做修改后重新部署，并且刷新一下。2.仔细检查一下queey函数的内容是否正确（包含使用的逗号等）。',
          tag: '云函数'
        },{
          
          name: '什么时候用双引号什么时候用单引号？',
          reason:'都可以！有极少数情况会需要区分哟',
          tag:'wxml'
        },{
          
          name: ' js里注释是不能用<!--**-->但能用//吗',
          reason: '<!--  -->用于 HTML；css 用 /**/；js /**/    //都可以；json文件没有注释！如果试图在json文件中添加注释，会直接标红线！',
          tag: 'js'
        },{
          
          name: '（js文件中）xx定义为" "， 那" "代表了啥吗',
          reason: '其代表空字符串',
          tag: 'js'
        },{
          
          name: '图片显示不出来，音频放不出来（会出现报错：Failed to load local image resource /images/xx.png）',
          reason: '原因：1.图片（音频）的名称里面混有中文字符（包含中文逗号等），2.是在代码中图片（音频）的路径不对3.可能图片的位置被无意间改动了。4.图片命名里可能有空格。解决方案：1.检查一下图片（音频）的名称里面是否有中文字符，如果有，对图片（音频）文件重命名。2.检查一下代码里面所写的程序路径是否正确。3.回想一下之前有没有把图片（音频）移动了位置，如果图片移动过位置的话，在小程序里面也要改相应的路径哟。4.注意这些路径里面的空格，可能会很坑！',
          tag: 'wxml'
        },{
          
          name: '如何重返上一步操作？回退？',
          reason: 'ctrl+z',
          tag: '撤销'
        },{
          
          name: 'search云函数调用了数据库操作的api，为什么不直接在函数里调用api',
          reason: '这是可以的',
          tag: '云函数'
        },{
          
          name: '查找图片时往上翻文件夹是.. \，往下翻用什么？（如何用绝对路径来使用名为xx的图片？）',
          reason: '是../作为上翻，往下翻用文件名就OK（e.g：../../images/gogogo.png）',
          tag: '路径'
        },{
          
          name: '既然app,json文件可以配置小程序页面，那么它和index相关文件又有什么从属关系呢',
          reason: 'index相关文件优先，app.json作为备用，有相同的配置项以page.json为准。但是在实际运用中，我们经常会配置app.json的内容哟',
          tag: 'json'
        },{
          
          name: 'text和view是等效的吗？',
          reason: '不是的，view有独占一行效果，在这里view相当于html的div标签而text是文本标签',
          tag:'wxml'
        },{
          
          name: '如何用绝对路径来使用名为xx的图片？',
          reason: '可以参考html中的绝对路径，例如：C:\windows\system32\cmd.exe，是从盘开始的路径。',
          tag: '路径'
        },{
          
          name: '为什么会提示‘a’未定义呢（error：‘a’ is no defined）',
          reason: '原因：1.在js文件中很可能是在没有定义变量a的情况下就使用了变量a。比如：Var b ，len len=a+b  解决方案：1.在js文件中尝试将a定义出来。2.可以试试在wxml文件中对变量a进行数据绑定。3.注意一下有没有与a相关的变量名打错了',
          tag: '报错'
        },{
          
          name: '云控制台上显示了向数据库有读取记录，但是为什么hello集合里显示不出数据呢。昨天还可以，从今天开始就不行了？',
          reason: '可能是在对数据进行删改的时候没有进行commit，切记要保存！',
          tag: '云函数'
        },{
          
          name: '云函数的回调结果不是预期的结果',
          reason: '原因：1、有可能是在更改云函数代码后，没有重新部署 2、误改动原来的云函数 解决方案：1.重新部署云函数  2、仔细检查原来的函数',
          tag: '云函数'
        },{
          
          name: '为何在数据比较时出现报错？',
          reason: '原因：1.数据绑定的方式不对。2.也有可能在比较时错误的用到了”=”。解决方案：1.按照这样的形式进行数据绑定："{{xxx}}".2.试着检查一下是否出现了原因2中的情况，比较中有“=”号的地方替换为“==”号。',
          tag: 'js'
        },{
          
          name: '请问读取时为何出错（提交正常，云数据库有正常显示）',
          reason: '原因：1.有可能是将setData写成了诸如setData1，setData 2 这些不是自定义东西。（note: setData是原生语法）2.函数内部嵌套时不能用this，如果这么做，会出现报错（this.setData is not a function; at api cloud. callFunction success callback function）解决方案：1.检查是否出现类似于将setData写成setData1的情况。2.定义一个that来代替this的职责（note: var that=this）',
          tag: '云函数'
        },{
          
          name: '如何调整按钮大小',
          reason: '利用wxss中的属性，打开微信开发者工具，开wxml文件，wxml文件中写上button标签,里面写上文字，然后设置size的值，设置按钮的类型。',
          tag: 'wxss'
        },{
          
          name: '为何点击按钮后没有反应？',
          reason: '原因：1.可能是js中与按钮反应相关的变量拼写错误。2.如果该按钮和页面跳转相关的话，有可能在app.json文件中没有对应的页面路径。此外，在app.json文件中路径的名字不小心写错了也会出现类似的情况哟。解决方案：1.检查js文件中与wxml文件中变量的拼写是否正确。2.检查一下对应路径有没有在app.json文件中。若没有，试试手动添加对应的路径。其次，检查一下路径名是否正确，这个点很坑！',
          tag: 'js'
        },{
          
          name: '没有报错，但是没有（在页面中）输出结果',
          reason: '原因：1.可能是wxml里没有绑定输出。2.也有可能是wxml文件中与绑定输出相关的变量不小心拼写错了(与js文件中的不一致)  解决方案：1.如果检查到第一种情况，试试在wxml文件中添加相关输出的数据绑定。2.检查一下变量拼写哟',
          tag: '报错'
        },{
          
          name: '调字体大小用什么属性啊，是font-size吗',
          reason: '是的！',
          tag: 'wxss'
        },{
          
          name: '想要让图片只在一个区间内显示，该怎么编写if语句呢',
          reason: '考虑一下利用else if语句',
          tag: 'js'
        },{
          
          name: '云开发控制台打不开了',
          reason: '试试重启微信开发者工具',
          tag: '云函数'
        },{
          
          name: '为什么没有显示出正确的样式？',
          reason: '原因：1.wxml文件中class里面的部分与wxss中的样式不太一样 2.wxss中样式设定得不对  解决方法：1.仔细检查class中书写的内容 2.试试看一下wxss文件里面样式名是否正确。',
          tag: 'wxss'
        },{
          
          name: '页面为何消失了',
          reason: '原因：1.很可能是js文件中的Page没有了（如果是这样，控制栏（console）会出现这样的提示：Page "pages/index/index" has not been registered yet. ）2.程序中出现了一些语法错误  解决方案：1.给js文件加上Page，2.如果是第二种情况，试试修改程序中出现的语法错误。3，如果之前页面是正常的，现在忽然打不开了，那么可能是IDE的问题，试试重启',
          tag: 'js'
        },{
          
          name: '如何修改云函数中的环境名？',
          reason: '改不了，只能重新新建一个环境了。',
          tag: '云函数'
        },{
          
          name: '请问js里那个冒号（正确的标点）是怎么打出来的，我打出来是红色的',
          reason: '这是中文的冒号（标点）',
          tag: 'js'
        },{
          
          name: '让一个变量除2取余，如果得0是一张图片（一个结果），得1是另一张图片（另一种结果），要怎么写if和else语句',
          reason: '假如这个变量是a，a%2即可。0为False，1为True，False时显示一张图片，True时显示另外一张图片',
          tag: 'js'
        },{
          
          name: '读取数据库中数据时没有报错，但是就是读取不出来',
          reason: '原因：在云函数中读取函数中写的数据库名与js中读取函数中写的数据库名字不一样。（本人出现过这样的情况） 解决方案：试试检查一下云函数中的数据库名与js中读取函数的数据库名是否一致，很有可能出现了这种情况。',
          tag: '数据库'
        },{
          
          name: '如何获取字符串的长度？',
          reason: '试试str.length，如果是回调的数据，试试e.detail.value.str,length(note: str对应已经定义的字符串的名字)',
          tag: 'js'
        },{
          
          name: '运行的时候不报错，一输数字就报错了',
          reason: '原因：1.没有向js文件传递参数。（如果是这样，会出现报错：e is not defined; at pages/index/index page xx function）2.也有可能是定义的参数与传递时的参数不一致（如果是这样，会出现报错：xx变量 is not defined; at pages/index/index page xx function）解决方案：试试在函数的（）中填入e作为参数',
          tag: 'wxml'
        },{
          
          name: '出现报错page is not constructed because it is not found',
          reason: '原因：这种一般是引入路径不对，移动了下位置，先前引入的相对地址就报错了。解决方法：修复console下的所有错误提示。（tip：console控制台中，不要只看当前报错信息，往上翻，寻找到所有报错信息，并且尝试解决。）',
          tag: '报错'
        },{
          
          name: '怎么能表示等于',
          reason: '使用==，类似于c语言',
          tag: 'js'
        },{
          
          name: ' 鼠标放上去没有可选择的环境咋办',
          reason: '1.试试创建环境。2.也有可能是IDE出问题了，试试重启',
          tag: '云函数'
        },{
          
          name: '请问一下为什么云函数同步不了呢？',
          reason: '可能是没有选择云开发环境',
          tag: '云函数'
        },{
          
          name: '请问下这里的e.detail.value是什么意思，没有在别的地方出现过',
          reason: '试试利用console（控制台）看看这个是怎么来的，很有用哟',
          tag: 'js'
        },{
          
          name: '为什么input1输出不了',
          reason: '有可能是把bind之类的东西打错了，注意wxml代码的细节！',
          tag: 'wxml'
        },{
          
          name: '显示上传成功了，但是数据库里面没有需要的东西，且没有报错（或者是数据上传得太慢了）',
          reason: '原因：1. wxml文件中与绑定上传相关的代码（如bindinput）拼写有误。2.有可能是data后面没有加数组下标（e.g：data[0]）。（note: 这时会出现提示：Setting data field "xx" to undefined is invalid。因为data是数组名，无法从数组名中读取信息（具体原因将在c++课程中第四章学习））解决方案：1.试试检查一下wxml文件中有关的地方，这里拼写不对是不会报错的哟。2. 试试给data数组添加下标。',
          tag: '数据库'
        },{
          
          name: ' console.log()里面的c大写对吗',
          reason: 'console.log()里面的 c需要小写',
          tag: 'js'
        },{
          name: ' 输出了［Object,object&#93;是怎么回事？',
          reason: '原因：直接输出会调用toString方法（note: toString() 方法用于返回以一个字符串表示的 Number 对象值,实际上就是这里输出的乱码）解决方法：试试在js文件中的读取函数补上wxml文件中承接打印内容的变量。',
          tag: 'js'
        },{
          name: '为什么number后面接＝报错但是接＞不报错',
          reason: '“=”是赋值符号，”>”是大于号，要表示两个值相等，请用”==”，这些符号的运用类似于c++，可以参考一下。',
          tag: '报错'
        },{
          name: '渲染出的页面怎么变得更好一些 比如有个颜色',
          reason: '试试利用wxss文件中的样式。就像网页前端中的css一样，详情可以参考css（note：background 设置背景颜色, box-shadow设置阴影）',
          tag: 'wxss'
        },{
          name: '我配置了两个环境，如何删掉其中的一个（亦或如何改变代码初始化默认环境）',
          reason: '试试修改云能力初始化函数wx.cloud.init()的参数。如果有两个环境，则修改env属性（这个属性默认为空）为自己需要的环境的环境名',
          tag: '云函数'
        },{
          name: '为什么没有边框呀（wxss）',
          reason: '注意solid的拼写！',
          tag: 'wxss'
        },{
          name: '请问wxss中各种样式哪里有？',
          reason: '1.查看官方文档 2.查看百度相关内容（ps：css的样式特别多，在用的时候百度就OK，如果想深入学习CSS可忽略）',
          tag: 'wxss'
        },{
          name: '这个wx未定义是什么问题呀（wx is no defined）',
          reason: '原因：1、可能是把小程序代码一不小心写进云函数里了。2、在服务端获取数据库不需要wx命名空间，云函数上const db = wx.cloud.database这一句把wx.去掉，官方文档里分为小程序端和服务端，这两个是有区别的   解决方案：仔细检查代码',
          tag: 'js'
        },{
          name: '这个black是从wxss里面拿的吗？',
          reason: '如果是改变字体颜色的话，在<body></body>标签里面利用<style>标签可以改变字体颜色或者用<div>标签改变块记元素的颜色，具体可以参考百度教程。',
          tag: 'wxss'
        },{
          
          name: '问一下，index是不是相当于全局的一个子目录，然后app文件就是相当于全局的？',
          reason: 'app.json 用来对微信小程序进行全局配置，决定页面文件的路径、窗口表现、设置网络超时时间、设置多 tab 等，每一个小程序页面都必须在app.json文件中进行注册,而index.json 是指页面的配置文件，这个是非必要的。当有页面的配置文件时，配置项在该页面会覆盖app.json的window中的相同的配置项。',
          tag: 'json'
        },{
          name: '云开发没有权限是什么问题？',
          reason: '1、重启一下开发者程序（权限开通会有一定的时间）2、云函数所在目录缺少一个权限声明文件。',
          tag: '云函数'
        },{
          name: ' 文本就是自己打的字吗？',
          reason: '对的，如果想要设置文本输入，可以利用text area进行文本输入。',
          tag: 'wxml'
        },{
          name: '有人知道如何把图片居中不	？',
          reason: '',
          tag: 'wxss'
        },{
          name: '如何改变按钮，字体等颜色？',
          reason: '1.可以直接利用style，来写入样式，如：<button style=”font-size: 40px; color: red”>点我试试！</button> 对于<text>与<view>标签，也可以这么做！利用id or class设定属性名，利用属性名在wxss文件中设定样式如：<view class=”text”>利用属性名也可以在wxss文件中设定样式</view>',
          tag: 'wxml'
        },{
          name: ' 问一个外行问题，小程序的ui怎么美化	？',
          reason: '如果想要设计一个好的UI那么可以试试自己添加图片，改变颜色，调整字距哦。想进一步美化ui？试试利用以下的ui框架:1.	微信小程序官方ui库——weui 2.	color-ui 3.	vant weapp',
          tag: 'wxss'
        },{
          name: '请问做完这一步ctrl s没反应是啥情况?',
          reason: '有可能是微信开发者工具无响应，等待一下即可。2. 如果这个现象出现了较长的时间，试试重启IDE',
          tag: '报错'
        },{
          name: ' 在read过程中为什么result里面没有data？',
          reason: '原因: 1.有可能数据库里面本身就没有存进数据。 2. 云函数书写不规范。3.云函数里面忘写return了 4. 数据返回的形式不对  解决方案：1、检查一下数据库，看看有没有存进数据，如果没有，检查一下上传环节是否出现问题 2.规范云函数的书写代码 3.如果忘写return了，试试补上return还有返回的值。4.检查一下数据返回的形式 5、也有可能数据库的权限设置不合理，这个时候试试配置权限（这种情况少见，但是涉及到一些大创or互联网+的项目中可能会碰到这种问题）',
          tag: '云函数'
        },{
          name: '请问这个black是从wxss中拿的吗？',
          reason: '是的！这么写是这个意思：这个时候wxml里的{{color}}部分就被替换成了black，就相当于class=”black”，然后再去wxss里面找.black样式。',
          tag: 'wxss'
        },{
          name: '在js文件中如何赋值一个变量已经有值怎么赋给另一个变量？',
          reason: '可以在百度上面依据实际情况进行调用不同函数，现在不要求掌握js的太多用法。 ',
          tag: 'js'
        },{
          name: '请问到底怎么才能把变量上传到云数据库里？',
          reason: '先建立云函数，然后使用request库内容进行传递，然后查询安装node和npm，进行安装request。最后在函数右端选择云端安装不依赖上传。详情可以参考火花空间',
          tag: '云函数'
        },{
          name: '请问界面预览图怎么弄？',
          reason: '微信开发者工具的右上方有预览的按钮，点击生成二维码扫描即可。',
          tag: '预览'
        },{
          name: ' 怎么改变背景色啊？',
          reason: '在app.json中的window中可以进行修改导航栏的背景颜色，可以在app.wxss文件中利用background-color来设定整个小程序页面的颜色',
          tag: 'json'
        }],
        search:''
      }
    },
    computed: {
        // 模糊搜索
        tables () {
            const search = this.search;
            if (search) {
                // filter() 方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。
                // 注意： filter() 不会对空数组进行检测。
                // 注意： filter() 不会改变原始数组。
                return this.tableData.filter(data => {
                    // some() 方法用于检测数组中的元素是否满足指定条件;
                    // some() 方法会依次执行数组的每个元素：
                    // 如果有一个元素满足条件，则表达式返回true , 剩余的元素不会再执行检测;
                    // 如果没有满足条件的元素，则返回false。
                    // 注意： some() 不会对空数组进行检测。
                    // 注意： some() 不会改变原始数组。
                    return Object.keys(data).some(key => {
                        // indexOf() 返回某个指定的字符在某个字符串中首次出现的位置，如果没有找到就返回-1；
                        // 该方法对大小写敏感！所以之前需要toLowerCase()方法将所有查询到内容变为小写。
                        return String(data[key]).toLowerCase().indexOf(search) > -1
                    })
                })
            }
            return this.tableData
        }
    },
    methods: {
      resetDateFilter() {
        this.$refs.filterTable.clearFilter('date');
      },
      clearFilter() {
        this.$refs.filterTable.clearFilter();
      },
      formatter(row, column) {
        return row.address;
      },
      filterTag(value, row) {
        return row.tag === value;
      },
      filterHandler(value, row, column) {
        const property = column['property'];
        return row[property] === value;
      },
      setCurrent(row) {
        this.$refs.singleTable.setCurrentRow(row);
      },
      handleCurrentChange(val) {
        this.currentRow = val;
      }
    }
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
</style>
