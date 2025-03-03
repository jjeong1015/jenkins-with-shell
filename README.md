# 🚀 Spring Boot 자동 배포 스크립트
이 스크립트는 **Jenkins 기반 CI/CD 자동 배포**를 위한 배포 자동화 스크립트입니다.  
Spring Boot 애플리케이션을 **중단 없이 업데이트**하고, **로그를 관리**하며, **기존 버전을 백업**하는 기능을 수행합니다.

<br>

## 📌 이 스크립트는 언제, 왜 사용될까?
- Jenkins와 연동하여 자동 배포할 때
Jenkins의 Post Build 단계에서 실행하면 CI/CD 자동화 가능<br>
ex) 새로운 코드가 Git에 푸시되면 빌드 → 테스트 → 배포가 자동으로 수행됨

- 수동 배포를 편리하게 하고 싶을 때
매번 수동으로 JAR 파일을 복사하고 실행할 필요 없이 스크립트 한 줄로 배포 가능<br>
sh deploy.sh 한 번 실행하면 자동으로 배포됨

- 롤백이 필요한 상황을 대비할 때
기존 버전을 .bak로 백업하여, 문제가 생기면 이전 버전으로 복구 가능

<br>

## 🛠 스크립트 설명
```bash
#!/bin/bash

# 변수 설정
JAR_FILE="SpringApp-0.0.1-SNAPSHOT.jar" # 배포할 Spring Boot 애플리케이션 JAR 파일 이름
DEPLOY_DIR="/home/username/step07cicd" # 애플리케이션을 배포할 디렉터리

# 이전 JAR 파일 백업
if [ -f "$DEPLOY_DIR/$JAR_FILE" ]; then 
  mv "$DEPLOY_DIR/$JAR_FILE" "$DEPLOY_DIR/$JAR_FILE.bak" # 기존에 실행 중이던 JAR 파일이 있다면 .bak 확장자로 변경하여 백업
fi

# 새로운 JAR 파일 복사
cp $JAR_FILE $DEPLOY_DIR/$JAR_FILE 

# 기존 8899 포트 사용 중인 프로세스 종료
if sudo lsof -i :8899 > /dev/null; then
  sudo kill -9 $(sudo lsof -t -i:8899)
fi

# 백그라운드에서 새로 실행
nohup java -jar $DEPLOY_DIR/$JAR_FILE > $DEPLOY_DIR/app.log 2>&1 &

echo "배포완료 및 실행됩니다."
```

<br>

## ✅ 결론
이 스크립트는 Spring Boot 애플리케이션을 자동으로 배포 및 실행하는 CI/CD 자동화 스크립트입니다.
Jenkins와 연동하여 무중단 배포가 가능하며, 백업 및 로그 관리 기능도 포함되어 있어 효율적인 운영이 가능합니다. 🚀
