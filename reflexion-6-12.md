# Reflexión 6.12

**1. Explica con tus palabras cómo funciona `git bisect` y qué problema resuelve.**
`git bisect` es una herramienta de Git que automatiza la búsqueda de un commit que introdujo un error (bug) en el código. Funciona realizando una búsqueda binaria en el historial de commits. Empieza pidiéndote que le indiques un commit "bueno" (donde el código funcionaba) y uno "malo" (donde el código falla). Luego, Git se posiciona en el commit que está justo en la mitad de ese rango y te permite probar el código. Dependiendo de si ese commit intermedio está bien o mal, Git descarta la mitad del historial y repite el proceso hasta aislar el commit exacto que rompió el código. Resuelve el problema de tener que revisar manualmente decenas o cientos de commits uno por uno para encontrar dónde se originó un fallo.

**2. Describe una situación real en un proyecto donde `git bisect` sería especialmente útil.**
Imagina que estás trabajando en una aplicación web que se actualiza constantemente. Hace tres semanas se lanzó una versión que funcionaba perfectamente. Hoy, tras más de 100 nuevos commits realizados por varios desarrolladores, te das cuenta de que una funcionalidad crítica (como el sistema de inicio de sesión) ha dejado de funcionar de manera intermitente, y nadie sabe en qué momento ocurrió. En lugar de revisar el código de esos 100 commits, usas `git bisect`. Indicas el commit de hace tres semanas como bueno, y el actual como malo. En unos pocos pasos (probablemente unos 7 saltos), encontrarás el commit exacto que rompió el inicio de sesión.

**3. ¿Qué requisitos previos debe cumplir el proyecto para que `git bisect` sea eficaz?**
Para que `git bisect` sea realmente eficaz, el proyecto debe cumplir ciertos requisitos:
- **Historial de commits limpio y atómico:** Cada commit debe hacer una sola cosa y el código debe poder compilarse o ejecutarse en cada punto del historial. Si hay commits intermedios que rompen la compilación, será más difícil probarlos.
- **Tests reproducibles:** Debes tener una forma clara y rápida de saber si un commit está "bien" o "mal". Lo ideal es tener pruebas automatizadas (tests unitarios o de integración) que puedan ejecutarse rápidamente para que `git bisect run` automatice todo el proceso.
- **Conocer un punto bueno y uno malo:** Necesitas saber con certeza al menos un commit en el pasado donde el código funcionaba sin el bug.
