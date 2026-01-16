
# HARDENING CHECKLIST (Windows + Trend Micro WF)

- [ ] Desabilitar NetBIOS sobre TCP/IP quando possível
- [ ] SMB: exigir assinatura quando aplicável
- [ ] Atualizações automáticas do agente WF habilitadas
- [ ] Restrições por IP/Subnet para SQL/SMB
- [ ] Inspeção HTTPS/URL filtering (se disponível)
- [ ] Logging detalhado habilitado e enviado ao SIEM


🔍 Por que a porta 445 não responde, mesmo com liberação no firewall?
✅ Primeiro: entenda o que é a porta 445

    - A porta 445/TCP é usada pelo protocolo SMB (Server Message Block) para compartilhamento de arquivos, impressoras e IPC$ em redes Windows modernas (Windows 2000+).
    - Diferente da porta 3389 (RDP), o SMB exige que o serviço esteja ativo e configurado corretamente — não basta só abrir a porta.

🚫 Causas mais comuns do erro
1. Serviço SMB desativado no destino (192.168.0.3)

Mesmo com a porta aberta, se o serviço "Server" (que fornece SMB) estiver parado ou desativado, a porta 445 não responderá.

✅ Verifique no computador de destino (192.168.0.3):

    Pressione Win + R → digite services.msc
    Localize o serviço chamado "Server"
    Ele deve estar em execução e definido como "Automático"

    ```
    ⚠️ Em edições "Home" do Windows (ex: Windows 10/11 Home), o compartilhamento SMB pode estar limitado ou desativado por padrão.
    ```

2. Firewall do Windows bloqueando a porta 445 (mesmo com Worry-Free liberado)

O Worry-Free é um firewall de terceiros, mas o Firewall do Windows também pode estar ativo e bloqueando.

✅ No computador de destino (192.168.0.3), execute:

powershell
   ```
    Set-NetFirewallRule -DisplayGroup "File and Printer Sharing" -Enabled True
    ```

Se estiver desabilitado, ative com:

powershell
    ```
    Get-NetFirewallRule -DisplayName "*File and Printer Sharing*" | Select DisplayName, Enabled
    ```

Ou via GUI:

    - Painel de Controle > Firewall do Windows > Permitir um recurso...  
    - Marque "Compartilhamento de arquivo e impressora" para Rede privada.

3. SMBv1 desativado (e cliente tentando usar SMBv1)

Versões modernas do Windows desativam o SMBv1 por segurança. Se o cliente for muito antigo ou mal configurado, pode falhar.

✅ Verifique se o SMB está habilitado (no destino):

powershell
    ```
    Get-SmbServerConfiguration | Select EnableSMB1Protocol, EnableSMB2Protocol
    ```

    - EnableSMB2Protocol deve ser True (é o padrão).
    - Evite ativar SMB1 (inseguro).

    O SMBv2/v3 usa a mesma porta 445, então isso raramente causa "falha de conexão", mas sim "acesso negado". Ainda assim, vale checar.

4. Antivírus/firewall (Worry-Free) não liberou corretamente

Mesmo que você tenha "liberado a porta 445", o Worry-Free pode ter regras adicionais que bloqueiam tráfego SMB por comportamento (não só por porta).

✅ Verifique no console do Worry-Free:

    Regras de "Application Control" ou "Behavior Monitoring" podem estar bloqueando svchost.exe (que hospeda o SMB).
    Procure por alertas relacionados a "SMB", "File Sharing" ou "Port 445" nos logs.

    Dica: tente desativar temporariamente o Worry-Free no destino (192.168.0.3) e repita o teste:

    powershell
    ```
    Test-NetConnection 192.168.0.3 -Port 445
    ```
    
    Se funcionar, o problema é no Worry-Free, não na configuração do Windows.

5. Rede configurada como "Pública" (não "Privada")

O Windows bloqueia compartilhamento de arquivos em redes públicas por padrão.

✅ No destino (192.168.0.3):

    Vá em Configurações > Rede e Internet > Ethernet/Wi-Fi
    Clique na rede atual
    Defina como "Rede privada"

✅ Resumo das ações para testar AGORA
Passo / Ação

1. No 192.168.0.3, verifique se o serviço "Server" está em execução
2. Ative as regras de firewall do Windows para "File and Printer Sharing"
3. Certifique-se de que a rede está definida como "Privada"
4. Teste com o Worry-Free desativado temporariamente
5. Tente acessar o compartilhamento diretamente:

cmd
    ```
    net view \\192.168.0.3
    ```

🧪 Comando de teste alternativo

Além do Test-NetConnection, use:

cmd
    ```
    telnet 192.168.0.3 445
    ```

(Se o telnet não estiver instalado, ative em "Recursos do Windows")

Se a tela ficar preta → conexão bem-sucedida.
Se der erro imediato → porta fechada/bloqueada.

Conclusão

O fato de a porta 3389 funcionar prova que há conectividade de rede básica.
A falha na porta 445 quase sempre está relacionada a:

➡️ Serviço SMB desativado
➡️ Firewall do Windows bloqueando
➡️ Worry-Free com regras adicionais de comportamento
➡️ Rede definida como "Pública"

Comece verificando o serviço "Server" e o firewall do Windows — são os culpados em 90% dos casos.
