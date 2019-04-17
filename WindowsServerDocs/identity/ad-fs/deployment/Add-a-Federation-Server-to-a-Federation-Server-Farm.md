---
ms.assetid: 6ecf8d85-cd61-4c87-add8-00a679a6e3ff
title: "Adicionar um servidor de federação para um Farm de servidores de Federação"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d67f4c252ad25a05f11b88771f12fd01d13137d4
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-federation-server-to-a-federation-server-farm"></a>Adicionar um servidor de federação para um Farm de servidores de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Depois de instalar o serviço de função do serviço de Federação e configurar os certificados necessários em um computador, você estará pronto para configurar o computador para se tornar um servidor de Federação. Você pode usar o procedimento a seguir para ingressar um computador em um novo farm de servidores de Federação.  
  
Ingressar um computador para um farm com o Assistente para configuração do servidor de Federação do AD FS. Quando você usar esse assistente para ingressar em um computador a um farm existente, o computador está configurado com uma cópia somente read\ do banco de dados de configuração do AD FS e ele deve receber atualizações de um servidor de Federação principal.  
  
> [!NOTE]  
> Para o design \(SSO\) federados Single\-Sign\-On da Web, você deve ter pelo menos um servidor de federação na organização do parceiro de conta e pelo menos um servidor de federação na organização do parceiro de recurso. Para obter mais informações, consulte [onde colocar um servidor de Federação](https://technet.microsoft.com/library/dd807127.aspx).  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-add-a-federation-server-to-a-federation-server-farm"></a>Para adicionar um servidor de Federação a um farm de servidores de Federação  
  
1.  Há duas maneiras de iniciar o Assistente para configuração do servidor de Federação do AD FS. Para iniciá-lo, siga um destes procedimentos:  
  
    -   Após a instalação de serviço de função do serviço de Federação for concluída, abra o AD FS snap\-in Gerenciamento e clique no **Assistente de configuração de servidor de Federação do AD FS** vincular no **visão geral** página ou no **ações** painel.  
  
    -   A qualquer momento depois que o Assistente para instalação for concluída, abra o Windows Explorer, navegue até o **C:\\Windows\\ADFS** pasta e clique em double\ **FsConfigWizard.exe**.  
  
2.  No **boas-vindas** página, verifique **adicionar um servidor de federação para um serviço de Federação existente** está selecionado e clique em **próxima**.  
  
3.  Se o banco de dados do AD FS que você selecionou já existir, o **existente AD FS configuração do banco de dados detectado** página será exibida. Se isso ocorrer, clique em **banco de dados de excluir**e clique em **próxima**.  
  
    > [!CAUTION]  
    > Selecione essa opção somente quando tiver certeza de que os dados neste banco de dados do AD FS não são importantes ou que não é usado em um farm de servidores de federação de produção.  
  
4.  Sobre o **especificar o servidor de Federação primário e a conta de serviço** página, em **nome do servidor de Federação principal**, digite o nome do computador do servidor de Federação principal no farm e, em seguida, clique em **procurar**. No **procurar** caixa de diálogo, localize a conta de domínio que é usada como a conta de serviço por todos os outros servidores de federação no farm de servidores de Federação existente e clique em **Okey**. Digite a senha e confirmá-la e, em seguida, clique em **próxima**:  
  
    > [!NOTE]  
    > Para obter mais informações sobre como especificar uma conta de serviço para um farm de servidores de federação, consulte [manualmente configurar uma conta de serviço para um Farm de servidores de Federação](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md). Cada servidor de federação no farm de servidor de federação deve especificar a mesma conta de serviço para que o farm estar funcionando. Por exemplo, se a conta de serviço que foi criada foi contoso\\ADFS2SVC, cada computador que você configura para a função de servidor de Federação e que participarão mesmo farm deve especificar contoso\\ADFS2SVC nessa etapa no Assistente de configuração de servidor de Federação do farm se torne operacional.  
  
5.  Sobre o **pronto para aplicar configurações** página, examine os detalhes. Se as configurações parecerem corretas, clique em **próxima** para começar a configurar o AD FS com essas configurações.  
  
6.  Sobre o **resultados de configuração** página, examinar os resultados. Quando terminarem de todas as etapas de configuração, clique em **fechar** para sair do assistente.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurar um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  

