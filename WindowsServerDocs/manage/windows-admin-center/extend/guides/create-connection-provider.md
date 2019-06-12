---
title: Criar um provedor de conexão para uma extensão da solução
description: Desenvolver uma extensão da solução (projeto Paulo) do SDK do Windows Admin Center - criar um provedor de conexão
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: b79e832ee45990d18baf4c211ab68b907134ceb7
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811840"
---
# <a name="create-a-connection-provider-for-a-solution-extension"></a>Criar um provedor de conexão para uma extensão da solução

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Provedores de Conexão desempenham um papel importante na maneira como o Windows Admin Center define e se comunica com os objetos conectáveis ou destinos. Basicamente, um provedor de Conexão executa ações enquanto está sendo feita uma conexão, como garantir que o destino está online e disponível e também garantir que o usuário conectado tem permissão para acessar o destino.

Por padrão, o Windows Admin Center é fornecido com os seguintes provedores de Conexão:

* Servidor
* Windows Client
* Cluster de failover
* Cluster HCI

Para criar seu próprio provedor de Conexão personalizada, siga estas etapas:

* Adicionar detalhes do provedor de Conexão para ```manifest.json```
* Definir provedor de Status de Conexão
* Implementar o provedor de Conexão na camada de aplicativo

## <a name="add-connection-provider-details-to-manifestjson"></a>Adicionar detalhes do provedor de Conexão ao manifest. JSON

Agora vamos examinar o que você precisa saber para definir um provedor de Conexão em seu projeto ```manifest.json``` arquivo.

### <a name="create-entry-in-manifestjson"></a>Criar entrada no manifest. JSON

O ```manifest.json``` arquivo está localizado na pasta \src. e contém, entre outras coisas, definições de pontos de entrada em seu projeto. Tipos de pontos de entrada incluem ferramentas, soluções e provedores de Conexão. Podemos estarei definindo um provedor de Conexão.

Um exemplo de uma entrada de provedor de Conexão no manifest. JSON está abaixo:

``` json
    {
      "entryPointType": "connectionProvider",
      "name": "addServer",
      "path": "/add",
      "displayName": "resources:strings:addServer_displayName",
      "icon": "sme-icon:icon-win-server",
      "description": "resources:strings:description",
      "connectionType": "msft.sme.connection-type.server",
      "connectionTypeName": "resources:strings:addServer_connectionTypeName",
      "connectionTypeUrlName": "server",
      "connectionTypeDefaultSolution": "msft.sme.server-manager!servers",
      "connectionTypeDefaultTool": "msft.sme.server-manager!overview",
      "connectionStatusProvider": {
        "powerShell": {
          "script": "## Get-My-Status ##\nfunction Get-Status()\n{\n# A function like this would be where logic would exist to identify if a node is connectable.\n$status = @{label = $null; type = 0; details = $null; }\n$caption = \"MyConstCaption\"\n$productType = \"MyProductType\"\n# A result object needs to conform to the following object structure to be interpreted properly by the Windows Admin Center shell.\n$result = @{ status = $status; caption = $caption; productType = $productType; version = $version }\n# DO FANCY LOGIC #\n# Once the logic is complete, the following fields need to be populated:\n$status.label = \"Display Thing\"\n$status.type = 0 # This value needs to conform to the LiveConnectionStatusType enum. >= 3 represents a failure.\n$status.details = \"success stuff\"\nreturn $result}\nGet-Status"
        },
        "displayValueMap": {
          "wmfMissing-label": "resources:strings:addServer_status_wmfMissing_label",
          "wmfMissing-details": "resources:strings:addServer_status_wmfMissing_details",
          "unsupported-label": "resources:strings:addServer_status_unsupported_label",
          "unsupported-details": "resources:strings:addServer_status_unsupported_details"
        }
      }
    },
```

Um ponto de entrada do tipo "connnectionProvider" indica para o shell do Windows Admin Center que o item que está sendo configurado é um provedor que será usado por uma solução para validar um estado de conexão. Pontos de entrada do provedor de Conexão contém um número de propriedades importantes, como definido abaixo:

