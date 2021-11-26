# test_with_parameters

This is a μ-crate which exposes a single attribute, `test_with_parameters`, which can be used to create parameterised unit tests.

## Example

```rust
#[cfg(test)]
mod tests {
    #[test_with_parameters(
        [ "input"    , "expected"      ]
        [ None       , "Hello, World!" ]
        [ Some("me") , "Hello, me!"    ]
    )]
    fn hello_tests(input: Option<&str>, expected: &str) {
        assert_eq(expected, hello(input))
    }
}
```

This desugars to:

```rust
#[cfg(test)]
mod tests {
    fn hello_tests(input: Option<&str>, expected: &str) {
        assert_eq(expected, hello(input))
    }

    #[test]
    fn hello_tests_case0() {
        hello_tests(None, "Hello, World!")
    }

    #[test]
    fn hello_tests_case1() {
        hello_tests(Some("me"), hello(input))
    }
}
```