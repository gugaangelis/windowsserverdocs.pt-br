---
title: Planejar NPS como um proxy RADIUS
description: Este tópico fornece informações sobre a implantação de proxy de RADIUS do servidor de diretiva de rede planejamento no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca77d64a-065b-4bf2-8252-3e75f71b7734
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 83fbe57ee62480439190dcc53428e02a4f8e6897
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829557"
---
# <a name="plan-nps-as-a-radius-proxy"></a>Planejar NPS como um proxy RADIUS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Quando você implanta o servidor de diretivas de rede (NPS) como um Remote Authentication Dial-In User Service \(RADIUS\) proxy, o NPS recebe solicitações de conexão de clientes RADIUS, como servidores de acesso à rede ou outros proxies RADIUS e, em seguida, encaminha essas solicitações de conexão para servidores que executam o NPS ou outros servidores RADIUS. Você pode usar essa diretrizes de planejamento para simplificar a implantação do RADIUS.

Essa diretrizes de planejamento não incluem as circunstâncias em que você deseja implantar o NPS como servidor RADIUS. Quando você implanta o NPS como servidor RADIUS, o NPS executa autenticação, autorização e contabilização para solicitações de conexão para o domínio local e para os domínios que confiam no domínio local.

Antes de implantar o NPS como um proxy RADIUS em sua rede, use as seguintes diretrizes para planejar a implantação.

- Planeje a configuração do NPS.

- Plano de clientes RADIUS.

- Planeje grupos de servidores RADIUS remotos.

- Planeje as regras de manipulação de atributos para encaminhamento de mensagens.

- Planeje as diretivas de solicitação de conexão.

- Planeje a contabilidade NPS.

## <a name="plan-nps-configuration"></a>Planejar a configuração do NPS

Quando você usa o NPS como um proxy RADIUS, o NPS encaminha solicitações de conexão para um NPS ou outros servidores RADIUS para processamento. Por isso, a associação de domínio do proxy com o NPS é irrelevante. O proxy não precisa ser registrado no Active Directory Domain Services \(AD DS\) porque ela não precisa ter acesso às propriedades de discagem das contas de usuário. Além disso, você não precisará configurar políticas de rede em um proxy do NPS, porque o proxy não executar a autorização para solicitações de conexão. O proxy NPS pode ser um membro do domínio ou pode ser um servidor autônomo com nenhuma associação de domínio.

O NPS deve ser configurado para se comunicar com clientes RADIUS, também chamados de servidores de acesso de rede, usando o protocolo RADIUS. Além disso, você pode configurar os tipos de eventos que registra o NPS no evento de log e você pode inserir uma descrição para o servidor.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento para a configuração de proxy do NPS, você pode usar as etapas a seguir.

- Determine as portas RADIUS que o proxy NPS usa para receber mensagens RADIUS dos clientes RADIUS e para enviar mensagens RADIUS para membros de grupos de servidores RADIUS remotos. As portas de protocolo UDP (User Datagram) padrão são 1812 e 1645 para mensagens de autenticação RADIUS e as portas UDP 1813 e 1646 para mensagens de contabilização RADIUS.

- Se o proxy NPS está configurado com vários adaptadores de rede, determine os adaptadores sobre o qual você deseja que o tráfego RADIUS seja permitido.

- Determine os tipos de eventos que você deseja que o NPS para gravar no Log de eventos. Você pode registrar solicitações de conexão rejeitada, solicitações de conexão bem-sucedida ou ambos.

- Determine se você estiver implantando mais de um proxy do NPS. Para fornecer tolerância a falhas, use pelo menos dois proxies NPS. Um proxy do NPS é usado como o proxy RADIUS primário e o outro é usado como um backup. Cada cliente RADIUS está configurado, em seguida, em ambos os proxies do NPS. Se o proxy NPS primário ficar indisponível, clientes RADIUS, em seguida, enviam mensagens de solicitação de acesso para o proxy NPS alternativo.

- Planeje o script usado para copiar uma configuração de proxy do NPS para outros proxies do NPS para economizar em sobrecarga administrativa e evitar a configuração incorreta de um servidor. NPS fornece os comandos do Netsh que permitem que você copiar todas ou parte de uma configuração de proxy do NPS para importação em outro proxy NPS. Você pode executar manualmente os comandos no prompt do Netsh. No entanto, se você salvar sua sequência de comando como um script, você pode executar o script em uma data posterior se você decidir alterar suas configurações de proxy.

## <a name="plan-radius-clients"></a>Planejar clientes RADIUS

