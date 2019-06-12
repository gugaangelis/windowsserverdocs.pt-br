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
ms.openlocfilehash: 50fa737db5862132c1dde5cb6eb6b83674b3f02e
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811389"
---
# <a name="administer-a-server-core-server"></a>Administrar um servidor server Core

>Aplica-se a: Windows Server (canal semestral) e Windows Server 2016

Como o Server Core não tem uma interface do usuário, você precisará usar ferramentas remotas, ferramentas de linha de comando ou cmdlets do Windows PowerShell para executar tarefas administrativas básicas. As seções a seguir descrevem os cmdlets do PowerShell e comandos usados para tarefas básicas. Você também pode usar [Windows Admin Center](../../manage/windows-admin-center/overview.md), um portal de gerenciamento unificado atualmente em visualização pública, para administrar sua instalação. 

## <a name="administrative-tasks-using-powershell-cmdlets"></a>Tarefas administrativas usando cmdlets do PowerShell
Use as informações a seguir para executar tarefas administrativas básicas com os cmdlets do Windows PowerShell.

### <a name="set-a-static-ip-address"></a>Definir um endereço IP estático
Quando você instala um servidor server Core, por padrão ele tem o endereço de um DHCP. Se você precisar de um endereço IP estático, você pode defini-lo usando as etapas a seguir.

Para exibir a configuração de rede atual, use **Get-NetIPConfiguration**.

Para exibir os endereços IP que você já está usando, use **Get-NetIPAddress**.

Para definir um endereço IP estático, faça o seguinte: 

1. Execute **Get-NetIPInterface**. 
2. Observe o número de **IfIndex** coluna para a interface IP ou o **InterfaceDescription** cadeia de caracteres. Se você tiver mais de um adaptador de rede, observe o número ou cadeia de caracteres correspondente à interface que você deseja definir o endereço IP estático.
3. Execute o seguinte cmdlet para definir o endereço IP estático:

   ```powershell
   New-NetIPaddress -InterfaceIndex 12 -IPAddress 192.0.2.2 -PrefixLength 24 -DefaultGateway 192.0.2.1
   ```

   onde:
   - **InterfaceIndex** é o valor da **IfIndex** da etapa 2. (Em nosso exemplo, 12)
   - **IPAddress** é o endereço IP estático que você deseja definir. (Em nosso exemplo, 191.0.2.2)
   - **PrefixLength** é o comprimento do prefixo (outra forma de máscara de sub-rede) para o endereço IP que você está definindo. (Para nosso exemplo, 24)
   - **DefaultGateway** é o endereço IP para o gateway padrão. (Para nosso exemplo, 192.0.2.1)
4. Execute o seguinte cmdlet para definir o cliente DNS endereço do servidor: 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4
   ```
   
   onde:
   - **InterfaceIndex** é o valor do IfIndex da etapa 2.
   - **ServerAddresses** é o endereço IP do seu servidor DNS.
5. Para adicionar vários servidores DNS, execute o seguinte cmdlet: 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4,192.0.2.5
   ```

   onde, neste exemplo, **192.0.2.4** e **e 192.0.2.5** são os dois endereços IP dos servidores DNS.

Se você precisa passar a usar DHCP, execute **Set-DnsClientServerAddress – InterfaceIndex 12 – ResetServerAddresses**.

### <a name="join-a-domain"></a>Ingressar em um domínio
Use os seguintes cmdlets para ingressar um computador a um domínio.

1. Execute **Add-Computer**. Você será solicitado para ambas as credenciais para ingressar no domínio e o nome de domínio.
2. Se você precisar adicionar uma conta de usuário de domínio ao grupo Administradores local, execute o seguinte comando no prompt de comando (não na janela do PowerShell):

   ```
   net localgroup administrators /add <DomainName>\<UserName>
   ```
3. Reinicie o computador. Você pode fazer isso executando **Restart-Computer**.

### <a name="rename-the-server"></a>Renomear o servidor
Use as etapas a seguir para renomear o servidor.

1. Determine o nome atual do servidor com o comando **hostname** ou **ipconfig** .
2. Execute **Rename-Computer - ComputerName \<new_name\>** .
3. Reinicie o computador.

### <a name="activate-the-server"></a>Ativar o servidor

Execute **slmgr. vbs – ipk\<productkey\>** . Em seguida, execute **slmgr. vbs – ato**. Se a ativação for bem-sucedida, você não receberá uma mensagem.

