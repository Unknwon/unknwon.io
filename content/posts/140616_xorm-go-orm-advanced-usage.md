+++ 
draft = false
date = 2014-06-16T10:59:00-04:00
title = "XORM - Go ORM: advanced usage"
slug = "" 
tags = ["XORM"]
categories = ["Go"]
thumbnail = "<no value>"
description = ""
+++

## Transaction

[Sample code 1](https://gist.github.com/Unknwon/861dfa5107c6e9a65974)

This part is based on last post [XORM - Go ORM: basic guide](https://unknwon.io/xorm-go-orm-basic-guide/) and make some improvements in sample code. In the last post, the transfer part didn't use transcation to make operation safe, and now it is: 

```go
// Create Session object.
sess := x.NewSession()
defer sess.Close()
// Start transcation.
if err = sess.Begin(); err != nil {
    return err
}

if _, err = sess.Update(a1); err != nil {
    // Roll back when error occurs.
    sess.Rollback()
    return err
} else if _, err = sess.Update(a2); err != nil {
    sess.Rollback()
    return err
}
// Commit transcation.
return sess.Commit()
```

- First of all, call method `x.NewSession` to get a session object, then do not forget use `defer` to close it when return.
- Use method `sess.Begin` to tell xorm you are going to make everything after this be in the transcation, and roll back if necessary.
- Relatively, call method `Update` to do update operations. Same thing for inserting, deleting, etc. Just change from `xorm.Engine` to `xorm.Session`, methods' name are the same.
- Keep in mind that call `sess.Rollback` before return statement, this is the key for transcation.
- Finally, call `sess.Commit` to finish the transcation. 

## Common functions and methods

[Sample code 2](https://gist.github.com/Unknwon/a7c9162bce3f20d3bee6)

### Count records

Use method `Count` to count how many records in given table:

```go
func getAccountCount() (int64, error) {
	return x.Count(new(Account))
}
```

It returns all records number because we do not offer any condition. The `new(Account)` just for reflection, does not indicates any query condition.

### Iterative query

Use method `Iterate` to query all matched records iteratively:

```go
x.Iterate(new(Account), printFn)
```

Body of function `printFn`:

```go
var printFn = func(idx int, bean interface{}) error {
	fmt.Printf("%d: %#v\n", idx, bean.(*Account))
	return nil
}
```

Make it as a function is because sample code will use it multiple times.

The purpose of argument of method `Iterate` is same as method `Count`, just for reflection.

Let's focus on the iterative function declaration: it accepts 2 arguments, the first one is the index of result set(no relation to its ID value); the second one is the empty interface which contains the value, and we need to assert it before using it. In the above example, we know the type is `Account`, so we use `bean.(*Account)` to get the actual object we want.

If you think method `Iterate` isn't flexible enough, then use the following solution to gain more control of everything:

```go
a := new(Account)
rows, err := x.Rows(a)
if err != nil {
	log.Fatalf("Fail to rows: %v", err)
}
defer rows.Close()

for rows.Next() {
	if err = rows.Scan(a); err != nil {
		log.Fatalf("Fail get row: %v", err)
	}
	fmt.Printf("%#v\n", a)
}
```

The above example does the exactly same thing as the example of method `Iterate`. Therefore, use method `Iterate` can achieve most of simple jobs.

### Query specified fields

Use method `Cols` can specify fields you want to return(or fields that are valuable at the time):

```go
x.Cols("name").Iterate(new(Account), printFn)
```

In this case, only field `Name` has value, all the other fields have zero values. Note that method `Cols` accepts name in the data table, not the field name in the struct.

### Omit specified fields

Use method `Omit` when you want to omit query of some fields:

```go
x.Omit("name").Iterate(new(Account), printFn)
```

In this case, only field `Name` has zero value. Same argument rule for method `Omit` and `Cols`.

### Result offset

Offset query results is very common in paging applications, use method `Limit` can achieve the same goal:

```go
x.Limit(3, 2).Iterate(new(Account), printFn)
```
    
This method accepts at least 1 argument, the first one indicates max number of results; if it has second argument, then means the offset of results. In this case, query results offset 2 and return at most 3 records.

## Logging

Generally, use `x.ShowSQL = true` to enable logging feature of xorm, all SQL statements will be printed to console. If you want to save them into a file, use the following code:

```go
f, err := os.Create("sql.log")
if err != nil {
	log.Fatalf("Fail to create log file: %v\n", err)
	return
}
x.Logger = xorm.NewSimpleLogger(f)
```

## LRU Cache

Xorm is the only ORM that supports LRU cache, it's very easy to use:

```go
cacher := xorm.NewLRUCacher(xorm.NewMemoryStore(), 1000)
x.SetDefaultCacher(cacher)
```

Now you have the very default LRU cache. ~~For more details like omit or only cache some tables, see [documentation](https://github.com/go-xorm/xorm/blob/master/docs/QuickStart.md#120).~~

## Event hook

There are 6 kinds of hooks(author is too lazy to add document for it) you can use in xorm. In the sample code, it demonstrates 2, which are `BeforeInsert` and `AfterInsert`.

They will be called **right before insertion** and **right after insertion**:

```go
func (a *Account) BeforeInsert() {
	log.Printf("before insert: %s", a.Name)
}

func (a *Account) AfterInsert() {
	log.Printf("after insert: %s", a.Name)
}
```

Run the code and you'll see the differences.

## Summary

Until now, all posts about xorm are finished. It's true that I didn't cover everything about xorm, but at least most common ones. Other handy features are better for you to explore during the development of your applications.

## Use cases

- [Gogs - Go Git Service](http://gogs.io)
- [Go Walker](https://gowalker.org)
- [Sodu China](http://sudochina.com)