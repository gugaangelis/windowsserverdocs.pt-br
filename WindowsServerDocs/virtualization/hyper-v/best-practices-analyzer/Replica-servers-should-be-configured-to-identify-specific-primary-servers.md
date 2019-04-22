---
title: Servidores de réplica devem ser configurados para identificar servidores primários específicos autorizados a enviar o tráfego de replicação
description: Fornece instruções para resolver o problema relatado por essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 0aeb1f4b-2e75-430b-9557-fe64738c4992
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 47b215d4c84e68d93ae1189ddd370358e2781eff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822237"
---
# <a name="replica-servers-should-be-configured-to-identify-specific-primary-servers-authorized-to-send-replication-traffic"></a>Servidores de réplica devem ser configurados para identificar servidores primários específicos autorizados a enviar o tráfego de replicação

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Conforme configurado, esse servidor de réplica aceita tráfego de replicação de todos os servidores primários e os armazena em um único local.*  
  
### <a name="impact"></a>Impacto  
*Toda a replicação de todos os servidores primários é armazenados em um único local, que pode introduzir problemas de segurança ou privacidade.*  
  
## <a name="resolution"></a>Resolução  
*Use o Gerenciador do Hyper-V para criar novas entradas de autorização para os servidores primários específicos e especificar locais de armazenamento separada para cada um deles. Você pode usar caracteres curinga para agrupar os servidores primários em conjuntos para cada entrada de autorização.*  
  
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
  
## <a name="see-also"></a>Consulte também  
[New-VMReplicationAuthorizationEntry](https://technet.microsoft.com/library/hh848606.aspx)  
  


