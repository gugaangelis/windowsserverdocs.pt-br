---
title: Processamento de solicitação de Conexão
description: Este tópico fornece uma visão geral da solicitação de conexão do servidor de política de rede processamento no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 849d661a-42c1-4f93-b669-6009d52aad39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6756c89dadbd01998ffef6a6d785c079076f2532
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="connection-request-processing"></a>Processamento de solicitação de Conexão

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber mais sobre a solicitação de conexão de processamento no servidor de política de rede no Windows Server 2016.

>[!NOTE]
>Além de neste tópico, a seguinte documentação de processamento de solicitação de conexão está disponível.
> - [Políticas de solicitação de Conexão](nps-crp-crpolicies.md)
> - [Nomes de territórios](nps-crp-realm-names.md)
> - [Grupos de servidores RADIUS remotos](nps-crp-rrsg.md)

Você pode usar o processamento de solicitação de conexão para especificar onde a autenticação de solicitações de conexão é efetuada - no computador local ou em um servidor RADIUS remoto que seja um membro de um grupo de servidores remotos RADIUS. 

Se quiser que o servidor local que executa o servidor de política de rede (NPS) para realizar a autenticação de solicitações de conexão, você pode usar a política de solicitação de conexão padrão sem configuração adicional. Com base na política padrão, o NPS autentica os usuários e computadores que tenham uma conta no domínio local e em domínios confiáveis.

Se você quiser encaminhar solicitações de conexão para um NPS remoto ou outro servidor RADIUS, crie um grupo de servidores remotos RADIUS e, em seguida, configurar uma política de solicitação de conexão que encaminha as solicitações para esse grupo de servidores remotos RADIUS. Com essa configuração, NPS podem encaminhar solicitações de autenticação para qualquer servidor RADIUS e os usuários com contas de domínios não confiáveis podem ser autenticados.

A ilustração a seguir mostra o caminho de uma mensagem de solicitação de acesso de um servidor de acesso de rede para um proxy RADIUS e, em seguida, em um servidor RADIUS em um grupo de servidores remotos RADIUS. Sobre o proxy RADIUS, o servidor de acesso de rede está configurado como um cliente RADIUS; e, em cada servidor RADIUS, o proxy RADIUS está configurado como um cliente RADIUS.


![Processamento de solicitação de Conexão de NPS](../../media/Nps-Connection-Request-Processing/Nps-Connection-Request-Processing.jpg)


>[!NOTE]
>Os servidores de acesso de rede que você usa com NPS podem ser dispositivos de gateway compatíveis com o protocolo RADIUS, como comutadores de autenticação e pontos de acesso sem fio 802.1 X, servidores que executam o acesso remoto que estão configurados como VPN ou servidores dial-up, ou outros dispositivos compatíveis com o RAIO.

Se você quiser NPS processe algumas solicitações de autenticação localmente enquanto encaminhar outras solicitações para um grupo de servidores remotos RADIUS, configure mais de uma política de solicitação de conexão.

Para configurar uma política de solicitação de conexão que especifica qual grupo de servidores RADIUS ou NPS processa solicitações de autenticação, consulte as políticas de solicitação de Conexão.

Para especificar o NPS ou outros servidores RADIUS para autenticação solicitações são encaminhadas, consulte grupos de servidores remotos RADIUS.

## <a name="nps-as-a-radius-server-connection-request-processing"></a>NPS como um processamento de solicitação de conexão de servidor RADIUS

Quando você usa o NPS como um servidor RADIUS, o RAIO mensagens fornecem autenticação, autorização e estatísticas para conexões de acesso à rede da seguinte maneira:

1. Servidores de acesso, como servidores de acesso à rede dial-up, servidores VPN e pontos de acesso sem fio, recebem solicitações de conexão de clientes de acesso. 

2. O servidor de acesso, configurado para usar RADIUS como a autenticação, autorização e protocolo de contabilidade, cria uma mensagem de solicitação de acesso e envia ao servidor NPS. 

