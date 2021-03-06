![MIT license](http://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat)
![Build Status](https://travis-ci.org/benas/jcql.svg?branch=master)
![Development Stage](https://img.shields.io/badge/development%20stage-alpha-orange.svg)

**Important note: This project is in early stage and not released yet. The project name may change.
 You can still [give it a try](https://github.com/benas/jcql#getting-started) and send us your feedback, you are very welcome!**

# About JCQL

JCQL (Java Code Query Language) makes it possible to query Java source code in plain SQL. This allows you to ask questions like:

* Is there any class that has no unit tests?
* What is the longest class name?
* Is there any class that has more than 20 methods?

This kind of static analysis gives you precious insights in order to improve the quality of your code base.
JCQL tools can be easily integrated in your build process to continuously check if your coding rules are respected.

# How does it work

JCQL tools create a relational model from a given Java source code base. Once you have created the relational model, you can
query it with standard SQL. The relational model has been designed to be intuitive, natural and easy to understand and query.
Here is a quick overview of the core tables:

![ER diagram](https://raw.githubusercontent.com/benas/jcql/master/jcql-erd.png)

Here are some examples of queries:

###### Find the top 10 longest class names:

```sql
select NAME, LENGTH(NAME) as length from CLASS order by length desc limit 10
```

###### Find the top 10 longest method names:

```sql
select NAME, LENGTH(NAME) as length from METHOD order by length desc limit 10
```

# Getting started

JCQL tools are in early stage and are not released yet. You can still try the first snapshot version:

* jcql-core: [download jar](https://oss.sonatype.org/content/repositories/snapshots/io/github/benas/jcql-core/0.1-SNAPSHOT/jcql-core-0.1-20160719.092316-1.jar)
* jcql-shell: [download jar](https://oss.sonatype.org/content/repositories/snapshots/io/github/benas/jcql-shell/0.1-SNAPSHOT/jcql-shell-0.1-20160719.092346-1.jar)

**Note: JCQL tools require Java 1.8+**

# Index your code base

JCQL tools provide two ways to create a relational database from a given code base:

#### Using maven

Add the following repository in your `pom.xml` :

```xml
<repository>
    <id>ossrh</id>
    <url>https://oss.sonatype.org/content/repositories/snapshots</url>
</repository>
```

and run the following command in the root folder of the project you want to analyse:

```
mvn io.github.benas:jcql-maven-plugin:0.1-SNAPSHOT:index
```

This will create a file named `jcql.db` in the `target` directory that you can
query with your favorite SQL client or the one provided by JCQL tools

#### Using java

Run the following command:

```
java -jar jcql-core-{latest-version}.jar /path/to/project/to/analyse /path/to/database/directory
```

# Query your Java code with SQL

Once you have indexed your Java code in a relational format, you can query it with plain SQL:

#### Using a SQL client

Just open the `jcql.db` file with your favorite SQL client and start writing queries. You can use [Sqlite tools](https://www.sqlite.org/download.html) or [Sqlite browser](http://sqlitebrowser.org/).

#### Using JCQL Shell

The JCQL shell is a command line tool that allows you to write queries against the generated database.

You can start it as follows:

```
java -jar /path/to/jcql-shell-{latest-version}.jar /path/to/jcql.db
```

# Credits

* JCQL tools use the great [java-parser](https://github.com/javaparser/javaparser) library to parse Java code and transform it a relational model.

* JCQL tools use [Sqlite](https://www.sqlite.org) to store and query relational data about Java code
