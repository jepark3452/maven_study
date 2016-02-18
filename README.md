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

빌드환경의 필요성
빌드(Build)란?
소스 코드 파일을 동작하는 독립적인 소프트웨어 산출물로 만드는 과정이다.
빌드의 가장 중요한 단계중 하나는 소스 코드 파일을 실행 코드로 변환하는 컴파일 과정이다. 
간단한 프로그램은 하나의 파일만 컴파일해도 되겠지만, 많은 소스코드와 다양한 버전닝을 필요하는 복잡한 프로젝트는 어떻게 할것인가?
빌드툴(Build Tool)이 이런 수고로움을 덜어주고 있다!!
빌드 툴(Build Tool)
새로운 버전의 프로그램을 빌드할때 사용하는 툴
일반적인 빌드 툴이 제공하는 기능 
전처리(preprocessing), 컴파일(compilation), 패키징(packaging), 테스팅(testing), 배포(distribution)
빌드 자동화란?
개발자가 매일 진행하는 일을 자동화 하는 행동들, 빌드툴들이 이런 자동화를 도와준다
소스 코드를 바이너리 코드로 컴파일
바이너리 코드 패키징
테스트 수행
실서비스 시스템 배포
도큐먼트 및 릴리즈 노트 생성
개발에 있어 빌드란?
애플리케이션 개발에 있어 빌드는 개발하는 애플리케이션을 완성 시켜주는 마지막 단계 
개발한 것들을 산출물, 결과물을 내는 과정
빌드를 잘하는 것은  훌륭한 애플리케이션을 만드는데 중요한 과정이다
좋은 빌드 툴을 선택하는것은 개발의 기본 요건이다!
내가 개발을 하면서 느낀 빌드란
갓 회사 입사했을때 : 빌드툴이라는 개념조차 몰랐다;; 그때 ANT를 사용하였는데 난 스크립트만 돌리면 알아서 서버에 빌드되고 잘돌아갔던거 같다.
몇년 후 : ANT라는 존재를 알았으나 새 프로젝트를 진행하지 않는 이상 빌드툴을 고려할 일이 없었다. 단지, 이클립스로 열심히 코드 작성하고 필요한 라이브러리 다운 
받거나 디렉토리 수정이 다였다.
Maven 프로젝트를 받았을때 그 신기함을 잊을수 없다. 프로젝트 구성이 자동으로 되고 라이브러리 파일을 다운받을 필요도 없었고 레파지토리(저장소)의 존재는놀라웠다.
지금은? Maven 프로젝트를 배포를 담당하고 있고 매주 빌드와 배포를 반복하고 있다. 서로 의존하고 있는 큰 프로젝트를 이런 빌드툴없이 사용하였다면 얼마나 많은 시간은 빌드에 소비하였을까? 사실 빌드툴 없는 환경은 생각해본적이 없다.
왜 빌드 툴이 필요한가? 이거 정말 중요할 듯. 
아직도 빌드 툴의 필요성에 대해 의구심을 가지는 개발자들이 있다.
필요성만 느낀다면 이후에는 관심을 가지고 학습하지 않을까?
빌드 툴 소개
Apache Ant 
자바로 구현됨, 자바 프로젝트 빌드에 적합
XML 파일(build.xml)을 사용하여 빌드 프로세스와 의존성 정의 
Apache Maven
Apache Ant와 비슷하나 컨셉과 동작방식이 다름
C#, Ruby, Scala등 Java가 아닌 프로젝트도 빌드와 관리가 가능
XML 파일(pom.xml)에 정의함(의존성, 빌드 순서, plug-in등)
Maven Repository - 자바 라이브러리나 플러그인을 다운받을수 있음
Gradle
Ant나 Maven과 비슷한 컨셉
XML대신 DSL을 통해 프로젝트 정의 
Maven 소개
Maven 이란?
빌드툴? 프로젝트 관리 툴?
소스 코드로 부터 배포 가능한 산출물을 빌드하는 '빌드툴'
프로젝트 관리 도구
프로젝트 오브젝트 모델, 표준 집합, 프로젝트 라이프사이클, 의존성관리 시스템, 라이프사이클에 정의된 단계에서 플러그인 골을 실행하기 위한 로직을 포함하는 관리 툴이다
Archetype 통해 적합한 구조의 프로젝트 생성, 
애플리케이션 명칭과 버전 및 연관된 라이브러리에 대한 정보 등을 종합적으로 관리
서브 프로젝트간의 관계 손쉽게 설정
Nexus를 통해 저장소 관리 
Maven이 제공하는 기능
Builds
Documentation
Reporting
Dependencies
SCMs
Releases
Distribution
 
Maven의 시작
메이븐은 Jakarta Turbine 프로젝트의 빌드 프로세스를 단순화 하기 위해 시작되었다. 
여러 프로젝트 마다 자신의 ANT 빌드 파일과 JAR파일들이 존재 하고 있었다. 이런 환경은 빌드를 복잡하게 만든다. 
점차 프로젝트가 커짐에 따라 프로젝트를 빌드하는 표준방법 필요하게 되었다. 이로 인해 나온것이 바로 메이븐이다.
Maven의 목적
개발자가 짧은 기간에 개발의 전체 상태를 이해 할 수 있도록한다
 
