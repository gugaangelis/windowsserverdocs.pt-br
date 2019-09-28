---
ms.assetid: 2c76e81a-c2eb-439f-a89f-7d3d70790244
title: Deploy Encryption of Office Files (Demonstration Steps)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 05da1b7df2e3242c9b68bd7858c824f91e81a563
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407107"
---
# <a name="deploy-encryption-of-office-files-demonstration-steps"></a>Deploy Encryption of Office Files (Demonstration Steps)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O departamento de finanças da Contoso tem vários servidores de arquivos que armazenam seus documentos. Esses documentos podem ser documentação em geral ou HBI (com alto impacto nos negócios). Por exemplo, qualquer documento contendo informações confidenciais é considerado, pela Contoso, como HBI. A Contoso deseja garantir que todos os seus documentos tenham o mínimo de proteção e que os documentos HBI sejam restritos às pessoas apropriadas. Para fazer isso, a Contoso está explorando o uso da FCI (infraestrutura de classificação de arquivos) e AD RMS que estão disponíveis no Windows Server 2012. Usando a FCI, a Contoso poderá classificar todos os documentos do servidor de arquivos com base no conteúdo e depois usar o AD RMS para aplicar as políticas de direitos adequadas.  
  
Nesse cenário, você executará as seguintes etapas:  
  
|Tarefa|Descrição|  
|--------|---------------|  
|[Habilitar Propriedades de recurso](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_1.1)|Habilitar as propriedades do recurso **Impacto** e **Informações de identificação pessoal** .|  
|[Criar regras de classificação](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_2)|Crie as seguintes regras de classificação: **Regra de classificação de HBI** e **Regra de classificação de PII**.|  
|[Usar tarefas de gerenciamento de arquivos para proteger automaticamente documentos com AD RMS](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_3)|Crie uma tarefa de gerenciamento de arquivo usada automaticamente pelo AD RMS para proteger documentos contendo PII (informações de identificação pessoal) alto. Somente membros do grupo FinanceAdmin terão acesso aos documentos que contêm PII Alto.|  
|[Exibir os resultados](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_4)|Examine a classificação dos documentos e observe como eles mudam ao alterar o conteúdo do documento. Verifique também como o documento é protegido pelo AD RMS.|  
|[Verificar proteção de AD RMS](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_5)|Verifique se o documento está protegido pelo AD RMS.|  
|||  
  
## <a name="BKMK_1.1"></a>Etapa 1: Habilitar as propriedades do recurso  
  
#### <a name="to-enable-resource-properties"></a>Para habilitar as propriedades do recurso  
  
1. No Gerenciador do Hyper-V, conecte-se ao servidor ID_AD_DC1. Entre no servidor usando Contoso\Administrator com a senha <strong>pass@word1</strong>.  
  
2. Abra o Centro Administrativo do Active Directory e clique em **Visualização de árvore**.  
  
3. Expanda **CONTROLE DE ACESSO DINÂMICO** e selecione **Propriedades do recurso**.  
  
4. Arraste para a propriedade **Impacto** na coluna **Nome de exibição**. Clique com o botão direito do mouse em **Impacto** e em **Habilitar**.  
  
5. Arraste para a propriedade **Informações de identificação pessoal** na coluna **Nome de exibição**. Clique com o botão direito do mouse em **Informações de Identificação Pessoal** e em **Habilitar**.  
  
6. Para publicar as propriedades do recurso na **Lista global de recurso**, clique em **Listas de propriedades do recurso** no painel à esquerda e clique duas vezes em **Lista global de propriedades do recurso**.  
  
7. Clique em **Adicionar**, arraste para baixo e clique em **Impacto** para adicioná-lo à lista. Faça o mesmo para **Informações de identificação pessoal**. Clique em **OK** duas vezes para concluir.  
  
