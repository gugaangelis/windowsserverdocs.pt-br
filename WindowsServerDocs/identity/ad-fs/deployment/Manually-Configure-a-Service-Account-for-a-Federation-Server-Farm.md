---
ms.assetid: 5a1ae56b-adcb-447e-9e34-c0629d7cb241
title: Configurar manualmente uma conta de serviço para um farm de servidores de federação
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5d0495d43eecd2508a6535411ef4e226db0ebacf
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86964788"
---
# <a name="manually-configure-a-service-account-for-a-federation-server-farm"></a>Configurar manualmente uma conta de serviço para um farm de servidores de federação

Se você pretende configurar um ambiente de farm de servidores de Federação no Serviços de Federação do Active Directory (AD FS) \( AD FS \) , você deve criar e configurar uma conta de serviço dedicada no Active Directory Domain Services \( AD DS \) em que o farm residirá. Você deverá configurar cada servidor de federação no farm para usar esta conta. Você deve concluir as seguintes tarefas em sua organização quando desejar permitir que computadores cliente na rede corporativa se autentiquem em qualquer um dos servidores de Federação em um farm de AD FS usando a autenticação integrada do Windows.  

> [!IMPORTANT]
> A partir do AD FS 3,0 (Windows Server 2012 R2), AD FS dá suporte ao uso de uma [conta de serviço gerenciado de grupo](../../../security/group-managed-service-accounts/group-managed-service-accounts-overview.md) \( gMSA \) como a conta de serviço.  Essa é a opção recomendada, pois elimina a necessidade de gerenciar a senha da conta de serviço ao longo do tempo.  Este documento aborda o caso alternativo de usar uma conta de serviço tradicional, como em domínios que ainda executam um nível funcional de domínio do Windows Server 2008 R2 ou anterior \( DFL \) .

> [!NOTE]  
> Você tem que executar as tarefas neste procedimento somente uma vez para todo o farm de servidores de federação. Posteriormente, quando criar um servidor de federação usando o Assistente de Configuração do Servidor de Federação do AD FS, deverá especificar esta mesma conta na página do assistente **Conta de Serviço** em cada servidor de federação no farm.  
  
#### <a name="create-a-dedicated-service-account"></a>Criar uma conta de serviço dedicada  
  
1.  Crie uma conta de \/ serviço de usuário dedicada na floresta Active Directory que está localizada na organização do provedor de identidade. Essa conta é necessária para que o protocolo de autenticação Kerberos funcione em um cenário de farm e permita a \- autenticação de passagem em cada um dos servidores de Federação. Use essa conta somente para os fins do farm de servidores de Federação.  
  
2.  Edite as propriedades da conta do usuário e selecione a caixa de seleção **A senha nunca expira**. Esta ação garante que a função da conta de serviço não seja interrompida devido aos requisitos de alteração de senha do domínio.  
  
    > [!NOTE]  
    > Usar a conta de Serviço de Rede para esta conta dedicada resultará em falhas aleatórias ao tentar acesso por meio da Autenticação Integrada do Windows, devido aos tíquetes do Kerberos não estarem sendo validados de um servidor para outro.  
  
#### <a name="to-set-the-spn-of-the-service-account"></a>Para definir o SPN da conta de serviço  
  
1.  Como a identidade do pool de aplicativos para o AD FS AppPool está sendo executado como uma conta de serviço de usuário de domínio \/ , você deve configurar o SPN do nome da entidade \( \) de serviço para essa conta no domínio com a ferramenta de linha de comando Setspn.exe \- . O Setspn.exe é instalado por padrão em computadores que executam o Windows Server 2008. Execute o seguinte comando em um computador que tenha ingressado no mesmo domínio em que a \/ conta de serviço do usuário reside:  
  
    ```  
    setspn -a host/<server name> <service account>  
    ```  
  
    Por exemplo, em um cenário no qual todos os servidores de Federação estão clusterizados sob o nome de domínio DNS do sistema de nomes de domínios \( \) FS.fabrikam.com e o nome da conta de serviço que é atribuído ao AD FS AppPool é chamado de adfs2farm, digite o comando da seguinte maneira e pressione ENTER:  
  
    ```  
    setspn -a host/fs.fabrikam.com adfs2farm  
    ```  
  
    Será necessário executar esta tarefa somente uma vez para esta conta.  
  
2.  Depois que a identidade de AppPool AD FS for alterada para a conta de serviço, defina as ACLs de listas de controle de acesso \( \) no banco de dados SQL Server para permitir o acesso de leitura a essa nova conta, de modo que o AppPool AD FS possa ler os dados da política.  
  
