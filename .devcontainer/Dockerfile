FROM rust

RUN apt update && \
    apt upgrade --yes && \
    apt install --yes git git-lfs curl jq gawk findutils grep exa nano screen rsync build-essential pkg-config make cmake libssl-dev lldb htop valgrind google-perftools llvm ffmpeg 7zip poppler-utils fd-find ripgrep fzf zoxide imagemagick && \
    curl --silent https://raw.githubusercontent.com/nektos/act/master/install.sh | bash && \
    rustup toolchain install stable nightly --component rustfmt llvm-tools-preview rust-src clippy && \
    rustup toolchain install 1.70 1.65 1.61 1.60 --profile minimal && \
    rustup component add miri --toolchain nightly && \
    rustup default stable && \  
    rustup update && \
    cargo install cargo-watch cargo-update cargo-expand cargo-audit cargo-msrv cargo-hack cargo-semver-checks cargo-release cargo-bloat cargo-llvm-cov yazi-fm yazi-cli && \
    apt autoremove --yes && \
    apt clean --yes && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    chmod -R a+rwx /usr/local/rustup /usr/local/cargo