3. O servidor NPS avalia a mensagem de solicitação de acesso. 

4. Se necessário, o servidor NPS envia uma mensagem de desafio de acesso ao servidor de acesso. O servidor de acesso processa o desafio e envia uma solicitação de acesso atualizada para o servidor NPS. 

5. As credenciais do usuário são verificadas e as propriedades de discagem rápida da conta do usuário são obtidas usando uma conexão segura para um controlador de domínio. 

6. A tentativa de conexão é autorizada com ambos os as propriedades de discagem das políticas de rede e de conta de usuário. 

7. Se a tentativa de conexão foi autenticada e autorizada, o servidor NPS envia uma mensagem de aceitação de acesso ao servidor de acesso. Se a tentativa de conexão não é autenticada ou não autorizada, o servidor NPS envia uma mensagem de rejeição de acesso ao servidor de acesso. 

8. O servidor de acesso conclui o processo de conexão com o cliente de acesso e envia uma mensagem de solicitação de contabilização ao servidor NPS, onde a mensagem é registrada. 

9. O servidor NPS envia uma resposta de contabilidade para o servidor de acesso. 

>[!NOTE]
>O servidor de acesso também envia mensagens de solicitação de contabilização durante o tempo em que a conexão é estabelecida, quando a conexão do cliente de acesso é fechado e quando o servidor de acesso é iniciado e interrompido.

## <a name="nps-as-a-radius-proxy-connection-request-processing"></a>NPS como um processamento de solicitação de conexão de proxy RADIUS

Quando o NPS é usado como um proxy RADIUS entre um cliente RADIUS e um servidor RADIUS, as mensagens de RAIO para rede acesso conexão tentativas são encaminhadas da seguinte maneira:

1. Servidores de acesso, como servidores de acesso à rede dial-up, servidores de rede virtual privada (VPN) e pontos de acesso sem fio, recebem solicitações de conexão de clientes de acesso.

2. O servidor de acesso, configurado para usar RADIUS como a autenticação, autorização e protocolo de contabilidade, cria uma mensagem de solicitação de acesso e envia ao servidor NPS que está sendo usado como o proxy RADIUS de NPS.

3. O proxy RADIUS de NPS recebe a mensagem de solicitação de acesso e, com base em diretivas de solicitação de conexão configuradas localmente, determina onde encaminhar a mensagem de solicitação de acesso.

4. O proxy RADIUS de NPS encaminha a mensagem de solicitação de acesso ao servidor RADIUS apropriado.

5. O servidor RADIUS avalia a mensagem de solicitação de acesso.

6. Se necessário, o servidor RADIUS envia uma mensagem de desafio de acesso para o proxy RADIUS de NPS, no qual ele é encaminhado para o servidor de acesso. O servidor de acesso processa o desafio com o cliente de acesso e envia uma solicitação de acesso atualizada para o proxy RADIUS de NPS, onde ele é encaminhado para o servidor RADIUS.

7. O servidor RADIUS autentica e autoriza a tentativa de conexão.

8. Se a tentativa de conexão foi autenticada e autorizada, o servidor RADIUS envia uma mensagem de aceitação de acesso para o proxy RADIUS de NPS, onde ele é encaminhado para o servidor de acesso. Se a tentativa de conexão não é autenticada ou não autorizada, o servidor RADIUS envia uma mensagem de rejeição de acesso para o proxy RADIUS de NPS, no qual ele é encaminhado para o servidor de acesso.

9. O servidor de acesso conclui o processo de conexão com o cliente de acesso e envia uma mensagem de solicitação de contabilização para o proxy RADIUS de NPS. O proxy RADIUS de NPS registra os dados contábeis e encaminha a mensagem para o servidor RADIUS.

10. O servidor RADIUS envia uma resposta de contabilidade para o proxy RADIUS de NPS, onde ele é encaminhado para o servidor de acesso.

Para obter mais informações sobre o NPS, consulte [servidor de política de rede (NPS)](nps-top.md).
