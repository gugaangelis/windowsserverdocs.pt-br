---
ms.assetid: 5a1ae56b-adcb-447e-9e34-c0629d7cb241
title: Configurar manualmente uma conta de serviço para um farm de servidores de federação
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8240903b3c446d4f02ca93dc053e520480f5e8ca
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359494"
---
# <a name="manually-configure-a-service-account-for-a-federation-server-farm"></a>Configurar manualmente uma conta de serviço para um farm de servidores de federação

Se você pretende configurar um ambiente de farm de servidores de Federação em Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1, você deve criar e configurar uma conta de serviço dedicada no Active Directory Domain Services \(AD DS @ no__t-3 em que o farm residirá. Você deverá configurar cada servidor de federação no farm para usar esta conta. Você deve concluir as seguintes tarefas em sua organização quando desejar permitir que computadores cliente na rede corporativa se autentiquem em qualquer um dos servidores de Federação em um farm de AD FS usando a autenticação integrada do Windows.  

> [!IMPORTANT]
> A partir do AD FS 3,0 (Windows Server 2012 R2), AD FS dá suporte ao uso de uma [conta de serviço gerenciado de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) \(gMSA @ no__t-2 como a conta de serviço.  Essa é a opção recomendada, pois elimina a necessidade de gerenciar a senha da conta de serviço ao longo do tempo.  Este documento aborda o caso alternativo de usar uma conta de serviço tradicional, como em domínios que ainda executam um nível funcional de domínio do Windows Server 2008 R2 ou anterior \(DFL @ no__t-1.

> [!NOTE]  
> Você tem que executar as tarefas neste procedimento somente uma vez para todo o farm de servidores de federação. Posteriormente, ao criar um servidor de Federação usando o assistente de configuração do servidor de Federação AD FS, você deverá especificar essa mesma conta na página do assistente de **conta de serviço** em cada servidor de Federação no farm.  
  
#### <a name="create-a-dedicated-service-account"></a>Criar uma conta de serviço dedicada  
  
1.  Crie uma conta de usuário dedicado @ no__t-0service na floresta Active Directory que está localizada na organização do provedor de identidade. Essa conta é necessária para que o protocolo de autenticação Kerberos funcione em um cenário de farm e permita a autenticação Pass @ no__t-0through em cada um dos servidores de Federação. Use essa conta somente para os fins do farm de servidores de Federação.  
  
2.  Edite as propriedades da conta do usuário e marque a caixa de seleção **A senha nunca expira**. Esta ação garante que a função da conta de serviço não seja interrompida devido aos requisitos de alteração de senha do domínio.  
  
    > [!NOTE]  
    > Usar a conta de Serviço de Rede para esta conta dedicada resultará em falhas aleatórias ao tentar acesso por meio da Autenticação Integrada do Windows, devido aos tíquetes do Kerberos não estarem sendo validados de um servidor para outro.  
  
#### <a name="to-set-the-spn-of-the-service-account"></a>Para definir o SPN da conta de serviço  
  
1.  Como a identidade do pool de aplicativos para o AD FS AppPool está sendo executada como uma conta de usuário do domínio @ no__t-0service, você deve configurar o nome da entidade de serviço \(SPN @ no__t-2 para essa conta no domínio com a ferramenta de comando SetSPN. exe @ no__t-3line. O Setspn. exe é instalado por padrão em computadores que executam o Windows Server 2008. Execute o seguinte comando em um computador que tenha ingressado no mesmo domínio em que a conta usuário @ no__t-0service reside:  
  
    ```  
    setspn -a host/<server name> <service account>  
    ```  
  
    Por exemplo, em um cenário no qual todos os servidores de Federação estão clusterizados no sistema de nomes de domínio \(DNS @ no__t-1 nome do host fs.fabrikam.com e o nome da conta de serviço atribuída ao AD FS AppPool é denominado adfs2farm, digite o comando da seguinte maneira e em seguida, pressione ENTER:  
  
    ```  
    setspn -a host/fs.fabrikam.com adfs2farm  
    ```  
  
    Será necessário executar esta tarefa somente uma vez para esta conta.  
  
2.  Depois que a identidade de AppPool AD FS for alterada para a conta de serviço, defina as listas de controle de acesso \(ACLs @ no__t-1 no banco de dados SQL Server para permitir o acesso de leitura a essa nova conta, de forma que o AppPool AD FS possa ler os dados da política.  
  

