---
title: nfsadmin
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2658cf610e4328d382b9224f4230d68a022d1cc3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373224"
---
# <a name="nfsadmin"></a>nfsadmin

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar o **nfsadmin** para gerenciar o servidor NFS e o Client for NFS.  
  
## <a name="syntax"></a>Sintaxe  
**nfsadmin server** `[`*ComputerName*`] [`\-u *UserName*`[`\-p *password*`]]` \-l  
  
**nfsadmin server** `[`*ComputerName*`] [`\-u *UserName* `[`\-p *password*`]]` \-r `{`*cliente* `|` todos`}`  
  
**nfsadmin server** `[`*ComputerName*`] [`\-u *UserName* `[`\-p *senha*`]] {`Start `|` Stop`}`  
  
**nfsadmin server** `[`*ComputerName*`] [`\-u *UserName* `[`\-p *password*`]]` config *Option*`[...]`  
  
**nfsadmin server** `[`*ComputerName*`] [`\-u username `[`\-p *password*`]]` criar *nome* do *usuário*  
  
**nfsadmin server** `[`*ComputerName*`] [`\-u *UserName* `[`\-p *password*`]]` listgroups  
  
**nfsadmin server** `[`*ComputerName*`] [`\-u nomedeusuário `[`\-p *senha*`]]` excluir *nome* do *usuário*  
  
**nfsadmin server** `[`*ComputerName*`] [`\-u *nomedeusuário* `[`\-p *senha*`]]` renomear *OldName NewName*  
  
**nfsadmin server** `[`*ComputerName*`] [`\-u *UserName* `[`\-p *senha*`]]` addmembers *Name host*`[...]`  
  
**nfsadmin server** `[`*ComputerName*`] [`\-u *UserName* `[`\-p *password*`]]` listmembers  
  
**nfsadmin server** `[`*ComputerName*`] [`\-u *UserName* `[`\-p *password*`]]` host do *grupo* deletemembers`[...]`  
  
**nfsadmin client** `[`*ComputerName*`] [`\-u *UserName* `[`\-p *senha*`]] {`Start `|` Stop`}`  
  
**nfsadmin client** `[`*ComputerName*`] [`\-u *UserName* `[`\-p *password*`]]` config *Option*`[...]`  
  
## <a name="description"></a>Descrição  
O utilitário de linha de\-de comando **nfsadmin** administra o Server for NFS ou Client for NFS no computador local ou remoto executando os serviços da Microsoft para o sistema de arquivos de rede \(NFS\). Se você estiver conectado com uma conta que não tem os privilégios necessários, poderá especificar um nome de usuário e senha de uma conta que o faça. A ação executada pelo **nfsadmin** depende dos argumentos de comando fornecidos por você.  
  
Além do serviço\-argumentos e opções de comando específicos, o **nfsadmin** aceita o seguinte:  
  
*computerName*  
Especifica o computador remoto que você deseja administrar. Você pode especificar o computador usando um serviço de cadastramento na Internet do Windows \(o nome do\) WINS ou um sistema de nomes de domínio \(nome do\) DNS ou pelo protocolo IP \(endereço\).  
  
**\-u** *nome de usuário*  
Especifica o nome de usuário do usuário cujas credenciais devem ser usadas. Pode ser necessário adicionar o nome de domínio ao nome de usuário no formato <em>domínio</em> **\\** <em>nome</em>  
  
*senha* do **\-p**  
Especifica a senha do usuário especificado usando a opção **\-u** . Se você especificar a opção **\-u** , mas omitir a opção **\-p** , será solicitada a senha do usuário.  
  
#### <a name="administering-server-for-nfs"></a>Administrando o servidor para NFS  
Use o comando **nfsadmin server** para administrar o servidor para NFS. A ação específica que o **servidor nfsadmin** usa depende da opção de comando ou do argumento que você especificar:  
  
**\-l**  
lista todos os bloqueios mantidos pelos clientes.  
  
