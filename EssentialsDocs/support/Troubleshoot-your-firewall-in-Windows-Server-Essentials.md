---
title: Solucionar problemas do firewall no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 51d94b67-8b9b-4159-80dd-f652d73a43cb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 432acd242bcd9fcb2ecaa7a820accda2ac5a1d20
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85470051"
---
# <a name="troubleshoot-your-firewall-in-windows-server-essentials"></a>Solucionar problemas do firewall no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Se você tiver problemas com o acesso remoto, execute o assistente Reparar Acesso em Qualquer Local.

### <a name="to-run-the-repair-anywhere-access-wizard"></a>Para executar o assistente Reparar Acesso em Qualquer Local

1. Abra o Painel.

2. Clique em **Configurações**, clique na guia **Acesso em Qualquer Local** e clique em **Reparar**.

3. Siga as instruções no assistente Reparar Acesso em Qualquer Local.

   Se você estiver usando uma configuração de rede avançada ou um firewall não Microsoft, talvez seja necessário abrir portas adicionais no firewall. As portas na tabela a seguir são registradas na IANA (Internet Assigned Numbers Authority).

|Número da porta|Descrição|
|-----------------|-----------------|
|65500|Serviço Web de certificado|
|65510 e 65515|Site de implantação do computador cliente|
|65520|Serviço Web para computadores cliente Mac|
|65532|Estrutura do provedor para comunicações loopback do servidor|
|6602|Estrutura do provedor para a comunicação entre os computadores cliente e servidor|

## <a name="see-also"></a>Veja também

-   [Usar o Acesso via Web Remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)

-   [Gerenciar o Acesso via Web Remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)

-   [Gerenciar o Acesso em Qualquer Local](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)

-   [Gerenciar o Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)

-   [Suporte ao Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

