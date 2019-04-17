---
ms.assetid: 8738c03d-6ae8-49a7-8b0c-bef7eab81057
title: "Implantar uma política de acesso Central (etapas de demonstração)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 16973b32407985ffc3f66bf5ac579384a756b5d0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-a-central-access-policy-demonstration-steps"></a>Implantar uma política de acesso Central (etapas de demonstração)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Nesse cenário, as operações de segurança do departamento de finanças está trabalhando com a segurança das informações central para especificar a necessidade de uma política de acesso central para que eles podem proteger as informações de finanças arquivado armazenadas em servidores de arquivos. As informações de finanças arquivado de cada país podem ser acessadas como somente leitura por funcionários de finanças do mesmo país. Um grupo de administração de finanças central pode acessar as informações de Finanças de todos os países.  
  
Implantar uma política de acesso central inclui as seguintes fases:  
  
|Fase|Descrição  
|---------|---------------  
|[Planejamento: Identificar a necessidade de política e a configuração necessária para implantação](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.2)|Identifique a necessidade de uma política e a configuração necessária para implantação. 
|[Implemente: Configurar os componentes e política](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.3)|Configure os componentes e a política.  
|[Implantar a política de acesso central](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.4)|Implante a política.  
|[Manter: Alterar e testar a política](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.5)|Alterações de política e teste. 
  
## <a name="BKMK_1.1"></a>Configurar um ambiente de teste  
Antes de começar, você precisa configurar o laboratório para testar esse cenário. As etapas para configurar o laboratório são explicadas mais detalhadamente em [apêndice b: configuração de backup o ambiente de teste ](Appendix-B--Setting-Up-the-Test-Environment.md).  
  
## <a name="BKMK_1.2"></a>Planejamento: Identificar a necessidade de política e a configuração necessária para implantação  
Esta seção fornece a série de alto nível das etapas que auxiliam na fase de planejamento da implantação.  
  
||Etapa|Exemplo|  
|-|--------|-----------|  
|1.1|Negócios determina que é necessária uma política de acesso central|Para proteger as informações de finanças são armazenadas nos servidores de arquivos, as operações de segurança do departamento de finanças está trabalhando com segurança das informações central para especificar a necessidade de uma política de acesso central.|  
|1.2|Expressar a política de acesso|Documentos de finanças só devem ser lido por membros do departamento de finanças. Membros do departamento de finanças só devem acessar documentos em seu próprio país. Somente os administradores de finanças devem ter acesso de gravação. Uma exceção será permitida para os membros do grupo FinanceException. Esse grupo terá acesso de leitura.|  
|1.3|Express constrói a política de acesso no Windows Server 2012|Direcionamento:<br /><br />-Resource.Department contém Finanças<br /><br />Regras de acesso:<br /><br />-Permitir lidos User.Country=Resource.Country e User.department = Resource.Department<br />-Permitir controle total User.MemberOf(FinanceAdmin)<br /><br />Exceção:<br /><br />Permitir memberOf(FinanceException) leitura|  
|1.4|Determinar as propriedades de arquivo necessárias para a política|Arquivos de marca com:<br /><br />-Departamento<br />-País|  
|1.5|Determinar os tipos de declaração e os grupos necessários para a política|Tipos de declaração:<br /><br />-País<br />-Departamento<br /><br />Grupos de usuários:<br /><br />-FinanceAdmin<br />-FinanceException|  
|1.6|Determinar os servidores nos quais aplicar essa política|Aplica a política em todos os servidores de arquivos de finanças.|  
  
## <a name="BKMK_1.3"></a>Implemente: Configurar os componentes e política  
Esta seção apresenta um exemplo que implanta uma política de acesso central para documentos de finanças.  
  
