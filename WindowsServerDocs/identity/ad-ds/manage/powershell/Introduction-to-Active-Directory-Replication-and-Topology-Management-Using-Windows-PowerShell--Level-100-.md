---
ms.assetid: c54b544f-cc32-4837-bb2d-a8656b22f3de
title: "Introdução à replicação do Active Directory e gerenciamento de topologia usando o Windows PowerShell (nível 100)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 006179bb3220f7bccfc7510e1b8ef69678321074
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="introduction-to-active-directory-replication-and-topology-management-using-windows-powershell-level-100"></a>Introdução à replicação do Active Directory e gerenciamento de topologia usando o Windows PowerShell (nível 100)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para o Active Directory do Windows PowerShell inclui a capacidade de gerenciar a replicação, sites, domínios e florestas, controladores de domínio e partições. Snap-in usuários anteriores de ferramentas de gerenciamento, como os serviços e Sites do Active Directory e repadmin.exe perceberão uma funcionalidade semelhante agora está disponível no Windows PowerShell para o contexto do Active Directory. Além disso, os cmdlets são compatíveis com o existente do Windows PowerShell para cmdlets do Active Directory, criando uma experiência simplificada, portanto e permitindo que os clientes criem facilmente scripts de automação.

> [!NOTE]
> O Windows PowerShell para replicação do Active Directory e cmdlets topologia estão disponíveis nos seguintes ambientes:
> 
> -    Controlador de domínio do Windows Server 2012
> -    Windows Server 2012 com as ferramentas de administração de servidor remoto para o AD DS e do AD LDS instalado.
> -   Windows&reg; 8 com as ferramentas de administração de servidor remoto para o AD DS e do AD LDS instalado.

