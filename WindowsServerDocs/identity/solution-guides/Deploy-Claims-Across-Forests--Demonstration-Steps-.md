---
ms.assetid: 846c3680-b321-47da-8302-18472be42421
title: Implantar declarações em florestas (passo a passo)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a6f2e5d3a227384b20735ab99ee1ab5ea77bd913
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445847"
---
# <a name="deploy-claims-across-forests-demonstration-steps"></a>Implantar declarações em florestas (passo a passo)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Neste tópico, abordaremos um cenário básico que explica como configurar as transformações de declarações entre florestas confiantes e confiáveis. Você aprenderá como objetos de política de transformação de declarações podem ser criados e vinculados a relação de confiança da floresta confiante e a floresta confiável. Você irá validar o cenário.  

## <a name="scenario-overview"></a>Visão geral do cenário  
Adatum Corporation oferece serviços financeiros para a Contoso, Ltd. Cada trimestre, contadores de Adatum copiar suas planilhas de conta para uma pasta em um servidor de arquivos localizado na Contoso, Ltd. Há uma relação de confiança bidirecional configurada da Contoso para Adatum. A Contoso, Ltd. quer proteger o compartilhamento para que somente os funcionários da Adatum possam acessar o compartilhamento remoto.  

Neste cenário:  

1.  [Configurar os pré-requisitos e o ambiente de teste](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_1.1)  

2.  [Configurar a transformação de declarações em florestas confiáveis (Adatum)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_3)  

3.  [Configurar a transformação de declarações na floresta confiante (Contoso)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_4)  

4.  [Validar o cenário](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_5)  

## <a name="BKMK_1.1"></a>Configurar os pré-requisitos e o ambiente de teste  
A configuração de teste envolve configurar duas florestas: Adatum Corporation e Contoso, Ltd e ter uma relação de confiança bidirecional entre a Contoso e Adatum. "adatum.com" é a floresta confiável e "contoso.com" é a floresta confiante.  

O cenário da transformação de declarações demonstra a transformação de uma declaração na floresta confiável para uma declaração na floresta confiante. Para fazer isso, você precisa configurar uma nova floresta chamada adatum.com e preencher a floresta com um usuário de teste com o valor da empresa 'Adatum'. Em seguida, você precisa configurar uma confiança bidirecional entre contoso.com e adatum.com.  

> [!IMPORTANT]  
> Ao configurar as florestas de Contoso e Adatum, você deve garantir que os dois domínios raiz da correm o Windows Server 2012 nível funcional de domínio para a transformação de declarações para trabalhar.  

Você precisa configurar o seguinte para o laboratório. Esses procedimentos são explicados em detalhes em [apêndice b: Como configurar o ambiente de teste](Appendix-B--Setting-Up-the-Test-Environment.md)  

Você precisa implementar os procedimentos a seguir para configurar o laboratório para este cenário:  

1.  [Definir Adatum como floresta confiável para a Contoso](Appendix-B--Setting-Up-the-Test-Environment.md)  

2.  [Criar o tipo de declaração 'Empresa' na Contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.8)  

3.  [Habilitar a propriedade de recurso 'Empresa' na Contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.55)  

4.  [Criar a regra de acesso central](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.9)  

5.  [Criar a política de acesso central](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.10)  

6.  [Publicar a nova política por meio da diretiva de grupo](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.11)  

7.  [Crie a pasta ganhos no servidor de arquivos](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.12)  

8.  [Definir a classificação e aplicar a política de acesso central na nova pasta](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.13)  

Use as informações a seguir para concluir esse cenário:  

|Objects|Detalhes|  
|-----------|-----------|  
|Usuários|Jeff Low, Contoso|  
|Declarações de usuário na Adatum e Contoso|ID: ad: / / ext/da empresa: ContosoAdatum,<br /><br />Atributo de origem: empresa<br /><br />Valores sugeridos: Contoso, a Adatum **importantes:** Você deve definir a ID de 'Empresa' tipo de declaração na Contoso e Adatum para ser o mesmo para a transformação de declarações para trabalhar.|  
|Regra de acesso central na Contoso|AdatumEmployeeAccessRule|  
|Política de acesso central na Contoso|Política de acesso somente para adatum|  
|Políticas de transformação de declarações em Adatum e Contoso|Empresa DenyAllExcept|  
|Pasta de arquivos na Contoso|D:\EARNINGS|  

