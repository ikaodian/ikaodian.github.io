---
title: get_the_category()详解
date: 2019-08-12
categories: [aihao]
category_slugs: [aihao]
original_id: 74
original_url: https://kaodian.org/?id=74
---
#  get_the_category() 获取当前文章的分类

wordpress主题函数 get_the_category() 详解使用

说明

获取与查询参数相匹配的类别对象

用法

<?php get_the_category( $id ) ?>  
参数

id

(整数) (可选) 文章编号

默认: $post->ID (当前文章的编号)

  


<?php get_the_category( $post->ID ) ?>

例子

获取指定文章编号的类别信息

<?php  
global $post;  
$categories = get_the_category($post->ID);  
var_dump($categories);  
?>  
返回的是一个数组，调用数组 如：echo $categories [0]->cat.name

cat_ID

类别编号，存储在term_id字段

cat_name

类别名称，存储在name字段

category_nicename

别名，存储在slug字段

category_description

类别描述，存储在description字段

category_parent

父类别编号，没有父类的为0，存储在parent字段

category_count

类别使用的数量，存储在count字段

  


  


显示类别的图片

<?php  
foreach((get_the_category()) as $category) {  
echo ‘<img src=”http://www.wpmee.com/images/’ . $category->cat_ID . ‘.jpg” alt=”‘ . $category->cat_name . ‘” />’;  
}  
?>  
显示第一个类别的名称

<?php  
$category = get_the_category();  
echo $category[0]->cat_name;  
?>

显示第一个类别的连接

<?php  
$category = get_the_category();  
if($category[0]){  
echo ‘<a href=”‘.get_category_link($category[0]->term_id ).'”>’.$category[0]->cat_name.'</a>’;  
}  
?>

  


**本文地址：**<https://www.kaodian.org/?id=74>  
**版权声明：** 本文为原创文章，版权归 [fengzhikuang](https://www.kaodian.org/?auth=1) 所有，欢迎分享本文，转载请保留出处！ 

[PREVIOUS:【最新】部编版九年级上册第2课《我爱这土地》知识点+图文解读](https://www.kaodian.org/?id=73)

[NEXT:真题汇编|2019年全国中考语文古诗文默写与运用](https://www.kaodian.org/?id=75)

## 文章导航

[__](https://www.kaodian.org/?id=73)

[__](https://www.kaodian.org/?id=75)
