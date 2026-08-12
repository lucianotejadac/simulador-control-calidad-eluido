# Simulador de Control de Calidad del Eluido de ⁹⁹ᵐTc

Simulador didáctico del control de calidad que se realiza al eluido de ⁹⁹ᵐTcO₄⁻ recién extraído del generador, desarrollado en el Departamento de Tecnología Médica, Universidad de Chile.

El usuario ejecuta las cinco pruebas del protocolo — transparencia, pH, pureza radionucleídica, pureza química y pureza radioquímica — manipulando el material con el puntero, registra las lecturas, emite un veredicto por prueba y después lo compara con el estado real del lote, que el simulador mantiene oculto hasta ese momento. Todo corre en el navegador, en un único archivo HTML sin dependencias externas.

> ⚠️ Modelo educativo. No apto para decisiones clínicas ni para reemplazar los procedimientos de control de calidad de una unidad de medicina nuclear.

## Acceso en línea

👉 https://lucianotejadac.github.io/simulador-control-calidad-eluido/

No requiere instalación. Basta un navegador moderno con soporte de SVG y eventos de puntero.

## El lote: por qué no se puede adivinar

Cada sesión genera un lote nuevo con identificador del tipo `TC-AAMMDD-nn`, volumen de eluido entre 4 y 10 mL y actividad calibrada de ⁹⁹ᵐTc entre 180 y 750 mCi. Detrás de eso, el simulador sortea de forma independiente si el lote falla cada prueba, con probabilidades deliberadamente altas: alrededor de nueve de cada diez lotes fallan al menos un ensayo, de modo que “conforme” nunca es una respuesta segura y hay que medir de verdad para decidir.

El botón **Nuevo lote** genera otro caso, y un contador lleva la cuenta de veredictos acertados sobre veredictos emitidos.

## Las cinco pruebas

| # | Ensayo | Procedimiento simulado | Límite de aceptación |
| --- | --- | --- | --- |
| 1 | Transparencia | inspección visual al trasluz: encender la caja de luz, agitar el vial con suavidad y observarlo frente a la pantalla iluminada | límpido, incoloro, sin partículas |
| 2 | Determinación del pH | jeringa de 1 mL por el septo, extraer ≈ 0,1 mL y dejar caer gotas sobre la tira de pH 2,0–10,0 hasta humedecerla; comparar con el patrón colorimétrico | 4,0 – 8,0 |
| 3 | Pureza radionucleídica (⁹⁹Mo) | activímetro y canister de plomo de 6 mm: lectura A del vial blindado en canal ⁹⁹Mo y lectura B del vial sin blindaje en canal ⁹⁹ᵐTc | ≤ 0,15 µCi de ⁹⁹Mo por mCi de ⁹⁹ᵐTc |
| 4 | Pureza química (Al³⁺) | ensayo colorimétrico: una gota del patrón de Al de 10 µg/mL y una gota del eluido sobre la tira con agente complejante; comparar intensidades | ≤ 10 µg/mL |
| 5 | Pureza radioquímica | cromatografía en capa fina con solución salina al 0,9 %: sembrar, dejar subir el frente, cortar en Rf 0,5 y contar cada mitad en el contador de pozo NaI(Tl) | ≥ 95 % como ⁹⁹ᵐTcO₄⁻ |

Cada prueba trae su propio fundamento en pantalla. El del ensayo radionucleídico explica el factor de corrección del método de atenuación: el plomo de 6 mm frena más del 99 % de los fotones de 140 keV del ⁹⁹ᵐTc pero solo la mitad de los de 740 y 780 keV del ⁹⁹Mo, de ahí que la razón se calcule como `(A × 2) / B`. El de la cromatografía explica que el pertecnetato migra con el frente mientras el Tc reducido e hidrolizado y el TcO₂ coloidal quedan en el origen, y advierte que retirar la tira antes de que el frente llegue a la marca sesga el resultado.

## Cómo se trabaja