Os clientes RADIUS são servidores de acesso de rede, como pontos de acesso sem fio, rede virtual privada \(VPN\) servidores, 802.1 comutadores compatíveis e servidores dial-up. Os proxies RADIUS que encaminham mensagens de solicitação em servidores RADIUS de conexão, também são clientes RADIUS. O NPS dá suporte a todos os servidores de acesso de rede e proxies RADIUS que são compatíveis com o protocolo RADIUS, conforme descrito no RFC 2865, "Remote Authentication Dial-in User Service \(RADIUS\)," e RFC 2866, "Contabilidade do RADIUS".

Além disso, os pontos de acesso sem fio e comutadores devem ser capazes de autenticação 802.1 X. Se você quiser implantar Protected Extensible Authentication Protocol (PEAP) ou o protocolo EAP (Extensible Authentication), comutadores e pontos de acesso devem suportar o uso de EAP.

Para testar a interoperabilidade básica para conexões PPP para pontos de acesso sem fio, configure o ponto de acesso e o cliente de acesso para usar o protocolo de autenticação de senha (PAP). Use protocolos de autenticação adicionais baseadas em PPP, como PEAP, até que você tenha testado aqueles que você pretende usar para acesso à rede.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento para clientes RADIUS, você pode usar as etapas a seguir.

- Documente os atributos específicos do fornecedor (VSAs), que você deve configurar no NPS. Se precisam de sua NASs VSAs, registrar as informações de VSA para uso posterior quando você configurar suas políticas de rede no NPS.

- Documente os endereços IP de clientes RADIUS e o proxy do NPS para simplificar a configuração de todos os dispositivos. Quando você implanta os clientes RADIUS, você deve configurá-los para usar o protocolo RADIUS, com o endereço IP do proxy NPS inserido como o servidor de autenticação. E, quando você configura o NPS para se comunicar com os clientes RADIUS, você deve inserir os endereços IP do cliente RADIUS no snap-in do NPS.

- Crie os segredos compartilhados para configuração nos clientes RADIUS e o snap-in NPS. Você deve configurar clientes RADIUS com um segredo compartilhado, ou senha, o que você também vai inserir no snap-in NPS ao configurar clientes RADIUS no NPS.

## <a name="plan-remote-radius-server-groups"></a>Planejar grupos de servidores remotos RADIUS

Quando você configura um grupo de servidores RADIUS remotos em um proxy do NPS, você está dizendo ao proxy NPS para onde enviar a conexão de algumas ou todas as mensagens de solicitação recebidos dos servidores de acesso de rede e proxies NPS ou outros proxies RADIUS.

Você pode usar o NPS como um proxy RADIUS para encaminhar a conexão solicita a uma ou mais remotos grupos de servidores RADIUS e cada grupo podem conter um ou mais servidores RADIUS. Quando você quiser que o proxy do NPS para encaminhar mensagens para vários grupos, configure uma política de solicitação de conexão por grupo. A diretiva de solicitação de conexão contém informações adicionais, como regras de manipulação de atributo, o que dizer quais mensagens para enviar para o grupo de servidores remotos RADIUS especificado na política de proxy NPS.

Você pode configurar grupos de servidores RADIUS remotos, usando os comandos Netsh para NPS, ao configurar grupos diretamente no snap-in NPS em grupos de servidores RADIUS remotos ou executando o Assistente de nova diretiva de solicitação de Conexão.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento para grupos de servidores remotos RADIUS, você pode usar as etapas a seguir.

- Determine os domínios que contêm os servidores RADIUS ao qual você deseja que o proxy do NPS para encaminhar solicitações de conexão. Esses domínios contêm as contas de usuário para usuários que se conectam à rede por meio de clientes RADIUS que você implanta.

- Determine se você precisa adicionar novos servidores RADIUS em domínios onde RADIUS já não está implantada.

- Documente os endereços IP dos servidores RADIUS que você deseja adicionar a grupos de servidores RADIUS remotos.

- Determine quantos grupos de servidores RADIUS remotos você precisa criar. Em alguns casos, é melhor criar um grupo de servidores RADIUS remotos por domínio e, em seguida, adicione os servidores RADIUS para o domínio ao grupo. No entanto, pode haver casos em que você tem uma grande quantidade de recursos em um domínio, incluindo um grande número de usuários com contas de usuário no domínio, um grande número de controladores de domínio e um grande número de servidores RADIUS. Ou seu domínio pode abranger uma grande área geográfica, fazendo com que você tiver servidores de acesso de rede e servidores RADIUS em locais que estão distantes uns dos outros. Nessas e possivelmente outros casos, você pode criar vários grupos de servidores RADIUS remotos por domínio.

- Crie os segredos compartilhados para configuração no proxy NPS e nos servidores RADIUS remotos.

## <a name="plan-attribute-manipulation-rules-for-message-forwarding"></a>Planejar as regras de manipulação de atributos para encaminhamento de mensagens

Regras de manipulação de atributos que são definidas nas diretivas de solicitação de conexão, permitem que você identifique as mensagens de solicitação de acesso que você deseja encaminhar para um grupo específico de servidores remotos RADIUS.

