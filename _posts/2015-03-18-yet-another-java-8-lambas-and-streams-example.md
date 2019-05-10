---
id: 2341
title: Yet another Java 8 Lambas and Streams example
date: 2015-03-18T13:33:48+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=2341
permalink: /ama/yet-another-java-8-lambas-and-streams-example/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3605686719
fsb_social_facebook:
  - 1
fsb_social_google:
  - 3
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - code
  - java
tags:
  - comparator
  - iterator
  - java
  - java8
  - lambdas
  - streams
---
I&#8217;ve been lagging behind with what Java 8 features exercising concerns, so in this post I will briefly present my initial experience with lambdas and streams.<!--more-->

As usual, I will focus on a <a title="https://github.com/Codingpedia/podcastpedia/podcasting" href="https://github.com/Codingpedia/podcastpedia/podcasting" target="_blank">Podcast</a> class:

<pre class="lang:java decode:true">package org.codingpedia.learning.java.core;

import java.util.Comparator;

public class Podcast {

    int id;
    String title;
    String producer;
    int subscriptionsNumber;

    /** number of up votes(likes) */
    int upVotes;

    /** number of down votes*/
    int downVotes;

    public Podcast() {
        this.subscriptionsNumber = 0;
    }

    public Podcast(int id, String title, String producer, int subscriptionsNumber, int upVotes, int downVotes) {
        this.id = id;
        this.title = title;
        this.producer = producer;
        this.subscriptionsNumber = subscriptionsNumber;
        this.upVotes = upVotes;
        this.downVotes = downVotes;
    }

    public static final Comparator&lt;Podcast&gt; BY_POSITIVE_VOTES_DIFFERENCE =
            (left, right) -&gt; (right.getUpVotes()-right.getDownVotes()) - (left.getUpVotes()-left.getDownVotes());

    @Override
    public String toString() {
        return "Podcast{" +
                "title='" + title + '\'' +
                ", producer='" + producer + '\'' +
                ", upVotes=" + upVotes +
                ", downVotes=" + downVotes +
                ", subscriptionsNumber=" + subscriptionsNumber +
                '}';
    }

    public static String toJSON(Podcast p) {
        return "{" +
                "title:'" + p.title + '\'' +
                ", producer:'" + p.producer + '\'' +
                ", upVotes:" + p.upVotes +
                ", downVotes:" + p.downVotes +
                ", subscriptionsNumber:" + p.subscriptionsNumber +
                "}";
    }

    public int getUpVotes() {
        return upVotes;
    }

    public void setUpVotes(int upVotes) {
        this.upVotes = upVotes;
    }

    public int getDownVotes() {
        return downVotes;
    }

    public void setDownVotes(int downVotes) {
        this.downVotes = downVotes;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getProducer() {
        return producer;
    }

    public void setProducer(String producer) {
        this.producer = producer;
    }

    public int getSubscriptionsNumber() {
        return subscriptionsNumber;
    }

    public void setSubscriptionsNumber(int subscriptionsNumber) {
        this.subscriptionsNumber = subscriptionsNumber;
    }

    public int getId() {

        return id;
    }

    public void setId(int id) {
        this.id = id;
    }
}
</pre>

which I will use in different operations, built with lambdas and streams. But I will let the code speak for itself this time:

<pre class="lang:java decode:true" title="Lambdas and streams example">package org.codingpedia.learning.java.core;

import java.util.*;
import java.util.function.Predicate;
import java.util.stream.Collectors;

public class LambdasAndStreams {

