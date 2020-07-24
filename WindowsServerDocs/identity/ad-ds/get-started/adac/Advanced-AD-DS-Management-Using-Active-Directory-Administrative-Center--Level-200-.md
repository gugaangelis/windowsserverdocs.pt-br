---
ms.assetid: 4d21d27d-5523-4993-ad4f-fbaa43df7576
title: Advanced AD DS Management Using Active Directory Administrative Center (Level 200)
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 63764ff94c10376c76eec493c618214a183bf528
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86961098"
---
# <a name="advanced-ad-ds-management-using-active-directory-administrative-center-level-200"></a>Advanced AD DS Management Using Active Directory Administrative Center (Level 200)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico abrange o Centro Administrativo do Active Directory atualizado, com a nova lixeira do Active Directory, política de Senha Refinada e Visualizador de Histórico do Windows PowerShell com mais detalhes, incluindo arquitetura, exemplos de tarefas comuns e informações sobre solução de problemas. Para obter uma introdução, consulte [introdução a centro administrativo do Active Directory aprimoramentos &#40;nível 100&#41;](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md).  
  
- [Arquitetura do Centro Administrativo do Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Arch)  
- [Habilitando e gerenciando a lixeira do Active Directory usando o Centro Administrativo do Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_EnableRecycleBin)  
- [Configurando e gerenciando políticas de senha refinada usando o Centro Administrativo do Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_FGPP)  
- [Usando o visualizador de histórico do Windows PowerShell no Centro Administrativo do Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_HistoryViewer)  
- [Solucionando problemas de gerenciamento AD DS](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Tshoot)  
  
## <a name="active-directory-administrative-center-architecture"></a><a name="BKMK_Arch"></a>Arquitetura do Centro Administrativo do Active Directory  
  
### <a name="active-directory-administrative-center-executables-dlls"></a>Centro Administrativo do Active Directory executáveis, DLLs  

O módulo e a arquitetura subjacentes do Centro Administrativo do Active Directory não mudaram com os recursos da nova lixeira, FGPP e visualizador de histórico.  
  
- Microsoft.ActiveDirectory.Management.UI.dll  
- Microsoft.ActiveDirectory.Management.UI.resources.dll  
- Microsoft.ActiveDirectory.Management.dll  
- Microsoft.ActiveDirectory.Management.resources.dll  
- ActiveDirectoryPowerShellResources.dll  
  
O Windows PowerShell e camada de operações subjacentes para a nova funcionalidade Lixeira são ilustrados abaixo:  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/adds_adrestore.png)  
  
## <a name="enabling-and-managing-the-active-directory-recycle-bin-using-active-directory-administrative-center"></a><a name="BKMK_EnableRecycleBin"></a>Habilitando e gerenciando a lixeira do Active Directory usando o Centro Administrativo do Active Directory  
  
### <a name="capabilities"></a>Funcionalidades  
  
- O Windows Server 2012 ou mais recente Centro Administrativo do Active Directory permite que você configure e gerencie o Active Directory Lixeira para qualquer partição de domínio em uma floresta. Não há mais um requisito para usar o Windows PowerShell ou Ldp.exe para habilitar a Lixeira do Active Directory ou restaurar objetos em partições de domínio.
- O Centro Administrativo do Active Directory tem critérios de filtragem avançados, facilitando a restauração de destino em grandes ambientes, com muitos objetos excluídos intencionalmente.
  
### <a name="limitations"></a>Limitações  
  
- Como o Centro Administrativo do Active Directory só pode gerenciar partições de domínio, ele não consegue restaurar objetos excluídos de partições de Configuração, DNS de Domínio ou DNS de Floresta (não é possível excluir objetos da partição Esquema). Para restaurar objetos de partições não domínio, use [Restore-ADObject](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee617262(v=technet.10)).  

