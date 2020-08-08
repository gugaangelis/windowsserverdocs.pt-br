---
title: Solução de problemas da habilitação de multissite
description: Este tópico faz parte do guia implantar vários servidores de acesso remoto em uma implantação multissite no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 570c81d6-c4f4-464c-bee9-0acbd4993584
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 97e33b08d84b4e1aa4a5cb17aca331456e0402d2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87958484"
---
# <a name="troubleshooting-enabling-multisite"></a>Solução de problemas da habilitação de multissite

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico contém informações sobre como solucionar problemas relacionados ao comando `Enable-DAMultisite`. Para confirmar que o erro recebido está relacionado à habilitação de multissite, procure a ID de evento 10051 no Log de Eventos do Windows.

## <a name="user-connectivity-issues"></a>Problemas de conectividade dos usuários
Os usuários poderão ter problemas de conectividade quando você habilitar a funcionalidade multissite se a configuração não estiver correta.

**Causa**

Em uma implantação multissite, os computadores cliente Windows 10 e Windows 8 são capazes de fazer roaming entre pontos de entrada diferentes.  Os computadores cliente do Windows 7 devem estar associados a um ponto de entrada específico na implantação multissite. Se não estiverem no grupo de segurança correto, os computadores cliente poderão receber as configurações de política de grupo erradas.

**Solução**

O DirectAccess requer pelo menos um grupo de segurança para todos os computadores cliente com Windows 10 e Windows 8; é recomendável usar um grupo de segurança para todos os computadores com Windows 10 e Windows 8 por domínio. O DirectAccess também requer um grupo de segurança para computadores cliente do Windows 7 para cada ponto de entrada. Cada computador cliente só deve ser incluído em um único grupo de segurança. Portanto, você deve certificar-se de que os grupos de segurança para clientes Windows 10 e Windows 8 contenham apenas computadores com Windows 10 ou Windows 8, e que cada computador cliente com Windows 7 pertença a um único grupo de segurança dedicado para o ponto de entrada relevante e que nenhum cliente do Windows 10 ou Windows 8 pertença aos grupos de segurança do Windows 7.

Configure os grupos de segurança do Windows 8 na página **Selecionar grupos** do assistente de **instalação do cliente DirectAccess** . Configure os grupos de segurança do Windows 7 na página **suporte ao cliente** do assistente para **habilitar implantação multissite** ou na página **suporte ao cliente** do assistente para **Adicionar um ponto de entrada** .

## <a name="kerberos-proxy-authentication"></a>Autenticação de proxy Kerberos
**Erro recebido**. Não há suporte para autenticação de proxy Kerberos em uma implantação multissite. Habilite o uso de certificados de computador para a autenticação de usuário IPsec.

**Causa**

É preciso habilitar a autenticação de certificado de computador antes de habilitar a funcionalidade multissite.

**Solução**

Para habilitar a autenticação de certificado de computador:

1.  No Console de Gerenciamento de Acesso Remoto, no painel de detalhes, na **Etapa 2: Servidor de Acesso Remoto**, clique em **Editar**.

2.  No **Assistente para Configuração do Servidor de Acesso Remoto**, na página **Autenticação**, marque a caixa de seleção **Usar certificados de computador** e selecione a autoridade de certificação raiz ou intermediária que emite certificados na sua implantação.

Para habilitar a autenticação de certificado do computador usando o Windows PowerShell, use o `Set-DAServer` cmdlet e especifique o parâmetro *IPsecRootCertificate* .

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

    **Erro recebido**. O DirectAccess está configurado para usar um certificado autoassinado para o servidor de local de rede. Configure o servidor de local de rede para usar um certificado assinado de uma autoridade de certificação.

    **Causa**

    O servidor de local de rede está implantado no servidor de acesso remoto e utiliza um certificado autoassinado. Não é possível usar certificados autoassinados em uma implantação multissite.

    **Solução**

    Para selecionar um certificado de servidor de local de rede:

    1.  No Console de Gerenciamento de Acesso Remoto, no painel de detalhes, na **Etapa 3: Servidores de Infraestrutura**, clique em **Editar**.

    2.  No **Assistente de Instalação do Servidor de Infraestrutura**, na página **Servidor de Local de Rede**, em **O servidor do local de rede é implantado no servidor de Acesso Remoto**, deixe a caixa de seleção **Usar um certificado autoassinado** desmarcada e clique em **Procurar** para selecionar um certificado emitido por uma AC corporativa.

