---
ms.assetid: 093ef1ae-ebc1-490f-9fb1-2c000ce89eb6
title: Usando o modelo de floresta de domínio organizacional
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 876a15dcdd951e0323fb7ddb7be96317f5512f0f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875907"
---
# <a name="using-the-organizational-domain-forest-model"></a>Usando o modelo de floresta de domínio organizacional

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

No modelo de floresta de domínio organizacional, vários autônomos grupos cada possuem um domínio em uma floresta. Cada grupo de controles de administração do serviço de nível de domínio, que permite que eles gerenciem determinados aspectos do gerenciamento de serviço de forma autônoma enquanto o proprietário da floresta controla o gerenciamento de serviços de nível de floresta.  

A ilustração a seguir mostra um modelo de floresta de domínio organizacional.  

![usando o modelo de floresta de domínio da organização](../../media/Using-the-Organizational-Domain-Forest-Model/c50a3c6a-b0e4-43ec-ad62-f05d05f0bbd2.gif)  

## <a name="domain-level-service-autonomy"></a>Autonomia do serviço de nível de domínio

O modelo de floresta de domínio organizacional permite a delegação de autoridade de gerenciamento de serviços de nível de domínio. A tabela a seguir lista os tipos de gerenciamento de serviços que podem ser controlados no nível do domínio.  

|Tipo de gerenciamento de serviços|Tarefas associadas|  
|------------------------------|--------------------|  
|Gerenciamento de operações do controlador de domínio|-Criando e removendo controladores de domínio<br />-Monitorar o funcionamento dos controladores de domínio<br />-Gerenciamento de serviços que são executados em controladores de domínio<br />-Fazer backup e restaurar o diretório|  
|Definição das configurações de todo o domínio|-Criação de usuário de domínio e domínio diretivas de conta, senha, Kerberos e as políticas de bloqueio de conta<br />-Criando e aplicando a diretiva de grupo em domínios|  
|Delegação de administração de nível de dados|-Criar unidades organizacionais (OUs) e delegação de administração<br />-Reparo dos problemas na estrutura de UO OU proprietários não têm direitos de acesso suficientes para corrigir|  
|Gerenciamento de relações de confiança externas|-Estabelecer relações de confiança com domínios fora da floresta|  

Outros tipos de gerenciamento de serviços, como o esquema ou gerenciamento de topologia de replicação, são de responsabilidade do proprietário da floresta.  

## <a name="domain-owner"></a>Proprietário do domínio

Em um modelo de floresta de domínio organizacional, os proprietários do domínio serão responsáveis por tarefas de gerenciamento de serviço de nível de domínio. Os proprietários do domínio possuem autoridade sobre todo o domínio, bem como acesso a todos os outros domínios na floresta. Por esse motivo, os proprietários do domínio devem ser selecionadas pelo proprietário da floresta de pessoas confiáveis.  

Delegar o gerenciamento de serviços de nível de domínio para o proprietário de um domínio, se as seguintes condições forem atendidas:  

- O novo proprietário do domínio e as práticas de gerenciamento de serviço do novo domínio de confiança todos os grupos que participam da floresta.  

- O novo proprietário do domínio confia que o proprietário da floresta e todos os outros proprietários de domínio.  

- Todos os proprietários de domínio na floresta concordam que o novo proprietário do domínio tem gerenciamento de administradores de serviço e políticas de seleção e práticas recomendadas que são iguais ou mais estrita que seus próprios.  

- Todos os proprietários de domínio na floresta concordam que os controladores de domínio gerenciados pelo proprietário do novo domínio no novo domínio estejam fisicamente protegidos.  

Observe que se uma floresta proprietário delegados nível de domínio gerenciamento de serviços para um proprietário do domínio, outros grupos podem optar por não ingressar nessa floresta, se eles não confiar no proprietário desse domínio.  

Todos os proprietários do domínio devem estar cientes de que se alguma dessas condições mudar no futuro, talvez seja necessário mover os domínios organizacionais em uma implantação de várias florestas.  

> [!NOTE]  
> Outra maneira de minimizar os riscos de segurança em um domínio do Active Directory do Windows Server 2008 é empregar a separação de funções de administrador, que requer a implantação de um controlador de domínio somente leitura (RODC) em sua infraestrutura do Active Directory. Um RODC é um novo tipo de controlador de domínio no sistema operacional Windows Server 2008 que hospeda partições somente leitura do banco de dados do Active Directory. Antes do lançamento do Windows Server 2008, qualquer trabalho de manutenção do servidor em um controlador de domínio deveria ser executada por um administrador de domínio. No Windows Server 2008, você pode delegar permissões administrativas locais para um RODC para qualquer usuário do domínio, sem conceder os direitos administrativos para o domínio ou outros controladores de domínio. Isso permite que o usuário delegado para fazer logon em um RODC e executar o trabalho de manutenção, como atualizar um driver, no servidor. No entanto, esse usuário delegado não é possível fazer logon no outro controlador de domínio ou executar todas as outras tarefas administrativas no domínio. Dessa forma, qualquer confiável usuário pode ser delegadas a capacidade de gerenciar com eficiência o RODC sem comprometer a segurança do resto do domínio. Para obter mais informações sobre RODCs, consulte [AD DS: Controladores de domínio somente leitura](https://go.microsoft.com/fwlink/?LinkId=106616).  
