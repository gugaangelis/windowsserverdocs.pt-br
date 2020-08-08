---
title: AD FS solução de problemas-eventos e log de auditoria
description: Este documento descreve como usar os vários logs de AD FS para solucionar problemas
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2018
ms.topic: article
ms.openlocfilehash: ef0adfa8b0e565981ea5273508f8c943d67e2d02
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953140"
---
# <a name="ad-fs-troubleshooting---events-and-logging"></a>Solução de problemas de AD FS-eventos e log
AD FS fornece dois logs principais que podem ser usados na solução de problemas.  Eles são:

- o log do administrador
- o log de rastreamento

Cada um desses logs será explicado abaixo.

## <a name="admin-log"></a>Log do administrador
O log de administração fornece informações de alto nível sobre problemas que estão ocorrendo e é habilitado por padrão.

### <a name="to-view-the-admin-log"></a>Para exibir o log do administrador
1.  Abra o Visualizador de Eventos
2.  Expanda **log de aplicativos e serviços**.
3.  Expanda **AD FS**.
4.  Clique em **Admin**.

![Aprimoramentos de auditoria](media/ad-fs-tshoot-logging/event1.PNG)

## <a name="trace-log"></a>Log de rastreamento
O log de rastreamento é onde as mensagens detalhadas são registradas e serão o log mais útil ao solucionar problemas. Como muitas informações de log de rastreamento podem ser geradas em um curto período de tempo, o que pode afetar o desempenho do sistema, os logs de rastreamento são desabilitados por padrão.

### <a name="to-enable-and-view-the-trace-log"></a>Para habilitar e exibir o log de rastreamento
1.  Abra o Visualizador de Eventos
2.  Clique com o botão direito do mouse no **log de aplicativos e serviços** e selecione Exibir e clique em **Mostrar logs analíticos e de depuração**.  Isso mostrará nós adicionais à esquerda.
![Aprimoramentos de auditoria](media/ad-fs-tshoot-logging/event2.PNG)
3.  Expandir rastreamento de AD FS
4.  Clique com o botão direito do mouse em Depurar e selecione **habilitar log**.
![Aprimoramentos de auditoria](media/ad-fs-tshoot-logging/event3.PNG)


## <a name="event-auditing-information-for-ad-fs-on-windows-server-2016"></a>Informações de auditoria de eventos para AD FS no Windows Server 2016
Por padrão, AD FS no Windows Server 2016 tem um nível básico de auditoria habilitado.  Com a auditoria básica, os administradores verão 5 ou menos eventos para uma única solicitação.  Isso marca uma diminuição significativa no número de eventos que os administradores precisam examinar para ver uma única solicitação.   O nível de auditoria pode ser gerado ou reduzido usando o cmdlt do PowerShell:

```PowerShell
Set-AdfsProperties -AuditLevel
```

A tabela a seguir explica os níveis de auditoria disponíveis.

|Nível de Auditoria|Sintaxe do PowerShell|Descrição|
|----- | ----- | ----- |
|Nenhum|Set-Adfsproperties-AuditLevel None|A auditoria está desabilitada e nenhum evento será registrado.|
|Básico (padrão)|Set-Adfsproperties-AuditLevel básico|Não mais de 5 eventos serão registrados em log para uma única solicitação|
|Detalhado|Set-Adfsproperties-AuditLevel Verbose|Todos os eventos serão registrados em log.  Isso registrará uma quantidade significativa de informações por solicitação.|

Para exibir o nível de auditoria atual, você pode usar o PowerShell cmdlt: Get-Adfsproperties.

![Aprimoramentos de auditoria](media/ad-fs-tshoot-logging/ADFS_Audit_1.PNG)

O nível de auditoria pode ser gerado ou reduzido usando o cmdlt do PowerShell: Set-Adfsproperties-AuditLevel.

![Aprimoramentos de auditoria](media/ad-fs-tshoot-logging/ADFS_Audit_2.png)

