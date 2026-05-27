---
layout: default
title:  "clojure"
description: Clojure macros
---

# Clojure

Imports

```clojure
(require '[clojure.string :as str])
```

Macros

```clojure
(defmacro map* [[f & args] coll]
  `(map (partial ~f ~@args) ~coll))

(defmacro filter* [[f & args] coll]
  `(filter (partial ~f ~@args) ~coll))

(defmacro reduce* [[acc init x coll] & body]
  `(reduce (fn [~acc ~x] ~@body) ~init ~coll))

(defmacro reduce* [init [x coll] & body]
  (let [acc-names (map name (keys init))
        acc-syms  (map symbol acc-names)]
    `(reduce (fn [{:keys [~@acc-syms]} ~x]
               ~@body)
             ~init
             ~coll)))
```
