# maven_study
spring_maven_build
Maven을 이용한 Spring 실습
이 글은 http://spring.io/guides/gs/maven/를 번역한 것입니다.
Maven 설치
http://maven.apache.org/download.cgi 링크에서 바이너리 파일을 다운로드한다.
적당한 곳에 압축을 푼다.
Maven이 설치된 전체 경로를 값으로 환경 변수 MAVEN_HOME을 만든다.
MAVEN_HOME이 의미하는 디렉터리에는 bin 서브 디렉터리가 있어야 한다.
JDK가 설치된 전체 경로를 값으로 가지는 JAVA_HOME 환경 변수가 있는지 확인하고 없으면 만든다.
JAVA_HOME 환경 변수 설정은 Java 설치를 참고한다.
Path 환경 변수에 %MAVEN_HOME%\bin 경로를 추가한다.
명령 프롬프트를 새로 열고 다음 명령으로 메이븐 버전을 확인한다.
mvn -v
메이븐 프로젝트 생성
아래 예제를 따라 하면서 기본적인 메이븐 사용법을 익힌다.
메이븐의 기본 구조로 디렉터리를 만든다.
프로젝트 루트 디렉터리를 생성한다.
C:\maven\HelloWord를 프로젝트 루트 디렉터리라면, HelloWorld에는 아래처럼 서브 디렉터리를 만든다.
HelloWorld (프로젝트 루트)
   └── src
        └── main
             └── java
                  └── hello
HelloWorld/src/main/java/hello/HelloWorld.java
package hello;

public class HelloWorld {
    public static void main(String[] args) {
        Greeter greeter = new Greeter();
        System.out.println(greeter.sayHello());
    }
}
HelloWorld/src/main/java/hello/Greeter.java
package hello;

public class Greeter {
    public String sayHello() {
        return "Hello world!";
    }
}
프로젝트 루트에 pom.xml 파일을 작성한다.
pom.xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                             http://maven.apache.org/maven-v4_0_0.xsd">
                       
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.springframework.gs</groupId>
    <artifactId>gs-maven-initial</artifactId>
    <version>0.1.0</version>
    <packaging>jar</packaging>
    
</project>
<modelVersion> - POM model version으로 언제나 4.0.0
<groupId> - 이 프로젝트의 소유자인 조직이나 기관명으로 대부분 역순 도메인명(예를 들면 net.java_school)
<artifactId> - jar 나 war 파일에 붙여지는 이름
<version> - 빌드 되는 프로젝트 버전 번호
<packaging> - 프로젝트를 어떻게 패키지 할 것인가에 대한 설정. 디폴트는 jar, 웹 애플리케이션은 war

빌드
mvn compile
pom.xml 파일이 있는 프로젝트 루트에서 mvn compile 을 실행한다.
이 명령은 메이븐을 실행하고 컴파일 goal 을 실행한다.
작업이 성공했다면 컴파일된 파일을 target/classes 폴더에서 찾을 수 있다.
이 폴더는 컴파일이 수행되면서 만들어진다.
mvn package
package goal 은 자바 코드를 컴파일하고 테스트를 수행하고 패키지로 묶어서 target 디렉터리에 복사한다.
패키지 된 파일의 이름은 pom.xml에서 설정한 대로 gs-maven-initial-0.1.0.jar란 이름으로 만들어진다.
mvn install
만약에 이 프로젝트의 jar 파일을 로컬 저장소에 설치하기를 원한다면
install goal을 수행해야 한다.
install goal은 컴파일하고 테스트한 후 패키지로 만든 파일을 로컬 저장소에 복사한다.
mvn clean
clean goal 은 빌드를 통해 생성된 모든 산출물을 삭제한다.
mvn clean compile
중복해서 goal를 수행할 수 있다.
의존 라이브러리 설정을 pom.xml에 추가
대부분의 애플리케이션이 외부 라이브러리에 의존한다.
우리의 예제도 외부 라이브러리에 의존하도록 바꾸겠다.

HelloWorld.java를 아래처럼 변경한다.
HelloWorld/src/main/java/hello/HelloWorld.java
package hello;

import org.joda.time.LocalTime;

public class HelloWorld {
  public static void main(String[] args) {
    LocalTime currentTime = new LocalTime();
    System.out.println("The current local time is: " + currentTime);

    Greeter greeter = new Greeter();
    System.out.println(greeter.sayHello());
  }
}
이런 다음 바로 mvn compile 하면 빌드가 실패한다.
왜냐하면 pom.xml에 의존 라이브러리 설정을 추가하지 않았기 때문이다.
다음을 project 엘리먼트 안에 붙여 넣는다.
<dependencies>
    <dependency>
        <groupId>joda-time</groupId>
        <artifactId>joda-time</artifactId>
        <version>2.2</version>
    </dependency>
</dependencies>
위에서는 빠져있지만 dependency 의 하위 엘리먼트로 scope 엘리먼트가 있다.
scope 엘리먼트의 디폴트 값은 compile이다.
그 외에 중요한 스코프는 아래와 같다.

provided - 컴파일할 때 필요하지만 런타임에는 컨테이너에 의해 제공되는 것.(Servlet API)
test - 컴파일하고 테스트에는 필요하지만 빌드(컴파일과 배포) 하거나 실행할 때는 필요 없는 것.

