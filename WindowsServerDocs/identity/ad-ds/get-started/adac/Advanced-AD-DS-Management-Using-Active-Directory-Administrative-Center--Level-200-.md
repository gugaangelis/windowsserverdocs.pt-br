---
ms.assetid: 4d21d27d-5523-4993-ad4f-fbaa43df7576
title: "Avançadas do AD DS gerenciamento usando o Centro Administrativo do Active Directory (nível 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 19e83ce0123bd484c61d5a4522ebdd90cd40241e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="advanced-ad-ds-management-using-active-directory-administrative-center-level-200"></a>Avançadas do AD DS gerenciamento usando o Centro Administrativo do Active Directory (nível 200)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico aborda o Centro Administrativo do diretório Active atualizada com sua nova a Lixeira do Active Directory, política de senha refinadas e Visualizador de histórico do Windows PowerShell com mais detalhes, inclusive a arquitetura, exemplos para tarefas comuns e informações de solução de problemas. Para obter uma introdução, consulte [Introdução ao Active Directory administrativas centro aprimoramentos & #40; Nível de 100 & #41; ](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md).  
  
-   [Arquitetura de centro administrativo do Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Arch)  
  
-   [Habilitando e gerenciando o Active Directory Lixeira usando o Centro Administrativo do Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_EnableRecycleBin)  
  
-   [Configurar e gerenciar políticas de senha refinadas usando o Centro Administrativo do Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_FGPP)  
  
-   [Usando o Visualizador de histórico do PowerShell do Windows de centro administrativo do Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_HistoryViewer)  
  
-   [Solucionando problemas de gerenciamento do AD DS](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Tshoot)  
  
## <a name="BKMK_Arch"></a>Arquitetura de centro administrativo do Active Directory  
  
### <a name="adprep-executables-dlls"></a>ADPrep arquivos executáveis, DLLs  
O módulo e a arquitetura subjacente do Centro Administrativo do Active Directory não mudou com o novo Lixeira, FGPP e recursos de Visualizador de histórico.  
  
-   Microsoft.ActiveDirectory.Management.UI.dll  
  
-   Microsoft.ActiveDirectory.Management.UI.resources.dll  
  
-   Microsoft.ActiveDirectory.Management.dll  
  
-   Microsoft.ActiveDirectory.Management.resources.dll  
  
-   ActiveDirectoryPowerShellResources.dll  
  
O Windows PowerShell subjacente e a camada de operações para a nova funcionalidade da Lixeira são ilustradas abaixo:  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/adds_adrestore.png)  
  
## <a name="BKMK_EnableRecycleBin"></a>Habilitando e gerenciando o Active Directory Lixeira usando o Centro Administrativo do Active Directory  
  
### <a name="capabilities"></a>Recursos  
  
-   O Windows Server 2012 Active Directory centro administrativo permite que você a configurar e gerenciar o Active Directory Lixeira para qualquer partição de domínio em uma floresta. Não é um requisito para usar o Windows PowerShell ou Ldp.exe para habilitar o Active Directory Lixeira ou restaurar objetos em partições de domínio.  
  
-   O Centro Administrativo do Active Directory avançou critérios de filtragem, facilitando a restauração direcionada em ambientes de grandes com muitos objetos intencionalmente excluídos.  
  
### <a name="limitations"></a>Limitações  
  
