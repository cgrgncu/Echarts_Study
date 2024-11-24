```html
<!DOCTYPE html>
<html lang="zh-TW" style="margin: 0;padding: 0;height: 100%;overflow: hidden;">
<head>
	<!-- meta 開始 -->
	<meta charset="utf-8">
	<meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no,shrink-to-fit=no">
	<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
	<!-- meta 結束 -->
    <title>ECharts範例 - 展示基礎模板</title>
    <script src="https://cdn.jsdelivr.net/npm/echarts@5.4.0/dist/echarts.min.js"></script>
</head>
<body style="margin: 0;padding: 0;height: 100%;overflow: hidden;">
   <!-- Loading 動畫 -->
    <div id="loading" style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: rgba(255, 255, 255, 0.7); display: flex; justify-content: center; align-items: center; z-index: 9999;">
        <div class="circle-container" style="position: relative; width: 150px; height: 150px; display: flex; justify-content: center; align-items: center;">
            <svg width="150" height="150" style="transform: rotate(-90deg);">
                <circle class="background" cx="75" cy="75" r="65" style="fill: none; stroke: #f3f3f3; stroke-width: 10;"></circle> <!-- 背景圓環 -->
                <circle class="progress" cx="75" cy="75" r="65" style="fill: none; stroke: #3498db; stroke-width: 10; stroke-linecap: round; stroke-dasharray: 440; stroke-dashoffset: 440; transition: stroke-dashoffset 0.5s ease-out;"></circle>   <!-- 進度圓環 -->
            </svg>
            <div class="percentage" id="percentage" style="position: absolute; font-size: 24px; font-weight: bold; color: #3498db;">0%</div> <!-- 顯示百分比 -->
        </div>
    </div>
    <!-- ECharts 圖表容器 -->
    <div id="chart" style="width: 100%; height: 100%;"></div>

    <script>
        // 初始化 ECharts
        var myChart = echarts.init(document.getElementById('chart'));

        // 設定圖表的配置
        var option = {
            title: {
                text: 'ECharts 示例',
                subtext: '簡單柱狀圖',
                left: 'center'
            },
            tooltip: {},
            xAxis: {
                type: 'category',
                data: ['一月', '二月', '三月', '四月', '五月', '六月']
            },
            yAxis: {
                type: 'value'
            },
            series: [{
                data: [5, 20, 36, 10, 10, 20],
                type: 'bar'
            }]
        };

        // 計算圓環的總長度 (2 * pi * 半徑)
        var totalLength = 2 * Math.PI * 65;
        var progress = 0;
        var percentageElement = document.getElementById('percentage');
        var progressCircle = document.querySelector('.circle-container .progress');

        // 設置進度條
        function updateProgressBar(progress) {
            var offset = totalLength - (totalLength * progress / 100);  // 根據百分比來調整圓環的顯示
            progressCircle.style.strokeDashoffset = offset;  // 更新進度圓環的偏移
            percentageElement.innerText = progress + '%';  // 顯示當前百分比
        }

        // 模擬載入檔案，延遲 1 秒後顯示圖表
        var interval = setInterval(function() {
            if (progress >= 100) {
                clearInterval(interval);
                // 進度條完成後隱藏載入畫面並顯示圖表
                document.getElementById('loading').style.display = 'none';
                document.getElementById('chart').style.display = 'block';

                // 使用配置項填充圖表
                myChart.setOption(option);
            } else {
                progress++;
                updateProgressBar(progress);
            }
        }, 10); // 每10毫秒更新一次，達到1秒內完成

        // 窗口大小改變時，重繪圖表以適應螢幕尺寸
        window.onresize = function () {
            myChart.resize();
        };
    </script>
</body>
</html>

```
