---
title: Planejar NPS como um proxy RADIUS
description: Este tópico fornece informações sobre o planejamento de implantação de proxy RADIUS do servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ca77d64a-065b-4bf2-8252-3e75f71b7734
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 29a48275dfd56cbf223e0fca0c9c276f35a675cc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396018"
---
# <a name="plan-nps-as-a-radius-proxy"></a>Planejar NPS como um proxy RADIUS

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Ao implantar o NPS (servidor de diretivas de rede) como um serviço RADIUS \(proxy de\) RADIUS, o NPS recebe solicitações de conexão de clientes RADIUS, como servidores de acesso à rede ou outros proxies RADIUS, e encaminha essas solicitações de conexão para servidores que executam o NPS ou outros servidores RADIUS. Você pode usar essas diretrizes de planejamento para simplificar a implantação do RADIUS.

Essas diretrizes de planejamento não incluem circunstâncias nas quais você deseja implantar o NPS como um servidor RADIUS. Quando você implanta o NPS como um servidor RADIUS, o NPS executa autenticação, autorização e contabilização de solicitações de conexão para o domínio local e para domínios que confiam no domínio local.

Antes de implantar o NPS como um proxy RADIUS em sua rede, use as diretrizes a seguir para planejar sua implantação.

- Planejar a configuração do NPS.

- Planejar clientes RADIUS.

- Planejar grupos de servidores remotos RADIUS.

- Planejar regras de manipulação de atributos para encaminhamento de mensagens.

- Planejar políticas de solicitação de conexão.

- Planejar a contabilidade do NPS.

## <a name="plan-nps-configuration"></a>Planejar a configuração do NPS

Quando você usa o NPS como um proxy RADIUS, o NPS encaminha as solicitações de conexão para um NPS ou outros servidores RADIUS para processamento. Por isso, a associação de domínio do proxy NPS é irrelevante. O proxy não precisa ser registrado no Active Directory Domain Services \(AD DS\) porque ele não precisa acessar as propriedades de discagem de contas de usuário. Além disso, você não precisa configurar políticas de rede em um proxy NPS porque o proxy não executa autorização para solicitações de conexão. O proxy NPS pode ser um membro de domínio ou pode ser um servidor autônomo sem Associação de domínio.

O NPS deve ser configurado para se comunicar com clientes RADIUS, também chamados de servidores de acesso à rede, usando o protocolo RADIUS. Além disso, você pode configurar os tipos de eventos que o NPS registra no log de eventos e pode inserir uma descrição para o servidor.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento para a configuração de proxy do NPS, você pode usar as etapas a seguir.

- Determine as portas RADIUS que o proxy NPS usa para receber mensagens RADIUS de clientes RADIUS e enviar mensagens RADIUS para membros de grupos de servidores RADIUS remotos. As portas UDP (User Datagram Protocol) padrão são 1812 e 1645 para mensagens de autenticação RADIUS e portas UDP 1813 e 1646 para mensagens de contabilização RADIUS.

- Se o proxy NPS estiver configurado com vários adaptadores de rede, determine os adaptadores sobre os quais você deseja que o tráfego RADIUS seja permitido.

- Determine os tipos de eventos que você deseja que o NPS registre no log de eventos. Você pode registrar solicitações de conexão rejeitadas, solicitações de conexão bem-sucedidas ou ambos.

- Determine se você está implantando mais de um proxy NPS. Para fornecer tolerância a falhas, use pelo menos dois proxies NPS. Um proxy NPS é usado como o proxy RADIUS primário e o outro é usado como um backup. Cada cliente RADIUS é configurado em ambos os proxies do NPS. Se o proxy NPS primário ficar indisponível, os clientes RADIUS enviarão mensagens de solicitação de acesso para o proxy NPS alternativo.

- Planeje o script usado para copiar uma configuração de proxy NPS para outros proxies NPS para economizar na sobrecarga administrativa e para impedir a configuração incorreta de um servidor. O NPS fornece os comandos netsh que permitem copiar toda ou parte de uma configuração de proxy NPS para importação em outro proxy NPS. Você pode executar os comandos manualmente no prompt do netsh. No entanto, se você salvar a sequência de comandos como um script, poderá executar o script em uma data posterior se decidir alterar as configurações de proxy.

## <a name="plan-radius-clients"></a>Planejar clientes RADIUS

Os clientes RADIUS são servidores de acesso à rede, como pontos de acesso sem fio, rede virtual privada \(servidores VPN\), comutadores compatíveis com 802.1 X e servidores dial-up. Os proxies RADIUS, que encaminham mensagens de solicitação de conexão para servidores RADIUS, também são clientes RADIUS. O NPS dá suporte a todos os servidores de acesso à rede e proxies RADIUS que estão em conformidade com o protocolo RADIUS, conforme descrito na RFC 2865, "serviço de usuário dial-in de autenticação remota \(RADIUS\)" e RFC 2866, "RADIUS Accounting".

