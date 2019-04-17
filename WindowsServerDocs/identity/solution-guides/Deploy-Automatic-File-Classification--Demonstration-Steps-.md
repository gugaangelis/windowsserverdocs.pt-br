---
ms.assetid: 01988844-df02-4952-8535-c87aefd8a38a
title: "Implantar a classificação de arquivo automático (etapas de demonstração)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1c5c0fa221e0d7375216426f838ba37bee852984
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-automatic-file-classification-demonstration-steps"></a>Implantar a classificação de arquivo automático (etapas de demonstração)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica como habilitar as propriedades do recurso no Active Directory, crie as regras de classificação no servidor de arquivos e, depois, atribuir valores às propriedades do recurso para arquivos no servidor de arquivos. Para este exemplo, as seguintes regras de classificação são criadas:  
  
-   Uma regra de classificação de conteúdo que procura um conjunto de arquivos para a cadeia de caracteres 'Contoso confidenciais'. Se a cadeia de caracteres for encontrada em um arquivo, a propriedade de recurso impacto é definida como alta no arquivo.  
  
-   Uma regra de classificação de conteúdo que procura um conjunto de arquivos para uma expressão regular que corresponde a um número de CPF pelo menos 10 vezes em um arquivo. Se o padrão for encontrado, o arquivo é classificado como tendo informações pessoalmente identificáveis e a propriedade de recurso de informações pessoalmente identificáveis estiver definida como alta.  
  
**Neste documento**  
  
-   [Etapa 1: Criar recursos definições de propriedade](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step1)  
  
-   [Etapa 2: Criar uma regra de classificação de conteúdo de cadeia de caracteres](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step2)  
  
-   [Etapa 3: Criar uma regra de classificação de conteúdo de expressão regular](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step3)  
  
