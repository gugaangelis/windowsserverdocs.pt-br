---
title: Processo de implantação de acesso sem fio
description: Este tópico faz parte do guia de rede do Windows Server 2016 "Implantar baseada em senha 802.1 X autenticado sem fio acesso"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2555f238-926e-4b20-9bfb-9774831062da
author: shortpatti
ms.author: pashort
ms.openlocfilehash: fcdbe796e4d530604dfec448e817eab877d34c4f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment-process"></a>Processo de implantação de acesso sem fio

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

O processo que você usa para implantar o acesso sem fio ocorre em desses estágios:

## <a name="stage-1--ap-deployment"></a>Estágio 1 – implantação PA

Planejar, implantar e configurar os pontos de acesso para conectividade sem fio do cliente e para uso com NPS. Dependendo de suas preferência e dependências da rede, você pode qualquer pre\-definir as configurações em seu PAS antes de instalá-los em sua rede sem fio, ou você pode configurá-los remotamente após a instalação.

## <a name="stage-2--ad-ds-group-configuration"></a>Etapa 2 – a configuração de grupo do AD DS

No AD DS, você deve criar um ou mais usuários sem fio grupos de segurança.

Em seguida, identifique os usuários que têm permissão de acesso sem fio à rede.

Por fim, adicione os usuários aos grupos de segurança aos usuários apropriados de sem fio que você criou.

>[!NOTE]
>Por padrão, o **permissão de acesso de rede** nas propriedades de discagem rápida da conta de usuário está definida com a configuração **controlar o acesso por meio da política de rede NPS**. A menos que você tenha motivos específicos para alterar essa configuração, é recomendável que você mantenha o padrão. Isso permite que você controle o acesso à rede com as políticas de rede que você configurar no NPS.

## <a name="stage-3--group-policy-configuration"></a>Etapa 3 – a configuração de política de grupo

Configurar a rede com fio \ (IEEE 802.11\) extensão de políticas da política de grupo usando o Editor de gerenciamento de política de grupo Microsoft Management Console \(MMC\).

Para configurar computadores membro domain\ usando as configurações na seção políticas de rede sem fio, você deve aplicar a política de grupo. Quando um computador pela primeira vez é ingressado no domínio, política de grupo é aplicada automaticamente. Se alterações são feitas em política de grupo, as novas configurações serão aplicadas automaticamente:

- Pela política de grupo em intervalos de determinado pre\

- Se um usuário de domínio faz logoff e, em seguida, faça logon na rede

- Reiniciando o computador cliente e fazer logon no domínio

Você também pode impor a política de grupo para atualizar enquanto estiver conectado a um computador executando o comando **gpupdate** no prompt de comando.

## <a name="stage-4--nps-server-configuration"></a>Etapa 4 – a configuração do servidor NPS

Use um Assistente de configuração no NPS adicionar pontos de acesso sem fio como clientes RADIUS e criar as políticas de rede NPS usa ao processar solicitações de conexão.

Ao usar o Assistente para criar as políticas de rede, especifique PEAP como o tipo EAP e o grupo de segurança de usuários sem fio que foi criado na segunda etapa.

## <a name="stage-5--deploy-wireless-clients"></a>Etapa 5 – implantar clientes sem fio

Use computadores cliente para se conectar à rede.

Para computadores de membro do domínio que podem fazer logon rede local com fio, as configurações sem fio necessárias são aplicadas automaticamente quando a diretiva de grupo é atualizada.

Se você tiver ativado a configuração de rede sem fio \ (IEEE 802.11\) políticas para se conectar automaticamente quando o computador estiver no intervalo de transmissão da rede sem fio, sua vontade de computadores ingressados em domain\ sem fio, em seguida, automaticamente tentam se conectar com a rede sem fio.

Para se conectar à rede sem fio, os usuários só precisam fornecer suas usuário nome e a senha credenciais de domínio quando solicitado pelo Windows.

Para planejar a implantação de acesso sem fio, consulte [planejamento de implantação de acesso sem fio](d-wireless-access-planning.md).
