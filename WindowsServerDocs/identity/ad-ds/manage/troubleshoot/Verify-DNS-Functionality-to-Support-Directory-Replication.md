---
ms.assetid: 709353b0-b913-4367-8580-44745183e2bc
title: Verificar a funcionalidade do DNS para oferecer suporte à replicação de diretório
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: ''
ms.suite: na
ms.technology: identity-adds
ms.author: joflore
ms.date: 05/31/2017
ms.tgt_pltfrm: na
author: Femila
ms.openlocfilehash: a55b95ee516abda8bdbae6e9829a161ef060012e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871867"
---
# <a name="verify-dns-functionality-to-support-directory-replication"></a>Verificar a funcionalidade do DNS para oferecer suporte à replicação de diretório

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Para verificar as configurações do sistema de nome de domínio (DNS) que podem interferir com a replicação do Active Directory, você pode começar executando o teste básico que garante que o DNS está funcionando corretamente para seu domínio. Depois de executar o teste básico, você pode testar outros aspectos da funcionalidade do DNS, incluindo o registro de recurso e a atualização dinâmica.

Embora seja possível executar esse teste de funcionalidade básica de DNS em qualquer controlador de domínio, normalmente você executar esse teste nos controladores de domínio que você acha que esteja enfrentando problemas de replicação, por exemplo, os controladores de domínio que relatam 1844 de IDs de evento, 1925, 2087, ou 2088 no log do DNS do serviço de diretório de Visualizador de eventos.



## <a name="running-the-domain-controller-basic-dns-testtitle"></a>Executando o teste de DNS de controlador de domínio básico</title>

O teste básico de DNS verifica os seguintes aspectos da funcionalidade do DNS:


- **Conectividade:** O teste determina se registrados no DNS, controladores de domínio pode ser contatado pelo <system>ping</system> de comando e ter Lightweight Directory Access Protocol / conectividade LDAP/RPC () de chamada de procedimento remoto. Se o teste de conectividade falhar em um controlador de domínio, não há outros testes são executados em relação a esse controlador de domínio. O teste de conectividade é executado automaticamente antes de qualquer outro teste DNS é executado.
- **Serviços essenciais:** O teste confirma que os seguintes serviços estão em execução e disponíveis no controlador de domínio testado: Serviço cliente DNS, o serviço de Logon de rede, serviço do Centro de distribuição de chaves (KDC) e serviço do servidor DNS (se o DNS é instalado no controlador de domínio).
- **Configuração do cliente DNS:**  O teste confirma que os servidores DNS em todos os adaptadores de rede do computador cliente DNS estão acessíveis.
- **Registros de registro de recurso:** O teste confirma que o registro de recurso de host (A) de cada controlador de domínio é registrado em pelo menos um dos servidores DNS que está configurado no computador cliente.
- **Zona e início de autoridade (SOA):** Se o controlador de domínio estiver executando o serviço servidor DNS, o teste confirma que a zona do domínio do Active Directory e o início do registro de recurso de autoridade (SOA) para a zona de domínio do Active Directory estão presentes.
- **Zona raiz:** Verifica se a zona raiz (.) está presente.

A associação em Admins corporativos ou equivalente, é o mínimo necessário para concluir esses procedimentos.

Você pode usar o procedimento a seguir para verificar a funcionalidade básica do DNS.
     
### <a name="to-verify-basic-dns-functionality"></a>Para verificar a funcionalidade básica do DNS:


1. No controlador de domínio que você deseja testar ou em um computador membro do domínio que tenha as ferramentas de serviços de domínio Active Directory (AD DS) instaladas, abra um prompt de comando como administrador. Para abrir um prompt de comando como administrador, clique em **Iniciar**. 
2. Em Iniciar pesquisa, digite no Prompt de comando. 
3. Na parte superior do menu Iniciar, clique com botão direito o Prompt de comando e, em seguida, clique em Executar como administrador. Se a caixa de diálogo Controle de Conta de Usuário aparecer, confirme a ação exibida e clique em Continuar.
4. No prompt de comando, digite o seguinte comando e pressione ENTER: `dcdiag /test:dns /v /s:<DCName> /DnsBasic /f:dcdiagreport.txt`
</br></br>Substitua o nome diferenciado real, o nome NetBIOS ou o nome DNS do controlador de domínio para &lt;DCName&gt;. Como alternativa, você pode testar todos os controladores de domínio na floresta, digitando /e: em vez de /s:. O comutador /f Especifica um nome de arquivo que o comando anterior é dcdiagreport.txt. Se você quiser colocar o arquivo em um local diferente do diretório de trabalho atual, você pode especificar um caminho de arquivo, como /f:c:reportsdcdiagreport.txt.

