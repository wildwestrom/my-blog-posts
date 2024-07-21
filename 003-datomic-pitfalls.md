---
title: Datomic Pitfalls
subtitle: Have you tried using Datomic? Were you confused by its documentation? You may have fallen into these two pitfalls.
date: 2021-07-13
tags: [programming, clojure, datomic, database]
id: datomic-pitfalls
---

# What is Datomic?

Datomic is a database. From what I hear, it's much nicer to work with than SQL. I don't want to worry about concatenating strings to make queries. I just want to pass around Clojure data like I always do.

Anyway, I went along the documentation trying to figure out how to use Datomic, and I just couldn't get passed a couple blocks until a friend from lambdaisland helped me out.

# The two pitfalls of learning Datomic.

## Where do I get Datomic in the first place?

This isn't really a pitfall, so much as it was not easy for me to find this information. So when I started out, I used a library called [Datalevin](https://github.com/juji-io/datalevin), which mimics Datomic's API. It was good enough for my purposes, but it isn't quite up to par with Datomic itself. Anyway, the free version is called `com.Datomic/Datomic-free`{.verbatim}. You can add the latest version to your project by copying the info from [clojars](https://clojars.org/com.datomic/datomic-free). 

## Pitfall #1 - Schema-Data Mismatch

A schema looks like a vector of maps. Each map will have 3 required attributes.

These attributes are:

1.  `:db/ident`
2.  `:db/valueType`
3.  `:db/cardinality`

These get passed into a map. Looking something like this:

```clojure
{:db/ident       :item/id-number
 :db/valueType   :db.type/int
 :db/cardinality :db.cardinality/one}
```

You will get an error if you try to pass in data that doesn't match the type you defined. I encountered this when parsing csv data as the csv library I was using takes everything as strings.

```clojure
{:item/id-number (Integer/parseInt "1111")}
```

## Pitfall #2 - Query mistakes

Queries are written in a language called datalog, a variant of prolog for data.

Querying the connection won't work.

```clojure
(d/q '[:find ?e
       :where [?e :item/item-number]] conn)
```

For some reason it's abstracted by the `db` function.

```clojure
(d/q '[:find ?e
       :where [?e :item/item-number]] (d/db conn))
```

This one won't work.

```clojure
(d/q [:find ?e
      :where [?e :item/item-number]] (d/db conn))
```

The query must be quoted before passed to the query function.

```clojure
(d/q '[:find ?e
       :where [?e :item/item-number]] (d/db conn))
```

The one above returns the IDs of each entity.

This one reuturns the items.

```clojure
(d/q '[:find ?c
       :where
       [?e :item/item-number ?c]] (d/db conn))
```
