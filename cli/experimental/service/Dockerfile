FROM golang:1.19@sha256:5d947843dde82ba1df5ac1b2ebb70b203d106f0423bf5183df3dc96f6bc5a705 AS base
WORKDIR /src
ENV CGO_ENABLED=0
COPY . ./
RUN go mod vendor
RUN go build -o slsa-verifier -trimpath -ldflags "-s -w -extldflags=-static" -mod=vendor ./cli/experimental/service/main.go

# For testing.
# COPY ./service/service slsa-verifier
# RUN chmod o+x ./slsa-verifier

FROM gcr.io/distroless/base:nonroot@sha256:a6441d19dfd755cd0782a4480fbeeb196a049c52d23b0dba81539098fda25d16
COPY --from=base /src/slsa-verifier /
ENTRYPOINT ["/slsa-verifier"]
