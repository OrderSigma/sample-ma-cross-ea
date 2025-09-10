# sample MA Cross EA

**言語：** 日本語 | [English](./README.md)

## 1. 概要

- 本 EA は、2 本の単純移動平均線（SMA）のクロスを売買シグナルとする、シンプルな MetaTrader 4 用の自動売買システムです。常に 1 ポジションのみを保有し、反対シグナルが発生すると即座にドテンします。
- トレーリングストップ、テイクプロフィット／ストップロス（TP/SL）、スプレッドフィルタ、詳細なログ出力機能を搭載しています。
- 本 EA はポートフォリオ提示を目的としたサンプルです。本 EA の使用に起因するいかなる損失・損害についても、作者は一切の責任を負いません。ご利用は自己責任でお願いします。

## 2. 特徴

- **移動平均クロス戦略**  
  Fast SMA と Slow SMA のクロスを検出し、ゴールデンクロスで BUY、デッドクロスで SELL を実行。

- **常に1ポジション＆ドテン**  
  保有ポジションは常に 1 つのみで、反対シグナル発生時には既存ポジションをクローズし、反対方向にエントリー。

- **カスタマイズ可能な TP / SL**  
  テイクプロフィット（TP）およびストップロス（SL）を pips 指定で入力。0 を設定すると無効化。

- **トレーリングストップ機能**  
  開始条件（Start）、距離（Distance）、任意のステップ幅（Step）を個別に設定可能。利益が伸びた後のリスク管理を自動化。

- **スプレッドフィルター & スリッページ設定**  
  指定以上のスプレッド時はエントリーをスキップし、注文時のスリッページ上限を points 単位で制御。

- **描画オプション**  
  エントリー時にチャートへ BUY（青）/SELL（赤）の矢印を表示するかどうかを切り替え可能。

- **詳細ログ出力**  
  `LogEnabled` を `true` にすると、シグナル発生・注文送信／修正／決済・トレーリング更新などをターミナルログへ記録。

- **安全性チェック**  
  初期化時に `Lots > 0`、`FastMA < SlowMA`、`TrailingDist >= TrailingStep` などの入力値の検証を行い、設定ミスを防止。

## 3. パラメータ一覧

| パラメータ | 変数名 | デ⁠フ⁠ォ⁠ル⁠ト | 単位 / 型 | その他 |
| :-------- | :----- | :-------- | :------- | :----- |
| Fast SMA period (must be < Slow)        | FastMAPeriod      | 25   | 期間 / int      | - |
| Slow SMA period                         | SlowMAPeriod      | 75   | 期間 / int      | - |
| Lot size (must be > 0)                  | Lots              | 0.10 | ロ⁠ッ⁠ト / double | - |
| Take Profit (pips)                      | TakeProfitPips    | 30   | pips / double   | 0 で無効 |
| Initial Stop Loss (pips)                | StopLossPips      | 20   | pips / double   | 0 で無効 |
| Max allowed spread (pips)               | MaxSpreadPips     | 2.0  | pips / double   | - |
| Max slippage (points)                   | Slippage          | 3    | points / int    | - |
| Start trailing after this profit (pips) | TrailingStartPips | 20   | pips / double   | 詳⁠細⁠は⁠「⁠4⁠. 使⁠用⁠方⁠法⁠」⁠を⁠参⁠照 |
| Distance from price (pips)              | TrailingDistPips  | 20   | pips / double   | 詳細は「4. 使用方法」を参照 |
| Update step (pips), 0 = every tick      | TrailingStepPips  | 0    | pips / double   | 0 で毎ティック |
| Magic number                            | MagicNumber       | 20250630 | - / int     | - |
| Draw BUY/SELL arrows on entry           | DrawArrows        | true     | - / bool    | - |
| Log ON/OFF                              | LogEnabled        | true     | - / bool    | - |

## 4. 使用方法

**4-1. 用語の補足**
- Fast SMA…短期単純移動平均線
- Slow SMA…長期単純移動平均線

