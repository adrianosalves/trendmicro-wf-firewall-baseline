
# SECURITY

## Princípios
- **Least Privilege / Deny by default**
- **Segmentação** por funções (Servidor, Estações, VPN)
- **Minimização de superfície** (NetBIOS bloqueado, SMB apenas 445)
- **Observabilidade** (logs e auditoria)

## Riscos tratáveis
- Exposição indevida de compartilhamentos SMB
- Exploração de serviços legados (NetBIOS)
- Download de conteúdo malicioso (inspeção HTTPS/URL filtering quando disponível)

## Testes recomendados
- Port scan controlado (Nmap) a partir de segmentos distintos
- Testes de conectividade (SQL/SMB/HTTPS)
- Validação de logs no console Trend Micro e SIEM