## <a name="installing-the-active-directory-module-for-windows-powershell"></a>Instalar o módulo do Active Directory para Windows PowerShell
O Active Directory módulo para Windows PowerShell é instalado por padrão quando a função de servidor do AD DS é instalada em um servidor que executa o Windows Server 2012. Não há etapas adicionais são necessárias diferente adicionando a função de servidor. Você também pode instalar o módulo do Active Directory em um servidor que executa o Windows Server 2012 instalando as ferramentas de administração de servidor remoto, e você pode instalar o módulo do Active Directory em um computador executando o Windows 8 Baixando e instalando a [ferramentas administrativas de servidor remoto (RSAT)](https://www.microsoft.com/download/details.aspx?id=28972). Consulte [instruções](https://www.microsoft.com/download/details.aspx?id=28972)nas etapas de instalação.

## <a name="scenarios-for-testing-windows-powershell-for-active-directory-replication-and-topology-management-cmdlets"></a>Cenários de teste do Windows PowerShell para cmdlets de gerenciamento de replicação e topologia do Active Directory
Os cenários a seguir são projetados para os administradores podem se familiarizar com os novos cmdlets de gerenciamento:

-   Obter uma lista de todos os controladores de domínio e seus sites correspondentes

-   Gerenciar topologia de replicação

-   Exibir o status de replicação e informações

## <a name="lab-requirements"></a>Requisitos de laboratório

-   Dois controladores de domínio do Windows Server 2012: **DC1** e **DC2** que fazem parte do domínio contoso.com e residem no site corporativo dentro desse domínio.

## <a name="view-domain-controllers-and-their-sites"></a>Controladores de domínio do modo de exibição e seus sites
Nesta etapa, você usará o Active Directory módulo para Windows PowerShell para exibir os controladores de domínio existentes e a topologia de replicação para o domínio.

Para concluir as etapas nos procedimentos a seguir, você deve ser um membro do grupo Admins. do domínio ou tem permissões equivalentes.

#### <a name="to-view-all-active-directory-sites"></a>Para exibir todos os sites do Active Directory

1.  Em **DC1**, clique em **do Windows PowerShell** na barra de tarefas.

2.  Digite o seguinte comando:

    `Get-ADReplicationSite -Filter *`

    Isso retorna informações detalhadas sobre cada site. O `Filter`parâmetro é usado em todo o Active Directory PowerShell cmdlets para limitar a lista de objetos retornados. Nesse caso, o asterisco (*) indica a todos os objetos de site.

    > [!TIP]
    > Você pode usar a tecla Tab para comandos de preenchimento automático no Windows PowerShell.
    > 
    > Exemplo: Digite `Get-ADRep`e pressione Tab várias vezes para ignorá-las os comandos correspondentes até chegar `Get-ADReplicationSite`. Preenchimento automático também funciona para nomes de parâmetros como `Filter`.

    Para formatar a saída do `Get-ADReplicationSite`comando como uma tabela e limitar a exibição para campos específicos, você pode direcionar a saída para o `Format-Table`comando (ou "`ft`" de forma abreviada):

    `Get-ADReplicationSite -Filter * | ft Name`

    Isso retorna uma versão menor de lista de sites, incluindo apenas o campo de nome.

#### <a name="to-produce-a-table-of-all-domain-controllers"></a>Para produzir uma tabela de todos os controladores de domínio

-   Digite o seguinte comando na **módulo do Active Directory para Windows PowerShell** prompt:

    `Get-ADDomainController -Filter * | ft Hostname,Site`

    Esse comando retorna que os controladores de domínio hospedam nome, bem como suas associações de site.

## <a name="manage-replication-topology"></a>Gerenciar topologia de replicação
Na etapa anterior, depois de executar o comando `Get-ADDomainController -Filter * | ft Hostname,Site`, **DC2** foi listados como parte do **corporativa** site. Os procedimentos a seguir, você criará um novo site do office ramificação, **BRANCH1**, crie um novo link de site, definir a frequência de custo e replicação de link do site e, em seguida, mova **DC2** para **BRANCH1**.

Para concluir as etapas nos procedimentos a seguir, você deve ser um membro do grupo Admins. do domínio ou tem permissões equivalentes.

#### <a name="to-create-a-new-site"></a>Para criar um novo site

-   Digite o seguinte comando na **módulo do Active Directory para Windows PowerShell** prompt:

    `New-ADReplicationSite BRANCH1`

    Esse comando cria o novo site de filial, branch1.

#### <a name="to-create-a-new-site-link"></a>Para criar um novo link de site

-   Digite o seguinte comando na **módulo do Active Directory para Windows PowerShell** prompt:

    `New-ADReplicationSiteLink 'CORPORATE-BRANCH1'  -SitesIncluded CORPORATE,BRANCH1 -OtherAttributes @{'options'=1}`

    Esse comando criou o link de site para **BRANCH1** e ativou o processo de notificação de alteração.

    > [!TIP]
    > Use o guia para nomes de parâmetros de preenchimento automático como `-SitesIncluded`e `-OtherAttributes`em vez de digitá-los manualmente.

#### <a name="to-set-the-site-link-cost-and-replication-frequency"></a>Para definir a frequência de custo e replicação de link do site

-   Digite o seguinte comando na **módulo do Active Directory para Windows PowerShell** prompt:

    `Set-ADReplicationSiteLink CORPORATE-BRANCH1 -Cost 100 -ReplicationFrequencyInMinutes 15`

    Esse comando define o custo do link para **BRANCH1** em **100** e definir a frequência de replicação com o site para **15 minutos **.

#### <a name="to-move-a-domain-controller-to-a-different-site"></a>Para mover um controlador de domínio para um site diferente

-   Digite o seguinte comando na **módulo do Active Directory para Windows PowerShell** prompt:

    `Get-ADDomainController DC2 | Move-ADDirectoryServer -Site BRANCH1`

    Esse comando move o controlador de domínio, **DC2** para o **BRANCH1** site.

### <a name="verification"></a>Verificação

##### <a name="to-verify-site-creation-new-site-link-and-cost-and-replication-frequency"></a>Para verificar a criação de sites, novo link de site e frequência de custo e replicação

-   Clique em **Gerenciador do servidor**, clique em **ferramentas** e, em seguida, clique em **serviços e Sites do Active Directory** e verifique o seguinte:

    Verifique o **BRANCH1** site contém todos os valores corretos dos comandos do Windows PowerShell.

    Verifique se o **corporativa BRANCH1** link site é criado e se conecta a **BRANCH1** e **corporativa** sites.

    Verifique se **DC2** agora está no **BRANCH1** site. Como alternativa, você pode abrir o **módulo Active Directory para Windows PowerShell** e digite o seguinte comando para verificar **DC2** agora está no **BRANCH1** site:`Get-ADDomainController -Filter * | ft Hostname,Site`.

## <a name="view-replication-status-information"></a>Exibir informações de status de replicação
Os procedimentos a seguir, você irá usar uma do Windows PowerShell para replicação do Active Directory e cmdlets de gerenciamento, `Get-ADReplicationUpToDatenessVectorTable DC1`, para produzir um relatório de replicação simples usando a tabela de vetor UTDV mantida por cada controlador de domínio. Esta tabela de vetor UTDV mantém o controle das USN a gravação mais alto originador visto de cada controlador de domínio na floresta.

Para concluir as etapas nos procedimentos a seguir, você deve ser um membro do grupo Admins. do domínio ou tem permissões equivalentes.

#### <a name="to-view-the-up-to-dateness-vector-table-for-a-single-domain-controller"></a>Para exibir a tabela de vetor UTDV para um controlador de domínio único

1.  Digite o seguinte comando na **módulo do Active Directory para Windows PowerShell** prompt:

    `Get-ADReplicationUpToDatenessVectorTable DC1`

    Isso mostra uma lista de mais USNs visto pelo **DC1** para cada controlador de domínio na floresta. O **servidor** valor refere-se ao servidor mantendo a tabela, neste caso **DC1**. O **parceiro** valor refere-se para o parceiro de replicação (direto ou indireto) em que foram feitas mudanças. O valor de UsnFilter é o mais alto USN visto pelo **DC1** do parceiro. Se um novo controlador de domínio é adicionado à floresta, ele não aparecerá em **DC1**da tabela até **DC1** recebe uma alteração que originada do novo domínio.

#### <a name="to-view-the-up-to-dateness-vector-table-for-all-domain-controllers-in-a-domain"></a>Para exibir a tabela de vetor UTDV para todos os controladores de domínio em um domínio

1.  Digite o seguinte comando no módulo do Active Directory para prompt do Windows PowerShell:

    `Get-ADReplicationUpToDatenessVectorTable * | sort Partner,Server | ft Partner,Server,UsnFilter`

    Esse comando substitui **DC1** com `*`, portanto, coletando os dados da tabela UTDV vetor de todos os controladores de domínio. Os dados são classificados por **parceiro** e **servidor** e, depois, exibidos em uma tabela.

    A classificação permite que você facilmente comparar o último USN visto por cada controlador de domínio para um parceiro de replicação determinado. Isso é uma maneira rápida de verificar que a replicação está ocorrendo em seu ambiente. Se a replicação está funcionando corretamente, os valores de UsnFilter relatados para o parceiro de replicação determinado devem ser razoavelmente semelhantes entre todos os controladores de domínio.

## <a name="see-also"></a>Consulte também
[Replicação do Active Directory e gerenciamento de topologia usando o Windows PowerShell & #40; avançados Nível 200 & #41;](Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md)


