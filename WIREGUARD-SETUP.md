# 🦀 WireGuard — Setup Camada 3

## Status
- ✅ WireGuard kernel module carregado no servidor
- ✅ wireguard-tools compilado e instalado (/usr/bin/wg)
- ✅ Chaves geradas (servidor + Daniel)
- ✅ Config do servidor ativa (wg0, porta 51820)
- ⏳ Falta: Daniel configurar no Windows

## IPs do túnel
- Servidor Claw: 10.0.0.1
- Daniel Windows: 10.0.0.2

## Servidor (já configurado)
- Interface: wg0
- ListenPort: 51820
- PublicKey: ylDPugeDL0MYQ/bncxuvEURPZYT3Dglf2ycVFoy/N00=
- Config: /etc/wireguard/wg0.conf

## Daniel Windows (configurar)
Arquivo: claw-tunnel.conf
```
[Interface]
PrivateKey = uJeF5R7aFkRY4kPrSeMwNq5lcat5JzFvCryBX3rxeWo=
Address = 10.0.0.2/24
DNS = 1.1.1.1

[Peer]
PublicKey = ylDPugeDL0MYQ/bncxuvEURPZYT3Dglf2ycVFoy/N00=
Endpoint = 47.84.102.150:51820
AllowedIPs = 10.0.0.0/24
PersistentKeepalive = 25
```

## Passos para Daniel
1. Baixar WireGuard: https://www.wireguard.com/install/
2. Criar claw-tunnel.conf com o conteúdo acima
3. Importar no WireGuard (Import Tunnel from File)
4. Ativar (Activate)
5. Testar: ping 10.0.0.1

## Compilar wireguard-tools (se precisar de novo)
```bash
cd /tmp && git clone https://git.zx2c4.com/wireguard-tools
cd wireguard-tools/src && make && make install
```

## Subir WireGuard no servidor
```bash
modprobe wireguard
wg-quick up wg0
wg show
```

## ⚠️ Security Group
Porta 51820 UDP precisa estar aberta no Alibaba Cloud Security Group
