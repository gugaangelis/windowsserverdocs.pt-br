---
title: Aplicação de patch do Server Core
description: Saiba como atualizar uma instalação Server Core do Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: b19512a6f34e13469433aba6051f1232824beb0e
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034151"
---
# <a name="patch-a-server-core-installation"></a>Corrigir uma instalação Server Core

> Aplica-se a: Windows Server (canal semestral) e Windows Server 2016

Você pode aplicar o patch de um servidor que executa a instalação do Server Core das seguintes maneiras:

- **Usando o Windows Update automaticamente ou com o Windows Server Update Services (WSUS)** . Usando o Windows Update, automaticamente ou com ferramentas de linha de comando ou o Windows Server Update Services (WSUS), você pode atender a servidores que executam uma instalação Server Core.

- **Manualmente**. Até mesmo em organizações que não usam a atualização do Windows ou o WSUS, você pode aplicar atualizações manualmente.

## <a name="view-the-updates-installed-on-your-server-core-server"></a>Exibir as atualizações instaladas no seu servidor server Core
Antes de adicionar uma nova atualização para o Server Core, é uma boa ideia para ver quais atualizações já foram instaladas.

Para exibir as atualizações usando o Windows PowerShell, execute **Get-Hotfix**.

Para exibir as atualizações ao executar um comando, execute **systeminfo.exe**. Pode haver um pequeno atraso enquanto a ferramenta inspeciona seu sistema.

Você também pode executar **wmic qfe lista** da linha de comando. 

## <a name="patch-server-core-automatically-with-windows-update"></a>Patch do Server Core automaticamente com o Windows Update

Use as seguintes etapas para corrigir o servidor automaticamente com o Windows Update:

1. Verifique se a configuração atual do Windows Update:
   ```
   %systemroot%\system32\Cscript scregedit.wsf /AU /v 
   ```

2. Para habilitar atualizações automáticas:

   ```
   Net stop wuauserv 
   %systemroot%\system32\Cscript scregedit.wsf /AU 4 
   Net start wuauserv
   ```  

3. Para desabilitar atualizações automáticas, execute:

   ```
   Net stop wuauserv 
   %systemroot%\system32\Cscript scregedit.wsf /AU 1 
   Net start wuauserv 
   ```

Se o servidor for membro de um domínio, você também pode configurar o Windows Update usando a Política de Grupo. Para obter mais informações, consulte https://go.microsoft.com/fwlink/?LinkId=192470. No entanto, quando você usa esse método, apenas a opção 4 ("baixar automaticamente e agendar a instalação") é relevante para as instalações Server Core devido à falta de uma interface gráfica. Para obter mais controle sobre quais atualizações são instaladas e quando, você pode usar um script que fornece uma linha de comando equivalente da maioria da interface gráfica do Windows Update. Para obter informações sobre o script, consulte https://go.microsoft.com/fwlink/?LinkId=192471.

Para forçar o Windows Update a detectar imediatamente e instalar todas as atualizações disponíveis, execute o seguinte comando:

```
Wuauclt /detectnow 
```

Dependendo das atualizações que são instaladas, pode ser necessário reiniciar o computador, embora o sistema não o notifique sobre isso. Para determinar se o processo de instalação foi concluído, use o Gerenciador de tarefas para verificar se o **Wuauclt** ou **Trusted Installer** processos não estão funcionando ativamente. Você também pode usar os métodos [exibir as atualizações instaladas no seu servidor server Core](#view-the-updates-installed-on-your-server-core-server) para verificar a lista de atualizações instaladas.

## <a name="patch-the-server-with-wsus"></a>Patch do servidor com o WSUS 

Se o servidor Server Core é membro de um domínio, você pode configurá-lo para usar um servidor WSUS com a Política de Grupo. Para obter mais informações, baixe o [informações de referência de política de grupo](https://www.microsoft.com/download/details.aspx?id=25250). Você também pode examinar [configurar configurações de diretiva de grupo para atualizações automáticas](../windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates.md)

## <a name="patch-the-server-manually"></a>Patch do servidor manualmente

Baixe a atualização e disponibilizá-lo para a instalação Server Core.
Em um prompt de comando, execute o seguinte comando:

```
Wusa <update>.msu /quiet 
```

Dependendo das atualizações que são instaladas, pode ser necessário reiniciar o computador, embora o sistema não o notifique sobre isso.

Para desinstalar uma atualização manualmente, execute o seguinte comando:

```
Wusa /uninstall <update>.msu /quiet 
```

