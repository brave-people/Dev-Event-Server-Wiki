## 1. 콘솔 바로 접속하기

<table> 	
<tr>    
    <td>Index</td> 	    
    <td>Console URL</td> 
</tr>	
<tr>	    
    <td>1</td> 	    
    <td><a href="https://console.cloud.google.com/apis/credentials" target="_blank">구글 콘솔</a></td> 	
</tr>
<tr>	    
    <td>2</td> 	    
    <td><a href="https://developers.kakao.com/console/app" target="_blank">카카오 콘솔</a></td> 	
</tr>
<tr>	    
    <td>3</td> 	    
    <td><a href="https://developers.naver.com/apps/#/list" target="_blank">네이버 콘솔</a></td> 	
</tr>
<tr>	    
    <td>4</td> 	    
    <td><a href="https://github.com/settings/developers" target="_blank">깃헙 콘솔</a></td> 	
</tr>
</table>

<br />
<br />

## 2. 사용법

#### 1) 구글

> 1. 새 프로젝트 생성

![image](https://user-images.githubusercontent.com/42775225/173222901-727ed23e-ef83-4ce9-9772-05f28e27900d.png)

![image](https://user-images.githubusercontent.com/42775225/173222905-ecc12f10-8c3e-430d-a0c1-f9e7853ade5d.png)

<br />
<br />

> 2. OAuth 동의 화면

![image](https://user-images.githubusercontent.com/42775225/173222963-81712150-f7cf-4850-bee6-a140dc16587c.png)

![image](https://user-images.githubusercontent.com/42775225/173222984-1e194c29-1cf5-40a1-be54-4489ce1f8b8d.png)

![image](https://user-images.githubusercontent.com/42775225/173222988-df8cc40b-cf6e-4b4d-9d06-d601a34c2a96.png)


<br />
<br />

> 3. 사용자 인증 정보 만들기 -> OAuth 클라이언트 생성

![image](https://user-images.githubusercontent.com/42775225/173222913-cb05fb7b-9e2d-4b8d-b1bf-5866e692b4e9.png)

![image](https://user-images.githubusercontent.com/42775225/173223065-d92f2ab6-e398-4446-812b-77f46477fa3b.png)

![image](https://user-images.githubusercontent.com/42775225/173223069-31a0ac0b-6556-4b16-bd64-f61c4fb7414e.png)


<br />
<br />

> 4. 클라이언트 ID, 클라이언트 보안 비밀번호 입력

**application.yml**
```yaml
spring:
  security:
    oauth2:
      redirect-uri: https://my-app-domain.com/login/oauth2/code/google
      client:
        registration:
          google:
            client-id: ${google.client.id}
            client-secret: ${google.client.secret}
            scope: profile, email
```

**build.gradle**
```
    implementation 'org.springframework.boot:spring-boot-starter-oauth2-client'
    implementation 'org.springframework.boot:spring-boot-starter-oauth2-resource-server'
```

**SecurityConfig.java 파일도 필요에 맞게 수정해주어야 합니다.**


<br />

#### 2) 카카오

: 

<br />

#### 3) 네이버

: 

<br />

#### 4) 깃헙

: 

<br />