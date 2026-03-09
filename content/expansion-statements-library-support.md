---
execute: true
---

## What It Does

`std::integer_sequence` can now be used in `template for` to conveniently write a loop 
where the loop index is a `constexpr` variable. Additionally it can be used to create a 
`constexpr` pack in the current function scope through structured bindings.

This is made possible by implementing the tuple protocol for `std::integer_sequence` (
specializing `std::tuple_size` + `std::tuple_element` and providing overloads for `get`).

## Why It Matters

These changes obsolete the common metaprogramming idiom of using an immediately invoked lambda 
expression to introduce a pack of constant indices. This is especially interesting when operating
on multiple packs (or tuple-likes) of equal size at the same time.

This change has intended symbiotic effects with
- P1306 expansion statements (`template for`)
- P2662 pack indexing (`Pack...[Idx]`)
- P1061 structured bindings can introduce a pack + P2686 constexpr structured bindings
- P3096 function parameter reflection (IILE trick cannot be used here)


## Example

```cpp
#include <cassert>
#include <utility>
#include <tuple>


// before C++26
template <typename... Ts>
bool is_eq_iile(std::tuple<Ts...> lhs, std::tuple<Ts...> rhs) {
    return [&]<std::size_t... Idx>(std::index_sequence<Idx...>) {
        return ((get<Idx>(lhs) == get<Idx>(rhs)) && ...);
    }(std::index_sequence_for<Ts...>());
}

// with structured bindings
template <typename... Ts>
bool is_eq_fold(std::tuple<Ts...> lhs, std::tuple<Ts...> rhs) {
    static constexpr auto [...Idx] = std::index_sequence_for<Ts...>();
    return ((get<Idx>(lhs) == get<Idx>(rhs)) && ...);
}

// with expansion statements
template <typename... Ts>
bool is_eq_template_for(std::tuple<Ts...> lhs, std::tuple<Ts...> rhs) {
    template for (constexpr auto Idx : std::index_sequence_for<Ts...>()) {
        if (get<Idx>(lhs) != get<Idx>(rhs)) {
            return false;
        }
    }
    return true;
}

int main() {
    auto tup = std::tuple{1, 3.14, "hello"};
    auto tup2 = std::tuple{2, 3.14, "hell"};

    assert(!is_eq_iile(tup, tup2));
    assert(!is_eq_fold(tup, tup2));
    assert(!is_eq_template_for(tup, tup2));

    assert(is_eq_iile(tup, tup));
    assert(is_eq_fold(tup, tup));
    assert(is_eq_template_for(tup, tup));
}
```
