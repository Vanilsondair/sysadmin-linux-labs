# sysadmin-linux-labs
Laboratórios práticos de SysAdmin Linux — Networking, Firewall, VPN e Monitoring

# 🖥️ SysAdmin Linux Labs

Laboratórios práticos de administração de sistemas Linux.  
Foco em ambientes reais com Ubuntu Server no VirtualBox.

---

## 📡 Semana 1 — Networking no Linux

### O que aprendi
- Configuração de interfaces de rede com `ip addr`, `ip link`, `ip route`
- Configuração persistente de rede via **Netplan** (`/etc/netplan/`)
- Diagnóstico de conectividade com `ping`, `curl`, `traceroute`
- Resolução DNS com `resolvectl`
- Scan de rede com `nmap`
- Análise de portas abertas com `ss -tulnp`
- Captura e análise de tráfego com `tcpdump`

---

### 🔧 Comandos principais

#### Interfaces e rotas
```bash
ip addr show              # ver interfaces e IPs
ip route show             # ver tabela de rotas
ip link set enp0s3 up     # activar interface
```

#### Netplan — IP estático
```bash
# Editar /etc/netplan/50-cloud-init.yaml
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: false
      addresses:
        - 192.168.1.100/24
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]

sudo netplan apply        # aplicar configuração
```

#### Diagnóstico
```bash
ping -c 4 8.8.8.8                  # testar conectividade
traceroute 8.8.8.8                 # rastrear rota
resolvectl query google.com        # testar DNS
sudo ss -tulnp                     # ver portas em escuta
```

#### nmap
```bash
sudo apt install nmap -y
nmap -sn 192.168.1.0/24            # descobrir hosts na rede
nmap -p 22,80,443 192.168.1.10     # scan de portas específicas
```

#### tcpdump
```bash
sudo tcpdump -i enp0s3             # capturar todo o tráfego
sudo tcpdump -i enp0s3 icmp        # só pings
sudo tcpdump -i enp0s3 port 53     # só DNS
sudo tcpdump -i enp0s3 -w cap.pcap -c 50   # gravar 50 pacotes
sudo tcpdump -r cap.pcap icmp      # ler ficheiro filtrado
```

---

### 💡 Lição do laboratório
> Diagnosticar um problema de rede não é adivinhar —  
> é observar, filtrar e interpretar dados reais.

---

## 🔜 Próximas semanas
- [ ] Semana 2 — Firewall com UFW e iptables
- [ ] Semana 3 — VPN com WireGuard  
- [ ] Semana 4 — Monitoring com Netdata

---

---

## 🛡️ Semana 2 — Firewall com UFW, iptables e Fail2ban

### O que aprendi
- Configuração de firewall com **UFW** — bloquear tudo e permitir só o necessário
- Regras avançadas com **iptables** — bloquear IPs, limitar conexões por minuto
- Protecção contra brute force com **Fail2ban**
- Teste de regras de firewall com **nmap**
- Análise de logs de autenticação com **fail2ban-regex**

---

### 🔧 Comandos principais

#### UFW — Firewall simplificado
```bash

---

## 🔐 Semana 3 — VPN com WireGuard

### O que aprendi
- Conceito de túnel cifrado e como funciona uma VPN
- WireGuard vs OpenVPN — porquê o WireGuard é o futuro
- Geração de chaves criptográficas (pública/privada)
- Configuração de servidor e cliente WireGuard
- Abertura de porta UDP 51820 no UFW
- Verificação de tráfego com `wg show` e `tcpdump`

---

### 🔧 Comandos principais

#### Instalação e chaves
```bash
sudo apt install wireguard -y

# Gerar par de chaves
wg genkey | tee chave_privada | wg pubkey > chave_publica
cat chave_privada
cat chave_publica
```

#### Configuração do servidor (/etc/wireguard/wg0.conf)
```ini
[Interface]
PrivateKey = CHAVE_PRIVADA_DO_SERVIDOR
Address = 10.0.0.1/24
ListenPort = 51820

[Peer]
PublicKey = CHAVE_PUBLICA_DO_CLIENTE
AllowedIPs = 10.0.0.2/32
```

#### Configuração do cliente (/etc/wireguard/wg1.conf)
```ini
[Interface]
PrivateKey = CHAVE_PRIVADA_DO_CLIENTE
Address = 10.0.0.2/24

[Peer]
PublicKey = CHAVE_PUBLICA_DO_SERVIDOR
Endpoint = 10.0.0.1:51820
AllowedIPs = 10.0.0.0/24
```

#### Gerir a VPN
```bash
sudo chmod 600 /etc/wireguard/wg0.conf   # proteger ficheiro de chaves
sudo wg-quick up wg0                      # activar servidor
sudo wg-quick up wg1                      # activar cliente
sudo wg-quick down wg0                    # desactivar
sudo wg show                              # ver estado do túnel e peers
sudo systemctl enable wg-quick@wg0       # activar no boot
```

#### Abrir porta no UFW
```bash
sudo ufw allow 51820/udp
```

#### Verificar tráfego
```bash
sudo tcpdump -i wg0                       # capturar tráfego na interface VPN
ping -c 4 10.0.0.1 -I wg1               # testar túnel pelo cliente
```

---

### 💡 Lição do laboratório
> Segurança não é só firewall — é camadas.  
> A VPN é uma delas.

---

## 🔜 Próximas semanas
- [x] Semana 1 — Networking no Linux
- [x] Semana 2 — Firewall com UFW e iptables
- [x] Semana 3 — VPN com WireGuard
- [ ] Semana 4 — Monitoring com Netdata

*Ubuntu Server | VirtualBox | Moçambique 🇲🇿*
