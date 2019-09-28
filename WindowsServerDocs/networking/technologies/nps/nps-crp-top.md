---
title: Processamento de solicitações de conexão
description: Este tópico fornece uma visão geral do processamento de solicitação de conexão do servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 849d661a-42c1-4f93-b669-6009d52aad39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 268a307336d9a4fae1be5621eec7c32a06a6204e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405396"
---
# <a name="connection-request-processing"></a>Processamento de solicitações de conexão

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre o processamento de solicitação de conexão no servidor de políticas de rede no Windows Server 2016.

>[!NOTE]
>Além deste tópico, a documentação de processamento de solicitação de conexão a seguir está disponível.
> - [Políticas de solicitação de conexão](nps-crp-crpolicies.md)
> - [Nomes de territórios](nps-crp-realm-names.md)
> - [Grupos de servidores RADIUS remotos](nps-crp-rrsg.md)

Você pode usar o processamento de solicitação de conexão para especificar onde a autenticação de solicitações de conexão é executada-no computador local ou em um servidor RADIUS remoto que é membro de um grupo de servidores RADIUS remoto. 

Se desejar que o servidor local que está executando o NPS (servidor de diretivas de rede) execute a autenticação para solicitações de conexão, você poderá usar a política de solicitação de conexão padrão sem configuração adicional. Com base na política padrão, o NPS autentica usuários e computadores que têm uma conta no domínio local e em domínios confiáveis.

Se você quiser encaminhar as solicitações de conexão para um NPS remoto ou outro servidor RADIUS, crie um grupo de servidores remotos RADIUS e configure uma política de solicitação de conexão que encaminhe solicitações para esse grupo de servidores remotos RADIUS. Com essa configuração, o NPS pode encaminhar solicitações de autenticação para qualquer servidor RADIUS, e os usuários com contas em domínios não confiáveis podem ser autenticados.

A ilustração a seguir mostra o caminho de uma mensagem de solicitação de acesso de um servidor de acesso à rede para um proxy RADIUS e, em seguida, para um servidor RADIUS em um grupo de servidores RADIUS remoto. No proxy RADIUS, o servidor de acesso à rede é configurado como um cliente RADIUS; e, em cada servidor RADIUS, o proxy RADIUS é configurado como um cliente RADIUS.


![Processamento de solicitação de conexão NPS](../../media/Nps-Connection-Request-Processing/Nps-Connection-Request-Processing.jpg)


>[!NOTE]
>Os servidores de acesso à rede que você usa com o NPS podem ser dispositivos de gateway em conformidade com o protocolo RADIUS, como pontos de acesso sem fio 802.1 X e comutadores de autenticação, servidores que executam acesso remoto que são configurados como servidores VPN ou discadas, ou outros dispositivos compatíveis com RADIUS.

Se você quiser que o NPS processe algumas solicitações de autenticação localmente ao encaminhar outras solicitações para um grupo de servidores RADIUS remotos, configure mais de uma política de solicitação de conexão.

Para configurar uma política de solicitação de conexão que especifica qual NPS ou grupo de servidores RADIUS processa solicitações de autenticação, consulte políticas de solicitação de conexão.

Para especificar o NPS ou outros servidores RADIUS aos quais as solicitações de autenticação são encaminhadas, consulte grupos de servidores remotos RADIUS.

## <a name="nps-as-a-radius-server-connection-request-processing"></a>NPS como um processamento de solicitação de conexão de servidor RADIUS

Quando você usa o NPS como um servidor RADIUS, as mensagens RADIUS fornecem autenticação, autorização e contabilidade para conexões de acesso à rede da seguinte maneira:

1. Servidores de acesso, como servidores de acesso à rede dial-up, servidores VPN e pontos de acesso sem fio, recebem solicitações de conexão de clientes de acesso. 

2. O servidor de acesso, configurado para usar o RADIUS como o protocolo de autenticação, autorização e contabilização, cria uma mensagem de solicitação de acesso e a envia para o NPS. 

