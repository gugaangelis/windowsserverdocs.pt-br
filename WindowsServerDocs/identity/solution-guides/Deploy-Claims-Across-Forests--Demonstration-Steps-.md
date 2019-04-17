---
ms.assetid: 846c3680-b321-47da-8302-18472be42421
title: "Implantar requerimentos judiciais ou Extrajudiciais entre florestas (etapas de demonstração)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3bab7a396061ecae8a187cc6986d6d026a9e4b32
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-claims-across-forests-demonstration-steps"></a>Implantar requerimentos judiciais ou Extrajudiciais entre florestas (etapas de demonstração)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Neste tópico, cobriremos um cenário básico que explica como configurar as transformações de declarações entre florestas confiantes e confiáveis. Você aprenderá como objetos de política de transformação de declarações podem ser criados e vinculados à relação de confiança da floresta confiante e de floresta confiável. Em seguida, você irá validar o cenário.  
  
## <a name="scenario-overview"></a>Visão geral do cenário  
Adatum Corporation fornece serviços financeiros para Contoso, Ltd. Cada trimestre, contadores Adatum copie suas planilhas de conta em uma pasta em um servidor de arquivos localizado na Contoso, Ltd. Há uma relação de confiança bidirecional configurado da Contoso para Adatum. Contoso, Ltd. deseja proteger o compartilhamento para que somente Adatum funcionários possam acessar o compartilhamento remoto.  
  
Neste cenário:  
  
1.  [Configurar os pré-requisitos e o ambiente de teste](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_1.1)  
  
2.  [Configurar a transformação de declarações em floresta confiável (Adatum)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_3)  
  
3.  [Configurar a transformação de declarações na floresta confiante (Contoso)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_4)  
  
4.  [Validar o cenário](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_5)  
  
## <a name="BKMK_1.1"></a>Configurar os pré-requisitos e o ambiente de teste  
A configuração de teste envolve configurar duas florestas: Adatum Corporation e Contoso, Ltd e ter uma relação de confiança entre Contoso e Adatum bidirecional. "adatum.com" é a floresta confiável e "contoso.com" é a floresta confiável.  
  
O cenário de transformação requerimentos judiciais ou Extrajudiciais demonstra a transformação de uma declaração na floresta confiável a uma reclamação na floresta confiante. Para fazer isso, você precisa configurar uma nova floresta chamado adatum.com e popular da floresta com um usuário de teste com um valor de empresa de 'Adatum'. Em seguida, você precisa configurar uma relação de confiança bidirecional entre contoso.com e adatum.com.  
  
> [!IMPORTANT]  
> Ao configurar as florestas Contoso e Adatum, certifique-se de que ambos os domínios raiz têm o Windows Server 2012 nível funcional do domínio de transformação de declarações para trabalhar.  
  
Você precisa configurar o seguinte para o laboratório. Esses procedimentos são explicados mais detalhadamente em [apêndice b: configuração de backup o ambiente de teste](Appendix-B--Setting-Up-the-Test-Environment.md)  
  
Você precisa implementar os procedimentos a seguir para configurar o laboratório para esse cenário:  
  
1.  [Definir Adatum como floresta confiável para Contoso](Appendix-B--Setting-Up-the-Test-Environment.md)  
  
2.  [Criar o tipo de declaração 'Empresa' na Contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.8)  
  
3.  [Habilitar a propriedade de recurso 'Empresa' em Contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.55)  
  
4.  [Criar a regra de acesso central](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.9)  
  
5.  [Criar a política de acesso central](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.10)  
  
6.  [Publicar a nova política por meio da política de grupo](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.11)  
  
7.  [Crie a pasta de lucros no servidor de arquivos](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.12)  
  
8.  [Definir a classificação e aplicar a política de acesso central na nova pasta](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.13)  
  
Use as seguintes informações para completar esse cenário:  
  
