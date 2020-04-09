---
ms.assetid: 846c3680-b321-47da-8302-18472be42421
title: Implantar declarações em florestas (passo a passo)
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 05ca21f343d2ad3db4ce00b53a66b3cd6dd6de16
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861239"
---
# <a name="deploy-claims-across-forests-demonstration-steps"></a>Implantar declarações em florestas (passo a passo)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Neste tópico, abordaremos um cenário básico que explica como configurar transformações de declarações entre florestas confiáveis e confiantes. Você aprenderá como os objetos de política de transformação de declarações podem ser criados e vinculados à relação de confiança na floresta confiável e na floresta confiável. Em seguida, você validará o cenário.  

## <a name="scenario-overview"></a>Visão geral do cenário  
A adatum Corporation fornece serviços financeiros para a contoso, Ltd. A cada trimestre, os contadores do adatum copiam suas planilhas de conta para uma pasta em um servidor de arquivos localizado na contoso, Ltd. Há uma relação de confiança bidirecional configurada de contoso para adatum. A contoso, Ltd. deseja proteger o compartilhamento para que somente os funcionários adatum possam acessar o compartilhamento remoto.  

Neste cenário:  

1.  [Configurar os pré-requisitos e o ambiente de teste](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_1.1)  

2.  [Configurar a transformação de declarações na floresta confiável (adatum)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_3)  

3.  [Configurar a transformação de declarações na floresta confiante (contoso)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_4)  

4.  [Validar o cenário](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_5)  

## <a name="set-up-the-prerequisites-and-the-test-environment"></a><a name="BKMK_1.1"></a>Configurar os pré-requisitos e o ambiente de teste  
A configuração de teste envolve a configuração de duas florestas: adatum Corporation e contoso, Ltd e ter uma relação de confiança bidirecional entre a Contoso e a adatum. "adatum.com" é a floresta confiável e "contoso.com" é a floresta confiante.  

O cenário de transformação de declarações demonstra a transformação de uma declaração na floresta confiável para uma declaração na floresta confiante. Para fazer isso, você precisa configurar uma nova floresta chamada adatum.com e popular a floresta com um usuário de teste com um valor de empresa de ' adatum '. Em seguida, você precisa configurar uma relação de confiança bidirecional entre contoso.com e adatum.com.  

> [!IMPORTANT]  
> Ao configurar as florestas Contoso e adatum, você deve garantir que os domínios raiz estejam no nível funcional de domínio do Windows Server 2012 para que a transformação de declarações funcione.  

Você precisa configurar o seguinte para o laboratório. Esses procedimentos são explicados em detalhes no [Apêndice B: Configurando o ambiente de teste](Appendix-B--Setting-Up-the-Test-Environment.md)  

Você precisa implementar os seguintes procedimentos para configurar o laboratório para este cenário:  

1.  [Definir Adatum como floresta confiável para contoso](Appendix-B--Setting-Up-the-Test-Environment.md)  

2.  [Criar o tipo de declaração ' Company ' na contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.8)  

3.  [Habilitar a propriedade de recurso ' Company ' na contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.55)  

4.  [Criar a regra de acesso central](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.9)  

5.  [Criar a política de acesso central](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.10)  

6.  [Publicar a nova política por meio de Política de Grupo](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.11)  

7.  [Criar a pasta de ganhos no servidor de arquivos](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.12)  

8.  [Definir a classificação e aplicar a política de acesso central na nova pasta](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.13)  

Use as seguintes informações para concluir este cenário:  

|Objetos|Detalhes|  
|-----------|-----------|  
|Usuários|Jeff Low, contoso|  
|Declarações do usuário em adatum e contoso|ID: ad://ext/Company:ContosoAdatum,<p>Atributo de origem: empresa<p>Valores sugeridos: contoso, adatum **importante:** você deve definir a ID no tipo de declaração ' Company ' em contoso e adatum como o mesmo para que a transformação de declarações funcione.|  
|Regra de acesso central na contoso|AdatumEmployeeAccessRule|  
|Política de acesso central na contoso|Política de acesso somente adatum|  
|Políticas de transformação de declarações em adatum e contoso|Empresa DenyAllExcept|  
|Pasta de arquivos na contoso|D:\EARNINGS|  

