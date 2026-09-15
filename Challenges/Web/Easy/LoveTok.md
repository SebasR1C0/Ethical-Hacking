Entramos al link y vemos que hay un parametro que puede ser manipulado por el usuario. Además es el única forma que el usuario tiene control, asi que hay que buscar en el código fuente como manipular esto para hallar un rce
<img width="1420" height="816" alt="image" src="https://github.com/user-attachments/assets/1750cc0b-df7a-4a2f-8807-4ddc0071d5ff" />

Vemos que el valor del parámetro pasa por un addslashes y despues va al eval. Tomar en cuenta que addslashes solo modifica estos caracteres especiales ' " \ y el byte NUL — pero no modifica $. Por eso podemos usar interpolación de variables ${...} dentro de las comillas dobles del eval(), lo cual hace que PHP ejecute la expresión interna como código.
<img width="1051" height="465" alt="image" src="https://github.com/user-attachments/assets/23be4d71-7045-4f7c-90ca-f395c491df38" />

Por eso en el parametro colocamos: ``` format=${system($_GET[1])}&1=id ```
<img width="1399" height="786" alt="image" src="https://github.com/user-attachments/assets/1aedf20b-121a-4bc3-bea6-bb8fce28249f" />