|Objetos|Detalhes|  
|-----------|-----------|  
|Usuários|Jeff baixa, Contoso|  
|Declarações do usuário em Adatum e Contoso|ID: anúncio: / / ext/empresa: ContosoAdatum,<br /><br />Atributo Source: empresa<br /><br />Valores sugeridos: Contoso, Adatum **importante:** você deve definir a ID sobre a empresa tipo de declaração no Contoso e Adatum para ser o mesmo para a transformação de declarações para trabalhar.|  
|Regras de acesso central em Contoso|AdatumEmployeeAccessRule|  
|Política de acesso central em Contoso|Política de acesso único adatum|  
|Políticas de transformação de declarações em Adatum e Contoso|Empresa DenyAllExcept|  
|Pasta de arquivos na Contoso|D:\EARNINGS|  
  
## <a name="BKMK_3"></a>Configurar a transformação de declarações em floresta confiável (Adatum)  
Nesta etapa, você cria uma política de transformação no Adatum para negar todas as reclamações exceto 'Empresa' para passar para Contoso.  
  
O módulo do Active Directory para Windows PowerShell fornece o **DenyAllExcept** argumento, que descarta tudo exceto as declarações especificadas na política de transformação.  
  
Para configurar uma transformação de declarações, você precisa criar uma política de transformação de declarações e vinculá-la entre as florestas confiáveis e confiantes.  
  
### <a name="BKMK_2.2"></a>Criar uma política de transformação de declarações em Adatum  
  
##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-claims-except-company"></a>Para criar uma política de transformação Adatum para negar todas as reclamações exceto 'Empresa'  
  
1.  Fazer logon no controlador de domínio, adatum.com como administrador com a senha **pass@word1**.  
  
2.  Abra um prompt de comando com privilégios elevados no Windows PowerShell e digite o seguinte:  
  
    ```  
    New-ADClaimTransformPolicy `  
    -Description:"Claims transformation policy to deny all claims except Company"`  
    -Name:"DenyAllClaimsExceptCompanyPolicy" `  
    -DenyAllExcept:company `  
    -Server:"adatum.com" `  
  
    ```  
  
### <a name="BKMK_2.3"></a>Defina um link de transformação de declarações no objeto de domínio de confiança do Adatum  
Nesta etapa, você aplica a política de transformação de declarações recém-criado no objeto de domínio de confiança do Adatum da Contoso.  
  
##### <a name="to-apply-the-claims-transformation-policy"></a>Para aplicar a política de transformação de declarações  
  
1.  Fazer logon no controlador de domínio, adatum.com como administrador com a senha **pass@word1**.  
  
2.  Abra um prompt de comando com privilégios elevados no Windows PowerShell e digite o seguinte:  
  
    ```  
  
      Set-ADClaimTransformLink `  
    -Identity:"contoso.com" `  
    -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
    '"TrustRole:Trusted `  
  
    ```  
  
## <a name="BKMK_4"></a>Configurar a transformação de declarações na floresta confiante (Contoso)  
Nesta etapa você criar uma política de transformação de declarações em Contoso (floresta confiante) para negar todas as reclamações exceto 'Empresa'. Você precisa criar uma política de transformação de declarações e vinculá-la para a relação de confiança de floresta.  
  
### <a name="BKMK_4.1"></a>Criar uma política de transformação de declarações em Contoso  
  
##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-except-company"></a>Para criar uma política de transformação Adatum para negar tudo, exceto 'Empresa'  
  
1.  Fazer logon no controlador de domínio, contoso.com como administrador com a senha **pass@word1**.  
  
2.  Abra um prompt de comando com privilégios elevados no Windows PowerShell e digite o seguinte:  
  
    ```  
    New-ADClaimTransformPolicy `  
    -Description:"Claims transformation policy to deny all claims except company" `  
    -Name:"DenyAllClaimsExceptCompanyPolicy" `  
    -DenyAllExcept:company `  
    -Server:"contoso.com" `  
  
    ```  
  
### <a name="BKMK_4.2"></a>Defina um link de transformação de declarações no objeto de domínio de confiança da Contoso  
Nesta etapa, você aplica o recém-criado requerimentos judiciais ou Extrajudiciais transformação política no objeto de domínio de confiança contoso.com para Adatum permitir que o "Empresa" ser passadas para contoso.com. O objeto de domínio de confiança é nomeado adatum.com.  
  
##### <a name="to-set-the-claims-transformation-policy"></a>Para definir as declarações de política de transformação  
  
