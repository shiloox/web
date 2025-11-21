Enunciado:
Caso Delta Faucet Company
Delta Faucet Company (https://www.deltafaucetcompany.com/), también conocida como DFC, fabrica grifería
residencial y comercial, además de otros productos para cocinas y baños que transforman la experiencia con el
agua. Fundada por Masco Corporation en 1954 con la introducción del grifo monomando, Delta Faucet Company
se enorgullece de ser el líder en innovación de grifos de Estados Unidos, con productos de las marcas Delta®,
Brizo®, Kraus® y Peerless®.
DFC extiende la experiencia a través de DFC@Home™, una smartphone app (https://www.deltafaucet.com/app),
con versiones en:
Google Play Store: https://play.google.com/store/apps/details?id=com.deltafaucet.DFCatHome&hl=en_US y
Apple AppStore: https://apps.apple.com/us/app/dfc-home/id6474621406
DFC@Home™ es la solución integral para aprovechar al máximo los productos Delta® conectados. La aplicación
brinda información de los dispositivos y permite la configuración remota de los mismos, como thresholds para
temperature y water flow. La aplicación se empareja con los dispositivos disponibles en la red WiFi.
Para los Partners ofrece el portal https://www.dfcpro.com, donde brinda información técnica sobre los diferentes
productos.
Página 2 de 5
Pregunta 1 (20 p.).
Usted se integra al backend software developer team, a cargo de la creación de un RESTful API que brinde
soporte a las operaciones de Delta Faucet Company.
El ecosistema de Delta Faucet Company requiere que la aplicación de backend cuente con Endpoints en el
RESTful API, para el manejo de la información de Dispatch Orders (DispatchOrder) conformadas por los
atributos dispatchOrderId (DispatchOrderId), productId (ProductId), requestedUnits (int), dispatchExpectedAt
(datetime), qualityLevel (EQualityLevel), dispatchCompletedAt(datetime), notes(string).
Como reglas de negocio, Delta Faucet Company:
• No permite que se registre un dispatch order con un valor de requested units menor o igual a 100 ni
mayor a 5000.
• El valor de expected at debe ser al menos 30 días menor o igual a la fecha actual.
• El valor de completed at debe ser mayor o igual que la fecha de expected at.
• El valor de quality level debe hacer referencia a una de las posibilidades de un enumeration
EQualityLevel, definido como sigue:
Id Name Criterio de asignación
1 Outstanding dispatchCompletedAt >= dispatchExpectedAt por 2 días o menos
2 Normal dispatchCompletedAt > dispatchExpectedAt por mas de 2 días
3 BellowExpected dispatchCompletedAt > dispatchExpectedAt por mas de 5 días
4 Inefficient dispatchCompletedAt > dispatchExpectedAt por mas de 7 días
• El tipo DispatchOrderId es un value object type que representa el identificador de la orden de
producción para el negocio. Contiene un valor de tipo Guid no nulo y no vacío. Es generado
automáticamente al momento del registro.
• El tipo ProductId es un value object type que representa al identificador de un producto en el
negocio, que se genera en otro bounded context. Contiene un valor de tipo Guid no nulo y no vacío,
que debe ser proporcionado para proceder al registro.
• Establece que la información que se desea preservar de los Dispatch Orders incluye id (int,
obligatorio, autogenerado, llave primaria), dispatchOrderId (DispatchOrderId, obligatorio),
productId (ProductId, obligatorio), requestedUnits (int, obligatorio), dispatchExpectedAt (datetime,
obligatorio), dispatchCompletedAt (datetime, obligatorio), qualityLevel (EQualityLevel,
obligatorio), notes (string, opcional).
• El valor de qualityLevel no se solicita en el request, sino que se asigna internamente uno de los
valores de EQualityLevel según los criterios de asignación de la tabla indicada líneas arriba.
• En el caso de responses que retorna el API con información de dispatch orders, para cada
DispatchOrder se debe incluir id, dispatchOrderId (como string), productId (como string),
requestedUnits, qualityLevel (como string), dispatchCompletedAt, dispatchDays (número de días
entre expected at y completed at), notes. No se incluye expected at.
• Considere que DispatchOrder es un aggregate root y por lo tanto es auditable, con el fin de tener un
registro de las fechas de creación y última actualización.
Durante la etapa de desarrollo, le asignan trabajar en específico sobre el Endpoint:
/api/v1/dispatch-orders
Página 3 de 5
Dispatch Orders Endpoint ( http://localhost:{port}/api/v1/dispatch-orders )
Debe implementar solo una operación en el RESTful API: agregar (POST) un dispatch order. Los valores de id
son autogenerados al momento de almacenar la información. Ante una adición satisfactoria, retorne el HTTP
Status 201 y en el body del response el resource incluyendo la información del dispatch order y su id
generado. En caso de error incluya el HTTP Status correcto según el error, así como el mensaje de error en el
body del response.
Incluya como parte del desarrollo la implementación de las reglas de negocio.
Technical constraints
1. Elabore la solución con C#, NET 9 y ASP.NET Core Framework.
2. Cree su solución y el proyecto de software con el nombre pc2<NRC>u<código-estudiante>.API (por
ejemplo, pc27454u201621873.API).
3. Considere que los conceptos DispatchOrder, ProductId e EQualityLevel pertenece al bounded
context SCM.
4. Considere el bounded context Shared con los elementos que correspondan, incluyendo clases base,
interfaces base, clases de propósito general, extensiones generales o de políticas (según ejemplos en
clase).
5. La información debe ser persistente en una base de datos relacional (MySQL), en un esquema dfc.
6. Aplique buenas prácticas de Arquitectura de Software, enfoque de Domain-Driven Design,
separación en bounded contexts, layered architecture (domain, application, interfaces,
infrastructure), patrones de strategic y tactical Domain-Driven Design, patrón CQRS, Resource,
Assembler, principios y patrones de diseño de software orientado a objetos, convenciones de
nomenclatura en inglés, así como buenas prácticas de nomenclatura en C# (entre ellas Upper Camel
Case para Clases, Properties, Métodos; Lower Camel Case para atributos privados) y buenas prácticas
para nomenclatura de objetos de Base de Datos (entre ellas snake case, tablas en plural, sin
mnemónicos, id como nombre de primary key).
7. Utilice minúsculas para los nombres de URL para todos los endpoints.
8. Utilice Properties para representar los atributos de las clases Entity.
9. Documente su código con XML documentation comments, colocando información de propósito para
principales objetos de programación, así como propósito, parámetros y valor retornado en clases y
métodos relevantes. Incluya como parte de la documentación sus nombres y apellidos como valor en
un tag <remarks>.
10. Incluya documentación de Endpoints con OpenAPI con textos en inglés.
11. Todos los textos de mensajes deben ser en inglés.
12. Incluya en el archivo README.md, la información en inglés de la aplicación, descripción y su
información como author.
13. Considere la gestión de excepciones en la aplicación.
14. Empaquete su solución como un archivo .zip. (único formato válido) con el nombre
pc2<NRC>u<código-estudiante>.zip (por ejemplo, pc27454u201621873.zip).
15. Suba su archivo de solución en la Actividad indicada para la Práctica Calificada 2.
NO forma parte del alcance del proyecto:
1. Soporte de CORS.
2. Security.
3. Testin
