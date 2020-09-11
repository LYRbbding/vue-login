<template>
  <el-collapse accordion>
  <el-collapse-item>
    <template slot="title">
      报错类型：page is not defined
    </template>
    <div>原因：这往往是index.js里面Page的P没有大写导致的，也有可能是app.json文件里面页面路径配置不对</div>
    <div>解决方法：1.检查一下index.js文件中Page里面的P是否大写了。2.检查一下app.json文件里面的路径配置</div>
    <div>后果：页面无法正常显示，一片空白</div>
  </el-collapse-item>
  <el-collapse-item title="给云函数安装依赖时怎么都不能成功" name="2">
    <div>解决方法：直接部署，不用安装，不会造成其他不良后果</div>
  </el-collapse-item>
  <el-collapse-item title="改动原来代码后onLoad函数出错" name="3">
    <div>原因1：对函数内部的嵌套函数不清楚（多去编写代码这个问题会被解决)</div>
    <div>复现：</div>
    <div class="demo-image__preview">
      <el-image style="width: 100px; height: 100px" :src="url1" :preview-src-list="srcList1"></el-image>
    </div>
    <div>原因2：onLoad函数没有删全，只删了上半部分，下半部分没有删掉</div>
  </el-collapse-item>
  <el-collapse-item title="怎么改margin创造的边框的颜色" name="4">
    <div>这是错误的认识,margin不创造边框，边框用border。</div>
  </el-collapse-item>
  <el-collapse-item title="数据库不显示上传的内容" name="5">
    <div>解决方案：1.尝试刷新2.注意：集合名里面的内容和代码里面的要一致</div>
  </el-collapse-item>
  <el-collapse-item title="已经显示正确的结果，但云函数那块（queey函数）还是有报错" name="6">
    <div>解决方案：1.每次做修改后重新部署  2.仔细检查一下queey函数的内容是否正确（包含使用的逗号等）</div>
  </el-collapse-item>
  <el-collapse-item title="什么时候用双引号什么时候用单引号？" name="7">
    <div>都可以！</div>
  </el-collapse-item>
  <el-collapse-item title="js里注释是不能用<!--**-->但能用//吗" name="8">
    <div>用于 HTML；css 用 /**/；js /**/    //都可以；json文件没有注释</div>
  </el-collapse-item>
  <el-collapse-item title=" xx定义为' '    那' '代表了啥吗" name="9">
    <div>其代表空字符串</div>
  </el-collapse-item>
  <el-collapse-item title="（wxml中）图片显示不出来，音频放不出来" name="10">
    <div>原因：1.图片（音频）的名称里面混有中文字符（包含中文逗号等），2.是在代码中图片（音频）的路径不对3.图片命名里可能有空格</div>
    <div>解决方案：1.检查一下图片（音频）的名称里面是否有中文字符，如果有，对图片（音频）文件重命名。2.检查一下代码里面所写的程序路径是否正确。3.想一下之前有没有改过文件路径，改过的话，在小程序里面也要改哟。4.注意这些路径里面的空格，可能会很坑！</div>
  </el-collapse-item>
  <el-collapse-item title="如何重返上一步操作？回退？" name="11">
    <div>ctrl+z</div>
  </el-collapse-item>
  <el-collapse-item title="search云函数调用了数据库操作的api，为什么不直接在函数里调用api" name="12">
    <div>这是可以的</div>
  </el-collapse-item>
  <el-collapse-item title="查找图片时往上翻文件夹是.. \，往下翻用什么？" name="13">
    <div>是../作为上翻，往下翻用文件名就OK</div>
  </el-collapse-item>
  <el-collapse-item title="既然app,json文件可以配置小程序页面，那么它和index相关文件又有什么从属关系呢" name="14">
    <div>index相关文件优先，app.json作为备用，有相同的配置项以page.json为准</div>
  </el-collapse-item>
  <el-collapse-item title="text和view是等效的吗" name="15">
    <div>不是的，view有独占一行效果</div>
  </el-collapse-item>
  <el-collapse-item title="如何用绝对路径来使用名为xx的图片？" name="16">
    <div>先上倒到page页，然后在指向xx，再调用图片</div>
  </el-collapse-item>
  <el-collapse-item title="为什么会提示'a'未定义呢（error：‘a’ is no defined）" name="17">
    <div>原因：1.在js文件中很可能是在没有定义变量a的情况下就使用了变量a。比如：Var b ，len   len=a+b </div>
    <div>解决方案：1.在js文件中尝试将a定义出来。2.可以试试在wxml文件中对变量a进行数据绑定。3.注意一下有没有与a相关的变量名打错了</div>
  </el-collapse-item>
  <el-collapse-item title="云函数的回调结果不是预期的结果" name="18">
    <div>原因：有可能是在更改云函数代码后，没有重新部署</div>
    <div>解决方案：重新部署云函数</div>
  </el-collapse-item>
  <el-collapse-item title="为何在数据比较时出现报错？" name="19">
    <div>原因：1.数据绑定的方式不对，2.也有可能在比较时错误的用到了”=”</div>
    <div>解决方案：1.按照这样的形式进行数据绑定："{{xxx}}".2.试着检查一下是否出现了原因2中的情况，比较中有“=”号的地方替换为“==”号。</div>
  </el-collapse-item>
  <el-collapse-item title="请问读取时为何出错（提交正常，云数据库有正常显示）" name="20">
    <div>原因：</div>
    <div>1.有可能是将setData写成了诸如setData1，setData 2 这些不是自定义东西。（note: setData是原生语法）</div>
    <div>2.函数内部嵌套时不能用this，如果这么做，会出现报错（this.setData is not a function; at api cloud. callFunction success callback function）</div>
    <div>解决方案：1.检查是否出现类似于将setData写成setData1的情况。2.定义一个that来代替this的职责（note: var that=this）</div>
  </el-collapse-item>
  <el-collapse-item title="如何调整按钮大小" name="21">
    <div>利用wxss中的属性</div>
  </el-collapse-item>
  <el-collapse-item title="为何点击按钮后没有反应？" name="22">
    <div>原因：可能是js中与按钮反应相关的变量拼写错误</div>
    <div>解决方案：检查js文件中与wxml文件中变量的拼写是否正确</div>
  </el-collapse-item>
  <el-collapse-item title="没有报错，但是没有结果输出" name="23">
    <div>原因：很可能是wxml里没有绑定输出的结果</div>
    <div>解决方案：试试在wxml文件中添加相关输出的数据绑定</div>
  </el-collapse-item>
  <el-collapse-item title="调字体大小用什么属性啊，是font-size吗" name="24">
    <div>是的是的</div>
  </el-collapse-item>
  <el-collapse-item title="想要让图片只在一个区间内显示，该怎么编写if语句呢" name="25">
    <div>考虑一下利用else if语句</div>
  </el-collapse-item>
  <el-collapse-item title="云开发控制台打不开了" name="26">
    <div>解决方案：重启微信开发者工具</div>
  </el-collapse-item>
  <el-collapse-item title="为什么没有显示出正确的样式？" name="27">
    <div>原因：1.wxml文件中class里面的内容书写不正确 2.wxss中样式设定得不对</div>
    <div>解决方法：1.仔细检查class中书写的内容 2.试试看一下wxss中设定的样式是否正确。</div>
  </el-collapse-item>
  <el-collapse-item title="页面为何消失了" name="28">
    <div>原因：1.很可能是js文件中的Page没有了（如果是这样，控制栏（console）会出现这样的提示：Page "pages/index/index" has not been registered yet. ）2.程序中出现了一些语法错误</div>
    <div>解决方案：1.给js文件加上Page，2.如果是第二种情况，试试修改程序中出现的语法错误</div>
  </el-collapse-item>
  <el-collapse-item title="如何修改云函数中的环境名？" name="29">
    <div>改不了，试试新建一个环境</div>
  </el-collapse-item>
  <el-collapse-item title="请问js里那个冒号（正确的标点）是怎么打出来的，我打出来是红色的" name="30">
    <div>原因：这是中文的冒号（标点）</div>
    <div>解决方案：按shift将中文输入切换成英文，再输入冒号即可</div>
  </el-collapse-item>
  <el-collapse-item title="让一个变量除2取余，如果得0是一张图片（一个结果），得1是另一张图片（另一种结果），要怎么写if和else语句" name="31">
    <div>假如这个变量是a，a%2即可。0为False，1为True，False时显示一张图片，True时显示另外一张图片</div>
  </el-collapse-item>
  <el-collapse-item title="读取数据库中数据时没有报错，但是就是读取不出来" name="32">
    <div>原因：在云函数中读取函数中写的数据库名与js中读取函数中写的数据库名字不一样。（本人出现过这样的情况）</div>
    <div>解决方案：试试检查一下云函数中的数据库名与js中读取函数的数据库名是否一致，很有可能出现了这种情况。</div>
  </el-collapse-item>
  <el-collapse-item title="如何获取字符串的长度？" name="33">
    <div>试试str.length，如果是回调的数据，试试e.detail.value.str,length(note: str对应已经定义的字符串的名字)</div>
  </el-collapse-item>
  <el-collapse-item title="运行的时候不报错，一输数字就报错了" name="34">
    <div>原因：1.没有向js文件传递参数。（如果是这样，会出现报错：e is not defined; at pages/index/index page xx function）2.也有可能是定义的参数与传递时的参数不一致（如果是这样，会出现报错：xx变量 is not defined; at pages/index/index page xx function）</div>
    <div>解决方案：试试在函数的（）中填入e作为参数</div>
  </el-collapse-item>
  <el-collapse-item title="出现报错page is not constructed because it is not found" name="35">
    <div>原因：这种一般是引入路径不对，移动了下位置，先前引入的相对地址就报错了。</div>
    <div>解决方法：修复console下的所有错误提示。（tip：console控制台中，不要只看当前报错信息，往上翻，寻找到所有报错信息，并且尝试解决。）</div>
  </el-collapse-item>
  <el-collapse-item title="怎么能表示等于" name="36">
    <div>==</div>
  </el-collapse-item>
  <el-collapse-item title="鼠标放上去没有可选择的环境咋办" name="37">
    <div>试试创建环境</div>
  </el-collapse-item>
  <el-collapse-item title="请问一下为什么云函数同步不了呢？" name="38">
    <div>可能是没有选择环境，试试选择一下云开发环境</div>
  </el-collapse-item>
  <el-collapse-item title="请问下这里的e.detail.value是什么意思，没有在别的地方出现过" name="39">
    <div>试试利用console（控制台）看看这个是怎么来的，很有用哟</div>
  </el-collapse-item>
  <el-collapse-item title="为什么input1输出不了" name="40">
    <div>原因：有可能是把bind之类的东西打错了，注意wxml代码的细节！</div>
  </el-collapse-item>
  <el-collapse-item title="显示上传成功了，但是数据库里面没有需要的东西，且没有报错（或者是数据上传得太慢了）" name="41">
    <div>原因：1. wxml文件中与绑定上传相关的代码（如bindinput）拼写有误。2.有可能是data后面没有加数组下标（e.g：data[0]）。（note: 这时会出现提示：Setting data field "xx" to undefined is invalid。因为data是数组名，无法从数组名中读取信息（具体原因将在c++课程中第四章学习））</div>
    <div>解决方案：1.试试检查一下wxml文件中有关的地方，这里拼不对是不会报错的哟。2. 试试给data数组添加下标。</div>
  </el-collapse-item>
  <el-collapse-item title="console.log()里面的c大写对吗" name="42">
    <div>console.log()里面的 c需要小写</div>
  </el-collapse-item>
  <el-collapse-item title="输出了［Object,object&#93;是怎么回事？" name="43">
    <div>原因：直接输出会调用toString方法（note: toString() 方法用于返回以一个字符串表示的 Number 对象值,实际上就是这里输出的乱码）</div>
    <div>解决方法：试试在js文件中的读取函数补上wxml文件中承接打印内容的变量。</div>
  </el-collapse-item>
  <el-collapse-item title="为什么number后面接＝报错但是接＞不报错" name="44">
    <div>“=”是赋值符号，”>”是大于号，要表示两个值相等，请用”==”</div>
  </el-collapse-item>
  <el-collapse-item title="渲染出的页面怎么变得更好一些 比如有个颜色" name="45">
    <div>试试利用wxss文件中的样式。（note：background 设置背景颜色, box-shadow设置阴影）</div>
  </el-collapse-item>
  <el-collapse-item title="我配置了两个环境，如何删掉其中的一个（亦或如何改变代码初始化默认环境）" name="46">
    <div>回答：试试修改云能力初始化函数wx.cloud.init()的参数。如果有两个环境，则修改env属性（这个属性默认为空）为自己需要的环境的环境名</div>
    <div>示例：</div>
    <div class="demo-image__preview">
      <el-image style="width: 100px; height: 100px" :src="url2" :preview-src-list="srcList2"></el-image>
    </div>
    <div>如果想了解更多关于该初始化函数的内容，试试看一下本文档的内容哟：</div>
    <a href="https://developers.weixin.qq.com/miniprogram/dev/wxcloud/reference-client-api/init.html">https://developers.weixin.qq.com/miniprogram/dev/wxcloud/reference-client-api/init.html</a>
  </el-collapse-item>
  <el-collapse-item title="为什么没有边框呀（wxss）" name="47">
    <div>注意solid的拼写！</div>
  </el-collapse-item>
  <el-collapse-item title="请问wxss中各种样式哪里有？" name="48">
    <div>1.试试官方文档，官方文档地址：</div>
    <a href="https://developers.weixin.qq.com/">https://developers.weixin.qq.com/</a>
    <div>2.试试百度相关内容（ps：css的样式特别多，在用的时候百度就OK，如果想深入学习CSS可忽略）</div>
  </el-collapse-item>
  <el-collapse-item title="这个wx未定义是什么问题呀（wx is no defined）" name="49">
    <div>原因：可能是吧小程序代码一不小心写进云函数里了</div>
  </el-collapse-item>
  <el-collapse-item title="这个black是从wxss里面拿的吗？" name="50">
    <div>如果是改变字体颜色的话，在body标签里面利用style标签可以改变字体颜色或者用div标签改变块记元素的颜色，具体可以参考百度教程。</div>
  </el-collapse-item>
  <el-collapse-item title="问一下，index是不是相当于全局的一个子目录，然后app文件就是相当于全局的？" name="51">
    <div>app.json 用来对微信小程序进行全局配置，决定页面文件的路径、窗口表现、设置网络超时时间、设置多 tab 等，每一个小程序页面都必须在app.json文件中进行注册,而index.json 是指页面的配置文件，这个是非必要的。当有页面的配置文件时，配置项在该页面会覆盖app.json的window中的相同的配置项。</div>
  </el-collapse-item>
  <el-collapse-item title="云开发没有权限是什么问题？" name="52">
    <div>1、重启一下开发者程序（权限开通会有一定的时间）</div>
    <div>2、云函数所在目录缺少一个权限声明文件。</div>
  </el-collapse-item>
  <el-collapse-item title="文本就是自己打的字吗？" name="53">
    <div>对的，如果想要设置文本输入，可以利用textarea进行文本输入。</div>
  </el-collapse-item>
  <el-collapse-item title="如何把图片居中" name="54">
    <div>在home.wxss中进行调试</div>
    <div class="demo-image__preview">
      <el-image style="width: 100px; height: 100px" :src="url3" :preview-src-list="srcList3"></el-image>
    </div>
  </el-collapse-item>
  <el-collapse-item title="摁钮颜色怎么改?" name="55">
    <div>可以直接在style中写入，如：</div>
    <div class="demo-image__preview">
      <el-image style="width: 100px; height: 100px" :src="url4" :preview-src-list="srcList4"></el-image>
    </div>
  </el-collapse-item>
  <el-collapse-item title="怎么给字体改变颜色？" name="56">
    <div>如果简单的改变显示的字体的颜色，可以直接在某个class下面text直接提供样式。</div>
  </el-collapse-item>
  <el-collapse-item title="小程序的ui怎么美化	？" name="57">
    <div>如果想要设计一个好的UI那么同学可以自己添加图片，改变颜色，调整字距哦。</div>
  </el-collapse-item>
  <el-collapse-item title="请问做完这一步ctrl s没反应是啥情况?" name="58">
    <div>有可能是微信开发者工具无响应，等待一下即可。</div>
  </el-collapse-item>
  <el-collapse-item title="在read过程中为什么result里面没有data？" name="59">
    <div>1、数据库的权限设置</div>
    <div>2、云函数里面的return没有写</div>
    <div>3、云函数书写不规范</div>
    <div>4、数据返回的形式不对</div>
  </el-collapse-item>
  <el-collapse-item title="请问这个black是从wxss中拿的吗？" name="60">
    <div>这么写之后，wxml里的{{color}}部分就被替换成了black，就相当于class=”black”，然后再去wxss里面找.black样式。</div>
  </el-collapse-item>
  <el-collapse-item title="怎么安装这个云函数依赖呢？" name="61">
    <div>详情可参考:</div>
    <a href="https://blog.csdn.net/qq_36538012/article/details/85348019">https://blog.csdn.net/qq_36538012/article/details/85348019</a>
    <div>或</div>
    <a href="https://blog.csdn.net/xjun0812/article/details/87973081?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase">
    https://blog.csdn.net/xjun0812/article/details/87973081?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase
    </a>
  </el-collapse-item>
  <el-collapse-item title="在js文件中如何赋值一个变量已经有值怎么赋给另一个变量？" name="62">
    <div>可以在百度上面依据实际情况进行调用不同函数，现在不要求掌握js的太多用法。</div>
  </el-collapse-item>
  <el-collapse-item title="请问到底怎么才能把变量上传到云数据库里？" name="63">
    <div>先建立云函数，然后使用request库内容进行传递，然后查询安装node和npm，进行安装request。最后在函数右端选择云端安装不依赖上传。详情可以参考火花空间</div>
  </el-collapse-item>
  <el-collapse-item title="请问界面预览图怎么弄？" name="64">
    <div>微信开发者工具的右上方有预览的按钮，点击生成二维码扫描即可。</div>
  </el-collapse-item>
  <el-collapse-item title="怎么改变背景色啊？" name="65">
    <div>在app.json中的window中可以进行修改背景颜色（修改的颜色使用十六进制可在百度自己查找）</div>
  </el-collapse-item>
  <el-collapse-item title="parselnt（）函数为啥未被定义？" name="66">
    <div>通常来说函数未被定义有如下原因：</div>
    <div>1.没有添加路径</div>
    <div>2.添加的里没有该h文件</div>
    <div>3.同一个工程下有多个同名的h文件，恰好被引用的h文件中没有这个函数</div>
  </el-collapse-item>
  <el-collapse-item title="如何把表单数据上传到云端？" name="67">
    <div>先将form表单写入Page的data，接着再从data里取出来写入数据库，详细可以参考火花空间关于云函数上传数据的教程。</div>
  </el-collapse-item>
  <el-collapse-item title="input里字体颜色怎么改？" name="68">
    <div>有内联式和外嵌式两种方法，举一个内联式的例子：</div>
    <div class="demo-image__preview">
      <el-image style="width: 100px; height: 100px" :src="url5" :preview-src-list="srcList5"></el-image>
    </div>
  </el-collapse-item>