> [!NOTE]
> Você também pode ativar o servidor por telefone, usando um [servidor do serviço de gerenciamento de chaves (KMS)](../../get-started/server-2016-activation.md), ou remotamente. Para ativar remotamente, execute o seguinte cmdlet em um computador remoto: 
> 
> ```powershell
> **cscript windows\system32\slmgr.vbs <ServerName> <UserName> <password>:-ato**
> ```
 
### <a name="configure-windows-firewall"></a>Configurar o Firewall do Windows

Você pode configurar o Firewall do Windows localmente no computador Server Core usando cmdlets e scripts do Windows PowerShell. Ver [NetSecurity](/powershell/module/netsecurity/?view=win10-ps) para os cmdlets que você pode usar para configurar o Firewall do Windows.

### <a name="enable-windows-powershell-remoting"></a>Habilitar a comunicação remota do Windows PowerShell

Você pode habilitar a Comunicação Remota do Windows PowerShell, na qual comandos digitados no Windows PowerShell em um computador funcionam em outro computador. Habilitar a comunicação remota do Windows PowerShell com **Enable-PSRemoting**.

Para obter mais informações, consulte [perguntas Frequentes sobre About_remote](/powershell/module/microsoft.powershell.core/about/about_remote_faq?view=powershell-5.1).

## <a name="administrative-tasks-from-the-command-line"></a>Tarefas administrativas da linha de comando
Use as seguintes informações de referência para executar tarefas administrativas da linha de comando.

### <a name="configuration-and-installation"></a>Configuração e instalação

|                             Tarefa                              |                                                                                                                                                                                                                 Comando                                                                                                                                                                                                                 |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|             Definir a senha administrativa local             |                                                                                                                                                                                                      **administrador de usuário NET** \*                                                                                                                                                                                                      |
|                  Ingressar um computador em um domínio                  |                                                                                                                                                       **Netdom join % computername %** **/domain:\<domínio\> /userd:\<domínio\\username\> /senhad:** \* <br> Reinicie o computador.                                                                                                                                                        |
|              Confirmar que o domínio foi alterado              |                                                                                                                                                                                                                 **set**                                                                                                                                                                                                                 |
|                Remover um computador de um domínio                |                                                                                                                                                                                                   **Netdom remove \<computername\>**                                                                                                                                                                                                    |
|         Adicionar um usuário ao grupo Administradores local          |                                                                                                                                                                                       **net localgroup administradores / adicione \<domínio\\nome de usuário\>**                                                                                                                                                                                       |
|       Remover um usuário do grupo Administradores local       |                                                                                                                                                                                     **net localgroup administradores /delete \<domínio\\nome de usuário\>**                                                                                                                                                                                      |
|               Adicionar um usuário ao computador local                |                                                                                                                                                                                                **usuário NET \<domínio \ nomedeusuário\> \* / adicionar**                                                                                                                                                                                                 |
|               Adicionar um grupo ao computador local               |                                                                                                                                                                                                 **net localgroup \<nome do grupo\> / adicionar**                                                                                                                                                                                                  |
|          Alterar o nome de um computador associado a um domínio          |                                                                                                                                                           **Netdom renamecomputer % computername % /NewName:\<novo nome do computador\> /userd:\<domínio\\username\> /senhad:** \*                                                                                                                                                            |
|                 Confirmar o novo nome do computador                 |                                                                                                                                                                                                                 **set**                                                                                                                                                                                                                 |
|         Alterar o nome de um computador em um grupo de trabalho         |                                                                                                                                                                **netdom renamecomputer \<currentcomputername\> /NewName:\<newcomputername\>** <br>Reinicie o computador.                                                                                                                                                                 |
|                Desabilitar o gerenciamento de arquivos de paginação                 |                                                                                                                                                                        **WMIC computersystem em que nome = "\<computername\>" definir AutomaticManagedPagefile = False**                                                                                                                                                                         |
|                   Configurar o arquivo de paginação                   |                                                            **WMIC pagefileset em que nome = "\<caminho/nome de arquivo\>" definir InitialSize =\<initialsize\>, MaximumSize =\<maxsize\>** <br>Onde *caminho/nome de arquivo* é o caminho e o nome do arquivo de paginação *initialsize* é o tamanho inicial do arquivo de paginação, em bytes, e *maxsize* é o tamanho máximo das arquivo de paginação, em bytes.                                                             |
|                 Alterar para um endereço IP estático                 | **ipconfig /all** <br>Registre as informações relevantes ou redirecioná-la para um arquivo de texto (**ipconfig/all > ipconfig.txt**).<br>**netsh interface ipv4 show interfaces**<br>Verifique se há uma lista de interfaces.<br>**netsh interface ipv4 definir nome do endereço \<ID da lista de interfaces\> origem = endereço estático =\<preferencial de endereço IP\> gateway =\<endereço do gateway\>**<br>Execute **ipconfig/all** para verificar se o DHCP ativado está definido como **não**. |
|                   Defina um endereço DNS estático.                   |   <strong>netsh interface ipv4 Adicionar nome de dnsserver =\<nome ou ID da placa de interface de rede\> endereço =\<endereço IP do servidor DNS primário\> índice = 1 <br></strong>netsh interface ipv4 Adicionar nome de dnsserver =\<nome do servidor DNS secundário\> endereço =\<endereço IP do servidor DNS secundário\> índice = 2\*\* <br> Repita conforme apropriado para adicionar outros servidores.<br>Execute **ipconfig/all** para verificar se os endereços estão corretos.   |
| Alterar para um endereço IP fornecido pelo DHCP de um endereço IP estático |                                                                                                                                      **netsh interface ipv4 definir nome de endereço =\<endereço IP do sistema local\> origem = DHCP** <br>Execute **ipconfig/all** para verificar se o DHCP habilitado estiver definida como **Sim**.                                                                                                                                      |
|                      Inserir uma chave do produto                      |                                                                                                                                                                                                   **slmgr. vbs – ipk \<chave do produto\>**                                                                                                                                                                                                    |
|                  Ativar o servidor localmente                  |                                                                                                                                                                                                           **slmgr.vbs -ato**                                                                                                                                                                                                            |
|                 Ativar o servidor remotamente                  |                                            **cscript slmgr. vbs – ipk \<chave do produto\>\<nome do servidor\>\<username\>\<senha\>** <br>**cscript slmgr.vbs -ato \<servername\> \<username\> \<password\>** <br>Obter o GUID do computador executando **cscript slmgr. vbs-fez** <br> Execute **cscript slmgr. vbs – dli \<GUID\>** <br>Verificar se o status da licença é definido como **licenciado (ativado)** .                                             |

