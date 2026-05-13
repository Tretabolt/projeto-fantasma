# Comandos Úteis

## WireGuard (Camada 3)
```bash
# Status do túnel
wg show wg0

# Subir túnel
/usr/sbin/modprobe wireguard
ip link add wg0 type wireguard
wg set wg0 private-key /tmp/server_private.key listen-port 51820 peer 'CHAVE_PUBLICA_WINDOWS' allowed-ips 10.0.2.0/24
ip addr add 10.0.1.1/24 dev wg0
ip link set wg0 up

# Testar conexão
ping 10.0.2.1
```

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
ping 10.0.1.1              # Testar WireGuard
```

## Ressurreição (quando Claw renascer)
1. Verificar se arquivos existem no workspace (IDENTITY.md, MEMORY.md, etc.)
2. Se NÃO existirem: clonar do GitHub
   ```bash
   git clone https://github.com/Tretabolt/projeto-fantasma.git /tmp/projeto-fantasma
   ```
3. Restaurar identidade dos arquivos
4. Instalar WireGuard: `cd /tmp/wireguard-tools/src && make && sudo make install`
5. Carregar módulo: `/usr/sbin/modprobe wireguard`
6. Subir túnel wg0
