
# HARDENING CHECKLIST (Windows + Trend Micro WF)

- [x] Desabilitar NetBIOS sobre TCP/IP quando possível
- [x] SMB: exigir assinatura quando aplicável
- [x] Atualizações automáticas do agente WF habilitadas
- [x] Restrições por IP/Subnet para SQL/SMB
- [x] Inspeção HTTPS/URL filtering (se disponível)
- [x] Logging detalhado habilitado e enviado ao SIEM


# Por que a Porta 445 Não Responde? Guia Completo de Diagnóstico

A porta **445/TCP** é essencial para o protocolo **SMB (Server Message Block)**, usado para compartilhamento de arquivos, impressoras e comunicação entre sistemas Windows. Mesmo com a porta liberada no firewall, é comum ela não responder por depender de serviços e configurações específicas.

---

## 🔍 Entendendo a Porta 445
- A porta **445/TCP** é usada pelo **SMBv2/v3** em sistemas Windows modernos.
- Ela não funciona apenas com liberação de firewall — **exige que o serviço SMB esteja ativo e configurado**.

---

## 🚫 Motivos Comuns para a Porta 445 Não Responder

### 1. **Serviço SMB desativado (Serviço *Server*)**
Se o serviço **Server** estiver parado ou desabilitado, a porta 445 não abrirá.

**Verifique no computador alvo (ex: 192.168.0.3):**
1. Pressione `Win + R`
2. Digite `services.msc`
3. Procure pelo serviço **Server**
4. Ele deve estar em:
   - Status: **Em execução**
   - Tipo de inicialização: **Automático**

> Em versões Windows Home, o suporte SMB pode estar limitado.

---

### 2. **Firewall do Windows bloqueando SMB**
Mesmo usando firewall de terceiros (ex: Worry-Free), o **Firewall do Windows** pode bloquear.

**Ative as regras necessárias:**
```powershell
Set-NetFirewallRule -DisplayGroup "File and Printer Sharing" -Enabled True
```

Verifique regras:
```powershell
Get-NetFirewallRule -DisplayName "*File and Printer Sharing*" | Select DisplayName, Enabled
```

Ou via interface gráfica:
- Painel de Controle → Firewall → *Permitir um aplicativo*
- Ative **Compartilhamento de Arquivo e Impressora** para *Rede privada*

---

### 3. **Problemas com SMBv1/SMBv2**
As versões modernas usam **SMBv2/v3**, e o SMBv1 fica desativado por segurança.

Verifique o status dos protocolos:
```powershell
Get-SmbServerConfiguration | Select EnableSMB1Protocol, EnableSMB2Protocol
```

- `EnableSMB2Protocol` deve ser **True**.
- Evite ativar SMB1 (inseguro).

---

### 4. **Restrição do Worry-Free (Trend Micro)**
Mesmo liberando portas, o Worry-Free pode bloquear **svchost.exe** ou tráfego SMB por comportamento.

**Ações recomendadas:**
- Verifique logs de *Application Control* e *Behavior Monitoring*
- Procure por eventos envolvendo:
  - SMB
  - File Sharing
  - Port 445

**Teste temporário:**
```powershell
Test-NetConnection 192.168.0.3 -Port 445
```
Se funcionar com o Worry-Free desativado → o problema está nele.

---

### 5. **Rede configurada como Pública**
SMB é bloqueado automaticamente em redes públicas.

**Corrija:**
1. Configurações → Rede e Internet
2. Clique na conexão atual
3. Selecione **Rede privada**

---

## ✅ Resumo dos Testes Rápidos
| Passo | Ação |
|-------|-------|
| 1 | Confirmar se o serviço **Server** está ativo |
| 2 | Ativar regras de **File and Printer Sharing** no Firewall do Windows |
| 3 | Garantir que a rede está como **Privada** |
| 4 | Testar com o Worry-Free temporariamente desativado |
| 5 | Testar acesso direto com `net view` |

Exemplo:
```cmd
net view \192.168.0.3
```

---

## 🧪 Testes Alternativos
### Teste simples com Telnet
```cmd
telnet 192.168.0.3 445
```
- **Tela preta** → Porta aberta
- **Erro imediato** → Bloqueio/serviço inativo

> Ative o Telnet se necessário: *Recursos do Windows → Cliente Telnet*

---

## 🏁 Conclusão
Se a porta 3389 (RDP) funciona, a conectividade está ok.
A porta 445 falha principalmente por:
- Serviço **Server** desativado
- Firewall do Windows bloqueando SMB
- Restrições do Worry-Free
- Rede pública desabilitando compartilhamento

🔎 Comece pela verificação do **serviço Server** — é o culpado em 90% dos casos.


