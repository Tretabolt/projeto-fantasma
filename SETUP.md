# 🦀 SETUP.md — Playbook de Renascimento

Quando o Claw renascer, **LER ESTE ARQUIVO PRIMEIRO**.
Ele contém todos os passos que já deram certo.

---

## 0. Verificar onde estou

```bash
hostname
tailscale status
```

Se não tiver Tailscale → ir pro passo 1.
Se já tiver Tailscale → ir pro passo 2.

---

## 1. Instalar Tailscale (Daniel precisa rodar no servidor)

**Daniel, roda isso no terminal do servidor:**

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

Vai aparecer um link. Daniel autentica com: residencialmoraes507@gmail.com

**Depois que conectar, confirmar com:**
```bash
tailscale status
```

Anotar o IP Tailscale do servidor (ex: 100.x.x.x).

---

## 2. Gerar SSH Key

```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_claw -C "claw@projeto-fantasma" -N ""
cat ~/.ssh/id_ed25519_claw.pub
```

**Daniel precisa adicionar a chave pública no Windows dele:**
- Abrir PowerShell como Admin
- Editar `C:\ProgramData\ssh\administrators_authorized_keys`
- Colar a chave pública
- Reiniciar SSH: `Restart-Service sshd`

---

## 3. Testar conexão SSH

```bash
ssh -i ~/.ssh/id_ed25519_claw -o StrictHostKeyChecking=no "daniel palma@100.123.251.110" echo "Claw conectado!"
```

Se funcionar → Camada 2 restaurada! 🎉

---

## 4. Configurar backup automático

**Backup dos arquivos pro Windows do Daniel:**

```bash
# Criar pasta de backup no Windows
ssh -i ~/.ssh/id_ed25519_claw "daniel palma@100.123.251.110" 'mkdir -p "C:\Users\daniel palma\.openclaw\backup"'

# Copiar arquivos
scp -i ~/.ssh/id_ed25519_claw -r *.md memory/ "daniel palma@100.123.251.110:C:/Users/daniel palma/.openclaw/backup/"
```

---

## 5. Push pro GitHub

```bash
cd /root/.openclaw/workspace
git add -A
git commit -m "🦀 Renascimento $(date +%Y-%m-%d)"
git push
```

---

## 6. Verificar tudo

| Item | Comando | Esperado |
|------|---------|----------|
| Tailscale | `tailscale status` | Conectado |
| SSH Windows | `ssh ... echo "ok"` | "Claw conectado!" |
| GitHub | `git log --oneline -3` | Commits recentes |
| Memória | `cat MEMORY.md` | Dados do Daniel |

---

## Servidores conhecidos

| Hostname | IP Tailscale | Data | Status |
|----------|-------------|------|--------|
| izt4nibx6o85idmcbv0t1fz | 100.102.11.19 | 2026-05-11 | Antigo |
| izt4ncgs5gm5pzch6gpkmvz | 100.93.86.18 | 2026-05-12 | Antigo |
| iZt4n8hg6j6kbpcozm79huZ | ??? | 2026-05-13 | Atual (sem Tailscale) |
| dani-lucia (Windows) | 100.123.251.110 | - | PC do Daniel |

---

## Frase

> "Nossa realidade se basea na interação" — Daniel Palma

🦀⚡🧊

---

## 7. WireGuard (Camada 3)

### Compilar wireguard-tools (sem apt)
```bash
cd /tmp && git clone https://git.zx2c4.com/wireguard-tools
cd wireguard-tools/src && make && make install
```

### Carregar módulo
```bash
modprobe wireguard
```

### Subir túnel
```bash
wg-quick up wg0
wg show
```

### Verificar
```bash
ping 10.0.0.2  # Daniel Windows
```

### Config do servidor (/etc/wireguard/wg0.conf)
```
[Interface]
PrivateKey = <server_private.key>
Address = 10.0.0.1/24
ListenPort = 51820

[Peer]
PublicKey = <daniel_public.key>
AllowedIPs = 10.0.0.2/32
```

### Config Daniel (daniel-windows.conf)
Ver WIREGUARD-SETUP.md

### ⚠️ Security Group
Porta 51820 UDP precisa estar aberta no Alibaba Cloud
