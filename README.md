# Guia de instalación de la Base de Datos
## Crear la Base de Datos
En la carpeta DB, esta el archivo `cesur.sql`. Este es el archivo que debemos importar en el phpMyAdmin. Una vez creada la base de datos continuamos con la configuración del conector a la misma.

## Configuración del conector
Para la utilización actual de los Web Services debemos configurar una base de datos. Para ello primero configuramos el archivo `db.php`. Este archivo es el conector a nuestra base de datos. Dentro de el, debemos editar los campos `$dsn, $user y $pass` que hay en la clase `DB_Configuration`. Completando con los datos de nuestra Base de Datos.
```
  public $dsn = "mysql:host=localhost;dbname=cesur";
  public $user = "root";
  public $pass = "";
```
Una vez cambiados los campos, los Web services seran funcionales.

# Guia de uso de los Web Services
## Rutas y métodos
| Método | Nombre          | Ruta                              |
|------- |-----------------|-----------------------------------|
| `POST` | signin.php      | **API/jwt/signin.php**            |
| `POST` | frases.php      | **API/frases.php**                |
| `POST` | mediasexcel.php | **API/php-excel/mediasexcel.php** |
| `POST` | raexcel.php     | **API/php-excel/raexcel.php**     |

## signin.php
Este archivo es el encargado de crear el token jwt, para ello necesita leer un archivo `JSON` con la siguiente estructura:  
```
{
    "email":"xxx",
    "password": "xxx"
}
```
Al hacer uso de este web service envia al servidor una respuesta tambien en `JSON` como la siguiente:
```
{
    "message": "Successful login",
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJsb2NhbGhvc3QiLCJhdWQiOiJUSEVfQVVESUVOQ0UiLCJpYXQiOjE3MDYwODMxODcsIm5iZiI6MTcwNjA4MzE5NywiZXhwIjoxNzA2MDgzMjQ3LCJkYXRhIjp7ImRuaSI6IjExMTIyMjMzMyIsIm5vbWJyZSI6IkV2YSIsImFwZWxsaWRvMSI6IkV2YW5zIiwiYXBlbGxpZG8yIjoiTWlsbGVyIiwiRW1haWwiOiJldmEuZUBleGFtcGxlLmNvbSJ9fQ.TRHoI_gXwLmbaflaUa9iZzGvoR_KmQ9bR34EgwVJQwA",
    "email_address": "eva.e@example.com",
    "expire": 1706083247
}
```

## frases.php
Este web service devuelve al front-end una frase aleatoria, de una tabla con frases motivadoras de nuestra base de datos. Codificandola en `JSON` con el siguiente formato:
``` 
{
    "frase": "No importa lo lento que vayas, siempre y cuando no te detengas."
}
```

## mediasexcel.php
Este WS lee todos los archivos Excel (`.xlsx`) segun el año y una id de usuario. Tambien **necesita obtener** mediante las cabeceras HTTP un token valido.
```
{
    "ano":"2324",
    "id":1
}
``` 
Devolvera un `JSON`con las asignaturas donde se encuentra matriculado ese usuario y la media de la correspondiente asignatura
```
{
    "DWES": {
        "Media": 3.0833333333333335
    },
    "DWEC": {
        "Media": 3.0833333333333335
    },
    "DAW": {
        "Media": 3.0833333333333335
    },
    "DIW": {
        "Media": 3.0833333333333335
    }
}
```

## raexcel.php
Este archivo realiza un web service que envia un `JSON` con las notas de todos los RA y sus respectivos porcentajes. Para ello necesita obtener el modulo, el año del curso y la id del alumno. Al igual que `mediasexcel.php` necesita tambien un token valido obtenido desde las cabeceras HTTP.
```
{
    "modul":"DWES",
    "ano":"2324",
    "id":1
}
```
El resultado sera codificado y enviado en `JSON` con el siguiente formato:
```
{
    "Notas": {
        "RA1": {
            "Nota": 4,
            "Porcentaje": 0.111
        },
        "RA2": {
            "Nota": 5.29,
            "Porcentaje": 0.111
        },
        "RA3": {
            "Nota": 5.92,
            "Porcentaje": 0.111
        },
        "RA4": {
            "Nota": 5.26,
            "Porcentaje": 0.111
        },
        "RA5": {
            "Nota": 6.29,
            "Porcentaje": 0.111
        },
        "RA6": {
            "Nota": 6.54,
            "Porcentaje": 0.111
        },
        "RA7": {
            "Nota": 0,
            "Porcentaje": 0.111
        },
        "RA8": {
            "Nota": 0,
            "Porcentaje": 0.111
        },
        "RA9": {
            "Nota": 0,
            "Porcentaje": 0.111
        }
    }
}
```