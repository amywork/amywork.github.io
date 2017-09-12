---
layout: post
title: "용어 정리 02. Backend"
author: "younari"
---

> 서버, 데이터 베이스, 네트워크와 관련한 기초 용어들을 찾아보고 정리하는 노트입니다.

# Data

### Database(DB)
- A database is an organized collection of data. 
- It is a collection of schemas, tables, queries, reports, views, and other objects. 


### RDB
- A relational database is a digital database whose organization is based on the relational model of data. The various software systems used to maintain relational databases are known as a relational database management system (RDBMS). Virtually all relational database systems use SQL (Structured Query Language) as the language for querying and maintaining the database.


### DBMS
- A database-management system (DBMS) is a computer-software application that interacts with end-users, other applications, and the database itself to capture and analyze data. 
- A general-purpose DBMS allows the definition, creation, querying, update, and administration of databases. 
- Well-known DBMSs include MySQL, PostgreSQL, MongoDB, MariaDB, Microsoft SQL Server, Oracle, Sybase, SAP HANA, MemSQL, SQLite and IBM DB2.


### SQL - Structured Query Language
- the language for querying and maintaining the database
- A domain-specific language used in programming and designed for managing data held in a relational database management system (RDBMS)


### NoSQL
- https://namu.wiki/w/NoSQL
- NoSQL databases are increasingly used in big data and real-time web applications.
- 웹 2.0 환경과 빅데이터가 등장하면서 RDBMS는 난관에 부딪히게 되었는데, 바로 ‘데이터를 처리하는 데 필요한 비용의 증가’때문이다. 데이터와 트래픽의 양이 기하급수적으로 증가함에 따라 한 대에서 실행되도록 설계된 관계형 데이터베이스를 사용하는 것은 하드웨어적으로 큰 비용이 들게 되었다. 장비의 성능이 좋을수록, 성능을 향상시키는 데(Scale-up : 수직적 확장) 비용이 기하급수적으로 증가하기 때문이다.
- 분산 저장을 지원하는 NoSQL 데이터베이스의 경우, 집합-지향(Aggregate-oriented) 모델을 사용하여 이러한 문제를 해결한다. 연관된 데이터들이 함께 분산되므로, 관계형 모델에서처럼 복잡한 제어가 필요하지 않게 된다.
- 예를 들어 구매 내역이나 게임의 로그 같은 데이터들은 매 초마다 엄청난 양이 생성되지만 한번 저장되고 난 뒤에는 수정될 일이 거의 없다. 이런 데이터들을 저장하는 데 데이터의 일관성을 보장하기 위해 ACID 트랜잭션을 지원할 필요는 없을 것이다. 거기다 생성되는 데이터의 양도 많기 때문에 장비의 성능에도 상당한 영향을 미칠 것이다. NoSQL은 이러한 데이터들을 효율적으로 저장할 수 있다. 여러 대의 장비에 빠른 속도로 저장이 가능하며, 데이터의 양이 누적되더라도 얼마든지 수평적 확장이 가능하기 때문이다.
- 실제로 페이스북이나 트위터같은 소셜 네트워크 서비스에서는 게시글들을 저장하는 데 NoSQL 데이터베이스를 사용하고 있다. 매 초에 수백 기가~ 수 테라바이트씩 생성되는 데이터들을 RDBMS를 사용해 저장한다면, 글 작성 버튼을 누른 후 글이 중앙 데이터베이스에 저장되기까지 한참을 기다려야 글을 성공적으로 게시할 수 있을 것이다. 하지만 NoSQL의 분산 데이터베이스를 사용한다면 부하가 분산되기 때문에 우리가 글쓰기 버튼을 누르고 한참을 기다릴 필요가 없게 된다.
- 하지만 데이터의 일관성이 보장되어야 하거나 여러번의 조인 연산이 필요한 데이터라면 NoSQL을 사용하는 것 보다 RDBMS를 사용하는 것이 좋을 것이다. NoSQL은 RDBMS를 대체하기 위한 데이터베이스가 아니라 상호 보완할 수 있는 데이터베이스이며, 따라서 목적에 맞게 사용하는 것이 중요하다.
- C++, Java, Python 등 여러 언어를 사용해서 하나의 프로그램을 만들 수 있는 것처럼(Polyglot Programming) 데이터베이스 역시 다양한 저장소(Polyglot Persistance)를 사용할 수 있게 되었다는 점에서 NoSQL의 의의는 크다고 볼 수 있다. 