5. Abra o arquivo de dcdiagreport.txt no bloco de notas ou em um editor de texto semelhantes. Para abrir o arquivo no bloco de notas, no prompt de comando, digite dcdiagreport.txt do bloco de notas e, em seguida, pressione ENTER. Se você colocou o arquivo em um diretório de trabalho diferente, inclua o caminho para o arquivo. Por exemplo, se você colocou o arquivo em c:reports, digite c:reportsdcdiagreport.txt do bloco de notas e, em seguida, pressione ENTER.
6. Role até a tabela de resumo na parte inferior do arquivo. 
</br></br>Observe os nomes de todos os controladores de domínio com esse status de "Aviso" ou "Falha" na tabela de resumo do relatório.  Tente determinar se há um controlador de domínio do problema, localizando a seção de análise detalhada por meio de pesquisa a cadeia de caracteres "controlador de domínio: DCName,"onde DCName é o nome real do controlador de domínio.

Se você vir as alterações de configuração óbvio que são necessárias, você deve torná-los, conforme apropriado. Por exemplo, se você perceber que um dos seus controladores de domínio tem um endereço IP incorreto é claro, você pode corrigi-lo. Em seguida, execute novamente o teste.

Para validar as alterações de configuração, execute novamente o comando de /v Dcdiag /Test: DNS com a opção /e: ou /s:, conforme apropriado. Se você não tiver o IP versão 6 (IPv6) habilitada no controlador de domínio, você deve esperar que a parte de validação do host (AAAA) do teste falhe, mas se você não estiver usando o IPv6 na sua rede, esses registros não são necessários.
            
## <a name="verifying-resource-record-registration"></a>Verificando o registro de registro de recurso
    
O controlador de domínio de destino usa o registro de recurso de alias (CNAME) de DNS para localizar seu parceiro de replicação de controlador de domínio de origem. Embora os controladores de domínio executando o Windows Server (começando com o Windows Server 2003 com Service Pack 1 (SP1)) podem localizar os parceiros de replicação de origem usando nomes de domínio totalmente qualificados (FQDNs) ou, se isso falhar, o NetBIOS namesthe presença do alias (CNAME) registro de recurso é esperado e deve ser verificado para DNS apropriado está funcionando. 
      
Você pode usar o procedimento a seguir para verificar o registro de registros de recursos, incluindo o registro de registros de recurso de alias (CNAME).
      
### <a name="to-verify-resource-record-registrationtitle"></a>Para verificar o registro de recurso</title>


1. Abra um prompt de comando como administrador. Para abrir um prompt de comando como administrador, clique em Iniciar. Em Iniciar pesquisa, digite no Prompt de comando. 
2. Na parte superior do menu Iniciar, clique com botão direito o Prompt de comando e, em seguida, clique em Executar como administrador. Se a caixa de diálogo Controle de Conta de Usuário aparecer, confirme a ação exibida e clique em Continuar.  </br></br>Você pode usar a ferramenta Dcdiag para verificar o registro de todos os registros de recursos que são essenciais para o local do controlador de domínio executando o `dcdiag /test:dns /DnsRecordRegistration` comando.

Esse comando verifica o registro dos registros de recursos no DNS:


- **alias (CNAME):** o identificador global exclusivo (GUID)-com base em registro de recurso que localiza um parceiro de replicação
- **host (A):** o registro de recurso de host que contém o endereço IP do controlador de domínio
- **LDAP SRV:** os registros de recursos de serviço (SRV) que localizem servidores LDAP
- **GC SRV**: os registros de recursos de serviço (SRV) Localize global de servidores de catálogo
- **SRV PDC**: os registros de recursos de serviço (SRV) que localize mestres de operações do emulador PDC (controlador) de domínio primário

Você pode usar o procedimento a seguir para verificar o alias (CNAME) registro de recurso sozinho.

### <a name="to-verify-alias-cname-resource-record-registration"></a>Para verificar o registro de recurso de alias (CNAME)

1. Abra o snap-in DNS. Para abrir o DNS, clique em Iniciar. Em Iniciar pesquisa, digite Dnsmgmt. msc e pressione ENTER. Se a caixa de diálogo controle de conta de usuário aparecer, confirme que ele exibe a ação que você deseja e, em seguida, clique em continuar.
2. Use o snap-in do DNS para localizar qualquer controlador de domínio que está executando o serviço servidor DNS, em que o servidor hospeda a zona DNS com o mesmo nome de domínio do Active Directory do controlador de domínio.
3. Na árvore de console, clique na zona MSDCS chamado. Dns_Domain_Name.
4. No painel de detalhes, verifique se os registros de recursos a seguir estão presentes: um registro de recurso de alias (CNAME) chamado Dsa_Guid._msdcs. <placeholder>Dns_Domain_Name</placeholder> e um correspondente de host (A) registro de recurso para o nome do servidor DNS.

