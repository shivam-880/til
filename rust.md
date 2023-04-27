## Difference HashSet
```rust
let mut last = HashSet::<&u64>::new();
        last.insert(&1);
        last.insert(&2);
        last.insert(&3);
        last.insert(&4);

        let mut curr = HashSet::<&u64>::new();
        curr.insert(&1);
        curr.insert(&2);
        curr.insert(&3);
        curr.insert(&4);
        curr.insert(&5);

        let difference: Vec<_> = curr.iter().filter(|item| !last.contains(*item)).collect();
        println!("difference = {:?}", difference);
```