-   Porque o Centro Administrativo do Active Directory somente pode gerenciar partições de domínio, ele não poderá restaurar objetos excluídos das partições de configuração, DNS do domínio ou floresta DNS (você não pode excluir objetos da partição de esquema). Para restaurar os objetos de partições não do domínio, use [restauração ADObject](https://technet.microsoft.com/library/ee617262.aspx).  
  
-   O Centro Administrativo do Active Directory não é possível restaurar subárvores de objetos em uma única ação. Por exemplo, se você excluir uma UO com aninhados UOs, os usuários, grupos e computadores, restaurando a UO base não restaurar os objetos filho.  
  
    > [!NOTE]  
    > A operação de restauração do lote de centro administrativo do Active Directory faz um "melhor esforço" tipo dos objetos excluídos *dentro da seleção somente* para que os pais são ordenados antes das crianças para a lista de restauração. Em casos de teste simples, subárvores de objetos podem ser restaurados em uma única ação. Mas casos de canto, como uma seleção que contenha árvores parciais - árvores com alguns de nós pai excluído ausentes - ou casos de erro, como ignorar os objetos filho quando restaurar pai falhar, talvez não funcionem conforme o esperado. Por esse motivo, você sempre deve restaurar subárvores de objetos como uma ação separada depois de restaurar os objetos pai.  
  
A Lixeira do Active Directory requer um Windows Server 2008 R2 nível funcional da floresta, e você deve ser um membro do grupo Administradores corporativos. Uma vez habilitado, você não pode desabilitar a Lixeira do Active Directory. A Lixeira do Active Directory aumenta o tamanho do banco de dados do Active Directory (NTDS. DIT) em todos os controladores de domínio na floresta. Espaço em disco usado pela Lixeira continua aumentando ao longo do tempo à medida que ele preserva objetos e todos os seus dados de atributo.  
  
Para obter mais informações, consulte [Active Directory reciclar Bin guia passo a passo](https://technet.microsoft.com/library/dd392261(v=WS.10).aspx) e [avaliar os requisitos de Hardware (do Active Directory Lixeira)](https://technet.microsoft.com/library/cc753439(WS.10).aspx).  
  
Você pode *inferior* as floresta e domínio níveis funcionais do Windows Server 2012 para Windows Server 2008 R2, mesmo depois de habilitar o Active Directory Lixeira. Isso requer o uso do **conjunto ADForestMode** e **conjunto ADDomainMode** cmdlets do Active Directory.  
  
Por exemplo:  
  
```  
Set-AdForestMode -identity corp.contoso.com -server dc1.corp.contoso.com -forestmode Windows2008R2Forest  
Set-AdDomainMode -identity research.corp.contoso.com -server dc3.research.corp.contoso.com -domainmode Windows2008R2Domain  
  
```  
  
Você não pode usar o Centro Administrativo do Active Directory para fazer essa alteração - ela só gera níveis funcionais.  
  
### <a name="enabling-active-directory-recycle-bin-using-active-directory-administrative-center"></a>Habilitando o Active Directory Lixeira usando o Centro Administrativo do Active Directory  
Para ativar o Active Directory Lixeira, abra o **Centro Administrativo do Active Directory** e clique no nome da floresta no painel de navegação. Do **tarefas** painel, clique em **habilitar Lixeira**.  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBin.png)  
  
O Centro Administrativo do Active Directory mostra o **habilitar confirmação de Bin reciclar** caixa de diálogo. Essa caixa de diálogo avisa que é irreversível habilitando a Lixeira. Clique em **Okey** para permitir que o Active Directory Lixeira. O Centro Administrativo do Active Directory mostra outra caixa de diálogo para lembrá-lo que o Active Directory Lixeira não é totalmente funcional até que todos os controladores de domínio duplicam a alteração de configuração.  
  
> [!IMPORTANT]  
> A opção para habilitar o Active Directory Lixeira estará disponível se:  
>   
> -   O nível funcional da floresta for menor que o Windows Server 2008 R2  
> -   Ele já está habilitado  
  
Ainda é o equivalente cmdlet do Active Directory Windows PowerShell:  
  
```  
Enable-ADOptionalFeature  
```  
  
Para obter mais informações sobre como usar o Windows PowerShell para permitir que o Active Directory Lixeira, consulte o [Active Directory reciclar Bin guia passo a passo](https://technet.microsoft.com/library/dd392261(v=WS.10).aspx).  
  
### <a name="managing-active-directory-recycle-bin-using-active-directory-administrative-center"></a>Gerenciando o Active Directory Lixeira usando o Centro Administrativo do Active Directory  
Esta seção usa o exemplo de um domínio existente chamado **corp.contoso.com**. Neste domínio organiza os usuários em um pai OU chamada **UserAccounts**. O **UserAccounts** OU contém três OUs filho nomeadas pelo departamento, que cada contêm ainda mais unidades organizacionais, usuários e grupos.  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBinExampleOU.png)  
  
#### <a name="storage-and-filtering"></a>Armazenamento e filtragem  
O Active Directory Lixeira preserva todos os objetos excluídos na floresta. Ele salva esses objetos de acordo com o **msDS-deletedObjectLifetime** atributo, que, por padrão, é definido para corresponder a **vida útil para desativação** atributo da floresta. Em qualquer floresta criada com o Windows Server 2003 SP1 ou posterior, o valor de **vida útil para desativação** é definido como 180 dias por padrão. Em qualquer floresta atualizado do Windows 2000 ou instalado com o Windows Server 2003 (sem service pack), o atributo de vida útil para desativação padrão não é definido e, portanto, o Windows usa o padrão interno de 60 dias. Tudo isso é configurável. Você pode usar o Centro Administrativo do Active Directory para restaurar todos os objetos excluídos das partições de domínio da floresta. Você deve continuar a usar o cmdlet **restauração ADObject** restaurar objetos excluídos de outras partições, como Configuration.Enabling faz com que o Active Directory Lixeira o **Deleted Objects** contêiner visível em todas as partições de domínio em que o Centro Administrativo do Active Directory.  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_DeletedObjectsContainer.png)  
  
