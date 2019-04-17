---
title: Administrar o Server Core
description: Saiba como administrar uma instalação Server Core do Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.date: 12/18/2018
ms.openlocfilehash: 39fbb92645d39a46613f2142d0258c78a6ba425b
ms.sourcegitcommit: 4df1bc940338219316627ad03f0525462a05a606
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/19/2018
ms.locfileid: "8977993"
---
# Administrar um servidor do Server Core

>Aplicável a: Windows Server (canal semestral) e Windows Server 2016

Como Server Core não tem uma interface do usuário, você precisará usar os cmdlets do Windows PowerShell, ferramentas de linha de comando ou ferramentas remotas para executar tarefas de administração básica. As seções a seguir descrevem os comandos usados para tarefas básicas e cmdlets do PowerShell. Você também pode usar o [Windows Admin Center](../../manage/windows-admin-center/overview.md), um portal de gerenciamento unificada atualmente em visualização pública, para administrar sua instalação. 

## Tarefas administrativas usando os cmdlets do PowerShell
Use as seguintes informações para executar tarefas administrativas básicas com cmdlets do Windows PowerShell.

### Definir um endereço IP estático
Quando você instala um servidor do Server Core, por padrão ele tem o endereço de um DHCP. Se você precisar de um endereço IP estático, você pode definir usando as etapas a seguir.

Para exibir sua configuração de rede atual, use **Get-NetIPConfiguration**.

Para exibir os endereços IP que você já estiver usando, use **Get-NetIPAddress**.

Para definir um endereço IP estático, faça o seguinte: 

1. Execute **Get-NetIPInterface**. 
2. Observe o número na coluna **IfIndex** para sua interface IP ou a cadeia de caracteres **InterfaceDescription** . Se você tiver mais de um adaptador de rede, observe o número ou cadeia de caracteres correspondente a interface que você deseja definir o endereço IP estático.
3. Execute o seguinte cmdlet para definir o endereço IP estático:

   ```powershell
   New-NetIPaddress -InterfaceIndex 12 -IPAddress 192.0.2.2 -PrefixLength 24 -DefaultGateway 192.0.2.1
   ```

   onde:
   - **InterfaceIndex** é o valor de **IfIndex** da etapa 2. (No nosso exemplo, 12)
   - **Endereço IP** é o endereço IP estático que você deseja definir. (No nosso exemplo, 191.0.2.2)
   - **PrefixLength** é o comprimento do prefixo (outra forma de máscara de sub-rede) para o endereço IP que você estiver configurando. (Para nosso exemplo, 24)
   - **DefaultGateway** é o endereço IP para o gateway padrão. (Para nosso exemplo, 192.0.2.1)
4. Execute o seguinte cmdlet para definir o cliente DNS endereço do servidor: 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4
   ```
   
   onde:
   - **InterfaceIndex** é o valor de IfIndex da etapa 2.
   - **ServerAddresses** é o endereço IP do seu servidor DNS.
5. Para adicionar vários servidores DNS, execute o seguinte cmdlet: 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4,192.0.2.5
   ```

   em que, neste exemplo, **192.0.2.4** e **192.0.2.5** são ambos os endereços IP dos servidores DNS.

Se você precisará alternar para usar o DHCP, execute **DnsClientServerAddress conjunto – InterfaceIndex 12 – ResetServerAddresses**.

### Ingressar em um domínio
Use os cmdlets a seguir para adicionar um computador a um domínio.

1. Execute a **Adicionar o computador**. Você será solicitado para ambas as credenciais para ingressar no domínio e o nome de domínio.
2. Se você precisar adicionar uma conta de usuário de domínio ao grupo Administradores local, execute o seguinte comando em um prompt de comando (não na janela do PowerShell):

   ```
   net localgroup administrators /add <DomainName>\<UserName>
   ```
3. Reinicie o computador. Você pode fazer isso executando **Restart-Computer**.

### Renomear o servidor
Use as etapas a seguir para renomear o servidor.

