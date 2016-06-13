	---
layout:     post
title:      "formdata+php 简易图片上传"
subtitle:   "formdata、php"
date:       2016-06-12 13:20:30
author:     "iMaple"
header-img: "img/post-bg-re-vs-ng2.jpg"
header-mask: 0.3
catalog:    true
tags:
    - javascript
---

直接贴代码

css

```
.uploadWrapper{position:relative;height:120px;}
.uploadImg{width:100px;height:100px;border-radius:10px;vertical-align:top;position:absolute;left:10px;}
.uploadBtn{margin-left:120px;display:inline-block;vertical-align:top;position:relative;margin-right:10px;overflow:hidden;display:block;background:#fff;height:30px;line-height:30px;border-radius:5px;text-align:center;top:50%;transform:translateY(-50%);-webkit-transform:translateY(-50%);-moz-transform:translateY(-50%);-o-transform:translateY(-50%);box-shadow:1px 1px 3px rgba(0,0,0,0.5);}
#uploadify{opacity:0;width:100%;height:100%;position:absolute;top:0;left:0;}
```

html

```
<div class="uploadWrapper">
	<input type="hidden" class="imgSrc">
	<img src="http://bac.yqlfy.com/images/nopic.gif" class="uploadImg"><a href="javascript:;" class="uploadBtn">+ 请选择上传文件 <input id="uploadify" type="file"/></a>
</div>
```

js

```
     var picData = null;
		$('#uploadify').on({
			change : function(){
				picData = new FormData();

				picData.append("file", $(this)[0].files[0]);

				$.ajax({
				  url: "upPic.php",
				  type: 'POST',
				  dataType: 'json',
				  cache: false,
				  data: picData,
				  //必须false才会自动加上正确的Content-Type
				  contentType:false,
				  //必须false才会避开jQuery对 formdata 的默认处理 XMLHttpRequest会对 formdata 进行正确的处理
				  processData:false
				}).done(function(result){
				  if(result.isSuccess){
				  	alert(result.msg, 1500);
				  	$('.uploadImg').attr('src', result.src);
        			$('.imgSrc').val(result.src)
				  }else{
				  	alert(result.msg, 1500);
				  }
				}).fail(function(err){
				  console.info(err);
				});
			}
		});
```

upPic.php

```
<?php  
$a_path = "D:/wwwroot/bac_yqlfy_com_1l0uub/web/m/upload/"; //绝对路径
$path = "upload/"; //相对路径
 
$extArr = array("jpg", "png", "gif"); 

$response = array();
$response['isSuccess'] = false;
 
if(isset($_POST) and $_SERVER['REQUEST_METHOD'] == "POST"){ 
    $name = $_FILES['file']['name']; 
    $size = $_FILES['file']['size']; 
    
    if(empty($name)){ 
    	$response['msg'] = '请选择要上传的图片';
        echo json_encode($response); 
        exit; 
    } 
    $ext = extend($name); 
    if(!in_array($ext,$extArr)){ 
        $response['msg'] = '图片格式错误！';
        echo json_encode($response); 
        exit; 
    } 
    if($size>(2048*1024)){ 
        $response['msg'] = '图片大小不能超过2M';
        echo json_encode($response); 
        exit; 
    } 
    $image_name = time().rand(100,999).".".$ext; 
    $tmp = $_FILES['file']['tmp_name']; 
    if(move_uploaded_file($tmp, $a_path.$image_name)){ 
        $response['isSuccess'] = true;
    	$response['msg'] = '上传成功！';
        $response['src'] = $path.$image_name;
        echo json_encode($response); 
    }else{ 
        $response['msg'] = '上传出错了！';
        echo json_encode($response); 
    } 
    exit; 
} 
 
//获取文件类型后缀 
function extend($file_name){ 
    $extend = pathinfo($file_name); 
    $extend = strtolower($extend["extension"]); 
    return $extend; 
} 
?>
```

效果展示

![](http://o83pigran.bkt.clouddn.com/uploadBefore.png)

![](http://o83pigran.bkt.clouddn.com/uploadSuccess.png)