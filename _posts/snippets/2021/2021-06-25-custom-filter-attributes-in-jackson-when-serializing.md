---
layout: post
title: Custom filter attributes in Jackson when serializing
description: "Code snippets showing how to filter attributes of an object with Jackson when serializing."
author: ama
permalink: /snippets/60d5c35b4095c2046612ee26/custom-filter-attributes-in-jackson-when-serializing
published: true
snippetId: 60d5c35b4095c2046612ee26
categories: [snippets]
tags: [java, jackson, json, serialization, codever-snippets]
---

First use the `@JsonFilter` jackson annotation where you define an **id** of the filter

```java
import com.fasterxml.jackson.annotation.JsonFilter;
import com.fasterxml.jackson.annotation.JsonProperty;

@JsonFilter("IndividualComparisonResultsFilter")
public class ComparisonResultBulk implements Serializable {

  //other fields ignored for brevity

  @JsonProperty("max_latency_millis")
  private String maxLatencyMillis;

  @JsonProperty("individual_comparison_results")
  private List<ComparisonResult> comparisonResults;

  @JsonProperty("not_comparable_results_size")
  private Integer notComparableResultsSize;

  @JsonProperty("not_comparable_results")
  private List<ComparisonResult> notComparableResults;
}
```

Then create a `filterProvider`, for example a `SimpleFilterProvider` for the **id** where you set the filter options.

In the example below everything is serialized except two properties with the help of `SimpleBeanPropertyFilter.serializeAllExcept()` method (`serializeAll()` is also present, and the opposite `SimpleBeanPropertyFilter.filterOutAllExcept("individual_comparison_results", "not_comparable_results")` which would serialize **only** these properties).  In the end set the `filterProvider`  set to the `ObjectMapper`  where you serialize your object:

```java
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.ser.impl.SimpleBeanPropertyFilter;
import com.fasterxml.jackson.databind.ser.impl.SimpleFilterProvider;

private ComparisonReport generateReport(ComparisonResultBulk comparisonResultBulk)
      throws JsonProcessingException {
    ComparisonReport comparisonReport = new ComparisonReport();

    ObjectMapper mapper = new ObjectMapper();
    SimpleFilterProvider filterProvider = new SimpleFilterProvider();
    filterProvider.addFilter("IndividualComparisonResultsFilter", SimpleBeanPropertyFilter.serializeAllExcept("individual_comparison_results", "not_comparable_results"));
    mapper.setFilterProvider(filterProvider);

    comparisonReport.setPayload(mapper.writeValueAsBytes(comparisonResultBulk));

    return comparisonReport;
}
```

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="60d5c35b4095c2046612ee26" %}
