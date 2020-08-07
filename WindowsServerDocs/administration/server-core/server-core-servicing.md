---
title: Núcleo do servidor de aplicação de patch
description: Saiba como atualizar uma instalação Server Core do Windows Server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: eb444dd05359f033bec8b45aa9ed53a6ed5096c6
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895853"
---
# <a name="patch-a-server-core-installation"></a>Corrigir uma instalação do Server Core

> Aplica-se a: Windows Server 2019, Windows Server 2016 e Windows Server (canal semestral)

Você pode corrigir um servidor executando a instalação do Server Core das seguintes maneiras:

- **Usar Windows Update automaticamente ou com o Windows Server Update Services (WSUS)**. Usando Windows Update, automaticamente ou com ferramentas de linha de comando ou Windows Server Update Services (WSUS), você pode fazer a manutenção de servidores que executam uma instalação Server Core.

- **Manualmente**. Mesmo em organizações que não usam o Windows Update ou o WSUS, você pode aplicar atualizações manualmente.

## <a name="view-the-updates-installed-on-your-server-core-server"></a>Exibir as atualizações instaladas no servidor Server Core
Antes de adicionar uma nova atualização ao Server Core, é uma boa ideia ver quais atualizações já foram instaladas.

Para exibir as atualizações usando o Windows PowerShell, execute o **Get-Hotfix**.

Para exibir as atualizações executando um comando, execute **systeminfo.exe**. Pode haver um pequeno atraso enquanto a ferramenta inspeciona o sistema.

Você também pode executar a **lista WMIC QFE** na linha de comando.

## <a name="patch-server-core-automatically-with-windows-update"></a>Núcleo do servidor de patch automaticamente com Windows Update

Use as etapas a seguir para corrigir o servidor automaticamente com Windows Update:

1. Verifique a configuração atual de Windows Update:
   ```
   %systemroot%\system32\Cscript %systemroot%\system32\scregedit.wsf /AU /v
   ```

2. Para habilitar as atualizações automáticas:

   ```
   Net stop wuauserv
   %systemroot%\system32\Cscript %systemroot%\system32\scregedit.wsf /AU 4
   Net start wuauserv
   ```

3. Para desabilitar as atualizações automáticas, execute:

   ```
   Net stop wuauserv
   %systemroot%\system32\Cscript %systemroot%\system32\scregedit.wsf /AU 1
   Net start wuauserv
   ```

Se o servidor for membro de um domínio, você também pode configurar o Windows Update usando a Política de Grupo. Para obter mais informações, consulte https://go.microsoft.com/fwlink/?LinkId=192470. No entanto, quando você usa esse método, somente a opção 4 ("baixar automaticamente e agendar a instalação") é relevante para instalações do Server Core devido à falta de uma interface gráfica. Para obter mais controle sobre quais atualizações são instaladas e quando, você pode usar um script que fornece uma linha de comando equivalente da maioria da interface gráfica do Windows Update. Para obter informações sobre o script, consulte https://go.microsoft.com/fwlink/?LinkId=192471 .

Para forçar o Windows Update a detectar imediatamente e instalar todas as atualizações disponíveis, execute o seguinte comando:

```
Wuauclt /detectnow
```

Dependendo das atualizações que são instaladas, pode ser necessário reiniciar o computador, embora o sistema não o notifique sobre isso. Para determinar se o processo de instalação foi concluído, use o Gerenciador de tarefas para verificar se os processos do **wuauclt** ou do **instalador confiável** não estão em execução ativamente. Você também pode usar os métodos em [exibir as atualizações instaladas no servidor Server Core](#view-the-updates-installed-on-your-server-core-server) para verificar a lista de atualizações instaladas.

## <a name="patch-the-server-with-wsus"></a>Corrigir o servidor com o WSUS

Se o servidor Server Core é membro de um domínio, você pode configurá-lo para usar um servidor WSUS com a Política de Grupo. Para obter mais informações, baixe as [informações de referência de política de grupo](https://www.microsoft.com/download/details.aspx?id=25250). Você também pode examinar [definir configurações de política de grupo para atualizações automáticas](../windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates.md)

## <a name="patch-the-server-manually"></a>Corrigir o servidor manualmente

Baixe a atualização e torne-a disponível para a instalação do Server Core.
No prompt de comando, execute o comando a seguir:

```
Wusa <update>.msu /quiet
```

Dependendo das atualizações que são instaladas, pode ser necessário reiniciar o computador, embora o sistema não o notifique sobre isso.

Para desinstalar uma atualização manualmente, execute o seguinte comando:

```
Wusa /uninstall <update>.msu /quiet
```

