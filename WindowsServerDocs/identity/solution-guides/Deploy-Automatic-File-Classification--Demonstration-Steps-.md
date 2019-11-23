---
ms.assetid: 01988844-df02-4952-8535-c87aefd8a38a
title: Deploy Automatic File Classification (Demonstration Steps)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 7b8d613653bc2effdae155d34a1a94a820bae3aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357591"
---
# <a name="deploy-automatic-file-classification-demonstration-steps"></a>Deploy Automatic File Classification (Demonstration Steps)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica como habilitar as propriedades do recurso no Active Directory, criar regras de classificação no servidor de arquivos e atribuir valores a propriedades do recurso para os arquivos do servidor de arquivos. Neste exemplo, são criadas as seguintes regras de classificação:  
  
-   Uma regra de classificação de conteúdo que pesquisa um conjunto de arquivos para a cadeia de caracteres ' contoso Confidential. ' Se a cadeia de caracteres for encontrada em um arquivo, a propriedade do recurso Impacto é definida para Alto neste arquivo.  
  
-   Uma regra de classificação que pesquisa em um conjunto de arquivos uma expressão regular compatível com um número de seguro social pelo menos dez vezes em um arquivo. Se o padrão for encontrado, o arquivo é classificado como possuindo informações de identificação pessoal e a propriedade de recurso Informações de identificação pessoal é definida como Alto.  
  
**Neste documento**  
  
-   [Etapa 1: criar definições de propriedade de recurso](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step1)  
  
-   [Etapa 2: criar uma regra de classificação de conteúdo de cadeia de caracteres](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step2)  
  
-   [Etapa 3: criar uma regra de classificação de conteúdo de expressão regular](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step3)  
  
