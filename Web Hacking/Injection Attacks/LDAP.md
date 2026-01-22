# LDAP Search Filter Syntax

| Name               | Operand | Example           | Example Description                                                                 |
|--------------------|---------|-------------------|--------------------------------------------------------------------------------------|
| Equality           | =       | (name=Kaylie)     | Matches all entries that contain a name attribute with the value Kaylie              |
| Greater-Or-Equal   | >=      | (uid>=10)         | Matches all entries that contain a uid attribute with a value greater than or equal to 10 |
| Less-Or-Equal      | <=      | (uid<=10)         | Matches all entries that contain a uid attribute with a value less than or equal to 10 |
| Approximate Match  | ~=      | (name~=Kaylie)    | Matches all entries that contain a name attribute with approximately the value Kaylie |
|------|----------|--------------------------------------|--------------------------------------------------------------------------------------|
| And  | (&()())  | (&(name=Kaylie)(title=Manager))      | Matches all entries that contain a name attribute with the value Kaylie and a title attribute with the value Manager |
| Or   | (|()())  | (|(name=Kaylie)(title=Manager))      | Matches all entries that contain a name attribute with the value Kaylie or a title attribute with the value Manager |
| Not  | (!())    | (!(name=Kaylie))                     | Matches all entries that contain a name attribute with a value different from Kaylie |

Note: And and Or filters support more than two arguments. For instance, (&(attr1=a)(attr2=b)(attr3=c)(attr4=d)) is a valid search filter.
