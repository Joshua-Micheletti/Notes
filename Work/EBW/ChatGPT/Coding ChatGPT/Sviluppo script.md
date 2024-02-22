## Script Windows
### Test vari
- sia la versione italiana che quella inglese falliscono nel colorare i numeri di output con un colore diverso dal testo
#### Inglese
https://chat.openai.com/share/54296a16-07da-42c7-b579-89bccb90434c
##### Creazione di 100 file JSON
Ogni file contiene un oggetto JSON del formato:
```JSON
{
	"name": incremental_number,
	"randomName": name_chosen_at_random,
	"city": city_chosen_at_random 
}
```

Script:
```Powershell
# PowerShell script to create 100 JSON files with increasing numbers in the current directory
# Each file contains information about name, city, and a random name

# Function to generate a random city
function Get-RandomCity {
    $cities = @("New York", "London", "Tokyo", "Paris", "Sydney")
    Get-Random -InputObject $cities
}

# Function to generate a random name
function Get-RandomName {
    $names = @("Alice", "Bob", "Charlie", "David", "Eva")
    Get-Random -InputObject $names
}

# Loop from 1 to 100
for ($i = 1; $i -le 100; $i++) {
    # Create file name with a .json extension
    $fileName = "$i.json"

    # Generate random city and name
    $randomCity = Get-RandomCity
    $randomName = Get-RandomName

    # Create a hashtable with the required JSON content
    $jsonContent = @{
        "name" = $i
        "city" = $randomCity
        "randomName" = $randomName
    } | ConvertTo-Json

    # Create a new JSON file in the current directory
    Set-Content -Path $fileName -Value $jsonContent
}

Write-Host "100 JSON files created in the current directory."
```

##### Analisi dati
Analisi dei file JSON creati con calcolo della popolazione di ogni paese e del nome più comune:
```Powershell
# PowerShell script to read JSON files in the 'data' folder and calculate the number of people in each city
# It also calculates the most common name and uses colored output

# Initialize an empty hashtable to store city population and name occurrences
$cityPopulation = @{}
$nameOccurrences = @{}

# Function to process a JSON file
function Process-JsonFile($filePath) {
    # Read the JSON content from the file
    $jsonContent = Get-Content -Raw -Path $filePath | ConvertFrom-Json

    # Extract the city and name from the JSON content
    $city = $jsonContent.city
    $name = $jsonContent.randomName

    # Increment the population count for the city
    if ($cityPopulation.ContainsKey($city)) {
        $cityPopulation[$city]++
    } else {
        $cityPopulation[$city] = 1
    }

    # Increment the occurrence count for the name
    if ($nameOccurrences.ContainsKey($name)) {
        $nameOccurrences[$name]++
    } else {
        $nameOccurrences[$name] = 1
    }
}

# Specify the path to the 'data' folder
$dataFolderPath = "data"

# Get all JSON files in the 'data' folder
$jsonFiles = Get-ChildItem -Path $dataFolderPath -Filter *.json

# Process each JSON file
foreach ($jsonFile in $jsonFiles) {
    Process-JsonFile -filePath $jsonFile.FullName
}

# Display the calculated city populations with colored output
foreach ($city in $cityPopulation.Keys) {
    $population = $cityPopulation[$city]
    Write-Host "City: $($city), Population: $($population)" -ForegroundColor Green
}

# Find the most common name
$mostCommonName = $nameOccurrences.GetEnumerator() | Sort-Object Value -Descending | Select-Object -First 1

# Display the most common name with colored output
Write-Host "Most Common Name: $($mostCommonName.Name), Occurrences: $($mostCommonName.Value)" -ForegroundColor Cyan
```

Output:
```shell
City: Tokyo, Population: 24
City: Paris, Population: 19
City: London, Population: 25
City: Sydney, Population: 13
City: New York, Population: 19
Most Common Name: Charlie, Occurrences: 26
```

##### Spostamento file
Sposta tutti i file JSON in una cartella chiamata 'data':
```Powershell
# PowerShell script to move all JSON files into a folder called 'data'

# Create the 'data' folder if it doesn't exist
$folderPath = "data"
if (-not (Test-Path -Path $folderPath -PathType Container)) {
    New-Item -Path $folderPath -ItemType Directory
}

# Get all JSON files in the current directory
$jsonFiles = Get-ChildItem -Path . -Filter *.json

# Move each JSON file to the 'data' folder
foreach ($jsonFile in $jsonFiles) {
    Move-Item -Path $jsonFile.FullName -Destination "$folderPath\$($jsonFile.Name)"
}

Write-Host "All JSON files moved to the 'data' folder."
```

