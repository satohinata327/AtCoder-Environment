# ==============================
# AtCoder Docker 環境
# ==============================
#
# [イメージをビルド]
# AtCoder/ ディレクトリで実行
#   docker compose build
#
# キャッシュを使わず完全に再ビルドする場合
#   docker compose build --no-cache
#
# [コンテナを起動して中に入る]
#   docker compose run --rm atcoder
#
# --rm:
#   コンテナ終了時にそのコンテナを自動削除する
#   Mac 側の AtCoder/ や Docker Volume のデータは消えない
#
# [コンテナから抜ける]
#   exit
#
# [AtCoder コンテストを取得]
#   acc new abcXXX
#
# 全問題を自動取得する設定（初回のみ）
#   acc config default-task-choice all
#
# [問題をコンパイル + サンプルテスト]
#   cd abcXXX/a
#   ojrun
#
# ojrun の内部処理:
#   g++ -std=c++17 -O2 main.cpp -o main
#   oj test -c "./main" -d tests
#
# [Docker の状態確認]
# 起動中のコンテナ
#   docker ps
#
# 終了済みも含む全コンテナ
#   docker ps -a
#
# 作成済みイメージ
#   docker images
#
# ==============================

FROM gcc:15.3.0

RUN apt-get update && apt-get install -y --no-install-recommends \
    python3 \
    python3-venv \
    nodejs \
    npm \
    git \
    openssh-client \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

RUN python3 -m venv /opt/oj \
    && /opt/oj/bin/pip install --no-cache-dir --upgrade pip setuptools \
    && /opt/oj/bin/pip install --no-cache-dir online-judge-tools

# atcoder-cli をインストールし、acc new 時のログインチェックを削除
RUN npm install -g atcoder-cli \
    && python3 -c "from pathlib import Path; p=Path('/usr/local/lib/node_modules/atcoder-cli/bin/index.js'); s=p.read_text(); old='await s.checkSession()||await s.login();'; assert old in s, 'atcoder-cli login check pattern not found'; p.write_text(s.replace(old, '', 1))"

COPY ojrun /usr/local/bin/ojrun
RUN chmod +x /usr/local/bin/ojrun

ENV PATH="/opt/oj/bin:${PATH}"

WORKDIR /workspace

CMD ["bash"]

FROM gcc:15.3.0

RUN apt-get update && apt-get install -y --no-install-recommends \
    python3 \
    python3-venv \
    nodejs \
    npm \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# online-judge-tools
RUN python3 -m venv /opt/oj \
    && /opt/oj/bin/pip install --no-cache-dir --upgrade pip setuptools \
    && /opt/oj/bin/pip install --no-cache-dir online-judge-tools

# atcoder-cli
# acc new時のログインチェックを削除
RUN npm install -g atcoder-cli \
    && python3 -c "from pathlib import Path; p=Path('/usr/local/lib/node_modules/atcoder-cli/bin/index.js'); s=p.read_text(); old='await s.checkSession()||await s.login();'; assert old in s, 'atcoder-cli login check pattern not found'; p.write_text(s.replace(old, '', 1))"

# ojrun
COPY ojrun /usr/local/bin/ojrun
RUN chmod +x /usr/local/bin/ojrun

ENV PATH="/opt/oj/bin:${PATH}"

WORKDIR /workspace

CMD ["bash"]