---
title: FAQ (perguntas frequentes) sobre o serviço de migração de armazenamento
description: Perguntas frequentes sobre o serviço de migração de armazenamento, como quais arquivos são excluídos das transferências ao migrar de um servidor para outro.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 08/19/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: f086143ae2e02a2d049189ff248e02fc44fe3cb2
ms.sourcegitcommit: e2b565ce85a97c0c51f6dfe7041f875a265b35dd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69584804"
---
# <a name="storage-migration-service-frequently-asked-questions-faq"></a>FAQ (perguntas frequentes) sobre o serviço de migração de armazenamento

Este tópico contém respostas a perguntas frequentes sobre como usar o [serviço de migração de armazenamento](overview.md) para migrar servidores.

## <a name="what-files-and-folders-are-excluded-from-transfers"></a>Quais arquivos e pastas são excluídos das transferências?

O serviço de migração de armazenamento não transferirá arquivos ou pastas que sabemos que podem interferir na operação do Windows. Especificamente, aqui está o que não vamos transferir ou mover para a pasta PreExistingData no destino:

- Windows, arquivos de programas, arquivos de programas (x86), dados de programa, usuários
- $Recycle. bin, recycleer, reciclado, informações de volume do sistema, $UpgDrv $, $SysReset, $Windows. ~ BT, $Windows. ~ LS, Windows. old, inicialização, recuperação, documentos e configurações
- pagefile. sys, hiberfil. sys, arquivo de permuta. sys, winpepge. sys, config. sys, Bootsect. bak, bootmgr, bootnxt
- Quaisquer arquivos ou pastas no servidor de origem que estejam em conflito com as pastas excluídas no destino. <br>Por exemplo, se há uma pasta N:\Windows na origem e ela é mapeada para C:\ volume no destino, ele não será transferido, independentemente do que ele contém, pois ele interferiria com a pasta C:\Windows System no destino.

## <a name="are-domain-migrations-supported"></a>Há suporte para migrações de domínio?

O serviço de migração de armazenamento não permite a migração entre domínios de Active Directory. As migrações entre servidores sempre ingressarão no servidor de destino no mesmo domínio. Você pode usar as credenciais de migração de domínios diferentes na floresta Active Directory. O serviço de migração de armazenamento oferece suporte à migração entre grupos de os.  

## <a name="are-clusters-supported-as-sources-or-destinations"></a>Há suporte para clusters como origens ou destinos?

O serviço de migração de armazenamento não migra atualmente entre clusters no Windows Server 2019. Planejamos adicionar suporte a cluster em uma versão futura do serviço de migração de armazenamento.

## <a name="do-local-groups-and-local-users-migrate"></a>Os grupos locais e os usuários locais são migrados?

Atualmente, o serviço de migração de armazenamento não migra usuários locais ou grupos locais no Windows Server 2019. Planejamos adicionar o suporte de migração de usuário local e de grupo local em uma versão futura do serviço de migração de armazenamento.

## <a name="is-domain-controller-migration-supported"></a>Há suporte para a migração do controlador de domínio?

O serviço de migração de armazenamento não migra atualmente os controladores de domínio no Windows Server 2019. Como alternativa, contanto que você tenha mais de um controlador de domínio no domínio Active Directory, rebaixe o controlador de domínio antes de migrá-lo e, em seguida, promova o destino após a conclusão da redução. Planejamos adicionar o suporte de migração de controlador de domínio em uma versão futura do serviço de migração de armazenamento.

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
    - Criptografar dados
    - Comunicação remota de identidade
    - Infraestrutura
    - Nome
    - Path
    - Com escopo
    - Nome do escopo
    - Descritor de Segurança
    - Cópia de sombra
    - Especial
    - Temporary

## <a name="can-i-consolidate-multiple-servers-into-one-server"></a>Posso consolidar vários servidores em um servidor?

