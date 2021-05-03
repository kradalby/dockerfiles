FROM swift:5.4-bionic AS builder

RUN git clone -b swift-5.4-branch https://github.com/apple/swift-format.git /app/swift-format \
    && cd /app/swift-format \
    && swift build --configuration release \
    && ln -s /app/swift-format/.build/release/swift-format /usr/local/bin/swift-format

FROM swift:5.4-slim
COPY --from=builder /app/swift-format/.build/release/swift-format /usr/local/bin/swift-format
CMD ["/usr/local/bin/swift-format"]
