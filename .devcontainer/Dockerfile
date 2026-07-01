FROM ubuntu:resolute

ENV DEBIAN_FRONTEND=noninteractive

# Setup general.
RUN apt update && \
    apt upgrade --yes && \
    apt install --no-install-recommends --no-install-suggests --auto-remove --upgrade --yes ca-certificates sudo curl grep git git-lfs jq yq gawk rsync findutils apt-utils nano screen gettext dpkg procps software-properties-common build-essential pkg-config htop llvm lldb libssl-dev libatomic1 gnupg gpg poppler-utils openssl openssh-client make cmake valgrind google-perftools llvm-dev libclang-dev libopencv-dev clang parallel pulseaudio alsa-utils libasound2-dev lsb-release ripgrep fd-find fzf bat tree zip unzip eza zoxide duf btop btm hyperfine git-delta sd && \
    ln --symbolic --force /usr/bin/fdfind /usr/local/bin/fd && \
    ln --symbolic --force /usr/bin/batcat /usr/local/bin/bat && \
    ln --symbolic --force /usr/bin/btm /usr/local/bin/bottom && \
    update-ca-certificates && \
    apt autoremove --yes && \
    apt clean --yes && \
    rm --recursive --force /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Setup node.
RUN curl --fail --silent --show-error --location https://deb.nodesource.com/setup_lts.x | bash - && \
    apt install --yes nodejs && \
    npm install --global @github/copilot @openai/codex @google/gemini-cli @anthropic-ai/claude-code @kilocode/cli opencode-ai @earendil-works/pi-coding-agent @oh-my-pi/pi-coding-agent cline typescript typescript-language-server vscode-langservers-extracted prettier eslint-config-prettier eslint-plugin-import eslint-plugin-prettier eslint-plugin-react eslint-plugin-simple-import-sort license-checker @stryker-mutator/core

# Setup python.
RUN apt update && \
    apt install --no-install-recommends --yes python3 python3-pip python3-venv && \
    pip install --break-system-packages --ignore-installed --no-cache-dir --upgrade basedpyright taplo schemathesis git+https://github.com/NousResearch/hermes-agent.git

# Setup gh.
RUN mkdir --parents --mode 0755 "/etc/apt/keyrings" \
    && curl --location --silent --show-error --fail \
    "https://cli.github.com/packages/githubcli-archive-keyring.gpg" \
    --output "/etc/apt/keyrings/githubcli-archive-keyring.gpg" \
    && chmod go+r "/etc/apt/keyrings/githubcli-archive-keyring.gpg" \
    && mkdir --parents --mode 0755 "/etc/apt/sources.list.d" \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" > "/etc/apt/sources.list.d/github-cli.list" \
    && apt update \
    && apt install --no-install-recommends --yes gh

# Setup act.
RUN VERSION=$(curl --location --silent --show-error --fail "https://api.github.com/repos/nektos/act/releases/latest" | jq --raw-output '.tag_name') && \
    curl --location --silent --show-error --fail "https://github.com/nektos/act/releases/download/${VERSION}/act_Linux_x86_64.tar.gz" | \
    tar --extract --gzip --mode=0755 --directory="/usr/local/bin" --exclude=LICENSE --exclude=README.md

# Setup trivy.
RUN mkdir --parents "/usr/share/keyrings" && \
    curl --location --silent --show-error --fail "https://aquasecurity.github.io/trivy-repo/deb/public.key" | gpg --dearmor --output "/usr/share/keyrings/trivy.gpg" && \
    echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb generic main" > "/etc/apt/sources.list.d/trivy.list" && \
    apt update --yes && \
    apt install --no-install-recommends --yes trivy

# Setup osv.
RUN curl --location --silent --show-error --fail \
    "https://github.com/google/osv-scanner/releases/latest/download/osv-scanner_linux_amd64" \
    --output "/usr/local/bin/osv-scanner" && \
    chmod 0755 "/usr/local/bin/osv-scanner"

# Setup scorecard.
RUN VERSION=$(curl --location --silent --show-error --fail "https://api.github.com/repos/ossf/scorecard/releases/latest" | jq --raw-output '.tag_name') && \
    curl --location --silent --show-error --fail \
    "https://github.com/ossf/scorecard/releases/download/${VERSION}/scorecard_${VERSION#v}_linux_amd64.tar.gz" \
    --output "/tmp/scorecard.tar.gz" && \
    tar --extract --gzip --file "/tmp/scorecard.tar.gz" --directory "/tmp" && \
    mv "/tmp/scorecard" "/usr/local/bin/scorecard" && \
    chmod 0755 "/usr/local/bin/scorecard" && \
    rm --force "/tmp/scorecard.tar.gz"

# Setup gitleaks.
RUN VERSION=$(curl --location --silent --show-error --fail "https://api.github.com/repos/gitleaks/gitleaks/releases/latest" | jq --raw-output '.tag_name') && \
    curl --location --silent --show-error --fail \
    "https://github.com/gitleaks/gitleaks/releases/download/${VERSION}/gitleaks_${VERSION#v}_linux_x64.tar.gz" \
    --output "/tmp/gitleaks.tar.gz" && \
    tar --extract --gzip --file "/tmp/gitleaks.tar.gz" --directory "/tmp" gitleaks && \
    mv "/tmp/gitleaks" "/usr/local/bin/gitleaks" && \
    chmod 0755 "/usr/local/bin/gitleaks" && \
    rm --force "/tmp/gitleaks.tar.gz"

# Setup rust.
ENV RUSTUP_HOME="/usr/local/rustup"
ENV CARGO_HOME="/usr/local/cargo"
ENV PATH="${PATH}:${CARGO_HOME}/bin"
RUN curl --location --silent --show-error --fail "https://sh.rustup.rs" | sh -s -- -y --default-toolchain none && \
    rustup toolchain install stable nightly --component rustfmt --component llvm-tools-preview --component rust-src --component clippy && \
    rustup component add miri --toolchain nightly && \
    rustup default stable
RUN VERSION=$(curl --location --silent --show-error --fail "https://api.github.com/repos/cargo-bins/cargo-binstall/releases/latest" | jq --raw-output '.tag_name') && \
    curl --location --silent --show-error --fail "https://github.com/cargo-bins/cargo-binstall/releases/download/${VERSION}/cargo-binstall-x86_64-unknown-linux-gnu.tgz" | \
    tar --extract --gzip --mode=0755 --directory="/usr/local/bin"
RUN cargo binstall --secure --no-confirm cargo-nextest cargo-watch cargo-update cargo-edit cargo-expand cargo-audit cargo-msrv cargo-hack cargo-bloat cargo-llvm-cov cargo-license cargo-deny cargo-dupes cargo-semver-checks cargo-release cargo-mutants taplo-cli && \
    chmod --recursive 0777 "$CARGO_HOME"

# Setup user.
RUN echo 'ubuntu ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/ubuntu
USER ubuntu

ENV CARGO_TARGET_DIR="/home/ubuntu/.rust/target"
RUN trivy image --download-db-only