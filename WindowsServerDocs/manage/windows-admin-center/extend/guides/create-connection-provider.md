---
title: Crie um provedor de conexão para uma extensão de solução
description: Desenvolver uma extensão de solução SDK do Windows Admin Center (Project Honolulu) - criar um provedor de conexão
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 883fba96fcb71cb1c6e8162c1564d66924c4e24d
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081133"
---
# Crie um provedor de conexão para uma extensão de solução

>Aplica-se a: Windows Admin Center, Visualização do Windows Admin Center

Provedores de Conexão desempenham um papel importante em como o Windows Admin Center define e se comunica com objetos conectáveis ou destinos. Basicamente, um provedor de Conexão executa ações enquanto está sendo feita uma conexão como garantir que o destino seja online e disponível e também garantir que o usuário conectado tenha permissão para acessar o destino.

Por padrão, o Windows Admin Center vem com os seguintes provedores de Conexão:

* Servidor
* Cliente do Windows
* Cluster de failover
* Cluster HCI

Para criar seu próprio provedor de Conexão personalizado, siga estas etapas:

* Adicionar detalhes do provedor de Conexão para ```manifest.json```
* Definir o provedor de Status de Conexão
* Implementar o provedor de Conexão na camada de aplicativo

## Adicionar detalhes do provedor de Conexão ao manifest.json

Agora vamos examinar o que você precisa saber para definir um provedor de Conexão em seu projeto ```manifest.json``` arquivo.

### Criar a entrada no manifest.json

O ```manifest.json``` arquivo está localizado na pasta \src e contém, entre outras coisas, definições de pontos de entrada para o seu projeto. Tipos de pontos de entrada incluem ferramentas, soluções e provedores de Conexão. Nós vai ser definindo um provedor de Conexão.

Um exemplo de uma entrada de provedor de Conexão em manifest.json está abaixo:

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

Um ponto de entrada do tipo "connnectionProvider" indica ao shell do Windows Admin Center que o item que está sendo configurado é um provedor que será usado por uma solução para validar um estado de conexão. Pontos de entrada do provedor de Conexão contém um número de propriedades importantes, definido abaixo:

| Propriedade | Descrição |
| -------- | ----------- |
| entryPointType | Isso é uma propriedade necessária. Existem três valores válidos: "ferramenta de", "solução" e "connectionProvider". | 
| name | Identifica o provedor de Conexão no escopo de uma solução. Esse valor deve ser exclusivo dentro de uma instância completa do Windows Admin Center (não apenas uma solução). |
| path | Representa o caminho da URL para o "Adicionar Conexão" da interface do usuário, se ele será configurado pela solução. Esse valor deve mapear para uma rota que está configurada no arquivo de aplicativo routing.module.ts. Quando o ponto de entrada de solução está configurado para usar o rootNavigationBehavior conexões, essa rota carregará o módulo que é usado pelo Shell para exibir a interface do usuário adicionar de Conexão. Mais informações disponíveis na seção sobre rootNavigationBehavior. |
| displayName | O valor inserido aqui é exibido no lado direito do shell, abaixo da barra de Windows Admin Center preto quando um usuário carrega a página de conexões de uma solução. |
| icon | Representa o ícone usado no menu suspenso soluções para representar a solução. |
| description | Insira uma breve descrição do ponto de entrada. |
| connectionType | Representa o tipo de conexão que o provedor será carregado. O valor inserido aqui também será usado no ponto de entrada de solução para especificar que a solução pode carregar essas conexões. O valor inserido aqui também será usado em pontos de entrada de ferramenta para indicar que a ferramenta é compatível com esse tipo. Esse valor inserido aqui também será usado no objeto de conexão que é enviado para o RPC chamar em "Adicionar janela", na etapa de implementação de camada de aplicativo. |
| connectionTypeName | Usada na tabela de conexões para representar uma conexão que usa o provedor de Conexão. Isso deve ser o nome plural do tipo. |
| connectionTypeUrlName | Usado na criação de URL para representar a solução carregada, depois que o Windows Admin Center tem conectado a uma instância. Essa entrada é usada após conexões e antes do destino. Neste exemplo, "connectionexample" é onde esse valor é exibido na URL:http://localhost:6516/solutionexample/connections/connectionexample/con-fake1.corp.contoso.com |
| connectionTypeDefaultSolution | Representa o componente padrão que deve ser carregado pelo provedor de Conexão. Esse valor é uma combinação de: [a] o nome do pacote de extensão definido na parte superior do manifesto; [b] ponto de exclamação (!); [c] nome do ponto de entrada a solução.    Para um projeto com o nome "msft.sme.mySample-extensão" e um ponto de entrada de solução com o nome "exemplo", esse valor seria "extensão msft.sme.solutionExample! exemplo". |
| connectionTypeDefaultTool | Representa o padrão de ferramenta que deve ser carregado em uma conexão bem-sucedida. Esse valor de propriedade é composto de duas partes, semelhantes do connectionTypeDefaultSolution. Esse valor é uma combinação de: [a] o nome do pacote de extensão definido na parte superior do manifesto; [b] ponto de exclamação (!); [c] o nome de ponto de entrada de ferramenta para a ferramenta que deve ser carregado inicialmente. Para um projeto com o nome "msft.sme.solutionExample-extensão" e um ponto de entrada de solução com o nome "exemplo", esse valor seria "extensão msft.sme.solutionExample! exemplo". |
| connectionStatusProvider | Consulte a seção "Definir o provedor de Status de Conexão" |