**\-r** {*cliente* | **todos**}  
Libera os bloqueios mantidos por um *cliente* ou, se **todos** forem especificados, por todos os clientes.  
  
**start**  
inicia o servidor para o serviço NFS.  
  
**stop**  
Interrompe o serviço do servidor NFS.  
  
**config**  
Especifica as configurações gerais do servidor para NFS. Você deve fornecer pelo menos uma das seguintes opções com o argumento de comando **config** :  
  
**mapsvr\=** <em>Server</em>  
Define o *servidor* como o servidor de mapeamento de nomes de usuário do servidor para NFS. Embora essa opção continue a ter suporte para compatibilidade com versões anteriores, você deve usar o utilitário **sfuadmin** em vez disso.  
  
**auditlocation\=** {**eventlog** | **arquivo** | **ambos** | **nenhum**}  
Especifica se os eventos serão auditados e onde os eventos serão registrados. Um dos argumentos a seguir é necessário.  
  
**log**  
Especifica que os eventos auditados serão registrados apenas no log do aplicativo Visualizador de Eventos.  
  
**file**  
Especifica que os eventos auditados serão registrados somente no arquivo especificado por **config fname**.  
  
**mesmo**  
Especifica que os eventos auditados serão registrados no log do aplicativo Visualizador de Eventos, bem como o arquivo especificado por **config fname**.  
  
**None**  
Especifica que os eventos não serão auditados.  
  
<em>arquivo</em> de **\=fname**  
Define o arquivo especificado pelo *arquivo* como o arquivo de auditoria. O padrão é% sfudir%\\log\\nfssvr. log  
  
**fsize\=** \=*tamanho*  
Define o *tamanho* como o tamanho máximo em megabytes do arquivo de auditoria. O tamanho máximo padrão é 7 MB.  
  
**audit\=** \[ **\+** | **\-** \]**montar** \[ **\+** | **\-** \]**ler** \[ **\+** | **\-** \]**gravação** \[ **\+** | **\-** \]**criar** \[ **\+|\-** \]**excluir** \[ **\+** | **\-** \]**bloqueio** \[ **\+|\-** \]**todos**  
Especifica os eventos a serem registrados. Para iniciar o registro em log de um evento, digite um sinal de adição \( **\+** \) antes do nome do evento; para parar de registrar um evento, digite um sinal de subtração \( **\-** \) antes do nome do evento. Se o sinal for omitido, o sinal de adição será assumido. Não use **todos** com nenhum outro nome de evento.  
  
**lockperiod\=** <em>segundos</em>  
Especifica o número de segundos que o servidor para NFS aguardará para recuperar bloqueios depois que uma conexão com o servidor para NFS for perdida e, em seguida, restabelecida ou depois que o servidor para o serviço NFS for reiniciado.  
  
