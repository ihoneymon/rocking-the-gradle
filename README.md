무식하게 배우는 gradle
=====================================
<h3>rocking-the-gradle</h3>

> 본 내용은 손권남(http://kwon37xi.egloos.com/)님이 작성하신 [Gradle tutorial](https://github.com/kwon37xi/gradle-tutorial)을 기초로 하고 있습니다. 

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
    
*****
### ● Step02: 프로젝트 자바 기본설정
> build.gradle 수정

```
/*
 * This build file was auto generated by running the Gradle 'buildSetup' task
 * by 'ihoneymon' at '13. 10. 12 오후 9:26' with Gradle 1.8
 *
 * This generated file contains a sample Java project to get you started.
 * For more details take a look at the Java Quickstart chapter in the Gradle
 * user guide available at http://gradle.org/docs/1.8/userguide/tutorial_java_projects.html
 */

// Apply the java plugin to add support for Java
apply plugin: 'java'

ext {
  javaVersion='1.7'
}

buildDir = 'build' // 기본값

sourceCompatibility = javaVersion
targetCompatibility = javaVersion
[compileJava, compileTestJava]*.options*.encoding = 'UTF-8'

// In this section you declare where to find the dependencies of your project
repositories {
    // Use 'maven central' for resolving your dependencies.
    // You can declare any Maven/Ivy/file repository here.
    mavenCentral()
}

// In this section you declare the dependencies for your production and test code
dependencies {
    // The production code uses the SLF4J logging API at compile time
    compile 'org.slf4j:slf4j-api:1.7.5'

    // Declare the dependency for your favourite test framework you want to use in your tests.
    // TestNG is also supported by the Gradle Test task. Just change the
    // testCompile dependency to testCompile 'org.testng:testng:6.8.1' and add
    // 'test.useTestNG()' to your build script.
    testCompile "junit:junit:4.11"
}
```

*****
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
  main = "Library"
  classpath += sourceSets.main.runtimeClasspath
  args "Gradle"
}
```
* 작성된 태스크 확인
```
./gradlew tasks
```
	* runLibarary 작성전
    > 이미지 대체
    * runLibrary 작성 후
    > 이미지 대체

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
* 참고: [Gradle: Task](http://www.gradle.org/docs/current/dsl/org.gradle.api.Task.html)
	* Task.doFirst()
    * Task.doLast()
* gradle 자바 플러그인 관련Task 분류
	* Build Setup tasks(gradle 공통 태스크)
    * Help tasks(gradle 공통 태스크)
	* Build tasks
    * Documentation tasks
    * Verification Tasks
    * Other tasks

*****
git st

*****
### ● Step05: 이클립스Eclipse 설정
* build.gradle 에 'eclipse' 플러그인 추가
* ```./gradlew tasks``` 를 통해서 IDE tasks에 eclipse 관련항목 추가 확인
* ```./gradlew eclipse``` 태스크 실행
* 이클립스 프로젝트 추가
* 이클립스 외부툴에 프로그램 추가

### 참고문헌
* [The Eclipse Plugin](http://www.gradle.org/docs/current/userguide/eclipse_plugin.html)

*****
### ● Step06: 멀티 프로젝트 전환(도메인 프로젝트+웹 프로젝트)
1. rocking-core 디렉토리 생성 및 파일 이동
```
mkdir rocking-core
mv build.gradle src rocking-core/
```

2. rocking-web 디렉토리 생성 및 gradle 자바 프로젝트 생성
```
mkdir rocking-web
cd rocking-web
gradle setupBuild --type java-library
# 불필요한 파일 제거
rm settings.gradle
rm gradlew*
rm -rf gradle
//web 기본디렉토리
mkdir -p src/main/webapp/WEB-INF
```

3. rocking-web/build.gradle 에 이클립스 설정 추가

    ```
    apply plugin: 'java'
    apply plugin: 'war'
    apply plugin: 'eclipse'
    apply plugin: 'eclipse-wtp'
    
    buildDir = "build"
    
    eclipse {
        classpath {
            downloadSources = true
            defaultOutputDir = file("${buildDir}/classes/main")
        }   
        wtp {
            component {
                contextPath = '/'
            }
            facet {
                facet name: 'jst.web', version: '3.0'
                facet name: 'java', version: '1.7'
            }
        }    
    }
    
    repositories {
        mavenCentral()
    }
    
    dependencies {
        compile 'org.slf4j:slf4j-api:1.7.5'
        testCompile "junit:junit:4.11"
        providedCompile 'javax.servlet:javax.servlet-api:3.0.1'
    }
    ```

4. ./settins.gradle 에 rocking-core, rocking-web 추가
```
rootProject.name = 'rocking-the-gradle'
include ':rocking-core', ':rocking-web'
// include 'rocking-core'
// include 'rocking-web'
```

6. 기존 이클립스 프로젝트 제거
```
rm -rf .classpath .project .settings
```

7. 이클립스 프로젝트 설정
```
./gradlew eclipse
```

8. 이클립스 실행 후 프로젝트 추가

9. 서블릿 생성 후 실행
* RockingTheGradleServlet.java
* home.jsp

*****
* 참조: [Gradle Multiproject](http://www.gradle.org/docs/current/userguide/multi_project_builds.html)

*****
### ● Step07: build.gradle 정리 및 의존성 지정
* ./build.gradle: allprojects vs subprojects

```
allprojects {
    group = 'org.ksug.springcamp.gradle'
    version = '0.0.1-SNAPSHOT'
}

