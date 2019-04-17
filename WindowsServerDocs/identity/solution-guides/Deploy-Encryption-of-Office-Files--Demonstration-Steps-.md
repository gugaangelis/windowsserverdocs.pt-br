---
ms.assetid: 2c76e81a-c2eb-439f-a89f-7d3d70790244
title: "Implantar a criptografia de arquivos do Office (etapas de demonstração)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 529000c60a80ee33fc2aa7d09370d8ac1e06311c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="deploy-encryption-of-office-files-demonstration-steps"></a>Implantar a criptografia de arquivos do Office (etapas de demonstração)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Departamento de finanças da Contoso tem um número de servidores de arquivos que armazenam seus documentos. Esses documentos podem ser documentação geral ou eles podem ter um impacto de negócios alto (AIN). Por exemplo, qualquer documento que contém informações confidenciais for considerado, pela Contoso, para ter um impacto de negócios alto. Contoso quer garantir que todos os seu documentação tem uma quantidade mínima de proteção e que sua documentação AIN é restrita para as pessoas adequadas. Para fazer isso, Contoso está explorando usando a infraestrutura de classificação de arquivo (FCI) e do AD RMS que está disponível no Windows Server 2012. Usando FCI, Contoso será classificar todos os documentos em seu servidor de arquivos, com base no conteúdo e usar o AD RMS para aplicar a política de direitos apropriados.  
  
Nesse cenário, você vai executar as seguintes etapas:  
  
|Tarefa|Descrição|  
|--------|---------------|  
|[Habilitar as propriedades do recurso](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_1.1)|Habilitar o **impacto** e **informações pessoalmente identificáveis** propriedades do recurso.|  
|[Criar regras de classificação](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_2)|Crie as seguintes regras de classificação: **regra de classificação AIN** e **regra de classificação de PII **.|  
|[Use tarefas de gerenciamento de arquivo para proteger automaticamente os documentos com o AD RMS](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_3)|Crie uma tarefa de gerenciamento de arquivo que usado automaticamente do AD RMS para proteger os documentos com altas informações pessoalmente identificáveis (PII). Apenas os membros do grupo FinanceAdmin terão acesso a documentos que contêm PII alto.|  
|[Exibir os resultados](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_4)|Examine a classificação de documentos e observe como eles mudam conforme você alterar o conteúdo no documento. Verifique também como o documento fica protegido pelo AD RMS.|  
|[Verifique se a proteção de AD RMS](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_5)|Verifique se que o documento está protegido com o AD RMS.|  
|||  
  
## <a name="BKMK_1.1"></a>Etapa 1: Habilitar propriedades do recurso  
  
#### <a name="to-enable-resource-properties"></a>Para habilitar as propriedades do recurso  
  
1.  No Gerenciador do Hyper-V, se conecte ao servidor ID_AD_DC1. Fazer logon servidor usando Contoso\administrador com a senha **pass@word1**.  
  
2.  Centro Administrativo do Open Active Directory e clique em **modo de exibição de árvore **.  
  
3.  Expanda **controle de acesso dinâmico**e selecione **propriedades do recurso **.  
  
4.  Role para baixo até o **impacto** propriedade no **nome de exibição** coluna. Clique com botão direito **impacto**e clique em **habilitar **.  
  
5.  Role para baixo até o **informações pessoalmente identificáveis** propriedade no **nome de exibição** coluna. Clique com botão direito **informações pessoalmente identificáveis**e clique em **habilitar **.  
  
6.  Para publicar as propriedades do recurso no **lista Global de recursos**, no painel esquerdo, clique em **listas de recursos de propriedade**e clique duas vezes em **lista Global de propriedade de recurso **.  
  
7.  Clique em **adicionar**e, em seguida, role para baixo e clique em **impacto** para adicioná-lo à lista. Faça o mesmo para **informações pessoalmente identificáveis **. Clique em **Okey** duas vezes para concluir.  
  
