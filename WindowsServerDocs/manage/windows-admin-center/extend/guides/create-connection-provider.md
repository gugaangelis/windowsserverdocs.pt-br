---
title: Criar um provedor de conexão para uma extensão de solução
description: Desenvolver uma extensão de solução SDK do centro de administração do Windows (projeto Honolulu) – criar um provedor de conexão
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: ec11b8a6b9129348ec2405548c21fa9d6ec5deff
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952674"
---
# <a name="create-a-connection-provider-for-a-solution-extension"></a>Criar um provedor de conexão para uma extensão de solução

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Os provedores de conexão desempenham um papel importante no modo como o centro de administração do Windows define e se comunica com objetos conectáveis ou destinos. Principalmente, um provedor de conexão executa ações enquanto uma conexão está sendo feita, como garantir que o destino esteja online e disponível, e também garantir que o usuário que está se conectando tenha permissão para acessar o destino.

Por padrão, o centro de administração do Windows é fornecido com os seguintes provedores de conexão:

* Servidor
* Windows Client
* Cluster de failover
* Cluster HCI

Para criar seu próprio provedor de conexão personalizado, siga estas etapas:

* Adicionar detalhes do provedor de conexão a```manifest.json```
* Definir provedor de status de conexão
* Implementar o provedor de conexão na camada de aplicativo

## <a name="add-connection-provider-details-to-manifestjson"></a>Adicionar detalhes do provedor de conexão a manifest.jsem

Agora vamos examinar o que você precisa saber para definir um provedor de conexão no arquivo do seu projeto ```manifest.json``` .

### <a name="create-entry-in-manifestjson"></a>Criar entrada no manifest.jsem

O ```manifest.json``` arquivo está localizado na pasta \src e contém, entre outras coisas, definições de pontos de entrada em seu projeto. Os tipos de pontos de entrada incluem ferramentas, soluções e provedores de conexão. Vamos definir um provedor de conexão.

Um exemplo de uma entrada de provedor de conexão no manifest.jsno está abaixo:

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

Um ponto de entrada do tipo "connnectionProvider" indica para o Shell do centro de administração do Windows que o item que está sendo configurado é um provedor que será usado por uma solução para validar um estado de conexão. Os pontos de entrada do provedor de conexão contêm várias propriedades importantes, definidas abaixo:

| Propriedade | Descrição |
| -------- | ----------- |
| entryPointType | Esta é uma propriedade obrigatória. Há três valores válidos: "ferramenta", "solução" e "ConnectionProvider". |
| name | Identifica o provedor de conexão dentro do escopo de uma solução. Esse valor deve ser exclusivo dentro de uma instância completa do centro de administração do Windows (não apenas uma solução). |
| caminho | Representa o caminho da URL para a interface do usuário "Adicionar conexão", caso ela seja configurada pela solução. Esse valor deve mapear para uma rota configurada no arquivo de roteamento de aplicativo. Module. TS. Quando o ponto de entrada da solução estiver configurado para usar as conexões rootNavigationBehavior, essa rota carregará o módulo usado pelo shell para exibir a interface do usuário Adicionar conexão. Mais informações estão disponíveis na seção sobre rootNavigationBehavior. |
| displayName | O valor digitado aqui é exibido no lado direito do Shell, abaixo da barra preta do centro de administração do Windows quando um usuário carrega a página de conexões de uma solução. |
| ícone | Representa o ícone usado no menu suspenso soluções para representar a solução. |
| descrição | Insira uma breve descrição do ponto de entrada. |
| connectionType | Representa o tipo de conexão que o provedor carregará. O valor inserido aqui também será usado no ponto de entrada da solução para especificar que a solução pode carregar essas conexões. O valor inserido aqui também será usado em ponto (s) de entrada de ferramenta para indicar que a ferramenta é compatível com esse tipo. Esse valor inserido aqui também será usado no objeto de conexão que é enviado para a chamada RPC em "Adicionar janela", na etapa de implementação da camada de aplicativo. |
| ConnectionName | Usado na tabela de conexões para representar uma conexão que usa seu provedor de conexão. Espera-se que seja o nome do plural do tipo. |
| connectionTypeUrlName | Usado na criação da URL para representar a solução carregada, depois que o centro de administração do Windows tiver se conectado a uma instância do. Essa entrada é usada após as conexões e antes do destino. Neste exemplo, "connectionexample" é onde esse valor aparece na URL:`http://localhost:6516/solutionexample/connections/connectionexample/con-fake1.corp.contoso.com` |
| connectionTypeDefaultSolution | Representa o componente padrão que deve ser carregado pelo provedor de conexão. Esse valor é uma combinação de: <br>[a] o nome do pacote de extensão definido na parte superior do manifesto; <br>[b] ponto de exclamação (!); <br>[c] o nome do ponto de entrada da solução.    <br>Para um projeto com o nome "MSFT. SME. MySample-Extension" e um ponto de entrada de solução com o nome "example", esse valor seria "MSFT. SME. solutionExample-Extension! example". |
| connectionTypeDefaultTool | Representa a ferramenta padrão que deve ser carregada em uma conexão bem-sucedida. Esse valor de propriedade é composto de duas partes, semelhante ao connectionTypeDefaultSolution. Esse valor é uma combinação de: <br>[a] o nome do pacote de extensão definido na parte superior do manifesto; <br>[b] ponto de exclamação (!); <br>[c] o nome do ponto de entrada da ferramenta para a ferramenta que deve ser carregada inicialmente. <br>Para um projeto com o nome "MSFT. SME. solutionExample-Extension" e um ponto de entrada de solução com o nome "example", esse valor seria "MSFT. SME. solutionExample-Extension! example". |
| connectionStatusProvider | Consulte a seção "definir provedor de status de conexão" |

