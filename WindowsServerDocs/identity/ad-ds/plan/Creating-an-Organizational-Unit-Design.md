---
ms.assetid: b8df1828-5ead-4c90-b0fe-95c675116b7c
title: Criar um design de unidade organizacional
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 96fd3dd2d090ef6b39b99962e6b639bf2abdcb62
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947737"
---
# <a name="creating-an-organizational-unit-design"></a>Criar um design de unidade organizacional

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os proprietários da floresta são responsáveis por criar designs de UO (unidade organizacional) para seus domínios. A criação de um design de UO envolve a criação da estrutura de UO, a atribuição da função de proprietário da UO e a criação de UOs de contas e recursos.

Inicialmente, projete sua estrutura de UO para habilitar a delegação de administração. Quando o design da UO for concluído, você poderá criar estruturas de UO adicionais para a aplicação de Política de Grupo aos usuários e computadores e limitar a visibilidade dos objetos. Para obter mais informações, consulte [projetando uma infraestrutura de política de grupo](/previous-versions/windows/it-pro/windows-server-2003/cc786524(v=ws.10)).

## <a name="ou-owner-role"></a>Função de proprietário da UO
O proprietário da floresta designa um proprietário da UO para cada UO que você cria para o domínio. Os proprietários da UO são gerenciadores de dados que controlam uma subárvore de objetos no Active Directory Domain Services (AD DS). Os proprietários da UO podem controlar como a administração é delegada e como a política é aplicada aos objetos dentro de sua UO. Eles também podem criar novas subárvores e delegar a administração de UOs dentro dessas subárvores.

Como os proprietários da UO não possuem ou controlam a operação do serviço de diretório, você pode separar a propriedade e a administração do serviço de diretório da propriedade e da administração de objetos, reduzindo o número de administradores de serviço que têm altos níveis de acesso.

As UOs fornecem autonomia administrativa e o meio de controlar a visibilidade dos objetos no diretório. As UOs fornecem isolamento de outros administradores de dados, mas não fornecem isolamento de administradores de serviço. Embora os proprietários da UO tenham controle sobre uma subárvore de objetos, o proprietário da floresta retém controle total sobre todas as subárvores. Isso permite que o proprietário da floresta corrija erros, como um erro em uma ACL (lista de controle de acesso), e recupere subárvores delegadas quando os administradores de dados são encerrados.

## <a name="account-ous-and-resource-ous"></a>UOs de conta e UOs de recurso
As UOs de conta contêm objetos de usuário, grupo e computador. Os proprietários da floresta devem criar uma estrutura de UO para gerenciar esses objetos e delegar o controle da estrutura ao proprietário da UO. Se você estiver implantando um novo domínio de AD DS, crie uma UO de conta para o domínio para que você possa delegar o controle das contas no domínio.

As UOs de recurso contêm recursos e as contas que são responsáveis por gerenciar esses recursos. O proprietário da floresta também é responsável por criar uma estrutura de UO para gerenciar esses recursos e delegar o controle dessa estrutura ao proprietário da UO. Crie UOs de recurso conforme necessário com base nos requisitos de cada grupo dentro de sua organização para autonomia no gerenciamento de dados e equipamentos.

## <a name="documenting-the-ou-design-for-each-domain"></a>Documentando o design de UO para cada domínio
Monte uma equipe para criar a estrutura da UO que você usa para delegar o controle sobre os recursos na floresta. O proprietário da floresta pode estar envolvido no processo de design e deve aprovar o design da UO. Você também pode envolver pelo menos um administrador de serviços para garantir que o design seja válido. Outros participantes da equipe de design podem incluir os administradores de dados que trabalharão nas UOs e os proprietários da UO que serão responsáveis por gerenciá-los.

É importante documentar seu design de UO. Liste os nomes das UOs que você planeja criar. E, para cada UO, documente o tipo de UO, o proprietário da UO, a UO pai (se aplicável) e a origem dessa UO.

Para uma planilha para ajudá-lo a documentar seu design de UO, baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de [ajudas de trabalho para o Windows Server 2003 Deployment Kit](https://microsoft.com/download/details.aspx?id=9608) e abra "identificando UOs para cada domínio" (DSSLOGI_9.doc).

## <a name="in-this-section"></a>Nesta seção

- [Como examinar conceitos de design da UO](../../ad-ds/plan/Reviewing-OU-Design-Concepts.md)

- [Como delegar a administração usando objetos da UO](../../ad-ds/plan/Delegating-Administration-by-Using-OU-Objects.md)
