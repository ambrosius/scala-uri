# scala-uri

[![Build Status](https://secure.travis-ci.org/theon/scala-uri.png?branch=master)](https://travis-ci.org/theon/scala-uri)

`scala-uri` is a small self contained Scala library that helps you work with URIs. It has the following features:

 * A DSL for building URIs
 * A parser to parse URIs from Strings.
 * Can be used outside a servlet environment as it has zero dependencies on the servlet spec or existing web frameworks.

## Building URIs with the DSL

```scala
import com.github.theon.uri.Uri._
val uri = "http://theon.github.com/scala-uri" ? ("p1" -> "one") & ("p2" -> 2) & ("p3" -> true)

uri.toString //This is: http://theon.github.com/scala-uri?p1=one&p2=2&p3=true
```

By importing `com.github.theon.uri.Uri._`, Strings can be _implicitly_ converted to URIs.

To add query string parameters, use either the `?` or `&` method and pass a `Tuple2` as an argument. The first value in the Tuple is a name of the query string parameter, the second is the value. If a parameter value is an `Option`, it will only be rendered provided it is not `None`.

To _explicitly_ create a `Uri`, you can use the following constructors:

```scala
val uri = Uri("http", "theon.github.com", "/scala-uri") //Absolute URI
val uri = Uri("/scala-uri") //Relative URI
```

## Parsing URIs

You can parse URIs as Strings into a `Uri` instance like so:

```scala
import com.github.theon.uri.Uri._
val uri = parseUri("http://theon.github.com/scala-uri?param1=1&param2=2")
```

## Options and Query String Parameters

You can specify an `Option` as the value of a query string parameter. It will get rendered if it is `Some(x)` and won't get rendered if it is `None`

```scala
import com.github.theon.uri.Uri._
val uri = "http://theon.github.com/scala-uri" ? ("param1" -> Some("1")) & ("param2" -> None)

uri.toString //This is: http://theon.github.com/scala-uri?param1=1
```

## URL Percent Encoding

By Default, `scala-uri` will URL percent encode paths and query string parameters. To prevent this, you can call the `uri.toStringRaw` method:

```scala
val uri = "http://example.com/path with space" ? ("param" -> "üri")

uri.toString //This is: http://example.com/path%20with%20space?param=%C3%BCri

uri.toStringRaw //This is: http://example.com/path with space?param=üri
```

## Replacing Query String Parameters

The `?` and `&` methods can be used to add query string parameters, however if you wish to replace all existing query string parameters with the same name, you can use the `uri.replace()` method:

```scala
val uri = "http://example.com/path" ? ("param" -> "1")
uri.replace("param", "2")

uri.toString //This is: http://example.com/path?param=2
```

## Get query string parameters

To get the query string parameters as a `Map[String,List[String]]` you can do the following:

```scala
val uri = "http://example.com/path" ? ("param" -> "1") & ("param2" -> 2)
uri.query.params //This is: Map(param -> List(1), param2 -> List(2))
```

## Including scala-uri your SBT project

Add the following dependency:

```scala
"com.github.theon" %% "scala-uri" % "0.1"
```

### Latest snapshot builds

Add the Sonatype OSS repo to your SBT build configuration:

```scala
resolvers += "Sonatype OSS" at "http://oss.sonatype.org/content/public"
```

Add the following dependency:

```scala
"com.github.theon" %% "scala-uri" % "0.2-SNAPSHOT"
```

# License

`scala-uri` is open source software released under the [Apache 2 License](http://www.apache.org/licenses/LICENSE-2.0).