# Cómo trabajamos en UnKraken

Protocolo común para todos los proyectos de la organización. Cada repo añade lo suyo (reparto, normas, cómo probar) en su `AGENTS.md` y su `README.md`.

## Las cinco reglas

1. **Todo trabajo empieza con un issue**: qué hay que hacer y por qué.
2. **Todo cambio entra por PR.** Nadie sube directo a `main`, ni las personas ni los agentes de IA.
3. **Rama `tipo/N-descripcion`**, donde `N` es el número del issue: `fix/3-alias-productsupdate`.
4. **Commits en castellano con [Conventional Commits](https://www.conventionalcommits.org/es/)**: `fix: corrige el alias de ProductsUpdate`.
5. **Merge con *Squash and merge*, con el CI en verde.** Lo hace una persona, nunca un agente.

## El flujo, con `gh`

```
gh issue create                                          # 1. issue (o desde la web)
gh issue develop 3 --name fix/3-alias --checkout         # 2. rama ligada al issue
git add .
git commit -m "fix: corrige el alias de ProductsUpdate"  # 3. commits (los que hagan falta)
git push -u origin fix/3-alias
gh pr create --fill                                      # 4. PR: añadir "closes #3" en la descripción
gh pr checks                                             # 5. esperar al verde
gh pr merge --squash --delete-branch                     # 6. merge y borrar la rama
git switch main
git pull                                                 # 7. dejar main al día
```

- **`closes #N` va en la descripción del PR**, no en los commits: al mergear, el issue se cierra solo.
- Pon al issue y al PR el **label** de su tipo (`feat`, `fix`...).
- Al mergear, todos los commits de la rama se convierten en uno: puedes hacer tantos como quieras.

## Tipos de commit

Ocho tipos, cada uno con su label del mismo nombre:

| Tipo | Para |
|---|---|
| `feat` | Funcionalidad nueva |
| `fix` | Corrección |
| `refactor` | Reestructuración sin cambio funcional, incluidas las mejoras de rendimiento |
| `docs` | Documentación |
| `chore` | Mantenimiento, configuración, formato |
| `ci` | Workflows |
| `test` | Pruebas |
| `revert` | Deshacer un cambio anterior |

## Revisiones

- **No hay aprobaciones obligatorias**: quien abre el PR lo mergea cuando está en verde.
- **Si tocas el área del otro**, pídele revisión (`gh pr create --reviewer usuario`) y espera su visto bueno antes de mergear.
- Las dudas sobre una línea concreta van como comentario en esa línea del PR (pestaña *Files changed*).

## Trabajar con IA

Cada repo tiene un **`AGENTS.md`** con las reglas para los agentes (Codex, ChatGPT, Claude...):

- **Codex** y la mayoría de agentes lo leen solos.
- **Claude Code** lo lee a través de `CLAUDE.md`, que lo importa.
- **ChatGPT en el chat** no lee el repo: copia el `AGENTS.md` en las instrucciones del proyecto de ChatGPT.

Un agente puede crear la rama, hacer commits y abrir el PR. **El merge lo hace siempre una persona**, después de revisar el PR.

## Versiones

Cuando `main` está listo para entregar, se publica con un tag ([versionado semántico](https://semver.org/lang/es/)):

```
git switch main
git pull
git tag -a v1.1.0 -m "descripción"
git push origin v1.1.0
```

| Cambio | Versión |
|---|---|
| Correcciones | `v1.0.1` |
| Funcionalidad nueva | `v1.1.0` |
| Cambio incompatible (hay que reinstalar o migrar algo a mano) | `v2.0.0` |

## Comunicación

- **Lo que hay que hacer va en un issue**, no solo en un mensaje o una llamada: así queda enlazado al código que lo resuelve.
- Para pedir algo al otro, **menciónale** en el issue o el PR (`@srcarlesamado`, `@XavierDuran`): GitHub le avisa.

## Proyecto nuevo

1. Crear el repo en `UnKraken`, con la rama `main`.
2. Copiar los labels: `gh label clone UnKraken/.github --repo UnKraken/<repo>`.
3. Copiar `plantilla/AGENTS.md` y `plantilla/CLAUDE.md` de este repo a la raíz del nuevo y rellenar la sección del proyecto.
4. Si el proyecto genera un ejecutable o despliega SQL, añadir un workflow que lo pruebe en cada PR (ejemplo: `UnKraken/krknSSM`).

Si este protocolo cambia, se cambia aquí por PR y se actualiza el `AGENTS.md` de cada repo.
