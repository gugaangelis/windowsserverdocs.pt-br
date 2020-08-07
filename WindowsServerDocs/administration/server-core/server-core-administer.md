---
title: Administrar o Server Core
description: Saiba como administrar uma instalação Server Core do Windows Server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.date: 12/18/2018
ms.openlocfilehash: 8eba1e081c57c1c668a4e52ecb85fb4716a36fdc
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895908"
---
# <a name="administer-a-server-core-server"></a>Administrar um servidor Server Core

>Aplica-se a: Windows Server 2019, Windows Server 2016 e Windows Server (canal semestral)

Como o Server Core não tem uma interface do usuário, você precisa usar cmdlets do Windows PowerShell, ferramentas de linha de comando ou ferramentas remotas para executar tarefas básicas de administração. As seções a seguir descrevem os cmdlets e os comandos do PowerShell usados para tarefas básicas. Você também pode usar o [centro de administração do Windows](../../manage/windows-admin-center/overview.md), um portal de gerenciamento unificado atualmente em visualização pública, para administrar sua instalação.

## <a name="administrative-tasks-using-powershell-cmdlets"></a>Tarefas administrativas usando cmdlets do PowerShell
Use as informações a seguir para executar tarefas administrativas básicas com cmdlets do Windows PowerShell.

### <a name="set-a-static-ip-address"></a>Definir um endereço IP estático
Quando você instala um servidor Server Core, por padrão ele tem um endereço DHCP. Se você precisar de um endereço IP estático, poderá defini-lo usando as etapas a seguir.

Para exibir sua configuração de rede atual, use **Get-NetIPConfiguration**.

Para exibir os endereços IP que você já está usando, use **Get-netipaddress**.

Para definir um endereço IP estático, faça o seguinte:

1. Execute **Get-NetIPInterface**.
2. Observe o número na coluna **IfIndex** para sua interface IP ou a cadeia de caracteres **InterfaceDescription** . Se você tiver mais de um adaptador de rede, observe o número ou a cadeia de caracteres correspondente à interface para a qual você deseja definir o endereço IP estático.
3. Execute o seguinte cmdlet para definir o endereço IP estático:

   ```powershell
   New-NetIPaddress -InterfaceIndex 12 -IPAddress 192.0.2.2 -PrefixLength 24 -DefaultGateway 192.0.2.1
   ```

   onde:
   - **InterfaceIndex** é o valor de **IfIndex** da etapa 2. (Em nosso exemplo, 12)
   - **IPAddress** é o endereço IP estático que você deseja definir. (Em nosso exemplo, 191.0.2.2)
   - **PrefixLength** é o comprimento do prefixo (outra forma de máscara de sub-rede) para o endereço IP que você está definindo. (Para nosso exemplo, 24)
   - **DefaultGateway** é o endereço IP para o gateway padrão. (Para nosso exemplo, 192.0.2.1)
4. Execute o seguinte cmdlet para definir o endereço do servidor do cliente DNS:

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

   em que, neste exemplo, **192.0.2.4** e **192.0.2.5** são endereços IP dos servidores DNS.

Se você precisar alternar para usando DHCP, execute **set-DnsClientServerAddress – InterfaceIndex 12 – ResetServerAddresses**.

### <a name="join-a-domain"></a>Ingressar em um domínio
Use os cmdlets a seguir para ingressar um computador em um domínio.

1. Execute **Add-Computer**. Você será solicitado a fornecer as duas credenciais para ingressar no domínio e no nome de domínio.
2. Se você precisar adicionar uma conta de usuário de domínio ao grupo de administradores locais, execute o seguinte comando em um prompt de comando (não na janela do PowerShell):

   ```
   net localgroup administrators /add <DomainName>\<UserName>
   ```
3. Reinicie o computador. Você pode fazer isso executando **Restart-Computer**.

### <a name="rename-the-server"></a>Renomear o servidor
Use as etapas a seguir para renomear o servidor.

1. Determine o nome atual do servidor com o comando **hostname** ou **ipconfig** .
2. Execute **Rename-Computer-ComputerName \<new_name\> **.
3. Reinicie o computador.

### <a name="activate-the-server"></a>Ativar o servidor

Execute **slmgr. vbs – ipk \<productkey\> **. Em seguida, execute **slmgr. vbs – ato**. Se a ativação for realizada com sucesso, você não receberá uma mensagem.

