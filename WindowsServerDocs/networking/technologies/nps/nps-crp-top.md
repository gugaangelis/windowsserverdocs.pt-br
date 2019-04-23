---
title: Processamento de solicitações de conexão
description: Este tópico fornece uma visão geral do processamento no Windows Server 2016 de solicitação de conexão de servidor de políticas de rede.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 849d661a-42c1-4f93-b669-6009d52aad39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6ab08dced15cfadd5a0fc773efc2ddaa2da24c08
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829107"
---
# <a name="connection-request-processing"></a>Processamento de solicitações de conexão

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre o processamento no servidor de políticas de rede no Windows Server 2016 de solicitação de conexão.

>[!NOTE]
>Além deste tópico, a seguinte documentação de processamento de solicitação de conexão está disponível.
> - [Diretivas de solicitação de Conexão](nps-crp-crpolicies.md)
> - [Nomes de território](nps-crp-realm-names.md)
> - [Grupos de servidores remotos RADIUS](nps-crp-rrsg.md)

Você pode usar o processamento de solicitação de conexão para especificar onde a autenticação de solicitações de conexão é executada - no computador local ou em um servidor RADIUS remoto que é um membro de um grupo de servidores RADIUS remotos. 

Se você quiser que o servidor local executando o servidor de diretivas de rede (NPS) para realizar a autenticação para solicitações de conexão, você pode usar a diretiva de solicitação de conexão padrão sem configuração adicional. Com base na política padrão, o NPS autentica usuários e computadores que têm uma conta no domínio local e em domínios confiáveis.

Se você quiser encaminhar solicitações de conexão para um NPS remoto ou outro servidor RADIUS, crie um grupo de servidores remotos RADIUS e, em seguida, configurar uma diretiva de solicitação de conexão que encaminha solicitações para esse grupo de servidores RADIUS remotos. Com essa configuração, o NPS pode encaminhar solicitações de autenticação para qualquer servidor RADIUS e os usuários com contas em domínios não confiáveis podem ser autenticados.

A ilustração a seguir mostra o caminho de uma mensagem de solicitação de acesso de um servidor de acesso de rede para um proxy RADIUS e, em seguida, para um servidor RADIUS em um grupo de servidores RADIUS remotos. No proxy de RADIUS, o servidor de acesso de rede está configurado como um cliente RADIUS; e, em cada servidor RADIUS, proxy RADIUS está configurado como um cliente RADIUS.


![Processamento de solicitação de Conexão de NPS](../../media/Nps-Connection-Request-Processing/Nps-Connection-Request-Processing.jpg)


>[!NOTE]
>Os servidores de acesso de rede que você usa com o NPS podem ser dispositivos de gateway que são compatíveis com o protocolo RADIUS, como 802.1 X de pontos de acesso sem fio e comutadores de autenticação, os servidores que executam o acesso remoto e que são configurados como VPN ou dial-up servidores, ou outros dispositivos compatíveis do RADIUS.

Se você quiser que o NPS para processar algumas solicitações de autenticação localmente, enquanto encaminhar outras solicitações para um grupo de servidores remotos RADIUS, configure mais de uma diretiva de solicitação de conexão.

Para configurar uma diretiva de solicitação de conexão que especifica qual grupo de servidor RADIUS ou NPS processa solicitações de autenticação, consulte as diretivas de solicitação de Conexão.

Para especificar o NPS ou outros servidores RADIUS para autenticação que as solicitações são encaminhadas, consulte grupos de servidores RADIUS remotos.

## <a name="nps-as-a-radius-server-connection-request-processing"></a>NPS como um processamento de solicitação de conexão do servidor RADIUS

Quando você usa o NPS como servidor RADIUS, as mensagens RADIUS fornecem autenticação, autorização e contabilização para conexões de acesso de rede da seguinte maneira:

1. Servidores de acesso, como servidores de acesso de rede dial-up, servidores VPN e pontos de acesso sem fio, recebem solicitações de conexão de clientes de acesso. 

