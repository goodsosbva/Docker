
### [3 layer 컨테이너간 통신 구현]
- mongoDB, backend, frontend
- 가장 기본적인 방법으로 도커화해서 컨테이너간 통신 구현 완료

#### mongoDB
  - docker run --name mongodn --rm -d -p 27017:27017 mongo

#### node.js 앱


```Dockerfile
FROM node

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

CMD ["node", "app.js"]
```


  - docker build -t goals-note .
  - docker run --name goals-backend --rm -d -p 80:80 goals-note

#### react 앱


```Dockefile

FROM node:16

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]

```
  - docker build -t goals-react .
  - docker run --name goals-frontend --rm -p 3000:3000 -it goals-react

##### 구현 하면서 생각해볼만한 점.
- node 버전이 17이상으로 들어오면 OpenSSL 암호화 변경으로 인해 이전에 쓰던 코드를 쓰려면 설정이 필요 했었음.
1. Node.js 버전 변경
Node.js 16 버전으로 다운그레이드하면 문제가 해결될 수 있습니다. Node.js 16은 OpenSSL 3.0을 사용하지 않으므로 이 에러가 발생하지 않음
-> 학습 목적이기 때문이 이번에는 이 방법 채택 (docker 학습이 우선이므로)

3. OpenSSL legacy 모드 활성화
만약 Node.js 17 이상 버전을 유지하고 싶다면, OpenSSL의 legacy 모드를 활성화할 수 있습니다. 이를 위해서는 환경 변수를 설정해서 바꿀 수도 있음.

```bash
export NODE_OPTIONS=--openssl-legacy-provider
```
```bash
set NODE_OPTIONS=--openssl-legacy-provider
```
- react 앱의 경우 -it (interactive, tty) 설정을 안하면 fe앱에 관심이 없다고 판단해서 컨테이너가 자동으로 지워진다고 함. 기억할 필요가 있는 부분임.
- 각각 Expose, listen 한 포트에 맞춰서 docker run 할 때 포트 설정을 해야 올바르게 컨테이너간 통신이 가능함.
