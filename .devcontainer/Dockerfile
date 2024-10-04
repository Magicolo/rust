FROM rust

RUN \
    apt update && \
    apt upgrade --yes && \
    apt install --yes git git-lfs curl jq gawk findutils grep exa nano screen rsync build-essential pkg-config make cmake libssl-dev lldb htop valgrind google-perftools llvm && \
    rustup install stable nightly && \
    rustup default stable && \  
    rustup component add rustfmt clippy llvm-tools-preview --toolchain stable && \
    rustup component add rustfmt clippy miri llvm-tools-preview --toolchain nightly && \
    rustup update && \
    cargo install cargo-expand cargo-audit cargo-msrv cargo-hack cargo-semver-checks cargo-release cargo-bloat cargo-llvm-cov && \
    curl --silent https://raw.githubusercontent.com/nektos/act/master/install.sh | bash && \
    apt autoremove --yes && \
    apt clean --yes && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    chmod -R a+rwx /usr/local/rustup /usr/local/cargo