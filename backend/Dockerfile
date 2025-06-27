# ==========  ベースイメージ設定  ==========
# OpenJDK 17をベースにする（Spring Boot 3.x推奨）
FROM openjdk:17-jdk-slim

# ==========  作業ディレクトリ設定  ==========
# コンテナ内での作業フォルダを指定
WORKDIR /app

# ==========  Gradleラッパーをコピー  ==========
# Gradle関連ファイルを先にコピー（キャッシュ効率化）
COPY gradlew .
COPY gradle gradle/
COPY build.gradle.kts .
COPY settings.gradle.kts .

# ==========  Gradleラッパーに実行権限付与  ==========
RUN chmod +x ./gradlew

# ==========  依存関係のダウンロード  ==========
# ソースコードをコピーする前に依存関係をダウンロード
# （依存関係が変わらない限り、この層はキャッシュされる）
RUN ./gradlew dependencies --no-daemon

# ==========  ソースコードをコピー  ==========
# アプリケーションのソースコードをコピー
COPY src src/

# ==========  アプリケーションビルド  ==========
# Kotlinコンパイル & JARファイル作成
RUN ./gradlew build --no-daemon -x test

# ==========  ポート公開  ==========
# Spring Bootのデフォルトポート8080を公開
EXPOSE 8080

# ==========  アプリケーション起動  ==========
# 開発モード：ソースコード変更を監視して自動再起動
CMD ["./gradlew", "bootRun", "--no-daemon"]

# ==========  本番用起動コマンド（コメントアウト）  ==========
# 本番環境では以下を使用：
# CMD ["java", "-jar", "build/libs/portfolio-0.0.1-SNAPSHOT.jar"]