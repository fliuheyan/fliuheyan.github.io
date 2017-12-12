# Argonaut

## Building JSON
* build json
just like the jackson

```
// JsonString
val jsonString: Json  = jString("JSON!")
// JsonNumber
val jsonNumber: Json       = jNumber(20)
// JsonArray
val jsonArray: Json =
    Json.array(jsonNumber, jsonString)
// JsonObject
val jsonObject: Json =
    Json("key1" -> jsonNumber, "key2" -> jsonString)
```
* combine json

```
//append
val appendedString: Json =
    jString("JSO").withString(_ + "N") // withString expect a JSON -> JSON lambda

// extract value from json, return value is Some(true)
val nestedObjectAccess: Option[Json] =
    jSingleObject("field",
      jSingleObject("nested", jTrue)) -|| List("field", "nested")

// remove json object, return value is {}
val modifiedObject: Json =
    jSingleObject("field", jTrue).withObject(_ - "field")      
```

## Traversal JSON

## Encode JSON
```
case class Person(name: String, age: Int)
implicit def PersonEncodeJson: EncodeJson[Person] =
    EncodeJson((p: Person) =>
      ("name" := p.name) ->: ("age" := p.age) ->: jEmptyObject)

      val json = PersonEncodeJson(Person("haha",21))
```


## Decode JSON

## Parsing String to JSON

## Pretty Printing
