# Introduction
- $where - Matches documents that satisfy a JavaScript expression.
```
{ $where: "this.age > 18" }
```
- $ne - Matches all values that are not equal to a specified value.
```
{ role: { $ne: "user" } }
```
- $in - Matches all of the values specified in an array.
```
{ category: { $in: ["Gifts", "Books"] } }

```
- $regex - Selects documents where values match a specified regular expression.
1. Indica que el valor empieza con adm: ```{ username: { $regex: "^adm" } }```
2. Indica que el valor acaba con in: ```{ username: { $regex: "in$" } }```

NOte: Only it if necessary 
1. Convert the request method from GET to POST.
2. Change the Content-Type header to application/json.
3. Add JSON to the message body.
4. Inject query operators in the JSON