> [!NOTE]
> Você também pode ativar o servidor por telefone, usando um [servidor kms (serviço de gerenciamento de chaves)](../../get-started/server-2016-activation.md)ou remotamente. Para ativar remotamente, execute o seguinte cmdlet em um computador remoto:
>
> ```
> cscript windows\system32\slmgr.vbs <ServerName> <UserName> <password>:-ato
> ```

### <a name="configure-windows-firewall"></a>Configurar o firewall do Windows

Você pode configurar o Firewall do Windows localmente no computador Server Core usando cmdlets e scripts do Windows PowerShell. Consulte [NetSecurity](/powershell/module/netsecurity/?view=win10-ps) para os cmdlets que você pode usar para configurar o Firewall do Windows.

### <a name="enable-windows-powershell-remoting"></a>Habilitar a comunicação remota do Windows PowerShell

Você pode habilitar a Comunicação Remota do Windows PowerShell, na qual comandos digitados no Windows PowerShell em um computador funcionam em outro computador. Habilite a comunicação remota do Windows PowerShell com **Enable-PSRemoting**.

Para obter mais informações, consulte [sobre perguntas frequentes remotas](/powershell/module/microsoft.powershell.core/about/about_remote_faq?view=powershell-5.1).

## <a name="administrative-tasks-from-the-command-line"></a>Tarefas administrativas na linha de comando
Use as informações de referência a seguir para executar tarefas administrativas na linha de comando.

### <a name="configuration-and-installation"></a>Configuração e instalação

|                             Tarefa                              |                                                                                                                                                                                                                 Comando                                                                                                                                                                                                                 |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|             Definir a senha administrativa local             |                                                                                                                                                                                                      **administrador de usuário de rede**\*                                                                                                                                                                                                      |
|                  Ingressar um computador em um domínio                  |                                                                                                                                                       **Netdom join% ComputerName%** **/Domain: \<domain\> /userd: \<domain\\username\> /passwordd:**\* <br> Reinicie o computador.                                                                                                                                                        |
|              Confirmar que o domínio foi alterado              |                                                                                                                                                                                                                 **set**                                                                                                                                                                                                                 |
|                Remover um computador de um domínio                |                                                                                                                                                                                                   **Netdom remover\<computername\>**                                                                                                                                                                                                    |
|         Adicionar um usuário ao grupo local de administradores          |                                                                                                                                                                                       **Administradores do net localgroup/Add\<domain\\username\>**                                                                                                                                                                                       |
|       Remover um usuário do grupo Administradores local       |                                                                                                                                                                                     **Administradores do net localgroup/Delete\<domain\\username\>**                                                                                                                                                                                      |
|               Adicionar um usuário ao computador local                |                                                                                                                                                                                                **net user \<domain\username\> \* /Add**                                                                                                                                                                                                 |
|               Adicionar um grupo ao computador local               |                                                                                                                                                                                                 **net localgroup \<group name\> /Add**                                                                                                                                                                                                  |
|          Alterar o nome de um computador associado a um domínio          |                                                                                                                                                           **netdom RENAMECOMPUTER% ComputerName%/NewName: \<new computer name\> /userd: \<domain\\username\> /passwordd:**\*                                                                                                                                                            |
|                 Confirmar o novo nome do computador                 |                                                                                                                                                                                                                 **set**                                                                                                                                                                                                                 |
|         Alterar o nome de um computador em um grupo de trabalho         |                                                                                                                                                                **Netdom RENAMECOMPUTER \<currentcomputername\> /NewName:\<newcomputername\>** <br>Reinicie o computador.                                                                                                                                                                 |
|                Desabilitar o gerenciamento de arquivos de paginação                 |                                                                                                                                                                        **WMIC ComputerSystem, em que Name = " \<computername\> " Set AutomaticManagedPagefile = false**                                                                                                                                                                         |
|                   Configurar o arquivo de paginação                   |                                                            **WMIC filepageset em que Name = " \<path/filename\> " Set initialSize = \<initialsize\> , MaximumSize =\<maxsize\>** <br>Em que *Path/filename* é o caminho para e o nome do arquivo de paginação, *initialSize* é o tamanho inicial do arquivo de paginação, em bytes, e *MaxSize* é o tamanho máximo do arquivo da página, em bytes.                                                             |
|                 Alterar para um endereço IP estático                 | **ipconfig/all** <br>Registre as informações relevantes ou Redirecione-as para um arquivo de texto (**ipconfig/all >ipconfig.txt**).<br>**netsh interface IPv4 mostrar interfaces**<br>Verifique se há uma lista de interfaces.<br>**netsh interface ipv4 set endereço \<Name ID from interface list\> origem = estático endereço = \<preferred IP address\> Gateway =\<gateway address\>**<br>Execute **ipconfig/all** para verificar se o DHCP habilitado está definido como **não**. |
|                   Defina um endereço DNS estático.                   |   <strong>netsh interface ipv4 add dnsserver name = \<name or ID of the network interface card\> endereço = \<IP address of the primary DNS server\> índice = 1 <br></strong>netsh interface ipv4 add dnsserver name = \<name of secondary DNS server\> endereço = \<IP address of the secondary DNS server\> índice = 2\*\* <br> Repita conforme apropriado para adicionar mais servidores.<br>Execute **ipconfig/all** para verificar se os endereços estão corretos.   |
| Alterar para um endereço IP fornecido pelo DHCP de um endereço IP estático |                                                                                                                                      **netsh interface ipv4 set endereço name = \<IP address of local system\> Source = DHCP** <br>Execute **ipconfig/all** para verificar se o DHCP habilitado está definido como **Sim**.                                                                                                                                      |
|                      Inserir uma chave do produto                      |                                                                                                                                                                                                   **slmgr. vbs – ipk\<product key\>**                                                                                                                                                                                                    |
|                  Ativar o servidor localmente                  |                                                                                                                                                                                                           **slmgr. vbs-ato**                                                                                                                                                                                                            |
|                 Ativar o servidor remotamente                  |                                            **cscript slmgr. vbs – ipk\<product key\>\<server name\>\<username\>\<password\>** <br>**cscript slmgr. vbs-ato \<servername\> \<username\>\<password\>** <br>Obtenha o GUID do computador executando **cscript slmgr. vbs-did** <br> Executar **cscript slmgr. vbs-DLI \<GUID\> ** <br>Verifique se o status da licença está definido como **licenciado (ativado)**.                                             |

