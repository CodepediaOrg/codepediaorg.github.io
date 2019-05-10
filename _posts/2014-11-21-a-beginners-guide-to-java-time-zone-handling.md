---
id: 2113
title: 'A beginner&#8217;s guide to java time zone handling'
date: 2014-11-21T15:52:20+00:00
author: Vlad Mihalcea
layout: post
guid: http://www.codingpedia.org/?p=2113
permalink: /vladmihalcea/a-beginners-guide-to-java-time-zone-handling/
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:75:"https://github.com/vladmihalcea/vladmihalcea.wordpress.com/tree/master/misc";s:11:"ribbon-type";i:10;}'
fsb_show_social:
  - 0
dsq_thread_id:
  - 3245471040
fsb_social_facebook:
  - 2
fsb_social_google:
  - 5
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - java
tags:
  - beginner
  - calendar
  - date
  - datetime
  - java8
  - joda
---
<h2 style="font-weight: inherit; color: #444444;">
  <span id="Basic_time_notions">Basic time notions</span>
</h2>

<p style="text-align: justify;">
  Most web applications have to support different time-zones and properly handling time-zones is no way easy. To make matters worse, you have to make sure that timestamps are consistent across various programming languages (e.g. JavaScript on the front-end, Java in the middleware and MongoDB as the data repository). This post aims to explain the basic notions of absolute and relative time.<!--more-->
</p>

<p style="text-align: justify;">
  <div id="toc_container" class="no_bullets">
    <p class="toc_title">
      Contents
    </p>
    
    <ul class="toc_list">
      <li>
        <a href="#Basic_time_notions">Basic time notions</a><ul>
          <li>
            <a href="#Epoch">Epoch</a>
          </li>
          <li>
            <a href="#Relative_numerical_timestamp">Relative numerical timestamp</a>
          </li>
          <li>
            <a href="#Time_zone">Time zone</a>
          </li>
          <li>
            <a href="#ISO_8601">ISO 8601</a>
          </li>
        </ul>
      </li>
      
      <li>
        <a href="#Java_time_basics">Java time basics</a><ul>
          <li>
            <a href="#javautilDate">java.util.Date</a>
          </li>
          <li>
            <a href="#javautilCalendar">java.util.Calendar</a>
          </li>
          <li>
            <a href="#orgjodatimeDateTime"> org.joda.time.DateTime</a>
          </li>
        </ul>
      </li>
      
      <li>
        <a href="#Relative_vs_Absolute_time_instances">Relative vs Absolute time instances</a><ul>
          <li>
            <a href="#Relative_timestamp">Relative timestamp</a>
          </li>
          <li>
            <a href="#Absolute_timestamp">Absolute timestamp</a>
          </li>
        </ul>
      </li>
      
      <li>
        <a href="#Puzzles">Puzzles</a><ul>
          <li>
            <a href="#javatextSimpleDateFormat">java.text.SimpleDateFormat</a><ul>
              <li>
                <a href="#Use_case_1">Use case 1</a>
              </li>
              <li>
                <a href="#Use_case_2">Use case 2</a>
              </li>
              <li>
                <a href="#Use_case_3">Use case 3</a>
              </li>
              <li>
                <a href="#Use_case_4">Use case 4</a>
              </li>
              <li>
                <a href="#Use_case_5">Use case 5</a>
              </li>
              <li>
                <a href="#Use_case_6">Use case 6</a>
              </li>
            </ul>
          </li>
          
          <li>
            <a href="#orgjodatimeDateTime-2">org.joda.time.DateTime</a>
          </li>
        </ul>
      </li>
    </ul>
  </div>
</p>

<h3 style="font-weight: inherit; color: #444444;">
  <span id="Epoch">Epoch</span>
</h3>

