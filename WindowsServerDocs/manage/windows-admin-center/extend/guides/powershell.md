---
title: Como usar o PowerShell em sua extensão
description: Usando o PowerShell em sua extensão SDK do centro de administração do Windows (projeto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 05/09/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 6e99fc43d4acb7a70dfd3a8ba19dae6492c41b2b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357045"
---
# <a name="using-powershell-in-your-extension"></a>Como usar o PowerShell em sua extensão #

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Vamos aprofundar-se no SDK de extensões do centro de administração do Windows – vamos falar sobre como adicionar comandos do PowerShell à sua extensão.

## <a name="powershell-in-typescript"></a>PowerShell no TypeScript ##

O processo de compilação Gulp tem uma etapa de geração ```{!ScriptName}.ps1``` que levará qualquer um deles ```\src\resources\scripts``` na pasta e os ```powershell-scripts``` criará na classe sob a ```\src\generated``` pasta.

>[!NOTE] 
> Não atualize ```powershell-scripts.ts``` manualmente nem os ```strings.ts``` arquivos. Qualquer alteração feita será substituída na próxima geração.

## <a name="running-a-powershell-script"></a>Executando um script do PowerShell ##
Todos os scripts que você deseja executar em um nó podem ser colocados no ```\src\resources\scripts\{!ScriptName}.ps1```. 
>[!IMPORTANT] 
> As alterações feitas em um ```{!ScriptName}.ps1``` arquivo não serão refletidas no projeto até que ```gulp generate``` o tenha sido executado.

A API funciona primeiro criando uma sessão do PowerShell nos nós que você está direcionando, criando o script do PowerShell com quaisquer parâmetros que precisam ser passados e, em seguida, executando o script nas sessões que foram criadas.

Por exemplo, temos este script ```\src\resources\scripts\Get-NodeName.ps1```:
``` ps1
Param
 (
    [String] $stringFormat
 )
 $nodeName = [string]::Format($stringFormat,$env:COMPUTERNAME)
 Write-Output $nodeName
```

Criaremos uma sessão do PowerShell para nosso nó de destino:
``` ts
const session = this.appContextService.powerShell.createSession('{!TargetNode}'); 
```
Em seguida, criaremos o script do PowerShell com um parâmetro de entrada:
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
Agora, será necessário assinar a função observável que acabamos de criar. Coloque isso onde você precisa chamar a função para executar o script do PowerShell:
```ts
this.getNodeName().subscribe(
     response => {
    console.log(response)
     }
);
```
Ao fornecer o nome do nó para o método CreateSession, uma nova sessão do PowerShell é criada, usada e, em seguida, destruída imediatamente após a conclusão da chamada do PowerShell. 

### <a name="key-options"></a>Opções de chave ###
Algumas opções estão disponíveis ao chamar a API do PowerShell. Cada vez que uma sessão é criada, ela pode ser criada com ou sem uma chave. 

**Chave:** Isso cria uma sessão com chave que pode ser pesquisada e reutilizada, mesmo entre componentes (o que significa que Component1 pode criar uma sessão com a chave "SME-ROCKS", e Component2 pode usar essa mesma sessão). Se uma chave for fornecida, a sessão criada deverá ser descartada chamando Dispose () como foi feito no exemplo acima. Uma sessão não deve ser mantida sem ser descartada por mais de 5 minutos. 
```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNode}', '{!Key}');
```

**Sem chave** Uma chave será criada automaticamente para a sessão. Esta sessão com ser descartada automaticamente após 3 minutos. Usar a configuração de menos permite que sua extensão recicle o uso de qualquer runspace que já esteja disponível no momento da criação de uma sessão. Se nenhum runspace estiver disponível do que um novo, ele será criado. Essa funcionalidade é válida para chamadas únicos, mas o uso repetido pode afetar o desempenho. Uma sessão leva aproximadamente 1 segundo para ser criada, portanto, A reciclagem contínua de sessões pode causar lentidão.

```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNodeName}');
```
ou 
``` ts 
const session = this.appContextService.powerShell.createAutomaticSession('{!TargetNodeName}');
```
Na maioria das situações, crie uma sessão com chave ```ngOnInit()``` no método e, em seguida, descarte ```ngOnDestroy()```-a em. Siga esse padrão quando houver vários scripts do PowerShell em um componente, mas a sessão subjacente não for compartilhada entre componentes.
Para obter melhores resultados, certifique-se de que a criação da sessão seja gerenciada dentro de componentes em vez de serviços – isso ajuda a garantir que o tempo de vida e a limpeza possam ser gerenciados corretamente.

Para obter melhores resultados, certifique-se de que a criação da sessão seja gerenciada dentro de componentes em vez de serviços – isso ajuda a garantir que o tempo de vida e a limpeza possam ser gerenciados corretamente.

### <a name="powershell-stream"></a>Fluxo do PowerShell ###
Se você tiver um script de execução longa e os dados forem enviados progressivamente, um fluxo do PowerShell permitirá que você processe os dados sem precisar aguardar a conclusão do script. O observável Next () será chamado assim que os dados forem recebidos.
```ts
this.appContextService.powerShellStream.run(session, script);
```

### <a name="long-running-scripts"></a>Scripts de longa execução ###
Se você tiver um script de longa execução que deseja executar em segundo plano, um item de trabalho poderá ser enviado. O estado do script será acompanhado pelo gateway e as atualizações para o status poderão ser enviadas para uma notificação. 
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
> Para que o progresso seja mostrado, o Write-Progress deve ser incluído no script que você escreveu. Por exemplo:
> ``` ps1
>  Write-Progress -Activity ‘The script is almost done!' -percentComplete 95
>```

#### <a name="workitem-options"></a>Opções de item de trabalho ####

| function | Explicação |
| ----- | ----------- |
| Enviar () | Envia o item de trabalho 
| submitAndWait() | Enviar o item de trabalho e aguardar a conclusão de sua execução
| aguardar () | Aguardar a conclusão do item de trabalho existente
| consulta () | Consultar um item de trabalho existente por ID
| Localizar () | Localize um item de trabalho existente por TargetNodeName, ModuleName ou typeId.

### <a name="powershell-batch-apis"></a>APIs do lote do PowerShell ###
Se você precisar executar o mesmo script em vários nós, uma sessão do PowerShell do lote poderá ser usada. Por exemplo:
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
| Option | Explicação |
| ----- | ----------- |
| runSingleCommand | Executar um único comando em todos os nós na matriz 
| Funcionam | Executar comando correspondente no nó emparelhado
| cancelar | Cancelar o comando em todos os nós na matriz
