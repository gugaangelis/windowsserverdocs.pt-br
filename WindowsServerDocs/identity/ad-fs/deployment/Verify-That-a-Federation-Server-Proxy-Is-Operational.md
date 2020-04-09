---
ms.assetid: d555041a-709e-42c7-920a-8dfd2c7e0488
title: Verificar se um proxy do servidor de federação está funcionando
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 3e74af4a63476040ca44522ceb7c0ae22e914fec
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855849"
---
# <a name="verify-that-a-federation-server-proxy-is-operational"></a>Verificar se um proxy do servidor de federação está funcionando


Você pode usar o procedimento a seguir para verificar se o proxy do servidor de Federação pode se comunicar com o Serviço de Federação em Serviços de Federação do Active Directory (AD FS) \(AD FS\). Execute esse procedimento depois de executar o **AD FS assistente de configuração de proxy do servidor de Federação** para configurar o computador a ser executado na função proxy do servidor de Federação. Para obter mais informações sobre como executar esse assistente, consulte [configurar um computador para a função proxy do servidor de Federação](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
> [!IMPORTANT]  
> O resultado desse teste é a geração bem-sucedida de um evento específico no Visualizador de Eventos no computador proxy de servidor de federação.  
  
A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-verify-that-a-federation-server-proxy-is-operational"></a>Para verificar se um proxy do servidor de Federação está operacional  
  
1.  Faça logon no proxy do servidor de Federação como administrador.  
  
2.  Na tela **Iniciar** , digite**Visualizador de eventos**e pressione Enter.  
  
3.  No painel de detalhes, clique duas vezes\-clicar em **logs de aplicativos e serviços**, duplo\-clique em **AD FS eventos**e, em seguida, clique em **admin**.  
  
4.  Na coluna **ID do Evento**, procure o ID de evento 198.  
  
    Se o proxy do servidor de Federação estiver configurado corretamente, você verá um novo evento no log do aplicativo de Visualizador de Eventos, com a ID de evento 198. Este evento verifica se o serviço proxy do servidor de Federação foi iniciado com êxito e agora está online.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando um proxy de servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

