---
ms.assetid: c54b544f-cc32-4837-bb2d-a8656b22f3de
title: Introdução à replicação do Active Directory e ao gerenciamento de topologia usando o Windows PowerShell (nível 100)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: c8a5863865d465d55f1d5865fdcbdeeb942ce194
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71409089"
---
# <a name="introduction-to-active-directory-replication-and-topology-management-using-windows-powershell-level-100"></a>Introdução à replicação do Active Directory e ao gerenciamento de topologia usando o Windows PowerShell (nível 100)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O Windows PowerShell para Active Directory inclui a capacidade de gerenciar replicação, sites, domínios e florestas, controladores de domínio e partições. Os usuários de ferramentas de gerenciamento anteriores, como o snap-in de Sites e Serviços do Active Directory e repadmin.exe perceberão que, agora, funcionalidades semelhantes estão disponíveis no Windows PowerShell para o contexto do Active Directory. Além disso, os cmdlets são compatíveis com os cmdlets atuais do Windows PowerShell para Active Directory, criando assim uma experiência agilizada e permitindo clientes facilmente scripts de automação.

> [!NOTE]
> Os cmdlets de replicação e topologia do Windows PowerShell para Active Directory estão disponíveis nos seguintes ambientes:
> 
> -    Controlador de domínio do Windows Server 2012
> -    O Windows Server 2012 com o Ferramentas de Administração de Servidor Remoto para AD DS e AD LDS instalado.
> -   Windows @ no__t-0 8 com o Ferramentas de Administração de Servidor Remoto para AD DS e AD LDS instalados.