</el-collapse>
</template>

<script>
  export default {
    data() {
      return {
        activeNames: ['1'],
        fits: ['fill', 'contain', 'cover', 'none', 'scale-down'],
        url1: 'http://m.qpic.cn/psc?/V13QIILW3g4iNW/UbDh**QEFrkBQEvKgeO0F5sgcFATU01CLKyAyaPMM21MtY2svwCrZ8iqpl2ZY8*Cj3lnp4L0GWCFJbWlxQZWcA!!/mnull&bo=bwEfAQAAAAADB1I!&rf=photolist&t=5',
        srcList1:[
          'http://m.qpic.cn/psc?/V13QIILW3g4iNW/UbDh**QEFrkBQEvKgeO0F5sgcFATU01CLKyAyaPMM21MtY2svwCrZ8iqpl2ZY8*Cj3lnp4L0GWCFJbWlxQZWcA!!/mnull&bo=bwEfAQAAAAADB1I!&rf=photolist&t=5'
        ],
        url2: 'http://m.qpic.cn/psc?/V13QIILW3g4iNW/UbDh**QEFrkBQEvKgeO0FxmTl4uLl6.BYGomo*qOUzdQQpkmkMeY9BsPKUdNDw3H3Vd*B0XysafpnmPRZI9pzw!!/mnull&bo=KgFzACoBcwADCSw!&rf=photolist&t=5',
        srcList2:[
          'http://m.qpic.cn/psc?/V13QIILW3g4iNW/UbDh**QEFrkBQEvKgeO0FxmTl4uLl6.BYGomo*qOUzdQQpkmkMeY9BsPKUdNDw3H3Vd*B0XysafpnmPRZI9pzw!!/mnull&bo=KgFzACoBcwADCSw!&rf=photolist&t=5'
        ],
        url3: 'http://m.qpic.cn/psc?/V13QIILW3g4iNW/UbDh**QEFrkBQEvKgeO0F6XMIxREQuvIb0m3rFovshu6uQ8HvmuYc0Y6BRe.kBJMI3mZRCOuUZ85FKT*YlAr9Q!!/mnull&bo=GQHJAAAAAAADB*M!&rf=photolist&t=5',
        srcList3:[
          'http://m.qpic.cn/psc?/V13QIILW3g4iNW/UbDh**QEFrkBQEvKgeO0F6XMIxREQuvIb0m3rFovshu6uQ8HvmuYc0Y6BRe.kBJMI3mZRCOuUZ85FKT*YlAr9Q!!/mnull&bo=GQHJAAAAAAADB*M!&rf=photolist&t=5'
        ],
        url4: 'http://m.qpic.cn/psc?/V13QIILW3g4iNW/UbDh**QEFrkBQEvKgeO0FwMoct1C2I.LClqQkidJKoj83mveNSVsYsnpaQBvXoPdTY5Ph1XBmal75dD8HR3L8w!!/mnull&bo=KgIlAAAAAAADBy8!&rf=photolist&t=5',
        srcList4:[
          'http://m.qpic.cn/psc?/V13QIILW3g4iNW/UbDh**QEFrkBQEvKgeO0FwMoct1C2I.LClqQkidJKoj83mveNSVsYsnpaQBvXoPdTY5Ph1XBmal75dD8HR3L8w!!/mnull&bo=KgIlAAAAAAADBy8!&rf=photolist&t=5'
        ],
        url5: 'http://m.qpic.cn/psc?/V13QIILW3g4iNW/UbDh**QEFrkBQEvKgeO0F6DNh3mzplqSvr8Vr*DADbrnrICxh5sN5W9RCdbttyEDuIypr0gS8d*nSjKvRkFaZQ!!/mnull&bo=KgIsAAAAAAADByY!&rf=photolist&t=5',
        srcList5:[
          'http://m.qpic.cn/psc?/V13QIILW3g4iNW/UbDh**QEFrkBQEvKgeO0F6DNh3mzplqSvr8Vr*DADbrnrICxh5sN5W9RCdbttyEDuIypr0gS8d*nSjKvRkFaZQ!!/mnull&bo=KgIsAAAAAAADByY!&rf=photolist&t=5'
        ]
      };
    },
    
  }
</script>

<style>
  .text {
    font-size: 14px;
  }

  .item {
    margin-bottom: 18px;
  }

  .clearfix:before,
  .clearfix:after {
    display: table;
    content: "";
  }
  .clearfix:after {
    clear: both
  }

  .box-card {
    width: 800px;
  }
</style>
