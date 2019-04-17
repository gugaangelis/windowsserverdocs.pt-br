---
title: Como usar o PowerShell em sua extensão
description: Usando o PowerShell em sua extensão do SDK do Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: b1be4fe7639d913243cc28371dff9e98e0f5827e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296718"
---
# Como usar o PowerShell em sua extensão #

>Aplicável à: Windows Admin Center, a visualização do Windows Admin Center

Vamos mais detalhada para o SDK do Windows Admin Center extensões – vamos falar sobre como adicionar comandos do PowerShell para sua extensão.

## PowerShell no TypeScript ##

O processo de compilação gulp tem uma etapa de gerar que levará qualquer ```{!ScriptName}.ps1``` que é colocado no ```\src\resources\scripts``` pasta e compile-las para o ```powershell-scripts``` classe sob o ```\src\generated``` pasta.

>[!NOTE] 
> Não atualizar manualmente o ```powershell-scripts.ts``` nem o ```strings.ts``` arquivos. Qualquer alteração feita será substituída na próxima generate.

## Executar um Script Powerhell ##
Os scripts que você deseja executar em um nó podem ser colocados em ```\src\resources\scripts\{!ScriptName}.ps1```. 
>[!IMPORTANT] 
> Fazer alterações em um ```{!ScriptName}.ps1``` arquivo não será refletido no seu projeto, a menos que um gerar 

A API funciona criando primeiro uma sessão do PowerShell em nós são direcionamento, criar o script do PowerShell com os parâmetros que precisam ser passados e, em seguida, executar o script em sessões que foram criadas.

Por exemplo, temos que esse script ```\src\resources\scripts\Get-NodeName.ps1```:
``` ps1
Param
 (
    [String] $stringFormat
 )
 $nodeName = [string]::Format($stringFormat,$env:COMPUTERNAME)
 Write-Output $nodeName
```

Vamos criar uma sessão do PowerShell para nosso nó de destino:
``` ts
const session = this.appContextService.powerShell.createSession('{!TargetNode}'); 
```
Em seguida, vamos criar o script do PowerShell com um parâmetro de entrada:
```ts
const script = PowerShell.createScript(PowerShellScripts.Get_NodeName, {stringFormat: 'The name of the node is {0}!'});
```
Por fim, precisamos executar esse script na sessão que criamos:
``` ts
  public ngOnInit(): void {
    this.session = this.appContextService.powerShell.createAutomaticSession('{!TargetNode}');
  }

  public getNodeName(): Observable<any> {
    const script = PowerShell.createScript(PowerShellScripts.Get_NodeName.script, { stringFormat: 'The name of the node is {0}!'});
    return this.appContextService.powerShell.run(this.session, script)
    .pipe(
        map(
        response => {
            if (response && response.results) {
                return response.results;
            }
            return 'no response';
        }
      ) 
    );
  }

  public ngOnDestroy(): void {
    this.session.dispose()
  }

```
Agora precisamos inscrever-se para a função observável que acabamos de criar. Coloque isso em que você precisa chamar a função para executar o script do PowerShell:
```ts
this.getNodeName().subscribe(
     response => {
    console.log(response)
     }
);
```
Ao fornecer o nome do nó para o método createSession, uma nova sessão do PowerShell é criada, usada e destruída imediatamente após a conclusão da chamada do PowerShell. 

### Opções de chave ###
Algumas opções estão disponíveis ao chamar a API do PowerShell. Sempre que for criada uma sessão ele pode ser criado com ou sem uma chave. 

**Chave:** Isso cria uma sessão de chave que pode ser pesquisada e reutilizada, até mesmo em componentes (o que significa que Component1 pode criar uma sessão com a chave "EA-ROCHAS", e Component2 podem usar essa mesma sessão). Se uma chave for fornecida, a sessão que é criada deve ser descartada de pela chamada Dispose () como foi feito no exemplo acima. Uma sessão não deve ser mantida sem sendo descartadas por mais de 5 minutos. 
```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNode}', '{!Key}');
```

