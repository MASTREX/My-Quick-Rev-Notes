> Only has points that I may forget.

> Message to author: If you didn't found something you forgot, Add it!

## Basic points
- Json can be added directly in yaml. As YAML is, in fact, a superset of JSON. All JSON files are valid YAML files, but not the other way around.

  `martin: {name: Martin D'vloper, job: Developer, skill: Elite}`
- A space followed by the pound sign " #" starts a comment.
- Indentation can be of any size but it must be consistent.
  ```yaml
  maze:                  # this is correct
        - left
        - right
        - up
        - down
  ```
- `' '` or `" "` quotes are used if string literal contains special characters ``: ' " [] {} > | * & ! % # ` @ , -- < = \ `` and `? : -` (should be followed by non-space char to be treated as string). To insert these use either unicode or quotes

## Multiline fold with `|` and `>`
- Indentation spaces are ignore and only used for block or scope of multiline
- `|>` can be followed by ChompsOff characters.ChompOff `+` to preserve the final whitespace character at ending and `-` to remove it.
```yaml
# > fold with newline converted to spaces
message: >
 even though
 it looks like
 this is a multiline message,
 it is actually not

equivalentTo: "even though it looks like this is a multiline message,it is actually not"
```
```yaml
# | fold with newline preserved
message: |
  this is
  a real multiline
  message

equivalentTo: "this is\na real multiline\nmessage\n"
```

## Anchor `&` and Alias `*`

```yaml
---
vars:
   service1:
       config: &service_config
           env: prod
           retries: 3
           version: 4.8
   service2:
       config: *service_config
           version: 5
   service3:
       config:          # overriding anchor
           <<: *service_config
           version: 4.2
...
```


## Schema parser
- **FailSafe Schema** understands only maps, sequences, and strings and is guaranteed to work with any YAML file.
- **JSON schema** understands all types supported within JSON, including boolean, null, int, and float, as well as those in the FailSafe schema.
- **Core schema** is an extension of the JSON schema, making it more human-readable supporting the same types but in multiple forms.

  For example: 1. null | Null | NULL will all be resolved to the same type null, and true | True | TRUE will all be resolved to the same boolean value.

## Tags or Expiclit type decleration
```yaml
---
# A sample yaml file
company: !!str spacelift
domain:
 - !!str devops
 - !!str devsecops
tutorial:
   - name: !!str yaml
   - type: !!str awesome
   - rank: !!int 1
   - born: !!int 2001
author: !!str omkarbirade
published: !!bool true
scalars:
 - !!str true
 - random
```