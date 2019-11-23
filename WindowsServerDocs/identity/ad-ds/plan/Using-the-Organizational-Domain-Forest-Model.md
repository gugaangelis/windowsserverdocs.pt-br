---
ms.assetid: 093ef1ae-ebc1-490f-9fb1-2c000ce89eb6
title: Usando o modelo de floresta de domínio organizacional
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: af5fd2da396ecc27db68d3be8d1c0eda82314d6f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402446"
---
# <a name="using-the-organizational-domain-forest-model"></a>Usando o modelo de floresta de domínio organizacional

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

No modelo de floresta de domínio organizacional, vários grupos autônomos têm um domínio em uma floresta. Cada grupo controla a administração do serviço no nível do domínio, o que permite que eles gerenciem determinados aspectos do gerenciamento de serviços de forma autônoma, enquanto o proprietário da floresta controla o gerenciamento de serviços no nível da floresta.  

A ilustração a seguir mostra um modelo de floresta de domínio organizacional.  

![usando o modelo de floresta de domínio da organização](../../media/Using-the-Organizational-Domain-Forest-Model/c50a3c6a-b0e4-43ec-ad62-f05d05f0bbd2.gif)  

## <a name="domain-level-service-autonomy"></a>Autonomia do serviço de nível de domínio

O modelo de floresta do domínio organizacional permite a delegação de autoridade para o gerenciamento de serviços no nível do domínio. A tabela a seguir lista os tipos de gerenciamento de serviços que podem ser controlados no nível de domínio.  

|Tipo de gerenciamento de serviços|Tarefas associadas|  
|------------------------------|--------------------|  
|Gerenciamento de operações do controlador de domínio|-Criando e removendo controladores de domínio<br />-Monitorando o funcionamento de controladores de domínio<br />-Gerenciando serviços que estão sendo executados em controladores de domínio<br />-Fazendo backup e restaurando o diretório|  
|Configuração de configurações de todo o domínio|-Criando políticas de conta de usuário de domínio e domínio, como senha, Kerberos e políticas de bloqueio de conta<br />-Criando e aplicando Política de Grupo em todo o domínio|  
|Delegação de administração de nível de dados|-Criando UOs (unidades organizacionais) e delegando a administração<br />-Reparo de problemas na estrutura da UO que os proprietários da UO não têm direitos de acesso suficientes para corrigir|  
|Gerenciamento de relações de confiança externas|-Estabelecendo relações de confiança com domínios fora da floresta|  

Outros tipos de gerenciamento de serviços, como o gerenciamento de topologia de replicação ou de esquema, são de responsabilidade do proprietário da floresta.  

## <a name="domain-owner"></a>Proprietário do domínio

Em um modelo de floresta de domínio organizacional, os proprietários de domínio são responsáveis por tarefas de gerenciamento de serviço de nível de domínio. Os proprietários de domínio têm autoridade sobre todo o domínio, bem como acesso a todos os outros domínios na floresta. Por esse motivo, os proprietários de domínio devem ser indivíduos confiáveis selecionados pelo proprietário da floresta.  

Delegar o gerenciamento de serviços em nível de domínio a um proprietário de domínio, se as seguintes condições forem atendidas:  

- Todos os grupos que participam da floresta confiam no novo proprietário do domínio e nas práticas de gerenciamento de serviço do novo domínio.  

- O novo proprietário do domínio confia no proprietário da floresta e em todos os outros proprietários do domínio.  

- Todos os proprietários de domínio na floresta concordam que o novo proprietário de domínio tem políticas de seleção e gerenciamento de administrador de serviços e práticas que são iguais ou mais estritas do que suas próprias.  

- Todos os proprietários de domínio na floresta concordam que os controladores de domínio gerenciados pelo novo proprietário de domínio no novo domínio são fisicamente protegidos.  

Observe que, se um proprietário de floresta delegar o gerenciamento de serviço no nível de domínio para um proprietário de domínio, outros grupos poderão optar por não ingressar na floresta se não confiarem no proprietário do domínio.  

Todos os proprietários de domínio devem estar cientes de que, se qualquer uma dessas condições mudar no futuro, poderá ser necessário mover os domínios organizacionais para uma implantação de várias florestas.  

> [!NOTE]  
> Outra maneira de minimizar os riscos de segurança para um domínio de Active Directory do Windows Server 2008 é empregar a separação de funções de administrador, o que exige a implantação de um RODC (controlador de domínio somente leitura) em sua infraestrutura de Active Directory. Um RODC é um novo tipo de controlador de domínio no sistema operacional Windows Server 2008 que hospeda partições somente leitura do banco de dados do Active Directory. Antes do lançamento do Windows Server 2008, qualquer trabalho de manutenção de servidor em um controlador de domínio precisava ser executado por um administrador de domínio. No Windows Server 2008, você pode delegar permissões administrativas locais para um RODC a qualquer usuário de domínio sem conceder a esse usuário quaisquer direitos administrativos para o domínio ou outros controladores de domínio. Isso permite que o usuário delegado faça logon em um RODC e execute o trabalho de manutenção, como a atualização de um driver, no servidor. No entanto, esse usuário delegado não pode fazer logon em nenhum outro controlador de domínio nem executar outra tarefa administrativa no domínio. Dessa forma, qualquer usuário confiável pode ser delegado a capacidade de gerenciar o RODC com eficiência sem comprometer a segurança do restante do domínio. Para obter mais informações sobre RODCs, consulte [AD DS: controladores de domínio somente leitura](https://go.microsoft.com/fwlink/?LinkId=106616).  