O **Deleted Objects** contêiner mostra todos os objetos serão recuperáveis nessa partição domínio. Excluído objetos mais antigos que **msDS-deletedObjectLifetime** são conhecidos como objetos reciclados. O Centro Administrativo do Active Directory não mostra os objetos reciclados e você não poderá restaurar esses objetos usando o Centro Administrativo do Active Directory.  
  
Para obter uma explicação mais profunda de arquitetura a Lixeira e regras de processamento, consulte [a Lixeira AD: Noções básicas sobre, implementação, práticas recomendadas e solução de problemas](http://blogs.technet.com/b/askds/archive/2009/08/27/the-ad-recycle-bin-understanding-implementing-best-practices-and-troubleshooting.aspx).  
  
O Centro Administrativo do Active Directory artificialmente limita o número padrão de objetos retornados de um contêiner de 20.000 objetos. Você pode disparar esse limite tão alto quanto 100.000 objetos clicando o **gerenciar** menu, em seguida, **opções de gerenciamento de lista**.  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_MgmtList.png)  
  
#### <a name="restoration"></a>Restauração  
  
##### <a name="filtering"></a>Filtragem  
Centro Administrativo do Active Directory oferece critérios potentes e opções de filtragem que você deve se familiarizar com antes que você precise usá-los em uma restauração vida real. Domínios intencionalmente excluir muitos objetos através de seu tempo de vida. Com um tempo de vida do objeto provavelmente excluído de 180 dias, você simplesmente não poderá restaurar todos os objetos quando ocorre um acidente.  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_AddCriteria.png)  
  
Em vez de escrever filtros LDAP complexos e converter valores UTC em datas e horas, use o básico e avançado **filtro** menu para listar apenas os objetos relevantes. Se você souber o dia de exclusão, os nomes dos objetos ou quaisquer outros dados da chave, use isso para sua vantagem quando a filtragem. Mude as opções de filtro avançado, clique na divisa à direita da caixa de pesquisa.  
  
A operação de restauração dá suporte a todas as opções de critérios filtro padrão, o mesmo como qualquer outra pesquisa. Os filtros internos, os importantes para restaurar objetos normalmente são:  
  
-   *ANR (resolução de nomes ambíguos - não listada no menu, mas o que é usado quando você digitar o * * * filtro * * * caixa)*  
  
-   Última modificada entre fornecido datas  
  
-   Objeto é a unidade do usuário/inetorgperson/computador/grupo/organização  
  
-   Nome  
  
-   Quando excluído  
  
-   Último pai conhecido  
  
-   Tipo  
  
-   Descrição  
  
-   Cidade  
  
-   País /region  
  
-   Departamento  
  
-   ID do funcionário  
  
-   Sobrenome  
  
-   Cargo  
  
-   Sobrenome  
  
-   SAMaccountname  
  
-   Estado/província  
  
-   Número de telefone  
  
-   UPN  
  
-   CEP  
  
Você pode adicionar vários critérios. Por exemplo, você pode encontrar todos os objetos de usuário excluídos em 24 de setembro<sup>th</sup> 2012 pela Chicago, Illinois com um título de trabalho do Gerenciador.  
  
Você também pode adicionar, modificar ou reordenar os cabeçalhos de coluna para fornecer mais detalhes ao avaliar quais objetos para recuperar.  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ColumnHeaders.png)  
  
Para obter mais informações sobre a resolução de nomes ambíguos, consulte [atributos ANR](https://msdn.microsoft.com/library/ms675092(VS.85).aspx).  
  
##### <a name="single-object"></a>Objeto único  
Restauração de objetos excluídos sempre tem sido uma única operação.  O Centro Administrativo do Active Directory facilita a operação. Para restaurar um objeto excluído, como um único usuário:  
  
1.  Clique no nome de domínio no painel de navegação do Centro Administrativo do Active Directory.  
  
2.  Clique duas vezes em **Deleted Objects** na lista de gerenciamento.  
  
3.  Clique com botão direito do objeto e, em seguida, clique em **restaurar**, ou clique em **restaurar** do **tarefas** painel.  
  
Restaura o objeto para seu local original.  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreSingle.gif)  
  
Clique em **restaurar para... ** para alterar o local de restauração. Isso é útil se o contêiner do pai do objeto excluído também foi excluída, mas você não quer restaurar o pai.  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreToSingle.gif)  
  
