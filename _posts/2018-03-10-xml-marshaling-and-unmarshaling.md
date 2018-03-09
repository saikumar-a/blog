---
layout: post
---
At the time of testing you may required to convert XML string to POJO and vice versa.<br>
After so many searches, only the below way worked for me for XML marshaling and unmarshaling without root element.
<br>
```java
//marshal
JAXBContext jaxbContext = JAXBContext.newInstance(Source.class);//JAXB library
Marshaller jaxbMarshaller = jaxbContext.createMarshaller();
jaxbMarshaller.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
ObjectFactory factory=new ObjectFactory();//Generated class from XSD/WSDL file
JAXBElement<Source> jaxbElement=factory.createSource(sourceObj);        
jaxbMarshaller.marshal(jaxbElement, System.out);

//unmarshal
JAXBContext jaxbContext = JAXBContext.newInstance(Source.class);
XMLStreamReader xmlStreamReader=XMLInputFactory.newInstance().createXMLStreamReader(new FileReader("./src/test/resources/source.xml"));
Unmarshaller jaxbUnmarshaller = jaxbContext.createUnmarshaller();
JAXBElement<Source> jAXBElement = (JAXBElement<Source>) jaxbUnmarshaller.unmarshal(xmlStreamReader, Source.class);
Source sourceObj = jAXBElement.getValue();
```