- O Centro Administrativo do Active Directory não pode restaurar subárvores de objetos em uma única ação. Por exemplo, se você excluir uma UO com UOs, usuários, grupos e computadores aninhados, a restauração da UO de base não restaurará os objetos filhos.  
  
    > [!NOTE]  
    > A operação de restauração de lote Centro Administrativo do Active Directory faz uma classificação de "melhor esforço" dos objetos excluídos *dentro da seleção somente* para que os pais sejam ordenados antes dos filhos da lista de restauração. Em casos de teste simples, subárvores de objetos podem ser restauradas em uma única ação. Mas os casos de canto, como uma seleção que contém árvores parciais, com alguns dos nós pai excluídos ausentes ou casos de erro, como ignorar os objetos filho quando a restauração pai falham, podem não funcionar conforme o esperado. Por isso, você deve sempre restaurar subárvores de objetos como uma ação separada depois de restaurar os objetos pais.  
  
O Active Directory Lixeira requer um nível funcional de floresta do Windows Server 2008 R2 e você deve ser um membro do grupo Administradores de empresa. Uma vez habilitada, não é possível desabilitar a Lixeira do Active Directory. A Lixeira do Active Directory aumenta o tamanho do banco de dados do Active Directory (NTDS.DIT​​) em cada controlador de domínio na floresta. O espaço em disco usado pela lixeira continua a aumentar ao longo do tempo, uma vez que preserva objetos e todos os seus dados de atributos.  
  
### <a name="enabling-active-directory-recycle-bin-using-active-directory-administrative-center"></a>Habilitando a Lixeira do Active Directory usando o Centro Administrativo do Active Directory

Para habilitar a Lixeira do Active Directory, abra o **Centro Administrativo do Active Directory** e clique no nome da floresta no painel de navegação. No painel **Tarefas**, clique em **Habilitar Lixeira**.  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBin.png)  
  
O Centro Administrativo do Active Directory mostra a caixa de diálogo **Habilitar Confirmação de Lixeira**. Essa caixa de diálogo avisa que a habilitação da lixeira é irreversível. Clique em **OK** para habilitar a Lixeira do Active Directory. O Centro Administrativo do Active Directory mostra outra caixa de diálogo para lembrá-lo que a Lixeira do Active Directory não estará totalmente funcional até que todos os controladores de domínio repliquem a alteração de configuração.  
  
> [!IMPORTANT]  
> A opção para habilitar a Lixeira do Active Directory não estará disponível se:  
>
> - O nível funcional da floresta for mais baixo do que o do Windows Server 2008 R2  
> - Já estiver habilitada  

O cmdlet equivalente Active Directory Windows PowerShell é:  

```powershell
Enable-ADOptionalFeature  
```

Para obter mais informações sobre como usar o Windows PowerShell para habilitar o a Lixeira do Active Directory, consulte o [Guia passo a passo da Lixeira do Active Directory](./introduction-to-active-directory-administrative-center-enhancements--level-100-.md#active-directory-recycle-bin-step-by-step).  
  
### <a name="managing-active-directory-recycle-bin-using-active-directory-administrative-center"></a>Gerenciando a Lixeira do Active Directory usando o Centro Administrativo do Active Directory

Esta seção usa o exemplo de um domínio existente chamado **corp.contoso.com**. Este domínio organiza os usuários em uma UO pai chamada **UserAccounts**. A UO **UserAccounts** contém três UOs nomeadas por departamento, cada qual com mais UOs, usuários e grupos.  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBinExampleOU.png)  
  
#### <a name="storage-and-filtering"></a>Armazenamento e filtragem

A Lixeira do Active Directory preserva todos os objetos excluídos na floresta. Ela salva esses objetos de acordo com o atributo **msDS-deletedObjectLifetime** que, por padrão, é definido para corresponder ao atributo **tombstoneLifetime** da floresta. Em qualquer floresta criada usando o Windows Server 2003 SP1 ou mais recente, o valor de **tombstoneLifetime** é definido para 180 dias por padrão. Em qualquer floresta atualizada do Windows 2000 ou instalada com o Windows Server 2003 (sem service pack), o atributo padrão tombstoneLifetime NÃO É DEFINIDO e, portanto, o Windows usa o padrão interno de 60 dias. Tudo isso é configurável. Você pode usar o Centro Administrativo do Active Directory para restaurar quaisquer objetos excluídos das partições de domínio da floresta. Você deve continuar a usar o cmdlet **Restore-ADObject** para restaurar objetos excluídos de outras partições, como Configuração. A habilitação da Lixeira do Active Directory faz com que o contêiner **Objetos Excluídos** fique visível em cada partição de domínio no Centro Administrativo do Active Directory.  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_DeletedObjectsContainer.png)  
  
