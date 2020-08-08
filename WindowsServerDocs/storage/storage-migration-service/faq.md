---
title: FAQ (perguntas frequentes) sobre o serviço de migração de armazenamento
description: Perguntas frequentes sobre o serviço de migração de armazenamento, como quais arquivos são excluídos das transferências ao migrar de um servidor para outro.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 06/02/2020
ms.topic: article
ms.openlocfilehash: ff7a276802a0bd6a61fc0ea5eb6148972ac6d674
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87939303"
---
# <a name="storage-migration-service-frequently-asked-questions-faq"></a>FAQ (perguntas frequentes) sobre o serviço de migração de armazenamento

Este tópico contém respostas a perguntas frequentes sobre como usar o [serviço de migração de armazenamento](overview.md) para migrar servidores.

## <a name="what-files-and-folders-are-excluded-from-transfers"></a>Quais arquivos e pastas são excluídos das transferências?

O serviço de migração de armazenamento não transferirá arquivos ou pastas que sabemos que podem interferir na operação do Windows. Especificamente, aqui está o que não vamos transferir ou mover para a pasta PreExistingData no destino:

- Windows, arquivos de programas, arquivos de programas (x86), dados de programa, usuários
- $Recycle. bin, recycleer, reciclado, informações de volume do sistema, $UpgDrv $, $SysReset, $Windows. ~ BT, $Windows. ~ LS, Windows. old, inicialização, recuperação, documentos e configurações
- pagefile.sys, hiberfil.sys, swapfile.sys, winpepge.sys, config.sys, Bootsect. bak, bootmgr, bootnxt
- Quaisquer arquivos ou pastas no servidor de origem que estejam em conflito com as pastas excluídas no destino. <br>Por exemplo, se há uma pasta N:\Windows na origem e ela é mapeada para C:\ volume no destino, ele não será transferido, independentemente do que ele contém, pois ele interferiria com a pasta C:\Windows System no destino.

## <a name="are-locked-files-migrated"></a>Os arquivos bloqueados são migrados?

O serviço de migração de armazenamento não migra arquivos que os aplicativos bloqueiam exclusivamente. O serviço automaticamente tenta novamente três vezes com um atraso de 60 segundos entre as tentativas, e você pode controlar o número de tentativas e o atraso. Você também pode executar novamente as transferências para copiar apenas os arquivos que foram ignorados anteriormente devido a violações de compartilhamento.

## <a name="are-domain-migrations-supported"></a>Há suporte para migrações de domínio?

O serviço de migração de armazenamento não permite a migração entre Active Directory domínios. As migrações entre servidores sempre ingressarão no servidor de destino no mesmo domínio. Você pode usar as credenciais de migração de domínios diferentes na floresta Active Directory. O serviço de migração de armazenamento oferece suporte à migração entre grupos de os.

## <a name="are-clusters-supported-as-sources-or-destinations"></a>Há suporte para clusters como origens ou destinos?

O serviço de migração de armazenamento dá suporte à migração de e para clusters após a instalação da atualização cumulativa [KB4513534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) ou atualizações subsequentes. Isso inclui a migração de um cluster de origem para um cluster de destino, bem como a migração de um servidor de origem autônomo para um cluster de destino para fins de consolidação de dispositivos. No entanto, não é possível migrar um cluster para um servidor autônomo.

## <a name="do-local-groups-and-local-users-migrate"></a>Os grupos locais e os usuários locais são migrados?

O serviço de migração de armazenamento dá suporte à migração de usuários e grupos locais após a instalação da atualização cumulativa [KB4513534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) ou atualizações subsequentes.

## <a name="is-domain-controller-migration-supported"></a>Há suporte para a migração do controlador de domínio?

Atualmente, o serviço de migração de armazenamento não migra controladores de domínio no Windows Server 2019. Como alternativa, contanto que você tenha mais de um controlador de domínio no domínio Active Directory, rebaixe o controlador de domínio antes de migrá-lo e, em seguida, promova o destino após a conclusão da redução. Se você optar por migrar uma origem ou destino do controlador de domínio, não será possível recortar. Você nunca deve migrar usuários e grupos ao migrar do ou para um controlador de domínio.

