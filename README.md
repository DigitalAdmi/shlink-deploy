# shlink-deploy

Despliegue de [Shlink](https://shlink.io) (acortador de enlaces) para
Digital ICON, como servicio **Compose** de Dokploy.

- `https://shortdi.link/<código>`: enlaces cortos
- `https://app.shortdi.link`: panel (shlink-web-client), con basicAuth

Las imágenes se **construyen desde los forks** de esta cuenta, en una
versión fija (tag), no se descargan de Docker Hub:

| Servicio | Fork | Versión |
|---|---|---|
| shlink | [DigitalAdmi/shlink](https://github.com/DigitalAdmi/shlink) | `v5.1.8` |
| web | [DigitalAdmi/shlink-web-client](https://github.com/DigitalAdmi/shlink-web-client) | `v4.9.0` |
| db | `postgres:17-alpine` (imagen oficial) | 17 |

## Secretos

Van en la pestaña **Environment** del Compose en Dokploy (lista en
`.env.example`). Nunca se suben a este repo.

## Actualizar

1. En GitHub, en el fork correspondiente: **Sync fork** (trae los tags nuevos).
2. Cambiar el tag en `docker-compose.yml` (`#v5.1.8` → `#vX.Y.Z`) y, en
   `shlink`, el arg `SHLINK_VERSION`. También el `image:` local.
3. Commit + push, y en Dokploy → **Deploy**.

## Migrar a otro servidor

1. En el servidor nuevo, Dokploy → Compose desde este repo, con las mismas
   variables de entorno.
2. Restaurar la base: `pg_dump` del contenedor `db` actual → `psql` en el
   nuevo antes de exponerlo.
3. Reapuntar los registros A `@` y `app` de `shortdi.link`.
