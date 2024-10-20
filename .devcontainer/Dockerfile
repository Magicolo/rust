FROM rust

RUN \
    apt update && \
    apt upgrade --yes && \
    apt install --yes git git-lfs curl jq gawk findutils grep exa nano screen rsync build-essential pkg-config make cmake libssl-dev lldb htop valgrind google-perftools llvm && \
    curl --silent https://raw.githubusercontent.com/nektos/act/master/install.sh | bash && \
    rustup toolchain install stable nightly --component rustfmt llvm-tools-preview rust-src clippy && \
    rustup toolchain install 1.70.0 1.65.0 1.60.0 --component clippy --profile minimal && \
    rustup component add miri --toolchain nightly && \
    rustup default stable && \  
    rustup update && \
    cargo install cargo-update cargo-expand cargo-audit cargo-msrv cargo-hack cargo-semver-checks cargo-release cargo-bloat cargo-llvm-cov && \
    apt autoremove --yes && \
    apt clean --yes && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    chmod -R a+rwx /usr/local/rustup /usr/local/cargo