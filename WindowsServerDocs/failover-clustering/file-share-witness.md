---
title: Implantar uma testemunha de compartilhamento de arquivos no Windows Server de 2019
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/24/2019
description: As testemunhas de compartilhamento de arquivos permitem que você use um compartilhamento de arquivos para votar em quorum do cluster. Este tópico descreve as testemunhas de compartilhamento de arquivo e a nova funcionalidade, incluindo o uso de uma unidade USB conectada a um roteador como uma testemunha de compartilhamento de arquivos.
ms.localizationpriority: medium
ms.openlocfilehash: 47371be946c08cac2f271138d701922fc340a89d
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453038"
---
# <a name="deploy-a-file-share-witness"></a>Implantar uma testemunha de compartilhamento de arquivo

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Uma testemunha de compartilhamento de arquivo é um compartilhamento SMB que usa o Cluster de Failover como um voto de quorum do cluster. Este tópico fornece uma visão geral da tecnologia e a nova funcionalidade no Windows Server 2019, incluindo o uso de uma unidade USB conectada a um roteador como uma testemunha de compartilhamento de arquivos.

As testemunhas de compartilhamento de arquivo são úteis nas seguintes circunstâncias:  

- Uma testemunha de nuvem não pode ser usada porque nem todos os servidores no cluster tem uma conexão de Internet confiável
- Uma testemunha de disco não pode ser usada porque não existem em todas as unidades a ser usado para uma testemunha de disco compartilhadas. Isso pode ser um cluster de espaços de armazenamento diretos, SQL Server sempre em grupos AG (disponibilidade), grupo de disponibilidade na banco de dados do Exchange (DAG), etc.  Nenhum desses tipos de clusters usar discos compartilhados.

## <a name="file-share-witness-requirements"></a>Requisitos de testemunha de compartilhamento de arquivos

Você pode hospedar uma testemunha de compartilhamento de arquivo em um servidor do Windows ingressados no domínio, ou se o cluster estiver executando Windows Server 2019, qualquer dispositivo que podem host um SMB 2 ou posterior compartilhamento de arquivos.

|Tipo de servidor de arquivo                 | Clusters com suporte |
|---------------------------------|--------------------|
|Qualquer compartilhamento de dispositivo w/um SMB 2 arquivos | Windows Server 2019|
|Servidor Windows ingressado no domínio     | Windows Server 2008 e posterior|

Se o cluster estiver executando o Windows Server 2019, aqui estão os requisitos:

- Um compartilhamento de arquivos SMB *em qualquer dispositivo que usa o SMB 2 ou posterior protocolo*, incluindo:
    - Dispositivos de armazenamento conectado à rede (NAS)
    - Os computadores do Windows associados a um grupo de trabalho
    - Roteadores com o armazenamento USB conectado localmente
- Uma conta local no dispositivo para autenticar o cluster
- Se você estiver usando o Active Directory em vez disso, para autenticar o cluster com o compartilhamento de arquivos, o objeto de nome de Cluster (CNO) deve ter permissões de gravação no compartilhamento e o servidor deve ser na mesma floresta do Active Directory que o cluster
- O compartilhamento de arquivos tem um mínimo de 5 MB de espaço livre

Se o cluster está executando o Windows Server 2016 ou anterior, aqui estão os requisitos:

- Compartilhamento de arquivos SMB *em um servidor do Windows associado à mesma floresta do Active Directory que o cluster*
- O objeto de nome de Cluster (CNO) deve ter permissões de gravação no compartilhamento
- O compartilhamento de arquivos tem um mínimo de 5 MB de espaço livre

Outras Observações:
- Para usar uma testemunha de compartilhamento de arquivos hospedada por dispositivos que não seja um servidor do Windows ingressados no domínio, você no momento, deve usar o **Set-ClusterQuorum-credencial** cmdlet do PowerShell para definir a testemunha, conforme descrito mais adiante neste tópico.
- Para alta disponibilidade, você pode usar uma testemunha de compartilhamento de arquivos em um Cluster de Failover separado
- O compartilhamento de arquivos pode ser usado por vários clusters
- Não há suporte para o uso de um compartilhamento de sistema de arquivos distribuído (DFS) ou o armazenamento replicado com qualquer versão do cluster de failover.  Isso pode causar uma situação de dupla personalidade de divisão onde servidores clusterizados estão em execução independentemente uns dos outros e pode causar perda de dados.

## <a name="creating-a-file-share-witness-on-a-router-with-a-usb-device"></a>Criação de uma testemunha de compartilhamento de arquivos em um roteador com um dispositivo USB

No [Microsoft Ignite 2018](https://azure.microsoft.com/ignite/), [armazenamento de dados](http://www.dataonstorage.com/) tinha um Cluster de espaços de armazenamento diretos em suas áreas de quiosque.  Este cluster foi conectado a um [NetGear](https://www.netgear.com) semelhante a este de testemunha de compartilhamento de Nighthawk X4S Wi-Fi roteador usando a porta USB como um arquivo.

![NetGear testemunha](media/File-Share-Witness/FSW1.png)

As etapas para criar uma testemunha de compartilhamento de arquivo usando um dispositivo USB no roteador específico estão listadas abaixo.  Observe que as etapas em outros roteadores e os dispositivos NAS irão variar e devem ser feitas usando o fornecedor fornecido direções.


1. Faça logon no roteador com o dispositivo USB conectado.

   ![NetGear Interface](media/File-Share-Witness/FSW2.png)

2. Na lista de opções, selecione ReadySHARE que é onde os compartilhamentos podem ser criados.

   ![NetGear ReadySHARE](media/File-Share-Witness/FSW3.png)

3. Para uma testemunha de compartilhamento de arquivo, um compartilhamento básico é que tudo o que é necessário.  Selecionando o botão Edit será exibida uma caixa de diálogo onde o compartilhamento pode ser criado no dispositivo USB.

   ![Interface NetGear compartilhamento](media/File-Share-Witness/FSW4.png)

4. Depois de selecionar o botão Aplicar, o compartilhamento é criado e pode ser visto na lista.

   ![Compartilhamentos NetGear](media/File-Share-Witness/FSW5.png)

5. Depois que o compartilhamento tiver sido criado, a testemunha de compartilhamento de arquivo para o Cluster é criado com o PowerShell.

   ```PowerShell
   Set-ClusterQuorum -FileShareWitness \\readyshare\Witness -Credential (Get-Credential)
   ```

   Isso exibe uma caixa de diálogo para inserir a conta local no dispositivo.

Essas mesmas etapas semelhantes podem ser feitas em outros roteadores com recursos USB, dispositivos NAS ou outros dispositivos do Windows.
