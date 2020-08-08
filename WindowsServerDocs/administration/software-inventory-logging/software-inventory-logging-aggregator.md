---
title: Agregador de Log de Inventário de Software
description: Descreve como instalar e gerenciar o log de inventário de software agregador-software-inventário de log
ms.topic: article
ms.assetid: e4230a75-6bcd-47d9-ba92-a052a90a6abc
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4f8e7743e51a5316df474ad97768cf01292db668
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87991920"
---
# <a name="software-inventory-logging-aggregator"></a>Agregador de Log de Inventário de Software

> Aplica-se a: Windows Server 2012 R2

## <a name="what-is-software-inventory-logging-aggregator"></a>O que é agregador de log de inventário de software?

O SILA (Agregador do Log de Inventário de Software) recebe, faz agregações e produz relatórios básicos da quantidade e dos tipos de software corporativo da Microsoft instalados em Windows Servers em um datacenter.

Ele é um software que você instala no Windows Server, mas que não está incluído em sua instalação. Para instalar o software, primeiro baixe-o gratuitamente no Centro de Download do Windows: [Agregador do Log de Inventário de Software 1.0 para Windows Server](https://www.microsoft.com/download/details.aspx?id=49046)

A estrutura do Log de Inventário de Software destina-se a reduzir os custos operacionais do inventário de software da Microsoft implantado em vários servidores em um ambiente de TI. Essa estrutura consiste em dois componentes, esse agregador SIL e o recurso do Windows Server, introduzidos no Windows Server 2012 R2, o log de inventário de software (SIL). Este Agregador do Log de Inventário de Software 1.0 será instalado em um servidor e receberá dados de inventário de qualquer Windows Server configurado para encaminhar os dados a ele por meio do SIL. O design permite que os administradores do datacenter habilitem o SIL nas imagens mestre do Windows Server destinadas à ampla distribuição em todo o seu ambiente.  Este pacote de software é o ponto de destino e destina-se a ser instalado pelos clientes em seus locais para um log fácil dos dados de inventário com o tempo. Este software também permite a criação periódica de relatórios de inventário básico no Microsoft Excel. Os relatórios do Agregador do Log de Inventário de Software 1.0 incluem contagens de instalações do Windows Server, System Center e SQL Server.

> [!IMPORTANT]
> Nenhum dado é enviado à Microsoft com o uso deste software.

### <a name="data-sil-collects-over-time"></a>Coleta de dados pelo SIL com o tempo

Depois de implantados corretamente, os seguintes dados podem ser exibidos no Agregador do SIL:

-   O Windows Server exclusivo é instalado em seu datacenter

-   FQDN

-   GUIDs de identificação

-   Número de processadores físicos e núcleos

-   Número de processadores virtuais (no caso de VM)

-   Modelo e tipo de processadores físicos

-   Se a tecnologia Hyper-Threading estiver habilitada ou não nos processadores físicos

-   Número de série do chassi

-   Contagem de marca d'água alta e identidade de VMs com execução simultânea do Windows Server (no caso de um host que executa um hipervisor) em cada host, com o tempo

-   A contagem de marcas de alta água e o nome do host, de execução simultânea do \( agente do System Center gerenciado, apresentam \) VMs do Windows Server em cada host, ao longo do tempo

-   Nome dos agentes do System Center instalados em VMs contadas em \- marca d' água alta gerenciada

-   Contagem e localização de instalações de SQL Server ao longo do tempo \( apenas SKUs e edições que exigem uma licença\)

-   Lista de softwares instalados em Adicionar ou Remover Programas

### <a name="who-will-use-sil"></a>Quem usará o SIL?

-   **Profissionais de TI ou administradores de datacenter** que buscam um método de baixo custo para coletar dados de inventário de software valiosos, de forma automática e que abrangem um período de tempo.

-   **Cios e controladores de finanças**, que precisam relatar o uso do software corporativo da Microsoft nas implantações de ti de suas organizações.

## <a name="getting-started"></a>Introdução
**Pré-requisitos**

Agregador do SIL (Agregador do Log de Inventário de Software) em, no mínimo, um servidor para agregação e relatórios (em uma VM ou no hardware físico):

-   **Windows Server 2012 R2** (Standard ou Datacenter Edition)

-   **Função de servidor IIS** com o .NET Framework 4.5, os Serviços do WCF e a Ativação de HTTP, todos na mesma árvore de seleção no **Assistente Adicionar Funções e Recursos**.

-   **SQL Server** 2012 Sp2 Standard Edition ou SQL Server 2014 Standard Edition

-   **Microsoft Excel de 64 bits** 2013 (opcional para instalação, mas necessário para a criação de relatórios)

-   Opcional: **VMware PowerCLI 5.5.0.5836** (necessário em ambientes VMware)

>[!Note]
>Ao usar o Windows Management Framework, há um problema de compatibilidade conhecido com a versão 5,1 do WMF, somente no agregador SIL.  Não é necessário exceder a versão 4,0 do WMF em servidores com o agregador SIL instalado.

O SIL (Log de Inventário de Software) está disponível nas versões do Windows Server com as seguintes atualizações instaladas:

-   **Windows Server 2016**ou superior

-   **Windows Server 2012 R2** (Standard ou Datacenter Edition)

    -   Atualização do Windows Server 2012 R2 **KB3000850** (novembro de 2014)

    -   Atualização do Windows Server 2012 R2 **KB3060681** (junho de 2015) (pode aparecer como uma atualização opcional no Windows Update)

### <a name="security-and-account-types"></a>Segurança e tipos de conta
**Requisito de certificado**

O SIL e o Agregador do SIL dependem de certificados SSL para a comunicação autenticada. A implementação comum disso será a instalação do Agregador do SIL com um certificado (correspondência entre nome do servidor e nome do certificado) para hospedar o serviço Web que recebe os dados de inventário. Em seguida, os Windows Servers a serem inventariados com o recurso SIL usarão um certificado de cliente diferente, para enviar dados por push para o Agregador do SIL. Um cmdlet do PowerShell (Set-SilAggregator, mais detalhes abaixo) precisa ser usado para adicionar impressões digitais de certificado à lista de certificados aprovados do agregador SIL da qual o agregador aceitará os dados associados. O Agregador do SIL continua com o processamento e a inserção em seu banco de dados após a autenticação de cada carga de dados com um certificado. Veja a seção **Detalhes de cmdlets do Agregador do SIL** para obter detalhes mais específicos sobre como isso funciona.

### <a name="polling-account-setup"></a>Configuração da conta de sondagem
Ao adicionar credenciais ao Agregador do SIL para permitir operações de sondagem, você deve usar uma abordagem de conta com menos privilégios. Além disso, como prática recomendada de segurança, você não deve usar as mesmas credenciais para todos os hosts, ou muitos, em um data center ou em outra implantação de ti.

Em um host do Windows Server que você deseja configurar para sondagem pelo Agregador do SIL, e para evitar o uso de um usuário no grupo de administradores, siga estas etapas para fornecer acesso suficiente a uma conta de usuário:

#### <a name="to-setup-a-polling-account"></a>Para configurar uma conta de sondagem

1.  No host Hyper-V do Windows Server que você deseja pesquisar do Agregador do SIL, crie uma conta de usuário local usando o **Gerenciamento do Computador** no Windows (lembre-se de desmarcar a caixa que força uma alteração de senha no primeiro logon).

2.  Adicione este usuário ao grupo **Usuários do Gerenciamento Remoto**.

3.  Adicione este usuário ao grupo **Administradores do Hyper-V**.

4.  Abra **WMIMgmt. msc** com **Iniciar** -> **execução**.

5.  Clique em **Mais Ações** na seção **Ações** e selecione **Propriedades**.

6.  Clique em **Segurança**.

7.  Selecione **namespace cimv2** no modo de exibição em árvore **Namespace**.

8.  Clique em **Segurança** (botão).

9. Adicione o grupo **Usuários do Gerenciamento Remoto** no formato **nomecomputador\nome grupo**

10. Clique em **OK**.

11. De volta em Segurança na janela **root\cimv2**, selecione os **Usuários do Gerenciamento Remoto**.

12. Na seção permissões na parte inferior, verifique se a opção **habilitar remotamente** está marcada.

13. Clique em **Aplicar** e em **OK**.

14. Clique em **OK** na janela **Propriedades** .

### <a name="installing-sil-aggregator"></a>Instalando o Agregador do SIL
Há algumas coisas de que você precisa ter certeza antes de instalar o Agregador do SIL em um Windows Server:

-   **Você tem um certificado SSL válido** que deseja usar para hospedar o serviço Web desse software.

    -   O certificado deve estar no formato **.pfx**

    -   O nome do Windows Server e o nome do certificado devem corresponder.

-   O **SQL Server Standard Edition está instalado** ou está instalado em um servidor remoto que você pretende usar com este software.

    -   O Agregador do SIL funciona com o SQL Server 2012 sp2 e SQL Server 2014. Não há nada fora do comum necessário ao fazer seleções durante a instalação do SQL Server.

    -   A conta usada para instalar o Agregador do SIL deve ser uma função de sysadmin no SQL para poder criar o banco de dados durante a instalação.

    -   A conta usada para instalar o Agregador do SIL deve ser adicionada como um administrador no SQL Analysis Services antes da instalação do Agregador do SIL.

    -   Depois de instalado, o Agente do SQL Server deve ser configurado para execução automática.

-   **A função de servidor IIS é adicionada** com o .NET Framework 4.5, os Serviços do WCF e a Ativação de HTTP, todos na mesma árvore de seleção no **Assistente Adicionar Funções e Recursos**.

-   Você está **conectado ao servidor com uma conta que tem privilégios administrativos** no servidor.

-   Você está **conectado ao servidor com uma conta que tem privilégios de sysadmin no SQL Server**, caso deseje a Autenticação do Windows

    OU

    Caso deseje a Autenticação do SQL, **você tem a senha de uma conta que tem privilégios administrativos do SQL**.

#### <a name="to-install-software-inventory-logging-aggregator"></a>Para instalar o Agregador do Log de Inventário de Software

1.  Clique duas vezes em **Setup.exe** para iniciar a instalação.

2.  Clique em **Avançar** na janela de boas-vindas.

3.  Se você aceitar o EULA, marque a caixa aceitar o contrato e clique em **Avançar**.

4.  Em **Escolher Recursos**, selecione **Instalar Agregador do Log de Inventário de Software e o Módulo de Relatório** e clique em **Avançar**.

    Para obter mais informações sobre como instalar apenas o módulo de relatórios, veja `Publish-SilReport` na seção **Detalhes de cmdlets do Agregador do SIL**.

5.  Depois que todos os pré-requisitos forem verificados, clique em **Avançar**.

6.  Em **Escolher um Tipo de Conta**, selecione **usuário local** ou **gMSA**, dependendo de sua preferência.

    A escolha da opção de conta de usuário local criará um usuário local com uma senha forte gerada automaticamente. Essa conta será usada para todos os serviços do Agregador do SIL e operações de tarefa no servidor local.  É recomendado usar a gMSA (Contas de Serviço Gerenciado de Grupo) se o Agregador fizer parte de um domínio do Active Directory (Windows Server 2012 e posterior). Para obter mais informações sobre a gMSA, veja: [Visão geral das Contas de Serviço do Gerenciado de Grupo](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831782(v=ws.11))

    -   A opção de conta gMSA deve ser usada se você pretende executar o banco de dados SQL Server em um servidor separado do Agregador do SIL.

    -   Não se esqueça de reinicializar o servidor depois de adicionar a conta de computador ao grupo de segurança habilitado para gMSA no Active Directory.

7.  Em **Escolher um SQL Server**, insira o SQL Server em que a sua instância do SQL está instalada ou **localhost**, se ele estiver instalado no servidor local.

    Há suporte para apenas um Agregador do SIL por instância do SQL.

8.  Selecione o tipo de autenticação e clique em **Verificar SQL**.

9. Clique em **Avançar** e, em seguida, em **Detalhes do Servidor de Serviços de Informações da Internet**, selecione um número da porta ou mantenha o padrão.

10. Procure o local do arquivo **.pfx**, digite a senha para o arquivo .pfx e clique em **Avançar**.

11. A última tela mostrará o andamento da instalação. Depois de concluído com êxito, clique em **Concluir**.

### <a name="uninstalling-sil-aggregator"></a>Desinstalando o Agregador do SIL

#### <a name="to-uninstall-software-inventory-logging-aggregator"></a>Para desinstalar o Agregador do Log de Inventário de Software

1.  Abra o **PowerShell** como administrador e digite `Stop-SilAggregator` . Quando o prompt retornar, isso significa que o Agregador do SIL foi interrompido.

    Por design, o Agregador do SIL processará arquivos após 20 minutos ou depois que 100 arquivos forem recebidos.  Em ambientes de alta escala, nunca ocorrerá nesse cenário, mas em escala baixa, alguns arquivos podem permanecer para serem processados antes que o agregador possa ser interrompido. Use o parâmetro `–Force` se esses arquivos forem mantidos e os dados não forem necessários.

2.  Vá para **Painel de Controle**, clique em **Programas e Recursos**, em **Desinstalar Programas**, em **Agregador do Log de Inventário de Software** e em **Desinstalar**.

    O Agregador do Log de Inventário de Software abrirá uma janela para solicitar que você escolha entre excluir ou manter todos os dados no banco de dados. A seleção padrão é mantê-los (se desejar fazer uma reinstalação, você poderá anexar o banco de dados existente para escolher o ponto em que o Agregador parou).

3.  Selecione **Manter** ou **Excluir** e clique em **Avançar**.

4.  Após a conclusão da barra de progresso, clique em **Concluir**.

### <a name="start-using-sil-and-the-sil-aggregator"></a>Começar a usar o SIL e o Agregador do SIL

#### <a name="introduction-to-sil-aggregator-powershell-cmdlets"></a>Introdução aos cmdlets do PowerShell no Agregador do SIL
Os comandos a seguir podem ser executados no console do Windows PowerShell como administrador.

|Cmdlet do Windows PowerShell|Função|
|-----------------------------|------------|
|`Start-SilAggregator`|Inicia todos os serviços e tarefas do Agregador do Log de Inventário de Software. Isso é necessário para que o Agregador receba dados via HTTPS de servidores com o Log do SIL iniciado.|
|`Stop-SilAggregator`|Interrompe todos os serviços e tarefas do Agregador do Log de Inventário de Software. Se alguma tarefa ou serviço estiver no meio das operações, pode haver um atraso para a conclusão desse comando.|
|`Set-SilAggregator`|Permite que o administrador altere a configuração do Agregador do Log de Inventário de Software.|
|`Add-SilVmHost`|Usado para adicionar nomes de host específicos, ou uma matriz de nomes de host, a ser sondada em um intervalo regular \( padrão é de uma hora \) .|
|`Remove-SilVmHost`|Usado para remover nomes de host específicos, ou uma matriz de nomes de host, a serem pesquisadas em um intervalo regular.|
|`Get-SilVMHost`|Usado para recuperar a lista de hosts físicos que o Agregador do Log de Inventário de Software é configurado para pesquisar em relação aos dados contínuos de estado de execução de VM.|
|`Get-SILAggregatorData`|Usado para recuperar dados do banco de dados para o console do PowerShell.|
|`Publish-SilReport`|Usado para criar relatórios do banco de dados dos dados do Log de Inventário de Software. **Observação:** O processamento de cubos no agregador ocorre uma vez por dia. Portanto, os dados capturados no Agregador não aparecerão nos relatórios até o dia seguinte.|

#### <a name="suggested-order-to-start"></a>Ordem sugerida para iniciar
Depois de instalar o Agregador do Log de Inventário de Software no servidor, abra o PowerShell como administrador.

-   No Agregador do SIL:

    -   Executar `Start-SilAggregator`

        Isso é necessário para que o Agregador receba ativamente os dados que são encaminhados a ele via HTTPS dos servidores que você configurou (ou que configurará) a serem inventariados. Observe que mesmo se você tiver habilitado seus servidores para encaminhar para esse Agregador primeiro, não há problema algum, já que eles armazenarão em cache suas cargas de dados localmente por até 30 dias. Depois que o agregador, seu "targetUri" estiver em execução, todos os dados armazenados em cache serão encaminhados de uma vez ao agregador e todos os dados serão processados.

    -   Executar `Add-SilVMHost`

        Exemplo: `add-silvmhost –vmhostname contoso1 –hostcredential get-credential`

        -   Neste exemplo, **contoso1** é o nome da rede (ou o endereço IP) do servidor host físico que você deseja que o Agregador realize a sondagem quanto às atualizações regulares, para descobrir quais VMs estão sendo executadas nele, a fim de acompanhar esses dados com o tempo. Get-Credential solicitará que o usuário conectado insira uma conta a ser usada para pesquisar esse host neste ponto. Executar o mesmo comando, no mesmo host, permitirá que você atualize a conta usada a qualquer momento. Tenha cuidado com alterações e expirações da senha da conta com o tempo. Se as credenciais forem alteradas ou expirarem, a sondagem no host falhará.

        -   Por padrão, a sondagem começará a cada hora, iniciando uma hora depois que `Start-SilAggregator` for executado ou uma hora depois que um host for recém-adicionado à lista de sondagem.  O intervalo de sondagem pode ser alterado por meio do `Set-SilAggregator cmdlet`.

        -   Esse cmdlet detectará automaticamente de uma lista predefinida de opções (veja a seção **Detalhes de cmdlets do Agregador do SIL**) e indicará qual HostType e HyperVisorType são corretos para o host que você está adicionando. Se não for possível reconhecê-los ou se as credenciais fornecidas forem incorretas, um prompt será exibido. Se você aceitar com uma entrada **Y**, o host será adicionado e listado como **Desconhecido**, mas não será pesquisado.

    -   Execute `Set-SilAggregator –AddCertificateThumbprint` "a impressão digital do seu certificado de cliente"

        Isso é necessário para receber dados via HTTPS de Windows Servers com o Log do SIL habilitado. A impressão digital será adicionada à lista de impressões digitais da qual o Agregador do SIL aceitará dados. O Agregador do SIL foi projetado para aceitar certificados de autenticação de cliente corporativo válidos. O certificado usado precisará ser instalado no armazenamento ** \\ Localmachine\MY (computador local > pessoal**) no servidor que encaminha os dados.

-   Nos Windows Servers a serem inventariados, abra o PowerShell como administrador e execute estes comandos:

    -   Executar `Set-SilLogging –TargetUri "https://contososilaggregator" –CertificateThumbprint "your client certificate's thumbprint"`

        -   Isso informará o SIL no Windows Server para qual local se deve enviar os dados de inventário e qual certificado deve ser usado para a autenticação.

            > [!IMPORTANT]
            > Verifique se "https://" está no valor de TargetUri.

        -   O certificado de cliente corporativo com essa impressão digital deve ser instalado em **\localmachine\MY** ou use **certmgr.msc** para instalar o certificado no repositório **Computador Local -> Pessoal**.

            > [!IMPORTANT]
            > Se esses valores não estiverem corretos ou se o certificado não estiver instalado no repositório correto (ou se ele for inválido), os encaminhamentos para o destino falharão quando o Log do SIL for iniciado. Os dados serão armazenados em cache localmente por até 30 dias.

    -   Executar `Start-SilLogging`

        Isso inicia o Log do SIL. A cada hora, em intervalos aleatórios dentro da mesma hora, o SIL encaminhará seus dados de inventário para o Agregador especificado com o parâmetro `–targeturi` . O primeiro encaminhamento será um conjunto completo de dados. Cada encaminhamento subsequente será mais de uma "pulsação" com apenas a identificação de dados que nada mudou. Se houver alguma alteração ao conjunto de dados, outro conjunto completo de dados será encaminhado.

    -   Executar `Publish-SilData`

        -   Na primeira vez que o SIL for habilitado para o log, esta etapa será opcional.

        -   Esse é um único encaminhamento manual de um conjunto completo de dados.

        -   Se o Log do SIL tiver sido iniciado por algum tempo e um novo Agregador do SIL for designado com o `Set-SilLogging`, será necessário executar esse cmdlet somente uma vez para enviar um conjunto completo de dados para o novo Agregador.

Depois de seguir estas etapas para adicionar hosts físicos que executam máquinas virtuais do Windows Server E depois de habilitar o Log de Inventário de Software (ou o Log do SIL) nesses Windows Servers, é possível executar o `Publish-SilReport –OpenReport` a qualquer momento no Agregador do SIL (exige o Excel 2013). No entanto, observe que o SQL Server Analysis Services realiza processos do cubo uma vez por dia; portanto, os dados não estão disponíveis nos relatórios no mesmo dia.

## <a name="architectural-overview"></a>Visão geral da arquitetura
O SIL funciona nos modos push e pull e consiste em dois componentes que funcionam paralelamente: o recurso SIL (Log de Inventário de Software) no Windows Server e o SILA (Agregador do Log de Inventário de Software), um MSI que pode ser baixado. Os servidores a serem inventariados enviam por push os dados de inventário do software via HTTPS, com o SIL, para o Agregador do SIL (a cada hora em momentos aleatórios dentro de cada hora). Por sua vez, o Agregador sonda, ou consulta, os hosts do hipervisor físico para enviar por push os dados de inventário de hardware a cada hora. O envio por push e o pull precisam ser configurados corretamente para habilitar a funcionalidade completa do SIL. Eles podem ser configurados em qualquer ordem. No entanto, o processamento do cubo do Agregador ocorre uma vez por dia; portanto, os dados capturados no agregador, por meio do envio por push ou pull, não aparecerão nos relatórios até o dia seguinte.

![Diagrama do agregador de log de inventário de software](../media/software-inventory-logging/SILA_Architecture.png)

> [!IMPORTANT]
> Nenhum dado é enviado à Microsoft com o uso deste software.

## <a name="enable-sil-on-multiple-servers"></a>Habilitar o SIL em vários servidores
Há várias maneiras de habilitar o SIL em uma infraestrutura de servidor distribuída, como em uma nuvem privada de máquinas virtuais.  Veja a seguir um exemplo de uma maneira de como configurar as imagens do Windows Server para enviar automaticamente os dados de inventário para um Agregador do SIL quando eles são iniciados na rede pela primeira vez.

Execute os cmdlets a seguir no console do PowerShell como um administrador em cada VM ou no computador/dispositivo físico em execução, com o Windows Server instalado (veja a seção **Pré-requisitos**):

Você precisará de um certificado SSL de cliente válido no formato. pfx usar estas etapas.  A impressão digital desse certificado precisará ser adicionada a um Agregador SIL usando o cmdlet `Set-SILAggregator –AddCertificateThumbprint`. Esse certificado de cliente não precisa corresponder ao nome do Agregador SIL.

-   `$secpasswd = ConvertTo-SecureString "`**<password for the account with permissions to the network location holding your client pfx file>**`" -AsPlainText –Force`

-   `$mycreds = New-Object System.Management.Automation.PSCredential ("`**<user account with permissions to the network location holding your client  pfx file>**`", $secpasswd)`

-   `$driveLetters = ([int][char]'C')..([int][char]'Z') | % {[char]$_}`

-   `$occupiedDriveLetters = Get-Volume | % DriveLetter`

-   `$availableDriveLetters = $driveLetters | ? {$occupiedDriveLetters -notcontains $_}`

-   `$firstAvailableDriveLetter = $availableDriveLetters[0]`

-   `New-PSDrive -Name $firstAvailableDriveLetter -PSProvider filesystem -root`** < \\ server\path para compartilhar que contém o arquivo de certificado pfx>**`-credential $mycreds`

-   `Copy-Item ${firstAvailableDriveLetter}:\`**<arquivo CertificateName. pfx no diretório da nova unidade> c:\<location of your choice>**

-   `Remove-PSDrive –Name $firstAvailableDriveLetter`

-   `$mypwd = ConvertTo-SecureString -String "`**<password for the certificate pfx file>**`" -Force –AsPlainText`

-   `Import-PfxCertificate -FilePath c:\`**<local \\ CertificateName. pfx>**`cert:\localMachine\my -Password $mypwd`

-   `Set-sillogging –targeturi "https://`**<machinename of your SIL Aggregator>** `–certificatethumbprint`

> [!NOTE]
> Use a impressão digital do certificado do seu arquivo PFX do cliente e adicionado ao agregador SIL usando o cmdlet **set-SilAggregator '-AddCertificateThumbprint '** .

-   `Start-sillogging`

Sempre que não for possível estabelecer a conexão com um Agregador do SIL, os dados de inventário do SIL serão armazenados em cache localmente nos Windows Servers por até 30 dias. Depois que um envio por push bem-sucedido é realizado para o Agregador, todos os dados armazenados em cache são encaminhados.

Adicione `Publish-SilData` à lista acima em caso de envio por push dos dados do SIL para um novo Agregador do SIL após envios por push bem-sucedidos para um agregador antigo (isso enviará um complemento completo dos dados do SIL, que o novo agregador precisará para este computador).

## <a name="software-inventory-logging-aggregator-reports"></a>Relatórios do Agregador do Log de Inventário de Software
![Imagem do relatório agregador de log de inventário de software](../media/software-inventory-logging/SILA_Report.png)

### <a name="cube-processing"></a>Processamento do Cubo
No Agregador do Log de Inventário de Software, o cubo do SQL Server Analysis Services será processado uma vez por dia às 3h00min00s no horário do sistema local. Os relatórios refletirão todos os dados até aquele horário, mas nada após esse horário, no mesmo dia.

### <a name="high-water-mark"></a>Marca d'água alta
Um aspecto fundamental dos relatórios de agregador de log de inventário de software é a captura do que é comumente chamado de "marca de alta água" de execução simultânea de servidores Windows. Isso se aplica às contagens do Windows Server e do System Center nesses relatórios. Em relação ao Windows Server, cada host físico tem um ponto no tempo (independentemente do tipo de SO no host), ao longo de um mês, quando a maioria das VMs do Windows Server é executada simultaneamente. Essa é a marca d'água alta do mês. Além disso, em relação ao System Center, há um ponto no tempo no mês em que a maioria dos Windows Servers gerenciados é executada simultaneamente por host físico (um servidor gerenciado é identificado quando houver um ou mais agentes do System Center presentes). Somente a marca d'água alta mais recente para qualquer host físico será mostrada no relatório. Nenhum dado após a marca d'água alta será mostrado. e é possível pressupor que o número de VMs do Windows Server (guias WS) ou de VMs gerenciadas do Windows Server (guias SC) tenha ficado abaixo da marca d'água alta após esse ponto. Essa maneira de acompanhar e representar o uso destina-se a ajudar na capacidade de planejamento, bem como no alinhamento com modelos de licença para esses produtos.

Nas guias relacionadas ao SQL no relatório, SQL Server instalações são contadas de forma cumulativa; Não por hig-marca d' água. O total é que uma contagem contínua das instalações do SQL Server.

> [!NOTE]
> O uso do Log de Inventário de Software não substitui a obrigação de relatar com precisão o uso do software da Microsoft de acordo com os termos de licença aplicáveis.

### <a name="poll-date-time"></a>Data e hora da sondagem
Ao usar o Agregador do Log de Inventário de Software, é importante entender que a agregação para contagens de marca d'água alta é controlada por sondagem. Em outras palavras, uma marca d'água alta pode ser capturada somente por uma sondagem do host físico subjacente. Assim, as contagens de marcas de água alta são diretamente associadas a uma "data e hora de sondagem" correspondente. Embora o intervalo de sondagem seja ajustável, a fidelidade das marcas d'água alta capturadas será afetada se for usado um valor mais alto do intervalo. Quanto maior o intervalo, menos representativos de uso real serão os dados.

### <a name="reports-are-month-by-month"></a>Os relatórios são gerados a cada mês
Todos os relatórios, mesmo relatórios anuais, são representados como relatórios gerados a cada mês. O total das marcas d'água alta, bem como os dados do computador, são redefinidos no início de cada mês corrido.

Os dados de relatório afetados pela troca para um novo mês incluem:

-   Todas as marcas d'água alta de todos os hosts são redefinidas no início de um novo mês.

-   Se o Agregador receber pelo menos uma carga completa de uma VM (via HTTPS), mas parar de receber pulsações, todas as pesquisas do host subjacente dentro desse mês presumirão a associação entre o host, a VM e os dados da VM, já que a VM está em execução ou é parada em todo o mês. No início do novo mês, essa associação é apagada até que uma carga completa ou pulsação seja recebida dessa VM.

### <a name="additional-notes-on-report-behavior"></a>Observações adicionais sobre o comportamento do relatório

-   Guias de resumo devem ser listas de referência rápida do inventário. Os hosts e suas VMs são listados na mesma coluna.

-   Ignore todos os valores cinza ou Dim. Esses são artefatos da criação de relatório do cubo do SSAS.

-   Se uma VM estiver listada com "so desconhecido", significa que o agregador não recebeu uma carga de dados completa dessa VM via SIL sobre HTTPS.

-   As VMs listadas em "host desconhecido" são VMs do Windows Server que encaminham com êxito os dados de inventário por HTTPS para o agregador, mas o agregador não está ativamente ou sondando o host subjacente com êxito para essa VM. As contagens sempre serão zero para essas entradas, uma vez que o host subjacente é desconhecido. Use o cmdlet `Add-SilVMHost`, com as credenciais corretas, para adicionar o host (ou todos os hosts) ao Agregador do SIL para sondagem. Depois de pesquisados com êxito, os dados da VM e do host serão associados em relatórios que são encaminhados.

-   Todas as datas e horas são locais em relação à hora do sistema e à localidade do Agregador do SIL. Isso inclui os dados de inventário recebidos via HTTPS dos sistemas habilitados para SIL. Quando esses arquivos são processados (não mais de 20 minutos após o recebimento), os dados são inseridos no banco de dados com a hora do sistema local.

-   O "agregador SIL" será indicado em qualquer computador servidor que tenha o agregador de log de inventário de software instalado.

-   Se um host físico altera o número de processadores ou a quantidade de memória física, uma nova linha aparecerá no relatório junto com a linha antiga. As atualizações de sondagem serão interrompidas na linha antiga e continuarão na nova linha como se fosse um host recém-adicionado.

-   Nas guias **Resumo** e **Detalhes** , o total listado nas colunas para Windows Servers em execução simultânea ou Windows Servers gerenciados indica um total de todas as marcas d'água alta de todos os hosts abaixo. Isso inclui servidores Windows que não são hosts do hipervisor e não têm VMs em execução, bem como servidores que podem ter VMs em execução, mas são "desconhecidos", pois nenhum dado está sendo recebido de dentro da VM de SIL via HTTPS. Eles são somados para sua conveniência.

-   Na seção **SQL Server** da guia **Painel**, a contagem total de instalações do SQL Server é um resumo de todos os totais de edição no Painel.  Isso pode levar a uma discrepância entre o total visto na guia **Detalhes do SQL** nos casos em que várias edições do SQL estão instaladas em um único servidor.  O Painel as contaria separadamente em cada servidor, a guia **Detalhes** não.  Várias edições do SQL instaladas em um Windows Server sempre são contados como uma contagem de um, de acordo com os termos de licenciamento.

-   Na seção **Windows Server** da guia **Painel**, as linhas de **Outros Hosts de Hipervisor** e **Total de Hosts de Hipervisor** incluem hosts físicos do Windows Server que podem ou NÃO estar executando o Hyper-V.

### <a name="column-descriptions"></a>Descrições de coluna
Veja a seguir as descrições de cada coluna na guia **Detalhes do Windows Server** do relatório baseado no Excel criado pelo Agregador do SIL. Outras guias de dados são as mesmas ou um subconjunto dessas colunas. A única exceção seria a "contagem de instalação" nas guias de SQL Server (consulte a seção **marca d' água alta** ).

|Cabeçalho de coluna|Descrição|
|-----------------|---------------|
|Mês do Calendário|Os dados em relatórios são agrupados por mês, com os mais recentes em primeiro lugar. Os dados no mês não estão listados em uma ordem específica.|
|Nome de host|O nome da rede, ou o FQDN, do host físico que o Agregador do SIL está sondando com êxito.<p>Use o cmdlet Get-SilVMHost para localizar os hosts que foram adicionados, mas que não são, ou que não estão mais, sendo pesquisados com êxito. A última sondagem bem-sucedida será exibida.|
|Tipo de host|Fabricante do Sistema Operacional no host físico.|
|Tipo de hipervisor|Fabricante do hipervisor no host físico.|
|Fabricante do processador|Fabricante do processador dos processadores no host físico.|
|Modelo de processador|O modelo de processador dos processadores no host físico.|
|É habilitado para Hyper Threading?|É exibido como True ou False dependendo se o hyper threading está habilitado nos processadores do host físico.|
|Nome da VM|O nome da rede, ou o FQDN, da máquina virtual do Windows Server. Se o Agregador não recebeu dados neste computador via HTTPS, o nome amigável da VM no hipervisor é listado.|
|VMs do Windows Server em execução simultânea por host|Contagem de VMs do Windows Server em execução simultânea no host. O número mais alto no mês para que o host é a contagem de marca d'água alta listada e capturada nesse ponto no tempo.<p>Veja a seção **Marca d'água alta** desta documentação.<p>Hosts físicos com Windows Server instalado ou com Windows Server instalado e nenhuma VM conhecida do Windows Server em execução sempre terão uma contagem de um. Se, pelo menos, uma VM conhecida do Windows Server estiver em execução no host e o Windows Server estiver em execução no próprio host, o SO do host não fará parte da contagem.|
|Contagem de processadores físicos|Número de processadores físicos instalados no host físico.|
|Contagem de núcleos físicos|Número de núcleos de processador físico instalados no host físico.|
|Contagem de processadores virtuais|Número de processadores virtuais que o Windows reconhece dentro da VM. Esse valor é fornecido apenas pelos dados encaminhados via HTTPS com o SIL em um Windows Server.|
|Data e hora da sondagem|Data e hora do ponto mais recente da marca d'água alta de VMs do Windows Server em execução simultânea nesse host físico.<p>Consulte a seção **data e hora da sondagem** desta documentação.|
|Data hora da VM vista pela última vez|Data e hora do último recebimento pelo Agregador do inventário de dados via HTTPS dessa VM do Windows Server.|
|Data hora do host vista pela última vez|Data e hora do último recebimento pelo Agregador do inventário de dados via HTTPS desse host físico do Windows Server.<p>Há suporte para hosts físicos, que executam o Windows Server e o HyperV, para habilitar o SIL e encaminhar dados de inventário via HTTPS para um Agregador do SIL.|

## <a name="sil-aggregator-cmdlets-detail"></a>Detalhes de cmdlets do Agregador do SIL
Veja a seguir os detalhes de cmdlets do Agregador do SIL. Para obter a documentação completa de cmdlets, veja: [Cmdlets do PowerShell do Agregador do SIL](/previous-versions/windows/powershell-scripting/mt548455(v=wps.640))

### <a name="publish-silreport"></a>Publish-SilReport

-   Esse cmdlet, usado como está, criará um relatório de log de inventário de software e o posicionará no diretório de documentos do usuário conectado (o Excel 2013 é necessário no computador em que o cmdlet é executado).

-   Usado com o parâmetro `–OpenReport`, ele criará o relatório e vai abri-lo no Excel para exibição.

-   Ao instalar o Agregador do SIL, você observará que há uma opção para instalar somente o módulo de relatório. É possível instalar o módulo de relatório em um sistema operacional do cliente do Windows, como o Windows 8.1 ou Windows 10. Isso permite que um cliente fino, como um laptop ou tablet, se conecte a um servidor de banco de dados do Agregador do SIL para publicar relatórios do SIL diretamente.

    -   No exemplo a seguir, serão solicitadas as credenciais a serem usadas, haverá uma conexão a um servidor de banco de dados do Agregador do SIL chamado SILContoso e a criação e abertura de um relatório do SIL no computador local.

        `Publish-SilReport -DBServerName "SILContoso" -DBServerCredential Get-Credential –OpenReport`

    -   Antes de se conectar pela primeira vez, na maioria dos casos, você precisará abrir uma porta no firewall do servidor de banco de dados do Agregador do SIL para permitir conexões. Os profissionais de TI desejarão configurá-la antecipadamente para permitir o acesso aos controladores de finanças ou a outros gerentes de inventário para que eles criem seus próprios relatórios. Para obter as etapas para fazer isso, veja o link abaixo. Uma porta padrão típica para o SQL Server Analysis Services é 2383.

### <a name="add-silvmhost"></a>Add-SilVMHost
Há suporte para os seguintes tipos de host e versões de hipervisor ao usar o cmdlet `Add-SilVMHost` . Observe que não é necessário especificá-los. O cmdlet `Add-SilVMHost` detectará automaticamente uma combinação com suporte. Se não for possível detectá-la ou se as credenciais fornecidas forem incorretas, um prompt será exibido. Se o usuário aceitar com uma entrada "Y", o host será adicionado, mas não será sondado. Ele será adicionado como "desconhecido".

|Versão do hipervisor|Agregador do SIL         Valor de HostType|Valor HypervisorType do Agregador do SIL|
|----------------------|-----------------------------------------|---------------------------------------|
|Windows Server, 2012 R2|Windows|HyperV|
|VMware 5.5|VMware|Esxi|
|Xen 4.x|Ubuntu, OpenSuse ou CentOS|Xen|
|XenServer 6.2|Citrix|XenServer|
|KVM|Ubuntu, OpenSuse ou CentOS|KVM|

Outras versões dessas plataformas e tipos de hipervisor também podem funcionar.  O Agregador do SIL é fornecido com a versão de sshnet indicada abaixo.  Isso é usado para a comunicação com plataformas de virtualização baseadas em Linux.

<pre>sshnet 2014.4.6-beta1
https://sshnet.codeplex.com/releases/view/120504
Copyright (c) 2010, RENCI</pre>

### <a name="get-silaggregator"></a>Get-SilAggregator
O `Get-SilAggregator` fornece informações de configuração para o seu aplicativo do Agregador do Log de Inventário de Software. A seguinte saída de exemplo mostra:

-   Aplicativo em execução

-   O intervalo de sondagem é a cada hora (pode ser alterado em incrementos de hora)

-   Hora do início da sondagem

-   URI de destino em que outros computadores devem ser definidos para encaminhar os dados para esse agregador

-   As impressões digitais de certificado das quais esse Agregador aceita dados do SIL

-   Tipo de conta especificado na instalação

    `PS C:\Windows\system32> Get-SilAggregator`

    ``

    `State          : Running HostPollIntervalInHours : Every 1 Hour(s)`

    `PollStartTime      : 8/24/2015 5:07:33 AM`

    `TargetURI        : https://SilContoso`

    `CertificateThumbprint  : 3efc6b8ce7d5eefba5107ede9d1caca550417452, 2dc4ea8bfb64b1246a8c1ffa1b701cd1042a3412`

    `UserProfile       : Local`

### <a name="set-silaggregator"></a>Set-SilAggregator
Com o cmdlet `Set-SilAggregator`, você pode:

-   Alterar o intervalo de hora no qual a sondagem ocorrerá.

-   Alterar a data de início e a hora para que a sondagem seja iniciada no intervalo especificado.

-   Adicione ou remova as impressões digitais de certificado das quais o Agregador do SIL aceitará dados ou às quais ele será associado.

### <a name="get-aggregatordata"></a>Get-AggregatorData

-   Esse cmdlet, usado sozinho, exibe o conteúdo da guia Detalhes do Windows Server de um relatório do Excel do Agregador do SIL.

-   Usado com parâmetros, este cmdlet recuperará dados diretamente do banco de dados que se destina a ajudar nos usos personalizados da solução geral do SIL.

-   Observe que os parâmetros `–StartTime` e `–Endtime` mostrarão os dados de relatório do primeiro mês da data de início e do último dia do mês da data de término.

![Imagem do cmdlet Get-AggregatorData concluído](../media/software-inventory-logging/SILA_Get-SILAggregator.png)

### <a name="get-silvmhost"></a>Get-SilVMHost

-   Esse cmdlet gera a lista de hosts físicos que o Agregador do SIL está configurado para pesquisar, a data e hora mais recente da pesquisa bem-sucedida e o HostType (ou fabricante do SO) e o HypervisorType (fabricante de hipervisor). Veja os detalhes de Add-SilVMHost para obter mais informações sobre HostType e HypervisorType.

    Se um host não tiver nenhuma data e hora de sondagem, mas tiver um HostType e HypervisorType com suporte, isso significa que a sondagem ainda não foi iniciada ou que ainda não teve êxito.

-   Esse cmdlet também listará todos os nomes de host que foram adicionados por meio dos dados provenientes das próprias VMs, se estiverem disponíveis na VM. Eles serão exibidos na lista, mas não terão nenhum HostType ou HypervisorType. Esses dados podem ajudar na correspondência de VMs e hosts que não podem ser configurados para sondagem.

-   Use os parâmetros `–StartTime` e `–EndTime` para ajudá-lo a entender quando os hosts foram adicionados pela primeira vez ou pesquisados pela última vez.

### <a name="remove-silvmhost"></a>Remove-SilVMHost

-   Este cmdlet removerá qualquer host da lista de hosts a serem pesquisados. Se um host for removido, é possível que uma VM no host adicione novamente o host à lista, mas o host não será pesquisado com as credenciais corretas especificadas com o cmdlet `Add-SilVMHost`.

-   Se um host for removido, ele será removido da sondagem, mas não será removido dos relatórios. Já que a sondagem será interrompida, o host não estará presente nos relatórios do(s) mês(meses) seguinte(s).

-   Use os parâmetros `–StartTime` e`–EndTime` individualmente para ajudá-lo a remover grupos de hosts pesquisados com êxito até uma data ou a partir de uma data.

## <a name="avoid-these-errors-and-issues-with-sil-and-sil-aggregator-troubleshooting-guide"></a>Evitar esses erros e problemas com o SIL e o Agregador do SIL (Guia de solução de problemas)

-   Coisas para verificar em caso de falha ou erro no cmdlet `SilLogging` ou `Publish-Sildata`:

    -   Certifique-se de que **targetUri** tem **https://** na entrada.

    -   Certifique-se de que todas as atualizações necessárias para o Windows Server estão instaladas (veja os Pré-requisitos para o SIL).  Uma maneira rápida de verificar é procurar por eles usando o seguinte cmdlet:`Get-SilWindowsUpdate *3060*, *3000*`

    -   Verifique se o certificado usado para autenticar no agregador está instalado no repositório correto no servidor local a ser inventariado com o SilLogging (veja a seção Introdução).

    -   No Agregador do SIL, certifique-se que a impressão digital do certificado que está sendo usada para autenticar no agregador é adicionada à lista usando o cmdlet `Set-SilAggregator –AddCertificateThumbprint` (veja a seção Introdução).

    -   Se estiver usando certificados corporativos, verifique se o servidor com o SIL habilitado está ingressado no domínio para o qual o certificado foi criado ou se ele é, de outro modo, verificável com uma autoridade raiz. Se um certificado não for confiável no computador local que tenta encaminhar/enviar por push os dados para um Agregador, essa ação falhará com um erro.

    -   Se todos os itens acima foram verificados, é possível verificar se o certificado usado para instalar o Agregador do SIL está íntegro e corresponde ao próprio nome do servidor do Agregador do SIL (essa etapa será necessária se outros computadores estiverem encaminhando com êxito para o mesmo Agregador do SIL).

    -   Você pode verificar o seguinte local para arquivos SIL armazenados em cache no servidor que está tentando encaminhar/enviar por push, \Windows\System32 \\ LogFiles \\ Sil. Se o `SilLogging` foi iniciado e está em execução há mais de uma hora, ou se o `Publish-SilData` foi executado recentemente, e não existem arquivos neste diretório, isso significa que o log no agregador foi bem-sucedido.

-   Confirme se o usuário conectado tem o banco de dados SQL e acesso ao Analysis Services.

    -   Isso é necessário ao instalar o Agregador do SIL.

    -   Isso é necessário ao usar o PowerShell remotamente para gerenciar o agregador SIL.

-   Para publicar relatórios do Agregador do SIL de um SO da área de trabalho do cliente.

    -   Use a opção para instalar o Módulo de Relatório somente no cliente do Windows (8.1/10).

    -   Se você tiver problemas ao tentar criar um relatório usando o PowerShell remotamente, você provavelmente precisa ter uma porta de firewall aberta no Agregador do SIL ao qual você está tentando se conectar (veja Cmdlet do `Publish-SilReport` na seção Detalhes de cmdlets do Agregador do SIL).

-   Ao usar a opção gMSA:

    -   Não se esqueça de reinicializar o servidor depois de ingressar no grupo de computadores habilitado para gMSA em Active Directory.

    -   No processo de instalação, não use o domínio totalmente qualificado ao inserir Domain\User. Por exemplo, use **mydomain\gmsaaccount**. Não insira **mydomain. <i></i> com\gmsaaccount**.

-   Ao usar o Windows Management Framework em seu ambiente:

    -   Verifique se o (s) servidor (es) com SILA instalado não têm o WMF 5,1 instalado.  É possível chegar a um erro no log de eventos referente à DLL **' mpunits.dll '**.  Isso impedirá a operação adequada.  SILA requer apenas o WMF 4,0.

## <a name="managing-sil-over-time"></a>Gerenciando o SIL com o tempo

### <a name="uninstallreinstall-sil-aggregator"></a>Desinstalar/reinstalar o Agregador do SIL
Se for necessário desinstalar e reinstalar o Agregador do SIL, você pode fazer isso sem perder dados de inventário existentes e históricos. Basta desinstalar (seguindo as etapas fornecidas nesta documentação) e selecionar a opção para manter o banco de dados do Log de Inventário de Software. Em seguida, reinstale o Agregador do SIL (seguindo as etapas fornecidas nesta documentação) e selecione a opção de anexar um banco de dados existente.

Depois de executar essa operação, é necessário atualizar as credenciais usando o cmdlet `Add-SilVMHost` em quaisquer hosts que anteriormente estavam sendo pesquisados pelo Agregador do SIL (supondo que você deseja continuar coletando dados destes hosts). Além disso, para evitar entradas duplicadas para o mesmo host em relatórios, é necessário adicionar hosts novamente para sondagem usando o mesmo endereço de rede que foi adicionado originalmente. Aqui estão os três tipos de vmhostname com suporte que podem ser usados para adicionar um host usando o cmdlet `Add-SilVMHost`:

-   Endereço IP

-   FQDN (nome de domínio totalmente qualificado)

-   Nome NetBIOS

### <a name="changing-sil-aggregators"></a>Alterando os Agregadores do SIL
Quando você desejar iniciar o inventário de servidores em seu ambiente com um Agregador do SIL diferente, basta usar o cmdlet do SIL nesses servidores para alterar o targeturi (e a impressão digital de certificado, se necessário), `Set-SilLogging –TargetUri`. Observe que depois de fazer isso é necessário usar o cmdlet `Publish-SilData` pelo menos uma vez para encaminhar um inventário completo para o Agregador do SIL especificado mais recentemente.

### <a name="changing-or-updating-certificates"></a>Alterando ou atualizando certificados
**ETAPAS IMPORTANTES PARA EVITAR A PERDA DE DADOS:** se for necessário alterar o certificado que os servidores estão usando para encaminhar dados para um Agregador do SIL, mas o Agregador de destino permanece o mesmo, siga estas etapas para evitar uma possível perda de dados em trânsito para o Agregador:

-   No Agregador do SIL, use o cmdlet `Set-SilAggregator –AddCertificateThumbprint` para adicionar a nova impressão digital ao Agregador do SIL.

-   Em TODOS os servidores que encaminham dados, instale o novo certificado a ser usado no **\COMPUTADORLOCAL\MEU** usando o seu método preferencial.

-   Em TODOS os servidores que encaminham dados, use o cmdlet `Set-SilLogging –CertificateThumbprint` para atualizar para a impressão digital do novo certificado.

-   **CRÍTICO: somente depois que todos os servidores que encaminham dados forem atualizados, remova a impressão digital antiga** do Agregador do SIL usando o cmdlet `Set-SilAggregator –RemoveCertificateThumbprint`. Se um servidor que encaminha dados continuar encaminhando com um certificado antigo que foi removido do Agregador do SIL, **os dados serão perdidos** e não são inseridos no banco de dados do Agregador. Isso afeta apenas os cenários em que um servidor encaminhou com êxito os dados para um agregador SIL e o certificado é removido da lista de impressões digitais do agregador SIL para aceitar dados.

## <a name="release-notes"></a>Notas de versão

-   Há um problema conhecido que o Agregador SIL não processará e relatará na presentação das instalaçoes do SQL Server Standard Edition.  Estas são as etapas para corrigir isso:

    1.  Abra o SQL Server Management Studio no Agregador SIL.

    2.  Não é possível conectar-se ao Mecanismo do Banco de Dados.

    3.  Expanda o banco de dados SoftwareInventoryLogging e Tabelas, na árvore de seleção.

    4.  Clique com o botão direito em **dbo. SqlServerEdition**e selecione "editar as**200 linhas superiores**".

    5.  Altere o PropertyNumValue ao lado de "Standard Edition" para **2760240536** (de-1534726760).

    6.  Feche a consulta para salvar a alteração.

    7.  Para qualquer servidor executando SIL que já tem dados conectados a este Agregador, talvez seja necessário executar o Cmdlet `Publish-SilData` uma vez para o Agregador processar corretamente a presença do SQL Server Standard Edition.

-   Nos relatórios gerados pelo SIL, todas as contagens de núcleos de processador incluem a contagem de threads se o hyper-threading estiver habilitado no servidor físico.  Para obter as contagens reais de núcleos físicos em servidores com o hyper-threading habilitado, é necessário reduzir essas contagens pela metade.

-   Os totais nas linhas (na guia **painel** ) e colunas (nas guias **Resumo e detalhes** ) rotulados como "**executando simultaneamente**...", para o Windows Server e o System Center, não correspondem exatamente aos dois locais. Na guia **painel** , é necessário adicionar o valor "**dispositivos do Windows Server (sem VMs conhecidas**)" ao "**executando simultaneamente**..." valor para igual a esse número nas guias **Resumo e detalhes** .

-   Veja **ETAPAS IMPORTANTES PARA EVITAR PERDA DE DADOS** ao alterar ou atualizar certificados de acordo com a seção **Gerenciando o SIL com o tempo** desta documentação.

-   Embora seja possível adicionar hosts do Windows Server 2008 R2 e do Windows Server 2012 à lista de hosts de sondagem, esta versão (1.0) do Agregador do SIL dá suporte apenas à sondagem do Windows Server 2012 R2, para hosts baseados no Windows/Hyper-V, para ter êxito com todos os recursos e funcionalidades.  Em particular, é comum que durante a sondagem de hosts do Windows Server 2008 R2, as máquinas virtuais e os hosts talvez não sejam correspondentes nos relatórios do Agregador do SIL.

## <a name="see-also"></a>Consulte Também
[Agregador do Log de Inventário de Software 1.0 para Windows Server](https://www.microsoft.com/download/details.aspx?id=49046)<br>
[Cmdlets do PowerShell do Agregador do SIL](/previous-versions/windows/powershell-scripting/mt548455(v=wps.640))<br>
[Cmdlets do PowerShell do SIL](/powershell/module/softwareinventorylogging/?view=winserver2012R2-ps)<br>
[Uma visão geral do SIL](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn268301(v=ws.11))<br>
[Gerenciando o SIL](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383584(v=ws.11))