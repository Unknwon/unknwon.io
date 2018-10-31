+++ 
draft = false
date = 2014-05-02T18:54:00-04:00
title = "XORM - Go ORM: basic guide"
slug = "" 
tags = ["XORM"]
categories = ["Go"]
thumbnail = "<no value>"
description = ""
+++

## Introduction

xorm is a Go ORM which provides rich functionality with very simple APIs. It supports all main stream databases, including MySQL, PostgreSQL, SQLite3 and MsSQL. It allows you use chain operations and combine with raw SQL statements. Moreover, it has session that supports transactions to make your business logic safe.

## Installation

	go get github.com/go-xorm/xorm
		
### API Documentation

Please visit [Go Walker](http://gowalker.org/github.com/go-xorm/xorm).

## Basic Usages

[Sample Code](https://gist.github.com/Unknwon/03c4e9dec8ea97b3a010)

### Define model

You have to define model before using the ORMs in Go. The model is just a normal struct with tag of field in Go. In the sample code, we use following code to define a account model:

```go
type Account struct {
	Id      int64
	Name    string `xorm:"unique"`
	Balance float64
	Version int `xorm:"version"` // Optimistic Locking
}
```

For convience, the field named `Id` and has type `int64` will automatically become the primary key. If you want to use other fields, you need to set tag `pk` to tell xorm.

Field `Name`'s tag uses `unique` to make record value unique in the table, in this case, one user name can only appear once.

The last field is the Optimistic Locking, we'll get into it later.

According to the visibility rule of Go, all fields have to be capitalized; otherwise, it leads to panic when ORM is reflecting on the struct.

~~The sample code is too simple to list all tags that xorm supports, you can see the full list [here](https://github.com/go-xorm/xorm/blob/master/docs/QuickStart.md#24column-defenition).~~

### Create ORM engine

After you define the model, it's time to create the ORM engine for your application. In this step, you need to specify what database driver you use, along with its connection information(database host, user name and password). In the sample code, we use SQLite3, so the database file path is all we need:

```go
x, err = xorm.NewEngine("sqlite3", "./bank.db")
```

Go asks for registering database driver before using it, so all drivers will register themselves in `init` function. Similarly, this step cannot be omit when we use ORM.

There are many database drivers exist, one ORM usually supports one for each database. You'd better check the documentation of the ORM you choose to use and see what database drivers it supports. ~~In this case, we can find 5 database drivers that xorm supports [here](https://github.com/go-xorm/xorm/blob/master/docs/QuickStart.md#1create-orm-engine).~~

The following code registers the database driver:

```go
import (
	_ "github.com/mattn/go-sqlite3"
)
```

Add underscore before import path means only execute the `init` function of that package and nothing else. Therefore, you will not get error like "import but not used".

### Auto-sync table

xorm has an awesome feature that incremental synchronize from models to database tables automatically. What does that mean? Well, it checks database tables and updates if needed when tables are not same as models you defined. Moreover, it's incremental operation, which means xorm will not delete any column if you delete a field or change the name of field, and keeps it where it was, then create a new column. Same rule will be applied when you change the tag of any field.

The following code does the job of enabling this feature:

```go
err = x.Sync(new(Account))
```

Pass all models to method `Sync`. In the sample code, there is only one model called `Account`. The reason we use `new()` here is because reflection works on instance of a struct in order to help ORM gets fields and their tags.

### Add, delete and update

Let's take a look at how to use xorm to do add, delete and update operations.

#### Add record

To insert a new record, the record must not exist, error occurs otherwise:

```go
_, err := x.Insert(&Account{Name: name, Balance: balance})
```

#### Delete record

Delete a record:

```go
_, err := x.Delete(&Account{Id: id})
```

Method `Delete` use the parameter as a query condition, they delete all records that match the condition. In the sample code, we only give the `Id` field of `Account`, then xorm only deletes the record that has that `ID`. However, in the cases there are more than one records has the same `ID`, all of them will be deleted.

#### Get and update record

To update a record, the record must exist. Thus, we have to get the record before modifying it:

```go
a := &Account{}
has, err := x.Id(id).Get(a)
```
    
Method `Get` returns only one record. Like delete operation, the variable `a` is also the query condition. But the goodness of xorm is that it also allows you use chain operations to specify condition, just like the `Id` method we use here.

The method returns two values, one is `bool` type to indicates there is a record or not, another one is `error` type to show if any error occurs.

After we got the record, we now need to make some changes and update back to database:

```go
a.Balance += deposit
_, err = x.Update(a)
```

You must pass a pointer of variable as first parameter to method `Update`. It accepts second parameter as query condition as well.

### Get batch of records

In some situations, we need to get a list of records from database and do some ordering.

So the corresponding method of `Get` method is the `Find` method for getting all records which match the query condition:

```go
err = x.Desc("balance").Find(&as)
```

We use `Desc` method before `Find` will lead the results have a order from largest to smallest depends on the value of `balance`.

You must pass a pointer of `slice` type variable as first parameter to method `Find`. It accepts second parameter as query condition as well.

### Optimistic Locking

Optimistic Locking is a feature powered by xorm, and by adding `version` in field tag to enable it. That field will increase by 1 every time you update the record. This can make sure only one change happens at one time when your application is multithreading, and returns error when the operation is outdated.

## Summary

This post is the part 1 of xorm, code is basic and straightforward to make as simple as possible. Therefore, a bad usage exists in code(pointed in source file). 

Sample code is a complete application and you can play with it. Also, make some changes to see how its behave changes. 