O contêiner **Objetos Excluídos** mostra todos os objetos restauráveis ​​naquela partição de domínio. Objetos excluídos mais antigos que **msDS-deletedObjectLifetime** são conhecidos como objetos reciclados. O Centro Administrativo do Active Directory não mostra objetos reciclados e não é possível restaurar esses objetos usando o Centro Administrativo do Active Directory.  
  
Para uma explicação mais profunda sobre a arquitetura e as regras de processamento da lixeira, consulte [A lixeira do AD: Noções básicas, implementação, práticas recomendadas e solução de problemas](/archive/blogs/askds/the-ad-recycle-bin-understanding-implementing-best-practices-and-troubleshooting).  
  
O Centro Administrativo do Active Directory limita artificialmente o número padrão de objetos retornados de um contêiner a 20.000 objetos. Você pode aumentar esse limite até 100.000 objetos, clicando no menu **Gerenciar** e depois em **Opções da Lista de Gerenciamento**.  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_MgmtList.png)  
  
#### <a name="restoration"></a>Restauração  
  
##### <a name="filtering"></a>Filtragem

O Centro Administrativo do Active Directory oferece critérios poderosos e opções de filtragem com os quais você deve se familiarizar antes de precisar usá-los em uma restauração real. Os domínios excluem intencionalmente muitos objetos ao longo de sua vida útil. Sendo que a vida útil do objeto excluído provavelmente é de 180 dias, não é possível simplesmente restaurar todos os objetos quando ocorre um acidente.  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_AddCriteria.png)  
  
Em vez de escrever filtros LDAP complexos e converter valores UTC em datas e horas, use o menu básico e avançado **Filtrar** para listar somente os objetos relevantes. Se você souber o dia da exclusão, os nomes dos objetos ou quaisquer outros dados importantes, use isso a seu favor durante a filtragem. Alterne as opções de filtro avançadas, clicando na divisa à direita da caixa de pesquisa.  
  
A operação de restauração dá suporte a todas as opções de critérios de filtro padrão, como em qualquer outra pesquisa. Dos filtros embutidos, os mais importantes para a restauração de objetos são tipicamente:  
  
- *ANR (resolução de nome ambígua-não listado no menu, mas o que é usado quando você digita na caixa * * * * filtro * * * *)*  
- Última modificação entre datas determinadas  
- O objeto é user/inetorgperson/computer/group/organization unit  
- Nome  
- Quando excluído  
- Último pai conhecido  
- Tipo  
- Descrição  
- City  
- País/região  
- Departamento  
- ID do funcionário  
- Nome  
- Cargo  
- Sobrenome  
- SAMaccountname  
- Estado/Província  
- Número de telefone  
- UPN  
- CEP  

Você pode adicionar vários critérios. Por exemplo, você pode encontrar todos os objetos de usuário excluídos em 24 de setembro de 2012 de Chicago, Illinois com um cargo de gerente.
  
Você também pode adicionar, modificar ou reorganizar os cabeçalhos da coluna para fornecer mais detalhes ao avaliar os objetos a serem recuperados.  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ColumnHeaders.png)  
  
Para obter mais informações sobre resolução de nomes ambíguos, consulte [Atributos de ANR](/windows/win32/adschema/attributes-anr).  
  
##### <a name="single-object"></a>Objeto único

A restauração de objetos excluídos sempre foi uma operação única.  O Centro Administrativo do Active Directory facilita esta operação. Para restaurar um objeto excluído, como um único usuário:  
  
1. Clique no nome de domínio no painel de navegação do Centro Administrativo do Active Directory.  
2. Clique duas vezes em **Objetos excluídos** na lista de gerenciamento.  
3. Clique com o botão direito do mouse no objeto e, em seguida, em **Restaurar**, ou clique em **Restaurar** no painel **Tarefas**.  
  
O objeto é restaurado ao seu local original.  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreSingle.gif)  
  
