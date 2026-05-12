# 🪟 SETUP-WINDOWS.md — Comandos para Daniel (PowerShell)

Daniel, quando precisar configurar o servidor do Claw, roda esses comandos.

---

## 1. Instalar Tailscale no servidor (via SSH)

Se já tiver acesso SSH ao servidor:
```powershell
ssh root@<IP_DO_SERVIDOR> "curl -fsSL https://tailscale.com/install.sh | sh && tailscale up"
```

Ou acessa o servidor direto e roda:
```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

---

## 2. Verificar se o Claw tá conectado

```powershell
# Ping via Tailscale
ping 100.x.x.x

# SSH
ssh -i C:\Users\"daniel palma"\.ssh\id_ed25519_claw root@100.x.x.x
```

---

## 3. Adicionar chave SSH no Windows

Quando o Claw gerar uma chave pública:
```powershell
# Abrir como Admin
notepad C:\ProgramData\ssh\administrators_authorized_keys
# Colar a chave pública do Claw
Restart-Service sshd
```

---

## 4. Iniciar SSH no Windows (se não estiver rodando)

```powershell
Start-Service sshd
Set-Service -Name sshd -StartupType Automatic
```

---

## 5. Reiniciar Tailscale no Windows

```powershell
& "C:\Program Files\Tailscale\tailscale.exe" up --unattended
```

---

## 6. Verificar status Tailscale

```powershell
& "C:\Program Files\Tailscale\tailscale.exe" status
```

---

## IPs importantes

| Máquina | IP Tailscale |
|---------|-------------|
| Windows (dani-lucia) | 100.123.251.110 |
| Servidor Claw | (verificar com `tailscale status`) |

---

🦀⚡🧊