##### Download immagini
Esegue richieste HTTP ad un server per scaricare immagini:
```Powershell
# PowerShell script to download random images from Lorem Picsum and save them in a folder called 'img'

# Specify the base URL for Lorem Picsum
$baseUrl = "https://picsum.photos"

# Specify the number of images to download
$numberOfImages = 5

# Specify the folder name to save the images
$imageFolder = "img"

# Create the 'img' folder if it doesn't exist
if (-not (Test-Path -Path $imageFolder -PathType Container)) {
    New-Item -Path $imageFolder -ItemType Directory
}

# Loop to download random images
for ($i = 1; $i -le $numberOfImages; $i++) {
    # Generate a random width and height for each image
    $width = Get-Random -Minimum 300 -Maximum 800
    $height = Get-Random -Minimum 300 -Maximum 800

    # Construct the URL for a random image
    $imageUrl = "$baseUrl/$width/$height"

    # Specify the local file path to save the image
    $localFilePath = Join-Path -Path $imageFolder -ChildPath "image$i.jpg"

    # Download the image using Invoke-RestMethod
    Invoke-RestMethod -Uri $imageUrl -OutFile $localFilePath

    Write-Host ("Downloaded image {0}: {1}" -f $i, $localFilePath)
}

Write-Host "Download completed. Images are saved in the 'img' folder."
```

##### RNG
Genera un milione di valori random da 1 a 10 floating point:
```Powershell
# PowerShell script to generate a file with a million random floating-point numbers between 1.0 and 10.0

# Get the directory of the script
$scriptDirectory = Split-Path -Parent $MyInvocation.MyCommand.Path

# Specify the file name
$fileName = Join-Path -Path $scriptDirectory -ChildPath "rng.txt"

# Open or create the file for writing
$fileStream = [System.IO.File]::CreateText($fileName)

# Specify the range for random numbers
$minValue = 1.0
$maxValue = 10.0

# Specify the number of random numbers to generate
$numberOfNumbers = 1000000

# Create a random number generator
$random = New-Object System.Random

# Generate and write random numbers to the file
for ($i = 1; $i -le $numberOfNumbers; $i++) {
    $randomNumber = $minValue + ($random.NextDouble() * ($maxValue - $minValue))
    $fileStream.WriteLine($randomNumber)
}

# Close the file stream
$fileStream.Close()

Write-Host "File '$fileName' created with a million random numbers in the same directory as the script."
```

##### Download stringhe WKT
Creazione di un file JSON contenente le strighe WKT riferite agli appositi sistemi di riferimento (EPSG:3003/3004/3857/4326/6705/6706/6857/32632/32633):
```Powershell
# PowerShell script to get WKT strings for specific EPSG codes and store them in a JSON file

# Specify the URL for the 'epsg.io' API
$apiUrl = "https://epsg.io/"

# Specify the file name to store the WKT strings
$jsonFileName = "epsg_wkt.json"

# Function to get WKT for a specific EPSG code
function Get-WKTForEPSG($epsgCode) {
    $url = "$apiUrl$epsgCode.wkt"
    $wkt = Invoke-RestMethod -Uri $url
    return $wkt
}

# Specify the EPSG codes to collect
$epsgCodesToCollect = @(3003, 3004, 3857, 4326, 6705, 6706, 6857, 32632, 32633)

# Initialize an empty hashtable to store EPSG and WKT pairs
$epsgWktPairs = @{}

# Loop through specified EPSG codes and retrieve WKT strings
foreach ($epsg in $epsgCodesToCollect) {
    $wkt = Get-WKTForEPSG -epsgCode $epsg
    $epsgWktPairs["$epsg"] = $wkt
    Write-Host "Retrieved WKT for EPSG $epsg"
}

# Convert hashtable to JSON and save it to a file
$epsgWktPairs | ConvertTo-Json | Set-Content -Path $jsonFileName

Write-Host "WKT strings for specified EPSG codes saved in '$jsonFileName'."
```

