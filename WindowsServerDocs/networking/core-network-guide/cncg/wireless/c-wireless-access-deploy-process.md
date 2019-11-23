---
title: Processo da implantação de acesso sem fio
description: Este tópico faz parte do guia de rede do Windows Server 2016 "implantar o acesso sem fio autenticado 802.1 X com base em senha"
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2555f238-926e-4b20-9bfb-9774831062da
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 10c69e1aa6157c7f088f190c0283b33b630bc25f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406254"
---
# <a name="wireless-access-deployment-process"></a>Processo da implantação de acesso sem fio

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

O processo que você usa para implantar o acesso sem fio ocorre nestes estágios:

## <a name="stage-1--ap-deployment"></a>Estágio 1 – implantação de AP

Planeje, implante e configure seus APs para conectividade de cliente sem fio e para uso com o NPS. Dependendo de suas dependências de preferência e de rede, você pode\-definir configurações em seus APs sem fio antes de instalá-los em sua rede, ou você pode configurá-los remotamente após a instalação.

## <a name="stage-2--adds-group-configuration"></a>Etapa 2 – configuração do grupo de AD DS

No AD DS, você deve criar um ou mais grupos de segurança de usuários sem fio.

Em seguida, identifique os usuários que têm permissão de acesso sem fio à rede.

Por fim, adicione os usuários aos grupos de segurança apropriados de usuários sem fio que você criou.

>[!NOTE]
>Por padrão, a configuração de **permissão de acesso à rede** nas propriedades de discagem da conta de usuário é definida com a configuração controlar o **acesso por meio da política de rede do NPS**. A menos que você tenha motivos específicos para alterar essa configuração, é recomendável manter o padrão. Isso permite que você controle o acesso à rede por meio das políticas de rede que você configura no NPS.

## <a name="stage-3--group-policy-configuration"></a>Etapa 3 – configuração do Política de Grupo

Configure a rede sem fio \(extensão de políticas de\) IEEE 802,11 do Política de Grupo usando o Editor de Gerenciamento de Política de Grupo console de gerenciamento Microsoft \(MMC\).

Para configurar computadores membros de\-de domínio usando as configurações nas políticas de rede sem fio, você deve aplicar Política de Grupo. Quando um computador é ingressado no domínio pela primeira vez, Política de Grupo é aplicado automaticamente. Se forem feitas alterações a Política de Grupo, as novas configurações serão aplicadas automaticamente:

- Por Política de Grupo em intervalos determinados de\-

- Se um usuário de domínio fizer logoff e, em seguida, voltar à rede

- Reiniciando o computador cliente e fazendo logon no domínio

Você também pode forçar o Política de Grupo a atualizar enquanto estiver conectado a um computador executando o comando **gpupdate** no prompt de comando.

## <a name="stage-4--nps-configuration"></a>Etapa 4 – configuração do NPS

Use um assistente de configuração no NPS para adicionar pontos de acesso sem fio como clientes RADIUS e para criar as políticas de rede que o NPS usa ao processar solicitações de conexão.

Ao usar o assistente para criar as diretivas de rede, especifique PEAP como o tipo de EAP e o grupo de segurança usuários sem fio que foi criado no segundo estágio.

## <a name="stage-5--deploy-wireless-clients"></a>Estágio 5 – implantar clientes sem fio

Use computadores cliente para se conectar à rede.

Para computadores membros do domínio que podem fazer logon na LAN com fio, as definições de configuração sem fio necessárias são aplicadas automaticamente quando Política de Grupo é atualizada.

Se você habilitou a configuração em rede sem fio \(as políticas do IEEE 802,11\) para se conectar automaticamente quando o computador estiver dentro do intervalo de difusão da rede sem fio, seus computadores sem fio,\-de domínio, serão automaticamente tentados a se conectar à LAN sem fio.

Para se conectar à rede sem fio, os usuários precisam apenas fornecer suas credenciais de nome de usuário e senha de domínio quando solicitado pelo Windows.

Para planejar sua implantação de acesso sem fio, consulte [planejamento de implantação de acesso sem fio](d-wireless-access-planning.md).
