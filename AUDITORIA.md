# Bitácora de Auditoría de Desempeño Web — AstroBitácora

**Materia / Práctica:** Diseño Web Adaptable — Auditoría y optimización del tiempo de carga  
**Fecha:** 17 de octubre de 2026  
**Herramienta principal:** Google Chrome Lighthouse / PageSpeed Insights  
**Entorno de prueba:** Mobile / Desktop  

---

## 1. Medición Inicial (Línea Base / Baseline)

> Medición realizada antes de aplicar cualquier optimización sobre el sitio base intencionalmente degradado.

| Métrica | Inicial (Mobile) | Inicial (Desktop) | Estado |
| :--- | :--- | :--- | :--- |
| **Puntaje de Rendimiento (Performance Score)** | *Pendiente* | *Pendiente* | ⚠️ A mejorar |
| **FCP (First Contentful Paint)** | *Pendiente* | *Pendiente* | — |
| **LCP (Largest Contentful Paint)** | *Pendiente* | *Pendiente* | — |
| **CLS (Cumulative Layout Shift)** | *Pendiente* | *Pendiente* | — |
| **TBT (Total Blocking Time)** | *Pendiente* | *Pendiente* | — |
| **Speed Index** | *Pendiente* | *Pendiente* | — |
| **Peso total transferido (KB/MB)** | *Pendiente* | *Pendiente* | — |

### Principales oportunidades y diagnósticos detectados por Lighthouse:
1. **Eliminar los recursos que bloqueen el renderizado**:
   - `script.js` se carga en el `<head>` sin atributo `defer` ni `async`.
2. **Dimensiones no explícitas en elementos de imagen**:
   - Las imágenes carecen de atributos `width` y `height`, provocando cambios de diseño acumulados (CLS).
3. **Cargar de forma diferida las imágenes que no aparecen en pantalla (Offscreen images)**:
   - Las imágenes del archivo celeste se descargan de inmediato aunque están fuera del viewport inicial.
4. **Optimizar el elemento del LCP (Largest Contentful Paint)**:
   - La imagen del Hero (`hero-cosmos`) no tiene prioridad de carga (`fetchpriority="high"`).
5. **Servir imágenes en formatos de próxima generación**:
   - Formatos JPEG pesados sin compresión adaptativa ni formatos modernos (WebP/AVIF).

---

## 2. Hipótesis de Optimización

1. **Hipótesis 1 (FCP y TBT - Render Blocking)**:  
   Al añadir el atributo `defer` a la etiqueta `<script src="script.js">`, el navegador no detendrá el análisis ni la construcción del árbol DOM, reduciendo el FCP y minimizando el TBT a 0 ms.

2. **Hipótesis 2 (CLS - Cumulative Layout Shift)**:  
   Al especificar los atributos `width` y `height` junto con `height: auto` en CSS para las imágenes, el navegador reservará el espacio exacto (aspect ratio) antes de que las imágenes terminen de descargarse, llevando el CLS a 0.

3. **Hipótesis 3 (LCP - Largest Contentful Paint)**:  
   Al asignar `fetchpriority="high"` a la imagen principal del Hero y utilizar entrega optimizada (`f_auto,q_auto` en Cloudinary), el navegador priorizará este recurso en la cola de red, disminuyendo el tiempo de LCP sensiblemente.

4. **Hipótesis 4 (Transferencia de datos y Speed Index - Lazy Loading)**:  
   Al agregar `loading="lazy"` y `decoding="async"` en las imágenes secundarias del catálogo fuera de pantalla, se evitará la descarga de megabytes innecesarios en el primer render, acelerando la interactividad del sitio.

---

## 3. Registro de Cambios Implementados

- [ ] Agregar `defer` a `script.js` en [index.html](file:///c:/Users/Kedwin%20Isai%20Gutierre/Documents/astro-performance-lab/index.html).
- [ ] Incorporar `width` y `height` a todas las imágenes `<img>`.
- [ ] Aplicar `loading="lazy"` y `decoding="async"` a imágenes debajo del pliegue.
- [ ] Asignar `fetchpriority="high"` a la imagen Hero.
- [ ] Optimizar entrega de imágenes vía Cloudinary (`f_auto,q_auto`).
- [ ] Ajustar reglas CSS en [styles.css](file:///c:/Users/Kedwin%20Isai%20Gutierre/Documents/astro-performance-lab/styles.css) para compatibilidad responsiva con `width/height`.

---

## 4. Medición Final (Posterior a la Optimización)

| Métrica | Inicial | Final | Diferencia / Impacto |
| :--- | :--- | :--- | :--- |
| **Puntaje de Rendimiento** | — | — | — |
| **FCP** | — | — | — |
| **LCP** | — | — | — |
| **CLS** | — | — | — |
| **TBT** | — | — | — |
| **Speed Index** | — | — | — |
| **Peso total transferido** | — | — | — |

---

## 5. Conclusiones y Análisis
*(A completar con los hallazgos tras la comparativa final)*
