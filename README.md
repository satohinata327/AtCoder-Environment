# AtCoder Development Environment

Mac + Docker + VS Code Dev Containers でAtCoder用のC++環境を構築するためのリポジトリです。

macOSでは標準のClang環境とAtCoderのGCC環境に差があり、`#include <bits/stdc++.h>` などで問題が起きるため、Docker上にLinux + GCC環境を構築しています。

- GCC
- atcoder-cli (`acc`)
- online-judge-tools (`oj`)
- Git / SSH
- `ojrun` によるコンパイル + サンプルテスト

AtCoderのコード自体はDocker Volume上に保存し、別リポジトリ `MyAtCoder` で管理します。

## Requirements

以下をインストールしてください。

- Docker Desktop
- VS Code
- VS Code拡張機能 `Dev Containers`

## Project Structure
以下のディレクトリ構成で環境を構築します。
```text
AtCoder-Environment/
├── .devcontainer/
│   └── devcontainer.json
├── docker/
│   ├── Dockerfile
│   └── ojrun
├── compose.yaml
└── README.md
```

Docker Volume側：

```text
/workspace
└── contests/
    ├── abc123/
    │   ├── a/
    │   │   ├── main.cpp
    │   │   └── tests/
    │   ├── b/
    │   └── ...
    ├── abc456/
    └── ...
```
## Setup

### 1. このリポジトリをclone

```bash
git clone <AtCoder-EnvironmentのURL>
cd AtCoder-Environment
```

### 2. Docker Volumeを作成

```bash
docker volume create atcoder-workspace-linux
docker volume create atcoder_atcoder-home
```
atcoder-workspace-linux: AtCoderのコードやGitリポジトリを保存。
atcoder_atcoder-home: SSHやGitなどのユーザー設定を保存。

どちらもDev Containerを終了・Rebuildしても保持されます。

### 3. VS Codeで開く

VS Codeで、

```text
Cmd + Shift + P
→ Dev Containers: Reopen in Container
```

を実行します。

初回はDocker imageがbuildされます。

## AtCoderコードの準備

Dev Container内では `/workspace/contests` がワークスペースとして開きます。

## atcoder-cli 初期設定

初回のみ実行します。

```bash
acc config default-task-choice all
```

これにより `acc new` でそのコンテストの全問題を取得します。

## Usage

コンテストを取得：

```bash
cd contests/
acc new abcXXX
```

問題ディレクトリへ移動：

```bash
cd abcXXX/a
```

`main.cpp` を編集し、

```bash
ojrun
```

を実行すると、内部で

```bash
g++ -std=c++17 -O2 main.cpp -o main
oj test -c "./main" -d tests
```

が実行され、コンパイルとサンプルテストをまとめて行えます。

## Notes

- AtCoderのコードはローカルのディレクトリへbind mountせず、Docker named volumeに保存しています。(deadlock発生のため)

- Dev Containerを終了・Rebuildしても `/workspace` のデータは残ります。

- ただし、以下のVolumeを削除するとAtCoderのコードや設定も失われるため注意してください。

```text
atcoder-workspace-linux
atcoder_atcoder-home
```

- 提出はブラウザで手動で行ってください。acc submitにはまだ対応していないです。