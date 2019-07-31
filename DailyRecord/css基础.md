#一.css基本属性:
##1.width
####设置方式分为百分比,像素,厘米(%,px,em);默认为像素;
	例:	div	{width:100px}
####min-width和max-width可以分别设置最大和最小宽度;
	注:可以使用max-width:1000px;_width:expression((document.documentElement.clientWidth||document.body.clientWidth)<1000?"1000px":"");这种类型的表达式来实现最大和最小宽度;
	参考:https://www.cnblogs.com/mrdooo/p/6633688.html; document.documentElement和document.body的区别
##2.height
####设置方式可以是auto,像素,厘米,单独对一个div高度设置为百分比没有效果;
	table的td/th,img标签设置时,height不能设置单位,默认为px;
####lineheight:行高属性,用于设置文字的高度,一般用于实现垂直居中效果
	在一排文字或内容布局中,如果要让内容上下垂直居中,只需要设置line-height和height高度相同,即可实现垂直居中;如果是多列的文章或内容,通常会设置每一行文字平均上下间隔,此时只需要设置line-height即可
####min-width和max-width限制最大和最小高度,设置方式和ie6兼容同min-height和max-height
####overflow:hidden;该设置可以隐藏超出DIV宽度高度的内容(包括图片);
	text-overflow:clip|ellipsis;该设置可以使得文本溢出时,多余内容的显示方式;clip:简单的裁切,不显示省略标记;ellipsis:溢出时显示省略标记(...);如果仅设置text-overflow还需要嵌套<nobr></nobr>以避免浏览器换行
#3.border