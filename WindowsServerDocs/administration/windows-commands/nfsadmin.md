---
title: nfsadmin
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7375b2cf-c6b8-45b5-abf6-6c10e462defd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 955f6d93379802444d542ea571f98b69b9191f5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837107"
---
# <a name="nfsadmin"></a>nfsadmin

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar **nfsadmin** para gerenciar o Server for NFS e Client for NFS.  
  
## <a name="syntax"></a>Sintaxe  
**servidor nfsadmin** `[` *computerName*`] [`\-u *nome de usuário*`[`\-p *senha* `]]` \-l  
  
**servidor nfsadmin** `[` *computerName*`] [`\-u *nome de usuário* `[` \-p *senha* `]]` \-r `{` *cliente* `|` todos`}`  
  
**servidor nfsadmin** `[` *computerName*`] [`\-u *nome de usuário* `[` \-p *senha* `]] {`iniciar `|` parar`}`  
  
**servidor nfsadmin** `[` *computerName*`] [`\-u *nome de usuário* `[` \-p *senha* `]]` config *opção*`[...]`  
  
**servidor nfsadmin** `[` *computerName*`] [`\-u *nome de usuário* `[` \-p *senha* `]]` creategroup *nome*  
  
**servidor nfsadmin** `[` *computerName*`] [`\-u *nome de usuário* `[` \-p *senha* `]]` listgroups  
  
**servidor nfsadmin** `[` *computerName*`] [`\-u *nome de usuário* `[` \-p *senha* `]]` deletegroup *nome*  
  
**servidor nfsadmin** `[` *computerName*`] [`\-u *nome de usuário* `[` \-p *senha* `]]` renamegroup *OldName NewName*  
  
**servidor nfsadmin** `[` *computerName*`] [`\-u *nome de usuário* `[` \-p *senha* `]]` addmembers *nome do Host*`[...]`  
  
**servidor nfsadmin** `[` *computerName*`] [`\-u *nome de usuário* `[` \-p *senha* `]]` listmembers  
  
**servidor nfsadmin** `[` *computerName*`] [`\-u *nome de usuário* `[` \-p *senha* `]]` deletemembers *grupo de Host*`[...]`  
  
**nfsadmin client** `[` *computerName*`] [`\-u *nome de usuário* `[` \-p *senha* `]] {`iniciar `|` parar`}`  
  
**nfsadmin client** `[` *computerName*`] [`\-u *nome de usuário* `[` \-p *senha* `]]` config *opção*`[...]`  
  
## <a name="description"></a>Descrição  
O **nfsadmin** comando\-utilitário de linha de administra o Server for NFS ou o Client for NFS no computador local ou remoto executando o Microsoft Services for Network File System \(NFS\). Se você estiver conectado com uma conta que não tem os privilégios necessários, você pode especificar um nome de usuário e senha de uma conta que possua. A ação executada pelo **nfsadmin** depende dos argumentos de comando que você fornecer.  
  
Além de serviço\-argumentos de comando específicos e opções, **nfsadmin** aceita o seguinte:  
  
*computerName*  
Especifica o computador remoto que você deseja administrar. Você pode especificar o computador usando um Windows Internet Name Service \(WINS\) nome ou um sistema de nomes de domínio \(DNS\) ou pelo protocolo de Internet \(IP\) endereço.  
  
**\-u** *UserName*  
Especifica o nome de usuário do usuário cujas credenciais devem ser usados. Talvez seja necessário adicionar o nome de domínio para o nome de usuário na forma *domínio***\\***nome de usuário*  
  
**\-p** *senha*  
Especifica a senha do usuário especificado usando o  **\-u** opção. Se você especificar o  **\-u** opção, mas omita a  **\-p** opção, será solicitada a senha do usuário.  
  
#### <a name="administering-server-for-nfs"></a>Administrando o Server for NFS  
Use o **nfsadmin server** comando para administrar o Server for NFS. A ação específica que **nfsadmin server** leva depende da opção de comando ou o argumento especificado:  
  
