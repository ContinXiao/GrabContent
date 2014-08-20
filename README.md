说明：
--
   1、这是一个使用基于行块提取算法的java实现,做了很多改进。
   2、针对与正文文本长度还没有广告长的页面，提取算法无法实现有效功能
   3、能够有效的提取出文章标题（基于权值的运算）、作者，写作时间等信息

优化：
--
1.行块步长的值根据页面动态生成
2.针对html页面不带<charset=>值的页面通过获取html包头形式得到。


改进：
--
算法缺点1;行块提取算法对html排版要求非常高，如果排版没有体现出行块区别,无法有效的提取
解决方案：首先清除html排版，再根据视觉呈现重新排版，在根据语意逻辑排版。

算法缺点1；针对正文内容比广告等干扰信息还短的文本
解决方案：同时抓取所有可能的文本块（文本块符合的条件是：波峰大于最大文本块波峰的1/3）,
          然后运算权值（文本密度，文本在文章位置，文本中标点符号多少等等）
案例：http://www.sandahb.com/show_gg.asp?id=846

算法缺点；如果一个正文由于排版问题从空间上隔绝成了很多块，但是事实上他们都是正文内容
解决方案：根据间隔之间长短，各个文本权值是否和上下文接近，浅文本特征来判断是否要连接到一起
案例：http://www.ynzmsj.com/index.htm

算法缺点；无法识别出该页面是一个导航页面还是一个内容页面
解决方案；根据提取出来的文本块，计算字数链接密度，行链接密度。进行权值判断
案例：http://www.hjxf.net/


待完善的地方：
--
待续~~



使用说明：
--
  1.把/lib/juniversalchardet-1.0.3.jar添加到编译路径中
  2.在/src/HtmlExtractorText.java中把你要爬取的网页url填入 HtmlResult r = e.extractContent("你的url")
  3.编译运行。Good Luck!
  
  
基本来源：
http://wenku.baidu.com/link?url=5NHqleEjLtPFhLJUa9h1Oz9bmms6XVezwfI9OQ8dUNdavj5DKTqqlGiba2ODi7Ll1mSiI57HLmiXmfTeuLu_Yiz1il9jnBmy1uwzF2g72fi

改进思路参考：
www.l3s.de/~kohlschuetter/publications/wsdm187-kohlschuetter.pdf
readability算法思想
n-gram算法思想

优势：上诉算法都是基于对非常规范化的大型网站的网页进行正文提取，对于小众的，编辑非常不规范的页面，提取正确率很低。本文基于行快提取算法，结合其他算法的思路
     在效率上比启发式机器学习算法效率高，比单纯的基于dom树分析算法高，比基于行快的提取算法正确率高～～