<p style="text-align: justify;">
  An <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://en.wikipedia.org/wiki/Epoch_%28reference_date%29">epoch</a> is a an absolute time reference. Most programming languages (e.g Java, JavaScript, Python) use the <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://en.wikipedia.org/wiki/Unix_time">Unix epoch (Midnight 1 January 1970)</a> when expressing a given timestamp as the number of milliseconds elapsed since a fixed point-in-time reference.
</p>

<h3 style="font-weight: inherit; color: #444444;">
  <span id="Relative_numerical_timestamp">Relative numerical timestamp</span>
</h3>

<p style="color: #444444;">
  The relative numerical timestamp is expressed as the number of milliseconds elapsed since epoch.
</p>

<h3 style="font-weight: inherit; color: #444444;">
  <span id="Time_zone">Time zone</span>
</h3>

The <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://en.wikipedia.org/wiki/Coordinated_Universal_Time">coordinated universal time (UTC)</a> is the most common time standard. The UTC time zone (equivalent to <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://en.wikipedia.org/wiki/Greenwich_Mean_Time">GMT</a>) represents the time reference all other time zones relate to (through a positive/negative offset).

UTC time zone is commonly refereed as Zulu time (Z) or UTC+0. Japan time zone is UTC+9 and Honolulu time zone is UTC-10. At the time of Unix epoch (1 January 1970 00:00 UTC time zone) it was 1 January 1970 09:00 in Tokyo and 31 December 1969 14:00 in Honolulu.

<h3 style="font-weight: inherit;">
  <span id="ISO_8601">ISO 8601</span>
</h3>

<a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://en.wikipedia.org/wiki/ISO_8601">ISO 8601</a> is the most widespread date/time representation standard and it uses the following date/time formats:

<table style="color: #444444;">
  <tr style="font-weight: inherit; font-style: inherit;">
    <th style="font-style: inherit;">
      TIME ZONE
    </th>
    
    <th style="font-style: inherit;">
      NOTATION
    </th>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      UTC
    </td>
    
    <td style="font-style: inherit;">
      1970-01-01T00:00:00.000+00:00
    </td>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      UTC Zulu time
    </td>
    
    <td style="font-style: inherit;">
      1970-01-01T00:00:00.000+Z
    </td>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      Tokio
    </td>
    
    <td style="font-style: inherit;">
      1970-01-01T00:00:00.000+09:00
    </td>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      Honolulu
    </td>
    
    <td style="font-style: inherit;">
      1969-12-31T14:00:00.000-10:00
    </td>
  </tr>
</table>

<h2 style="font-weight: inherit; color: #444444;">
  <span id="Java_time_basics">Java time basics</span>
</h2>

<h3 style="font-weight: inherit; color: #444444;">
  <span id="javautilDate">java.util.Date</span>
</h3>

<a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="https://docs.oracle.com/javase/8/docs/api/java/util/Date.html">java.util.Date</a> is definitely the most common time-related class. It represents a fixed point in time, expressed as the relative number of milliseconds elapsed since epoch. java.util.Date is <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://stackoverflow.com/questions/1516213/java-util-date-is-using-time%20zone">time zone independent</a>, except for the <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="https://docs.oracle.com/javase/8/docs/api/java/util/Date.html#toString--">toString</a> method which uses a the <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="https://docs.oracle.com/javase/8/docs/api/java/text/DateFormat.html">local time zone</a> for generating a String representation.

<h3 style="font-weight: inherit; color: #444444;">
  <span id="javautilCalendar">java.util.Calendar</span>
</h3>

<p style="color: #444444; text-align: justify;">
  The <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="https://docs.oracle.com/javase/8/docs/api/java/util/Calendar.html">java.util.Calendar</a> is both a Date/Time factory as well as a time zone aware timing instance. It’s one of the least user-friendly Java API class to work with and we can demonstrate this in the following example:
</p>

