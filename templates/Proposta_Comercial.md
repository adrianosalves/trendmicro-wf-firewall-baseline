
# Proposta Comercial — Implantação de Baseline de Firewall (Trend Micro Worry‑Free) + OpenVPN

**Cliente:** <Nome da Empresa>  
**Contato:** <Nome / Cargo / E-mail / Telefone>  
**Data:** <AAAA-MM-DD>  
**Versão:** 1.0

## 1. Contexto & Objetivo
Descrever brevemente o cenário atual e objetivo de reduzir superfície de ataque, padronizar regras e habilitar observabilidade para SOC.

## 2. Escopo
- Criação/ajuste de políticas de firewall no Trend Micro Worry‑Free
- Regras: SQL (1433), SMB (445), Web (80/443), E‑mail (25/587/993/995), OpenVPN (1194)
- Bloqueio de NetBIOS (137–139)
- Exceções para `*.trendmicro.com`
- Testes de validação e entrega de documentação

## 3. Entregáveis
- Políticas aplicadas (CSV/JSON)
- Guia de Implantação preenchido
- Relatório de Implantação com evidências
- Playbook SOC ajustado ao cliente
