# SSL 적용

https://certbot.eff.org/instructions?ws=other&os=ubuntufocal 에서 본인에게 맞는 SofWare와 os를 선택합니다.

- software : spring boot가 없어서 other로 선택
- os : Ubuntu 20

![image](https://user-images.githubusercontent.com/42775225/150618704-01990d23-aded-4f71-ae17-b66299f88ab4.png)


__1. WAS 서버에 접속한다.__

__2. snap을 설치 및 최신 버전 확인__

(참고: https://snapcraft.io/docs/snapcraft-overview)

```shell
sudo snap install snapcraft --classic
sudo snap install core; sudo snap refresh core
```

__3. 기존 certbot 설치되어 있다면 삭제__

```shell
sudo apt-get remove certbot
```

__4. certbot 설치__

```
sudo snap install --classic certbot
```

__5. 도메인 - IP 맵핑을 확인한다.__

```
nslookup [도메인 주소]
```


__6. 상황에 맞게 certbot 실행__

일단, 80, 443 도는지 확인

```shell
sudo apt-get install net-tools

sudo netstat -lntp | grep 80
sudo netstat -lntp | grep 443
```

- 웹 서버 중지할 수 있다면 중지를 하고 아래 명령어

```shell
sudo certbot certonly --standalone -d [도메인 주소]
```

- 웹 서버 중지 못할 경우 아래 명령어

```shell
sudo certbot certonly --webroot
```

__7. 내용 입력__

   ```
   email: [본인 이메일]
   이메일 공유 등 Y
   ```

__8. 인증서 확인__

   ```shell
   sudo certbot certificates
   cd [해당 위치]
   ```

__9. spring boot는 JKS 또는 PCKS12 인증서가 필요. (openssl 명령어로 PCKS12 키를 생성)__

```shell
openssl pkcs12 -export -in cert.pem -inkey privkey.pem -out keystore.p12 -name ttp -CAfile chain.pem -caname root
```

__10. application.yml 에 정보 추가__

```yaml
server:
   port: 443
   ssl:
      key-store: ${pemkey.path} # p12key 파일 위치
      key-store-type: PKCS12
      key-store-password: ${ssl.password} #키 생성시 입력한 비밀번호
```

<br />
<br />
<br />


### 트러블 슈팅

1. 만약 80 port 에러가 난다면

![image](https://user-images.githubusercontent.com/42775225/150620601-e78ddc1b-6036-4979-8828-41fbcc4bf2c8.png)

아래 전설의 명령어 주기

```
sudo iptables -I INPUT 5 -i ens3 -p tcp --dport 443 -m state --state NEW,ESTABLISHED -j ACCEPT
```

<br />
<br />
<br />

### 인증서 자동 갱신
```shell
crontab -e

# 아래와 같이 명령어 추가
0 2 1 * * /usr/bin/certbot renew
```
- 참고 URL : https://jiwontip.tistory.com/83

<br />
<br />
<br />

### 인증서 남은일자 확인
```
certbot certificates
```

<br />
<br />
<br />

### Verify return code: 21 (unable to verify the first certificate)) 문제

```
openssl s_client -showcerts -connect {host}:443
```


```
  ssl on;
  ssl_certificate /etc/nginx/hub.prgmr.net/cert.pem;
  ssl_certificate_key /etc/nginx/hub.prgmr.net/privkey.pem;
```
- 여기서, ssl_certificate 를 서버 인증서 파일인 으로 지정했는데, 실제로는 “서버 인증서\n중간 단계인증서k\n중간단계인증서k-1 … \n중간단계인증서0” 처럼 중간단계 인증서를 모두 포함한 파일로 지정해야 한다. Let’s Encrypt 인증서를 쓴다면 이미 해당 파일을 fullchain.pem 이란 이름으로 제공한다. 이 파일을 cert.pem 대신 지정해주면 정상적으로 TLS 연결을 맺는다.
- https://rein.kr/blog/archives/4525


- Intermediate와 Root 인증서들의 Chain 순서가 잘못되었을 때 나타나는 것
- nginx 설정
   - https://serverfault.com/questions/875297/verify-return-code-21-unable-to-verify-the-first-certificate-lets-encrypt-apa
   - https://serverfault.com/questions/1075514/how-to-fix-certificate-chain-with-letsencrypt-certbot
   - nginx full command: https://ittone.ma/ittone/java-ssl-error-unable-to-verify-the-first-certificate-nginx-spring-boot/
   - SSL 인증서 설정 설명: https://engineering.linecorp.com/ko/blog/best-practices-to-secure-your-ssl-tls/
- https://tod2.tistory.com/215


<br />
<br />


# SSL 재발급

1. 서버가 돌고 있는지 확인합니다.

    ```shell
    $ sudo netstat -lntp | grep 443 | awk '{print $7}'
    ```

<br />

2. 서버가 돌고 있으면 종료합니다.

    ```shell
    $ kill -9 [PID넘버]
    ```

<br />

3. 인증서 재발급을 진행합니다. 
    
    - p12 파일 생성 
    
        (만료 날짜가 남았다고 해도 keystore.p12파일 생성날짜를 기준으로 보면된다. 당황X)

    ```shell
    $ certbot renew
    $ cd /etc/letsencrypt/live/[인증서 폴더]
    $ openssl pkcs12 -export -in cert.pem -inkey privkey.pem -out keystore.p12 -name ttp -CAfile chain.pem -caname root
    ```

    - jks 파일 생성

    ```shell
    $ keytool -importkeystore -deststorepass [사용할 비번] -destkeypass [사용할 비번] -destkeystore [생성할 파일명].jks -srckeystore keystore.p12 -srcstoretype PKCS12 -srcstorepass [PKCS12키 암호] -alias [openssl에서 -name 속성에 입력한 이름]
    $ keytool -import -trustcacerts -alias root -file chain.pem -keystore [생성된 파일명].jks
    ```

<br />

4. 문제가 없는지 확인한다.

    (서버를 켜고 테스트를 하세요~~~)

    ```shell
    $ openssl s_client -connect [백엔드서버 IP]:443
    ```

<br />
<br />
<hr />

## Tips) 트러블 슈팅

<br />

1. key를 다시 등록하려고 하는데, 에러를 만나게 되는 경우
    ```
    keytool error: java.lang.Exception: Certificate not imported, alias <root> already exists 에러를 만나면
    ```

    => (해결 방법)
    ```shell
    $ keytool -delete -noprompt -alias "[openssl에서 -name 속성에 입력한 이름]" -keystore "/etc/letsencrypt/live/[인증서 폴더]/[생성된 파일명].jks"
    ```

<br />

2. 서버 재시작하면 꼭 실행해주세요~!

    ```shell
    $ sudo iptables -I INPUT 5 -i ens3 -p tcp --dport 443 -m state --state NEW,ESTABLISHED -j ACCEPT
    $ sudo iptables -I INPUT 5 -i ens3 -p tcp --dport 80 -m state --state NEW,ESTABLISHED -j ACCEPT
    ```