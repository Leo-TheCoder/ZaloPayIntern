# Maven

## Mục lục
[1. Giới thiệu về Maven](#1-giới-thiệu-về-maven)

[2. Tạo một dự án Maven](#2-tạo-một-dự-án-maven)

[3. Cấu trúc POM.xml](#3-cấu-trúc-pom)

[4. Build Lifecycle](#4-build-lifecycle)

[5. Dependency Management](#5-dependency-management)

[6. Khởi chạy application từ Maven](#6-khởi-chạy-application-từ-maven)

[7. Tích hợp Sonarqube](#7-tích-hợp-sonarqube)

# 1. Giới thiệu về Maven
![](https://cloudviet.com.vn/wp-content/uploads/2021/10/Apache-maven.jpg)
Apache Maven là một công cụ quản lý dự án phần mềm. Dựa trên khái niệm về mô hình đối tượng dự án (Project Object Model - POM), Maven có thể quản lý quá trình xây dựng, báo cáo và tài liệu của dự án từ một phần thông tin trung tâm.

Mục tiêu chính của Maven là cho phép các developer nắm được các trạng thái trong suốt quá trình phát triển của một dự án phần mềm một cách dễ dàng và nhanh chóng. Để đạt được điều này, Maven cần đạt được những mục tiêu nhỏ sau:
-	Làm cho quá trình build trở nên dễ dàng: Maven hỗ trợ những chi tiết cầu kì trong lúc build, khiến việc build dễ dàng hơn, tăng hiệu suất tối ưu nhất cho dự án.
-	Cung cấp hệ thống build đồng nhất: Maven build một dự án với việc sử dụng POM và nhiều plugins. Một khi đã quen với một dự án Maven, bạn sẽ hiểu được những dự án Maven khác. Thiết lập tính nhất quán được đảm bảo trên tất cả các dự án.
-	Là một nơi quản lý và lưu trữ các thư viện, các dependency, ... thông qua Maven Repository và liên tục được cập nhật.

# 2. Tạo một dự án Maven
Các bước cài đặt một project sử dụng Maven:
- Cài đặt Maven: https://maven.apache.org/install.html
- Chạy lệnh sau để khởi tạo một project: 
```bash
mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```
- Giải thích các tham số:
    -	groupId: thường đặt theo tên của tổ chức hoặc nhóm tạo ra dự án
    -	artifactId: thường lấy theo tên viết tắt của dự án.
    -	archetypeArtifactId: là loại dự án sẽ được tạo, Maven cung cấp nhiều kiểu mẫu có sẵn cho người dùng lựa chọn khi khởi tạo.
- Kết quả thu được sẽ là một project với kiến trúc như sau:
```
my-app
|-- pom.xml
`-- src
    |-- main
    |   `-- java
    |       `-- com
    |           `-- mycompany
    |               `-- app
    |                   `-- App.java
    `-- test
        `-- java
            `-- com
                `-- mycompany
                    `-- app
                        `-- AppTest.java

```
# 3. Cấu trúc POM
Tệp pom.xml là cấu hình cốt lõi của một dự án Maven. Đây là tệp cấu hình duy nhất chứa phần lớn thông tin cần thiết để xây dựng một dự án theo cách ta muốn. Tệp pom.xml thường rất lớn và có thể gây khó khăn bởi sự phức tạp của nó, nhưng ta không cần thiết phải hiểu tất cả mọi dòng và vẫn có thể sử dụng nó một cách hiệu quả. Những từ khóa thường thấy trong POM bao gồm:
- **project:** Là thẻ top-level trong một file pom.xml.
- **modelVersion:** Thẻ này cho biết phiên bản của object model mà POM này đang sử dụng. Bản thân phiên bản của object model rất hiếm khi thay đổi nhưng thẻ này là bắt buộc để đảm bảo tính ổn định của việc sử dụng và khi phải thay đổi mô hình.
- **groupId:** Thẻ này cho biết mã định danh duy nhất của tổ chức hoặc nhóm đã tạo dự án. groupId là một trong những mã định danh chính của dự án và thường dựa trên tên miền đầy đủ của tổ chức. Ví dụ: org.apache.maven.plugins là groupId được chỉ định cho tất cả các plugin Maven.
- **artifactId:** Thẻ này cho biết tên của thành phẩm chính được tạo bởi dự án này. Thành phẩm chính của dự án thường là file JAR. Các thành phẩm phụ như source bundles cũng sử dụng artifactId như một phần trong tên cuối cùng của chúng. Một thành phẩm điển hình do Maven tạo ra sẽ có dạng ``<artifactId>-<version>.<extension>`` (ví dụ: myapp-1.0.jar).
- **version:** Thẻ này cho biết phiên bản của thành phẩm do dự án tạo ra. Giúp ta quản lý phiên bản và ta sẽ thường thấy SNAPSHOT trong một phiên bản, biểu thị rằng một dự án đang ở trạng thái phát triển
- **name:** Thẻ này cho biết tên hiển thị sử dụng cho dự án. Tên này thường được sử dụng trong các tài liệu do ;Maven tạo ra.
- **url:** Thẻ này biểu thị đường dẫn đến dự án. Đường dẫn này thường được sử dụng trong các tài liệu do Maven tạo ra
- **properties:** Thẻ chứa những thông tin thuộc tính được sử dụng trong file POM.
- **dependencies:** Thẻ chứa danh sách những dependencies của dự án, là một thẻ cốt lõi của POM.
- **build:** Thẻ này có công dụng khai báo và quản lý các plugin được sử dụng trong quá trình build.

Ví dụ một file pom.xml:
```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0-SNAPSHOT</version>

  <name>my-app</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>

  <build>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
        <!-- clean lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#clean_Lifecycle -->
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <!-- default lifecycle, jar packaging: see https://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_jar_packaging -->
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        ...
        <plugin>
          <artifactId>maven-project-info-reports-plugin</artifactId>
          <version>3.0.0</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>
```
# 4. Build lifecycle
Build lifecycle là những định nghĩa về tiến trình build và distribute của một dự án.

Có 3 loại built-in lifecycle gồm:
-	`default`: handles your project deployment
-	`clean`: handles project cleaning
-	`site`: handles the creation of your project's web site

Một build lifecycle bao gồm nhiều giai đoạn, ví dụ đối với `default` lifecycle sẽ bao gồm:
- `validate` - validate the project is correct and all necessary information is available
- `compile` - compile the source code of the project
- `test` - test the compiled source code using a suitable unit testing framework. These tests should not require the code be packaged or deployed
- `package` - take the compiled code and package it in its distributable format, such as a JAR.
- `verify` - run any checks on results of integration tests to ensure quality criteria are met
- `install` - install the package into the local repository, for use as a dependency in other projects locally
- `deploy` - done in the build environment, copies the final package to the remote repository for sharing with other developers and projects.

# 5. Dependency Management

Maven cung cấp một cơ chế để dễ dàng điều phối các dependency từ nhiều nơi khác nhau. Cụ thể hơn với những dependency mà dự án của ta sử dụng, Maven sẽ kiểm tra các dependency mà những dependency này phụ thuộc ở remote repository. Cứ đệ quy như vậy đến những project và dependency khác, tạo thành một cây dependency trong dự án của ta. Đây được gọi là Transitive Dependencies.
```
  A
  ├── B
  │   └── C
  │       └── D
  ├── E
  │   ├── F
  │   └── G
  ...
```

Do có tính chất đệ quy mà cây dependency này sẽ mau chóng phát triển ra, dễ dẫn đến những tình huồng phụ thuộc chồng chéo giữa các dependency. Vì thế Maven có những tính năng nhằm giảm thiểu điều này:

- Dependency Mediation: đây là cơ chế giúp xác định version của một artifact khi nhiều version khác nhau của artifact đó được xác định trong cây dependency. Maven sẽ lựa chọn sử dụng version của dependency "gần nhất" trong cây.
Ví dụ:
```
  A
  ├── B
  │   └── C
  │       └── D 2.0
  └── E
      └── D 1.0
```
Ở đây có 2 dependency D được xác định => Maven sẽ chọn D 1.0 do đường đi A->B->C->D xa hơn A->E->D.
Trong trường hợp cả 2 đều có cùng level (độ dài đường đi là như nhau) thì Maven sẽ tự động chọn dependency đầu tiên load được.

- Dependency Management: Maven có thể cho ta tự chọn version của một dependency dù project không trực tiếp sử dụng dependency đó, thông qua việc khai báo thẳng version của artifact vào danh sách các dependency trực tiếp. 
Trong ví dụ trên, ta cũng có thể ép Maven sử dụng D 2.0 bằng cách tự khai báo dependency đó trong dự án:
```
  A
  ├── B
  │   └── C
  │       └── D 2.0
  ├── E
  │   └── D 1.0
  │
  └── D 2.0      
```

- Dependency Scope: Với chức năng này, ta có thể xác định được dependency sẽ được sử dụng trong từng giai đoạn build

- Excluded Dependencies: Nếu project X phụ thuộc vào Y, Y phụ thuộc vào Z thì X có thể hoàn toàn loại Z ra , ta có sử dụng thẻ ``<exclusion>``
```
 X
 ├── Y
 │   └── Z    <!-- Can be excluded -->
 │       └── F
 ├── C
 │   └── D
```

- Optional Dependencies: Nếu Y phụ thuộc vào Z thì Y có thể xem Z là optional dependency, Khi X phụ thuộc Y thì chỉ phụ thuộc mỗi Y mà không phụ thuộc Z. X có thể trực tiếp thêm dependency Z vào nếu có nhu cầu. **Có thểm xem như đây là excluded by default**

# 6. Khởi chạy application từ Maven

## Cách 1:
1. Config JVM: Thêm vào enviroment variable ``MAVEN_OPTS`` với giá trị 
- ``-Xms256m -Xmx512m``
Config này giúp hiệu chỉnh heapstack khởi tạo JVM cho dự án này (cụ thể như trên sẽ là tối thiểu 256MB đến 512MB)
- Chọn thuật toán cho Garbage Collection
```bash
-XX:+UseSerialGC => sử dụng Serial GC
-XX:+UseParallelGC => sử dụng Parallel GC
-XX:+USeParNewGC => sử dụng CMS GC
-XX:+UseG1GC => sử dụng G1 GC
```
- Ngoài ra có thể lưu lại hoạt động GC, để chúng ta có thể kiểm tra khi có lỗi liên quan đến bộ nhớ xảy ra, ví dụ như:
```bash
-XX:+UseGCLogFileRotation
-XX:NumberOfGCLogFiles=10
-XX:GCLogFileSize=50M
-Xloggc:/home/user/log/gc.log
```
Trong đó:
  - ``-XX:+UseGCLogFileRotation``: việc ghi log sẽ xoay vòng qua từng file khi nó đạt đến giới hạn kích thước và số lượng file
  - ``-XX:NumberOfGCLogFiles``: Số lượng file log tối đa của một ứng dụng, những file cũ sẽ bị xóa nhường chỗ cho file mới
  - ``-XX:GCLogFileSize``: kích thước tối đa của 1 file
  - ``-Xloggc``: vị trí đặt file log
2. Edit POM.xml
  ```xml
  <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <version>2.4</version>
                <configuration>
                    <archive>
                        <manifest>
                            <mainClass>com.mycompany.app.App</mainClass>
                        </manifest>
                    </archive>
                </configuration>
            </plugin>
            <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>exec-maven-plugin</artifactId>
        <version>3.1.0</version>
      </plugin>
        </plugins>
    </build>     
  ``` 

3. Build

Chạy lệnh ``mvn clean package`` để build dự án thành file JAR, WAR, ...

4. Run

Dùng lệnh `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"` để tiến hành chạy project
Tham số ``exec.mainClass`` dùng để Maven xác định main class của dự án.

5. Kết quả
![](https://scontent.fsgn12-1.fna.fbcdn.net/v/t1.15752-9/318578251_734167348310932_972204297386700114_n.png?_nc_cat=110&ccb=1-7&_nc_sid=ae9488&_nc_ohc=jL-CcI-vb-MAX-uAVCa&_nc_ht=scontent.fsgn12-1.fna&oh=03_AdRJK4CIT6vfZ7h9jq4YDIguXZDDmDRFktf0B37tpZVcew&oe=63C018AC)

## Cách 2:
1. Build:
Chạy lệnh ``mvn clean package`` để build dự án thành file JAR, WAR, ...
2. Sử dụng lệnh ``java -jar -Xms256m -Xmx512m target/my-app-1.0-SNAPSHOT.jar``
3. Kết quả:
![](https://scontent.fsgn6-2.fna.fbcdn.net/v/t1.15752-9/315521025_454806316802016_3480769265275235368_n.png?_nc_cat=105&ccb=1-7&_nc_sid=ae9488&_nc_ohc=aXX3fAmvau0AX91C5Wc&_nc_ht=scontent.fsgn6-2.fna&oh=03_AdTgRdmXARxcUbSP79BsfQYZiT1Ywj-uGpA9YpeWaabzgA&oe=63C6963D)
# 7. Tích hợp Sonarqube

1. Sonarqube là gì?

Sonarqube là một nền tảng mã nguồn mở được phát triển từ mười năm trước bởi SonarSource với mục đích kiểm tra liên tục chất lượng code, review của dự án bằng cách phân tích code để phát hiện các đoạn code không tốt, code lỗi hay những lỗ hổng bảo mật.
Các chức năng chính của Sonarqube có thể kể đến là:
- Phát hiện bug
- Phát hiện code smell, duplicate
- Tính toán độ phủ của unit test (Unit-test converage)
- Tính toán technical debt
- So sánh chất lượng code với các lần kiểm tra trước
2. Cài đặt và sử dụng Sonarqube
- Cài đặt server Sonarqube chạy local: https://docs.sonarqube.org/latest/setup-and-upgrade/install-the-server/
![](https://scontent.fsgn12-1.fna.fbcdn.net/v/t1.15752-9/318444883_1538801843302204_1513320237755330929_n.png?_nc_cat=104&ccb=1-7&_nc_sid=ae9488&_nc_ohc=FycA8aZmoEAAX9i8Sqp&_nc_ht=scontent.fsgn12-1.fna&oh=03_AdRP3pZyZHkft95ldDNXXVopvbbMqmE-DHN_dw0tIggmxg&oe=63C644EE)
- Analyze project maven với câu lệnh ``mvn clean verify sonar:sonar -Dsonar.login=myAuthenticationToken`` với ``myAuthenticationToken`` lấy từ setting Sonarqube
![](https://scontent.fsgn6-1.fna.fbcdn.net/v/t1.15752-9/317493744_493455752887012_3507777952413794895_n.png?_nc_cat=106&ccb=1-7&_nc_sid=ae9488&_nc_ohc=HbduRP7u4VkAX_bp-iv&_nc_ht=scontent.fsgn6-1.fna&oh=03_AdSFO_DvOx1fDGtIZntVRK8k9fciY57P6c7JgVI_sHU6FQ&oe=63C65E98)
- Có thể thấy được SonarQube phân tích cho ta thấy những điểm sai sót, chưa tối ưu trong code, ví dụ như: 
![](https://scontent.fsgn12-1.fna.fbcdn.net/v/t1.15752-9/320182952_956742499064623_2011948380284941668_n.png?_nc_cat=102&ccb=1-7&_nc_sid=ae9488&_nc_ohc=xUFGDR2G1BsAX-iEAeB&_nc_ht=scontent.fsgn12-1.fna&oh=03_AdQFDltW28Ar0IMUozyQ793SOEIDOCfhFOVTgSkyypr1og&oe=63C67547)
Và SonarQube giải thích lý do và đưa ra giải pháp cho từng trường hợp, ví dụ:
![](https://scontent.fsgn6-2.fna.fbcdn.net/v/t1.15752-9/318335152_520736070010098_8264256139249853149_n.png?_nc_cat=105&ccb=1-7&_nc_sid=ae9488&_nc_ohc=szWBOZN41dEAX8ZxqG5&_nc_ht=scontent.fsgn6-2.fna&oh=03_AdTdB6C7HXINgtJzG6FjkNGDv5hWjnyyB0yoF_MuLJuQgQ&oe=63C65988)
Nguồn tham khảo: 
- https://maven.apache.org/
- https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/Maven_SE/Maven.html#section5s2
- https://docs.sonarqube.org/latest/analyzing-source-code/scanners/sonarscanner-for-maven/