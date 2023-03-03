# 자바 파일 입출력
- 참고 : https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=eunjin6132&logNo=220888811967


## FileOutputSTream / 생성자
- 데이터파일을 바이트 스트림으로 저장하기 위해 사용

### FileOutputStream(File file)
- File을 쓰기 위한 객체를 생성
- 주어진 File 객체가 가리키는 파일을 생성함
- 기존 파일 존재시 내용ㅇ르 지우고 새로운 파일을 생성함

### FileOutputStream(String fileName)
- 주어진 이름의 파일을 쓰기 위한 객체를 생성

### FileOutputStream(String fileName, boolean apend)
- 주어진 append 값에 따라 새로운 파일을 생성하거나 기존의 내용에 추가


# 압축 파일 (zip)
- 참고 (클래스) : 
- 참고 (예제) : https://madplay.github.io/post/java-file-zip

## ZipOutputStream

## ZipEntry

- ZipEntry e = new ZipEntry("folderName/mytext.txt");
- 이런식으로 하위 디렉토리도 지정할 수 있음