Clique em **restaurar para...** para alterar o local de restauração. Isso será útil se o contêiner pai do objeto excluído também tiver sido excluído, mas você não quiser restaurar o pai.  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreToSingle.gif)  
  
##### <a name="multiple-peer-objects"></a>Vários objetos pares

É possível restaurar vários objetos pares, assim como todos os usuários em uma UO. Mantenha pressionada a tecla CTRL e clique em um ou mais objetos excluídos que você deseja restaurar. Clique em **Restaurar** no painel Tarefas. Você também pode selecionar todos os objetos exibidos, mantendo pressionadas as teclas CTRL e A, ou um intervalo de objetos usando SHIFT e clique.  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestorePeers.png)  
  
##### <a name="multiple-parent-and-child-objects"></a>Vários objetos pais e filhos

É fundamental entender o processo de restauração de uma restauração multi-pai-filho, porque o Centro Administrativo do Active Directory não pode restaurar uma árvore aninhada de objetos excluídos com uma única ação.  
  
1. Restaure o objeto mais excluído em uma árvore.  
2. Restaure os filhos imediatos desse objeto pai.  
3. Restaure os filhos imediatos desses objetos pai.  
4. Repita conforme necessário até que todos os objetos sejam restaurados.  
  
Não é possível restaurar um objeto filho antes de restaurar seu pai. A tentativa dessa restauração retorna o seguinte erro:  
  
**A operação não pôde ser executada porque o pai do objeto está não instanciado ou foi excluído.**  
  
O atributo **Último Pai Conhecido** mostra a relação de pai de cada objeto. O atributo **Último Pai Conhecido** muda do local excluído para o local restaurado quando você atualiza o Centro Administrativo do Active Directory após a restauração de um pai. Portanto, você pode restaurar esse objeto filho quando o local de um objeto pai não exibir mais o nome distinto do contêiner objetos excluídos.  
  
Considere o cenário em que um administrador exclui acidentalmente a UO de Vendas, que contém UOs filhas e usuários.  
  
Primeiro, observe o valor do **último atributo pai conhecido** para todos os usuários excluídos e como ele lê **ou = Sales\0ADEL:*<GUID + nome diferenciado do contêiner de objetos excluídos> * * *:  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParent.gif)  
  
Filtre o nome ambíguo de Vendas para retornar a UO excluída que pode, então, ser restaurada:  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSales.png)  
  
Atualize a Centro Administrativo do Active Directory para ver a última alteração do atributo pai conhecido do objeto de usuário excluído para o nome distinto da UO de vendas restauradas:  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesRestored.gif)  
  
Filtre todos os usuários de Vendas. Mantenha pressionadas as teclas CTRL e A para selecionar todos os usuários de Vendas excluídos. Clique em **Restaurar** para mover os objetos do contêiner **Objetos Excluídos** para a UO de Vendas com suas associações de grupo e atributos intactos.  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesUndelete.png)  
  
Se a UO de **Vendas** contivesse UOs filhas próprias, então, você restauraria a UO filha primeiro antes de restaurar seus filhos, e assim por diante.  
  
Para restaurar todos os objetos excluídos aninhados especificando um contêiner pai excluído, consulte o [Apêndice B: Restaurar vários objetos excluídos do Active Directory (script de exemplo)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379504(v=ws.10)).  
  
O cmdlet do Windows PowerShell para Active Directory para a restauração de objetos excluídos é:  

```powershell
Restore-adobject  
```

A funcionalidade do cmdlet **Restore-ADObject** não foi alterada entre o Windows Server 2008 R2 e o Windows Server 2012.  
  
##### <a name="server-side-filtering"></a>Filtragem de servidor

É possível que, com o tempo, o contêiner Objetos Excluídos acumule mais de 20.000 (ou até mesmo 100.000) objetos em médias e grandes empresas e tenha dificuldades em mostrar todos os objetos. Como o mecanismo de filtro no Centro Administrativo do Active Directory baseia-se na filtragem de cliente, ele não pode mostrar estes objetos adicionais. Para contornar essa limitação, use as seguintes etapas para realizar uma busca no servidor:  
  
