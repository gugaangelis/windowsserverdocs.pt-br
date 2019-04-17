---
ms.assetid: 5a1ae56b-adcb-447e-9e34-c0629d7cb241
title: "Configurar uma conta de serviço manualmente para um Farm de servidores de Federação"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5b5a8d198f93772903ea9b0a2b4b01075799bf0f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="manually-configure-a-service-account-for-a-federation-server-farm"></a>Configurar uma conta de serviço manualmente para um Farm de servidores de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se você pretende configurar um ambiente de farm de servidor de Federação em \(AD FS\) serviços de Federação do Active Directory, você deve criar e configurar uma conta de serviço dedicado no Active Directory Domain Services \(AD DS\) onde ficará o farm. Você então configurar cada servidor de federação no farm para usar essa conta. Você deve concluir as tarefas a seguir em sua organização quando você deseja permitir que cliente computadores na rede corporativa para autenticar qualquer um dos servidores de Federação em um farm de AD FS usando a autenticação integrada do Windows.  
  
> [!NOTE]  
> Você precisa executar as tarefas neste procedimento somente uma vez para o farm de servidores de Federação inteiro. Mais tarde, quando você cria um servidor de Federação usando o Assistente para configuração do servidor de Federação do AD FS, você deve especificar essa mesma conta no **conta de serviço** página do assistente em cada servidor de federação no farm.  
  
#### <a name="create-a-dedicated-service-account"></a>Criar uma conta de serviço dedicado  
  
1.  Crie uma conta de serviço/user\ dedicada na floresta do Active Directory está localizada na organização de provedor de identidade. Essa conta é necessária para o protocolo de autenticação Kerberos para trabalhar em um cenário de farm e permitem a autenticação por meio do pass\ em cada um dos servidores de Federação. Use esta conta apenas para fins de farm do servidor de Federação.  
  
2.  Editar as propriedades da conta de usuário e selecione o **senha nunca expira** caixa de seleção. Isso garante que a função da conta de serviço não seja interrompida devido a requisitos de alteração de senha de domínio.  
  
    > [!NOTE]  
    > Usando a conta de serviço de rede para essa conta dedicada resultará em falhas aleatórias quando o acesso é tentado por meio de autenticação integrada do Windows, como resultado de tíquetes Kerberos não Validando de um servidor para outro.  
  
#### <a name="to-set-the-spn-of-the-service-account"></a>Para definir o SPN da conta de serviço  
  
1.  Porque a identidade do pool de aplicativos para o AD FS AppPool está em execução como uma conta de domínio user\/serviço, você deve configurar \(SPN\) o nome Principal do serviço para a conta do domínio com a ferramenta de linha de command\ Setspn.exe. Setspn.exe é instalada por padrão em computadores que executam o Windows Server 2008. Execute o seguinte comando em um computador que tenha ingressado no mesmo domínio em que reside a conta de serviço/user\:  
  
    ```  
    setspn -a host/<server name> <service account>  
    ```  
  
    Por exemplo, em um cenário em que todos os servidores de Federação agrupados sob o nome de host do sistema de nomes de domínio \(DNS\) fs.fabrikam.com e o nome da conta de serviço que é atribuído para o AD FS AppPool é denominado adfs2farm, digite o comando a seguinte e pressione ENTER:  
  
    ```  
    setspn -a host/fs.fabrikam.com adfs2farm  
    ```  
  
    É necessário concluir essa tarefa somente uma vez para essa conta.  
  
2.  Depois que a identidade do AD FS AppPool é alterada para a conta de serviço, defina o acesso controle listas \(ACLs\) no banco de dados SQL Server para permitir o acesso de leitura para essa nova conta para que o AD FS AppPool possa ler os dados de política.  
  

