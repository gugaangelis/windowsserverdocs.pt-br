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

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar o **nfsadmin** para gerenciar o servidor NFS e o Client for NFS.  
  
## <a name="syntax"></a>Sintaxe  
**nfsadmin server** `[`*ComputerName*`] [` @ no__t-4U *username*`[` @ no__t-7P *password*`]]` 0L  
  
**nfsadmin server** `[`*ComputerName*`] [` @ no__t-4u *nome de usuário* `[` @ no__t-7P *senha*`]]` 0R 1*cliente* 3 todos os @ no__t-14  
  
**nfsadmin server** `[`*ComputerName*`] [` @ no__t-4U *username* `[` @ no__t-7P *password*`]] {`start 0 Stop @ no__t-11  
  
**nfsadmin server** `[`*ComputerName*`] [` @ no__t-4U *username* `[` @ no__t-7P *password*`]]` config *Option*1  
  
**nfsadmin server** `[`*ComputerName*`] [` @ no__t-4U username `[` @ no__t-7P *password*`]]` *nome* do *usuário* de criação  
  
**nfsadmin server** `[`*ComputerName*`] [` @ no__t-4U *username* `[` @ no__t-7P *password*`]]` listgroups  
  
**nfsadmin server** `[`*ComputerName*`] [` @ no__t-4U username `[` @ no__t-7P *password*`]]` excluir *nome* do *usuário*  
  
**nfsadmin server** `[`*ComputerName*`] [` @ no__t-4u *nome de usuário* `[` @ no__t-7P *senha*`]]` renomear *OldName NewName*  
  
**nfsadmin server** `[`*ComputerName*`] [` @ no__t-4u *nome de usuário* `[` @ no__t-7P *senha*`]]` addmembers *Name host*1  
  
**nfsadmin server** `[`*ComputerName*`] [` @ no__t-4U *username* `[` @ no__t-7P *password*`]]` listmembers  
  
**nfsadmin server** `[`*ComputerName*`] [` @ no__t-4u *nome de usuário* `[` @ no__t-7P *password*`]]` deletemembers host de *grupo*1  
  
**nfsadmin client** `[`*ComputerName*`] [` @ no__t-4u *nome de usuário* `[` @ no__t-7P *password*`]] {`start 0 Stop @ no__t-11  
  
**nfsadmin client** `[`*ComputerName*`] [` @ no__t-4U *username* `[` @ no__t-7P *password*`]]` config *Option*1  
  
