---
title: Perguntas frequentes (FAQ) do serviço de migração de armazenamento
description: Perguntas frequentes sobre o serviço de migração de armazenamento, como quais arquivos são excluídos do transferências ao migrar de um servidor para outro.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 06/04/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: 8f0f16f14ccf9099af8ff8bb8b27209c75c87cfc
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284466"
---
# <a name="storage-migration-service-frequently-asked-questions-faq"></a>Perguntas frequentes (FAQ) do serviço de migração de armazenamento

Este tópico contém respostas para perguntas frequentes (FAQs) sobre como usar [serviço de migração de armazenamento](overview.md) a migração de servidores.

## <a name="excluded-files"></a> Quais arquivos e pastas são excluídas de transferências?

Serviço de migração de armazenamento não transferir arquivos ou pastas que sabemos que poderia interferir na operação do Windows. Especificamente, aqui está o que nós não transferir ou mover para a pasta PreExistingData no destino:

- Arquivos, arquivos de programas (x86), dados do programa, os usuários de programa do Windows,
- $Recycle.bin, recycler, reciclagem, System Volume Information, $ $UpgDrv, $SysReset, $Windows. ~ BT, $Windows. ~ LS, old, inicialização, recuperação, documentos e configurações
- pagefile.sys, hiberfil.sys, swapfile.sys, winpepge.sys, config.sys, bootsect.bak, bootmgr, bootnxt
- Os arquivos ou pastas no servidor de origem que está em conflito com as pastas excluídas no destino. <br>Por exemplo, se houver em uma pasta N:\Windows na origem e é mapeada para o C:\ volume no destino, ele não obtenha transferido — independentemente do que ele contém — porque ela interfira na pasta C:\Windows sistema no destino.

## <a name="domain-migration"></a> Há suporte para migrações de domínio?

Serviço de migração de armazenamento não permite a migração entre domínios do Active Directory. Migrações entre servidores sempre ingressará o servidor de destino ao mesmo domínio. Você pode usar as credenciais de migração de diferentes domínios na floresta do Active Directory. O serviço de migração de armazenamento oferece suporte a migração entre grupos de trabalho.  

## <a name="cluster-support"></a> Clusters têm suporte como origens ou destinos?

Serviço de migração de armazenamento no momento, não migrar entre Clusters no Windows Server 2019. Estamos planejando adicionar suporte a cluster em uma versão futura do serviço de migração de armazenamento.

## <a name="local-principals"></a> Não grupos locais e migrar os usuários locais?

Serviço de migração de armazenamento no momento, não migrar usuários locais ou grupos locais do Windows Server 2019. Estamos planejando adicionar suportam a migração de grupo local e de usuário local em uma versão futura do serviço de migração de armazenamento.

## <a name="domain-controller"></a> Há suporte para migração de controlador de domínio

Serviço de migração de armazenamento no momento, não migrar controladores de domínio no Windows Server 2019. Como alternativa, desde que você tiver mais de um controlador de domínio no domínio do Active Directory, rebaixar o controlador de domínio antes de migrá-la, promova o destino após a conclusão da transferência. Estamos planejando adicionar suporte à migração de controlador de domínio em uma versão futura do serviço de migração de armazenamento.

## <a name="share-attributes"></a> Quais atributos são migrados pelo serviço de migração de armazenamento?

Serviço de armazenamento de migração migra todos os sinalizadores, configurações e segurança dos compartilhamentos SMB. Essa lista de sinalizadores que migra o serviço de migração de armazenamento inclui:

    - Estado de compartilhamento
    - Tipo de disponibilidade
    - Tipo de compartilhamento
    - Modo de enumeração de pasta *(também conhecido como baseada em acesso de enumeração ou ABE)*
    - Modo de cache
    - Modo de leasing
    - Instância de SMB
    - Tempo limite de autoridade de certificação
    - Limite de usuários simultâneos
    - Continuamente disponíveis
    - Descrição           
    - Criptografar dados
    - Comunicação remota de identidade
    - Infraestrutura
    - Nome
    - `Path`
    - No escopo
    - Nome do escopo
    - Descritor de Segurança
    - Cópia de sombra
    - Especiais
    - Temporário