# A RESTful API

In fact, this client/server relationship is a prerequisite of a set of principles called REST (or Representational State Transfer ). This sounds kind of scary, but it's super easy—let's walk through it together.

Remember how we said HTTP involves sending hypertext (text with links)? Whenever you navigate through a site by clicking links, you're making a state transition, which brings you to the next page (representing the next state of the application). That's it!

An API, or application programming interface, is kind of like a coding contract: it specifies the ways a program can interact with an application. For example, if you want to write a program that reads and analyzes data from Twitter, you'd need to use the Twitter API, which would specify the process for authentication, important URLs, classes, methods, and so on.

For an API or web service to be RESTful, it must do the following:

- Separate the client from the server
- Not hold state between requests (meaning that all the information necessary to respond to a request is available in each individual request; no data, or state, is held by the server from request to request)
- Use HTTP and HTTP methods (as explained in the next section).

The number of HTTP methods you'll use is quite small—there are just four HTTP "verbs" you'll need to know! 

They are:

- GET: retrieves information from the specified source (you just saw this one!)
- POST: sends new information to the specified source.
- PUT: updates existing information of the specified source.
- DELETE: removes existing information from the specified source.


# Ajax 

Asynchronous (비동기통신) JavaScript And XML.
AJAX is not a programming language. AJAX just uses a combination of:

- A browser built-in XMLHttpRequest object (to request data from a web server)
- JavaScript and HTML DOM (to display or use the data)

AJAX allows web pages to be updated asynchronously by exchanging data with a web server behind the scenes. This means that it is possible to update parts of a web page, without reloading the whole page.



# Parser
- A program or part of a program that interprets input to a computer by recognizing key words or analysing sentencestructure. (syntax analyzer)

### Parsing XML : Exchanging Information

XML (which stands for Extensible Markup Language) is very similar to HTML—it uses tags between angle brackets. The difference is that XML allows you to use tags that you make up, rather than tags that the W3C decided on. For instance, you could create an API that returns information about a pet:

```
  <pet>
    <name>Jeffrey</name>
    <species>Giraffe</species>
  </pet>
```

As long as you document the structure of your API's response, other people can use your API to get information about <pets>.


### Parsing JSON
- JSON (JavaScript Object Notation) is a textual data interchange format and language-independent.
- JSON is an alternative to XML. It gets its name from the fact that its data format resembles JavaScript objects, and it is often more succinct than the equivalent XML. For instance, our same Jeffrey would look like this in JSON:

```
    {
       "pets": {
       "name": "Jeffrey",
       "species": "Giraffe"
     }
    }
```


### CSV Files
- In computing, a comma-separated values (CSV) file stores tabular data (numbers and text) in plain text. Each line of the file is a data record. Each record consists of one or more fields, separated by commas. The use of the comma as a field separator is the source of the name for this file format.

- The CSV file format is not standardized. The basic idea of separating fields with a comma is clear, but that idea gets complicated when the field data may also contain commas or even embedded line-breaks. CSV implementations may not handle such field data, or they may use quotation marks to surround the field. Quotation does not solve everything: some fields may need embedded quotation marks, so a CSV implementation may include escape characters or escape sequences.

- In addition, the term "CSV" also denotes some closely related delimiter-separated formats that use different field delimiters. These include tab-separated values and space-separated values. A delimiter that is not present in the field data (such as tab) keeps the format parsing simple. These alternate delimiter-separated files are often even given a .csv extension despite the use of a non-comma field separator. This loose terminology can cause problems in data exchange. Many applications that accept CSV files have options to select the delimiter character and the quotation character.


