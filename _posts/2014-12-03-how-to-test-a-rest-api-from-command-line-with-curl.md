---
id: 2124
title: How to test a REST api from command line with curl
date: 2014-12-03T07:19:41+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=2124
permalink: /ama/how-to-test-a-rest-api-from-command-line-with-curl/
fsb_show_social:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:54:"https://github.com/Codingpedia/demo-rest-jersey-spring";s:11:"ribbon-type";i:10;}'
fsb_social_facebook:
  - 12
fsb_social_google:
  - 9
fsb_social_linkedin:
  - 17
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 3285553465
categories:
  - testing
tags:
  - curl
  - http
  - http header
  - http response
  - jersey
  - rest
  - rest api
  - test
  - testing
---
<p style="text-align: justify;">
  If you want to quickly test your REST api from the command line, you can use <a title="http://curl.haxx.se/" href="http://curl.haxx.se/" target="_blank">curl</a>. In this post I will present how to execute GET, POST, PUT, HEAD, DELETE HTTP Requests against a REST API. For the purpose of this blog post I will be using the REST api developed in my post <a title="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" href="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" target="_blank">Tutorial – REST API design and implementation in Java with Jersey and Spring</a>
</p>
<!--more-->

<p style="text-align: justify;">
  <div id="toc_container" class="no_bullets">
    <p class="toc_title">
      Contents
    </p>

    <ul class="toc_list">
      <li>
        <a href="#1_Introduction">1. Introduction</a><ul>
          <li>
            <a href="#11_What_is_curl">1.1. What is curl?</a>
          </li>
          <li>
            <a href="#12_HEAD_requests">1.2. HEAD requests</a>
          </li>
          <li>
            <a href="#13_GET_request">1.3. GET request</a>
          </li>
          <li>
            <a href="#14_Curl_request_with_multiple_headers">1.4. Curl request with multiple headers</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#2_SOAPui_test_suite_translated_to_curl_requests">2. SOAPui test suite translated to curl requests</a><ul>
          <li>
            <a href="#21_Create_podcasts_resource">2.1. Create podcast(s) resource</a><ul>
              <li>
                <a href="#211_Delete_all_podcasts_preparation_step">2.1.1. Delete all podcasts (preparation step)</a>
              </li>
              <li>
                <a href="#212_POST_new_podcast_without_feed_8211_400_BAD_REQUEST">2.1.2. POST new podcast without feed &#8211; 400 (BAD_REQUEST)</a>
              </li>
              <li>
                <a href="#213_POST_new_podcast_correctly_8211_201_CREATED">2.1.3. POST new podcast correctly &#8211; 201 (CREATED)</a>
              </li>
              <li>
                <a href="#214_POST_same_podcast_as_before_to_receive_8211_409_CONFLICT">2.1.4. POST same podcast as before to receive &#8211; 409 (CONFLICT)</a>
              </li>
              <li>
                <a href="#215_PUT_new_podcast_at_location_8211_201_CREATED">2.1.5. PUT new podcast at location &#8211; 201 (CREATED)</a>
              </li>
            </ul>
          </li>

          <li>
            <a href="#22_Read_podcast_resource">2.2. Read podcast resource</a><ul>
              <li>
                <a href="#221_GET_new_inserted_podcast_8211_200_OK">2.2.1. GET new inserted podcast &#8211; 200 (OK)</a>
              </li>
              <li>
                <a href="#222_GET_podcasts_sorted_by_insertion_date_DESC_8211_200_OK">2.2.2. GET podcasts sorted by insertion date DESC &#8211; 200 (OK)</a>
              </li>
            </ul>
          </li>

          <li>
            <a href="#23_Update_podcast_resource">2.3. Update podcast resource</a><ul>
              <li>
                <a href="#231_PUT_not_8220complete8221_podcast_for_FULL_update_8211_400_BAD_REQUEST">2.3.1. PUT not &#8220;complete&#8221; podcast for FULL update &#8211; 400 (BAD_REQUEST)</a>
              </li>
              <li>
                <a href="#232_PUT_podcast_for_FULL_update_8211_200_OK">2.3.2. PUT podcast for FULL update &#8211; 200 (OK)</a>
              </li>
            </ul>
          </li>

          <li>
            <a href="#233_POST_partial_update_for_not_existent_podcast_8211_404_NOT_FOUND">2.3.3. POST (partial update) for not existent podcast &#8211; 404 (NOT_FOUND)</a><ul>
              <li>
                <a href="#234POST_partial_update_podcast_8211_200_OK">2.3.4. POST (partial update) podcast &#8211; 200 (OK)</a>
              </li>
            </ul>
          </li>

          <li>
            <a href="#24_DELETE_resource">2.4. DELETE resource</a><ul>
              <li>
                <a href="#241_DELETE_second_inserted_podcast_8211_204_NO_CONTENT">2.4.1. DELETE second inserted podcast &#8211; 204 (NO_CONTENT)</a>
              </li>
              <li>
                <a href="#242_GET_deleted_podcast_8211_404_NOT_FOUND">2.4.2. GET deleted podcast &#8211; 404 (NOT_FOUND)</a>
              </li>
            </ul>
          </li>

          <li>
            <a href="#25_Bonus_operations">2.5. Bonus operations</a><ul>
              <li>
                <a href="#251_Add_podcast_from_application_form_urlencoded">2.5.1. Add podcast from application form urlencoded</a>
              </li>
            </ul>
          </li>
        </ul>
      </li>

      <li>
        <a href="#Resources">Resources</a>
      </li>
    </ul>
  </div>
