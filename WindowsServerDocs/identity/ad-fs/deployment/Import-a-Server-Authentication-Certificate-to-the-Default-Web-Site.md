---
ms.assetid: e1f2ce2d-b24f-4ccd-8add-9e69419fc6c1
title: Importar um Certificado de Autenticação de Servidor para o site padrão
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: da91a3e8c34c86f7fd03ca875b3800fdb6001750
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192123"
---
# <a name="import-a-server-authentication-certificate-to-the-default-web-site"></a>Importar um Certificado de Autenticação de Servidor para o site padrão

Depois de obter um servidor de certificado de autenticação de uma autoridade de certificação \(autoridade de certificação\), você deve instalar manualmente esse certificado no Site da Web padrão para cada servidor de Federação ou proxy de servidor de Federação em um farm de servidores.  
  
Para servidores Web, instale manualmente o certificado de autenticação de servidor no site ou no diretório virtual apropriado em que reside seu aplicativo federado.  
  
Se estiver configurando um farm, assegure-se de realizar esse procedimento de forma idêntica — usando exatamente as mesmas configurações — em cada um dos servidores em seu farm.  
  
> [!NOTE]  
> O snap de gerenciamento do AD FS\-refere-se aos certificados de autenticação de servidor para servidores de federação como certificados de comunicação de serviço.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-import-a-server-authentication-certificate-to-the-default-web-site"></a>Para importar um certificado de autenticação de servidor no Site Padrão  
  
1.  Sobre o **iniciar** tela, digite**serviços de informações da Internet \(IIS\) Manager**, e, em seguida, pressione ENTER.  
  
2.  Na árvore de console, clique em **ComputerName**.  
  
3.  No painel central, clique duas vezes\-clique em **certificados de servidor**.  
  
4.  No painel **Ações**, clique em **Importar**.  
  
5.  No **Importar certificado** caixa de diálogo, clique o **...** .  
  
6.  Procure o local do arquivo de certificado pfx, destaque-o e clique em **Abrir**.  
  
7.  Digite uma senha para o certificado e clique em **OK**.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Como configurar um servidor de federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de verificação: Como configurar um proxy do servidor de federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de certificado para servidores de federação](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Requisitos de certificado para proxies do servidor de federação](https://technet.microsoft.com/library/dd807054.aspx)  
   
  