## <a name="BKMK_3"></a>Configurar a transformação de declarações em florestas confiáveis (Adatum)  
Nesta etapa, você cria uma política de transformação em Adatum para negar todas as declarações, exceto 'Empresa' para passar para a Contoso.  

O módulo do Active Directory para o Windows PowerShell fornece o **DenyAllExcept** argumento, que descarta tudo, exceto as declarações especificadas na política de transformação.  

Para configurar uma transformação de declarações, você precisa criar uma política de transformação de declarações e vinculá-lo entre as florestas confiantes e confiáveis.  

### <a name="BKMK_2.2"></a>Criar uma política de transformação de declarações no Adatum  

##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-claims-except-company"></a>Para criar uma política de transformação Adatum para negar todas as declarações, exceto 'Empresa'  

1. Entrar para o controlador de domínio adatum.com como administrador com a senha <strong>pass@word1</strong>.  

2. Abra um prompt de comando elevado no Windows PowerShell e digite o seguinte:  

   ```  
   New-ADClaimTransformPolicy `  
   -Description:"Claims transformation policy to deny all claims except Company"`  
   -Name:"DenyAllClaimsExceptCompanyPolicy" `  
   -DenyAllExcept:company `  
   -Server:"adatum.com" `  

   ```  

### <a name="BKMK_2.3"></a>Defina um link de transformação de declarações no objeto de domínio de confiança da Adatum  
Nesta etapa, você aplicar a política de transformação de declarações recém-criado no objeto de domínio de confiança da Adatum da Contoso.  

##### <a name="to-apply-the-claims-transformation-policy"></a>Para aplicar a política de transformação de declarações  

1. Entrar para o controlador de domínio adatum.com como administrador com a senha <strong>pass@word1</strong>.  

2. Abra um prompt de comando elevado no Windows PowerShell e digite o seguinte:  

   ```  

     Set-ADClaimTransformLink `  
   -Identity:"contoso.com" `  
   -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
   '"TrustRole:Trusted `  

   ```  

## <a name="BKMK_4"></a>Configurar a transformação de declarações na floresta confiante (Contoso)  
Nesta etapa, você cria uma política de transformação de declarações na Contoso (a floresta confiante) para negar todas as declarações, exceto 'Empresa'. Você precisa criar uma política de transformação de declarações e vinculá-lo para a relação de confiança de floresta.  

### <a name="BKMK_4.1"></a>Criar uma política de transformação de declarações na Contoso  

##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-except-company"></a>Para criar uma política de transformação Adatum para negar tudo, exceto 'Empresa'  

1. Entrar no controlador de domínio, contoso.com como administrador com a senha <strong>pass@word1</strong>.  

2. Abra um prompt de comando elevado no Windows PowerShell e digite o seguinte:  

   ```  
   New-ADClaimTransformPolicy `  
   -Description:"Claims transformation policy to deny all claims except company" `  
   -Name:"DenyAllClaimsExceptCompanyPolicy" `  
   -DenyAllExcept:company `  
   -Server:"contoso.com" `  

   ```  

### <a name="BKMK_4.2"></a>Defina um link de transformação de declarações no objeto de domínio de confiança da Contoso  
Nesta etapa, você aplica o recém-criado política de transformação de declarações no objeto de domínio de confiança contoso.com para Adatum permitir que "Empresa" ser passado para contoso.com. O objeto de confiança de domínio é denominado adatum.com.  

##### <a name="to-set-the-claims-transformation-policy"></a>Para definir as declarações de política de transformação  

1. Entrar no controlador de domínio, contoso.com como administrador com a senha <strong>pass@word1</strong>.  

