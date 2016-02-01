---
layout: post
title: "处理wordpress上传中文名附件乱码问题"
date: 2015-09-19 21:59:40
categories: Wordpress
tags: wordpress
---

wordpress上传中文名附件乱码问题如何解决，在谷歌刨了半天找出一段处理wordpress上传中文名附件乱码问题的代码：

{% highlight php startinline %} 

function upload_file($filename) {
$parts = explode('.', $filename);
$filename = array_shift($parts);
$extension = array_pop($parts);
foreach ( (array) $parts as $part)
$filename .= '.' . $part;
 
if(preg_match('/[一-龥]/u', $filename)){
$filename = md5($filename);
}
$filename .= '.' . $extension;
return $filename ;
}
add_filter('sanitize_file_name', 'upload_file', 5,1);

{% endhighlight %}

代码的效果是判断附件的文件名如果是中文就自动改成md5的储存名，测试了下该代码目前还是有效的，另外有些wordpress小白可能不知道代码加在哪，小编在这里提示下，此段代码要加到当前主题的functions.php文件中，或者functions.php包含的文件中。