A versão do serviço de migração de armazenamento fornecida no Windows Server 2019 não dá suporte à consolidação de vários servidores em um servidor. Um exemplo de consolidação seria a migração de três servidores de origem separados, que podem ter os mesmos nomes de compartilhamento e caminhos de arquivo locais, em um único servidor novo que virtualizava esses caminhos e compartilhamentos para evitar qualquer sobreposição ou colisão e, em seguida, responder a todos os três nomes de servidores anteriores e endereço IP. Podemos adicionar essa funcionalidade em uma versão futura do serviço de migração de armazenamento. 

## <a name="can-i-migrate-from-sources-other-than-windows-server"></a>Posso migrar de fontes diferentes do Windows Server?

A versão do serviço de migração de armazenamento fornecida no Windows Server 2019 dá suporte à migração do Windows Server 2003 e sistemas operacionais posteriores. Você também pode migrar o armazenamento de um servidor Linux ou dispositivo que usa o samba; para fazer isso, execute o serviço de migração de armazenamento em um servidor que executa o Windows Server, versão 1903 ou posterior.

## <a name="can-i-migrate-previous-file-versions"></a>Posso migrar versões de arquivo anteriores?

A versão do serviço de migração de armazenamento fornecida no Windows Server 2019 não dá suporte à migração de versões anteriores (feitas com o serviço de cópias de sombra de volume) de arquivos. Somente a versão atual será migrada. 

## <a name="optimizing-inventory-and-transfer-performance"></a>Otimizando o desempenho do inventário e da transferência

O serviço de migração de armazenamento contém um mecanismo de leitura e cópia com vários threads chamado serviço de proxy de serviço de migração de armazenamento, que criamos para ser tão rápido, bem como para oferecer uma fidelidade de dados perfeita em muitas ferramentas de cópia de arquivo. Embora a configuração padrão seja ideal para muitos clientes, há maneiras de melhorar o desempenho do SMS durante o inventário e a transferência.

- **Use o Windows Server 2019 para o sistema operacional de destino.** O Windows Server 2019 contém o serviço de proxy de serviço de migração de armazenamento. Quando você instala esse recurso e migra para destinos do Windows Server 2019, todas as transferências funcionam como uma linha de visão direta entre a origem e o destino. Esse serviço é executado no Orchestrator durante a transferência se os computadores de destino forem Windows Server 2012 R2 ou Windows Server 2016, o que significa que o salto duplo de transferências e será muito mais lento. Se houver vários trabalhos em execução com destinos do Windows Server 2012 R2 ou do Windows Server 2016, o orquestrador se tornará um afunilamento. 

- **Altere os threads de transferência padrão.** O serviço de proxy de serviço de migração de armazenamento copia 8 arquivos simultaneamente em um determinado trabalho. Você pode aumentar o número de threads de cópia simultâneas ajustando o seguinte nome de valor REG_DWORD do registro em decimal em cada nó que executa o proxy do SMS:

    HKEY_Local_Machine\Software\Microsoft\SMSProxy FileTransferThreadCount

   O intervalo válido é de 1 a 128 no Windows Server 2019. Após a alteração, você deve reiniciar o serviço de proxy de serviço de migração de armazenamento em todos os computadores que participam de uma migração. Tome cuidado com essa configuração; a configuração mais alta pode exigir núcleos adicionais, desempenho de armazenamento e largura de banda de rede. Configurá-lo muito alto pode levar a um desempenho reduzido em comparação com as configurações padrão. A capacidade de alterar heurísticamente as configurações de thread com base na CPU, na memória, na rede e no armazenamento é planejada para uma versão mais recente do SMS.

- **Adicione núcleos e memória.**  É altamente recomendável que os computadores de origem, Orchestrator e de destino tenham pelo menos dois núcleos de processador ou dois vCPUs, e mais pode ajudar significativamente o desempenho de inventário e transferência, especialmente quando combinado com FileTransferThreadCount (acima). Ao transferir arquivos maiores do que os formatos usuais do Office (gigabytes ou superior), o desempenho de transferência se beneficiará de mais memória do que o mínimo de 2 GB padrão.

