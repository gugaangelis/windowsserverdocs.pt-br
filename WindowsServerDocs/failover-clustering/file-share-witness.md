---
title: Implantar uma testemunha de compartilhamento de arquivos no Windows Server 2019
ms.prod: windows-server
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/24/2019
description: O compartilhamento de arquivos testemunhas permite que você use um compartilhamento de arquivos para votar no quorum do cluster. Este tópico descreve o compartilhamento de arquivos testemunhas e a nova funcionalidade, incluindo o uso de uma unidade USB conectada a um roteador como uma testemunha de compartilhamento de arquivos.
ms.localizationpriority: medium
ms.openlocfilehash: 9f0a0c5b48f7c382367e4b1100ff649fe73d3be9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369763"
---
# <a name="deploy-a-file-share-witness"></a>Implantar uma testemunha de compartilhamento de arquivos

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Uma testemunha de compartilhamento de arquivos é um compartilhamento SMB que o cluster de failover usa como um voto no quorum do cluster. Este tópico fornece uma visão geral da tecnologia e da nova funcionalidade do Windows Server 2019, incluindo o uso de uma unidade USB conectada a um roteador como uma testemunha de compartilhamento de arquivos.

O compartilhamento de arquivos testemunhas é útil nas seguintes circunstâncias:  

- Não é possível usar uma testemunha de nuvem porque nem todos os servidores no cluster têm uma conexão de Internet confiável
- Não é possível usar uma testemunha de disco porque não há unidades compartilhadas a serem usadas para uma testemunha de disco. Pode ser um cluster Espaços de Armazenamento Diretos, SQL Server Always On grupos de disponibilidade (AG), grupo de disponibilidade de banco de dados do Exchange (DAG), etc.  Nenhum desses tipos de clusters usam discos compartilhados.

## <a name="file-share-witness-requirements"></a>Requisitos de testemunha de compartilhamento de arquivos

Você pode hospedar uma testemunha de compartilhamento de arquivos em um Windows Server ingressado no domínio ou se o cluster estiver executando o Windows Server 2019, qualquer dispositivo que possa hospedar um compartilhamento de arquivos SMB 2 ou posterior.

|Tipo de servidor de arquivos                 | Clusters com suporte |
|---------------------------------|--------------------|
|Qualquer dispositivo com compartilhamento de arquivos SMB 2 | Windows Server 2019|
|Windows Server ingressado no domínio     | Windows Server 2008 e posterior|

Se o cluster estiver executando o Windows Server 2019, aqui estão os requisitos:

- Um compartilhamento de arquivos SMB *em qualquer dispositivo que usa o protocolo SMB 2 ou posterior*, incluindo:
    - Dispositivos NAS (armazenamento conectado à rede)
    - Computadores com Windows ingressados em um grupo de trabalho
    - Roteadores com armazenamento USB conectado localmente
- Uma conta local no dispositivo para autenticar o cluster
- Se você estiver usando Active Directory para autenticar o cluster com o compartilhamento de arquivos, o CNO (objeto de nome de cluster) deverá ter permissões de gravação no compartilhamento e o servidor deverá estar na mesma floresta Active Directory que o cluster
- O compartilhamento de arquivos tem um mínimo de 5 MB de espaço livre

Se o cluster estiver executando o Windows Server 2016 ou anterior, aqui estão os requisitos:

- O compartilhamento de arquivos SMB *em um Windows Server ingressou na mesma floresta Active Directory que o cluster*
- O objeto de nome de cluster (CNO) deve ter permissões de gravação no compartilhamento
- O compartilhamento de arquivos tem um mínimo de 5 MB de espaço livre

Outras observações:
- Para usar uma testemunha de compartilhamento de arquivos hospedada por dispositivos que não sejam um Windows Server ingressado no domínio, você deve usar o cmdlet **set-ClusterQuorum-Credential** do PowerShell para definir a testemunha, conforme descrito posteriormente neste tópico.
- Para alta disponibilidade, você pode usar uma testemunha de compartilhamento de arquivos em um cluster de failover separado
- O compartilhamento de arquivos pode ser usado por vários clusters
- Não há suporte para o uso de um compartilhamento de Sistema de Arquivos Distribuído (DFS) ou armazenamento replicado com nenhuma versão do clustering de failover.  Isso pode causar uma situação de falha de divisão em que os servidores clusterizados são executados de forma independente um do outro e podem causar perda de dados.

## <a name="creating-a-file-share-witness-on-a-router-with-a-usb-device"></a>Criando uma testemunha de compartilhamento de arquivos em um roteador com um dispositivo USB

No [Microsoft Ignite 2018](https://azure.microsoft.com/ignite/), o [armazenamento de dados](http://www.dataonstorage.com/) tinha um cluster espaços de armazenamento diretos em sua área de quiosque.  Esse cluster foi conectado a um roteador [Netgear](https://www.netgear.com) Nighthawk X4S WiFi usando a porta USB como uma testemunha de compartilhamento de arquivos semelhante a esta.

![Testemunha de NetGear](media/File-Share-Witness/FSW1.png)

As etapas para criar uma testemunha de compartilhamento de arquivos usando um dispositivo USB nesse roteador específico estão listadas abaixo.  Observe que as etapas em outros roteadores e dispositivos NAS irão variar e devem ser realizadas usando as instruções fornecidas pelo fornecedor.


1. Faça logon no roteador com o dispositivo USB conectado.

   ![Interface NetGear](media/File-Share-Witness/FSW2.png)

2. Na lista de opções, selecione ReadySHARE, que é onde os compartilhamentos podem ser criados.

   ![ReadySHARE NetGear](media/File-Share-Witness/FSW3.png)

3. Para uma testemunha de compartilhamento de arquivos, um compartilhamento básico é tudo o que é necessário.  A seleção do botão Editar exibirá uma caixa de diálogo em que o compartilhamento pode ser criado no dispositivo USB.

   ![Interface de compartilhamento de NetGear](media/File-Share-Witness/FSW4.png)

4. Depois de selecionar o botão aplicar, o compartilhamento é criado e pode ser visto na lista.

   ![Compartilhamentos de NetGear](media/File-Share-Witness/FSW5.png)

5. Após a criação do compartilhamento, a criação da testemunha de compartilhamento de arquivos para cluster é feita com o PowerShell.

   ```PowerShell
   Set-ClusterQuorum -FileShareWitness \\readyshare\Witness -Credential (Get-Credential)
   ```

   Isso exibe uma caixa de diálogo para inserir a conta local no dispositivo.

Essas mesmas etapas semelhantes podem ser feitas em outros roteadores com recursos USB, dispositivos NAS ou outros dispositivos Windows.
