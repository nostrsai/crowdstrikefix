# crowdstrikefix
# simple script to remove the .sys


# Ejec
$currentExecutionPolicy = Get-ExecutionPolicy -Scope Process

# Bypass temp
Set-ExecutionPolicy Bypass -Scope Process -Force

# Define the path where the driver files are located
$driverPath = "C:\Windows\System32\drivers"

# Define the patterns of the files to be displayed
$filePatterns = @("C-00000291*.sys")

# Function to find and display files based on the pattern
function Find-DriverFiles {
 param (
 [string]$path,
 [array]$patterns
 )
 
 $foundFiles = @()
 
 foreach ($pattern in $patterns) {
 $files = Get-ChildItem -Path $path -Filter $pattern -ErrorAction SilentlyContinue
 
 foreach ($file in $files) {
 Write-Host "Found file: $($file.FullName)"
 $foundFiles += $file
 }
 }
 
 return $foundFiles
}

# Find the driver files
$foundFiles = Find-DriverFiles -path $driverPath -patterns $filePatterns

# If no files are found, display a message
if ($foundFiles.Count -eq 0) {
 Write-Host "No matching files found."
}

# Execution Policy 
Set-ExecutionPolicy $currentExecutionPolicy -Scope Process -Force
