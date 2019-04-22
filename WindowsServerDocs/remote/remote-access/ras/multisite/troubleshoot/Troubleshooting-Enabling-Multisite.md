---
title: Solução de problemas da habilitação de multissite
description: Este tópico faz parte do guia de implantar vários servidores de acesso remoto em uma implantação multissite no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 570c81d6-c4f4-464c-bee9-0acbd4993584
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 35b008b12cde28391876f914f25002ce8cb8d56c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822647"
---
# <a name="troubleshooting-enabling-multisite"></a>Solução de problemas da habilitação de multissite

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico contém informações sobre como solucionar problemas relacionados ao comando `Enable-DAMultisite`. Para confirmar que o erro recebido está relacionado à habilitação de multissite, procure a ID de evento 10051 no Log de Eventos do Windows.  
  
## <a name="user-connectivity-issues"></a>Problemas de conectividade dos usuários  
Os usuários poderão ter problemas de conectividade quando você habilitar a funcionalidade multissite se a configuração não estiver correta.  
  
**Causa**  
  
Em uma implantação multissite, os computadores cliente Windows 10 e Windows 8 são capazes de se movimentarem entre os pontos de entrada diferentes.  Computadores cliente do Windows 7 devem ser associados um ponto de entrada específico na implantação multissite. Se não estiverem no grupo de segurança correto, os computadores cliente poderão receber as configurações de política de grupo erradas.  
  
**Solução**  
  
O DirectAccess requer pelo menos um grupo de segurança para todos os Windows 10 e Windows 8 computadores cliente; é recomendável usar um grupo de segurança para todos os Windows 10 e computadores com Windows 8 por domínio. O DirectAccess também requer um grupo de segurança para computadores de cliente do Windows 7 para cada ponto de entrada. Cada computador cliente só deve ser incluído em um único grupo de segurança. Portanto, assegure-se de que os grupos de segurança para Windows 10 e clientes do Windows 8 contenham apenas computadores com Windows 10 ou Windows 8 e que cada computador de cliente do Windows 7 pertence a um grupo de segurança dedicado de único para o ponto de entrada relevante e Nenhum Windows 10 ou Windows 8 cliente pertença aos grupos de segurança do Windows 7.  
  
Configurar os grupos de segurança do Windows 8 na **selecionar grupos** página do **instalação do cliente DirectAccess** assistente. Configurar grupos de segurança do Windows 7 no **suporte ao cliente** página do **habilitar implantação multissite** assistente, ou nos **suporte ao cliente** página do  **Adicionar um ponto de entrada** assistente.  
  
## <a name="kerberos-proxy-authentication"></a>Autenticação de proxy Kerberos  
**Erro recebido**. Não há suporte para autenticação de proxy Kerberos em uma implantação multissite. Habilite o uso de certificados de computador para a autenticação de usuário IPsec.  
  
**Causa**  
  
É preciso habilitar a autenticação de certificado de computador antes de habilitar a funcionalidade multissite.  
  
**Solução**  
  
Para habilitar a autenticação de certificado de computador:  
  
1.  No Console de Gerenciamento de Acesso Remoto, no painel de detalhes, na **Etapa 2: Servidor de Acesso Remoto**, clique em **Editar**.  
  
2.  No **Assistente para Configuração do Servidor de Acesso Remoto**, na página **Autenticação**, marque a caixa de seleção **Usar certificados de computador** e selecione a autoridade de certificação raiz ou intermediária que emite certificados na sua implantação.  
  
Para habilitar a autenticação de certificado de computador usando o Windows PowerShell, use o `Set-DAServer` cmdlet e especifique o *IPsecRootCertificate* parâmetro.  
  
## <a name="ip-https-certificates"></a>Certificados IP-HTTPS  
**Erro recebido**. O servidor DirectAccess usa um certificado IP-HTTPS autoassinado. Configure o IP-HTTPS para usar um certificado assinado de uma autoridade de certificação conhecida.  
  
**Causa**  
  
O certificado IP-HTTPS é autoassinado. Não é possível usar certificados autoassinados em uma implantação multissite.  
  
**Solução**  
  
Para selecionar um certificado IP-HTTPS:  
  
1.  No Console de Gerenciamento de Acesso Remoto, no painel de detalhes, na **Etapa 2: Servidor de Acesso Remoto**, clique em **Editar**.  
  
2.  No **Assistente para Configuração do Servidor de Acesso Remoto**, na página **Adaptadores de Rede**, em **Selecionar o certificado usado para autenticar conexões IP-HTTPS**, deixe a caixa de seleção **Usar um certificado autoassinado criado automaticamente pelo DirectAccess** desmarcada e clique em **Procurar** para selecionar um certificado emitido por uma AC confiável.  
  
## <a name="network-location-server"></a>Servidor de local da rede  
  
-   **Problema 1**  
  
    **Erro recebido**. DirectAccess é configurado para usar um certificado autoassinado para o servidor de local de rede. Configure o servidor de local de rede para usar um certificado assinado de uma autoridade de certificação.  
  
    **Causa**  
  
    O servidor de local de rede está implantado no servidor de acesso remoto e utiliza um certificado autoassinado. Não é possível usar certificados autoassinados em uma implantação multissite.  
  
    **Solução**  
  
    Para selecionar um certificado de servidor de local de rede:  
  
    1.  No Console de Gerenciamento de Acesso Remoto, no painel de detalhes, na **Etapa 3: Servidores de Infraestrutura**, clique em **Editar**.  
  
    2.  No **Assistente de Instalação do Servidor de Infraestrutura**, na página **Servidor de Local de Rede**, em **O servidor do local de rede é implantado no servidor de Acesso Remoto**, deixe a caixa de seleção **Usar um certificado autoassinado** desmarcada e clique em **Procurar** para selecionar um certificado emitido por uma AC corporativa.  
  