**Sem chave:** Uma chave será criada automaticamente para a sessão. Esta sessão com ser descartados automaticamente após 3 minutos. Usando sem chave permite que sua extensão reciclar o uso de qualquer Runspace que já está disponível no momento da criação de uma sessão. Se nenhum Runspace está disponível de um novo será criado. Essa funcionalidade é bom para chamadas únicas, mas o uso repetido pode afetar o desempenho. Uma sessão leva aproximadamente 1 segundo para criar, portanto continuamente reciclagem sessões pode causar gargalos.

```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNodeName}');
```
ou 
``` ts 
const session = this.appContextService.powerShell.createAutomaticSession('{!TargetNodeName}');
```
Na maioria das situações, criar uma sessão inserida no ```ngOnInit()``` método e, em seguida, descarte-em ```ngOnDestroy()```. Seguem esse padrão quando houver vários scripts do PowerShell em um componente, mas a sessão subjacente que não é compartilhado entre componentes.
Para obter melhores resultados, certifique-se de criação de sessão é gerenciada dentro de componentes, em vez de serviços - isso ajuda a garantir que esse tempo de vida e limpeza pode ser gerenciada corretamente.

Para obter melhores resultados, certifique-se de criação de sessão é gerenciada dentro de componentes, em vez de serviços - isso ajuda a garantir que esse tempo de vida e limpeza pode ser gerenciada corretamente.

### Fluxo do PowerShell ###
Se você tiver um script de longa execução e dados é saída progressivamente, um fluxo de PowerShell permitirá que você processar os dados sem precisar aguardar o script concluir. O observável Next () será chamado assim que os dados são recebidos.
```ts
this.appContextService.powerShellStream.run(session, script);
```

### Scripts de execução longa ###
Se você tiver um script de longa duração que você deseja executar em segundo plano, um item de trabalho pode ser enviado. O estado do script será controlado pelo Gateway e atualizações para o status podem ser enviadas para uma notificação. 
```ts
const workItem: WorkItemSubmitRequest = {
    typeId: 'Long Running Script',
    objectName: 'My long running service',
    powerShellScript: script,
    
    //in progress notifications
    inProgressTitle: 'Executing long running request',
    startedMessage: 'The long running request has been started',
    progressMessage: 'Working on long running script – {{ percent }} %',

    //success notification
    successTitle: 'Successfully executed a long running script!',
    successMessage: '{{objectName}} was successful',
    successLinkText: 'Bing',
    successLink: 'http://www.bing.com',
    successLinkType: NotificationLinkType.Absolute,

    //error notification
    errorTitle: 'Failed to execute long running script',
    errorMessage: 'Error: {{ message }}'

    nodeRequestOptions: {
       logAudit: true,
       logTelemetry: true
    }
};

return this.appContextService.workItem.submit('{!TargetNode}', workItem);
```

>[!NOTE] 
> De progresso seja mostrado, Write-Progress devem ser incluído no script que você escreveu. Por exemplo:
> ``` ps1
>  Write-Progress -Activity ‘The script is almost done!’ -percentComplete 95
>```

#### Opções de item de trabalho ####

| function | Explicação |
| ----- | ----------- |
| Submit() | Envia o item de trabalho 
| submitAndWait() | Enviar um item de trabalho e aguardar a conclusão de sua execução
| wait() | Aguarde até que o trabalho existente item para concluir
| Query) | Consulta de um item de trabalho existente por id
| Find() | Encontre e existente do item de trabalho pelo TargetNodeName, módulo ou identificação de tipo.

### PowerShell lote APIs # # #
Se você precisar executar o script mesmo em vários nós, uma sessão do PowerShell em lotes pode ser usada. Por exemplo:
```ts
const batchSession = this.appContextService.powerShell.createBatchSession(
    ['{!TargetNode1}', '{!TargetNode2}', sessionKey);
  this.appContextService.powerShell.runBatchSingleCommand(batchSession, command).subscribe((responses: PowerShellBatchResponseItem[]) => {
    for (const response of responses) { 
      if (response.error || response.errors) {
        //handle error
      } else {
        const results = response.properties && response.properties.results;
        //response.nodeName
        //results[0]
      }
    }
     },
     Error => { /* handle error */ });  

```


#### Opções de PowerShellBatch ####
| opção | Explicação |
| ----- | ----------- |
| runSingleCommand | Executar um único comando contra todos os nós na matriz 
| executar | Execute o comando correspondente no nó emparelhado
| Cancelar | Cancelar o comando em todos os nós na matriz