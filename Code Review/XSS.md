# SanitizaciONO
Cuando el codigo no sanitiza el input del usuario
```
<?php echo $_GET['name']?>
```
Utilizando sanitizacion simple
```
<?php echo htmlspecialchars($_GET['name'], ENT_QUOTES, 'UTF-8'); ?>
```