## Definir o provedor de Status de Conexão

Provedor de Status de Conexão é o mecanismo pelo qual um destino será validado para estar online e disponível, garantindo que o usuário conectado tem permissão para acessar o destino. Atualmente, há dois tipos de provedores de Status de Conexão: PowerShell e RelativeGatewayUrl.

*   Provedor de Status de Conexão do PowerShell
    *   Determina se um destino está online e acessíveis com um script do PowerShell. O resultado deve ser retornado em um objeto com uma única propriedade "status", definido abaixo.
*   Provedor de Status de Conexão RelativeGatewayUrl
    *   Determina se um destino está online e acessíveis com uma chamada rest. O resultado deve ser retornado em um objeto com uma única propriedade "status", definido abaixo.

### Definir o status

Provedores de Status de Conexão são necessárias para retornar um objeto com uma única propriedade ```status``` que está em conformidade com o seguinte formato:

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

* Rótulo
    * Um rótulo que descreve o tipo de retorno de status. Observe que os valores de rótulo podem ser mapeados no tempo de execução. Consulte a entrada abaixo para valores de mapeamento no tempo de execução.

* Tipo
    * O tipo de retorno de status. Tipo tem os seguintes valores de enumeração. Para qualquer valor 2 ou superior, a plataforma não navegará para o objeto conectado, e um erro será exibido na interface do usuário.

Tipos:

| Valor | Descrição |
| ----- | ----------- |
| 0 | Online |
| 1 | Aviso |
| 2 | Não autorizado |
| 3 | Erro |
| 4 | Fatal |
| 5 | Desconhecido |

* Detalhes
    * Detalhes adicionais que descreve o status de tipo de retorno.

### Script de provedor de Status de Conexão do PowerShell

O script do PowerShell de provedor de Status de Conexão determina se um destino está online e acessíveis com um script do PowerShell. O resultado deve ser retornado em um objeto com uma única propriedade "status". Um script de exemplo é mostrado abaixo.

Exemplo de script do PowerShell:

``` ts
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

### Definir o método de provedor de Status de Conexão RelativeGatewayUrl

O provedor de Status da Conexão ```RelativeGatewayUrl``` método chama um rest API para determinar se um destino está online e acessíveis. O resultado deve ser retornado em um objeto com uma única propriedade "status". Um exemplo de entrada do provedor de Conexão no manifest.json de um RelativeGatewayUrl é mostrado abaixo.

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

Observações sobre como usar RelativeGatewayUrl:

* "relativeGatewayUrl" Especifica onde obter o status de conexão de uma URL de gateway. Esse URI é relativo do /api. Se $connectionName for encontrada na URL, ele será substituído pelo nome da conexão.
* Todas as propriedades de relativeGatewayUrl devem ser executadas contra o gateway do host, que pode ser realizado criando uma extensão de gateway

### Valores de mapa em tempo de execução

Os valores de rótulo e detalhes no status de retorno do objeto pode ser formatado em ajustar o tempo, incluindo chaves e valores na propriedade "defaultValueMap" do provedor.

Por exemplo, se você adicionar o valor abaixo, a qualquer momento que "defaultConnection_test" mostrado como um valor de rótulo ou detalhes, o Windows Admin Center será automaticamente substitua a chave com o valor de cadeia de caracteres de recursos configurado.

``` json
    "defaultConnection_test": "resources:strings:addServer_status_defaultConnection_label"
```

## Implementar o provedor de Conexão na camada de aplicativo

Agora, vamos para implementar o provedor de Conexão na camada de aplicativo, criando uma classe TypeScript que implementa OnInit. A classe tem as seguintes funções:

| Função | Descrição |
| -------- | ----------- |
| construtor (appContextService particular: AppContextService, rota particular: ActivatedRoute) |  |
| ngOnInit() pública |  |
| onSubmit() pública | Contém a lógica para atualizar o shell quando é feita uma tentativa de conexão de add |
| onCancel() pública | Contém a lógica para atualizar o shell quando uma tentativa de conexão de add é cancelada |

### Definir onSubmit

```onSubmit``` problemas de uma RPC retorno de chamada para o contexto do aplicativo para notificar o shell de um "Adicionar Conexão". A chamada básica usa "updateData" desta forma:

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

### Definir onCancel

```onCancel``` Cancela uma tentativa de "Adicionar Conexão" passando uma matriz de conexões vazia:

``` ts
this.appContextService.rpc.updateData(EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
```

## Exemplo de provedor de Conexão

A classe TypeScript completa para a implementação de um provedor de conexão está abaixo. Observe que a cadeia de caracteres "connectionType" corresponde a "connectionType conforme definido no provedor da conexão manifest.json.

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
