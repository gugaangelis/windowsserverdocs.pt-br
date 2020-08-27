---
ms.assetid: 709353b0-b913-4367-8580-44745183e2bc
title: Verificar a funcionalidade do DNS para oferecer suporte à replicação de diretório
ms.author: iainfou
ms.date: 05/31/2017
author: Femila
ms.openlocfilehash: c59160cb3242a91ef8a86d9e8247e0f2d376395b
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88938056"
---
# <a name="verify-dns-functionality-to-support-directory-replication"></a>Verificar a funcionalidade do DNS para oferecer suporte à replicação de diretório

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Para verificar as configurações de DNS que podem interferir na replicação do Active Directory, você pode começar executando o teste básico que garante que o DNS esteja operando corretamente para seu domínio. Depois de executar o teste básico, você pode testar outros aspectos da funcionalidade do DNS, incluindo o registro de registro de recurso e a atualização dinâmica.

Embora você possa executar esse teste de funcionalidade básica do DNS em qualquer controlador de domínio, normalmente você executa esse teste em controladores de domínio que podem estar enfrentando problemas de replicação, por exemplo, controladores de domínio que relatam IDs de evento 1844, 1925, 2087 ou 2088 no log DNS do serviço de diretório Visualizador de Eventos.



## <a name="running-the-domain-controller-basic-dns-testtitle"></a>Executando o teste de DNS básico do controlador de domínio</title>

O teste DNS básico verifica os seguintes aspectos da funcionalidade do DNS:


- **Conectividade:** O teste determina se os controladores de domínio estão registrados no DNS, podem ser contatados pelo comando <system>ping</system> e têm conectividade LDAP/RPC (Lightweight Directory Access Protocol/chamada de procedimento remoto). Se o teste de conectividade falhar em um controlador de domínio, nenhum outro teste será executado nesse controlador de domínio. O teste de conectividade é executado automaticamente antes de qualquer outro teste de DNS ser executado.
- **Serviços essenciais:** O teste confirma que os seguintes serviços estão em execução e disponíveis no controlador de domínio testado: serviço cliente DNS, serviço de logon da rede, serviço de centro de distribuição de chaves (KDC) e serviço do servidor DNS (se o DNS estiver instalado no controlador de domínio).
- **Configuração do cliente DNS:**  O teste confirma que os servidores DNS em todos os adaptadores de rede do computador cliente DNS estão acessíveis.
- **Registros de registro de recurso:** O teste confirma que o registro de recurso de host (A) de cada controlador de domínio está registrado em pelo menos um dos servidores DNS configurados no computador cliente.
- **Zona e início de autoridade (SOA):** Se o controlador de domínio estiver executando o serviço do servidor DNS, o teste confirmará que a zona de domínio Active Directory e o registro de recurso de início de autoridade (SOA) para a zona de domínio Active Directory estão presentes.
- **Zona raiz:** Verifica se a zona raiz (.) está presente.

A associação em administradores corporativos, ou equivalente, é o requisito mínimo necessário para concluir esses procedimentos.

Você pode usar o procedimento a seguir para verificar a funcionalidade básica do DNS.

### <a name="to-verify-basic-dns-functionality"></a>Para verificar a funcionalidade básica do DNS:


1. No controlador de domínio que você deseja testar ou em um computador membro do domínio que tenha as ferramentas Active Directory Domain Services (AD DS) instaladas, abra um prompt de comando como administrador. Para abrir um prompt de comando como administrador, clique em **Iniciar**.
2. Em Iniciar Pesquisa, digite Prompt de Comando.
3. Na parte superior do menu Iniciar, clique com o botão direito no Prompt de Comando e, em seguida, clique em Executar como administrador. Se a caixa de diálogo Controle de Conta de Usuário aparecer, confirme a ação exibida e clique em Continuar.
4. No prompt de comando, digite o seguinte comando e pressione ENTER: `dcdiag /test:dns /v /s:<DCName> /DnsBasic /f:dcdiagreport.txt`
</br></br>Substitua o nome distinto real, o nome NetBIOS ou o nome DNS do controlador de domínio para &lt; DCNAME &gt; . Como alternativa, você pode testar todos os controladores de domínio na floresta digitando/e: em vez de/s:.
A opção/f especifica um nome de arquivo, que no comando anterior é dcdiagreport.txt. Se você quiser posicionar o arquivo em um local diferente do diretório de trabalho atual, poderá especificar um caminho de arquivo, como/f:c:reportsdcdiagreport.txt.

