# SOLID

객체 지향 프로그래밍 5원칙의 앞글자를 따서 만들어진 단어

[SRP](#single-resposibility-principlesrp)
[OCP](#open-closed-principleocp)
[LSP](#liskov-substitution-princlplelsp)
[ISP](#interface-segregation-principleisp)
[DIP](#dependency-inversion-principledip)

## Single Resposibility Principle(SRP)

하나의 모듈이 하나의 책임을 가져야 한다는 뜻은 모듈이 변경되는 이유가 한가지여야 한다, 는 뜻으로 받아들이는 것이 좋다.  
변경의 이유가 한가지다 -> 해당 모듈이 여러 대상에 대해 책임을 가지면 안되고, 오직 하나의 대상에 대해서만 책임을 져야 한다.  
만약 여러 대상에 대한 책임을 진다면 수정해야하는 이유가 여러개가 될 수 있음

예를 들어 사용자의 입력 정보를 받고, 비밀번호를 암호화하여 데이터베이스에 저장하는 로직이 있다고 하자.

```java
@Service
@RequiredArgsConstructor
public class UserService {

    private final UserRepository userRepository;

    public void addUser(final String email, final String pw) {
        final StringBuilder sb = new StringBuilder();

        for(byte b : pw.getBytes(StandardCharsets.UTF_8)) {
            sb.append(Integer.toString((b & 0xff) + 0x100, 16).substring(1));
        }

        final String encryptedPassword = sb.toString();
        final User user = User.builder()
            .email(email)
            .pw(encryptedPassword).build();

        userRepository.save(user);
    }
}
출처: https://mangkyu.tistory.com/194 [MangKyu's Diary:티스토리]
```

<!-- 근데 내용이 너무 어려워서 지금 못하겠다 나중에 하자 -->

## Open-Closed Principle(OCP)

## Liskov Substitution Princlple(LSP)

## Interface Segregation Principle(ISP)

## Dependency Inversion Principle(DIP)

### 출처

[망나니개발자](https://mangkyu.tistory.com/194)