1. Determine o nome do servidor com o comando de **nome de host** ou **ipconfig** atual.
2. Execute **Renomear computador \<new_name\ - ComputerName >**.
3. Reinicie o computador.

### Ativar o servidor

Execute **slmgr. vbs – ipk\<productkey\ >**. Em seguida, execute **slmgr. vbs – ato**. Se a ativação for bem-sucedida, você não receberá uma mensagem.

> [!NOTE]
> Você também pode ativar o servidor por telefone, usando um [servidor de serviço de gerenciamento de chaves (KMS)](../../get-started/server-2016-activation.md), ou remotamente. Para ativar remotamente, execute o seguinte cmdlet em um computador remoto: 

>```powershell
>**cscript windows\system32\slmgr.vbs <ServerName> <UserName> <password>:-ato**
>```
 
### Configurar o Firewall do Windows

Você pode configurar o Firewall do Windows localmente no computador do Server Core usando scripts e cmdlets do Windows PowerShell. Consulte [NetSecurity](/powershell/module/netsecurity/?view=win10-ps) para os cmdlets que você pode usar para configurar o Firewall do Windows.

### Habilitar a comunicação remota do Windows PowerShell

Você pode habilitar o Windows PowerShell Remoting, no qual os comandos digitados no Windows PowerShell em um computador que execute em outro computador. Habilite o Windows PowerShell Remoting usando **Enable-PSRemoting**.

Para obter mais informações, consulte [Sobre remoto FAQ](/powershell/module/microsoft.powershell.core/about/about_remote_faq?view=powershell-5.1)
</br>

## Tarefas administrativas na linha de comando
Use as seguintes informações de referência para executar tarefas administrativas na linha de comando.

### Instalação e configuração
|Tarefa | Comando |
|-----|-------|
|Definir a senha de administrador local| **administrador de usuários NET** \* |
|Adicionar um computador a um domínio| **Netdom ingresso % computername %** **/domain:\<domain\ > /userd:\<domain\\username\ > /senhad:**\* <br> Reinicie o computador.|
|Confirme que o domínio foi alterado| **set** |
|Remover um computador de um domínio|**Remover Netdom \ < computername\ >**| 
|Adicionar um usuário ao grupo Administradores local|**net localgroup administradores / adicionar \ < domain\\username\ >** |
|Remover um usuário do grupo Administradores local|**net localgroup administradores /delete \ < domain\\username\ >** |
|Adicionar um usuário no computador local|**usuário NET \ < domain\username\ > * / Adicionar** |
|Adicionar um grupo no computador local|**net localgroup \ < grupo name\ > / Adicionar**|
|Alterar o nome de um computador ingressado no domínio|**Netdom renamecomputer % computername % /NewName:\<new computador name\ > /userd:\<domain\\username\ > /senhad:** * |
|Confirme se o novo nome de computador|**set**| 
|Alterar o nome de um computador em um grupo de trabalho|**Netdom renamecomputer \ < currentcomputername\ > /NewName: \ < newcomputername\ >** <br>Reinicie o computador.|
|Desativar o gerenciamento de arquivo de paginação|**sistema de computador WMIC onde nome = "< computername\ >" definir AutomaticManagedPagefile = False**| 
|Configurar o arquivo de paginação|**pagefileset WMIC onde nome = "< caminho/filename\ >" definir InitialSize = \ < initialsize\ > MaximumSize = \ < maxsize\ >** <br>Onde o *caminho/nome do arquivo* é o caminho e o nome do arquivo de paginação, *initialsize* é o tamanho inicial do arquivo de paginação, em bytes, e *maxsize* é o tamanho máximo do arquivo de página, em bytes.|
|Altere para um endereço IP estático|**ipconfig/todos os** <br>Registre as informações relevantes ou redirecioná-la para um arquivo de texto (**ipconfig/todos os > ipconfig. txt**).<br>**interfaces de Mostrar netsh interface ipv4**<br>Verifique se há uma lista de interface.<br>**netsh interface ipv4 definir nome de endereço \ origem < ID de interface list\ > = endereço estático = \ < preferencial IP address\ > gateway = \ < gateway address\ >**<br>Execute **ipconfig/todos os** para vierfy DHCP ativado está definido como **não**.|
|Defina um endereço estático do DNS.|**netsh interface ipv4 adicionar ServidorDNS nome = \<name ou ID do card\ da interface de rede > endereço = \<IP endereço do principal servidor de DNS \ > índice = 1 <br> **netsh interface ipv4 adicionar ServidorDNS nome = \<name de servidor de DNS secundário \ > endereço = \<IP endereço do servidor de \ DNS secundário > índice = 2 * * <br> Repita conforme apropriado para adicionar servidores adicionais.<br>Execute **ipconfig/todos os** para verificar se os endereços estão corretos.|
|Altere a um endereço IP fornecido pelo DHCP de um endereço IP estático|**netsh interface ipv4 definir nome de endereço = \ origem < endereço IP local system\ > = DHCP** <br>Execute **Ipconfig/todos os** para verificar se o DHCP ativado está definido como **Sim**.|
|Insira uma chave do produto|**slmgr. vbs – ipk \ < \<chave produto >**| 
|Ativar o servidor localmente|**slmgr. vbs - ato**| 
|Ativar o servidor remotamente|**cscript slmgr. vbs – ipk \ < produto \<chave > \ < servidor name\ > \ < username\ > \ < password\ >** <br>**cscript slmgr. vbs - ato \ < servername\ > \ < username\ > \ < password\ >** <br>Obter o GUID do computador executando **cscript slmgr. vbs-fez** <br> Execute **cscript slmgr. vbs - dli \<GUID\ >** <br>Verifique se que o status da licença é definida como **licenciado (ativado)**.


