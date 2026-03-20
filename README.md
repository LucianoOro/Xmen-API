## Instrucciones y Características

Esta API ha sido diseñada para detectar si un humano es mutante a partir de su secuencia de ADN. El sistema se basa en patrones específicos dentro de la secuencia de ADN y permite almacenar cada análisis en una base de datos para futuras referencias.

### Características

- **Base de Datos**: Utiliza una base de datos H2 embebida para almacenamiento rápido y eficiente.
- **Hosting**: La API está desplegada en Render y puede ser accesible en la siguiente URL: [https://xmenparcial1.onrender.com](https://xmenparcial1.onrender.com).

---

## Endpoints

### Detectar Mutante

- **Ruta**: `/api/v1/dna/mutant`
- **Método**: `POST`
- **Descripción**: Este endpoint permite verificar si una secuencia de ADN corresponde a un mutante. La secuencia de ADN se almacena en la base de datos junto con el resultado del análisis.

#### Ejemplo de Uso
Para detectar si una secuencia de ADN corresponde a un mutante, realiza una solicitud HTTP `POST` en formato JSON a la URL:

https://xmenparcial1.onrender.com/api/v1/dna/mutant

#### Cuerpo de la Solicitud (Ejemplo de ADN No Mutante)
`json
{
    "dna": [
        "ATGCGA",
        "CAGTGC",
        "TTATGT",
        "AGACGG",
        "CAGCTA",
        "TCACTG"
    ]
}`


Si la secuencia de ADN enviada no pertenece a un mutante, la API devolverá una respuesta 403 Forbidden.

Cuerpo de la Solicitud (Ejemplo de ADN Mutante)

`json
{
    "dna": [
        "ATGCGA",
        "CAGTGC",
        "TTATGT",
        "AGAAGG",
        "CCCATA",
        "TCACTG"
    ]
}`
Si la secuencia de ADN corresponde a un mutante, la API devolverá una respuesta 200 OK.

#### Manejo de Errores
Si la matriz de ADN enviada no es de tipo NxN o contiene caracteres no válidos (es decir, que no sean 'A', 'T', 'C' o 'G'), la respuesta será:


    json
    {
     //Si la matriz no es cuadrada:
        "error": "La matriz de ADN no es de tamaño NxN."
     //Si se ingresan caracteres no validos:
    	"error": "El ADN contiene caracteres no válidos"
    }
Este mensaje indica que la secuencia de ADN no cumple con el formato requerido.

------------


### Obtener Estadísticas de ADN (Servicio /stats)

- **Ruta**: `/api/v1/dna/stats`
- **Método**: `GET`
- **Descripción**: Este endpoint proporciona estadísticas sobre el ADN almacenado en la base de datos. La respuesta incluye el número total de ADN humanos, el número de ADN mutantes y el ratio de mutantes a humanos.

#### Ejemplo de Uso
Para obtener las estadísticas, realiza una solicitud HTTP `GET` en la URL:

https://xmenparcial1.onrender.com/api/v1/dna/stats


#### Respuesta

`json
{
    "count_human_dna": 2,
    "count_mutant_dna": 2,
    "ratio": 1.0
}`

En este ejemplo, la respuesta indica que hay un total de 2 secuencias de ADN humano, 2 secuencias de ADN mutante, y un ratio de 1.0 entre humanos y mutantes.

------------


## Base de Datos H2
La base de datos H2 almacena cada secuencia de ADN junto con un indicador de si pertenece a un mutante o no. Esto permite que la API tenga acceso rápido y eficiente a los datos de ADN, mejorando la velocidad de respuesta en solicitudes futuras.

| ID  | DNA Sequence                                                            | Mutante |
|-----|-------------------------------------------------------------------------|---------|
| 1   | `["ATGCGA", "CAGTGC", "TTATGT", "AGACGG", "CAGCTA", "TCACTG"]`          | false   |
| 2   | `["ATGCGA", "CAGTGC", "TTATGT", "AGAAGG", "CCCATA", "TCACTG"]`          | true    |


Nota
En la entidad DNA se almacenan las secuencias de ADN indicando si cada secuencia corresponde a un mutante o no.
