---
ms.assetid: 5a1ae56b-adcb-447e-9e34-c0629d7cb241
title: Configurar manualmente uma conta de serviço para um farm de servidores de federação
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b027bff4645203c44e228f11c651b767fa4502e0
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192060"
---
# <a name="manually-configure-a-service-account-for-a-federation-server-farm"></a>Configurar manualmente uma conta de serviço para um farm de servidores de federação

Se você pretende configurar um ambiente de farm de servidor de federação nos serviços de Federação do Active Directory \(do AD FS\), você deve criar e configurar uma conta de serviço dedicada no Active Directory Domain Services \(doADDS\) onde o farm residirá. Você deverá configurar cada servidor de federação no farm para usar esta conta. Você deve concluir as tarefas a seguir em sua organização quando você deseja permitir que computadores cliente na rede corporativa sejam autenticados para qualquer um dos servidores de Federação em um farm do AD FS usando a autenticação integrada do Windows.  

> [!IMPORTANT]
> A partir do AD FS 3.0 (Windows Server 2012 R2), o AD FS suporta o uso de um [conta de serviço gerenciado do grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) \(gMSA\) como a conta de serviço.  Isso é a opção recomendada, pois ele remove a necessidade de gerenciar a senha da conta de serviço ao longo do tempo.  Este documento aborda o caso alternativo do uso de uma conta de serviço tradicional, como em domínios ainda em execução um Windows Server 2008 R2 ou o nível funcional de domínio anterior \(DFL\).

> [!NOTE]  
> Você tem que executar as tarefas neste procedimento somente uma vez para todo o farm de servidores de federação. Posteriormente, quando você cria um servidor de Federação usando o Assistente de configuração do servidor de Federação do AD FS, você deve especificar essa mesma conta de **conta de serviço** página do assistente em cada servidor de federação no farm.  
  
#### <a name="create-a-dedicated-service-account"></a>Criar uma conta de serviço dedicada  
  
1.  Criar um usuário dedicado\/conta na floresta do Active Directory que está localizada na organização do provedor de identidade de serviço. Essa conta é necessária para o protocolo de autenticação de Kerberos funcione em um cenário de farm e para permitir a passagem de\-por meio da autenticação em cada um dos servidores de Federação. Use essa conta somente para fins de farm de servidores de Federação.  
  
2.  Edite as propriedades da conta do usuário e marque a caixa de seleção **A senha nunca expira**. Esta ação garante que a função da conta de serviço não seja interrompida devido aos requisitos de alteração de senha do domínio.  
  
    > [!NOTE]  
    > Usar a conta de Serviço de Rede para esta conta dedicada resultará em falhas aleatórias ao tentar acesso por meio da Autenticação Integrada do Windows, devido aos tíquetes do Kerberos não estarem sendo validados de um servidor para outro.  
  
#### <a name="to-set-the-spn-of-the-service-account"></a>Para definir o SPN da conta de serviço  
  
1.  Porque a identidade do pool de aplicativos para o AppPool do AD FS está em execução como um usuário de domínio\/conta de serviço você deve configurar o nome de entidade de serviço \(SPN\) para essa conta no domínio com o comando Setspn.exe\-ferramenta de linha. Setspn.exe é instalado por padrão em computadores que executam o Windows Server 2008. Execute o seguinte comando em um computador que ingressou no mesmo domínio onde o usuário\/reside a conta de serviço:  
  
    ```  
    setspn -a host/<server name> <service account>  
    ```  
  
    Por exemplo, em um cenário no qual federação todos os servidores estão clusterizados no sistema de nomes de domínio \(DNS\) nome de host fs.fabrikam.com e o nome da conta de serviço que é atribuído para o AppPool do AD FS é denominado adfs2farm, digite o comando como segue e pressione ENTER:  
  
    ```  
    setspn -a host/fs.fabrikam.com adfs2farm  
    ```  
  
    Será necessário executar esta tarefa somente uma vez para esta conta.  
  
2.  Depois que a identidade do AD FS AppPool é alterada para a conta de serviço, defina as listas de controle de acesso \(ACLs\) no banco de dados do SQL Server para permitir o acesso de leitura para essa nova conta para que o AD FS AppPool possa ler os dados de política.  
  

