# 1. Aşama: Build (Daha yeni bir Go sürümü kullanıyoruz)
FROM golang:1.24 AS builder

# xcaddy aracını kur
RUN go install github.com/caddyserver/xcaddy/cmd/xcaddy@latest

WORKDIR /build

# Caddy'yi Coraza ile derle
# ÖNEMLİ: Caddy sürümünü v2.8.4 olarak sabitledik, uyumluluk sorunu olmasın diye.
RUN xcaddy build v2.9.1 \
    --with github.com/corazawaf/coraza-caddy/v2@latest

# 2. Aşama: Final İmaj
FROM caddy:2.9.1

# Derlenmiş binary'yi kopyala
COPY --from=builder /build/caddy /usr/bin/caddy

# Klasörleri oluştur ve izinleri ver
RUN mkdir -p /audit /ruleset && \
    chmod 777 /audit
