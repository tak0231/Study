## API 구현 
- API는 프론트 클라이언트가 다양(IOS,안드로이드,Vue,React 등
- 클라이언트 - 서버 간 통신 -> API로만 제공되는 경우가 많음
- 때문에 일반적으로 API로 요청을 받아서 데이터(JSON 등)으로 response를 보냄

- 이 때문에 기존에 Controller에서 form데이터를 받아서 처리하는 것과 조금 방식이 다름


## Entity
- 절대 Response / Request를 직접 받거나 처리하지 않음
- 이럴 경우 Presentation을 위한 로직이 Entity에 추가됨
- 또한, API 검증을 위한 로직이 Entity에 추가되어 버림(@NotEmpty)
- 또한, 기능별로 API가 다양하게 만들어지는데 -> 위와 같은 경우에는 한 Entity에서 모든 요청이 들어옴
- 또한, Entity가 변경되면 API 스펙이 변하는 문제가 발생함 

- 이를 방지하기 위해서 직접 Entity에 Response / Request를 받지 않음
- 대신 API 요청 스펙에 맞춘 별도의 DTO를 생성, 파라미터로 받거나 return 시킴


ex)
```
@Service
@RequiredArgsConstructor
public class BoardService {
    private final BoardRepository boardRepository;

    /*
        게시글 생성
    */
    @Transactional
    public Long save(final BoardRequestDto params){
        Board entity = boardRepository.save(params.toEntity());
        return entity.getId();
    }


    /*
        게시글 수정
    */
    @Transactional
    public Long update(final Long id, final BoardRequestDto params) {
        Board entity = boardRepository.findById(id).orElseThrow(() -> new CustomException(ErrorCode.POSTS_NOT_FOUND));
        entity.update(params.getTitle(), params.getContent(), params.getWriter());
        return id;
    }

    /*
        게시글 삭제
     */
    @Transactional
    public Long delete(final Long id) {
        Board entity = boardRepository.findById(id).orElseThrow(() -> new CustomException(ErrorCode.POSTS_NOT_FOUND));
        entity.delete();
        return id;
    }

    /*
       게시글 리스트 조회
   */
    public List<BoardResponseDto> findAll(){
        // DESC -> 내림차순
        Sort sort = Sort.by(Sort.Direction.DESC,"id","createDate");
        List<Board> list = boardRepository.findAll(sort);
        return list.stream().map(BoardResponseDto::new).collect(Collectors.toList());
    }

   

    public List<BoardResponseDto> findAllByDeleteYn(final char deleteYn){
        Sort sort = Sort.by(Sort.Direction.DESC,"id","createdDate");
        List<Board> list = boardRepository.findAllByDeleteYn(deleteYn,sort);
        return list.stream().map(BoardResponseDto::new).collect(Collectors.toList());
    }

    /*
        게시글 상세정보 조회
    */
    @Transactional
    public BoardResponseDto findById(final Long id){
        Board entity = boardRepository.findById(id).orElseThrow(()-> new CustomException(ErrorCode.POSTS_NOT_FOUND));
        entity.increaseHits();
        return new BoardResponseDto(entity);
    }
}
```

## DTO
- DTO 사용함으로써 Entity에서 필요한 값들만 서로 주고받을 수 있게됨

  
### RequestDTO
=> DTO에서 Request를 받음
- 주로 클라이언트 -> 서버 (맞나?)
- 요청받은 내용을 Entity에 반영(전달)하는 역할


ex) RequestDTO 예시
```
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class BoardRequestDto {
              ...
}  
```

> @NoArgsConstructor(access = AccessLevel.PROTECTED) 사용 이유
> - 요약 : 객체 생성시 안전성을 보장할 수 있음
> - 무분별한 객체 생성에 대해 한번 더 체크할 수 있는 수단이 되기 때문
> - IDE 단계에서 이런 누락을 방지시킬 수 있음
> 
> - 또한, JPA에서는 프록시 생성을 위해서 기본생성자를 반드시 하나 생성해야함
> - 그런데 굳이 외부에서 생성을 열어둘 필요가 없음
> - 객체 생성시의 안전성을 떨어트리기 때문
> 
> - 참고 : https://cobbybb.tistory.com/14
> - 참고 : https://www.popit.kr/%EC%8B%A4%EB%AC%B4%EC%97%90%EC%84%9C-lombok-%EC%82%AC%EC%9A%A9%EB%B2%95/

  
ex) toEntity() 예시
```
public Board toEntity(){
      return Board.builder()
              .title(title)
              .content(content)
              .writer(writer)
              .hits(0)
              .deleteYn(deleteYn)
              .build();
}
```
- toEntity() 메서드를 만드는 편 (Entity에 전달하는 역할)
- RequestDto에서 Entity 객체를 만들어서 Return 시킴


### ResponseDTO
=> DTO에서 Response로 반환함
- 주로 서버 -> 클라이언트 (맞나?)
- 

Controller서 호출 -> Service -> DTO -> Repository -> Entity -> 검색 결과를 반환 해서 다시 최종적으로 Service -> Controller(request에 전달)


### @Tranactional
- 서비스 클래스에서 필수적으로 사용되어야하는 어노테이션
- 일반적으로 메서드 레벨에서 선언함
- 메서드의 실행, 종료, 예외를 기준으로 각각 실행(begin), 종료(commit), 예외(rollback)을 자동으로 처리함함
- Trasctional이 선언된 메서드는 메서드의 로직이 정상적으로 종료되었을 떄 실행된 SQL 쿼리에 대한 Commit을 수행하고, 해당 메서드에 대한 트랜잭션을 종료함


### Stream API
- Java는 객체지향 언어 -> 함수형 불가
- Java8부터 지원하는 Stream API, 람다식 덕에 함수형도 가능해짐

- Stream api -> 데이터 추상화, 처리되는데 자주 사용되는 함수들 존재

<특징>
- 원본의 데이터를 변경하지 않음
- 일회용임
- 내부 반복으로 작업을 처리함함

<br/>

### 참고
- (API) : https://velog.io/@k_ms1998/API-BasicsPart-1-Request-Response%EB%A5%BC-%EC%97%94%ED%8B%B0%ED%8B%B0%EB%A1%9C-%EC%A7%81%EC%A0%91-%EC%82%AC%EC%9A%A9
- 스프링 부트와 AWS로 혼자 구현하는 웹 서비스
- (프록시) : https://erjuer.tistory.com/105