##### <a name="multiple-peer-objects"></a>Vários objetos de par  
Você pode restaurar vários objetos de nível de ponto a ponto, como todos os usuários em uma unidade Organizacional. Mantenha pressionada a tecla CTRL e clique em um ou mais objetos excluídos, que você quer restaurar. Clique em **restaurar** do painel de tarefas. Você também pode selecionar objetos exibidos todos pressionando a teclas CTRL e um, ou um intervalo de objetos usando SHIFT e clicar.  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestorePeers.png)  
  
##### <a name="multiple-parent-and-child-objects"></a>Vários objetos filho e pai  
É fundamental para entender o processo de restauração para uma restauração vários parent filho, porque o Centro Administrativo do Active Directory não é possível restaurar uma árvore de objetos excluídos com uma única ação aninhada.  
  
1.  Restaure o objeto excluído mais alto em uma árvore.  
  
2.  Restaure os filhos imediatos do objeto pai.  
  
3.  Restaure os filhos imediatos desses objetos pai.  
  
4.  Repita conforme necessário até que todos os objetos restaurar.  
  
Você não poderá restaurar um objeto filho antes de restaurar seu pai. A restauração a tentativa retorna o seguinte erro:  
  
**A operação não pôde ser executada porque o pai do objeto é ocorrência ou excluído.**  
  
O **último pai conhecido** atributo mostra a relação de pai de cada objeto. O **último pai conhecido** atributo mudanças do local excluído do local restaurado quando você atualiza o Centro Administrativo do Active Directory depois de restaurar um pai. Portanto, você poderá restaurar esse objeto filho ao local do objeto de um pai não mostra o nome do contêiner de objetos excluídos distinto.  
  
Considere o cenário em que um administrador acidentalmente exclui a UO de vendas, que contém a UOs de filho e os usuários.  
  
Primeiro, observe o valor da **último pai conhecido** atributo para todos os usuários excluídos e como ele lê * *UO = Sales\0ADEL:*< guid + contêiner de objetos excluídos diferenciadas nome >** *:  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParent.gif)  
  
Filtrar o nome ambíguos vendas para retornar a OU excluída, em seguida, restaurar:  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSales.png)  
  
Atualize o Centro Administrativo do Active Directory para ver o atributo de último conhecido pai do objeto de usuário excluída mudar para o nome diferenciado de vendas OU restaurado:  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesRestored.gif)  
  
Filtrar todos os usuários de vendas. Pressione a teclas CTRL e um para selecionar todos os usuários de vendas excluídos. Clique em **restaurar** para mover os objetos do **Deleted Objects** contêiner à UO de vendas com seus membros do grupo e atributos intactos.  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesUndelete.png)  
  
Se o **vendas** UO contidos UOs filho de sua própria e deseja restaurar as UOs filho primeiro antes de restaurar seus filhos e assim por diante.  
  
Para restaurar aninhados todos os objetos excluídos, especificando um contêiner pai excluídos, consulte [apêndice b: restaurar vários, excluídos objetos do Active Directory (Script de exemplo)](https://technet.microsoft.com/library/dd379504(WS.10).aspx).  
  
O cmdlet do Active Directory do Windows PowerShell para restaurar objetos excluídos é:  
  
```  
Restore-adobject  
  
```  
  
O **restauração ADObject** funcionalidade cmdlet não foram alterados entre o Windows Server 2008 R2 e Windows Server 2012.  
  
##### <a name="server-side-filtering"></a>Filtragem de servidor  
É possível que, ao longo do tempo, o contêiner de objetos excluídos será acumular mais de 20.000 (ou até mesmo 100.000) objetos em empresas de médio e grandes e ter dificuldade para mostrar todos os objetos. Desde que o mecanismo de filtro no Centro Administrativo do Active Directory depende filtrando do lado do cliente, ele não pode mostrar esses objetos adicionais. Para contornar essa limitação, use as seguintes etapas para realizar uma pesquisa de servidor:  
  
1.  Clique com botão direito do **Deleted Objects** contêiner e clique em **pesquisa sob esse nó**.  
  
2.  Clique na divisa para expor o **+ adicionar critérios de** menu, selecione e adicione **última modificação entre fornecido datas**. A hora da última modificação (o **whenChanged** atributo) é uma aproximação fechar o tempo de exclusão; Na maioria dos ambientes, eles são idênticos. Essa consulta executa uma pesquisa de servidor.  
  
3.  Localize os objetos excluídos restaurar usando outra exibição filtragem, classificação e assim por diante nos resultados e restaurá-los normalmente.  
  
## <a name="BKMK_FGPP"></a>Configurar e gerenciar políticas de senha refinadas usando o Centro Administrativo do Active Directory  
  
### <a name="configuring-fine-grained-password-policies"></a>Configurar políticas de senha refinadas  
O Centro Administrativo do Active Directory permite que você criar e gerenciar objetos de política de senha refinadas (FGPP). Windows Server 2008 introduziu o recurso FGPP mas Windows Server 2012 tem a primeira interface gráfica de gerenciamento para ele. Aplique políticas de senha refinadas em um nível de domínio e permite substituir a senha de domínio único exigida pelo Windows Server 2003. Criando FGPP diferente com configurações diferentes, usuários individuais ou grupos obtém diferentes políticas de senha em um domínio.  
  
Para obter informações sobre a política de senha refinadas, consulte [AD DS refinadas senha e política de bloqueio de conta Step-by-Step guia (Windows Server 2008 R2)](https://technet.microsoft.com/library/cc770842(WS.10).aspx).  
  
No painel de navegação, clique em modo de exibição de árvore, clique em seu domínio **sistema**, clique em **contêiner de configurações de senha**e, em seguida, no painel de tarefas, clique em **nova** e **configurações de senha**.  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_PasswordSettings.png)  
  
### <a name="managing-fine-grained-password-policies"></a>Gerenciamento de políticas de senha refinadas  
Criando um novo FGPP ou editando uma existente abre o **configurações de senha** editor. A partir daqui, você configurar todas as políticas de senha desejada, como você teria no Windows Server 2008 ou Windows Server 2008 R2, só agora com um editor de uso específico.  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_CreatePasswordSettings.png)  
  
