---
title: Permissões de acesso
description: Este tópico fornece uma visão geral de permissão de acesso de diretiva de rede para o servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d6d1ca5e-bde0-4509-9e14-dc3fa9ff447e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cdec41fb7925061bb8c8402634e1d9b1625bf301
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849487"
---
# <a name="access-permission"></a>Permissões de acesso

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Permissão de acesso está configurada na **visão geral** guia de cada política de rede no servidor de diretivas de rede (NPS). 

Essa configuração permite que você configure a política para conceder ou negar acesso a usuários se as condições e restrições da diretiva de rede são correspondidas pela solicitação de conexão. 

Configurações de permissão de acesso têm o seguinte efeito:

- **Conceder acesso**. O acesso é concedido se a solicitação de conexão corresponde às condições e restrições que são configuradas na política.
- **Negar acesso**. Acesso negado se a solicitação de conexão corresponde às condições e restrições que são configuradas na política.

Permissão de acesso também é concedida ou negada com base em sua configuração das propriedades dial-in de cada conta de usuário.

>[!NOTE]
>Contas de usuário e suas propriedades, como propriedades de discagem, são configuradas no tanto os usuários do Active Directory e computadores ou os usuários locais e grupos Microsoft Management Console \(MMC\) snap-in, dependendo se você tiver Active Directory&reg; Domain Services (AD DS) instalado.

A configuração de conta de usuário **permissão de acesso de rede**, que é configurado nas propriedades de discagem das contas de usuário, substituições de configuração de permissão de acesso de diretiva de rede. Quando a permissão de acesso de rede em uma conta de usuário é definida como o **controlar o acesso por meio da diretiva de rede do NPS** opção, a configuração de permissão de acesso de diretiva de rede determina se o usuário é concedido ou negado acesso.

>[!NOTE]
>No Windows Server 2016, o valor padrão de **permissão de acesso de rede** no AD DS propriedades conta de usuário dial-in é **controlar o acesso por meio da diretiva de rede do NPS**.

Quando o NPS avalia as solicitações de conexão em relação às políticas de rede configurado, ele executa as seguintes ações:

- Se as condições da primeira diretiva não forem atendidas, o NPS avalia a próxima diretiva e continua esse processo até que uma correspondência for encontrada ou todas as políticas foram avaliadas para uma correspondência.
- Se as condições e restrições de uma política forem atendidas, o NPS concede ou nega acesso, dependendo do valor da configuração de permissão de acesso na política.
- Se as condições de uma correspondência de política, mas as restrições na política não corresponderem, o NPS rejeita a solicitação de conexão.
- Se as condições de todas as políticas não corresponderem, o NPS rejeita a solicitação de conexão.

## <a name="ignore-user-account-dial-in-properties"></a>Ignorar propriedades de discagem da conta de usuário

Você pode configurar a política de rede do NPS para ignorar as propriedades de discagem das contas de usuário marcando ou desmarcando as **Ignorar propriedades de discagem da conta de usuário** caixa de seleção a **visão geral** guia de uma rede política. 

Normalmente quando o NPS realiza a autorização de uma solicitação de conexão, ele verifica as propriedades de discagem da conta de usuário, em que a permissão de acesso de rede definindo o valor pode afetar se o usuário está autorizado a se conectar à rede. Quando você configura o NPS para ignorar as propriedades de discagem das contas de usuário durante a autorização, configurações de diretiva de rede determinam se o usuário será concedido o acesso à rede.

As propriedades de discagem das contas de usuário contêm o seguinte:

- Permissão de acesso de rede
- ID do chamador
- Opções de retorno de chamada
- Endereço IP estático
- Rotas estáticas

Para dar suporte a vários tipos de conexões para o qual o NPS fornece autenticação e autorização, ele pode ser necessário desabilitar o processamento de propriedades de discagem da conta de usuário. Isso pode ser feito para dar suporte a cenários em que as propriedades de discagem específicas não são necessárias.

Por exemplo, a ID do chamador, retorno de chamada, endereço IP estático e as propriedades de rotas estáticas são projetadas para um cliente que está discando em um servidor de acesso de rede \(NAS\), e não para clientes que se conectam a pontos de acesso sem fio. Um ponto de acesso sem fio que recebe essas configurações em uma mensagem RADIUS do NPS pode não conseguir processá-las, que pode fazer com que o cliente sem fio a ser desconectado.

Quando o NPS fornece autenticação e autorização para usuários que estão discando e acessando a rede da sua organização por meio de pontos de acesso sem fio, você deve configurar as propriedades de discagem para dar suporte tanto a conexões dial-in \(por Definindo propriedades de discagem\) ou conexões sem fio \(não definindo propriedades de discagem\).

Você pode usar o NPS para habilitar as propriedades de discagem da conta de usuário em alguns cenários de processamento \(, como dial-in\) e para desativar as propriedades de discagem em outros cenários de processamento \(como o 802.1X sem fio e chave de autenticação\).

Você também pode usar **Ignorar propriedades de discagem da conta de usuário** para gerenciar o controle de acesso à rede por meio de grupos e a configuração de permissão de acesso na diretiva de rede. Quando você seleciona os **Ignorar propriedades de discagem da conta de usuário** caixa de seleção, permissão de acesso de rede na conta de usuário será ignorada.

A única desvantagem a essa configuração é que você não pode usar as adicionais de usuário dial-in de propriedades da conta de ID do chamador, o retorno de chamada, o endereço IP estático e rotas estáticas.

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).
