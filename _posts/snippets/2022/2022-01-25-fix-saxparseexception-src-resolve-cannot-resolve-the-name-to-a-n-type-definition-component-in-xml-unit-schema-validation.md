---
layout: post
title: "Fix SAXParseException; src-resolve: Cannot resolve the name '…' to a(n) 'type definition' component in Xml unit schema validation"
description: "Fix SAXParseException; src-resolve: Cannot resolve the name '…' to a(n) 'type definition' component in Xml unit schema validation code snippet"
author: ama
permalink: /snippets/606d9f08f442e014739a56f5/fix-saxparseexception-src-resolve-cannot-resolve-the-name-to-a-n-type-definition-component-in-xml-unit-schema-validation
published: true
snippetId: 606d9f08f442e014739a56f5
categories: [snippets]
tags: [java, testing, xml, xmlunit, codever-snippets]
---

**TLDR;** - include also the imported xsd file in the schema sources in xml unit validator

When trying to validate an xml document against an `xsd` schema with **xmlunit** I get the following exception

```shell
org.xmlunit.XMLUnitException: The schema is invalid

	at org.xmlunit.validation.JAXPValidator.validateInstance(JAXPValidator.java:81)
	at ch.mobi.siefs.connector.helper.XmlUnitComparisonTest.givenXml_whenValidatesAgainstXsd_thenCorrect(XmlUnitComparisonTest.java:48)
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
.....
Caused by: org.xml.sax.SAXParseException; lineNumber: 89; columnNumber: 98; src-resolve: Cannot resolve the name 'mobits:HistoryTimestamp' to a(n) 'type definition' component.
	at java.xml/com.sun.org.apache.xerces.internal.util.ErrorHandlerWrapper.createSAXParseException(ErrorHandlerWrapper.java:204)
	at java.xml/com.sun.org.apache.xerces.internal.util.ErrorHandlerWrapper.error(ErrorHandlerWrapper.java:135)
....
```

The unit test looked like the following:

```java
@Test
public void givenXml_whenValidatesAgainstXsd_thenCorrect() {
  Validator v = Validator.forLanguage(Languages.W3C_XML_SCHEMA_NS_URI);
  v.setSchemaSource(Input.fromStream(
          XmlUnitComparisonTest.class.getResourceAsStream("xsd/bookmark.xsd")).build());
  ValidationResult r = v.validateInstance(Input.fromStream(
          XmlUnitComparisonTest.class.getResourceAsStream("message/bookmark-example.xml")).build());
  Iterator<ValidationProblem> probs = r.getProblems().iterator();
  while (probs.hasNext()) {
    probs.next().toString();
  }
  Assertions.assertThat(r.isValid()).isTrue();
}

```

The problem element mentioned in the stack trace comes from the imported xsd, which xml wise everything seems ok:


```xml
<?xml version="1.0" encoding="utf-8" ?>
<xsd:schema
            xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:common="http://xml.bookmarks.dev/datatype/common/Commons/v3">
    <xsd:import namespace=""http://xml.bookmarks.dev/datatype/common/Commons/v3" schemaLocation="common-Commons-3.0.xsd"/>
```

The solution was to add also the imported `xsd` schema when building the validator in xmlunit

```java
  @Test
  public void givenXml_whenValidatesAgainstXsd_thenCorrect() {
    Validator v = Validator.forLanguage(Languages.W3C_XML_SCHEMA_NS_URI);
    v.setSchemaSources( // order of xsd sources is important
        Input.fromStream(
                getClass().getClassLoader().getResourceAsStream("xsd/bookmarks.xsd"))
            .build(),
        Input.fromStream(
                getClass().getClassLoader().getResourceAsStream("xsd/common-Commons-3.0.xsd"))
            .build());
    ValidationResult r =
        v.validateInstance(
            Input.fromStream(
                    getClass()
                        .getClassLoader()
                        .getResourceAsStream("message/bookmark-example.xml"))
                .build());
    Iterator<ValidationProblem> probs = r.getProblems().iterator();
    while (probs.hasNext()) {
      probs.next().toString();
    }
    assertThat(r.isValid()).isTrue();
  }
```

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="606d9f08f442e014739a56f5" %}