<pre class="lang:java decode:true ">@Test
public void testTimeZonesWithCalendar() throws ParseException {
    assertEquals(0L, newCalendarInstanceMillis("GMT").getTimeInMillis());
    assertEquals(TimeUnit.HOURS.toMillis(-9), newCalendarInstanceMillis("Japan").getTimeInMillis());
    assertEquals(TimeUnit.HOURS.toMillis(10), newCalendarInstanceMillis("Pacific/Honolulu").getTimeInMillis());
    Calendar epoch = newCalendarInstanceMillis("GMT");
    epoch.setTimeZone(TimeZone.getTimeZone("Japan"));
    assertEquals(TimeUnit.HOURS.toMillis(-9), epoch.getTimeInMillis());
}
 
private Calendar newCalendarInstance(String timeZoneId) {
    Calendar calendar = new GregorianCalendar();
    calendar.set(Calendar.YEAR, 1970);
    calendar.set(Calendar.MONTH, 0);
    calendar.set(Calendar.DAY_OF_MONTH, 1);
    calendar.set(Calendar.HOUR_OF_DAY, 0);
    calendar.set(Calendar.MINUTE, 0);
    calendar.set(Calendar.SECOND, 0);
    calendar.set(Calendar.MILLISECOND, 0);
    calendar.setTimeZone(TimeZone.getTimeZone(timeZoneId));
    return calendar;
}</pre>

At the time of Unix epoch (the UTC time zone), Tokyo time was nine hours ahead, while Honolulu was ten hours behind.

Changing a Calendar time zone preserves the actual time while shifting the zone offset. The relative timestamp changes along with the Calendar time zone offset.

<a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://www.joda.org/joda-time/">Joda-Time</a> and Java 8 Date Time API simply make <em style="font-weight: inherit;">java.util.Calandar</em> obsolete so you no longer have to employ this quirky API.

### <span id="orgjodatimeDateTime"> org.joda.time.DateTime</span>

<p style="color: #444444;">
  <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://www.joda.org/joda-time/">Joda-Time</a> aims to fix the legacy Date/Time API by offering:
</p>

<ul style="color: #444444;">
  <li style="font-weight: inherit; font-style: inherit;">
    both immutable and mutable date structures
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    a fluent API
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://joda-time.sourceforge.net/apidocs/org/joda/time/format/ISODateTimeFormat.html">better support for ISO 8601 standard</a>
  </li>
</ul>

<p style="color: #444444;">
  With Joda-Time this is how our previous test case looks like:
</p>

<pre class="lang:java decode:true">@Test
public void testTimeZonesWithDateTime() throws ParseException {
    assertEquals(0L, newDateTimeMillis("GMT").toDate().getTime());
    assertEquals(TimeUnit.HOURS.toMillis(-9), newDateTimeMillis("Japan").toDate().getTime());
    assertEquals(TimeUnit.HOURS.toMillis(10), newDateTimeMillis("Pacific/Honolulu").toDate().getTime());
    DateTime epoch = newDateTimeMillis("GMT");
    assertEquals("1970-01-01T00:00:00.000Z", epoch.toString());
    epoch = epoch.toDateTime(DateTimeZone.forID("Japan"));
    assertEquals(0, epoch.toDate().getTime());
    assertEquals("1970-01-01T09:00:00.000+09:00", epoch.toString());
    MutableDateTime mutableDateTime = epoch.toMutableDateTime();
    mutableDateTime.setChronology(ISOChronology.getInstance().withZone(DateTimeZone.forID("Japan")));
    assertEquals("1970-01-01T09:00:00.000+09:00", epoch.toString());
}
 
 
private DateTime newDateTimeMillis(String timeZoneId) {
    return new DateTime(DateTimeZone.forID(timeZoneId))
            .withYear(1970)
            .withMonthOfYear(1)
            .withDayOfMonth(1)
            .withTimeAtStartOfDay();
}</pre>

The <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://joda-time.sourceforge.net/apidocs/org/joda/time/DateTime.html">DateTime</a> fluent API is much easier to use than <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="https://docs.oracle.com/javase/8/docs/api/java/util/Calendar.html#set-int-int-">java.util.Calendar#set</a>. DateTime is immutable but we can easily switch to a <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://joda-time.sourceforge.net/apidocs/org/joda/time/MutableDateTime.html">MutableDateTime</a> if it’s appropriate for our current use case.