## <a name="description"></a>Descrição  
O utilitário **nfsadmin** comando @ no__t-1line administra o servidor para NFS ou cliente para NFS no computador local ou remoto que executa os serviços da Microsoft para o sistema de arquivos de rede \(NFS @ no__t-3. Se você estiver conectado com uma conta que não tem os privilégios necessários, poderá especificar um nome de usuário e senha de uma conta que o faça. A ação executada pelo **nfsadmin** depende dos argumentos de comando fornecidos por você.  
  
Além dos argumentos e opções de comando do serviço @ no__t-0specific, o **nfsadmin** aceita o seguinte:  
  
*computerName*  
Especifica o computador remoto que você deseja administrar. Você pode especificar o computador usando um serviço de nome de Internet do Windows \(WINS @ no__t-1 ou um sistema de nomes de domínio \(DNS @ no__t-3 Name ou pelo endereço IP \(IP @ no__t-5 address.  
  
**@no__t-nome de** *usuário* 1U  
Especifica o nome de usuário do usuário cujas credenciais devem ser usadas. Pode ser necessário adicionar o nome de domínio ao nome de usuário no formato <em>domínio</em> **\\** <em>nome</em>  
  
*senha* **de @no__t 1p**  
Especifica a senha do usuário especificado usando a opção **\-U** . Se você especificar a opção **\-U** , mas omitir a opção **\-P** , será solicitada a senha do usuário.  
  
#### <a name="administering-server-for-nfs"></a>Administrando o servidor para NFS  
Use o comando **nfsadmin server** para administrar o servidor para NFS. A ação específica que o **servidor nfsadmin** usa depende da opção de comando ou do argumento que você especificar:  
  
**\-L**  
lista todos os bloqueios mantidos pelos clientes.  
  
**\-R** {*cliente* | **todos**}  
Libera os bloqueios mantidos por um *cliente* ou, se **todos** forem especificados, por todos os clientes.  
  
**Início**  
inicia o servidor para o serviço NFS.  
  
**stop**  
Interrompe o serviço do servidor NFS.  
  
**config**  
Especifica as configurações gerais do servidor para NFS. Você deve fornecer pelo menos uma das seguintes opções com o argumento de comando **config** :  
  
<em>servidor</em> **mapsvr @ no__t-1**  
Define o *servidor* como o servidor de mapeamento de nomes de usuário do servidor para NFS. Embora essa opção continue a ter suporte para compatibilidade com versões anteriores, você deve usar o utilitário **sfuadmin** em vez disso.  
  
**auditlocation @ no__t-1**{**EventLog** | **file** | **both** | **None**}  
Especifica se os eventos serão auditados e onde os eventos serão registrados. Um dos argumentos a seguir é necessário.  
  
**log**  
Especifica que os eventos auditados serão registrados apenas no log do aplicativo Visualizador de Eventos.  
  
**file**  
Especifica que os eventos auditados serão registrados somente no arquivo especificado por **config fname**.  
  
**mesmo**  
Especifica que os eventos auditados serão registrados no log do aplicativo Visualizador de Eventos, bem como o arquivo especificado por **config fname**.  
  
**None**  
Especifica que os eventos não serão auditados.  
  
<em>arquivo</em> **fname @ no__t-1**  
Define o arquivo especificado pelo *arquivo* como o arquivo de auditoria. O padrão é% sfudir% @no__t -0log\\nfssvr.log  
  
**fsize @ no__t-1**\=*tamanho*  
Define o *tamanho* como o tamanho máximo em megabytes do arquivo de auditoria. O tamanho máximo padrão é 7 MB.  
  
**Audit @ no__t-1**\[ **\+** | **\-** \]**mount** \=0**2**3**5**6**Read** 8**0**1 **@no__ t-23**4**gravação** 6**8**9**1**2**criar** 4**6**7**9**0**delete** 2 **@no__ t-44**5**7**8**locking** 0**2**3**5**6**All**  
Especifica os eventos a serem registrados. Para iniciar o registro em log de um evento, digite um sinal de adição \( **\+** \) antes do nome do evento; para parar de registrar um evento, digite um sinal de subtração \( **\-** \) antes do nome do evento. Se o sinal for omitido, o sinal de adição será assumido. Não use **todos** com nenhum outro nome de evento.  
  
**lockperiod @ no__t-1**<em>segundos</em>  
Especifica o número de segundos que o servidor para NFS aguardará para recuperar bloqueios depois que uma conexão com o servidor para NFS for perdida e, em seguida, restabelecida ou depois que o servidor para o serviço NFS for reiniciado.  
  
Portmapprotocol @ no__t-0 {TCP | UDP | TCP @ no__t-1UDP  
Especifica quais protocolos de transporte o portmap dá suporte. A configuração padrão é **TCP @ no__t-1UDP**.  
  
mountprotocol @ no__t-0 {TCP | UDP | TCP @ no__t-1UDP}  
Especifica a quais protocolos de transporte a montagem oferece suporte. A configuração padrão é **TCP @ no__t-1UDP**.  
  
nfsprotocol @ no__t-0 {TCP | UDP | TCP @ no__t-1UDP}  
Especifica para quais protocolos de transporte o sistema de arquivos de rede \(NFS @ no__t-1 dá suporte. A configuração padrão é **TCP @ no__t-1UDP**  
  
nlmprotocol @ no__t-0 {TCP | UDP | TCP @ no__t-1UDP}  
Especifica quais protocolos de transporte o Gerenciador de bloqueio de rede \(NLM @ no__t-1 dá suporte. A configuração padrão é **TCP @ no__t-1UDP**.  
  
nsmprotocol @ no__t-0 {TCP | UDP | TCP @ no__t-1UDP}  
Especifica quais protocolos de transporte o Gerenciador de status de rede \(NSM @ no__t-1 dá suporte. A configuração padrão é **TCP @ no__t-1UDP**.  
  
**enableV3 @ no__t-1**{**Sim** | **não**}  
Especifica se os protocolos de NFS versão 3 terão suporte. A configuração padrão é **Sim**.  
  
**renewauth @ no__t-1**{**Sim** | **não**}  
Especifica se as conexões de cliente precisarão ser reautenticadas após o período especificado por **config renewauthinterval**. A configuração padrão é **não**.  
  
**renewauthinterval @ no__t-1**<em>segundos</em>  
Especifica o número de segundos decorridos antes que um cliente seja forçado a ser autenticado novamente se a **configuração renewauth** for definida como **Sim**. O valor padrão é 600 segundos.  
  
**dircache @ no__t-1**  
Especifica o tamanho em kilobytes do cache de diretório. O número especificado como *tamanho* deve ser um múltiplo de 4 entre 4 e 128. O tamanho padrão do diretório @ no__t-0cache é 128 KB.  
  
@no__t de **conversão**-1 @ no__t-2file @ no__t-3  
Especifica um arquivo que contém informações de mapeamento para substituir caracteres nos nomes de arquivos ao movê-los do Windows @ no__t-0based para sistemas de arquivos UNIX @ no__t-1based. Se o *arquivo* não for especificado, a conversão de caracteres de nome de arquivo será desabilitada. Se o valor de **translationfile** for alterado, você deverá reiniciar o servidor para que a alteração entre em vigor.  
  
**dotfileshidden**\= {**Yes** | **no**}  
Especifica se os arquivos criados com nomes que começam com um período \(. \) serão marcados como ocultos no sistema de arquivos do Windows e, consequentemente, ocultos dos clientes NFS. A configuração padrão é **não**.  
  
**casesensitivelookups @ no__t-1**{**Sim** | **não**}  
Especifica se as pesquisas de diretório terão distinção entre maiúsculas e minúsculas \(requiring a correspondência exata do caso de caractere @ no__t-1.  
  
Você também precisa desabilitar o caso do kernel do Windows @ no__t-0insensitivity para que o servidor para o NFS dê suporte a nomes de arquivo do caso @ no__t-1sensitive. Você pode desabilitar o caso do kernel do Windows @ no__t-0insensitivity limpando a seguinte chave do registro para 0:  
  
HKLM @ no__t-0SYSTEM @ no__t-1CurrentControlSet @ no__t-2Control @ no__t-3Session Manager @ no__t-4kernel  
  
ObCaseInsensitive DWOrd   
  
> [!IMPORTANT]  
> Esta seção se aplica somente ao Windows Server 2008 R2, ao Windows Server 2008 e ao Windows Server 2003. Esta seção não se aplica ao Windows Server 2012 R2 ou ao Windows Server 2012.  
  
**ntfscase @ no__t-1**{**Lower** | **superior** | **preserve**}  
Especifica se o caso de caracteres nos nomes de arquivos no sistema de arquivos NTFS será retornado em letras minúsculas, em letras maiúsculas ou no formato armazenado no diretório. A configuração padrão é **preserve**. Essa configuração não poderá ser alterada se **casesensitivelookups** for definido como **Sim**.  
  
*nome* de criação de um  
Cria um novo grupo de clientes, dando a ele o *nome*especificado.  
  
**listgroups**  
Exibe os nomes de todos os grupos de clientes.  
  
**excluir** *nome* do  
Remove o grupo de clientes especificado pelo *nome*.  
  
**renomear** *OldName NewName*  
altera o nome do grupo de clientes especificado por *OldName* para *NewName*  
  
**addmembers** *nome host*\[... \]  
Adiciona o *host* ao grupo de clientes especificado pelo *nome*.  
  
*nome* do listmembers  
lista os computadores host no grupo de clientes especificado por *nome*.  
  
@no__t do *host do grupo*de **deletemembers** -2... \]  
Remove o cliente especificado pelo *host* do grupo de clientes especificado pelo *grupo*.  
  
Se você não especificar uma opção de comando ou argumento, **nfsadmin server** exibirá o servidor atual para as definições de configuração de NFS.  
  
#### <a name="administering-client-for-nfs"></a>Administrando o cliente para NFS  
Use o comando **nfsadmin client** para administrar o cliente para NFS. A ação específica que o **cliente nfsadmin** usa depende do argumento de comando que você especificar:  
  
**Início**  
inicia o cliente para o serviço NFS.  
  
**stop**  
Interrompe o cliente para o serviço NFS.  
  
**config**  
Especifica as configurações gerais para o cliente para NFS. Você deve fornecer pelo menos uma das seguintes opções com o argumento de comando **config** :  
  
<em>modo</em> **FileAccess @ no__t-1**  
-   Especifica o modo de permissão padrão para arquivos criados nos servidores do sistema de arquivos de rede \(NFS @ no__t-1. O argumento *Mode* consiste em três dígitos de 0 a 7 \(inclusive @ no__t-2 que representam as permissões padrão concedidas ao usuário, ao grupo e a outros \(respectively @ no__t-4. Os dígitos são convertidos para permissões UNIX @ no__t-0style da seguinte maneira: 0 @ no__t-0NONE, 1 @ no__t-1x, 2 @ no__t-2W, 3 @ no__t-3wx, 4 @ no__t-4R, 5 @ no__t-5rx, 6 @ no__t-6RW e 7 @ no__t-7rwx. Por exemplo, **FileAccess @ no__t-1750** fornece permissão de rwx ao proprietário, permissão de RX para o grupo e nenhuma permissão de acesso para outras pessoas.  
  
<em>servidor</em> **mapsvr @ no__t-1**  
Define o *servidor* como o mapeamento de nomes de usuário Server para o cliente para NFS. Embora essa opção continue a ter suporte para compatibilidade com versões anteriores, você deve usar o utilitário **sfuadmin** em vez disso.  
  
**mtype @ no__t-1**{**Hard** | **Soft**}  
Especifica o tipo de montagem padrão. Para uma montagem rígida, o cliente para NFS continua tentando novamente um RPC com falha até obter êxito. Para uma montagem reversível, o cliente para NFS retorna falha ao aplicativo de chamada depois de repetir a chamada do número de vezes especificado pela opção de **repetição** .  
  
Repita o<em>número</em> **@ no__t-1**  
Especifica o número de vezes para tentar estabelecer uma conexão para uma montagem flexível. Esse valor deve ser de 1 a 10, inclusive. O padrão é 1.  
  
**tempo limite @ no__t-1**<em>segundos</em>  
Especifica o número de segundos a aguardar por uma conexão \(remote chamar o procedimento @ no__t-1. Esse valor deve ser 0,8, 0,9 ou um número inteiro de 1 a 60, inclusive. O padrão é 0,8.  
  
**Protocolo @ no__t-1 {TCP | UDP | TCP @ no__t-2UDP}**  
Especifica quais protocolos de transporte o cliente dá suporte. A configuração padrão é **TCP @ no__t-1UDP**  
  
**rsize @ no__t-1**  
Especifica o tamanho, em kilobytes, do buffer de leitura. Esse valor pode ser 0,5, 1, 2, 4, 8, 16 ou 32. O padrão é 32.  
  
**wSize @ no__t-1**  
Especifica o tamanho, em kilobytes, do buffer de gravação. Esse valor pode ser 0,5, 1, 2, 4, 8, 16 ou 32. O padrão é 32.  
  
**perf @ no__t-1default**  
Restaura as seguintes configurações de desempenho para valores padrão:  
  
-   **mtype**  
  
-   **Repita**  
  
-   **timeout**  
  
-   **rsize**  
  
-   **wsize**  
  
<em>modo</em> **FileAccess @ no__t-1**  
Especifica o modo de permissão padrão para arquivos criados nos servidores do sistema de arquivos de rede \(NFS @ no__t-1. O argumento *Mode* consiste em três dígitos de 0 a 7 \(inclusive @ no__t-2 que representam as permissões padrão concedidas ao usuário, ao grupo e a outros \(respectively @ no__t-4. Os dígitos são convertidos para permissões UNIX @ no__t-0style da seguinte maneira: 0 @ no__t-0NONE, 1 @ no__t-1x, 2 @ no__t-2W, 3 @ no__t-3wx, 4 @ no__t-4R, 5 @ no__t-5rx, 6 @ no__t-6RW e 7 @ no__t-7rwx. Por exemplo, **FileAccess @ no__t-1750** fornece permissão de rwx ao proprietário, permissão de RX para o grupo e nenhuma permissão de acesso para outras pessoas.  
  
Se você não especificar uma opção de comando ou argumento, **nfsadmin client** exibirá as definições de configuração do cliente atual para NFS.  
  

