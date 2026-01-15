
# IMPLEMENTATION GUIDE

## Pré‑requisitos
- Console do **Trend Micro Worry‑Free Services**
- Inventário de **IPs/Subnets** (LAN e VPN)
- Hosts/ALGs de e‑mail e domínios permitidos (ex.: `*.trendmicro.com`)

## Passos
1. **Importe** o `firewall-rules.json` ou **replique** a partir do `firewall-rules.csv`.
2. Crie **grupos/policies**: Servidor, Estações, VPN-Clients.
3. Ajuste **origens/destinos** por IP/Subnet e a **ordem das regras**.
4. **Aplicar** a política por escopo (OU, grupo, rótulo).
5. **Validar** conectividade (ping/TCPing, `Test-NetConnection`), logs e geração de eventos.

## Validação rápida
- SQL: `Test-NetConnection -ComputerName <SQLHost> -Port 1433`
- SMB: `Test-NetConnection -ComputerName <FileSrv> -Port 445`
- Web: `Invoke-WebRequest https://example.com` (esperado 200)

## Evidências
- Capturas do console, export de logs, hash do pacote aplicado.
