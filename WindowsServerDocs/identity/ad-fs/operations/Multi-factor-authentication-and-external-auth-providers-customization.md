---
title: "A autenticação multifator e personalização de provedores de autenticação externa"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 08724d45-9be4-4c56-a5f1-2cf40864e136
ms.technology: identity-adfs
ms.openlocfilehash: 6d06c017601003e3b93df32f5fa50190ce54541d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="multi-factor-authentication-and-external-authentication-providers-customization"></a>A autenticação multifator e personalização de provedores de autenticação externa 

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

No AD FS, o suporte para autenticação multifator é fornecido out\-of\-the\-box. Por exemplo, você pode configurar o AD FS para usar autenticação de certificado built\-in como a segunda autenticação de fator. Você também pode usar provedores de autenticação externa. Essa abordagem pode habilitar o AD FS integrar com serviços adicionais, como Azure a autenticação multifator, ou você pode desenvolver seu próprio provedor. Consulte [guia de solução: gerenciar o risco com controle de acesso do fator de Multi\](https://technet.microsoft.com/library/dn280937.aspx) para obter mais informações sobre como registrar o provedor de autenticação externo usando o AD FS.  
  
Nós recomendamos que um provedor de autenticação externo use as classes que são definidas no arquivo. CSS que o AD FS fornece para criar a interface do usuário de autenticação. Você pode usar o seguinte cmdlet para exportar o tema da web padrão e inspecione as classes de interface do usuário e elementos que são definidos no arquivo. CSS. O arquivo. CSS pode ser usado no desenvolvimento de interface do usuário sign\-in de um provedor de autenticação externa.  
  

    Export-AdfsWebTheme -Name default -DirectoryPath C:\theme  
 
  
A seguir está um exemplo da interface do usuário sign\-in, que está realçado em vermelho, por um provedor de autenticação externa. A interface do usuário usa as classes de interface do usuário no arquivo. CSS AD FS.  
  
![AD FS e MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom8.png)  
  
Antes de gravar um novo método de autenticação personalizado, recomendamos que você estudar as definições de tema e estilo do AD FS para entender o requisitos de criação de conteúdo.  
  
-   Um método de autenticação personalizado só cria um segmento HTML na página do AD FS sign\-in e não a página inteira. Você deve usar a definição de estilo do AD FS para obter a aparência e comportamento consistentes.  
  
![AD FS e MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom9.png)  
  
-   Lembre-se de que os administradores do AD FS podem personalizar os estilos do AD FS. . Não recomendamos para codificar seus próprios estilos. Em vez disso, recomendamos usar estilos do AD FS sempre que possível.  
  
-   Out\ of\-pronta, estilos do AD FS são criados com um estilo \(LTR\) left\ to\-direito e uma \(RTL\) right\-to\-left. Os administradores podem personalizar ambos e podem fornecer estilos específicos de language\ por meio da definição do tema da web. Cada folha de estilos tem três seções com os respectivos comentários:  
  
    -   **estilos de tema** \-esses estilos não devem estar conectados e não pode ser usados. Esses estilos devem definir o tema em todas as páginas. Eles são definidos por uma ID do elemento propositadamente para que eles não são reutilizados.  
  
    -   **estilos comuns** \-esses são os estilos que devem ser usados por seu conteúdo.  
  
    -   **estilos de fator de forma** \-esses são estilos para diferentes fatores forma. Você deve compreender nesta seção para garantir que seu conteúdo funcione com diferentes fatores forma, por exemplo, telefones e tablets.  
  
Para obter informações adicionais, consulte [guia de solução: gerenciar o risco com controle de acesso do fator de Multi\](https://technet.microsoft.com/library/dn280937.aspx) e [guia de solução: gerenciar o risco com autenticação de fator de Multi\ adicionais para aplicativos confidenciais](https://tnstage.redmond.corp.microsoft.com/library/dn280949.aspx).  

## <a name="additional-references"></a>Referências adicionais 
[AD FS usuário entrar personalização](AD-FS-user-sign-in-customization.md) 