## <a name="what-attributes-are-migrated-by-the-storage-migration-service"></a>Quais atributos são migrados pelo serviço de migração de armazenamento?

O serviço de migração de armazenamento migra todos os sinalizadores, configurações e segurança de compartilhamentos SMB. A lista de sinalizadores que o serviço de migração de armazenamento migra inclui:

- Estado de compartilhamento
- Tipo de disponibilidade
- Tipo de compartilhamento
- Modo de enumeração de pasta *(também conhecido como enumeração baseada em acesso ou Abe)*
- Modo de cache
- Modo de leasing
- Instância SMB
- Tempo limite da CA
- Limite de usuários simultâneos
- Continuamente disponível
- Descrição
- Criptografar Dados
- Comunicação remota de identidade
- Infraestrutura
- Nome
- Caminho
- Com escopo
- Nome do escopo
- Descritor de Segurança
- Cópia de sombra
- Especial
- Temporário

## <a name="can-i-consolidate-multiple-servers-into-one-server"></a>Posso consolidar vários servidores em um servidor?

A versão do serviço de migração de armazenamento fornecida no Windows Server 2019 não dá suporte à consolidação de vários servidores em um servidor. Um exemplo de consolidação seria migrar três servidores de origem separados, que podem ter os mesmos nomes de compartilhamento e caminhos de arquivo locais – em um único servidor novo que virtualizava esses caminhos e compartilhamentos para evitar qualquer sobreposição ou colisão, então respondeu a todos os três nomes de servidores e endereços IP anteriores. No entanto, você pode migrar servidores autônomos para vários recursos de servidor de arquivos em um único cluster.

## <a name="can-i-migrate-from-sources-other-than-windows-server"></a>Posso migrar de fontes diferentes do Windows Server?

O serviço de migração de armazenamento dá suporte à migração de servidores Linux do samba após a instalação da atualização cumulativa [KB4513534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) ou atualizações posteriores. Consulte os requisitos para obter uma lista de versões do samba e distribuições do Linux com suporte.

## <a name="can-i-migrate-previous-file-versions"></a>Posso migrar versões de arquivo anteriores?

A versão do serviço de migração de armazenamento fornecida no Windows Server 2019 não dá suporte à migração de versões anteriores (feitas com o serviço de cópias de sombra de volume) de arquivos. Somente a versão atual será migrada.

## <a name="optimizing-inventory-and-transfer-performance"></a>Otimizando o desempenho do inventário e da transferência

O serviço de migração de armazenamento contém um mecanismo de leitura e cópia com vários threads chamado serviço de proxy de serviço de migração de armazenamento, que criamos para ser tão rápido, bem como para oferecer uma fidelidade de dados perfeita em muitas ferramentas de cópia de arquivo. Embora a configuração padrão seja ideal para muitos clientes, há maneiras de melhorar o desempenho do SMS durante o inventário e a transferência.

- **Use o Windows Server 2019 para o sistema operacional de destino.** O Windows Server 2019 contém o serviço de proxy de serviço de migração de armazenamento. Quando você instala esse recurso e migra para destinos do Windows Server 2019, todas as transferências funcionam como uma linha de visão direta entre a origem e o destino. Esse serviço é executado no Orchestrator durante a transferência se os computadores de destino forem Windows Server 2012 R2 ou Windows Server 2016, o que significa que o salto duplo de transferências e será muito mais lento. Se houver vários trabalhos em execução com destinos do Windows Server 2012 R2 ou do Windows Server 2016, o orquestrador se tornará um afunilamento.

