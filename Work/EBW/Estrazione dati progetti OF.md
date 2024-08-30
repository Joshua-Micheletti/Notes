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
#### Min, Max, Av