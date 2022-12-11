# Maven

## Mục lục
[1. Giới thiệu về Maven](#1-giới-thiệu-về-maven)
[2. Tạo một dự án Maven](#2-tạo-một-dự-án-maven)
[3. Cấu trúc POM.xml](#3-cấu-trúc-pom)
[4. Build Lifecycle](#4-build-lifecycle)

# 1. Giới thiệu về Maven
![](https://cloudviet.com.vn/wp-content/uploads/2021/10/Apache-maven.jpg)
Apache Maven là một công cụ quản lý dự án phần mềm. Dựa trên khái niệm về mô hình đối tượng dự án (Project Object Model - POM), Maven có thể quản lý quá trình xây dựng, báo cáo và tài liệu của dự án từ một phần thông tin trung tâm.
Mục tiêu chính của Maven là cho phép các developer nắm được các trạng thái trong suốt quá trình phát triển của một dự án phần mềm một cách dễ dàng và nhanh chóng. Để đạt được điều này, Maven cần đạt được những mục tiêu nhỏ sau:
-	Làm cho quá trình build trở nên dễ dàng: Maven hỗ trợ những chi tiết cầu kì trong lúc build, khiến việc build dễ dàng hơn
-	Cung cấp hệ thống build đồng nhất: Maven build một dự án với việc sử dụng POM và nhiều plugins. Một khi đã quen với một dự án Maven, bạn sẽ hiểu được những dự án Maven khác 
-	Cung cấp những thông tin quan trọng của project:  change log from source control, cross referenced sources, các dependencies của project, …

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
- **artifactId:** Thẻ này cho biết tên của thành phẩm chính được tạo bởi dự án này. Thành phẩm chính của dự án thường là file JAR. Các thành phẩm phụ như source bundles cũng sử dụng artifactId như một phần trong tên cuối cùng của chúng. Một thành phẩm điển hình do Maven tạo ra sẽ có dạng <artifactId>-<version>.<extension> (ví dụ: myapp-1.0.jar).
- **version:** Thẻ này cho biết phiên bản của thành phẩm do dự án tạo ra. Giúp ta quản lý phiên bản và ta sẽ thường thấy SNAPSHOT trong một phiên bản, biểu thị rằng một dự án đang ở trạng thái phát triển
- **name:** Thẻ này cho biết tên hiển thị sử dụng cho dự án. Tên này thường được sử dụng trong các tài liệu do ;Maven tạo ra.
- **url:** Thẻ này biểu thị đường dẫn đến dự án. Đường dẫn này thường được sử dụng trong các tài liệu do Maven tạo ra
- **properties:** Thẻ chứa những thông tin thuộc tính được sử dụng trong file POM.
- **dependencies:** Thẻ chứa danh sách những dependencies của dự án, là một thẻ cốt lõi của POM.
- **build:** Thẻ này xử lý những thứ như khai báo cấu trúc thư mục dự án và quản lý các plugin.

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
Build lifecycle là những định nghĩa về tiến trình build và distribute của một dự án
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

