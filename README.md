# Rust Foundation Project Grant 2022

This repository is used to track information about my 
[Rust Foundation Project Grant](https://foundation.rust-lang.org/news/2022-06-14-community-grants-program-awards-announcement/) 
to improve error messages emitted by rustc for trait heavy crates. 
As part of this work I will focus on the following points:

1. Collect and categorise various examples of non-optimal error messages from the rust ecosystem. 
This includes error messages generated by crates like diesel, axum or nalgebra which relay heavily
on complex trait bounds 
2. Experiment with example cases to see which error messages could be improved by 
the usage of `#[rustc_on_unimplemented]`
3. Implement [RFC-2397](https://github.com/rust-lang/rfcs/blob/master/text/2397-do-not-recommend.md)
4. Experiment with example cases to see which error messages could be improved by
the usage of `#[do_not_recommend]`

## Call for participation

Please submit examples of bad error messages in the context of trait heavy crates 
as issue or PR (with minimal example) to this repository.


## Test cases

| crate           | test case                     | error type                                       |
|-----------------|-------------------------------|--------------------------------------------------|
| [uom]           | [type_mismatch.rs]            | type mismatch                                    |
| [typed_builder] | [mismatch.rs]                 | type mismatch + missing free standing function   |
| [easy_ml]       | [recursion.rs]                | type recursion                                   |
| [diesel]        | [bad_insertable_field.rs]     | trait not implemented + misleading wildcard impl |
| [diesel]        | [bad_sql_query.rs]            | trait not implemented                            |
| [diesel]        | [invalid_query.rs]            | traits not implemented + "duplicated errors"     |
| [diesel]        | [queryable_order_mismatch.rs] | trait not implemented with large types           |
| [chumsky]       | [json.rs]                     | associated type mismatch                         |
| [bevy]          | [system_mismatch.rs]          | trait not implemented + HRTB error               |
| [axum]          | [argument_not_extractor.rs]   | debug_handler                                    |
| [axum]          | [extract_self_mut.rs]         | debug_handler                                    |
| [axum]          | [extract_self_ref.rs]         | debug_handler                                    |
| [axum]          | [generics.rs]                 | debug_handler                                    |
| [axum]          | [invalid_attrs.rs]            | debug_handler                                    |
| [axum]          | [missing_deserialize.rs]      | trait not implemented                            |
| [axum]          | [multiple_body_extractors.rs] | debug_handler                                    |
| [axum]          | [multiple_paths.rs]           | debug_handler                                    |
| [axum]          | [not_a_function.rs]           | debug_handler                                    |
| [axum]          | [not_async.rs]                | debug_handler                                    |
| [axum]          | [not_send.rs]                 | debug_handler                                    |
| [axum]          | [request_not_last.rs]         | debug_handler                                    |
| [axum]          | [too_many_extractors.rs]      | debug_handler                                    |
| [axum]          | [wrong_return_type.rs]        | debug_handler                                    |


[uom]: https://crates.io/crates/uom
[typed_builder]: https://crates.io/crates/typed_builder
[easy_ml]: https://crates.io/crates/easy_ml
[diesel]: https://crates.io/crates/diesel
[chumsky]: https://crates.io/crates/chumsky
[bevy]: https://crates.io/crates/bevy
[axum]: https://crates.io/crates/axum

[type_mismatch.rs]: #uom_type_mismatch
[mismatch.rs]: #typed_builder_mismatch
[recursion.rs]: #easy_ml_recursion
[bad_insertable_field.rs]: #diesel_bad_insertable
[bad_sql_query.rs]: #diesel_bad_sql_query
[invalid_query.rs]: #diesel_invalid_query
[queryable_order_mismatch.rs]: #diesel_queryable
[json.rs]: #chumsky_json
[system_mismatch.rs]: #bevy_system_mismatch
[argument_not_extractor.rs]: #axum_argument_not_extractor
[extract_self_mut.rs]: #axum_extract_self_mut
[extract_self_ref.rs]: #axum_extract_self_ref
[generics.rs]: #axum_generics
[invalid_attrs.rs]: #axum_invalid_attrs
[missing_deserialize.rs]: #axum_missing_deserialize
[multiple_body_extractors.rs]: #axum_multiple_body_extractors
[multiple_paths.rs]: #axum_multiple_paths
[not_a_function.rs]: #axum_not_a_function
[not_async.rs]: #axum_not_async
[not_send.rs]: #axum_not_send
[request_not_last.rs]: #axum_not_last
[too_many_extractors.rs]: #axum_too_many_extractors.rs]
[wrong_return_type.rs]: #axum_wrong_return_type

### uom

#### type_mismatch.rs <a name="uom_type_mismatch></a>

Versions:

| version | link (code)                            | link (error message)                           | change since last version |
|---------|----------------------------------------|------------------------------------------------|---------------------------|
| 1       | [type_mismatch.rs][type_mismatch.rs-1] | [type_mismatch.stderr][type_mismatch.stderr-1] |                           |

[type_mismatch.rs-1]: https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/uom/type_mismatch.rs
[type_mismatch.stderr-1]: https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/uom/type_mismatch.stderr


### typed_builder

Typed builder generates hidden types/deprecating messages to improve its error messages

#### mismatch.rs <a name="typed_builder_mismatch"></a>
 
 Versions: 
 
 | version | link (code)                  | link (error message)                 | change since last version |
 |---------|------------------------------|--------------------------------------|---------------------------|
 | 1       | [mismatch.rs][mismatch.rs-1] | [mismatch.stderr][mismatch.stderr-1] |                           |

[mismatch.rs-1]: https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/typed_builder/mismatch.rs
[mismatch.stderr-1]: https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/typed_builder/mismatch.stderr

### easy_ml

#### recursion.rs <a name="easy_ml_recursion"></a>

Versions:

 | version | link (code)                    | link (error message)                   | change since last version |
 |---------|--------------------------------|----------------------------------------|---------------------------|
 | 1       | [recursion.rs][recursion.rs-1] | [recursion.stderr][recursion.stderr-1] |                           |

[recursion.rs-1]:https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/easy_ml/recursion.rs
[recursion.stderr-1]: https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/easy_ml/recursion.stderr


### diesel

#### bad_insertable_field.rs <a name = "diesel_bad_insertable"></a>

 | version | link (code)                                    | link (error message)                                  | change since last version |
 |---------|------------------------------------------------|-------------------------------------------------------|---------------------------|
 | 1       | [bad_insertable_field.rs][bad_insertable.rs-1] | [bad_insertable_field.stderr][bad_sql_query.stderr-1] |                           |

[bad_insertable.rs-1]:https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/diesel/bad_insertable_field.rs
[bad_insertable.stderr-1]: https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/diesel/bad_insertable_field.stdr


#### bad_sql_query.rs <a name = "diesel_bad_sql_query"></a>

 | version | link (code)                    | link (error message)                   | change since last version |
 |---------|--------------------------------|----------------------------------------|---------------------------|
 | 1       | [bad_sql_query.rs][bad_sql_query.rs-1] | [bad_sql_query.stderr][bad_sql_query.stderr-1] |                           |

[bad_sql_query.rs-1]:https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/bad_sql_query.rs
[bad_sql_query.stderr-1]: https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/diesel/bad_sql_query.stderr


#### invalid_query.rs <a name="diesel_invalid_query"></a>

 | version | link (code)                    | link (error message)                   | change since last version |
 |---------|--------------------------------|----------------------------------------|---------------------------|
 | 1       | [invalid_query.rs][invalid_query.rs-1] | [invalid_query.stderr][invalid_query.stderr-1] |                           |

[invalid_query.rs-1]:https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/diesel/invalid_query.rs
[invalid_query.stderr-1]: https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/diesel/invalid_query.stderr


#### queryable_order_mismatch.rs <a name = "diesel_queryable"></a>

 | version | link (code)                    | link (error message)                   | change since last version |
 |---------|--------------------------------|----------------------------------------|---------------------------|
 | 1       | [queryable_order_mismatch.rs][queryable_order_mismatch.rs-1] | [queryable_order_mismatch.stderr][queryable_order_mismatch.stderr-1] 

[queryable_order_mismatch.rs-1]:https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/diesel/queryable_order_mismatch.rs
[queryable_order_mismatch.stderr-1]: https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/diesel/queryable_order_mismatch.stderr

### chumsky

#### json.rs <a name="chumsky_json"></a>


 | version | link (code)          | link (error message)          | change since last version |
 |---------|----------------------|-------------------------------|---------------------------|
 | 1       | [json.rs][json.rs-1] | [json.stderr][json.stderr-1]  |                           |

[json.rs-1]:https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/chumsky/json.rs
[json.stderr-1]: https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/chumsky/json.stderr

### bevy

#### system_mismatch.rs <a name="bevy_system_mismatch"></a>


 | version | link (code)                                | link (error message)                               | change since last version |
 |---------|--------------------------------------------|----------------------------------------------------|---------------------------|
 | 1       | [system_mismatch.rs][system_mismatch.rs-1] | [system_mismatch.stderr][system_mismatch.stderr-1] |                           |

[system_mismatch.rs-1]:https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/bevy/system_mismatch.rs
[system_mismatch.stderr-1]: https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/bevy/system_mismatch.stderr

### axum

axum provides a `#[debug_handler]` attribute which emits better error messages is some cases

#### argument_not_extractor.rs <a name="axum_argument_not_extractor"></a>

 | version | link (code)                                    | link (error message)                                   | change since last version |
 |---------|------------------------------------------------|--------------------------------------------------------|---------------------------|
 | 1       | [argument_not_extractor.rs][argument_not_extractor.rs-1] | [argument_not_extractor.stderr][argument_not_extractor.stderr-1] |                           |

[argument_not_extractor.rs-1]:https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/axum/argument_not_extractor.rs
[argument_not_extractor.stderr-1]: https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/axum/argument_not_extractor.stderr

#### missing_deserialize.rs <a name="axum_missing_deserialize"></a>

 | version | link (code)                                        | link (error message)                                       | change since last version |
 |---------|----------------------------------------------------|------------------------------------------------------------|---------------------------|
 | 1       | [missing_deserialize.rs][missing_deserialize.rs-1] | [missing_deserialize.stderr][missing_deserialize.stderr-1] |                           |


[missing_deserialize.rs-1]:https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/axum/missing_deserialize.rs
[missing_deserialize.stderr-1]: https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/axum/missing_deserialize.stderr


#### wrong_return_type.rs <a name="axum_wrong_return_type"></a>

 | version | link (code)                                    | link (error message)                                   | change since last version |
 |---------|------------------------------------------------|--------------------------------------------------------|---------------------------|
 | 1       | [wrong_return_type.rs][wrong_return_type.rs-1] | [wrong_return_type.stderr][wrong_return_type.stderr-1] |                           |

[wrong_return_type.rs-1]:https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/axum/wrong_return_type.rs
[wrong_return_type.stderr-1]: https://github.com/weiznich/rust-foundation-community-grant/blob/883de46cbea5873bcc4af60e47f872efaa77a2b7/test_cases/tests/axum/wrong_return_type.stderr