| Propriedade | Descrição |
| -------- | ----------- |
| entryPointType | Esta é uma propriedade necessária. Há três valores válidos: "tool", "solução" e "connectionProvider". | 
| name | Identifica o provedor de Conexão dentro do escopo de uma solução. Esse valor deve ser exclusivo dentro de uma instância completa do Windows Admin Center (não apenas uma solução). |
| path | Representa o caminho da URL para o "Adicionar Conexão" da interface do usuário, se ele estará configurado pela solução. Esse valor deve mapear para uma rota que é configurada no arquivo de aplicativo routing.module.ts. Quando o ponto de entrada de solução é configurado para usar o rootNavigationBehavior conexões, essa rota carregará o módulo que é usado pelo Shell para exibir a interface do usuário adicionar de Conexão. Mais informações disponíveis na seção sobre rootNavigationBehavior. |
| displayName | O valor digitado aqui é exibido no lado direito do shell, abaixo da barra preta do Windows Admin Center quando um usuário carrega a página de conexões da solução. |
| ícone | Representa o ícone usado no menu suspenso de soluções para representar a solução. |
| description | Insira uma breve descrição do ponto de entrada. |
| connectionType | Representa o tipo de conexão que o provedor será carregado. O valor digitado aqui também será usado no ponto de entrada de solução para especificar que a solução pode carregar essas conexões. O valor digitado aqui também será usado no ponto (s) de entrada de ferramenta para indicar que a ferramenta é compatível com esse tipo. Esse valor digitado aqui também será usado no objeto de conexão que é enviado para o RPC chamar em "Adicionar janela", na etapa de implementação de camada de aplicativo. |
| connectionTypeName | Usado na tabela de conexões para representar uma conexão que usa o provedor de Conexão. Isso é esperado para ser o nome no plural do tipo. |
| connectionTypeUrlName | Usado na criação de URL para representar a solução carregada, depois que o Windows Admin Center se conectou a uma instância. Essa entrada é usada depois de conexões e antes do destino. Neste exemplo, "connectionexample" é onde esse valor aparece na URL: `http://localhost:6516/solutionexample/connections/connectionexample/con-fake1.corp.contoso.com` |
| connectionTypeDefaultSolution | Representa o componente padrão que deve ser carregado pelo provedor de Conexão. Esse valor é uma combinação de: <br>[a] o nome do pacote de extensão definido na parte superior do manifesto; <br>[b] ponto de exclamação (!); <br>[c] nome do ponto de entrada a solução.    <br>Para um projeto com "msft.sme.mySample-extensão de nome" e um ponto de entrada de solução com o nome "exemplo", esse valor seria "extensão msft.sme.solutionExample! exemplo". |
| connectionTypeDefaultTool | Representa o padrão de ferramenta que deve ser carregado em uma conexão bem-sucedida. Esse valor da propriedade é composto de duas partes, semelhantes ao connectionTypeDefaultSolution. Esse valor é uma combinação de: <br>[a] o nome do pacote de extensão definido na parte superior do manifesto; <br>[b] ponto de exclamação (!); <br>[c] o nome de ponto de entrada de ferramenta para a ferramenta que deve ser carregado inicialmente. <br>Para um projeto com "msft.sme.solutionExample-extensão de nome" e um ponto de entrada de solução com o nome "exemplo", esse valor seria "extensão msft.sme.solutionExample! exemplo". |
| connectionStatusProvider | Consulte a seção "Definir o provedor de Status de Conexão" |

## <a name="define-connection-status-provider"></a>Definir provedor de Status de Conexão

Provedor de Status de Conexão é o mecanismo pelo qual um destino é validado para estar online e disponível, garantindo que o usuário conectado tem permissão para acessar o destino. Atualmente, há dois tipos de provedores de Status de Conexão:  PowerShell e RelativeGatewayUrl.

*   <strong>Provedor de Status de Conexão do PowerShell</strong> -determina se um destino está online e acessível com um script do PowerShell. O resultado deve ser retornado em um objeto com uma única propriedade "status" definida abaixo.
*   <strong>Provedor de Status de Conexão RelativeGatewayUrl</strong> -determina se um destino está online e acessível com uma chamada rest. O resultado deve ser retornado em um objeto com uma única propriedade "status" definida abaixo.

### <a name="define-status"></a>Definir status

Provedores de Status de Conexão são necessárias para retornar um objeto com uma única propriedade ```status``` que esteja de acordo com o seguinte formato:

``` json
{
    status: {
        label: string;
        type: int;
        details: string;
    }
}
```

Propriedades de status:

* <strong>Rótulo</strong> – um rótulo que descreve o tipo de retorno de status. Observe que os valores para o rótulo podem ser mapeados no tempo de execução. Consulte a entrada abaixo para mapear valores no tempo de execução.