1. Clique com o botão direito do mouse no contêiner **Objetos Excluídos** e depois em **Pesquisar neste nó**.  
2. Clique na divisa para expor o menu **+Adicionar critérios**, selecione e adicione **Última modificação entre determinadas datas**. A hora da última modificação (o atributo **whenChanged**) é uma aproximação da hora de exclusão; na maioria dos ambientes, elas são idênticas. Esta consulta realiza uma pesquisa de servidor.  
3. Localize os objetos excluídos para restaurar usando outra filtragem de exibição, classificação e assim por diante nos resultados e, em seguida, restaure-os normalmente.  
  
## <a name="configuring-and-managing-fine-grained-password-policies-using-active-directory-administrative-center"></a><a name="BKMK_FGPP"></a>Configurando e gerenciando políticas de senha refinada usando o Centro Administrativo do Active Directory  
  
### <a name="configuring-fine-grained-password-policies"></a>Configurando políticas de senha refinada

O Centro Administrativo do Active Directory permite que você crie e gerencie objetos de FGPP (Política de Senha Refinada). O Windows Server 2008 introduziu o recurso FGPP, mas o Windows Server 2012 tem a primeira interface gráfica de gerenciamento para ele. Você aplica as Políticas de Senha Refinada em um nível de domínio e ele permite substituir a senha de domínio único exigida pelo Windows Server 2003. Ao criar uma FGPP diferente, com diferentes configurações, usuários individuais ou grupos obtêm diferentes políticas de senha em um domínio.  
  
Para obter informações sobre Políticas de Senha Refinada, consulte o [Guia passo a passo da política de bloqueio de senhas e contas refinadas do AD DS (Windows Server 2008 R2)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770842(v=ws.10)).  
  
No painel de navegação, clique no modo de exibição de árvore, depois no domínio, em seguida, em **Sistema**, clique em **Contêiner de Configuração de Senha** e depois, no painel Tarefas, clique em **Novo** e **Configurações de Senha**.  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_PasswordSettings.png)  
  
### <a name="managing-fine-grained-password-policies"></a>Gerenciando políticas de senha refinada

A criação de uma nova FGPP ou edição de uma existente traz o editor de **Configurações de Senha**. A partir daqui, você pode configurar todas as políticas de senha desejadas, como no Windows Server 2008 ou Windows Server 2008 R2, só que agora com um editor construído para esse fim.  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_CreatePasswordSettings.png)  
  
Preencha todos os campos obrigatórios (com asterisco vermelho) e quaisquer campos opcionais, e clique em **Adicionar** para definir os usuários ou grupos que recebem esta política. A FGPP substitui as configurações de política de domínio padrão para as entidades de segurança especificadas. Na figura acima, uma política extremamente restritiva se aplica apenas à conta de Administrador interno, para evitar comprometimento. A política é muito complexa para que os usuários padrão estejam em conformidade, mas é perfeita para uma conta de alto risco usada apenas por profissionais de TI.  
  
Você também define prioridade e a quais usuários e grupos a política se aplica, dentro de determinado domínio.  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_Precedence.png)  
  
Os cmdlet do Windows PowerShell para o Active Directory para a Política de Senha Refinada são:  
  
```powershell
Add-ADFineGrainedPasswordPolicySubject  
Get-ADFineGrainedPasswordPolicy  
Get-ADFineGrainedPasswordPolicySubject  
New-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicySubject  
Set-ADFineGrainedPasswordPolicy  
```

A funcionalidade de cmdlet da Política de Senha Refinada não foi alterada entre o Windows Server 2008 R2 e o Windows Server 2012. Por conveniência, o diagrama a seguir ilustra os argumentos associados aos cmdlets:  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPP.gif)  
  
O Centro Administrativo do Active Directory também permite localizar o conjunto resultante da FGPP aplicada a um usuário específico. Clique com o botão direito do mouse em qualquer usuário e clique em **exibir configurações de senha resultante...** para abrir a página *configurações de senha* que se aplica a esse usuário por meio de atribuição implícita ou explícita:  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RSOP.png)  
  
A análise das **Propriedades** de qualquer usuário ou grupo mostra as **Configurações de Senha Diretamente Associadas**, que são as FGPPs explicitamente atribuídas:  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPPSettings.gif)  
  
