# Build the manager binary
FROM golang:1.16.6-alpine3.13 as builder

WORKDIR /workspace
# Copy the Go Modules manifests
COPY go.mod go.mod
COPY go.sum go.sum
# cache deps before building and copying source so that we don't need to re-download as much
# and so that source changes don't invalidate our downloaded layer

RUN go env -w GO111MODULE=on ; \
    go env -w GOPROXY=https://goproxy.cn,direct;\
    go mod download

# Copy the go source
COPY cmd/fluent-manager/main.go main.go
COPY apis apis/
COPY controllers controllers/
COPY pkg pkg/

# Build
RUN CGO_ENABLED=0 go build -a -o manager main.go


FROM ubuntu:20.04
WORKDIR /
COPY --from=builder /workspace/manager .

ENTRYPOINT ["/manager"]
