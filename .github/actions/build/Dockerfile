FROM rust:latest

RUN cargo install mdbook --no-default-features --features output --vers "^0.4.28"
RUN cargo install mdbook-mermaid

CMD ["mdbook", "build"]