## <a name="types-of-events"></a>Tipos de eventos
AD FS eventos podem ser de tipos diferentes, com base nos diferentes tipos de solicitações processadas pelo AD FS. Cada tipo de evento tem dados específicos associados a ele.  O tipo de eventos pode ser diferenciado entre solicitações de logon (ou seja, solicitações de token) versus solicitações do sistema (chamadas de servidor-Server, incluindo a busca de informações de configuração).

A tabela a seguir descreve os tipos básicos de eventos.

|Tipo de evento|ID do evento|Descrição|
|----- | ----- | ----- |
|Êxito na validação da credencial|1202|Uma solicitação em que as novas credenciais são validadas com êxito pelo Serviço de Federação. Isso inclui WS-Trust, WS-Federation, SAML-P (primeiro trecho para gerar SSO) e os pontos de extremidade de autorização OAuth.|
|Novo erro de validação de credencial|1203|Uma solicitação na qual a nova validação de credencial falhou no Serviço de Federação. Isso inclui WS-Trust, WS-enalimentate, SAML-P (primeiro segmento para gerar SSO) e os pontos de extremidade de autorização do OAuth.|
|Êxito do token de aplicativo|1200|Uma solicitação em que um token de segurança é emitido com êxito pelo Serviço de Federação. Para o WS-Federation, SAML-P isso é registrado quando a solicitação é processada com o artefato de SSO. (como o cookie de SSO).|
|Falha no token do aplicativo|1201|Uma solicitação em que a emissão de token de segurança falhou na Serviço de Federação. Para o WS-Federation, SAML-P isso é registrado quando a solicitação foi processada com o artefato de SSO. (como o cookie de SSO).|
|Êxito na solicitação de alteração de senha|1204|Uma transação em que a solicitação de alteração de senha foi processada com êxito pelo Serviço de Federação.|
|Erro de solicitação de alteração de senha|1205|Uma transação em que a solicitação de alteração de senha não pôde ser processada pelo Serviço de Federação.|
|Sair do sucesso|1206|Descreve uma solicitação de saída bem-sucedida.|
|Falha ao sair|1207|Descreve uma solicitação de saída com falha.|

## <a name="security-auditing"></a>Auditoria de segurança
Às vezes, a auditoria de segurança da conta de serviço AD FS pode ajudar a rastrear problemas com atualizações de senha, registro em log de solicitação/resposta, cabeçalhos de contect de solicitação e resultados de registro do dispositivo.  Por padrão, a auditoria da conta de serviço de AD FS está desabilitada.

### <a name="to-enable-security-auditing"></a>Para habilitar a auditoria de segurança
1. Clique em Iniciar, aponte para **programas**, aponte para **Ferramentas administrativas**e clique em **política de segurança local**.
2. Navegue até a pasta **Configurações de Segurança\Políticas Locais\Gerenciamento de Direitos do Usuário** e clique duas vezes em **Gerar Auditorias de Segurança**.
3. Na guia **Configuração de segurança local**, verifique se a conta de serviço AD FS está listada. Se não estiver, clique em Adicionar Usuário ou Grupo e adicione-a à lista. Em seguida, clique em OK.
4. Abra um prompt de comando com privilégios elevados e execute o seguinte comando para habilitar a auditoria auditpol.exe/set/SubCategory: "gerado pelo aplicativo"/Failure: habilitar/Success: habilitar
5. Feche **Política de Segurança Local** e então abra o snap-in Gerenciamento do AD FS.

Para abrir o snap-in Gerenciamento de AD FS, clique em Iniciar, aponte para programas, aponte para ferramentas administrativas e clique em gerenciamento de AD FS.

6. No painel Ações, clique em Editar propriedades de Serviço de Federação
7. Na caixa de diálogo Propriedades do Serviço de Federação, clique na guia Eventos.
8. Marque as caixas de seleção **Auditorias com êxito** e **Auditorias com falha**.
9. Clique em OK.

![Aprimoramentos de auditoria](media/ad-fs-tshoot-logging/event4.PNG)