subprojects {    
    apply plugin: 'java'
    apply plugin: 'eclipse'
    apply plugin: 'eclipse-wtp'

    repositories {
        mavenCentral()
    }

    ext {
      javaVersion = '1.7'
    }

    buildDir = 'build' // 기본값

    sourceCompatibility = javaVersion
    targetCompatibility = javaVersion
    [compileJava, compileTestJava]*.options*.encoding = 'UTF-8'
    
    dependencies {
        compile 'org.slf4j:slf4j-api:1.7.5'
        testCompile "junit:junit:4.11"
    }

    tasks.eclipse.dependsOn cleanEclipse
}
```

* ./rocking-core/build.gradle

```
// JavaExec라는 Task 클래스를 상속하여 태스크 생성
// http://www.gradle.org/docs/current/dsl/org.gradle.api.tasks.JavaExec.html
task runLibrary(type: JavaExec) {
  main = "Library"
  classpath += sourceSets.main.runtimeClasspath
  args "Gradle"
}
```

* ./rocking-web/build.gradle

```
apply plugin: 'war'

eclipse {
    classpath {
        downloadSources = true
        defaultOutputDir = file('${buildDir}/classes/main')
    }
    wtp {
        component {
            contextPath = '/'
        }
        facet {
            facet name: 'jst.web', version: '3.0' 
            facet name: 'java', version: '1.7'
        }
    }
}

