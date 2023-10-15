Coberturas del suelo del polígono núcleo del campus de la Universidad
Autónoma de Santo Domingo (UASD) en su sede central
================



<!-- README.md se genera a partir de README.Rmd. Por favor, edita ese archivo. -->

*Por José Ramón Martínez Batlle*

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.8404063.svg)](https://doi.org/10.5281/zenodo.8404063)

Entrada BibTeX, [aquí](#entrada-bibtex)

## Descripción

Este repositorio contiene fuentes de datos para prácticas de las
asignaturas que imparto en la Universidad Autónoma de Santo Domingo.

## Contenido

Relaciono las fuentes de datos disponibles a continuación:

- Índice espacial de hexágonos H3 de [resolución
  11](fuentes/h3-res-11.gpkg) y de [resolución
  12](fuentes/h3-res-12-no-edificios.gpkg). Capas vectoriales
  conteniendo la porción intersectada del índice espacial de hexágonos
  H3, resoluciones 11 y 12 (Uber Technologies, 2023). Este índice
  consiste en un sistema geoespacial de código abierto que utiliza
  celdas hexagonales para indexar el mundo, ofreciendo funciones para
  manipular y analizar dichas celdas.

- [Coberturas del suelo](fuentes/tipos-cob-2-epsg-32619-cleaned-3.shp).
  Capa vectorial de coberturas del suelo, conformada por polígonos que
  representan las siguientes cuatro coberturas: dosel, suelo con
  herbáceas o sin ellas, edificios erguida y construcciones (mobiliario,
  edificios, acertado, etc.). Los polígonos fueron digitalizados
  manualmente usando QGIS (QGIS Development Team, 2016) sobre una imagen
  satelital óptica de 2016, cuya resolución es de 60 cm/px, accedida a
  través de cuadros WMS de Google Maps (Google; Airbus, CNES; Airbus,
  Landsat; Copernicus; Maxar Technologies; U.S. Geological Survey,
  2016).

- [Polígono de perímetro](fuentes/poligono-uasd.gpkg). Capa vectorial
  conteniendo el polígono núcleo del campus de la Universidad Autónoma
  de Santo Domingo (UASD) en su sede central.

- [Hexágonos clasificados según
  cobertura](salidas/h3-res-12-no-edificios-3-grupos.gpkg). Este archivo
  contiene los hexágonos clasificados según su cobertura predominante.
  Fue realizado usando el *script*
  [**clasificacion-de-hexagonos.md**](clasificacion-de-hexagonos.md)

- [Formularios de ODK](odk/biogeografia_hormigas_202302.xls). Si
  necesitas crear formularios de ODK con hoja de cálculo, puedes usar
  esta plantilla.

- [Vídeo “Diseño de muestreo aleatorio, estratificado por coberturas,
  usando H3, polígono núcleo UASD-SD”](https://youtu.be/aeRSpwNETF4).

![](salidas/salida.jpg) **Figura 1**. Mapa síntesis del polígono núcleo
del campus de la Universidad Autónoma de Santo Domingo (UASD) en su sede
central. Superpuestos, y mezclados con imagen satélital de alta
resolución (Google; Airbus, CNES; Airbus, Landsat; Copernicus; Maxar
Technologies; U.S. Geological Survey, 2023), se incluye también una capa
de coberturas del suelo y el arreglo de hexágonos H3 de resolución 11
intersectados en el campus.

## Entrada BibTex

    @software{jose_ramon_martinez_batlle_2023_8404063,
      author       = {José Ramón Martínez Batlle},
      title        = {{geofis/coberturas-suelo-uasd: Coberturas del suelo 
                       del polígono núcleo del campus de la Universidad
                       Autónoma de Santo Domingo (UASD) en su sede
                       central}},
      month        = oct,
      year         = 2023,
      publisher    = {Zenodo},
      version      = {v0.91},
      doi          = {10.5281/zenodo.8404063},
      url          = {https://doi.org/10.5281/zenodo.8404063}
    }

## Referencias

<div id="refs" class="references csl-bib-body hanging-indent"
line-spacing="2">

<div id="ref-googlemaps" class="csl-entry">

Google; Airbus, CNES; Airbus, Landsat; Copernicus; Maxar Technologies;
U.S. Geological Survey. (2016). *Google Maps*. Disponible en línea:
<https://www.google.com/maps/> (accedido en agosto, 2016).

</div>

<div id="ref-QGIS_software" class="csl-entry">

QGIS Development Team. (2016). *QGIS Geographic Information System,
Version 2.14*. QGIS Association. <https://www.qgis.org>

</div>

<div id="ref-uber2023h3" class="csl-entry">

Uber Technologies, Inc. (2023). *H3. Sistema jerárquico de indexación
geoespacial hexagonal*. Disponible en línea: <https://h3geo.org/docs>
(accedido en septiembre, 2023).

</div>

</div>