2. Abra um prompt de comando elevado no Windows PowerShell e digite o seguinte:  

   ```  

     Set-ADClaimTransformLink   
   -Identity:"adatum.com" `  
   -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
   -TrustRole:Trusting `  

   ```  

## <a name="BKMK_5"></a>Validar o cenário  
Nesta etapa, você tentar acessar a pasta D:\EARNINGS que foi configurada no servidor de arquivos FILE1 para validar que o usuário tem acesso à pasta compartilhada.  

#### <a name="to-ensure-that-the-adatum-user-can-access-the-shared-folder"></a>Para garantir que o usuário Adatum pode acessar a pasta compartilhada  

1. Máquina de entrada para o cliente, CLIENT1 como Jeff Low com a senha <strong>pass@word1</strong>.  

2. Navegue até a pasta \\\FILE1.contoso.com\Earnings.  

3. Jeff Low deve ser capaz de acessar a pasta.  

## <a name="additional-scenarios-for-claims-transformation-policies"></a>Cenários adicionais para políticas de transformação de declarações  
A seguir está uma lista de casos comuns adicionais na transformação de declarações.  


|                                                 Cenário                                                 |                                                                                                                                                                                                                                           Política                                                                                                                                                                                                                                            |
|----------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  Permitir todas as declarações que vêm de Adatum de passar para a Contoso Adatum                  |                                                          Código- <br />New-ADClaimTransformPolicy \`<br /> -Descrição: "Política para permitir que todas as declarações de transformação de declarações" \`<br />-Name:"AllowAllClaimsPolicy" \`<br />-AllowAll \`<br />-Server:"contoso.com" \`<br />Conjunto ADClaimTransformLink \`<br />-Identity:"adatum.com" \`<br />-Policy:"AllowAllClaimsPolicy" \`<br />-TrustRole: confiar em \`<br />-Server:"contoso.com" \`                                                          |
|                  Negar todas as declarações que vêm de Adatum de passar para a Contoso Adatum                   |                                                            Código- <br />New-ADClaimTransformPolicy \`<br />-Descrição: "Política para negar todas as declarações de transformação de declarações" \`<br />-Name:"DenyAllClaimsPolicy" \`<br /> -DenyAll \`<br />-Server:"contoso.com" \`<br />Conjunto ADClaimTransformLink \`<br />-Identity:"adatum.com" \`<br />-Policy:"DenyAllClaimsPolicy" \`<br />-TrustRole: confiar em \`<br />-Server:"contoso.com"\`                                                             |
| Permitir todas as declarações que vêm de Adatum, exceto "Empresa" e "Departamento" de passar para a Contoso Adatum | Código <br />- New-ADClaimTransformationPolicy \`<br />-Descrição: "Política para permitir que todas as declarações, exceto da empresa e o departamento de transformação de declarações" \`<br /> -Name:"AllowAllClaimsExceptCompanyAndDepartmentPolicy" \`<br />-AllowAllExcept: da empresa, o departamento \`<br />-Server:"contoso.com" \`<br />Conjunto ADClaimTransformLink \`<br /> -Identity:"adatum.com" \`<br />-Policy:"AllowAllClaimsExceptCompanyAndDepartmentPolicy" \`<br /> -TrustRole: confiar em \`<br />-Server:"contoso.com" \` |

## <a name="BKMK_Links"></a>Consulte também  

-   Para obter uma lista de todos os cmdlets do Windows PowerShell que estão disponíveis para a transformação de declarações, consulte [referência de Cmdlet do PowerShell do Active Directory](https://go.microsoft.com/fwlink/?LinkId=243150).  

-   Para tarefas avançadas que envolvem a exportação e importação de informações de configuração de DAC entre duas florestas, use o [referência do PowerShell de controle de acesso dinâmico](https://go.microsoft.com/fwlink/?LinkId=243150)  

-   [Implantar declarações em florestas](Deploy-Claims-Across-Forests.md)  

-   [Linguagem de regras de transformação de declarações](Claims-Transformation-Rules-Language.md)  

-   [Controle de acesso dinâmico: visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  


