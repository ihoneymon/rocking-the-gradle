rocking-the-gradle
==================

### ● Step07: build.gradle 정리 및 프로젝트 의존성 지정
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
