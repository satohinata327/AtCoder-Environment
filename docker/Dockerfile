# AtCoder用の開発環境（GCC、acc、oj、Vimなど）
#
# VS Code: Dev Containers: Reopen in Container
# 変更を反映: Dev Containers: Rebuild Container
#
# Docker Composeで使う場合（リポジトリルートで実行）:
#   docker compose build atcoder
#   docker compose run --rm atcoder
#
# コンテナ内での使用例:
#   acc new abcXXX
#   cd abcXXX/a
#   vim main.cpp
#   ojrun
#
FROM gcc:15.3.0

RUN apt-get update && apt-get install -y --no-install-recommends \
    python3 \
    python3-venv \
    nodejs \
    npm \
    git \
    openssh-client \
    vim \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# online-judge-tools
RUN python3 -m venv /opt/oj \
    && /opt/oj/bin/pip install --no-cache-dir --upgrade pip setuptools \
    && /opt/oj/bin/pip install --no-cache-dir online-judge-tools

# atcoder-cli
RUN npm install -g atcoder-cli

# ojrun
COPY ojrun /usr/local/bin/ojrun
RUN chmod +x /usr/local/bin/ojrun

ENV PATH="/opt/oj/bin:${PATH}"

WORKDIR /workspace

CMD ["bash"]