dependencies {
    compile project(":rocking-core")
    compile springframeworks
    
    providedCompile "javax.servlet:javax.servlet-api:3.0.1"
}
```

*****
### ● Step08: 프로젝트 사용 의존성 라이브러리 지정
* SpringFramework 라이브러리 추가
* Hibernate 라이브러리 추가

    ```
    def springframeworks = [
        "org.springframework:spring-core:3.2.4.RELEASE",
        "org.springframework:spring-context:3.2.4.RELEASE",
        "org.springframework:spring-aop:3.2.4.RELEASE",
        "org.springframework:spring-tx:3.2.4.RELEASE",
        "org.springframework:spring-aspects:3.2.4.RELEASE",
        "org.springframework:spring-jdbc:3.2.4.RELEASE",
        "org.springframework:spring-oxm:3.2.4.RELEASE",
        "org.springframework:spring-orm:3.2.4.RELEASE",
        "org.springframework:spring-test:3.2.4.RELEASE"
    ]
    
    def hibernate = [
        "org.hibernate:hibernate-core:4.2.0.Final",
        "org.hibernate:hibernateged-entitymanager:4.2.0.Final",
        "org.hibernate:hibernate-validator:5.0.1.Final",
        "org.hibernate.javax.persistence:hibernate-jpa-2.1-api:1.0.0.Final"
    ]
    
    dependencies {
        /**
         * SpringFramework: http://www.springsource.org/spring-framework
         */
        //compile springframeworks //도 가능
        springframeworks.collect {
            compile(it) {
                exclude(group: "cglib", module: "cglib")
            }
        }
        
        /**
         * Hibernate: http://www.hibernate.org/
         * cglib 모듈 제외
         */
        hibernate.collect {
            compile(it) {
                exclude(group: "cglib", module: "cglib")
            }
        }
        
        /**
         * SLF4j & Logback
         * SLF4j: http://www.slf4j.org/
         * Logback: http://logback.qos.ch/
         */
        compile "org.slf4j:slf4j-api:1.6.6"
        runtime "org.slf4j:jcl-over-slf4j:1.6.6"
        runtime "org.slf4j:log4j-over-slf4j:1.6.6"
        compile "ch.qos.logback:logback-classic:1.0.13"
    
        /**
         * Google Guava(Collection Utils): http://code.google.com/p/guava-libraries/
         */
        compile "com.google.guava:guava:14.0.1"
        
        /**
         * Logback을 사용하기 때문에 모든 의존성 라이브러리에서 common-logging는 제외
         */
        [configurations.runtime, configurations.default]*.exclude(module: 'commons-logging')
    }
    ```

* ./rocking-web/build.gradle

    ```
    apply plugin: 'war'
    
    eclipse {
        classpath {
            downloadSources = true
            defaultOutputDir = file("${buildDir}/classes/main")
        }
        wtp {
            component {
                contextPath = "/"
            }
            facet {
                facet name: 'jst.web', version: '3.0' // Servlet Spec Version 지정
                facet name: 'jst.java', version: '1.7' // Java Version 지정
            }
        }
    }
    
    def springframeworks = [    
        "org.springframework:spring-web:3.2.4.RELEASE",
        "org.springframework:spring-webmvc:3.2.4.RELEASE"
    ]
    
    dependencies {
        compile project(":rocking-core") 
        compile springframeworks      
        providedCompile "javax.servlet:javax.servlet-api:3.0.1"
        /**
         * Logback을 사용하기 때문에 모든 의존성 라이브러리에서 common-logging는 제외
         */
        [configurations.runtime, configurations.default]*.exclude(module: 'commons-logging')
    }
    ```

* 최종 디렉토리 구조

    ```
    .
    ├── README.md
    ├── bin
    │   ├── Library.class
    │   └── LibraryTest.class
    ├── build.gradle
    ├── gradle
    │   └── wrapper
    │       ├── gradle-wrapper.jar
    │       └── gradle-wrapper.properties
    ├── gradlew
    ├── gradlew.bat
    ├── rocking-core
    │   ├── bin
    │   │   ├── Library.class
    │   │   └── LibraryTest.class
    │   ├── build.gradle
    │   └── src
    │       ├── main
    │       │   └── java
    │       │       └── Library.java
    │       └── test
    │           └── java
    │               └── LibraryTest.java
    ├── rocking-web
    │   ├── build
    │   │   └── classes
    │   │       └── main
    │   │           ├── Library.class
    │   │           ├── LibraryTest.class
    │   │           └── org
    │   │               └── ksug
    │   │                   └── springcamp
    │   │                       └── gradle
    │   │                           └── RockingTheGradleServlet.class
    │   ├── build.gradle
    │   ├── settings.gradle
    │   └── src
    │       ├── main
    │       │   ├── java
    │       │   │   ├── Library.java
    │       │   │   └── org
    │       │   │       └── ksug
    │       │   │           └── springcamp
    │       │   │               └── gradle
    │       │   │                   └── RockingTheGradleServlet.java
    │       │   └── webapp
    │       │       └── WEB-INF
    │       │           └── views
    │       │               └── home.jsp
    │       └── test
    │           └── java
    │               └── LibraryTest.java
    └── settings.gradle
    
    30 directories, 23 files
    ```