2. O servidor de acesso, configurado para usar RADIUS como a autenticação, autorização e protocolo de contabilidade, cria uma mensagem de solicitação de acesso e envia para o NPS. 

3. O NPS avalia a mensagem de solicitação de acesso. 

4. Se necessário, o NPS envia uma mensagem de desafio de acesso ao servidor de acesso. O servidor de acesso processa o desafio e envia uma solicitação de acesso atualizada para o NPS. 

5. As credenciais do usuário são verificadas e as propriedades de discagem da conta de usuário são obtidas usando uma conexão segura para um controlador de domínio. 

6. A tentativa de conexão for autorizada com as duas as propriedades de discagem das políticas de rede e conta de usuário. 

7. Se a tentativa de conexão é autenticada e autorizada, o NPS envia uma mensagem de aceitação de acesso ao servidor de acesso. Se a tentativa de conexão não autenticada ou não autorizada, o NPS envia uma mensagem de rejeição de acesso ao servidor de acesso. 

8. O servidor de acesso conclui o processo de conexão com o cliente de acesso e envia uma mensagem de solicitação de estatísticas para o NPS, no qual a mensagem é registrada. 

9. O NPS envia uma resposta de estatísticas para o servidor de acesso. 

>[!NOTE]
>O servidor de acesso também envia mensagens de solicitação de estatísticas durante o tempo em que a conexão é estabelecida, quando a conexão de cliente de acesso é fechada, e quando o servidor de acesso é iniciado e parado.

## <a name="nps-as-a-radius-proxy-connection-request-processing"></a>NPS como um processamento de solicitação de conexão de proxy RADIUS

Quando o NPS é usado como um proxy RADIUS entre um cliente RADIUS e um servidor RADIUS, as mensagens RADIUS para rede acesso à conexão tentativas são encaminhadas da seguinte maneira:

1. Servidores de acesso, como servidores de acesso de rede dial-up, servidores de rede virtual privada (VPN) e pontos de acesso sem fio, recebem solicitações de conexão de clientes de acesso.

2. O servidor de acesso, configurado para usar RADIUS como a autenticação, autorização e protocolo de contabilidade, cria uma mensagem de solicitação de acesso e envia para o NPS que está sendo usado como o proxy RADIUS do NPS.

3. O proxy RADIUS NPS recebe a mensagem de solicitação de acesso e, com base em diretivas de solicitação de conexão configuradas localmente, determina para onde encaminhar a mensagem de solicitação de acesso.

4. O proxy RADIUS NPS encaminha a mensagem de solicitação de acesso para o servidor RADIUS adequado.

5. O servidor RADIUS avalia a mensagem de solicitação de acesso.

6. Se necessário, o servidor RADIUS envia uma mensagem de Access-Challenge para o proxy de RADIUS do NPS, no qual ele é encaminhado para o servidor de acesso. O servidor de acesso processa o desafio com o cliente de acesso e envia uma solicitação de acesso atualizada para o proxy de RADIUS do NPS, no qual ele é encaminhado para o servidor RADIUS.

7. O servidor RADIUS autentica e autoriza a tentativa de conexão.

8. Se a tentativa de conexão é autenticada e autorizada, o servidor RADIUS envia uma mensagem de aceitação de acesso para o proxy de RADIUS do NPS, no qual ele é encaminhado para o servidor de acesso. Se a tentativa de conexão não autenticada ou não autorizada, o servidor RADIUS envia uma mensagem de rejeição de acesso para o proxy de RADIUS do NPS, no qual ele é encaminhado para o servidor de acesso.

9. O servidor de acesso conclui o processo de conexão com o cliente de acesso e envia uma mensagem de solicitação de estatísticas para o proxy RADIUS do NPS. O proxy RADIUS NPS registra os dados de estatísticas e encaminha a mensagem para o servidor RADIUS.

10. O servidor RADIUS enviará uma resposta de contabilização para o proxy de RADIUS do NPS, no qual ele é encaminhado para o servidor de acesso.

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).
