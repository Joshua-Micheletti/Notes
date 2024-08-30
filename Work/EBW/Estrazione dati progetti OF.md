## Requisito
Trova tutti i progetti creati nel 2024 che non sono stati cancellati.

Per ogni tipo di progetto, recupera:
- numero di progetti
- superficie media
- progetto con superficie maggiore e minore

Trova il progetto che più si avvicina alla superficie media. screenshot visualizzazione

### Lista di tipi di progetti
```SQL
SELECT DISTINCT "type" FROM public.projects;
```

#### Result
```
null
"creation_cd"
"variante_delivery_cd_gis"
"census"
"census_civil_network"
"creation"
"Census"
"walk-out"
"nam"
"variante_delivery_cd_easy"
"delivery_cd_gis"
"consultazione"
"delivery_cd_easy"
"variante_delivery"
"censuscd"
"census_civil_optical_network"
"sketch"
"Creation"
```

### Query
#### Min, Max, Avg e project più vicino al Avg:
```SQL
WITH surface_area AS (
    SELECT AVG(ST_Area(ST_Transform(geom, 3857)) / 1000000) AS avg_surface_area, MAX(ST_Area(ST_Transform(geom, 3857)) / 1000000) AS max_surface_area, MIN(ST_Area(ST_Transform(geom, 3857)) / 1000000) AS min_surface_area
    FROM projects
    WHERE "type" = 'creation'
	AND (ST_Area(ST_Transform(geom, 3857)) / 1000000) > 0
    AND status != 'cancelled'
)
SELECT geom, (ST_Area(ST_Transform(geom, 3857)) / 1000000) as area, surface_area.max_surface_area, surface_area.min_surface_area, surface_area.avg_surface_area
FROM projects, surface_area
WHERE "type" = 'creation'
AND status != 'cancelled'
ORDER BY ABS((ST_Area(ST_Transform(geom, 3857)) / 1000000) - surface_area.avg_surface_area) ASC
LIMIT 1;
```