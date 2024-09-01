## 첫번째 도커 이미지 생성 실습

1. Dockearfile 안에 내가 이미지로 만들 때 필요한 명령어를 기입
2. 각각의 명령어에 대한 설명은 다음과 주석으로 기입.

```Dockerfile
# 공식 Node.js 14 이미지를 사용합니다.
FROM node:14

# 작업 디렉토리를 /app으로 설정합니다.
WORKDIR /app

# package.json 파일을 컨테이너의 현재 작업 디렉토리로 복사합니다.
COPY package.json .

# package.json에 명시된 의존성을 설치합니다.
RUN npm install

# 현재 디렉토리의 모든 파일을 컨테이너의 작업 디렉토리(/app)로 복사합니다.
COPY . .

# 컨테이너가 외부에서 접근할 수 있도록 3000번 포트를 엽니다.
EXPOSE 3000

# 컨테이너가 시작될 때 app.mjs 파일을 Node.js로 실행합니다.
CMD ["node", "app.mjs"]
```

3. docker build - Dockerfile 기반으로 이미지를 빌드 -> 빌드 되면 이미지 id를 리턴 해줌.
4. docker run -p 3000:3000 [img id] -> 첫번째 3000: 호스트머신에서 사용할 포트, 두번째 3000: 컨테이너 내부에서 사용할 포트