![guias de soluções](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com"  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com" 
```  
  
## <a name="BKMK_2"></a>Etapa 2: Criar as regras de classificação  
Esta etapa explica como criar o **alto impacto** regra de classificação. Essa regra irá procurar o conteúdo de documentos e se a cadeia de caracteres "Contoso confidenciais" for encontrada, ele classificará neste documento como tendo impacto comercial alto. Essa classificação substituirá qualquer classificação anteriormente atribuída de impacto comercial baixo.  
  
Você também criará um **PII alto** regra. Essa regra pesquisa o conteúdo de documentos e, se um número de CPF for encontrado, ele classifica o documento como tendo PII alto.  
  
#### <a name="to-create-the-high-impact-classification-rule"></a>Para criar a regra de classificação de alto impacto  
  
1.  No Gerenciador do Hyper-V, se conecte ao servidor ID_AD_FILE1. Fazer logon servidor usando Contoso\administrador com a senha **pass@word1**.  
  
2.  Você precisa atualizar as propriedades do recurso Global do Active Directory. Abra o Windows PowerShell e digite: `Update-FSRMClassificationPropertyDefinition`, e pressione ENTER. Feche o Windows PowerShell.  
  
3.  Abra o Gerenciador de recursos do servidor de arquivos. Para abrir o Gerenciador de recursos do servidor de arquivos, clique em **iniciar**, tipo **Gerenciador de recursos do servidor de arquivos**e clique em **Gerenciador de recursos do servidor de arquivos**.  
  
4.  No painel esquerdo do Gerenciador de recursos de servidor de arquivos, expanda **gerenciamento de classificação**e, em seguida, selecione **as regras de classificação **.  
  
5.  No **ações** painel, clique em **configurar agendamento de classificação **. No **classificação automática**, selecione **habilitar agendamento fixo**, selecione um **dia da semana**e, em seguida, selecione o **permitir classificação contínua para os novos arquivos** caixa de seleção. Clique em **Okey**.  
  
6.  No **ações** painel, clique em **criar a regra de classificação **. Isso abre a **criar a regra de classificação** caixa de diálogo.  
  
7.  No **nome da regra**, digite **alto impacto comercial **.  
  
8.  No **descrição**, digite **determina se o documento tem um alto impacto comercial com base na presença da cadeia de caracteres "Contoso confidenciais"**  
  
9. No **escopo**, clique em **definir propriedades de gerenciamento de pasta**, selecione **uso da pasta**, clique em **adicionar**, em seguida, clique em **procurar**, vá até documentos D:\Finance como o caminho, clique em **Okey**e, em seguida, escolha um valor de propriedade denominado **agrupar arquivos** e clique em **fechar **. Depois que o gerenciamento de propriedades é definido, pela **escopo de regra** guia selecione **agrupar arquivos **.  
  
10. Clique no **Classification** guia.  Em **escolher um método para atribuir a propriedade arquivos**, selecione **classificador conteúdo** na lista suspensa.  
  
11. Em **escolher uma propriedade para atribuir a arquivos**, selecione **impacto** na lista suspensa.  
  
12. Em **especificar um valor**, selecione **alto** na lista suspensa.  
  
13. Clique em **configurar** em **parâmetros **.  No **parâmetros de classificação** na caixa o **tipo de expressão** lista, selecione **cadeia de caracteres **. No **expressão**, digite: **Contoso confidenciais**e clique em **Okey **.  
  
14. Clique no **avaliação tipo** guia.  Clique em **reavaliar valores de propriedade existentes**, clique em **Overwrite**existente valor e, em seguida, clique em **Okey** ao fim.  
  
![guias de soluções](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.  
  
```  
Update-FSRMClassificationPropertyDefinition  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name "High Business Impact" -Property "Impact_MS" -Description "Determines if the document has a high business impact based on the presence of the string 'Contoso Confidential'" -PropertyValue "3000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
#### <a name="to-create-the-high-pii-classification-rule"></a>Para criar a regra de classificação PII alto  
  
1.  No Gerenciador do Hyper-V, se conecte ao servidor ID_AD_FILE1. Fazer logon servidor usando Contoso\administrador com a senha **pass@word1**.  
  
2.  Na área de trabalho, abra a pasta denominada **expressões regulares**e, em seguida, abra o documento de texto chamado **RegEx SSN **. Realçar e copiar a seguinte cadeia de caracteres de expressão regular: **^ (?! 000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-]?) (?! 00) \d\d\3 (?! \d {4}$ 0000)**. Essa cadeia de caracteres será usada mais adiante nesta etapa assim mantê-lo em sua área de transferência.  
  
3.  Abra o Gerenciador de recursos do servidor de arquivos. Para abrir o Gerenciador de recursos do servidor de arquivos, clique em **iniciar**, tipo **Gerenciador de recursos do servidor de arquivos**e clique em **Gerenciador de recursos do servidor de arquivos**.  
  
