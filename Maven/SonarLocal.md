# SonarQube Local Exclusion

## 1. Setup SonarQube local
Dựa trên [document của Sonar](https://docs.sonarqube.org/9.6/analyzing-source-code/scanners/sonarscanner-for-maven/), ta có thể thiết lập các thông số mặc định khi chạy lệnh Sonar trên local theo cách sau:

Tại thư mục ``<MAVEN_HOME>/conf`` hoặc ``~/.m2``, chỉnh sửa (hoặc tạo) file ``setting.xml`` để thiết lập các tham số cấu hình Sonar. Chẳng hạn như:
```xml
<settings>
    <pluginGroups>
        <pluginGroup>org.sonarsource.scanner.maven</pluginGroup>
    </pluginGroups>
    <profiles>
        <profile>
            <id>sonar</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <properties>
                <!-- Optional URL to server. Default value is http://localhost:9000 -->
                <sonar.host.url>
                  http://localhost:4950
                </sonar.host.url>
                <!-- Access token of SonarQube --> 
                <sonar.login>
                    squ_d962f768743d014018371b1463344180e95a5e05
                </sonar.login>
                <!-- Other properties here... -->
            </properties>
        </profile>
     </profiles>
</settings>
```

## 2. Chạy lệnh analyze của Sonar

Sau khi thiết lập thông số mặc định xong, ta có thể vào project của mình và chạy lệnh phân tích Sonar với câu lệnh ngắn gọn sau:
``mvn clean verify sonar:sonar``

## 3. Exclude các file, folder không cần thiết

Trong file ``pom.xml`` của dự án, ta có thể exclude các file bằng cách thêm thẻ ``<sonar.exclusions>`` vào ``<properties>``. Ví dụ:
```xml
...
<properties>
    ...
    <sonar.exclusions>
        **/Application.java, <!-- các file có tên Application.java -->
        **/WebSecurityConfig.java,
        **/**DTO.java, <!-- các file tên có đuôi là DTO.java -->
        **/entity/** <!-- các file nằm trong đường dẫn có chứa thư mục entity -->
    </sonar.exclusions>
</properties>
```

Lưu ý: các file, folder được liệt kê trong mục này đều là các file non-test.
Để exclude những file test (các file trong ``src/test/...``), ta sử dụng thẻ ``<sonar.test.exclusions>``. Ví dụ:
```xml
...
<properties>
    ...
    <sonar.exclusions>
        **/Application.java, <!-- các file có tên Application.java -->
        **/WebSecurityConfig.java,
        **/**DTO.java, <!-- các file tên có đuôi là DTO.java -->
        **/entity/** <!-- các file nằm trong đường dẫn có chứa thư mục entity -->
    </sonar.exclusions>
    <sonar.test.exclusions>
        **/controller/** 
    </sonar.test.exclusions>
</properties>
```
