# Waiting Server

Waiting Server는 접속 대기열 시스템으로 일시적으로 많은 트래픽이 발생하는 서비스에 과도한 트래픽의 유입을 방지해주고 서비스를 보호합니다.

서버 흐름 및 구조에 대한 자세한 설명은 [여기](https://velog.io/@duffyishere/서버계의-테이블링-구현하기)를 참고해주세요.

## 요구사항

애플리케이션을 구축하고 실행하려면 다음 요구사항이 필요합니다:
- [Go 1.21.0](https://go.dev/dl) 
- [Redis 7.0.12](https://redis.io)

## 실행하기

서버를 실행하는 방법에는 다음 옵션이 있습니다.

**Docker Compose로 실행하기**

```shell
docker-compose up -d
```

**Docker로 실행하기**

```shell
docker build -t waiting-server:1.0 .
docker run -p 3000:3000 waiting-server:1.0
```

**로컬 터미널에서 실행하기**
```shell
cd ./core
go build -o app main.go redisUtils.go
./app
```

서버가 실행되면 [http://localhost:3000/p](http://localhost:3000/p)로 접속할 수 있습니다.

## 시작하기

서버를 사용하기 위한 간단한 가이드를 제공합니다.

### 요청

다음의 HTTP GET 요청을 사용하세요:

```http
GET localhost:3000/p
```

**헤더**

요청 헤더에는 다음 정보를 포함해야 합니다:

- `$REQUEST_ID_HEADER_KEY_NAME`: 문자열 타입, 응답에서 반환될 request-id 값입니다.

### 사용법

1. '/p'로 GET 요청을 보냅니다.
2. 응답으로 받은 request-id 값을 '.env' 파일의 'REQUEST_ID_HEADER_KEY_NAME'로 넣어서 1초마다 다시 요청합니다.
3. ticket의 값이 빈 문자열("")이 아닌 경우 해당 문자열을 사용하여 Main 서버로 입장을 요청합니다.

### 응답

서버 응답은 다음과 같은 JSON 형식을 갖습니다:
```json
{
  "request-id" : "string",
  "count"      : 1,
  "ticket"     : "string"
}
```