<p style="color: #444444;">
  Compared to our Calendar test case, when changing the time zone the relative timestamp doesn’t change a bit, therefore remaining the same original point in time.
</p>

<p style="color: #444444;">
  It’s only the human time perception that changes (<em style="font-weight: inherit;">1970-01-01T00:00:00.000Z</em> and<em style="font-weight: inherit;">1970-01-01T09:00:00.000+09:00</em> pointing to the very same absolute time).
</p>

<h2 style="font-weight: inherit; color: #444444;">
  <span id="Relative_vs_Absolute_time_instances">Relative vs Absolute time instances</span>
</h2>

<p style="color: #444444;">
  When supporting time zones, you basically have two main alternatives: a relative timestamp and an absolute time info.
</p>

<h3 style="font-weight: inherit; color: #444444;">
  <span id="Relative_timestamp">Relative timestamp</span>
</h3>

The numeric timestamp representation (the numbers of milliseconds since epoch) is a relative info. This value is given against the UTC epoch but you still need a time zone to properly represent the actual time on a particular region.

<p style="color: #444444;">
  Being a long value, it’s the most compact time representation and it’s ideal when exchanging huge amounts of data.
</p>

If you don’t know the original event time zone, you risk of displaying a timestamp against the current local time zone and this is not always desirable.

<h3 style="font-weight: inherit; color: #444444;">
  <span id="Absolute_timestamp">Absolute timestamp</span>
</h3>

The absolute timestamp contains both the relative time as well as the time zone info. It’s quite common to express timestamps in their ISO 8601 string representation.

Compared to the numerical form (a 64 bit long) the string representation is less compact and it might take up to 25 characters (200 bits in UTF-8 encoding).

The ISO 8601 is quite common in XML files because the <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://www.w3.org/TR/xmlschema-2/#isoformats">XML schema uses a lexical format inspired by the ISO 8601 standard</a>.

An absolute time representation is much more convenient when we want to reconstruct the time instance against the original time zone. An e-mail client might want to display the email creation date using the sender’s time zone, and this can only be achieved using absolute timestamps.

<h2 style="font-weight: inherit; color: #444444;">
  <span id="Puzzles">Puzzles</span>
</h2>

<p style="color: #444444;">
  The following exercise aims to demonstrate how difficult is to properly handle an ISO 8601 compliant date/time structure using the ancient <em style="font-weight: inherit;">java.text.DateFormat</em> utilities.
</p>

<h3 style="font-weight: inherit; color: #444444;">
  <span id="javatextSimpleDateFormat">java.text.SimpleDateFormat</span>
</h3>

<p style="color: #444444;">
  First we are going to test the <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html">java.text.SimpleDateFormat</a> parsing capabilities using the following test logic:
</p>

<pre class="lang:java decode:true ">/**
 * DateFormat parsing utility
 * @param pattern date/time pattern
 * @param dateTimeString date/time string value
 * @param expectedNumericTimestamp expected millis since epoch 
 */
private void dateFormatParse(String pattern, String dateTimeString, long expectedNumericTimestamp) {
    try {
        Date utcDate = new SimpleDateFormat(pattern).parse(dateTimeString);
        if(expectedNumericTimestamp != utcDate.getTime()) {
            LOGGER.warn("Pattern: {}, date: {} actual epoch {} while expected epoch: {}", new Object[]{pattern, dateTimeString, utcDate.getTime(), expectedNumericTimestamp});
        }
    } catch (ParseException e) {
        LOGGER.warn("Pattern: {}, date: {} threw {}", new Object[]{pattern, dateTimeString, e.getClass().getSimpleName()});
    }
}</pre>

<h4 style="font-weight: inherit; color: #444444;">
  <span id="Use_case_1">Use case 1</span>
</h4>

