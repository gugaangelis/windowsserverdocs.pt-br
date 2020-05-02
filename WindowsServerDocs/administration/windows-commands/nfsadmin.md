---
title: nfsadmin
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7375b2cf-c6b8-45b5-abf6-6c10e462defd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c8020b028a046ead36b5f95604cd81d679861746
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723771"
---
# <a name="nfsadmin"></a>nfsadmin

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar o **nfsadmin** para gerenciar o servidor NFS e o Client for NFS.  
  
## <a name="syntax"></a>Sintaxe  
**nfsadmin server** `[` *ComputerName*`] [``[``]]` *UserName* \-u Nome_do_Usuário\-p *senha* l\-  
  
**nfsadmin server** `[` *ComputerName*`] [` \- `{` *client* *Password* `|` *UserName* `]]` u username `[`p senha \-r cliente All\-`}`  
  
**nfsadmin server** `[` *ComputerName*`] [` \- `|` *UserName* `]] {`u username `[`p *senha*Start Stop\-`}`  
  
*opção* **nfsadmin server** `[` *ComputerName*`] [`\-u *username* `[` *Password* p password`]]` config \-`[...]`  
  
**nfsadmin server** `[` *computerName*`] [`ComputerName\-u Nome_do_Usuário *UserName* `[` \-p`]]` *senha* criar *nome* do usuário  
  
**nfsadmin server** `[` *ComputerName*`] [` `[``]]` *UserName* u username \-p *password* listgroups\-  
  
**nfsadmin server** `[` *computerName*`] [`ComputerName\-u username *UserName* `[` \-p`]]` *senha* Delete *nome* do usuário  
  
**nfsadmin server** `[` *ComputerName*`] [` `[` *Password* *UserName* `]]` *OldName NewName* u nomedeusuário \-p senha renomear OldName NewName\-  
  
**nfsadmin server** `[` *ComputerName*`] [` `[` *Password* *UserName* `]]` *Name Host* u username \-p senha addmembers nome host\-`[...]`  
  
**nfsadmin server** `[` *ComputerName*`] [` `[``]]` *UserName* u username \-p *password* listmembers\-  
  
**nfsadmin server** `[` *ComputerName*`] [` `[` *Password* *UserName* `]]` *Group Host* u Nome_do_Usuário \-p senha deletemembers grupo host\-`[...]`  
  
**nfsadmin client** `[` *ComputerName*`] [` \- `|` *UserName* `]] {`u username `[`p *senha*Start Stop\-`}`  
  
**nfsadmin client** `[` *ComputerName*`] [` `[``]]` *Option* *Password* *UserName* u username \-p password config Option\-`[...]`  
  
## <a name="description"></a>Descrição  
O **nfsadmin** utilitário de\-linha de comando nfsadmin administra o servidor para NFS ou cliente para NFS no computador local ou remoto que executa os serviços da Microsoft \(para\)NFS do sistema de arquivos de rede. Se você estiver conectado com uma conta que não tem os privilégios necessários, poderá especificar um nome de usuário e senha de uma conta que o faça. A ação executada pelo **nfsadmin** depende dos argumentos de comando fornecidos por você.  
  
Além dos argumentos e\-opções de comando específicos do serviço, o **nfsadmin** aceita o seguinte:  
  
*computerName*  
Especifica o computador remoto que você deseja administrar. Você pode especificar o computador \(usando um nome WINS\) do serviço de cadastramento na Internet do Windows \(ou\) um nome DNS do sistema de \(nome\) de domínio ou pelo endereço IP do protocolo de Internet.  
  
u nome de *usuário* ** \-**  
Especifica o nome de usuário do usuário cujas credenciais devem ser usadas. Pode ser necessário adicionar o nome de domínio ao nome de usuário no formato <em>domínio</em>**\\**<em>nome</em>  
  
*senha de* p ** \-**  
Especifica a senha do usuário especificado usando a ** \-opção u** . Se você especificar a ** \-opção u** , mas omitir a ** \-opção p** , será solicitada a senha do usuário.  
  
