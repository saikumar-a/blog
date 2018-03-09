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
System.out.println(mapper.writeValueAsString(sourceObj));//Now copy the string from console and save it as source.json
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
Source sourceObj = mapper.readValue(new File("./src/test/resources/source.json"), Source.class);
//Step 2
Mappings mapper=new Mappings();
Target targetObj=mapper.map(sourceObj);
//Step 3
ObjectMapper mapper2 = new ObjectMapper();
Target targetObjExpected = mapper2.readValue(new File("./src/test/resources/target.json"), Target.class);
//Step 4
Assertions.assertThat(targetObj).isEqualToComparingFieldByFieldRecursively(targetObjExpected);//compares each value in objects
```
If you want to ignore  some of the auto generated fields, you can do it by the following way
```java
Assertions.assertThat(target).usingComparatorForFields((a,b)->0, "student.StudentName","field2").isEqualToComparingFieldByFieldRecursively(targetExpected);
```
