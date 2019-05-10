---
layout: post
title: Parallel calls with async-await in javascript - I promise you all performance and simplicity
description: "I was blown away about the simplicity and performance gain of making parallel calls with the new async-await feature in javascript. See the blog post
 to understand why."
author: ama
permalink: /ama/parallel-calls-with-async-await-in-javascript-i-promise-you-all-performance-and-simplicity
published: true
categories: [javascript, nodejs]
tags: [javascript, nodejs, async-await, typescript]
---

Some days ago, I presented how the code got much cleaner when I replaced Mongoose callbacks with the
 <span class="highlight-yellow">async-await</span> feature from ES7, which NodeJs already support.
  I mentioned then, I was gonna do a post to demonstrate how `async-await` can also be used to make parallel calls.
   Here am I doing just that. I was actually blown away by its simplicity and performance gain when doing so.
  Let me show you what I mean.
  
  <!--more-->

## Use case
Imagine we have a notification service (might be a REST service) that sends emails to selected employees of an organisation - `sendEmailNotification(user)`

### Before
The code before using parallelization is iterating through the list of employees and send sequentially emails:

{% highlight typescript %}
    async notifyEmployees(organisationId: string): Promise<string> {

        const employees:Employee[] = await this.getEmployeesOfOrganisation(organisationId);

        for(let i = 0; i < employees.length; i++) {
            this.sendEmailNotification(employees[i]);
        }
        return "Sent sequentially emails to employees of company with id " + organisationId;
    } 
{% endhighlight %}

With an actual notification service implementation it took around 40 seconds for 30 employees. This is definitely a  NoGo
in modern times...

The notification service might look something like the following:

{% highlight typescript %}
    async sendEmailNotification(employee: Employee, notificationRequest:NotificationRequest): Promise<void> {
        let notificationRequestMessage: NotisRequestMessage = {
            channel: "EMAIL",
            name: employee.lastName,
            gender: employee.gender
        }
        const response = await superagent
            .post('NOTIFICATION_SERVICE_URL')
            .set('Content-Type', 'application/json')
            .send(notificationRequestMessage);
    }   
{% endhighlight %}

### After

Here is the same code, but now the emails are sent in parallel: 

{% highlight typescript %}
    async notifyEmployees(organisationId: string): Promise<string> {

        const employees:Employee[] = await this.getEmployeesOfOrganisation(organisationId);

        let emailNotificationPromises:Promise<void>[] = [];
        for(let i = 0; i < employees.length; i++) {
            emailNotificationPromises.push(this.sendEmailNotification(employees[i]));
        }

        await Promise.all(emailNotificationPromises);

        return "Sent in parallel emails to employees of company with id " + organisationId;
    }
{% endhighlight %}

First of all the performance gain was fantastic: <span class="highlight-yellow">down to 1.3 seconds from 40 seconds</span>... 

> It's more or less how long it takes the notification service to respond to a call

This is made possible with the help of [`Promise.all`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)
function which aggregates the result of multiple promises:
 
 1. we define an array of Promises of type the result expected from the notification service
 2. we iterate over the employees of the company and push the call to the array defined above
 3. we `await` for **all** the promises
 4. that's it - it cannot get much simpler than that...
   
Stay tuned for other cool async-await features I might come across along the way...
