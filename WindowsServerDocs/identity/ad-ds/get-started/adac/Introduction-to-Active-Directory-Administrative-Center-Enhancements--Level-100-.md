---
ms.assetid: 074e63e9-976c-49da-8cba-9ae0b3325e34
title: "Introdução aos aprimoramentos de centro administrativo do Active Directory (nível 100)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7f52b22ec74ba12c383952e68b412f871a56474c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="introduction-to-active-directory-administrative-center-enhancements-level-100"></a>Introdução aos aprimoramentos de centro administrativo do Active Directory (nível 100)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

ADAC no Windows Server 2012 inclui recursos de gerenciamento para o seguinte:

-   [Lixeira do Active Directory](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#ad_recycle_bin_mgmt)

-   [Política de senha refinada](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#fine_grained_pswd_policy_mgmt)

-   [Visualizador do Windows PowerShell histórico](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#windows_powershell_history_viewer)

## <a name="ad_recycle_bin_mgmt"></a>Lixeira do Active Directory
A exclusão acidental de objetos do Active Directory é uma ocorrência comuns para os usuários dos serviços de domínio do Active Directory (AD DS) e Active Directory Lightweight Directory Services (AD LDS). Em versões anteriores do Windows Server, antes do Windows Server 2008 R2, um pode recuperar objetos acidentalmente excluídos no Active Directory, mas as soluções tinham suas desvantagens.

No Windows Server 2008, você pode usar o recurso de Backup do Windows Server e **ntdsutil** comando restauração marcar objetos como autorizados para garantir que os dados restaurados foi duplicados em todo o domínio. A desvantagem da solução de restauração foi que precisava ser executada no modo de restauração dos serviços de diretório (DSRM). Durante a DSRM, o controlador de domínio que está sendo restaurado precisava permanecer offline. Portanto, não foi capaz de solicitações de serviço de cliente.

No Active Directory do Windows Server 2003 e Windows Server 2008 AD DS, você pode recuperar objetos do Active Directory excluídos por meio de reanimação modo tombstone. No entanto, reativada vinculados atributos dos objetos (por exemplo, membros do grupo de contas de usuário) que foram removidos fisicamente e atributos com valor link que foram limpas não foram recuperados. Portanto, os administradores podem não dependem de reanimação modo tombstone como a melhor solução para exclusão acidental de objetos. Para saber mais sobre reanimação marcado para exclusão, consulte [reanimar objetos do Active Directory modo Tombstone](https://go.microsoft.com/fwlink/?LinkID=125452).

Active Directory Lixeira, a partir do Windows Server 2008 R2, complementa a infraestrutura de reanimação modo tombstone existente e aprimora sua capacidade de preservar e recuperar objetos do Active Directory acidentalmente excluídos.

Quando você habilitar o Active Directory Lixeira, todos os valores de link com valor link atributos de objetos do Active Directory excluídos são preservados e os objetos são restaurados em sua totalidade no mesmo estado lógico consistente que estavam imediatamente antes da exclusão. Por exemplo, contas de usuário restaurados automaticamente recuperar todos os membros do grupo e os direitos de acesso correspondentes que tinham imediatamente antes da exclusão, dentro e entre domínios. A Lixeira do Active Directory funciona para ambientes AD DS e do AD LDS. Para obter uma descrição detalhada da Lixeira do Active Directory, consulte [What's New no AD DS: a Lixeira do Active Directory](https://technet.microsoft.com/library/dd391916(WS.10).aspx).

**O que há de novo?** No Windows Server 2012, o recurso a Lixeira do Active Directory foi aprimorado com uma nova interface gráfica do usuário para os usuários gerenciem e restaurar objetos excluídos. Os usuários podem agora visualmente localize uma lista de objetos excluídos e restaurá-las aos seus locais originais ou desejados.

Se você pretende habilitar Active Directory Lixeira no Windows Server 2012, considere o seguinte:

-   Por padrão, a Lixeira do Active Directory está desabilitada. Para habilitá-lo, primeiro você deve aumentar o nível funcional da floresta do AD DS ou AD LDS ambiente para o Windows Server 2008 R2 ou superior. Isso por sua vez exige que todos os controladores de domínio na floresta ou todos os servidores que hospedam instâncias de conjuntos de configuração do AD LDS estar executando o Windows Server 2008 R2 ou superior.

-   O processo de habilitar a Lixeira do Active Directory é irreversível. Depois de habilitar a Lixeira do Active Directory em seu ambiente, você não pode desativá-la.

-   Para gerenciar o recurso de Lixeira por meio de uma interface do usuário, você deve instalar a versão do Centro Administrativo do Active Directory no Windows Server 2012.

    > [!NOTE]
    > Você pode usar **Gerenciador do servidor** para instalar ferramentas de administração de servidor remoto (RSAT) no Windows Server 2012 computadores para usar a versão correta do Centro Administrativo do Active Directory para gerenciar a Lixeira por meio de uma interface do usuário.
    > 
    > Você pode usar [RSAT](https://go.microsoft.com/fwlink/?LinkID=238560) no Windows&reg; 8 computadores para usar a versão correta do Centro Administrativo do Active Directory para gerenciar a Lixeira por meio de uma interface do usuário.

### <a name="active-directory-recycle-bin-step-by-step"></a>Active Directory Lixeira passo a passo
Nas etapas a seguir, você usará ADAC para executar as seguintes tarefas a Lixeira do Active Directory no Windows Server 2012:

-   [Etapa 1: Aumentar o nível funcional da floresta](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_raise_ffl)

-   [Etapa 2: Habilitar a Lixeira](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_enable_recycle_bin)

-   [Etapa 3: Criar os usuários de teste, grupo e unidade organizacional](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_test_env)

-   [Etapa 4: Restaurar objetos excluídos](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_restore_del_obj)

> [!NOTE]
> A associação ao grupo Administradores corporativos ou permissões equivalentes é necessária para executar as etapas a seguir.

### <a name="bkmk_raise_ffl"></a>Etapa 1: Aumentar o nível funcional da floresta
Nesta etapa, você aumentará o nível funcional da floresta. Primeiro, você deve aumentar o nível funcional da floresta de destino para ser o Windows Server 2008 R2 no mínimo antes de habilitar a Lixeira do Active Directory.

##### <a name="to-raise-the-functional-level-on-the-target-forest"></a>Para aumentar o nível funcional na floresta de destino

1.  Clique com botão direito no ícone do Windows PowerShell, clique em **executar como administrador** e tipo **dsac.exe** abrir ADAC.

2.  Clique em **gerenciar**, clique em **adicionar nós de navegação** e selecione o domínio de destino apropriado no **adicionar nós de navegação** caixa de diálogo e clique **Okey**.

3.  Clique no domínio de destino no painel de navegação esquerdo e além do **tarefas** painel, clique em **aumentar o nível funcional da floresta**. Selecione um nível funcional da floresta que tenha pelo menos Windows Server 2008 R2 ou superior e, em seguida, clique em **Okey**.

![Introdução ao centro de administração do AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *

O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.

```
Set-ADForestMode -Identity contoso.com -ForestMode Windows2008R2Forest -Confirm:$false
```

Para o **-identidade** argumento, especifique o nome totalmente qualificado do DNS.

### <a name="bkmk_enable_recycle_bin"></a>Etapa 2: Habilitar a Lixeira
Nesta etapa, você permitirá que a Lixeira restaurar objetos excluídos no AD DS.

##### <a name="to-enable-active-directory-recycle-bin-in-adac-on-the-target-domain"></a>Para habilitar o Active Directory Lixeira no ADAC no domínio de destino

1.  Clique com botão direito no ícone do Windows PowerShell, clique em **executar como administrador** e tipo **dsac.exe** abrir ADAC.

2.  Clique em **gerenciar**, clique em **adicionar nós de navegação** e selecione o domínio de destino apropriado no **adicionar nós de navegação** caixa de diálogo e clique **Okey**.

3.  No **tarefas** painel, clique em **habilitar Lixeira... ** no **tarefas** painel, clique em **Okey** na caixa de mensagem de aviso e clique **Okey** à mensagem ADAC de atualização.

4.  Pressione F5 para atualizar ADAC.

![Introdução ao centro de administração do AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *

O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.

```
Enable-ADOptionalFeature -Identity 'CN=Recycle Bin Feature,CN=Optional Features,CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,DC=contoso,DC=com' -Scope ForestOrConfigurationSet -Target 'contoso.com'
```

### <a name="bkmk_create_test_env"></a>Etapa 3: Criar os usuários de teste, grupo e unidade organizacional
Os procedimentos a seguir, você irá criar dois usuários de teste. Você irá criar um grupo de teste e adicionar os usuários de teste ao grupo. Além disso, você criará uma unidade Organizacional.

##### <a name="to-create-test-users"></a>Para criar os usuários de teste

1.  Clique com botão direito no ícone do Windows PowerShell, clique em **executar como administrador** e tipo **dsac.exe** abrir ADAC.

2.  Clique em **gerenciar**, clique em **adicionar nós de navegação** e selecione o domínio de destino apropriado no **adicionar nós de navegação** caixa de diálogo e clique **Okey**.

3.  No **tarefas** painel, clique em **nova** e, em seguida, clique em **usuário**.

    ![Introdução ao centro de administração do AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewUser.gif)

4.  Insira as seguintes informações em **conta** e, em seguida, clique em Okey:

    -   Nome completo: test1

    -   Logon do usuário SamAccountName: test1

    -   Senha:p@ssword1

    -   Confirme senha:p@ssword1

5.  Repita as etapas anteriores para criar um segundo usuário, test2.

##### <a name="to-create-a-test-group-and-add-users-to-the-group"></a>Para criar um grupo de teste e adicionar usuários ao grupo

1.  Clique com botão direito no ícone do Windows PowerShell, clique em **executar como administrador** e tipo **dsac.exe** abrir ADAC.

2.  Clique em **gerenciar**, clique em **adicionar nós de navegação** e selecione o domínio de destino apropriado no **adicionar nós de navegação** caixa de diálogo e clique **Okey**.

3.  No **tarefas** painel, clique em **nova** e, em seguida, clique em **grupo**.

4.  Insira as seguintes informações em **grupo** e, em seguida, clique em **Okey**:

    -   **Nome de grupo: group1**

5.  Clique em **group1**e, em seguida, sob o **tarefas** painel, clique em **propriedades**.

6.  Clique em **membros**, clique em **adicionar**, tipo **test1; test2**e clique em **Okey**.

![Introdução ao centro de administração do AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *

O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.

```
Add-ADGroupMember -Identity group1 -Member test1
```

##### <a name="to-create-an-organizational-unit"></a>Para criar uma unidade organizacional

1.  Clique com botão direito no ícone do Windows PowerShell, clique em **executar como administrador** e tipo **dsac.exe** abrir ADAC.

2.  Clique em **gerenciar**, clique em **adicionar nós de navegação** e selecione o domínio de destino apropriado no **adicionar nós de navegação** caixa de diálogo e clique **Okey**.

3.  No **tarefas** painel, clique em **nova** e, em seguida, clique em **unidade organizacional**.

4.  Insira as seguintes informações em **unidade organizacional** e, em seguida, clique em **Okey**:

    -   **NameOU1**

![Introdução ao centro de administração do AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *

O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.

```
1..2 | ForEach-Object {New-ADUser -SamAccountName test$_ -Name "test$_" -Path "DC=fabrikam,DC=com" -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssword1" -Force) -Enabled $true}
New-ADGroup -Name "group1" -SamAccountName group1 -GroupCategory Security -GroupScope Global -DisplayName "group1"
New-ADOrganizationalUnit -Name OU1 -Path "DC=fabrikam,DC=com"

```

### <a name="bkmk_restore_del_obj"></a>Etapa 4: Restaurar objetos excluídos
Os procedimentos a seguir, você restaurará objetos excluídos do **Deleted Objects** contêiner para seu local original e em um local diferente.

##### <a name="to-restore-deleted-objects-to-their-original-location"></a>Para restaurar excluído objetos para o local original

1.  Clique com botão direito no ícone do Windows PowerShell, clique em **executar como administrador** e tipo **dsac.exe** abrir ADAC.

2.  Clique em **gerenciar**, clique em **adicionar nós de navegação** e selecione o domínio de destino apropriado no **adicionar nós de navegação** caixa de diálogo e clique **Okey**.

3.  Selecione os usuários **test1** e **test2**, clique em **excluir** no **tarefas** painel e clique **Sim** para confirmar a exclusão.

    ![Introdução ao centro de administração do AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *

    O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.

    ```
    Get-ADUser -Filter 'Name -Like "*test*"'|Remove-ADUser -Confirm:$false
    ```

4.  Navegue até o **Deleted Objects** contêiner, selecione **test2** e **test1** e, em seguida, clique em **restaurar** no **tarefas** painel.

5.  Para confirmar se que os objetos foram restaurados para seu local original, navegue até o domínio de destino e verifique se que as contas de usuário estão listadas.

    > [!NOTE]
    > Se você navegar para a **propriedades** das contas de usuário **test1** e **test2** e, em seguida, clique em **membro de**, você verá que sua participação no grupo também foi restaurada.

O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.

![Introdução ao centro de administração do AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *

```
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject
```

##### <a name="to-restore-deleted-objects-to-a-different-location"></a>Para restaurar excluído objetos para um local diferente

1.  Clique com botão direito no ícone do Windows PowerShell, clique em **executar como administrador** e tipo **dsac.exe** abrir ADAC.

2.  Clique em **gerenciar**, clique em **adicionar nós de navegação** e selecione o domínio de destino apropriado no **adicionar nós de navegação** caixa de diálogo e clique **Okey**.

3.  Selecione os usuários **test1** e **test2**, clique em **excluir** no **tarefas** painel e clique **Sim** para confirmar a exclusão.

4.  Navegue até o **Deleted Objects** contêiner, selecione **test2** e **test1** e, em seguida, clique em **restaurar em** no **tarefas** painel.

5.  Selecione **OU1** e, em seguida, clique em **Okey**.

6.  Para confirmar os objetos foram restauradas para **OU1**, navegue até o domínio de destino, clique duas vezes **OU1** e verifique se as contas de usuário estão listadas.

![Introdução ao centro de administração do AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *

O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.

```
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject -TargetPath "OU=OU1,DC=contoso,DC=com"
```

## <a name="fine_grained_pswd_policy_mgmt"></a>Política de senha refinada
O sistema operacional Windows Server 2008 oferece às organizações uma maneira de definir políticas de bloqueio de conta para diferentes conjuntos de usuários e de senha diferentes em um domínio. Nos domínios do Active Directory anteriores ao Windows Server 2008, política apenas uma senha e política de bloqueio de conta podem ser aplicadas a todos os usuários no domínio. Essas políticas foram especificadas na política de domínio padrão para o domínio. Como resultado, as organizações que desejava diferentes configurações de bloqueio de conta e senha para diferentes conjuntos de usuários tinham que criar um filtro de senha ou implantar vários domínios. Ambos são opções barata.

Você pode usar políticas de senha refinadas para especificar várias políticas de senha em um único domínio e restrições diferentes políticas de bloqueio de conta e senha para diferentes conjuntos de usuários em um domínio. Por exemplo, você pode aplicar configurações mais estritas para contas privilegiadas e menos estrito para as contas de outros usuários. Em outros casos, talvez você deseja aplicar uma política de senha especial para contas cujas senhas são sincronizadas com outras fontes de dados. Para obter uma descrição detalhada de política de senha refinadas, veja [AD DS: políticas de senha refinadas](https://technet.microsoft.com/library/cc770394(WS.10).aspx)

**O que há de novo?** No Windows Server 2012, o gerenciamento de política de senha refinadas é feito mais fácil e mais visual, fornecendo uma interface do usuário para os administradores do AD DS para gerenciá-los em ADAC. Os administradores agora podem exibir política resultante de um determinado usuário, exibir e classificar todas as políticas de senha de um determinado domínio e gerenciar políticas de senha individuais visualmente.

Se você pretende usar políticas de senha refinadas no Windows Server 2012, considere o seguinte:

-   Políticas de senha refinadas se aplicam somente grupos de segurança globais e objetos de usuário (ou objetos inetOrgPerson se eles são usados em vez de objetos de usuário). Por padrão, somente membros do grupo Admins. do domínio podem definir políticas de senha refinadas. No entanto, você também pode delegar a capacidade de definir essas políticas para outros usuários. O nível funcional do domínio deve ser o Windows Server 2008 ou superior.

-   Você deve usar a versão do Windows Server 2012 do Centro Administrativo do Active Directory para administrar políticas de senha refinadas através de uma interface gráfica do usuário.

    > [!NOTE]
    > Você pode usar **Gerenciador do servidor** para instalar ferramentas de administração de servidor remoto (RSAT) no Windows Server 2012 computadores para usar a versão correta do Centro Administrativo do Active Directory para gerenciar a Lixeira por meio de uma interface do usuário.
    > 
    > Você pode usar [RSAT](https://go.microsoft.com/fwlink/?LinkID=238560) no Windows&reg; 8 computadores para usar a versão correta do Centro Administrativo do Active Directory para gerenciar a Lixeira por meio de uma interface do usuário.

### <a name="fine-grained-password-policy-step-by-step"></a>Política de senha refinadas passo a passo
Nas etapas a seguir, você usará ADAC para executar as seguintes tarefas de política de senha refinadas:

-   [Etapa 1: Aumentar o nível funcional do domínio](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_raise_dfl)

-   [Etapa 2: Criar unidade organizacional, grupo e os usuários de teste](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk2_test_fgpp)

-   [Etapa 3: Criar uma nova política de senha refinadas](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)

-   [Etapa 4: Exibir um conjunto de políticas para um usuário resultante](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_view_resultant_fgpp)

-   [Etapa 5: Editar uma política de senha refinadas](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_edit_fgpp)

-   [Etapa 6: Excluir uma política de senha refinadas](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_delete_fgpp)

> [!NOTE]
> Membro do grupo Admins. do domínio ou permissões equivalentes é necessária para executar as etapas a seguir.

#### <a name="bkmk_raise_dfl"></a>Etapa 1: Aumentar o nível funcional do domínio
O procedimento a seguir, você aumentará o nível funcional de domínio do domínio de destino para o Windows Server 2008 ou superior. Um nível funcional do domínio do Windows Server 2008 ou posterior é necessário para habilitar as políticas de senha refinadas.

###### <a name="to-raise-the-domain-functional-level"></a>Para aumentar o nível funcional do domínio

1.  Clique com botão direito no ícone do Windows PowerShell, clique em **executar como administrador** e tipo **dsac.exe** abrir ADAC.

2.  Clique em **gerenciar**, clique em **adicionar nós de navegação** e selecione o domínio de destino apropriado no **adicionar nós de navegação** caixa de diálogo e clique **Okey**.

3.  Clique no domínio de destino no painel de navegação esquerdo e além do **tarefas** painel, clique em **aumentar o nível funcional do domínio**. Selecione um nível funcional da floresta que tenha pelo menos Windows Server 2008 ou superior e, em seguida, clique em **Okey**.

![Introdução ao centro de administração do AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *

O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.

```
Set-ADDomainMode -Identity contoso.com -DomainMode 3
```

#### <a name="bkmk2_test_fgpp"></a>Etapa 2: Criar unidade organizacional, grupo e os usuários de teste
Para criar os usuários de teste e agrupar necessidade para esta etapa, siga os procedimentos localizados aqui: [etapa 3: criar os usuários de teste, grupo e unidade organizacional](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_test_env) (você não precisa criar a unidade Organizacional para demonstrar a política de senha refinada).

#### <a name="bkmk_create_fgpp"></a>Etapa 3: Criar uma nova política de senha refinadas
O procedimento a seguir, você criará uma nova política de senha refinadas usando a interface do usuário em ADAC.

###### <a name="to-create-a-new-fine-grained-password-policy"></a>Para criar uma nova política de senha refinadas bem

1.  Clique com botão direito no ícone do Windows PowerShell, clique em **executar como administrador** e tipo **dsac.exe** abrir ADAC.

2.  Clique em **gerenciar**, clique em **adicionar nós de navegação** e selecione o domínio de destino apropriado no **adicionar nós de navegação** caixa de diálogo e clique **Okey**.

3.  No painel de navegação ADAC, abra o **sistema** contêiner e depois clique em **contêiner de configurações de senha**.

4.  No **tarefas** painel, clique em **nova**e clique em **configurações de senha**.

    Preencha ou editar os campos dentro da página de propriedade para criar um novo **configurações de senha** objeto. O **nome** e **precedência** campos são necessários.

    ![Introdução ao centro de administração do AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewFGPP.gif)

5.  Em **diretamente aplica-se a**, clique em **adicionar**, tipo **group1**e clique em **Okey**.

    Isso associa o objeto de política de senha com os membros do grupo global que você criou para o ambiente de teste.

6.  Clique em **Okey** para enviar a criação.

![Introdução ao centro de administração do AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *

O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.

```
New-ADFineGrainedPasswordPolicy TestPswd -ComplexityEnabled:$true -LockoutDuration:"00:30:00" -LockoutObservationWindow:"00:30:00" -LockoutThreshold:"0" -MaxPasswordAge:"42.00:00:00" -MinPasswordAge:"1.00:00:00" -MinPasswordLength:"7" -PasswordHistoryCount:"24" -Precedence:"1" -ReversibleEncryptionEnabled:$false -ProtectedFromAccidentalDeletion:$true
Add-ADFineGrainedPasswordPolicySubject TestPswd -Subjects group1
```

#### <a name="bkmk_view_resultant_fgpp"></a>Etapa 4: Exibir um conjunto de políticas para um usuário resultante
No procedimento a seguir, você poderá ver as configurações de senha resultante de um usuário que seja um membro do grupo ao qual você atribuído a uma política de senha refinadas bem em [etapa 3: criar uma nova política de senha refinadas](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp).

###### <a name="to-view-a-resultant-set-of-policies-for-a-user"></a>Para exibir um conjunto de políticas para um usuário resultante

1.  Clique com botão direito no ícone do Windows PowerShell, clique em **executar como administrador** e tipo **dsac.exe** abrir ADAC.

2.  Clique em **gerenciar**, clique em **adicionar nós de navegação** e selecione o domínio de destino apropriado no **adicionar nós de navegação** caixa de diálogo e clique **Okey**.

3.  Selecione um usuário, **test1** que pertence ao grupo, **group1** que você associou uma política de senha refinadas com em [etapa 3: criar uma nova política de senha refinadas](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp).

4.  Clique em **configurações do modo de exibição resultante senha** no **tarefas** painel.

5.  Examinar a política de configuração de senha e clique em **Cancelar**.

![Introdução ao centro de administração do AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *

O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.

```
Get-ADUserResultantPasswordPolicy test1
```

#### <a name="bkmk_edit_fgpp"></a>Etapa 5: Editar uma política de senha refinadas
No procedimento a seguir, você irá editar a política de senha refinadas bem que você criou na [etapa 3: criar uma nova política de senha refinadas](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)

###### <a name="to-edit-a-fine-grained-password-policy"></a>Para editar uma política de senha refinadas

1.  Clique com botão direito no ícone do Windows PowerShell, clique em **executar como administrador** e tipo **dsac.exe** abrir ADAC.

2.  Clique em **gerenciar**, clique em **adicionar nós de navegação** e selecione o domínio de destino apropriado no **adicionar nós de navegação** caixa de diálogo e clique **Okey**.

3.  No ADAC **painel de navegação**, expanda **sistema** e, em seguida, clique em **contêiner de configurações de senha**.

4.  Selecione a política de senha refinadas bem que você criou na [etapa 3: criar uma nova política de senha refinadas](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp) e clique em **propriedades** no **tarefas** painel.

5.  Em **Impor histórico de senhas**, altere o valor da **número de senhas lembradas** para **30**.

6.  Clique em **Okey**.

![Introdução ao centro de administração do AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *

O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.

```
Set-ADFineGrainedPasswordPolicy TestPswd -PasswordHistoryCount:"30"
```

#### <a name="bkmk_delete_fgpp"></a>Etapa 6: Excluir uma política de senha refinadas

###### <a name="to-delete-a-fine-grained-password-policy"></a>Para excluir uma política de senha refinadas

1.  Clique com botão direito no ícone do Windows PowerShell, clique em **executar como administrador** e tipo **dsac.exe** abrir ADAC.

2.  Clique em **gerenciar**, clique em **adicionar nós de navegação** e selecione o domínio de destino apropriado no **adicionar nós de navegação** caixa de diálogo e clique **Okey**.

3.  No painel de navegação ADAC, expanda **sistema** e, em seguida, clique em **contêiner de configurações de senha**.

4.  Selecione a política de senha refinadas bem que você criou na [etapa 3: criar uma nova política de senha refinadas](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp) no **tarefas** painel clique **propriedades**.

5.  Limpar o **proteger contra exclusão acidental** caixa de seleção e clique em **Okey**.

6.  Selecione a política de senha refinadas bem e além do **tarefas** clique em painel **excluir**.

7.  Clique em **Okey** na caixa de diálogo de confirmação.

![Introdução ao centro de administração do AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *

O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.

```
Set-ADFineGrainedPasswordPolicy -Identity TestPswd -ProtectedFromAccidentalDeletion $False
Remove-ADFineGrainedPasswordPolicy TestPswd -Confirm
```

## <a name="windows_powershell_history_viewer"></a>Visualizador do Windows PowerShell histórico
ADAC é uma ferramenta de interface de usuário criada com base em Windows PowerShell.  No Windows Server 2012, os administradores de TI podem aproveitar ADAC para saber o Windows PowerShell para cmdlets do Active Directory usando o Visualizador de histórico do Windows PowerShell. Como ações são executadas na interface do usuário, o comando equivalente do Windows PowerShell é mostrado para o usuário no Visualizador de histórico do Windows PowerShell. Isso permite que os administradores criem scripts automatizados e reduzir tarefas repetitivas, aumentando a produtividade de TI.  Além disso, esse recurso reduz o tempo para aprender o Windows PowerShell para o Active Directory e aumenta a confiança dos usuários na correção dos seus scripts de automação.

Ao usar o Visualizador de histórico do Windows PowerShell no Windows Server 2012, considere o seguinte:

-   Para usar o Visualizador de Script do Windows PowerShell, você deve usar a versão do Windows Server 2012 do ADAC

    > [!NOTE]
    > Você pode usar **Gerenciador do servidor** para instalar ferramentas de administração de servidor remoto (RSAT) no Windows Server 2012 computadores para usar a versão correta do Centro Administrativo do Active Directory para gerenciar a Lixeira por meio de uma interface do usuário.
    > 
    > Você pode usar [RSAT](https://go.microsoft.com/fwlink/?LinkID=238560) no Windows&reg; 8 computadores para usar a versão correta do Centro Administrativo do Active Directory para gerenciar a Lixeira por meio de uma interface do usuário.

-   Tem algum conhecimento básico do Windows PowerShell. Por exemplo, você precisa saber como funciona o piping no Windows PowerShell. Para saber mais sobre piping no Windows PowerShell, consulte [Piping e o Pipeline no Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).

### <a name="windows-powershell-history-viewer-step-by-step"></a>Visualizador de histórico do Windows PowerShell passo a passo
O procedimento a seguir, você usará o Visualizador de histórico do Windows PowerShell em ADAC para construir um script do Windows PowerShell.  Antes de começar este procedimento, remova o usuário, **test1** do grupo, **group1**.

##### <a name="to-construct-a-script-using-powershell-history-viewer"></a>Para construir um script usando o Visualizador de histórico do PowerShell

1.  Clique com botão direito no ícone do Windows PowerShell, clique em **executar como administrador** e tipo **dsac.exe** abrir ADAC.

2.  Clique em **gerenciar**, clique em **adicionar nós de navegação** e selecione o domínio de destino apropriado no **adicionar nós de navegação** caixa de diálogo e clique **Okey**.

3.  Expanda o **história do Windows PowerShell** painel na parte inferior da tela ADAC.

4.  Selecione usuário, **test1**.

5.  Clique em **adicionar ao grupo... ** no **tarefas** painel.

6.  Navegue até **group1** e clique em **Okey** na caixa de diálogo.

7.  Navegue até o **história do Windows PowerShell** painel e localize o comando apenas gerado.

8.  Copie o comando e colá-lo em seu editor desejado para construir seu script.

    Por exemplo, você pode modificar o comando para adicionar outro usuário **group1**, ou adicionar **test1** para um grupo diferente.

## <a name="see-also"></a>Consulte também
[Avançadas do AD DS gerenciamento usando o Centro Administrativo do Active Directory & #40; Nível 200 & #41;](Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md)