- **Altere os threads de transferência padrão.** O serviço de proxy de serviço de migração de armazenamento copia 8 arquivos simultaneamente em um determinado trabalho. Você pode aumentar o número de threads de cópia simultâneas ajustando o seguinte registro REG_DWORD nome do valor em decimal em cada nó que executa o proxy do serviço de migração de armazenamento:

    HKEY_Local_Machine \Software\Microsoft\SMSProxy

    FileTransferThreadCount

   O intervalo válido é de 1 a 512 no Windows Server 2019. Você não precisa reiniciar o serviço para começar a usar essa configuração, desde que você crie um novo trabalho. Tome cuidado com essa configuração; a configuração mais alta pode exigir núcleos adicionais, desempenho de armazenamento e largura de banda de rede. Configurá-lo muito alto pode levar a um desempenho reduzido em comparação com as configurações padrão.

- **Alterar threads de compartilhamento paralelo padrão.** O serviço de proxy de serviço de migração de armazenamento copia de 8 compartilhamentos simultaneamente em um determinado trabalho. Você pode aumentar o número de threads de compartilhamento simultâneos ajustando o seguinte registro REG_DWORD nome do valor em decimal no servidor do Orchestrator do serviço de migração de armazenamento:

    HKEY_Local_Machine \Software\Microsoft\SMS

    EndpointFileTransferTaskCount

   O intervalo válido é de 1 a 512 no Windows Server 2019. Você não precisa reiniciar o serviço para começar a usar essa configuração, desde que você crie um novo trabalho. Tome cuidado com essa configuração; a configuração mais alta pode exigir núcleos adicionais, desempenho de armazenamento e largura de banda de rede. Configurá-lo muito alto pode levar a um desempenho reduzido em comparação com as configurações padrão.

    A soma de FileTransferThreadCount e EndpointFileTransferTaskCount é quantos arquivos o serviço de migração de armazenamento pode copiar simultaneamente de um nó de origem em um trabalho. Para adicionar mais nós de origem paralelos, crie e execute trabalhos mais simultâneos.

- **Adicione núcleos e memória.**  É altamente recomendável que os computadores de origem, Orchestrator e de destino tenham pelo menos dois núcleos de processador ou dois vCPUs, e mais pode ajudar significativamente o desempenho de inventário e transferência, especialmente quando combinado com FileTransferThreadCount (acima). Ao transferir arquivos maiores do que os formatos usuais do Office (gigabytes ou superior), o desempenho de transferência se beneficiará de mais memória do que o mínimo de 2 GB padrão.

- **Crie vários trabalhos.** Ao criar um trabalho com várias fontes de servidor, cada servidor é contatado de modo Serial para inventário, transferência e transição. Isso significa que cada servidor deve concluir sua fase antes do início de outro servidor. Para executar mais servidores em paralelo, basta criar vários trabalhos, com cada trabalho contendo apenas um servidor. O SMS dá suporte a até 100 executando trabalhos simultaneamente, o que significa que um único orquestrador pode paralelizar muitos computadores de destino do Windows Server 2019. Não recomendamos a execução de vários trabalhos paralelos se os computadores de destino forem Windows Server 2016 ou Windows Server 2012 R2, assim como sem o serviço de proxy do SMS em execução no destino, o orquestrador deverá executar todas as transferências e se tornar um afunilamento. A capacidade de execução de servidores em paralelo dentro de um único trabalho é um recurso que planejamos adicionar em uma versão posterior do SMS.

- **Use o SMB 3 com redes RDMA.** Se estiver transferindo de um computador de origem do Windows Server 2012 ou posterior, o SMB 3. x dá suporte ao modo SMB direto e à rede RDMA. O RDMA move a maior parte do custo da CPU de transferência das CPUs da motherboard para processadores de NIC integrados, reduzindo a latência e a utilização da CPU do servidor. Além disso, redes RDMA, como ROCE e iWARP, normalmente têm largura de banda consideravelmente maior do que o TCP/Ethernet típico, incluindo 25, 50 e velocidades de 100 GB por interface. O uso do SMB Direct normalmente move o limite de velocidade de transferência da rede para o próprio armazenamento.

