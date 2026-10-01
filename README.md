<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>席替えアプリ (44席バージョン)</title>
    <style>
        body {
            font-family: 'Helvetica Neue', Arial, sans-serif;
            background-color: #f5f5f5;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
        }

        .container {
            background-color: #ffffff;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            text-align: center;
            max-width: 700px; /* 列が増えたため横幅を拡張 */
            width: 100%;
        }

        /* 黒板のスタイル */
        .blackboard {
            background-color: #2e5c36;
            color: #ffffff;
            border: 5px solid #8b5a2b;
            border-radius: 4px;
            padding: 10px;
            font-weight: bold;
            font-size: 18px;
            margin: 0 auto 25px;
            width: 200px;
            box-shadow: inset 0 0 10px rgba(0,0,0,0.5);
        }

        /* 操作エリア */
        .controls {
            margin-bottom: 25px;
        }

        .btn-start {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 30px;
            font-size: 18px;
            font-weight: bold;
            border-radius: 6px;
            cursor: pointer;
            transition: background-color 0.2s;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }

        .btn-start:hover {
            background-color: #0056b3;
        }

        .btn-start:active {
            transform: scale(0.98);
        }

        /* 教室の座席レイアウト (6つの縦列を並べるコンテナ) */
        .classroom-layout {
            display: flex;
            justify-content: center;
            gap: 15px; /* 縦列どうしの間隔 */
            margin-top: 10px;
            overflow-x: auto; /* 万が一画面幅が狭い場合は横スクロール可能に */
            padding-bottom: 10px;
        }

        /* 各縦列（カラム）のスタイル */
        .column {
            display: flex;
            flex-direction: column;
            gap: 10px; /* 席の縦の間隔 */
        }

        /* 席（マス目）の基本スタイル */
        .seat {
            width: 55px;
            height: 55px;
            background-color: #e9ecef;
            border: 2px solid #dee2e6;
            border-radius: 6px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 18px;
            font-weight: bold;
            color: #495057;
            transition: all 0.3s ease;
        }

        /* 数字が入ったときのスタイル */
        .seat.filled {
            background-color: #fff3cd;
            border-color: #ffeba2;
            color: #856404;
            animation: popIn 0.3s ease;
        }

        @keyframes popIn {
            0% { transform: scale(0.8); opacity: 0.5; }
            100% { transform: scale(1); opacity: 1; }
        }
    </style>
</head>
<body>

<div class="container">
    <!-- 黒板 -->
    <div class="blackboard">黒 板</div>

    <!-- スタートボタン -->
    <div class="controls">
        <button class="btn-start" id="startBtn">スタート</button>
    </div>

    <!-- 座席エリア：6つの縦列を配置 -->
    <div class="classroom-layout">
        <div class="column" id="col1"></div> <!-- 1列目 (7席) -->
        <div class="column" id="col2"></div> <!-- 2列目 (7席) -->
        <div class="column" id="col3"></div> <!-- 3列目 (8席) -->
        <div class="column" id="col4"></div> <!-- 4列目 (8席) -->
        <div class="column" id="col5"></div> <!-- 5列目 (8席) -->
        <div class="column" id="col6"></div> <!-- 6列目 (6席) -->
    </div>
</div>

<script>
    // 各列の席数を左から順に定義
    const columnSizes =; 
    const totalSeats = 44; // 合計44席

    // 各列のHTML要素を取得
    const columns = [
        document.getElementById('col1'),
        document.getElementById('col2'),
        document.getElementById('col3'),
        document.getElementById('col4'),
        document.getElementById('col5'),
        document.getElementById('col6')
    ];
    
    const startBtn = document.getElementById('startBtn');
    let seatElements = [];

    // 初期状態の座席（マス目）を作成
    function initClassroom() {
        seatElements = [];
        
        // 一度各列を空にする
        columns.forEach(col => col.innerHTML = '');

        // 左の列から順番に指定された数だけ席を配置
        columnSizes.forEach((size, colIndex) => {
            for (let i = 0; i < size; i++) {
                const seat = document.createElement('div');
                seat.className = 'seat';
                seat.textContent = ''; // 最初は空欄
                
                columns[colIndex].appendChild(seat);
                seatElements.push(seat); // 全44席の要素を配列にまとめる
            }
        });
    }

    // 席替えを実行する関数
    function shuffleSekigae() {
        // 1から44までの数字の配列を作成
        const numbers = Array.from({ length: totalSeats }, (_, i) => i + 1);
        
        // フィッシャー・イェーツのシャッフルアルゴリズム
        for (let i = numbers.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [numbers[i], numbers[j]] = [numbers[j], numbers[i]];
        }

        // シャッフルした数字（1〜44）を各マス目に表示
        seatElements.forEach((seat, index) => {
            seat.textContent = numbers[index];
            seat.classList.add('filled');
        });
    }

    // ボタンクリック時のイベント
    startBtn.addEventListener('click', () => {
        initClassroom();
        setTimeout(shuffleSekigae, 100);
    });

    // ページ読み込み時に初期化
    initClassroom();
</script>

</body>
</html>
