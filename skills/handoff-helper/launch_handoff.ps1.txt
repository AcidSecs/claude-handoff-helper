# launch_handoff.ps1 : opens a NEW Claude desktop chat with a start prompt and presses Enter.
# Usage: powershell -File launch_handoff.ps1 -PromptFile "C:\path\to\start_prompt.txt"
# It only runs after the user typed OK. It sends nothing over the network.
param([Parameter(Mandatory=$true)][string]$PromptFile)
Add-Type -AssemblyName System.Windows.Forms,Microsoft.VisualBasic
if (-not (Test-Path $PromptFile)) { Write-Error "Prompt file not found: $PromptFile"; exit 1 }
$q = Get-Content -Raw -Encoding UTF8 $PromptFile
Start-Process ("claude://cowork/new?q=" + [uri]::EscapeDataString($q))
Start-Sleep 8
$p = Get-Process claude -ErrorAction SilentlyContinue | Where-Object { $_.MainWindowHandle -ne 0 } | Select-Object -First 1
if ($p) {
  [Microsoft.VisualBasic.Interaction]::AppActivate($p.Id)
  Start-Sleep 1
  [System.Windows.Forms.SendKeys]::SendWait("{ENTER}")
  Write-Output "Sent Enter to the Claude window."
} else {
  Write-Output "No Claude window found. Press Enter yourself in the new chat."
}