Se o registro de recurso de alias (CNAME) não estiver registrado, verifique se que a atualização dinâmica está funcionando corretamente. Use o teste na seção a seguir para verificar a atualização dinâmica.
    
## <a name="verifying-dynamic-update"></a>Verificando a atualização dinâmica
    
Se o teste básico de DNS mostra que os registros de recursos não existem no DNS, use o teste de atualização dinâmica para determinar por que o serviço de Logon de rede não registrou os registros de recursos automaticamente. Para verificar se a zona de domínio do Active Directory está configurada para aceitar atualizações dinâmicas seguras e realizar o registro de um registro de teste (_dcdiag_test_record), use o procedimento a seguir. O registro de teste é excluído automaticamente após o teste.

### <a name="to-verify-dynamic-updatetitle"></a>Para verificar a atualização dinâmica</title>


1. Abra um prompt de comando como administrador. Para abrir um prompt de comando como administrador, clique em Iniciar. Em Iniciar pesquisa, digite no Prompt de comando. Na parte superior do menu Iniciar, clique com botão direito o Prompt de comando e, em seguida, clique em Executar como administrador. Se a caixa de diálogo Controle de Conta de Usuário aparecer, confirme a ação exibida e clique em Continuar.
2. No prompt de comando, digite o seguinte comando e pressione ENTER: `dcdiag /test:dns /v /s:<DCName> /DnsDynamicUpdate`
   </br></br>Substitua o nome diferenciado, o nome NetBIOS ou o nome DNS do controlador de domínio para &lt;DCName&gt;. Como alternativa, você pode testar todos os controladores de domínio na floresta, digitando /e: em vez de /s:. Se você não tem IPv6 habilitado no controlador de domínio, você deve esperar a parte de host (AAAA) recursos registro o teste falhe, que é uma condição normal quando o IPv6 não está habilitado.

Se as atualizações dinâmicas não estiverem configuradas, você pode usar o procedimento a seguir para configurá-los.

### <a name="to-enable-secure-dynamic-updates"></a>Para habilitar atualizações dinâmicas seguras


1. Abra o snap-in DNS. Para abrir o DNS, clique em Iniciar. 
2. Em Iniciar pesquisa, digite Dnsmgmt. msc e pressione ENTER. Se a caixa de diálogo controle de conta de usuário aparecer, confirme que ele exibe a ação que você deseja e, em seguida, clique em continuar.
3. Na árvore de console, clique com botão direito na zona aplicável e, em seguida, clique em Propriedades.
4. Na guia geral, verifique se o tipo de zona integrada ao Active Directory.
5. Em atualizações dinâmicas, clique em Secure somente.

## <a name="registering-dns-resource-records"></a>Registrar os registros de recurso DNS
    
Se os registros de recursos DNS não aparecem no DNS para o controlador de domínio de origem, você verificou atualizações dinâmicas e você deseja registrar os registros de recursos DNS imediatamente, você pode forçar o registro manualmente usando o procedimento a seguir. O serviço de Logon de rede em um controlador de domínio registra os registros de recursos DNS que são necessários para o controlador de domínio a ser localizado na rede. O serviço cliente DNS registra o registro de recurso de host (A) que aponta o registro de alias (CNAME).

### <a name="to-register-dns-resource-records-manuallytitle"></a>Para registrar os registros de recursos DNS manualmente</title>


1. Abra um prompt de comando como administrador. Para abrir um prompt de comando como administrador, clique em Iniciar. 
2. Em Iniciar pesquisa, digite no Prompt de comando. 
3. Na parte superior do início, Prompt de comando com o botão direito e, em seguida, clique em Executar como administrador. Se a caixa de diálogo Controle de Conta de Usuário aparecer, confirme a ação exibida e clique em Continuar.
4. Iniciar o registro de recurso de localizador do controlador de domínio registros manualmente no controlador de domínio de origem, no prompt de comando, digite o seguinte comando em pressione ENTER: `net stop netlogon && net start netlogon`
5. Para iniciar o registro do host (A) registro de recurso manualmente, no prompt de comando, digite o seguinte comando e pressione ENTER: `ipconfig /flushdns && ipconfig /registerdns`
6. No prompt de comando, digite o seguinte comando e pressione ENTER: `dcdiag /test:dns /v /s:<DCName>` </br></br>Substitua o nome diferenciado, o nome NetBIOS ou o nome DNS do controlador de domínio para &lt;DCName&gt;. Examine a saída do teste para garantir que os testes DNS passaram. Se você não tem IPv6 habilitado no controlador de domínio, você deve esperar a parte de host (AAAA) recursos registro o teste falhe, que é uma condição normal quando o IPv6 não está habilitado.
