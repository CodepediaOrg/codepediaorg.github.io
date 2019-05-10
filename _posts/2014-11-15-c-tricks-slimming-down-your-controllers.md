---
id: 2094
title: 'C# Tricks: Slimming down your controllers'
date: 2014-11-15T19:41:46+00:00
author: Johannes Brodwall
layout: post
guid: http://www.codingpedia.org/?p=2094
permalink: /jhannes/c-tricks-slimming-down-your-controllers/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3228268433
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
  - 'C#'
  - code
  - 'tips &amp; tricks'
tags:
  - controller
  - slim
  - software development
  - tips and tricks
---
<p style="text-align: justify;">
  This blog post is dedicated to my colleague <a style="color: #2a4639;" href="http://seminda.wordpress.com/">Seminda</a> who has been experimenting with how to create simple and powerful web applications. Thank you for showing me your ideas and discussing improvements with me, Seminda.
</p>

<p style="text-align: justify;">
  I find many C# applications have much unnecessary code. This is especially true as the weight of the business logic of many applications are shifting from the backend to JavaScript code in the web pages. When the job of your application is to provide data to a front-end, it’s important to keep it slim.
</p>

<p style="text-align: justify;">
  In this article, I set out to simplify a standard MVC 4 API controller by generalizing the functionality, centralizing exception handling and adding extension methods to the DB set that is used to fetch my data.<!--more-->
</p>

<p style="color: #333333;">
  If you generate an API controller based on an existing Entity and remove some of the noise, your code may look like this:
</p>

<pre class="lang:c# decode:true ">public class PersonController : ApiController
{
    private readonly DbContext _dbContext;
    private readonly IDbSet&lt;Person&gt; _people;

    public PersonController()
    {
        _dbContext = new ApplicationDbContext();
        _people = dbContext.Set&lt;Person&gt;();
    }

    public Person GetPerson(int key)
    {
        return _people.Find(key);
    }

    public HttpResponseMessage PostPerson(Person value)
    {
        _people.Add(value);
        _dbContext.SaveChanges();
        return Request.CreateResponse(HttpStatusCode.Created);
    }

    public IEnumerable&lt;Person&gt; GetPeople()
    {
        return _people.ToList();
    }

    public IEnumerable&lt;PersonSummary&gt; GetAdminsInCity(string city)
    {
        return _people
            .Where(p =&gt; p.City == city && p.Type == PersonType.Admin)
            .Select(p =&gt; new PersonSummary { FullName = p.FirstName + " " + p.LastName });
    }

    public HttpResponseMessage DeletePerson(int id)
    {
        Person person = db.Persons.Find(id);
        _people.Remove(person);

        try
        {
            _dbContext.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            return Request.CreateErrorResponse(HttpStatusCode.NotFound, ex);
        }
        return Request.CreateResponse(HttpStatusCode.OK, person);
    }
}</pre>

<p style="color: #333333;">
  This code is a simplified version of what the API 4 Controller wizard will give you. It includes a GetPerson method that returns a person by Id, PostPerson which saves a new person, GetPeople which returns all people in the database, GetAdminsInCity, which filters people on city and type and DeletePerson which finds the person with the specified Id and deletes it.
</p>

<p style="color: #333333;">
  I have replaced DbContext and IDbSet with interfaces instead of the concrete subclass of DbContext makes it easy to create tests that use a double for the database, for example MockDbSet.
</p>

<h3 style="color: #000000;">
  Generify the controller
</h3>

<p style="color: #333333;">
  This is easy and safe as long as things are simple. As a general tip, rename you methods to “Get”, “Post” and “Index” rather than “GetPerson”, “PostPerson” and “GetPeople”. This will make it possible to generalize the controller thus:
</p>

<pre class="lang:c# decode:true ">public class PersonController : EntityController&lt;Person&gt;
{
    public PersonController() : base(new ApplicationDbContext())
    {
    }

    public IEnumerable&lt;PersonSummary&gt; GetAdminsInCity(string city)
    {
        return _dbSet
            .Where(p =&gt; p.City == city && p.Type == PersonType.Admin)
            .Select(p =&gt; new PersonSummary { FullName = p.FirstName + " " + p.LastName });
    }
}</pre>

<span style="color: #333333;">The generic EntityController can be used for any class that is managed with EntityFramework.</span>

<pre class="lang:c# decode:true ">public abstract class EntityController&lt;T&gt; : ApiController where T : class
{
    private readonly DbContext _dbContext;
    protected IDbSet&lt;T&gt; _dbSet;

    protected EntityController(DbContext dbContext)
    {
        _dbContext = dbContext;
        _dbSet = dbContext.Set&lt;T&gt;();
    }

    public HttpResponseMessage Post(Person value)
    {
        _dbSet.Add(value);
        _dbContext.SaveChanges();
        return Request.CreateResponse(HttpStatusCode.Created);
    }

    public IEnumerable&lt;T&gt; Get()
    {
        return _dbSet.ToList();
    }

    public T Get(int key)
    {
        return _dbSet.Find(key);
    }

    public HttpResponseMessage Delete(int id)
    {
        T entity = _dbSet.Find(id);
        _dbSet.Remove(entity);

        try
        {
            _dbContext.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            return Request.CreateErrorResponse(HttpStatusCode.NotFound, ex);
        }
        return Request.CreateResponse(HttpStatusCode.OK, entity);
    }
}</pre>