5. Abra o arquivo de dcdiagreport.txt no bloco de notas ou um editor de texto semelhante. Para abrir o arquivo no bloco de notas, no prompt de comando, digite notepad dcdiagreport.txt e pressione ENTER. Se você colocou o arquivo em um diretório de trabalho diferente, inclua o caminho para o arquivo. Por exemplo, se você colocou o arquivo em c:Reports, digite notepad c:reportsdcdiagreport.txt e pressione ENTER.
6. Role até a tabela de resumo próxima à parte inferior do arquivo.
</br></br>Observe os nomes de todos os controladores de domínio que relatam o status "avisar" ou "falha" na tabela de resumo.  Tente determinar se existe um controlador de domínio com problema encontrando a seção detalhada de debates pesquisando a cadeia de caracteres "DC: DCName", em que DCName é o nome real do controlador de domínio.

Se você vir alterações de configuração óbvias que são necessárias, faça-as, conforme apropriado. Por exemplo, se você observar que um de seus controladores de domínio tem um endereço IP obviamente incorreto, você pode corrigi-lo. Em seguida, execute novamente o teste.

Para validar as alterações de configuração, execute novamente o comando Dcdiag/test: DNS/v com a opção/e: ou/s:, conforme apropriado. Se você não tiver o IP versão 6 (IPv6) habilitado no controlador de domínio, deverá esperar que a parte de validação do host (AAAA) do teste falhe, mas se você não estiver usando o IPv6 em sua rede, esses registros não serão necessários.

## <a name="verifying-resource-record-registration"></a>Verificando o registro de registro de recurso

O controlador de domínio de destino usa o registro de recurso de alias DNS (CNAME) para localizar seu parceiro de replicação do controlador de domínio de origem. Embora os controladores de domínio que executam o Windows Server (a partir do Windows Server 2003 com Service Pack 1 (SP1)) possam localizar parceiros de replicação de origem usando FQDNs (nomes de domínio totalmente qualificados) ou, se isso falhar, a presença de namesthe NetBIOS do registro de recurso de alias (CNAME) é esperada e deve ser verificada quanto ao funcionamento adequado do DNS.

Você pode usar o procedimento a seguir para verificar o registro de registro de recurso, incluindo o registro de registro de recurso de alias (CNAME).

### <a name="to-verify-resource-record-registrationtitle"></a>Para verificar o registro do registro de recurso</title>


1. Abra um prompt de comando como administrador. Para abrir um prompt de comando como administrador, clique em Iniciar. Em Iniciar Pesquisa, digite Prompt de Comando.
2. Na parte superior do menu Iniciar, clique com o botão direito no Prompt de Comando e, em seguida, clique em Executar como administrador. Se a caixa de diálogo Controle de Conta de Usuário aparecer, confirme a ação exibida e clique em Continuar.  </br></br>Você pode usar a ferramenta Dcdiag para verificar o registro de todos os registros de recursos que são essenciais para o local do controlador de domínio executando o `dcdiag /test:dns /DnsRecordRegistration` comando.

Este comando verifica o registro dos seguintes registros de recursos no DNS:


- **alias (CNAME):** o registro de recurso baseado em GUID (identificador global exclusivo) que localiza um parceiro de replicação
- **host (A):**  o registro de recurso do host que contém o endereço IP do controlador de domínio
- **SRV LDAP:** os registros de recurso de serviço (SRV) que localizam servidores LDAP
- **GC SRV**: os registros de recurso de serviço (SRV) que localizam servidores de catálogo global
- **PDC SRV**: os registros de recurso do serviço (SRV) que localizam mestres de operações do emulador do controlador de domínio primário (PDC)

Você pode usar o procedimento a seguir para verificar o registro de recurso de alias (CNAME) sozinho.

### <a name="to-verify-alias-cname-resource-record-registration"></a>Para verificar o registro de recurso de alias (CNAME)

1. Abra o snap-in DNS. Para abrir o DNS, clique em Iniciar. Em Iniciar pesquisa, digite DNSMGMT. msc e pressione ENTER. Se a caixa de diálogo controle de conta de usuário for exibida, confirme se ela exibe a ação desejada e clique em continuar.
2. Use o snap-in DNS para localizar qualquer controlador de domínio que esteja executando o serviço do servidor DNS, no qual o servidor hospeda a zona DNS com o mesmo nome que o domínio Active Directory do controlador de domínio.
3. Na árvore de console, clique na zona chamada _msdcs. Dns_Domain_Name.
4. No painel de detalhes, verifique se os seguintes registros de recursos estão presentes: um registro de recurso de alias (CNAME) chamado Dsa_Guid. _msdcs. <placeholder>Dns_Domain_Name</placeholder> e um registro de recurso de host (a) correspondente para o nome do servidor DNS.

