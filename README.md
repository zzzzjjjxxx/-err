# mpvue-err1
用来记录我用mpvue写小程序过程中遇到的一些问题，总结！
1.怎么在原生的mpvue项目上增加，mpvue-entry，除了查看原生的文档之外，删除的时候，还需要好好利用，dist文件夹和no文件夹，重新编译。
# mpvue-err2
怎么制作一张展示的海报图片：
<div  @click='onSaveImg'>一个按钮
 // 先制作一个canvas标签，再保存成图片
    onSaveImg () {
      const that = this
      that.modalflag = true
      const ctx = wx.createCanvasContext('myCanvas') // 看回wxml里的canvas标签，这个的myCanvas要和标签里的canvas-id一致
      // ctx.createPattern('../../../../static/images/back.jpg', 'no-repeat')
      ctx.drawImage(that.downloadimgUrl, 0, 0, 300, 400)
      ctx.fillStyle = '#fff'
      ctx.fillRect(0, 400, 300, 100)
      ctx.drawImage(that.downloadqrCodeUrl, 180, 400, 100, 100)
      ctx.setFillStyle('#02446e')
      ctx.setFontSize(16)
      ctx.fillText('手机：' + that.phone, 30, 440)
      ctx.fillText('微信：' + that.weChat, 30, 460)
      ctx.fillText('地址：' + that.address, 30, 480)
      ctx.setTextAlign('center')
      var self = this
      ctx.draw(true, setTimeout(function () { // 为什么要延迟100毫秒？大家测试一下
        wx.canvasToTempFilePath({
          x: 0,
          y: 0,
          width: 300,
          height: 500,
          destWidth: 300,
          destHeight: 500,
          canvasId: 'myCanvas',
          success: function (res) {
            self.data.savedImgUrl = res.tempFilePath
            self.saveImageToPhoto()
          }
        })
      }, 500))
    },
    // 保存图片到相册
    saveImageToPhoto () {
      if (this.data.savedImgUrl !== '') {
        wx.saveImageToPhotosAlbum({
          filePath: this.data.savedImgUrl,
          success: function () {
            wx.showModal({
              title: '保存图片成功',
              content: '寻人启事已经保存到相册，您可以手动分享到朋友圈！',
              showCancel: false
            })
          },
          fail: function (res) {
            console.log(res)
            if (res.errMsg === 'saveImageToPhotosAlbum:fail cancel') {
              wx.showModal({
                title: '保存图片失败',
                content: '您已取消保存图片到相册！',
                showCancel: false
              })
            } else {
              wx.showModal({
                title: '提示',
                content: '保存图片失败，您可以点击确定设置获取相册权限后再尝试保存！',
                complete: function (res) {
                  console.log(res)
                  if (res.confirm) {
                    wx.openSetting({}) // 打开小程序设置页面，可以设置权限
                  } else {
                    wx.showModal({
                      title: '保存图片失败',
                      content: '您已取消保存图片到相册！',
                      showCancel: false
                    })
                  }
                }
              })
            }
          }
        })
      }
    }以上看似可以成功，但是当小程序直接在手机上运行的时候就失败了，这该怎么办呢！
    先把图片下载下来=>   wx.downloadFile({
        url: that.imgUrl,
        success: function (res) {
          console.log(res)
          if (res.statusCode === 200) {
            that.downloadimgUrl = res.tempFilePath
          }
        }
      })
      完成！
# mpvue-err3
业务需要，我需要传一张图片，通过提交表单的方法来把图片传递到后台中

# mpvue-video
这是最近找到的网站的原生的视频分享，我在git上下载的源码，可以通过http://uyi2.com
这里面的视频进行，一边看源码，一边看代码解析。码一下。

# 分享最近读的一本书的读书笔记
