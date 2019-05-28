---
title: Como usar o PowerShell em sua extensão
description: Usando o PowerShell em sua extensão Windows Admin Center SDK (projeto Paulo)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 05/09/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 7375732fd464519cd1533043d271065e488fd46a
ms.sourcegitcommit: 7cb939320fa2613b7582163a19727d7b77debe4b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65621361"
---
# <a name="using-powershell-in-your-extension"></a>Como usar o PowerShell em sua extensão #

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Vamos mais detalhada para o SDK do Windows Admin Center extensões – vamos falar sobre como adicionar comandos do PowerShell à sua extensão.

## <a name="powershell-in-typescript"></a>PowerShell no TypeScript ##

O processo de compilação do gulp tem uma etapa de gerar que levarão a qualquer ```{!ScriptName}.ps1``` que é colocado na ```\src\resources\scripts``` pasta e criá-los na ```powershell-scripts``` classe sob o ```\src\generated``` pasta.

>[!NOTE] 
> Não atualizar manualmente a ```powershell-scripts.ts``` nem o ```strings.ts``` arquivos. Qualquer alteração feita será substituída na próxima generate.

## <a name="running-a-powershell-script"></a>Executar um Script do PowerShell ##
Os scripts que você deseja executar em um nó podem ser colocados em ```\src\resources\scripts\{!ScriptName}.ps1```. 
>[!IMPORTANT] 
> Qualquer alteração que faz em uma ```{!ScriptName}.ps1``` arquivo não será refletido no seu projeto até ```gulp generate``` foi executado.

A API funciona criando primeiro uma sessão do PowerShell em nós são de direcionamento, criando o script do PowerShell com os parâmetros que precisam ser passados e, em seguida, executar o script nas sessões que foram criadas.

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
Fornecendo o nome do nó para o método createSession, uma nova sessão do PowerShell é criada, usada e destruída imediatamente após a conclusão da chamada do PowerShell. 

### <a name="key-options"></a>Opções de chave ###
Algumas opções estão disponíveis ao chamar a API do PowerShell. Sempre que uma sessão é criada ele pode ser criado com ou sem uma chave. 

**Chave:** Isso cria uma sessão com chave que pode ser pesquisada e reutilizada, mesmo entre componentes (o que significa que Component1 pode criar uma sessão com a chave "ROCKS SME" e Component2 pode usar essa mesma sessão). Se uma chave for fornecida, a sessão que é criada deve ser descartada por chamada Dispose () como foi feito no exemplo acima. Uma sessão não deve ser mantida sem sendo descartados por mais de 5 minutos. 
```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNode}', '{!Key}');
```

**Sem chave:** Uma chave será criada automaticamente para a sessão. Esta sessão com ser descartados automaticamente após três minutos. O uso sem chave permite que sua extensão reciclar o uso de qualquer espaço de execução que já está disponível no momento da criação de uma sessão. Se nenhum espaço de execução está disponível de um novo será criado. Essa funcionalidade é bom para chamadas individuais, mas o uso repetido pode afetar o desempenho. Uma sessão leva aproximadamente 1 segundo para criar, então continuamente a reciclagem de sessões pode provocar lentidão.

```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNodeName}');
```
ou 
``` ts 
const session = this.appContextService.powerShell.createAutomaticSession('{!TargetNodeName}');
```
Na maioria das situações, criar uma sessão com chave na ```ngOnInit()``` método e, em seguida, descarte-a no ```ngOnDestroy()```. Siga esse padrão quando há vários scripts do PowerShell em um componente, mas a sessão subjacente que não é compartilhado entre componentes.
Para obter melhores resultados, certifique-se de criação da sessão é gerenciada dentro de componentes em vez de serviços - isso ajuda a garantir que esse tempo de vida e a limpeza pode ser gerenciada corretamente.

Para obter melhores resultados, certifique-se de criação da sessão é gerenciada dentro de componentes em vez de serviços - isso ajuda a garantir que esse tempo de vida e a limpeza pode ser gerenciada corretamente.

### <a name="powershell-stream"></a>Stream do PowerShell ###
Se você tiver um script e os dados em tempo de execução é produzida progressivamente, um fluxo de PowerShell permitirá que você processar os dados sem ter de esperar para concluir o script. O observável Next () será chamado, assim como os dados são recebidos.
```ts
this.appContextService.powerShellStream.run(session, script);
```

### <a name="long-running-scripts"></a>Scripts de execução longa ###
Se você tiver um script de execução longa que você deseja executar em segundo plano, um item de trabalho pode ser enviado. O estado do script será rastreado pelo Gateway e atualizações para o status podem ser enviadas a uma notificação. 
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
> Para ver o andamento a serem mostrados, Write-Progress devem ser incluída no script que você tenha escrito. Por exemplo: 
> ``` ps1
>  Write-Progress -Activity ‘The script is almost done!’ -percentComplete 95
>```

#### <a name="workitem-options"></a>Opções de item de trabalho ####

| function | Explicação |
| ----- | ----------- |
| submit() | Envia o item de trabalho 
| submitAndWait() | Enviar o item de trabalho e aguarde a conclusão da execução.
| wait() | Aguarde até que o trabalho existente item seja concluída
| query() | Consulta de um item de trabalho existente por id
| find() | Encontre e existente de item de trabalho pelo TargetNodeName, ModuleName ou typeId.

### <a name="powershell-batch-apis"></a>APIs do lote do PowerShell ###
Se você precisar executar o mesmo script em vários nós, uma sessão do PowerShell em lotes pode ser usada. Por exemplo:
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


#### <a name="powershellbatch-options"></a>Opções de PowerShellBatch ####
| Opção | Explicação |
| ----- | ----------- |
| runSingleCommand | Executar um único comando em todos os nós na matriz 
| executar | Execute o comando correspondente no nó par
| Cancelar | Cancelar o comando em todos os nós na matriz
