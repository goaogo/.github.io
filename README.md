[index.html](https://github.com/user-attachments/files/28699565/index.html)
# .github.io
爱丽丝梦游仙境天蝎座卡
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, user-scalable=no" />
    <title>天蝎座 · 星座运势AR卡片</title>
    <!-- 引入库文件 -->
    <script src="https://aframe.io/releases/1.2.0/aframe.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/ar.js@3.4.2/aframe/build/aframe-ar.min.js"></script>
    <style>
      .ar-text { text-align: center; font-family: "PingFang SC", "Microsoft YaHei", sans-serif; background: rgba(0,0,0,0.7); color: white; padding: 0.2em 0.5em; border-radius: 8px; border-left: 4px solid #ff7e3e; white-space: pre-line; }
    </style>
  </head>
  <body style="margin: 0; overflow: hidden">
    <a-scene embedded arjs="trackingMethod: best; sourceType: webcam; debugUIEnabled: false;">
      <!-- 这里需要替换为你的 .patt 文件 -->
      <a-marker type="pattern" url="pattern-天蝎.patt">
        <!-- 动态文本组件，id用于JS更新 -->
        <a-text id="daily-fortune"
          value="🔮 正在加载今日运势..."
          align="center"
          color="white"
          background="black"
          opacity="0.8"
          width="4"
          wrap-count="20"
          position="0 1.5 0"
          class="ar-text">
        </a-text>
      </a-marker>
      <a-entity camera></a-entity>
    </a-scene>

    <!-- 添加JavaScript代码来获取和更新运势 -->
    <script>
      // 当A-Frame场景加载完毕后，执行内部代码
      window.addEventListener('load', function () {
        // 你需要替换成在聚合数据申请到的APPKEY
        const APPKEY = 'YOUR_API_KEY_HERE'; 
        // 星座名称，务必确保与API文档要求一致，如“天蝎座”
        const CONSTELLATION = '天蝎座'; 

        // 1. 构建请求URL
        const apiUrl = `https://web.juhe.cn:8080/constellation/getAll?consName=${encodeURIComponent(CONSTELLATION)}&type=today&key=${APPKEY}`;

        // 2. 发起请求获取数据
        fetch(apiUrl)
          .then(response => response.json()) // 将返回的数据解析为JSON
          .then(data => {
            // 3. 请求成功后，处理数据并更新文字组件
            if (data.error_code === 0) {
              // 成功获取到数据，拼接成漂亮的字符串
              const fortuneText = `🔮 ${CONSTELLATION} · 今日运势 🔮\n\n✨ 综合运势：${data.all}\n💖 爱情运势：${data.love}\n🚀 事业运势：${data.work}\n💰 财富运势：${data.money}\n💎 幸运颜色：${data.color}\n🔢 幸运数字：${data.number}\n💡 速配星座：${data.QFriend}`;
              // 找到我们之前定义的文本组件，更新它的value属性
              const textElement = document.getElementById('daily-fortune');
              if (textElement) {
                textElement.setAttribute('value', fortuneText);
              }
            } else {
              // 如果API返回了错误码，就在AR中显示错误信息
              const textElement = document.getElementById('daily-fortune');
              if (textElement) {
                textElement.setAttribute('value', `⚠️ 运势获取失败：${data.reason || '请稍后重试'}`);
              }
            }
          })
          .catch(error => {
            // 4. 网络请求失败时的处理
            console.error('获取运势失败:', error);
            const textElement = document.getElementById('daily-fortune');
            if (textElement) {
              textElement.setAttribute('value', `⚠️ 网络出错，请检查网络后刷新页面重试。`);
            }
          });
      });
    </script>
  </body>
</html>
[Uploading pattern-天蝎.patt…]()
