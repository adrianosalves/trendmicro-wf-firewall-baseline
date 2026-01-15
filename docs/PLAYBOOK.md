
# SOC PLAYBOOK – Regras de Firewall (WF)

## Escopo
Eventos relacionados a **bloqueios/permitidos** nas políticas WF que afetem SQL, SMB, Web, E‑mail e VPN.

## Detecção
- Alertas do console WF (bloqueios anômalos)
- Queda de conectividade reportada por usuários
- Correlacionar com EDR/EPP (quarentenas, beaconing, C2)

## Contenção
- Isolar host via policy restritiva temporária
- Bloquear IP/Subnet suspeita ou FQDN

## Erradicação
- Remover persistência, revalidar integridade do agente
- Revisar regras/ordem e exceções aplicadas

## Recuperação
-
