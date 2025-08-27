FROM ubuntu

# Setup general.
RUN apt update && \
    apt upgrade --yes && \
    apt install --no-install-recommends --no-install-suggests --auto-remove --upgrade --yes ca-certificates git git-lfs curl jq gawk findutils grep nano screen rsync apt-utils dpkg gettext procps build-essential pkg-config make cmake libssl-dev lldb htop valgrind google-perftools llvm 7zip poppler-utils fd-find ripgrep fzf zoxide parallel software-properties-common pulseaudio alsa-utils libasound2-dev llvm-dev libclang-dev clang libopencv-dev gnupg gpg openssl && \
    update-ca-certificates && \
    apt autoremove --yes && \
    apt clean --yes && \
    rm --recursive --force /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Setup act.
RUN VERSION=$(curl --location --silent --show-error --fail "https://api.github.com/repos/nektos/act/releases/latest" | jq --raw-output '.tag_name') && \
    curl --location --silent --show-error --fail "https://github.com/nektos/act/releases/download/${VERSION}/act_Linux_x86_64.tar.gz" | \
    tar --extract --gzip --mode=0755 --directory="/usr/local/bin" --exclude=LICENSE --exclude=README.md

# Setup gemini.
RUN apt update && \ 
    apt install --no-install-recommends --no-install-suggests --auto-remove --upgrade --yes npm && \
    npm install --global @google/gemini-cli

# Setup rust.
ENV RUSTUP_HOME="/usr/local/rustup"
ENV CARGO_HOME="/usr/local/cargo"
ENV PATH="${PATH}:${CARGO_HOME}/bin"
RUN curl --location --silent --show-error --fail "https://sh.rustup.rs" | sh -s -- -y --default-toolchain none && \
    rustup toolchain install stable nightly --component rustfmt --component llvm-tools-preview --component rust-src --component clippy && \
    rustup toolchain install 1.79 1.75 1.70 1.65 1.64 1.63 1.62 1.61 1.60 --profile minimal && \
    rustup component add miri --toolchain nightly && \
    rustup default stable && \
    for name in $(rustup toolchain list --quiet); do rustup run "$name" cargo search --limit 1; done;

# Setup cargo.
RUN VERSION=$(curl --location --silent --show-error --fail "https://api.github.com/repos/cargo-bins/cargo-binstall/releases/latest" | jq --raw-output '.tag_name') && \
    curl --location --silent --show-error --fail "https://github.com/cargo-bins/cargo-binstall/releases/download/${VERSION}/cargo-binstall-x86_64-unknown-linux-gnu.tgz" | \
    tar --extract --gzip --mode=0755 --directory="/usr/local/bin" && \
    cargo binstall cargo-watch cargo-update cargo-expand cargo-audit cargo-msrv cargo-hack cargo-semver-checks cargo-release cargo-nextest cargo-bloat cargo-llvm-cov cargo-mutants taplo-cli && \
    cargo install --git "https://github.com/chevdor/tera-cli" && \
    chmod --recursive 0777 "$CARGO_HOME"