Se o registro de recurso de alias (CNAME) não estiver registrado, verifique se a atualização dinâmica está funcionando corretamente. Use o teste na seção a seguir para verificar a atualização dinâmica.

## <a name="verifying-dynamic-update"></a>Verificando a atualização dinâmica

Se o teste DNS básico mostrar que os registros de recurso não existem no DNS, use o teste de atualização dinâmica para determinar por que o serviço de logon de rede não registrou os registros de recursos automaticamente. Para verificar se a zona de domínio Active Directory está configurada para aceitar atualizações dinâmicas seguras e para executar o registro de um registro de teste (_dcdiag_test_record), use o procedimento a seguir. O registro de teste é excluído automaticamente após o teste.

### <a name="to-verify-dynamic-updatetitle"></a>Para verificar a atualização dinâmica</title>


1. Abra um prompt de comando como administrador. Para abrir um prompt de comando como administrador, clique em Iniciar. Em Iniciar Pesquisa, digite Prompt de Comando. Na parte superior do menu Iniciar, clique com o botão direito no Prompt de Comando e, em seguida, clique em Executar como administrador. Se a caixa de diálogo Controle de Conta de Usuário aparecer, confirme a ação exibida e clique em Continuar.
2. No prompt de comando, digite o seguinte comando e pressione ENTER: `dcdiag /test:dns /v /s:<DCName> /DnsDynamicUpdate`
   </br></br>Substitua o nome distinto, o nome NetBIOS ou o nome DNS do controlador de domínio para &lt; DCNAME &gt; . Como alternativa, você pode testar todos os controladores de domínio na floresta digitando/e: em vez de/s:. Se você não tiver o IPv6 habilitado no controlador de domínio, deverá esperar que a parte do registro de recurso do host (AAAA) do teste falhe, que é uma condição normal quando o IPv6 não está habilitado.

Se as atualizações dinâmicas seguras não estiverem configuradas, você poderá usar o procedimento a seguir para configurá-las.

### <a name="to-enable-secure-dynamic-updates"></a>Para habilitar atualizações dinâmicas seguras


1. Abra o snap-in DNS. Para abrir o DNS, clique em Iniciar.
2. Em Iniciar pesquisa, digite DNSMGMT. msc e pressione ENTER. Se a caixa de diálogo controle de conta de usuário for exibida, confirme se ela exibe a ação desejada e clique em continuar.
3. Na árvore de console, clique com o botão direito do mouse na zona aplicável e clique em Propriedades.
4. Na guia geral, verifique se o tipo de zona é Active Directory integrado.
5. Em atualizações dinâmicas, clique em somente segurança.

## <a name="registering-dns-resource-records"></a>Registrando registros de recursos de DNS

Se os registros de recursos DNS não aparecerem no DNS para o controlador de domínio de origem, você verificou as atualizações dinâmicas e deseja registrar imediatamente os registros de recursos DNS, você pode forçar o registro manualmente usando o procedimento a seguir. O serviço de logon de rede em um controlador de domínio registra os registros de recursos de DNS necessários para que o controlador de domínio esteja localizado na rede. O serviço cliente DNS registra o registro de recurso de host (A) ao qual o registro de alias (CNAME) aponta.

### <a name="to-register-dns-resource-records-manuallytitle"></a>Para registrar registros de recursos de DNS manualmente</title>


1. Abra um prompt de comando como administrador. Para abrir um prompt de comando como administrador, clique em Iniciar.
2. Em Iniciar Pesquisa, digite Prompt de Comando.
3. Na parte superior do início, clique com o botão direito do mouse em prompt de comando e clique em executar como administrador. Se a caixa de diálogo Controle de Conta de Usuário aparecer, confirme a ação exibida e clique em Continuar.
4. Para iniciar o registro de registros de recursos do localizador do controlador de domínio manualmente no controlador de domínio de origem, no prompt de comando, digite o seguinte comando e pressione ENTER: `net stop netlogon && net start netlogon`
5. Para iniciar o registro do recurso de host (A) manualmente, no prompt de comando, digite o seguinte comando e pressione ENTER: `ipconfig /flushdns && ipconfig /registerdns`
6. No prompt de comando, digite o seguinte comando e pressione ENTER: `dcdiag /test:dns /v /s:<DCName>` </br></br>Substitua o nome distinto, o nome NetBIOS ou o nome DNS do controlador de domínio para &lt; DCNAME &gt; . Examine a saída do teste para garantir que os testes de DNS sejam aprovados. Se você não tiver o IPv6 habilitado no controlador de domínio, deverá esperar que a parte do registro de recurso do host (AAAA) do teste falhe, que é uma condição normal quando o IPv6 não está habilitado.