Preencher os campos necessários do (asterisco vermelho) e qualquer opcionais e clique em **adicionar** para definir os usuários ou grupos que recebe essa política. FGPP substitui as configurações de política de domínio padrão para essas entidades de segurança especificado. Na figura acima, uma política extremamente restritiva se aplica somente a conta de administrador interno, para evitar o comprometimento. A política é muito complexa para usuários padrão em conformidade com, mas é perfeita para uma conta de alto risco usada somente por profissionais de TI.  
  
Você também definir precedência e para quais usuários e grupos a política se aplica dentro de um determinado domínio.  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_Precedence.png)  
  
Os cmdlets do Active Directory do Windows PowerShell para política de senha refinadas são:  
  
```  
Add-ADFineGrainedPasswordPolicySubject  
Get-ADFineGrainedPasswordPolicy  
Get-ADFineGrainedPasswordPolicySubject  
New-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicySubject  
Set-ADFineGrainedPasswordPolicy  
  
```  
  
A funcionalidade do cmdlet de política de senha refinada não foram alterados entre o Windows Server 2008 R2 e Windows Server 2012. Como uma conveniência, o diagrama a seguir ilustra os argumentos associados para cmdlets:  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPP.gif)  
  
O Centro Administrativo do Active Directory também permite que você encontrar o conjunto de FGPP aplicada para um determinado usuário resultante. Qualquer usuário clique com botão direito e clique em **exibir configurações de senha resultante... ** para abrir o *configurações de senha* página que se aplica ao usuário por meio de atribuição implícito ou explícito:  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RSOP.png)  
  
Examinando a **propriedades** de qualquer usuário ou grupo mostra o **associados diretamente configurações de senha**, quais são as FGPPs explicitamente atribuídos:  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPPSettings.gif)  
  
Atribuição de FGPP implícita não for exibida aqui; Para isso, você deve usar o **exibir configurações de senha resultante... ** opção.  
  
## <a name="BKMK_HistoryViewer"></a>Usando o Visualizador de histórico do PowerShell do Windows de centro administrativo do Active Directory  
O futuro do gerenciamento do Windows é o Windows PowerShell. Por ferramentas gráficas de divisão em camadas na parte superior de uma estrutura de automação da tarefa, o gerenciamento dos mais complexos sistemas distribuídos torna-se consistente e eficiente. Você precisa compreender o funcionamento do Windows PowerShell para acessar todo o seu potencial e maximizar seus investimentos de computação.  
  
Agora, o Centro Administrativo do Active Directory fornece um histórico completo de todos os cmdlets do Windows PowerShell que ele é executado e seus valores e argumentos. Você pode copiar o histórico do cmdlet em outro lugar para estudo ou modificação e reutilização. Você pode criar anotações de tarefa para ajudar a isolar quais seu centro de administrativo do Active Directory comandos resultaram em Windows PowerShell. Você também pode filtrar o histórico para encontrar pontos de interesse.  
  
Objetivo do Active Directory administrativas centro Visualizador do Windows PowerShell histórico é para você aprender com a experiência prática.  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_HistoryViewer.gif)  
  
