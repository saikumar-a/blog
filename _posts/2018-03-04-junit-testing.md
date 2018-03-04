---
layout: post
---
Recently, I have started working on Apache Camel project. Here, I came to know about new way of testing in java.
<br><br>
Before to that, let see the way of printing json from POJO.
```java
ObjectMapper mapper = new ObjectMapper(); //Jackson library 
mapper.setSerializationInclusion(Include.NON_EMPTY); //optional
mapper.setDateFormat(new SimpleDateFormat("yyyy-MM-dd"));//optional
System.out.println(mapper.writeValueAsString(source));//Now copy the string from console and save it as source.json
```
It follows like this.
1.  Get the source object from json file.
2.  Call the actual method with source object  which returns target object.
3.  Get the expected target object from json file.
4.  Now compare actual and expected target objects with Assertj function.

```java
//Step 1
ObjectMapper mapper = new ObjectMapper(); 
mapper.setPropertyNamingStrategy(PropertyNamingStrategy.SNAKE_CASE); // This is optional
Source source = mapper.readValue(new File("./src/test/resources/source.json"), Source.class);
//Step 2
Mappings mapper=new Mappings();
Target target=mapper.map(source);
//Step 3
ObjectMapper mapper2 = new ObjectMapper();
Target targetExpected = mapper2.readValue(new File("./src/test/resources/target.json"), Target.class);
//Step 4
Assertions.assertThat(target).isEqualToComparingFieldByFieldRecursively(targetExpected);//compares each value in objects
```
