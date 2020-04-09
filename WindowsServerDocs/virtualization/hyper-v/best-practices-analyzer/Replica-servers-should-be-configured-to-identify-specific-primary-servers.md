---
title: Os servidores de réplica devem ser configurados para identificar servidores primários específicos autorizados a enviar tráfego de replicação
description: Fornece instruções para resolver o problema relatado por essa regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 0aeb1f4b-2e75-430b-9557-fe64738c4992
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 649af22f615f2f36baceb1fa23b79c54b038f9c2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861829"
---
# <a name="replica-servers-should-be-configured-to-identify-specific-primary-servers-authorized-to-send-replication-traffic"></a>Os servidores de réplica devem ser configurados para identificar servidores primários específicos autorizados a enviar tráfego de replicação

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Conforme configurado, esse servidor de réplica aceita o tráfego de replicação de todos os servidores primários e os armazena em um único local.*  
  
### <a name="impact"></a>Impacto  
*Toda a replicação de todos os servidores primários é armazenada em um local, o que pode introduzir problemas de privacidade ou segurança.*  
  
## <a name="resolution"></a>Resolução  
*Use o Gerenciador do Hyper-V para criar novas entradas de autorização para os servidores primários específicos e especificar locais de armazenamento separados para cada um deles. Você pode usar caracteres curinga para agrupar servidores primários em conjuntos para cada entrada de autorização.*  
  
#### <a name="create-authorization-entries-using-hyper-v-manager"></a>Criar entradas de autorização usando o Gerenciador do Hyper-V  
  
1.  Abra o Gerenciador Hyper-V. (Em Gerenciador do Servidor, clique em **ferramentas** > **Gerenciador do Hyper-V**.)  
  
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
  
## <a name="see-also"></a>Consulte também  
[New-VMReplicationAuthorizationEntry](https://technet.microsoft.com/library/hh848606.aspx)  
  


