---
title: Desabilitar Arquivos Offline em pastas redirecionadas individuais
description: Como desativar o cache de arquivos Offline em pastas individuais que são redirecionadas para compartilhamentos de rede usando o redirecionamento de pasta.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: adc93906cb7ff958fc1db7b00abdc557623e764e
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339304"
---
# Desabilitar Arquivos Offline em pastas redirecionadas individuais

>Aplica-se a: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

Este tópico descreve como desabilitar o cache de arquivos Offline em pastas individuais que são redirecionadas para compartilhamentos de rede usando o redirecionamento de pasta. Isso fornece a capacidade de especificar quais pastas para excluir do armazenamento em cache localmente, reduzindo o cache de arquivos Offline tamanho e o tempo necessário para sincronizar os arquivos Offline.

>[!NOTE]
>Este tópico inclui cmdlets do Windows PowerShell de exemplo que você pode usar para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [Noções básicas do Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/fundamental/windows-powershell-basics?view=powershell-6).

## Pré-requisitos

Para desativar o cache de arquivos Offline de pastas redirecionadas específicas, seu ambiente deve satisfazer os pré-requisitos a seguir.

- Um domínio do Active Directory Domain Services (AD DS), com computadores cliente ingressado no domínio. Não há nenhum requisito de esquema ou requisitos de nível funcional de floresta ou domínio.
- Computadores cliente que executam o Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.
- Um computador com o gerenciamento de política de grupo instalado.

## Desabilitar Arquivos Offline em pastas redirecionadas individuais

Para desabilitar o cache de arquivos Offline de pastas redirecionadas específicas, use a política de grupo para habilitar a configuração de política **não automaticamente tornar pastas redirecionadas específicas disponíveis off-line** para objeto de política de grupo (GPO) apropriado. Definir essa configuração de política como **desabilitado** ou **Não configurado** disponibiliza todas as pastas redirecionadas off-line.

>[!NOTE]
>Apenas os administradores de domínio, os administradores de empresa e membros do grupo de proprietários de criadores de política de grupo podem criar GPOs.

### Desabilitar Arquivos Offline em pastas redirecionadas específicas

1. Abra o **gerenciamento de política de grupo**.
2. Para criar opcionalmente um novo GPO que especifica quais usuários devem ter às pastas redirecionadas excluídas de serem disponibilizados offline, clique com botão direito no domínio apropriado ou unidade organizacional (UO) e selecione **criar um GPO neste domínio e vinculá-lo aqui **.
3. Na árvore de console, clique com botão direito no GPO para o qual você deseja definir as configurações de redirecionamento de pasta e, em seguida, selecione **Editar**. O Editor de gerenciamento de política de grupo é exibida.
4. Na árvore de console, em **Configuração do usuário**, expanda **políticas**, modelos **Administrativos**, expanda **sistema**e expanda **Redirecionamento de pasta**.
5. Clique com botão direito **automaticamente tornar pastas redirecionadas específicas disponíveis off-line** e, em seguida, selecione **Editar**. A janela **automaticamente tornar pastas redirecionadas específicas disponíveis off-line** é exibida.
6. Selecione **Habilitado**. No painel **Opções** , selecione as pastas que devem não fique disponíveis off-line, selecionando as caixas de seleção. Selecione **OK**.

### Comandos equivalentes do Windows PowerShell

O seguinte cmdlet do Windows PowerShell ou cmdlets executar a mesma função que o procedimento descrito [Desabilitando Offline arquivos em pastas redirecionadas individuais](#disabling-offline-files-on-individual-redirected-folders). Insira cada cmdlet em uma única linha, mesmo que eles podem aparecer quebra de várias linhas aqui devido a restrições de formatação.

Este exemplo cria um novo GPO chamado *Configurações de arquivos Offline* na unidade organizacional *MyOu* no domínio *contoso.com* (é o nome distinto LDAP "UO = MyOU, dc = contoso, dc = com"). Em seguida, desativa os arquivos Offline para a pasta de vídeos redirecionados.

```PowerShell
New-GPO -Name "Offline Files Settings" | New-Gplink -Target "ou=MyOu,dc=contoso,dc=com" -LinkEnabled Yes

Set-GPRegistryValue –Name "Offline Files Settings" –Key
"HKCU\Software\Policies\Microsoft\Windows\NetCache\{18989B1D-99B5-455B-841C-AB7C74E4DDFC}" -ValueName DisableFRAdminPinByFolder –Type DWORD –Value 1
```

Consulte a tabela a seguir para obter uma lista de nomes do registro (pasta GUIDs) a ser usado para cada pasta redirecionada.

|Pasta redirecionada|Nome da chave do registro (pasta GUID)|
|---|---|
|AppData (roaming)|{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}|
|Área de trabalho|{B4BFCC3A-DB2C-424C-B029-7FE99A87C641}|
|Menu Iniciar|{625B53C3-AB48-4EC1-BA1F-A1EF4146FC19}|
|Documentos|{FDD39AD0-238F-46AF-ADB4-6C85480369C7}|
|Imagens|{33E28130-4E1E-4676-835A-98395C3BC3BB}|
|Música|{4BD8D571-6D19-48D3-BE97-422220080E43}|
|Vídeos|{18989B1D-99B5-455B-841C-AB7C74E4DDFC}|
|Favoritos|{1777F761-68AD-4D8A-87BD-30B759FA33DD}|
|Contatos|{56784854-C6CB-462b-8169-88E350ACB882}|
|Downloads|{374DE290-123F-4565-9164-39C4925E467B}|
|Links|{BFB9D5E0-C6A9-404C-B2B2-AE6DB6AF4968}|
|Pesquisas|{7D1D3A04-DEBB-4115-95CF-2F29DA2920DA}|
|Jogos salvos|{4C5C32FF-BB9D-43B0-B5B4-2D72E54EAAA4}|

## Mais informações

- [Visão geral de redirecionamento de pasta, arquivos Offline e perfis de usuário móvel](folder-redirection-rup-overview.md)
- [Implanta o redirecionamento de pasta com arquivos Offline](deploy-folder-redirection.md)