## <a name="move-db"></a> Posso mover o banco de dados do serviço de migração de armazenamento?

O serviço de migração de armazenamento usa um banco de dados de (ESE) do mecanismo de armazenamento extensível que é instalado por padrão na pasta c:\programdata\microsoft\storagemigrationservice oculto. Este banco de dados aumentará conforme os trabalhos são adicionados e as transferências são concluídas e podem consumir o espaço em disco significativo após a migração milhões de arquivos, se você não excluir trabalhos. Se o banco de dados precisa ser movido, execute as seguintes etapas:

1. Interrompa o serviço de "Serviço de migração de armazenamento" no computador do orchestrator.
2. Apropriar-se da `%programdata%/Microsoft/StorageMigrationService` pasta
3. Adicione sua conta de usuário para ter controle total sobre o que compartilhar e todos os seus arquivos e subpastas.
4. Mova a pasta para outra unidade no computador do orchestrator.
5. Defina o seguinte valor REG_SZ de registro:

    DatabasePath HKEY_Local_Machine\Software\Microsoft\SMS = *caminho para a nova pasta de banco de dados em um volume diferente* . 
6. Certifique-se de que o sistema tem controle total para todos os arquivos e subpastas da pasta
7. Remova suas próprias permissões de contas.
8. Inicie o serviço de "Serviço de migração de armazenamento".

## <a name="non-windows"></a> Pode migrar de fontes diferentes do Windows Server?

A versão de serviço de migração de armazenamento fornecida no Windows Server 2019 dá suporte à migração do Windows Server 2003 e sistemas operacionais posteriores. Atualmente, ele não pode migrar do Linux, Samba, NetApp, EMC ou outros dispositivos de armazenamento SAN e NAS. Estamos planejando permitir isso em uma versão futura do serviço de migração de armazenamento, começando com o suporte a Linux Samba.

## <a name="previous-versions"></a> Posso migrar de versões anteriores dos arquivos?

A versão de serviço de migração de armazenamento fornecida no Windows Server 2019 não dá suporte a migrar as versões anteriores (feitas com o serviço de cópias de sombra de volume) de arquivos. Somente a versão atual serão migrados. 

## <a name="ntfs-refs"></a> Pode migrar de NTFS para REFS?

A versão de serviço de migração de armazenamento fornecida no Windows Server 2019 não dá suporte a migração do NTFS para sistemas de arquivos REFS. Você pode migrar do NTFS para NTFS e REFS para ReFS. Isso ocorre por design, devido a muitas diferenças na funcionalidade, metadados e outros aspectos que ReFS não duplique do NTFS. ReFS destina-se como um sistema de arquivos de carga de trabalho do aplicativo, não é um sistema de arquivos gerais. Para obter mais informações, consulte [visão geral do sistema de arquivos resiliente (ReFS)](../refs/refs-overview.md)

## <a name="consolidate-servers"></a> Pode consolidar vários servidores em um servidor?

A versão de serviço de migração de armazenamento fornecida no Windows Server 2019 não dá suporte a consolidação de vários servidores em um servidor. Um exemplo de consolidação seria migrando três servidores de origem separado - que podem ter os mesmos nomes de compartilhamento e caminhos de arquivo local - em um único servidor novo que virtualizados esses caminhos e compartilhamentos para evitar qualquer colisão, ou a sobreposição respondeu todos os três nomes de servidores anteriores e o endereço IP. Poderemos adicionar essa funcionalidade em uma versão futura do serviço de migração de armazenamento.  

## <a name="optimize"></a> Otimizando o desempenho do inventário e de transferência

O serviço de migração de armazenamento contém um mecanismo de cópia chamado serviço de Proxy de serviço de migração de armazenamento que é projetado para ser rápido, bem como trazer a falta de fidelidade de dados perfeito em muitas ferramentas de cópia de arquivo e de leitura com multithread. Enquanto a configuração padrão será ideal para muitos clientes, há maneiras de melhorar o desempenho de SMS durante o inventário e de transferência.

