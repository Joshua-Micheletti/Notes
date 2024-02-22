## Script Windows
### Test vari
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

```