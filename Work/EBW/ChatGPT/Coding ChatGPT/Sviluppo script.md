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