3. O NPS avalia a mensagem de solicitação de acesso. 

4. Se necessário, o NPS envia uma mensagem de desafio de acesso ao servidor de acesso. O servidor de acesso processa o desafio e envia uma solicitação de acesso atualizada para o NPS. 

5. As credenciais do usuário são verificadas e as propriedades de discagem da conta de usuário são obtidas usando uma conexão segura com um controlador de domínio. 

6. A tentativa de conexão é autorizada com as propriedades de discagem da conta de usuário e das diretivas de rede. 

7. Se a tentativa de conexão for autenticada e autorizada, o NPS enviará uma mensagem de aceitação de acesso ao servidor de acesso. Se a tentativa de conexão não for autenticada ou não autorizada, o NPS enviará uma mensagem de rejeição de acesso ao servidor de acesso. 

8. O servidor de acesso conclui o processo de conexão com o cliente de acesso e envia uma mensagem de solicitação de contabilização para o NPS, onde a mensagem é registrada. 

9. O NPS envia uma resposta de contabilidade para o servidor de acesso. 

>[!NOTE]
>O servidor de acesso também envia mensagens de solicitação de contabilização durante o tempo em que a conexão é estabelecida, quando a conexão do cliente de acesso é fechada e quando o servidor de acesso é iniciado e interrompido.

## <a name="nps-as-a-radius-proxy-connection-request-processing"></a>NPS como um processamento de solicitação de conexão proxy RADIUS

Quando o NPS é usado como um proxy RADIUS entre um cliente RADIUS e um servidor RADIUS, as mensagens RADIUS para tentativas de conexão de acesso à rede são encaminhadas da seguinte maneira:

1. Servidores de acesso, como servidores de acesso à rede dial-up, servidores de rede virtual privada (VPN) e pontos de acesso sem fio, recebem solicitações de conexão de clientes de acesso.

2. O servidor de acesso, configurado para usar o RADIUS como o protocolo de autenticação, autorização e contabilização, cria uma mensagem de solicitação de acesso e a envia para o NPS que está sendo usado como o proxy RADIUS do NPS.

3. O proxy RADIUS do NPS recebe a mensagem de solicitação de acesso e, com base nas políticas de solicitação de conexão configuradas localmente, determina onde encaminhar a mensagem de solicitação de acesso.

4. O proxy RADIUS do NPS encaminha a mensagem de solicitação de acesso para o servidor RADIUS apropriado.

5. O servidor RADIUS avalia a mensagem de solicitação de acesso.

6. Se necessário, o servidor RADIUS envia uma mensagem de desafio de acesso para o proxy RADIUS do NPS, onde ele é encaminhado para o servidor de acesso. O servidor de acesso processa o desafio com o cliente de acesso e envia uma solicitação de acesso atualizada para o proxy RADIUS do NPS, onde ele é encaminhado para o servidor RADIUS.

7. O servidor RADIUS autentica e autoriza a tentativa de conexão.

8. Se a tentativa de conexão for autenticada e autorizada, o servidor RADIUS enviará uma mensagem de aceitação de acesso ao proxy RADIUS do NPS, onde ele será encaminhado para o servidor de acesso. Se a tentativa de conexão não for autenticada ou não autorizada, o servidor RADIUS enviará uma mensagem de rejeição de acesso ao proxy RADIUS do NPS, onde ele será encaminhado para o servidor de acesso.

9. O servidor de acesso conclui o processo de conexão com o cliente de acesso e envia uma mensagem de solicitação de contabilização para o proxy RADIUS do NPS. O proxy RADIUS do NPS registra os dados de estatísticas e encaminha a mensagem para o servidor RADIUS.

10. O servidor RADIUS envia uma resposta de contabilidade para o proxy RADIUS do NPS, onde ele é encaminhado para o servidor de acesso.

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).