#### <a name="administering-server-for-nfs"></a>Administrando o servidor para NFS  
Use o comando **nfsadmin server** para administrar o servidor para NFS. A ação específica que o **servidor nfsadmin** usa depende da opção de comando ou do argumento que você especificar:  
  
**\-debug**  
lista todos os bloqueios mantidos pelos clientes.  
  
r {*cliente* | **All**} ** \-**  
Libera os bloqueios mantidos por um *cliente* ou, se **todos** forem especificados, por todos os clientes.  
  
**start**  
inicia o servidor para o serviço NFS.  
  
**stop**  
Interrompe o serviço do servidor NFS.  
  
**configuração**  
Especifica as configurações gerais do servidor para NFS. Você deve fornecer pelo menos uma das seguintes opções com o argumento de comando **config** :  
  
<em>servidor</em> **mapsvr\=**  
Define o *servidor* como o servidor de mapeamento de nomes de usuário do servidor para NFS. Embora essa opção continue a ter suporte para compatibilidade com versões anteriores, você deve usar o utilitário **sfuadmin** em vez disso.  
  
**auditlocation\=**{**eventlog** | **file**arquivo | **both**EventLog não é**nenhum**} |   
Especifica se os eventos serão auditados e onde os eventos serão registrados. Um dos argumentos a seguir é necessário.  
  
**log**  
Especifica que os eventos auditados serão registrados apenas no log do aplicativo Visualizador de Eventos.  
  
**Grupo**  
Especifica que os eventos auditados serão registrados somente no arquivo especificado por **config fname**.  
  
**mesmo**  
Especifica que os eventos auditados serão registrados no log do aplicativo Visualizador de Eventos, bem como o arquivo especificado por **config fname**.  
  
**nenhum**  
Especifica que os eventos não serão auditados.  
  
<em>arquivo</em> **fname\=**  
Define o arquivo especificado pelo *arquivo* como o arquivo de auditoria. O padrão é% sfudir%\\log\\nfssvr. log  
  
**\=**\=*tamanho* de fsize  
Define o *tamanho* como o tamanho máximo em megabytes do arquivo de auditoria. O tamanho máximo padrão é 7 MB.  
  
