FROM golang:1.14 as builder

RUN mkdir -p /podinfo/

WORKDIR /podinfo

COPY . .

RUN GIT_COMMIT=$(git rev-list -1 HEAD) && \
    CGO_ENABLED=0 GOOS=linux go build -ldflags "-s -w \
    -X github.com/trilogy-group/podinfo/pkg/version.REVISION=${GIT_COMMIT}" \
    -a -o bin/podinfo cmd/podinfo/*


FROM alpine:3.11

RUN addgroup -S app \
    && adduser -S -g app app \
    && apk --no-cache add \
    curl openssl netcat-openbsd

WORKDIR /home/app

COPY --from=builder /podinfo/bin/podinfo .
COPY ./ui ./ui
RUN chown -R app:app ./

USER app

CMD ["./podinfo"]