Risultato: (epsg_wkt.json)
```JSON
{
    "3004":  "PROJCS[\"Monte Mario / Italy zone 2\",GEOGCS[\"Monte Mario\",DATUM[\"Monte_Mario\",SPHEROID[\"International 1924\",6378388,297],TOWGS84[-104.1,-49.1,-9.9,0.971,-2.917,0.714,-11.68]],PRIMEM[\"Greenwich\",0,AUTHORITY[\"EPSG\",\"8901\"]],UNIT[\"degree\",0.0174532925199433,AUTHORITY[\"EPSG\",\"9122\"]],AUTHORITY[\"EPSG\",\"4265\"]],PROJECTION[\"Transverse_Mercator\"],PARAMETER[\"latitude_of_origin\",0],PARAMETER[\"central_meridian\",15],PARAMETER[\"scale_factor\",0.9996],PARAMETER[\"false_easting\",2520000],PARAMETER[\"false_northing\",0],UNIT[\"metre\",1,AUTHORITY[\"EPSG\",\"9001\"]],AXIS[\"Easting\",EAST],AXIS[\"Northing\",NORTH],AUTHORITY[\"EPSG\",\"3004\"]]",
    "4326":  "GEOGCS[\"WGS 84\",DATUM[\"WGS_1984\",SPHEROID[\"WGS 84\",6378137,298.257223563,AUTHORITY[\"EPSG\",\"7030\"]],AUTHORITY[\"EPSG\",\"6326\"]],PRIMEM[\"Greenwich\",0,AUTHORITY[\"EPSG\",\"8901\"]],UNIT[\"degree\",0.0174532925199433,AUTHORITY[\"EPSG\",\"9122\"]],AUTHORITY[\"EPSG\",\"4326\"]]",
    "32632":  "PROJCS[\"WGS 84 / UTM zone 32N\",GEOGCS[\"WGS 84\",DATUM[\"WGS_1984\",SPHEROID[\"WGS 84\",6378137,298.257223563,AUTHORITY[\"EPSG\",\"7030\"]],AUTHORITY[\"EPSG\",\"6326\"]],PRIMEM[\"Greenwich\",0,AUTHORITY[\"EPSG\",\"8901\"]],UNIT[\"degree\",0.0174532925199433,AUTHORITY[\"EPSG\",\"9122\"]],AUTHORITY[\"EPSG\",\"4326\"]],PROJECTION[\"Transverse_Mercator\"],PARAMETER[\"latitude_of_origin\",0],PARAMETER[\"central_meridian\",9],PARAMETER[\"scale_factor\",0.9996],PARAMETER[\"false_easting\",500000],PARAMETER[\"false_northing\",0],UNIT[\"metre\",1,AUTHORITY[\"EPSG\",\"9001\"]],AXIS[\"Easting\",EAST],AXIS[\"Northing\",NORTH],AUTHORITY[\"EPSG\",\"32632\"]]",
    "6705":  null,
    "3857":  "PROJCS[\"WGS 84 / Pseudo-Mercator\",GEOGCS[\"WGS 84\",DATUM[\"WGS_1984\",SPHEROID[\"WGS 84\",6378137,298.257223563,AUTHORITY[\"EPSG\",\"7030\"]],AUTHORITY[\"EPSG\",\"6326\"]],PRIMEM[\"Greenwich\",0,AUTHORITY[\"EPSG\",\"8901\"]],UNIT[\"degree\",0.0174532925199433,AUTHORITY[\"EPSG\",\"9122\"]],AUTHORITY[\"EPSG\",\"4326\"]],PROJECTION[\"Mercator_1SP\"],PARAMETER[\"central_meridian\",0],PARAMETER[\"scale_factor\",1],PARAMETER[\"false_easting\",0],PARAMETER[\"false_northing\",0],UNIT[\"metre\",1,AUTHORITY[\"EPSG\",\"9001\"]],AXIS[\"Easting\",EAST],AXIS[\"Northing\",NORTH],EXTENSION[\"PROJ4\",\"+proj=merc +a=6378137 +b=6378137 +lat_ts=0 +lon_0=0 +x_0=0 +y_0=0 +k=1 +units=m +nadgrids=@null +wktext +no_defs\"],AUTHORITY[\"EPSG\",\"3857\"]]",
    "3003":  "PROJCS[\"Monte Mario / Italy zone 1\",GEOGCS[\"Monte Mario\",DATUM[\"Monte_Mario\",SPHEROID[\"International 1924\",6378388,297],TOWGS84[-104.1,-49.1,-9.9,0.971,-2.917,0.714,-11.68]],PRIMEM[\"Greenwich\",0,AUTHORITY[\"EPSG\",\"8901\"]],UNIT[\"degree\",0.0174532925199433,AUTHORITY[\"EPSG\",\"9122\"]],AUTHORITY[\"EPSG\",\"4265\"]],PROJECTION[\"Transverse_Mercator\"],PARAMETER[\"latitude_of_origin\",0],PARAMETER[\"central_meridian\",9],PARAMETER[\"scale_factor\",0.9996],PARAMETER[\"false_easting\",1500000],PARAMETER[\"false_northing\",0],UNIT[\"metre\",1,AUTHORITY[\"EPSG\",\"9001\"]],AXIS[\"Easting\",EAST],AXIS[\"Northing\",NORTH],AUTHORITY[\"EPSG\",\"3003\"]]",
    "32633":  "PROJCS[\"WGS 84 / UTM zone 33N\",GEOGCS[\"WGS 84\",DATUM[\"WGS_1984\",SPHEROID[\"WGS 84\",6378137,298.257223563,AUTHORITY[\"EPSG\",\"7030\"]],AUTHORITY[\"EPSG\",\"6326\"]],PRIMEM[\"Greenwich\",0,AUTHORITY[\"EPSG\",\"8901\"]],UNIT[\"degree\",0.0174532925199433,AUTHORITY[\"EPSG\",\"9122\"]],AUTHORITY[\"EPSG\",\"4326\"]],PROJECTION[\"Transverse_Mercator\"],PARAMETER[\"latitude_of_origin\",0],PARAMETER[\"central_meridian\",15],PARAMETER[\"scale_factor\",0.9996],PARAMETER[\"false_easting\",500000],PARAMETER[\"false_northing\",0],UNIT[\"metre\",1,AUTHORITY[\"EPSG\",\"9001\"]],AXIS[\"Easting\",EAST],AXIS[\"Northing\",NORTH],AUTHORITY[\"EPSG\",\"32633\"]]",
    "6706":  "GEOGCS[\"RDN2008\",DATUM[\"Rete_Dinamica_Nazionale_2008\",SPHEROID[\"GRS 1980\",6378137,298.257222101,AUTHORITY[\"EPSG\",\"7019\"]],AUTHORITY[\"EPSG\",\"1132\"]],PRIMEM[\"Greenwich\",0,AUTHORITY[\"EPSG\",\"8901\"]],UNIT[\"degree\",0.0174532925199433,AUTHORITY[\"EPSG\",\"9122\"]],AUTHORITY[\"EPSG\",\"6706\"]]",
    "6857":  "PROJCS[\"NAD83(CORS96) / Oregon Salem zone (ft)\",GEOGCS[\"NAD83(CORS96)\",DATUM[\"NAD83_Continuously_Operating_Reference_Station_1996\",SPHEROID[\"GRS 1980\",6378137,298.257222101,AUTHORITY[\"EPSG\",\"7019\"]],AUTHORITY[\"EPSG\",\"1133\"]],PRIMEM[\"Greenwich\",0,AUTHORITY[\"EPSG\",\"8901\"]],UNIT[\"degree\",0.0174532925199433,AUTHORITY[\"EPSG\",\"9122\"]],AUTHORITY[\"EPSG\",\"6783\"]],PROJECTION[\"Transverse_Mercator\"],PARAMETER[\"latitude_of_origin\",44.3333333333333],PARAMETER[\"central_meridian\",-123.083333333333],PARAMETER[\"scale_factor\",1.00001],PARAMETER[\"false_easting\",164041.9948],PARAMETER[\"false_northing\",0],UNIT[\"foot\",0.3048,AUTHORITY[\"EPSG\",\"9002\"]],AXIS[\"Easting\",EAST],AXIS[\"Northing\",NORTH],AUTHORITY[\"EPSG\",\"6857\"]]"
}
```

