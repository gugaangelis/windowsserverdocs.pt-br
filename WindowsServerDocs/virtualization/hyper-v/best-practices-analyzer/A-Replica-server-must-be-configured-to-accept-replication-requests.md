---
title: Um servidor de réplica deve ser configurado para aceitar solicitações de replicação
description: Fornece instruções para resolver o problema relatado por essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c5e30ddc50b176b83db081a29c6427356ab946c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827517"
---
# <a name="a-replica-server-must-be-configured-to-accept-replication-requests"></a>Um servidor de réplica deve ser configurado para aceitar solicitações de replicação

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Erro|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.
  
## <a name="issue"></a>Problema  
*Este computador é designado como um servidor de réplica do Hyper-V, mas não está configurado para aceitar dados de replicação de entrada de servidores primários.*  
  
## <a name="impact"></a>Impacto  
*Este servidor não pode aceitar o tráfego de replicação de servidores primários.*  
  
## <a name="resolution"></a>Resolução  
*Use o Gerenciador do Hyper-V para especificar quais servidores primários este servidor de réplica deve aceitar dados de replicação do.*  
  
#### <a name="create-authorization-entries-using-hyper-v-manager"></a>Criar entradas de autorização usando o Gerenciador do Hyper-V  
  
1.  Abra o Gerenciador Hyper-V. (No Gerenciador do servidor, clique em **ferramentas** > **Gerenciador Hyper-V**.)  
  
2.  Na lista de hosts, clique com botão direito aquele que você deseja e clique **configurações do Hyper-V**.  
  
3.  No painel de navegação, clique em **configuração de replicação**.  
  
4.  Sob **autorização e armazenamento**, clique em **permitem que a replicação dos servidores especificados**.  
  
5.  Abaixo da lista de servidores, clique em **adicionar**.  
  
6.  Sob **Adicionar entrada de autorização**:  
  
    -   Digite o nome totalmente qualificado do servidor primeiro.  
  
    -   Especifique um local dedicado para armazenar apenas os arquivos desse servidor.  
  
7.  Clique em **OK**.  
  
8.  Repita para cada servidor primário.  
  
9. Clique em **Okey** novamente para concluir e fechar a janela.  
  
### <a name="create-authorization-entries-using-windows-powershell"></a>Criar entradas de autorização usando o Windows PowerShell  
  
1.  Abra o Windows PowerShell. (Na área de trabalho, clique em Iniciar e comece a digitar **Windows PowerShell**.)  
  
2.  Clique com botão direito **Windows PowerShell** e clique em **executar como administrador**.  
  
3.  Execute um comando semelhante ao seguinte, substituindo:  
  
    -   O nome do servidor primário do server01.domain01.contoso.com com o nome de domínio totalmente qualificado do seu servidor.  
  
    -   O local do D:\ReplicaVMStorage com o seu local.  
  
    -   O grupo de confiança denominado padrão com o nome do grupo, se você tiver criado um. Caso contrário, use o padrão.  
  
```  
New-VMReplicationAuthorizationEntry server01.domain01.contoso.com D:\ReplicaVMStorage DEFAULT  
```  
  


