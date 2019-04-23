---
title: Como usar o PowerShell em sua extensão
description: Usando o PowerShell em sua extensão Windows Admin Center SDK (projeto Paulo)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ae5004104150c510a56c06161c9280e029968298
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867597"
---
# <a name="using-powershell-in-your-extension"></a>Como usar o PowerShell em sua extensão #

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Vamos mais detalhada para o SDK do Windows Admin Center extensões – vamos falar sobre como adicionar comandos do PowerShell à sua extensão.

## <a name="powershell-in-typescript"></a>PowerShell no TypeScript ##

O processo de compilação do gulp tem uma etapa de gerar que levarão a qualquer ". ps1" que é colocado na pasta "/ src/recursos/scripts" e criá-los na classe "scripts do powershell" sob a pasta "src/gerado".

>[!NOTE] 
> Não atualize manualmente o "powershell-scripts.ts" nem os arquivos de "strings.ts". Qualquer alteração feita será substituída na próxima generate.

### <a name="adding-your-own-powershell-script"></a>Adicionar seu próprio script do PowerShell ##

Vamos adicionar mais informações sobre como adicionar seu próprio script do PowerShell em breve.

### <a name="managing-powershell-sessions"></a>Gerenciando sessões do PowerShell ###

Quando você estiver trabalhando com o PowerShell no Windows Admin Center, as sessões são um componente necessário do processo de execução de script. Para executar scripts em servidores gerenciados remotos, o PowerShell usa espaços de execução. Windows Admin Center encapsula o espaço de execução criação e gerenciamento em um objeto PowerShellSession para gerenciar o tempo de vida e permitir a reutilização de espaço de execução para a execução do script sequencial.

Cada componente precisa criar uma referência a um objeto de sessão é criada pela classe AppContextService usando três opções diferentes:
<!-- I don't 100% get this part - it looks like you're adding 3 arguments - nodeName, <session key>, and <PowerShellSessionRequestOptions>. I got that from looking at the examples, not the text. We need to rework those paras explaining. -->
``` ts
this.psSession = this.appContextService.powerShell.createSession(this.appContextService.activeConnection.nodeName);
```

Fornecendo o nome do nó para o método createSession, uma nova sessão do PowerShell é criada, usada e destruída imediatamente após a conclusão da chamada do PowerShell. Essa funcionalidade é bom para chamadas individuais, mas o uso repetido pode afetar o desempenho. Uma sessão leva aproximadamente 1 segundo para criar, então continuamente a reciclagem de sessões pode provocar lentidão.

``` ts
this.psSession = this.appContextService.powerShell.createSession(this.appContextService.activeConnection.nodeName, '<session key>');
```

O primeiro parâmetro opcional é a \<chave de sessão\> parâmetro. Isso cria uma sessão com chave que pode ser pesquisada e reutilizada, mesmo entre componentes (o que significa que Component1 pode criar uma sessão com a chave "ROCKS SME" e Component2 pode usar essa mesma sessão).  

``` ts
this.psSession = this.appContextService.powerShell.createSession(this.appContextService.activeConnection.nodeName, '<session key>', <PowerShellSessionRequestOptions>);
```
<!-- The second optional parameter is \<PowerShellSessionRequestOptions\> that does ... ? -->
### <a name="common-patterns"></a>Padrões comuns ###

Na maioria das situações, criar uma sessão com chave na **ngOnInit** método e, em seguida, descarte-a em um **ngOnDestroy**. Siga esse padrão quando há vários scripts do PowerShell em um componente, mas a sessão subjacente que não é compartilhado entre componentes.

Para obter melhores resultados, certifique-se de criação da sessão é gerenciada dentro de componentes em vez de serviços - isso ajuda a garantir que esse tempo de vida e a limpeza pode ser gerenciada corretamente.