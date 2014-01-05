# muster
======

A library for macro based serializers to many different formats.
It uses scala macros so no reflection is involved and it will generate code at compile time
that kind of looks like it would have been handwritten.  It is written with the idea of extension, so it's easy to
add your own formats.

The idea is that things work a little bit like this

```scala
case class Person(id: Long, name: String, age: Int)
val person = Person(1, "Luke", 38)
person.writeFormatted(Muster.into.ByteString)
person.writeFormatted(Muster.into.ByteBuffer)
person.writeFormatted(Muster.into.ShowFormat)
person.writeFormatted(Muster.into.CompactJsonString)
person.writeFormatted(Muster.into.PrettyJsonString)
person.writeFormatted(Muster.into.Protobuf[Protocol.Person])
```


Similarly reading can be achieved with

```scala
Muster.consume.Json.as[Person](/* file | string | reader | byte array | input stream | URL */ input)
Muster.consume.Protobuf[Protocol.Person].as[Person](/* file | string | reader | byte array | input stream | URL */ input)
Muster.consume.ByteString.as[Person](/* file | string | reader | byte array | input stream | URL */ input)
Muster.consume.ByteBuffer.as[Person](/* file | string | reader | byte array | input stream | URL */ input)
```

## What's inside

Currently muster supports JSON through parsing with jackson and it can extract the following things:
* Primitive values like String, Int, Date
* All scala collections
* Scala maps
* Java collections like java.util.List and java.util.Set
* Java maps (that implement java.util.Map)
* Java classes with bean getter/setters
* Scala classes with public vars
* Scala classes with java style getter/setter methods
* Scala case classes
* Classes initialized through a constructor only

Expected to be added next:
* Classes with type parameters
* Polymorphic classes/collections through a configurable type hint field and strategy
* Support for maps with different keys than String
* Support for streaming large collections through a scala iterator, java iterator or scala stream