- **Criar vários trabalhos.** Ao criar um trabalho com várias fontes de servidor, cada servidor é contatado de modo Serial para inventário, transferência e transição. Isso significa que cada servidor deve concluir sua fase antes do início de outro servidor. Para executar mais servidores em paralelo, basta criar vários trabalhos, com cada trabalho contendo apenas um servidor. O SMS dá suporte a até 100 executando trabalhos simultaneamente, o que significa que um único orquestrador pode paralelizar muitos computadores de destino do Windows Server 2019. Não recomendamos a execução de vários trabalhos paralelos se os computadores de destino forem Windows Server 2016 ou Windows Server 2012 R2, assim como sem o serviço de proxy do SMS em execução no destino, o orquestrador deverá executar todas as transferências e poderá se tornar um atraso. A capacidade de execução de servidores em paralelo dentro de um único trabalho é um recurso que planejamos adicionar em uma versão posterior do SMS.

- **Use o SMB 3 com redes RDMA.** Se estiver transferindo de um computador de origem do Windows Server 2012 ou posterior, o SMB 3. x dá suporte ao modo SMB direto e à rede RDMA. O RDMA move a maior parte do custo da CPU de transferência das CPUs da motherboard para processadores de NIC integrados, reduzindo a latência e a utilização da CPU do servidor. Além disso, redes RDMA, como ROCE e iWARP, normalmente têm largura de banda consideravelmente maior do que o TCP/Ethernet típico, incluindo 25, 50 e velocidades de 100 GB por interface. O uso do SMB Direct normalmente move o limite de velocidade de transferência da rede para o próprio armazenamento.   

- **Use o SMB 3 multicanal.** Se estiver transferindo de um computador de origem do Windows Server 2012 ou posterior, o SMB 3. x oferece suporte a cópias multicanal que podem melhorar muito o desempenho da cópia de arquivos. Esse recurso funciona automaticamente desde que a origem e o destino tenham:

   - Vários adaptadores de rede
   - Um ou mais adaptadores de rede que dão suporte ao dimensionamento lateral de recepção (RSS)
   - Um ou mais adaptadores de rede configurados usando o agrupamento NIC
   - Um ou mais adaptadores de rede com suporte a RDMA

- **Atualize os drivers.** Conforme apropriado, instale o armazenamento do fornecedor mais recente e o firmware e drivers de compartimento, drivers de HBA mais recentes, firmware de BIOS/UEFI mais recente, drivers de rede de fornecedor mais recentes e drivers de chipset de motherboard mais recentes na origem, no destino e no Orchestrator servidores. Reinicie os nós conforme necessário. confira a documentação do fornecedor de hardware para configurar o armazenamento compartilhado e o hardware de rede.

- **Habilite o processamento de alto desempenho.** Verifique se as configurações de BIOS/UEFI para servidores permitem o alto desempenho, como desabilitar C-State, definir a velocidade de QPI, habilitar NUMA e definir a frequência de memória mais alta. Verifique se o gerenciamento de energia no Windows Server está definido como alto desempenho. Reinicie conforme necessário. Não se esqueça de retorná-los para os Estados apropriados depois de concluir a migração. 

