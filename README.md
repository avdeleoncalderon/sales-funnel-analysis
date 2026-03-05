# Análisis de comportamiento de usuarios y embudo de ventas
Este proyecto analiza el embudo de ventas de la aplicación de una empresa de alimentos para **identificar las etapas con mayores pérdidas de usuarios** y evalúa, mediante un experimento A/A/B, si un nuevo diseño de fuentes puede **mejorar la conversión** en comparación con el diseño actual. El objetivo es proporcionar **insights basados en datos** que guíen **decisiones estratégicas sobre diseño y funcionalidad**.

### Herramientas y tipo de proyecto
![Python](https://img.shields.io/badge/python-357ebd?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/pandas-%23357ebd.svg?style=for-the-badge&logo=pandas&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-357ebd?style=for-the-badge)
![Plotly](https://img.shields.io/badge/Plotly-%23357ebd.svg?style=for-the-badge&logo=plotly&logoColor=white)
![Limpieza de datos](https://img.shields.io/badge/Limpieza_de_datos-295F98?style=for-the-badge)
![Transformación de datos](https://img.shields.io/badge/Transformación_de_datos-295F98?style=for-the-badge)
![Análisis de datos](https://img.shields.io/badge/Análisis_de_datos-295F98?style=for-the-badge)
![Tests A/B](https://img.shields.io/badge/Tests_A/B-295F98?style=for-the-badge)
![Pruebas de hipótesis](https://img.shields.io/badge/Pruebas_de_hipótesis-295F98?style=for-the-badge)
![Visualización de datos](https://img.shields.io/badge/Visualización_de_datos-295F98?style=for-the-badge)

## Preguntas clave
1. ¿Qué eventos del embudo de ventas tienen mayores tasas de abandono?
2. ¿Qué porcentaje de usuarios completa el embudo de ventas desde el inicio hasta el pago?
3. ¿El cambio en el diseño de las fuentes afecta significativamente la conversión?
4. ¿Hay diferencias estadísticas entre los grupos de control y el grupo de prueba?

## Metodología
- **Preprocesamiento de datos:** Se ajustaron los nombres de las columnas, se eliminaron duplicados y se filtraron registros incompletos.
- **Análisis del embudo de ventas:** Se identificaron eventos clave y la proporción de usuarios que avanzan entre etapas.
- **Experimentación A/A/B:** Se compararon conversiones entre grupos de control y prueba mediante pruebas de hipótesis estadísticas.

## Conclusiones y recomendaciones
### Embudo de ventas:
- El evento OffersScreenAppear es donde más usuarios abandonan (61.9%).
- Solo el 47.7% de los usuarios completa el embudo de ventas hasta el pago exitoso.
### Resultados del experimento:
- No se encontraron diferencias estadísticas significativas entre los grupos de control y el grupo de prueba.
- Las nuevas fuentes no generan un impacto positivo en la conversión, por lo que no se recomienda implementar este cambio.
### Recomendaciones:
- Optimizar la pantalla de ofertas para retener más usuarios en esa etapa.
- Priorizar otros cambios en el diseño o funcionalidad de la aplicación con mayor potencial de impacto.

## Diccionario de datos
Cada entrada de registro es una acción de usuario o un evento.
- `EventName`: nombre del evento.
- `DeviceIDHash`: identificador de usuario unívoco.
- `EventTimestamp`: hora del evento.
- `ExpId`: número de experimento: 246 y 247 son los grupos de control, 248 es el grupo de prueba.