<p style="color: #444444;">
  Let’s see how various ISO 8601 patterns behave against this first parser:
</p>

<pre class="lang:java decode:true ">dateFormatParse("yyyy-MM-dd'T'HH:mm:ss.SSS'Z'", "1970-01-01T00:00:00.200Z", 200L);
</pre>

<span style="color: #444444;">Yielding the following outcome:</span>

<pre class="lang:sh decode:true ">Pattern: yyyy-MM-dd'T'HH:mm:ss.SSS'Z', date: 1970-01-01T00:00:00.200Z actual epoch -7199800 while expected epoch: 200
</pre>

This pattern is not ISO 8601 compliant. The single quote character is an escape sequence so the final <em style="font-weight: inherit;">‘Z’</em> symbol is not treated as a time directive (e.g. Zulu time). After parsing, we’ll simply get a local time zone Date reference.

<p style="color: #444444;">
  This test was run using my current system default <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://en.wikipedia.org/wiki/Europe/Athens">Europe/Athens</a> time zone, which as of writing this post, it’s two hours ahead of UTC.
</p>

#### <span id="Use_case_2">Use case 2</span>

<p style="color: #444444;">
  According to <em style="font-weight: inherit;">java.util.SimpleDateFormat</em> documentation the following pattern: <em style="font-weight: inherit;">yyyy-MM-dd’T’HH:mm:ss.SSSZ</em> should match an ISO 8601 date/time string value:
</p>

<pre class="lang:java decode:true ">dateFormatParse("yyyy-MM-dd'T'HH:mm:ss.SSSZ", "1970-01-01T00:00:00.200Z", 200L);
</pre>

<span style="color: #444444;">But instead we got the following exception:</span>

<pre class="lang:sh decode:true ">Pattern: yyyy-MM-dd'T'HH:mm:ss.SSSZ, date: 1970-01-01T00:00:00.200Z threw ParseException
</pre>

<span style="color: #444444;">So this pattern doesn’t seem to parse the Zulu time UTC string values.</span>

<h4 style="font-weight: inherit; color: #444444;">
  <span id="Use_case_3">Use case 3</span>
</h4>

<p style="color: #444444;">
  The following patterns works just fine for explicit offsets:
</p>

<pre class="lang:java decode:true">dateFormatParse("yyyy-MM-dd'T'HH:mm:ss.SSSZ", "1970-01-01T00:00:00.200+0000", 200L);
</pre>

<h4 style="font-weight: inherit; color: #444444;">
  <span id="Use_case_4">Use case 4</span>
</h4>

<p style="color: #444444;">
  This pattern is also compatible with other time zone offsets:
</p>

<pre class="lang:java decode:true ">dateFormatParse("yyyy-MM-dd'T'HH:mm:ss.SSSZ", "1970-01-01T00:00:00.200+0100", 200L - 1000 * 60 * 60);
</pre>

<h4 style="font-weight: inherit; color: #444444;">
  <span id="Use_case_5">Use case 5</span>
</h4>

<p style="color: #444444;">
  To match the Zulu time notation we need to use the following pattern:
</p>

<pre class="lang:java decode:true">dateFormatParse("yyyy-MM-dd'T'HH:mm:ss.SSSXXX", "1970-01-01T00:00:00.200Z", 200L);
</pre>

<h4 style="font-weight: inherit; color: #444444;">
  <span id="Use_case_6">Use case 6</span>
</h4>

<p style="color: #444444;">
  Unfortunately, this last pattern is not compatible with explicit time zone offsets:
</p>

<pre class="lang:java decode:true ">dateFormatParse("yyyy-MM-dd'T'HH:mm:ss.SSSXXX", "1970-01-01T00:00:00.200+0000", 200L);
</pre>

<span style="color: #444444;">Ending-up with the following exception:</span>

<pre class="lang:sh decode:true ">Pattern: yyyy-MM-dd'T'HH:mm:ss.SSSXXX, date: 1970-01-01T00:00:00.200+0000 threw ParseException
</pre>

