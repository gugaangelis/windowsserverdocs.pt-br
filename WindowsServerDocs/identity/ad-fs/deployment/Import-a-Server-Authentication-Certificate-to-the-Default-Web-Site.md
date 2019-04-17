---
ms.assetid: e1f2ce2d-b24f-4ccd-8add-9e69419fc6c1
title: "Importar um certificado de autenticação de servidor para o Site da Web padrão"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 7bc890c744de5cd86d4e8b0418e75512518f656c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="import-a-server-authentication-certificate-to-the-default-web-site"></a>Importar um certificado de autenticação de servidor para o Site da Web padrão

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Depois de obter um servidor de certificado de autenticação de uma autoridade de certificação \(CA\), você deve instalar manualmente esse certificado no Site da Web padrão para cada servidor de Federação ou o proxy do servidor de Federação em um farm de servidores.  
  
Para servidores Web, instale manualmente o certificado de autenticação de servidor no diretório em que seu aplicativo federado reside ou site apropriado.  
  
Se você estiver configurando um farm, certifique-se de executar este procedimento de forma idêntica — usando as mesmas configurações exatas — em cada um dos servidores em seu farm.  
  
> [!NOTE]  
> O AD FS snap\-in Gerenciamento refere-se aos certificados de autenticação de servidor para servidores de federação como certificados de comunicação do serviço.  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-import-a-server-authentication-certificate-to-the-default-web-site"></a>Para importar um certificado de autenticação de servidor para o Site padrão  
  
1.  Sobre o **iniciar** de tela, digite**\(IIS\) Internet Information Services Manager**, e pressione ENTER.  
  
2.  Na árvore de console, clique em **ComputerName**.  
  
3.  No painel central, clique em double\ **certificados de servidor**.  
  
4.  No **ações** painel, clique em **importar**.  
  
5.  No **Importar certificado** caixa de diálogo, clique no **...** botão.  
  
6.  Navegue até o local do arquivo de certificado pfx, realçá-la e, em seguida, clique em **abrir**.  
  
7.  Digite uma senha para o certificado e clique em **Okey**.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurar um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de verificação: Configurando um Proxy de servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de certificado para servidores de Federação](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Requisitos de certificado para Proxies de servidor de Federação](https://technet.microsoft.com/library/dd807054.aspx)  
   
  

