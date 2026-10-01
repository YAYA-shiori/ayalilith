# GHOST.md

このゴーストに固有の情報です。AI エージェントは、作業の前に `AGENTS.md` とあわせて読みます。
作者が自由に書き換えるファイルで、開発キットの更新（`tools/update-devkit.ps1`）で上書きされることはありません。

## このゴーストについて

- 名前（`ghost/master/descript.txt` の `name`）: 紺野りりす（ディレクトリ名 `ayalilith`）
- キャラクター（`sakura.name` / `kero.name`）: りりす / りりしば
- 作者: 整備班（`craftman,seibihan`）。YAYA 版の改変は Couperin、Minaduki-ran（`readme-aya.txt` より）
- 配布先・ネットワーク更新の URL: http://ms.shillest.net/housekeeper/zz_other/ayalilith/（`yaya_homeurl.txt` の `On_homeurl`）
- 元にしたテンプレートやゴースト: 「あやりりすEX」テンプレートゴースト。リポジトリは https://github.com/YAYA-shiori/ayalilith
- 位置づけ: このゴーストはテンプレートそのものの開発用。独自のゴーストとして独立させる前提では書かない

## ライセンス

- 辞書: Unlicense
- シェル: フリーシェル「サイレンサーガール」（作者 mkbt）。
  - 改変・ゴースト以外での使用は、「改変の有無にかかわらず作者名を隠す、または変えての再配布」以外なら自由。
  - 商用は念のため作者に一報。同人の範囲内なら自由。
  - 作者名（`shell/master/descript.txt` の `craftman`）は残すこと。
  - 記述の仕様上、SSP 専用。
  - 同梱 PSD と `install.txt` は、公開時に消してよい。

## 辞書の構成

- 読み込む辞書（`ghost/master/yaya.txt` の `dic` / `dicdir`）: `system_config.txt`（システム辞書の指定）と `lilith_config.txt`（あやりりすライブラリ）を include し、`dicdir, dic/normal` で `ghost/master/dic/normal/` 以下を読む
- 緊急モード: `dic/emerg/yaya_emerg_dic.dic`（OnBoot で「致命的エラー発生」と出すだけ）と `yaya_homeurl.dic`
- システム辞書: `ghost/master/dic/system/`（submodule、YAYA-shiori/yaya-dic）。あやりりすEX 本体は `dic/system/aya_lilith/`。いずれも編集しない

## イベントと辞書ファイルの対応

`ghost/master/dic/normal/` 以下:

| ファイル | 主な中身 |
|---|---|
| `yaya_aitalk.txt` | ランダムトーク（`ランダムトーク通常`） |
| `yaya_bootend.txt` | 起動・終了トーク、`OnWindowStateRestore` |
| `yaya_change.txt` | ゴースト切り替えトーク |
| `yaya_communicate.txt` | コミュニケート（他ゴーストやユーザー入力への反応） |
| `yaya_etc.txt` | 更新、インストール、バニッシュ、BIFF、SNTP などの種々のイベント |
| `yaya_menu.txt` | メニュー処理 |
| `yaya_mouse.txt` | マウス反応。関数名は [種別]+[スコープ]+[部位]（例: `なでなで0Head`） |
| `yaya_string.txt` | 文字列リソース、`On_username` |
| `yaya_word.txt` | 単語辞書（`ms` `mc` など） |
| `yaya_homeurl.txt` | ネットワーク更新の URL（`On_homeurl`） |

新しいイベントに反応させるときに書く場所: 種類の近いファイル（マウスなら `yaya_mouse.txt`）。迷えば `yaya_etc.txt`。

## キャラクターとサーフェス

| スコープ | キャラクター | 人物像（一人称、口調、性格） | 使えるサーフェス |
|---|---|---|---|
| `\0` | りりす | （未定。作者が後日記入） | 0 素（基本の顔）/ 1 照れ（頬が赤く、視線をそらす）/ 2 驚き（口が開く）/ 3 困惑 / 4 不安 / 5 笑顔（目を細めて微笑む）/ 6 目閉じ / 7 怒り / 8 冷笑 / 9 照れ怒り / 20 微笑 / 21 照れ困り（目を閉じて赤面）/ 22 照れ笑い笑顔（目を閉じて笑い、赤面）/ 23 素よそ見 / 24 不安よそ見 |
| `\1` | りりしば | （未定。作者が後日記入） | 10 素（にっこり）/ 11 ニヤソ（口が大きい笑顔） |

- 表情の見た目は `tools/dump-surface.ps1` で確かめた。0〜9 と 20〜24 は、目と口と頬の差が小さい。画像で細かい違い（特に 3 と 4、7 と 8）を選び分けるときは、もう一度見比べること
- 当たり判定（`surfaces.txt` の `collision`、surface0-9,20-24）: `Head`、`Bust`、`Cheek`。`\1` 側には無い
- トークで使わないサーフェス（アニメーション用の部品など）: 900、901、910、980、981、982（`__parts`）

## トークの書き方

あやりりすEX の簡易記法で書く。`りりす０５：台詞` のように、話し手の名前、表情番号（全角数字 2 桁）、全角コロンの順に書く。
`待ち無し` `待ち多め` `改行無し` などの前置きや、`（ユーザ）` も使える（詳しくは `ghost/master/dic/system/aya_lilith/aya_lilith_ex.dic` の冒頭）。
`ランダムトーク通常` の中だけ `<<' '>>` で囲み、ほかは `<<" ">>` で囲む。

例（`yaya_aitalk.txt` より）:

```
<<'
りりす０６：りりしば、赤く塗ってあげようか？
しば１０：みどりがいいなー
りりす０８：それは、私がおことわりよ……
'>>
```

## 独自のルール

- 人物像が決まるまでは、トークを新しく書くときも、既存の台詞を書き換えるときも、口調を足さない。