-   [Etapa 4: Verifique se que os arquivos são classificados](Deploy-Automatic-File-Classification--Demonstration-Steps-.md#BKMK_Step4)  
  
> [!NOTE]  
> Este tópico inclui cmdlets do Windows PowerShell de exemplo que você pode usar para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [usando os Cmdlets do](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Step1"></a>Etapa 1: Criar recursos definições de propriedade  
As propriedades do recurso impacto e informações pessoalmente identificáveis são habilitadas para que a infraestrutura de classificação de arquivo pode usar essas propriedades do recurso para marcar os arquivos que são verificados em uma pasta compartilhada da rede.  
  
[Fazer isso usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>A criação de definições de propriedade de recurso  
  
1.  No controlador de domínio, faça logon no servidor como um membro do grupo de segurança Administradores de domínio.  
  
2.  Abra o Centro Administrativo do Active Directory. No Gerenciador do servidor, clique em **ferramentas**e clique em **Centro Administrativo do Active Directory**.  
  
3.  Expanda **controle de acesso dinâmico**e clique em **propriedades do recurso**.  
  
4.  Clique com botão direito **impacto**e clique em **habilitar **.  
  
5.  Clique com botão direito **informações pessoalmente identificáveis**e clique em **habilitar **.  
  
![guias de soluções](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.  
  
```  
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'   
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>Etapa 2: Criar uma regra de classificação de conteúdo de cadeia de caracteres  
Uma regra de classificação de conteúdo de cadeia de caracteres lê um arquivo para uma cadeia de caracteres específico. Se a cadeia de caracteres for encontrada, o valor de uma propriedade de recurso pode ser configurado. Neste exemplo, nós examinará cada arquivo em uma pasta compartilhada da rede e procure a cadeia de caracteres 'Contoso confidenciais'. Se a cadeia de caracteres for encontrada, o arquivo associado é classificado como ter alto impacto comercial.  
  
[Fazer isso usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-create-a-string-content-classification-rule"></a>Para criar uma regra de classificação de conteúdo de cadeia de caracteres  
  
1.  Fazer logon servidor de arquivos como um membro do grupo de segurança Administradores.  
  
2.  No prompt de comando do Windows PowerShell, digite **FsrmClassificationPropertyDefinition atualização** e pressione ENTER. Isso sincronizará as definições de propriedade criadas no controlador de domínio para o servidor de arquivos.  
  
3.  Abra o Gerenciador de recursos do servidor de arquivos. No Gerenciador do servidor, clique em **ferramentas**e clique em **Gerenciador de recursos do servidor de arquivos**.  
  
4.  Expanda **gerenciamento de classificação**, clique com botão direito **as regras de classificação**e clique em **configurar agendamento de classificação**.  
  
5.  Selecione o **habilitar agendamento fixo** caixa de seleção, selecione o **permitir classificação contínua para os novos arquivos** caixa de seleção, escolha um dia da semana para executar a classificação e, em seguida, clique em **Okey**.  
  
6.  Clique com botão direito **as regras de classificação**e clique em **criar a regra de classificação**.  
  
7.  No **geral** guia, o **nome da regra**, digite um nome de regra, como **Contoso confidenciais**.  
  
8.  Sobre o **escopo**, clique em **adicionar**e escolher as pastas que devem ser incluídas nessa regra, como documentos D:\Finance.  
  
    > [!NOTE]  
    > Você também pode escolher um espaço de nome dinâmico para o escopo. Para saber mais sobre namespaces dinâmicas para as regras de classificação, consulte [What's New no Gerenciador de recursos de servidor de arquivos no Windows Server 2012 \[redirected\]](assetId:///d53c603e-6217-4b98-8508-e8e492d16083).  
  
9. Sobre o **Classification** guia, configure o seguinte:  
  
    -   No **escolher um método para atribuir uma propriedade arquivos** caixa, certifique-se de que **classificador conteúdo** é selecionado.  
  
    -   No **escolher uma propriedade para atribuir a arquivos**, clique em **impacto**.  
  
    -   No **especificar um valor**, clique em **alto**.  
  
10. Sob o **parâmetros** título, clique em **configurar**.  
  
11. No **tipo de expressão** coluna, selecione **cadeia de caracteres**.  
  
12. No **expressão** coluna, digite **Contoso confidenciais**e clique em **Okey**.  
  
13. Sobre o **tipo de avaliação**, selecione o **reavaliar valores de propriedade existentes** caixa de seleção, clique em **substituir o valor existente**e, em seguida, clique em **Okey**.  
  
![guias de soluções](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.  
  
```  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;$AutomaticClassificationScheduledTask  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name 'Contoso Confidential' -Property "Impact_MS" -PropertyValue "3000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step3"></a>Etapa 3: Criar uma regra de classificação de conteúdo de expressão regular  
Uma regra de classificação de expressão regular lê um arquivo para um padrão que corresponde à expressão regular. Se uma cadeia de caracteres que corresponde à expressão regular for encontrada, o valor de uma propriedade de recurso pode ser configurado. Neste exemplo, nós examinará cada arquivo em uma pasta compartilhada da rede e procure por uma cadeia de caracteres que corresponde ao padrão de um número de CPF (XXX-XX-XXXX). Se o padrão for encontrado, o arquivo associado é classificado como tendo informações pessoalmente identificáveis.  
  
[Fazer isso usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-regular-expression-content-classification-rule"></a>Para criar uma regra de classificação de conteúdo de expressão regular  
  
1.  Fazer logon servidor de arquivos como um membro do grupo de segurança Administradores.  
  
2.  No prompt de comando do Windows PowerShell, digite **FsrmClassificationPropertyDefinition atualização**, e pressione ENTER. Isso sincronizará as definições de propriedade que são criadas no controlador de domínio para o servidor de arquivos.  
  
3.  Abra o Gerenciador de recursos do servidor de arquivos. No Gerenciador do servidor, clique em **ferramentas**e clique em **Gerenciador de recursos do servidor de arquivos**.  
  
4.  Clique com botão direito **as regras de classificação**e clique em **criar a regra de classificação**.  
  
5.  No **geral** guia, o **nome da regra**, digite um nome para a regra de classificação, como PII regra.  
  
6.  Sobre o **escopo**, clique em **adicionar**e, em seguida, escolha as pastas que devem ser incluídas nessa regra, como documentos D:\Finance.  
  
7.  Sobre o **Classification** guia, configure o seguinte:  
  
    -   No **escolher um método para atribuir uma propriedade arquivos** caixa, certifique-se de que **classificador conteúdo** é selecionado.  
  
    -   No **escolher uma propriedade para atribuir a arquivos**, clique em **informações pessoalmente identificáveis**.  
  
    -   No **especificar um valor**, clique em **alto**.  
  
8.  Sob o **parâmetros** título, clique em **configurar**.  
  
9. No **tipo de expressão** coluna, selecione **Expressão Regular**.  
  
10. No **expressão** coluna, digite **^ (?! 000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-]?) (?! 00) \d\d\3 (?! \d {4}$ 0000)**  
  
11. No **mínimo ocorrências** coluna, digite **10**e clique em **Okey**.  
  
12. Sobre o **tipo de avaliação**, selecione o **reavaliar valores de propriedade existentes** caixa de seleção, clique em **substituir o valor existente**e, em seguida, clique em **Okey**.  
  
![guias de soluções](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.  
  
```  
New-FSRMClassificationRule -Name "PII Rule" -Property "PII_MS" -PropertyValue "5000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=10;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step4"></a>Etapa 4: Verifique se que os arquivos são classificados corretamente  
Você pode verificar que os arquivos são classificados corretamente exibindo as propriedades de um arquivo que foi criado na pasta especificada nas regras de classificação.  
  
#### <a name="to-verify-that-the-files-are-classified-correctly"></a>Para verificar que os arquivos são classificados corretamente  
  
1.  No servidor de arquivos, execute as regras de classificação usando o Gerenciador de recursos do servidor de arquivos.  
  
    1.  Clique em **gerenciamento de classificação**, clique com botão direito **as regras de classificação**e clique em **executar Classification com todas as regras agora**.  
  
    2.  Clique no **Aguarde a classificação concluir** opção e, em seguida, clique em **Okey**.  
  
    3.  Feche o relatório de classificação automática.  
  
    4.  Você pode fazer isso usando o Windows PowerShell com o seguinte comando: **inicial FSRMClassification ' "RunDuration 0 - confirmar: $false**  
  
2.  Navegue até a pasta que foi especificada nas regras de classificação, como documentos D:\Finance.  
  
3.  Selecione o arquivo nessa pasta e clique em **propriedades**.  
  
4.  Clique no **Classification** guia e verifique se que o arquivo é classificado corretamente.  
  
## <a name="BKMK_Links"></a>Consulte também  
  
-   [Cenário: Obtenha ideias para seus dados por meio de classificação](Scenario--Get-Insight-into-Your-Data-by-Using-Classification.md)  
  
-   [Planejar a classificação do arquivo automática](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955)  
  
-   [Controle de acesso dinâmico: Visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  
  