A atribuição de FGPP implícita não é exibida aqui; para isso, você deve usar a opção **exibir configurações de senha resultante...** .  
  
## <a name="using-the-active-directory-administrative-center-windows-powershell-history-viewer"></a><a name="BKMK_HistoryViewer"></a>Usando o visualizador de histórico do Windows PowerShell no Centro Administrativo do Active Directory

O futuro do gerenciamento do Windows é o Windows PowerShell. Ao sobrepor ferramentas gráficas sobre uma estrutura de automação de tarefas, o gerenciamento dos sistemas distribuídos mais complexos torna-se consistente e eficiente. Você precisa entender como funciona o Windows PowerShell para atingir seu pleno potencial e maximizar seus investimentos em computação.  
  
O Centro Administrativo do Active Directory agora fornece um histórico completo de todos os cmdlets do Windows PowerShell que ele executa e seus argumentos e valores. Você pode copiar o histórico do cmdlet em outro lugar para estudo ou modificação e reutilização. Você pode criar notas de Tarefas para ajudar a isolar os resultados dos comandos do Centro Administrativo do Active Directory no Windows PowerShell. Você também pode filtrar o histórico para encontrar pontos de interesse.  
  
O objetivo do Visualizador de Histórico do Windows PowerShell no Centro Administrativo do Active Directory é você aprender através da experiência prática.  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_HistoryViewer.gif)  
  
Clique na divisa (seta) para mostrar o Visualizador de Histórico do Windows PowerShell.  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RaiseViewer.png)  
  
Em seguida, crie um usuário ou modifique a associação de um grupo. O visualizador de histórico é atualizado continuamente com uma exibição recolhida de cada cmdlet que o Centro Administrativo do Active Directory executou com os argumentos especificados.  
  
Expanda qualquer item de linha de interesse para ver todos os valores fornecidos aos argumentos do cmdlet:  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ViewArgs.png)  
  
Clique no menu **Iniciar Tarefa** para criar uma notação manual antes de usar o Centro Administrativo do Active Directory para criar, modificar ou excluir um objeto. Digite o que você estava fazendo.  Ao terminar a alteração, selecione **Finalizar Tarefa**. A nota da tarefa agrupa todas as ações realizadas em uma nota recolhível que você pode usar para melhor compreensão.  
  
Por exemplo, para ver os comandos do Windows PowerShell usados ​​para alterar a senha de um usuário e removê-lo de um grupo:  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RemoveUser.gif)  
  
Se você marcar a caixa de seleção Mostrar Tudo, os cmdlets do Windows PowerShell verbo Get-* que apenas recuperam dados também serão mostrados.  
  
![Gerenciamento de AD DS avançado](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ShowAll.png)  
  
O visualizador de histórico mostra os comandos literais executados pelo Centro Administrativo do Active Directory e você pode notar que alguns cmdlets parecem sem executados desnecessariamente. Por exemplo, você pode criar um novo usuário com:  

```powershell
new-aduser
```

e não precisa usar:  

```powershell
set-adaccountpassword  
enable-adaccount  
set-aduser  
```

O design do Centro Administrativo do Active Directory exige o uso mínimo de código e modularidade. Portanto, em vez de um conjunto de funções que cria novos usuários e outro conjunto que modifica usuários existentes, ele executa minimamente cada função e, em seguida, as encadeia com os cmdlets. Lembre-se disso quando estiver aprendendo o Active Directory para o Windows PowerShell. Você também pode usar isso como uma técnica de aprendizagem, em que verá como é simples usar o Windows PowerShell para completar uma única tarefa.  
  
## <a name="troubleshooting-ad-ds-management"></a><a name="BKMK_Tshoot"></a>Solucionando problemas de gerenciamento AD DS  
  
### <a name="introduction-to-troubleshooting"></a>Introdução à solução de problemas

Por ser relativamente novo e pela falta de uso em ambientes existentes do cliente, o Centro Administrativo do Active Directory tem opções limitadas para solução de problemas.  
  
### <a name="troubleshooting-options"></a>Opções para solução de problemas  
  
#### <a name="logging-options"></a>Opções de log