|Não|Etapa|Exemplo|  
|------|--------|-----------|  
|2.1|Criar tipos de declaração|Crie os seguintes tipos de declaração:<br /><br />-Departamento<br />-País|  
|2.2|Criar propriedades do recurso|Criar e habilitar as seguintes propriedades do recurso:<br /><br />-Departamento<br />-País|  
|2.3|Configurar uma regra de acesso central|Crie uma regra de finanças documentos que inclui a política de determinado na seção anterior.|  
|2.4|Configurar uma política de acesso central (CAP)|Crie um limite chamado política finanças e adicione a regra de finanças documentos para esse limite.|  
|2.5|Política de acesso central de destino aos servidores de arquivos|Publica o limite de utilização de política de finanças aos servidores de arquivos.|  
|2.6|Habilite o suporte KDC para declarações, autenticação composta e proteção Kerberos.|Habilite o suporte KDC para declarações, autenticação composta e proteção Kerberos para contoso.com.|  
  
O procedimento a seguir, você cria dois tipos de declaração: país e departamento.  
  
#### <a name="to-create-claim-types"></a>Para criar os tipos de declaração  
  
1.  Abra DC1 do servidor no Gerenciador do Hyper-V e o log em como Contoso\administrador, com a senha **pass@word1**.  
  
2.  Abra o Centro Administrativo do Active Directory.  
  
3.  Clique no **ícone modo de exibição de árvore**, expanda **controle de acesso dinâmico**e, em seguida, selecione **tipos de declaração **.  
  
    Clique com botão direito **tipos de declaração**, clique em **nova**e clique em **tipo de declaração **.  
  
    > [!TIP]  
    > Você também pode abrir um **criar o tipo de declaração:** janela a partir do **tarefas** painel. Sobre o **tarefas** painel, clique em **nova**e clique em **tipo de declaração **.  
  
4.  No **atributo Source** listar, role para baixo a lista de atributos e clique em **departamento**. Isso deve preencher o **nome de exibição** campo com **departamento**. Clique em **Okey**.  
  
5.  Em **tarefas** painel, clique em **nova**e clique em **tipo de declaração**.  
  
6.  No **atributo Source** listar, role para baixo a lista de atributos e, em seguida, clique no **c** atributo (país-Name). No **nome de exibição**, digite **país**.  
  
7.  No **valores sugeridos** seção, selecione **sugerimos os seguintes valores:**e clique em **adicionar**.  
  
8.  No **valor** e **nome de exibição** campos, digite **EUA**e clique em **Okey**.  
  
9. Repita a etapa acima. No **adicione um valor de sugerir** caixa de diálogo, digite **JP** no **valor** e **nome de exibição** campos e clique em **Okey**.  
  
