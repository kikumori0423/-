<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>株式銘柄分析アプリ</title>
  <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body {
      font-family: 'Helvetica Neue', Arial, sans-serif;
      background-color: #f5f7fa;
    }
    .card {
      background-color: white;
      border-radius: 0.5rem;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      margin-bottom: 1.5rem;
      padding: 1.5rem;
    }
    .chart-container {
      position: relative;
      height: 300px;
      width: 100%;
    }
  </style>
</head>
<body class="min-h-screen">
  <header class="bg-blue-600 text-white shadow-lg">
    <div class="container mx-auto px-4 py-6">
      <h1 class="text-3xl font-bold">株式銘柄分析アプリ</h1>
      <p class="mt-2">企業分析・財務データ・市場センチメント・テクニカル分析を一括表示</p>
    </div>
  </header>

  <main class="container mx-auto px-4 py-8">
    <div class="card">
      <h2 class="text-xl font-semibold mb-4">銘柄検索</h2>
      <div class="flex flex-col md:flex-row gap-4">
        <input type="text" id="stockSymbol" placeholder="銘柄コード（例: AAPL, MSFT, GOOGL）" 
               class="flex-grow px-4 py-2 border rounded focus:outline-none focus:ring-2 focus:ring-blue-500">
        <button id="searchButton" class="bg-blue-600 hover:bg-blue-700 text-white px-6 py-2 rounded">
          分析開始
        </button>
      </div>
      <div class="mt-2 text-sm text-gray-500">
        ※ このデモでは、AAPL（アップル）のサンプルデータを表示します
      </div>
    </div>

    <div id="analysisResults" class="hidden">
      <!-- 株価チャート -->
      <div class="card">
        <h2 class="text-xl font-semibold mb-4">株価チャート分析</h2>
        <div class="chart-container">
          <canvas id="stockChart"></canvas>
        </div>
      </div>

      <!-- 企業概要 -->
      <div class="card">
        <h2 class="text-xl font-semibold mb-4">企業概要</h2>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div>
            <h3 class="text-lg font-medium text-gray-900 mb-3">基本情報</h3>
            <table class="w-full">
              <tr>
                <td class="py-2 text-gray-600 font-medium">企業名</td>
                <td class="py-2">Apple Inc.</td>
              </tr>
              <tr>
                <td class="py-2 text-gray-600 font-medium">業種</td>
                <td class="py-2">テクノロジー - 家電製品</td>
              </tr>
              <tr>
                <td class="py-2 text-gray-600 font-medium">設立</td>
                <td class="py-2">1976年</td>
              </tr>
              <tr>
                <td class="py-2 text-gray-600 font-medium">従業員数</td>
                <td class="py-2">154,000人</td>
              </tr>
              <tr>
                <td class="py-2 text-gray-600 font-medium">本社</td>
                <td class="py-2">アメリカ合衆国カリフォルニア州クパチーノ</td>
              </tr>
            </table>
          </div>
          <div>
            <h3 class="text-lg font-medium text-gray-900 mb-3">事業概要</h3>
            <p class="text-gray-700">
              Appleはスマートフォン（iPhone）、タブレット（iPad）、パソコン（Mac）、ウェアラブルデバイス（Apple Watch）などのハードウェア製品と、iOS、macOSなどのソフトウェア製品を開発・販売しています。また、App Store、Apple Music、Apple TV+などのサービスも提供しています。
            </p>
            <div class="mt-4">
              <h4 class="font-medium text-gray-900">主要製品・サービス</h4>
              <ul class="list-disc pl-5 mt-2 space-y-1 text-gray-700">
                <li>iPhone</li>
                <li>iPad</li>
                <li>Mac</li>
                <li>Apple Watch</li>
                <li>AirPods</li>
                <li>Apple TV</li>
                <li>サービス（App Store, Apple Music, iCloud等）</li>
              </ul>
            </div>
          </div>
        </div>
      </div>

      <!-- 財務データ分析 -->
      <div class="card">
        <h2 class="text-xl font-semibold mb-4">財務データ分析</h2>
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 mb-6">
          <div class="bg-white p-4 rounded-lg shadow-sm">
            <h4 class="text-sm font-medium text-gray-700 mb-3">四半期収益 (百万ドル)</h4>
            <div class="h-64">
              <canvas id="revenueChart"></canvas>
            </div>
          </div>
          <div class="bg-white p-4 rounded-lg shadow-sm">
            <h4 class="text-sm font-medium text-gray-700 mb-3">財務指標評価</h4>
            <div class="h-64">
              <canvas id="radarChart"></canvas>
            </div>
          </div>
        </div>
        
        <div class="mt-6">
          <h3 class="text-lg font-medium text-gray-900 mb-3">貸借対照表概要</h3>
          <div class="overflow-x-auto">
            <table class="min-w-full divide-y divide-gray-200">
              <thead class="bg-gray-50">
                <tr>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">項目</th>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">2023年</th>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">2022年</th>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">変化率</th>
                </tr>
              </thead>
              <tbody class="bg-white divide-y divide-gray-200">
                <tr>
                  <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">総資産</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">$352.8B</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">$336.2B</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-green-600">+4.9%</td>
                </tr>
                <tr>
                  <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">総負債</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">$290.4B</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">$302.7B</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-green-600">-4.1%</td>
                </tr>
                <tr>
                  <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">株主資本</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">$62.4B</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">$33.5B</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-green-600">+86.3%</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
        
        <div class="mt-6">
          <h3 class="text-lg font-medium text-gray-900 mb-3">キャッシュフロー分析</h3>
          <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
            <div class="bg-green-50 p-4 rounded-lg">
              <h4 class="text-sm font-medium text-green-700 mb-2">営業キャッシュフロー</h4>
              <p class="text-2xl font-bold text-green-800">$109.6B</p>
              <p class="text-sm text-green-600 mt-1">前年比 +5.2%</p>
            </div>
            <div class="bg-red-50 p-4 rounded-lg">
              <h4 class="text-sm font-medium text-red-700 mb-2">投資キャッシュフロー</h4>
              <p class="text-2xl font-bold text-red-800">-$18.2B</p>
              <p class="text-sm text-red-600 mt-1">前年比 -12.3%</p>
            </div>
            <div class="bg-blue-50 p-4 rounded-lg">
              <h4 class="text-sm font-medium text-blue-700 mb-2">財務キャッシュフロー</h4>
              <p class="text-2xl font-bold text-blue-800">-$110.3B</p>
              <p class="text-sm text-blue-600 mt-1">前年比 +15.7%</p>
            </div>
          </div>
        </div>
      </div>

      <!-- 市場センチメント分析 -->
      <div class="card">
        <h2 class="text-xl font-semibold mb-4">市場センチメント分析</h2>
        
        <div class="mb-6">
          <h3 class="text-lg font-medium text-gray-900 mb-3">アナリスト評価</h3>
          <div class="overflow-x-auto">
            <table class="min-w-full divide-y divide-gray-200">
              <thead class="bg-gray-50">
                <tr>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">証券会社</th>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">評価</th>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">目標株価</th>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">更新日</th>
                </tr>
              </thead>
              <tbody class="bg-white divide-y divide-gray-200">
                <tr>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">JPモルガン</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-green-600 font-medium">買い</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">$210.00</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">2025-02-15</td>
                </tr>
                <tr>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">ゴールドマン・サックス</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-green-600 font-medium">買い</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">$223.00</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">2025-02-10</td>
                </tr>
                <tr>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">モルガン・スタンレー</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-yellow-600 font-medium">保持</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">$190.00</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">2025-02-05</td>
                </tr>
                <tr>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">バンク・オブ・アメリカ</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-green-600 font-medium">買い</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">$215.00</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">2025-01-28</td>
                </tr>
                <tr>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">ドイツ銀行</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-yellow-600 font-medium">保持</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">$185.00</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">2025-01-20</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
        
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
          <div>
            <h3 class="text-lg font-medium text-gray-900 mb-3">センチメント指標</h3>
            <div class="bg-white p-4 rounded-lg shadow-sm">
              <div class="grid grid-cols-2 gap-4">
                <div class="bg-gray-50 p-3 rounded-lg">
                  <h4 class="text-xs font-medium text-gray-500 uppercase">短期見通し</h4>
                  <p class="text-lg font-semibold text-green-600 mt-1">ポジティブ</p>
                </div>
                <div class="bg-gray-50 p-3 rounded-lg">
                  <h4 class="text-xs font-medium text-gray-500 uppercase">中期見通し</h4>
                  <p class="text-lg font-semibold text-green-600 mt-1">ポジティブ</p>
                </div>
                <div class="bg-gray-50 p-3 rounded-lg">
                  <h4 class="text-xs font-medium text-gray-500 uppercase">長期見通し</h4>
                  <p class="text-lg font-semibold text-yellow-600 mt-1">中立</p>
                </div>
                <div class="bg-gray-50 p-3 rounded-lg">
                  <h4 class="text-xs font-medium text-gray-500 uppercase">センチメントスコア</h4>
                  <p class="text-lg font-semibold text-blue-600 mt-1">78/100</p>
                </div>
              </div>
            </div>
          </div>
          
          <div>
            <h3 class="text-lg font-medium text-gray-900 mb-3">最近のニュース影響</h3>
            <div class="space-y-3">
              <div class="bg-green-50 p-3 rounded-lg">
                <p class="text-sm text-gray-800">Appleが新型iPhoneの販売好調を報告、予想を上回る</p>
                <div class="flex justify-between items-center mt-1">
                  <span class="text-xs text-gray-500">2025-03-15</span>
                  <span class="text-xs font-medium text-green-600">ポジティブ</span>
                </div>
              </div>
              <div class="bg-red-50 p-3 rounded-lg">
                <p class="text-sm text-gray-800">中国市場でのシェア低下が懸念材料に</p>
                <div class="flex justify-between items-center mt-1">
                  <span class="text-xs text-gray-500">2025-03-10</span>
                  <span class="text-xs font-medium text-red-600">ネガティブ</span>
                </div>
              </div>
              <div class="bg-green-50 p-3 rounded-lg">
                <p class="text-sm text-gray-800">新サービス「Apple Intelligence」の発表が好評</p>
                <div class="flex justify-between items-center mt-1">
                  <span class="text-xs text-gray-500">2025-03-05</span>
                  <span class="text-xs font-medium text-green-600">ポジティブ</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- テクニカル分析 -->
      <div class="card">
        <h2 class="text-xl font-semibold mb-4">テクニカル分析</h2>
        
        <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-6">
          <div class="bg-white p-4 rounded-lg shadow-sm">
            <h4 class="text-sm font-medium text-gray-700 mb-2">サポートレベル</h4>
            <p class="text-2xl font-bold text-gray-800">$170.50</p>
            <p class="text-sm text-gray-600 mt-1">直近の安値から算出</p>
          </div>
          <div class="bg-white p-4 rounded-lg shadow-sm">
            <h4 class="text-sm font-medium text-gray-700 mb-2">レジスタンスレベル</h4>
            <p class="text-2xl font-bold text-gray-800">$195.25</p>
            <p class="text-sm text-gray-600 mt-1">直近の高値から算出</p>
          </div>
          <div class="bg-white p-4 rounded-lg shadow-sm">
            <h4 class="text-sm font-medium text-gray-700 mb-2">ボラティリティ</h4>
            <p class="text-2xl font-bold text-gray-800">18.5%</p>
            <p class="text-sm text-gray-600 mt-1">過去30日間の標準偏差</p>
          </div>
        </div>
        
        <div class="mb-6">
          <h3 class="text-lg font-medium text-gray-900 mb-3">テクニカル指標</h3>
          <div class="overflow-x-auto">
            <table class="min-w-full divide-y divide-gray-200">
              <thead class="bg-gray-50">
                <tr>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">指標</th>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">値</th>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">シグナル</th>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">説明</th>
                </tr>
              </thead>
              <tbody class="bg-white divide-y divide-gray-200">
                <tr>
                  <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">RSI (14)</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">62.5</td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-yellow-600 font-medium">中立</td>
                  <td class="px-6 py-4 whitespace<response clipped><NOTE>To save on context only part of this file has been shown to you. You should retry this tool after you have searched inside the file with `grep -n` in order to find the line numbers of what you are looking for.</NOTE>