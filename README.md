### 表白利器 ball rotating

![动画展示](https://raw.githubusercontent.com/fridaydream/blogpic/master/ball-rotating.gif)

为了节约gif的大小，只录制了部分

[在线浏览](https://www.daxierhao.com/tomyfriend.html)


3个动画组成，和2个过渡时期

1、开始部分，5个球从左边移动到右边,然后在距离80的地方停止

```
function animation(ball, marginLeft) {
  return new Promise(function (resolve, reject) {
    function repeat() {
      var ballLeft = parseInt(ball.style.left);
      if (ballLeft == marginLeft) {
        resolve()
      } else {
        if (ballLeft > marginLeft) {
          ballLeft--;
        } else {
          ballLeft++;
        }
        ball.style.left = ballLeft + 'px';
        setTimeout(function() {
          repeat(ball, marginLeft);
        }, 20)
      }
    }
    setTimeout(repeat, 20);
  })
}

;(async function () {
  await animation(ball1, 30)
  await animation(ball2, 130)
  await animation(ball3, 230)
  await animation(ball3, 80)
  await animation(ball2, 80)
  await animation(ball1, 80)
  await animation(ball4, 80)
  await animation(ball5, 80)
  $(".ballWrap").addClass("swiperRotate");
  allTextRoate();
  setTimeout(function(){
    fallDown(0, 0, 0);
  }, 3000);
})();
```

通过js，每次left+1，控制5个球在80位置停下

2、在水平方向平移

```
function fallDown(index,stopTop,startSpeed){
  if(index==1){
    stopTop=-159;
    startSpeed=-159;       
  }
  var nowSpeed=startSpeed;
  var ele=$(".ball").eq(index);
  $(".ball").removeClass("zIndex100")
  ele.addClass("zIndex100");
  var timer=setInterval(function(){
    if(getHeight(ele)<=160&&nowSpeed>=0){
      nowSpeed++;
      ele.css("top",nowSpeed+'px');
      if(nowSpeed==160){
        nowSpeed=-nowSpeed;
      }
    }else{
      //clearInterval(timer);
      nowSpeed++;
      ele.css("top",-nowSpeed+'px');
      //console.log(nowSpeed,stopTop);
      if(nowSpeed==stopTop){
        clearInterval(timer);
        if(index===1){
          ele.css("top",160+'px');
          returnBack();
          return
        }
        stopTop=stopTop-40;  
        if (index === 0) {
          index = 3
        } else if (index === 3) {
          index = 2
        } else if (index === 2) {
          index = 4
        } else if (index === 4) {
          index = 1
        }
        startSpeed = stopTop;
        fallDown(index, stopTop, startSpeed);
      }
    }
  }, 20);
}
```

水平移动时需要注意球的顺序是0，3，2，4，1。因为要与最后的rotating结合，球的顺序是打乱的，移动的时候zIndex也要改变

3、无限rotating，这个是我看一个gif运动之后想到自己怎么通过代码实现，就使用上了

其实是3个正方形的运动，1号球和5号球一组，2号球和4号球一组，3号球单独一组

[github代码](https://github.com/fridaydream/cuteGril)

表白利器，欢迎fork和star