**\-l**  
lista todos os bloqueios mantidos por clientes.  
  
**\-r** {*client* | **all**}  
Libera os bloqueios mantidos por um *client* ou, se **todos os** for especificado, todos os clientes.  
  
**Início**  
Inicia o serviço Server for NFS.  
  
**stop**  
Interrompe o serviço Server for NFS.  
  
**config**  
Especifica as configurações gerais para o Server for NFS. Você deve fornecer pelo menos uma das opções a seguir com o **config** argumento de comando:  
  
**mapsvr\=***server*  
Conjuntos *server* como o servidor de mapeamento de nome de usuário para o Server for NFS. Embora essa opção continua a ter suporte para compatibilidade com versões anteriores, você deve usar o **sfuadmin** utilitário em vez disso.  
  
**auditlocation\=**{**eventlog** | **arquivo** | **ambos** | **none** }  
Especifica se os eventos serão auditados e onde os eventos serão registrados. Um dos argumentos a seguir é obrigatório.  
  
**eventlog**  
Especifica que os eventos auditados serão gravados somente do log de eventos de aplicativo do visualizador.  
  
**file**  
Especifica que os eventos auditados serão registrados apenas no arquivo especificado por **fname config**.  
  
**both**  
Especifica que os eventos auditados serão gravados no log do Visualizador de eventos do aplicativo, bem como o arquivo especificado por **fname config**.  
  
**None**  
Especifica que eventos não serão auditados.  
  
**fname\=***file*  
Define o arquivo especificado por *arquivo* como o arquivo de auditoria. O padrão é % sfudir\\log\\nfssvr.log  
  
**fsize\=**\=*size*  
Conjuntos *tamanho* como o tamanho máximo em megabytes, do arquivo de auditoria. O tamanho máximo padrão é 7 MB.  
  
**auditar\=**\[**\+**|**\-**\]**montar** \[ **\+** | **\-** \] **ler** \[ **\+** | **\-** \] **gravar** \[ **\+** | **\-** \] **crie** \[ **\+** | **\-** \] **excluir** \[ **\+** | **\-** \] **bloqueio** \[ **\+** | **\-** \] **todos**  
Especifica os eventos a serem registrados. Para começar a registrar um evento, digite um sinal de adição \( **\+** \) antes do nome do evento; para parar o log de um evento, digite um sinal de subtração \( **\-** \) antes do nome do evento. Se o sinal for omitido, o sinal será assumido. Não use **todos os** com qualquer outro nome de evento.  
  
**lockperiod\=***seconds*  
Especifica o número de segundos que o Server for NFS irá esperar para recuperar os bloqueios após uma conexão para o Server for NFS foi perdido e, em seguida, restabelecida ou após o serviço Server for NFS foi reiniciado.  
  