- **Use o Windows Server 2019 para o sistema operacional de destino.** Windows Server 2019 contém o serviço de Proxy de serviço de migração de armazenamento. Quando você instala este recurso e migrar para o Windows Server 2019 destinos, todas as transferências de operam como linha de visão direta entre a origem e destino. Esse serviço é executado o orquestrador durante a transferência se os computadores de destino são o Windows Server 2012 R2 ou Windows Server 2016, o que significa que as transferências de salto duplo e serão muito mais lentos. Se houver vários trabalhos em execução com Windows Server 2012 R2 ou Windows Server 2016 destinos, o orquestrador se tornará um gargalo. 

- **Altere os threads de transferência padrão.** O serviço de Proxy de serviço de migração de armazenamento copia 8 arquivos simultaneamente em um determinado trabalho. Você pode aumentar o número de threads de cópia simultâneas, ajustando o seguinte nome de valor REG_DWORD do registro no formato decimal em cada nó que está executando o Proxy de SMS:

    HKEY_Local_Machine\Software\Microsoft\SMSProxy   FileTransferThreadCount

   O intervalo válido é de 1 a 128, no Windows Server 2019. Depois de alterar, você deve reiniciar o serviço de Proxy de serviço de migração de armazenamento em todos os computadores que participam de uma migração. Tenha cuidado com essa configuração; defini-lo o mais alto pode exigir núcleos adicionais, o desempenho de armazenamento e largura de banda de rede. Defini-lo muito alto pode levar a desempenho reduzido em comparação comparado as configurações padrão. A capacidade de alterar heuristicamente as configurações de threads com base na CPU, memória, rede e armazenamento está planejada para uma versão posterior do SMS.

- **Adicione os núcleos e memória.**  É altamente recomendável que os computadores de origem, o orchestrator e o destino tem pelo menos dois núcleos de processador ou duas vCPUs e significativamente mais podem ajudar a inventário e transferência de desempenho, especialmente quando combinadas com FileTransferThreadCount (acima). Durante a transferência de arquivos que são maiores do que os formatos comuns do Office (gigabytes ou superior) desempenho de transferência irá se beneficiar de mais memória que o mínimo de 2GB padrão.

- **Crie trabalho de várias.** Ao criar um trabalho com várias fontes de servidor, cada servidor é contatado em modo serial para o inventário de transferência e a transferência. Isso significa que cada servidor deve concluir sua fase antes do início de outro servidor. Para executar mais servidores em paralelo, basta crie vários trabalhos, com cada trabalho que contém apenas um servidores. SMS oferece suporte a até 100 simultaneamente a execução de trabalhos, que significa que um único orchestrator pode paralelizar muitos computadores de destino do Windows Server 2019. Não recomendamos a execução de vários trabalhos paralelos se seus computadores de destino são o Windows Server 2016 ou Windows Server 2012 R2 como sem o serviço de proxy SMS em execução no destino, o orquestrador deve executar todas as transferências em si e pode se tornar um gargalo. A capacidade para servidores executar em paralelo dentro de um único trabalho é um recurso que estamos planejando adicionar em uma versão posterior do SMS.

- **Use o SMB 3 com redes RDMA.** Se a transferência de um Windows Server 2012 ou o computador de origem posterior, o modo direto do SMB 3.x dá suporte a SMB e a rede RDMA. RDMA move a maioria dos custo de CPU de transferência da placa-mãe CPUs para processadores NIC integrados, reduzindo a utilização de CPU de latência e o servidor. Além disso, redes RDMA, como ROCE e iWARP normalmente têm substancialmente maior largura de banda que o TCP/ethernet típico, incluindo a 25, 50 e velocidades de 100Gb por interface. Usando o SMB Direct normalmente move o limite de velocidade de transferência da rede até o armazenamento em si.   

- **Use 3 SMB multichannel.** Se transferir de um computador de origem posterior ou o Windows Server 2012, o SMB 3.x dá suporte a vários canais cópias que podem melhorar significativamente a arquivo desempenho da cópia. Esse recurso funciona automaticamente, desde que tenham a origem e destino:

   - Vários adaptadores de rede
   - Um ou mais adaptadores de rede que dão suporte a RSS Receive Side Scaling)
   - Um dos mais adaptadores de rede são configuradas usando o agrupamento NIC
   - Um ou mais adaptadores de rede com suporte a RDMA

