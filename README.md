rocking-the-gradle
==================

### ● Step03: **태스크Task**
* 태스크의 기본적인 문법
```
task myTask
task myTask { configure closure }
task myType << { task action }
task myTask(type: SomeType)
task myTask(type: SomeType) { configure closure }
```

* Library.java 수정
* 태스크 runLibrary 작성
```
// JavaExec라는 Task 클래스를 상속하여 태스크 생성
// http://www.gradle.org/docs/current/dsl/org.gradle.api.tasks.JavaExec.html
task runLibrary(type: JavaExec) {
  main = 'Library'
  classpath += sourceSets.main.runtimeClasspath
  args 'Gradle'
}
```
* 작성된 태스크 확인
```
./gradlew tasks
```

* 태스크 실행
```
./gradlew build
./gradlew runLibrary
```
> build 태스크 실행 후 디렉토리 변화
<pre><code>.
├── build
│   ├── classes
│   │   ├── main
│   │   │   └── Library.class
│   │   └── test
│   │       └── LibraryTest.class
│   ├── dependency-cache
│   ├── libs
│   │   └── gradle.jar
│   ├── reports
│   │   └── tests
│   │       ├── LibraryTest.html
│   │       ├── base-style.css
│   │       ├── css3-pie-1.0beta3.htc
│   │       ├── default-package.html
│   │       ├── index.html
│   │       ├── report.js
│   │       └── style.css
│   ├── test-results
│   │   ├── TEST-LibraryTest.xml
│   │   └── binary
│   │       └── test
│   │           └── results.bin
│   └── tmp
│       └── jar
│           └── MANIFEST.MF
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
20 directories, 21 files
</code></pre>

### 참고사항
* [Gradle: Task](http://www.gradle.org/docs/current/dsl/org.gradle.api.Task.html)