Cada estación es una escena manipulable: se arrastra el vial con la pinza hasta la caja de luz, la jeringa hasta el septo, el canister hasta el pozo del activímetro, el capilar hasta la línea de origen de la tira. Una lista de procedimiento al costado se va marcando sola a medida que se ejecutan los pasos en el orden correcto, y todo ocurre detrás del blindaje, con pinzas largas y el menor tiempo de exposición posible, como recuerda la propia interfaz.

En las pruebas que exigen cálculo, primero se registran las lecturas crudas — A y B del activímetro, o las cuentas de la mitad del origen y la del frente — y el criterio de aceptación queda oculto en un desplegable, con la instrucción explícita de intentar el cálculo antes de abrirlo. Después se emite el veredicto con los botones CONFORME o NO CONFORME, y el simulador responde de inmediato si acertaste y cuál era el estado real del lote. La pestaña INFORME cierra el ejercicio con la tabla de las cinco pruebas: resultado medido, límite, tu veredicto, estado real y evaluación.

## Qué falla en los lotes defectuosos

| Prueba | Modo de falla simulado |
| --- | --- |
| Transparencia | partículas visibles en suspensión, del tipo de los finos de alúmina arrastrados de la columna, o una leve coloración amarillenta |
| pH | eluido demasiado ácido o demasiado alcalino, fuera del rango de aceptación |
| Pureza radionucleídica | razón ⁹⁹Mo/⁹⁹ᵐTc por sobre el límite, con ruptura de ⁹⁹Mo desde la columna |
| Pureza química | Al³⁺ por encima del patrón de 10 µg/mL |
| Pureza radioquímica | Tc reducido, hidrolizado o coloidal por sobre el 5 % de impurezas |

## Stack técnico

| Capa | Tecnología |
| --- | --- |
| UI | HTML5 + CSS3 con custom properties y tema oscuro |
| Escenas | SVG manipulable, con arrastre por eventos de puntero |
| Lógica | JavaScript ES6 vanilla, sin transpilación |
| Tipografías | fuentes del sistema |

Un solo archivo HTML autocontenido: sin npm, sin bundler, sin servidor, sin CDN y sin ninguna librería externa.

## Limitaciones conocidas

Las lecturas se generan a partir del lote sorteado más un ruido acotado, no de un modelo de transporte de radiación: el activímetro y el contador de pozo entregan valores plausibles, no simulados desde la física de detección. El decaimiento del ⁹⁹ᵐTc y del ⁹⁹Mo en el tiempo no se modela, así que la razón medida no cambia con el reloj. La comparación de color, tanto en la tira de pH como en el ensayo de aluminio, depende de la pantalla y del ojo del usuario, igual que en el laboratorio, pero sin las variaciones reales de iluminación. Los tiempos de cada operación son instantáneos o abreviados. Y la interacción es solo por puntero: aún no hay navegación por teclado.

## Roadmap

- [ ] Navegación por teclado y revisión de accesibilidad.
- [ ] Decaimiento en el tiempo, con hora de elución y recálculo de la razón ⁹⁹Mo/⁹⁹ᵐTc.
- [ ] Ingreso del cálculo por parte del usuario, con corrección del valor y no solo del veredicto.
- [ ] Informe exportable en PDF o CSV para entregar como evidencia del ejercicio.
- [ ] Series de lotes encadenados, con seguimiento del rendimiento a lo largo de varias sesiones.
- [ ] Versión en inglés.

## Cómo citar

> Tejada Castro, L. (2026). Simulador de control de calidad del eluido de ⁹⁹ᵐTc [Software]. Departamento de Tecnología Médica, Universidad de Chile.

## Autor

Luciano Tejada Castro

lucianotejada@uchile.cl

Departamento de Tecnología Médica · Facultad de Medicina · Universidad de Chile

## Licencia

© 2026 Luciano Tejada Castro. Todos los derechos reservados. Obra protegida por la Ley N° 17.336 sobre Propiedad Intelectual de Chile. Cualquier reproducción, adaptación o comunicación pública distinta del uso personal requiere autorización escrita del autor.
