---
ms.assetid: 6ecf8d85-cd61-4c87-add8-00a679a6e3ff
title: Adicionar um servidor de federação a um farm de servidores de federação
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 57c59241c36ba4ed657b83e7c691931f57978a7c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408524"
---
# <a name="add-a-federation-server-to-a-federation-server-farm"></a>Adicionar um servidor de federação a um farm de servidores de federação


Depois de instalar o Serviço de Federação serviço de função e configurar os certificados necessários em um computador, você estará pronto para configurar o computador para se tornar um servidor de Federação. Use o procedimento a seguir para ingressar um computador em um novo farm de servidores de federação.  
  
Você adiciona um computador a um farm com o assistente de configuração do servidor de Federação AD FS. Quando você usa esse assistente para ingressar um computador em um farm existente, o computador é configurado com uma cópia @ no__t-0only de leitura do banco de dados de configuração do AD FS e deve receber atualizações de um servidor de Federação primário.  
  
> [!NOTE]  
> Para o design da Web federado @ no__t-0Sign @ no__t-1On \(SSO @ no__t-3, você deve ter pelo menos um servidor de Federação na organização do parceiro de conta e pelo menos um servidor de Federação na organização do parceiro de recurso. Para obter mais informações, consulte [Onde colocar um servidor de federação](https://technet.microsoft.com/library/dd807127.aspx).  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examinar os detalhes sobre como usar as contas apropriadas e as associações de grupo em \( [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477) http:\/\/go.Microsoft.com\/fwlink\/? \=LinkId 83477.\)   
  
### <a name="to-add-a-federation-server-to-a-federation-server-farm"></a>Para adicionar um servidor de Federação a um farm de servidores de Federação  
  
1.  Há duas maneiras de iniciar o assistente de configuração do servidor de Federação AD FS. Para iniciar o assistente, tome uma das seguintes ações:  
  
    -   Depois que a instalação do serviço de função Serviço de Federação for concluída, abra o AD FS Management snap @ no__t-0in e clique no link do **Assistente de configuração do servidor de federação AD FS** na página **visão geral** ou no painel **ações** .  
  
    -   A qualquer momento depois que o assistente de instalação for concluído, abra o Windows Explorer, navegue até a pasta **C: \\Windows @ no__t-2ADFS** e clique duas vezes em @ no__t-3Click **FsConfigWizard. exe**.  
  
2.  Na página **Bem-vindo**, verifique se a opção **Adicionar um servidor de federação a um Serviço de Federação existente** está selecionada e clique em **Avançar**.  
  
3.  Se o banco de dados de AD FS que você selecionou já existir, a página **banco de dados de configuração AD FS existente detectado** será exibida. Se isso ocorrer, clique em **Excluir banco de dados** e clique em **Avançar**.  
  
    > [!CAUTION]  
    > Selecione esta opção somente quando você tiver certeza de que os dados nesse banco de dado AD FS não são importantes ou que não são usados em um farm de servidores de Federação de produção.  
  
4.  Na página **Especificar o Servidor de Federação Primário e a Conta de Serviço**, em **Nome do servidor de federação primário**, digite o nome do computador do servidor de federação primário no farm e clique em **Procurar**. Na caixa de diálogo **Procurar**, localize a conta de domínio que será usada como a conta de serviço por todos os outros servidores de federação no farm de servidores de federação existente e clique em **OK**. Digite a senha e confirme-a e clique em **Avançar**:  
  
    > [!NOTE]  
    > Para obter mais informações sobre como especificar uma conta de serviço para um farm de servidores de Federação, consulte [configurar manualmente uma conta de serviço para um farm de servidores de Federação](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md). Cada servidor de Federação no farm de servidores de Federação deve especificar a mesma conta de serviço para o farm estar operacional. Por exemplo, se a conta de serviço criada foi contoso @ no__t-0ADFS2SVC, cada computador configurado para a função de servidor de Federação e que participará do mesmo farm deverá especificar contoso @ no__t-1ADFS2SVC nesta etapa no servidor de Federação Assistente de configuração para o farm estar operacional.  
  
5.  Na página **Pronto para Aplicar Configurações**, examine os detalhes. Se as configurações parecerem corretas, clique em **Avançar** para começar a configurar AD FS com essas configurações.  
  
6.  Na página **Resultados da Configuração**, examine os resultados. Quando todas as etapas de configuração forem concluídas, clique em **fechar** para sair do assistente.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Como configurar um servidor de federação](Checklist--Setting-Up-a-Federation-Server.md)  
  