### <a name="networking-and-firewall"></a>Rede e firewall

|Tarefa|Comando|
|----|-------|
|Configurar o servidor para usar um servidor proxy|**netsh WinHTTP Set proxy \<servername\> :\<port number\>** <br>**Observação:** As instalações do Server Core não podem acessar a Internet por meio de um proxy que requer uma senha para permitir conexões.|
|Configurar o servidor para ignorar o proxy para endereços da Internet|**netsh WinHTTP Set proxy \<servername\> : \<port number\> bypass-List = " \<local\> "**|
|Exibir ou modificar a configuração de IPSEC|**netsh ipsec**|
|Exibir ou modificar a configuração de NAP|**netsh nap**|
|Exibir ou modificar o IP para conversão de endereço físico|**arp**|
|Exibir ou configurar a tabela de roteamento local|**route**|
|Exibir ou definir as configurações do servidor DNS|**nslookup**|
|Exibir estatísticas de protocolo e conexões de rede TCP/IP atuais|**netstat**|
|Exibir estatísticas de protocolo e conexões TCP/IP atuais usando NetBIOS sobre TCP/IP (NBT)|**nbtstat**|
|Exibir saltos para conexões de rede|**pathping**|
|Saltos de rastreamento para conexões de rede|**tracert**|
|Exibir a configuração do roteador multicast|**mrinfo**|
|Habilitar a administração remota do firewall|**netsh advfirewall firewall set rule Group = "gerenciamento remoto do Windows Defender firewall" novo habilitar = Sim**|


### <a name="updates-error-reporting-and-feedback"></a>Atualizações, relatório de erros e comentários

|                               Tarefa                                |                                                                                                                               Comando                                                                                                                                |
|-------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                         Instalar uma atualização                         |                                                                                                                    **wusa \<update\> . msu/Quiet**                                                                                                                    |
|                      Listar atualizações instaladas                       |                                                                                                                            **systeminfo**                                                                                                                            |
|                         Remover uma atualização                          |                                 **Expanda/f: \* \<update\> . msu c:\test** <br>Navegar até c:\test e abrir \<update\>.xml em um editor de texto.<br>Substitua **instalar** por **remover** e salve o arquivo.<br>**pkgmgr/n: \<update\> . xml**                                 |
|                    Configurar atualizações automáticas                    |          Para verificar a configuração atual: **cscript%SystemRoot%\system32\scregedit.wsf/au/v \* \* <br> para habilitar as atualizações automáticas: \* \* cscript scregedit. wsf/au 4** <br>Para desabilitar as atualizações automáticas: **cscript%SystemRoot%\SYSTEM32\SCREGEDIT.wsf/au 1**          |
|                      Habilitar relatório de erros                       | Para verificar a configuração atual: **serverWerOptin/Query** <br>Para enviar relatórios detalhados automaticamente: **serverWerOptin/detailed** <br>Para enviar relatórios de resumo automaticamente: **serverWerOptin/Summary** <br>Para desabilitar o relatório de erros: **serverWerOptin/Disable** |
| Participar do CEIP (Programa de Aperfeiçoamento da Experiência do Usuário) |                                                     Para verificar a configuração atual: **serverCEIPOptin/Query** <br>Para habilitar o CEIP: **serverCEIPOptin/Enable** <br>Para desabilitar o CEIP: **serverCEIPOptin/Disable**                                                      |