### Rede e o firewall

|Tarefa|Comando| 
|----|-------|
|Configurar seu servidor para usar um servidor proxy|**Netsh Winhttp Definir proxy \ < servername\ >: \ < número de porta \ >** <br>**Observação:** Instalações do Server Core não podem acessar a Internet por meio de um proxy que exija uma senha para permitir conexões.|
|Configurar seu servidor para ignorar o proxy para endereços da Internet|**Netsh winttp Definir proxy \ < servername\ >: \ < número de porta \ > bypass-list = "< local\ >"**| 
|Exibir ou modificar a configuração do IPSEC|**netsh ipsec**| 
|Exibir ou modificar a configuração de NAP|**netsh nap**| 
|Exibir ou modificar IP para conversão de endereço físico|**arp**| 
|Exibir ou definir a tabela de roteamento local|**rota**| 
|Exibir ou definir as configurações do servidor DNS|**nslookup**| 
|Exibir estatísticas de protocolo e conexões de rede TCP/IP atuais|**netstat**| 
|Exibir estatísticas de protocolo e conexões TCP/IP atuais usando NetBIOS sobre TCP/IP (NBT)|**nbtstat**| 
|Exibir saltos para conexões de rede|**pathping**| 
|Saltos de rastreamento para conexões de rede|**tracert**| 
|Exibir a configuração do roteador multicast|**mrinfo**| 
|Habilitar a administração remota do firewall|**netsh advfirewall firewall Definir grupo de regra = "Gerenciamento remoto do Windows Firewall" new enable = yes**| 
 

### Atualizações, relatório de erros e comentários

|Tarefa|Comando| 
|----|-------|
|Instalar uma atualização|**WUSA \ < update\ >. msu /quiet**| 
|Atualizações de lista instalada|**systeminfo**| 
|Remover uma atualização|**Expanda /f:\* \ < update\ >. msu c:\test** <br>Navegue até c:\test\ e abra \ < update\ >. XML em um editor de texto.<br>Substitua **instalar** **Remover** e salve o arquivo.<br>**. XML do pkgmgr /n:\ < update\ >**|
|Configurar atualizações automáticas|Para verificar a configuração atual: * * cscript %systemroot%\system32\scregedit.wsf /AU /v * *<br>Para ativar atualizações automáticas: **scregedit.wsf cscript /AU 4** <br>Para desativar as atualizações automáticas: **%systemroot%\system32\scregedit.wsf cscript /AU 1**| 
|Habilitar o relatório de erros|Para verificar a configuração atual: **/query serverWerOptin** <br>Para enviar automaticamente relatórios detalhados: **serverWerOptin / detalhadas** <br>Para enviar automaticamente relatórios de resumo: **serverWerOptin /summary** <br>Para desabilitar o relatório de erros: **/disable serverWerOptin**|
|Participar o programa de Aperfeiçoamento da experiência do usuário (CEIP)|Para verificar a configuração atual: **/query serverCEIPOptin** <br>Para habilitar CEIP: **/enable serverCEIPOptin** <br>Desabilitar CEIP: **/disable serverCEIPOptin**|