![guias de soluções](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.  
  
 
    New-ADClaimType country -SourceAttribute c -SuggestedValues:@((New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("US","US","")), (New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("JP","JP","")))  
    New-ADClaimType department -SourceAttribute department  
      
 
  
> [!TIP]  
> Você pode usar o Visualizador de histórico do Windows PowerShell no Centro Administrativo do Active Directory para procurar os cmdlets do Windows PowerShell para cada procedimento que executar no Centro Administrativo do Active Directory. Para obter mais informações, consulte [Visualizador de histórico do Windows PowerShell](https://technet.microsoft.com/library/hh831702)  
  
A próxima etapa é criar propriedades do recurso. O procedimento a seguir você cria uma propriedade de recurso que é automaticamente adicionada à lista de propriedades do recurso Global no controlador de domínio, para que ele fique disponível para o servidor de arquivos.  
  
#### <a name="to-create-and-enable-pre-created-resource-properties"></a>Para criar e habilitar as propriedades do recurso pré-criados  
  
1.  No painel à esquerda do Centro Administrativo do Active Directory, clique em **modo de exibição de árvore**. Expanda **controle de acesso dinâmico**e, em seguida, selecione **propriedades do recurso**.  
  
2.  Clique com botão direito **propriedades do recurso**, clique em **nova**e clique em **referência de recurso propriedade**.  
  
    > [!TIP]  
    > Você também pode escolher uma propriedade de recurso da **tarefas** painel. Clique em **nova** e, em seguida, clique em **referência de recurso propriedade**.  
  
3.  Em **lista de valores de selecionar um tipo de declaração para compartilhá-los é sugerido**, clique em **país**.  
  
4.  No **nome de exibição**, digite **país**e clique em **Okey**.  
  
5.  Clique duas vezes o **propriedades do recurso** listar, role para baixo até o **departamento** propriedade resource. Clique com botão direito e clique em **habilitar**. Isso permitirá que integrados **departamento** propriedade resource.  
  
6.  No **propriedades do recurso** lista no painel de navegação o Centro Administrativo do Active Directory, agora você terá duas propriedades do recurso habilitado:  
  
    -   País  
  
    -   Departamento  
  
![guias de soluções](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.  
  
```  
New-ADResourceProperty Country -IsSecured $true -ResourcePropertyValueType MS-DS-MultivaluedChoice -SharesValuesWith country  
Set-ADResourceProperty Department_MS -Enabled $true  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Country  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Department_MS  
  
```  
  
A próxima etapa é criar regras de acesso central que definem quem podem acessar os recursos. Neste cenário as regras de negócios são:  
  
-   Documentos de finanças podem ser lidas apenas por membros do departamento de finanças.  
  
-   Membros do departamento de finanças podem acessar somente os documentos em seu próprio país.  
  
-   Apenas os administradores de Finanças pode ter acesso de gravação.  
  
-   Nós permitirá que uma exceção para membros do grupo FinanceException. Esse grupo terá acesso de leitura.  
  
-   O administrador e o proprietário do documento ainda terão acesso completo.  
  
Ou para expressar as regras com construções do Windows Server 2012:  
  
Direcionamento: Resource.Department contém Finanças  
  
Regras de acesso:  
  
-   Permitir User.Country=Resource.Country de leitura e User.department = Resource.Department  
  
-   Permitir controle total User.MemberOf(FinanceAdmin)  
  
-   Permitir User.MemberOf(FinanceException) de leitura  
  
#### <a name="to-create-a-central-access-rule"></a>Para criar uma regra de acesso central  
  
1.  No painel esquerdo do Centro Administrativo do Active Directory, clique em **modo de exibição de árvore**, selecione **controle de acesso dinâmico**e clique em **regras de acesso Central**.  
  
2.  Clique com botão direito **regras de acesso Central**, clique em **nova**e clique em **regras de acesso Central**.  
  
3.  No **nome**, digite **Finanças documentos regra**.  
  
4.  No **recursos de destino** seção, clique em **editar**e no **regras de acesso Central** caixa de diálogo, clique em **adicionar uma condição**. Adicione as seguintes condições:   
    [**Recurso**] [**Departamento**] [**é igual a**] [**Value**] [**Finanças**] e, em seguida, clique em **Okey**.  
  
5.  No **permissões** seção, selecione **execute os seguintes permissões como permissões atuais**, clique em **editar**e no **configurações de segurança avançadas para permissões** click da caixa de diálogo **adicionar**.  
  
    > [!NOTE]  
    > **Use as seguintes permissões como permissões propostas** opção permite que você crie a política no teste. Para obter mais informações sobre como fazer isso consultem o manter: mudança e estágio a seção política neste tópico.  
  
6.  No **entrada de permissão para permissões** caixa de diálogo, clique em **selecionar uma entidade de segurança**, tipo **usuários autenticados**e clique em **Okey**.  
  
7.  No **entrada de permissão para permissões** caixa de diálogo, clique em **adicionar uma condição**e adicione as seguintes condições:   
    [**User**] [**país**] [**Any of**] [**Recurso**] [**país**]   
     Clique em **adicionar uma condição**.   
     [**And**]   
    Clique em [**usuário**] [**departamento**] [**qualquer uma das**] [**recurso**] [**departamento**]. Defina o **permissões** para **leitura**.  
  
8.  Clique em **Okey**e clique em **adicionar**. Clique em **selecionar uma entidade de segurança**, tipo **FinanceAdmin**e clique em **Okey**.  
  
9. Selecione o **modificar, ler e executar, ler, gravar** permissões e clique em **Okey**.  
  
10. Clique em **adicionar**, clique em **selecionar uma entidade de segurança**, tipo **FinanceException**e clique em **Okey**. Selecione as permissões para ser **leitura** e **ler e executar**.  
  
11. Clique em **Okey** três vezes para concluir e retornar ao centro administrativo do Active Directory.  
  
    ![guias de soluções](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
    O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.  
  
 
    $countryClaimType = Get-ADClaimType país  
    $departmentClaimType = Get-ADClaimType departamento  
    $countryResourceProperty = Get-ADResourceProperty país  
    $departmentResourceProperty = Get-ADResourceProperty departamento  
    $currentAcl = "O:SYG:SYD:AR(A;; FA;; O W) (A; FA;; BA) (A; 0 x1200a9;; S-1-5-21-1787166779-1215870801-2157059049-1113) (A; 0 x1301bf;; S-1-5-21-1787166779-1215870801-2157059049-1112) (A; FA;; SY) (XA; 0 x1200a9;; AU;((@USER." + $countryClaimType.Name + "Any_of @RESOURCE." + $countryResourceProperty.Name + ") & & (@USER." + $departmentClaimType.Name + "Any_of @RESOURCE." + $departmentResourceProperty.Name + ")))"  
    $resourceCondition = "(@RESOURCE." + $departmentResourceProperty.Name + "Contains {`"Finance`"}) "  
    New-ADCentralAccessRule "Finanças documentos regra" - CurrentAcl $currentAcl - ResourceCondition $resourceCondition  

  
> [!IMPORTANT]  
> No exemplo acima do cmdlet, os identificadores de segurança (SIDs) para o grupo FinanceAdmin e usuários é determinado no momento da criação e será diferentes no seu exemplo. Por exemplo, o SID valor fornecido (S-1-5-21-1787166779-1215870801-2157059049-1113) para o FinanceAdmins precisa ser substituído com o SID para o grupo de FinanceAdmin que você precisa criar em sua implantação real. Você pode usar o Windows PowerShell para procurar o valor do SID desse grupo, atribua esse valor a uma variável e, em seguida, use a variável aqui. Para obter mais informações, consulte [dica do Windows PowerShell: Trabalhando com SIDs](https://go.microsoft.com/fwlink/?LinkId=253545).  
  
Agora você deve ter uma regra de acesso central que permite que as pessoas acessem documentos do mesmo país e o mesmo departamento. A regra permite que o grupo FinanceAdmin editar os documentos e permite que o grupo FinanceException ler os documentos. Essa regra destina-se apenas os documentos classificados como finanças.  
  
#### <a name="to-add-a-central-access-rule-to-a-central-access-policy"></a>Para adicionar uma regra de acesso central a uma política de acesso central  
  
1.  No painel esquerdo do Centro Administrativo do Active Directory, clique em **controle de acesso dinâmico**e clique em **políticas de acesso Central**.  
  
2.  No **tarefas** painel, clique em **nova**e clique em **política de acesso Central**.  
  
3.  Em **criar a política de acesso Central:**, tipo **Finanças política** no **nome** caixa.  
  
4.  Em **regras de acesso central membro**, clique em **adicionar**.  
  
5.  Clique duas vezes o **Finanças documentos regra** para adicioná-lo para o **adicione as seguintes regras de acesso central** listar e clique em **Okey **.  
  
6.  Clique em **Okey** ao fim. Agora você deve ter uma política de acesso central chamada de política de finanças.  
  
    ![guias de soluções](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
    O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.  
  
    ```  
    New-ADCentralAccessPolicy "Finance Policy" Add-ADCentralAccessPolicyMember   
    -Identity "Finance Policy"   
    -Member "Finance Documents Rule"  
  
    ```  
  
#### <a name="to-apply-the-central-access-policy-across-file-servers-by-using-group-policy"></a>Para aplicar a política de acesso central em servidores de arquivos usando a política de grupo  
  
1.  No **iniciar** de tela, além do **pesquisa**, digite **Group Policy Management **. Clique duas vezes em **gerenciamento de política de grupo **.  
  
    > [!TIP]  
    > Se o **ferramentas administrativas Mostrar** configuração está desabilitada, o **ferramentas administrativas** pasta e seu conteúdo não aparecerão no **configurações** resultados.  
  
    > [!TIP]  
    > No ambiente de produção, você deve criar uma unidade de organização de servidor de arquivos (UO) e adicionar todos os seus servidores de arquivos para essa UO à qual você deseja aplicar essa política. Em seguida, você pode criar uma política de grupo e adicionar essa UO para essa política.  
  
2.  Nesta etapa, você editar o objeto de diretiva de grupo que você criou na [compilar o controlador de domínio](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_Build) seção no ambiente de teste para incluir a política de acesso central que você criou. No Editor de gerenciamento de política de grupo, navegue até e selecione a unidade organizacional no domínio (contoso.com neste exemplo): **Group Policy Management**, **floresta: contoso.com**, **domínios**, **contoso.com**, **Contoso**, **FileServerOU **.  
  
3.  Clique com botão direito **FlexibleAccessGPO**e clique em **editar **.  
  
4.  Na janela do Editor de gerenciamento de política de grupo, navegue até **configuração do computador**, expanda **políticas**, expanda **configurações do Windows**e clique em **configurações de segurança **.  
  
5.  Expanda **sistema de arquivos**, clique com botão direito **política de acesso Central**e clique em **políticas de acesso Central gerenciar**.  
  
6.  No **configuração de políticas de acesso Central** caixa de diálogo caixa, adicione **Finanças política**e clique em **Okey **.  
  
7.  Role para baixo até **configuração avançada de política de auditoria**e expandi-la.  
  
8.  Expanda **políticas de auditoria**e selecione **acesso a objetos **.  
  
9. Clique duas vezes em **preparo de política de acesso Central de auditoria **. Selecione todas as três caixas de seleção e, em seguida, clique em **Okey **. Esta etapa permite que o sistema receber eventos de auditoria relacionados a políticas de preparo de acesso Central.  
  
10. Clique duas vezes em **propriedades do sistema de arquivos de auditoria **. Selecione todas as três caixas de seleção e clique em **Okey **.  
  
11. Feche o Editor de gerenciamento de política de grupo. Agora você ter incluído a política de acesso central para a política de grupo.  
  
Para controladores de domínio do domínio fornecer declarações ou dados de autorização do dispositivo, os controladores de domínio precisam ser configurados para dar suporte ao controle de acesso dinâmico.  
  
#### <a name="to-enable-support-for-claims-and-compound-authentication-for-contosocom"></a>Para habilitar o suporte para declarações e autenticação composta para contoso.com  
  
1.  Abra o gerenciamento de política de grupo, clique em **contoso.com**e clique em **controladores de domínio **.  
  
2.  Clique com botão direito **política de controladores de domínio padrão**e clique em **editar**.  
  
3.  Na janela do Editor de gerenciamento de política de grupo, clique duas vezes em **configuração do computador**, clique duas vezes em **políticas**, clique duas vezes em **modelos administrativos**, clique duas vezes em **sistema**e clique duas vezes em **KDC**.  
  
4.  Clique duas vezes em **suporte KDC para declarações, autenticação composta e proteção Kerberos **. No **suporte KDC para declarações, autenticação composta e proteção Kerberos** caixa de diálogo, clique em **Enabled** e selecione **Supported** do **opções** lista suspensa. (Você precisa habilitar essa configuração usar as declarações do usuário em políticas de acesso central.)  
  
5.  Fechar **gerenciamento de política de grupo**.  
  
6.  Abra um prompt de comando e digite `gpupdate /force`.  
  
## <a name="BKMK_1.4"></a>Implantar a política de acesso central  
  
||Etapa|Exemplo|  
|-|--------|-----------|  
|3.1|Atribua o limite de utilização para as pastas compartilhadas apropriadas no servidor de arquivos.|Atribua a política de acesso central para a pasta compartilhada apropriada no servidor de arquivos.|  
|3.2|Verifique se o acesso é configurado corretamente.|Verifique o acesso de usuários diferentes países e departamentos.|  
  
Nesta etapa, você atribuirá a política de acesso central para um servidor de arquivos. Você vai fazer logon em um servidor de arquivo que está recebendo a política de acesso central que você criou as etapas anteriores e atribuí-la para uma pasta compartilhada.  
  
#### <a name="to-assign-a-central-access-policy-to-a-file-server"></a>Para atribuir uma política de acesso central para um servidor de arquivos  
  
1.  No Gerenciador do Hyper-V, se conecte ao servidor Arquivo1. O servidor usando Contoso\administrador com a senha de logon:**pass@word1**.  
  
2.  Abra um prompt de comando com privilégios elevados e digite: **gpupdate /force**. Isso garante que as alterações de política de grupo entrem em vigor em seu servidor.  
  
3.  Você também precisa atualizar as propriedades do recurso Global do Active Directory. Abra uma janela do Windows PowerShell com privilégios elevados e digite `Update-FSRMClassificationpropertyDefinition`. Pressione a tecla ENTER e, em seguida, feche o Windows PowerShell.  
  
    > [!TIP]  
    > Você também pode atualizar as propriedades do recurso Global por logon o servidor de arquivos. Para atualizar as propriedades do recurso Global do servidor de arquivos, faça o seguinte  
    >   
    > 1.  Logon arquivo1 do servidor de arquivos como Contoso\administrador, usando a senha **pass@word1**.  
    > 2.  Abra o Gerenciador de recursos do servidor de arquivos. Para abrir o Gerenciador de recursos do servidor de arquivos, clique em **iniciar**, tipo **Gerenciador de recursos do servidor de arquivos**e clique em **Gerenciador de recursos do servidor de arquivos**.  
    > 3.  No Gerenciador de recursos de servidor de arquivos, clique em **gerenciamento de classificação de arquivo**, clique com botão direito **propriedades de classificação** e, em seguida, clique em **atualizar **.  
  
4.  Abra o Windows Explorer e no painel esquerdo, clique em unidade D. Clique com botão direito do **documentos de finanças** pasta e clique em **propriedades **.  
  
5.  Clique no **Classification**, clique em **país**e, em seguida, selecione **EUA** no **valor** campo.  
  
6.  Clique em **departamento**, em seguida, selecione **Finanças** no **valor** campo e, em seguida, clique em **aplicar **.  
  
    > [!NOTE]  
    > Lembre-se de que a política de acesso central tiver sido configurada para arquivos de destino para o departamento de finanças. As etapas anteriores marcar todos os documentos na pasta com os atributos de país e departamento.  
  
7.  Clique no **segurança** guia e, em seguida, clique em **avançado**. Clique no **política Central** guia.  
  
8.  Clique em **alteração**, selecione **Finanças política** do menu suspenso e depois clique em **aplicar **. Você pode ver o **Finanças documentos regra** listados na política. Expanda o item para exibir todas as permissões que você definiu quando você criou a regra no Active Directory.  
  
9. Clique em **Okey** para retornar ao Windows Explorer.  
  
Na próxima etapa, você garantir acesso está configurado corretamente.  Contas de usuário precisam ter o conjunto de atributo departamento apropriado (defina usando o Centro Administrativo do Active Directory). A maneira mais simples para exibir os resultados da nova diretiva efetivos é usar o **acesso eficaz** guia no Windows Explorer. O **acesso eficaz** guia mostra os direitos de acesso para uma determinada conta de usuário.  
  
#### <a name="to-examine-the-access-for-various-users"></a>Para examinar o acesso para vários usuários  
  
1.  No Gerenciador do Hyper-V, se conecte ao servidor Arquivo1. Faça logon no servidor usando Contoso\administrador. Navegue até D:\ no Windows Explorer. Clique com botão direito do **documentos de finanças** pasta e clique em **propriedades **.  
  
2.  Clique no **segurança**, clique em **avançado**e, em seguida, clique no **acesso eficaz** guia.  
  
3.  Para examinar as permissões para um usuário, clique em **selecionar um usuário**, digite o nome do usuário e, em seguida, clique em **exibir acesso efetivo** para ver os direitos de acesso eficaz. Por exemplo:  
  
    -   Myriam Delesalle (MDelesalle) está no departamento de finanças e deve ter acesso de leitura para a pasta.  
  
    -   Miles Reid (MReid) é um membro do grupo FinanceAdmin e deve ter acesso Modificar à pasta.  
  
    -   Esther Valle (EValle) não está no departamento de finanças; No entanto, ela é um membro do grupo FinanceException e deve ter acesso de leitura.  
  
    -   Maira Wenzel (MWenzel) não é do departamento de finanças e é não é um membro do grupo FinanceAdmin ou FinanceException. Ela não deve ter qualquer acesso à pasta.  
  
    Observe que a última coluna denominada **acesso limitado por** na janela de acesso eficaz. Essa coluna informa quais gates são afeta as permissões da pessoa. Nesse caso, as permissões de compartilhamento e NTFS permitir que todos os usuários controle total. No entanto, a política de acesso central restringe o acesso com base nas regras que você configurou anteriormente.  
  
## <a name="BKMK_1.5"></a>Manter: Alterar e testar a política  
  
||||  
|-|-|-|  
|Número|Etapa|Exemplo|  
|4.1|Configurar as declarações de dispositivo para clientes|Defina a configuração de política de grupo para habilitar as declarações de dispositivo|  
|4.2|Habilite uma declaração para dispositivos.|Habilite o tipo de declaração do país para dispositivos.|  
|4.3|Adicione uma política de preparo para a regra de acesso central existente que você deseja modificar.|Modifique a regra de documentos de finanças para adicionar uma diretiva de preparo.|  
|4.4|Exiba os resultados da política de preparo.|Verifique as permissões do Ester Velle.|  
  
#### <a name="to-set-up-group-policy-setting-to-enable-claims-for-devices"></a>Para configurar a política de grupo configuração Habilitar declarações para dispositivos  
  
1.  Fazer logon em DC1, gerenciamento de política de grupo aberto, clique **contoso.com**, clique em **política de domínio padrão**, clique com botão direito e selecione **editar **.  
  
2.  Na janela do Editor de gerenciamento de política de grupo, navegue até **configuração do computador**, **políticas**, **modelos administrativos**, **sistema**, **Kerberos **.  
  
3.  Selecione **suporte do cliente Kerberos declarações, autenticação composta e proteção Kerberos** e clique em **habilitar **.  
  
#### <a name="to-enable-a-claim-for-devices"></a>Para habilitar uma declaração para dispositivos  
  
1.  Abra DC1 do servidor no Gerenciador do Hyper-V e o log em como Contoso\administrador, com a senha **pass@word1**.  
  
2.  Do **ferramentas** menu, abrir Centro de administrativo do Active Directory.  
  
3.  Clique em **modo de exibição de árvore**, expanda **controle de acesso dinâmico**, clique duas vezes em **tipos de declaração**e clique duas vezes o **país** reivindicar.  
  
4.  Em **requerimentos judiciais ou Extrajudiciais desse tipo podem ser emitidas para as seguintes classes**, selecione o **computador** caixa de seleção. Clique em **Okey**.   
    Ambos **usuário** e **computador** caixas de seleção agora deverá ser selecionadas. A declaração de país agora pode ser usada com dispositivos além dos usuários.  
  
A próxima etapa é criar uma regra de política de preparo. Preparo políticas podem ser usadas para monitorar os efeitos de uma nova entrada de política antes de ativá-lo. Na etapa a seguir, você criará uma entrada preparo de política e monitorar o efeito em sua pasta compartilhada.  
  
#### <a name="to-create-a-staging-policy-rule-and-add-it-to-the-central-access-policy"></a>Criar uma regra de política de preparo e adicioná-la à política de acesso central  
  
1.  Abra DC1 do servidor no Gerenciador do Hyper-V e o log em como Contoso\administrador, com a senha **pass@word1**.  
  
2.  Abra o Centro Administrativo do Active Directory.  
  
3.  Clique em **modo de exibição de árvore**, expanda **controle de acesso dinâmico**e selecione **regras de acesso Central **.  
  
4.  Clique com botão direito **Finanças documentos regra**e clique em **propriedades **.  
  
5.  No **proposta permissões** seção, selecione o **permissão de habilitar preparo configuração** caixa de seleção, clique em **editar**e clique em **adicionar **. No **entrada de permissão para permissões proposta** janela, clique no **selecionar uma entidade de segurança** vincular, digite **usuários autenticados**e clique em **Okey **.  
  
6.  Clique no **adicionar uma condição** vincular e adicione as seguintes condições:   
     [**User**] [**país**] [**Any of**] [**Recurso**] [**País**].  
  
7.  Clique em **adicionar uma condição** novamente e adicione as seguintes condições:  
    [**And**]   
     [**Dispositivo**] [**país**] [**Any of**] [**Recurso**] [**País**]  
  
8.  Clique em **adicionar uma condição** novamente e adicione as seguintes condições.  
    [E]   
     [**User**] [**Group**] [**Membro de qualquer**] [**Valor**] \ (**FinanceException**)  
  
9. Para definir o FinanceException, de grupo, clique em **adicionar itens** no **Selecionar usuário, computador, conta de serviço ou grupo** janela, digite **FinanceException **.  
  
10. Clique em **permissões**, selecione **controle total**e clique em **Okey **.  
  
11. Nas configurações de segurança antecipada para janela proposta permissões, selecione **FinanceException** e clique em **remover **.  
  
12. Clique em **Okey** duas vezes para concluir.  
  
![guias de soluções](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.  
  
```  
Set-ADCentralAccessRule  
-Identity: "CN=FinanceDocumentsRule,CN=CentralAccessRules,CN=ClaimsConfiguration,CN=Configuration,DC=Contoso.com"  
-ProposedAcl: "O:SYG:SYD:AR(A;;FA;;;BA)(A;;FA;;;SY)(A;;0x1301bf;;;S-1-21=1426421603-1057776020-1604)"  
-Server: "WIN-2R92NN8VKFP.Contoso.com"  
  
```  
  
> [!NOTE]  
> No exemplo acima do cmdlet, o valor de servidor reflete o servidor no ambiente de laboratório de teste. Você pode usar o Visualizador de histórico do Windows PowerShell para procurar os cmdlets do Windows PowerShell para cada procedimento que executar no Centro Administrativo do Active Directory. Para obter mais informações, consulte [Visualizador de histórico do Windows PowerShell](https://technet.microsoft.com/library/hh831702)  
  
Neste conjunto de permissões proposta, membros do grupo FinanceException terão acesso completo aos arquivos do seu próprio país quando eles acessá-los por meio de um dispositivo do país mesmo que o documento. Entradas de auditoria estão disponíveis nos servidores de arquivo de log de segurança quando alguém do departamento de finanças tenta acessar os arquivos. No entanto, as configurações de segurança não são impostas até que a política seja promovida de preparo.  
  
No próximo procedimento, verifique se os resultados da política de preparo. Você acessa a pasta compartilhada com um nome de usuário que tenha permissões com base na regra atual. Esther Valle (EValle) é um membro do FinanceException, e ela atualmente tem direitos de leitura. Acordo com nossa política de preparo, EValle não deve ter direitos.  
  
#### <a name="to-verify-the-results-of-the-staging-policy"></a>Para verificar os resultados da política de preparo  
  
1.  Se conectar ao arquivo1 de servidor de arquivo no Gerenciador do Hyper-V e o log em como Contoso\administrador, com a senha **pass@word1**.  
  
2.  Abra uma janela Prompt de comando e digite **gpupdate /force **. Isso garante que as alterações de política de grupo entrarão em vigor em seu servidor.  
  
3.  No Gerenciador do Hyper-V, se conecte ao servidor CLIENT1. Fazer logoff do usuário que está conectado no momento. Reinicie a máquina virtual, CLIENT1. Em seguida, logon no computador usando contoso\EValle pass@word1.  
  
4.  Clique duas vezes o atalho da área de trabalho para \\\FILE1\Finance documentos. EValle ainda devem ter acesso aos arquivos. Alternar de volta para Arquivo1.  
  
5.  Abrir **Visualizador de eventos** de atalho na área de trabalho. Expanda **Logs do Windows**e, em seguida, selecione **segurança **. Abra as entradas com **4818 de ID de evento**sob o **preparo de política de acesso Central** categoria da tarefa. Você verá que EValle teve permissão acesso; No entanto, acordo com a política de preparo, o usuário deve ter acesso negado.  
  
## <a name="next-steps"></a>Próximas etapas  
Se você tiver um sistema de gerenciamento de servidor central como o System Center Operations Manager, você pode também configurar o monitoramento de eventos. Isso permite que os administradores monitorar os efeitos das políticas de acesso central antes de forçá-los.  
  