### <a name="services-processes-and-performance"></a>Serviços, processos e desempenho

|                               Tarefa                               |                                                                                                                                                                                                             Comando                                                                                                                                                                                                              |
|------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                    Listar os serviços em execução                     |                                                                                                                                                                                                  **sc query** ou **net start**                                                                                                                                                                                                   |
|                         Iniciar um serviço                          |                                                                                                                                                                                 **início \<service name\> do SC** ou **net start \<service name\> **                                                                                                                                                                                  |
|                          Parar um serviço                          |                                                                                                                                                                                  **sc stop \<service name\> ** ou **net stop \<service name\> **                                                                                                                                                                                   |
| Recuperar uma lista de aplicativos em execução e processos associados |                                                                                                                                                                                                           **tasklist**                                                                                                                                                                                                           |
|                        Iniciar o Gerenciador de Tarefas                        |                                                                                                                                                                                                           **taskmgr**                                                                                                                                                                                                            |
|    Criar e gerenciar logs de desempenho e sessão de rastreamento de eventos    | Para criar um contador, rastreamento, coleta de dados de configuração ou API: **logman criar** <br>Para consultar as propriedades do coletor de dados: **consulta logman** <br>Para iniciar ou parar a coleta de dados: **logman start \| Stop** <br>Para excluir um coletor: **logman Delete** <br> Para atualizar as propriedades de um coletor: **atualização de logman** <br>Para importar um conjunto de coletores de dados de um arquivo XML ou exportá-lo para um arquivo XML: **logman Import \| Export** |

### <a name="event-logs"></a>Logs de eventos

|Tarefa|Comando|
|----|-------|
|Listar logs de eventos|**wevtutil El**|
|Eventos de consulta em um log especificado|**wevtutil qe/f: texto\<log name\>**|
|Exportar um log de eventos|**EPL de wevtutil\<log name\>**|
|Limpar um log de eventos|**CL de wevtutil\<log name\>**|


### <a name="disk-and-file-system"></a>Disco e sistema de arquivos

|                   Tarefa                   |                        Comando                        |
|------------------------------------------|-------------------------------------------------------|
|          Gerenciar partições de disco          | Para obter uma lista completa de comandos, execute o **DiskPart/?**  |
|           Gerenciar RAID de software           | Para obter uma lista completa de comandos, execute o **DiskRAID/?**  |
|        Gerenciar pontos de montagem de volume        | Para obter uma lista completa de comandos, execute **Mountvol/?**  |
|           Desfragmentar um volume            |  Para obter uma lista completa de comandos, execute **Defrag/?**   |
| Converter um volume no sistema de arquivos NTFS |        **Converter \<volume letter\> /FS: NTFS**         |
|              Compactar um arquivo              |  Para obter uma lista completa de comandos, execute **Compact/?**  |
|          Administrar arquivos abertos           | Para obter uma lista completa de comandos, execute **Openfiles/?** |
|          Administrar pastas do VSS          | Para obter uma lista completa de comandos, execute **vssadmin/?**  |
|        Administrar o sistema de arquivos        |  Para obter uma lista completa de comandos, execute **fsutil/?**   |
|    Apropriar-se de um arquivo ou pasta    |  Para obter uma lista completa de comandos, execute **icacls/?**   |

### <a name="hardware"></a>Hardware

|Tarefa|Comando|
|----|-------|
|Adicionar um driver para um novo dispositivo de hardware|Copie o driver para uma pasta em% HomeDrive% \\ \<driver folder\> . Execute **PnPUtil-i-a% HomeDrive% \\ \<driver folder\> \\ \<driver\> . inf**|
|Remover um driver para um dispositivo de hardware|Para obter uma lista de Drivers carregados, execute **sc query Type = driver**. Em seguida, execute **SC Delete \<service_name\> **|
