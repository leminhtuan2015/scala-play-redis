###*I - Typesafe Inc*
- `Typesafe Inc` Là công ty xây dựng ra Scala language (https://www.typesafe.com/)
- Cũng đồng thời là công ty tạo ra `Play framework`
- Ngoài ra Typesafe còn cung cấp thêm một số tool phổ biến như :
  + SBT : Scala build tool.
  + Slick : Functional Relational Mapping for Scala.
  + Eclipse for scala


###*II - SBT (Scala build tool)*
- What is SBT ?
  + SBT là một open-source tool cho Scala và Java project
  + SBT tương tự
    + Maven - Java
    + Ant - Java
    + Make - Linux
- What is it use for ?
  + SBT dùng để build ra một project Scala/Java
  + SBT quản lý các thư viện trong một project Scala/Java (tương tự bundle-ruby)

- Install SBT

```shell
  echo "deb https://dl.bintray.com/sbt/debian /" | sudo tee -a /etc/apt/sources.list.d/sbt.list
  sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 642AC823
  sudo apt-get update
  sudo apt-get install sbt
```
- How to use ?
  + SBT cho pháp định nghĩa ra một mô tả về project trong file `build.sbt`
  + Tạo một project Scala bằng sbt

  ```
  $ mkdir hello
  $ cd hello
  $ vim build.sbt
  $ mkdir -p src/main/scala/
  $ sbt run
  ```
  + => sau khi chạy những command trên thì `sbt` đã tạo ra một scala project trong folder `hello`
  + Code ta tạo những file `.scala` và đặt trong src/main/scala
  + Chú ý : SBT ngu hơn maven hay những tool khác : nó không hề tạo cho chúng ta cấu trúc của 1 project (project layout folder)
    mà ta phải tạo bằng tay, thằng sbt nó dùng để chạy, build, test, quản lý thư viện.....

- Cú pháp một file .stb

```scala
// Set the project name to the string "my-project" and the version to 1.0.0.
name := "my-project"

version := "1.0.0"

// Add a single dependency, for tests.
libraryDependencies += "junit" % "junit" % "4.8" % "test"

// Add multiple dependencies.
libraryDependencies ++= Seq(
  "net.databinder" %% "dispatch-google" % "0.7.8",
  "net.databinder" %% "dispatch-meetup" % "0.7.8"
)

// Use the project version to determine the repository to publish to.
publishTo := Some(if (version.value endsWith "-SNAPSHOT") "http://example.com/maven/snapshots" else "http://example.com/maven/releases")
```

*SBT Commands*
- `sbt clean` : Deletes all generated files (in the target directory).
- `sbt compile` : Compiles the main sources (in src/main/scala and src/main/java directories).
- `sbt run` : Runs the main class for the project in the same virtual machine as sbt.
- `sbt console` : tarts the Scala interpreter with a classpath including the compiled sources and all dependencies

###*III - Typesafe Activator*
- What is it ?
  + Là một tool dùng để tạo ra một `Play project`
  + `activator` : Base trên `sbt`
    + Tương tự như `rails new` trong Rails framework ta có : `activator new` trong Play
- Install
  + step 1: Dowload from > https://www.typesafe.com/get-started
  + step 2: Sau khi nhận được 1 thư mục activator sẽ chứa một file `activator`
  + Để sử dụng ta sẽ buildpath vào .bashrc để dừng ở mọi nơi (nếu không mỗi khi dùng activator ta sẽ phải trỏ vào tận link tuyệt đối của
    file chứa activator.)

- How to use :
  + activator new => create a play project  (rails new)
  + activator run => run a project Play     (rails server)
  + activator shell  => interact with project by terminal (rails console gọi irb)  
    `activator shell` gọi `sbt`

###*IV - Scala with Redis*
####*Khái niệm basic*
- Redis là một app open-source cho việc lưu trữ dữ liệu
- Redis lưu dữ liệu dưới dạng Key - Value
- Các kiểu dự liệu hỗ trợ :
  + strings
  + hashes
  + lists
  + sets
  + sorted sets
  + bitmaps
  + and hyperloglogs.
- Các ngôn ngữ Redis support :
  + ActionScript  
  + C C#  C++  Clojure  Common
  + Lisp D  Dart  
  + Elixir  emacs lisp  Erlang  
  + Fancy GNU Prolog  Go  Haskell  Haxe  Io  
  + Java Lua  Matlab  
  + Nim  Node.js  
  + Objective-C  OCaml
  + Perl  PHP  Pure
  + Data  
  + Python
  + R  Rebol Ruby  Rust
  + Scala  Scheme  Smalltalk  
  + Tcl VCL
- Hệ thống được recommend để setup Redis là Linux:
  `we recommend using Linux for deploying.There is no official support for Windows builds`

- Redis Replication:
  + là một khái niệm để mô tả việc Redis server cho phép copies một server giống hệt nó.
  + Redis có một primary server là `master server` nhưng nó hỗ trợ việc nhân rộng server, nghĩa là coppy một server khác giống như master server
  + Redis sử dụng cơ chế (`asynchronous replication`) nghĩa là nhân rộng bất đồng bộ
  + Một Redis master server có thể có nhiều bản copy.

- Vấn đề đồng bộ :
  + asynchronous I/O or non-blocking I/O (bất đồng bộ) : Cho phép những tiến trình hoạt động song song
  + synchronous I/O or blocking I/O  (đồng bộ)  : các tiến trình chạy tuần tự

  => Redis hỗ trợ `master-slave asynchronous replication`:
    + nghĩa là `master-slave` từ một master(server chính) ta có thể copy ra thành nhiều `slave`(nô lệ)
    + và `asynchronous` chính là non-blocking : EM không hiểu đoạn này // TODO
####*Features*

#####*Redis client*
- Một `Redis server` sẽ có rất nhiều `Redis client` trỏ tới (access) để query data
- Mỗi `Client` kết nối tới `Redis server` sẽ được Redis server tạo ra một ID và gán cho client đó
- Thông tin của một Redis client sẽ được server lưu như sau:

```shell
id=2 addr=127.0.0.1:43826 fd=7 name= age=7159 idle=114 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=0 obl=0 oll=0 omem=0 events=r cmd=client
id=6 addr=127.0.0.1:45264 fd=8 name= age=5 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=0 oll=0 omem=0 events=r cmd=client
```
=> Có 2 client đang kết nối vào server với thông số như trên

- Để kiểm tra xem có bao nhiêu client đang kết nối vào server ta có thể sử dụng câu lệnh (Dùng ở phía client)
- http://redis.io/commands/client-list
```shell
 CLIENT LIST
```

#####*Pipelining: Learn how to send multiple commands at once, saving on round trip time.*
- Cho phép gửi nhiều commands lên server cùng một lúc
- Khi gửi đồng thời các commands lên server thì server sẽ thực thi lần lượt từng câu lệnh và lưu lại kết quả trả về vào `queue`
sau đó kết quả trong queue sẽ được gửi về client.

#####*Pub/Sub*
- Cơ chế cho pháp tạo ra một `channel`, và những client có thể `SUBSCRIBE, UNSUBSCRIBE and PUBLISH` với một channel nào đó.

*SUBSCRIBE :*
- Khi Subcribe một channel nghĩa là ta đã đăng kí nhận dữ liệu khi có một client nào đó gưi dữ liệu vào channel đó
- Có thể Subcribe một hay nhiều channel,  cú pháp : `SUBSCRIBE channels_name`
  ```shell
  SUBSCRIBE foo bar
  ```

*PUBLISH*

- Client Post message vào một channel đã tồn tại.
- Cú pháp : `PUBLISH channel_name message`
```scala
PUBLISH second Hello
```

*Unsubscribes*
- Unsubscribes một channel

#####*Transactions Redis*
- MULTI, EXEC, DISCARD and WATCH là những thành phần của Transcation trong Redis
- Một Transcation là bao gồm tập hợ các câu lệnh được gói chung để thực hiện 1 cách tuần tự
- Khi một client gọi tới một Transcation thì lập tức Transcation sẽ bị khóa lại và nếu 1 client B khác cũng gọi đến Transcation đó
  khi mà nó đang chạy bởi CLient A thì Client B sẽ không thể thực hiện transcation đó
- Nếu bất kì một câu lệnh nào trong Transaction trả về Fails thì cả transcation sẽ thưcj hiện việc callback (hoàn trả lại tất cả những gì đã thực hiện)

#####*Cheat SHEET*
- http://www.cheatography.com/tasjaevan/cheat-sheets/redis/

#####*Data types*

*Strings*
- A String value can be at max 512 Megabytes in length. (Tối đa 512 MB trong một String Redis)
- String trong Redis là kiểu dữ liệu có thể chứa bất kì một kiểu dữ liệu nào
  + String có thể chứa một tấm ảnh JPEG
  + String có thể chứa mộ Object Ruby đã được serialized

*Lists* (SORTED)
- List của String , sorted by insertion order
- The max length of a list is 232 - 1 elements (4294967295, more than 4 billion of elements per list).

*Sets* (NO SORT) (UNIQUE)
- Redis Sets are an unordered collection of Strings.
- Không cho phép chứa 2 phần tử trùng nhau (nếu add một phần tử đã tồn tại thì sẽ k add được)
*Hashes*
- Redis Hashes are maps between string fields and string values, so they are the perfect data type to represent objects
- Every hash can store up to 232 - 1 field-value pairs (more than 4 billion).

*Sorted sets*(UNIQUE)(score)
- Redis Sorted Sets are, similarly to Redis Sets, non repeating collections of Strings
- Có một khái niệm là score để sort các phần tử.
