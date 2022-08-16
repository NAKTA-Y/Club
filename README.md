# Club
코드로 배우는 스프링 부트 웹 프로젝트 Part 5 실습

</br></br></br>

# 학습 내용 캡처 사진
### 1. Spring Security 기본 로그인 폼 </br>
* Spring Security를 넣고 localhost로 접속하면 기본적으로 제공되는 로그인 폼이 생성된다.
![image](https://user-images.githubusercontent.com/74942574/184796206-533d0752-71f7-4cdb-bcaf-12b5b3c24c89.png)
</br></br></br>
* Spring을 실행했을 때 생성되는 security password로 로그인 할 수 있다.
![image](https://user-images.githubusercontent.com/74942574/184796100-040118f3-569a-4298-a6c1-3b1e920b57e2.png)
</br></br></br>
* 실행 했을 때 여러 개의 필터들이 동작되는 것을 확인할 수 있다.
![image](https://user-images.githubusercontent.com/74942574/184796588-dacfcb18-0835-47ad-8522-369272dba482.png)
</br></br></br>
### 2. Spring Security 커스터마이징 </br>
* 다음과 같이 DefaultFilterChainProxy를 대체하여 유저가 만든 CustomFilterChainProxy를 만들 수 있다.
```
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public UserDetailsService users() {
        UserDetails user = User.builder()
                .username("user")
                .password("{noop}password")
                .roles("USER")
                .build();

        UserDetails admin = User.builder()
                .username("admin")
                .password("{noop}password")
                .roles("USER", "ADMIN")
                .build();

        return new InMemoryUserDetailsManager(user, admin);
    }

    @Bean
    protected SecurityFilterChain filterChain(HttpSecurity http) throws Exception{
        http
                .authorizeRequests()
                .antMatchers("/sample/member").hasRole("USER")
                .antMatchers("/sample/admin").hasRole("ADMIN")
                .anyRequest().permitAll()
                    .and()
                .formLogin()
                    .and()
                .logout()
                    .and()
                .csrf().disable();

        return http.build();
    }
}
```
</br></br></br>

### 3. OAuth2를 이용한 로그인 방식 </br>
* 다음과 같이 구글 OAuth2 클라이언트 ID를 생성하여 API를 사용할 수 있도록 한다.
![image](https://user-images.githubusercontent.com/74942574/184797903-b6b4cae5-468d-4cd1-ad5c-9dc9d2296722.png)
</br></br></br>
* OAuth 클라이언트 ID를 생성하고 프로젝트에 추가하면 다음과 같이 구글로 로그인 할 수 있는 화면을 생성할 수 있다.
![image](https://user-images.githubusercontent.com/74942574/184798117-726ac2a4-b095-4bd5-a333-bed67e117df2.png)
![image](https://user-images.githubusercontent.com/74942574/184798287-4b5ac4a0-f264-4c78-bc28-05cf3038ea3f.png)
</br></br></br>
* 실제로 구글로 로그인 하여 데이터베이스를 확인해보면 데이터가 잘 들어온 것을 확인할 수 있다.
![image](https://user-images.githubusercontent.com/74942574/184798410-f61c4cb4-61fd-4aab-bd15-09238076b632.png)

</br></br></br>

### 4. API 서비스 만들기 </br>
* POST 방식으로 JSON 데이터를 전달하면 데이터 베이스에 추가된 것을 확인할 수 있다.
* 이때 서버 응답으로 받은 데이터는 데이터베이스에 등록된 Note Number이다.
![image](https://user-images.githubusercontent.com/74942574/184798913-1ab53533-3d66-4fb8-aaf3-6b672a2cc60d.png)
</br></br></br>
* GET 방식으로 특정 번호의 Note를 확인할 수 있다.
![image](https://user-images.githubusercontent.com/74942574/184799133-27c6760f-4ab2-43d0-bb8c-b37c6a2e07b0.png)
</br></br></br>
* GET 방식으로 특정 이메일을 파라미터로 전달하면 특정 회원의 모든 Note를 확인할 수 있다.
![image](https://user-images.githubusercontent.com/74942574/184799267-ceee0975-8a78-40b9-b4dd-1c69728f1688.png)
</br></br></br>
* DELETE 방식으로 특정 번호의 Note를 삭제할 수 있다.
![image](https://user-images.githubusercontent.com/74942574/184799369-d69f5eb9-c2b1-4469-901e-15a5c223f290.png)
</br></br></br>
* PUT 방식으로 특정 번호의 Note를 수정할 수 있다.
![image](https://user-images.githubusercontent.com/74942574/184799489-b6b9ed29-2e35-4642-97f4-78aa1477ab17.png)

</br></br></br>

### 5. JWT를 이용한 토큰 생성/검증 처리 </br>
* 테스트를 위하여 GET 방식으로 JWT를 발행할 수 있게 만들었다.
![image](https://user-images.githubusercontent.com/74942574/184799832-619ed3a8-e07c-4d53-84ee-4f37ffeb6e27.png)
</br></br></br>
* Authorization 헤더 메시지를 통해서 JWT를 확인하여 API 서비스를 이용할 수 있도록 할 수 있다.
* Authorization 헤더 메시지를 생략하면 다음과 같이 403 코드의 "FAIL CHECK API TOKEN"을 반환한다.
![image](https://user-images.githubusercontent.com/74942574/184800051-4837d689-d75d-4857-8332-7bfe65269023.png)
</br></br></br>
* Authorization 헤더 메시지를 추가하여 API 서비스를 호출하면 데이터를 잘 반환하는 것을 확인할 수 있다.
![image](https://user-images.githubusercontent.com/74942574/184800236-ed1ddb4b-9b32-4501-bf56-39075bdf1817.png)
![image](https://user-images.githubusercontent.com/74942574/184800299-e19768e6-9432-4c9f-a877-5a798ef9dc00.png)

</br></br></br>

# 엔티티 관계도
![image](https://user-images.githubusercontent.com/74942574/184800420-e48b9940-1d61-48d0-98f6-7c656c4edb59.png)
* club_member_role_set == N : 1 == club_member
* club_member == 1 : N == note

</br></br></br>

# Git Model
* main
  * 최종 릴리즈 브랜치
* develop
  * 개발 브랜치
  * 기능 구현을 테스트
* feature
  * 기능 구현 브랜치
  
</br></br>

## Git Flow 캡처 화면
![image](https://user-images.githubusercontent.com/74942574/184800714-8943c0a6-4ab0-448b-a339-1a7d5ce4bf11.png)
