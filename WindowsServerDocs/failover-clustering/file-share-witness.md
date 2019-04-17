---
title: Implantar uma testemunha de compartilhamento de arquivo no Windows Server 2019
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/24/2019
description: Testemunhas de compartilhamento de arquivo permitem que você use um compartilhamento de arquivos para votar em quórum do cluster. Este tópico descreve testemunhas de compartilhamento de arquivo e a nova funcionalidade, incluindo o uso de uma unidade USB conectada a um roteador como uma testemunha de compartilhamento de arquivo.
ms.localizationpriority: medium
ms.openlocfilehash: 1888142f96208800a0417c9caeea89e8a0472e88
ms.sourcegitcommit: d622f7af181ed0063d716b30278d41887a57db19
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/08/2019
ms.locfileid: "9150886"
---
# Implantar uma testemunha de compartilhamento de arquivos

> Aplicável a: Windows Server 2019 Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Uma testemunha de compartilhamento de arquivo é um compartilhamento SMB que usa o Cluster de Failover como um voto no quórum do cluster. Este tópico fornece uma visão geral da tecnologia e a nova funcionalidade no Windows Server 2019, incluindo o uso de uma unidade USB conectada a um roteador como uma testemunha de compartilhamento de arquivo.

Testemunhas de compartilhamento de arquivo são úteis nas seguintes circunstâncias:  

- Uma testemunha de nuvem não pode ser usada porque nem todos os servidores no cluster tem uma conexão de Internet confiável
- Uma testemunha de disco não pode ser usada porque não há qualquer unidades compartilhadas a ser usado para uma testemunha de disco. Isso pode ser um cluster de espaços de armazenamento diretos, SQL Server sempre em disponibilidade grupos (AG), Exchange banco de dados de disponibilidade de grupo (Dag mão), etc.  Nenhum desses tipos de clusters usar discos compartilhados.

## Requisitos de testemunha de compartilhamento de arquivos

Você pode hospedar uma testemunha de compartilhamento de arquivo em um domínio do Windows server, ou se o cluster estiver executando o Windows Server 2019, qualquer dispositivo que pode host um SMB 2 ou posterior arquivo compartilhar.

|Tipo de servidor de arquivo                 | Clusters com suporte |
|---------------------------------|--------------------|
|Qualquer compartilhamento de dispositivo w/uma SMB 2 arquivos | Windows Server 2019|
|Ingressado no domínio Windows Server     | Windows Server 2008 e posterior|

Se o cluster estiver executando o Windows Server 2019, aqui estão os requisitos:

- Compartilhe um arquivo SMB *em qualquer dispositivo que usa o SMB 2 ou posterior protocolo*, incluindo:
    - Dispositivos de armazenamento conectado à rede (NAS)
    - Computadores Windows associados a um grupo de trabalho
    - Roteadores com o armazenamento USB conectado localmente
- Uma conta local no dispositivo para autenticação do cluster
- Se você estiver em vez disso, usando o Active Directory para autenticar o cluster com o compartilhamento de arquivos, o objeto de nome de Cluster (CNO) deve ter permissões de gravação no compartilhamento, e o servidor deve estar na mesma floresta do Active Directory como o cluster
- O compartilhamento de arquivo tem um mínimo de 5 MB de espaço livre

Se o cluster executando o Windows Server 2016 ou anterior, aqui estão os requisitos:

- *Em um servidor Windows associado à mesma floresta do Active Directory como o cluster* de compartilhamento de arquivo SMB
- O objeto de nome de Cluster (CNO) deve ter permissões de gravação no compartilhamento
- O compartilhamento de arquivo tem um mínimo de 5 MB de espaço livre

Outras Observações:
- Para usar uma testemunha de compartilhamento de arquivo hospedada por dispositivos que não sejam um domínio do Windows server, você atualmente deve usar o **Set-ClusterQuorum-Credential** cmdlet do PowerShell para definir a testemunha, conforme descrito mais adiante neste tópico.
- Para alta disponibilidade, você pode usar uma testemunha de compartilhamento de arquivo em um Cluster de Failover separado
- O compartilhamento de arquivos pode ser usado por vários clusters
- O uso de um compartilhamento de sistema de arquivos distribuído (DFS) ou armazenamento replicado não é compatível com qualquer versão do cluster de failover.  Elas podem causar uma situação de cérebro divisão onde servidores clusterizados executam independentemente uns dos outros e podem causar perda de dados.

## Criando uma testemunha de compartilhamento de arquivo em um roteador com um dispositivo USB

No [Microsoft Ignite 2018](https://azure.microsoft.com/ignite/), o [Armazenamento DataOn](http://www.dataonstorage.com/) tinha um Cluster de espaços de armazenamento diretos em suas áreas de quiosque.  Esse cluster foi conectado a um roteador de Wi-Fi [NetGear](https://www.netgear.com) Nighthawk X4S usando a porta USB como uma testemunha de compartilhamento de arquivo semelhante a este.

![Testemunha NetGear](media\File-Share-Witness\FSW1.png)

As etapas para criar uma testemunha de compartilhamento de arquivo usando um dispositivo USB neste roteador específico estão listadas abaixo.  Observe que as etapas em outros roteadores e dispositivos NAS irá variar e devem ser feitas usando o fornecedor fornecido direções.


1. Faça logon o roteador com o dispositivo USB conectado.

   ![Interface NetGear](media\File-Share-Witness\FSW2.png)

2. Na lista de opções, selecione ReadySHARE que é onde os compartilhamentos podem ser criados.

   ![NetGear ReadySHARE](media\File-Share-Witness\FSW3.png)

3. Para uma testemunha de compartilhamento de arquivos, um compartilhamento básico é tudo o que é necessário.  Selecionando o botão Edit será exibida uma caixa de diálogo onde o compartilhamento pode ser criado no dispositivo USB.

   ![Interface de compartilhamento NetGear](media\File-Share-Witness\FSW4.png)

4. Depois de selecionar o botão Aplicar, o compartilhamento é criado e pode ser visto na lista.

   ![NetGear compartilhamentos](media\File-Share-Witness\FSW5.png)

5. Quando o compartilhamento tiver sido criado, criando a testemunha de compartilhamento de arquivos para Cluster é feito com o PowerShell.

   ```PowerShell
   Set-ClusterQuorum -FileShareWitness \\readyshare\Witness -Credential (Get-Credential)
   ```

   Isso exibe uma caixa de diálogo para inserir a conta local no dispositivo.

Essas mesmas etapas semelhantes podem ser feitas em outros roteadores com recursos USB, dispositivos NAS ou outros dispositivos Windows.
