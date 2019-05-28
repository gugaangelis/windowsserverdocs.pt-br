---
title: A autenticação multifator e personalização de provedores de autenticação externa
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 08724d45-9be4-4c56-a5f1-2cf40864e136
ms.technology: identity-adfs
ms.openlocfilehash: 347b4783e82a6561334f8757029b1fddec6a85a3
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189078"
---
# <a name="multi-factor-authentication-and-external-authentication-providers-customization"></a>A autenticação multifator e personalização de provedores de autenticação externa 



No AD FS, o suporte para a autenticação multifator é fornecido fora\-dos\-o\-caixa. Por exemplo, você pode configurar o AD FS para uso incorporada\-na autenticação de certificado como a autenticação de dois fatores. Você também pode usar provedores de autenticação externa. Essa abordagem pode permitir que o AD FS integrar com serviços adicionais, como a autenticação multifator, ou você pode desenvolver seu próprio provedor. Consulte [guia de solução: Gerencie riscos com vários\-fatorar o controle de acesso](https://technet.microsoft.com/library/dn280937.aspx) para obter mais informações sobre como registrar o provedor de autenticação externa usando o AD FS.  
  
É recomendável que um provedor de autenticação externa use as classes que são definidas no arquivo. CSS que o AD FS fornece para criar a interface do usuário de autenticação. Você pode usar o seguinte cmdlet para exportar o tema da Web padrão e inspecionar as classes de interface do usuário e os elementos definidos no arquivo .css. O arquivo. CSS pode ser usado no desenvolvimento do sinal de\-na interface do usuário de um provedor de autenticação externa.  
  

    Export-AdfsWebTheme -Name default -DirectoryPath C:\theme  
 
  
A seguir está um exemplo de como o sinal\-na interface do usuário, que é destacado em vermelho, por um provedor de autenticação externa. A interface do usuário usa as classes de interface do usuário no arquivo. CSS do AD FS.  
  
![AD FS e MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom8.png)  
  
Antes de gravar um novo método de autenticação personalizada, é recomendável que você estude as definições de tema e um estilo de AD FS para entender os requisitos de criação de conteúdo.  
  
-   Um método de autenticação personalizado somente cria um segmento HTML na entrada do AD FS\-na página e não a página inteira. Você deve usar a definição de estilo do AD FS para obter a aparência e comportamento consistentes.  
  
![AD FS e MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom9.png)  
  
-   Lembre-se de que os administradores do AD FS podem personalizar os estilos do AD FS. . Não recomendamos codificar seus estilos próprios. Em vez disso, é recomendável para usar o AD FS estilos sempre que possível.  
  
-   -Out\-de\-caixa, estilos de AD FS são criados com o que resta\-para\-direita \(LTR\) estilo e uma para a direita\-para\-esquerdo \(RTL\). Os administradores podem personalizar ambos e pode fornecer a linguagem\-estilos específicos por meio da definição de tema da web. Cada folha de estilo possui três seções com os respectivos comentários:  
  
    -   **estilos de tema** \- esses estilos não devem e não pode ser usados. Esses estilos são destinados a definir o tema em todas as páginas. Eles são definidos por um ID de elemento intencionalmente, de forma que nunca sejam reutilizados.  
  
    -   **estilos comuns** \- são estilos que devem ser usados para o seu conteúdo.  
  
    -   **estilos de fator forma** \- esses são estilos para diferentes fatores forma. Você deve compreender esta seção para assegurar que seu conteúdo funcione com diferentes fatores forma, por exemplo, telefones e tablets.  
  
Para obter mais informações, consulte [guia de solução: Gerencie riscos com vários\-fatorar o controle de acesso](https://technet.microsoft.com/library/dn280937.aspx) e [guia de solução: Gerencie riscos com adicional Multi\-fatores de autenticação para aplicativos confidenciais](https://tnstage.redmond.corp.microsoft.com/library/dn280949.aspx).  

## <a name="additional-references"></a>Referências adicionais 
[AD FS Sign-personalização de usuário](AD-FS-user-sign-in-customization.md) 
