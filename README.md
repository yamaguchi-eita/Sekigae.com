<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>席替えアプリ</title>
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
            max-width: 600px;
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

        /* 教室の座席レイアウト (グリッドシステム) */
        .classroom-grid {
            display: grid;
            grid-template-columns: repeat(6, 60px); /* 横6列 */
            gap: 10px;
            justify-content: center;
            margin-top: 10px;
        }

        /* 席（マス目）の基本スタイル */
        .seat {
            width: 60px;
            height: 60px;
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

        /* 最後の行（5マス）を中央に寄せるための調整 */
        .seat.hidden {
            visibility: hidden;
            pointer-events: none;
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

    <!-- 座席（マス目）エリア -->
    <div class="classroom-grid" id="classroom">
        <!-- JavaScriptで自動生成します -->
    </div>
</div>

<script>
    // 設計図の形（1〜7行目：6マス、8行目：左側5マス＋右端1マス空白 ＝ 計47マス分で制御）
    // 41番目の要素（8行目の右端）を空白(hidden)にすることで、合計41マスの変則レイアウトを作ります。
    const totalSlots = 42; 
    const hiddenIndex = 41; // 0から数えて41番目（最後のマス）を非表示にする

    const classroom = document.getElementById('classroom');
    const startBtn = document.getElementById('startBtn');
    
    // 座席の要素を格納する配列
    let seatElements = [];

    // 初期状態の座席（マス目）を作成
    function initClassroom() {
        classroom.innerHTML = '';
        seatElements = [];
        
        for (let i = 0; i < totalSlots; i++) {
            const seat = document.createElement('div');
            
            // 8行目の右端だけ非表示（画像通りの凸型レイアウトを再現）
            if (i === hiddenIndex) {
                seat.className = 'seat hidden';
            } else {
                seat.className = 'seat';
                seat.textContent = ''; // 最初は空欄
                seatElements.push(seat); // 有効な41マスを配列に追加
            }
            classroom.appendChild(seat);
        }
    }

    // 席替えを実行する関数
    function shuffleSekigae() {
        // 1から41までの数字の配列を作成
        const numbers = Array.from({ length: 41 }, (_, i) => i + 1);
        
        // フィッシャー・イェーツのシャッフルアルゴリズムでランダムに入れ替え
        for (let i = numbers.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [numbers[i], numbers[j]] = [numbers[j], numbers[i]];
        }

        // シャッフルした数字を各マス目に表示
        seatElements.forEach((seat, index) => {
            seat.textContent = numbers[index];
            seat.classList.add('filled');
        });
    }

    // ボタンクリック時のイベント
    startBtn.addEventListener('click', () => {
        // 一度綺麗にしてからシャッフル
        initClassroom();
        // 少しだけ演出っぽく遅らせて表示
        setTimeout(shuffleSekigae, 100);
    });

    // ページ読み込み時に初期化
    initClassroom();
</script>

</body>
</html>