    public static void main(String[] args) {

        List&lt;Podcast&gt; podcasts  = Arrays.asList(
                //new Podcast(podcastId, title, producer, subscriptionsNumber, upVotes, downVotes),
                new Podcast(1, "Quarks&Co", "wdr", 50, 18, 1),
                new Podcast(2, "Angeklickt - zum Mitnehmen", "wdr", 10, 5, 1),
                new Podcast(3, "Leonardo im WDR 5-Radio zum Mitnehmen", "wdr", 12, 10, 5),
                new Podcast(4, "L'ESPRIT PUBLIC", "France culture", 3, 10, 1),
                new Podcast(5, "LA FABRIQUE DE L'HISTOIRE", "France culture", 10, 4, 1),
                new Podcast(6, "LES MATINS DE FRANCE CULTURE", "France culture", 46, 12, 8)
        );

        System.out.println("*********** Display initial podcasts with forEach ************");
        podcasts.forEach(podcast -&gt; System.out.println(podcast));

        System.out.println("\n\n********************** Sorting with lambdas ***********************");
        // Sort by title
        System.out.println("\n*********** Sort by title (default alphabetically) - highlight comparator ************");
        Collections.sort(podcasts, Comparator.comparing(Podcast::getTitle));
        podcasts.forEach(podcast -&gt; System.out.println(podcast));

        System.out.println("\n*********** Sort by numbers of subscribers DESCENDING - highlight reversed ************");
        Collections.sort(podcasts, Comparator.comparing(Podcast::getSubscriptionsNumber).reversed());
        podcasts.forEach(podcast -&gt; System.out.println(podcast));

        System.out.println("\n*********** Sort by producer and then by title - highlight composed conditions************");
        Collections.sort(podcasts, Comparator.comparing(Podcast::getProducer).thenComparing(Podcast::getTitle));
        podcasts.forEach(podcast -&gt; System.out.println(podcast));

        System.out.println("\n*********** Sort by difference in positive votes DESCENDING ************");
        Collections.sort(podcasts, Podcast.BY_POSITIVE_VOTES_DIFFERENCE);
        podcasts.forEach(podcast -&gt; System.out.println(podcast));


        System.out.println("\n\n******************** Streams *************************");
        System.out.println("\n*********** Filter podcasts with more than 21 subscribers - highlight filters ************");
        podcasts.stream()
                .filter((podcast)-&gt; podcast.getSubscriptionsNumber() &gt;= 21)
                .forEach((podcast)-&gt;
                        System.out.println(podcast));

        System.out.println("\n********* Filter podcasts from producer with more than 21 subscribers - highlight predicate **************");
        Predicate&lt;Podcast&gt; hasManySubscribers = (podcast) -&gt; podcast.getSubscriptionsNumber() &gt;= 21;
        Predicate&lt;Podcast&gt; wdrProducer = (podcast) -&gt; podcast.getProducer().equals("wdr");
        podcasts.stream()
                .filter(hasManySubscribers.and(wdrProducer))
                .forEach((podcast) -&gt;
                        System.out.println(podcast));

        System.out.println("\n********* Display popular podcasts - highlight \"or\" in predicate **************");
        Predicate&lt;Podcast&gt; hasManyLikes = (podcast) -&gt; (podcast.getUpVotes()-podcast.getDownVotes()) &gt; 8;
        podcasts.stream()
                .filter(hasManySubscribers.or(hasManyLikes))
                .forEach((podcast) -&gt;
                        System.out.println(podcast));

        System.out.println("\n********* Collect subscription numbers - highlight \"mapToInt\" **************");
        int numberOfSubscriptions = podcasts.stream()
                .mapToInt(Podcast::getSubscriptionsNumber).sum();
        System.out.println("Number of all subscriptions : " + numberOfSubscriptions);

        System.out.println("\n********* Display podcast with most subscriptions -highlight \"map reduce\" capabilities **************");
        Podcast podcastWithMostSubscriptions;
        podcastWithMostSubscriptions = podcasts.stream()
                .map(podcast -&gt; new Podcast(podcast.getId(), podcast.getTitle(), podcast.getProducer(), podcast.getSubscriptionsNumber(), podcast.getUpVotes(), podcast.getDownVotes()))
                .reduce(new Podcast(),
                        (pod1, pod2) -&gt; (pod1.getSubscriptionsNumber() &gt; pod2.getSubscriptionsNumber()) ? pod1 : pod2);
        System.out.println(podcastWithMostSubscriptions);

        System.out.println("\n********* Display podcasts titles in XML format -highlight \"map reduce\" capabilities **************");
        String titlesInXml =
                "&lt;podcasts data='titles'&gt;" +
                podcasts.stream()
                    .map(podcast -&gt; "&lt;title&gt;" + podcast.getTitle() + "&lt;/title&gt;")
                    .reduce("", String::concat)
                + "&lt;/podcasts&gt;";
        System.out.println(titlesInXml);

        System.out.println("\n********* Display podcasts in JSON format -highlight \"map reduce\" capabilities **************");
        String json =
                podcasts.stream()
                .map(Podcast::toJSON)
                .reduce("[", (l, r) -&gt; l + (l.equals("[") ? "" : ",") + r)
                + "]";
        System.out.println(json);

        System.out.println("\n********* Display sorted podcasts by title in JSON format -highlight \"map collect\" capabilities **************");
        String jsonViaCollectors =
                podcasts.stream()
                        .sorted(Comparator.comparing(Podcast::getTitle))
                        .map(Podcast::toJSON)
                        .collect(Collectors.joining(",", "[", "]"));
        System.out.println(jsonViaCollectors);

        System.out.println("\n********* Select first 3 podcasts with most subscribers -highlight \"map collect\" capabilities **************");
        List&lt;Podcast&gt; podcastsWithMostSubscribers = podcasts.stream()
                .sorted(Comparator.comparing(Podcast::getSubscriptionsNumber).reversed())
                .limit(3)
                .collect(Collectors.toList());
        System.out.println(podcastsWithMostSubscribers);

        System.out.println("\n********* Get podcasts grouped by producer -highlight \"collector\" capabilities **************");
        Map&lt;String, List&lt;Podcast&gt;&gt; podcastsByProducer = podcasts.stream()
                .collect(Collectors.groupingBy(podcast -&gt; podcast.getProducer()));
        System.out.println(podcastsByProducer);
    }
}
</pre>

## Resources

  1. <a title="http://www.oracle.com/technetwork/java/javase/overview/java8-2100321.html" href="http://www.oracle.com/technetwork/java/javase/overview/java8-2100321.html" target="_blank">Java 8 Central</a>
  2. <a title="http://www.oracle.com/technetwork/articles/java/architect-lambdas-part1-2080972.html" href="http://www.oracle.com/technetwork/articles/java/architect-lambdas-part1-2080972.html" target="_blank">Java 8: Lambdas, Part 1</a>
  3. <a title="http://www.oracle.com/technetwork/articles/java/architect-lambdas-part2-2081439.html" href="http://www.oracle.com/technetwork/articles/java/architect-lambdas-part2-2081439.html" target="_blank">Java 8: Lambdas, Part 2</a>