-   **Problema 2**  
  
    **Erro recebido**. Para implantar uma carga de rede balanceada cluster ou implantação multissite, obtenha um certificado para o servidor de local de rede com um nome de assunto que é diferente do nome interno do servidor de acesso remoto.  
  
    **Causa**  
  
    O nome de entidade do certificado usado para o site do servidor de local de rede é igual ao nome interno do servidor de acesso remoto. Isso causará problemas na resolução de nomes.  
  
    **Solução**  
  
    Obtenha um certificado com um nome de entidade que seja diferente do nome interno do servidor de acesso remoto.  
  
    Para configurar o servidor de local de rede:  
  
    1.  No Console de Gerenciamento de Acesso Remoto, no painel de detalhes, na **Etapa 3: Servidores de Infraestrutura**, clique em **Editar**.  
  
    2.  No **Assistente de Instalação do Servidor de Infraestrutura**, na página **Servidor de Local de Rede**, em **O servidor do local de rede é implantado no servidor de Acesso Remoto**, clique em **Procurar** para selecionar o certificado obtido anteriormente. O certificado deve ter um nome de entidade que seja diferente do nome interno do servidor de acesso remoto.  
  
## <a name="windows-7-client-computers"></a>Computadores cliente Windows 7  
**Aviso recebido**. Ao habilitar multissite, os grupos de segurança configurados para clientes DirectAccess não devem conter computadores com Windows 7. Para dar suporte a computadores clientes com Windows 7 em uma implantação multissite, selecione um grupo de segurança que contenha os clientes de cada ponto de entrada.  
  
**Causa**  
  
Na implantação existente do DirectAccess, foi habilitado o suporte de cliente do Windows 7.  
  
**Solução**  
  
O DirectAccess requer pelo menos um grupo de segurança para todos os computadores de cliente do Windows 8 e um grupo de segurança para computadores de cliente do Windows 7 para cada ponto de entrada. Cada computador cliente só deve ser incluído em um único grupo de segurança. Portanto, você deve garantir que o grupo de segurança para clientes do Windows 8 contém apenas os computadores que executam o Windows 8 e que cada computador de cliente do Windows 7 pertence a um grupo de segurança dedicado de único para o ponto de entrada relevante e nenhum cliente do Windows 8 pertence aos grupos de segurança do Windows 7.  
  
## <a name="active-directory-site"></a>Site do Active Directory  
**Erro recebido**. O servidor < nome_do_servidor > não está associado um Site do Active Directory.  
  
**Causa**  
  
O DirectAccess não pôde determinar o site do Active Directory. No console Serviços e Sites do Active Directory, você pode configurar as diversas sub-redes da sua rede e associar cada uma delas ao site do Active Directory relevante. Esse erro poderá ocorrer se o endereço IP do servidor de acesso remoto não pertencer a nenhuma das sub-redes ou se a sub-rede à qual o endereço IP pertence não estiver definida com um site do Active Directory.  
  
**Solução**  
  
Confirme se é esse mesmo o problema executando o comando `nltest /dsgetsite` no servidor de acesso remoto. Se for, o comando retornará ERROR_NO_SITENAME. Para resolver o problema, no controlador de domínio, assegure-se de que exista uma sub-rede que contenha o endereço IP do servidor interno e de que ela esteja definida com um site do Active Directory.  
  
## <a name="SaveGPOSettings"></a>Salvando as configurações de GPO do servidor  
**Erro recebido**. Ocorreu um erro ao salvar configurações de acesso remoto no GPO < nome_do_gpo >.  
  
**Causa**  
  
As alterações no GPO do servidor não podem ser salvo devido a problemas de conectividade ou se houver uma violação de compartilhamento no arquivo Registry pol, por exemplo, outro usuário bloqueou o arquivo.  
  
**Solução**  
  
Assegure-se de que exista conectividade entre o servidor de acesso remoto e o controlador de domínio. Se houver conectividade, verifique no controlador se o arquivo registry.pol foi bloqueado por outro usuário e, se necessário, encerre a sessão desse usuário para desbloquear o arquivo.  
  
## <a name="InternalServerError"></a>Ocorreu um erro interno  
**Erro recebido**. Ocorreu um erro interno.  
  
**Causa**  
  
Isso pode ser causado por uma configuração inesperada da tabela de pontos de entrada no GPO de cliente. Isso poderá ocorrer se o administrador usar cmdlets de cliente do DirectAccess para editar a tabela de pontos de entrada no GPO de cliente.  
  
**Solução**  
  
Examine a configuração da tabela de pontos de entrada em todos os GPOs de cliente e corrija as inconsistências encontradas na configuração de multissite entre as diversas instâncias dos GPOs de cliente e a configuração do DirectAccess. Use o cmdlet `Get-DaEntryPointTableItem` com o nome do GPO de cliente para obter a tabela de pontos de entrada no cliente. Use o cmdlet `Get-NetIPHttpsConfiguration` para obter todos os perfis IP-HTTPS para todos os pontos de entrada.  
  
Para obter mais informações, consulte [Cmdlets do cliente DirectAccess no Windows PowerShell](https://technet.microsoft.com/library/hh848426).  
  


