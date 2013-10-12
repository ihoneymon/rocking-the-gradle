rocking-the-gradle
==================

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