1.  Fazer logon no controlador de domínio, contoso.com como administrador com a senha **pass@word1**.  
  
2.  Abra um prompt de comando com privilégios elevados no Windows PowerShell e digite o seguinte:  
  
    ```  
  
      Set-ADClaimTransformLink   
    -Identity:"adatum.com" `  
    -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
    -TrustRole:Trusting `  
  
    ```  
  
## <a name="BKMK_5"></a>Validar o cenário  
Nesta etapa, você tentar acessar a pasta D:\EARNINGS que foi configurada no servidor de arquivos arquivo1 para validar que o usuário tenha acesso à pasta compartilhada.  
  
#### <a name="to-ensure-that-the-adatum-user-can-access-the-shared-folder"></a>Para garantir que o usuário Adatum pode acessar a pasta compartilhada  
  
1.  Faça logon no computador cliente, CLIENT1 como Jeff Low com a senha **pass@word1**.  
  
2.  Navegue até a pasta \\\FILE1.contoso.com\Earnings.  
  
3.  Jeff Low deve ser capaz de acessar a pasta.  
  
## <a name="additional-scenarios-for-claims-transformation-policies"></a>Cenários adicionais para políticas de transformação de declarações  
Esta é uma lista de casos comuns adicionais de transformação de declarações.  
  
|Cenário|Política|  
|------------|----------|  
|Permitir que todos os requerimentos provenientes de Adatum para passar pelo processo para Contoso Adatum|Código: <br />New-ADClaimTransformPolicy \'<br /> -Descrição: "Política de transformação para permitir que todos os requerimentos diz" \'<br />-Name: "AllowAllClaimsPolicy" \'<br />-AllowAll \'<br />-Server: "contoso.com" \'<br />Set-ADClaimTransformLink \'<br />-Identidade: "adatum.com" \'<br />-Policy: "AllowAllClaimsPolicy" \'<br />-TrustRole: confiando \'<br />-Server: "contoso.com" '|  
|Negar todas as reclamações provenientes de Adatum para passar pelo processo para Contoso Adatum|Código: <br />New-ADClaimTransformPolicy \'<br />-Descrição: "Política de transformação para negar todas as reclamações diz" \'<br />-Name: "DenyAllClaimsPolicy" \'<br /> -DenyAll \'<br />-Server: "contoso.com" \'<br />Set-ADClaimTransformLink \'<br />-Identidade: "adatum.com" \'<br />-Policy: "DenyAllClaimsPolicy" \'<br />-TrustRole: confiando \'<br />-Server: "contoso.com" '|  
|Permitir que todos os requerimentos provenientes de Adatum exceto "Empresa" e "Departamento" para passar pelo processo para Contoso Adatum|Código <br />-New-ADClaimTransformationPolicy \'<br />-Descrição: "Política de transformação para permitir que todos os requerimentos exceto da empresa e departamento diz" \'<br /> -Name: "AllowAllClaimsExceptCompanyAndDepartmentPolicy" \'<br />-AllowAllExcept: empresa, departamento \'<br />-Server: "contoso.com" \'<br />Set-ADClaimTransformLink \'<br /> -Identidade: "adatum.com" \'<br />-Policy: "AllowAllClaimsExceptCompanyAndDepartmentPolicy" \'<br /> -TrustRole: confiando \'<br />-Server: "contoso.com" '|  
  
## <a name="BKMK_Links"></a>Consulte também  
  
-   Para obter uma lista de todos os cmdlets do Windows PowerShell que estão disponíveis para a transformação de declarações, consulte [referência do Active Directory PowerShell Cmdlet ](https://go.microsoft.com/fwlink/?LinkId=243150).  
  
-   Para tarefas avançadas que envolvem exportação e importação de informações de configuração DAC entre duas florestas, use o [referência do PowerShell de controle de acesso dinâmico](https://go.microsoft.com/fwlink/?LinkId=243150)  
  
-   [Implantar requerimentos judiciais ou Extrajudiciais entre florestas](Deploy-Claims-Across-Forests.md)  
  
-   [Linguagem de regras de transformação de declarações](Claims-Transformation-Rules-Language.md)  
  
-   [Controle de acesso dinâmico: Visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  
  