Além disso, os pontos de acesso sem fio e os comutadores devem ser compatíveis com a autenticação 802.1 X. Se você quiser implantar o protocolo EAP (Extensible Authentication Protocol) ou o protocolo PEAP, os pontos de acesso e os comutadores devem oferecer suporte ao uso do EAP.

Para testar a interoperabilidade básica para conexões PPP para pontos de acesso sem fio, configure o ponto de acesso e o cliente de acesso para usar o protocolo de autenticação de senha (PAP). Use protocolos de autenticação adicionais baseados em PPP, como o PEAP, até que você tenha testado aqueles que pretende usar para acesso à rede.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento para clientes RADIUS, você pode usar as etapas a seguir.

- Documente os atributos específicos do fornecedor (VSAs) que você deve configurar no NPS. Se seus NASs exigirem VSAs, registre as informações de VSA para uso posterior ao configurar as políticas de rede no NPS.

- Documente os endereços IP de clientes RADIUS e seu proxy NPS para simplificar a configuração de todos os dispositivos. Ao implantar seus clientes RADIUS, você deve configurá-los para usar o protocolo RADIUS, com o endereço IP do proxy NPS inserido como o servidor de autenticação. E, ao configurar o NPS para se comunicar com seus clientes RADIUS, você deve inserir os endereços IP do cliente RADIUS no snap-in do NPS.

- Crie segredos compartilhados para configuração nos clientes RADIUS e no snap-in do NPS. Você deve configurar clientes RADIUS com um segredo compartilhado, ou senha, que também será inserido no snap-in do NPS ao configurar clientes RADIUS no NPS.

## <a name="plan-remote-radius-server-groups"></a>Planejar grupos de servidores RADIUS remotos

Quando você configura um grupo de servidores remotos RADIUS em um proxy NPS, está informando ao proxy NPS onde enviar algumas ou todas as mensagens de solicitação de conexão recebidas de servidores de acesso à rede e proxies NPS ou outros proxies RADIUS.

Você pode usar o NPS como um proxy RADIUS para encaminhar solicitações de conexão a um ou mais grupos de servidores RADIUS remotos, e cada grupo pode conter um ou mais servidores RADIUS. Quando desejar que o proxy NPS encaminhe mensagens a vários grupos, configure uma política de solicitação de conexão por grupo. A política de solicitação de conexão contém informações adicionais, como regras de manipulação de atributo, que dizem ao proxy NPS quais mensagens enviar ao grupo de servidores remotos RADIUS especificado na política.

Você pode configurar grupos de servidores remotos RADIUS usando os comandos netsh para NPS, configurando grupos diretamente no snap-in do NPS em grupos de servidores RADIUS remotos ou executando o assistente de nova política de solicitação de conexão.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento de grupos de servidores RADIUS remotos, você pode usar as etapas a seguir.

- Determine os domínios que contêm os servidores RADIUS aos quais você deseja que o proxy NPS encaminhe as solicitações de conexão. Esses domínios contêm as contas de usuário para usuários que se conectam à rede por meio de clientes RADIUS que você implanta.

- Determine se você precisa adicionar novos servidores RADIUS em domínios em que o RADIUS ainda não está implantado.

- Documente os endereços IP dos servidores RADIUS que você deseja adicionar aos grupos de servidores RADIUS remotos.

- Determine quantos grupos de servidores RADIUS remotos você precisa criar. Em alguns casos, é melhor criar um grupo de servidores RADIUS remoto por domínio e, em seguida, adicionar os servidores RADIUS para o domínio ao grupo. No entanto, pode haver casos em que você tem uma grande quantidade de recursos em um domínio, incluindo um grande número de usuários com contas de usuário no domínio, um grande número de controladores de domínio e um grande número de servidores RADIUS. Ou seu domínio pode abranger uma grande área geográfica, fazendo com que você tenha servidores de acesso à rede e servidores RADIUS em locais distantes uns dos outros. Nesses e possivelmente outros casos, você pode criar vários grupos de servidores RADIUS remotos por domínio.

- Crie segredos compartilhados para configuração no proxy NPS e nos servidores RADIUS remotos.

## <a name="plan-attribute-manipulation-rules-for-message-forwarding"></a>Planejar regras de manipulação de atributos para encaminhamento de mensagens

As regras de manipulação de atributo, que são configuradas em políticas de solicitação de conexão, permitem que você identifique as mensagens de solicitação de acesso que deseja encaminhar para um grupo de servidores RADIUS remoto específico.