- **Use o SMB 3 multicanal.** Se estiver transferindo de um computador de origem do Windows Server 2012 ou posterior, o SMB 3. x oferece suporte a cópias multicanal que podem melhorar muito o desempenho da cópia de arquivos. Esse recurso funciona automaticamente desde que a origem e o destino tenham:

   - Vários adaptadores de rede
   - Um ou mais adaptadores de rede que dão suporte ao dimensionamento lateral de recepção (RSS)
   - Um ou mais adaptadores de rede configurados usando o agrupamento NIC
   - Um ou mais adaptadores de rede com suporte a RDMA

- **Atualize os drivers.** Conforme apropriado, instale o armazenamento do fornecedor mais recente e o firmware e drivers de compartimento, drivers de HBA mais recentes, firmware de BIOS/UEFI mais recente, drivers de rede de fornecedor mais recentes e drivers de chipset de motherboard mais recentes nos servidores de origem, destino e Orchestrator. Reinicie os nós conforme necessário. confira a documentação do fornecedor de hardware para configurar o armazenamento compartilhado e o hardware de rede.

- **Habilite o processamento de alto desempenho.** Verifique se as configurações de BIOS/UEFI para servidores permitem o alto desempenho, como desabilitar C-State, definir a velocidade de QPI, habilitar NUMA e definir a frequência de memória mais alta. Verifique se o gerenciamento de energia no Windows Server está definido como alto desempenho. Reinicie conforme necessário. Não se esqueça de retorná-los para os Estados apropriados depois de concluir a migração.

- **Ajustar o hardware** Examine as [diretrizes de ajuste de desempenho do Windows server 2016](/windows-server/administration/performance-tuning/) para ajustar o Orchestrator e os computadores de destino que executam o windows Server 2019 e o windows Server 2016. A seção de [ajuste de desempenho do subsistema de rede](../../networking/technologies/network-subsystem/net-sub-performance-tuning-nics.md) contém informações especialmente valiosas.

- **Use um armazenamento mais rápido.** Embora possa ser difícil atualizar a velocidade de armazenamento do computador de origem, você deve garantir que o armazenamento de destino seja pelo menos rápido no desempenho de e/s de gravação, pois a origem está em desempenho de e/s de leitura para garantir que não haja nenhum afunilamento desnecessário nas transferências. Se o destino for uma VM, certifique-se de que, pelo menos, para fins de migração, seja executado na camada de armazenamento mais rápida de seus hosts de hipervisor, como na camada flash ou com Espaços de Armazenamento Diretos clusters HCI utilizando espaços em Flash ou híbridos espelhados. Quando a migração do SMS é concluída, a VM pode ser migrada ao vivo para uma camada ou host mais lento.

- **Atualizar o antivírus.** Sempre verifique se a origem e o destino estão executando a versão mais recente com patches do software antivírus para garantir a sobrecarga mínima de desempenho. Como um teste, você pode excluir *temporariamente* a verificação de pastas que está inventariando ou migrando nos servidores de origem e de destino. Se o desempenho de transferência for melhorado, entre em contato com seu fornecedor de software antivírus para obter instruções ou para obter uma versão atualizada do software antivírus ou uma explicação da degradação de desempenho esperada.

## <a name="can-i-migrate-from-ntfs-to-refs"></a>Posso migrar de NTFS para REFS?

A versão do serviço de migração de armazenamento fornecida no Windows Server 2019 não dá suporte à migração do NTFS para os sistemas de arquivos REFS. Você pode migrar de NTFS para NTFS e REFS para ReFS. Isso é por design, devido às muitas diferenças na funcionalidade, nos metadados e em outros aspectos que o ReFS não duplica do NTFS. ReFS é destinado a um sistema de arquivos de carga de trabalho de aplicativo, não a um sistema de arquivos geral. Para obter mais informações, consulte [visão geral do ReFS (sistema de arquivos resiliente)](../refs/refs-overview.md)

## <a name="can-i-move-the-storage-migration-service-database"></a>Posso mover o banco de dados do serviço de migração de armazenamento?

O serviço de migração de armazenamento usa um banco de dados ESE (mecanismo de armazenamento extensível) que é instalado por padrão na pasta c:\programdata\microsoft\storagemigrationservice oculta. Esse banco de dados aumentará à medida que os trabalhos forem adicionados e as transferências forem concluídas e poderão consumir espaço em disco significativo após a migração de milhões de arquivos se você não excluir os trabalhos. Se o banco de dados precisar ser movido, execute as seguintes etapas:

1. Pare o serviço de "serviço de migração de armazenamento" no computador do Orchestrator.
2. Apropriar-se da `%programdata%/Microsoft/StorageMigrationService` pasta
3. Adicione sua conta de usuário para ter controle total sobre esse compartilhamento e todos os seus arquivos e subpastas.
4. Mova a pasta para outra unidade no computador do Orchestrator.
5. Defina o seguinte valor de REG_SZ do registro:

    HKEY_Local_Machine \Software\Microsoft\SMS DatabasePath = *caminho para a nova pasta de banco de dados em um volume diferente*

6. Verifique se o sistema tem controle total para todos os arquivos e subpastas dessa pasta
7. Remova suas próprias permissões de conta.
8. Inicie o serviço "serviço de migração de armazenamento".

## <a name="does-the-storage-migration-service-migrate-locally-installed-applications-from-the-source-computer"></a>O serviço de migração de armazenamento migra os aplicativos instalados localmente do computador de origem?

Não, o serviço de migração de armazenamento não migra os aplicativos instalados localmente. Depois de concluir a migração, reinstale todos os aplicativos no computador de destino que estavam em execução no computador de origem. Não é necessário reconfigurar nenhum usuário ou seus aplicativos; o serviço de migração de armazenamento foi projetado para tornar a mudança de servidor invisível para os clientes.

## <a name="what-happens-with-existing-files-on-the-destination-server"></a>O que acontece com os arquivos existentes no servidor de destino?

Ao executar uma transferência, o serviço de migração de armazenamento procura espelhar dados do servidor de origem. O servidor de destino não deve conter dados de produção ou usuários conectados, pois esses dados podem ser substituídos. Por padrão, a primeira transferência faz uma cópia de backup de todos os dados no servidor de destino como uma proteção. Em todas as transferências subsequentes, por padrão, o serviço de migração de armazenamento espelhará os dados no destino; Isso significa não apenas adicionar novos arquivos, mas também substituir arbitrariamente todos os arquivos existentes e excluir os arquivos que não estão presentes na origem. Esse comportamento é intencional e fornece fidelidade perfeita com o computador de origem.

## <a name="what-do-the-error-numbers-mean-in-the-transfer-csv"></a>O que significam os números de erro no CSV de transferência?

A maioria dos erros encontrados no arquivo CSV de transferência são os códigos de erro do sistema Windows. Você pode descobrir o que cada erro significa examinando a [documentação de códigos de erro do Win32](/windows/win32/debug/system-error-codes).

## <a name="what-are-my-options-to-give-feedback-file-bugs-or-get-support"></a><a name="give-feedback"></a>Quais são minhas opções para fornecer comentários, arquivos de erros ou obter suporte?

Para fornecer comentários sobre o serviço de migração de armazenamento:

- Use a ferramenta de Hub de comentários incluída no Windows 10, clicando em "sugerir um recurso" e especificando a categoria do "Windows Server" e a subcategoria de "migração de armazenamento"
- Usar o site [UserVoice do Windows Server](https://windowsserver.uservoice.com)
- Email smsfeed@microsoft.com

Para arquivar bugs:

- Use a ferramenta de Hub de comentários incluída no Windows 10, clicando em "relatar um problema" e especificando a categoria do "Windows Server" e a subcategoria de "migração de armazenamento"
- Abrir um caso de suporte por meio de [suporte da Microsoft](https://support.microsoft.com)

Para obter suporte:

 - Poste uma pergunta na [comunidade técnica do Windows Server](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)
 - Poste no [Fórum do Windows Server 2019](https://docs.microsoft.com/answers/topics/windows-server-2019.html)
 - Abrir um caso de suporte por meio de [suporte da Microsoft](https://support.microsoft.com)

## <a name="additional-references"></a>Referências adicionais

- [Visão geral do serviço de migração de armazenamento](overview.md)