* <strong>Tipo</strong> -o status do tipo de retorno. Tipo tem os seguintes valores de enumeração. Para qualquer valor 2 ou superior, a plataforma não navegará para o objeto conectado, e um erro será exibido na interface do usuário.

   Tipos:

  | Valor | Descrição |
  | ----- | ----------- |
  | 0 | Online |
  | 1 | Aviso |
  | 2 | Não autorizado |
  | 3 | Erro |
  | 4 | Fatal |
  | 5 | Desconhecido |

* <strong>Detalhes</strong> – detalhes adicionais que descrevem o tipo de retorno de status.

### <a name="powershell-connection-status-provider-script"></a>Script de provedor de Status de Conexão do PowerShell

O script do PowerShell de provedor de Status de Conexão determina se um destino está online e acessível com um script do PowerShell. O resultado deve ser retornado em um objeto com uma única propriedade "status". Abaixo está um exemplo de script.

Exemplo de script do PowerShell:

```PowerShell
## Get-My-Status ##

function Get-Status()
{
    # A function like this would be where logic would exist to identify if a node is connectable.
    $status = @{label = $null; type = 0; details = $null; }
    $caption = "MyConstCaption"
    $productType = "MyProductType"

    # A result object needs to conform to the following object structure to be interperated properly by the Windows Admin Center shell.
    $result = @{ status = $status; caption = $caption; productType = $productType; version = $version }

    # DO FANCY LOGIC #

    # Once the logic is complete, the following fields need to be populated:
    $status.label = "Display Thing"
    $status.type = 0 # This value needs to conform to the LiveConnectionStatusType enum. >= 3 represents a failure.
    $status.details = "success stuff"

    return $result
}

Get-Status
```

### <a name="define-relativegatewayurl-connection-status-provider-method"></a>Definir método de provedor de Status de Conexão RelativeGatewayUrl

O provedor de Status de Conexão ```RelativeGatewayUrl``` método chama uma rest API para determinar se um destino está online e acessível. O resultado deve ser retornado em um objeto com uma única propriedade "status". Abaixo está um exemplo de entrada do provedor de Conexão no manifest. JSON de um RelativeGatewayUrl.

``` json
    {
      "entryPointType": "connectionProvider",
      "name": "addServer",
      "path": "/add/server",
      "displayName": "resources:strings:addServer_displayName",
      "icon": "sme-icon:icon-win-server",
      "description": "resources:strings:description",
      "connectionType": "msft.sme.connection-type.server",
      "connectionTypeName": "resources:strings:addServer_connectionTypeName",
      "connectionTypeUrlName": "server",
      "connectionTypeDefaultSolution": "msft.sme.server-manager!servers",
      "connectionTypeDefaultTool": "msft.sme.server-manager!overview",
      "connectionStatusProvider": {
        "relativeGatewayUrl": "<URL here post /api>",
        "displayValueMap": {
          "wmfMissing-label": "resources:strings:addServer_status_wmfMissing_label",
          "wmfMissing-details": "resources:strings:addServer_status_wmfMissing_details",
          "unsupported-label": "resources:strings:addServer_status_unsupported_label",
          "unsupported-details": "resources:strings:addServer_status_unsupported_details"
        }
      }
    },
```

Observações sobre o uso de RelativeGatewayUrl:

* "relativeGatewayUrl" Especifica onde obter o status de conexão de uma URL de gateway. Esse URI é relativo da/API. Se $connectionName for encontrado na URL, ele será substituído pelo nome da conexão.
* Todas as propriedades de relativeGatewayUrl devem ser executadas no gateway de host, que pode ser feito criando uma extensão de gateway

### <a name="map-values-in-runtime"></a>Mapear valores no tempo de execução

Os valores de rótulo e os detalhes no status do objeto de retorno pode ser formatado em ajustar o tempo, incluindo chaves e valores na propriedade "defaultValueMap" do provedor.

Por exemplo, se você adicionar o valor abaixo, sempre que "defaultConnection_test" apareceu como um valor para o rótulo ou obter detalhes, Windows Admin Center substituirá automaticamente a chave com o valor de cadeia de caracteres de recurso configurado.

``` json
    "defaultConnection_test": "resources:strings:addServer_status_defaultConnection_label"
```

## <a name="implement-connection-provider-in-application-layer"></a>Implementar o provedor de Conexão na camada de aplicativo

Agora, vamos implementar o provedor de Conexão na camada de aplicativo, criando uma classe do TypeScript que implementa o OnInit. A classe tem as seguintes funções:

