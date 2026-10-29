# ToPu_CameraList

Blender のカメラ作業をまとめて扱うアドオンです。シーン内カメラの一覧、カメラごとの解像度、ビュー記憶に加えて、
3D ビューポートに直接描かれる **カメラ HUD** から、カメラの設定・構図・被写界深度・カメラリグをその場で調整できます。

> English summary is available [below](#english).

- 対応バージョン: Blender 4.2 以降（4.2 / 5.1 / 5.2 で動作確認）
- 言語: 日本語 / English（Blender のインターフェース言語に合わせて自動で切り替わります）

<!-- スクリーンショットを追加する場合はここに（例: docs/hud_camera.png） -->

---

## 目次

- [インストール](#インストール)
- [使い方](#使い方)
  - [カメラリスト（Alt + C）](#カメラリストalt--c)
  - [カメラ HUD](#カメラ-hud)
  - [カメラ視点の HUD](#カメラ視点の-hud)
  - [最小化表示](#最小化表示)
  - [カメラリグ（ターゲット / 軌道）](#カメラリグターゲット--軌道)
  - [キーフレーム](#キーフレーム)
  - [カメラごとの解像度](#カメラごとの解像度)
- [アドオン設定](#アドオン設定)
- [言語と翻訳](#言語と翻訳)
- [注意事項](#注意事項)
- [クレジット / ライセンス](#クレジット--ライセンス)
- [English](#english)

---

## インストール

1. [Releases](../../releases) から `ToPu_CameraList_vX_Y_Z.zip` をダウンロードします（zip は展開しません）。
2. Blender の **編集 > プリファレンス > アドオン** を開き、右上のメニューから **ディスクからインストール** を選んで zip を指定します。
3. 一覧の **ToPu_CameraList** を有効にします。

**更新するとき:** 一度アドオンを無効にして Blender を再起動してから、新しい zip をインストールしてください。

> 単体版の「Per-Camera Resolution」アドオンを有効にしている場合は無効にしてください。同じ機能を統合しているため、
> プロパティやオペレーターが競合します。

---

## 使い方

### カメラリスト（Alt + C）

3D ビューで **Alt + C** を押すと、カメラリストのポップアップが開きます（キーはアドオン設定で変更できます）。

- レンダー / アニメーションレンダー / レンダー結果の表示、レンダー範囲、レンダーエンジンの切り替え
- ビューからカメラを作成、選択中のカメラを今のビューに合わせる
- カメラを名前順 / コレクション別に一覧表示
  - 選択、カメラ視点への切り替え、ターゲット Empty 作成、マーカー割り当て、ドリーズーム、ビューポート表示設定、
    カメラ設定（レンズ / シフト / クリップ / 被写界深度 / カスタム解像度）、削除
- ビュー記憶（今のビューの保存・呼び出し・名前変更・削除）

### カメラ HUD

3D ビューのヘッダーで、**「ビュー / 選択 / 追加 / オブジェクト」メニューの右横にあるカメラアイコン**を押すと、
そのビューポートに HUD が表示されます。

- オン / オフは **ビューポートごと** に切り替えられ、.blend に保存されます。
- オーバーレイをオフにすると HUD も非表示になります。
- **カメラ視点以外** のときは **カメラ一覧** が表示されます。
  - クリックでカメラを選択し、その位置をロックオン風に強調表示します。カーソルを乗せると、リストからカメラまで
    引き出し線を引き、視錐台を強調します。画面外のカメラは画面端に矢印で示します。
  - ダブルクリック（または ▶）でカメラ視点に入ります。
  - 今の視点からカメラを作成できます。
  - ビュー記憶（保存 / 呼び出し / 名前変更 / 削除）
  - 表示モード（通常 / シルエット / Z深度）
- パネルはタイトル部分をドラッグして移動でき、ダブルクリックで元の位置に戻ります。

### カメラ視点の HUD

カメラ視点に入ると、カメラ設定パネルと、その周りのアイコン群が表示されます。

| 位置 | 内容 |
|---|---|
| 左のパネル | カメラ名（クリックで名前変更）、レンズ、フォーカス、カメラリグ、出力 |
| 左上 | ビュー連動（カメラをビューにロック）、位置固定、飛行モード、歩行モード |
| 右上 | シェーディング（ワイヤー / ソリッド / マテリアル / レンダー）、表示モード（通常 / シルエット / Z深度）、ビューポート DOF |
| 下中央 | 構図ガイド（三分割 / 中心 / 対角 / 黄金比 / 黄金三角形 A・B / 調和三角形 A・B）、パスパルトゥー |

**値の操作（HUD 共通）**

| 操作 | 内容 |
|---|---|
| 左右ドラッグ | 値を調整（Shift: 微調整 / Ctrl: スナップ） |
| ホイール | 1 段ずつ変更 |
| クリック | 数値入力（四則演算可 / Enter で確定 / Esc で取消） |
| 右クリック / Esc（ドラッグ中） | ドラッグ前の値に戻す |

**主な機能**

- **ドリーズーム:** 被写体の見た目の大きさを保ったまま、焦点距離とカメラ位置を同時に変えます。基準は注視ターゲット →
  カメラ前方の 3D カーソル → フォーカス点の順に決まります。
- **フォーカス:** 被写界深度、焦点オブジェクト（選択中のオブジェクトを指定 / ビューポートでクリックして距離を指定）、
  F 値、絞り羽根、回転、比率。合焦範囲を数値で表示します。
- **フォーカス表示:** 合焦している範囲をシーン上に青く色付けし、フォーカス距離の位置に線を引きます。
- **シルエット / Z 深度:** 形だけの確認や、奥行きの確認ができます。「通常」に戻すと元の表示設定に戻ります。
- カメラの切り替え（◀ ▶）、カメラ視点の終了（×）、パネルの最小化、セクションの折りたたみ

### 最小化表示

カメラ設定パネルを最小化すると、主要な設定がカメラ枠の中に小さな文字表示として並び、その場で調整できます。

| 位置 | 内容 |
|---|---|
| 左上 | 被写界深度 ON/OFF、フォーカス表示 ON/OFF |
| 上中央 | 焦点オブジェクト、選択を対象、距離をピック、解除 |
| 右上 | 距離、F 値 |
| 右中央 | ターゲット（選択 / 連動 / 作成・更新 / 注視）、軌道（角度 / スケール / 位置 / 1 周アニメ / 解除）、リグを選択 |
| 左下 | ドリーズーム |
| 下中央 | 全設定にキー |
| 右下 | シフト、クリップ、解像度スケール・カメラ別解像度、焦点距離・解像度（直接編集可） |

カメラ枠が小さいときはラベルや一部の項目を省き、他の HUD と重ならないように配置します。

### カメラリグ（ターゲット / 軌道）

今の見た目を変えずにカメラリグを作成できます。

```
<カメラ名>_Rig                 リグ全体の親（動かすとリグ全体が移動）
 ├ <カメラ名>_Target          注視ターゲット（Child Of 拘束で Rig に連動）
 └ <カメラ名>_OrbitPivot      回転の中心
     ├ カメラ                  ピボットの子（G / R や飛行・歩行で自由に動かせる）
     └ <カメラ名>_Orbit        目安の円（選択不可）
```

- **ターゲット作成:** カメラ前方に Empty を作り、Track To 拘束で注視させます。ロールしているカメラでも見た目は変わりません。
- **ターゲット注視:** オン / オフ（オフにしても今の見た目のまま止まります）
- **ターゲット連動:** ターゲットを軌道位置と一緒に動かすかどうか。Child Of 拘束の影響度で切り替えるので、
  **キーフレームでアニメーション中に切り替え** られます。
- **軌道:** 軌道角度（ピボットの回転）、軌道スケール（作成時の半径に対する比）、軌道位置（Rig の位置）、
  フレーム範囲で 1 周するアニメーション、軌道解除（カメラは今の位置のまま残る）

### キーフレーム

- 各値の左の **◇** で現在のフレームにキーを打ちます（◆ のときに押すとキーを削除）。
- タイトルの ◇ / 「全設定にキー」で、位置・回転・焦点距離・シフト・クリップ・被写界深度をまとめてキーにします。
- Blender の自動キー挿入がオンなら、HUD で値を変えると自動でキーが入ります。

### カメラごとの解像度

Damien Picard による Per-Camera Resolution（2.3.2）を統合しています。

- カメラごとにカスタム解像度をオン / オフでき、そのカメラがシーンカメラになるとレンダー設定へ自動で反映されます
  （フレーム変更やカメラマーカーでの切り替え、レンダー開始時にも反映）。
- カメラデータプロパティに「カスタム解像度」パネル、出力プロパティのフォーマットに解像度プリセットを追加します。
- プリセット: HD / Full HD / QHD / 4K UHD / DCI 4K、正方形、縦動画、A0〜A6・B0〜B6・Letter（300dpi、縦 / 横）など
- 解像度がフレームごとに変わるアニメーションは、画像シーケンスで「カメラ解像度対応アニメーションレンダー」を使ってください。

---

## アドオン設定

| 項目 | 内容 |
|---|---|
| HUD 位置 / スケール / アクセントカラー | HUD の配置・大きさ・色 |
| リスト最大行数 | カメラ一覧に表示する行数 |
| カメラ枠ガイド | カメラ枠のコーナーブラケットと中心マーカー |
| HUD 位置をリセット | ドラッグで動かした HUD を元の位置に戻す |
| フォールバック距離 / Empty サイズ / Track To 拘束を追加 | ターゲット Empty 作成時の設定 |
| キーマップ | カメラリストを開くキー |

---

## 言語と翻訳

UI・ツールチップ・メッセージは、Blender の **インターフェース言語** と「翻訳」設定に合わせて自動で切り替わります。

- 対応: 日本語 / English（Per-Camera Resolution 由来の項目はフランス語 / 中国語の訳も利用できます）
- コード中の文字列は英語（翻訳キー）で書き、訳は次のファイルにまとめています。
  `i18n.py` がアドオン登録時に 1 回だけ登録します。

| ファイル | 内容 |
|---|---|
| `i18n.py` | 翻訳の登録と、`iface_` / `tip_` / `rpt_` ヘルパー |
| `i18n_ja_ops.py` | オペレーター・ポップアップの日本語訳 |
| `i18n_ja_res.py` | カメラごとの解像度・プリセットの日本語訳 |
| `i18n_ja_misc.py` | カメラリスト・設定・ビュー記憶などの日本語訳 |
| `hud.py`（`_JA_HUD` / `_JA_UI` / `_JA_OPS`） | HUD の日本語訳 |
| `translations.py` | Per-Camera Resolution 由来のフランス語 / 中国語訳 |

訳を追加・修正するときは、各ファイルの辞書に `"English": "日本語"` を追加・編集してください。
Blender 本体にも同じ英語がある場合は Blender 本体の訳が優先されるため、必要に応じて別の英語や専用のコンテキストを使ってください。

---

## 注意事項

- 親を持つカメラには軌道を作成できません。
- フォーカス表示 / Z 深度表示はシーンの深度情報を使って描いています。X 線表示中は表示されません。
- 旧版（Follow Path 方式）で作った軌道は、軌道を作り直すと新しい方式に置き換わります。
- 解像度プリセットのファイル名（`presets/per_camera_resolution/`）はデータのため翻訳されません。

---

## クレジット / ライセンス

- ToPu_CameraList: http4211
- Per-Camera Resolution 統合部分: Damien Picard による Per-Camera Resolution 2.3.2 をもとにしています（GPL-3.0-or-later）。
- Stored Views 部分: Blender Foundation の Stored Views アドオン（v0.4.0）をもとにしています。

本アドオンは GPL-2.0-or-later のコードと GPL-3.0-or-later のコードを含むため、全体として **GPL-3.0-or-later** で配布されます。

---

## English

**ToPu_CameraList** is a Blender add-on (Blender 4.2+) for working with cameras: a camera list, per-camera resolution,
stored views, and a **viewport camera HUD** that lets you adjust camera settings directly in the 3D Viewport.

**Install:** Download the zip from [Releases](../../releases), then *Edit > Preferences > Add-ons > Install from Disk*.
When updating, disable the add-on and restart Blender before installing the new zip.
Disable the standalone Per-Camera Resolution add-on if you use it (its features are integrated here).

**Features**

- **Camera List (Alt + C):** render buttons, render engine switch, create camera from view, list cameras by name or
  collection (select / view / target empty / marker / dolly zoom / display / settings / delete), stored views.
- **Camera HUD:** toggle per viewport with the camera icon next to the View / Select / Add / Object menus.
  - *Outside camera view:* camera list with lock-on highlighting, create camera from view, stored views,
    view modes (Normal / Silhouette / Z-Depth).
  - *In camera view:* camera panel (lens, focus, rig, output), view lock / transform lock / fly / walk,
    shading and view modes, viewport DOF, composition guides and passepartout.
  - Drag / scroll / click-to-type on any value; minimize the panel to get compact readouts inside the camera frame.
  - Dolly zoom, focus object picking, focus range visualization, silhouette and Z-depth display.
- **Camera Rig:** create a look-at target and an orbit rig without changing the current view.
  Orbit angle / scale / position, one-turn animation, and a keyable "Link Target" switch (Child Of influence).
- **Keyframes:** ◇ buttons on each value, "Key All", and auto-keying support.
- **Per-Camera Resolution:** integrated from Damien Picard's add-on, with resolution presets (video, square,
  vertical, A/B/Letter print sizes at 300 dpi).
- **Languages:** English / Japanese, switching automatically with Blender's interface language
  (French / Chinese for the Per-Camera Resolution parts).

**License:** GPL-3.0-or-later (contains GPL-2.0-or-later code and GPL-3.0-or-later code from Per-Camera Resolution).
Credits: http4211; Per-Camera Resolution by Damien Picard; Stored Views by the Blender Foundation.
