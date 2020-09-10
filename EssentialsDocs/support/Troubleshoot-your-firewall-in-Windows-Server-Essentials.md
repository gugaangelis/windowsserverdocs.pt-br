---
title: Solucionar problemas do firewall no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 51d94b67-8b9b-4159-80dd-f652d73a43cb
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: d1a36ecbd62e658c6361c004cf0166a295641b35
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89625043"
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

## <a name="see-also"></a>Confira também

-   [Usar o Acesso via Web Remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)

-   [Gerenciar o Acesso via Web Remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)

-   [Gerenciar o Acesso em Qualquer Local](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)

-   [Gerenciar o Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)

-   [Suporte ao Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