Clique na divisa (seta) para mostrar o Visualizador de histórico do Windows PowerShell.  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RaiseViewer.png)  
  
Em seguida, crie um usuário ou modificar os membros do grupo. O Visualizador de histórico continuamente atualiza com um modo de exibição recolhido de cada cmdlet que executou o Centro Administrativo do Active Directory com os argumentos especificados.  
  
Expanda qualquer item de linha de interesse para ver todos os valores fornecidos para os argumentos do cmdlet:  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ViewArgs.png)  
  
Clique no **Iniciar tarefa** menu para criar uma notação manual antes de usar o Centro Administrativo do Active Directory para criar, modificar ou excluir um objeto. Digite o que estava fazendo.  Quando terminar com a alteração, selecione **Finalizar tarefa**. Os grupos de observação de tarefa todas essas ações executadas em uma anotação recolhível, que você pode usar para uma melhor compreensão.  
  
Por exemplo, para ver os comandos do Windows PowerShell usados para alterar a senha do usuário e removê-lo de um grupo:  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RemoveUser.gif)  
  
Marcar a caixa de seleção Mostrar tudo também mostra o Get-* verbo cmdlets do Windows PowerShell que recuperar somente os dados.  
  
![Gerenciamento do avançadas do AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ShowAll.png)  
  
O Visualizador de histórico mostra o literal comandos são executados pelo Centro Administrativo do Active Directory e você pode observar que alguns cmdlets parecer que a execução desnecessariamente. Por exemplo, você pode criar um novo usuário com:  
  
```  
new-aduser   
```  
  
e não precisa usar:  
  
```  
set-adaccountpassword  
enable-adaccount  
set-aduser  
  
```  
  
Design do centro do Active Directory administrativa necessária modularidade e uso de um mínimo de código. Portanto, em vez de um conjunto de funções que criar novos usuários e outro conjunto que modificam usuários existentes, ele faz minimamente cada função e, em seguida, é aprovado-las junto com os cmdlets. Tenha isso em mente quando você estiver aprendendo o Active Directory do Windows PowerShell. Você também pode usar que como uma técnica de aprendizagem, onde você ver simplesmente como você pode usar o Windows PowerShell para concluir uma única tarefa.  
  
## <a name="BKMK_Tshoot"></a>Solucionando problemas de gerenciamento do AD DS  
  
### <a name="introduction-to-troubleshooting"></a>Introdução à solução de problemas  
Devido à sua renovação relativa e ausência de uso em ambientes de clientes existentes, o Centro Administrativo do Active Directory tem limitado opções de solução de problemas.  
  
### <a name="troubleshooting-options"></a>Opções de solução de problemas  
  
#### <a name="logging-options"></a>Opções de registro em log  
O Centro Administrativo do Active Directory agora contém o registro em log integrados no Windows Server 2012, como parte de um arquivo de configuração de rastreamento. Criar/modificar o arquivo na mesma pasta como dsac.exe a seguir:  
  
**Dsac.exe.config**  
  
Crie o seguinte conteúdo:  
  
```  
<appSettings>  
  <add key="DsacLogLevel" value="Verbose" />  
</appSettings>  
<system.diagnostics>   
 <trace autoflush="false" indentsize="4">   
  <listeners>   
   <add name="myListener"   
    type="System.Diagnostics.TextWriterTraceListener"   
    initializeData="dsac.trace.log" />   
   <remove name="Default" />   
  </listeners>   
 </trace>   
</system.diagnostics>  
  
```  
  
O detalhamento níveis para **DsacLogLevel** são **None**, **erro**, **aviso**, **informações**, e **Verbose**. O nome do arquivo de saída é configurável e grava na mesma pasta que dsac.exe. A saída saberá mais sobre como ADAC está funcionando, quais controladores de domínio-contatado, quais comandos do Windows PowerShell executados, as respostas e ainda mais detalhes.  
  
Por exemplo, ao usar o nível de informações, que retorna todos os resultados, exceto o detalhamento do nível de rastreamento:  
  
-   DSAC.exe é iniciado  
  
-   Log inicia  
  
-   Controlador de domínio solicitado para retornar informações de domínio inicial  
  
    ```  
    [12:42:49][TID 3][Info] Command Id, Action, Command, Time, Elapsed Time ms (output), Number objects (output)  
    [12:42:49][TID 3][Info] 1, Invoke, Get-ADDomainController, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADDomainController-Discover:$null-DomainName:"CORP"-ForceDiscover:$null-Service:ADWS-Writable:$null  
    ```  
  
-   Controlador de domínio DC1 retornado do domínio Corp  
  
