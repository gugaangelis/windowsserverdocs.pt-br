---
title: Permissão de acesso
description: Este tópico fornece uma visão geral de permissão de acesso de política de rede para o servidor de política de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d6d1ca5e-bde0-4509-9e14-dc3fa9ff447e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: aeacfaeb159598d2e53bac29fb09ffc3e7739476
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="access-permission"></a>Permissão de acesso

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Permissão de acesso é configurado no **visão geral** guia de cada política de rede no servidor de política de rede (NPS). 

Esta configuração permite que você configure a política para conceder ou negar o acesso aos usuários se as condições e restrições da diretiva de rede correspondem à solicitação de conexão. 

As configurações de permissão de acesso tem o seguinte efeito:

- **Conceder acesso**. O acesso é concedido se a solicitação de conexão corresponde às condições e restrições que estão configuradas na política.
- **Negar acesso**. Acesso será negado se a solicitação de conexão corresponder às condições e restrições que estão configuradas na política.

Permissão de acesso é concedido ou negado também com base em sua configuração das propriedades da discagem rápida de cada conta de usuário.

>[!NOTE]
>Contas de usuário e suas propriedades, como propriedades de discagem rápida, são configuradas ambos os usuários do Active Directory e computadores ou o Console de gerenciamento Microsoft grupos e usuários locais \(MMC\) snap-in, dependendo se você tiver o Active Directory&reg; Domain Services (AD DS) instalado.

A configuração de conta de usuário **permissão de acesso de rede**, que está configurado nas propriedades de discagem rápida de contas de usuário, substituições de configuração de permissão de acesso da política de rede. Quando a permissão de acesso de rede em uma conta de usuário é definida como o **controlar o acesso por meio da política de rede NPS** opção, a configuração de permissão de acesso de política de rede determina se o usuário é concedido ou negado acesso.

>[!NOTE]
>No Windows Server 2016, o valor padrão de **permissão de acesso de rede** no AD DS usuário dial-propriedades da conta é **controlar o acesso por meio da política de rede NPS**.

Quando o NPS avalia as solicitações de conexão contra políticas de rede configuradas, ele executa as seguintes ações:

- Se as condições da primeira diretiva não forem correspondentes, o NPS avalia a próxima diretiva e continua esse processo até que uma correspondência for encontrada ou todas as diretivas tenham sido avaliadas para uma correspondência.
- Se as condições e restrições de uma política forem atendidas, o NPS concede ou nega o acesso, dependendo do valor da configuração na política de permissão de acesso.
- Se as condições de uma correspondência de política, mas as restrições na diretiva não corresponderem, o NPS rejeita a solicitação de conexão.
- Se as condições de todas as políticas não corresponderem, o NPS rejeita a solicitação de conexão.

## <a name="ignore-user-account-dial-in-properties"></a>Ignorar propriedades de discagem rápida da conta de usuário

Você pode configurar a política de rede NPS para ignorar as propriedades de discagem rápida de contas de usuário marcando ou desmarcando a **Ignorar propriedades de discagem rápida da conta de usuário** caixa de seleção no **visão geral** guia de uma política de rede. 

Normalmente quando o NPS realiza a autorização de uma solicitação de conexão, ele verifica as propriedades de discagem rápida da conta de usuário, onde a permissão de acesso de rede definindo o valor pode afetar se o usuário está autorizado para se conectar à rede. Quando você configura o NPS para ignorar as propriedades de discagem de contas de usuário durante a autorização, configurações de política de rede determinam se o usuário recebe acesso à rede.

As propriedades de discagem rápida de contas de usuário contêm o seguinte:

- Permissão de acesso de rede
- ID do autor
- Opções de retorno de chamada
- Endereço IP estático
- Rotas estáticas

Para dar suporte a vários tipos de conexões para o qual o NPS fornece autenticação e autorização, talvez seja necessário desabilitar o processamento de propriedades de discagem rápida da conta de usuário. Isso pode ser feito para dar suporte a cenários nos quais propriedades de discagem específicas não são necessárias.

Por exemplo, a ID do autor, retorno de chamada, endereço IP estático e rotas estáticas propriedades são projetadas para um cliente que está discando em um servidor de acesso de rede \(NAS\), não para clientes que estejam conectados para acesso sem fio pontos. Um ponto de acesso sem fio que recebe essas configurações em uma mensagem RADIUS de NPS pode não ser capaz de processá-las, que pode fazer com que o cliente sem fio seja desconectado.

Quando o NPS fornece autenticação e autorização para usuários que estão discando e acessando a rede da organização por meio de pontos de acesso sem fio, você deve configurar as propriedades de discagem rápida para dar suporte a qualquer uma das conexões de discagem \ (pela configuração discagem properties\) ou conexões sem fio \ (definindo não discagem properties\).

Você pode usar o NPS para habilitar as propriedades de discagem para a conta de usuário em alguns cenários de processamento \ (como dial-in\) e desabilitar a propriedades de discagem em outros cenários de processamento \ (como 802.1 X switch\ de autenticação e sem fio).

Você também pode usar **Ignorar propriedades de discagem rápida da conta de usuário** para gerenciar o controle de acesso à rede por meio de grupos e a configuração de permissão de acesso a política de rede. Quando você seleciona o **Ignorar propriedades de discagem rápida da conta de usuário** caixa de seleção, permissão de acesso de rede na conta de usuário é ignorada.

A única desvantagem para essa configuração é que você não pode usar as usuário adicionais conta dial-propriedades da ID do autor, retorno de chamada, endereço IP estático e rotas estáticas.

Para obter mais informações sobre o NPS, consulte [servidor de política de rede (NPS)](nps-top.md).
