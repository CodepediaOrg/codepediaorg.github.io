---
layout: post
title: Unmarshalling xml string to java object with JAXB
description: "Unmarshalling xml string to java object with JAXB code snippet"
author: ama
permalink: /snippets/620fb164e102f148ceba0188/unmarshalling-xml-string-to-java-object-with-jaxb
published: true
snippetId: 620fb164e102f148ceba0188
categories: [snippets]
tags: [java, xml, jaxb, codever-snippets]
---

Given the following `Superhero` class:

```java
import javax.xml.bind.annotation.XmlAccessType;
import javax.xml.bind.annotation.XmlAccessorType;
import javax.xml.bind.annotation.XmlElement;
import javax.xml.bind.annotation.XmlRootElement;

import lombok.Getter;
import lombok.Setter;

@XmlRootElement(name = "super-hero")
@XmlAccessorType(XmlAccessType.FIELD)
@Getter
@Setter
class SuperHero {
  @XmlElement(name = "name")
  String name;

  @XmlElement(name = "super-power")
  String superPower;
}
```

You can convert an XML String to a `SuperHero` instance with the following code:


```java
import static org.junit.jupiter.api.Assertions.assertEquals;

import java.io.StringReader;

import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBException;
import javax.xml.bind.Unmarshaller;

import org.junit.jupiter.api.Test;
import org.junit.platform.runner.JUnitPlatform;
import org.junit.runner.RunWith;

@RunWith(JUnitPlatform.class)
class TestXmlStringToObjectUnmarshalling {

  static final String superHeroXml =
      "<?xml version=\"1.0\" encoding=\"UTF-8\" standalone=\"yes\"?>"
          + "<super-hero>"
          + "  <name>Superman</name>"
          + "  <super-power>Flight</super-power>"
          + "</super-hero>";

  @Test
  void testXmlUnmarshalling() throws JAXBException {
    JAXBContext jaxbContext = JAXBContext.newInstance(SuperHero.class);
    Unmarshaller unmarshaller = jaxbContext.createUnmarshaller();

    StringReader reader = new StringReader(superHeroXml);
    SuperHero superHero = (SuperHero) unmarshaller.unmarshal(reader);

    assertEquals("Flight", superHero.getSuperPower());
  }
}

```

**Note**:
- create a `JAXBContext`  which includes the `SuperHero` class - `JAXBContext.newInstance(SuperHero.class)`
- create a JAXB `Unmarshaller` and apply the `unmarshal` method to a `StringReader` wrapping the text (it could be also a `FileReader` or any other `Reader` for that matter)



<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="620fb164e102f148ceba0188" %} 