### Serviços, processos e desempenho

|Tarefa|Comando| 
|----|-------|
|Lista os serviços em execução|**consulta SC** ou **net start**| 
|Iniciar um serviço|**sc iniciar \<service name\ >** ou **net start-\<service name\ >**| 
|Parar um serviço|**sc stop \<service name\ >** ou **net stop \<service name\ >**| 
|Recuperar uma lista de execução de aplicativos e processos associados|**tasklist**||Interromper um processo forçadamente|Executar **tasklist** recuperar o processo de identificação (PID) e, em seguida, executar **taskkill /PID \<process ID\ >**|
|Inicie o Gerenciador de tarefas|**Taskmgr**| 
|Criar e gerenciar os logs de desempenho e a sessão de rastreamento de eventos|Para criar um contador, o rastreamento, a coleta de dados de configuração ou a API: **Crie logman** <br>Para propriedades de coletor de dados de consulta: **consulta logman** <br>Para iniciar ou parar a coleta de dados: **start\ logman | parar** <br>Para excluir um coletor: **Excluir logman** <br> Para atualizar as propriedades de um coletor: **logman atualizar** <br>Para importar um conjunto de coletor de dados de um arquivo XML ou exportá-la para um arquivo XML: **import\ logman | exportar**|

### Logs de eventos

|Tarefa|Comando| 
|----|-------|
|Logs de eventos de lista|**el wevtutil**| 
|Consulta de eventos em um log especificado|**wevtutil qe /f:text \ < log name\ >**| 
|Exportar um log de eventos|**wevtutil. exe epl \ < log name\ >**| 
|Limpar um log de eventos|**wevtutil cl \ < log name\ >**| 


### Sistema de arquivos e de disco

|Tarefa|Comando|
|----|-------|
|Gerenciar partições de disco|Para obter uma lista completa de comandos, execute **diskpart /?**|  
|Gerenciar software RAID|Para obter uma lista completa de comandos, execute **diskraid /?**|  
|Gerenciar pontos de montagem de volume|Para obter uma lista completa de comandos, execute **mountvol /?**| 
|Desfragmentar um volume|Para obter uma lista completa de comandos, execute **defrag /?**|  
|Converter um volume para o sistema de arquivos NTFS|**Converter \ < volume letter\ > /FS**| 
|Compactar um arquivo|Para obter uma lista completa de comandos, execute **compact /?**|  
|Administrar arquivos abertos|Para obter uma lista completa de comandos, execute **openfiles /?**|  
|Administrar pastas VSS|Para obter uma lista completa de comandos, execute **vssadmin /?**| 
|Administrar o sistema de arquivos|Para obter uma lista completa de comandos, execute **fsutil /?**||Verificar uma assinatura do arquivo|**Sigverif /?**| 
|Apropriar-se de um arquivo ou pasta|Para obter uma lista completa de comandos, execute **icacls /?**| 
 
### Hardware

|Tarefa|Comando| 
|----|-------|
|Adicionar um driver para um novo dispositivo de hardware|Copie o driver para uma pasta na %homedrive%\\\ < driver folder\ >. Execute **pnputil -i - um folder\ %homedrive%\\\<driver > \\\<driver\ >. inf**|
|Remover um driver para um dispositivo de hardware|Para obter uma lista de drivers carregados, execute **tipo de consulta sc = driver**. Em seguida, execute **sc excluir \<service_name\ >**|