![solution guia](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com"  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com" 
```  
  
## <a name="BKMK_2"></a>Etapa 2: Criar regras de classificação  
Esta etapa explica como criar a regra de classificação de **Alto impacto** . Esta regra pesquisará o conteúdo dos documentos e se a cadeia de caracteres "contoso Confidential" for encontrada, ele classificará este documento como tendo um impacto alto nos negócios. Essa classificação substituirá qualquer classificação previamente atribuída de baixo impacto nos negócios.  
  
Você também criará uma regra de **Alto PII**. Esta regra pesquisará o conteúdo dos documentos e, caso encontre um número de seguro social, classificará o documento como PII alto.  
  
#### <a name="to-create-the-high-impact-classification-rule"></a>Para criar a regra de classificação de alto impacto  
  
1. No Gerenciador do Hyper-V, conecte-se ao servidor ID_AD_FILE1. Entre no servidor usando Contoso\Administrator com a senha <strong>pass@word1</strong>.  
  
2. Será necessário atualizar as Propriedades globais do recurso do Active Directory. Abra o Windows PowerShell e digite: `Update-FSRMClassificationPropertyDefinition`e pressione ENTER. Feche o Windows PowerShell.  
  
3. Abra o Gerenciador de Recursos de Servidor de Arquivos. Para abrir o Gerenciador de Recursos de Servidor de Arquivos, clique em **Iniciar**, digite **gerenciador de recursos do servidor de arquivos**e clique em **Gerenciador de Recursos do Servidor de Arquivos**.  
  
4. No painel esquerdo do Gerenciador de Recursos de Servidor de Arquivos, expanda **Gerenciamento de classificação**e selecione **Regras de classificação**.  
  
5. No painel **Ações** , clique em **Configurar agenda de classificação**. Na guia **Classificação automática**, selecione **Habilitar agenda fixa**, selecione um **Dia da semana** e marque a caixa de seleção **Permitir a classificação contínua de novos arquivos**. Clique em **OK**.  
  
6. No painel **Ações**, clique em **Criar regra de classificação**. Isso abre a caixa de diálogo **Criar Regra de Classificação**.  
  
7. Na caixa **Nome da regra**, digite **Alto impacto nos negócios**.  
  
8. Na caixa **Descrição** , digite **determina se o documento tem alto impacto nos negócios com base na presença da cadeia de caracteres "confidencial da Contoso"**  
  
9. Na guia **Escopo** , clique em **Definir as propriedades de gerenciamento de pasta**, selecione **Uso da pasta**, clique em **Adicionar**e em **Navegar**, navegue para D:\Finance Documents como o caminho, clique em **OK**, escolha um valor da propriedade chamado **Arquivos do grupo** e clique em **Fechar**. Depois de definir as propriedades de gerenciamento, na guia **Escopo da regra** , selecione **Arquivos do grupo**.  
  
10. Clique na guia **Classificação**.  Em Escolha um método para atribuir a propriedade aos arquivos, selecione **Classificador de conteúdo** na lista suspensa.  
  
11. Em **Escolha uma propriedade para atribuir aos arquivos**, selecione **Impacto** na lista suspensa.  
  
12. Em **Especificar um valor**, selecione **Alto** na lista suspensa.  
  
13. Clique em **Configurar** em **Parâmetros**.  Na caixa de diálogo **Parâmetros de classificação** , na lista **Tipo de expressão** , selecione **Cadeia de caracteres**. Na caixa **Expressão**, digite: Contoso Confidential e clique em **OK**.  
  
14. Clique na guia **Tipo de avaliação** .  Clique em Reavaliar valores de propriedade existentes, em **Substituir**o valor existente e em **OK** para concluir.  
  
![solution guia](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
Update-FSRMClassificationPropertyDefinition  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name "High Business Impact" -Property "Impact_MS" -Description "Determines if the document has a high business impact based on the presence of the string 'Contoso Confidential'" -PropertyValue "3000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
#### <a name="to-create-the-high-pii-classification-rule"></a>Para criar a regra de classificação de PII alto  
  
1. No Gerenciador do Hyper-V, conecte-se ao servidor ID_AD_FILE1. Entre no servidor usando Contoso\Administrator com a senha <strong>pass@word1</strong>.  
  
2. Na área de trabalho, abra a pasta chamada **Expressões regulares** e abra o documento de texto chamado **RegEx-SSN**. Realce e copie a seguinte cadeia de caracteres de expressão regular: **^ (?! 000) ([0-7] \d @ no__t-1 | 7 ([0-7] \d | 7 [012])) ([-]?) (?! 00) \d\d\3 (?! 0000) \d @ no__t-2 $** . Esta cadeia de caracteres será usada posteriormente, por isso mantenha-a na área de transferência.  
  
3. Abra o Gerenciador de Recursos de Servidor de Arquivos. Para abrir o Gerenciador de Recursos de Servidor de Arquivos, clique em **Iniciar**, digite **gerenciador de recursos do servidor de arquivos**e clique em **Gerenciador de Recursos do Servidor de Arquivos**.  
  
4. No painel esquerdo do Gerenciador de Recursos de Servidor de Arquivos, expanda **Gerenciamento de classificação**e selecione **Regras de classificação**.  
  
5. No painel **Ações** , clique em **Configurar agenda de classificação**. Na guia **Classificação automática**, selecione **Habilitar agenda fixa**, selecione um **Dia da semana** e marque a caixa de seleção **Permitir a classificação contínua de novos arquivos**. Clique em OK.  
  
6. Na caixa **Nome da regra** , digite **PII alto**. Na caixa **Descrição**, digite **Determina se o documento possui um PII alto com base na presença de um número de seguro social.**  
  
7. Clique na guia **Escopo** e marque a caixa de seleção **Arquivos do grupo** .  
  
8. Clique na guia **Classificação**.  Em Escolha um método para atribuir a propriedade aos arquivos, selecione **Classificador de conteúdo** na lista suspensa.  
  
9. Em **Escolha uma propriedade para atribuir aos arquivos**, selecione **Informações de identificação pessoal** na lista suspensa.  
  
10. Em **Especificar um valor**, selecione **Alto** na lista suspensa.  
  
11. Clique em **Configurar** em **Parâmetros**.   
    No painel **Parâmetros de classificação**, na lista **Tipo de expressão** , selecione **Expressão Regular**. Na caixa **expressão** , Cole o texto da área de transferência: **^ (?! 000) ([0-7] \d @ no__t-2 | 7 ([0-7] \d | 7 [012])) ([-]?) (?! 00) \d\d\3 (?! 0000) \d @ no__t-3 $** e clique em **OK**.  
  
    > [!NOTE]  
    > Esta expressão permitirá números de seguro social inválidos. Isso permite usar os números de seguro social fictícios nesta demonstração.  
  
12. Clique na guia **Tipo de avaliação** .  Selecione Reavaliar valores de propriedade existentes, **Substituir**a valor existente e clique em **OK** para concluir.  
  
![solution guia](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
New-FSRMClassificationRule -Name "High PII" -Description "Determines if the document has a high PII based on the presence of a Social Security Number." -Property "PII_MS" -PropertyValue "5000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=1;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
Você deverá possuir duas regras de classificação:  
  
-   Alto impacto nos negócios  
  
-   Alto PII  
  
## <a name="BKMK_3"></a>Etapa 3: Usar tarefas de gerenciamento de arquivos para proteger automaticamente documentos com AD RMS  
Agora que você criou regras para classificar documentos automaticamente com base no conteúdo, a próxima etapa é criar uma tarefa de gerenciamento de arquivos que usa AD RMS para proteger automaticamente determinados documentos com base em sua classificação. Nesta etapa, você criará uma tarefa do gerenciador de arquivos para proteger automaticamente quaisquer documentos com um PII alto. Somente membros do grupo FinanceAdmin terão acesso aos documentos que contêm PII Alto.  
  
#### <a name="to-protect-documents-with-ad-rms"></a>Para proteger documentos com AD RMS  
  
1. No Gerenciador do Hyper-V, conecte-se ao servidor ID_AD_FILE1. Entre no servidor usando Contoso\Administrator com a senha <strong>pass@word1</strong>.  
  
2. Abra o Gerenciador de Recursos de Servidor de Arquivos. Para abrir o Gerenciador de Recursos de Servidor de Arquivos, clique em **Iniciar**, digite **gerenciador de recursos do servidor de arquivos**e clique em **Gerenciador de Recursos do Servidor de Arquivos**.  
  
3. No painel à esquerda, selecione **Tarefas de gerenciamento de arquivos**. No painel **Ações** , selecione **Criar tarefa de gerenciamento de arquivos**.  
  
4. No campo **Nome da tarefa:** , digite **PII alto**. No campo **Descrição**, digite **Proteção de RMS automática para documentos com PII alto**.  
  
5. Clique na guia **Escopo** e marque a caixa de seleção **Arquivos do grupo** .  
  
6. Clique na guia **Ações** . Em Tipo, selecione **Criptografia RMS**. Clique em **Navegar** para selecionar um modelo e selecione o modelo **Somente administradores de finanças da Contoso**.  
  
7. Clique na guia **Condição** e em **Adicionar**. Em **Propriedade**, selecione **Informações de identificação pessoal**. Em **Operador**, selecione**Igual a**. Em **Valor**, selecione **Alto**. Clique em **OK**.  
  
8. Clique na guia **Agenda** . Na seção Agenda, clique em **Semanal** e selecione o **Domingo**. Executar a tarefa uma vez por semana garantirá a identificação de qualquer documento que possa ter sido ignorado devido a uma falha de serviço ou outro evento de interrupção.  
  
9. Na seção **Operação contínua** , selecione **Executar a tarefa continuamente nos novos arquivos**e clique em **OK**. Agora, você terá uma tarefa de gerenciamento chamada PII alto.  
  
![solution guia](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
$fmjRmsEncryption = New-FSRMFmjAction -Type 'Rms' -RmsTemplate 'Contoso Finance Admin Only'  
$fmjCondition1 = New-FSRMFmjCondition -Property 'PII_MS' -Condition 'Equal' -Value '5000'  
$date = get-date  
$schedule = New-FsrmScheduledTask -Time $date -Weekly @('Sunday')    
$fmj1=New-FSRMFileManagementJob -Name "High PII" -Description "Automatic RMS protection for high PII documents" -Namespace @('D:\Finance Documents') -Action $fmjRmsEncryption -Schedule $schedule -Continuous -Condition @($fmjCondition1)  
```  
  
## <a name="BKMK_4"></a>Etapa 4: Ver os resultados  
É hora de dar uma olhada em sua nova classificação automática e regras de proteção de AD RMS em ação. Nesta etapa, você examinará a classificação dos documentos e observará como eles mudam ao alterar o conteúdo do documento.  
  
#### <a name="to-view-the-results"></a>Para ver os resultados  
  
1. No Gerenciador do Hyper-V, conecte-se ao servidor ID_AD_FILE1. Entre no servidor usando Contoso\Administrator com a senha <strong>pass@word1</strong>.  
  
2. No Windows Explorer, navegue para D:\Finance Documents.  
  
3. Clique com o botão direito no documento Memorando financeiro e clique em **Propriedades**. Clique na guia **Classificação** e observe que a propriedade Impacto ainda não possui valor. Clique em **Cancelar**.  
  
4. Clique com o botão direito do mouse no documento **Solicitação de aprovação para documento de contratação**e selecione **Propriedades**.  
  
5. Clique na guia **Classificação** e observe que a propriedade **Informações de identificação pessoal** ainda não possui valor. Clique em **Cancelar**.  
  
6. Alterne para o CLIENT1. Desconecte qualquer usuário que esteja conectado e, em seguida, entre como Contoso\MReid com a senha <strong>pass@word1</strong>.  
  
7. Na área de trabalho, abra a pasta compartilhada **Documentos financeiros** .  
  
8. Abra o documento **Memorando financeiro**. Próximo ao fim do documento, você verá a palavra **Confidential**. Modifique-a para: **Contoso Confidential**. Salve e feche o documento.  
  
9. Abra o documento **Solicitação de aprovação para contratação**. Na seção **Nº do seguro social:** , digite: 777-77-7777. Salve e feche o documento.  
  
    > [!NOTE]  
    > Pode ser necessário aguardar 30 segundos para que a classificação entre em vigor.  
  
10. Alterne novamente para o ID_AD_FILE1. No Windows Explorer, navegue para D:\Finance Documents.  
  
11. Clique com o botão direito do mouse no documento Memorando financeiro e clique em **Propriedades**. Clique na guia **Classificação**. Observe que a propriedade Impacto agora está definida para **Alto**. Clique em **Cancelar**.  
  
12. Clique com o botão direito do mouse no documento Solicitação de aprovação para contratação e clique em **Propriedades**.  
  
13. . Clique na guia **Classificação**. Observe que a propriedade Informações de identificação pessoal agora está definida para **Alto**. Clique em **Cancelar**.  
  
## <a name="BKMK_5"></a>Etapa 5: Verificar proteção com AD RMS  
  
#### <a name="to-verify-that-the-document-is-protected"></a>Para verificar se o documento está protegido  
  
1.  Alterne novamente para o ID_AD_CLIENT1.  
  
2.  Abra o documento **Solicitação de aprovação para contratação**.  
  
3.  Clique em **OK** para permitir que o documento se conecte ao servidor do AD RMS.  
  
4.  Agora, você pode ver que o documento foi protegido pelo AD RMS, pois contém um número de seguro social.  
  

