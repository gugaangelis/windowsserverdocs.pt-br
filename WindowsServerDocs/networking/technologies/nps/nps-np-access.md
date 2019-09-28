---
title: Permissões de acesso
description: Este tópico fornece uma visão geral da permissão de acesso de política de rede para o servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d6d1ca5e-bde0-4509-9e14-dc3fa9ff447e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8a63bf7d277108a528a3ab1c20263e31b59c1547
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405379"
---
# <a name="access-permission"></a>Permissões de acesso

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

A permissão de acesso é configurada na guia **visão geral** de cada diretiva de rede no servidor de diretivas de rede (NPS). 

Essa configuração permite que você configure a política para conceder ou negar acesso a usuários se as condições e restrições da política de rede forem correspondidas pela solicitação de conexão. 

As configurações de permissão de acesso têm o seguinte efeito:

- **Conceder acesso**. O acesso será concedido se a solicitação de conexão corresponder às condições e restrições configuradas na política.
- **Negar acesso**. O acesso será negado se a solicitação de conexão corresponder às condições e restrições configuradas na política.

A permissão de acesso também é concedida ou negada com base na configuração das propriedades de discagem de cada conta de usuário.

>[!NOTE]
>As contas de usuário e suas propriedades, como propriedades de discagem, são configuradas na Active Directory usuários e computadores ou nos usuários e grupos locais console de gerenciamento Microsoft \(MMC @ no__t-1 snap-in, dependendo se você estiver ativo Diretório @ no__t-2 Domain Services (AD DS) instalado.

A **permissão de acesso à rede**da configuração de conta de usuário, que é configurada nas propriedades de discagem de contas de usuário, substitui a configuração de permissão de acesso à política de rede. Quando a permissão de acesso à rede em uma conta de usuário é definida como a opção **controlar o acesso por meio da política de rede do NPS** , a configuração de permissão de acesso de política de rede determina se o acesso ao usuário foi concedido ou negado.

>[!NOTE]
>No Windows Server 2016, o valor padrão da **permissão de acesso à rede** em AD DS Propriedades de discagem da conta de usuário é **controlar o acesso por meio da política de rede do NPS**.

Quando o NPS avalia as solicitações de conexão em relação às políticas de rede configuradas, ele executa as seguintes ações:

- Se as condições da primeira política não forem correspondidas, o NPS avaliará a próxima política e continuará esse processo até que seja encontrada uma correspondência ou todas as políticas tenham sido avaliadas para uma correspondência.
- Se as condições e restrições de uma política forem correspondidas, o NPS concederá ou negará o acesso, dependendo do valor da configuração de permissão de acesso na política.
- Se as condições de uma política corresponderem, mas as restrições na política não corresponderem, o NPS rejeitará a solicitação de conexão.
- Se as condições de todas as políticas não corresponderem, o NPS rejeitará a solicitação de conexão.

## <a name="ignore-user-account-dial-in-properties"></a>Ignorar Propriedades de discagem da conta de usuário

Você pode configurar a política de rede do NPS para ignorar as propriedades de discagem de contas de usuário marcando ou desmarcando a caixa de seleção **ignorar Propriedades de discagem da conta de usuário** na guia **visão geral** de uma política de rede. 

Normalmente, quando o NPS executa a autorização de uma solicitação de conexão, ele verifica as propriedades de discagem da conta de usuário, em que o valor da configuração de permissão de acesso à rede pode afetar se o usuário está autorizado a se conectar à rede. Quando você configura o NPS para ignorar as propriedades de discagem de contas de usuário durante a autorização, as configurações de diretiva de rede determinam se o usuário tem acesso concedido à rede.

As propriedades de discagem de contas de usuário contêm o seguinte:

- Permissão de acesso à rede
- ID do chamador
- Opções de retorno de chamada
- Endereço IP estático
- Rotas estáticas

Para dar suporte a vários tipos de conexões para as quais o NPS fornece autenticação e autorização, pode ser necessário desabilitar o processamento de propriedades de discagem da conta de usuário. Isso pode ser feito para dar suporte a cenários em que Propriedades de discagem específicas não são necessárias.

Por exemplo, as propriedades chamador-ID, retorno de chamada, endereço IP estático e rotas estáticas são projetadas para um cliente que está discando para um servidor de acesso à rede \(NAS @ no__t-1, não para clientes que estão se conectando a pontos de acesso sem fio. Um ponto de acesso sem fio que recebe essas configurações em uma mensagem RADIUS do NPS pode não ser capaz de processá-las, o que pode fazer com que o cliente sem fio seja desconectado.

Quando o NPS fornece autenticação e autorização para usuários que estão discando e acessando a rede da sua organização por meio de pontos de acesso sem fio, você deve configurar as propriedades de discagem para dar suporte a conexões de discagem @no__t configuração-0by Propriedades de discagem @ no__t-1 ou conexões sem fio \(by não definir propriedades de discagem @ no__t-3.

Você pode usar o NPS para habilitar o processamento de propriedades de discagem para a conta de usuário em alguns cenários \(such como dial-in @ no__t-1 e para desabilitar o processamento de propriedades de discagem em outros cenários \(such como o 802.1 X sem fio e o comutador de autenticação @ no__t-3.

Você também pode usar **ignorar Propriedades de discagem da conta de usuário** para gerenciar o controle de acesso à rede por meio de grupos e a configuração de permissão de acesso na política de rede. Quando você marca a caixa de seleção **ignorar Propriedades de discagem de conta de usuário** , a permissão de acesso à rede na conta de usuário é ignorada.

A única desvantagem dessa configuração é que você não pode usar as propriedades de discagem da conta de usuário adicional de Caller-ID, retorno de chamada, endereço IP estático e rotas estáticas.

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).