</p>

## <span id="1_Introduction">1. Introduction</span>

<p style="text-align: justify;">
  If in the first part of the blog post I will do a brief introduction to curl and what it can do (HTTP requests with options), in the second part I will &#8220;translate&#8221; the <a title="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/master/src/main/resources/soapui/Test-Demo-REST-Jersey-with-Spring-soapui-project.xml" href="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/master/src/main/resources/soapui/Test-Demo-REST-Jersey-with-Spring-soapui-project.xml" target="_blank">SOAPui test suite</a> developed for the REST API tutorial to curl requests.
</p>

### <span id="11_What_is_curl"><strong>1.1. What is curl?</strong></span>

<p style="padding-left: 30px; text-align: justify;">
 <em>Curl is a command line tool and library for transferring data with URL syntax, supporting DICT, FILE, FTP, FTPS, Gopher, HTTP, HTTPS, IMAP, IMAPS, LDAP, LDAPS, POP3, POP3S, RTMP, RTSP, SCP, SFTP, SMTP, SMTPS, Telnet and TFTP. curl supports SSL certificates, HTTP POST, HTTP PUT, FTP uploading, HTTP form based upload, proxies, HTTP/2, cookies, user+password authentication (Basic, Digest, NTLM, Negotiate, Kerberos&#8230;), file transfer resume, proxy tunneling and more.[1]</em>
</p>

As mentioned, I will be using curl to simulate HEAD, GET, POST, PUT and DELETE request calls to the REST API.

### <span id="12_HEAD_requests">1.2. HEAD requests</span>

<p style="text-align: justify;">
  If you want to check if a resource is serviceable, what kind of headers it provides and other useful meta-information written in response headers, without having to transport the entire content, you can make a HEAD request.  Let&#8217;s say I want to see what I would GET when requesting a Podcast resource. I would issue the following HEAD request with curl:
</p>

**Request**

```bash
curl -I http://localhost:8888/demo-rest-jersey-spring/podcasts/1
```

  OR

```bash
curl -i -X HEAD http://localhost:8888/demo-rest-jersey-spring/podcasts/1
```

**Curl options **

  * `-i, --include` &#8211; include protocol headers in the output (H/F)
  * `-X, --request` &#8211; specify request  COMMAND (GET, PUT, DELETE&#8230;)  to use

**Response**

```bash
% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0   631    0     0    0     0      0      0 --:--:--  0:00:05 --:--:--     0
HTTP/1.1 200 OK
Date: Tue, 25 Nov 2014 12:54:56 GMT
Server: Jetty(9.0.7.v20131107)
Access-Control-Allow-Headers: X-extra-header
Access-Control-Allow-Headers: X-Requested-With, Content-Type, X-Codingpedia
Allow: OPTIONS
Content-Type: application/xml
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, DELETE, PUT
Vary: Accept-Encoding
Content-Length: 631
```

<p style="text-align: justify;">
  Note the following headers
</p>

  * `Access-Control-Allow-Headers: Content-Type`
  * `Access-Control-Allow-Methods: GET, POST, DELETE, PUT`
    and
  * `Access-Control-Allow-Origin: *`

in the response.

<span style="font-size: 16.3636360168457px; line-height: 1.5;">They&#8217;ve been added to support </span><a style="color: #bc360a;" title="http://www.w3.org/TR/cors/" href="http://www.w3.org/TR/cors/" target="_blank">Cross-Origing Resource Sharing (CORS)</a><span style="font-size: 16.3636360168457px; line-height: 1.5;">. You can find more about that in my post </span><a style="font-size: 16.3636360168457px; line-height: 1.5;" title="http://www.codingpedia.org/ama/how-to-add-cors-support-on-the-server-side-in-java-with-jersey/" href="http://www.codingpedia.org/ama/how-to-add-cors-support-on-the-server-side-in-java-with-jersey/" target="_blank">How to add CORS support on the server side in Java with Jersey</a><span style="font-size: 16.3636360168457px; line-height: 1.5;">.</span>

<p style="text-align: justify;">
  What I find a little bit intriguing is the response header <code>Content-Type: application/xml</code>, because I would have expected it to be <code>application/json</code>, since in the resource method defined with Jersey this should have taken precedence:
</p>

```java
@GET
@Path("{id}")
@Produces({ MediaType.APPLICATION_JSON, MediaType.APPLICATION_XML })
public Response getPodcastById(@PathParam("id") Long id, @QueryParam("detailed") boolean detailed)
        throws IOException,	AppException {
    Podcast podcastById = podcastService.getPodcastById(id);
    return Response.status(200)
            .entity(podcastById, detailed ? new Annotation[]{PodcastDetailedView.Factory.get()} : new Annotation[0])
            .header("Access-Control-Allow-Headers", "X-extra-header")
            .allow("OPTIONS").build();
}
```

### <span id="13_GET_request">1.3. GET request</span>

Executing curl with no parameters on a URL (resource) will execute a GET.

**Request**

```bash
curl http://localhost:8888/demo-rest-jersey-spring/podcasts/1
```

**Response**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<podcast>
   <id>1</id>
   <title>- The Naked Scientists Podcast - Stripping Down Science</title>
   <linkOnPodcastpedia>https://github.com/Codingpedia/podcastpedia/podcasts/792/-The-Naked-Scientists-Podcast-Stripping-Down-Science</linkOnPodcastpedia>
   <feed>feed_placeholder</feed>
   <description>The Naked Scientists flagship science show brings you a lighthearted look at the latest scientific breakthroughs, interviews with the world top scientists, answers to your science questions and science experiments to try at home.</description>
   <insertionDate>2014-10-29T10:46:02.00+0100</insertionDate>
</podcast>
```

<p style="text-align: justify;">
  Note that as expected from the HEAD request we get an xml document. Anyway we can force a JSON response by adding a header line to our curl request, setting the <code>Accept</code> HTTP header to <code>application/json</code>:
</p>

```bash
curl --header "Accept:application/json" http://localhost:8888/demo-rest-jersey-spring/podcasts/1
```

**Curl options **

  * `-H, --header` &#8211; customer header to pass to the server


```bash
curl -H "Accept:application/json" http://localhost:8888/demo-rest-jersey-spring/podcasts/1
```

**Response**

```js
{
  "id": 1,
  "title": "- The Naked Scientists Podcast - Stripping Down Science",
  "linkOnPodcastpedia": "https://github.com/Codingpedia/podcastpedia/podcasts/792/-The-Naked-Scientists-Podcast-Stripping-Down-Science",
  "feed": "feed_placeholder",
  "description": "The Naked Scientists flagship science show brings you a lighthearted look at the latest scientific breakthroughs, interviews with the world top scientists, answers to your science questions and science experiments to try at home.",
  "insertionDate": "2014-10-29T10:46:02.00+0100"
}
```

If you want to have it displayed prettier, you can use the following command, provided you have <a title="http://python.org" href="http://python.org" target="_blank">Python </a>installed on your machine.

**Request**

```bash
curl -H "Accept:application/json" http://localhost:8888/demo-rest-jersey-spring/podcasts/1 | python -m json.tool
```

**Response**

```bash
% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   758  100   758    0     0   6954      0 --:--:-- --:--:-- --:--:--  6954
[
    {
        "description": "The Naked Scientists flagship science show brings you a lighthearted look at the latest scientific breakthroughs, interviews with the world top scientists, answers to your science questions and science experiments to try at home.",
        "feed": "feed_placeholder",
        "id": 1,
        "insertionDate": "2014-10-29T10:46:02.00+0100",
        "linkOnPodcastpedia": "https://github.com/Codingpedia/podcastpedia/podcasts/792/-The-Naked-Scientists-Podcast-Stripping-Down-Science",
        "title": "- The Naked Scientists Podcast - Stripping Down Science"
    },
    {
        "description": "Quarks & Co: Das Wissenschaftsmagazin",
        "feed": "http://podcast.wdr.de/quarks.xml",
        "id": 2,
        "insert
        ionDate": "2014-10-29T10:46:13.00+0100",
        "linkOnPodcastpedia": "https://github.com/Codingpedia/podcastpedia/quarks",
        "title": "Quarks & Co - zum Mitnehmen"
    }
]
```

### <span id="14_Curl_request_with_multiple_headers">1.4. Curl request with multiple headers</span>

<p style="text-align: justify;">
  As you&#8217;ve found out in my latest post, <a title="http://www.codingpedia.org/ama/how-to-compress-responses-in-java-rest-api-with-gzip-and-jersey/" href="http://www.codingpedia.org/ama/how-to-compress-responses-in-java-rest-api-with-gzip-and-jersey/" target="_blank">How to compress responses in Java REST API with GZip and Jersey</a>, all the responses provided by the REST api are being compressed with GZip. This happens only if the client &#8220;suggests&#8221; that it accepts such encoding, by setting the following header <code>Accept-encoding:gzip</code>.
</p>

**Request**

```bash
curl -v -H "Accept:application/json" -H "Accept-encoding:gzip" http://localhost:8888/demo-rest-jersey-spring/podcasts/
```

**Curl options **

  * `-v, --verbose` &#8211; make the operation more talkative

To achieve that you need to simply **add another** -H option with the corresponding value. Of course in this case you would get some unreadable characters in the content, if you do not redirect the response to a file:

```bash
* Adding handle: conn: 0x28ddd80
* Adding handle: send: 0
* Adding handle: recv: 0
* Curl_addHandleToPipeline: length: 1
* - Conn 0 (0x28ddd80) send_pipe: 1, recv_pipe: 0
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* About to connect() to proxy vldn680 port 19001 (#0)
*   Trying 10.32.142.80...
* Connected to vldn680 (10.32.142.80) port 19001 (#0)
> GET http://localhost:8888/demo-rest-jersey-spring/podcasts/ HTTP/1.1
> User-Agent: curl/7.30.0
> Host: localhost:8888
> Proxy-Connection: Keep-Alive
> Accept:application/json
> Accept-encoding:gzip
>
< HTTP/1.1 200 OK
< Date: Tue, 25 Nov 2014 16:17:02 GMT
* Server Jetty(9.0.7.v20131107) is not blacklisted
< Server: Jetty(9.0.7.v20131107)
< Content-Type: application/json
< Access-Control-Allow-Origin: *
< Access-Control-Allow-Methods: GET, POST, DELETE, PUT
< Access-Control-Allow-Headers: X-Requested-With, Content-Type, X-Codingpedia
< Vary: Accept-Encoding
< Content-Encoding: gzip
< Content-Length: 413
< Via: 1.1 vldn680:8888
<
{ [data not shown]
100   413  100   413    0     0   2647      0 --:--:-- --:--:-- --:--:--  2647▒QKo▒0▒+▒▒g▒▒R▒+{▒V▒Pe▒▒؊c▒▒      n▒▒▒▒fæHH▒"▒▒g▒/?2▒eM▒gl▒a▒d
▒{=`7▒Eϖ▒▒c▒ZM

n8▒i▒▒▒}H▒▒i1▒3g▒▒▒▒▒   ;▒E▒0O▒n▒R*▒g/E▒▒n=▒▒▒▒)▒U▒▒▒lժ▒Φ▒h▒6▒▒▒_>w▒▒-▒▒:▒▒▒!▒Bb▒Z▒▒tO▒N@'= |▒▒C▒f▒▒loؠ▒,T▒▒A▒4▒▒:▒l+<▒▒▒▒P▒3▒▒A▒lR
▒u▒a▒͓9hO        #▒▒h▒i▒gq▒▒$▒▒|Ň        ▒▒▒08>#▒0b!▒▒'▒G▒^▒Iﺬ.TU▒▒▒z▒\▒i^]e▒▒▒▒2▒▒▒֯▒▒?▒:/▒m▒▒▒▒▒Y▒h▒▒▒_䶙V▒+R▒WT▒0▒?f{▒▒▒▒&▒l▒▒Sk▒iԽ~▒▒▒▒▒▒n▒▒▒▒_V]į▒
* Connection #0 to host vldn680 left intact
```

## <span id="2_SOAPui_test_suite_translated_to_curl_requests">2. SOAPui test suite translated to curl requests</span>

As mentioned, in this second part I will map to curl requests the SOAPui test suite presented <a title="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/#72_Integration_tests_with_SoapUI" href="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/#72_Integration_tests_with_SoapUI" target="_blank">here</a>.

### <span id="21_Create_podcasts_resource">2.1. Create podcast(s) resource</span>

#### <span id="211_Delete_all_podcasts_preparation_step">2.1.1. Delete all podcasts (preparation step)</span>

**Request**

```bash
curl -i -X DELETE http://localhost:8888/demo-rest-jersey-spring/podcasts/
```

**Response**

```bash
HTTP/1.1 204 No Content
Date: Tue, 25 Nov 2014 14:10:17 GMT
Server: Jetty(9.0.7.v20131107)
Content-Type: text/html
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, DELETE, PUT
Access-Control-Allow-Headers: X-Requested-With, Content-Type, X-Codingpedia
Vary: Accept-Encoding
Via: 1.1 vldn680:8888
Content-Length: 0
```

#### <span id="212_POST_new_podcast_without_feed_8211_400_BAD_REQUEST">2.1.2. POST new podcast without feed &#8211; 400 (BAD_REQUEST)</span>

**Request**

```bash
curl -i -X POST -H "Content-Type:application/json" http://localhost:8888/demo-rest-jersey-spring/podcasts/ -d '{"title":"- The Naked Scientists Podcast - Stripping Down Science-new-title2","linkOnPodcastpedia":"https://github.com/Codingpedia/podcastpedia/podcasts/792/-The-Naked-Scientists-Podcast-Stripping-Down-Science","description":"The Naked Scientists flagship science show brings you a lighthearted look at the latest scientific breakthroughs, interviews with the world top scientists, answers to your science questions and science experiments to try at home."}'
```

**Response**

```bash
HTTP/1.1 400 Bad Request
Date: Tue, 25 Nov 2014 15:12:11 GMT
Server: Jetty(9.0.7.v20131107)
Content-Type: application/json
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, DELETE, PUT
Access-Control-Allow-Headers: X-Requested-With, Content-Type, X-Codingpedia
Vary: Accept-Encoding
Content-Length: 271
Via: 1.1 vldn680:8888
Connection: close

{"status":400,"code":400,"message":"Provided data not sufficient for insertion","link":"http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/","developerMessage":"Please verify that the feed is properly generated/set"}
```

#### <span id="213_POST_new_podcast_correctly_8211_201_CREATED">2.1.3. POST new podcast correctly &#8211; 201 (CREATED)</span>

**Request**

```bash
curl -i -X POST -H "Content-Type:application/json" http://localhost:8888/demo-rest-jersey-spring/podcasts/ -d '{"title":"- The Naked Scientists Podcast - Stripping Down Science","linkOnPodcastpedia":"https://github.com/Codingpedia/podcastpedia/podcasts/792/-The-Naked-Scientists-Podcast-Stripping-Down-Science","feed":"feed_placeholder","description":"The Naked Scientists flagship science show brings you a lighthearted look at the latest scientific breakthroughs, interviews with the world top scientists, answers to your science questions and science experiments to try at home."}'
```

**Response**

```bash
HTTP/1.1 201 Created
Location: http://localhost:8888/demo-rest-jersey-spring/podcasts/2
Content-Type: text/html
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, DELETE, PUT
Access-Control-Allow-Headers: X-Requested-With, Content-Type, X-Codingpedia
Vary: Accept-Encoding
Content-Length: 60
Server: Jetty(9.0.7.v20131107)

A new podcast has been created AT THE LOCATION you specified
```

#### <span id="214_POST_same_podcast_as_before_to_receive_8211_409_CONFLICT">2.1.4. POST same podcast as before to receive &#8211; 409 (CONFLICT)</span>

**Request**

```bash
curl -i -X POST -H "Content-Type:application/json" http://localhost:8888/demo-rest-jersey-spring/podcasts/ -d '{"title":"- The Naked Scientists Podcast - Stripping Down Science","linkOnPodcastpedia":"https://github.com/Codingpedia/podcastpedia/podcasts/792/-The-Naked-Scientists-Podcast-Stripping-Down-Science","feed":"feed_placeholder","description":"The Naked Scientists flagship science show brings you a lighthearted look at the latest scientific breakthroughs, interviews with the world top scientists, answers to your science questions and science experiments to try at home."}'
```

**Response**

```bash
HTTP/1.1 409 Conflict
Date: Tue, 25 Nov 2014 15:58:39 GMT
Server: Jetty(9.0.7.v20131107)
Content-Type: application/json
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, DELETE, PUT
Access-Control-Allow-Headers: X-Requested-With, Content-Type, X-Codingpedia
Vary: Accept-Encoding
Content-Length: 300

{"status":409,"code":409,"message":"Podcast with feed already existing in the database with the id 1","link":"http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/","developerMessage":"Please verify that the feed and title are properly generated"}
```

#### <span id="215_PUT_new_podcast_at_location_8211_201_CREATED">2.1.5. PUT new podcast at location &#8211; 201 (CREATED)</span>

**Request**

```bash
curl -i -X PUT -H "Content-Type:application/json" http://localhost:8888/demo-rest-jersey-spring/podcasts/2 -d '{"id":2,"title":"Quarks & Co - zum Mitnehmen","linkOnPodcastpedia":"https://github.com/Codingpedia/podcastpedia/quarks","feed":"http://podcast.wdr.de/quarks.xml","description":"Quarks & Co: Das Wissenschaftsmagazin"}'
```

**Response**

```bash
HTTP/1.1 201 Created
Location: http://localhost:8888/demo-rest-jersey-spring/podcasts/2
Content-Type: text/html
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, DELETE, PUT
Access-Control-Allow-Headers: X-Requested-With, Content-Type, X-Codingpedia
Vary: Accept-Encoding
Content-Length: 60
Server: Jetty(9.0.7.v20131107)

A new podcast has been created AT THE LOCATION you specified
```

### <span id="22_Read_podcast_resource">2.2. Read podcast resource</span>

#### <span id="221_GET_new_inserted_podcast_8211_200_OK">2.2.1. GET new inserted podcast &#8211; 200 (OK)</span>

**Request**

```bash
curl -v -H "Accept:application/json" http://localhost:8888/demo-rest-jersey-spring/podcasts/1 | python -m json.tool
```

**Response**

```bash
< HTTP/1.1 200 OK
< Access-Control-Allow-Headers: X-extra-header
< Access-Control-Allow-Headers: X-Requested-With, Content-Type, X-Codingpedia
< Allow: OPTIONS
< Content-Type: application/json
< Access-Control-Allow-Origin: *
< Access-Control-Allow-Methods: GET, POST, DELETE, PUT
< Vary: Accept-Encoding
< Content-Length: 192
* Server Jetty(9.0.7.v20131107) is not blacklisted
< Server: Jetty(9.0.7.v20131107)
<
{ [data not shown]
* STATE: PERFORM => DONE handle 0x600056180; line 1626 (connection #0)
100   192  100   192    0     0   2766      0 --:--:-- --:--:-- --:--:--  3254
* Connection #0 to host localhost left intact
* Expire cleared
{
    "feed": "http://podcast.wdr.de/quarks.xml",
    "id": 1,
    "insertionDate": "2014-06-05T22:35:34.00+0200",
    "linkOnPodcastpedia": "https://github.com/Codingpedia/podcastpedia/quarks",
    "title": "Quarks & Co - zum Mitnehmen"
}
```

#### <span id="222_GET_podcasts_sorted_by_insertion_date_DESC_8211_200_OK">2.2.2. GET podcasts sorted by insertion date DESC &#8211; 200 (OK)</span>

**Request**

```bash
curl -v -H "Accept:application/json" http://localhost:8888/demo-rest-jersey-spring/podcasts?orderByInsertionDate=DESC | python -m json.tool
```

**Response**

```bash
< HTTP/1.1 200 OK
< Content-Type: application/json
< Access-Control-Allow-Origin: *
< Access-Control-Allow-Methods: GET, POST, DELETE, PUT
< Access-Control-Allow-Headers: X-Requested-With, Content-Type, X-Codingpedia
< Vary: Accept-Encoding
< Content-Length: 419
* Server Jetty(9.0.7.v20131107) is not blacklisted
< Server: Jetty(9.0.7.v20131107)
<
  0   419    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0{ [data not shown]
* STATE: PERFORM => DONE handle 0x600056180; line 1626 (connection #0)
100   419  100   419    0     0   6044      0 --:--:-- --:--:-- --:--:--  6983
* Connection #0 to host localhost left intact
* Expire cleared
[
    {
        "feed": "http://podcast.wdr.de/quarks.xml",
        "id": 1,
        "insertionDate": "2014-06-05T22:35:34.00+0200",
        "linkOnPodcastpedia": "https://github.com/Codingpedia/podcastpedia/quarks",
        "title": "Quarks & Co - zum Mitnehmen"
    },
    {
        "feed": "http://www.dayintechhistory.com/feed/podcast-2",
        "id": 2,
        "insertionDate": "2014-06-05T22:35:34.00+0200",
        "linkOnPodcastpedia": "https://github.com/Codingpedia/podcastpedia/podcasts/766/Day-in-Tech-History",
        "title": "Day in Tech History"
    }
]
```

### <span id="23_Update_podcast_resource">2.3. Update podcast resource</span>

#### <span id="231_PUT_not_8220complete8221_podcast_for_FULL_update_8211_400_BAD_REQUEST">2.3.1. PUT not &#8220;complete&#8221; podcast for FULL update &#8211; 400 (BAD_REQUEST)</span>

**Request**

```bash
curl -v -H "Content-Type:application/json" -X PUT http://localhost:8888/demo-rest-jersey-spring/podcasts/2 -d '{"id":2, "title":"Quarks & Co - zum Mitnehmen","linkOnPodcastpedia":"https://github.com/Codingpedia/podcastpedia/quarks","feed":"http://podcast.wdr.de/quarks.xml"}'
```

**Response**

```bash
< HTTP/1.1 400 Bad Request
< Content-Type: application/json
< Access-Control-Allow-Origin: *
< Access-Control-Allow-Methods: GET, POST, DELETE, PUT
< Access-Control-Allow-Headers: X-Requested-With, Content-Type, X-Codingpedia
< Vary: Accept-Encoding
< Content-Length: 290
* Server Jetty(9.0.7.v20131107) is not blacklisted
< Server: Jetty(9.0.7.v20131107)
<
* STATE: PERFORM => DONE handle 0x600056180; line 1626 (connection #0)
* Connection #0 to host localhost left intact
* Expire cleared
{"status":400,"code":400,"message":"Please specify all properties for Full UPDATE","link":"http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/","developerMessage":"required properties - id, title, feed, lnkOnPodcastpedia, description"}
```

#### <span id="232_PUT_podcast_for_FULL_update_8211_200_OK">2.3.2. PUT podcast for FULL update &#8211; 200 (OK)</span>

**Request**

```bash
$ curl -v -H "Content-Type:application/json" -X PUT http://localhost:8888/demo-rest-jersey-spring/podcasts/2 -d '{"id":2, "title":"Quarks & Co - zum Mitnehmen","linkOnPodcastpedia":"https://github.com/Codingpedia/podcastpedia/quarks","feed":"http://podcast.wdr.de/quarks.xml", "description":"Quarks & Co: Das Wissenschaftsmagazin"}'
```

**Response**

```bash
< HTTP/1.1 200 OK
< Location: http://localhost:8888/demo-rest-jersey-spring/podcasts/2
< Content-Type: text/html
< Access-Control-Allow-Origin: *
< Access-Control-Allow-Methods: GET, POST, DELETE, PUT
< Access-Control-Allow-Headers: X-Requested-With, Content-Type, X-Codingpedia
< Vary: Accept-Encoding
< Content-Length: 86
* Server Jetty(9.0.7.v20131107) is not blacklisted
< Server: Jetty(9.0.7.v20131107)
<
* STATE: PERFORM =&gt; DONE handle 0x600056180; line 1626 (connection #0)
* Connection #0 to host localhost left intact
* Expire cleared
The podcast you specified has been fully updated created AT THE LOCATION you specified
```

### <span id="233_POST_partial_update_for_not_existent_podcast_8211_404_NOT_FOUND">2.3.3. POST (partial update) for not existent podcast &#8211; 404 (NOT_FOUND)</span>

**Request**

```bash
$ curl -v -H "Content-Type:application/json" -X POST http://localhost:8888/demo-rest-jersey-spring/podcasts/3 -d '{"title":"Quarks & Co - zum Mitnehmen - GREAT PODCAST"}' | python -m json.tool
```

**Response**

```bash
< HTTP/1.1 404 Not Found
< Content-Type: application/json
< Access-Control-Allow-Origin: *
< Access-Control-Allow-Methods: GET, POST, DELETE, PUT
< Access-Control-Allow-Headers: X-Requested-With, Content-Type, X-Codingpedia
< Vary: Accept-Encoding
< Content-Length: 306
* Server Jetty(9.0.7.v20131107) is not blacklisted
< Server: Jetty(9.0.7.v20131107)
<
{ [data not shown]
* STATE: PERFORM =&gt; DONE handle 0x600056180; line 1626 (connection #0)
100   361  100   306  100    55   9069   1630 --:--:-- --:--:-- --:--:-- 13304
* Connection #0 to host localhost left intact
* Expire cleared
{
    "code": 404,
    "developerMessage": "Please verify existence of data in the database for the id - 3",
    "link": "http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/",
    "message": "The resource you are trying to update does not exist in the database",
    "status": 404
}
```

#### <span id="234POST_partial_update_podcast_8211_200_OK">2.3.4. POST (partial update) podcast &#8211; 200 (OK)</span>

**Request**

```bash
$ curl -v -H "Content-Type:application/json" -X POST http://localhost:8888/demo-rest-jersey-spring/podcasts/2 -d '{"title":"Quarks & Co - zum Mitnehmen - GREAT PODCAST"}'
```

**Response**

```bash
< HTTP/1.1 200 OK
< Content-Type: text/html
< Access-Control-Allow-Origin: *
< Access-Control-Allow-Methods: GET, POST, DELETE, PUT
< Access-Control-Allow-Headers: X-Requested-With, Content-Type, X-Codingpedia
< Vary: Accept-Encoding
< Content-Length: 55
* Server Jetty(9.0.7.v20131107) is not blacklisted
< Server: Jetty(9.0.7.v20131107)
<
* STATE: PERFORM =&gt; DONE handle 0x600056180; line 1626 (connection #0)
* Connection #0 to host localhost left intact
* Expire cleared
The podcast you specified has been successfully updated
```

### <span id="24_DELETE_resource">2.4. DELETE resource</span>

#### <span id="241_DELETE_second_inserted_podcast_8211_204_NO_CONTENT">2.4.1. DELETE second inserted podcast &#8211; 204 (NO_CONTENT)</span>

**Request**

```bash
$ curl -v -X DELETE http://localhost:8888/demo-rest-jersey-spring/podcasts/2
```

**Response**

```bash
< HTTP/1.1 204 No Content
< Content-Type: text/html
< Access-Control-Allow-Origin: *
< Access-Control-Allow-Methods: GET, POST, DELETE, PUT
< Access-Control-Allow-Headers: X-Requested-With, Content-Type, X-Codingpedia
< Vary: Accept-Encoding
* Server Jetty(9.0.7.v20131107) is not blacklisted
< Server: Jetty(9.0.7.v20131107)
<
* Excess found in a non pipelined read: excess = 42 url = /demo-rest-jersey-spring/podcasts/2 (zero-length body)
* STATE: PERFORM =&gt; DONE handle 0x600056180; line 1626 (connection #0)
* Connection #0 to host localhost left intact
* Expire cleared
```

#### <span id="242_GET_deleted_podcast_8211_404_NOT_FOUND">2.4.2. GET deleted podcast &#8211; 404 (NOT_FOUND)</span>

**Request**

```bash
curl -v http://localhost:8888/demo-rest-jersey-spring/podcasts/2 | python -m json.tool
```

**Response**

```bash
< HTTP/1.1 404 Not Found
< Content-Type: application/json
< Access-Control-Allow-Origin: *
< Access-Control-Allow-Methods: GET, POST, DELETE, PUT
< Access-Control-Allow-Headers: X-Requested-With, Content-Type, X-Codingpedia
< Vary: Accept-Encoding
< Content-Length: 306
* Server Jetty(9.0.7.v20131107) is not blacklisted
< Server: Jetty(9.0.7.v20131107)
<
{ [data not shown]
* STATE: PERFORM =&gt; DONE handle 0x600056180; line 1626 (connection #0)
100   306  100   306    0     0   8916      0 --:--:-- --:--:-- --:--:-- 13304
* Connection #0 to host localhost left intact
* Expire cleared
{
    "code": 404,
    "developerMessage": "Verify the existence of the podcast with the id 2 in the database",
    "link": "http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/",
    "message": "The podcast you requested with id 2 was not found in the database",
    "status": 404
}
```

### <span id="25_Bonus_operations">2.5. Bonus operations</span>

#### <span id="251_Add_podcast_from_application_form_urlencoded">2.5.1. Add podcast from application form urlencoded</span>

**Request**

```bash
curl -v --data-urlencode "title=Day in Tech History" --data-urlencode "linkOnPodcastpedia=https://github.com/Codingpedia/podcastpedia/podcasts/766/Day-in-Tech-History" --data-urlencode "feed=http://www.dayintechhistory.com/feed/podcast"
```

**Response**

```bash
< HTTP/1.1 201 Created
< Location: http://localhost:8888/demo-rest-jersey-spring/podcasts/null
< Content-Type: text/html
< Access-Control-Allow-Origin: *
< Access-Control-Allow-Methods: GET, POST, DELETE, PUT
< Access-Control-Allow-Headers: X-Requested-With, Content-Type, X-Codingpedia
< Vary: Accept-Encoding
< Content-Length: 81
* Server Jetty(9.0.7.v20131107) is not blacklisted
< Server: Jetty(9.0.7.v20131107)
<
* STATE: PERFORM =&gt; DONE handle 0x600056180; line 1626 (connection #0)
* Connection #0 to host localhost left intact
* Expire cleared
A new podcast/resource has been created at /demo-rest-jersey-spring/podcasts/null
```

<p class="note_normal">
  <strong>Note:</strong><br /> I am still at the beginning of using curl, so please if you have any suggestions leave a comment. Thank you.
</p>

## <span id="Resources">Resources</span>

  * <a title="http://curl.haxx.se/" href="http://curl.haxx.se/" target="_blank">Curl</a>
  * <a title="https://www.cygwin.com/" href="https://www.cygwin.com/" target="_blank">C</a><a title="https://www.cygwin.com/" href="https://www.cygwin.com/" target="_blank">ygwin</a>
  * [How to Set Up a Python Development Environment on Windows](http://www.davidbaumgold.com/tutorials/set-up-python-windows/ "http://www.davidbaumgold.com/tutorials/set-up-python-windows/")
  * <a title="http://www.python.org" href="http://www.python.org" target="_blank">Python.org</a>

<div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
  <img id="author_portrait" style="float: left; margin-right: 20px;" src="{{site.url}}/images/authors/amacoder.png" alt="Podcastpedia image" />

  <p id="about_author_header">
    <strong>Adrian Matei</strong>
  </p>

  <div id="author_details" style="text-align: justify;">
    Creator of <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> and <a title="Codingpedia, sharing coding knowledge" href="http://www.codingpedia.org" target="_blank">Codingpedia.org</a>, computer science engineer, husband, father, curious and passionate about science, computers, software, education, economics, social equity, philosophy - but these are just outside labels and not that important, deep inside we are all just consciousness, right?
  </div>

  <div id="follow_social" style="clear: both;">
    <div id="social_logos">
       <a class="icon-twitter" href="https://twitter.com/codingpedia" target="_blank"> </a> <a class="icon-facebook" href="https://www.facebook.com/codingpedia" target="_blank"> </a> <a class="icon-linkedin" href="https://www.linkedin.com/company/codingpediaorg" target="_blank"> </a> <a class="icon-github" href="https://github.com/adrianmatei-me" target="_blank"> </a>
    </div>

    <div class="clear">
    </div>
  </div>
</div>