- **Atualize os drivers.** Conforme apropriado, instale o armazenamento de fornecedor mais recente e firmware de compartimento e drivers, drivers HBA de fornecedor mais recentes, firmware UEFI/BIOS mais recente do fornecedor, drivers de rede de fornecedor mais recentes e drivers de chipsets da placa-mãe mais recentes na origem, destino e no orchestrator servidores. Reinicie os nós conforme necessário. confira a documentação do fornecedor de hardware para configurar o armazenamento compartilhado e o hardware de rede.

- **Habilite o processamento de alto desempenho.** Verifique se as configurações de BIOS/UEFI para servidores permitem o alto desempenho, como desabilitar C-State, definir a velocidade de QPI, habilitar NUMA e definir a frequência de memória mais alta. Certifique-se de que o gerenciamento de energia no Windows Server é definido como de alto desempenho. Reinicie conforme necessário. Não se esqueça de retornar esses estados apropriado depois de concluir a migração. 

- **Ajustar o hardware** examine as [desempenho ajustando as diretrizes para o Windows Server 2016](https://docs.microsoft.com/windows-server/administration/performance-tuning/) para ajustar o orchestrator e computadores de destino executando o Windows Server 2019 e Windows Server 2016. O [ajuste de desempenho do subsistema de rede](https://docs.microsoft.com/windows-server/networking/technologies/network-subsystem/net-sub-performance-tuning-nics) seção contém informações especialmente valiosas.

- **Use o armazenamento mais rápido.** Embora seja difícil atualizar a velocidade de armazenamento do computador de origem, verifique se que o armazenamento de destino seja pelo menos tão rápido em desempenho de e/s de gravação como a fonte está em desempenho de e/s de leitura para garantir que não há nenhum gargalo desnecessário em transferências. Se o destino for uma máquina virtual, certifique-se de que, pelo menos para fins de migração, ele é executado na camada de armazenamento mais rápida dos seus hosts de hipervisor, como na camada de flash ou com clusters HCI direta de espaços de armazenamento utilizando todos os flash espelhado ou espaços híbrido. Quando a migração de SMS é concluída a VM pode ser migrada dinamicamente para um host ou a camada mais lento.

- **Atualização de antivírus.** Sempre verifique se a origem e destino estão executando a versão corrigida mais recente do software antivírus para garantir que a sobrecarga de desempenho mínimo. Como um teste, você pode *temporariamente* excluir a verificação de pastas estiver inventariando ou migração nos servidores de origem e destino. Se sua transferência de desempenho é aprimorada, entre em contato com seu fornecedor de software antivírus para obter instruções, ou para uma versão atualizada do software antivírus ou obter uma explicação de degradação de desempenho esperados.

## <a name="give-feedback"></a> Quais são minhas opções para enviar comentários, arquivar bugs ou obter suporte?

Para fornecer comentários sobre o serviço de migração de armazenamento:

- Use a ferramenta de Hub de comentários incluída no Windows 10, clicando em "Sugerir um recurso" e especificando a categoria de "Windows Server" e a subcategoria de "Migração de armazenamento"
- Use o [UserVoice do Windows Server](https://windowsserver.uservoice.com) site
- Email smsfeed@microsoft.com

Bugs de arquivo:

- Use a ferramenta de Hub de comentários incluída no Windows 10, clicando em "Relatar um problema" e especificando a categoria de "Windows Server" e a subcategoria de "Migração de armazenamento"
- Abra um caso de suporte por meio de [Microsoft Support](https://support.microsoft.com)

Para obter suporte:

 - Postar uma pergunta sobre o [comunidade de tecnologia do Windows Server](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)
 - Postar no [Fórum do Technet do Windows Server de 2019](https://social.technet.microsoft.com/Forums/en-US/home?forum=ws2019&filter=alltypes&sort=lastpostdesc) 
 - Abra um caso de suporte por meio de [Microsoft Support](https://support.microsoft.com)


## <a name="see-also"></a>Consulte também

- [Visão geral do serviço de migração de armazenamento](overview.md)