## <a name="installing-the-active-directory-module-for-windows-powershell"></a>Instalando o módulo do Active Directory para Windows PowerShell
O módulo Active Directory para Windows PowerShell é instalado por padrão quando a função de servidor AD DS é instalada em um servidor que executa o Windows Server 2012. Nenhuma etapa adicional é necessária além de adicionar a função de servidor. Você também pode instalar o módulo Active Directory em um servidor que executa o Windows Server 2012 instalando o Ferramentas de Administração de Servidor Remoto, e você pode instalar o módulo Active Directory em um computador que executa o Windows 8 baixando e instalando o [ Ferramentas administrativas de servidor remoto (RSAT)](https://www.microsoft.com/download/details.aspx?id=28972). Consulte as [Instruções](https://www.microsoft.com/download/details.aspx?id=28972)para ver as etapas de instalação.

## <a name="scenarios-for-testing-windows-powershell-for-active-directory-replication-and-topology-management-cmdlets"></a>Cenários para testar os cmdlets de gerenciamento de replicação e topologia do Windows PowerShell para Active Directory.
Os seguintes cenários são concebidos para administradores com os novos cmdlets de gerenciamento:

-   Obtenha uma lista de todos os controladores de domínio e seus respectivos sites

-   a topologia de replicação

-   Visualize o status e informações de replicação

## <a name="lab-requirements"></a>Requisitos do laboratório

-   Dois controladores de domínio do Windows Server 2012: **DC1** e **DC2**, que fazem parte do domínio contoso.com e residem no site CORPORATE dentro desse domínio.

## <a name="view-domain-controllers-and-their-sites"></a>Visualize os controladores de domínio e seus sites
Nesta etapa, você usará o Módulo do Active Directory para Windows PowerShell visualizar os controladores de domínio atuais e a topologia de replicação  domínio.

Para concluir os passos nos procedimentos, você deve ser um membro do grupo de Administradores de Domínio ou ter equivalentes.

#### <a name="to-view-all-active-directory-sites"></a>Para visualizar todos os sites do Active Directory

1.  Em **DC1**, clique em **Windows PowerShell** na barra de tarefas.

2.  Digite o seguinte comando:

    `Get-ADReplicationSite -Filter *`

    Isto retorna informações detalhadas sobre cada site. O parâmetro `Filter` é usado em cmdlets PowerShell do Active Directory para limitar a lista de objetos retornados. Neste caso, o asterisco (*) indica todos os objetos de site.

    > [!TIP]
    > Você pode usar a tecla Tab para completar comandos automaticamente no Windows PowerShell.
    > 
    > Exemplo: digite `Get-ADRep` e pressione Tab várias vezes para saltar entre os comandos correspondentes até acessar `Get-ADReplicationSite`. O recurso de preenchimento automático também funciona para nomes de parâmetro, como `Filter`.

    Para formatar a saída do comando `Get-ADReplicationSite` como uma tabela e limitar a exibição a campos específicos, você pode canalizar a saída para o comando `Format-Table` (ou "`ft`" para curto):

    `Get-ADReplicationSite -Filter * | ft Name`

    Isto retorna uma versão reduzida da lista de sites, incluindo apenas o campo Nome.

#### <a name="to-produce-a-table-of-all-domain-controllers"></a>Para produzir uma tabela de todos os controladores de domínio

-   Digite o seguinte comando no prompt do **módulo do Active Directory para Windows PowerShell** :

    `Get-ADDomainController -Filter * | ft Hostname,Site`

    Este comando retorna os nomes de dos controladores de domínio, bem como .

## <a name="manage-replication-topology"></a>a topologia de replicação
Na etapa anterior, após a execução do comando, `Get-ADDomainController -Filter * | ft Hostname,Site`, **DC2** foi listado como parte do site **CORPORATE** . Nos procedimentos a seguir, você criará um novo site de filial, **BRANCH1**, criará um novo link de site, definirá a frequência de replicação e o custo desse link e depois moverá **DC2** para **BRANCH1**.

Para concluir os passos nos procedimentos, você deve ser um membro do grupo de Administradores de Domínio ou ter equivalentes.

#### <a name="to-create-a-new-site"></a>Para criar um novo site

-   Digite o seguinte comando no prompt do **módulo do Active Directory para Windows PowerShell** :

    `New-ADReplicationSite BRANCH1`

    Este comando cria um novo site de filial, filial1.

#### <a name="to-create-a-new-site-link"></a>Para criar um link do novo site

-   Digite o seguinte comando no prompt do **módulo do Active Directory para Windows PowerShell** :

    `New-ADReplicationSiteLink 'CORPORATE-BRANCH1'  -SitesIncluded CORPORATE,BRANCH1 -OtherAttributes @{'options'=1}`

    Esse comando criou o link de site para **BRANCH1** e ativou o processo de notificação de alterações.

    > [!TIP]
    > Use Tab para preencher automaticamente nomes de parâmetro, como `-SitesIncluded` e `-OtherAttributes`, em vez de digitá-los manualmente.

#### <a name="to-set-the-site-link-cost-and-replication-frequency"></a>Para definir o custo e frequência de replicação link do site

-   Digite o seguinte comando no prompt do **módulo do Active Directory para Windows PowerShell** :

    `Set-ADReplicationSiteLink CORPORATE-BRANCH1 -Cost 100 -ReplicationFrequencyInMinutes 15`

    Esse comando define o custo do link de site para **BRANCH1** como **100** e define a frequência de replicação no site como **15 minutos**.

#### <a name="to-move-a-domain-controller-to-a-different-site"></a>Para mover um controlador de domínio para um site diferente

-   Digite o seguinte comando no prompt do **módulo do Active Directory para Windows PowerShell** :

    `Get-ADDomainController DC2 | Move-ADDirectoryServer -Site BRANCH1`

    Esse comando move o controlador de domínio **DC2** até o site **BRANCH1**.

### <a name="verification"></a>Verificação

##### <a name="to-verify-site-creation-new-site-link-and-cost-and-replication-frequency"></a>Para confirmar a criação do site, novo link do site, custo e frequência de replicação

-   Clique em **Gerenciador do Servidor**, em **Ferramentas** e depois em **Sites e Serviços do Active Directory** e verifique o seguinte:

    Verifique se o site **BRANCH1** contém todos os valores corretos provenientes dos comandos Windows PowerShell.

    Verifique se o link de site **CORPORATE-BRANCH1** foi criado e se conecta aos sites **BRANCH1** e **CORPORATE**.

    Verifique se **DC2** está agora no site **BRANCH1** . Como alternativa, você pode abrir o **Módulo Active Directory para Windows PowerShell** e digitar o seguinte comando para verificar se **DC2** está agora no site **BRANCH1**: `Get-ADDomainController -Filter * | ft Hostname,Site`.

## <a name="view-replication-status-information"></a>Ver informações de status de replicação
Nos procedimentos a seguir, você usará um dos cmdlets de replicação e gerenciamento Windows PowerShell para Active Directory, `Get-ADReplicationUpToDatenessVectorTable DC1`, para produzir um relatório de replicação simples usando a tabela de vetores de atualização mantida por cada controlador de domínio. Esta tabela de vetores de  mantém um dos USN de gravação originárias vistas em cada controlador de domínio da floresta.

Para concluir os passos nos procedimentos, você deve ser um membro do grupo de Administradores de Domínio ou ter equivalentes.

#### <a name="to-view-the-up-to-dateness-vector-table-for-a-single-domain-controller"></a>Para visualizar a tabela de vetores de um único controlador de domínio

1.  Digite o seguinte comando no prompt do **módulo do Active Directory para Windows PowerShell** :

    `Get-ADReplicationUpToDatenessVectorTable DC1`

    Isso mostra uma lista dos USNs mais altos vistos por **DC1** para cada controlador de domínio da floresta. O valor **Server** refere-se ao servidor que mantém a tabela, neste caso **DC1**. O valor **Partner** refere-se ao parceiro de replicação (direto ou indireto) no qual foram feitas alterações. O valor UsnFilter é o USN mais alto visto por **DC1** a partir de Partner. Se um novo controlador de domínio for adicionado à floresta, ele não será exibido na tabela do **DC1**até que **DC1** receba uma alteração originada do novo domínio.

#### <a name="to-view-the-up-to-dateness-vector-table-for-all-domain-controllers-in-a-domain"></a>Para visualizar a tabela de vetores de para todos os controladores de domínio em um domínio

1.  Digite o seguinte comando do módulo do Active Directory Windows PowerShell:

    `Get-ADReplicationUpToDatenessVectorTable * | sort Partner,Server | ft Partner,Server,UsnFilter`

    Esse comando substitui **DC1** por `*`, coletando assim os dados da tabela de vertores de atualização de todos os controladores de domínio. Os dados são classificados por **Partner** e **Server** e depois exibidos em uma tabela.

    A triagem permite comparar facilmente o último USN visto por cada controlador de domínio para determinado parceiro de replicação. Esta é uma forma rápida de verificar se está ocorrendo replicação em seu ambiente. Se a replicação estiver funcionando corretamente, os valores do UsnFilter relatados para determinado parceiro de replicação devem ser bastante semelhantes em todos os controladores de domínio.

## <a name="see-also"></a>Consulte também
[Replicação avançada de Active Directory e gerenciamento de topologia usando &#40;o Windows PowerShell nível 200&#41;](Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md)


