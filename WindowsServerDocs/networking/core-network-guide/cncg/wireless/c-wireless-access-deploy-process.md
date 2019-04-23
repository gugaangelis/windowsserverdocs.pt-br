---
title: Processo da implantação de acesso sem fio
description: Este tópico faz parte do guia de rede do Windows Server 2016 "Implantar baseado em senha 802.1 X acesso autenticado sem fio"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2555f238-926e-4b20-9bfb-9774831062da
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 6a286cf10e066043ee6f514bbf468bfb2b13f162
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879837"
---
# <a name="wireless-access-deployment-process"></a>Processo da implantação de acesso sem fio

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

O processo que você usa para implantar o acesso sem fio ocorre nesses estágios:

## <a name="stage-1--ap-deployment"></a>Estágio 1 – implantação de AP

Planejar, implantar e configurar os pontos de acesso para conectividade de cliente sem fio e para uso com o NPS. Dependendo de sua preferência e dependências da rede, você pode pré-\-definir as configurações em seus PAS sem fio antes de instalá-los em sua rede, ou você pode configurá-los remotamente após a instalação.

## <a name="stage-2--adds-group-configuration"></a>Etapa 2 – configuração do grupo do AD DS

No AD DS, você deve criar grupos de segurança de um ou mais usuários sem fio.

Em seguida, identifique os usuários que têm permissão de acesso sem fio à rede.

Por fim, adicione os usuários aos grupos de segurança de usuários sem fio apropriados que você criou.

>[!NOTE]
>Por padrão, o **permissão de acesso de rede** nas propriedades de discagem da conta de usuário está definida com a configuração **controlar o acesso por meio da diretiva de rede do NPS**. A menos que você tenha motivos específicos para alterar essa configuração, é recomendável que você mantenha o padrão. Isso permite que você controle o acesso à rede por meio de políticas de rede que você configura no NPS.

## <a name="stage-3--group-policy-configuration"></a>Etapa 3 – configuração de política de grupo

Configurar a rede sem fio \(IEEE 802.11\) extensão de diretivas de diretiva de grupo usando o Editor de gerenciamento de diretiva de grupo Microsoft Management Console \(MMC\).

Configurar domínio\-computadores de membro usando as configurações nas diretivas de rede sem fio, você deve aplicar a política de grupo. Quando um computador pela primeira vez é associado ao domínio, diretiva de grupo é aplicada automaticamente. Se forem feitas alterações à política de grupo, as novas configurações são aplicadas automaticamente:

- Pela diretiva de grupo no pré\-determinados intervalos

- Se um usuário de domínio faz logoff e, em seguida, fazer logon na rede

- Reiniciando o computador cliente e fazer logon domínio

Você também pode forçar a diretiva de grupo para atualizar enquanto estiver conectado a um computador executando o comando **gpupdate** no prompt de comando.

## <a name="stage-4--nps-configuration"></a>Etapa 4 – configuração do NPS

Use um Assistente de configuração no NPS, adicione pontos de acesso sem fio como clientes RADIUS e para criar as políticas de rede que o NPS usa durante o processamento de solicitações de conexão.

Ao usar o Assistente para criar as políticas de rede, especifique o PEAP como o tipo de EAP e o grupo de segurança de usuários sem fio que foi criado no segundo estágio.

## <a name="stage-5--deploy-wireless-clients"></a>Etapa 5 – implantar clientes sem fio

Use computadores cliente para se conectar à rede.

Para computadores de membro de domínio que podem fazer logon em LAN com fio, as definições de configuração sem fio necessárias são aplicadas automaticamente quando a política de grupo é atualizada.

Se você tiver habilitado a configuração de rede sem fio \(IEEE 802.11\) políticas para se conectar automaticamente quando o computador estiver dentro do intervalo de rede sem fio, seu sem fio, o domínio de difusão\-computadores Unidos serão, em seguida, automaticamente tenta se conectar à rede local sem fio.

Para se conectar à rede sem fio, os usuários só precisam fornecer suas credenciais usuário do domínio nome e a senha quando solicitado pelo Windows.

Para planejar sua implantação do acesso sem fio, consulte [planejamento de implantação de acesso sem fio](d-wireless-access-planning.md).
