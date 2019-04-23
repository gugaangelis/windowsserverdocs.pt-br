---
ms.assetid: b8df1828-5ead-4c90-b0fe-95c675116b7c
title: Criar um design de unidade organizacional
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9ecd0228b50a4e597c3fa1dbd3fdaf1f84ca1d3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830397"
---
# <a name="creating-an-organizational-unit-design"></a>Criar um design de unidade organizacional

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os proprietários de floresta são responsáveis por criar designs de unidade organizacional (UO) para seus domínios. Criar um design de UO envolve projetar a estrutura de UO, atribuindo a função de proprietário de UO e criar contas e recursos UOs.  
  
Inicialmente, projetar a estrutura de UO para habilitar a delegação de administração. Quando o design de UO é concluído, você pode criar estruturas de UO adicionais para o aplicativo de diretiva de grupo para os usuários e computadores e para limitar a visibilidade dos objetos. Para obter mais informações, consulte Projetando uma infra-estrutura de diretiva de grupo ([https://go.microsoft.com/fwlink/?LinkId=106655](https://go.microsoft.com/fwlink/?LinkId=106655)).  
  
## <a name="ou-owner-role"></a>Função de proprietário de UO  
O proprietário da floresta designa um proprietário de UO para cada UO que você cria para o domínio. Os proprietários de UO são gerentes de data que controlam uma subárvore de objetos nos serviços de domínio Active Directory (AD DS). Os proprietários de UO podem controlar como a administração for delegada e como a política é aplicada aos objetos dentro da UO respectiva. Também podem criar novas subárvores e delegar a administração de OUs dentro dessas subárvores.  
  
Como proprietários OU não possui ou controla a operação do serviço de diretório, você pode separar a administração do serviço de diretório e propriedade de propriedade e administração de objetos, reduzindo o número de administradores de serviço com altos níveis de acesso.  
  
OUs fornecem os meios para controlar a visibilidade dos objetos no diretório e autonomia administrativa. As UOs fornecem isolamento de outros administradores de dados, mas eles não fornecem isolamento dos administradores de serviço. Embora os proprietários de UO têm controle sobre uma subárvore de objetos, o proprietário da floresta mantém controle total sobre todas as subárvores. Isso permite que o proprietário da floresta para corrigir os erros, como um erro em uma lista de controle de acesso (ACL) e para recuperar a Admins de subárvores quando os administradores de dados são encerrados.  
  
## <a name="account-ous-and-resource-ous"></a>Conta de UOs e UOs de recursos  
Conta UOs contêm objetos de computador, usuário e grupo. Proprietários da floresta devem criar uma estrutura de UO para gerenciar esses objetos e, em seguida, delegar o controle da estrutura para o proprietário de UO. Se você estiver implantando um novo domínio do AD DS, crie uma conta OU para o domínio para que você possa delegar controle das contas no domínio.  
  
UOs de recursos contém recursos e as contas que são responsáveis por gerenciar esses recursos. O proprietário da floresta também é responsável pela criação de uma estrutura de UO para gerenciar esses recursos e para delegar o controle dessa estrutura para o proprietário de UO. Crie recurso UOs conforme necessário com base nos requisitos de cada grupo em sua organização para autonomia no gerenciamento de dados e o equipamento.  
  
## <a name="documenting-the-ou-design-for-each-domain"></a>Como documentar o design de UO para cada domínio  
Monte uma equipe para criar a estrutura de UO que você usa para delegar o controle sobre os recursos dentro da floresta. O proprietário da floresta pode estar envolvido no processo de design e deve aprovar o design de UO. Você também pode envolver a pelo menos um administrador de serviço para garantir que ele é válido. Outros participantes da equipe de design podem incluir os administradores de dados que funcionarão em UOs e a UO proprietários que serão responsáveis por gerenciá-los.  
  
É importante documentar seu projeto de UO. Liste os nomes das UOs que você planeja criar. Além disso, para cada UO, o tipo de unidade Organizacional, o proprietário da UO, OU (se aplicável) pai e a origem da UO do documento.  
  
Para uma planilha ajudar a documentar seu projeto de UO, baixar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip do trabalho auxílios para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) e abra " Identificando UOs para cada domínio"(DSSLOGI_9.doc).  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Revisando os conceitos de Design de UO](../../ad-ds/plan/Reviewing-OU-Design-Concepts.md)  
  
-   [Delegando a administração por meio de objetos de UO](../../ad-ds/plan/Delegating-Administration-by-Using-OU-Objects.md)  
  