**4-2. 英語のパラメータラベルの日本語訳**
|                                         |                                    |
| :-------------------------------------- | :--------------------------------- |
| Fast SMA period (must be < Slow)        | 高⁠速⁠（⁠短⁠期⁠）⁠S⁠M⁠A の⁠期⁠間⁠（⁠Slow よ⁠り⁠小⁠）|
| Slow SMA period                         | 低速（長期）SMA の期間 |
| Lot size (must be > 0)                  | 注文ロット数（> 0）|
| Take Profit (pips)                      | テイクプロフィット幅 |
| Initial Stop Loss (pips)                | ストップロス幅 |
| Max allowed spread (pips)               | 許容最大スプレッド |
| Max slippage (points)                   | 最大スリッページ |
| Start trailing after this profit (pips) | トレーリング開始利益幅 |
| Distance from price (pips)              | 価格と SL の距離 |
| Update step (pips), 0 = every tick      | 更新ステップ幅（0 = 毎ティック）|
| Magic number (trade identifier)         | マジックナンバー（取引識別）|
| Draw BUY/SELL arrows on entry           | エントリー時の矢印描画 ON/OFF |
| Log ON/OFF                              | ログ出力 ON/OFF |

**4-3. TP / SL の無効化**
- テイクプロフィットを使わないときは Take Profit (pips) に 0 を入力する。
- ストップロスを使わないときは Initial Stop Loss (pips) に 0 を入力する。

**4-4. トレーリングストップの設定**
- トレーリングストップを有効にするときは、Start trailing after this profit (pips) と Distance from price (pips) に 0 より大きい好きな値を入力する。その際、Distance from price (pips) の値は Update step (pips), 0 = every tick 以上にする。現行価格と SL との距離よりも SL 更新のステップ幅が大きいと、SL が価格に追従できず EA が OrderModify（注文修正）を失敗するためエラーとなる。Update step (pips), 0 = every tick に 0 を入力すると、SL が毎ティック更新される。
- トレーリングストップのみ使うときは、上記設定に加えて Take Profit (pips) に 0 を入力する。
- トレーリングストップを無効にするときは、Start trailing after this profit (pips)、Distance from price (pips)、Update step (pips), 0 = every tickの 3 つすべてに 0 を入力する。

**4-5. マジックナンバー（取引識別）の設定**
- 同一口座で複数チャートや複数 EA を並行稼働する場合は、チャートごとに一意の整数を設定する。これにより、EA が自分のポジションだけを管理する。
- 例：EURUSD → 101、GBPUSD → 102、バックテスト → 1001 など

**4-6. 視認性を高める**
- 本 EA は内部で SMA を計算するがラインを描画しないため、クロスを目視したい場合はチャートに短期 SMA と長期 SMA を手動で追加する。

**4-7. その他**
- ジャーナルで一瞬見えるジグザグに関してのログは、ストラテジーテスターの描画スピードを落とすためにインジケーターのジグザグを色なしで適用したもので、本 EA との関係はない。

## 5. 動作デモ

- 下の GIF は、本 EA がどのように取引を実行するかを示す視覚的な参考資料です。本 EA はあくまでポートフォリオ用サンプルであるため、詳細なバックテストは行っていません。

<img src="images/SampleEA_GIF_Journal18f150_64d_nodither_l20.gif" width="700">

<img src="images/SampleEA_GIF_Results18f200_256_nodither_l20.gif" width="700">

## 6. 動作環境

|                    |                                         |
| :----------------- | :-------------------------------------- |
| プ⁠ラ⁠ッ⁠ト⁠フ⁠ォ⁠ー⁠ム    | MetaTrader 4 Build 1350 以⁠降⁠（⁠最⁠新⁠版⁠推⁠奨⁠）|
| ブローカー桁数      | 4 桁 / 5 桁の両方に対応 |
| 口座タイプ          | 標準口座（ヘッジ可）|
| OS                 | Windows 10 / 11（64-bit）推奨 |
| VPS                | 安定回線または VPS（Ping < 100 ms）|
| 追加ライブラリ      | 不要（EA 単体で動作）|
| 権限               | MT4 の自動売買を ON |

## 7. ライセンス

- MIT License の全文は [`LICENSE`](./LICENSE) ファイルをご覧ください（スマホなどで表示が崩れることがありますが、内容に問題はありません）。