O Centro Administrativo do Active Directory agora contém o registro em log interno, como parte de um arquivo de configuração de rastreamento. Crie/modifique o seguinte arquivo na mesma pasta que dsac.exe:  
  
**dsac.exe.config**
  
Crie o seguinte conteúdo:  
  
```xml
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

Os níveis de detalhamento para **DsacLogLevel** são **Nenhum**, **Erro**, **Aviso**, **Informações** e **Detalhado**. O nome do arquivo de saída é configurável e grava na mesma pasta que dsac.exe. A saída pode dizer mais sobre como o ADAC está funcionando, quais controladores de domínio são contatados, quais comandos do Windows PowerShell foram executados, quais foram as respostas e outros detalhes.  

Por exemplo, ao usar o nível INFORMAÇÕES, que retorna todos os resultados, exceto o detalhamento do rastreamento:  
  
- O DSAC.exe é iniciado  
- O registro em log é iniciado  
- O controlador de domínio solicitou o retorno de informações de domínio iniciais  

   ```
   [12:42:49][TID 3][Info] Command Id, Action, Command, Time, Elapsed Time ms (output), Number objects (output)  
   [12:42:49][TID 3][Info] 1, Invoke, Get-ADDomainController, 2012-04-16T12:42:49  
   [12:42:49][TID 3][Info] Get-ADDomainController-Discover:$null-DomainName:"CORP"-ForceDiscover:$null-Service:ADWS-Writable:$null  
   ```

- O controlador de domínio DC1 retornou do domínio Corp.  
- Unidade virtual PS AD carregada  

   ```
   [12:42:49][TID 3][Info] 1, Output, Get-ADDomainController, 2012-04-16T12:42:49, 1  
   [12:42:49][TID 3][Info] Found the domain controller 'DC1' in the domain 'CORP'.  
   [12:42:49][TID 3][Info] 2, Invoke, New-PSDrive, 2012-04-16T12:42:49  
   [12:42:49][TID 3][Info] New-PSDrive-Name:"ADDrive0"-PSProvider:"ActiveDirectory"-Root:""-Server:"dc1.corp.contoso.com"  
   [12:42:49][TID 3][Info] 2, Output, New-PSDrive, 2012-04-16T12:42:49, 1  
   [12:42:49][TID 3][Info] 3, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
   ```
  
- Obter informações de DSE de raiz de domínio  

   ```
   [12:42:49][TID 3][Info] Get-ADRootDSE -Server:"dc1.corp.contoso.com"  
   [12:42:49][TID 3][Info] 3, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
   [12:42:49][TID 3][Info] 4, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49  
   ```

- Obter informações da lixeira do AD de domínio  

   ```
   [12:42:49][TID 3][Info] Get-ADOptionalFeature -LDAPFilter:"(msDS-OptionalFeatureFlags=1)" -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 4, Output, Get-ADOptionalFeature, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 5, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADRootDSE -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 5, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 6, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADRootDSE -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 6, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 7, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADOptionalFeature -LDAPFilter:"(msDS-OptionalFeatureFlags=1)" -Server:"dc1.corp.contoso.com"
   [12:42:50][TID 3][Info] 7, Output, Get-ADOptionalFeature, 2012-04-16T12:42:50, 1
   [12:42:50][TID 3][Info] 8, Invoke, Get-ADForest, 2012-04-16T12:42:50
   ```

- Obter floresta AD  

   ```  
   [12:42:50][TID 3][Info] Get-ADForest -Identity:"corp.contoso.com" -Server:"dc1.corp.contoso.com"
   [12:42:50][TID 3][Info] 8, Output, Get-ADForest, 2012-04-16T12:42:50, 1  
   [12:42:50][TID 3][Info] 9, Invoke, Get-ADObject, 2012-04-16T12:42:50  
   ```

- Obter informações de esquema para tipos de criptografia com suporte, FGPP, algumas informações do usuário  

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

- Obter todas as informações sobre o objeto de domínio para exibir ao administrador que clicou no cabeçalho do domínio.  

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

A definição do nível de detalhe também mostra as pilhas .NET de cada função, mas não inclui dados suficientes para serem particularmente úteis, exceto ao solucionar o Dsac.exe que passa por uma violação de acesso ou pane. As duas causas prováveis ​​desse problema são:
  
- O serviço ADWS não está em execução em todos os controladores de domínio acessíveis.
- As comunicações de rede são bloqueadas para o serviço ADWS do computador que executa o Centro Administrativo do Active Directory.

> [!IMPORTANT]  
> Há também uma versão fora de banda do serviço chamada [Gateway de Gerenciamento do Active Directory](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=2852), que é executada no Windows Server 2008 SP2 e no Windows Server 2003 SP2.
>

Os erros mostrados quando nenhuma instância dos Serviços Web do Active Directory está disponível são:  
  
|Erro|Operação|
| --- | --- |  
|"Não é possível se conectar a nenhum domínio. Atualize ou tente novamente quando a conexão estiver disponível"|Mostrado no início do aplicativo do Centro Administrativo do Active Directory|
|"Não é possível encontrar um servidor disponível no *<NetBIOS domain name>* domínio que está executando o serviço Web do Active Directory (ADWS)"|Mostrado ao tentar selecionar um nó do domínio no aplicativo do Centro Administrativo do Active Directory|
  
Para solucionar esse problema, execute estas etapas:  
  
1. Verifique se o Serviço Web do Active Directory é iniciado em pelo menos um controlador de domínio no domínio (e, de preferência, todos os controladores de domínio na floresta). Verifique se ele está definido para iniciar automaticamente em todos os controladores de domínio também.
2. A partir do computador em execução no Centro Administrativo do Active Directory, confirme se você pode localizar um servidor que executa ADWS executando estes comandos NLTest.exe:  

   ```
   nltest /dsgetdc:<domain NetBIOS name> /ws /force
   nltest /dsgetdc:<domain fully qualified DNS name> /ws /force
   ```

   Se esses testes falharem mesmo que o serviço ADWS estiver funcionando, o problema será com a resolução de nome ou LDAP e não com o ADWS ou o Centro Administrativo do Active Directory. Este teste falha com o erro "1355 0x54B ERROR_NO_SUCH_DOMAIN" se o ADWS não estiver em execução em quaisquer controladores de domínio, assim, verifique novamente antes de chegar a qualquer conclusão.  
  
3. No controlador de domínio retornado por NLTest, despeje a lista da porta de escuta com o comando:  

   ```
   Netstat -anob > ports.txt  
   ```

   Examine o arquivo ports.txt e verifique se o serviço ADWS está escutando na porta 9389. Exemplo:  

   ```
   TCP    0.0.0.0:9389    0.0.0.0:0    LISTENING    1828  
   [Microsoft.ActiveDirectory.WebServices.exe]  

   TCP    [::]:9389       [::]:0       LISTENING    1828  
   [Microsoft.ActiveDirectory.WebServices.exe]  
   ```

   Se estiver escutando, valide as regras de Firewall do Windows e certifique-se de que permitem a entrada de TCO 9389. Por padrão, os controladores de domínio permitem a regra de firewall "Serviços Web do Active Directory (TCP-in)". Se não estiver escutando, confirme novamente se o serviço está sendo executado neste servidor e reinicie-o. Confirme se nenhum outro processo já está sendo executado na porta 9389.  
  
4. Instale o NetMon ou outro utilitário de captura de rede no computador que executa o Centro Administrativo do Active Directory e no controlador de domínio retornado por NLTEST. Reúna capturas de rede simultâneas de ambos os computadores onde você inicia o Centro Administrativo do Active Directory e veja o erro antes de parar as capturas. Verifique se o cliente é capaz de enviar e receber do controlador de domínio na porta TCP 9389. Se os pacotes são enviados, mas nunca chegam, ou chegam e o controlador de domínio responde, mas nunca chegam ao cliente, é provável que haja um firewall entre os computadores da rede deixando pacotes nessa porta. Esse firewall pode ser um software ou hardware, e pode ser parte de software (antivírus) de proteção de terminal de terceiros.  
  
## <a name="see-also"></a>Consulte Também

[Lixeira do AD, Política de Senha refinada e Histórico do PowerShell](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md)  
