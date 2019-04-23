---
title: Perguntas frequentes (FAQ) do serviço de migração de armazenamento
description: Perguntas frequentes sobre o serviço de migração de armazenamento, como quais arquivos são excluídos do transferências ao migrar de um servidor para outro.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 11/06/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: df03f722b7b36a163693f675a2eaade2fabeb82f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860907"
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
    - Caminho
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

## <a name="transfer-threads"></a> Posso aumentar o número de arquivos que a cópia simultaneamente?

O serviço de Proxy de serviço de migração de armazenamento copia 8 arquivos simultaneamente em um determinado trabalho. Esse serviço é executado sobre o orchestrator durante a transferência se os computadores de destino Windows Server 2012 R2 ou Windows Server 2016, mas também é executado em todos os nós de destino do Windows Server 2019. Você pode aumentar o número de threads de cópia simultâneas, ajustando o seguinte nome de valor REG_DWORD do registro no formato decimal em cada nó que está executando o Proxy de SMS:

    HKEY_Local_Machine\Software\Microsoft\SMSProxy
    FileTransferThreadCount

 O intervalo válido é de 1 a 128, no Windows Server 2019. 

 Depois de alterar você deve reiniciar o serviço de Proxy de serviço de migração de armazenamento no partipating de todos os computadores em uma migração. Planejamos aumentar esse número em uma versão futura do serviço de migração de armazenamento.

## <a name="non-windows"></a> Pode migrar de fontes diferentes do Windows Server?

A versão de serviço de migração de armazenamento fornecida no Windows Server 2019 dá suporte à migração do Windows Server 2003 e sistemas operacionais posteriores. Atualmente, ele não pode migrar do Linux, Samba, NetApp, EMC ou outros dispositivos de armazenamento SAN e NAS. Estamos planejando permitir isso em uma versão futura do serviço de migração de armazenamento, começando com o suporte a Linux Samba.

## <a name="previous-versions"></a> Posso migrar de versões anteriores dos arquivos?

A versão de serviço de migração de armazenamento fornecida no Windows Server 2019 não dá suporte a migrar as versões anteriores (feitas com o serviço de cópias de sombra de volume) de arquivos. Somente a versão atual serão migrados. 

## <a name="ntfs-refs"></a> Pode migrar de NTFS para REFS?

A versão de serviço de migração de armazenamento fornecida no Windows Server 2019 não dá suporte a migração do NTFS para sistemas de arquivos REFS. Você pode migrar do NTFS para NTFS e REFS para ReFS. Isso ocorre por design, devido a muitas diferenças na funcionalidade, metadados e outros aspectos que ReFS não duplique do NTFS. ReFS destina-se como um sistema de arquivos de carga de trabalho do aplicativo, não é um sistema de arquivos gerais. Para obter mais informações, consulte [visão geral do sistema de arquivos resiliente (ReFS)](../refs/refs-overview.md)

## <a name="consolidate-servers"></a> Pode consolidar vários servidores em um servidor?

A versão de serviço de migração de armazenamento fornecida no Windows Server 2019 não dá suporte a consolidação de vários servidores em um servidor. Um exemplo de consolidação seria migrando três servidores de origem separado - que podem ter os mesmos nomes de compartilhamento e caminhos de arquivo local - em um único servidor novo que virtualizados esses caminhos e compartilhamentos para evitar qualquer colisão, ou a sobreposição respondeu todos os três nomes de servidores anteriores e o endereço IP. Poderemos adicionar essa funcionalidade em uma versão futura do serviço de migração de armazenamento.  


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
