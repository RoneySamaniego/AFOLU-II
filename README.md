# AFOLU-II
PROYECTOS DE LA REGION
# AGROEUDR_MIAMBIENTE — Streamlit 

Prueba de concepto funcional para un visor de preevaluación espacial de deforestación post-2020 por parcela, pensada para el caso Río Sereno, Renacimiento, Chiriquí, Panamá.

El prototipo permite:

- cargar parcelas en GeoJSON, KML, GPKG o shapefile comprimido en ZIP;
- visualizar parcelas en un mapa interactivo;
- consultar capas de Google Earth Engine;
- cruzar parcelas con bosque base 2020 y pérdida forestal posterior a 2020;
- clasificar cada parcela en categorías preliminares de alerta;
- descargar resultados como CSV, GeoJSON y HTML.

## 1. Alcance

Esta aplicación no certifica cumplimiento EUDR. Funciona como herramienta de **preevaluación espacial** para identificar parcelas sin alerta aparente, parcelas con alerta potencial, parcelas con alerta significativa y parcelas que requieren revisión manual.

La señal de posible deforestación se calcula como:

```text
Bosque ESA WorldCover 2020 ∩ pérdida Hansen 2021-2025 ∩ parcela
```

## 2. Estructura del proyecto

```text
agroeudr_streamlit_poc/
│
├── app.py
├── requirements.txt
├── environment.yml
├── run_local.sh
├── run_local.bat
│
├── src/
│   ├── analysis.py
│   ├── config.py
│   ├── geo_io.py
│   ├── map_utils.py
│   └── rules.py
│
├── data/
│   ├── input_fincas/
│   │   └── sample_parcelas_rio_sereno.geojson
│   └── roi/
│       └── rio_sereno_area_referencia.geojson
│
├── docs/
│   ├── guia_desarrollo.md
│   └── modelo_logico.md
│
└── .streamlit/
    └── config.toml
```

## 3. Instalación con venv

```bash
cd agroeudr_streamlit_poc
python -m venv .venv
source .venv/bin/activate      # Linux/Mac
# .venv\Scripts\activate      # Windows
pip install --upgrade pip
pip install -r requirements.txt
```

## 4. Instalación con conda

```bash
cd agroeudr_streamlit_poc
conda env create -f environment.yml
conda activate agroeudr-streamlit
```

## 5. Autenticación de Google Earth Engine

Antes de ejecutar la app debe tener acceso a Google Earth Engine y un proyecto de Google Cloud habilitado para Earth Engine.

```bash
earthengine authenticate
earthengine set_project ee-pty
```

También puede definir la variable de entorno:

```bash
export EE_PROJECT=TU_PROJECT_ID
```

En Windows PowerShell:

```powershell
$env:EE_PROJECT="TU_PROJECT_ID"
```

## 6. Ejecutar la aplicación

```bash
streamlit run app.py
```

O bien:

```bash
./run_local.sh
```

En Windows:

```bat
run_local.bat
```

## 7. Uso básico

1. Abra la app en el navegador.
2. Escriba el Project ID de Earth Engine si no lo configuró antes.
3. Active las parcelas demo o suba su archivo.
4. Ajuste umbrales si lo desea.
5. Presione **Ejecutar análisis por parcela**.
6. Revise mapa, tabla y categorías.
7. Descargue CSV, GeoJSON o HTML.

## 8. Formatos aceptados

- GeoJSON `.geojson` o `.json`
- KML simple `.kml`
- GeoPackage `.gpkg`
- Shapefile comprimido `.zip`

Para shapefile ZIP, incluya como mínimo `.shp`, `.shx`, `.dbf` y `.prj`.

## 9. Datos usados

- ESA WorldCover 2020: cobertura de la tierra, clase 10 Tree cover.
- Hansen Global Forest Change 2025 v1.13: pérdida de cobertura arbórea 2001-2025.
- Sentinel-2 SR Harmonized: NDVI/NBR opcional.

## 10. Advertencia

Los resultados son preliminares. Para uso institucional se recomienda integrar mapas oficiales de bosque/cobertura de Panamá, límites prediales verificados, validación de campo y criterios técnicos formalizados.
