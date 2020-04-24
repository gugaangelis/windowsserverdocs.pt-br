---
title: Desabilitar Arquivos Offline em pastas redirecionadas individuais
description: Como desabilitar o armazenamento em cache de Arquivos Offline em pastas individuais redirecionadas para compartilhamentos de rede usando o Redirecionamento de Pastas.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: c2614c0180b32a0215454f2d725d6a962986ef1f
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "71394397"
---
# <a name="disable-offline-files-on-individual-redirected-folders"></a>Desabilitar Arquivos Offline em pastas redirecionadas individuais

>Aplica-se a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows (Canal Semestral)

Este tópico descreve como desabilitar o armazenamento em cache de Arquivos Offline em pastas individuais redirecionadas para compartilhamentos de rede usando o Redirecionamento de Pastas. Isso permite especificar quais pastas devem ser excluídas do cache localmente, reduzindo o tamanho do cache Arquivos Offline e o tempo necessário para sincronizar Arquivos Offline.

>[!NOTE]
>Este tópico inclui cmdlets do Windows PowerShell de exemplo que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, confira [Noções básicas sobre o Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/fundamental/windows-powershell-basics?view=powershell-6).

## <a name="prerequisites"></a>Pré-requisitos

Para desabilitar o armazenamento em cache de Arquivos Offline de pastas redirecionadas específicas, o ambiente deve cumprir os pré-requisitos a seguir.

- Um domínio AD DS (Active Directory Domain Services) com computadores cliente ingressados no domínio. Não há requisitos em nível funcional de domínio ou floresta nem requisitos de esquema.
- Computadores cliente executando o Windows 10, o Windows 8.1, o Windows 8, o Windows Server 2019, o Windows Server 2016, o Windows Server 2012 R2, o Windows Server 2012 ou o Windows (Canal Semestral).
- Um computador com Gerenciamento de Política de Grupo instalado.

## <a name="disabling-offline-files-on-individual-redirected-folders"></a>Como desabilitar Arquivos Offline em pastas redirecionadas individuais

Para desabilitar Arquivos Offline cache de pastas redirecionadas específicas, use Política de Grupo para habilitar a configuração de política **Não disponibilizar automaticamente pastas redirecionadas específicas offline** para o GPO (Objeto de Política de Grupo) apropriado. Definir essa configuração de política como **Desabilitada** ou **Não configurada** disponibiliza todas as pastas redirecionadas offline.

>[!NOTE]
>Somente administradores de domínio, administradores corporativos e membros do grupo de proprietários do criador da Política de Grupo podem criar GPOs.

### <a name="to-disable-offline-files-on-specific-redirected-folders"></a>Para desabilitar Arquivos Offline em pastas redirecionadas específicas

1. Abra **Gerenciamento de Política de Grupo**.
2. Para, opcionalmente, criar um GPO que especifica quais usuários devem ter pastas redirecionadas excluídas da disponibilidade offline, clique com o botão direito do mouse no domínio ou na UO (unidade organizacional) apropriada e, em seguida, selecione **Criar um GPO neste domínio e vinculá-lo aqui**.
3. Na árvore de console, clique com o botão direito do mouse no GPO para o qual você deseja definir as configurações de Redirecionamento de Pastas e, em seguida, selecione **Editar**. O Editor de Gerenciamento de Política de Grupo é exibido.
4. Na árvore de console, em **Configuração do Usuário**, expanda **Políticas**, expanda **Modelos Administrativos**, expanda **Sistema** e expanda **Redirecionamento de Pastas**.
5. Clique com o botão direito do mouse em **Não disponibilizar automaticamente pastas redirecionadas específicas offline** e, em seguida, selecione **Editar**. A janela **Não disponibilizar automaticamente pastas redirecionadas específicas offline** é exibida.
6. Selecione **Habilitado**. No painel **Opções**, selecione as pastas que não devem ser disponibilizadas offline marcando as caixas de seleção apropriadas. Selecione **OK**.

### <a name="windows-powershell-equivalent-commands"></a>Comandos equivalentes do Windows PowerShell

Os cmdlets a seguir do Windows PowerShell executam a mesma função que o procedimento descrito em [Como desabilitar Arquivos Offline em pastas redirecionadas individuais](#disabling-offline-files-on-individual-redirected-folders). Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

Este exemplo cria um GPO chamado *Configurações de Arquivos Offline* na unidade organizacional *MyOu* no domínio *contoso.com* (o nome diferenciado LDAP é "ou=MyOU,dc=contoso,dc=com"). Em seguida, ele desabilita Arquivos Offline para a pasta redirecionada de Vídeos.

```PowerShell
New-GPO -Name "Offline Files Settings" | New-Gplink -Target "ou=MyOu,dc=contoso,dc=com" -LinkEnabled Yes

Set-GPRegistryValue –Name "Offline Files Settings" –Key
"HKCU\Software\Policies\Microsoft\Windows\NetCache\{18989B1D-99B5-455B-841C-AB7C74E4DDFC}" -ValueName DisableFRAdminPinByFolder –Type DWORD –Value 1
```

Confira a tabela a seguir para obter uma lista de nomes de chave do Registro (GUIDs de pasta) a serem usados para cada pasta redirecionada.

|Pasta redirecionada|Nome da chave do Registro (GUID da pasta)|
|---|---|
|AppData(Roaming)|{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}|
|Desktop|{B4BFCC3A-DB2C-424C-B029-7FE99A87C641}|
|Menu Iniciar|{625B53C3-AB48-4EC1-BA1F-A1EF4146FC19}|
|Documentos|{FDD39AD0-238F-46AF-ADB4-6C85480369C7}|
|Imagens|{33E28130-4E1E-4676-835A-98395C3BC3BB}|
|Música|{4BD8D571-6D19-48D3-BE97-422220080E43}|
|Vídeos|{18989B1D-99B5-455B-841C-AB7C74E4DDFC}|
|Favorites|{1777F761-68AD-4D8A-87BD-30B759FA33DD}|
|Contacts|{56784854-C6CB-462b-8169-88E350ACB882}|
|Downloads|{374DE290-123F-4565-9164-39C4925E467B}|
|Links|{BFB9D5E0-C6A9-404C-B2B2-AE6DB6AF4968}|
|Searches|{7D1D3A04-DEBB-4115-95CF-2F29DA2920DA}|
|Saved Games|{4C5C32FF-BB9D-43B0-B5B4-2D72E54EAAA4}|

## <a name="more-information"></a>Mais informações

- [Visão geral de Redirecionamento de Pastas, Arquivos Offline e Perfis de Usuários Móveis](folder-redirection-rup-overview.md)
- [Implantar Redirecionamento de Pastas com Arquivos Offline](deploy-folder-redirection.md)