Making the build process easy
Providing a uniform build system
Providing quality project information
Providing guidelines for best practices development
Allowing transparent migration to new features
Maven 간단하게 보기 
참고할만한 페이지 http://maven.apache.org/guides/getting-started/maven-in-five-minutes.html
간단한 웹 프로젝트 시작하기
 
archetype:generate
$ mvn archetype:generate -DgroupId=net.sasary.study -DartifactId=sasary -DarchetypeArtifactId=maven-archetype-webapp
eclipse 프로젝트 설정 추가 
$ mvn eclipse:eclipse
eclipse에서 프로젝트 import
import.png
완성 
스크린샷 2012-11-20 오전 1.22.35.png
컴파일 하기( 이클립스에서만 띄운다면 mvn 툴을 사용해서 하면 된다 (smile) ) 
$ mvn compile
패키지 만들기
$ mvn package
 
프로젝트 공통 구조
src/main/java	Application/Library sources
src/main/resources	Application/Library resources
src/main/filters	Resource filter files
src/main/assembly	Assembly descriptors
src/main/config	Configuration files
src/main/scripts	Application/Library scripts
src/main/webapp	Web application sources
src/test/java	Test sources
src/test/resources	Test resources
src/test/filters	Test resource filter files
src/site	Site
LICENSE.txt	Project's license
NOTICE.txt	Notices and attributions required by libraries that the project depends on
README.txt	Project's readme
 
maven lifecycle과 dependency
lifecycle.jpg
 
phase & goal.JPG
 
repository.JPG
메이븐에서 의존관계 라이브러리 관리 방법
ant vs maven
자바 환경에서 주로 사용하는 Maven과 Ant는 항상 비교되는 이슈 중 하나인거 같다. 과연 둘을 비교하는것이 맞을까? 
내가 느낀 Ant와 Maven의 차이는 서로 대등한 기능들에 대한 비교상대는 아닌거 같다. 비교하기에는 서로의 컨셉부터 다르지 않은가?
Apache Ant
Apache Maven
기존의 make와 같은 빌드의 단점을 보완한 XML 기반의 빌드툴
하나이상의 정의된 xml 파일 스크립트에 의해 동
xml 파일에 정의된 target을 수행하고 target은 task수행
각 작업은 정의된 순서에 따라 진행되어 짐
Ant와 비슷하나 프로젝트 관리를 위한 컨셉으로 만들어진 프로젝트 관리툴
모든 프로젝트를 특정한 구조와 지원되는 task work-flow들의 일련으로 간주
Pom.xml에 프로젝트 구조와 동작 정의
프로젝트의 구조와 work-flow는 정해진 컨벤션에 맞춰 동작
무슨일을 하고 무엇에 의존하는지 명확하게 정의할수 있도록 xml 최적화
Ant가 실행하는 모든 것들은 Ant 스크립트에서 확인할수 있음
익숙하지 않은 사용자도 쉽게 접근 가능
간결한 프로젝트 정의, 다른 개발도구나 서버환경에서 자동으로 통합 가능함
쉽게 구조를 검사할수 있고, 결과물에 대해 이해한 상태에서 work-flow를 수행해 볼수 있다. 
숙련된 개발자도 스크립트 실행없이는 빌드를 추론하기 어렵고
복잡한 스크립트는 부담스러운 과제가 될수도 있음
메이븐 시작시 이해과정 필요, 이미 완성된 프로젝트에 적용하기 어려움
자율성이 떨어진다는 의견도 있다
Ant 예제 
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
project name="MyProject" default="dist" basedir=".">
    <description>
        simple example build file
    </description>
  <!-- set global properties for this build -->
  <property name="src" location="src"/>
  <property name="build" location="build"/>
  <property name="dist"  location="dist"/>
 
  <target name="init">
    <!-- Create the time stamp -->
    <tstamp/>
    <!-- Create the build directory structure used by compile -->
    <mkdir dir="${build}"/>
  </target>
 
  <target name="compile" depends="init"
        description="compile the source " >
    <!-- Compile the java code from ${src} into ${build} -->
    <javac srcdir="${src}" destdir="${build}"/>
  </target>
 
  <target name="dist" depends="compile"
        description="generate the distribution" >
    <!-- Create the distribution directory -->
    <mkdir dir="${dist}/lib"/>
 
    <!-- Put everything in ${build} into the MyProject-${DSTAMP}.jar file -->
    <jar jarfile="${dist}/lib/MyProject-${DSTAMP}.jar" basedir="${build}"/>
  </target>
 
  <target name="clean"
        description="clean up" >
    <!-- Delete the ${build} and ${dist} directory trees -->
    <delete dir="${build}"/>
    <delete dir="${dist}"/>
  </target>
</project>
Maven 예제 
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>Maven Quick Start Archetype</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
 
의문점?
Maven은 빌드 툴인가?
Gradle을 사용하는건 어떤가?
참고자료
http://maven.apache.org
http://en.wikipedia.org/wiki/Software_build
http://www.sonatype.com/books/mvnref-book/reference/
http://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html
http://en.wikipedia.org/wiki/Apache_Maven
http://ko.wikipedia.org/wiki/Apache_Ant
