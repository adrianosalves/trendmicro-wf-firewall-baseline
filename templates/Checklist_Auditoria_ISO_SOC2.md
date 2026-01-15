
# Checklist de Auditoria — Baseline de Firewall (ISO/IEC 27001/27002 & SOC 2)

## 1. Governança & Escopo
- [ ] Escopo claramente definido (LAN, VPN, borda, endpoints)
- [ ] Proprietários das políticas identificados
- [ ] Processo de aprovação de mudanças documentado

## 2. Controles Técnicos
- [ ] Padrão deny-by-default e allow‑list por IP/Subnet/FQDN
- [ ] SMB somente TCP 445; NetBIOS bloqueado
- [ ] SQL exposto apenas a subnets/grupos autorizados
