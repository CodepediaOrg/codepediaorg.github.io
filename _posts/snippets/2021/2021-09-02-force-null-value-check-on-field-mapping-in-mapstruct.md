---
layout: post
title: Force null value check on field mapping in Mapstruct
description: "Force null value check on field mapping in Mapstruct code snippet"
author: ama
permalink: /snippets/612f7432d578230b425d41a5/force-null-value-check-on-field-mapping-in-mapstruct
published: true
snippetId: 612f7432d578230b425d41a5
categories: [snippets]
tags: [java, mapstruct, mapping, codever-snippets]
---

Use the `nullValueCheckStrategy = NullValueCheckStrategy.ALWAYS)` on the mapping property you want to be checked.
 In this snippet you can see a `PostalAdress` entity is mapped to an entity of the same type, but the `id` is overwritten only when it is present in the `source`:


```java
import java.time.LocalDateTime;

import org.mapstruct.Mapper;
import org.mapstruct.Mapping;
import org.mapstruct.NullValueCheckStrategy;

import com.example.entity.PostalAddress;

@Mapper(
    componentModel = "cdi",
    imports = {LocalDateTime.class})
public interface PostalAddressEntity2EntityMapperService {

  @Mapping(source = "id", target = "id", nullValueCheckStrategy = NullValueCheckStrategy.ALWAYS)
  PostalAddress toPostalAddressntityToPostalAddressEntity(
      PostalAddress PostalAddress);
}
```

This will generate something similar with the following

```java
package com.example.entitytoentity;

import com.example.entity.PostalAddress;
import java.time.LocalDateTime;
import javax.annotation.processing.Generated;
import javax.enterprise.context.ApplicationScoped;

@Generated(
    value = "org.mapstruct.ap.MappingProcessor",
    date = "2021-09-01T14:28:11+0200",
    comments = "version: 1.4.2.Final, compiler: javac, environment: Java 11.0.12 (Azul Systems, Inc.)"
)
@ApplicationScoped
public class PostalAddressEntity2EntityMapperServiceImpl implements PostalAddressEntity2EntityMapperService {

    @Override
    public PostalAddress toPostalAddressEntity(PostalAddress PostalAddress) {
        if ( PostalAddress == null ) {
            return null;
        }

        PostalAddress PostalAddress1 = new PostalAddress();

        if ( PostalAddress.getId() != null ) {
            PostalAddress1.setId( PostalAddress.getId() );
        }
        PostalAddress1.setStreeNo( PostalAddress.getStreetNo() );
        PostalAddress1.setStreet( PostalAddress.getStreet() );
        PostalAddress1.setMainAdress( PostalAddress.getMainAdress() );

        return PostalAddress1;
    }
}

```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://mapstruct.org/documentation/stable/api/org/mapstruct/NullValueCheckStrategy.html" target="_blank" style="font-weight: lighter">
     https://mapstruct.org/documentation/stable/api/org/mapstruct/NullValueCheckStrategy.html
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="612f7432d578230b425d41a5" %}
