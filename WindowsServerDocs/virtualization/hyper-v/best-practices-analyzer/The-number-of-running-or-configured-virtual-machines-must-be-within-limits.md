---
title: Número de execução ou máquinas virtuais configuradas deve estar dentro dos limites com suporte
description: Fornece instruções para resolver o problema relatado por essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9d3c4aa3-8416-46ec-a253-26dc98088d7b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8a971a48b2d8199a6c279f1bd3f1715039fa6e0d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855347"
---
# <a name="the-number-of-running-or-configured-virtual-machines-must-be-within-supported-limits"></a>Número de execução ou máquinas virtuais configuradas deve estar dentro dos limites com suporte

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Erro  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Mais máquinas virtuais estão em execução ou configurados que têm suporte.*  
  
## <a name="impact"></a>Impacto  
*Microsoft não oferece suporte para o número atual de máquinas virtuais em execução ou configurados neste servidor.*  
  
## <a name="resolution"></a>Resolução  
*Mova uma ou mais máquinas virtuais para outro servidor.*  
  
Para obter detalhes sobre configurações com suporte máximo para o Hyper-V, como o número de máquinas virtuais em execução, consulte [planejar a escalabilidade do Hyper-V no Windows Server 2016](../plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md).  
  
Para mover uma máquina virtual para outro servidor, você pode:  
  
- Exportar a máquina virtual do servidor atual e, em seguida, importá-lo para um novo servidor conforme descrito abaixo.   
- Faça uma migração ao vivo:   
    - Se esse servidor pertence a um cluster de failover, use as ferramentas fornecidas com o recurso de cluster de Failover. Para obter instruções, consulte [migração ao vivo, migração rápida ou mover uma máquina Virtual de nó para nó](https://go.microsoft.com/fwlink/?LinkID=181519).  
    - Se esse for um servidor autônomo, consulte as instruções em [configurar migração ao vivo e migração de máquinas virtuais sem Clustering de Failover](https://technet.microsoft.com//library/jj134199(v=ws.11).aspx)  
  
### <a name="to-export-a-virtual-machine"></a>Para exportar uma máquina virtual  
  
   > [!IMPORTANT]  
   > Se o host do Hyper-V que você estiver exportando do pertence a um domínio e você deseja armazenar os arquivos exportados em um local remoto, o host do Hyper-V deve ser configurado para a delegação restrita. Um local remoto pode ser uma pasta de rede compartilhada ou uma pasta no host que você está importando. A delegação restrita permite que a conta de computador do host do Hyper-V fornecem credenciais delegadas para o serviço Common Internet File System (CIFS) no computador remoto. Para obter instruções sobre como configurar a delegação restrita, consulte a seção a seguir a exportação e importar instruções, abaixo.  
  
1.  Abra o Gerenciador Hyper-V. Clique em **Iniciar**, vá em **Ferramentas Administrativas** e clique em **Gerenciador do Hyper-V**.  
  
2.  No painel de resultados, sob **máquinas virtuais**, clique em uma máquina virtual e, em seguida, clique em **exportar**.  
  
3.  No **exportar uma máquina Virtual** caixa de diálogo, digite ou navegue até um local que tenha espaço livre suficiente para armazenar todos os recursos de máquina virtual. Quando você exporta uma máquina virtual, todos os discos rígidos virtuais (arquivos. vhd ou. vhdx), pontos de verificação (arquivos. avhd) e arquivos de estado salvo associados com a máquina virtual são copiados para a pasta especificada.  
  
4.  Clique em **Exportar**.  
  
Depois de exportar as máquinas virtuais, importe as máquinas virtuais para outro servidor.  
  
### <a name="to-import-a-virtual-machine-to-another-server"></a>Para importar uma máquina virtual para outro servidor  
  
1.  Conectar ao servidor que executa o Hyper-V e abra o Gerenciador do Hyper-V.  
  
2.  No **ação** painel, clique em **importar Máquina Virtual**.  
  
3.  No **importar Máquina Virtual** caixa de diálogo, especifique o local onde você exportou a máquina virtual. A menos que você deseja reimportar essa máquina virtual, deixe as configurações de importação como estão.  
  
4.  Clique em **Importar**.  
  
### <a name="to-configure-constrained-delegation"></a>Para configurar a delegação restrita  
  
Associação a **os administradores de domínio** grupo é necessário para concluir este procedimento.  
  
1.  Em um computador que tenha o recurso ferramentas de serviços de domínio Active Directory instalado, no **ferramentas administrativas**, abra **Active Directory Users and Computers**e, em seguida, navegue até a conta de computador o computador que executa o Hyper-V.  
  
    > [!NOTE]  
    > Se **Active Directory Users and Computers** não é listado, instale o recurso ferramentas de serviços de domínio do Active Directory. Para obter instruções, consulte [instalação de ferramentas de administração de servidor remoto para AD DS](https://go.microsoft.com/fwlink/?LinkId=140463) (https://go.microsoft.com/fwlink/?LinkId=140463).  
  
2.  A conta de computador para o computador que executa o Hyper-V com o botão direito e, em seguida, clique em **propriedades**.  
  
3.  Sobre o **delegação** , clique em **selecione o computador para delegação apenas a serviços especificados**e, em seguida, clique em **usar qualquer protocolo de autenticação**.  
  
4.  Para permitir que a conta de computador do Hyper-V apresentar credenciais delegadas ao computador remoto:  
  
    1.  Clique em **Adicionar**.  
  
    2.  No **adicionar serviços** caixa de diálogo, clique em **usuários ou computadores**, selecione o computador remoto e, em seguida, clique em **Okey**.  
  
    3.  No **serviços disponíveis** lista, selecione o **cifs** TCP/IP (também conhecido como o protocolo SMB Server Message Block ()) e, em seguida, clique em **adicionar**.  
  
  
  


