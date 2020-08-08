---
title: Processo da implantação de acesso sem fio
description: Este tópico faz parte do guia de rede do Windows Server 2016 "implantar o acesso sem fio autenticado 802.1 X com base em senha"
manager: brianlic
ms.topic: article
ms.assetid: 2555f238-926e-4b20-9bfb-9774831062da
author: eross-msft
ms.author: lizross
ms.openlocfilehash: 9c2326df824288b6adf4453d6ef272ba632eb6c2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969592"
---
# <a name="wireless-access-deployment-process"></a>Processo da implantação de acesso sem fio

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

O processo que você usa para implantar o acesso sem fio ocorre nestes estágios:

## <a name="stage-1--ap-deployment"></a>Estágio 1 – implantação de AP

Planeje, implante e configure seus APs para conectividade de cliente sem fio e para uso com o NPS. Dependendo de suas dependências de rede e de preferência, você pode pré-configurar \- as configurações em seus APS sem fio antes de instalá-los em sua rede, ou você pode configurá-los remotamente após a instalação.

## <a name="stage-2--adds-group-configuration"></a>Etapa 2 – configuração do grupo de AD DS

No AD DS, você deve criar um ou mais grupos de segurança de usuários sem fio.

Em seguida, identifique os usuários que têm permissão de acesso sem fio à rede.

Por fim, adicione os usuários aos grupos de segurança apropriados de usuários sem fio que você criou.

>[!NOTE]
>Por padrão, a configuração de **permissão de acesso à rede** nas propriedades de discagem da conta de usuário é definida com a configuração controlar o **acesso por meio da política de rede do NPS**. A menos que você tenha motivos específicos para alterar essa configuração, é recomendável manter o padrão. Isso permite que você controle o acesso à rede por meio das políticas de rede que você configura no NPS.

## <a name="stage-3--group-policy-configuration"></a>Etapa 3 – configuração do Política de Grupo

Configure a \( extensão de políticas IEEE 802,11 de rede sem fio \) do política de grupo usando o editor de gerenciamento de política de grupo MMC do console de gerenciamento Microsoft \( \) .

Para configurar \- computadores membros do domínio usando as configurações nas políticas de rede sem fio, você deve aplicar política de grupo. Quando um computador é ingressado no domínio pela primeira vez, Política de Grupo é aplicado automaticamente. Se forem feitas alterações a Política de Grupo, as novas configurações serão aplicadas automaticamente:

- Por Política de Grupo em \- intervalos predeterminados

- Se um usuário de domínio fizer logoff e, em seguida, voltar à rede

- Reiniciando o computador cliente e fazendo logon no domínio

Você também pode forçar o Política de Grupo a atualizar enquanto estiver conectado a um computador executando o comando **gpupdate** no prompt de comando.

## <a name="stage-4--nps-configuration"></a>Etapa 4 – configuração do NPS

Use um assistente de configuração no NPS para adicionar pontos de acesso sem fio como clientes RADIUS e para criar as políticas de rede que o NPS usa ao processar solicitações de conexão.

Ao usar o assistente para criar as diretivas de rede, especifique PEAP como o tipo de EAP e o grupo de segurança usuários sem fio que foi criado no segundo estágio.

## <a name="stage-5--deploy-wireless-clients"></a>Estágio 5 – implantar clientes sem fio

Use computadores cliente para se conectar à rede.

Para computadores membros do domínio que podem fazer logon na LAN com fio, as definições de configuração sem fio necessárias são aplicadas automaticamente quando Política de Grupo é atualizada.

Se você habilitou a configuração em políticas IEEE 802,11 de rede sem fio \( \) para se conectar automaticamente quando o computador estiver dentro do intervalo de difusão da rede sem fio, seus computadores sem fio e \- ingressados no domínio tentarão automaticamente se conectar à LAN sem fio.

Para se conectar à rede sem fio, os usuários precisam apenas fornecer suas credenciais de nome de usuário e senha de domínio quando solicitado pelo Windows.

Para planejar sua implantação de acesso sem fio, consulte [planejamento de implantação de acesso sem fio](d-wireless-access-planning.md).
