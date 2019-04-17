---
title: Aplicando o patch de núcleo do servidor
description: Saiba como atualizar uma instalação Server Core do Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: f51ffae5ed8f91cca386eb209e7a1d8cc664ceeb
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1447942"
---
# <a name="patch-a-server-core-installation"></a>Uma instalação Server Core do patch

> Aplica-se a: Windows Server (canal delimitadas anual) e Windows Server 2016

Você pode corrigir um servidor que executa a instalação Server Core das seguintes maneiras:

- **Usando o Windows Update automaticamente ou com o Windows Server Update Services (WSUS)**. Usando o Windows Update, tanto automaticamente ou com ferramentas de linha de comando ou Windows Server Update Services (WSUS), você pode atender a servidores que estejam executando uma instalação Server Core.

- **Manualmente**. Mesmo em organizações que não usam o Windows update ou o WSUS, você pode aplicar atualizações manualmente.

## <a name="view-the-updates-installed-on-your-server-core-server"></a>Exibir as atualizações instaladas no seu servidor de núcleo do servidor
Antes de adicionar uma nova atualização de núcleo do servidor, ele é uma boa ideia para ver quais atualizações já tiverem sido instaladas.

Para exibir atualizações usando o Windows PowerShell, execute **Get-Hotfix**.

Para exibir atualizações executando um comando, execute **systeminfo.exe**. Pode haver um pequeno atraso enquanto a ferramenta inspeciona seu sistema.

Também é possível executar **wmic qfe lista** da linha de comando. 

## <a name="patch-server-core-automatically-with-windows-update"></a>Núcleo do servidor do patch automaticamente com o Windows Update

Use as etapas a seguir para o servidor automaticamente com o Windows Update de patch:

1. Verifique se a configuração atual do Windows Update:
   ```
   %systemroot%\system32\Cscript scregedit.wsf /AU /v 
   ```

2. Para habilitar atualizações automáticas.

   ```
   Net stop wuauserv 
   %systemroot%\system32\Cscript scregedit.wsf /AU 4 
   Net start wuauserv
   ```  

3. Para desabilitar as atualizações automáticas, execute:

   ```
   Net stop wuauserv 
   %systemroot%\system32\Cscript scregedit.wsf /AU 1 
   Net start wuauserv 
   ```

Se o servidor for um membro de um domínio, você também pode configurar o Windows Update usando a diretiva de grupo. Para obter mais informações, consulte https://go.microsoft.com/fwlink/?LinkId=192470. No entanto, quando você usa esse método, única opção 4 ("fazer o download automático e agendar a instalar") é relevante para instalações de núcleo do servidor devido à falta de uma interface gráfica. Para ter mais controle sobre quais atualizações são instaladas e quando, você pode usar um script que fornece um linha de comando equivalente da maioria da interface gráfica do Windows Update. Para obter informações sobre o script, consulte https://go.microsoft.com/fwlink/?LinkId=192471.

Para forçar o Windows Update para detectar imediatamente e instale as atualizações disponíveis, execute o seguinte comando:

```
Wuauclt /detectnow 
```

Dependendo das atualizações que estão instaladas, você pode precisar reiniciar o computador, embora o sistema não notificará você disso. Para determinar se o processo de instalação for concluído, use o Gerenciador de tarefas para verificar se os processos de **Wuauclt** ou **Trusted Installer** ativamente não estão funcionando. Você também pode usar os métodos no [modo de exibição as atualizações instaladas no seu servidor de núcleo do servidor](#view-the-updates-installed-on-your-Server-Core-server) para verificar a lista de atualizações instaladas.

## <a name="patch-the-server-with-wsus"></a>Patch do servidor com o WSUS 

Se o servidor de núcleo do servidor é um membro de um domínio, você poderá configurá-lo para usar um servidor do WSUS com a diretiva de grupo. Para obter mais informações, baixe as [informações de referência de diretiva de grupo](https://www.microsoft.com/download/details.aspx?id=25250). Você também pode revisar as [Configurações de diretiva de grupo Configurar atualizações automáticas](../windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates.md)

## <a name="patch-the-server-manually"></a>O servidor de patch manualmente

Baixe a atualização e disponibilizá-lo para a instalação do núcleo do servidor.
Em um prompt de comando, execute o seguinte comando:

```
Wusa <update>.msu /quiet 
```

Dependendo das atualizações que estão instaladas, você pode precisar reiniciar o computador, embora o sistema não notificará você disso.

Para desinstalar uma atualização manualmente, execute o seguinte comando:

```
Wusa /uninstall <update>.msu /quiet 
```

