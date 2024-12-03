# L'adressage IPv4

## 03. Les réseaux

### Publique / Privé, WAN / LAN

L'Internet est un immense réseau de réseaux, où coexistent des réseaux publics et privés. Comprendre leur distinction est fondamental pour la sécurité et l'organisation des infrastructures.

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/1-2-Adressage-ipv4/Sources/03-001.svg">

#### Le WAN (Wide Area Network)

L'Internet public fonctionne exclusivement avec des adresses IP publiques, uniques à l'échelle mondiale. Ces adresses sont :

- Attribuées par les registres Internet régionaux (RIR)
- Routables sur Internet
- Généralement payantes et en nombre limité
- Directement accessibles depuis n'importe où sur Internet

#### Le LAN (Local Area Network)

Les réseaux locaux utilisent des plages d'adresses privées réservées :

1. **Classe A**
   - Plage : 10.0.0.0 à 10.255.255.255
   - Masque : /8
   - Capacité : Plus de 16 millions d'adresses

2. **Classe B**
   - Plage : 172.16.0.0 à 172.31.255.255
   - Masque : /12
   - Capacité : Environ 1 million d'adresses

3. **Classe C**
   - Plage : 192.168.0.0 à 192.168.255.255
   - Masque : /16
   - Capacité : 65 534 adresses

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/1-2-Adressage-ipv4/Sources/03-002.svg">
