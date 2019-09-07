---
ms.assetid: 074e63e9-976c-49da-8cba-9ae0b3325e34
title: Introduction to Active Directory Administrative Center Enhancements (Level 100)
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5b328db355810f8e3a33b28637f789e8c703d781
ms.sourcegitcommit: f3b61dcd8aa0aa744db4ea938aac633c19217b0a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746331"
---
# <a name="introduction-to-active-directory-administrative-center-enhancements-level-100"></a>Introduction to Active Directory Administrative Center Enhancements (Level 100)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O Centro Administrativo do Active Directory no Windows Server inclui recursos de gerenciamento para o seguinte:

- [Lixeira de Active Directory](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#ad_recycle_bin_mgmt)
- [Política de senha refinada](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#fine_grained_pswd_policy_mgmt)
- [Visualizador de histórico do Windows PowerShell](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#windows_powershell_history_viewer)

## <a name="ad_recycle_bin_mgmt"></a>Lixeira de Active Directory

A exclusão acidental de objetos do Active Directory é uma ocorrência comum aos usuários do AD DS (Serviços de Domínio Active Directory) e do AD LDS (Active Directory Lightweight Directory Services). Nas versões anteriores do Windows Server, antes do Windows Server 2008 R2, era possível recuperar objetos excluídos acidentalmente no Active Directory, mas as soluções tiveram suas desvantagens.

No Windows Server 2008, era possível usar o recurso Backup do Windows Server e o comando de restauração autoritativa de objetos **ntdsutil** para marcar objetos como autoritativos. Isso garantia que os dados restaurados fossem replicados em todo o domínio. A desvantagem da solução de restauração autoritativa era que ela deveria ser executada no DSRM (Modo de Restauração dos Serviços de Diretório). Durante o DSRM, o controlador de domínio que estivesse sendo restaurado deveria permanecer offline. Portanto, ele não podia atender às solicitações dos clientes.

No Windows Server 2003 Active Directory e no Windows Server 2008 AD DS, era possível recuperar objetos excluídos do Active Directory por meio de reanimação de marca de exclusão. Entretanto, atributos de valores vinculados de objetos reanimados (por exemplo, associações de grupos de contas de usuário) que eram fisicamente removidos e atributos de valores não vinculados que eram limpos não eram recuperados. Portanto, os administradores não podiam confiar na reanimação de marca de exclusão como solução final para a exclusão acidental de objetos. Para obter mais informações sobre reanimação de marca de exclusão, consulte [Reanimando objetos de marca de exclusão do Active Directory](https://go.microsoft.com/fwlink/?LinkID=125452).

A Lixeira do Active Directory, a partir do Windows Server 2008 R2, tem como base a infraestrutura existente de reanimação de marcas de exclusão e aprimora a capacidade de preservar e recuperar objetos excluídos acidentalmente do Active Directory.

Quando a Lixeira do Active Directory é habilitada, todos os atributos de valores vinculados e não vinculados dos objetos excluídos do Active Directory são preservados, e os objetos são integralmente restaurados para o mesmo estado lógico consistente em que estavam imediatamente antes da exclusão. Por exemplo, as contas de usuário restauradas automaticamente recuperam todas as associações de grupo e direitos de acesso correspondentes que tinham antes da exclusão, dentro e entre domínios. A Lixeira do Active Directory funciona nos ambientes do AD DS e do AD LDS. Para obter uma descrição detalhada do Active Directory Lixeira, consulte [novidades no AD DS: Active Directory Lixeira](https://technet.microsoft.com/library/dd391916(WS.10).aspx).

**O que há de novo?** No Windows Server 2012 e mais recente, o recurso lixeira do Active Directory é aprimorado com uma nova interface gráfica do usuário para que os usuários gerenciem e restaurem objetos excluídos. Os usuários agora podem localizar visualmente uma lista de objetos excluídos e restaurá-los nos seus locais originais ou desejados.

Se você planeja habilitar Active Directory Lixeira no Windows Server, considere o seguinte:

- Por padrão, a Lixeira do Active Directory é desabilitada. Para habilitá-lo, primeiro você deve aumentar o nível funcional da floresta de seu AD DS ou AD LDS ambiente para o Windows Server 2008 R2 ou superior. Isso, por sua vez, requer que todos os controladores de domínio na floresta ou todos os servidores que hospedam instâncias do AD LDS conjuntos de configuração estejam executando o Windows Server 2008 R2 ou superior.
- O processo de habilitar a Lixeira do Active Directory é irreversível. Depois de habilitar a Lixeira do Active Directory em seu ambiente, não será possível desabilitá-la.
- Para gerenciar o recurso lixeira por meio de uma interface do usuário, você deve instalar a versão do Centro Administrativo do Active Directory no Windows Server 2012.

    > [!NOTE]
    > Você pode usar **Gerenciador do servidor** para instalar o ferramentas de administração de servidor remoto (RSAT) para usar a versão correta do centro administrativo do Active Directory para gerenciar a lixeira por meio de uma interface do usuário.
    >
    > Para obter informações sobre como instalar o RSAT, consulte o artigo [ferramentas de administração de servidor remoto](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).

### <a name="active-directory-recycle-bin-step-by-step"></a>Lixeira do Active Directory passo a passo

Nas etapas a seguir, você usará o ADAC para executar as seguintes Active Directory tarefas da lixeira no Windows Server 2012:

- [Etapa 1: Aumentar o nível funcional da floresta](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_raise_ffl)
- [Etapa 2: Habilitar lixeira](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_enable_recycle_bin)
- [Etapa 3: Criar usuários de teste, grupo e unidade organizacional](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_test_env)
- [Etapa 4: Restaurar objetos excluídos](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_restore_del_obj)

> [!NOTE]
> Para executar as etapas a seguir, é necessário ter uma associação ao grupo Administradores Corporativos ou permissões equivalentes.

### <a name="bkmk_raise_ffl"></a>Etapa 1: Aumentar o nível funcional da floresta

Nesta etapa, você aumentará o nível funcional da floresta. Primeiro, você deve elevar o nível funcional na floresta de destino para ser o Windows Server 2008 R2 no mínimo antes de habilitar Active Directory Lixeira.

#### <a name="to-raise-the-functional-level-on-the-target-forest"></a>Para aumentar o nível funcional na floresta de destino

1. Clique com o botão direito do mouse no ícone do Windows PowerShell, clique em **Executar como administrador** e digite **dsac. exe** para abrir o ADAC.

2. Clique em **Gerenciar**, em **Adicionar Nós de Navegação** , selecione o domínio de destino apropriado na caixa de diálogo **Adicionar Nós de Navegação** e clique em **OK**.

3. Clique no domínio de destino no painel de navegação esquerdo e, no painel **Tarefas**, clique em **Aumentar nível funcional da floresta**. Selecione um nível funcional de floresta que seja pelo menos o Windows Server 2008 R2 ou superior e clique em **OK**.

![Introdução aos](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell no</em> centro de administração do AD***

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

```powershell
Set-ADForestMode -Identity contoso.com -ForestMode Windows2008R2Forest -Confirm:$false
```

Para o argumento **-Identity** , especifique o nome de domínio DNS totalmente qualificado.

### <a name="bkmk_enable_recycle_bin"></a>Etapa 2: Habilitar a Lixeira

Nesta etapa, você habilitará a Lixeira para restaurar objetos excluídos do AD DS.

#### <a name="to-enable-active-directory-recycle-bin-in-adac-on-the-target-domain"></a>Para habilitar a Lixeira do Active Directory no ADAC no domínio de destino

1. Clique com o botão direito do mouse no ícone do Windows PowerShell, clique em **Executar como administrador** e digite **dsac. exe** para abrir o ADAC.

2. Clique em **Gerenciar**, em **Adicionar Nós de Navegação** , selecione o domínio de destino apropriado na caixa de diálogo **Adicionar Nós de Navegação** e clique em **OK**.

3. No **Painel de Tarefas**, clique em **Habilitar Lixeira...** ; no **Painel de Tarefas**, clique em **OK** na caixa de mensagem de aviso e clique em **OK** para atualizar a mensagem do ADAC.

4. Pressione F5 para atualizar o ADAC.

![Introdução aos](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell no</em> centro de administração do AD***

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

```powershell
Enable-ADOptionalFeature -Identity 'CN=Recycle Bin Feature,CN=Optional Features,CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,DC=contoso,DC=com' -Scope ForestOrConfigurationSet -Target 'contoso.com'
```

### <a name="bkmk_create_test_env"></a>Etapa 3: Criar usuários, um grupo e uma unidade organizacional de teste

Nos procedimentos a seguir, você criará dois usuários de teste. Em seguida, criará um grupo de teste e adicionará os usuários de teste ao grupo. Além disso, criará uma unidade organizacional.

#### <a name="to-create-test-users"></a>Para criar usuários de teste

1. Clique com o botão direito do mouse no ícone do Windows PowerShell, clique em **Executar como administrador** e digite **dsac. exe** para abrir o ADAC.

2. Clique em **Gerenciar**, em **Adicionar Nós de Navegação** , selecione o domínio de destino apropriado na caixa de diálogo **Adicionar Nós de Navegação** e clique em **OK**.

3. No painel **Tarefas** , clique em **Novo** e depois em **Usuário**.

    ![Introdução ao centro de administração do AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewUser.gif)

4. Insira as informações a seguir em **Conta** e clique em OK:

   - Nome completo: test1
   - Logon SamAccountName do usuário: test1
   - Lap@ssword1
   - Confirmar senha:p@ssword1

5. Repita as etapas anteriores para criar um segundo usuário, test2.

#### <a name="to-create-a-test-group-and-add-users-to-the-group"></a>Para criar um grupo de teste e adicionar usuários ao grupo

1. Clique com o botão direito do mouse no ícone do Windows PowerShell, clique em **Executar como administrador** e digite **dsac. exe** para abrir o ADAC.
2. Clique em **Gerenciar**, em **Adicionar Nós de Navegação** , selecione o domínio de destino apropriado na caixa de diálogo **Adicionar Nós de Navegação** e clique em **OK**.
3. No painel **Tarefas**, clique em **Novo** e depois em **Grupo**.
4. Insira as informações a seguir em **Grupo** e clique em **OK**:

    -   **Nome do Grupo: grupo1**

5. Clique em **group1** e, no **Painel de Tarefas**, clique em **Propriedades**.
6. Clique em **Membros**, clique em **Adicionar**, digite **test1;test2**e clique em **OK**.

![Introdução aos](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell no</em> centro de administração do AD***

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

```powershell
Add-ADGroupMember -Identity group1 -Member test1
```

#### <a name="to-create-an-organizational-unit"></a>Para criar uma unidade organizacional

1. Clique com o botão direito do mouse no ícone do Windows PowerShell, clique em **Executar como administrador** e digite **dsac. exe** para abrir o ADAC.
2. Clique em **gerenciar**, clique em **adicionar nós de navegação** e selecione o domínio de destino apropriado na caixa de diálogo **adicionar nós de navegação** e clique em * * OK
3. No painel **Tarefas** , clique em **Novo** e depois em **Unidade Organizacional**.
4. Insira as informações a seguir em **Unidade Organizacional** e clique em **OK**:

   - **NameOU1**

![Introdução aos](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell no</em> centro de administração do AD***

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

```powershell
1..2 | ForEach-Object {New-ADUser -SamAccountName test$_ -Name "test$_" -Path "DC=fabrikam,DC=com" -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssword1" -Force) -Enabled $true}
New-ADGroup -Name "group1" -SamAccountName group1 -GroupCategory Security -GroupScope Global -DisplayName "group1"
New-ADOrganizationalUnit -Name OU1 -Path "DC=fabrikam,DC=com"
```

### <a name="bkmk_restore_del_obj"></a>Etapa 4: Restaurar objetos excluídos

Nos procedimentos a seguir, você restaurará objetos excluídos do contêiner **Objetos Excluídos** no local original e em outro local.

#### <a name="to-restore-deleted-objects-to-their-original-location"></a>Para restaurar objetos excluídos no local original

1. Clique com o botão direito do mouse no ícone do Windows PowerShell, clique em **Executar como administrador** e digite **dsac. exe** para abrir o ADAC.

2. Clique em **Gerenciar**, em **Adicionar Nós de Navegação** , selecione o domínio de destino apropriado na caixa de diálogo **Adicionar Nós de Navegação** e clique em **OK**.

3. Selecione os usuários **test1** e **test2**, clique em **Excluir** no **Painel de Tarefas** e clique em **Sim** para confirmar a exclusão.

    ![Introdução aos](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell no</em> centro de administração do AD***

    O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

    ```powershell
    Get-ADUser -Filter 'Name -Like "*test*"'|Remove-ADUser -Confirm:$false
    ```

4. Navegue até o contêiner **Objetos Excluídos** , selecione **test2** e **test1** , e depois clique em **Restaurar** no painel **Tarefas** .

5. Para confirmar se os objetos foram restaurados no local original, navegue até o domínio de destino e verifique se as contas de usuário estão listadas.

    > [!NOTE]
    > Se você navegar para as **Propriedades** das contas de usuário **test1** e **test2** e depois clicar em **Membro de**, verá que as respectivas associações de grupo também foram restauradas.

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

![Introdução aos](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell no</em> centro de administração do AD***

```powershell
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject
```

#### <a name="to-restore-deleted-objects-to-a-different-location"></a>Para restaurar objetos excluídos em outro local

1. Clique com o botão direito do mouse no ícone do Windows PowerShell, clique em **Executar como administrador** e digite **dsac. exe** para abrir o ADAC.

2. Clique em **Gerenciar**, em **Adicionar Nós de Navegação** , selecione o domínio de destino apropriado na caixa de diálogo **Adicionar Nós de Navegação** e clique em **OK**.

3. Selecione os usuários **test1** e **test2**, clique em **Excluir** no **Painel de Tarefas** e clique em **Sim** para confirmar a exclusão.

4. Navegue até o contêiner **Objetos Excluídos**, selecione **test2** e **test1**, e depois clique em **Restaurar em** no painel **Tarefas**.

5. Selecione **OU1** e clique em **OK**.

6. Para confirmar se os objetos foram restaurados em **OU1**, navegue até o domínio de destino, clique duas vezes em **OU1** e verifique se as contas de usuário estão listadas.

![Introdução aos](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell no</em> centro de administração do AD***

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

```powershell
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject -TargetPath "OU=OU1,DC=contoso,DC=com"
```

## <a name="fine_grained_pswd_policy_mgmt"></a>Política de senha refinada

O sistema operacional Windows Server 2008 permite que as organizações definam políticas de senha e de bloqueio de conta diferentes para diferentes conjuntos de usuários em um domínio. Em domínios Active Directory anteriores ao Windows Server 2008, somente uma política de senha e de bloqueio de conta podia ser aplicada a todos os usuários no domínio. Essas políticas eram especificadas na Política de Domínio Padrão do domínio. Como resultado, as organizações que queriam diferentes configurações de senha e de bloqueio de conta para diferentes conjuntos de usuários tinham de criar um filtro de senha ou implantar vários domínios. Ambas as opções são caras.

É possível usar políticas de senha refinada para especificar várias políticas de senha dentro de um único domínio, bem como aplicar diferentes restrições de políticas de senha e de bloqueio de conta a diferentes conjuntos de usuários em um domínio. Por exemplo, você pode aplicar configurações mais estritas a contas privilegiadas e configurações menos estritas às contas de outros usuários. Em outros casos, talvez você deseje aplicar uma política especial de senha a contas cujas senhas sejam sincronizadas com outras fontes de dados. Para obter uma descrição detalhada da política de senha refinada, [consulte AD DS: Políticas de senha refinadas](https://technet.microsoft.com/library/cc770394(WS.10).aspx)

**O que há de novo?**

No Windows Server 2012 e mais recente, o gerenciamento de diretiva de senha refinado é facilitado e mais visual, fornecendo uma interface do usuário para AD DS administradores para gerenciá-los no ADAC. Os administradores agora podem exibir a política resultante de um determinado usuário, exibir e classificar todas as políticas de senha em um determinado domínio e gerenciar políticas de senha individuais visualmente.

Se você planeja usar políticas de senha refinadas no Windows Server 2012, considere o seguinte:

- Políticas de senha refinadas se aplicam somente a grupos de segurança globais e objetos de usuário (ou objetos inetOrgPerson se forem usados em vez de objetos de usuário). Por padrão, somente membros do grupo Administradores de Domínio podem definir políticas de senha refinada. Entretanto, também é possível delegar a capacidade de definir essas políticas a outros usuários. O nível funcional do domínio deve ser Windows Server 2008 ou superior.

- Você deve usar o Windows Server 2012 ou versão mais recente do Centro Administrativo do Active Directory para administrar políticas de senha refinadas por meio de uma interface gráfica do usuário.

    > [!NOTE]
    > Você pode usar **Gerenciador do servidor** para instalar o ferramentas de administração de servidor remoto (RSAT) para usar a versão correta do centro administrativo do Active Directory para gerenciar a lixeira por meio de uma interface do usuário.
    >
    > Para obter informações sobre como instalar o RSAT, consulte o artigo [ferramentas de administração de servidor remoto](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).

### <a name="fine-grained-password-policy-step-by-step"></a>Política de senha refinada passo a passo

Nas etapas a seguir, você usará o ADAC para executar as seguintes tarefas de política de senha refinada:

- [Etapa 1: Aumentar o nível funcional do domínio](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_raise_dfl)
- [Etapa 2: Criar usuários de teste, grupo e unidade organizacional](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk2_test_fgpp)
- [Etapa 3: Criar uma nova política de senha refinada](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)
- [Etapa 4: Exibir um conjunto de políticas resultante para um usuário](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_view_resultant_fgpp)
- [Etapa 5: Editar uma política de senha refinada](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_edit_fgpp)
- [Etapa 6: Excluir uma política de senha refinada](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_delete_fgpp)

> [!NOTE]
> Para executar as etapas a seguir, é necessário ter uma associação ao grupo Administradores de Domínio ou permissões equivalentes.

#### <a name="bkmk_raise_dfl"></a>Etapa 1: Aumentar o nível funcional do domínio

No procedimento a seguir, você aumentará o nível funcional de domínio do domínio de destino para o Windows Server 2008 ou superior. Um nível funcional de domínio do Windows Server 2008 ou superior é necessário para habilitar políticas de senha refinadas.

##### <a name="to-raise-the-domain-functional-level"></a>Para aumentar o nível funcional do domínio

1. Clique com o botão direito do mouse no ícone do Windows PowerShell, clique em **Executar como administrador** e digite **dsac. exe** para abrir o ADAC.

2. Clique em **Gerenciar**, em **Adicionar Nós de Navegação** , selecione o domínio de destino apropriado na caixa de diálogo **Adicionar Nós de Navegação** e clique em **OK**.

3. Clique no domínio de destino no painel de navegação esquerdo e, no **Painel de Tarefas**, clique em **Aumentar nível funcional do domínio**. Selecione um nível funcional de floresta que seja pelo menos o Windows Server 2008 ou superior e clique em **OK**.

![Introdução aos](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell no</em> centro de administração do AD***

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

```powershell
Set-ADDomainMode -Identity contoso.com -DomainMode 3
```

#### <a name="bkmk2_test_fgpp"></a>Etapa 2: Criar usuários, um grupo e uma unidade organizacional de teste

Para criar os usuários de teste e o grupo necessários para esta etapa, siga os procedimentos localizados aqui: [Etapa 3: Criar usuários de teste, grupo e unidade](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_test_env) organizacional (você não precisa criar a UO para demonstrar a política de senha refinada).

#### <a name="bkmk_create_fgpp"></a>Etapa 3: Crie uma nova política de senha refinada

No procedimento a seguir, você criará uma nova política de senha refinada usando a interface do usuário no ADAC.

##### <a name="to-create-a-new-fine-grained-password-policy"></a>Para criar uma nova política de senha refinada

1. Clique com o botão direito do mouse no ícone do Windows PowerShell, clique em **Executar como administrador** e digite **dsac. exe** para abrir o ADAC.

2. Clique em **Gerenciar**, em **Adicionar Nós de Navegação** , selecione o domínio de destino apropriado na caixa de diálogo **Adicionar Nós de Navegação** e clique em **OK**.

3. No painel de navegação do ADAC, abra o contêiner **System** e clique em **Password Settings Container**.

4. No **Painel de Tarefas**, clique em **Novo** e clique em **Configurações de Senha**.

    Preencha ou edite campos da página de propriedades para criar um objeto **Configurações de Senha** . Os campos **Nome** e **Precedência** são obrigatórios.

    ![Introdução ao centro de administração do AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewFGPP.gif)

5. Em **Aplica-se Diretamente a**, em **Adicionar**, digite **group1**e clique em **OK**.

    O objeto Política de Senha será associado aos membros do grupo global que você criou para o ambiente de teste.

6. Clique em **OK** para enviar a criação.

![Introdução aos](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell no</em> centro de administração do AD***

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

```powershell
New-ADFineGrainedPasswordPolicy TestPswd -ComplexityEnabled:$true -LockoutDuration:"00:30:00" -LockoutObservationWindow:"00:30:00" -LockoutThreshold:"0" -MaxPasswordAge:"42.00:00:00" -MinPasswordAge:"1.00:00:00" -MinPasswordLength:"7" -PasswordHistoryCount:"24" -Precedence:"1" -ReversibleEncryptionEnabled:$false -ProtectedFromAccidentalDeletion:$true
Add-ADFineGrainedPasswordPolicySubject TestPswd -Subjects group1
```

#### <a name="bkmk_view_resultant_fgpp"></a>Etapa 4: Exiba um conjunto de políticas resultantes de um usuário

No procedimento a seguir, você exibirá as configurações de senha resultante para um usuário que seja membro do grupo ao qual você atribuiu uma política de senha refinada na [etapa 3: Crie uma nova política](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)de senha refinada.

##### <a name="to-view-a-resultant-set-of-policies-for-a-user"></a>Para exibir um conjunto de políticas resultantes de um usuário

1. Clique com o botão direito do mouse no ícone do Windows PowerShell, clique em **Executar como administrador** e digite **dsac. exe** para abrir o ADAC.

2. Clique em **Gerenciar**, em **Adicionar Nós de Navegação** , selecione o domínio de destino apropriado na caixa de diálogo **Adicionar Nós de Navegação** e clique em **OK**.

3. Selecione um usuário, **Test1** que pertence ao grupo, **grupo1** que você associou uma política de senha refinada com [na etapa 3: Crie uma nova política](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)de senha refinada.

4. Clique em **Exibir Configurações de Senha Resultantes** no **Painel de Tarefas**.

5. Examine a política de configuração de senha e clique em **Cancelar**.

![Introdução aos](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell no</em> centro de administração do AD***

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

```powershell
Get-ADUserResultantPasswordPolicy test1
```

#### <a name="bkmk_edit_fgpp"></a>Etapa 5: Edite uma política de senha refinada

No procedimento a seguir, você editará a política de senha refinada que você [criou na etapa 3: Criar uma nova política de senha refinada](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)

##### <a name="to-edit-a-fine-grained-password-policy"></a>Para editar uma política de senha refinada

1. Clique com o botão direito do mouse no ícone do Windows PowerShell, clique em **Executar como administrador** e digite **dsac. exe** para abrir o ADAC.

2. Clique em **Gerenciar**, em **Adicionar Nós de Navegação** , selecione o domínio de destino apropriado na caixa de diálogo **Adicionar Nós de Navegação** e clique em **OK**.

3. No **Painel de Navegação** do ADAC, expanda **Sistema** e clique em **Contêiner de Configuração de Senha**.

4. Selecione a política de senha refinada que você [criou na etapa 3: Crie uma nova política](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp) de senha refinada e clique em **Propriedades** no painel **tarefas** .

5. Em **Impor histórico de senhas**, altere o valor de **Número de senhas lembradas** para **30**.

6. Clique em **OK**.

![Introdução aos](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell no</em> centro de administração do AD***

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

```powershell
Set-ADFineGrainedPasswordPolicy TestPswd -PasswordHistoryCount:"30"
```

#### <a name="bkmk_delete_fgpp"></a>Etapa 6: Exclua uma política de senha refinada

##### <a name="to-delete-a-fine-grained-password-policy"></a>Para excluir uma política de senha refinada

1. Clique com o botão direito do mouse no ícone do Windows PowerShell, clique em **Executar como administrador** e digite **dsac. exe** para abrir o ADAC.

2. Clique em **Gerenciar**, em **Adicionar Nós de Navegação** , selecione o domínio de destino apropriado na caixa de diálogo **Adicionar Nós de Navegação** e clique em **OK**.

3. No Painel de Navegação do ADAC, expanda **Sistema** e clique em **Contêiner de Configurações de Senha**.

4. Selecione a política de senha refinada que você [criou na etapa 3: Crie uma nova política](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp) de senha refinada e, no painel **tarefas** , clique em **Propriedades**.

5. Desmarque a caixa de seleção **Proteger contra exclusão acidental** e clique em **OK**.

6. Selecione a política de senha refinada e, no painel **Tarefas**, clique em **Excluir**.

7. Clique em **OK** na caixa de diálogo de confirmação.

![Introdução aos](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell no</em> centro de administração do AD***

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

```powershell
Set-ADFineGrainedPasswordPolicy -Identity TestPswd -ProtectedFromAccidentalDeletion $False
Remove-ADFineGrainedPasswordPolicy TestPswd -Confirm
```

## <a name="windows_powershell_history_viewer"></a>Visualizador de histórico do Windows PowerShell

O ADAC é uma ferramenta da interface do usuário que fica na parte superior do Windows PowerShell. No Windows Server 2012 e mais recente, os administradores de ti podem aproveitar o ADAC para conhecer os cmdlets do Windows PowerShell para Active Directory usando o Visualizador de histórico do Windows PowerShell. À medida que as ações são executadas na interface do usuário, o comando do Windows PowerShell equivalente é mostrado para o usuário no Visualizador do Histórico do Windows PowerShell. Isso permite que os administradores criem scripts automatizados, bem como reduz tarefas repetitivas, aumentando a produtividade de TI. Além disso, esse recurso reduz o tempo para aprender o Windows PowerShell por Active Directory e aumenta a confiança dos usuários na exatidão de seus scripts de automação.

Ao usar o Visualizador de histórico do Windows PowerShell no Windows Server 2012 ou mais recente, considere o seguinte:

- Para usar o Visualizador de scripts do Windows PowerShell, você deve usar o Windows Server 2012 ou a versão mais recente do ADAC

    > [!NOTE]
    > Você pode usar **Gerenciador do servidor** para instalar o ferramentas de administração de servidor remoto (RSAT) para usar a versão correta do centro administrativo do Active Directory para gerenciar a lixeira por meio de uma interface do usuário.
    >
    > Para obter informações sobre como instalar o RSAT, consulte o artigo [ferramentas de administração de servidor remoto](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).

- Tenha uma compreensão básica do Windows PowerShell. Por exemplo, é preciso saber como funciona o pipe no Windows PowerShell. Para obter mais informações sobre pipe no Windows PowerShell, consulte [Pipe e pipeline no Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).

### <a name="windows-powershell-history-viewer-step-by-step"></a>Visualizador do Histórico do Windows PowerShell passo a passo

No procedimento a seguir, você usará o Visualizador do Histórico do Windows PowerShell no ADAC para construir um script do Windows PowerShell.  Antes de começar esse procedimento, remova o usuário **test1** do grupo **group1**.

#### <a name="to-construct-a-script-using-powershell-history-viewer"></a>Para construir um script usando o Visualizador do Histórico do PowerShell

1. Clique com o botão direito do mouse no ícone do Windows PowerShell, clique em **Executar como administrador** e digite **dsac. exe** para abrir o ADAC.

2. Clique em **Gerenciar**, em **Adicionar Nós de Navegação** , selecione o domínio de destino apropriado na caixa de diálogo **Adicionar Nós de Navegação** e clique em **OK**.

3. Expanda o painel **Histórico do Windows PowerShell** na parte inferior da tela do ADAC.

4. Selecione o usuário **test1**.

5. Clique em **Adicionar ao grupo...** no painel **tarefas** .

6. Navegue até **group1** e clique em **OK** na caixa de diálogo.

7. Navegue até o painel **Histórico do Windows PowerShell** e localize o comando que acabou de ser gerado.

8. Copie o comando e cole-o no editor desejado para construir seu script.

    Por exemplo, você pode modificar o comando para adicionar outro usuário ao grupo **group1** ou adicionar **test1** a outro grupo.

## <a name="see-also"></a>Consulte também

[Gerenciamento de AD DS avançado usando &#40;o nível de centro administrativo do Active Directory 200&#41;](Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md)
