# 1-
FROM rust:latest AS builder
WORKDIR /app
COPY . .
RUN cargo build --release

# 2-
FROM debian:bookworm-slim
WORKDIR /app

# install ca-certificates (as root, needed)
RUN apt-get update && apt-get install -y ca-certificates && rm -rf /var/lib/apt/lists/*

# create a non‑root user
RUN adduser --disabled-login --no-create-home --system appuser

# Copy binary and static files (still owned by root)
COPY --from=builder /app/target/release/html /app/crimson-engine
COPY --from=builder /app/public /app/public

# switch to the non root user for safety reasons
USER appuser

# run the engine
CMD ["./crimson-engine"]
