---
title: Getting Started with Group Managed Service Accounts
description: Segurança do Windows Server
ms.topic: article
ms.assetid: 7130ad73-9688-4f64-aca1-46a9187a46cf
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 19da2b6ec2a7a3ca31c479388c087850c77d9c23
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638052"
---
# <a name="getting-started-with-group-managed-service-accounts"></a>Getting Started with Group Managed Service Accounts

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016


Este guia fornece instruções passo a passo e informações básicas para habilitar e usar contas de serviço gerenciado de grupo no Windows Server 2012.

**Neste documento**

-   [Pré-requisitos](#BKMK_Prereqs)

-   [Introdução](#BKMK_Intro)

-   [Implantando um novo farm de servidores](#BKMK_DeployNewFarm)

-   [Adicionando hosts membros a um farm de servidores existente](#BKMK_AddMemberHosts)

-   [Atualizando as propriedades da conta de serviço gerenciado de grupo](#BKMK_Update_gMSA)

-   [Encerrando hosts membros em um farm de servidores existente](#BKMK_DecommMemberHosts)


> [!NOTE]
> Este tópico inclui cmdlets do Windows PowerShell de exemplo que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, confira [Usando os Cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).

## <a name="prerequisites"></a><a name="BKMK_Prereqs"></a>Pré-requisitos
Consulte a seção neste tópico em [Requisitos para as Contas de Serviço Gerenciado de grupo](#BKMK_gMSA_Req).

## <a name="introduction"></a><a name="BKMK_Intro"></a>Introdução
Quando um computador cliente se conecta a um serviço hospedado em um farm de servidores que usa NLB (balanceamento de carga de rede) ou algum outro método em que, para o cliente, todos os servidores parecem ser o mesmo serviço; os protocolos de autenticação que dão suporte à autenticação mútua, tal como o Kerberos, não podem ser usados, a menos que todas as instâncias dos serviços usem a mesma entidade. Isso significa que cada serviço tem que usar as mesmas senhas/chaves para provar sua identidade.

> [!NOTE]
> Os clusters de failover não dão suporte a gMSAs. No entanto, os serviços que são executados sobre o Serviço de cluster poderão usar uma gMSA ou uma sMSA, se forem um serviço Windows, um pool de aplicativos, uma tarefa agendada ou oferecerem suporte nativo a gMSA ou sMSA.

Os serviços têm as entidades a seguir entre as quais escolher, e cada uma delas apresenta determinadas limitações.

|Principals|Escopo|Serviços compatíveis|Gerenciamento de senhas|
|-------|-----|-----------|------------|
|Conta de Computador do sistema do Windows|Domínio|Limitado a um servidor ingressado no domínio|O computador gerencia|
|Conta de Computador sem o sistema do Windows|Domínio|Qualquer servidor ingressado no domínio|Nenhum|
|Conta Virtual|Local|Limitado a um servidor|O computador gerencia|
|Conta de Serviço Gerenciado independente do Windows 7|Domínio|Limitado a um servidor ingressado no domínio|O computador gerencia|
|Conta de Usuário|Domínio|Qualquer servidor ingressado no domínio|Nenhum|
|Conta de Serviço Gerenciado de Grupo|Domínio|Qualquer servidor ingressado no domínio do Windows Server 2012|O controlador de domínio gerencia e o host recupera|

Uma conta de computador do Windows, uma sMSA (Conta de Serviço Gerenciado independente) do Windows 7 ou contas virtuais não podem ser compartilhadas em diversos sistemas. Se você configurasse uma conta para serviços em farms de servidores a serem compartilhados, teria que escolher uma conta de usuário ou uma conta de computador separada do sistema do Windows. De qualquer maneira, essas contas não têm a funcionalidade de gerenciamento de senhas de ponto único de controle. Isso cria um problema em que cada organização precisa criar uma solução dispendiosa para atualizar chaves do serviço no Active Directory e distribuir as chaves a todas as instâncias desses serviços.

Com o Windows Server 2012, serviços ou administradores de serviço não precisam gerenciar a sincronização de senha entre instâncias de serviço ao usar contas de serviço gerenciado de grupo (gMSA). Você provisiona a gMSA no AD e configura os serviços que dão suporte a Contas de Serviço Gerenciado. Você pode provisionar uma gMSA usando os cmdlets *-ADServiceAccount que fazem parte do módulo do Active Directory. A configuração de identidade do serviço no host tem suporte por:

-   Algumas APIs, como sMSA, portanto, produtos que dão suporte a sMSA oferecerão suporte a gMSA

-   Serviços que usam o Gerenciador de Controle de Serviço para configurar a identidade de logon

-   Serviços que usam o gerenciador do IIS em pools de aplicativos para configurar identidade

-   Tarefas que usam o Agendador de Tarefas.

### <a name="requirements-for-group-managed-service-accounts"></a><a name="BKMK_gMSA_Req"></a>Requisitos para as Contas de Serviço Gerenciado de grupo
A tabela a seguir lista os requisitos de sistema operacional para a autenticação Kerberos funcionar com serviços que usam gMSA. Os requisitos do Active Directory estão listados após a tabela.

Uma arquitetura de 64 bits é necessária para executar os comandos do Windows PowerShell usados para administrar as Contas de Serviço Gerenciado de grupo.

**Requisitos do sistema operacional**

|Elemento|Requisito|Sistema operacional|
|------|--------|----------|
|Host de Aplicativo Cliente|Cliente Kerberos compatível com RFC|Pelo menos Windows XP|
|DCs de domínio da conta do usuário|KDC compatível com RFC|Pelo menos Windows Server 2003|
|Hosts membros de serviço compartilhado|| Windows Server 2012 |
|Controladores de domínio do host membro|KDC compatível com RFC|Pelo menos Windows Server 2003|
|Controladores de domínio da conta do gMSA| DCs do Windows Server 2012 disponíveis para o host recuperarem a senha|Domínio com o Windows Server 2012 que pode ter alguns sistemas anteriores ao Windows Server 2012 |
|Host de serviço back-end|Servidor de aplicativos Kerberos compatível com RFC|Pelo menos Windows Server 2003|
|Controladores de domínio da conta do serviço de back-end|KDC compatível com RFC|Pelo menos Windows Server 2003|
|Windows PowerShell para Active Directory|Windows PowerShell para Active Directory instalado localmente em um computador que dê suporte a uma arquitetura de 64 bits ou em seu computador de gerenciamento remoto (por exemplo, usando o Remote Server Administration Toolkit)| Windows Server 2012 |

**Requisitos do Serviço de Domínio Active Directory**

-   O esquema de Active Directory na floresta do domínio gMSA precisa ser atualizado para o Windows Server 2012 para criar um gMSA.

    Você pode atualizar o esquema instalando um controlador de domínio que executa o Windows Server 2012 ou executando a versão do adprep.exe de um computador que executa o Windows Server 2012. O valor de atributo de versão do objeto para o objeto CN=Schema,CN=Configuration,DC=Contoso,DC=Com deve ser 52.

-   Nova conta gMSA provisionada

-   Se você estiver gerenciando a permissão de host do serviço para usar a gMSA por grupo, o grupo de segurança novo ou existente

-   Se estiver gerenciando o controle de acesso ao serviço por grupo, o grupo de segurança novo ou existente

-   Se a primeira chave raiz mestra do Active Directory não estiver implantada no domínio ou não tiver sido criada, crie-a. O resultado dessa criação pode ser verificado no log Operacional KdsSvc, ID de Evento 4004.

Para obter instruções sobre como criar a chave, consulte [criar a chave de raiz KDS dos serviços de distribuição de chaves](create-the-key-distribution-services-kds-root-key.md). A chave raiz do Serviço de Distribuição de Chave da Microsoft (kdssvc.dll) do AD.

**Ciclo de vida**

O ciclo de vida de um farm de servidores que usa um recurso de gMSA geralmente envolve as seguintes tarefas:

-   Implantando um novo farm de servidores

-   Adicionando hosts membros a um farm de servidores existente

-   Encerrando hosts membros em um farm de servidores existente

-   Encerrando um farm de servidores existente

-   Removendo um host membro comprometido de um farm de servidores, se necessário.

## <a name="deploying-a-new-server-farm"></a><a name="BKMK_DeployNewFarm"></a>Implantando um novo farm de servidores
Ao implantar um novo farm de servidores, o administrador de serviço precisará determinar:

-   Se o serviço dá suporte ao uso de gMSAs

-   Se o serviço requer conexões autenticadas de entrada ou de saída

-   Os nomes de conta do computador dos hosts membros para o serviço que usa a gMSA

-   O nome NetBIOS para o serviço

-   O nome de host DNS para o serviço

-   Os SPNs (Nomes da Entidades de Serviço) para o serviço

-   O intervalo de alteração de senha (o padrão é 30 dias).

### <a name="step-1-provisioning-group-managed-service-accounts"></a><a name="BKMK_Step1"></a>Etapa 1: provisionando Contas de Serviço Gerenciado de grupo
Você pode criar um gMSA somente se o esquema de floresta foi atualizado para o Windows Server 2012, a chave raiz mestra para Active Directory foi implantada e há pelo menos um Windows Server 2012 DC no domínio no qual o gMSA será criado.

A associação em **Admins. do Domínio**, **Opers. de Contas** ou a capacidade de criar objetos msDS-GroupManagedServiceAccount é o mínimo necessário para concluir os procedimentos a seguir.

> [!NOTE]
> Um valor para o parâmetro-Name é sempre necessário (independentemente de você especificar-Name ou not), com-DNSHostName,-RestrictToSingleComputer e-RestrictToOutboundAuthentication os requisitos secundários para os três cenários de implantação.


#### <a name="to-create-a-gmsa-using-the-new-adserviceaccount-cmdlet"></a><a name="BKMK_CreateGMSA"></a>Para criar uma gMSA usando o cmdlet New-ADServiceAccount

1.  No controlador de domínio do Windows Server 2012, execute o Windows PowerShell na barra de tarefas.

2.  No prompt de comando do Windows PowerShell, digite os comandos a seguir e pressione ENTER. (O módulo Active Directory será carregado automaticamente.)

    **New-ADServiceAccount [-name] &lt; String &gt; -dNSHostName &lt; cadeia de caracteres &gt; [-KerberosEncryptionType &lt; ADKerberosEncryptionType &gt; ] [-ManagedPasswordIntervalInDays <Nullable [Int32] >] [-PrincipalsAllowedToRetrieveManagedPassword <ADPrincipal [] >] [-sAMAccountName &lt; String &gt; ] [-serviceprincipalnamenames <String [] >]**

    |Parâmetro|String|Exemplo|
    |-------|-----|------|
    |Nome|Nome da conta|ITFarm1|
    |DNSHostName|Nome de host DNS do serviço|ITFarm1.contoso.com|
    |KerberosEncryptionType|Quaisquer tipos de criptografia com suporte pelos servidores host|Nenhum, RC4, AES128, AES256|
    |ManagedPasswordIntervalInDays|Intervalo de alteração de senha em dias (o padrão é 30 dias, se nenhum tiver sido fornecido)|90|
    |PrincipalsAllowedToRetrieveManagedPassword|As contas de computador dos hosts membros ou o grupo de segurança dos quais os hosts membros fazem parte|ITFarmHosts|
    |SamAccountName|Nome NetBIOS para o serviço, se não for igual a Nome|ITFarm1|
    |ServicePrincipalNames|SPNs (Nomes da entidade de serviço) para o serviço|http/ITFarm1. contoso. com/contoso. com, http/ITFarm1. contoso. com/contoso, http/ITFarm1/contoso. com, http/ITFarm1/contoso, MSSQLSvc/ITFarm1. contoso. com: 1433, MSSQLSvc/ITFarm1. contoso. com: INST01|

    > [!IMPORTANT]
    > O intervalo de alteração de senha só pode ser configurado durante a criação. Se for necessário alterar o intervalo, crie uma nova gMSA e configure-a no momento da criação.

    **Exemplo**

    Insira o comando em uma única linha, mesmo se houver quebra automática de linha em várias linhas devido a restrições de formatação.

    ```Powershell
    New-ADServiceAccount ITFarm1 -DNSHostName ITFarm1.contoso.com -PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts$ -KerberosEncryptionType RC4, AES128, AES256 -ServicePrincipalNames http/ITFarm1.contoso.com/contoso.com, http/ITFarm1.contoso.com/contoso, http/ITFarm1/contoso.com, http/ITFarm1/contoso
    ```

A associação em **Admins. do Domínio**, **Opers. de Contas** ou a capacidade de criar objetos de msDS-GroupManagedServiceAccount é o mínimo necessário para concluir esse procedimento. Para obter informações detalhadas sobre como usar as contas e associações de grupo apropriadas, consulte [grupos padrão de domínio e locais](/previous-versions/orphan-topics/ws.10/dd728026(v=ws.10)).

##### <a name="to-create-a-gmsa-for-outbound-authentication-only-using-the-new-adserviceaccount-cmdlet"></a>Para criar uma gMSA para autenticação de saída usando apenas o cmdlet New-ADServiceAccount

1.  No controlador de domínio do Windows Server 2012, execute o Windows PowerShell na barra de tarefas.

2.  No prompt de comando do módulo Active Directory do Windows PowerShell, digite os seguintes comandos e pressione ENTER:

    **New-ADServiceAccount [-name] &lt; cadeia &gt; de caracteres-RestrictToOutboundAuthenticationOnly [-ManagedPasswordIntervalInDays <Nullable [Int32] >] [-PrincipalsAllowedToRetrieveManagedPassword <ADPrincipal [] >]**

    |Parâmetro|String|Exemplo|
    |-------|-----|------|
    |Nome|Nome da conta|ITFarm1|
    |ManagedPasswordIntervalInDays|Intervalo de alteração de senha em dias (o padrão é 30 dias, se nenhum tiver sido fornecido)|75|
    |PrincipalsAllowedToRetrieveManagedPassword|As contas de computador dos hosts membros ou o grupo de segurança dos quais os hosts membros fazem parte|ITFarmHosts|

    > [!IMPORTANT]
    > O intervalo de alteração de senha só pode ser configurado durante a criação. Se for necessário alterar o intervalo, crie uma nova gMSA e configure-a no momento da criação.

  **Exemplo**

```PowerShell
New-ADServiceAccount ITFarm1 -RestrictToOutboundAuthenticationOnly - PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts$
```

### <a name="step-2-configuring-service-identity-application-service"></a><a name="BKMK_ConfigureServiceIdentity"></a>Etapa 2: configurando o serviço de aplicativo de identidade do serviço
Para configurar os serviços no Windows Server 2012, consulte a seguinte documentação de recurso:

-   Pool de aplicativos do IIS

    Para obter mais informações, consulte [Especificar uma identidade para um pool de aplicativos (IIS 7)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771170(v=ws.10)).

-   Serviços Windows

    Para obter mais informações, consulte [Serviços](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772408(v=ws.11)).

-   Tarefas

    Para obter mais informações, consulte [Visão geral sobre o Agendador de Tarefas](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc721871(v=ws.11)).

Outros serviços poderiam dar suporte a gMSA. Consulte a documentação de produto apropriada para obter detalhes sobre como configurar esses serviços.

## <a name="adding-member-hosts-to-an-existing-server-farm"></a><a name="BKMK_AddMemberHosts"></a>Adicionando hosts membros a um farm de servidores existente
Se estiver usando grupos de segurança para gerenciar hosts membros, adicione a conta de computador do novo host membro ao grupo de segurança (que os hosts membros do gMSA são membros) usando um dos métodos a seguir.

A associação em **Admins. do Domínio** ou a capacidade de adicionar membros ao objeto de grupo de segurança é o mínimo necessário para concluir esses procedimentos.

-   Método 1: Usuários e computadores do Active Directory

    Para obter os procedimentos sobre como usar o método, consulte [Adicionar uma conta de computador a um grupo](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc733097(v=ws.11)) usando a interface do Windows e [Gerenciar diferentes domínios no Centro Administrativo do Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

-   Método 2: dsmod

    Para obter os procedimentos sobre como usar o método, consulte [Adicionar uma conta de computador a um grupo](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc733097(v=ws.11)) usando a linha de comando.

-   Método 3: cmdlet Active Directory Add-ADPrincipalGroupMembership do Windows PowerShell

    Para obter os procedimentos sobre como usar o método, consulte [Add-ADPrincipalGroupMembership](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee617203(v=technet.10)).

Se estiver usando contas de computador, localize as contas existentes e adicione a nova conta de computador.

A associação em **Admins. do Domínio**, **Opers. de Contas** ou a capacidade de gerenciar objetos de msDS-GroupManagedServiceAccount é o mínimo necessário para concluir esse procedimento. Para obter informações detalhadas sobre como usar as contas e as associações a grupos apropriadas, consulte Grupos padrão Local e Domínio.

#### <a name="to-add-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>Para adicionar hosts membros usando o cmdlet Set-ADServiceAccount

1.  No controlador de domínio do Windows Server 2012, execute o Windows PowerShell na barra de tarefas.

2.  No prompt de comando do módulo Active Directory do Windows PowerShell, digite os seguintes comandos e pressione ENTER:

    **Get-ADServiceAccount [-Identity] &lt; cadeia &gt; de caracteres-Propriedades PrincipalsAllowedToRetrieveManagedPassword**

3.  No prompt de comando do módulo Active Directory do Windows PowerShell, digite os seguintes comandos e pressione ENTER:

    **Set-ADServiceAccount [-Identity] &lt; String &gt; -PrincipalsAllowedToRetrieveManagedPassword <ADPrincipal [] >**

|Parâmetro|String|Exemplo|
|-------|-----|------|
|Nome|Nome da conta|ITFarm1|
|PrincipalsAllowedToRetrieveManagedPassword|As contas de computador dos hosts membros ou o grupo de segurança dos quais os hosts membros fazem parte|Host1, Host2, Host3|

**Exemplo**

Por exemplo, para adicionar hosts membros, digite os comandos a seguir e pressione ENTER.

```PowerShell
Get-ADServiceAccount [-Identity] ITFarm1 -Properties PrincipalsAllowedToRetrieveManagedPassword
```

```PowerShell
Set-ADServiceAccount [-Identity] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword Host1$,Host2$,Host3$
```

## <a name="updating-the-group-managed-service-account-properties"></a><a name="BKMK_Update_gMSA"></a>Atualizando as propriedades da Conta de Serviço Gerenciado de grupo
A associação em **Admins. do Domínio**, **Opers. de Contas** ou a capacidade de gravar em objetos msDS-GroupManagedServiceAccount é o mínimo necessário para concluir esses procedimentos.

Abra o Módulo Active Directory do Windows PowerShell e configure qualquer propriedade usando o cmdlet Set-ADServiceAccount.

Para obter informações detalhadas sobre como configurar essas propriedades, consulte [Set-ADServiceAccount](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee617252(v=technet.10)) na Biblioteca do TechNet ou digite **Get-Help Set-ADServiceAccount** no prompt de comando do módulo Active Directory para Windows PowerShell e pressione ENTER.

## <a name="decommissioning-member-hosts-from-an-existing-server-farm"></a><a name="BKMK_DecommMemberHosts"></a>Encerrando hosts membros em um farm de servidores existente
A associação em **Admins. do Domínio** ou a capacidade de remover membros do objeto de grupo de segurança é o mínimo necessário para concluir esses procedimentos.

### <a name="step-1-remove-member-host-from-gmsa"></a>Etapa 1: remover host membro da gMSA
Se estiver usando grupos de segurança para gerenciar hosts membros, remova a conta de computador do host do membro descomissionado do grupo de segurança ao qual os hosts membros do gMSA são membros usando qualquer um dos métodos a seguir.

-   Método 1: Usuários e computadores do Active Directory

    Para obter os procedimentos sobre como usar o método, consulte [Excluir uma conta de computador](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754624(v=ws.11)) usando a interface do Windows e [Gerenciar diferentes domínios no Centro Administrativo do Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

-   Método 2: drsm

    Para obter os procedimentos sobre como usar o método, consulte [Excluir uma conta de computador](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754624(v=ws.11)) usando a linha de comando.

-   Método 3: cmdlet Active Directory Remove-ADPrincipalGroupMembership do Windows PowerShell

    Para obter informações detalhadas sobre como fazer isso, consulte  [Remove-ADPrincipalGroupMembership](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee617243(v=technet.10)) na Biblioteca do TechNet ou digite **Get-Help Remove-ADPrincipalGroupMembership** no prompt de comando do módulo Active Directory para Windows PowerShell e pressione ENTER.

Se estiver listando contas de computador, recupere as contas existentes e adicione todas, exceto a conta de computador removida.

A associação em **Admins. do Domínio**, **Opers. de Contas** ou a capacidade de gerenciar objetos de msDS-GroupManagedServiceAccount é o mínimo necessário para concluir esse procedimento. Para obter informações detalhadas sobre como usar as contas e as associações a grupos apropriadas, consulte Grupos padrão Local e Domínio.

##### <a name="to-remove-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>Para remover hosts membros usando o cmdlet Set-ADServiceAccount

1.  No controlador de domínio do Windows Server 2012, execute o Windows PowerShell na barra de tarefas.

2.  No prompt de comando do módulo Active Directory do Windows PowerShell, digite os seguintes comandos e pressione ENTER:

    **Get-ADServiceAccount [-Identity] &lt; cadeia &gt; de caracteres-Propriedades PrincipalsAllowedToRetrieveManagedPassword**

3.  No prompt de comando do módulo Active Directory do Windows PowerShell, digite os seguintes comandos e pressione ENTER:

    **Set-ADServiceAccount [-Identity] &lt; String &gt; -PrincipalsAllowedToRetrieveManagedPassword <ADPrincipal [] >**

|Parâmetro|String|Exemplo|
|-------|-----|------|
|Nome|Nome da conta|ITFarm1|
|PrincipalsAllowedToRetrieveManagedPassword|As contas de computador dos hosts membros ou o grupo de segurança dos quais os hosts membros fazem parte|Host1, Host3|

**Exemplo**

Por exemplo, para remover hosts membros, digite os comandos a seguir e pressione ENTER.

```PowerShell
Get-ADServiceAccount [-Identity] ITFarm1 -Properties PrincipalsAllowedToRetrieveManagedPassword
```

```PowerShell
Set-ADServiceAccount [-Identity] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword Host1$,Host3$
```

### <a name="step-2-removing-a-group-managed-service-account-from-the-system"></a><a name="BKMK_RemoveGMSA"></a>Etapa 2: removendo uma Conta de Serviço Gerenciado de grupo do sistema
Remover as credenciais da gMSA armazenadas em cache do host membro usando a API Uninstall-ADServiceAccount ou NetRemoveServiceAccount no sistema host.

A associação em **Administradores** ou equivalente é o mínimo necessário para concluir esses procedimentos.

##### <a name="to-remove-a-gmsa-using-the-uninstall-adserviceaccount-cmdlet"></a>Para remover uma gMSA usando o cmdlet Uninstall-ADServiceAccount

1.  No controlador de domínio do Windows Server 2012, execute o Windows PowerShell na barra de tarefas.

2.  No prompt de comando do módulo Active Directory do Windows PowerShell, digite os seguintes comandos e pressione ENTER:

    **Uninstall-ADServiceAccount &lt; ADServiceAccount&gt;**

    **Exemplo**

    Por exemplo, para remover as credenciais armazenadas em cache para uma gMSA denominada ITFarm1, digite o seguinte comando e pressione ENTER:

    ```PowerShell
    Uninstall-ADServiceAccount ITFarm1
    ```

Para obter mais informações sobre o cmdlet Uninstall-ADServiceAccount, no prompt de comando do módulo Active Directory para Windows PowerShell, digite **Get-Help Uninstall-ADServiceAccount**e pressione ENTER ou consulte as informações na Web no TechNet em [Uninstall-ADServiceAccount](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee617202(v=technet.10)).



## <a name="see-also"></a><a name="BKMK_Links"></a>Consulte também

-   [Visão geral de contas de serviço gerenciado de grupo](group-managed-service-accounts-overview.md)