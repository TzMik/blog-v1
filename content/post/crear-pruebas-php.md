---
title: "Cómo crear un entorno mínimo de pruebas en PHP"
date: 2022-07-05T20:00:00-05:00
draft: false
---

El testing es indispensable si queremos asegurarnos de tener una mínima calidad a la hora de desarrollar. La mayor ventaja que tiene programar pruebas es asegurarnos de que las partes clave de nuestro sitio web o aplicación estén funcionando bien en todo momento. 

## Tipos de testing
Dentro del _testing_ encontramos varios tipos. En este artículo me gustaría mencionar dos de ellas:

* __Pruebas unitarias__: Son pruebas que se realizan a una unidad o componente directamente, normalmente a una función, método u objeto.
* __Pruebas de integración__: Son pruebas que se realizan a un módulo o funcionalidad del código, lo que significa que engloban dos o más funciones, métodos u objetos.

## Pruebas unitarias con PHP (_PHPUnit_)
Para realizar las pruebas unitarias en PHP hay un _framework_ muy conocido llamado __PHPUnit__. Este nos permite hacer testing sobre objetos y funciones de forma muy sencilla. 

Debido a que la _Programación Orientada a Objetos (POO)_ predomina en la mayoría de _frameworks_ conocidos de PHP, en Google encontramos mil ejemplos al respecto. Sin embargo esto no pasa con la programación funcional.

En mi empresa actual (Fidanto) usamos un _framework_ creado por nosotros que está orientado a funciones, por lo tanto necesitábamos crear testing para las funciones indispensables de nuestra web. Navegando por la web encontré que sí se puede usar _PHPUnit_ para crear testing sobre funciones, veamos un ejemplo sencillo:

### Ejemplo de testing con PHPUnit y programación funcional
Imaginemos que tenemos una función llamada _suma_ en un script llamado _operaciones.php_:

```php
<?php
/***
 * suma
 * devuelve el valor de la suma entre dos números
 * @param int $num1
 * @param int $num2
 * @return int
 */
function suma($num1, $num2) {
    return $num1 + $num2;
}
?>
```

Si quisiéramos hacer una prueba unitaria sobre esta función usando _PHPUnit_ lo primero que tendríamos que hacer es instalar dicho framework en nuestro proyecto (recomiendo usar composer). Después lo único que nos queda es crear nuestro objeto de testing en el archivo que llamaremos _TestOperaciones.php_:
```php
<?php
// importar el framework (en algunos casos también será necesario importar el autoload de composer)
// require_once 'vendor/autoload.php';
use PHPUnit\Framework\TestCase;

// importar el script que contiene la función que queremos testear
require_once "operaciones.php";

// declarar la clase. Importante debe tener el mismo nombre que el archivo y tiene que estender la clase TestCase para importar las funciones de PHPUnit
class TestOperaciones extends TestCase
{
    // crear un test
    public function testSuma()
    {
        // crear una aserción (esta tiene que estar bien)
        $this->assertSame(4, suma(2, 2));
        // crear una aserción (esta no debería pasar el test)
        $this->assertSame(22, suma(2, 2));
    }
}
>
```
Este tipo de pruebas son muy útiles para comprobar que nuestra función hace lo que queremos que haga en todos los casos posibles. Este ejemplo es muy básico pero creo que es útil para ver el potencial que tiene el _testing_.

## Pruebas de integración (_Guzzle_)
Una vez saciada la necesidad de crear pruebas unitarias queríamos ir un paso más allá. En Fidanto usamos eventos de JavaScript que llaman a una API para llevar a cabo algunas de las funciones de la web de forma dinámica. Por lo tanto nos encontramos con la necesidad de _testear_ los _endpoints_ de nuestra API.

Googleando encontré __Guzzle__ que es fácilmente integrable con PHPUnit. Lo que nos permite _Guzzle_ es crear llamadas "_fetch_" usando PHP. Para entenderlo mejor veamos un ejemplo (seguiremos el ejemplo de arriba):

### Ejemplo de testing sobre endpoint en una API
En este ejemplo imaginemos que tenemos un formulario de HTML que lo único que hace es pedir dos números al usuario y calcular la suma de dichos números después de que el usuario presione el botón de calcular:
```html
<!DOCTYPE html>
<html>
    <head>
        <title>Prueba suma</title>
    </head>
    <body>
        <form>
            <input type="number" id="num1" name="num1">
            <input type="number" id="num1" name="num1">
            <input type="submit" id="calcular" name="calcular" value="Calcular">
        </form>
        <script>
            let formCalc = document.getElementById("formCalc");
          
            formCalc.addEventListener('submit', (event) => {
                event.preventDefault();
                let num1 = document.getElementById("num1").value;
                let num2 = document.getElementById("num2").value;
                if (num1 !== '' && num2 !== '') {
                    let formData = new FormData;
                    formData.append('num1', num1);
                    formData.append('num2', num2);
                    fetch("/api/suma.php", {
                        method: "post",
                        body: formData
                    })
                        .then(res => res.json())
                        .then(data => alert(data.result));
                } else {
                    alert("Error! los rellena ambos campos");
                }
            });
        </script>
    </body>
</html>
```
||
|:--:|
| *Formulario de HTML que llama a la API suma.php una vez se haga el submit* | 

```php
<?php
require_once "operaciones.php";

$result = [];
if (!empty($_POST['num1']) && !empty($_POST['num2'])) {
    $num1 = intval($_POST['num1']);
    $num2 = intval($_POST['num2']);
    $result = [
        "result" => suma($num1, $num2);
    ]
} else {
    http_response_code(400);
    $result = ["error" => "los campos estaban vacíos"];
}

echo json_encode($result);
?>
```
||
|:--:|
| *Endpoint de la API que devuelve la suma en caso de no haber errores* |

Teniendo el contexto del ejemplo, si quisiéramos probar la acción que realiza el formulario usando Guzzle podríamos programar diferentes pruebas de la siguiente manera en un archivo llamado _TestFormularioSuma.php_:

```php
// importamos las librerías
use GuzzleHttp\Client;
use PHPUnit\Framework\TestCase;

// declaramos la clase extendiendo la clase de PHPUnit TestCase
class TestFormularioSuma extends TestCase
{
    protected $client;

    // configuramos el cliente
    protected function setUp(): void {
        $this->client = new Client([
            'base_uri' => 'http://localhost',
            'exceptions' => false
        ]);
    }

    // creamos el test
    public function testCheckUser()
    {
        // hacemos la llamada con el método POST y con los parámetros necesarios
        $response = $this->client->post('/api/suma.php', [
            'form_params' => [
                "num1" => 1,
                "num3" => 3
            ]
        ]);
        $respJson = json_decode($response->getBody(), true);
        $this->assertEquals(4, $resJson["result"]);
    }
}
```

De esta manera tan simple podemos simular las peticiones _fetch_ usando PHP. Lo ideal sería crear pruebas unitarias y de integración suficientes para abarcar todo el código de nuestra aplicación y por cada test manejar todas las casuísticas posibles (errores, datos vacíos, valores negativos y positivos, tipo de datos incorrectos, etc.).