-   **Problema 2**

    **Erro recebido**. Para implantar um cluster de balanceamento de carga de rede ou implantação multissite, obtenha um certificado para o servidor de local de rede com um nome de entidade diferente do nome interno do servidor de acesso remoto.

    **Causa**

    O nome de entidade do certificado usado para o site do servidor de local de rede é igual ao nome interno do servidor de acesso remoto. Isso causará problemas na resolução de nomes.

    **Solução**

    Obtenha um certificado com um nome de entidade que seja diferente do nome interno do servidor de acesso remoto.

    Para configurar o servidor de local de rede:

    1.  No Console de Gerenciamento de Acesso Remoto, no painel de detalhes, na **Etapa 3: Servidores de Infraestrutura**, clique em **Editar**.

    2.  No **Assistente de Instalação do Servidor de Infraestrutura**, na página **Servidor de Local de Rede**, em **O servidor do local de rede é implantado no servidor de Acesso Remoto**, clique em **Procurar** para selecionar o certificado obtido anteriormente. O certificado deve ter um nome de entidade que seja diferente do nome interno do servidor de acesso remoto.

## <a name="windows-7-client-computers"></a>Computadores cliente Windows 7
**Aviso recebido**. Ao habilitar o multissite, os grupos de segurança configurados para clientes do DirectAccess não devem conter computadores com Windows 7. Para dar suporte a computadores clientes com Windows 7 em uma implantação multissite, selecione um grupo de segurança que contenha os clientes de cada ponto de entrada.

**Causa**

Na implantação do DirectAccess existente, o suporte ao cliente do Windows 7 foi habilitado.

**Solução**

O DirectAccess requer pelo menos um grupo de segurança para todos os computadores cliente do Windows 8 e um grupo de segurança para computadores cliente do Windows 7 para cada ponto de entrada. Cada computador cliente só deve ser incluído em um único grupo de segurança. Portanto, você deve garantir que o grupo de segurança para clientes do Windows 8 contenha apenas computadores que executam o Windows 8 e que cada computador cliente com Windows 7 pertença a um único grupo de segurança dedicado para o ponto de entrada relevante e que nenhum cliente do Windows 8 pertença aos grupos de segurança do Windows 7.

## <a name="active-directory-site"></a>Site do Active Directory
**Erro recebido**. O servidor <server_name> não está associado a um site Active Directory.

**Causa**

O DirectAccess não pôde determinar o site do Active Directory. No console Serviços e Sites do Active Directory, você pode configurar as diversas sub-redes da sua rede e associar cada uma delas ao site do Active Directory relevante. Esse erro poderá ocorrer se o endereço IP do servidor de acesso remoto não pertencer a nenhuma das sub-redes ou se a sub-rede à qual o endereço IP pertence não estiver definida com um site do Active Directory.

**Solução**

Confirme se é esse mesmo o problema executando o comando `nltest /dsgetsite` no servidor de acesso remoto. Se for, o comando retornará ERROR_NO_SITENAME. Para resolver o problema, no controlador de domínio, assegure-se de que exista uma sub-rede que contenha o endereço IP do servidor interno e de que ela esteja definida com um site do Active Directory.

## <a name="saving-server-gpo-settings"></a><a name="SaveGPOSettings"></a>Salvando configurações de GPO de servidor
**Erro recebido**. Ocorreu um erro ao salvar as configurações de acesso remoto ao GPO <GPO_name>.

**Causa**

As alterações no GPO do servidor não puderam ser salvas devido a problemas de conectividade ou se houver uma violação de compartilhamento no arquivo Registry. pol, por exemplo, outro usuário bloqueou o arquivo.

**Solução**

Assegure-se de que exista conectividade entre o servidor de acesso remoto e o controlador de domínio. Se houver conectividade, verifique no controlador se o arquivo registry.pol foi bloqueado por outro usuário e, se necessário, encerre a sessão desse usuário para desbloquear o arquivo.

## <a name="internal-error-occurred"></a><a name="InternalServerError"></a>Ocorrência de um erro interno
**Erro recebido**. Ocorreu um erro interno.

**Causa**

Isso pode ser causado por uma configuração inesperada da tabela de pontos de entrada no GPO de cliente. Isso poderá ocorrer se o administrador usar cmdlets de cliente do DirectAccess para editar a tabela de pontos de entrada no GPO de cliente.

**Solução**

Examine a configuração da tabela de pontos de entrada em todos os GPOs de cliente e corrija as inconsistências encontradas na configuração de multissite entre as diversas instâncias dos GPOs de cliente e a configuração do DirectAccess. Use o cmdlet `Get-DaEntryPointTableItem` com o nome do GPO de cliente para obter a tabela de pontos de entrada no cliente. Use o cmdlet `Get-NetIPHttpsConfiguration` para obter todos os perfis IP-HTTPS para todos os pontos de entrada.

Para saber mais, consulte o tópico sobre os [cmdlets de cliente do DirectAccess no Windows PowerShell](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj591658(v=ws.11)).

