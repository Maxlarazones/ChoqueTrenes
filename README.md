# Choque AVE–Iryo en Urda (28/10/2026)

Dos gráficos estáticos para embeber por iframe en larazon.es:

| Pieza | Ruta |
|---|---|
| Mapa del PK 118 (tramo Toledo–Ciudad Real) | `/mapa/` |
| Infografía del alcance | `/infografia/` |

HTML estático, sin build ni dependencias. Solo carga Google Fonts (Fraunces + Archivo).

## Subir a GitHub

```bash
cd choque-urda
git init
git add .
git commit -m "Gráficos choque AVE-Iryo Urda"
git branch -M main
git remote add origin git@github.com:<org-larazon>/choque-urda.git
git push -u origin main
```

Usa la organización corporativa de GitHub, no una cuenta personal.

## Desplegar en Vercel

1. Vercel → **Add New → Project** → importar el repo `choque-urda`.
2. Framework Preset: **Other**. Build command: vacío. Output directory: vacío (raíz).
3. **Deploy**.

O por CLI:

```bash
npx vercel --prod
```

Cada push a `main` redespliega solo. La caché está a 60 s para que las correcciones salgan rápido mientras la noticia está viva.

## Embeber en el CMS

```html
<iframe src="https://<dominio-vercel>/mapa/" data-lr-embed="mapa-choque-urda"
  title="Mapa del choque entre un AVE y un Iryo en Urda (Toledo)"
  style="width:100%;border:0;height:900px" loading="lazy"></iframe>

<iframe src="https://<dominio-vercel>/infografia/" data-lr-embed="infografia-choque-urda"
  title="Esquema del alcance entre un AVE y un Iryo"
  style="width:100%;border:0;height:320px" loading="lazy"></iframe>

<script>
window.addEventListener("message", function (e) {
  if (!e.data || e.data.type !== "lr-embed-height") return;
  var f = document.querySelector('iframe[data-lr-embed="' + e.data.id + '"]');
  if (f) f.style.height = e.data.height + "px";
});
</script>
```

Cada página envía su altura al padre (`postMessage` con `type: "lr-embed-height"`), así el iframe se ajusta en móvil y escritorio. Si el CMS no permite el script, las alturas fijas de arriba funcionan en escritorio; en móvil el mapa ocupa unos 700 px.

## Datos y límites

- Punto del choque: PK 118 de la LAV Madrid–Sevilla, comunicado por Adif. Ubicación **estimada** sobre el trazado de Natural Earth (margen de 3 a 5 km). Si llegan coordenadas oficiales, cambiar el punto en `mapa/index.html` (círculos `.unc`, `.pulse`, `.hit` y el grupo `.callout`).
- Fuentes: Adif, Ministerio de Transportes, 112 Castilla-La Mancha. Cartografía: Natural Earth (dominio público).
- `noindex` puesto: el tráfico SEO debe ir a la noticia, no al dominio de Vercel.
