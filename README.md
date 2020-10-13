# QCElementToImg
html2canvas的使用，将element生成为图片
---
#### 一、安装三方包

##### 1.在package.json中引入

```
  "dependencies": {
    ...

    "html2canvas": "^1.0.0-rc.5"
  }
```
##### 2.npm install安装


#### 二、具体的使用

##### 1.在使用文件中引入
```
import html2canvas from 'html2canvas';//element转图片
```
##### 2.给需要转换的元素装ref，方便逻辑中拿到元素
```
<div class="bg" ref="share">

</div>
```
##### 3.转成图片
```
getImg(){
    return new Promise((response,reject)=>{
        try{
            // 需要截图的包裹的（原生的）DOM 对象
            var shareContent = this.$refs.share
            // 获取dom 宽度
            var width = shareContent.offsetWidth 
            // 获取dom 高度
            var height = shareContent.offsetHeight 
            // 创建一个canvas节点
            var canvas = document.createElement('canvas') 
            // 定义任意放大倍数 支持小数
            var scale = 1 
            // 定义canvas 宽度 * 缩放
            canvas.width = width * scale 
            // 定义canvas高度 *缩放
            canvas.height = height * scale 
            // 获取context,设置scale
            canvas.getContext('2d').scale(scale, scale) 
            // 配置
            var opts = {
            scale: 1, // scale 参数
            canvas: canvas, // 自定义 canvas
            logging: false, // 日志开关，便于查看html2canvas的内部执行流程
            width: width, // dom 原始宽度
            height: height,
            useCORS: true // 【重要】开启跨域配置
            }
            html2canvas(shareContent, opts).then(canvas => {
                response(canvas.toDataURL()) // 图片地址
            })

        } catch(e){

            reject(e)
        
        }
    })
},


```
##### 4.在需要使用图片的地方展开

```
this.getImg()
    .then(res=>{

        let uploadImg = res.substring(22) || '' //截掉'data:image/png;base64,'
        if(uploadImg.length>0){
            //进行保存或者分享操作
        }else{
            //提示保存失败
        }
    })
    .catch(()=>{
        //提示保存失败
    })

```