## <a name="set-up-claims-transformation-on-trusted-forest-adatum"></a><a name="BKMK_3"></a>Configurar a transformação de declarações na floresta confiável (adatum)  
Nesta etapa, você cria uma política de transformação em adatum para negar todas as declarações, exceto ' Company ', a serem passadas para contoso.  

O módulo Active Directory para o Windows PowerShell fornece o argumento **DenyAllExcept** , que descarta tudo, exceto as declarações especificadas na política de transformação.  

Para configurar uma transformação de declarações, você precisa criar uma política de transformação de declarações e vinculá-la entre as florestas confiáveis e confiáveis.  

### <a name="create-a-claims-transformation-policy-in-adatum"></a><a name="BKMK_2.2"></a>Criar uma política de transformação de declarações no adatum  

##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-claims-except-company"></a>Para criar uma política de transformação adatum para negar todas as declarações, exceto ' Company '  

1. Entre no controlador de domínio, adatum.com como administrador com a senha <strong>pass@word1</strong>.  

2. Abra um prompt de comando com privilégios elevados no Windows PowerShell e digite o seguinte:  

   ```  
   New-ADClaimTransformPolicy `  
   -Description:"Claims transformation policy to deny all claims except Company"`  
   -Name:"DenyAllClaimsExceptCompanyPolicy" `  
   -DenyAllExcept:company `  
   -Server:"adatum.com" `  

   ```  

### <a name="set-a-claims-transformation-link-on-adatums-trust-domain-object"></a><a name="BKMK_2.3"></a>Definir um link de transformação de declarações no objeto de domínio de confiança de adatum  
Nesta etapa, você aplica a política de transformação de declarações recém-criada no objeto de domínio confiável do adatum para a contoso.  

##### <a name="to-apply-the-claims-transformation-policy"></a>Para aplicar a política de transformação de declarações  

1. Entre no controlador de domínio, adatum.com como administrador com a senha <strong>pass@word1</strong>.  

2. Abra um prompt de comando com privilégios elevados no Windows PowerShell e digite o seguinte:  

   ```  

     Set-ADClaimTransformLink `  
   -Identity:"contoso.com" `  
   -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
   '"TrustRole:Trusted `  

   ```  

## <a name="set-up-claims-transformation-in-the-trusting-forest-contoso"></a><a name="BKMK_4"></a>Configurar a transformação de declarações na floresta confiante (contoso)  
Nesta etapa, você cria uma política de transformação de declarações na contoso (a floresta confiante) para negar todas as declarações, exceto ' Company. ' Você precisa criar uma política de transformação de declarações e vinculá-la à relação de confiança da floresta.  

### <a name="create-a-claims-transformation-policy-in-contoso"></a><a name="BKMK_4.1"></a>Criar uma política de transformação de declarações na contoso  

##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-except-company"></a>Para criar uma política de transformação adatum para negar tudo, exceto ' Company '  

1. Entre no controlador de domínio, contoso.com como administrador com a senha <strong>pass@word1</strong>.  

2. Abra um prompt de comando com privilégios elevados no Windows PowerShell e digite o seguinte:  

   ```  
   New-ADClaimTransformPolicy `  
   -Description:"Claims transformation policy to deny all claims except company" `  
   -Name:"DenyAllClaimsExceptCompanyPolicy" `  
   -DenyAllExcept:company `  
   -Server:"contoso.com" `  

   ```  

### <a name="set-a-claims-transformation-link-on-contosos-trust-domain-object"></a><a name="BKMK_4.2"></a>Definir um link de transformação de declarações no objeto de domínio de confiança da contoso  
Nesta etapa, você aplica a política de transformação de declarações recém-criada no objeto de domínio contoso.com Trust para adatum para permitir que "empresa" passe para contoso.com. O objeto de domínio de confiança é denominado adatum.com.  

##### <a name="to-set-the-claims-transformation-policy"></a>Para definir a política de transformação de declarações  

1. Entre no controlador de domínio, contoso.com como administrador com a senha <strong>pass@word1</strong>.  