### <a name="networking-and-firewall"></a>Rede e firewall

|Tarefa|Comando| 
|----|-------|
|Configurar seu servidor para usar um servidor proxy|**Netsh Winhttp Definir proxy \<nome_do_servidor\>:\<número da porta\>** <br>**Observação:** Instalações Server Core não é possível acessar a Internet através de um proxy que exija uma senha para permitir conexões.|
|Configurar seu servidor para ignorar o proxy para endereços da Internet|**netsh winttp set proxy \<servername\>:\<port number\> bypass-list="\<local\>"**| 
|Exibir ou modificar a configuração do IPSEC|**netsh ipsec**| 
|Exibir ou modificar a configuração de NAP|**netsh nap**| 
|Exibir ou modificar o IP à conversão de endereço físico|**arp**| 
|Exibir ou configurar a tabela de roteamento local|**route**| 
|Exibir ou configurar as configurações do servidor DNS|**nslookup**| 
|Exibir estatísticas de protocolo e conexões de rede TCP/IP atuais|**netstat**| 
|Exibir estatísticas de protocolo e conexões de TCP/IP atuais usando NetBIOS sobre TCP/IP (NBT)|**nbtstat**| 
|Exibir saltos para conexões de rede|**pathping**| 
|Rastrear saltos para conexões de rede|**tracert**| 
|Exibir a configuração do roteador multicast|**mrinfo**| 
|Habilitar a administração remota do firewall|**grupo de regras de conjunto de netsh advfirewall firewall = "Gerenciamento remoto do Windows Firewall" enable de nova = Sim**| 
 

### <a name="updates-error-reporting-and-feedback"></a>Atualizações, relatório de erros e comentários

|                               Tarefa                                |                                                                                                                               Comando                                                                                                                                |
|-------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                         Instalar uma atualização                         |                                                                                                                    **wusa \<update\>.msu /quiet**                                                                                                                    |
|                      Listar atualizações instaladas                       |                                                                                                                            **systeminfo**                                                                                                                            |
|                         Remover uma atualização                          |                                 **expand /f:\* \<update\>.msu c:\test** <br>Navegue até c:\test\ e abra \<atualizar\>. XML em um editor de texto.<br>Substitua **instale** com **remover** e salve o arquivo.<br>**pkgmgr /n:\<update\>.xml**                                 |
|                    Configurar as atualizações automáticas                    |          Para verificar a configuração atual: **cscript %systemroot%\system32\scregedit.wsf /AU /v \* \* <br>para habilitar as atualizações automáticas: \* \*scregedit.wsf cscript /AU 4** <br>Para desabilitar as atualizações automáticas: **cscript %systemroot%\system32\scregedit.wsf /AU 1**          |
|                      Habilitar relatório de erros                       | Para verificar a configuração atual: **serverWerOptin /query** <br>Para enviar automaticamente relatórios detalhados: **serverWerOptin / detalhadas** <br>Para enviar automaticamente relatórios resumidos: **serverWerOptin /Summary.** <br>Para desabilitar o relatório de erro: **serverWerOptin /disable** |
| Participar do CEIP (Programa de Aperfeiçoamento da Experiência do Usuário) |                                                     Para verificar a configuração atual: **serverCEIPOptin /query** <br>Para habilitar o CEIP: **serverCEIPOptin /enable** <br>Para desabilitar o CEIP: **serverCEIPOptin /disable**                                                      |