- **Ajustar o hardware** Examine as [diretrizes de ajuste de desempenho do Windows server 2016](https://docs.microsoft.com/windows-server/administration/performance-tuning/) para ajustar o Orchestrator e os computadores de destino que executam o windows Server 2019 e o windows Server 2016. A seção de [ajuste de desempenho](https://docs.microsoft.com/windows-server/networking/technologies/network-subsystem/net-sub-performance-tuning-nics) do subsistema de rede contém informações especialmente valiosas.

- **Use um armazenamento mais rápido.** Embora possa ser difícil atualizar a velocidade de armazenamento do computador de origem, você deve garantir que o armazenamento de destino seja pelo menos rápido no desempenho de e/s de gravação, pois a origem está em desempenho de e/s de leitura para garantir que não haja nenhum afunilamento desnecessário nas transferências. Se o destino for uma VM, certifique-se de que, pelo menos, para fins de migração, seja executado na camada de armazenamento mais rápida de seus hosts de hipervisor, como na camada flash ou com Espaços de Armazenamento Diretos clusters HCI utilizando espaços em Flash ou híbridos espelhados. Quando a migração do SMS é concluída, a VM pode ser migrada ao vivo para uma camada ou host mais lento.

- **Atualizar o antivírus.** Sempre verifique se a origem e o destino estão executando a versão mais recente com patches do software antivírus para garantir a sobrecarga mínima de desempenho. Como um teste, você pode excluir *temporariamente* a verificação de pastas que está inventariando ou migrando nos servidores de origem e de destino. Se o desempenho de transferência for melhorado, entre em contato com seu fornecedor de software antivírus para obter instruções ou para obter uma versão atualizada do software antivírus ou uma explicação da degradação de desempenho esperada.

## <a name="can-i-migrate-from-ntfs-to-refs"></a>Posso migrar de NTFS para REFS?

A versão do serviço de migração de armazenamento fornecida no Windows Server 2019 não dá suporte à migração do NTFS para os sistemas de arquivos REFS. Você pode migrar de NTFS para NTFS e REFS para ReFS. Isso é por design, devido às muitas diferenças na funcionalidade, nos metadados e em outros aspectos que o ReFS não duplica do NTFS. ReFS é destinado a um sistema de arquivos de carga de trabalho de aplicativo, não a um sistema de arquivos geral. Para obter mais informações, consulte [visão geral do ReFS (sistema de arquivos resiliente)](../refs/refs-overview.md) 

## <a name="can-i-move-the-storage-migration-service-database"></a>Posso mover o banco de dados do serviço de migração de armazenamento?

O serviço de migração de armazenamento usa um banco de dados ESE (mecanismo de armazenamento extensível) que é instalado por padrão na pasta c:\programdata\microsoft\storagemigrationservice oculta. Esse banco de dados aumentará à medida que os trabalhos forem adicionados e as transferências forem concluídas e poderão consumir espaço em disco significativo após a migração de milhões de arquivos se você não excluir os trabalhos. Se o banco de dados precisar ser movido, execute as seguintes etapas:

1. Pare o serviço de "serviço de migração de armazenamento" no computador do Orchestrator.
2. Apropriar-se `%programdata%/Microsoft/StorageMigrationService` da pasta
3. Adicione sua conta de usuário para ter controle total sobre esse compartilhamento e todos os seus arquivos e subpastas.
4. Mova a pasta para outra unidade no computador do Orchestrator.
5. Defina o seguinte valor REG_SZ do registro:

    HKEY_Local_Machine\Software\Microsoft\SMS DatabasePath = *caminho para a nova pasta de banco de dados em um volume diferente* . 
6. Verifique se o sistema tem controle total para todos os arquivos e subpastas dessa pasta
7. Remova suas próprias permissões de conta.
8. Inicie o serviço "serviço de migração de armazenamento".

## <a name="give-feedback"></a>Quais são minhas opções para fornecer comentários, arquivos de erros ou obter suporte?

Para fornecer comentários sobre o serviço de migração de armazenamento:

- Use a ferramenta de Hub de comentários incluída no Windows 10, clicando em "sugerir um recurso" e especificando a categoria do "Windows Server" e a subcategoria de "migração de armazenamento"
- Usar o site UserVoice do [Windows Server](https://windowsserver.uservoice.com)
- Emailsmsfeed@microsoft.com

Para arquivar bugs:

- Use a ferramenta de Hub de comentários incluída no Windows 10, clicando em "relatar um problema" e especificando a categoria do "Windows Server" e a subcategoria de "migração de armazenamento"
- Abrir um caso de suporte por meio de [suporte da Microsoft](https://support.microsoft.com)

Para obter suporte:

 - Poste uma pergunta na [comunidade técnica do Windows Server](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)
 - Poste no [Fórum do TechNet do Windows Server 2019](https://social.technet.microsoft.com/Forums/en-US/home?forum=ws2019&filter=alltypes&sort=lastpostdesc) 
 - Abrir um caso de suporte por meio de [suporte da Microsoft](https://support.microsoft.com)

## <a name="see-also"></a>Consulte também

- [Visão geral do serviço de migração de armazenamento](overview.md)
