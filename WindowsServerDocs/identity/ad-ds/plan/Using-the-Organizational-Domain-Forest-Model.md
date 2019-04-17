---
ms.assetid: 093ef1ae-ebc1-490f-9fb1-2c000ce89eb6
title: "Usando o modelo de floresta de domínio organizacional"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 22d871d9157622375619dd90336e597d4bfb3d68
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="using-the-organizational-domain-forest-model"></a>Usando o modelo de floresta de domínio organizacional

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

No modelo de floresta de domínio organizacional, vários autônomos grupos cada possuem um domínio em uma floresta. Cada grupo de controles de administração de serviço de nível de domínio, que permite gerenciar determinados aspectos do gerenciamento de serviços autonomia enquanto o proprietário de floresta controla o gerenciamento de serviços de nível de floresta.  
  
A ilustração a seguir mostra um modelo de floresta de domínio organizacional.  
  
![usando o modelo de floresta de domínio de organograma](../../media/Using-the-Organizational-Domain-Forest-Model/c50a3c6a-b0e4-43ec-ad62-f05d05f0bbd2.gif)  
  
## <a name="domain-level-service-autonomy"></a>Autonomia de serviço no nível do domínio  
O modelo de floresta de domínio organizacional permite a delegação de autoridade de gerenciamento de serviços de nível de domínio. A tabela a seguir lista os tipos de gerenciamento de serviços que podem ser controlados no nível do domínio.  
  
|Tipo de gerenciamento de serviço|Tarefas associadas|  
|------------------------------|--------------------|  
|Gerenciamento de operações de controlador de domínio|-Criando e removendo controladores de domínio<br />-Monitoramento o funcionamento de controladores de domínio<br />-Gerenciamento de serviços que são executados em controladores de domínio<br />-Fazendo backup e restaurando o diretório|  
|Configuração de configurações de todo o domínio|-Criação de domínio e do usuário de domínio políticas de conta, como senha, Kerberos e políticas de bloqueio de conta<br />-Criar e aplicar a política de grupo de todo o domínio|  
|Delegação de administração de nível de dados|-Criar unidades organizacionais (UOs) e delegação de administração<br />-Reparar problemas na estrutura de UO proprietários de UO não têm direitos de acesso suficientes para corrigir|  
|Gerenciamento de relações de confiança externas|-Estabelecer relações de confiança com domínios fora da floresta|  
  
Outros tipos de gerenciamento de serviços, como o esquema ou gerenciamento de topologia de replicação, são de responsabilidade do proprietário floresta.  
  
## <a name="domain-owner"></a>Proprietário do domínio  
Em um modelo de floresta de domínio organizacional, os proprietários de domínio são responsáveis por tarefas de gerenciamento de serviço de nível de domínio. Os proprietários de domínio tem autoridade ao longo de todo o domínio, bem como acesso a todos os outros domínios na floresta. Por esse motivo, os proprietários de domínio devem ser selecionados pelo proprietário floresta de indivíduos confiáveis.  
  
Delegar o gerenciamento de serviço de nível de domínio para um proprietário do domínio, se as seguintes condições forem atendidas:  
  
-   Todos os grupos de participação na floresta confiar no novo proprietário do domínio e as práticas de gerenciamento de serviço do novo domínio.  
  
-   O novo proprietário do domínio em que confia o proprietário de floresta e todos os outros proprietários de domínio.  
  
-   Todos os proprietários de domínio na floresta concordam que o novo proprietário do domínio tem gerenciamento do administrador de serviço e políticas de seleção e práticas recomendadas que são iguais ou mais restrito que seus próprios.  
  
-   Todos os proprietários de domínio na floresta concordam que os controladores de domínio gerenciados pelo proprietário novo domínio no novo domínio são fisicamente seguros.  
  
Observe que se uma floresta proprietário delegados nível de domínio gerenciamento de serviço para um proprietário do domínio, outros grupos podem optar por não ingressar floresta se eles não confia esse proprietário do domínio.  
  
Todos os proprietários de domínio devem estar cientes de que se alguma destas condições mudar no futuro, poderá ficar necessário mover os domínios organizacionais para uma implantação de floresta vários.  
  
> [!NOTE]  
> Outra maneira de reduzir os riscos de segurança em um domínio do Active Directory do Windows Server 2008 é empregar separação de função de administrador, que requer a implantação de um controlador de domínio somente leitura (RODC) em sua infraestrutura do Active Directory. Um RODC é um novo tipo de controlador de domínio no sistema operacional Windows Server 2008 que hospeda partições somente leitura do banco de dados do Active Directory. Antes do lançamento do Windows Server 2008, nenhum trabalho de manutenção do servidor em um controlador de domínio precisava ser executadas por um administrador de domínio. No Windows Server 2008, você pode delegar permissões administrativas locais para um RODC para qualquer usuário do domínio sem conceder os direitos administrativos para o domínio ou outros controladores de domínio. Isso permite que o usuário delegado para fazer logon em um RODC e executar o trabalho de manutenção, como atualizar um driver, no servidor. No entanto, esse usuário não pode fazer logon para qualquer outro controlador de domínio ou delegado executar qualquer outra tarefa administrativa no domínio. Dessa forma, qualquer trusted usuário pode ser recebido a autoridade adequada para gerenciar efetivamente o RODC sem comprometer a segurança do resto do domínio. Para obter mais informações sobre RODCs, veja AD DS: Read-Only controladores de domínio ([https://go.microsoft.com/fwlink/?LinkId=106616](https://go.microsoft.com/fwlink/?LinkId=106616)).  
  