4.  No painel esquerdo do Gerenciador de recursos de servidor de arquivos, expanda **gerenciamento de classificação**e, em seguida, selecione **as regras de classificação **.  
  
5.  No **ações** painel, clique em **configurar agendamento de classificação **. No **classificação automática**, selecione **habilitar agendamento fixo**, selecione um **dia da semana**e, em seguida, selecione o **permitir classificação contínua para os novos arquivos** caixa de seleção. Clique em Okey.  
  
6.  No **nome da regra**, digite **PII alto **. No **descrição**, digite **determina se o documento tem um alto PII com base na presença de um número de CPF.**  
  
7.  Clique no **escopo**, selecione o **agrupar arquivos** caixa de seleção.  
  
8.  Clique no **Classification** guia.  Em **escolher um método para atribuir a propriedade arquivos**, selecione **classificador conteúdo** na lista suspensa.  
  
9. Em **escolher uma propriedade para atribuir a arquivos**, selecione **informações pessoalmente identificáveis** na lista suspensa.  
  
10. Em **especificar um valor**, selecione **alto** na lista suspensa.  
  
11. Clique em **configurar** em **parâmetros **.   
    No **parâmetros de classificação**janela, no **tipo de expressão** lista, selecione **Expressão Regular **. No **expressão** caixa, colar o texto da área de transferência: **^ (?! 000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-]?) (?! 00) \d\d\3 (?! \d {4}$ 0000)**e clique em **Okey **.  
  
    > [!NOTE]  
    > Essa expressão permitirá que os números da Previdência Social inválidos. Isso nos permite usar números da Previdência Social fictícios na demonstração.  
  
12. Clique no **avaliação tipo** guia.  Selecione **reavaliar valores de propriedade existentes**, **Overwrite**existente valor e, em seguida, clique em **Okey** ao fim.  
  