**auditoria\=** \] **create** **all** **mount** **read** **write** **locking** **delete** Mount leitura gravação criar \[exclusão **\+** \[ **\+** \] | **\-** \[ | **\-** \] **\-** | **\+** \[ \] **\-** | **\+** \[ \] **\-** | **\+** \[\]**\-**|**\+**\[ **\+** | **\-** \]  
Especifica os eventos a serem registrados. Para iniciar o registro em log de um evento, \( **\+** \) digite um sinal de adição antes do nome do evento; para parar de registrar um evento, digite um sinal \( **\-** \) de subtração antes do nome do evento. Se o sinal for omitido, o sinal de adição será assumido. Não use **todos** com nenhum outro nome de evento.  
  
**lockperiod\=**<em>segundos</em>  
Especifica o número de segundos que o servidor para NFS aguardará para recuperar bloqueios depois que uma conexão com o servidor para NFS for perdida e, em seguida, restabelecida ou depois que o servidor para o serviço NFS for reiniciado.  
  
Portmapprotocol\={TCP | UDP | UDP\+TCP  
Especifica quais protocolos de transporte o portmap dá suporte. A configuração padrão é **TCP\+UDP**.  
  
mountprotocol\={TCP | UDP | TCP\+UDP}  
Especifica a quais protocolos de transporte a montagem oferece suporte. A configuração padrão é **TCP\+UDP**.  
  
nfsprotocol\={TCP | UDP | TCP\+UDP}  
Especifica a quais protocolos de transporte o \(NFS\) do sistema de arquivos de rede dá suporte. A configuração padrão é **TCP\+UDP**  
  
nlmprotocol\={TCP | UDP | TCP\+UDP}  
Especifica a quais protocolos de transporte o \(NLM\) do Gerenciador de bloqueio de rede dá suporte. A configuração padrão é **TCP\+UDP**.  
  
nsmprotocol\={TCP | UDP | TCP\+UDP}  
Especifica quais protocolos de transporte o NSM \(\) do Gerenciador de status de rede dá suporte. A configuração padrão é **TCP\+UDP**.  
  
**enableV3\=**{**Sim** | **não**}  
Especifica se os protocolos de NFS versão 3 terão suporte. A configuração padrão é **Sim**.  
  
**renewauth\=**{**Sim** | **não**}  
Especifica se as conexões de cliente precisarão ser reautenticadas após o período especificado por **config renewauthinterval**. A configuração padrão é **não**.  
  
**renewauthinterval\=**<em>segundos</em>  
Especifica o número de segundos decorridos antes que um cliente seja forçado a ser autenticado novamente se a **configuração renewauth** for definida como **Sim**. O valor padrão é de 600 segundos.  
  
<em>tamanho</em> do **dircache\=**  
Especifica o tamanho em kilobytes do cache de diretório. O número especificado como *tamanho* deve ser um múltiplo de 4 entre 4 e 128. O tamanho do\-cache do diretório padrão é 128 KB.  
  
**arquivo de conversão**\=\[\]  
Especifica um arquivo que contém informações de mapeamento para substituir caracteres nos nomes de arquivos ao movê-los\-do Windows com\-base em sistemas de arquivos baseados em UNIX. Se o *arquivo* não for especificado, a conversão de caracteres de nome de arquivo será desabilitada. Se o valor de **translationfile** for alterado, você deverá reiniciar o servidor para que a alteração entre em vigor.  
  
**dotfileshidden**\={**Sim** | **não**}  
Especifica se os arquivos criados com nomes começam com um ponto \(. \) será marcado como oculto no sistema de arquivos do Windows e, consequentemente, oculto dos clientes NFS. A configuração padrão é **não**.  
  
**casesensitivelookups\=**{**Sim** | **não**}  
Especifica se as pesquisas de diretório diferenciarão maiúsculas de minúsculas, \(exigindo\)a correspondência exata do caso de caractere.  
  
Você também precisa desabilitar a diferenciação de\-maiúsculas e minúsculas do Windows para que o servidor\-NFS dê suporte a nomes de arquivo com distinção de maiúsculas e minúsculas Você pode desabilitar a não diferenciação de maiúsculas e minúsculas\-do kernel do Windows limpando a seguinte chave do registro para 0:  
  
Kernel\\do\\Gerenciador\\\\de\\sessão de controle de CurrentControlSet do sistema HKLM  
  
ObCaseInsensitive DWOrd   
  
> [!IMPORTANT]  
> Esta seção se aplica somente ao Windows Server 2008 R2, ao Windows Server 2008 e ao Windows Server 2003. Esta seção não se aplica ao Windows Server 2012 R2 ou ao Windows Server 2012.  
  
**ntfscase\=**{**lower** | **upper****preservação**superior | inferior}  
Especifica se o caso de caracteres nos nomes de arquivos no sistema de arquivos NTFS será retornado em letras minúsculas, em letras maiúsculas ou no formato armazenado no diretório. A configuração padrão é **preserve**. Essa configuração não poderá ser alterada se **casesensitivelookups** for definido como **Sim**.  
  
**creategroup** *nome* de criação de um  
Cria um novo grupo de clientes, dando a ele o *nome*especificado.  
  
**listgroups**  
Exibe os nomes de todos os grupos de clientes.  
  
**excluir** *nome* do  
Remove o grupo de clientes especificado pelo *nome*.  
  
**renomear** *OldName NewName*  
altera o nome do grupo de clientes especificado por *OldName* para *NewName*  
  
\[host de *nome* **addmembers** ...\]  
Adiciona o *host* ao grupo de clientes especificado pelo *nome*.  
  
**listmembers** *nome* do listmembers  
lista os computadores host no grupo de clientes especificado por *nome*.  
  
**deletemembers** *Group Host*host\[do grupo de deletemembers...\]  
Remove o cliente especificado pelo *host* do grupo de clientes especificado pelo *grupo*.  
  
Se você não especificar uma opção de comando ou argumento, **nfsadmin server** exibirá o servidor atual para as definições de configuração de NFS.  
  
#### <a name="administering-client-for-nfs"></a>Administrando o cliente para NFS  
Use o comando **nfsadmin client** para administrar o cliente para NFS. A ação específica que o **cliente nfsadmin** usa depende do argumento de comando que você especificar:  
  
**start**  
inicia o cliente para o serviço NFS.  
  
**stop**  
Interrompe o cliente para o serviço NFS.  
  
**configuração**  
Especifica as configurações gerais para o cliente para NFS. Você deve fornecer pelo menos uma das seguintes opções com o argumento de comando **config** :  
  
<em>modo</em> **FileAccess\=**  
-   Especifica o modo de permissão padrão para arquivos criados em servidores NFS \(\) do sistema de arquivos de rede. O argumento *Mode* consiste em um dos três dígitos de 0 a \(7\) , inclusive representando as permissões padrão concedidas ao usuário, ao \(grupo\)e a outros, respectivamente. Os dígitos são convertidos\-em permissões de estilo do UNIX\=da seguinte maneira\=: 0 nenhum\=, 1 x\=, 2 w\=, 3 WX\=, 4 r\=, 5 RX,\=6 RW e 7 rwx. Por exemplo, **o\=FileAccess 750** fornece permissão de rwx para o proprietário, permissão de RX para o grupo e nenhuma permissão de acesso para outras pessoas.  
  
<em>servidor</em> **mapsvr\=**  
Define o *servidor* como o mapeamento de nomes de usuário Server para o cliente para NFS. Embora essa opção continue a ter suporte para compatibilidade com versões anteriores, você deve usar o utilitário **sfuadmin** em vez disso.  
  
**mtype\=**{**Hard** | **Soft**}  
Especifica o tipo de montagem padrão. Para uma montagem rígida, o cliente para NFS continua tentando novamente um RPC com falha até obter êxito. Para uma montagem reversível, o cliente para NFS retorna falha ao aplicativo de chamada depois de repetir a chamada do número de vezes especificado pela opção de **repetição** .  
  
<em>número</em> de **repetições\=**  
Especifica o número de vezes para tentar estabelecer uma conexão para uma montagem flexível. Esse valor deve ser de 1 a 10, inclusive. O padrão é 1.  
  
**tempo\=limite**em<em>segundos</em>  
Especifica o número de segundos de espera para uma chamada \(\)de procedimento remoto de conexão. Esse valor deve ser 0,8, 0,9 ou um número inteiro de 1 a 60, inclusive. O padrão é 0,8.  
  
**Protocolo\={TCP | UDP | TCP\+UDP}**  
Especifica quais protocolos de transporte o cliente dá suporte. A configuração padrão é **TCP\+UDP**  
  
<em>tamanho</em> do **rsize\=**  
Especifica o tamanho, em kilobytes, do buffer de leitura. Esse valor pode ser 0,5, 1, 2, 4, 8, 16 ou 32. O padrão é 32.  
  
<em>tamanho</em> do **wSize\=**  
Especifica o tamanho, em kilobytes, do buffer de gravação. Esse valor pode ser 0,5, 1, 2, 4, 8, 16 ou 32. O padrão é 32.  
  
**padrão\=de desempenho**  
Restaura as seguintes configurações de desempenho para valores padrão:  
  
-   **mtype**  
  
-   **Repita**  
  
-   **timeout**  
  
-   **rsize**  
  
-   **wsize**  
  
<em>modo</em> **FileAccess\=**  
Especifica o modo de permissão padrão para arquivos criados em servidores NFS \(\) do sistema de arquivos de rede. O argumento *Mode* consiste em um dos três dígitos de 0 a \(7\) , inclusive representando as permissões padrão concedidas ao usuário, ao \(grupo\)e a outros, respectivamente. Os dígitos são convertidos\-em permissões de estilo do UNIX\=da seguinte maneira\=: 0 nenhum\=, 1 x\=, 2 w\=, 3 WX\=, 4 r\=, 5 RX,\=6 RW e 7 rwx. Por exemplo, **o\=FileAccess 750** fornece permissão de rwx para o proprietário, permissão de RX para o grupo e nenhuma permissão de acesso para outras pessoas.  
  
Se você não especificar uma opção de comando ou argumento, **nfsadmin client** exibirá as definições de configuração do cliente atual para NFS.  
  

