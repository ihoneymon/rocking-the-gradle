rocking-the-gradle
==================

### ● Step08: 프로젝트에서 사용할 의존성 라이브러리 지정
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
         * Google Guava(Collecton Utils): http://code.google.com/p/guava-libraries/
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

