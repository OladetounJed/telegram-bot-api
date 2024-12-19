# Use Ubuntu 20.04 as the base image
FROM ubuntu:20.04

# Set environment variables to ensure non-interactive installation
ENV DEBIAN_FRONTEND=noninteractive

# Update and install dependencies
RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y \
    make \
    git \
    zlib1g-dev \
    libssl-dev \
    gperf \
    cmake \
    clang-10 \
    libc++-10-dev \
    libc++abi-10-dev

# Clone the Telegram Bot API server repository
RUN git clone --recursive https://github.com/tdlib/telegram-bot-api.git /telegram-bot-api

# Build the Telegram Bot API server
WORKDIR /telegram-bot-api
RUN rm -rf build && mkdir build && cd build && \
    CXXFLAGS="-stdlib=libc++" CC=/usr/bin/clang-10 CXX=/usr/bin/clang++-10 cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX:PATH=.. .. && \
    cmake --build . --target install

# Expose the default port (8081 for HTTP)
EXPOSE 8081

# Set the command to run the API server
CMD ["./bin/telegram-bot-api", "--http-port=8081", "--log=log.txt"]
