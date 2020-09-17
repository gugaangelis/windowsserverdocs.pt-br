---
title: Os servidores de réplica devem ser configurados para identificar servidores primários específicos autorizados a enviar tráfego de replicação
description: Fornece instruções para resolver o problema relatado por essa regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 0aeb1f4b-2e75-430b-9557-fe64738c4992
ms.date: 8/16/2016
ms.openlocfilehash: 68953fe3efaba64c853e4da83d4ca47ff13ca00a
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90745811"
---
# <a name="replica-servers-should-be-configured-to-identify-specific-primary-servers-authorized-to-send-replication-traffic"></a>Os servidores de réplica devem ser configurados para identificar servidores primários específicos autorizados a enviar tráfego de replicação

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, confira [Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema
*Conforme configurado, esse servidor de réplica aceita o tráfego de replicação de todos os servidores primários e os armazena em um único local.*

### <a name="impact"></a>Impacto
*Toda a replicação de todos os servidores primários é armazenada em um local, o que pode introduzir problemas de privacidade ou segurança.*

## <a name="resolution"></a>Resolução
*Use o Gerenciador do Hyper-V para criar novas entradas de autorização para os servidores primários específicos e especificar locais de armazenamento separados para cada um deles. Você pode usar caracteres curinga para agrupar servidores primários em conjuntos para cada entrada de autorização.*

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

## <a name="see-also"></a>Consulte Também
[New-VMReplicationAuthorizationEntry](/powershell/module/hyper-v/new-vmreplicationauthorizationentry?view=win10-ps)