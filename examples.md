# carp-sparseset Examples

## 1. Basic Initialization and Insertion
A SparseSet maps `Uint32` IDs to values of any type. It is perfect for tracking components in an ECS or managing indices in a pool.

```clojure
(load "sparse_set.carp")
(use SparseSet)

(defn main []
  (let [set (SparseSet.new)]
    (do
      ;; Insert some values at specific IDs
      (SparseSet.insert! &set 10u32 @"PositionComponent")
      (SparseSet.insert! &set 42u32 @"VelocityComponent")
      (SparseSet.insert! &set 7u32  @"HealthComponent")
      
      (println* "Set length: " (SparseSet.length &set)) ;; 3
      (println* "Contains ID 42? " (SparseSet.contains? &set 42u32)))))
```

## 2. Fast Lookups and Updates
SparseSet provides $O(1)$ lookups and updates.

```clojure
(load "sparse_set.carp")
(use SparseSet)

(defn main []
  (let [set (SparseSet.new)]
    (do
      (SparseSet.insert! &set 1u32 100)
      
      ;; Get a value (returns Maybe)
      (match (SparseSet.get &set 1u32)
        (Maybe.Just val) (println* "Value: " val)
        (Maybe.Nothing) (println* "Not found"))

      ;; Update a value directly via pointer
      (SparseSet.with-ptr &set 1u32 
        &(fn [p] (Pointer.set p (+ @p 50))))
      
      (println* "New Value: " (Maybe.unsafe-from (SparseSet.get &set 1u32))))))
```

## 3. Cache-Friendly Iteration
SparseSet maintains data in a contiguous "dense" array, making iteration extremely fast (cache-friendly).

```clojure
(load "sparse_set.carp")
(use SparseSet)

(defn main []
  (let [set (SparseSet.new)]
    (do
      (SparseSet.insert! &set 0u32 1.5)
      (SparseSet.insert! &set 100u32 2.5)
      (SparseSet.insert! &set 50u32 3.5)
      
      ;; Iterate over all values
      (SparseSet.for-each &(fn [v] (println* "Iterating value: " @v)) &set)
      
      ;; Use functional helpers
      (let [total (SparseSet.reduce &(fn [acc v] (+ acc @v)) 0.0 &set)]
        (println* "Total sum: " total)))))
```

## 4. Efficient Removal
Removal is $O(1)$ and uses the "Swap-with-last" pattern to keep the dense array contiguous.

```clojure
(load "sparse_set.carp")
(use SparseSet)

(defn main []
  (let [set (SparseSet.new)]
    (do
      (SparseSet.insert! &set 1u32 @"Alice")
      (SparseSet.insert! &set 2u32 @"Bob")
      (SparseSet.insert! &set 3u32 @"Charlie")
      
      ;; Remove 'Bob' - This swaps 'Charlie' into Bob's slot to keep the array dense
      (SparseSet.remove! &set 2u32)
      
      (println* "Length after removal: " (SparseSet.length &set)) ;; 2
      (println* "Remaining: " (str &set)))))
```