Portmapprotocol\={TCP | UDP | TCP\+UDP  
Especifica quais protocolos de transporte o portmap dá suporte. A configuração padrão é **TCP\+UDP**.  
  
mountprotocol\={TCP | UDP | TCP\+UDP}  
Especifica a quais protocolos de transporte a montagem oferece suporte. A configuração padrão é **TCP\+UDP**.  
  
nfsprotocol\={TCP | UDP | TCP\+UDP}  
Especifica a quais protocolos de transporte o sistema de arquivos de rede \(NFS\) dá suporte. A configuração padrão é **TCP\+UDP**  
  
nlmprotocol\={TCP | UDP | TCP\+UDP}  
Especifica quais protocolos de transporte o Gerenciador de bloqueio de rede \(NLM\) dá suporte. A configuração padrão é **TCP\+UDP**.  
  
nsmprotocol\={TCP | UDP | TCP\+UDP}  
Especifica quais protocolos de transporte o Gerenciador de status de rede \(NSM\) dá suporte. A configuração padrão é **TCP\+UDP**.  
  
**enableV3\=** {**Sim** | **não**}  
Especifica se os protocolos de NFS versão 3 terão suporte. A configuração padrão é **Sim**.  
  
**renewauth\=** {**Sim** | **não**}  
Especifica se as conexões de cliente precisarão ser reautenticadas após o período especificado por **config renewauthinterval**. A configuração padrão é **não**.  
  
**renewauthinterval\=** <em>segundos</em>  
Especifica o número de segundos decorridos antes que um cliente seja forçado a ser autenticado novamente se a **configuração renewauth** for definida como **Sim**. O valor padrão é 600 segundos.  
  
<em>tamanho</em>\=de dircache  
Especifica o tamanho em kilobytes do cache de diretório. O número especificado como *tamanho* deve ser um múltiplo de 4 entre 4 e 128. O diretório padrão\-tamanho do cache é 128 KB.  
  
arquivo de \[de\=de **conversão** de arquivos\]  
Especifica um arquivo que contém informações de mapeamento para substituir caracteres nos nomes de arquivos ao movê-los do Windows\-com base em sistemas de arquivos baseados em UNIX\-. Se o *arquivo* não for especificado, a conversão de caracteres de nome de arquivo será desabilitada. Se o valor de **translationfile** for alterado, você deverá reiniciar o servidor para que a alteração entre em vigor.  
  
**dotfileshidden**\={**Sim** | **não**}  
Especifica se os arquivos criados com nomes que começam com um período \(.\) será marcado como oculto no sistema de arquivos do Windows e, consequentemente, oculto dos clientes NFS. A configuração padrão é **não**.  
  
**casesensitivelookups\=** {**Sim** | **não**}  
Especifica se as pesquisas de diretório terão distinção entre maiúsculas e minúsculas \(exigir correspondência exata de maiúsculas e minúsculas de caracteres\).  
  
Você também precisa desabilitar o caso do kernel do Windows\-não diferenciação para que o servidor do NFS dê suporte ao caso\-nomes de arquivo confidenciais. Você pode desabilitar o caso do kernel do Windows\-a insensibilidade ao limpar a seguinte chave do registro para 0:  
  
HKLM\\SYSTEM\\CurrentControlSet\\controle\\Session Manager\\kernel  
  
ObCaseInsensitive DWOrd   
  
> [!IMPORTANT]  
> Esta seção se aplica somente ao Windows Server 2008 R2, ao Windows Server 2008 e ao Windows Server 2003. Esta seção não se aplica ao Windows Server 2012 R2 ou ao Windows Server 2012.  
  
**ntfscase\=** **{ | ** **inferior** | **preserve**  
Especifica se o caso de caracteres nos nomes de arquivos no sistema de arquivos NTFS será retornado em letras minúsculas, em letras maiúsculas ou no formato armazenado no diretório. A configuração padrão é **preserve**. Essa configuração não poderá ser alterada se **casesensitivelookups** for definido como **Sim**.  
  
*nome* de criação de um  
Cria um novo grupo de clientes, dando a ele o *nome*especificado.  
  
**listgroups**  
Exibe os nomes de todos os grupos de clientes.  
  
**excluir** *nome* do  
Remove o grupo de clientes especificado pelo *nome*.  
  
**renomear** *OldName NewName*  
altera o nome do grupo de clientes especificado por *OldName* para *NewName*  
  
**addmembers** *nome host*\[...\]  
Adiciona o *host* ao grupo de clientes especificado pelo *nome*.  
  
*nome* do listmembers  
lista os computadores host no grupo de clientes especificado por *nome*.  
  
\[do *host do grupo* de **deletemembers** ...\]  
Remove o cliente especificado pelo *host* do grupo de clientes especificado pelo *grupo*.  
  
Se você não especificar uma opção de comando ou argumento, **nfsadmin server** exibirá o servidor atual para as definições de configuração de NFS.  
  
#### <a name="administering-client-for-nfs"></a>Administrando o cliente para NFS  
Use o comando **nfsadmin client** para administrar o cliente para NFS. A ação específica que o **cliente nfsadmin** usa depende do argumento de comando que você especificar:  
  
**start**  
inicia o cliente para o serviço NFS.  
  
**stop**  
Interrompe o cliente para o serviço NFS.  
  
**config**  
Especifica as configurações gerais para o cliente para NFS. Você deve fornecer pelo menos uma das seguintes opções com o argumento de comando **config** :  
  
<em>modo</em> de\=de FileAccess  
-   Especifica o modo de permissão padrão para arquivos criados no sistema de arquivos de rede \(NFS\) servidores. O argumento *Mode* consiste em um dos três dígitos de 0 a 7 \(inclusive\) que representam as permissões padrão concedidas ao usuário, ao grupo e aos outros \(respectivamente\). Os dígitos são convertidos em permissões de estilo de\-do UNIX da seguinte maneira: 0\=nenhum, 1\=x, 2\=w, 3\=WX, 4\=r, 5\=RX, 6\=RW e 7\=rwx. Por exemplo, **fileaccess\=750** fornece permissão de rwx para o proprietário, permissão de RX para o grupo e nenhuma permissão de acesso para outras pessoas.  
  
**mapsvr\=** <em>Server</em>  
Define o *servidor* como o mapeamento de nomes de usuário Server para o cliente para NFS. Embora essa opção continue a ter suporte para compatibilidade com versões anteriores, você deve usar o utilitário **sfuadmin** em vez disso.  
  
**mtype\=** {**Hard** | **Soft**}  
Especifica o tipo de montagem padrão. Para uma montagem rígida, o cliente para NFS continua tentando novamente um RPC com falha até obter êxito. Para uma montagem reversível, o cliente para NFS retorna falha ao aplicativo de chamada depois de repetir a chamada do número de vezes especificado pela opção de **repetição** .  
  
**repetir\=** <em>número</em>  
Especifica o número de vezes para tentar estabelecer uma conexão para uma montagem flexível. Esse valor deve ser de 1 a 10, inclusive. O padrão é 1.  
  
**tempo limite\=** <em>segundos</em>  
Especifica o número de segundos a aguardar por uma conexão \(chamada de procedimento remoto\). Esse valor deve ser 0,8, 0,9 ou um número inteiro de 1 a 60, inclusive. O padrão é 0,8.  
  
**Protocolo\={TCP | UDP | TCP\+UDP}**  
Especifica quais protocolos de transporte o cliente dá suporte. A configuração padrão é **TCP\+UDP**  
  
<em>tamanho</em>\=de rsize  
Especifica o tamanho, em kilobytes, do buffer de leitura. Esse valor pode ser 0,5, 1, 2, 4, 8, 16 ou 32. O padrão é 32.  
  
<em>tamanho</em>\=de wSize  
Especifica o tamanho, em kilobytes, do buffer de gravação. Esse valor pode ser 0,5, 1, 2, 4, 8, 16 ou 32. O padrão é 32.  
  
**desempenho\=padrão**  
Restaura as seguintes configurações de desempenho para valores padrão:  
  
-   **mtype**  
  
-   **Repita**  
  
-   **timeout**  
  
-   **rsize**  
  
-   **wsize**  
  
<em>modo</em> de\=de FileAccess  
Especifica o modo de permissão padrão para arquivos criados no sistema de arquivos de rede \(NFS\) servidores. O argumento *Mode* consiste em um dos três dígitos de 0 a 7 \(inclusive\) que representam as permissões padrão concedidas ao usuário, ao grupo e aos outros \(respectivamente\). Os dígitos são convertidos em permissões de estilo de\-do UNIX da seguinte maneira: 0\=nenhum, 1\=x, 2\=w, 3\=WX, 4\=r, 5\=RX, 6\=RW e 7\=rwx. Por exemplo, **fileaccess\=750** fornece permissão de rwx para o proprietário, permissão de RX para o grupo e nenhuma permissão de acesso para outras pessoas.  
  
Se você não especificar uma opção de comando ou argumento, **nfsadmin client** exibirá as definições de configuração do cliente atual para NFS.  
  