| Função | Descrição |
| -------- | ----------- |
| Constructor(Private appContextService: AppContextService, rota privada: ActivatedRoute) |  |
| public ngOnInit() |  |
| onSubmit() pública | Contém a lógica para atualizar o shell quando é feita uma tentativa de conexão de adicionar |
| onCancel() pública | Contém a lógica para atualizar o shell quando é cancelada uma tentativa de conexão de adicionar |

### <a name="define-onsubmit"></a>Definir onSubmit

```onSubmit``` problemas de uma RPC retorno de chamada para o contexto de aplicativo para notificar o shell de um "Adicionar Conexão". A chamada básica usa "updateData" como esta:

``` ts
this.appContextService.rpc.updateData(
    EnvironmentModule.nameOfShell,
    '##',
    <RpcUpdateData>{
        results: {
            connections: connections,
            credentials: this.useCredentials ? this.creds : null
        }
    }
);
```

O resultado é uma propriedade de conexão, que é uma matriz de objetos que estão em conformidade com a seguinte estrutura:

``` ts

/**
 * The connection attributes class.
 */
export interface ConnectionAttribute {

    /**
     * The id string of this attribute
     */
    id: string;

    /**
     * The value of the attribute. used for attributes that can have variable values such as Operating System
     */
    value?: string | number;
}

/**
 * The connection class.
 */
export interface Connection {

    /**
     * The id of the connection, this is unique per connection
     */
    id: string;

    /**
     * The type of connection
     */
    type: string;

    /**
     * The name of the connection, this is unique per connection type
     */
    name: string;

    /**
     * The property bag of the connection
     */
    properties?: ConnectionProperties;

    /**
     * The ids of attributes identified for this connection
     */
    attributes?: ConnectionAttribute[];

    /**
     * The tags the user(s) have assigned to this connection
     */
    tags?: string[];
}

/**
 * Defines connection type strings known by core
 * Be careful that these strings match what is defined by the manifest of @msft-sme/server-manager
 */
export const connectionTypeConstants = {
    server: 'msft.sme.connection-type.server',
    cluster: 'msft.sme.connection-type.cluster',
    hyperConvergedCluster: 'msft.sme.connection-type.hyper-converged-cluster',
    windowsClient: 'msft.sme.connection-type.windows-client',
    clusterNodesProperty: 'nodes'
};
```

### <a name="define-oncancel"></a>Definir onCancel

```onCancel``` Cancela uma tentativa de "Adicionar Conexão", passando uma matriz vazia de conexões:

``` ts
this.appContextService.rpc.updateData(EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
```

## <a name="connection-provider-example"></a>Exemplo de provedor de Conexão

A classe completa de TypeScript para implementar um provedor de conexão está abaixo. Observe que a cadeia de caracteres "connectionType" corresponde a "connectionType conforme definido no provedor de conexão no manifest. JSON.

``` ts
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { AppContextService } from '@msft-sme/shell/angular';
import { Connection, ConnectionUtility } from '@msft-sme/shell/core';
import { EnvironmentModule } from '@msft-sme/shell/dist/core/manifest/environment-modules';
import { RpcUpdateData } from '@msft-sme/shell/dist/core/rpc/rpc-base';
import { Strings } from '../../generated/strings';

@Component({
  selector: 'add-example',
  templateUrl: './add-example.component.html',
  styleUrls: ['./add-example.component.css']
})
export class AddExampleComponent implements OnInit {
  public newConnectionName: string;
  public strings = MsftSme.resourcesStrings<Strings>().SolutionExample;
  private connectionType = 'msft.sme.connection-type.example'; // This needs to match the connectionTypes value used in the manifest.json.
  
  constructor(private appContextService: AppContextService, private route: ActivatedRoute) {
    // TODO:
  }

  public ngOnInit() {
    // TODO
  }

  public onSubmit() {
    let connections: Connection[] = [];

    let connection = <Connection> {
      id: ConnectionUtility.createConnectionId(this.connectionType, this.newConnectionName),
      type: this.connectionType,
      name: this.newConnectionName
    };

    connections.push(connection);

    this.appContextService.rpc.updateData(
      EnvironmentModule.nameOfShell,
      '##',
      <RpcUpdateData> {
        results: {
          connections: connections,
          credentials: null
        }
      }
    );
  }

  public onCancel() {
    this.appContextService.rpc.updateData(
      EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
  }
}

```
