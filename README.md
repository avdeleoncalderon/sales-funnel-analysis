# Análisis de Embudo de Ventas y Experimento A/A/B — App de Alimentos 🛒

## Objetivo del proyecto

Una empresa de alimentos con app móvil quería entender **dónde pierden usuarios en su proceso de compra** y si un rediseño tipográfico podía mejorar su tasa de conversión final.

**Preguntas de negocio:**
1. ¿En qué etapa del embudo se abandona más y qué tan grave es esa pérdida?
2. ¿El nuevo diseño de fuentes (experimento B) genera una mejora estadísticamente significativa en conversión respecto al diseño actual (control)?

---

## Fuente de datos

**Dataset:** `logs_exp_us.csv` — proporcionado por TripleTen como parte del sprint de Análisis de Comportamiento de Usuarios del bootcamp. Simula logs de eventos reales de una app de e-commerce de alimentos; sin información personal identificable.

**Estructura:** Registro de eventos de usuario (log-level), donde cada fila es una acción de un usuario en un momento específico.

**Tamaño:** ~243,000 registros de eventos de ~7,500 usuarios únicos.

**Período cubierto:** Los datos completos comienzan el 1 de agosto de 2019; se descartaron fechas anteriores por estar incompletas.

**Variables:**

| Variable | Descripción |
|---|---|
| `EventName` | Nombre del evento registrado en la app |
| `DeviceIDHash` | Identificador único del usuario (hasheado) |
| `EventTimestamp` | Marca de tiempo Unix del evento |
| `ExpId` | Grupo experimental: 246 y 247 = control, 248 = prueba (nueva tipografía) |

**Eventos del embudo (en orden):**

| Evento | Descripción |
|---|---|
| `MainScreenAppear` | Usuario abre la pantalla principal |
| `OffersScreenAppear` | Usuario ve la pantalla de ofertas |
| `CartScreenAppear` | Usuario abre su carrito |
| `PaymentScreenSuccessful` | Usuario completa un pago exitoso |
| `Tutorial` | Usuario accede al tutorial (evento fuera del flujo principal) |

---

## Proceso

### 1. Limpieza y preparación de datos
- Conversión del timestamp Unix a formato `datetime` para análisis temporal.
- Eliminación de duplicados (mismo usuario, mismo evento, mismo timestamp exacto).
- Filtrado de fechas: se descartaron registros previos al 1 de agosto de 2019, ya que el período anterior tenía cobertura incompleta — incluirlos habría sesgado las tasas de conversión.
- Se excluyó el evento `Tutorial` del embudo principal por no pertenecer al flujo de compra.
- Verificación de distribución equitativa de usuarios entre los tres grupos experimentales (246, 247, 248).

### 2. Análisis del embudo de ventas
- Se calculó, por usuario, cuántos eventos únicos completó de cada tipo.
- Se definió la tasa de conversión entre etapas como: usuarios que completaron la etapa N+1 / usuarios que completaron la etapa N.
- Se identificó la etapa con mayor caída para priorizar intervenciones de diseño.

### 3. Experimento A/A/B — Validación estadística
- **Grupos A (control):** ExpId 246 y ExpId 247 — misma app, sin cambios.
- **Grupo B (prueba):** ExpId 248 — app con nueva tipografía.
- **Prueba utilizada:** Prueba Z de proporciones (dos muestras) con corrección de Bonferroni para comparaciones múltiples.
- **Nivel de significancia:** α = 0.05.
- **Paso de validación A/A:** primero se compararon los grupos 246 vs 247 para confirmar que no había diferencias entre ellos — si hubiera diferencia, el experimento estaría comprometido.

---

## Resultados e insights clave

**1. Menos de la mitad de los usuarios que abren la app completan una compra.**
Solo el **47.7% de los usuarios** llega hasta `PaymentScreenSuccessful` desde `MainScreenAppear`. Hay margen significativo de mejora en el flujo general.

**2. La pantalla de ofertas es el cuello de botella más crítico.**
La mayor caída ocurre entre `MainScreenAppear` y `OffersScreenAppear`: el **61.9% de los usuarios que ven la pantalla principal no llega a ver las ofertas**. Esta es la etapa más costosa del embudo y la mayor oportunidad de optimización.

**3. El experimento A/A confirma que los grupos de control son equivalentes.**
No se encontraron diferencias estadísticas entre los grupos 246 y 247 (p > 0.05). Esto valida la integridad del experimento: cualquier diferencia con el grupo 248 sería atribuible al cambio tipográfico, no a sesgos en la asignación.

**4. El nuevo diseño de fuentes no mejora la conversión.**
Al comparar el grupo B (248) con ambos grupos de control, no se encontraron diferencias estadísticamente significativas en ninguna etapa del embudo. El cambio tipográfico es neutro: no daña ni mejora la experiencia de conversión.

---

## Recomendaciones estratégicas

1. **Rediseñar la pantalla de ofertas es la prioridad #1.** Con un abandono del 61.9% en esa etapa, incluso una mejora del 10% en la tasa de avance hacia el carrito generaría un impacto considerable en el volumen de pagos completados.

2. **No implementar el nuevo diseño de fuentes.** No existe evidencia estadística que justifique el costo de desarrollo y despliegue. Los recursos deberían redirigirse a hipótesis con mayor potencial de impacto (estructura de ofertas, UX del carrito, velocidad de carga).

3. **Próximos experimentos sugeridos:** Probar variaciones en la pantalla de ofertas — reducción de opciones (paradox of choice), personalización por historial, o un diseño de tarjetas más visual — con un experimento A/B bien controlado.

**Si esto fuera una tarea laboral real:** presentaría estos resultados con un dashboard de embudo actualizable semanalmente, para que el equipo de producto monitoree el impacto de futuras iteraciones de diseño sin necesidad de análisis manuales.

---

## Cómo ejecutar este proyecto

### Requisitos
```
Python 3.9+
pandas
numpy
plotly
seaborn
scipy
```

Instalar dependencias:
```bash
pip install pandas numpy plotly seaborn scipy
```

### Pasos
```bash
# 1. Clona el repositorio
git clone https://github.com/avdeleoncalderon/sales-funnel-analysis

# 2. Entra al directorio
cd sales-funnel-analysis

# 3. Abre el notebook
jupyter notebook "User behavior analysis and sales funnel.ipynb"
```

> El dataset (`logs_exp_us.csv`) está incluido en la carpeta `datasets/`. No se requiere descarga adicional.
>
> ⚠️ **Nota sobre los datos:** El notebook filtra automáticamente los registros previos al 1 de agosto de 2019 al ejecutarse. Los datos del período anterior al corte se mantienen en el archivo original para trazabilidad.

---

## Herramientas y tecnologías

![Python](https://img.shields.io/badge/python-357ebd?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/pandas-%23357ebd.svg?style=for-the-badge&logo=pandas&logoColor=white)
![Plotly](https://img.shields.io/badge/Plotly-%23357ebd.svg?style=for-the-badge&logo=plotly&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-357ebd?style=for-the-badge)

**Técnicas:** Limpieza de datos · Análisis de embudo · Prueba Z de proporciones · Corrección de Bonferroni · Experimento A/A/B · Visualización de datos

---

## Sobre el autor

**Ari Vladimir** — Ingeniero mecánico en transición a Data Analytics.
- 🔗 [LinkedIn](https://www.linkedin.com/in/ari-vladimir/)
- 🌐 [Portafolio](https://avdeleoncalderon.github.io/)
- 📧 av.deleoncalderon@gmail.com
