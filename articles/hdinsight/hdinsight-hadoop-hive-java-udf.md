---
title: "HDInsight에서 Hive와 함께 Java UDF(사용자 정의 함수) 사용 | Microsoft 문서"
description: "HDInsight의 Hive에서 Java UDF(사용자 정의 함수)를 만들고 사용하는 방법을 알아봅니다."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 8d4f8efe-2f01-4a61-8619-651e873c7982
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/27/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 240ba2289ededf7a65c71e2f5007cf3a52ca4f84


---
# <a name="use-a-java-udf-with-hive-in-hdinsight"></a>HDInsight에서 Hive와 함께 Java UDF 사용
Hive는 HDInsight의 데이터 작업에 적합하지만 보다 일반적인 언어가 필요한 경우도 있습니다. Hive를 통해 다양한 프로그래밍 언어를 사용하여 UDF(사용자 정의 함수)를 만들 수 있습니다. 이 문서에서는 Hive에서 Java UDF를 사용하는 방법을 알아봅니다.

## <a name="requirements"></a>요구 사항
* Azure 구독
* HDInsight 클러스터(Windows 또는 Linux 기반)
  
  > [!NOTE]
  > 이 문서에서 대부분 단계는 클러스터 형식 모두;에 대해 작동합니다. 그러나 클러스터에 컴파일된 UDF를 업로드하고 실행하는 데 사용하는 단계는 Linux 기반 클러스터와 관련이 있습니다. Windows 기반 클러스터와 함께 사용할 수 있는 정보에 대한 링크가 제공됩니다.
  > 
  > 