Você pode configurar o NPS para encaminhar todas as solicitações de conexão para um grupo de servidores remotos RADIUS sem usar regras de manipulação de atributos.

No entanto, se você tiver mais de um local para o qual deseja encaminhar solicitações de conexão, será necessário criar uma política de solicitação de conexão para cada local e, em seguida, configurar a política com o grupo de servidores remotos RADIUS para o qual você deseja encaminhar mensagens, bem como com as regras de manipulação de atributo que dizem ao NPS quais mensagens encaminhadas.

Você pode criar regras para os atributos a seguir.

- Called-Station-ID. O número de telefone do NAS (servidor de acesso à rede). O valor desse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência de padrões para especificar códigos de área.

- ID de estação de chamada. O número de telefone usado pelo chamador. O valor desse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência de padrões para especificar códigos de área.

- Nome de usuário. O nome de usuário que é fornecido pelo cliente do Access e que é incluído pelo NAS na mensagem de solicitação de acesso do RADIUS. O valor desse atributo é uma cadeia de caracteres que normalmente contém um nome de realm e um nome de conta de usuário.

Para substituir ou converter corretamente nomes de territórios no nome de usuário de uma solicitação de conexão, você deve configurar regras de manipulação de atributos para o atributo User-Name na diretiva de solicitação de conexão apropriada.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento de regras de manipulação de atributos, você pode usar as etapas a seguir.

- Planeje o roteamento de mensagens do NAS por meio do proxy para os servidores RADIUS remotos para verificar se você tem um caminho lógico com o qual encaminhar mensagens para os servidores RADIUS.

- Determine um ou mais atributos que você deseja usar para cada política de solicitação de conexão.

- Documente as regras de manipulação de atributo que você planeja usar para cada política de solicitação de conexão e correlacione as regras ao grupo de servidores remotos RADIUS para o qual as mensagens são encaminhadas.

## <a name="plan-connection-request-policies"></a>Planejar políticas de solicitação de conexão

A política de solicitação de conexão padrão é configurada para NPS quando é usada como um servidor RADIUS. Políticas de solicitação de conexão adicionais podem ser usadas para definir condições mais específicas, criar regras de manipulação de atributos que informam ao NPS quais mensagens encaminhadas para grupos de servidores RADIUS remotos e para especificar atributos avançados. Use o assistente para nova política de solicitação de conexão para criar políticas de solicitação de conexão comuns ou personalizadas.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento de políticas de solicitação de conexão, você pode usar as etapas a seguir.

- Exclua a política de solicitação de conexão padrão em cada servidor que executa o NPS que funciona apenas como um proxy RADIUS.

- Planeje condições e configurações adicionais que são necessárias para cada política, combinando essas informações com o grupo de servidores remotos RADIUS e as regras de manipulação de atributo planejadas para a política.

- Projete o plano para distribuir políticas comuns de solicitação de conexão para todos os proxies do NPS. Crie políticas comuns a vários proxies de NPS em um NPS e, em seguida, use os comandos netsh para NPS para importar as políticas de solicitação de conexão e a configuração do servidor em todos os outros proxies.

## <a name="plan-nps-accounting"></a>Planejar contabilidade do NPS

Ao configurar o NPS como um proxy RADIUS, você pode configurá-lo para executar a contabilização RADIUS usando arquivos de log de formato do NPS, arquivos de log de formato compatível com Banco de dados ou log de SQL Server do NPS.

Você também pode encaminhar mensagens de contabilidade para um grupo de servidores remotos RADIUS que executa a contabilidade usando um desses formatos de log.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento para a contabilidade do NPS, você pode usar as etapas a seguir.

- Determine se você deseja que o proxy NPS execute serviços de contabilidade ou encaminhe mensagens contábeis para um grupo de servidores RADIUS remoto para contabilidade.

- Planeje desabilitar a contabilização de proxy NPS local se você planeja encaminhar mensagens de contabilidade para outros servidores.

- Planejar etapas de configuração da política de solicitação de conexão se você planeja encaminhar mensagens de contabilidade para outros servidores. Se você desabilitar a contabilidade local para o proxy NPS, cada política de solicitação de conexão configurada nesse proxy deverá ter o encaminhamento de mensagens de contabilidade habilitado e configurado corretamente.

- Determine o formato de log que você deseja usar: Arquivos de log de formato do IAS, arquivos de log de formato compatível com Banco de dados ou log de SQL Server do NPS.

Para configurar o balanceamento de carga para NPS como um proxy RADIUS, consulte [balanceamento de carga do servidor proxy NPS](nps-manage-proxy-lb.md).

