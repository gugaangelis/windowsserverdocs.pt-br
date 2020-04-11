---
title: Diretrizes para solucionar problemas de ativação relacionados ao DNS
ms.topic: article
ms.date: 09/10/2019
ms.technology: server-general
author: Teresa-Motiv
ms.author: v-tea
ms.localizationpriority: medium
ms.openlocfilehash: 6cd94e997deaaf358c72793e6ff35d51a9ab3df6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80826180"
---
# <a name="guidelines-for-troubleshooting-dns-related-activation-issues"></a>Diretrizes para solucionar problemas de ativação relacionados ao DNS

Talvez você precise usar alguns desses métodos se uma ou mais das seguintes condições forem verdadeiras:

- Use a mídia licenciada por volume e uma&nbsp;chave do produto (Product Key) genérica de licença de volume para instalar um dos seguintes sistemas operacionais:
   - Windows Server 2019
   - Windows Server 2016
   - Windows Server 2012 R2
   - Windows Server 2012
   - Windows Server 2008 R2
   - Windows Server 2008
   - Windows 10
   - Windows 8.1
   - Windows 8
- O assistente para ativação não pode se conectar a um computador host KMS.

Quando você tenta ativar um sistema cliente, o assistente para ativação usa o DNS para localizar um computador correspondente que executa o software KMS. Se o assistente consultar o DNS e não encontrar a entrada DNS do computador host KMS, o assistente relatará um erro.   

<a id="list"></a>Examine a seguinte lista para encontrar uma abordagem que atenda às suas circunstâncias:

- Caso não possa instalar um host KMS nem usar a ativação KMS, experimente o procedimento [Alterar a chave do produto (Product Key) para um MAK](#change-the-product-key-to-an-mak).
- Caso precise instalar e configurar um host KMS, use o procedimento [Configurar um host KMS para ativação dos clientes](#configure-a-kms-host-for-the-clients-to-activate-against).
- Se o cliente não puder localizar o host KMS existente, use os procedimentos a seguir para solucionar problemas das configurações de roteamento. Esses procedimentos são organizados do mais simples ao mais complexo.
  - [Verificar a conectividade básica de IP com o servidor DNS](#verify-basic-ip-connectivity-to-the-dns-server)
  - [Verificar a configuração do host KMS](#verify-the-configuration-of-the-kms-host)  
  - [Determinar o tipo de problema de roteamento](#determine-the-type-of-routing-issue)
  - [Verificar a configuração do DNS](#verify-the-dns-configuration)
  - [Criar um registro SRV do KMS manualmente](#manually-create-a-kms-srv-record)
  - [Atribuir um host KMS a um cliente KMS manualmente](#manually-assign-a-kms-host-to-a-kms-client)
  - [Configurar o host KMS para publicação em vários domínios DNS](#configure-the-kms-host-to-publish-in-multiple-dns-domains)

## <a name="change-the-product-key-to-an-mak"></a>Alterar a chave do produto (Product Key) para um MAK

Caso não possa instalar um host KMS nem, por algum outro motivo, usar a ativação KMS, altere a chave do produto (Product Key) para um MAK. Se você baixou as imagens do Windows no MSDN (Microsoft Developer Network) ou no TechNet, os SKUs (unidades de manutenção de estoque) listados abaixo da mídia são geralmente uma mídia licenciada por volume e a chave do produto (Product Key) fornecida é uma chave MAK.

Para alterar a chave do produto (Product Key) de um MAK, siga estas etapas:

1. Abra uma janela do prompt de comando elevado. Para fazer isso, pressione a tecla do logotipo do Windows+X, clique com o botão direito do mouse em **Prompt de Comando** e selecione **Executar como administrador**. Caso precise inserir uma senha de administrador ou para confirmação, digite a senha ou forneça a confirmação.
2. No prompt de comando, execute o seguinte comando:
   ```cmd
    slmgr -ipk xxxxx-xxxxx-xxxxx-xxxxx-xxxxx
   ```
   > [!NOTE]
   > O espaço reservado **xxxxx-xxxxx-xxxxx-xxxxx-xxxxx** representa a chave do produto (Product Key) MAK.  

[Retorne à lista de procedimentos.](#list)

## <a name="configure-a-kms-host-for-the-clients-to-activate-against"></a>Configurar um host KMS para ativação dos clientes

A ativação KMS exige que um host KMS seja configurado para ativação dos clientes. Se não houver hosts KMS configurados em seu ambiente, instale e ative um usando uma chave do host KMS apropriada. Depois de configurar um computador na rede para hospedar o software KMS, publique as configurações do DNS (Sistema de Nomes de Domínio).

Para obter informações sobre o processo de configuração do host KMS, confira [Ativar usando o Serviço de Gerenciamento de Chaves](https://docs.microsoft.com/windows/deployment/volume-activation/activate-using-key-management-service-vamt) e [Instalar e Configurar o VAMT](https://docs.microsoft.com/windows/deployment/volume-activation/install-configure-vamt).

[Retorne à lista de procedimentos.](#list)

## <a name="verify-basic-ip-connectivity-to-the-dns-server"></a>Verificar a conectividade básica de IP com o servidor DNS

Verifique a conectividade básica de IP com o servidor DNS usando o comando ping. Para fazer isso, siga estas etapas no cliente KMS que está apresentando o erro e no computador host KMS:

1. Abra uma janela do prompt de comando elevado.
1. No prompt de comando, execute o seguinte comando:
   ```cmd
   ping <DNS_Server_IP_address>
   ```
   > [!NOTE]
   > Se a saída desse comando não incluir a frase "Responder de", haverá um problema de rede ou um problema do DNS que você precisará resolver para usar os outros procedimentos neste artigo. Para obter mais informações sobre como solucionar problemas de TCP/IP se não for possível executar ping no servidor DNS, confira [Solução de problemas avançada para problemas de TCP/IP](https://docs.microsoft.com/windows/client-management/troubleshoot-tcpip).

[Retorne à lista de procedimentos.](#list)

## <a name="verify-the-configuration-of-the-kms-host"></a>Verificar a configuração do host KMS

Verifique o Registro do servidor host KMS para determinar se ele está se registrando no DNS. Por padrão, um servidor host KMS registra dinamicamente um registro SRV do DNS uma vez a cada 24 horas. 
> [!IMPORTANT]
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes de modificá-lo, [faça backup do Registro para a restauração](https://support.microsoft.com/help/322756) em caso de problemas.  

Para verificar essa configuração, siga estas etapas:
1. Abra o Editor do Registro. Para fazer isso, clique com o botão direito do mouse em **Iniciar**, selecione **Executar**, digite **regedit** e, em seguida, pressione Enter.
1. Localize a subchave **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SL** e verifique o valor da entrada **DisableDnsPublishing**. Essa entrada tem os seguintes valores possíveis:
   - **0** ou indefinido (padrão): O servidor host KMS registra um registro SRV uma vez a cada 24 horas.
   - **1**: O servidor host KMS não registra automaticamente os registros SRV. Se a implementação não der suporte a atualizações dinâmicas, confira [Criar um registro SRV do KMS manualmente](#manually-create-a-kms-srv-record).  
1. Se a entrada **DisableDnsPublishing** estiver ausente, crie-a (o tipo é DWORD). Se o registro dinâmico for aceitável, mantenha o valor indefinido ou defina-o como **0**.

[Retorne à lista de procedimentos.](#list)

## <a name="determine-the-type-of-routing-issue"></a>Determinar o tipo de problema de roteamento

Use os comandos a seguir para determinar se esse é um problema de resolução de nomes ou de registro SRV.  

1. Em um cliente KMS, abra uma janela de prompt de comandos com privilégios elevados.  
1. No prompt de comando, execute o seguinte comando:
   ```cmd
   cscript \windows\system32\slmgr.vbs -skms <KMS_FQDN>:<port>
   cscript \windows\system32\slmgr.vbs -ato
   ```
   > [!NOTE]
   > Nesse comando, <FQDN_do_KMS> representa o FQDN (nome de domínio totalmente qualificado) do computador host KMS e \<port\> representa a porta TCP usada pelo KMS.  

   Se esses comandos resolverem o problema, esse será um problema de registro SRV. Você pode solucionar o problema usando um dos comandos que estão documentados no procedimento [Atribuir um host KMS a um cliente KMS manualmente](#manually-assign-a-kms-host-to-a-kms-client).  

1. Se o problema persistir, execute os seguintes comandos:
   ```cmd
   cscript \windows\system32\slmgr.vbs -skms <IP Address>:<port>
   cscript \windows\system32\slmgr.vbs -ato
   ```
   > [!NOTE]
   > Nesse comando, \<Endereço IP\> representa o endereço IP do computador host KMS e \<port\> representa a porta TCP usada pelo KMS.  

   Se esses comandos resolverem o problema, provavelmente, esse será um problema de resolução de nomes. Para obter mais informações sobre solução de problemas, confira o procedimento [Verificar a configuração do DNS](#verify-the-dns-configuration).

1. Se nenhum desses comandos resolver o problema, verifique a configuração do firewall do computador. Qualquer comunicação de ativação que ocorra entre clientes KMS e o host KMS usará a porta TCP 1688. Os firewalls no cliente KMS e no host KMS precisam permitir a comunicação pela porta 1688.

[Retorne à lista de procedimentos.](#list)

## <a name="verify-the-dns-configuration"></a>Verificar a configuração do DNS

>[!NOTE]
> Salvo indicação em contrário, siga estas etapas em um cliente KMS que tenha tido o erro aplicável.

1. Abra uma janela de prompt de comandos com privilégios elevados
1. No prompt de comando, execute o seguinte comando:
   ```cmd
   IPCONFIG /all
   ```
1. Nos resultados do comando, anote as seguintes informações:
   - O endereço IP atribuído do computador cliente do KMS
   - O endereço IP do servidor DNS primário usado pelo computador cliente do KMS
   - O endereço IP do gateway padrão usado pelo computador cliente do KMS
   - A lista de pesquisa de sufixo DNS usada pelo computador cliente do KMS
1. Verifique se os registros SRV do host KMS estão registrados no DNS. Para fazer isso, execute estas etapas:  
   1. Abra uma janela do prompt de comando elevado.
   1. No prompt de comando, execute o seguinte comando:
      ```cmd
      nslookup -type=all _vlmcs._tcp>kms.txt
      ```
   1. Abra o arquivo KMS.txt gerado pelo comando. Esse arquivo deve conter uma ou mais entradas que se assemelham à seguinte entrada:
       ```
       _vlmcs._tcp.contoso.com SRV service location:
       priority = 0
       weight = 0
       port = 1688 svr hostname = kms-server.contoso.com
       ```
       > [!NOTE]
       > Nessa entrada, contoso.com representa o domínio do host KMS.
      1. Verifique o endereço IP, o nome do host, a porta e o domínio do host KMS.
      1. Se essas entradas **_vlmcs** existirem e se elas contiverem os nomes do host KMS esperados, acesse [Atribuir um host KMS a um cliente KMS manualmente](#manually-assign-a-kms-host-to-a-kms-client).  
      > [!NOTE]
      > Se o comando [**nslookup**](https://docs.microsoft.com/windows-server/administration/windows-commands/nslookup) encontrar o host KMS, isso não significa que o cliente DNS poderá encontrar o host KMS. Se o comando **nslookup** encontrar o host KMS, mas você ainda não puder ativá-lo usando o host KMS, verifique as outras configurações do DNS, como o sufixo DNS primário e a lista de pesquisa do sufixo DNS.
1. Verifique se a lista de pesquisa do sufixo DNS primário contém o sufixo de domínio DNS que está associado ao host KMS. Se a lista de pesquisa não incluir essas informações, acesse o procedimento [Configurar o host KMS para publicação em vários domínios DNS](#configure-the-kms-host-to-publish-in-multiple-dns-domains).

[Retorne à lista de procedimentos.](#list)

## <a name="manually-create-a-kms-srv-record"></a>Criar um registro SRV do KMS manualmente

Para criar manualmente um registro SRV para um host KMS que usa um servidor DNS da Microsoft, siga estas etapas:

1. No servidor DNS, abra o Gerenciador DNS. Para abrir o Gerenciador DNS, selecione **Iniciar**, **Ferramentas Administrativas** e, em seguida, **DNS**.
1. Selecione o servidor DNS no qual você precisa criar o registro de recurso de serviços.
1. Na árvore de console, expanda **Zonas de Pesquisa Direta**, clique com o botão direito do mouse no domínio e, em seguida, selecione **Outros Registros Novos**.
1. Role a lista para baixo, selecione **Local do Serviço (SRV)** e, em seguida, selecione **Criar Registro**.
1. Digite as seguintes informações:
   - Serviço: **_VLMCS**
   - Protocolo: **_TCP**
   - Número da porta: **1688**
   - Host que oferece o serviço: **&lt;*FQDN do host KMS*&gt;**
1. Quando terminar, selecione **OK** e, em seguida, **Concluído**.

Para criar manualmente um registro SRV para um host KMS que usa um servidor DNS compatível com o BIND 9.x, siga as instruções para esse servidor DNS e forneça as seguintes informações do registro SRV:

- Nome:&nbsp; **_vlmcs._TCP**
- Tipo:&nbsp;**SRV**
- Prioridade: **0**
- Peso: **0**
- Porta: **1688**
- Nome do host: **&lt;*FQDN ou nome do host KMS*&gt;**

> [!NOTE]
> O KMS não usa os valores **Prioridade** ou **Peso**. No entanto, o registro precisa incluí-los.

Para configurar um servidor DNS compatível com o BIND 9.x para dar suporte à publicação automática do KMS, configure o servidor DNS para habilitar as atualizações de registro de recurso em hosts KMS. Por exemplo, adicione a seguinte linha à definição de zona em Named.conf ou em Named.conf.local:

```cmd
allow-update { any; };
```
## <a name="manually-assign-a-kms-host-to-a-kms-client"></a>Atribuir um host KMS a um cliente KMS manualmente

Por padrão, os clientes KMS usam o processo de descoberta automática. De acordo com esse processo, um cliente KMS consulta o DNS para obter uma lista de servidores que publicaram registros SRV _vlmcs na zona de associação do cliente. O DNS retorna a lista de hosts KMS em ordem aleatória. O cliente escolhe um host KMS e tenta estabelecer uma sessão nele. Se essa tentativa funcionar, o cliente armazenará em cache o nome do host KMS e tentará usá-lo para a próxima tentativa de renovação. Se a configuração da sessão falhar, o cliente selecionará aleatoriamente outro host KMS. Recomendamos expressamente que você use o processo de descoberta automática.  

No entanto, você pode atribuir manualmente um host KMS a um cliente KMS específico. Para fazer isso, siga estas etapas:

1. Em um cliente KMS, abra uma janela de prompt de comandos com privilégios elevados.
1. Dependendo da implementação, siga uma destas etapas:
   - Para atribuir um host KMS usando o FQDN do host, execute o seguinte comando:
     ```cmd
     cscript \windows\system32\slmgr.vbs -skms <KMS_FQDN>:<port>
     ```
   - Para atribuir um host KMS usando o endereço IP versão 4 do host, execute o seguinte comando:
     ```cmd
     cscript \windows\system32\slmgr.vbs -skms <IPv4Address>:<port>
     ```
    - Para atribuir um host KMS usando o endereço IP versão 6 do host, execute o seguinte comando:
      ```cmd
      cscript \windows\system32\slmgr.vbs -skms <IPv6Address>:<port>
      ```
    - Para atribuir um host KMS usando o nome NetBIOS do host, execute o seguinte comando:
      ```cmd
      cscript \windows\system32\slmgr.vbs -skms <NETBIOSName>:<port>
      ```
   - Para fazer a reversão para a descoberta automática em um cliente KMS, execute o seguinte comando:
     ```cmd
     cscript \windows\system32\slmgr.vbs -ckms
     ```
     > [!NOTE]
     > Esses comandos usam os seguintes espaços reservados:
     >- **<FQDN_do_KMS>** representa o FQDN (nome de domínio totalmente qualificado) do computador host KMS
     >- **\<IPv4Address\>** representa o endereço IP versão 4 do computador host KMS
     >- **\<IPv6Address\>** representa o endereço IP versão 6 do computador host KMS
     >- **\<NETBIOSName\>** representa o nome NetBIOS do computador host KMS
     >- **\<port\>** representa a porta TCP usada pelo KMS.  

## <a name="configure-the-kms-host-to-publish-in-multiple-dns-domains"></a>Configurar o host KMS para publicação em vários domínios DNS

> [!IMPORTANT]
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes de modificá-lo, [faça backup do Registro para a restauração](https://support.microsoft.com/help/322756) em caso de problemas.

Conforme descrito em [Atribuir um host KMS a um cliente KMS manualmente](#manually-assign-a-kms-host-to-a-kms-client), os clientes KMS normalmente usam o processo de descoberta automática para identificar os hosts KMS. Esse processo exige que os registros SRV _vlmcs estejam disponíveis na zona DNS do computador cliente do KMS. A zona DNS corresponde ao sufixo DNS primário do computador ou a uma das seguintes opções:
- Para computadores ingressados no domínio, o domínio do computador, conforme atribuído pelo sistema DNS – como o DNS do AD DS (Active Directory Domain Services).
- Para computadores do grupo de trabalho, o domínio do computador, conforme atribuído pelo protocolo DHCP. Esse nome de domínio é definido pela opção que tem o valor do código 15, conforme definido no RFC (Request for Comments) 2132.

Por padrão, um host KMS registra seus registros SRV na zona DNS que corresponde ao domínio do computador host do KMS. Por exemplo, suponha que um host KMS ingresse no domínio contoso.com. Nesse cenário, o host KMS registra seu registro SRV _vmlcs na zona DNS contoso.com. Portanto, o registro identifica o serviço como VLMCS._TCP.CONTOSO.COM.

Se o host KMS e os clientes KMS usarem zonas DNS diferentes, você deverá configurar o host KMS para publicar automaticamente seus registros SRV em vários domínios DNS. Para fazer isso, execute estas etapas:

1. No host KMS, inicie o Editor do Registro. 
1. Localize e, em seguida, selecione a subchave **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SL**.
1. No painel **Detalhes**, clique com o botão direito do mouse em uma área em branco, selecione **Novo** e, em seguida, **Valor de Cadeia de Caracteres Múltipla**.
1. Para o nome da nova entrada, insira **DnsDomainPublishList**.
1. Clique com o botão direito do mouse na nova entrada **DnsDomainPublishList** e, em seguida, selecione **Modificar**.
1. Na caixa de diálogo **Editar Cadeia de Caracteres Múltipla**, digite cada sufixo de domínio DNS publicado pelo KMS em uma linha separada e, em seguida, selecione **OK**.
   > [!NOTE]
   > Para o Windows Server 2008 R2, o formato para **DnsDomainPublishList** é diferente. Para obter mais informações, confira o Guia de Referência Técnica da Ativação de Volume.
1. Use a ferramenta administrativa Serviços para reiniciar o serviço de licenciamento de software. Essa operação cria os registros SRV.
1. Verifique se, usando um método típico, o cliente KMS pode contatar o host KMS que você configurou. Verifique se o cliente KMS identifica corretamente o host KMS pelo nome e pelo endereço IP. Se uma dessas verificações falhar, investigue esse problema do resolvedor de cliente DNS.
1. Para limpar os nomes de host KMS armazenados em cache anteriormente no cliente KMS, abra uma janela de prompt de comandos com privilégios elevados no cliente KMS e execute o seguinte comando:
   ```cmd
   cscript C:\Windows\System32\slmgr.vbs -ckms
   ```