## <a name="define-connection-status-provider"></a>Definir provedor de status de conexão

O provedor de status de conexão é o mecanismo pelo qual um destino é validado para estar online e disponível, garantindo também que o usuário que está se conectando tenha permissão para acessar o destino. Atualmente, há dois tipos de provedores de status de conexão: PowerShell e RelativeGatewayUrl.

*   <strong>Provedor de status de conexão do PowerShell</strong> – determina se um destino está online e acessível com um script do PowerShell. O resultado deve ser retornado em um objeto com uma única propriedade "status", definida abaixo.
*   <strong>Provedor de status de conexão do RelativeGatewayUrl</strong> – determina se um destino está online e acessível com uma chamada REST. O resultado deve ser retornado em um objeto com uma única propriedade "status", definida abaixo.

### <a name="define-status"></a>Definir status

Provedores de status de conexão são necessários para retornar um objeto com uma única propriedade ```status``` que esteja de acordo com o seguinte formato:

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

* <strong>Rótulo</strong> -um rótulo que descreve o tipo de retorno de status. Observe que os valores para rótulo podem ser mapeados em tempo de execução. Consulte a entrada abaixo para mapear valores em tempo de execução.

* <strong>Tipo</strong> -o tipo de retorno de status. O tipo tem os seguintes valores de enumeração. Para qualquer valor 2 ou superior, a plataforma não navegará até o objeto conectado e um erro será exibido na interface do usuário.

   Digita

  | Valor | Descrição |
  | ----- | ----------- |
  | 0 | Online |
  | 1 | Aviso |
  | 2 | Não Autorizado |
  | 3 | Erro |
  | 4 | Fatais |
  | 5 | Unknown |

* <strong>Detalhes</strong> -detalhes adicionais que descrevem o tipo de retorno de status.

### <a name="powershell-connection-status-provider-script"></a>Script do provedor de status de conexão do PowerShell

O script do PowerShell do provedor de status de conexão determina se um destino está online e acessível com um script do PowerShell. O resultado deve ser retornado em um objeto com uma única propriedade "status". Um exemplo de script é mostrado abaixo.

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

### <a name="define-relativegatewayurl-connection-status-provider-method"></a>Definir o método do provedor de status de conexão RelativeGatewayUrl

O método de provedor de status de conexão ```RelativeGatewayUrl``` chama uma API REST para determinar se um destino está online e acessível. O resultado deve ser retornado em um objeto com uma única propriedade "status". Uma entrada de provedor de conexão de exemplo no manifest.jsde um RelativeGatewayUrl é mostrada abaixo.

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

Observações sobre como usar o RelativeGatewayUrl:

* "relativeGatewayUrl" especifica onde obter o status da conexão de uma URL de gateway. Esse URI é relativo de/API. Se $connectionName for encontrado na URL, ele será substituído pelo nome da conexão.
* Todas as propriedades relativeGatewayUrl devem ser executadas no gateway de host, o que pode ser feito pela criação de uma extensão de gateway

### <a name="map-values-in-runtime"></a>Mapear valores em tempo de execução

Os valores de rótulo e detalhes no objeto de retorno de status podem ser formatados em tempo de ajuste, incluindo chaves e valores na propriedade "defaultValueMap" do provedor.

Por exemplo, se você adicionar o valor abaixo, sempre que "defaultConnection_test" for mostrado como um valor para o rótulo ou detalhes, o centro de administração do Windows substituirá automaticamente a chave pelo valor da cadeia de caracteres do recurso configurado.

``` json
    "defaultConnection_test": "resources:strings:addServer_status_defaultConnection_label"
```

## <a name="implement-connection-provider-in-application-layer"></a>Implementar o provedor de conexão na camada de aplicativo

Agora vamos implementar o provedor de conexão na camada de aplicativo, criando uma classe TypeScript que implementa OnInit. A classe tem as seguintes funções:

| Função | Descrição |
| -------- | ----------- |
| Construtor (particular appContextService: AppContextService, rota privada: ActivatedRoute) |  |
| ngOnInit público () |  |
| público-enviado por envio () | Contém a lógica para atualizar o shell quando uma tentativa de Adicionar conexão é feita |
| público OnCancel () | Contém a lógica para atualizar o shell quando uma tentativa de Adicionar conexão é cancelada |

### <a name="define-onsubmit"></a>Definir onsubmit

```onSubmit```emite uma chamada RPC de volta para o contexto do aplicativo para notificar o Shell de uma "Adicionar conexão". A chamada básica usa "updateData" como esta:

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

### <a name="define-oncancel"></a>Definir OnCancel

```onCancel```Cancela uma tentativa de "Adicionar conexão" passando uma matriz de conexões vazia:

``` ts
this.appContextService.rpc.updateData(EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
```

## <a name="connection-provider-example"></a>Exemplo de provedor de conexão

A classe TypeScript completa para implementar um provedor de conexão está abaixo. Observe que a cadeia de caracteres "ConnectionType" corresponde ao "ConnectionType conforme definido no provedor de conexão no manifest.jsem.

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
