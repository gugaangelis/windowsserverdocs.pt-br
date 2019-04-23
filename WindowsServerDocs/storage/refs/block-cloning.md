---
ms.assetid: fd427da3-3869-428f-bf2a-56c4b7d99b40
title: Clonagem de blocos em ReFS
description: ''
author: gawatu
ms.author: gawatu
manager: gawatu
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-file-systems
ms.openlocfilehash: 54165700209320eee50fc63d98d78cbf4a92d053
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838107"
---
# <a name="block-cloning-on-refs"></a>Clonagem de blocos em ReFS

>Aplica-se a: 2019, Windows Server 2016, Windows Server (canal semestral) do Windows Server

A clonagem de blocos instrui o sistema de arquivos a copiar um intervalo de bytes do arquivo em nome de um aplicativo, onde o arquivo de destino pode ser igual ao ou diferente do arquivo de origem. As operações de cópia, infelizmente, são caras, pois disparam leituras e gravações caras para os dados físicos subjacentes. 

No entanto, a clonagem de blocos no ReFS executa cópias como uma operação de metadados de baixo custo, em vez de ler e gravar dados do arquivo. Como o ReFS permite que vários arquivos compartilhem os mesmo clusters lógicos (locais físicos em um volume), as operações de cópia só precisam remapear uma região de um arquivo para um local físico à parte, convertendo uma operação física cara em uma operação rápida e lógica. Isso permite que as cópias terminem mais rapidamente e gerem menos E/S para o armazenamento subjacente. Essa melhoria também beneficia cargas de trabalho de virtualização, pois as operações de mesclagem de pontos de verificação de .vhdx são aceleradas drasticamente quando as operações de clonagem de blocos são usadas. Além disso, como vários arquivos podem compartilhar os mesmo clusters lógicos, os dados idênticos não são armazenados fisicamente várias vezes, melhorando a capacidade de armazenamento. 
  
## <a name="how-it-works"></a>Como funciona 

A clonagem de blocos do ReFS converte uma operação de dados de arquivo em uma operação de metadados. Para fazer essa otimização, o ReFS introduz contagens de referências em seus metadados para regiões copiadas. Essa contagem de referência registra o número de regiões distintas de arquivos referenciam as mesmas regiões físicas. Isso permite que vários arquivos compartilhem os mesmos dados físicos:

![Mostrar atualizações da contagem de referência quando vários arquivos referenciam a mesma região](media/ref-count-example.gif)

Ao manter uma contagem de referência para cada cluster lógico, o ReFS não interrompe o isolamento entre arquivos: as gravações em regiões compartilhadas disparam um mecanismo de alocação mediante gravação, onde o ReFS aloca uma nova região para a gravação de entrada. Esse mecanismo preserva a integridade dos clusters lógicos compartilhados. 

### <a name="example"></a>Exemplo
Suponhamos que haja dois arquivos, X e Y, onde cada arquivo é composto por três regiões, e cada região é mapeada para clusters lógicos separados.

![Dois arquivos com três regiões distintas cada que são mapeadas para regiões que têm a contagem de referência 1](media/block-clone-1.png)

Agora, vamos supor que um aplicativo emita uma operação de clonagem de blocos do Arquivo X para o Arquivo Y, para as regiões A e B serem copiadas no deslocamento da região E. O estado do sistema de arquivo a seguir resultaria em:

![A contagem de referência mostra 2 para a região do clone do bloco](media/block-clone-2.png)

Esse estado do sistema de arquivo revela uma duplicação bem-sucedida da região do clone do bloco. Como o ReFS executa essa operação de cópia atualizando apenas os mapeamentos de VCN para LCN, nenhum dado físico foi lido, nem os dados físicos no Arquivo Y foram substituídos. Agora os arquivos X e Y compartilham clusters lógicos, refletidos pelas contagens de referência na tabela. Como nenhum dado foi copiado fisicamente, o ReFS reduz o consumo de capacidade no volume. 

Agora vamos supor que o aplicativo tente substituir a região A no Arquivo X. O ReFS duplicará a região compartilhada, atualizará as contagens de referência adequadamente e executará a gravação de entrada na região recém-duplicada. Isso garante que o isolamento entre os arquivos seja preservado.   

![Isolamento preservado pela gravação em uma nova região G e atualização de contagens de referência](media/block-clone-3.png)

Após a gravação de modificação, a região B ainda é compartilhada pelos dois arquivos. Observe que, se a região A fosse maior que um cluster, somente o cluster modificado teria sido duplicado, e a parte restante continuaria compartilhada.


## <a name="functionality-restrictions-and-remarks"></a>Restrições e comentários sobre a funcionalidade
- As regiões de origem e de destino devem começar e terminar em um limite de cluster. 
- A região clonada deve ter menos de 4 GB. 
- O número máximo de regiões de arquivo que podem ser mapeadas para a mesma região física é 8175.
- A região de destino não deve ultrapassar o final do arquivo. Se o aplicativo quiser estender o destino com dados clonados, ele deverá chamar [SetEndOfFile](https://msdn.microsoft.com/library/windows/desktop/aa365531(v=vs.85).aspx) primeiro. 
- Se as regiões de origem e de destino estiverem no mesmo arquivo, elas não deverão se sobrepor. (O aplicativo poderá continuar dividindo a operação de clonagem de blocos em vários clones de blocos que não se sobrepõem mais.)
- Os arquivos de origem e de destino devem estar no mesmo volume do ReFS. 
- Os arquivos de origem e de destino devem ter a mesma configuração de [Fluxos de Integridade](https://msdn.microsoft.com/library/windows/desktop/gg258117(v=vs.85).aspx). 
- Se o arquivo de origem for esparso, o arquivo de destino também deverá ser esparso. 
- A operação de clonagem de blocos interromperá os bloqueios oportunistas compartilhados (também conhecidos como [bloqueios oportunistas de nível 2](https://msdn.microsoft.com/library/windows/desktop/aa365713(v=vs.85).aspx)).
- O volume do ReFS deverá ter sido formatado com o Windows Server 2016 e, se o Clustering de Failover estiver em uso, o Nível Funcional de Clustering deve ter sido o Windows Server 2016 ou posterior no momento da formatação. 

## <a name="see-also"></a>Consulte também

-   [Visão geral de reFS](refs-overview.md)
-   [Fluxos de integridade de reFS](integrity-streams.md)
-   [Visão geral direta de espaços de armazenamento](../storage-spaces/storage-spaces-direct-overview.md)
-   [DUPLICATE_EXTENTS_DATA](https://msdn.microsoft.com/library/windows/desktop/mt590821(v=vs.85).aspx)
-   [FSCTL_DUPLICATE_EXTENTS_TO_FILE](https://msdn.microsoft.com/library/windows/desktop/mt590823(v=vs.85).aspx)