-   [Etapa 4: verificar se os arquivos estão classificados](Deploy-Automatic-File-Classification--Demonstration-Steps-.md#BKMK_Step4)  
  
> [!NOTE]  
> Este tópico inclui cmdlets do Windows PowerShell de exemplo que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [Usando cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Step1"></a>Etapa 1: criar definições de propriedade de recurso  
As propriedades dos recursos Impacto e Informações de Identificação Pessoal são habilitadas de maneira que a Infraestrutura de Classificação de Arquivos possa usar tais propriedades de recurso para marcar os arquivos que são analisados em uma pasta compartilhada da rede.  
  
[Siga esta etapa usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>Para criar definições de propriedade de recurso  
  
1.  No controlador de domínio, entre no servidor como membro do grupo de segurança Admins. do Domínio.  
  
2.  Abra o Centro Administrativo do Active Directory. No Gerenciador do Servidor, clique em **Ferramentas** e em **Centro Administrativo do Active Directory**.  
  
3.  Expanda **Controle de Acesso Dinâmico** e clique em **Propriedades do Recurso**.  
  
4.  Clique com o botão direito do mouse em **Impacto** e em **Habilitar**.  
  
5.  Clique com o botão direito do mouse em **Informações de Identificação Pessoal** e em **Habilitar**.  
  
![guias de solução](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'   
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>Etapa 2: criar uma regra de classificação de conteúdo de cadeia de caracteres  
Uma regra de classificação de conteúdo de cadeia de caracteres analisa um arquivo em busca de uma cadeia de caracteres específica. Se a cadeia de caracteres for encontrada, o valor de uma propriedade de recurso poderá ser configurada. Neste exemplo, examinaremos cada arquivo em uma pasta de rede compartilhada e procuraremos a cadeia de caracteres "contoso Confidential". Se a cadeia de caracteres for encontrada, o arquivo associado é classificado como alto impacto dos negócios.  
  
[Siga esta etapa usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-create-a-string-content-classification-rule"></a>Para criar uma regra de classificação de conteúdo de cadeia de caracteres  
  
1.  Entre no servidor de arquivos como membro do grupo de segurança Administradores.  
  
2.  No prompt de comando do Windows PowerShell, digite **Update-FsrmClassificationPropertyDefinition** e pressione ENTER. Isso sincronizará as definições de propriedade criadas no controlador de domínio para o servidor de arquivos.  
  
3.  Abra o Gerenciador de Recursos de Servidor de Arquivos. No Gerenciador do Servidor, clique em **Ferramentas** e em **Gerenciador de Recursos de Servidor de Arquivos**.  
  
4.  Expanda **Gerenciamento de Classificação**, clique com o botão direito do mouse em **Regras de Classificação**e em **Configurar Agendamento de Classificação**.  
  
5.  Marque as caixas de seleção **Habilitar agendamento fixo** e **Permitir classificação contínua para novos arquivos**, escolha um dia da semana para execução da classificação e clique em **OK**.  
  
6.  Clique com o botão direito do mouse em **Regras de Classificação**e depois clique em **Criar Regra de Classificação**.  
  
7.  Na guia **Geral**, na caixa **Nome da regra**, digite um nome de regra como **Contoso Confidential**.  
  
8.  Na guia **Escopo** , clique em **Adicionar**e escolha as pastas que devem ser incluídas nessa regra, como D:\Finance Documents.  
  
    > [!NOTE]  
    > É possível escolher um namespace dinâmico para o escopo. Para obter mais informações sobre espaços de nome dinâmico para regras de classificação, consulte [novidades no Gerenciador de recursos de servidor de arquivos no Windows server 2012 \[\]Redirecionado ](assetId:///d53c603e-6217-4b98-8508-e8e492d16083).  
  
9. Na guia **Classificação**, configure o seguinte:  
  
    -   Na caixa **Escolha um método para atribuir uma propriedade aos arquivos**, verifique se **Classificador de Conteúdo** está selecionado.  
  
    -   Na caixa **Escolher uma propriedade a ser atribuída arquivos** , clique em **Impacto**.  
  
    -   Na caixa **Especifique um valor**, clique em **Alto**.  
  
10. No cabeçalho **Parâmetros**, clique em **Configurar**.  
  
11. Na coluna **Tipo de Expressão** , selecione **Cadeia de caracteres**.  
  
12. Na coluna **Expressão** , digite **Contoso Confidential**e clique em **OK**.  
  
13. Na guia **Tipo de Avaliação**, marque a caixa de seleção **Reavaliar os valores de propriedade existentes**, clique em **Substituir o valor existente** e clique em **OK**.  
  
![guias de solução](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;$AutomaticClassificationScheduledTask  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name 'Contoso Confidential' -Property "Impact_MS" -PropertyValue "3000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step3"></a>Etapa 3: criar uma regra de classificação de conteúdo de expressão regular  
Uma regra de expressão regular analisa um arquivo em busca de um padrão correspondente à expressão regular. Se uma cadeia de caracteres que correspode à expressão regular for encontrada, o valor de uma propriedade de recurso poderá ser configurada. Neste exemplo, analisaremos cada arquivo em uma pasta de rede compartilhada buscando por uma cadeia de caracteres correspondente ao número de seguro social (XXX-XX-XXXX). Se o padrão for encontrado, o arquivo associado a ele será classificado como portador de informações de identificação pessoal.  
  
[Siga esta etapa usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-regular-expression-content-classification-rule"></a>Para criar uma regra de classificação de conteúdo de expressão regular  
  
1.  Entre no servidor de arquivos como um membro do grupo de segurança Administradores.  
  
2.  No prompt de comando do Windows PowerShell, digite **Update-FsrmClassificationPropertyDefinition** e pressione ENTER. Isso sincronizará as definições de propriedade criadas no controlador de domínio para o servidor de arquivos.  
  
3.  Abra o Gerenciador de Recursos de Servidor de Arquivos. No Gerenciador do Servidor, clique em **Ferramentas** e em **Gerenciador de Recursos de Servidor de Arquivos**.  
  
4.  Clique com o botão direito do mouse em **Regras de Classificação**e depois clique em **Criar Regra de Classificação**.  
  
5.  Na guia **Geral**, na caixa **Nome da regra**, digite um nome para a regra de classificação, como Regra PII.  
  
6.  Na guia **Escopo**, clique em **Adicionar** e escolha as pastas que deverão ser incluídas nesta regra, como D:\Documentos financeiros.  
  
7.  Na guia **Classificação**, configure o seguinte:  
  
    -   Na caixa **Escolha um método para atribuir uma propriedade aos arquivos**, verifique se **Classificador de Conteúdo** está selecionado.  
  
    -   Na caixa **Escolher uma propriedade a ser atribuída aos arquivos** , clique em **Informações de Identificação Pessoal**.  
  
    -   Na caixa **Especifique um valor**, clique em **Alto**.  
  
8.  No cabeçalho **Parâmetros**, clique em **Configurar**.  
  
9. Na coluna **Tipo de Expressão** , selecione **Expressão regular**.  
  
10. Na coluna **expressão** , digite **^ (?! 000) ([0-7] \d{2}| 7 ([0-7] \d | 7 [012])) ([-]?) (?! 00) \d\d\3 (?! 0000) \d{4}$**  
  
11. Na coluna **Ocorrências Mínimas** , digite **10**e clique em **OK**.  
  
12. Na guia **Tipo de Avaliação**, marque a caixa de seleção **Reavaliar os valores de propriedade existentes**, clique em **Substituir o valor existente** e clique em **OK**.  
  
![guias de solução](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
New-FSRMClassificationRule -Name "PII Rule" -Property "PII_MS" -PropertyValue "5000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=10;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step4"></a>Etapa 4: verificar se os arquivos foram classificados corretamente  
Você pode verificar se os arquivos foram devidamente classificados ao ver as propriedades de um arquivo criado na pasta especificada nas regras de classificação.  
  
#### <a name="to-verify-that-the-files-are-classified-correctly"></a>Para verificar se os arquivos foram classificados corretamente  
  
1.  No servidor de arquivos, execute as regras de classificação usando o Gerenciador de Recursos de Servidor de Arquivos.  
  
    1.  Clique em **Gerenciamento de Classificação**, clique com o botão direito do mouse em **Regras de Classificação** e clique em **Executar a Classificação com Todas as Regras Agora**.  
  
    2.  Clique na opção **Aguardar a conclusão da classificação** e em **OK**.  
  
    3.  Feche o Relatório de Classificação Automática.  
  
    4.  Você pode fazer isso usando o Windows PowerShell com o seguinte comando: **Start-FSRMClassification ' "RunDuration 0-Confirm: $false**  
  
2.  Navegue para a pasta especificada nas regras de classificação, como D:\Documentos financeiros.  
  
3.  Clique com o botão direito do mouse na pasta e clique em **Propriedades**.  
  
4.  Clique na guia **Classificação** e verifique se o arquivo foi classificado corretamente.  
  
## <a name="BKMK_Links"></a>Consulte também  
  
-   [Cenário: Obtenha informações sobre seus dados usando a classificação](Scenario--Get-Insight-into-Your-Data-by-Using-Classification.md)  
  
-   [Planejar a classificação automática de arquivos](https://docs.microsoft.com/previous-versions/orphan-topics/ws.11/jj574209(v%3dws.11))  

  
-   [Controle de acesso dinâmico: visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  
  