>[!NOTE]
>As instruções acima são usadas somente quando AD FS está em um servidor membro autônomo.  Se AD FS estiver em execução em um controlador de domínio, em vez da política de segurança local, use a **política de controlador de domínio padrão** localizada em **política de grupo Gerenciamento/floresta/domínios/controladores de domínio**.  Clique em Editar e navegue até **computador Rights Management \** \ \ \ \ \ \ \ \ \ \ \

## <a name="windows-communication-foundation-and-windows-identity-foundation-messages"></a>Windows Communication Foundation e mensagens do Windows Identity Foundation
Além do registro em log de rastreamento, às vezes, talvez seja necessário exibir as mensagens Windows Communication Foundation (WCF) e Windows Identity Foundation (WIF) para solucionar um problema. Isso pode ser feito modificando o arquivo de **Microsoft.IdentityServer.ServiceHost.Exe.Config** no servidor AD FS.

Esse arquivo está localizado na **<% System raiz% > \windows\adfs** e está em formato XML. As partes relevantes do arquivo são mostradas abaixo:
```
<!-- To enable WIF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="Microsoft.IdentityModel" switchValue="Off"> … </source>

<!-- To enable WCF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="System.ServiceModel" switchValue="Off" > … </source>
```


Depois de aplicar essas alterações, salve a configuração e reinicie o serviço de AD FS. Depois de habilitar esses rastreamentos definindo as opções apropriadas, elas aparecerão no log de rastreamento AD FS na Visualizador de Eventos do Windows.

## <a name="correlating-events"></a>Correlacionando eventos
Uma das coisas mais difíceis de solucionar problemas é o acesso a problemas que geram muitos eventos de erro ou de depuração.

Para ajudar com isso, AD FS correlaciona todos os eventos que são registrados no Visualizador de Eventos, tanto no administrador quanto nos logs de depuração, que correspondem a uma determinada solicitação usando um GUID (identificador global exclusivo) exclusivo chamado de ID da atividade. Essa ID é gerada quando a solicitação de emissão de token é inicialmente apresentada ao aplicativo Web (para aplicativos que usam o perfil solicitante passivo) ou solicitações enviadas diretamente para o provedor de declarações (para aplicativos que usam WS-Trust).

![ActivityId](media/ad-fs-tshoot-logging/activityid1.png)

Essa ID de atividade permanece a mesma para toda a duração da solicitação e é registrada como parte de cada evento registrado no Visualizador de Eventos para essa solicitação. Isso significa:
 - a filtragem ou a pesquisa do Visualizador de Eventos usando essa ID de atividade pode ajudar a manter o controle de todos os eventos relacionados que correspondem à solicitação de token
 - a mesma ID de atividade é registrada em máquinas diferentes, o que permite solucionar problemas de uma solicitação de usuário em vários computadores, como o proxy do servidor de Federação (FSP)
 - a ID da atividade também será exibida no navegador do usuário se a solicitação de AD FS falhar de alguma forma, permitindo que o usuário comunique essa ID ao suporte técnico ou a ti.

![ActivityId](media/ad-fs-tshoot-logging/activityid2.png)

Para auxiliar no processo de solução de problemas, AD FS também registra o evento de ID do chamador sempre que o processo de emissão de token falha em um servidor AD FS. Esse evento contém o tipo de declaração e o valor de um dos tipos de declaração a seguir, supondo que essas informações foram passadas para o Serviço de Federação como parte de uma solicitação de token:
- https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountnameh
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upnh
- https://schemas.microsoft.com/ws/2008/06/identity/claims/upn
- http://schemas.xmlsoap.org/claims/UPN
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddressh
- https://schemas.microsoft.com/ws/2008/06/identity/claims/emailaddress
- http://schemas.xmlsoap.org/claims/EmailAddress
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
- https://schemas.microsoft.com/ws/2008/06/identity/claims/name
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier

O evento ID do chamador também registra a ID da atividade para permitir que você use essa ID de atividade para filtrar ou Pesquisar os logs de eventos em busca de uma solicitação específica.




## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)