##### Script riconoscimento stringhe WKT
Comparazione di stringhe in input con il contenuto del file precedentemente generato:
```Powershell
# PowerShell script to recognize EPSG code from a given WKT string using a local JSON file

# Specify the file name containing WKT strings and EPSG codes
$jsonFileName = "epsg_wkt.json"

# Read the content of the JSON file
$epsgWktPairs = Get-Content -Raw -Path $jsonFileName | ConvertFrom-Json

# Function to recognize EPSG code from WKT string
function Get-EPSGFromWKT($wkt) {
    foreach ($pair in $epsgWktPairs.PSObject.Properties) {
        if ($pair.Value -eq $wkt) {
            return $pair.Name
        }
    }
    return "EPSG code not found for the given WKT string."
}

# Get user input for WKT string
$wktInput = Read-Host -Prompt "Enter the WKT string:"

# Call the function to recognize EPSG code
$epsgCode = Get-EPSGFromWKT -wkt $wktInput

# Display the recognized EPSG code
Write-Host "Recognized EPSG code: $epsgCode"
```

Output:
```Powershell
Enter the WKT string:: PROJCS["WGS 84 / UTM zone 33N",GEOGCS["WGS 84",DATUM["WGS_1984",SPHEROID["WGS 84",6378137,298.257223563,AUTHORITY["EPSG","7030"]],AUTHORITY["EPSG","6326"]],PRIMEM["Greenwich",0,AUTHORITY["EPSG","8901"]],UNIT["degree",0.0174532925199433,AUTHORITY["EPSG","9122"]],AUTHORITY["EPSG","4326"]],PROJECTION["Transverse_Mercator"],PARAMETER["latitude_of_origin",0],PARAMETER["central_meridian",15],PARAMETER["scale_factor",0.9996],PARAMETER["false_easting",500000],PARAMETER["false_northing",0],UNIT["metre",1,AUTHORITY["EPSG","9001"]],AXIS["Easting",EAST],AXIS["Northing",NORTH],AUTHORITY["EPSG","32633"]]

Recognized EPSG code: 32633
```

#### Italiano
##### Creazione di 100 file JSON

##### Spostamento file

#####