### <a name="services-processes-and-performance"></a>Serviços, processos e desempenho

|                               Tarefa                               |                                                                                                                                                                                                             Comando                                                                                                                                                                                                              |
|------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                    Listar os serviços em execução                     |                                                                                                                                                                                                  **consulta sc** ou **net start**                                                                                                                                                                                                   |
|                         Iniciar um serviço                          |                                                                                                                                                                                 **início do SC \<nome do serviço\>**  ou **net start \<nome do serviço\>**                                                                                                                                                                                  |
|                          Parar um serviço                          |                                                                                                                                                                                  **Parar sc \<nome do serviço\>**  ou **net stop \<nome do serviço\>**                                                                                                                                                                                   |
| Recuperar uma lista de aplicativos em execução e processos associados |                                                                                                                                                                                                           **tasklist**                                                                                                                                                                                                           |
|                        Iniciar o Gerenciador de Tarefas                        |                                                                                                                                                                                                           **taskmgr**                                                                                                                                                                                                            |
|    Criar e gerenciar logs de desempenho e a sessão de rastreamento de eventos    | Para criar um contador, o rastreamento, a coleta de dados de configuração ou a API: **logman crie** <br>Ao consultar as propriedades do coletor de dados: **logman consulta** <br>Para iniciar ou parar a coleta de dados: **logman start\|parar** <br>Para excluir um coletor: **logman excluir** <br> Para atualizar as propriedades de um coletor: **logman update** <br>Para importar um conjunto de Coletores de dados de um arquivo XML ou exportá-lo para um arquivo XML: **importação logman\|exportar** |

### <a name="event-logs"></a>Logs de eventos

|Tarefa|Comando| 
|----|-------|
|Logs de eventos de lista|**wevtutil el**| 
|Consultar eventos em um log especificado|**wevtutil qe /f:text \<log name\>**| 
|Exportar um log de eventos|**wevtutil epl \<log name\>**| 
|Limpar um log de eventos|**wevtutil cl \<log name\>**| 


### <a name="disk-and-file-system"></a>Disco e sistema de arquivos

|                   Tarefa                   |                        Comando                        |
|------------------------------------------|-------------------------------------------------------|
|          Gerenciar partições de disco          | Para obter uma lista completa de comandos, execute **diskpart /?**  |
|           Gerenciar RAID de software           | Para obter uma lista completa de comandos, execute **diskraid /?**  |
|        Gerenciar pontos de montagem de volume        | Para obter uma lista completa de comandos, execute **mountvol /?**  |
|           Desfragmentar um volume            |  Para obter uma lista completa de comandos, execute **defrag /?**   |
| Converter um volume no sistema de arquivos NTFS |        **converter \<letra de volume\> /FS: NTFS**         |
|              Compactar um arquivo              |  Para obter uma lista completa de comandos, execute **compact /?**  |
|          Administrar arquivos abertos           | Para obter uma lista completa de comandos, execute **openfiles /?** |
|          Administrar pastas do VSS          | Para obter uma lista completa de comandos, execute **vssadmin /?**  |
|        Administrar o sistema de arquivos        |  Para obter uma lista completa de comandos, execute **fsutil /?**   |
|    Apropriar-se de um arquivo ou pasta    |  Para obter uma lista completa de comandos, execute **icacls /?**   |
 
### <a name="hardware"></a>Hardware

|Tarefa|Comando| 
|----|-------|
|Adicionar um driver para um novo dispositivo de hardware|Copie o driver para uma pasta em % homedrive %\\\<pasta do driver\>. Execute **pnputil -i - a % homedrive %\\\<pasta do driver\>\\\<driver\>. inf**|
|Remover um driver para um dispositivo de hardware|Para obter uma lista de drivers carregados, execute **tipo de consulta sc = driver**. Em seguida, execute **sc delete \<service_name\>**|