<h3 style="font-weight: inherit; color: #444444;">
  <span id="orgjodatimeDateTime-2">org.joda.time.DateTime</span>
</h3>

<p style="color: #444444;">
  As opposed to <em style="font-weight: inherit;">java.text.SimpleDateFormat</em>, <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://www.joda.org/joda-time/">Joda-Time</a> is compatible with any ISO 8601 pattern. The following test case is going to be used for the upcoming test cases:
</p>

<pre class="lang:java decode:true ">/**
 * Joda-Time parsing utility
 * @param dateTimeString date/time string value
 * @param expectedNumericTimestamp expected millis since epoch
 */
private void jodaTimeParse(String dateTimeString, long expectedNumericTimestamp) {
    Date utcDate = DateTime.parse(dateTimeString).toDate();
    if(expectedNumericTimestamp != utcDate.getTime()) {
        LOGGER.warn("date: {} actual epoch {} while expected epoch: {}", new Object[]{dateTimeString, utcDate.getTime(), expectedNumericTimestamp});
    }
}</pre>

<span style="color: #444444;">Joda-Time is compatible with all standard ISO 8601 date/time formats:</span>

<pre class="lang:sh decode:true ">jodaTimeParse("1970-01-01T00:00:00.200Z", 200L);
jodaTimeParse("1970-01-01T00:00:00.200+0000", 200L);
jodaTimeParse("1970-01-01T00:00:00.200+0100", 200L - 1000 * 60 * 60);
</pre>

<p style="color: #444444;">
  As you can see, the ancient Java Date/Time utilities are not easy to work with. Joda-Time is a much better alternative, offering better time handling features.
</p>

<p style="color: #444444;">
  If you happen to work with Java 8, it’s worth switching to the <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://www.oracle.com/technetwork/articles/java/jf14-date-time-2125367.html">Java 8 Date/Time API</a>, being designed from scratch but very much <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://blog.joda.org/2009/11/why-jsr-310-isn-joda-time_4941.html">inspired by Joda-Time</a>.
</p>

<p style="color: #444444;">
  Code available on <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="https://github.com/vladmihalcea/vladmihalcea.wordpress.com/tree/master/misc">GitHub</a>.
</p>

<p class="note_normal">
  Published at Codingpedia.org with permission of Vlad Mihalcea &#8211; source <a title="http://vladmihalcea.com/2014/11/17/a-beginners-guide-to-java-time-zone-handling/" href="http://vladmihalcea.com/2014/11/17/a-beginners-guide-to-java-time-zone-handling/" target="_blank">A beginner&#8217;s guide to java time zone handling</a> from http://vladmihalcea.com/
</p>

<div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
  <img id="author_portrait" style="float: left; margin-right: 20px;" src="https://lh5.googleusercontent.com/-TE09duPdvbA/U1pkmDy2uSI/AAAAAAAACUM/0AVivijfro4/w896-h897-no/VladMihalcea.jpg" alt="Vlad Mihalcea" /> 
  
  <p id="about_author_header">
    <strong>Vlad Mihalcea</strong>
  </p>
  
  <div id="author_details" style="text-align: justify;">
    Software architect passionate about software integration, high scalability and concurrency challenges
  </div>
  
  <div id="follow_social" style="clear: both;">
    <div id="social_logos">
      <a class="icon-earth" href="http://vladmihalcea.com/" target="_blank"> </a> <a class="icon-googleplus" href="https://plus.google.com/102351970868518518557/posts" target="_blank"> </a> <a class="icon-twitter" href="https://twitter.com/vlad_mihalcea" target="_blank"> </a> <a class="icon-github" href="https://github.com/vladmihalcea" target="_blank"> </a> <a class="icon-linkedin" href="https://www.linkedin.com/pub/vlad-mihalcea/20/a59/580" target="_blank"> </a>
    </div>
    
    <div class="clear">
    </div>
  </div>
</div>