Você pode configurar o NPS para encaminhar todas as solicitações de conexão para um grupo de servidores RADIUS remotos sem o uso de regras de manipulação de atributos.

Se você tiver mais de um local ao qual você deseja encaminhar solicitações de conexão, no entanto, você deve criar uma diretiva de solicitação de conexão para cada local, em seguida, configure a política com o grupo de servidores remotos RADIUS para o qual você deseja encaminhar mensagens, bem como com as regras de manipulação de atributos que informam ao NPS quais mensagens para encaminhar.

Você pode criar regras para os atributos a seguir.

- Chamado-ID da estação-. O número de telefone do servidor de acesso de rede (NAS). O valor desse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência de padrões para especificar códigos de área.

- Chamada de estação de ID O número de telefone usado pelo chamador. O valor desse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência de padrões para especificar códigos de área.

- Nome de usuário. O nome de usuário fornecido pelo cliente de acesso, que é incluído por na mensagem de solicitação de acesso RADIUS. O valor desse atributo é uma cadeia de caracteres que geralmente contém um nome de realm e um nome de conta de usuário.

Para substituir ou converter nomes de território no nome do usuário de uma solicitação de conexão corretamente, você deve configurar regras de manipulação de atributos para o atributo de nome de usuário na diretiva de solicitação de conexão apropriado.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento para regras de manipulação de atributos, você pode usar as etapas a seguir.

- Planeje o roteamento de mensagens do por meio do proxy para servidores remotos RADIUS para verificar se você tem um caminho lógico com o qual encaminhar mensagens para os servidores RADIUS.

- Determine um ou mais atributos que você deseja usar para cada diretiva de solicitação de conexão.

- Documente as regras de manipulação de atributos que você planeja usar para cada diretiva de solicitação de conexão e correspondem às regras para o grupo de servidores remotos RADIUS para o qual as mensagens são encaminhadas.

## <a name="plan-connection-request-policies"></a>Planejar as diretivas de solicitação de conexão

A diretiva de solicitação de conexão padrão está configurada para o NPS quando ele é usado como um servidor RADIUS. As diretivas de solicitação de conexão adicionais podem ser usadas para definir condições mais específicas, crie regras de manipulação que dizem ao NPS quais mensagens para encaminhar para grupos de servidores RADIUS remotos e para especificar atributos avançados de atributo. Use o Assistente de nova diretiva de solicitação de Conexão para criar diretivas de solicitação de conexão comum ou personalizado.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento para diretivas de solicitação de conexão, você pode usar as etapas a seguir.

- Exclua a política de solicitação de conexão padrão em cada servidor que executa o NPS que funciona somente como um proxy RADIUS.

- Planeje as condições adicionais e configurações que são necessárias para cada política, combinar essas informações com o grupo de servidores remotos RADIUS e as regras de manipulação de atributo planejadas para a política.

- Crie o plano para distribuir as diretivas de solicitação de conexão comum a todos os proxies do NPS. Criar políticas comuns a vários proxies do NPS em um NPS e, em seguida, usar os comandos Netsh para NPS para importar a configuração de políticas e o servidor da solicitação de conexão em todos os outros proxies.

## <a name="plan-nps-accounting"></a>Planejar a contabilidade NPS

Quando você configura o NPS como um proxy RADIUS, você pode configurar nele para realizar o RADIUS estatísticas por meio do NPS SQL Server, arquivos de log de formato compatível com o banco de dados ou arquivos de log de formato NPS registro em log.

Você também pode encaminhar mensagens de estatísticas para um grupo de servidores remotos RADIUS que executa as estatísticas, usando um desses formatos de log.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento para a contabilidade NPS, você pode usar as etapas a seguir.

- Determine se você deseja que o proxy do NPS para executar serviços de estatísticas ou para encaminhar mensagens de estatísticas para um grupo de servidores remotos RADIUS para contabilidade.

- Planeje desabilitar o proxy NPS local se planejar encaminhar mensagens de estatísticas para outros servidores de contabilização.

- Planejar etapas de configuração de diretiva de solicitação de conexão se planejar encaminhar mensagens de estatísticas para outros servidores. Se você desabilitar contabilidade local para o proxy do NPS, cada diretiva de solicitação de conexão que você configura o proxy deve ter o encaminhamento de mensagens de contabilização habilitado e configurado corretamente.

- Determine o formato de log que você deseja usar: Arquivos de log de formato do IAS, arquivos de log de formato compatível com o banco de dados ou registro em log do SQL Server do NPS.

Para configurar o balanceamento de carga para o NPS como um proxy RADIUS, consulte [NPS Proxy de servidor de balanceamento de carga de](nps-manage-proxy-lb.md).

