---
ms.assetid: 6ecf8d85-cd61-4c87-add8-00a679a6e3ff
title: Adicionar um servidor de federação a um farm de servidores de federação
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 040caf6395b7c70313de900d522241f97699a999
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192497"
---
# <a name="add-a-federation-server-to-a-federation-server-farm"></a>Adicionar um servidor de federação a um farm de servidores de federação


Depois de instalar o serviço de função serviço de Federação e configurar os certificados necessários em um computador, você está pronto para configurar o computador para se tornar um servidor de Federação. Use o procedimento a seguir para ingressar um computador em um novo farm de servidores de federação.  
  
Ingressar um computador a um farm com o Assistente de configuração do servidor de Federação do AD FS. Quando você usa este assistente para ingressar um computador a um farm existente, o computador está configurado com uma leitura\-somente cópia do banco de dados de configuração do AD FS e ele deve receber atualizações de um servidor de Federação primário.  
  
> [!NOTE]  
> Para um único de Web federado\-sinal\-na \(SSO\) design, você deve ter pelo menos um servidor de federação na organização do parceiro de conta e pelo menos um servidor de federação na organização do parceiro de recurso . Para obter mais informações, consulte [Onde colocar um servidor de federação](https://technet.microsoft.com/library/dd807127.aspx).  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-federation-server-to-a-federation-server-farm"></a>Para adicionar um servidor de Federação a um farm de servidores de Federação  
  
1.  Há duas maneiras de iniciar o Assistente de configuração do servidor de Federação do AD FS. Para iniciar o assistente, tome uma das seguintes ações:  
  
    -   Após a instalação do serviço de função serviço de Federação for concluída, abra o snap de gerenciamento do AD FS\-em e clique em de **Assistente de configuração do servidor de Federação do AD FS** no link a **visão geral** página ou o **ações** painel.  
  
    -   A qualquer momento depois que o Assistente de instalação for concluída, abra o Windows Explorer, navegue até a **c:\\Windows\\ADFS** pasta e double\-clique **FsConfigWizard.exe**.  
  
2.  Na página **Bem-vindo**, verifique se a opção **Adicionar um servidor de federação a um Serviço de Federação existente** está selecionada e clique em **Avançar**.  
  
3.  Se o banco de dados do AD FS que você selecionou já existir, o **banco de dados existente do AD FS Configuration detectado** página será exibida. Se isso ocorrer, clique em **Excluir banco de dados** e clique em **Avançar**.  
  
    > [!CAUTION]  
    > Selecione esta opção apenas quando tiver certeza de que os dados neste banco de dados do AD FS não são importantes ou que não é usado em um farm de servidores de federação de produção.  
  
4.  Na página **Especificar o Servidor de Federação Primário e a Conta de Serviço**, em **Nome do servidor de federação primário**, digite o nome do computador do servidor de federação primário no farm e clique em **Procurar**. Na caixa de diálogo **Procurar**, localize a conta de domínio que será usada como a conta de serviço por todos os outros servidores de federação no farm de servidores de federação existente e clique em **OK**. Digite a senha e confirmá-la e, em seguida, clique em **próxima**:  
  
    > [!NOTE]  
    > Para obter mais informações sobre como especificar uma conta de serviço para um farm de servidores de federação, consulte [configurar manualmente uma conta de serviço para um Farm de servidores de Federação](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md). Cada servidor de federação no farm de servidores de federação deve especificar a mesma conta de serviço para o farm fique operacional. Por exemplo, se a conta de serviço que foi criada era contoso\\ADFS2SVC, cada computador que você configurar para a função de servidor de Federação e que participará no mesmo farm deve especificar contoso\\ADFS2SVC isso entrar a Federação Server Assistente de configuração para o farm fique operacional.  
  
5.  Na página **Pronto para Aplicar Configurações**, examine os detalhes. Se as configurações parecerem corretas, clique em **próxima** para começar a configurar o AD FS com essas configurações.  
  
6.  Na página **Resultados da Configuração**, examine os resultados. Quando todas as etapas de configuração estiverem concluídas, clique em **fechar** para sair do assistente.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Como configurar um servidor de federação](Checklist--Setting-Up-a-Federation-Server.md)  
  

