FROM golang:1.13.4-alpine3.10 as build

# ワーキングディレクトリ指定 
WORKDIR /app

# 標準ライブラリをインストール
RUN apk update --no-cache \
  && apk add --no-cache \
    git \
    gcc \
    musl-dev

# ファイルをイメージ内にコピー
COPY go.mod .
COPY go.sum .

# 明示的な依存パッケージのダウンロード
RUN go mod download

COPY . .

# クロスコンパイル
RUN GOOS=linux GOARCH=amd64 go build -o app main.go

# ホットリロード
RUN GO111MODULE=off go get github.com/oxequa/realize

# DB
RUN GO111MODULE=off go get -tags 'mysql' -u github.com/golang-migrate/migrate/cmd/migrate