FROM scratch
# FROM rust

# RUN \
#     apt update && \
#     apt upgrade --yes && \
#     apt install --yes git git-lfs curl build-essential pkg-config cmake libssl-dev lldb google-perftools && \
#     rustup install stable nightly && \
#     rustup default stable && \
#     rustup component add rustfmt clippy --toolchain stable && \
#     rustup component add rustfmt clippy --toolchain nightly && \
#     rustup update && \
#     cargo install cargo-expand cargo-audit cargo-msrv cargo-hack cargo-semver-checks cargo-llvm-cov && \
#     curl --silent https://raw.githubusercontent.com/nektos/act/master/install.sh | bash && \
#     apt autoremove --yes && \
#     apt clean --yes && \
#     rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
#     chmod -R a+rwx /usr/local/rustup /usr/local/cargo