<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI虚拟试衣大屏原型</title>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: Arial, "Microsoft YaHei", sans-serif;
  background: #f5f3ef;
  color: #171717;
}

button {
  font: inherit;
  cursor: pointer;
  border: 0;
}

.app {
  width: 100vw;
  height: 100vh;
  min-height: 680px;
  overflow: hidden;
  background: linear-gradient(135deg, #f7f4ef, #eee8df);
}

.screen {
  display: none;
  width: 100%;
  height: 100%;
  padding: 38px 56px;
  position: relative;
}

.screen.active {
  display: block;
}

/* 顶部 */
.logo {
  font-size: 25px;
  font-weight: 800;
  letter-spacing: 1px;
}

.brand {
  font-size: 13px;
  color: #777;
  margin-top: 5px;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.step {
  font-size: 14px;
  color: #777;
}

/* 首页 */
.hero {
  height: calc(100% - 90px);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.hero h1 {
  font-size: 64px;
  margin: 0 0 16px;
}

.hero p {
  font-size: 22px;
  color: #666;
  margin: 0 0 38px;
}

/* 按钮 */
.primary {
  background: #171717;
  color: white;
  border-radius: 14px;
  padding: 19px 52px;
  font-size: 22px;
  font-weight: 700;
  box-shadow: 0 8px 20px #0002;
}

.secondary {
  background: white;
  color: #222;
  border: 1px solid #ddd;
  border-radius: 12px;
  padding: 14px 25px;
}

/* 卡片 */
.grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 18px;
  margin-top: 35px;
}

.card {
  background: white;
  border-radius: 18px;
  padding: 20px;
  box-shadow: 0 5px 18px #0000000c;
  border: 2px solid transparent;
}

.card.selected {
  border-color: #171717;
}

.avatar {
  height: 250px;
  border-radius: 14px;
  background: linear-gradient(160deg, #d8c8bb, #f3eee8);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 95px;
}

.cloth {
  height: 250px;
  border-radius: 14px;
  background: linear-gradient(160deg, #e4ddd5, #faf9f6);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 100px;
}

.card h3 {
  margin: 14px 0 5px;
}

.card p {
  margin: 0;
  color: #777;
}

/* 底部操作 */
.actions {
  position: absolute;
  bottom: 38px;
  left: 56px;
  right: 56px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

/* AI等待页 */
.loading {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
}

.loader {
  width: 86px;
  height: 86px;
  border: 7px solid #ddd;
  border-top-color: #171717;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin: auto;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.loading h1 {
  font-size: 38px;
}

.progress {
  width: 520px;
  height: 8px;
  background: #ddd;
  border-radius: 8px;
  overflow: hidden;
  margin: 25px auto;
}

.bar {
  height: 100%;
  width: 0;
  background: #171717;
  transition: width .5s;
}

.loading-reco {
  display: flex;
  gap: 15px;
  justify-content: center;
  margin-top: 25px;
}

.reco {
  background: white;
  padding: 12px 18px;
  border-radius: 12px;
  color: #555;
}

/* 结果页 */
.result-layout {
  height: calc(100% - 100px);
  display: grid;
  grid-template-columns: 1.1fr .9fr;
  gap: 42px;
  align-items: center;
}

.result-img {
  height: 70vh;
  max-height: 650px;
  border-radius: 25px;
  background: linear-gradient(160deg, #d7c8bc, #f2eee9);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 180px;
}

.info h1 {
  font-size: 42px;
  margin: 0 0 10px;
}

.price {
  font-size: 28px;
  font-weight: 700;
  margin: 18px 0;
}

.tag {
  display: inline-block;
  padding: 7px 12px;
  background: #e8e1d9;
  border-radius: 20px;
  color: #555;
}

/* 模拟二维码 */
.qr {
  width: 170px;
  height: 170px;
  margin: 25px 0;
  background:
    repeating-linear-gradient(
      45deg,
      #111 0 4px,
      #fff 4px 8px
    );
  border: 12px solid white;
  box-shadow: 0 4px 15px #0002;
}

/* Toast */
.toast {
  position: fixed;
  top: 25px;
  left: 50%;
  transform: translateX(-50%);
  background: #171717;
  color: #fff;
  padding: 13px 22px;
  border-radius: 30px;
  display: none;
  z-index: 9;
}

/* 移动端适配 */
@media(max-width: 900px) {

  .screen {
    padding: 25px;
  }

  .grid {
    grid-template-columns: repeat(2, 1fr);
  }

  .hero h1 {
    font-size: 42px;
  }

  .result-layout {
    grid-template-columns: 1fr;
  }

  .result-img {
    height: 45vh;
  }

  .actions {
    left: 25px;
    right: 25px;
  }

  .qr {
    width: 130px;
    height: 130px;
  }
}
</style>
</head>

<body>

<div class="app">

<!-- Toast提示 -->
<div id="toast" class="toast"></div>


<!-- ==================== -->
<!-- 01 首页 -->
<!-- ==================== -->

<section class="screen active" id="s1">

  <div class="header">

    <div>
      <div class="logo">
        BRAND AI FIT
      </div>

      <div class="brand">
        AI VIRTUAL TRY-ON
      </div>
    </div>

    <div class="step">
      免费体验 · 约1–3分钟
    </div>

  </div>


  <div class="hero">

    <h1>
      AI虚拟试衣
    </h1>

    <p>
      看看这件衣服，穿在你身上是什么效果
    </p>

    <button
      class="primary"
      onclick="go(2)"
    >
      立即开始体验 →
    </button>

  </div>

</section>



<!-- ==================== -->
<!-- 02 人物选择 -->
<!-- ==================== -->

<section class="screen" id="s2">

  <div class="header">

    <div class="logo">
      选择人物
    </div>

    <div class="step">
      1 / 3
    </div>

  </div>

  <p>
    选择一个模特，或使用你的照片进行试衣
  </p>


  <div class="grid">

    <!-- 人物1 -->

    <div
      class="card selected"
      onclick="selectCard(this)"
    >

      <div class="avatar">
        👩🏻
      </div>

      <h3>
        女性 · 经典
      </h3>

      <p>
        预设人物
      </p>

    </div>


    <!-- 人物2 -->

    <div
      class="card"
      onclick="selectCard(this)"
    >

      <div class="avatar">
        👩🏽
      </div>

      <h3>
        女性 · 活力
      </h3>

      <p>
        预设人物
      </p>

    </div>


    <!-- 人物3 -->

    <div
      class="card"
      onclick="selectCard(this)"
    >

      <div class="avatar">
        👨🏻
      </div>

      <h3>
        男性 · 简约
      </h3>

      <p>
        预设人物
      </p>

    </div>


    <!-- 用户照片 -->

    <div
      class="card"
      onclick="photoMode()"
    >

      <div class="avatar">
        📷
      </div>

      <h3>
        使用我的照片
      </h3>

      <p>
        拍照前查看隐私说明
      </p>

    </div>

  </div>


  <div class="actions">

    <button
      class="secondary"
      onclick="go(1)"
    >
      返回
    </button>

    <button
      class="primary"
      onclick="go(3)"
    >
      下一步 →
    </button>

  </div>

</section>



<!-- ==================== -->
<!-- 03 服装选择 -->
<!-- ==================== -->

<section class="screen" id="s3">

  <div class="header">

    <div class="logo">
      选择服装
    </div>

    <div class="step">
      2 / 3
    </div>

  </div>


  <p>
    选择你想试穿的款式
  </p>


  <div class="grid">

    <!-- 服装1 -->

    <div
      class="card selected"
      onclick="selectCard(this)"
    >

      <div class="cloth">
        👗
      </div>

      <h3>
        轻盈连衣裙
      </h3>

      <p>
        ¥699 · 米白
      </p>

    </div>


    <!-- 服装2 -->

    <div
      class="card"
      onclick="selectCard(this)"
    >

      <div class="cloth">
        🧥
      </div>

      <h3>
        城市风外套
      </h3>

      <p>
        ¥899 · 驼色
      </p>

    </div>


    <!-- 服装3 -->

    <div
      class="card"
      onclick="selectCard(this)"
    >

      <div class="cloth">
        👚
      </div>

      <h3>
        简约针织衫
      </h3>

      <p>
        ¥499 · 灰色
      </p>

    </div>


    <!-- 服装4 -->

    <div
      class="card"
      onclick="selectCard(this)"
    >

      <div class="cloth">
        👔
      </div>

      <h3>
        轻商务套装
      </h3>

      <p>
        ¥1,199 · 黑色
      </p>

    </div>

  </div>


  <div class="actions">

    <button
      class="secondary"
      onclick="go(2)"
    >
      返回
    </button>

    <button
      class="primary"
      onclick="generate()"
    >
      生成试衣效果 →
    </button>

  </div>

</section>



<!-- ==================== -->
<!-- 04 AI生成等待 -->
<!-- ==================== -->

<section class="screen" id="s4">

  <div class="loading">

    <div>

      <div class="loader"></div>

      <h1 id="loadingText">
        正在分析人物
      </h1>

      <div class="progress">

        <div
          class="bar"
          id="bar"
        ></div>

      </div>

      <p id="percent">
        预计15秒内完成 · 0%
      </p>


      <!-- 等待期间商品推荐 -->

      <div class="loading-reco">

        <div class="reco">
          这款还有3种颜色
        </div>

        <div class="reco">
          同系列热门款式
        </div>

      </div>

    </div>

  </div>

</section>



<!-- ==================== -->
<!-- 05 AI结果 -->
<!-- ==================== -->

<section class="screen" id="s5">

  <div class="header">

    <div class="logo">
      试衣效果
    </div>

    <div class="step">
      3 / 3 · AI生成完成
    </div>

  </div>


  <div class="result-layout">


    <!-- 左侧试衣效果 -->

    <div class="result-img">

      👩🏻

      <span style="font-size:80px">
        👗
      </span>

    </div>


    <!-- 右侧商品信息 -->

    <div class="info">

      <span class="tag">
        AI虚拟试穿
      </span>


      <h1>
        轻盈连衣裙
      </h1>


      <p>
        米白 · 春季系列 · 商品编号 A-1028
      </p>


      <div class="price">
        ¥699
      </div>


      <p>
        喜欢这套搭配？
        扫码带走试衣效果，
        并查看更多颜色、尺码与购买信息。
      </p>


      <!-- 模拟二维码 -->

      <div
        class="qr"
        title="模拟二维码"
      ></div>


      <div>
        <b>
          手机扫码
        </b>

        　保存试衣效果 / 查看同款
      </div>


      <!-- 操作按钮 -->

      <div style="margin-top:25px">

        <button
          class="primary"
          onclick="toast('已切换推荐款式')"
        >
          换一套
        </button>

        <button
          class="secondary"
          onclick="go(2)"
        >
          重新体验
        </button>

      </div>

    </div>

  </div>

</section>


</div>



<script>

/*
 * ============================
 * 页面切换
 * ============================
 */

function go(n) {

  document
    .querySelectorAll('.screen')
    .forEach(function(screen) {

      screen.classList.remove('active');

    });


  document
    .getElementById('s' + n)
    .classList.add('active');

}



/*
 * ============================
 * 卡片选择
 * ============================
 */

function selectCard(el) {

  el.parentElement
    .querySelectorAll('.card')
    .forEach(function(card) {

      card.classList.remove('selected');

    });


  el.classList.add('selected');

}



/*
 * ============================
 * 使用用户照片
 * ============================
 */

function photoMode() {

  toast(
    '已选择照片模式：实际产品此处应先展示隐私说明，再调用摄像头'
  );

}



/*
 * ============================
 * Toast提示
 * ============================
 */

function toast(msg) {

  let t = document.getElementById('toast');

  t.textContent = msg;

  t.style.display = 'block';


  setTimeout(function() {

    t.style.display = 'none';

  }, 2400);

}



/*
 * ============================
 * AI生成模拟
 * ============================
 */

function generate() {

  // 进入AI等待页
  go(4);


  let p = 0;

  let bar =
    document.getElementById('bar');

  let pct =
    document.getElementById('percent');

  let txt =
    document.getElementById('loadingText');


  /*
   * 每500ms增加5%
   * 总计约10秒
   */

  let timer = setInterval(function() {

    p += 5;


    // 更新进度条
    bar.style.width = p + '%';


    // 更新文字
    pct.textContent =
      '预计15秒内完成 · ' + p + '%';


    // 不同阶段显示不同状态

    if (p === 35) {

      txt.textContent =
        '正在匹配服装';

    }


    if (p === 70) {

      txt.textContent =
        '正在生成试衣效果';

    }


    /*
     * 生成完成
     */

    if (p >= 100) {

      clearInterval(timer);


      txt.textContent =
        '生成完成';


      setTimeout(function() {

        go(5);

      }, 500);

    }

  }, 500);

}

</script>

</body>
</html>
