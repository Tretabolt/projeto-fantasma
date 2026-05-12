# Comandos Úteis

## Tailscale
```bash
tailscale status          # Ver status
tailscale up --unattended # Reconectar
tailscale ip -4           # Ver IP
```

## SSH
```bash
ssh -i ~/.ssh/id_ed25519_claw "daniel palma@100.123.251.110"
```

## Backup
```bash
# Backup pro Windows do Daniel
for FILE in IDENTITY.md USER.md MEMORY.md SOUL.md AGENTS.md TOOLS.md HEARTBEAT.md; do
  B64=$(base64 -w0 /root/.openclaw/workspace/$FILE)
  ssh -i ~/.ssh/id_ed25519_claw "daniel palma@100.123.251.110" \
    "powershell -NoProfile -Command \"[IO.File]::WriteAllBytes('C:\\Users\\daniel palma\\.openclaw\\backup\\$FILE',[Convert]::FromBase64String('$B64'))\""
done
```

## Windows (PowerShell)
```powershell
Get-Service sshd           # Status SSH
Start-Service sshd         # Iniciar SSH
Get-Service Tailscale*     # Status Tailscale
& "C:\Program Files\Tailscale\tailscale.exe" up --unattended
```
