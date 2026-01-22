# LDAP Search Filter Syntax

| Name               | Operand | Example           | Example Description                                                                 |
|--------------------|---------|-------------------|--------------------------------------------------------------------------------------|
| Equality           | =       | (name=Kaylie)     | Matches all entries that contain a name attribute with the value Kaylie              |
| Greater-Or-Equal   | >=      | (uid>=10)         | Matches all entries that contain a uid attribute with a value greater than or equal to 10 |
| Less-Or-Equal      | <=      | (uid<=10)         | Matches all entries that contain a uid attribute with a value less than or equal to 10 |
| Approximate Match  | ~=      | (name~=Kaylie)    | Matches all entries that contain a name attribute with approximately the value Kaylie |

