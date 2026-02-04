# ====== KONFIGURACJA ======
$organization = "your-organization"
$project      = "your-project"
$pat          = "YOUR_PAT_HERE"

# ====== BUDOWA URL ======
$uri = "https://dev.azure.com/$organization/$project/_apis/tfvc?api-version=7.0"

# ====== AUTORYZACJA ======
# Azure DevOps wymaga Basic Auth, gdzie:
# username = cokolwiek (najczęściej pusty string)
# password = PAT
$base64AuthInfo = [Convert]::ToBase64String(
    [Text.Encoding]::ASCII.GetBytes(":$pat")
)

$headers = @{
    Authorization = "Basic $base64AuthInfo"
    Accept        = "application/json"
}

# ====== WYWOŁANIE API ======
try {
    $response = Invoke-RestMethod `
        -Method Get `
        -Uri $uri `
        -Headers $headers

    Write-Host "Request succeeded" -ForegroundColor Green
    $response | ConvertTo-Json -Depth 10
}
catch {
    Write-Host "Request failed" -ForegroundColor Red
    Write-Host $_.Exception.Message
}
