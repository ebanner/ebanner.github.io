---
layout: default
title:  "clojure"
description: Clojure macros
---

# Clojure

Emacs

```emacs
M-x cider-jack-in RET
SPC m s a ; cider-switch-to-repl-buffer
```

Imports

```clojure
(require '[clojure.string :as str])
```

Macros

```clojure
(defmacro reduce* [init coll f]
  `(reduce ~f ~init ~coll))

(defmacro map* [[f & args] coll]
  `(map (partial ~f ~@args) ~coll))

(defmacro filter* [[f & args] coll]
  `(filter (partial ~f ~@args) ~coll))
```

Elixir-style function clauses


```clojure
(def clauses [])
(defmacro defclause [name args pred & body]
  `(do
     (defn ~name [& args#]
       (loop [[clause# & rest#] clauses]
         (if (apply (:pred clause#) args#)
           (apply (:body clause#) args#)
           (recur rest#))))

     (let [clause# {:pred (fn ~args ~pred)
                    :body (fn ~args ~@body)}]

       (def clauses (conj clauses clause#)))))

(defclause expand [chunk] (string? chunk)
  chunk)

(defclause expand [[_ rep] s] :else
 (->>
  s
  (repeat rep)
  (apply str)))
```
