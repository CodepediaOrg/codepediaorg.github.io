---
layout: post
title: How to consume a text message from a JMS queue with a Message Driven Bean (MDB)
description: "Consume a text message from a JMS queue with a Message Driven Bean (MDB) code snippet"
author: ama
permalink: /snippets/620d48efe102f148ceb9faea/consume-a-text-message-from-a-jms-queue-with-a-message-driven-bean-mdb-
published: true
snippetId: 620d48efe102f148ceb9faea
categories: [snippets]
tags: [java, javaee, jms, message-driven-bean, codever-snippets]
---

In the snippet you can see a message driven bean set up to consume asynchronously messages from a JMS queue:

```java
import javax.ejb.ActivationConfigProperty;
import javax.ejb.MessageDriven;
import javax.ejb.MessageDrivenContext;
import javax.ejb.TransactionAttribute;
import javax.ejb.TransactionAttributeType;
import javax.inject.Inject;
import javax.jms.JMSException;
import javax.jms.Message;
import javax.jms.MessageListener;
import javax.jms.TextMessage;
import javax.xml.bind.JAXBException;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@MessageDriven(
    name = "AccountMessageBean",
    activationConfig = {
      @ActivationConfigProperty(
          propertyName = "destinationType",
          propertyValue = "javax.jms.Queue"),
      @ActivationConfigProperty(
          propertyName = "destination",
          propertyValue = "java:jboss/exported/queue/account.queue"),
      @ActivationConfigProperty(
          propertyName = "acknowledgeMode",
          propertyValue = "Auto-acknowledge")
    })
public class AccountMessageBean implements MessageListener {

  private static final Logger logger = LoggerFactory.getLogger(AccountMessageBean.class);

  @Inject  MessageDrivenContext messageDrivenContext;

  @Inject MessageService messageService;

  @Override
  @TransactionAttribute(TransactionAttributeType.REQUIRED)
  public void onMessage(Message message) {
    if (!(message instanceof TextMessage)) {
      logger.warn("This is not a text message. Break");
      return;
    }

    TextMessage messageText = (TextMessage) message;
    try {
      if (messageText.getText().contains("organisation")) {
        this.messageService.createOrganisation(messageText.getText());
      } else if (messageText.getText().contains("person"))
        this.messageService.createPerson(messageText.getText());
      else {
        logger.warn("Something went wrong, must be one of those types");
      }

    } catch (JMSException | JAXBException e) {
      logger.error("An error occured while processing the message, mark to rollback");
      messageDrivenContext.setRollbackOnly();
    }
  }
}
```

**Note** the following:
- `@MessageDriven`  annotation where the type of the consumed resource is defined (`destinationType` - here `javax.jms.Queue`,
 but it could also be `javax.jms.Topic`), the location of the queue (`propertyName = "destination"`)
 and that the message from the queue is acknowledged once the processing is successful (this is the **default** setup)
- the consuming message driven bean implements the `javax.jms.MessageListener` and the method `onMessage(Message message)`
- if a container-managed transaction is not present a new one is created - `@TransactionAttribute(TransactionAttributeType.REQUIRED)`
- the consumed message is expected to be text (`javax.jms.TextMessage`)
- you access the string value of the message by calling the `getText()` method - `messageText.getText()`
- `MessageDrivenContext` is used to gain additional access to transaction management - here the transaction is marked for rollback,
 if an error occurs `messageDrivenContext.setRollbackOnly();` (this is only possible in container-based transactions)

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://docs.oracle.com/javaee/6/tutorial/doc/bncgl.html#bncgq" target="_blank" style="font-weight: lighter">
     https://docs.oracle.com/javaee/6/tutorial/doc/bncgl.html#bncgq
  </a>
</span>

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="620d48efe102f148ceb9faea" %}
