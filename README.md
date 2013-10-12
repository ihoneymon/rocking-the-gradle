rocking-the-gradle
==================

### ● Step00: gradle 설치
* 다운로드: [Gradle 1.8 Download](http://www.gradle.org/downloads) - 09/24 배포
* 압축해제 및 설치
```
unzip gradle-1.8-all.zip
mv gradle-1.8 /usr/local/gradle/
```
* 환경설정
```	
export GRADLE_HOME=/usr/local/gradle/gradle-1.8
export PATH=$PATH:$GRADLE_HOME/bin
```
* 확인
```
gradle -v
```

*****

### ● Step01: 프로젝트 생성setupBuild
* 기본 gradle 프로젝트 생성
```
mkdir rocking-the-gradle
cd rocking-the-gradle
gradle setupBuild
```
> 실행결과 디렉토리 구조
<pre><code>.
├── build.gradle  // 빌드 관련 gradle 스크립트
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
└── settings.gradle	// 서브 프로젝트 설정
2 directories, 6 files
</code></pre>

* 기본 Java Project 생성
```
gradle setupBuild --type java-library
```
> --type java-library 을 주어 생성된 디렉토리의 구조는 다음과 같다.
<pre><code>.
├── build.gradle
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
├── settings.gradle
└── src
    ├── main
    │   └── java
    │       └── Library.java
    └── test
        └── java
            └── LibraryTest.java
7 directories, 8 files
</code></pre>

* 참고: [Build setup types](http://www.gradle.org/docs/current/userguide/build_setup_plugin.html)
> 현재는 3가지 형태만 지원, 추후 더 많은 유형 지원예정

	* <code>java-library</code>
    * <code>pom</code>
    * <code>basic</code>
