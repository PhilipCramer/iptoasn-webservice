FROM alpine:3.21 AS build
WORKDIR /tmp/iptoasn
RUN apk add --update --no-cache ca-certificates \
    llvm-libunwind \
    libgcc \
    rust \
    cargo \
    libressl-dev
COPY Cargo.* ./
COPY target/release ./target/release
COPY src ./src
RUN cargo build --release
RUN strip target/release/iptoasn-webservice


FROM alpine:3.21
COPY docker/iptoasn-entrypoint.sh /iptoasn-entrypoint.sh
RUN chmod +x /iptoasn-entrypoint.sh \
    && apk add --no-cache libgcc \
    libressl \
    llvm-libunwind \
    && adduser -D app \
    && mkdir /cache \
    && chown app /cache
USER app
COPY --from=build /tmp/iptoasn/target/release/iptoasn-webservice /usr/bin/iptoasn-webservice
ENV RUST_LOG=info
ENTRYPOINT ["/iptoasn-entrypoint.sh"]