pom.xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                             http://maven.apache.org/maven-v4_0_0.xsd">

    <modelVersion>4.0.0</modelVersion>
    <groupId>org.springframework</groupId>
    <artifactId>gs-maven</artifactId>
    <packaging>jar</packaging>
    <version>0.1.0</version>
    
    <dependencies>
        <dependency>
            <groupId>joda-time</groupId>
            <artifactId>joda-time</artifactId>
            <version>2.2</version>
        </dependency>
    </dependencies>
    
</project>
실행
빌드에 성공했다면 루트 디렉터리에서 다음과 같이 실행하여 테스트한다.
mvn exec:java -Dexec.mainClass=hello.HelloWorld
스프링 시작하기 강좌 중 Quick Start 정리
먼저 메이븐 프로젝트의 구조에 맞게 디렉토리를 만든다.
quick-start
   └── src
        └── main
             └── java
                  └── hello
프로젝트 루트(quick-start)에 pom.xml 파일을 만든다.
스프링 프레임워크를 의존 라이브러리 설정에 추가할 때는
http://projects.spring.io/spring-framework/에서 가장 최신 릴리스 버전을 선택한다.
pom.xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                             http://maven.apache.org/maven-v4_0_0.xsd">

  <modelVersion>4.0.0</modelVersion>
  <groupId>net.java_school</groupId>
  <artifactId>quick-start</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name>quick-start</name>
  <url>http://maven.apache.org</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>4.1.4.RELEASE</version>
    </dependency>
  </dependencies>
  
</project>
스프링은 빈을 관리하는 빈 컨테이너 기능이 핵심이다.
빈 컨테이너를 기반으로 많은 프로젝트가 발전하고 있다.
지금의 예제는 스프링의 핵심인 빈 컨테이너의 기능에 대해 알아보는 예제이다.
예제를 위해 pom.xml에는 spring-context를 의존성 설정에 추가한다.
메이븐은 빌드 툴로서 많은 기능을 내포하고 있는데, 그 기능 중 지금 예제에서 우리가 알아야 할 기능은 의존성 관리 기능이다.
메이븐의 의존성 관리 기능은 프로젝트가 A라는 라이브러리에 의존하는데 A는 B라는 라이브러리에 의존한다면, A만의 의존성 설정으로 A뿐 아니라 B 역시 저장소에 저장한다.
spring-context 만을 의존성 설정에 추가했더라도 spring-context 가 의존하는 다른 라이브러리 역시 저장소에 저장된다. 예제를 진행하면서 확인할 수 있다.
명령 프롬프트를 연다.
C:\maven\quick-start\src\main\java\hello>notepad MessageService.java
열린 메모장에 아래 코드를 복사하여 저장한 후 메모장을 닫는다.
MessageService.java
package hello;

public interface MessageService {
    String getMessage();
}
C:\maven\quick-start\src\main\java\hello>notepad MessagePrinter.java
열린 메모장에 아래 코드를 복사하여 저장한 후 메모장을 닫는다.
MessagePrinter.java
package hello;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class MessagePrinter {

    @Autowired
    private MessageService service;

    public void printMessage() {
        System.out.println(this.service.getMessage());
    }
}
C:\maven\quick-start\src\main\java\hello>notepad Application.java
열린 메모장에 아래 코드를 복사하여 저장한 후 메모장을 닫는다.
Application.java
package hello;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.*;

@Configuration
@ComponentScan
public class Application {

    @Bean
    MessageService mockMessageService() {
        return new MessageService() {
            public String getMessage() {
              return "Hello World!";
            }
        };
    }

  public static void main(String[] args) {
      ApplicationContext context = 
          new AnnotationConfigApplicationContext(Application.class);
      MessagePrinter printer = context.getBean(MessagePrinter.class);
      printer.printMessage();
  }
}
mvn compile로 빌드에 성공했다면 프로젝트 루트에서 다음과 실행하여 결과를 확인한다.
mvn exec:java -Dexec.mainClass=hello.Application

이 예제는 DI의 기본적인 예제이다.
MessagePrinter는 MessageService 구현체와 결합되어 있지 않다.
스프링 프레임워크가 묶어주고 있다.

만일 mvn compile 이 실패했고, 실패 메시지 중에 "annotations are not supported in -source 1.3" 문장이 있다면 pom.xml 파일을 열고 </project> 바로 위에 다음을 추가한다.
<build>
    <finalName>quick-start</finalName>
    <pluginManagement>
        <plugins>
            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.0</version>
            </plugin>
        </plugins>
    </pluginManagement>
</build>
테스트 실행까지 확인했다면 메이븐의 로컬 저장소에 저장된 어떤 라이브러리가 저장되었는지 확인한다.
참고로 메이븐의 로컬 저장소는 사용자 폴더 안의 .m2 폴더이다.
C:\Users\kim\.m2\repository\org\springframework>cd
C:\Users\kim\.m2\repository\org\springframework

C:\Users\kim\.m2\repository\org\springframework>dir /w

[spring-aop]  [spring-beans]  [spring-context]  [spring-core]  

[spring-expression]

spring-context 만 의존성 설정을 했으나 aop beans core expression 도 설치된 것을 확인할 수 있다.
이는 메이븐의 의존성 전이 기능 때문이다.
context 가 의존하는 라이브러리도 저장소에 설치된다.

여기까지가 스프링 공식 사이트의 "시작하기"에 관한 내용이다.
이후 스프링은 아래 순서로 진행한다.
Spring DI
Spring AOP
Spring JDBC
Spring 트랜잭션
Spring MVC
Spring Security
빈 유효성 검사

##참고
Building Java Projects with Maven
http://projects.spring.io/spring-framework/#quick-start

