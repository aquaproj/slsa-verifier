FROM golang:1.20@sha256:74a382917f6eaa7cc2d000dc2cd412a7f823f343b3b6268b20d84d057bc56718 AS base
WORKDIR /src
ENV CGO_ENABLED=0
COPY . ./
RUN go mod vendor
RUN go build -o slsa-verifier -trimpath -ldflags "-s -w -extldflags=-static" -mod=vendor ./cli/experimental/service/main.go

# For testing.
# COPY ./service/service slsa-verifier
# RUN chmod o+x ./slsa-verifier

FROM gcr.io/distroless/base:nonroot@sha256:bac3b6ac934c1a5eac5a3971b881079bb0e2960cfb2b848a42790d36e517f8ce
COPY --from=base /src/slsa-verifier /
ENTRYPOINT ["/slsa-verifier"]
