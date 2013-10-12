rocking-the-gradle
==================

### ● Step04: 테스트test
* 기본 작성된 테스트 실행
```
./gradlew test
./gradlew test -Dtest.single=LibraryTest
```
* 참고: [Java Command-line options](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html#CBBIJCHG)

* Library.java add 메소드 작성
```
public int add(int first, int second) {
	return first + second;
}
```
* LibraryTest.java add 테스트 코드 작성 및 확인
```
@Test
public void testAddMethod() {
	Library library = new Library();
    assertEquals(library.add(2,3), 5);
}
```
