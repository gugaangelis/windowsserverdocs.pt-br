---
ms.assetid: b8df1828-5ead-4c90-b0fe-95c675116b7c
title: Criando um Design de unidade organizacional
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3a74770a4558c79d1f9250f37181562e1d235d34
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="creating-an-organizational-unit-design"></a>Criando um Design de unidade organizacional

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os proprietários de floresta são responsáveis por criar designs de unidade organizacional (UO) para seus domínios. Criar um design de UO envolve criar a estrutura de UO, atribuindo a função de proprietário de UO e criar conta e UOs de recursos.  
  
Inicialmente, projete a estrutura de UO para permitir a delegação de administração. Quando o design de UO for concluído, você pode criar estruturas de UO adicionais para o aplicativo de política de grupo para os usuários e computadores e para limitar a visibilidade dos objetos. Para obter mais informações, consulte Projetando uma infraestrutura de política de grupo ([https://go.microsoft.com/fwlink/?LinkId=106655](https://go.microsoft.com/fwlink/?LinkId=106655)).  
  
## <a name="ou-owner-role"></a>Função de proprietário de UO  
O proprietário da floresta designa um proprietário de UO para cada unidade Organizacional que você criar para o domínio. Os proprietários de UO são gerentes de dados que controlam uma subárvore de objetos nos serviços de domínio do Active Directory (AD DS). Os proprietários de UO podem controlar como delegação da administração e como a política é aplicada a objetos dentro de suas unidades Organizacionais. Eles também podem criar novas subárvores e delegar a administração de UOs dentro desses subárvores.  
  
Porque os proprietários de UO não possui ou controla a operação do serviço de diretório, você pode separar a propriedade e a administração do serviço de diretório de propriedade e a administração de objetos, reduzindo o número de administradores de serviços que têm altos níveis de acesso.  
  
UOs fornecem autonomia administrativa e os meios para controlar a visibilidade de objetos no diretório. UOs fornecem isolamento de outros administradores de dados, mas eles não fornecer isolamento de administradores de serviços. Embora os proprietários de UO têm controle sobre uma subárvore de objetos, o proprietário de floresta retém controle total sobre todas as subárvores. Isso permite que o proprietário da floresta para corrigir os erros, por exemplo, um erro em uma lista de controle de acesso (ACL) e para recuperar as subárvores delegadas quando os administradores de dados são encerrados.  
  
## <a name="account-ous-and-resource-ous"></a>UOs de conta e UOs de recursos  
UOs de conta contêm objetos de usuário, grupo e computador. Os proprietários de floresta devem criar uma estrutura de UO para gerenciar esses objetos e, em seguida, delegar controle da estrutura para o proprietário de UO. Se você estiver implantando um novo domínio do AD DS, crie uma conta OU para o domínio para que você pode delegar controle das contas do domínio.  
  
UOs de recursos contêm recursos e as contas que são responsáveis pelo gerenciamento desses recursos. O proprietário da floresta também é responsável por criar uma estrutura de UO para gerenciar esses recursos e delegando o controle de estrutura para o proprietário de UO. Crie UOs de recursos conforme necessário com base nos requisitos de cada grupo em sua organização para autonomia no gerenciamento de dados e o equipamento.  
  
## <a name="documenting-the-ou-design-for-each-domain"></a>Documentar o design de UO para cada domínio  
Monte uma equipe para criar a estrutura de UO que você usa para delegar controle sobre os recursos dentro da floresta. O proprietário da floresta pode se envolver no processo de design e deve aprovar o design de UO. Você também pode envolver pelo menos um administrador de serviço para garantir que o design é válido. Outros participantes da equipe de design podem incluir os administradores de dados que funcionarão nas UOs e na UO proprietários quem serão responsáveis por gerenciá-los.  
  
É importante documentar o design de UO. Lista os nomes das UOs que você pretende criar. Além disso, para cada unidade Organizacional, o tipo de UO, o proprietário da unidade Organizacional, o pai UO (se aplicável) e a origem da UO do documento.  
  
Para uma planilha para ajudá-lo a documentar seu design de UO, baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip).  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Revisando os conceitos de Design de UO](../../ad-ds/plan/Reviewing-OU-Design-Concepts.md)  
  
-   [Delegar a administração usando objetos de UO](../../ad-ds/plan/Delegating-Administration-by-Using-OU-Objects.md)  
  


