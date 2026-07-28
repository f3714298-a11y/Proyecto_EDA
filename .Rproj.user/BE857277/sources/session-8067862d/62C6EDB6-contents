# =====================================================
# ANÁLISIS FINAL
# Impacto de la pandemia COVID-19 en el comercio exterior peruano
# Periodo: 2012 - 2025
# Fuente: Banco Central de Reserva del Perú (BCRP)
# =====================================================


# =====================================================
# 1. LIBRERÍAS
# =====================================================

library(tidyverse)
library(ggplot2)


# =====================================================
# 2. CARGAR BASE LIMPIA DEL EDA
# =====================================================

datos <- readRDS(
  "data/datos_limpios.rds"
)


glimpse(datos)



# =====================================================
# 3. FORMULACIÓN DE LA PREGUNTA DE ANÁLISIS
# =====================================================

# Pregunta:
# ¿Cómo cambió el desempeño del comercio exterior peruano
# antes, durante y después de la pandemia COVID-19?



# =====================================================
# 4. CREACIÓN DE PERIODOS COVID
# =====================================================


datos <- datos %>%
  mutate(
    periodo = case_when(
      
      Fecha < as.Date("2020-01-01") ~ "Antes de pandemia",
      
      Fecha >= as.Date("2020-01-01") &
        Fecha <= as.Date("2021-12-31") ~ "Pandemia",
      
      Fecha > as.Date("2021-12-31") ~ "Después de pandemia"
      
    )
  )


table(datos$periodo)



# =====================================================
# 5. INDICADORES COMPARATIVOS
# =====================================================


comparacion_periodos <- datos %>%
  group_by(periodo) %>%
  summarise(
    
    promedio_exportaciones =
      mean(exportaciones),
    
    promedio_importaciones =
      mean(importaciones),
    
    promedio_balanza =
      mean(balanza_comercial),
    
    max_balanza =
      max(balanza_comercial),
    
    min_balanza =
      min(balanza_comercial)
    
  )


comparacion_periodos



# =====================================================
# 6. CAMBIOS PORCENTUALES
# =====================================================


cambios <- comparacion_periodos %>%
  mutate(
    
    cambio_exportaciones =
      (promedio_exportaciones -
         lag(promedio_exportaciones)) /
      lag(promedio_exportaciones)*100,
    
    
    cambio_importaciones =
      (promedio_importaciones -
         lag(promedio_importaciones)) /
      lag(promedio_importaciones)*100,
    
    
    cambio_balanza =
      (promedio_balanza -
         lag(promedio_balanza)) /
      lag(promedio_balanza)*100
    
  )


cambios



# =====================================================
# 7. GRÁFICO 1
# Evolución de balanza comercial con pandemia
# =====================================================

grafico_covid_balanza <- ggplot(
  datos,
  aes(
    x = Fecha,
    y = balanza_comercial
  )
)+
  geom_rect(
    aes(
      xmin = as.Date("2020-03-01"),
      xmax = as.Date("2021-12-31"),
      ymin = -Inf,
      ymax = Inf
    ),
    inherit.aes = FALSE,
    fill = "#FEE08B",
    alpha = 0.25
  )+
  geom_line(
    color = "#2C7FB8",
    linewidth = 1.2
  )+
  geom_hline(
    yintercept = 0,
    color = "red",
    linetype = "dashed"
  )+
  geom_vline(
    xintercept = as.Date("2020-03-01"),
    color = "black",
    linetype = "dotted"
  )+
  labs(
    title = "Evolución de la balanza comercial peruana",
    subtitle = "La zona amarilla representa el período de pandemia",
    x = "Fecha",
    y = "Millones de US$ FOB"
  )+
  theme_minimal(base_size = 13)+
  theme(
    plot.title = element_text(face = "bold"),
    plot.subtitle = element_text(color = "gray40")
  )

grafico_covid_balanza


# =====================================================
# 8. GRÁFICO 2
# Comparación de exportaciones por periodo
# =====================================================

grafico_export_periodos <- ggplot(
  comparacion_periodos,
  aes(
    periodo,
    promedio_exportaciones,
    fill = periodo
  )
)+
  geom_col(
    width = .65
  )+
  geom_text(
    aes(
      label = round(promedio_exportaciones,0)
    ),
    vjust = -.4,
    fontface = "bold"
  )+
  scale_fill_manual(values = c(
    "#66C2A5",
    "#FC8D62",
    "#8DA0CB"
  ))+
  labs(
    title = "Exportaciones promedio por período",
    subtitle = "Comparación antes, durante y después de la pandemia",
    x = "",
    y = "Millones de US$ FOB"
  )+
  theme_minimal(base_size = 13)+
  theme(
    legend.position = "none",
    plot.title = element_text(face="bold")
  )

grafico_export_periodos


# =====================================================
# 9. GRÁFICO 3
# Comparación exportaciones e importaciones
# =====================================================
grafico_comercio_periodos <- ggplot(
  comercio_periodos,
  aes(
    periodo,
    valor,
    fill = variable
  )
)+
  geom_col(
    position = "dodge",
    width = .7
  )+
  geom_text(
    aes(
      label = round(valor,0)
    ),
    position = position_dodge(.7),
    vjust = -.35,
    size = 3.8
  )+
  scale_fill_manual(values = c(
    promedio_exportaciones = "#1B9E77",
    promedio_importaciones = "#D95F02"
  ))+
  labs(
    title = "Comercio exterior peruano antes, durante y después de la pandemia",
    subtitle = "Promedio mensual de exportaciones e importaciones",
    x = "",
    y = "Millones de US$ FOB",
    fill = ""
  )+
  theme_minimal(base_size = 13)+
  theme(
    plot.title = element_text(face="bold"),
    legend.position = "top"
  )

grafico_comercio_periodos

# Gráfico 3
grafico_balanza_periodos <- ggplot(
  comparacion_periodos,
  aes(
    periodo,
    promedio_balanza,
    fill = periodo
  )
)+
  geom_col(width=.6)+
  geom_text(
    aes(
      label = round(promedio_balanza,0)
    ),
    vjust=-.4,
    fontface="bold"
  )+
  scale_fill_manual(values=c(
    "#66C2A5",
    "#FC8D62",
    "#8DA0CB"
  ))+
  labs(
    title="Balanza comercial promedio por período",
    subtitle="Comparación antes, durante y después de la pandemia",
    x="",
    y="Millones de US$ FOB"
  )+
  theme_minimal(base_size=13)+
  theme(
    legend.position="none",
    plot.title=element_text(face="bold")
  )

grafico_balanza_periodos

# =====================================================
# 10. GUARDAR GRÁFICOS
# =====================================================


if(!dir.exists("figuras")){
  dir.create("figuras")
}



ggsave(
  "figuras/covid_balanza.png",
  grafico_covid_balanza,
  width=8,
  height=5
)


ggsave(
  "figuras/exportaciones_periodos_covid.png",
  grafico_export_periodos,
  width=8,
  height=5
)


ggsave(
  "figuras/comercio_periodos_covid.png",
  grafico_comercio_periodos,
  width=8,
  height=5
)

ggsave(
  "figuras/balanza_periodos_covid.png",
  grafico_balanza_periodos,
  width = 8,
  height = 5
)

# =====================================================
# FIN DEL ANÁLISIS FINAL
# =====================================================