2. Abra um prompt de comando com privilégios elevados no Windows PowerShell e digite o seguinte:  

   ```  

     Set-ADClaimTransformLink   
   -Identity:"adatum.com" `  
   -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
   -TrustRole:Trusting `  

   ```  

## <a name="validate-the-scenario"></a><a name="BKMK_5"></a>Validar o cenário  
Nesta etapa, você tenta acessar a pasta D:\EARNINGS que foi configurada no servidor de arquivos FILE1 para validar que o usuário tem acesso à pasta compartilhada.  

#### <a name="to-ensure-that-the-adatum-user-can-access-the-shared-folder"></a>Para garantir que o usuário adatum possa acessar a pasta compartilhada  

1. Entre no computador cliente, CLIENT1 como Jeff Low com a <strong>pass@word1</strong>de senha.  

2. Navegue até a pasta \\\FILE1.contoso.com\Earnings.  

3. Jeff Low deve ser capaz de acessar a pasta.  

## <a name="additional-scenarios-for-claims-transformation-policies"></a>Cenários adicionais para políticas de transformação de declarações  
A seguir está uma lista de casos comuns adicionais na transformação de declarações.  


|                                                 Cenário                                                 |                                                                                                                                                                                                                                           Política                                                                                                                                                                                                                                            |
|----------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  Permitir que todas as declarações provenientes de adatum passem para contoso adatum                  |                                                          Auto-completar <br />\` New-ADClaimTransformPolicy<br /> -Descrição: "política de transformação de declarações para permitir todas as declarações" \`<br />-Name: "AllowAllClaimsPolicy" \`<br />-AllowAll \`<br />-Server:"contoso. com" \`<br />Set-ADClaimTransformLink \`<br />-Identity:"adatum. com" \`<br />-Política: "AllowAllClaimsPolicy" \`<br />-TrustRole: confiar \`<br />-Server:"contoso. com" \`                                                          |
|                  Negar todas as declarações provenientes de adatum para passar para o contoso adatum                   |                                                            Auto-completar <br />\` New-ADClaimTransformPolicy<br />-Descrição: "política de transformação de declarações para negar todas as declarações" \`<br />-Name: "DenyAllClaimsPolicy" \`<br /> -DenyAll \`<br />-Server:"contoso. com" \`<br />Set-ADClaimTransformLink \`<br />-Identity:"adatum. com" \`<br />-Política: "DenyAllClaimsPolicy" \`<br />-TrustRole: confiar \`<br />-Server:"contoso. com"\`                                                             |
| Permitir que todas as declarações provenientes de adatum, exceto "empresa" e "departamento", passem para o contoso adatum | Código <br />-\` New-ADClaimTransformationPolicy<br />-Descrição: "política de transformação de declarações para permitir todas as declarações, exceto a empresa e o departamento" \`<br /> -Name: "AllowAllClaimsExceptCompanyAndDepartmentPolicy" \`<br />-AllowAllExcept: empresa, departamento \`<br />-Server:"contoso. com" \`<br />Set-ADClaimTransformLink \`<br /> -Identity:"adatum. com" \`<br />-Política: "AllowAllClaimsExceptCompanyAndDepartmentPolicy" \`<br /> -TrustRole: confiar \`<br />-Server:"contoso. com" \` |

## <a name="see-also"></a><a name="BKMK_Links"></a>Consulte também  

-   Para obter uma lista de todos os cmdlets do Windows PowerShell que estão disponíveis para transformação de declarações, consulte [Active Directory referência de cmdlet do PowerShell](https://go.microsoft.com/fwlink/?LinkId=243150).  

-   Para tarefas avançadas que envolvem a exportação e a importação de informações de configuração de DAC entre duas florestas, use a [referência do PowerShell de controle de acesso dinâmico](https://go.microsoft.com/fwlink/?LinkId=243150)  

-   [Implantar declarações em florestas](Deploy-Claims-Across-Forests.md)  

-   [Linguagem de regras de transformação de declarações](Claims-Transformation-Rules-Language.md)  

-   [Controle de acesso dinâmico: visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  