![guias de soluções](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.  
  
```  
New-FSRMClassificationRule -Name "High PII" -Description "Determines if the document has a high PII based on the presence of a Social Security Number." -Property "PII_MS" -PropertyValue "5000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=1;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
Agora você deve ter duas regras de classificação:  
  
-   Alto impacto comercial  
  
-   PII alto  
  
## <a name="BKMK_3"></a>Etapa 3: Usar tarefas de gerenciamento de arquivo para proteger automaticamente os documentos com o AD RMS  
Agora que você já criou as regras para classificar automaticamente os documentos com base no conteúdo, a próxima etapa é criar uma tarefa de gerenciamento de arquivos que usa AD RMS para proteger automaticamente certos documentos com base em suas classificações. Nesta etapa, você criará uma tarefa de gerenciamento de arquivos que protege automaticamente todos os documentos com um PII alto. Apenas os membros do grupo FinanceAdmin terão acesso a documentos que contêm PII alto.  
  
#### <a name="to-protect-documents-with-ad-rms"></a>Para proteger documentos com o AD RMS  
  
1.  No Gerenciador do Hyper-V, se conecte ao servidor ID_AD_FILE1. Fazer logon servidor usando Contoso\administrador com a senha **pass@word1**.  
  
2.  Abra o Gerenciador de recursos do servidor de arquivos. Para abrir o Gerenciador de recursos do servidor de arquivos, clique em **iniciar**, tipo **Gerenciador de recursos do servidor de arquivos**e clique em **Gerenciador de recursos do servidor de arquivos**.  
  
3.  No painel esquerdo, selecione **tarefas de gerenciamento de arquivo **. No **ações** painel, selecione **criar tarefas de gerenciamento de arquivo **.  
  
4.  No **nome da tarefa:**, digite **PII alto **. No **descrição**, digite **proteção de RMS automática para documentos PII alto **.  
  
5.  Clique no **escopo**, selecione o **agrupar arquivos** caixa de seleção.  
  
6.  Clique no **ação** guia. Em **tipo**, selecione **criptografia do RMS **. Clique em **procurar** para selecionar um modelo e, em seguida, selecione o **Contoso Finanças Admin apenas** modelo.  
  
7.  Clique no **condição** guia e, em seguida, clique em **adicionar **. Em **propriedade**, selecione **informações pessoalmente identificáveis **. Em **operador**, selecione **igual **. Em **valor**, selecione **alto **. Clique em **Okey**.  
  
8.  Clique no **agendamento** guia. No **agendamento** seção, clique em **semanais**e, em seguida, selecione **domingo **. Executar a tarefa uma vez por semana garantirá que você capture todos os documentos que possam ter sido perdidos devido a uma queda de serviço ou outros eventos de interrupção.  
  
9. No **operação contínua** seção, selecione **executar tarefa continuamente em novos arquivos**e clique em **Okey **. Agora você deve ter uma tarefa de gerenciamento de arquivo chamada PII alto.  
  
![guias de soluções](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.  
  
```  
$fmjRmsEncryption = New-FSRMFmjAction -Type 'Rms' -RmsTemplate 'Contoso Finance Admin Only'  
$fmjCondition1 = New-FSRMFmjCondition -Property 'PII_MS' -Condition 'Equal' -Value '5000'  
$date = get-date  
$schedule = New-FsrmScheduledTask -Time $date -Weekly @('Sunday')    
$fmj1=New-FSRMFileManagementJob -Name "High PII" -Description "Automatic RMS protection for high PII documents" -Namespace @('D:\Finance Documents') -Action $fmjRmsEncryption -Schedule $schedule -Continuous -Condition @($fmjCondition1)  
```  
  
## <a name="BKMK_4"></a>Etapa 4: Exibir os resultados  
É hora de dar uma olhada sua nova classificação automática e regras de proteção de AD RMS em ação. Nesta etapa você examinará a classificação de documentos e observe como eles mudam conforme você alterar o conteúdo no documento.  
  
#### <a name="to-view-the-results"></a>Para exibir os resultados  
  
1.  No Gerenciador do Hyper-V, se conecte ao servidor ID_AD_FILE1. Fazer logon servidor usando Contoso\administrador com a senha **pass@word1**.  
  
2.  No Windows Explorer, navegue até D:\Finance documentos.  
  
3.  Clique com botão direito no documento de memorando finanças e clique em **propriedades**. Clique no **Classification** guia e observe que a propriedade impacto atualmente não tem valor. Clique em **Cancelar **.  
  
4.  Clique com botão direito do **solicitação para aprovação contratação documento**e, em seguida, selecione **propriedades **.  
  
5.  Clique no **Classification** guia e observe que **a propriedade informações pessoalmente identificáveis** atualmente não tem valor. Clique em **Cancelar **.  
  
6.  Alternar para CLIENT1. Aprovação de qualquer usuário que está conectado e, em seguida, faça logon como Contoso\MReid com a senha **pass@word1**.  
  
7.  Na área de trabalho, abra o **documentos de finanças** pasta compartilhada.  
  
8.  Abrir o **Finanças memorando** documento. Na parte inferior do documento, você verá a palavra **confidencial **. Modificá-lo para ler: **Contoso confidenciais **. Salve o documento e fechá-lo.  
  
9. Abrir o **solicitação de aprovação para contratação** documento. No **CPF #:** seção, digite: 777-77-7777. Salve o documento e fechá-lo.  
  
    > [!NOTE]  
    > Talvez seja necessário aguardar 30 segundos para a classificação ocorra.  
  
10. Alternar de volta para ID_AD_FILE1. No Windows Explorer, navegue até D:\Finance documentos.  
  
11. Clique com botão direito no documento de memorando finanças e clique em **propriedades **. Clique no **Classification** guia. Observe que o **impacto** propriedade agora é definida como **alto **. Clique em **Cancelar **.  
  
12. Clique com botão direito a solicitação de aprovação de documento de contratação e clique em **propriedades **.  
  
13. . Clique no **Classification** guia. Observe que o **informações pessoalmente identificáveis** propriedade agora é definida como **alto **. Clique em **Cancelar **.  
  
## <a name="BKMK_5"></a>Etapa 5: Verifique se a proteção de AD RMS  
  
#### <a name="to-verify-that-the-document-is-protected"></a>Para verificar se o documento está protegido  
  
1.  Alternar de volta para ID_AD_CLIENT1.  
  
2.  Abrir o **solicitação de aprovação para contratação** documento.  
  
3.  Clique em **Okey** para permitir que o documento para se conectar ao seu servidor de AD RMS.  
  
4.  Agora você pode ver que o documento foi protegido pelo AD RMS porque ele contém um número de CPF.  
  

