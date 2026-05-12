# Guía de Deploy · Roieventos.es

## Paso 1 · Crear repo en GitHub

1. Entra en https://github.com/organizations/Procreartes/repositories/new
2. Repository name: `roieventos-web`
3. Visibilidad: **Public** (necesario para Coolify)
4. **NO** marques "Add a README" ni .gitignore (ya los tenemos en local)
5. Crea el repo

## Paso 2 · Subir el código desde tu ordenador

Abre PowerShell o Terminal en la carpeta `roieventos`:

```powershell
cd "C:\Users\Naroe\AppData\Roaming\Claude\local-agent-mode-sessions\4fb1b21f-8428-42ac-93cd-caaf3dd9e707\16ec8a66-f26e-433a-aa92-2067e9329d64\local_ad698d54-2a9a-423e-a466-2bb5c2592e83\outputs\roieventos"

git init -b main
git add -A
git commit -m "Initial commit: web Roieventos editorial"
git remote add origin https://github.com/Procreartes/roieventos-web.git
git push -u origin main
```

Si te pide credenciales, usa tu usuario de GitHub y un **Personal Access Token** (no la contraseña).

## Paso 3 · Deploy en Coolify

1. Abre http://46.225.143.200:8000
2. Login en Coolify
3. Selecciona un proyecto existente o crea uno nuevo llamado **"Naroe Consulting"**
4. **+ New Resource** → **Public Repository**
5. Repository URL: `https://github.com/Procreartes/roieventos-web.git`
6. Branch: `main`
7. **MARCA "Is it a static site?"** ← esto es crítico
8. Build pack: dejar en `Static`
9. Publish Directory: `/` (raíz)
10. **Deploy**

Coolify te dará una URL temporal tipo `https://xxxxx.46.225.143.200.sslip.io`. Verifica que la web carga bien.

## Paso 4 · Conectar el dominio roieventos.es

### En tu proveedor de dominio (donde compraste roieventos.es)

Crea estos registros DNS:

```
Tipo   Nombre   Valor                    TTL
A      @        46.225.143.200           3600
A      www      46.225.143.200           3600
```

### En Coolify

1. Ve a tu aplicación roieventos-web
2. Pestaña **Settings → Domains**
3. Añade dos dominios:
   - `roieventos.es`
   - `www.roieventos.es`
4. **Generate SSL** (Let's Encrypt) — Coolify se encarga del certificado automáticamente
5. Save & redeploy

La propagación DNS puede tardar entre 5 minutos y 24 horas.

## Paso 5 · Verificar

- [ ] Web carga en https://roieventos.es
- [ ] Certificado SSL válido (candado verde)
- [ ] Funciona en móvil y desktop
- [ ] Formulario de contacto abre el cliente de correo
- [ ] Imágenes de Unsplash cargan sin bloqueo
- [ ] No hay errores en la consola del navegador (F12)

## Actualizaciones futuras

Para cualquier cambio en la web:

```powershell
cd "ruta\al\repo\roieventos"
# (haces cambios en index.html)
git add -A
git commit -m "descripción del cambio"
git push
```

Coolify detecta el push automáticamente y redespliega en ~30 segundos.

---

**Soporte rápido:**
- Coolify dashboard: http://46.225.143.200:8000
- Servidor: Hetzner IP 46.225.143.200
- Repo: github.com/Procreartes/roieventos-web
