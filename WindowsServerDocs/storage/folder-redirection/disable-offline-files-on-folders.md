---
title: Desabilitar Arquivos Offline em pastas redirecionadas individuais
description: Como desabilitar o cache de arquivos Offline em pastas individuais são redirecionadas para compartilhamentos de rede usando o redirecionamento de pasta.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: b006742c9256c357d9aff3fb1b765dbed087383a
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475877"
---
# <a name="disable-offline-files-on-individual-redirected-folders"></a>Desabilitar Arquivos Offline em pastas redirecionadas individuais

>Aplica-se a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows (canal semestral)

Este tópico descreve como desabilitar o cache de arquivos Offline em pastas individuais são redirecionadas para compartilhamentos de rede usando o redirecionamento de pasta. Isso fornece a capacidade de especificar quais pastas a serem excluídos do armazenamento em cache localmente, reduzindo o cache de arquivos off-line tamanho e o tempo necessário para sincronizar arquivos Offline.

>[!NOTE]
>Este tópico inclui cmdlets do Windows PowerShell de exemplo que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [Noções básicas do Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/fundamental/windows-powershell-basics?view=powershell-6).

## <a name="prerequisites"></a>Pré-requisitos

Para desabilitar o cache de arquivos off-line de pastas redirecionadas específicas, seu ambiente deve cumprir os pré-requisitos a seguir.

- Um domínio de serviços de domínio Active Directory (AD DS), com os computadores cliente ingressados no domínio. Não há nenhum requisito de esquema ou requisitos de nível funcional de floresta ou domínio.
- Computadores de cliente que executam o Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows (canal semestral).
- Um computador com o gerenciamento de diretiva de grupo instalado.

## <a name="disabling-offline-files-on-individual-redirected-folders"></a>Desabilitar Arquivos Offline em pastas redirecionadas individuais

Para desabilitar o cache de arquivos off-line de pastas redirecionadas específicas, usar a diretiva de grupo para habilitar o **disponibilizar autom. pastas redirecionadas específicas offline** configuração de política para o apropriada grupo de política de GPO (objeto). Definir essa configuração de política como **desabilitados** ou **não configurado** disponibiliza todas as pastas redirecionadas off-line.

>[!NOTE]
>Somente os administradores de domínio, administradores de empresa e os membros do grupo de proprietários de criadores de diretiva de grupo podem criar GPOs.

### <a name="to-disable-offline-files-on-specific-redirected-folders"></a>Desabilitar Arquivos Offline em pastas redirecionadas específicas

1. Abra **gerenciamento de diretiva de grupo**.
2. Para opcionalmente criar um novo GPO que especifica quais usuários devem ter redirecionado pastas excluídas do que está sendo disponibilizado offline, clique no domínio apropriado ou a unidade organizacional (UO) e, em seguida, selecione **criar um GPO neste domínio e vincular IT aqui**.
3. Na árvore de console, clique com botão direito no GPO para o qual você deseja definir as configurações de redirecionamento de pasta e, em seguida, selecione **editar**. O Editor de gerenciamento de diretiva de grupo é exibida.
4. Na árvore de console, sob **configuração do usuário**, expanda **diretivas**, expanda **modelos administrativos**, expanda **sistema**, e expandir **redirecionamento de pasta**.
5. Clique com botão direito **disponibilizar autom. pastas redirecionadas específicas offline** e, em seguida, selecione **editar**. O **disponibilizar autom. pastas redirecionadas específicas offline** janela é exibida.
6. Selecione **Habilitado**. No **opções** painel selecione as pastas que não devem ficar disponíveis offline marcando as caixas de seleção apropriadas. Selecione **OK**.

### <a name="windows-powershell-equivalent-commands"></a>Comandos equivalentes do Windows PowerShell

O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam a mesma função conforme o procedimento descrito em [desabilitar arquivos Offline em pastas redirecionadas individuais](#disabling-offline-files-on-individual-redirected-folders). Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

Este exemplo cria um novo GPO chamado *configurações de arquivos off-line* na *MyOu* unidade organizacional no *contoso.com* domínio (o nome distinto LDAP é "UO = MyOU, dc = a Contoso, dc = com "). Em seguida, desabilita a arquivos Offline para essa pasta redirecionada.

```PowerShell
New-GPO -Name "Offline Files Settings" | New-Gplink -Target "ou=MyOu,dc=contoso,dc=com" -LinkEnabled Yes

Set-GPRegistryValue –Name "Offline Files Settings" –Key
"HKCU\Software\Policies\Microsoft\Windows\NetCache\{18989B1D-99B5-455B-841C-AB7C74E4DDFC}" -ValueName DisableFRAdminPinByFolder –Type DWORD –Value 1
```

Consulte a tabela a seguir para obter uma lista de nomes de chave do registro (GUIDs de pasta) a ser usado para cada pasta redirecionada.

|Pasta redirecionada|Nome da chave do registro (GUID da pasta)|
|---|---|
|AppData(Roaming)|{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}|
|Área de Trabalho|{B4BFCC3A-DB2C-424C-B029-7FE99A87C641}|
|Menu Iniciar|{625B53C3-AB48-4EC1-BA1F-A1EF4146FC19}|
|Documentos|{FDD39AD0-238F-46AF-ADB4-6C85480369C7}|
|Imagens|{33E28130-4E1E-4676-835A-98395C3BC3BB}|
|Música|{4BD8D571-6D19-48D3-BE97-422220080E43}|
|Vídeos|{18989B1D-99B5-455B-841C-AB7C74E4DDFC}|
|Favoritos|{1777F761-68AD-4D8A-87BD-30B759FA33DD}|
|Contatos|{56784854-C6CB-462b-8169-88E350ACB882}|
|Downloads|{374DE290-123F-4565-9164-39C4925E467B}|
|Links|{BFB9D5E0-C6A9-404C-B2B2-AE6DB6AF4968}|
|Searches|{7D1D3A04-DEBB-4115-95CF-2F29DA2920DA}|
|Saved Games|{4C5C32FF-BB9D-43B0-B5B4-2D72E54EAAA4}|

## <a name="more-information"></a>Mais informações

- [Visão geral do redirecionamento de pasta, arquivos Offline e perfis de usuário móvel](folder-redirection-rup-overview.md)
- [Implantar o redirecionamento de pasta com arquivos Offline](deploy-folder-redirection.md)