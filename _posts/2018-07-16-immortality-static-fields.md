---
title: 'The Immortality of C# Static Fields'
date: '2018-07-16T05:00:52-05:00'
author: 'Collin M. Barrett'
excerpt: 'A tale of improperly using a static field, preventing a colleague from doing the same, and a primer on why
static fields should be avoided.'
layout: post-wp-import
permalink: /immortality-static-fields
image: /assets/img/staticSlide_collinmbarrett.jpg
categories:
- Code
tags:
- Cache
- Database
- Dotnet
- Refactoring
- 'Shelby Systems'
- 'Visual Studio'
---

Twice in the last two weeks, my team has grappled with using static fields. The first was somewhat inadvertently
introduced by yours truly and wreaked a bit of havoc in our latest release. The latter was debating whether or not to
make use of one to perform in-memory caching of data. In both cases, and arguably in nearly all possible cases, using
static (non-constant) fields is probably a poor choice. I clarify non-constant since [`const` is also inherently
static](https://stackoverflow.com/questions/408192/why-cant-i-have-public-static-const-string-s-stuff-in-my-class/408201#408201),
but it is generally safer to use since it is compile-time constant and therefore burned into the build artifacts rather
than set in-memory.

## Case 1: Reducing Duplication

As a part of a bug fix in our last release, I moved some business logic from the view layer back into a new service
class. The class `new`-ed up five different repositories, each using the same `DataContext` instance. (Five repositories
are probably too many for one class, and I could have modularized the service more. However, refactoring is iterative.)
The service class had two different constructors, so rather than `new`-ing the context and each repository in both
constructors, I tried instantiating them at declaration.

Mainly, I wanted to replace this:

```csharp
public class MyServiceClass
{
    private readonly DataContext _context;
    private readonly Lazy<MyRepo1> _myRepo1;
    private readonly Lazy<MyRepo2> _myRepo2;
    private readonly Lazy<MyRepo3> _myRepo3;
    private readonly Lazy<MyRepo4> _myRepo4;
    private readonly Lazy<MyRepo5> _myRepo5;
    private readonly MyParamClass _myParamClass;

    public MyServiceClass()
    {
        _context = new DataContext();
        _myRepo1 = new Lazy<MyRepo1>(() => new MyRepo1(_context));
        _myRepo2 = new Lazy<MyRepo2>(() => new MyRepo2(_context));
        _myRepo3 = new Lazy<MyRepo3>(() => new MyRepo3(_context));
        _myRepo4 = new Lazy<MyRepo4>(() => new MyRepo4(_context));
        _myRepo5 = new Lazy<MyRepo5>(() => new MyRepo5(_context));
    }

    public MyServiceClass(MyParamClass myParamClass)
    {
        _myParamClass = myParamClass;
        _context = new DataContext();
        _myRepo1 = new Lazy<MyRepo1>(() => new MyRepo1(_context));
        _myRepo2 = new Lazy<MyRepo2>(() => new MyRepo2(_context));
        _myRepo3 = new Lazy<MyRepo3>(() => new MyRepo3(_context));
        _myRepo4 = new Lazy<MyRepo4>(() => new MyRepo4(_context));
        _myRepo5 = new Lazy<MyRepo5>(() => new MyRepo5(_context));
    }
}
```

With this:

```csharp
public class MyServiceClass
{
    private readonly DataContext _context = new DataContext();
    private readonly Lazy<MyRepo1> _myRepo1 = new Lazy<MyRepo1>(() => new MyRepo1(_context));
    private readonly Lazy<MyRepo2> _myRepo2 = new Lazy<MyRepo2>(() => new MyRepo2(_context));
    private readonly Lazy<MyRepo3> _myRepo3 = new Lazy<MyRepo3>(() => new MyRepo3(_context));
    private readonly Lazy<MyRepo4> _myRepo4 = new Lazy<MyRepo4>(() => new MyRepo4(_context));
    private readonly Lazy<MyRepo5> _myRepo5 = new Lazy<MyRepo5>(() => new MyRepo5(_context));
    private readonly MyParamClass _myParamClass;

    public MyServiceClass()
    {
    }

    public MyServiceClass(MyParamClass myParamClass)
    {
        _myParamClass = myParamClass;
    }
}
```

Initializing the repositories at declaration, however, throws a compiler error on the `context` parameter.

> Cannot access non-static field “context” in static context.

I was moving a little bit too quickly, and this seemed like a simple fix. Just stuff a `static` keyword on the `DataContext` declaration, watch the red squiggles fade away, and call it done.

```csharp
public class MyServiceClass
{
    private static readonly DataContext context = new DataContext();
    ...
}
```

The symptom that single `static` keyword caused cost our team a week or more of person-hours to resolve. Since our application runs in a multi-tenant environment, the first client who instantiated the service class loaded their `DataContext` into the static field. When the next request came in from a user of another tenant, a new instance of the service class was created, but the static value of `context` persisted.

Without knowing the cause, you can understand how the issue seemed not only intermittent but irreproducible on our development machines because we were not developing in a multi-tenant environment. Who cares if the `DataContext` is static if the environment ever only needs a single value of `DataContext`?

Thankfully, the service class threw all kinds of errors on a repository retrieval before it tried to do any writing back to the database, or the issue could have been much more costly.

**Update 7.18.18**: [DEV Community member Asti](https://dev.to/asti) pointed out that I could have also changed my second constructor to something like below to reduce the duplication:

```csharp
public MyServiceClass(MyParamClass myParamClass) : this()
{
    _myParamClass = myParamClass;
}
```

## Case 2: Cross-Request Caching

Our app uses [Telerik Reporting](https://www.telerik.com/products/reporting.aspx) for producing all kinds of reports for our users. The architecture of this framework is such that the report viewer loads client-side and then makes calls back to the server for each report it needs to display.

A colleague was performing an upgrade on a set of three reports that are displayed in the same viewer instance. A subset of the data is shared across the three reports, so he proposed caching the data in-memory using a static field so that the request for each of the following reports would not need to retrieve the data from the database again.

Learning what we did very recently in “Case 1”, we had to shoot this down quickly for multiple reasons.

- Functionality: We are currently in an effort to transform our application to be able to be load-balanced. Static fields are persisted in-memory only on a single node, which would fail in a load-balanced scenario.
- Security: It could potentially cause conflicts and even security holes (viewing the data of another customer) if multiple users were trying to run the same report at the same time.
- Performance: It would permanently utilize a non-trivial amount of memory for a cached dataset that we only needed in a transient manner (for no more than a few seconds).

## Consequences of Using Static Fields

- It permanently allocates memory for only that field's use.
- Its value lives on immortally beyond the lifetime of any given instance of its containing class.
- It is not accessible across nodes in a load-balanced environment.
- It persists across all tenants in the AppDomain of a multi-tenant environment.

## TL;DR

Using non-constant, static fields should be pretty much the last choice, and the consequences of using them should be fully understood.

*<small>Disclaimer: Opinions expressed are solely my own and do not express the views or opinions of my employer.</small>*