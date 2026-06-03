# デジタルツイン演習 — 提出用リポジトリ

本リポジトリは、デジタルツイン演習（90 分 × 2 コマ）の提出用テンプレートです。

提出対象は **Class ②（Day 2）の SLAM + Navigation 演習の成果物** です。
Class ① はマニュアルに沿って体験を進めるのみで、提出物はありません。

> 📖 詳しい演習手順は Moodle 配布の **「デジタルツイン演習 学生用マニュアル（Day 1 / Day 2）」** を参照してください。
> 本リポジトリでは提出物の置き場所と提出方法のみ案内します。

## 演習の流れ（90 分 × 2 コマ）

| Class | 内容 | 提出 |
|---|---|:-:|
| **①**（Day 1）| Docker → noVNC → ROS 2 → Gazebo + TurtleBot3 → 自律走行 | なし |
| **②**（Day 2）| SLAM で地図作成 → Navigation で自律移動 → 本リポジトリに提出 | ✅ |

## 提出物について

`提出物/` フォルダに以下を揃えて push してください。GitHub Classroom が自動採点します。

```
提出物/
├── COMPLETED.md           ← チェックリスト + 振り返り（記入）
├── screenshots/           ← Class ② で撮影したスクショ（必須 3 枚 + 任意 1 枚）
│   ├── day2-1-slam.png        SLAM 走行中（必須）
│   ├── day2-2-map.png         完成した地図（必須）
│   ├── day2-3-nav.png         Navigation 自律走行（必須）
│   └── day2-4-waypoint.png    Waypoint モードでの巡回（任意・加点）
└── maps/                  ← SLAM で生成した地図ファイル
    ├── map.pgm
    └── map.yaml
```

### スクリーンショットの撮影タイミング

| ファイル名 | 撮影タイミング | 必須／任意 |
|---|---|:-:|
| `day2-1-slam.png` | SLAM 走行が始まり、RViz の地図が半分以上描かれた時点 | 必須 |
| `day2-2-map.png` | マップ内のすべてを探索し終え、`map_saver_cli` で保存する直前の RViz | 必須 |
| `day2-3-nav.png` | RViz の「Nav2 Goal」で目的地を指定し、ロボットが自律走行している瞬間 | 必須 |
| `day2-4-waypoint.png` | Waypoint モードで複数の経由地を指定し、ロボットが巡回している最中 | 任意（加点）|

### 地図ファイルの保存

コンテナ内で地図を保存し、ホスト側にコピーします。

```bash
# コンテナ内（noVNC のターミナル）
ros2 run nav2_map_server map_saver_cli -f ~/map
```

```powershell
# ホスト側（PowerShell、リポジトリのルートで実行）
docker cp ros2-vnc:/home/ubuntu/map.pgm  .\提出物\maps\map.pgm
docker cp ros2-vnc:/home/ubuntu/map.yaml .\提出物\maps\map.yaml
```

### Git で push

```powershell
cd ~\eis\<リポジトリ名>
git add 提出物/
git commit -m "デジタルツイン演習 完了報告"
git push
```

## 採点項目（合計 5 点 + 加点 1 点）

| step | 内容 | 配点 |
|---|---|:-:|
| step1 | 学籍番号・氏名が記入されている | 1 |
| step2 | COMPLETED.md チェックボックスに `[x]` が 3 個以上 | 1 |
| step3 | 必須スクショ 3 枚（`day2-1-slam.png` ・ `day2-2-map.png` ・ `day2-3-nav.png`）すべて提出 | 1 |
| step4 | `maps/map.pgm` または `maps/map.yaml` が提出されている | 1 |
| step5 | 振り返り欄が記入されている | 1 |
| **加点** | `day2-4-waypoint.png` 提出（Waypoint 巡回）| **+1**（教員手動）|

採点結果は **「Actions」タブ → 最新のラン → Summary** で確認できます。
**自動採点は 5 点満点**で表示されます（緑チェック = 5/5）。
Waypoint スクショ（加点）は Summary 表に ✅／❌ で表示されますが、自動採点のスコアには含まれません。提出した場合は教員が後日 +1 点を加算します。

> Actions タブの 🔴 赤色は気にせず、Summary の ✅ / ❌ で点数を確認してください。

## コマンド早見表

新しいターミナルを開いたら、Gazebo 系コマンドの前に以下を 1 回実行：

```bash
source /usr/share/gazebo/setup.bash
export TURTLEBOT3_MODEL=burger
```

### Class ①（Day 1）の主要コマンド

| コマンド | 説明 |
|---|---|
| `ros2 run turtlesim turtlesim_node` | turtlesim 起動 |
| `ros2 run turtlesim turtle_teleop_key` | turtlesim 操作 |
| `ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py` | Gazebo + TurtleBot3 |
| `ros2 run turtlebot3_teleop teleop_keyboard` | TurtleBot3 を操作 |
| `ros2 run turtlebot3_gazebo turtlebot3_drive` | 自律走行 |

### Class ②（Day 2）の主要コマンド

| コマンド | 説明 |
|---|---|
| `ros2 launch turtlebot3_cartographer cartographer.launch.py use_sim_time:=True` | SLAM 起動（RViz も自動で開く）|
| `ros2 run nav2_map_server map_saver_cli -f ~/map` | 地図を保存（map.pgm + map.yaml）|
| `ros2 launch turtlebot3_navigation2 navigation2.launch.py use_sim_time:=True map:=$HOME/map.yaml` | Navigation 起動 |

## 締切

`授業 Moodle ページ` を参照してください。

## 困ったとき

詳しい Q&A は Moodle の学生用マニュアルを参照してください。
