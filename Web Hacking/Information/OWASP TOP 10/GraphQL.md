# GraphQL
- Example
```bash
{
  "data": {
    "users": [
      {
        "id": 1,
        "username": "htb-stdnt",
        "role": "user"
      },
      {
        "id": 2,
        "username": "admin",
        "role": "admin"
      }
    ]
  }
}
```
# Identifying
<img width="1546" height="507" alt="image" src="https://github.com/user-attachments/assets/2cc5b993-d3cb-4838-8ce4-f768190127d2" />

Note: there is a automated tool called [graphw00f](https://github.com/dolevf/graphw00f)

## Introspection
```bash
{__schema {types {name}}}
# OR
{__schema {queryType {fields {name description}}}}
```
<img width="1110" height="289" alt="image" src="https://github.com/user-attachments/assets/1b4209b1-6b67-400e-93f6-975cb39366d1" />

Once got the information:
```bash
{__type(name: "UserObject") {name fields {name type {name kind}}}}
```

## Get Information
```bash
query IntrospectionQuery { __schema { queryType { name } mutationType { name } subscriptionType { name } types { ...FullType } directives { name description locations args { ...InputValue } } } } fragment FullType on __Type { kind name description fields(includeDeprecated: true) { name description args { ...InputValue } type { ...TypeRef } isDeprecated deprecationReason } inputFields { ...InputValue } interfaces { ...TypeRef } enumValues(includeDeprecated: true) { name description isDeprecated deprecationReason } possibleTypes { ...TypeRef } } fragment InputValue on __InputValue { name description type { ...TypeRef } defaultValue } fragment TypeRef on __Type { kind name ofType { kind name ofType { kind name ofType { kind name ofType { kind name ofType { kind name ofType { kind name ofType { kind name } } } } } } } }
```

# IDOR
## Reconnaice
```bash
{ __type(name: \"SecretObject\") { name fields { name type { name kind } } } }
```
