Clasificación de hexágonos del polígono núcleo del campus de la
Universidad Autónoma de Santo Domingo (UASD) en su sede central, usando
coberturas del suelo
================



<!-- README.md se genera a partir de README.Rmd. Por favor, edita ese archivo. -->

*Por José Ramón Martínez Batlle*

[![DOI](https://zenodo.org/badge/692893783.svg)](https://zenodo.org/badge/latestdoi/692893783)

Entrada BibTeX, [aquí](#entrada-bibtex)

## Descripción

Este *script* realiza la clasificación de hexágonos del polígono núcleo
del campus de la Universidad Autónoma de Santo Domingo (UASD) en su sede
central, usando coberturas del suelo

## Paquetes, datos

``` r
# Paquetes
library(sf)
library(tidyverse)
library(RColorBrewer)

# Directorio de fuentes
fuentes <- 'fuentes'

# Leer los archivos
hexagonos <- st_read(paste0(fuentes, "/", "h3-res-12-no-edificios.gpkg")) %>% 
  st_transform(32619)
```

    ## Reading layer `h3-res-12-no-edificios' from data source 
    ##   `/home/jose/Documentos/git/coberturas-suelo-uasd/fuentes/h3-res-12-no-edificios.gpkg' 
    ##   using driver `GPKG'
    ## Simple feature collection with 1490 features and 2 fields
    ## Geometry type: POLYGON
    ## Dimension:     XY
    ## Bounding box:  xmin: -69.9222 ymin: 18.45794 xmax: -69.91225 ymax: 18.46511
    ## Geodetic CRS:  WGS 84

``` r
coberturas <- st_read(paste0(fuentes, "/", "tipos-cob-2-epsg-32619-cleaned-3.shp"))
```

    ## Reading layer `tipos-cob-2-epsg-32619-cleaned-3' from data source 
    ##   `/home/jose/Documentos/git/coberturas-suelo-uasd/fuentes/tipos-cob-2-epsg-32619-cleaned-3.shp' 
    ##   using driver `ESRI Shapefile'
    ## Simple feature collection with 473 features and 4 fields
    ## Geometry type: POLYGON
    ## Dimension:     XY
    ## Bounding box:  xmin: 402630.6 ymin: 2041036 xmax: 403673.1 ymax: 2041901
    ## Projected CRS: WGS 84 / UTM zone 19N

## Intersectar los hexágonos

``` r
hexagonos_cortados <- st_intersection(hexagonos, coberturas)
```

## Calcular el área de cada hexágono

``` r
hexagonos_cortados_area <- hexagonos_cortados %>% 
  mutate(area_hexagono = st_area(.) %>% units::drop_units())
```

## Calcular el porcentaje de cada cobertura

``` r
hexagonos_cortados_pct <- hexagonos_cortados_area %>%
  group_by(index) %>%
  mutate(porc_CONS = ifelse(tipo == "CONS", area_hexagono, 0) / sum(area_hexagono) * 100,
         porc_DOSE = ifelse(tipo == "DOSE", area_hexagono, 0) / sum(area_hexagono) * 100,
         porc_EDIF = ifelse(tipo == "EDIF", area_hexagono, 0) / sum(area_hexagono) * 100,
         porc_SUEL = ifelse(tipo == "SUEL", area_hexagono, 0) / sum(area_hexagono) * 100) %>%
  ungroup()

hexagonos_pct <- hexagonos %>%
  inner_join(hexagonos_cortados_pct %>%
               select(index, indice_propio, matches('porc*')) %>%
               st_drop_geometry() %>%
               pivot_longer(cols = matches('porc*')) %>%
               filter(value!=0) %>%
               group_by(index, indice_propio, name) %>%
               summarise(value = sum(value, na.rm = T)) %>%
               pivot_wider(names_from = 'name', values_from = 'value', values_fill = 0))
```

## Representar

``` r
hexagonos_pct %>%
  pivot_longer(matches('porc*')) %>% 
  ggplot +
  aes(fill = value) +
  geom_sf() +
  theme_bw() + 
  scale_fill_gradientn(colors = brewer.pal(7, "BrBG")) +
  facet_wrap(~ name, nrow = 2)
```

<img src="clasificacion-de-hexagonos_files/figure-gfm/unnamed-chunk-6-1.png" width="100%" style="display: block; margin: auto;" />

## Clasificar

``` r
distancias <- hexagonos_pct %>% st_drop_geometry() %>% select(where(is.numeric)) %>% dist()
agrupamiento <- hclust(distancias, method = 'ward')
hexagonos_pct$grupo <- factor(cutree(agrupamiento, k = 3), labels = c('DOSE', 'CONS', 'SUEL'))
hexagonos_pct %>%
  select(matches('porc*|grupo')) %>% 
  pivot_longer(matches('porc*')) %>% 
  ggplot +
  aes(x = name, y = value, fill = name) +
  geom_boxplot() +
  theme_bw() + 
  scale_fill_manual(values = c('#D2D2D2', '#428B07', '#4A4A4A', '#DDE78E')) +
  facet_grid(~ grupo)
```

<img src="clasificacion-de-hexagonos_files/figure-gfm/unnamed-chunk-7-1.png" width="100%" style="display: block; margin: auto;" />

## Exportar

``` r
hexagonos_pct %>% st_write(dsn = 'salidas/h3-res-12-no-edificios-3-grupos.gpkg', delete_dsn = T)
```

    ## Deleting source `salidas/h3-res-12-no-edificios-3-grupos.gpkg' using driver `GPKG'
    ## Writing layer `h3-res-12-no-edificios-3-grupos' to data source 
    ##   `salidas/h3-res-12-no-edificios-3-grupos.gpkg' using driver `GPKG'
    ## Writing 1490 features with 7 fields and geometry type Polygon.

## Referencias
