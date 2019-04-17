---
title: "Introdução ao grupo de contas de serviço gerenciado"
description: "Segurança do Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7130ad73-9688-4f64-aca1-46a9187a46cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: cd1e92f93701e5b430d1425fcf9a2f3590d1fe5d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="getting-started-with-group-managed-service-accounts"></a>Introdução ao grupo de contas de serviço gerenciado

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016


Este guia fornece instruções passo a passo e informações de plano de fundo para ativar e usar contas de serviço gerenciado do grupo no Windows Server 2012.

**Neste documento**

-   [Pré-requisitos](#BKMK_Prereqs)

-   [Introdução](#BKMK_Intro)

-   [Implantando um novo farm de servidores](#BKMK_DeployNewFarm)

-   [Adicionando hosts de membro a um farm de servidores existente](#BKMK_AddMemberHosts)

-   [Atualizando as propriedades da conta de serviço gerenciado do grupo](#BKMK_Update_gMSA)

-   [Aposentadoria hosts de membro de um farm de servidores existente](#BKMK_DecommMemberHosts)


> [!NOTE]
> Este tópico inclui cmdlets do Windows PowerShell de exemplo que você pode usar para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [usando os Cmdlets do](https://go.microsoft.com/fwlink/p/?linkid=230693).

## <a name="BKMK_Prereqs"></a>Pré-requisitos
Consulte a seção neste tópico sobre [requisitos para contas de serviço gerenciado do grupo](#BKMK_gMSA_Req).

## <a name="BKMK_Intro"></a>Introdução
Quando um computador cliente se conecta a um serviço que está hospedado em um farm de servidores usando (NLB) de balanceamento de carga de rede ou algum outro método onde todos os servidores parecem ser o mesmo serviço para o cliente, em seguida, dando suporte à autenticação mútua, como Kerberos de protocolos de autenticação não podem ser usados, a menos que todas as instâncias dos serviços de usam a mesma entidade de segurança. Isso significa que cada serviço deve usar as mesmas senhas/chaves para provar sua identidade.

> [!NOTE]
> Clusters de failover não dão suporte a gMSAs. No entanto, os serviços que são executados sobre o serviço de Cluster podem usar um gMSA ou um sMSA se eles são um serviço do Windows, um pool de aplicativo, uma tarefa agendada ou suporte nativo para gMSA ou sMSA.

Serviços tem os seguintes objetos para escolha, e cada um tem algumas limitações.

|Entidades|Escopo|Serviços de suporte|Gerenciamento de senhas|
|-------|-----|-----------|------------|
|Sistema de computador a conta do Windows|Domínio|Limitado a um domínio ingressou server|Gerencia do computador|
|Conta de computador sem sistema Windows|Domínio|Qualquer servidor de domínio associado|None|
|Conta virtual|Local|Limitado a um servidor|Gerencia do computador|
|Autônomo do Windows 7 conta de serviço gerenciado|Domínio|Limitado a um domínio ingressou server|Gerencia do computador|
|Conta de usuário|Domínio|Qualquer servidor de domínio associado|None|
|Conta de grupo serviço gerenciado|Domínio|Qualquer servidor de domínio do Windows Server 2012|Gerencia o controlador de domínio e recupera o host|

Uma conta de computador do Windows ou um aplicativo de autônomo do Windows 7 conta de serviço gerenciado (sMSA), ou contas virtuais não podem ser compartilhadas entre vários sistemas. Se você configurar uma conta para serviços em farms de servidor para compartilhar, você teria de escolher uma conta de usuário ou uma conta de computador, além de um sistema Windows. De qualquer forma, essas contas não têm a capacidade de gerenciamento de senhas único ponto de controle. Isso cria um problema em que cada organização precisa criar uma solução cara para atualizar as teclas para o serviço no Active Directory e, em seguida, distribuir as teclas para todas as instâncias desses serviços.

Com o Windows Server 2012, serviços ou administradores de serviços não precisa gerenciar a sincronização de senhas entre as instâncias de serviço ao usar contas de serviço gerenciado do grupo (gMSA). Você provisione o gMSA no AD e, em seguida, configure o serviço que oferece suporte a contas de serviço gerenciado. Você pode provisionar um gMSA usando o *-cmdlets ADServiceAccount que fazem parte do módulo do Active Directory. Configuração do serviço identidade no host é compatível com:

-   Mesmas APIs como sMSA, portanto, os produtos que dão suporte sMSA dará suporte gMSA

-   Serviços que usam o Gerenciador de controle de serviço para configurar a identidade de logon

-   Serviços que usam o Gerenciador do IIS para pools de aplicativos para configurar identidade

-   Usando o Agendador de tarefas.

### <a name="BKMK_gMSA_Req"></a>Requisitos para contas de serviço gerenciado do grupo
A tabela a seguir lista os requisitos de sistema operacional para autenticação Kerberos trabalhar com serviços usando gMSA. Requisitos do Active Directory são listados após a tabela.

Uma arquitetura de 64 bits é necessária para executar os comandos do Windows PowerShell usados para administrar as contas de serviço gerenciado do grupo.

**Requisitos de sistema operacional**

|Elemento|Requisito|Sistema Operacional|
|------|--------|----------|
|Host de aplicativo de cliente|Cliente do RFC compatível Kerberos|Pelo menos Windows XP|
|Domínio de conta de usuário controladores de domínio|KDC compatível com RFC|Pelo menos Windows Server 2003|
|Hosts de membro de serviço compartilhado|| Windows Server 2012 |
|Domínio do host do membro controladores de domínio|KDC compatível com RFC|Pelo menos Windows Server 2003|
|domínio da conta gMSA controladores de domínio| Windows Server 2012 controladores de domínio disponível para o host recuperar a senha|Domínio com o Windows Server 2012, que pode ter alguns sistemas anteriores ao Windows Server 2012 |
|Host de serviço de back-end|Servidor de aplicativo do RFC compatível com Kerberos|Pelo menos Windows Server 2003|
|Domínio da conta de serviço de back-end controladores de domínio|KDC compatível com RFC|Pelo menos Windows Server 2003|
|Para o Active Directory do Windows PowerShell|Windows PowerShell para o Active Directory instalados localmente em um computador com suporte a uma arquitetura de 64 bits ou em seu computador de gerenciamento remoto (por exemplo, usando o Kit de ferramentas de administração de servidor remoto)| Windows Server 2012 |

**Requisitos do Active Directory Domain Service**

-   Esquema do Active Directory na floresta do domínio gMSA precisa ser atualizado para o Windows Server 2012 para criar um gMSA.

    Você pode atualizar o esquema instalando um controlador de domínio que executa o Windows Server 2012 ou executando a versão do adprep.exe em um computador que executa o Windows Server 2012. O valor do atributo de versão do objeto para o objeto CN = Schema, CN = Configuration, DC = Contoso, DC = Com deve ser 52.

-   Nova conta gMSA provisionada

-   Se você estiver gerenciando a permissão de host do serviço para usar gMSA por grupo e, em seguida, grupo de segurança novos ou existentes

-   Se gerenciar o controle de acesso de serviço por grupo e, em seguida, grupo de segurança novos ou existentes

-   Se a chave mestra raiz primeiro para o Active Directory não é implantada no domínio ou não tiver sido criada, crie-o. O resultado de sua criação pode ser verificado no log operacional KdsSvc, 4004 de ID de evento.

Para obter instruções de como criar a chave, consulte [criar a chave de raiz chave distribuição serviços KDS](create-the-key-distribution-services-kds-root-key.md). Chave de distribuição serviço Microsoft (kdssvc.dll) a chave raiz de anúncio.

**Ciclo de vida**

O ciclo de vida de um farm de servidores usando o recurso de gMSA normalmente envolve as seguintes tarefas:

-   Implantando um novo farm de servidores

-   Adicionando hosts de membro a um farm de servidores existente

-   Aposentadoria hosts de membro de um farm de servidores existente

-   Encerrar um farm de servidores existente

-   Removendo um host de membro comprometido de um farm de servidores, se necessário.

## <a name="BKMK_DeployNewFarm"></a>Implantando um novo farm de servidores
Ao implantar um novo farm de servidores, o administrador de serviço precisa determinar:

-   Se o serviço dá suporte ao uso gMSAs

-   Se o serviço exigir entradas ou saídas conexões autenticadas

-   Os nomes de conta de computador para os hosts de membro para o serviço usando o gMSA

-   O nome NetBIOS do serviço

-   O nome de host DNS para o serviço

-   Os nomes de entidade de serviço (SPNs) para o serviço

-   Intervalo de alterar a senha (o padrão é 30 dias).

### <a name="BKMK_Step1"></a>Etapa 1: Provisionamento de contas de serviço gerenciado do grupo
Você pode criar um gMSA somente se o esquema de floresta foi atualizado para o Windows Server 2012, a chave mestra raiz para o Active Directory tenha sido implantada e há pelo menos um Windows Server 2012 controlador de domínio no domínio em que o gMSA será criado.

A associação ao grupo **Admins. do domínio**, **operadores de conta** ou a capacidade de criar objetos msDS-GroupManagedServiceAccount, é o requisito mínimo para concluir os procedimentos a seguir.

#### <a name="BKMK_CreateGMSA"></a>Para criar um gMSA usando o cmdlet New-ADServiceAccount

1.  No controlador de domínio do Windows Server 2012, execute o Windows PowerShell na barra de tarefas.

2.  No prompt de comando para o Windows PowerShell, digite os seguintes comandos e pressione ENTER. (O módulo do Active Directory será carregado automaticamente.)

    **Novo ADServiceAccount [-Name] <string> - DNSHostName <string> [-KerberosEncryptionType <ADKerberosEncryptionType>] [-ManagedPasswordIntervalInDays < Nullable [Int32] >] [-PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >] - SamAccountName <string> - ServicePrincipalNames < cadeia de caracteres [] >**

    |Parâmetro|Cadeia de caracteres|Exemplo|
    |-------|-----|------|
    |Nome|Nome da conta|ITFarm1|
    |DNSHostName|Nome de host DNS do serviço|ITFarm1.contoso.com|
    |KerberosEncryptionType|Quaisquer tipos de criptografia compatíveis com os servidores de host|RC4, AES128, AES256|
    |ManagedPasswordIntervalInDays|Intervalo de alteração de senha em dias (padrão é 30 dias se não for fornecido)|90|
    |PrincipalsAllowedToRetrieveManagedPassword|As contas de computador dos hosts de membro ou que hospeda o membro for um membro do grupo de segurança|ITFarmHosts|
    |SamAccountName|Nome NetBIOS para o serviço, se não igual ao nome|ITFarm1|
    |ServicePrincipalNames|Nomes de entidade de serviço (SPNs) para o serviço|http/ITFarm1.contoso.com/contoso.com, http/ITFarm1.contoso.com/contoso, http/ITFarm1/contoso.com, ITFarm1/http/contoso|

    > [!IMPORTANT]
    > O intervalo de alteração de senha só pode ser definido durante a criação. Se você precisar alterar o intervalo, você deve criar um novo gMSA e configurá-lo no momento da criação.

    **Exemplo**

    Insira o comando em uma única linha, mesmo que elas podem aparentar quebras automáticas em várias linhas devido a restrições de formatação.

    ```
    New-ADServiceAccount ITFarm1 -DNSHostName ITFarm1.contoso.com -PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts -KerberosEncryptionType RC4, AES128, AES256 -ServicePrincipalNames http/ITFarm1.contoso.com/contoso.com, http/ITFarm1.contoso.com/contoso, http/ITFarm1/contoso.com, http/ITFarm1/contoso

    ```

A associação ao grupo **Admins. do domínio**, **operadores de conta**, ou a capacidade de criar objetos msDS-GroupManagedServiceAccount, é o requisito mínimo para concluir este procedimento. Para obter informações detalhadas sobre como usar as contas apropriadas e associações de grupo, consulte [Local e os grupos de domínio padrão](https://technet.microsoft.com/library/dd728026(WS.10).aspx).

##### <a name="to-create-a-gmsa-for-outbound-authentication-only-using-the-new-adserviceaccount-cmdlet"></a>Para criar um gMSA para autenticação de saída somente usando o cmdlet New-ADServiceAccount

1.  No controlador de domínio do Windows Server 2012, execute o Windows PowerShell na barra de tarefas.

2.  No prompt de comando para o módulo do Active Directory do Windows PowerShell, digite os seguintes comandos e pressione ENTER:

    **Novo ADServiceAccount [-Name] <string> - RestrictToOutboundAuthenticationOnly [-ManagedPasswordIntervalInDays < anulável [Int32] >] [-PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >]**

    |Parâmetro|Cadeia de caracteres|Exemplo|
    |-------|-----|------|
    |Nome|Nome da conta|ITFarm1|
    |ManagedPasswordIntervalInDays|Intervalo de alteração de senha em dias (padrão é 30 dias se não for fornecido)|75|
    |PrincipalsAllowedToRetrieveManagedPassword|As contas de computador dos hosts de membro ou que hospeda o membro for um membro do grupo de segurança|ITFarmHosts|

    > [!IMPORTANT]
    > O intervalo de alteração de senha só pode ser definido durante a criação. Se você precisar alterar o intervalo, você deve criar um novo gMSA e configurá-lo no momento da criação.

**Exemplo**

```
New-ADServiceAccount ITFarm1 -RestrictToOutboundAuthenticationOnly - PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts

```

### <a name="BKMK_ConfigureServiceIdentity"></a>Etapa 2: Configurar o serviço do serviço identidade do aplicativo
Para configurar os serviços no Windows Server 2012, consulte a documentação de recursos a seguir:

-   Pool de aplicativos do IIS

    Para obter mais informações, consulte [especificar uma identidade para um Pool de aplicativos (IIS 7)](https://technet.microsoft.com/library/cc771170(WS.10).aspx).

-   Serviços do Windows

    Para obter mais informações, consulte [serviços](https://technet.microsoft.com/library/cc772408.aspx).

-   Tarefas

    Para obter mais informações, consulte o [visão geral do Agendador de tarefas](https://technet.microsoft.com/library/cc721871.aspx).

Outros serviços podem dar suporte a gMSA. Consulte a documentação do produto apropriada para obter detalhes sobre como configurar esses serviços.

## <a name="BKMK_AddMemberHosts"></a>Adicionando hosts de membro a um farm de servidores existente
Se usando grupos de segurança para o gerenciamento de hosts de membro, adicione a conta de computador para o novo host de membro para o grupo de segurança (hosts de membro do gMSA seja membro dessa) usando um dos métodos a seguir.

A associação ao grupo **Admins. do domínio**, ou a capacidade de adicionar membros para o objeto de grupo de segurança, é o requisito mínimo para concluir esses procedimentos.

-   Método 1: Os usuários do Active Directory e computadores

    Para obter os procedimentos como usá-lo, consulte [adicionar uma conta de computador a um grupo](https://technet.microsoft.com/library/cc733097.aspx) usando a interface do Windows, e [gerenciar domínios diferentes no Centro Administrativo do Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

-   Método 2: dsmod

    Para obter os procedimentos como usá-lo, consulte [adicionar uma conta de computador a um grupo](https://technet.microsoft.com/library/cc733097.aspx) usando a linha de comando.

-   Método 3: Active Directory do Windows PowerShell cmdlet Add-ADPrincipalGroupMembership

    Para obter os procedimentos como usá-lo, consulte [adicionar ADPrincipalGroupMembership](https://technet.microsoft.com/library/ee617203.aspx).

Se usar contas de computador, encontrar as contas existentes e, em seguida, adicione a nova conta de computador.

A associação ao grupo **Admins. do domínio**, **operadores de conta**, ou a capacidade de gerenciar objetos msDS-GroupManagedServiceAccount, é o requisito mínimo para concluir este procedimento. Para obter informações detalhadas sobre como usar as contas apropriadas e associações de grupo, consulte Local e os grupos de domínio padrão.

#### <a name="to-add-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>Para adicionar hosts membro usando o cmdlet Set-ADServiceAccount

1.  No controlador de domínio do Windows Server 2012, execute o Windows PowerShell na barra de tarefas.

2.  No prompt de comando para o módulo do Active Directory do Windows PowerShell, digite os seguintes comandos e pressione ENTER:

    **Get-ADServiceAccount [-Name] <string> - PrincipalsAllowedToRetrieveManagedPassword**

3.  No prompt de comando para o módulo do Active Directory do Windows PowerShell, digite os seguintes comandos e pressione ENTER:

    **Conjunto ADServiceAccount [-Name] <string> - PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >**

|Parâmetro|Cadeia de caracteres|Exemplo|
|-------|-----|------|
|Nome|Nome da conta|ITFarm1|
|PrincipalsAllowedToRetrieveManagedPassword|As contas de computador dos hosts de membro ou que hospeda o membro for um membro do grupo de segurança|Host1, Host2, Host3|

**Exemplo**

Por exemplo, Adicionar membro hosts digite os seguintes comandos e pressione ENTER.

```
Get-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword

```

```
Set-ADServiceAccount [-Name] ITFarm1-PrincipalsAllowedToRetrieveManagedPassword Host1 Host2 Host3

```

## <a name="BKMK_Update_gMSA"></a>Atualizando as propriedades de conta de serviço gerenciado do grupo
A associação ao grupo **Admins. do domínio**, **operadores de conta**, ou a capacidade de gravar em objetos msDS-GroupManagedServiceAccount, é o requisito mínimo para concluir esses procedimentos.

Abra o Active Directory módulo para Windows PowerShell e definir qualquer propriedade usando o cmdlet Set-ADServiceAccount.

Para saber mais como definir essas propriedades, consulte [conjunto ADServiceAccount](https://technet.microsoft.com/library/ee617252.aspx) na biblioteca do TechNet ou digitando **Get-Help conjunto-ADServiceAccount** no módulo do Active Directory para Windows PowerShell comando prompt e pressionando ENTER.

## <a name="BKMK_DecommMemberHosts"></a>Aposentadoria hosts de membro de um farm de servidores existente
A associação ao grupo **Admins. do domínio**, ou a capacidade de remover membros do objeto de grupo de segurança, é o requisito mínimo para concluir esses procedimentos.

### <a name="step-1-remove-member-host-from-gmsa"></a>Etapa 1: Remover membro host de gMSA
Se usando grupos de segurança para o gerenciamento de hosts de membro, remova a conta de computador para o host descomissionados membro do grupo de segurança que hosts de membro do gMSA serão um participante do usando qualquer um dos métodos a seguir.

-   Método 1: Os usuários do Active Directory e computadores

    Para obter os procedimentos como usá-lo, consulte [excluir uma conta de computador](https://technet.microsoft.com/library/cc754624.aspx) usando a interface do Windows, e [gerenciar domínios diferentes no Centro Administrativo do Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

-   Método 2: drsm

    Para obter os procedimentos como usá-lo, consulte [excluir uma conta de computador](https://technet.microsoft.com/library/cc754624.aspx) usando a linha de comando.

-   Método 3: Active Directory do Windows PowerShell cmdlet Remove-ADPrincipalGroupMembership

    Para saber mais como fazer isso, consulte [remover ADPrincipalGroupMembership](https://technet.microsoft.com/library/ee617243.aspx) na biblioteca do TechNet ou digitando **Get-Help Remove-ADPrincipalGroupMembership** no módulo do Active Directory para Windows PowerShell comando prompt e pressionando ENTER.

Se listando contas de computador, recuperar as contas existentes e, em seguida, adicione todas menos a conta de computador removido.

A associação ao grupo **Admins. do domínio**, **operadores de conta**, ou a capacidade de gerenciar objetos msDS-GroupManagedServiceAccount, é o requisito mínimo para concluir este procedimento. Para obter informações detalhadas sobre como usar as contas apropriadas e associações de grupo, consulte Local e os grupos de domínio padrão.

##### <a name="to-remove-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>Para remover membro hospeda usando o cmdlet Set-ADServiceAccount

1.  No controlador de domínio do Windows Server 2012, execute o Windows PowerShell na barra de tarefas.

2.  No prompt de comando para o módulo do Active Directory do Windows PowerShell, digite os seguintes comandos e pressione ENTER:

    **Get-ADServiceAccount [-Name] <string> - PrincipalsAllowedToRetrieveManagedPassword**

3.  No prompt de comando para o módulo do Active Directory do Windows PowerShell, digite os seguintes comandos e pressione ENTER:

    **Conjunto ADServiceAccount [-Name] <string> - PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >**

|Parâmetro|Cadeia de caracteres|Exemplo|
|-------|-----|------|
|Nome|Nome da conta|ITFarm1|
|PrincipalsAllowedToRetrieveManagedPassword|As contas de computador dos hosts de membro ou que hospeda o membro for um membro do grupo de segurança|Host1, Host3|

**Exemplo**

Por exemplo, para remover membro hosts digite os seguintes comandos e pressione ENTER.

```
Get-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword

```

```
Set-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword Host1 Host3

```

### <a name="BKMK_RemoveGMSA"></a>Etapa 2: Remoção de um grupo de conta de serviço gerenciado do sistema
Remova as credenciais armazenadas em cache gMSA do host membro usando desinstalar ADServiceAccount ou a API NetRemoveServiceAccount no sistema host.

A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para concluir esses procedimentos.

##### <a name="to-remove-a-gmsa-using-the-uninstall-adserviceaccount-cmdlet"></a>Para remover um gMSA usando o cmdlet desinstalar ADServiceAccount

1.  No controlador de domínio do Windows Server 2012, execute o Windows PowerShell na barra de tarefas.

2.  No prompt de comando para o módulo do Active Directory do Windows PowerShell, digite os seguintes comandos e pressione ENTER:

    **Desinstale-ADServiceAccount < ADServiceAccount >**

    **Exemplo**

    Por exemplo, para remover as credenciais armazenadas em cache para um gMSA ITFarm1 nomeado digite o seguinte comando e pressione ENTER:

    ```
    Uninstall-ADServiceAccount ITFarm1
    ```

Para obter mais informações sobre o cmdlet desinstalar ADServiceAccount, em que o módulo do Active Directory para o prompt de comando do Windows PowerShell, digite **Get-Help desinstalar-ADServiceAccount**e pressione ENTER, ou consulte as informações na web TechNet em [desinstalar ADServiceAccount](https://technet.microsoft.com/library/ee617202.aspx).



## <a name="BKMK_Links"></a>Consulte também

-   [Visão geral de contas de serviço gerenciado grupo](group-managed-service-accounts-overview.md)



