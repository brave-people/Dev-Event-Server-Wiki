# Heroku

## 로그 
```
heroku logs -a dev-event-server-monitoring
```
- 어플리케이션 이름을 지정해야 로그를 볼 수 있다. 


## 해로쿠 H10 에러

```
java.runtime.version=17
```
- system.properties에 JDK 버전 명시
- 명시 하지 않으면 기본 1.8 사용함


<br />
<br />


```
bootJar {
	archivesBaseName='monitoring'
	archiveFileName='monitoring.jar'
}
```
- application.yml에 jar 이름 변경

<br />
<br />


```
./gradlew build or intelij에서 build 수행
```

<br />
<br />

```
web: java -Dserver.port=$PORT $JAVA_OPTS -jar build/libs/monitoring.jar
```
- Procfile 생성
- jar 경로를 명시해줘야함 


<br />
<br />

- 참고
    - https://jungguji.github.io/2021/01/28/Spring-boot-Heroku%EC%97%90-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0/
    - https://somuchthings.tistory.com/104

