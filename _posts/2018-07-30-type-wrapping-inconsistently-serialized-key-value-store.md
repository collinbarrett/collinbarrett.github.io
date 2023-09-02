---
id: 7111
title: 'Type Wrapping an Inconsistently Serialized Key-Value Store'
date: '2018-07-30T04:00:40-05:00'
author: 'Collin M. Barrett'
excerpt: 'Migrating CRUD operations of a key-value store with inconsistent serialization formats from the view layer to typed POCOs in the data layer.'
layout: post
guid: '/?p=7111'
permalink: /type-wrapping-inconsistently-serialized-key-value-store/
wp_featherlight_disable:
    - ''
image: /assets/img/typeWrappingFruit_collinmbarrett.gif
categories:
    - Code
tags:
    - Career
    - Database
    - Dotnet
    - Refactoring
    - 'Shelby Systems'
---

## Background

The ASP.NET application I support has a table that stores transient data with a custom serializer per primary key (`PrefId` in the example below). The view layer consumes this data primarily for maintaining the user-specific state of controls like filters, pickers, and text boxes. The RDBMS table looks something like this:

| `UserId` | `PrefId` | `Pref` |
|---|---|---|
| `1` | `1` | `Key1= All Key2= 2,4,7` |
| `2` | `1` | `Key1= First Key2= 1,4` |
| `2` | `2` | `404Key= 12/5/2014 10:01:23 AM` |

As you can see in this small sample, the `Pref` values are a bit of a “garbage bin” (naming credit to my colleague). They are generally (but not always) serialized to a `VARCHAR` looking something like:

<div class="wp-block-syntaxhighlighter-code ">```
<pre class="brush: plain; title: ; notranslate" title="">
<Key1>= <Value1> <Key2>= <Value2>
```

</div>There can be any number of key/value pairs. Values can be of any type (such as `INTEGER`, `VARCHAR`, `BOOLEAN`, `TIMESTAMP`) and can also be a comma-separated list. There are even instances of comma-separated lists of different types (such as {`VARCHAR`, `TIMESTAMP`, `TIMESTAMP`}). Because the serialization is currently a responsibility of the view layer, format and naming are inconsistent from module-to-module.

The view layer is unfortunately also the owner of which types each value represents. Actually, in many cases, the type is never explicitly stated at all; it is just inferred based on the branching or binding logic that consumes it. We found the following patterns throughout the view layer just before the data is needed or to save:

```
<pre class="wp-block-preformatted"><pre class="brush: csharp; light: true; title: ; notranslate" title="">
value = Prefs.UserPrefGet(Key)
```

```
<pre class="wp-block-preformatted"><pre class="brush: csharp; light: true; title: ; notranslate" title="">
Prefs.UserPrefUpdate(Key, value)
```

These methods are custom getters and setters that find and insert respectively in the `Prefs` string (which represents the `Pref` value in the database). When a value is retrieved or before a value is saved, the view layer logic has to handle all the manipulation to convert from or to the custom serialization format.

## In-Place Refactor

To move towards a more flexible and expressive architecture, we wanted to treat this data just like any other typed POCO. Changing the architecture of the data store is too expensive and out of scope at the moment, but we could push the parsing and type-casting down to the data layer and use POCOs throughout the rest of the application.

### Modeling the Data

The solution we landed on was creating a set of base and derived classes to model the data in the data layer. The base class only contains the `PrefId`, while the derived classes implement the keys and their value types. Eventually, we will derive a `DerivedPrefX` (certainly with better, domain-specific names) for every `PrefId`.

```
<pre class="wp-block-preformatted"><pre class="brush: csharp; title: ; notranslate" title="">
public abstract class Pref {
PrefId { get; set; }
}

public class DerivedPref1 : Pref {
public DerivedPref1()
{
PrefId = (int)PrefIdEnum.DerivedPref1;
}

public string Key1 { get; set; }
public List&amp;lt;int&amp;gt; Key2 { get; set; }
}

public class DerivedPref2 : Pref {
public DerivedPref2()
{
PrefId = (int)PrefIdEnum.DerivedPref2;
}

public DateTime Key404 { get; set; }
}
```

### Renaming Incompatible Keys

Some of the keys in the `Pref` values start with an integer value. If you notice the third row in the original sample table has `404Key`, but I conveniently renamed this to `Key404` in `DerivedPref2` since C# does not support member names beginning with an integer. The data layer has to perform the 1-to-1 mapping back to the current data store, however.

We solved this by creating a new C# custom attribute:

```
<pre class="wp-block-preformatted"><pre class="brush: csharp; title: ; notranslate" title="">
public class PrefAttribute : Attribute
{
public string Key { get; set; }
}
```

`DerivedPref2` now looks like:

```
<pre class="wp-block-preformatted"><pre class="brush: csharp; title: ; notranslate" title="">
public class DerivedPref2 : Pref {
public DerivedPref2() { ... }

[Pref(Key="404Key")]
public DateTime Key404 { get; set; }
}
```

This attribute gives us the flexibility to rename any keys in the application independently of the database. If any names are found to be lying, or a better name exists, we can rename it in the application without breaking the existing data store. We have decoupled key names in the database from those in the application.

### Repository Getter with Default Deserializer

Using reflection and generics, we came up with the routine below to deserialize into the new derivations of `Pref`. This set of methods is a work in progress still and only captures the most common serialization formats.

