# syntax = docker/dockerfile-upstream:master-labs
#-*-mode:dockerfile;indent-tabs-mode:nil;tab-width:2;coding:utf-8-*-
# vi: ft=dockerfile tabstop=2 shiftwidth=2 softtabstop=2 expandtab:
# ─── USAGE ──────────────────────────────────────────────────────────────────────
# docker run --mount type=bind,source="$PWD",target='/userspace' --rm -it fjolsvin/sad
# ────────────────────────────────────────────────────────────────────────────────
FROM fjolsvin/rust-builder-alpine AS sad-builder
RUN \
  --mount=type=cache,target=/root/.cargo \
  --mount=type=cache,target=/usr/local/cargo/registry \
  cargo install --locked --all-features --root "/workspace" -j `nproc` --target "$(apk --print-arch)-unknown-linux-musl" --git https://github.com/ms-jpq/sad
FROM fjolsvin/upx AS compression-layer
COPY --from=sad-builder "/workspace" "/workspace"
RUN \
  compress ;
FROM alpine:edge
WORKDIR /userspace
VOLUME /userspace
COPY --from=compression-layer "/workspace" "/workspace"
ENTRYPOINT [ "/workspace/sad" ]
