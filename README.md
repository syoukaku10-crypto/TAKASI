<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>患者様用：受付システム</title>
    <style>
        body { font-family: sans-serif; padding: 20px; max-width: 500px; margin: auto; }
        .status-box { background: #e3f2fd; padding: 15px; border-radius: 8px; text-align: center; margin-bottom: 20px; }
        .input-group { margin-bottom: 10px; }
        input { width: 100%; padding: 8px; box-sizing: border-box; }
        button { width: 100%; padding: 10px; background: #007bff; color: white; border: none; cursor: pointer; }
    </style>
</head>
<body>
    <div class="status-box">
        <h2>現在の診察状況</h2>
        <p style="font-size: 24px;">ただいま <strong id="current-num">--</strong> 番まで終了</p>
    </div>

    <h3>新規予約</h3>
    <div class="input-group">
        <input type="text" id="name" placeholder="氏名">
    </div>
    <div class="input-group">
        <input type="tel" id="tel" placeholder="電話番号">
    </div>
    <button onclick="reserve()">予約する</button>

    <div id="result" style="margin-top: 20px; font-weight: bold; color: green;"></div>

    <script>
        // 診察状況を更新する
        function updateStatus() {
            const finished = localStorage.getItem('lastFinished') || 0;
            document.getElementById('current-num').innerText = finished;
        }

        function reserve() {
            const name = document.getElementById('name').value;
            const tel = document.getElementById('tel').value;
            if(!name || !tel) return alert("入力してください");

            // 既存のリストを取得
            let patients = JSON.parse(localStorage.getItem('patients') || "[]");
            
            // 新しい受付番号を発行
            const newNum = patients.length + 1;
            
            // データを追加
            patients.push({ num: newNum, name: name, tel: tel, status: "waiting" });
            localStorage.setItem('patients', JSON.stringify(patients));

            document.getElementById('result').innerText = `予約完了！あなたの番号は ${newNum} 番です。`;
            document.getElementById('name').value = "";
            document.getElementById('tel').value = "";
        }

        // 1秒ごとに状況を確認
        setInterval(updateStatus, 1000);
        updateStatus();
    </script>
</body>
</html>