-   PS Unidade virtual AD carregado  
  
    ```  
    [12:42:49][TID 3][Info] 1, Output, Get-ADDomainController, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] Found the domain controller 'DC1' in the domain 'CORP'.  
    [12:42:49][TID 3][Info] 2, Invoke, New-PSDrive, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] New-PSDrive-Name:"ADDrive0"-PSProvider:"ActiveDirectory"-Root:""-Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 2, Output, New-PSDrive, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 3, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
  
    ```  
  
-   Obtenha o domínio raiz DSE informações  
  
    ```  
    [12:42:49][TID 3][Info] Get-ADRootDSE  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 3, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 4, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49  
  
    ```  
  
-   Obtenha informações do domínio AD reciclagem bin  
  
    ```  
    [12:42:49][TID 3][Info] Get-ADOptionalFeature  
    -LDAPFilter:"(msDS-OptionalFeatureFlags=1)"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 4, Output, Get-ADOptionalFeature, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 5, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADRootDSE  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 5, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 6, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADRootDSE  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 6, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 7, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADOptionalFeature  
    -LDAPFilter:"(msDS-OptionalFeatureFlags=1)"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:50][TID 3][Info] 7, Output, Get-ADOptionalFeature, 2012-04-16T12:42:50, 1  
    [12:42:50][TID 3][Info] 8, Invoke, Get-ADForest, 2012-04-16T12:42:50  
  
    ```  
  
-   Obter floresta do AD  
  
    ```  
    [12:42:50][TID 3][Info] Get-ADForest  
    -Identity:"corp.contoso.com"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:50][TID 3][Info] 8, Output, Get-ADForest, 2012-04-16T12:42:50, 1  
    [12:42:50][TID 3][Info] 9, Invoke, Get-ADObject, 2012-04-16T12:42:50  
  
    ```  
  
-   Obtenha informações sobre o esquema para tipos de criptografia permitidos, FGPP, determinadas informações do usuário  
  
    ```  
    [12:42:50][TID 3][Info] Get-ADObject  
    -LDAPFilter:"(|(ldapdisplayname=msDS-PhoneticDisplayName)(ldapdisplayname=msDS-PhoneticCompanyName)(ldapdisplayname=msDS-PhoneticDepartment)(ldapdisplayname=msDS-PhoneticFirstName)(ldapdisplayname=msDS-PhoneticLastName)(ldapdisplayname=msDS-SupportedEncryptionTypes)(ldapdisplayname=msDS-PasswordSettingsPrecedence))"  
    -Properties:lDAPDisplayName  
    -ResultPageSize:"100"  
    -ResultSetSize:$null  
    -SearchBase:"CN=Schema,CN=Configuration,DC=corp,DC=contoso,DC=com"  
    -SearchScope:"OneLevel"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:50][TID 3][Info] 9, Output, Get-ADObject, 2012-04-16T12:42:50, 7  
    [12:42:50][TID 3][Info] 10, Invoke, Get-ADObject, 2012-04-16T12:42:50  
  
    ```  
  
-   Obter todas as informações sobre o objeto de domínio para exibir para o administrador que clicou no head do domínio.  
  
    ```  
    [12:42:50][TID 3][Info] Get-ADObject  
    -IncludeDeletedObjects:$false  
    -LDAPFilter:"(objectClass=*)"  
    -Properties:allowedChildClassesEffective,allowedChildClasses,lastKnownParent,sAMAccountType,systemFlags,userAccountControl,displayName,description,whenChanged,location,managedBy,memberOf,primaryGroupID,objectSid,msDS-User-Account-Control-Computed,sAMAccountName,lastLogonTimestamp,lastLogoff,mail,accountExpires,msDS-PhoneticCompanyName,msDS-PhoneticDepartment,msDS-PhoneticDisplayName,msDS-PhoneticFirstName,msDS-PhoneticLastName,pwdLastSet,operatingSystem,operatingSystemServicePack,operatingSystemVersion,telephoneNumber,physicalDeliveryOfficeName,department,company,manager,dNSHostName,groupType,c,l,employeeID,givenName,sn,title,st,postalCode,managedBy,userPrincipalName,isDeleted,msDS-PasswordSettingsPrecedence  
    -ResultPageSize:"100"  
    -ResultSetSize:"20201"  
    -SearchBase:"DC=corp,DC=contoso,DC=com"  
    -SearchScope:"Base"  
    -Server:"dc1.corp.contoso.com"  
  
    ```  
  
