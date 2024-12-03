# Rust Naming Convention



### General conventions [RFC #430]

In general, Rust tends to use `CamelCase` for "type-level" constructs

- Types
- Structs
- Traits
- Enum variants

이때는 대문자로 시작

acronyms count as one word: use `Uuid` rather than `UUID`



`snake_case` for "value-level" constructs

- Functions
- Methods
- Local variables
- acronyms are lower-cased: `is_xid_start`.

`SCREAMING_SNAKE_CASE`

- Static variables
- Constant variables

 a "word" should never consist of a single letter unless it is the last "word": `btree_map` rather than `b_tree_map`



constructor

- General constructors : `new` or `with_more_details`
- Conversion constructors : from_some_other_type



Crates & Modules

snake_case (but prefer single word)







### Referring to types in function/method names [RFC 344]

Function names often involve type names (ex. `as_slice`)

If the type has a purely textual name (ignoring parameters), it is straightforward to convert between type conventions and function conventions



| Type name  | Text in methods |
| ---------- | --------------- |
| `String`   | `string`        |
| `Vec<T>`   | `vec`           |
| `YourType` | `your_type`     |
| `&str`     | `str`           |
| `&[T]`     | `slice`         |
| `&mut [T]` | `mut_slice`     |
| `&[u8]`    | `bytes`         |
| `&T`       | `ref`           |
| `&mut T`   | `mut`           |
| `*const T` | `ptr`           |
| `*mut T`   | `mut_ptr`       |

There is some overlap on these rules; apply the most specific applicable rule:



### Avoid redundant prefixes [RFC 356]

Names of items within a module should not be prefixed with that module's name

```rust
mod foo {
    pub struct Error { ... } // O
}
```

```rust
mod foo {
    pub struct FooError { ... } // X
}
```

Library clients can rename on import to avoid clashes.



### Getter/setter methods [RFC 344]

Getter : 필드이름

Setter : set_필드이름



The convention for a field `foo: T` is:

- A method `foo(&self) -> &T` for getting the current value of the field.
- A method `set_foo(&self, val: T)` for setting the field. (The `val` argument here may take `&T` or some other type, depending on the context.)

`



### Predicates

Simple boolean predicates should be prefixed with `is_` or another short question word

`is_empty`

Common exceptions: `lt`, `gt`, and other established predicate names.