Portmapprotocol\={TCP | UDP | TCP\+UDP  
Especifica quais protocolos de transporte Portmap dá suporte. A configuração padrão é **TCP\+UDP**.  
  
mountprotocol\={TCP | UDP | TCP\+UDP}  
Especifica qual transporte protocolos montagem dá suporte. A configuração padrão é **TCP\+UDP**.  
  
nfsprotocol\={TCP | UDP | TCP\+UDP}  
Especifica qual transporte protocolos Network File System \(NFS\) dá suporte. A configuração padrão é **TCP\+UDP**  
  
nlmprotocol\={TCP | UDP | TCP\+UDP}  
Especifica qual transporte protocolos NLM \(NLM\) dá suporte. A configuração padrão é **TCP\+UDP**.  
  
nsmprotocol\={TCP | UDP | TCP\+UDP}  
Especifica qual transporte protocolos Gerenciador de Status de rede \(NSM\) dá suporte. A configuração padrão é **TCP\+UDP**.  
  
**enableV3\=**{**yes** | **no**}  
Especifica se os protocolos do NFS versão 3 terá suporte. A configuração padrão é **Sim**.  
  
**renewauth\=**{**yes** | **no**}  
Especifica se as conexões de cliente será necessárias para ser autenticado novamente após o período especificado por **config renewauthinterval**. A configuração padrão é **nenhum**.  
  
**renewauthinterval\=***seconds*  
Especifica o número de segundos decorridos antes que um cliente é forçado a ser autenticado novamente se **config renewauth** é definido como **Sim**. O valor padrão é 600 segundos.  
  
**dircache\=***size*  
Especifica o tamanho em quilobytes, do cache do diretório. O número especificado como *tamanho* deve ser um múltiplo de 4 entre 4 e 128. O diretório padrão\-tamanho do cache é 128 KB.  
  
**translationfile**\=\[file\]  
Especifica um arquivo que contém informações de mapeamento para a substituição de caracteres em nomes de arquivos ao movê-los do Windows\-com base em UNIX\-com base em sistemas de arquivos. Se *arquivo* não for especificado, em seguida, a conversão de caracteres de nome de arquivo está desabilitado. Se o valor de **translationfile** é alterado, você deve reiniciar o servidor para que a alteração tenha efeito.  
  
**dotfileshidden**\={**yes** | **nenhum**}  
Especifica se os arquivos que são criados com nomes que começam com um período \(.\) será marcado como oculto no sistema de arquivos do Windows e, consequentemente, ocultados dos clientes NFS. A configuração padrão é **nenhum**.  
  
**casesensitivelookups\=**{**yes** | **nenhum**}  
Especifica se as pesquisas de diretório serão diferencia maiusculas de minúsculas \(exigir que uma correspondência exata do caso de caractere\).  
  
Você também precisará desabilitar o caso de kernel do Windows\-diferenciação em ordem para o Server for NFS para dar suporte ao caso\-nomes de arquivos confidenciais. Você pode desabilitar o caso de kernel do Windows\-não diferenciação, limpando a seguinte chave do registro como 0:  
  
HKLM\\SYSTEM\\CurrentControlSet\\controle\\Gerenciador de sessão\\kernel  
  
DWOrd  obcaseinsensitive   
  
> [!IMPORTANT]  
> Esta seção se aplica somente ao Windows Server 2008 R2, Windows Server 2008 e Windows Server 2003. Esta seção não se aplica ao Windows Server 2012 R2 ou Windows Server 2012.  
  
**ntfscase\=**{**lower** | **upper** | **preserve**}  
Especifica se o caso de caracteres nos nomes de arquivos no sistema de arquivos NTFS será retornado em letras minúsculas, letras maiusculas, ou no formulário armazenados no diretório. A configuração padrão é **preservar**. Essa configuração não pode ser alterada se **casesensitivelookups** é definido como **Sim**.  
  
**creategroup** *name*  
cria um novo grupo de clientes, dando a ele especificado *nome*.  
  
**listgroups**  
Exibe os nomes de todos os grupos de cliente.  
  
**deletegroup** *name*  
Remove o grupo de cliente especificado pelo *nome*.  
  
**renamegroup** *OldName NewName*  
Altera o nome do grupo do cliente especificado pelo *OldName* para *NewName*  
  
**AddMembers** *nome do Host*\[...\]  
adiciona *Host* ao grupo de cliente especificado por *nome*.  
  
**listmembers** *Name*  
lista os computadores de host no grupo de cliente especificado pelo *nome*.  
  
**deletemembers** *grupo de Host*\[...\]  
Remove o cliente especificado pelo *Host* do grupo de cliente especificado por *grupo*.  
  
Se você não especificar uma opção de comando ou um argumento, **nfsadmin server** exibe o servidor atual para definições de configuração do NFS.  
  
#### <a name="administering-client-for-nfs"></a>Administrando o Client for NFS  
Use o **nfsadmin client** comando para administrar o Client for NFS. A ação específica que **nfsadmin client** leva depende do argumento de comando especificado:  
  
**Início**  
Inicia o serviço Client for NFS.  
  
**stop**  
Interrompe o serviço Client for NFS.  
  
**config**  
Especifica as configurações gerais do Client for NFS. Você deve fornecer pelo menos uma das opções a seguir com o **config** argumento de comando:  
  
**FileAccess\=* * * modo*  
-   Especifica o modo de permissão padrão para arquivos criados no sistema de arquivos de rede \(NFS\) servidores. O *modo* argumento consiste em um três dígitos de 0 a 7 \(inclusivo\) que representa as permissões padrão concedidas ao usuário, grupo e outros \(respectivamente\). Os dígitos traduzem para UNIX\-permissões de estilo da seguinte maneira: 0\=nenhum, 1\=x 2\=w, 3\=wx, 4\=r, 5\=rx, 6\=rw e 7\=rwx. Por exemplo, **fileaccess\=750** dá permissões rwx para o proprietário, permissão de rx ao grupo e nenhuma permissão de acesso a outras pessoas.  
  
**mapsvr\=***server*  
Conjuntos *server* como o servidor de mapeamento de nome de usuário do Client for NFS. Embora essa opção continua a ter suporte para compatibilidade com versões anteriores, você deve usar o **sfuadmin** utilitário em vez disso.  
  
**mtype\=**{**hard** | **soft**}  
Especifica o tipo de montagem padrão. Para uma montagem rígida, Client for NFS continuará a tentar novamente um RPC com falha até obter êxito. Para uma montagem flexível, Client for NFS retorna falha para o aplicativo de chamada após a chamada de repetir o número de vezes especificado pela **Repita** opção.  
  
**retry\=***number*  
Especifica o número de vezes para estabelecer uma conexão para uma montagem flexível. Esse valor deve ser de 1 a 10, inclusive. O padrão é 1.  
  
**timeout\=***seconds*  
Especifica o número de segundos a aguardar uma conexão \(chamada de procedimento remoto\). Esse valor deve ser 0,8, 0,9 ou um inteiro de 1 a 60, inclusivo. O padrão é 0,8.  
  
**Protocol\={TCP | UDP | TCP\+UDP}**  
Especifica qual transporte protocolos o oferece suporte ao cliente. A configuração padrão é **TCP\+UDP**  
  
**rsize\=***size*  
Especifica o tamanho, em quilobytes, do buffer de leitura. Esse valor pode ser 0,5, 1, 2, 4, 8, 16 ou 32. O padrão é 32.  
  
**wsize\=***size*  
Especifica o tamanho, em quilobytes, do buffer de gravação. Esse valor pode ser 0,5, 1, 2, 4, 8, 16 ou 32. O padrão é 32.  
  
**perf\=default**  
Restaura as seguintes configurações de desempenho para os valores padrão:  
  
-   **mtype**  
  
-   **retry**  
  
-   **timeout**  
  
-   **rsize**  
  
-   **wsize**  
  
**FileAccess\=* * * modo*  
Especifica o modo de permissão padrão para arquivos criados no sistema de arquivos de rede \(NFS\) servidores. O *modo* argumento consiste em um três dígitos de 0 a 7 \(inclusivo\) que representa as permissões padrão concedidas ao usuário, grupo e outros \(respectivamente\). Os dígitos traduzem para UNIX\-permissões de estilo da seguinte maneira: 0\=nenhum, 1\=x 2\=w, 3\=wx, 4\=r, 5\=rx, 6\=rw e 7\=rwx. Por exemplo, **fileaccess\=750** dá permissões rwx para o proprietário, permissão de rx ao grupo e nenhuma permissão de acesso a outras pessoas.  
  
Se você não especificar uma opção de comando ou um argumento, **nfsadmin client** exibe o cliente atual para definições de configuração do NFS.  
  

