# SSL 이슈

### 이슈 내용

프론트에서 API 호출 시에 500에러가 발생했습니다.

```
failed, reason: unable to verify the first certificate
```

이는 SSL인증서의 문제로 확인되었습니다.
OpenSSL의 명령어를 통해 인증서를 확인해본 결과 다음과 같이 동일한 에러 메세지를 확인할 수 있었습니다.

```shell
openssl s_client -connect [백엔드서버 IP]:443
```

![image](https://user-images.githubusercontent.com/42775225/173228618-9b4cca5d-e9b1-4a72-90a2-f83e1ae4a32f.png)


제대로 된 인증서라면 중간 레벨에 있는 인증서까지 모두 있어야 하는데 그것이 없어서 발생하는 이슈였다.

<br />
<hr />


### 해결

spring boot는 JKS 또는 PCKS12 인증서를 사용하기 때문에 
JKS파일로 다음과 같이 재생성 하였습니다. 

```shell
keytool -importkeystore -deststorepass [사용할 비번] -destkeypass [사용할 비번] -destkeystore [생성할 파일명].jks -srckeystore keystore.p12 -srcstoretype PKCS12 -srcstorepass [PKCS12키 암호] -alias [openssl에서 -name 속성에 입력한 이름]
```

```shell
keytool -import -trustcacerts -alias root -file chain.pem -keystore [생성된 파일명].jks
```

이렇게 다시 생성한 인증서로 재배포를 했고, 정상 해결되었습니다.

![image](https://user-images.githubusercontent.com/42775225/173228630-07d4d1c6-374f-4544-8dc7-2018638ccf51.png)

