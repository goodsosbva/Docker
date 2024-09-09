## old code

```dockerfile
FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "app.mjs"]

```

## new code
```dockerfile
FROM node:12

WORKDIR /app

COPY package.json /app

RUN npm install

COPY . /app

EXPOSE 80

CMD ["node", "server.js"]
```

### 설명 및 실습 요약

두 Dockerfile 모두 캐싱 최적화를 위해 package.json을 먼저 복사하고, 그 다음에 의존성 설치(npm install)을 수행합니다. 이 순서는 Docker의 레이어 기반 캐시 메커니즘을 활용하는 핵심 방법입니다.

1. COPY package.json . 또는 COPY package.json /app
이 단계에서 package.json 파일만을 복사합니다. Docker는 이 단계가 변경되지 않는 한, 캐시된 레이어를 재사용합니다. 만약 package.json이 변경되지 않았다면, Docker는 다음 단계인 npm install의 캐시도 그대로 사용할 수 있습니다.

2. RUN npm install
이 단계에서 패키지를 설치합니다. package.json이 변경되지 않았다면, Docker는 이 레이어도 캐시에서 재사용할 수 있으므로, 빌드 시간이 현저히 줄어듭니다. 만약 package.json이 변경되면, 이 단계에서만 새로 패키지를 설치하고 그 이후 단계의 캐시는 계속 유효하게 유지됩니다.

3. COPY . . 또는 COPY . /app
이 단계에서 소스 코드 전체를 복사합니다. 이 단계에서 소스 코드가 자주 변경되더라도, 이전 단계의 캐시가 유효하게 남아 있기 때문에 npm install이 다시 실행되지 않습니다. 이 방식은 불필요한 의존성 재설치를 방지해 효율적인 캐싱을 제공합니다.

#### 최적화 요약
의존성 설치는 package.json 파일의 변경 여부에만 영향을 받습니다. 소스 코드가 변경되더라도 npm install을 반복하지 않기 때문에 빌드 시간이 단축됩니다.
소스 코드를 복사하는 단계는 마지막에 수행되어야 의존성 설치 캐시가 최대한 활용됩니다.
이 구조 덕분에 소스 코드가 빈번하게 변경되더라도 의존성 설치는 캐시를 재사용하며, 변경된 부분에만 집중하여 빌드를 처리할 수 있습니다.


### 첫번째 도커 허브 공유한 앱
https://hub.docker.com/repository/docker/khs7019/khs-first-docker-app/general