```
<pre class="wp-block-preformatted"><pre class="brush: csharp; title: ; notranslate" title="">
public T GetPreferences&amp;lt;T&amp;gt;() where T : Pref, new()
{
var t = new T();
var pref = GetByID(t.PreferenceId)?.Value;
if (pref == null)
return t;
var props = GetObjectWritableProperties&amp;lt;T&amp;gt;();
foreach (var prop in props)
{
var value = pref.UserPreferenceGet(PrefKeychain.GetKey(prop));
SetPropValue(value, prop, t);
}

return t;
}

private static IEnumerable&amp;lt;PropertyInfo&amp;gt; GetObjectWritableProperties&amp;lt;T&amp;gt;()
{
var type = typeof(T);
var ignoredProps = new[] { "PrefId" };
return type.GetProperties()
.Where(x =&amp;gt; x.CanWrite &amp;amp;&amp;amp;
!ignoredProps.Contains(x.Name) &amp;amp;&amp;amp;
(x.PropertyType.IsValueType ||
x.PropertyType.IsList() &amp;amp;&amp;amp; x.PropertyType.GetGenericArguments()[0].IsValueType ||
Type.GetTypeCode(x.PropertyType) == TypeCode.String));
}

private static void SetPropValue&amp;lt;T&amp;gt;(string value, PropertyInfo prop, T t) where T : Pref, new()
{
var isList = prop.PropertyType.IsList();
var type = isList ? prop.PropertyType.GetGenericArguments()[0] : prop.PropertyType;

if (value == null &amp;amp;&amp;amp; isList)
{
prop.SetValue(t, Activator.CreateInstance(typeof(List&amp;lt;&amp;gt;).MakeGenericType(type)));
return;
}

switch (Type.GetTypeCode(type))
{
case TypeCode.Object:
if (type == typeof(Guid))
prop.SetValue(t, isList
? value.Split(',').Select(s =&amp;gt; s.ToGuid()).ToList()
: value.ToGuid() as object);
break;
case TypeCode.Boolean:
prop.SetValue(t, isList
? value.Split(',').Select(s =&amp;gt; s.ToBoolean()).ToList()
: value.ToBoolean() as object);
break;
case TypeCode.Int32:
prop.SetValue(t, isList
? value.Split(',').Select(s =&amp;gt; s.ToInt(-1)).ToList()
: value.ToInt(-1) as object);
break;
case TypeCode.DateTime:
prop.SetValue(t, isList
? value.Split(',').Select(s =&amp;gt; s.ToDateTime(DateTime.MinValue).Value).ToList()
: value.ToDateTime(DateTime.MinValue).Value as object);
break;
case TypeCode.String:
prop.SetValue(t, isList ? value.Split(',').ToList() : value as object);
break;
//...
//Other type cases omitted for brevity
//...
default:
if (type.IsEnum)
prop.SetValue(t, isList
? value.Split(',').Select(s =&amp;gt; s.ToEnum(type)).ToList()
: value.ToEnum(type));
break;
}
}
```

### Getting the Keys

While the idea is to refactor all of the `Pref` CRUD operations to use these new typed entities, it will take time to get there. We started with the read-only repository method above in this first iteration.

However, rather than continuing to use hard-coded strings for `Key` in the calls to `UserPrefUpdate(Key, Value)`, we did want to at least make use of the new POCOs to get the `Key`s. This promotes better maintainability (such as through IDE tools) than using string literals.

```
<pre class="wp-block-preformatted"><pre class="brush: csharp; light: true; title: ; notranslate" title="">
Prefs.UserPrefUpdate(nameof(DerivedPref1.Key1), value)
```

Using `nameof()` works great for all of the properties that we have not renamed, but handling the keys that were renamed with the custom attributes required a bit more work. With a bit of [StackOverflow](https://stackoverflow.com/questions/51540318/get-attribute-on-or-name-of-derived-classs-property) assistance, I came up with the method below to get the key of any property (whether it used the property name directly or the custom attribute). Note that `GetCustomAttributeValue<T>()` is a custom helper method that returns the value of an attribute in the type of `T`, but its implementation is out of scope for this article.

```
<pre class="wp-block-preformatted"><pre class="brush: csharp; title: ; notranslate" title="">
public static class PrefKeychain
{
public static string GetKey&amp;lt;TPref&amp;gt;(Expression&amp;lt;Func&amp;lt;TPref, object&amp;gt;&amp;gt; property) where TPref : Prefs =&amp;gt;
GetKey(GetMemberInfo(property));

public static string GetKey(MemberInfo memberInfo) =&amp;gt; GetAttributeKey(memberInfo) ?? memberInfo?.Name;

private static MemberInfo GetMemberInfo&amp;lt;TPref&amp;gt;(Expression&amp;lt;Func&amp;lt;TPref, object&amp;gt;&amp;gt; property) =&amp;gt;
(property.Body as MemberExpression ?? ((UnaryExpression)property.Body).Operand as MemberExpression)?.Member;

private static string GetAttributeKey(MemberInfo member) =&amp;gt;
member.GetCustomAttributeValue&amp;lt;string&amp;gt;(typeof(UserPrefAttribute), "Key");
}
```

This can be called like:

```
<pre class="wp-block-preformatted"><pre class="brush: csharp; light: true; title: ; notranslate" title="">
Prefs.UserPrefUpdate(PrefKeychain.GetKey&amp;lt;DerivedPref2&amp;gt;(p =&amp;gt; p.Key404), Value)
```

## Moving Forward

We are about a week and a half into this project, and it is not our primary focus at the moment. So, there are indeed many holes. Over time, we still need to:

- implement update routine
- support inconsistent serialization formats
- support generalized support for lists of different types (presumably through custom sub-classes)

Eventually, if the entire application was converted to this approach, we could easily swap out the serialization format to a standard such as JSON or XML. That seems quite far away, but we have laid the groundwork.

*<small>Disclaimer: Opinions expressed are solely my own and do not express the views or opinions of my employer.</small>*