Definindo o Verbose nível também mostra as pilhas de .NET para cada função, mas isso não inclui dados suficientes para ser particularmente útil, exceto quando o Dsac.exe sofrer uma violação de acesso ou a falha de solução de problemas.  
  
> [!IMPORTANT]  
> Também há uma versão fora de banda do serviço chamado o [Active Directory Management Gateway](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=2852), que é executado no Windows Server 2008 SP2 e Windows Server 2003 SP2.  
  
As duas causas prováveis desse problema são:  
  
-   O serviço ADWS não está executando em controladores de domínio acessíveis.  
  
-   Comunicações de rede são bloqueadas para o serviço ADWS no computador que executa o Centro Administrativo do Active Directory  
  
Os erros mostrados quando não há instâncias de serviços do Active Directory da Web estão disponíveis são:  
  
|||  
|-|-|  
|Erro|Operação|  
|"Não consegue se conectar em qualquer domínio. Atualizar ou tente novamente quando a conexão está disponível"|Mostrado na tela inicial do aplicativo Centro Administrativo do Active Directory|  
|"Não é possível localizar um servidor disponível no * <NetBIOS domain name> * domínio que esteja executando o Active Directory Web serviço ADWS ()"|Mostrado quando tentando selecionar um nó de domínio no aplicativo Centro Administrativo do Active Directory|  
  
Para solucionar esse problema, use estas etapas:  
  
1.  Valide a serviços Web do Active Directory serviço é iniciado pelo menos um controlador de domínio no domínio (e preferencialmente todos os controladores de domínio na floresta). Certifique-se de que está definido para ser iniciado automaticamente em todos os controladores de domínio.  
  
2.  No computador que executa o Centro Administrativo do Active Directory, valide que você pode localizar um servidor que executa ADWS executando estes comandos NLTest.exe:  
  
    ```  
    nltest /dsgetdc:<domain NetBIOS name> /ws /force   
    nltest /dsgetdc:<domain fully qualified DNS name> /ws /force  
  
    ```  
  
    Se esses testes falham mesmo que o serviço ADWS está em execução, o problema está com resolução de nomes LDAP e não ADWS ou centro administrativo do Active Directory. Esse teste falhar com o erro "1355 0x54B ERROR_NO_SUCH_DOMAIN" se ADWS não é executado em controladores de domínio porém, então verifique novamente antes de atingir qualquer conclusões.  
  
3.  No controlador de domínio retornado por NLTest despeje na lista porta escuta com o comando:  
  
    ```  
    Netstat -anob > ports.txt  
  
    ```  
  
    Examine o arquivo ports.txt e validar que o serviço ADWS está ouvindo na porta 9389. Exemplo:  
  
    ```  
    TCP    0.0.0.0:9389    0.0.0.0:0    LISTENING    1828  
    [Microsoft.ActiveDirectory.WebServices.exe]  
  
    TCP    [::]:9389       [::]:0       LISTENING    1828  
    [Microsoft.ActiveDirectory.WebServices.exe]  
  
    ```  
  
    Se ouvindo, validar as regras do Firewall do Windows e certifique-se de que eles permitem 9389 TCP entrada. Por padrão, controladores de domínio habilitam a regra de firewall "Active Directory serviços Web (TCP-in)". Se não estiver ouvindo, validar novamente se o serviço está em execução nesse servidor e reiniciá-lo. Valide que nenhum outro processo já está ouvindo na porta 9389.  
  
4.  Instale NetMon ou outro utilitário de captura de rede no computador que executa o Centro Administrativo do Active Directory e no controlador de domínio retornado por NLTEST. Colete captura simultânea rede nos dois computadores, onde você inicia o Centro Administrativo do Active Directory e veja o erro antes de parar a captura. Confirme se o cliente é capaz de enviar e receber do controlador de domínio na porta TCP 9389. Se os pacotes são enviados, mas nunca chegarão ou chegarem e as respostas de controlador de domínio, mas eles nunca alcançar o cliente, é provavelmente há um firewall entre os computadores na rede removendo pacotes nessa porta. Esse firewall pode ser hardware ou software e pode fazer parte do software (antivírus) de proteção de ponto de extremidade de terceiros.  
  
#### <a name="knownlikely-issues-and-support-scenarios"></a>Problemas conhecidos/provavelmente e cenários de suporte  
O problema provavelmente somente com o Centro Administrativo do Active Directory é a incapacidade de conectar-se para o Active Directory Web serviço ADWS () em execução em um controlador de domínio do Windows Server 2012 ou Windows Server 2008 R2.  
  
## <a name="see-also"></a>Consulte também  
[Introdução aos aprimoramentos de centro administrativo do Active Directory & #40; Nível de 100 & #41;](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md)  
  