<h3 style="color: #000000;">
  Exception handling
</h3>

I will dare to make a bold generalization: Most of the bugs that remain in production software are in error handling. Therefore I have a very simple guideline: No catch blocks that don’t rethrow another exception.

The actual handling of exceptions should be centralized. This both makes the code easier to read and it ensures that exceptions are handled consistently. In MVC 4, the place to do this is with a ExceptionFilterAttribute.

<p style="color: #333333;">
  We can thus simplify the EntityController:
</p>

<pre class="lang:c# decode:true ">[HandleApplicationExceptions]
public class PersonController : ApiController
{
    // Post and Get omitted for brevity

    public HttpResponseMessage Delete(int id)
    {
        _dbSet.Remove(_dbSet.Find(id));
        _dbContext.SaveChanges();
        return Request.CreateResponse(HttpStatusCode.OK);
    }
}</pre>

<span style="color: #333333;">The HandleApplicationException looks like this:</span>

<pre class="lang:c# decode:true ">public class HandleApplicationExceptionsAttribute : ExceptionFilterAttribute
{
    public override void OnException(HttpActionExecutedContext context)
    {
        var exception = context.Exception;
        if (exception is DbUpdateConcurrencyException)
        {
            context.Response =
                context.Request.CreateErrorResponse(HttpStatusCode.NotFound, exception);
            return;
        }
        context.Response =
            context.Request.CreateErrorResponse(HttpStatusCode.InternalServerError, exception);
    }
}</pre>

<p style="color: #333333;">
  This code code of course add special handling of other types of exceptions, logging etc.
</p>

<h3 style="color: #000000;">
  Dbset as a repository
</h3>

<p style="color: #333333; text-align: justify;">
  But one piece remains in PersonController: The rather complex use of LINQ to do a specialized query. This is where many developers would introduce a Repository with a specialized “FindAdminsByCity” and perhaps even a separate service layer with a method for “FindSummaryOfAdminsByCity”.
</p>

<p style="color: #333333; text-align: justify;">
  If you start down that path, many teams will choose to introduce the same layers (service and repository) even when these layers do nothing and create a lot of bulk in their applications. From my personal experience, this is worth fighting against! Needless layers is the cause of a huge amount of needless code in modern applications.
</p>

<p style="color: #333333;">
  Instead, you can make use of C# extension methods:
</p>

<pre class="lang:c# decode:true ">public static class PersonCollections
{
    public static IQueryable&lt;Person&gt; GetAdminsInCity(this IDbSet&lt;Person&gt; persons, string city, PersonType personType)
    {
        return persons.Where(p =&gt; p.City == city && p.Type == personType);
    }

    public static IEnumerable&lt;object&gt; SelectSummary(this IQueryable&lt;Person&gt; persons)
    {
        return persons.Select(p =&gt; new PersonSummary { FullName = p.FirstName + " " + p.LastName });
    }
}</pre>

<span style="color: #333333;">The resulting controller method becomes nice and encapsulated:</span>

<pre class="lang:c# decode:true ">public IEnumerable&lt;object&gt; SummarizeAdminsInCity(string city)
{
    return _dbSet.WhereTypeAndCity(city, PersonType.Admin).SelectSummary();
}</pre>

<p style="color: #333333;">
  That’s pretty minimal and straightforward!
</p>

<p style="color: #333333; text-align: justify;">
  I’ve showed three tricks to reduce the complexity in your controllers: First, parts of your controllers can be generalized, especially if you avoid the name of the entity in action method names; second, exception handling can be centralized, removing noise from the main application flow and enforcing consistency; finally, using extension methods on IQueryable<Person> gives my DbSet domain specific methods without having to implement extra layers in the architecture.
</p>

<p style="color: #333333;">
  I’d love to hear if you’re using these approach or if you’ve tried something like it but found it lacking.
</p>

<p class="note_normal" style="color: #333333;">
  Published at Codingpedia.org with the permission of Johannes Brodwall – source <a title="http://johannesbrodwall.com/2014/02/07/c-tricks-slimming-down-your-controllers/" href="http://johannesbrodwall.com/2014/02/07/c-tricks-slimming-down-your-controllers/" target="_blank">C# Tricks: Slimming down your controllers</a> from <a title="http://johannesbrodwall.com/" href="http://johannesbrodwall.com/" target="_blank">http://johannesbrodwall.com/</a>
</p>

<p style="color: #333333;">
  <div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
    <img id="author_portrait" style="float: left; margin-right: 20px;" src="{{site.url}}/images/authors/johannes-brodwall.jpeg" alt="Johannes Brodwall" /> 
    
    <p id="about_author_header">
      <strong>Johannes Brodwall</strong>
    </p>
    
    <div id="author_details" style="text-align: justify;">
      Johannes is the director of software development for the MRM product company BrandMaster. In his spare time he likes to coach teams and developers on better coding, collaboration, planning and product understanding.
    </div>
    
    <div id="follow_social" style="clear: both;">
      <div id="social_logos">
        <a class="icon-earth" href="http://johannesbrodwall.com/" target="_blank"> </a> <a class="icon-twitter" href="https://twitter.com/jhannes" target="_blank"> </a> <a class="icon-github" href="https://github.com/jhannes" target="_blank"> </a>
      </div>
      
      <div class="clear">
      </div>
    </div>
  </div>
</p>
