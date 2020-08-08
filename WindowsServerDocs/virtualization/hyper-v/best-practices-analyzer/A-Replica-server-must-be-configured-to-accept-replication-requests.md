---
title: Um servidor de réplica deve ser configurado para aceitar solicitações de replicação
description: Fornece instruções para resolver o problema relatado por essa regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 027b9df0bad5e37e0a6e2f2d9c44dde1a3e79127
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960739"
---
# <a name="a-replica-server-must-be-configured-to-accept-replication-requests"></a>Um servidor de réplica deve ser configurado para aceitar solicitações de replicação

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, confira [Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Erro|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema
*Este computador é designado como um servidor de réplica do Hyper-V, mas não está configurado para aceitar dados de replicação de entrada de servidores primários.*

## <a name="impact"></a>Impacto
*Este servidor não pode aceitar o tráfego de replicação de servidores primários.*

## <a name="resolution"></a>Resolução
*Use o Gerenciador do Hyper-V para especificar para quais servidores primários este servidor de réplica deve aceitar dados de replicação.*

#### <a name="create-authorization-entries-using-hyper-v-manager"></a>Criar entradas de autorização usando o Gerenciador do Hyper-V

1.  Abra o Gerenciador do Hyper-V. (Em Gerenciador do Servidor, clique em **ferramentas**  >  **Gerenciador do Hyper-V**.)

2.  Na lista de hosts, clique com o botão direito do mouse no que você deseja e clique em **configurações do Hyper-V**.

3.  No painel de navegação, clique em **configuração de replicação**.

4.  Em **autorização e armazenamento**, clique em **permitir replicação dos servidores especificados**.

5.  Abaixo da lista de servidores, clique em **Adicionar**.

6.  Em **Adicionar entrada de autorização**:

    -   Digite o nome totalmente qualificado do primeiro servidor.

    -   Especifique um local dedicado para armazenar somente os arquivos desse servidor.

7.  Clique em **OK**.

8.  Repita para cada servidor primário.

9. Clique em **OK** novamente para concluir e fechar a janela.

### <a name="create-authorization-entries-using-windows-powershell"></a>Criar entradas de autorização usando o Windows PowerShell

1.  Abra o Windows PowerShell. (Na área de trabalho, clique em Iniciar e comece a digitar **Windows PowerShell**.)

2.  Clique com o botão direito do mouse em **Windows PowerShell** e clique em **Executar como administrador**.

3.  Execute um comando semelhante ao seguinte, substituindo:

    -   O nome do servidor primário de server01.domain01.contoso.com com o nome de domínio totalmente qualificado do seu servidor.

    -   O local de D:\ReplicaVMStorage com seu local.

    -   O grupo de confiança chamado padrão com o nome do seu grupo, se você tiver criado um. Caso contrário, use DEFAULT.

```
New-VMReplicationAuthorizationEntry server01.domain01.contoso.com D:\ReplicaVMStorage DEFAULT
```



