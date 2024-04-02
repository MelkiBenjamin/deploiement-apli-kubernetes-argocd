FROM rust:1.77 as builder

RUN USER=root cargo new --bin product-service

# Create a new directory for our application
WORKDIR /product-service

# Set the build argument for the app version number
ARG APP_VERSION=0.1.0

# Copy the source files to the container
ADD . ./
RUN apt-get update && apt-get install -y musl-tools
RUN apt-get install -y openssl
RUN apt-get install -y musl-dev
RUN apt-get install -y libssl-dev
RUN ENV OPENSSL_DIR=/usr/include/openssl
RUN apt-get install -y pkg-config

RUN rustup target add x86_64-unknown-linux-musl
# Build the application
RUN cargo build --target x86_64-unknown-linux-musl --release

# Create a new stage and copy the binary from the builder stage
FROM alpine as runner
WORKDIR /app

# Set the build argument for the app version number
ARG APP_VERSION=0.1.0

# Not ideal but needed to execute health checks in docker-compose
RUN apk add wget

# Copy the binary from the builder stage
COPY --from=builder /product-service/target/release/product-service /app

# Set the environment variable for the app version number
ENV APP_VERSION=$APP_VERSION

# Run the application
CMD ["./product-service"]