* [JDK Java](http://www.oracle.com/technetwork/java/javase/downloads/) 7 이상(또는 OpenJDK와 같은 이와 동등한 프로그램)
* [Apache Maven](http://maven.apache.org/)
* 텍스트 편집기 또는 Java IDE
  
  > [!IMPORTANT]
  > Linux 기반 HDInsight 서버를 사용하지만 Windows 클라이언트에서 Python 파일을 만드는 경우 LF를 줄 끝으로 사용하는 편집기를 사용해야 합니다. 편집기에서 LF 또는 CRLF를 사용하는지 여부가 확실하지 않은 경우 HDInsight 클러스터에서 유틸리티를 사용하여 CR 문자를 제거하는 단계는 [문제 해결](#troubleshooting) 섹션을 참조하세요.
  > 
  > 

## <a name="create-an-example-udf"></a>UDF 예제 만들기
1. 명령줄에서 새 Maven을 만들려면 다음을 참조하세요.
   
        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   
   > [!NOTE]
   > PowerShell을 사용하는 경우 매개 변수를 묶는 따옴표를 배치해야 합니다. 예: `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.
   > 
   > 
   
    **exampleudf**라는 새 디렉터리를 만들고 여기에는 Maven 프로젝트가 포함됩니다.
2. 프로젝트를 만들면 프로젝트의 일부로 작성된 **exampleudf/src/test** 디렉터리를 삭제합니다. 그러나 이 예에서는 사용되지 않습니다.
3. **exampleudf/pom.xml**을 열고 기존 `<dependencies>` 항목을 다음으로 바꿉니다.
   
        <dependencies>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-client</artifactId>
                <version>2.7.1</version>
                <scope>provided</scope>
            </dependency>
            <dependency>
                <groupId>org.apache.hive</groupId>
                <artifactId>hive-exec</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
        </dependencies>
   
    이러한 항목은 HDInsight 3.3 및 3.4 클러스터와 함께 포함된 Hadoop 및 Hive의 버전을 지정합니다. [HDInsight 구성 요소 버전 관리](hdinsight-component-versioning.md) 문서에서 HDInsight를 제공하는 Hadoop 및 Hive의 버전에 대한 정보를 찾을 수 있습니다.
   
    파일 끝의 `</project>` 줄 앞에 `<build>` 섹션을 추가합니다. 이 섹션은 다음을 포함합니다.
   
        <build>
            <plugins>
                <!-- build for Java 1.7, even if you're on a later version -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.3</version>
                    <configuration>
                        <source>1.7</source>
                        <target>1.7</target>
                    </configuration>
                </plugin>
                <!-- build an uber jar -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-shade-plugin</artifactId>
                    <version>2.3</version>
                    <configuration>
                        <!-- Keep us from getting a can't overwrite file error -->
                        <transformers>
                            <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                            </transformer>
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
                            </transformer>
                        </transformers>
                        <!-- Keep us from getting a bad signature error -->
                        <filters>
                            <filter>
                                <artifact>*:*</artifact>
                                <excludes>
                                    <exclude>META-INF/*.SF</exclude>
                                    <exclude>META-INF/*.DSA</exclude>
                                    <exclude>META-INF/*.RSA</exclude>
                                </excludes>
                            </filter>
                        </filters>
                    </configuration>
                    <executions>
                        <execution>
                            <phase>package</phase>
                            <goals>
                                <goal>shade</goal>
                            </goals>
                        </execution>
                    </executions>
                </plugin>
            </plugins>
        </build>
   
    이러한 항목은 프로젝트를 빌드하는 방법을 정의합니다. 특히, 프로젝트에서 사용하는 Java 버전과 클러스터로의 배포를 위한 uberjar를 빌드하는 방법이 유용합니다.
   
    변경이 완료되면 파일을 저장합니다.
4. **exampleudf/src/main/java/com/microsoft/examples/App.java**라는 이름을 **ExampleUDF.java**로 바꾸고 편집기에서 파일을 엽니다.
5. **ExampleUDF.java** 파일의 내용을 다음 코드로 바꾼 다음 파일을 저장합니다.
   
        package com.microsoft.examples;
   
        import org.apache.hadoop.hive.ql.exec.Description;
        import org.apache.hadoop.hive.ql.exec.UDF;
        import org.apache.hadoop.io.*;
   
        // Description of the UDF
        @Description(
            name="ExampleUDF",
            value="returns a lower case version of the input string.",
            extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
        )
        public class ExampleUDF extends UDF {
            // Accept a string input
            public String evaluate(String input) {
                // If the value is null, return a null
                if(input == null)
                    return null;
                // Lowercase the input string and return it
                return input.toLowerCase();
            }
        }
   
    문자열 값을 허용하고 문자열의 소문자 버전을 반환하는 UDF를 구현합니다.

## <a name="build-and-install-the-udf"></a>UDF 빌드 및 설치
1. 다음 명령을 실행하여 UDF를 컴파일하고 패키징합니다.
   
        mvn compile package
   
    그러면 UDF를 **exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar**에 빌드하고 패키징합니다.
2. `scp` 명령을 사용하여 파일을 HDInsight 클러스터에 복사합니다.
   
        scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight
   
    **myuser** 를 클러스터의 SSH 사용자 계정으로 바꿉니다. **mycluster** 를 클러스터 이름으로 바꿉니다. 암호를 사용하여 SSH 계정을 보호한 경우 암호를 입력하라는 메시지가 나타납니다. 인증서를 사용하는 경우, `-i` 매개 변수를 사용하여 개인 키를 지정해야 합니다.
3. SSH를 사용하여 클러스터에 연결합니다. 
   
        ssh myuser@mycluster-ssh.azurehdinsight.net
   
    HDInsight에서 SSH를 사용하는 방법에 대한 자세한 내용은 다음 문서를 참조하세요.
   
   * [Linux, Unix 또는 OS X의 HDInsight에서 Linux 기반 Hadoop과 SSH 사용](hdinsight-hadoop-linux-use-ssh-unix.md)
   * [Windows의 HDInsight에서 Linux 기반 Hadoop과 SSH 사용](hdinsight-hadoop-linux-use-ssh-windows.md)
4. SSH 세션에서 HDInsight 저장소에 jar 파일을 복사합니다.
   
        hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars

## <a name="use-the-udf-from-hive"></a>Hive에서 UDF 사용
1. 다음을 사용하여 SSH 세션에서 Beeline 클라이언트를 시작합니다.
   
        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
   
    이 명령에서는 클러스터에 대한 로그인 계정에 **admin** 의 기본값을 사용했다고 가정합니다.
2. `jdbc:hive2://localhost:10001/>` 프롬프트가 표시되면 다음을 입력하여 Hive에 UDF를 추가하고 함수로 노출합니다.
   
        ADD JAR wasbs:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
        CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';
3. UDF를 사용하여 검색한 값을 테이블에서 소문자 문자열로 변환합니다.
   
        SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;
   
    그러면 테이블에서 장치 플랫폼(Android, Windows, iOS 등)을 선택하고 문자열을 소문자로 변환한 다음 표시하게 됩니다. 출력은 다음과 유사합니다.
   
        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## <a name="next-steps"></a>다음 단계
Hive로 작업하는 다른 방법은 [HDInsight와 함께 Hive 사용](hdinsight-use-hive.md)을 참조하세요.

Hive 사용자 정의 함수에 대한 자세한 내용은 apache.org의 Hive wiki에서 [Hive 연산자 및 사용자 정의 함수](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) 섹션을 참조하세요.




<!--HONumber=Nov16_HO3-->


