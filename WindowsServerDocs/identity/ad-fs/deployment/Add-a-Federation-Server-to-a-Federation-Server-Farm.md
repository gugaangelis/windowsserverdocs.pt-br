---
ms.assetid: 6ecf8d85-cd61-4c87-add8-00a679a6e3ff
title: Adicionar um servidor de federação a um farm de servidores de federação
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ea826f8984937c5d32a830131f042a8519528268
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815049"
---
# <a name="add-a-federation-server-to-a-federation-server-farm"></a>Adicionar um servidor de federação a um farm de servidores de federação


Depois de instalar o Serviço de Federação serviço de função e configurar os certificados necessários em um computador, você estará pronto para configurar o computador para se tornar um servidor de Federação. Use o procedimento a seguir para ingressar um computador em um novo farm de servidores de federação.  
  
Você une um computador a um farm com o Assistente de Configuração de Servidor de Federação do AD FS. Quando você usa esse assistente para ingressar um computador em um farm existente, o computador é configurado com uma cópia somente leitura\-do banco de dados de configuração do AD FS e deve receber atualizações de um servidor de Federação primário.  
  
> [!NOTE]  
> Para o\-de logon\-único da Web federado em \(design de\) SSO, você deve ter pelo menos um servidor de Federação na organização do parceiro de conta e pelo menos um servidor de Federação na organização do parceiro de recurso. Para obter mais informações, consulte [Onde colocar um servidor de federação](https://technet.microsoft.com/library/dd807127.aspx).  
  
A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=\)83477.   
  
### <a name="to-add-a-federation-server-to-a-federation-server-farm"></a>Para adicionar um servidor de Federação a um farm de servidores de Federação  
  
1.  Ha duas maneiras para iniciar o Assistente de Configuração do Servidor de Federação AD FS. Para iniciar o assistente,  faça o seguinte:  
  
    -   Depois que a instalação do serviço de função Serviço de Federação for concluída, abra o\-snap de gerenciamento de AD FS no e clique no link **Assistente de configuração do servidor de federação AD FS** na página **visão geral** ou no painel **ações** .  
  
    -   A qualquer momento depois que o assistente de instalação for concluído, abra o Windows Explorer, navegue até a pasta **C:\\Windows\\ADFS** e clique duas vezes\-clique em **FsConfigWizard. exe**.  
  
2.  Na página **Bem-vindo**, verifique que o **Adicionar um servidor de federação para um Serviço de Federação existente** esteja selecionado, e então clique em **Próximo**.  
  
3.  Se o banco de dados de AD FS que você selecionou já existir, a página **banco de dados de configuração AD FS existente detectado** será exibida. Se isso ocorrer, clique em **Excluir banco de dados**, e depois clique em **Próximo**.  
  
    > [!CAUTION]  
    > Selecione esta opção apenas quando você tiver certeza que os dados neste banco de dados do AD FS não forem importantes ou que não é usado em um farm de servidor de federação.  
  
4.  Na página **Especificar o Servidor de Federação Principal e a Conta de Serviço**, em **Nome de servidor de federação principal**, digite o nome do computador do servidor de federação principal no farm e clique em **Navegar**. Na caixa de diálogo **Navegar**, localizar a conta de domínio que será usada como a conta de serviço por todos os outros servidores de federação no farm de servidor de federação existente e clique em **OK**. Digite a senha e confirme ela e clique em **Próximo**:  
  
    > [!NOTE]  
    > Para obter mais informações sobre como especificar uma conta de serviço para um farm de servidores de Federação, consulte [configurar manualmente uma conta de serviço para um farm de servidores de Federação](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md). Cada servidor de Federação no farm de servidores de Federação deve especificar a mesma conta de serviço para o farm estar operacional. Por exemplo, se a conta de serviço criada foi contoso\\ADFS2SVC, cada computador configurado para a função de servidor de Federação e que participará do mesmo farm deverá especificar contoso\\ADFS2SVC nesta etapa no assistente de configuração do servidor de Federação para que o farm esteja operacional.  
  
5.  Na página **Pronto para Aplicar as Configurações**, verificar os detalhes. Se as configurações parecem estar certas, clique em **Próximo** para começar configurar o AD FS com estas configurações.  
  
6.  Na página **Resultados de Configuração**, analise os resultados. Quando todas as etapas de configuração forem concluídas, clique em **fechar** para sair do assistente.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  

