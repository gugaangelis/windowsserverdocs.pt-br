---
title: Solução de problemas - eventos de auditoria do AD FS e registro em log
description: Este documento descreve como usar os vários logs do AD FS para solucionar problemas
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 20e2d0747b98e7c7728230d0768506261f5b0d50
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825117"
---
# <a name="ad-fs-troubleshooting---events-and-logging"></a>Solucionando problemas do AD FS - eventos e registro em log
O AD FS fornece dois logs principais que podem ser usados na solução de problemas.  São eles:

- o logon de administrador
- o Log de rastreamento  
 
Cada um desses logs será explicado abaixo.

## <a name="admin-log"></a>Log do administrador
O log do administrador fornece informações de alto nível sobre os problemas que estão ocorrendo e são habilitados por padrão.

### <a name="to-view-the-admin-log"></a>Para exibir o log do administrador
1.  Abra o Visualizador de Eventos
2.  Expandir **logs de serviços e aplicativos**.
3.  Expandir **do AD FS**.
4.  Clique em **Admin**.

![aprimoramentos de auditoria](media/ad-fs-tshoot-logging/event1.PNG)  

## <a name="trace-log"></a>Log de rastreamento
O log de rastreamento é onde as mensagens detalhadas são registradas e serão o log mais úteis ao solucionar o problema. Uma vez que muitas informações de log de rastreamento podem ser geradas em um curto período de tempo, o que pode afetar o desempenho do sistema, os logs de rastreamento são desabilitados por padrão. 

### <a name="to-enable-and-view-the-trace-log"></a>Para habilitar e exibir o log de rastreamento
1.  Abra o Visualizador de Eventos
2.  Clique duas vezes em **logs de serviços e aplicativos** e selecione o modo de exibição e clique em **Mostrar Logs analíticos e depuração**.  Isso mostrará nós adicionais à esquerda.
![aprimoramentos de auditoria](media/ad-fs-tshoot-logging/event2.PNG)  
3.  Expanda o rastreamento do AD FS
4.  Clique com botão direito na depuração e selecione **Habilitar Log**.
![aprimoramentos de auditoria](media/ad-fs-tshoot-logging/event3.PNG)  


## <a name="event-auditing-information-for-ad-fs-on-windows-server-2016"></a>Informações de auditoria eventos para o AD FS no Windows Server 2016  
Por padrão, o AD FS no Windows Server 2016 tem um nível básico de auditoria habilitado.  Com a auditoria básica, os administradores verão eventos 5 ou para uma única solicitação.  Isso marca uma redução significativa no número de eventos, os administradores têm a analisar, para ver uma única solicitação.   O nível de auditoria pode ser aumentado ou diminuído a usando o cmdlet do PowerShell:  

```PowerShell
Set-AdfsProperties -AuditLevel 
```

A tabela a seguir explica os níveis de auditoria disponíveis.  

|Nível de Auditoria|Sintaxe do PowerShell|Descrição|  
|----- | ----- | ----- |
|Nenhuma|Set-AdfsProperties - AuditLevel None|A auditoria está desabilitada e nenhum evento será registrado.|  
|Básico (padrão)|Set-AdfsProperties - AuditLevel Basic|Não mais de 5 eventos serão registrados para uma única solicitação|  
|Verbose|Set-AdfsProperties - AuditLevel detalhado|Todos os eventos serão registrados.  Isso registrará uma quantidade significativa de informações por solicitação.|  
  
Para exibir o nível de auditoria atual, você pode usar o cmdlet do PowerShell:  Get-AdfsProperties.  
  
![aprimoramentos de auditoria](media/ad-fs-tshoot-logging/ADFS_Audit_1.PNG)  
  
O nível de auditoria pode ser aumentado ou diminuído a usando o cmdlet do PowerShell:  Set-AdfsProperties - AuditLevel.  
  
![aprimoramentos de auditoria](media/ad-fs-tshoot-logging/ADFS_Audit_2.png)  
  
## <a name="types-of-events"></a>Tipos de eventos  
Eventos do AD FS podem ser de tipos diferentes, com base em diferentes tipos de solicitações processadas pelo AD FS. Cada tipo de evento tem dados específicos associados a ele.  O tipo de eventos pode ser diferenciado entre as solicitações de logon (ou seja, solicitações de token) em vez de solicitações do sistema (chamadas de servidores como buscar informações de configuração).    

A tabela a seguir descreve os tipos básicos de eventos.  
  
|Tipo de Evento|ID de evento|Descrição| 
|----- | ----- | ----- | 
|Sucesso da validação de credencial atualizada|1202|Uma solicitação em que as novas credenciais são validadas com êxito pelo serviço de Federação. Isso inclui o WS-Trust, WS-Federation, SAML-P (primeira ramificação para gerar SSO) e pontos de extremidade OAuth autorizar.|  
|Erro de validação de credencial atualizada|1203|Uma solicitação em que a validação de credenciais atualizado falhou no serviço de Federação. Isso inclui o WS-Trust, WS-Fed, SAML-P (primeira ramificação para gerar SSO) e pontos de extremidade OAuth autorizar.|  
|Êxito de Token de aplicativo|1200|Uma solicitação em que um token de segurança é emitido com êxito pelo serviço de Federação. Para WS-Federation, isso é registrado quando a solicitação é processada com o artefato de SSO do SAML-P. (por exemplo, o cookie SSO).|  
|Falha de Token de aplicativo|1201|Uma solicitação em que a emissão de token de segurança falha no serviço de Federação. Para WS-Federation, isso é registrado quando a solicitação foi processada com o artefato de SSO do SAML-P. (por exemplo, o cookie SSO).|  
|Êxito na solicitação de alteração de senha|1204|Uma transação em que a alteração de senha solicitação foi processada com êxito pelo serviço de Federação.|  
|Erro de solicitação de alteração de senha|1205|Falha de uma transação em que a senha de solicitação de alteração a ser processado pelo serviço de Federação.| 
|Sair com êxito|1206|Descreve uma solicitação de saída com êxito.|  
|Sair com falha|1207|Descreve uma solicitação de saída com falha.|  

## <a name="security-auditing"></a>Auditoria de segurança
Auditoria de segurança da conta de serviço do AD FS, às vezes, pode ajudar no rastreamento de problemas com atualizações de senha, registro em log de solicitação/resposta, os cabeçalhos de solicitação contect e resultados de registro do dispositivo.  Auditoria da conta de serviço do AD FS está desabilitada por padrão.

### <a name="to-enable-security-auditing"></a>Para habilitar a auditoria de segurança
1.       Clique em Iniciar, aponte para **programas**, aponte para **ferramentas administrativas**e, em seguida, clique em **política de segurança Local**.
2.       Navegue até a pasta **Configurações de Segurança\Políticas Locais\Gerenciamento de Direitos do Usuário** e clique duas vezes em **Gerar Auditorias de Segurança**.
3.       Sobre o **configuração de segurança Local** guia, verifique se a conta de serviço do AD FS está listada. Se não estiver presente, clique em Adicionar usuário ou grupo e adicioná-lo à lista e clique em Okey.
4.       Abra um prompt de comando com privilégios elevados e execute o seguinte comando para habilitar a auditoria auditpol.exe /Set /subcategory: "Aplicativo gerado" /Success /failure:enable 5.       Fechar **política de segurança Local**e, em seguida, abra o snap-in de gerenciamento do AD FS.
 
Para abrir o snap-in de gerenciamento do AD FS, clique em Iniciar, aponte para programas, aponte para ferramentas administrativas e, em seguida, clique em gerenciamento do AD FS.
 
6.       No painel de ações, clique em Editar 7 de propriedades de serviço de Federação.       Na caixa de diálogo Propriedades do serviço de federação, clique na guia eventos. 8.       Selecione o **auditorias com êxito** e **auditorias com falha** caixas de seleção.
9.       Clique em OK.

![aprimoramentos de auditoria](media/ad-fs-tshoot-logging/event4.PNG)  
 
>[!NOTE]
>As instruções acima são usadas somente quando o AD FS está em um servidor autônomo do membro.  Se o AD FS está em execução em um controlador de domínio, em vez de diretiva de segurança Local, use o **política de controlador de domínio padrão** localizado em **controladores de gerenciamento/floresta/domínios/domínio de diretiva de grupo**.  Clique em Editar e navegue até **computador \ Diretivas \ Configurações de segurança locais \ Atribuição Rights Management**

## <a name="windows-communication-foundation-and-windows-identity-foundation-messages"></a>Mensagens do Windows Communication Foundation e o Windows Identity Foundation
Além do log de rastreamento, às vezes, você talvez precise exibir mensagens do Windows Communication Foundation (WCF) e o Windows Identity Foundation (WIF) para solucionar um problema. Isso pode ser feito modificando o **Microsoft.IdentityServer.ServiceHost.Exe.Config** arquivo no servidor do AD FS. 

Esse arquivo está localizado em **\Windows\ADFS < % % de raiz do sistema >** e está no formato XML. As partes relevantes do arquivo são mostradas abaixo: 
```
<!-- To enable WIF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="Microsoft.IdentityModel" switchValue="Off"> … </source>

<!-- To enable WCF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="System.ServiceModel" switchValue="Off" > … </source>
```


Depois de aplicar essas alterações, salve a configuração e reinicie o serviço do AD FS. Depois de habilitar esses rastreamentos Configurando as opções apropriadas, eles aparecerão no log de rastreamento do AD FS no Visualizador de eventos do Windows.

## <a name="correlating-events"></a>Correlação de eventos
Uma das coisas mais difíceis de solucionar problemas é problemas de acesso que geram muita erro ou eventos de depuração.

Para ajudar com isso, o AD FS correlaciona todos os eventos que são registrados no Visualizador de eventos, na administração e os logs de depuração, que correspondem a uma determinada solicitação usando um globalmente exclusivo GUID (identificador exclusivo) chamado a ID da atividade. Essa ID é gerada quando a solicitação de emissão de token é inicialmente apresentada para o aplicativo web (para aplicativos usando o perfil do solicitador passivo) ou as solicitações enviadas diretamente para o provedor de declarações (para aplicativos usando o WS-Trust). 

![ActivityId](media/ad-fs-tshoot-logging/activityid1.png)

Essa ID de atividade permanece o mesmo para a duração inteira da solicitação e é registrado como parte de cada evento de eventos registrados visualizador para a solicitação. Isso quer dizer que:
 - que filtrando ou pesquisando o Visualizador de eventos usando essa ID pode ajudar a manter o controle de todos os eventos relacionados que correspondem à solicitação de token de atividade
 - a mesma ID de atividade é registrada em máquinas diferentes, que permite a solução de problemas de uma solicitação de usuário em vários computadores, como o proxy do servidor de Federação (FSP)
 - a ID de atividade também aparecerá no navegador do usuário se a solicitação do AD FS falhar de forma alguma, permitindo que o usuário para se comunicar essa ID ao suporte técnico ou suporte de TI.

![ActivityId](media/ad-fs-tshoot-logging/activityid2.png)

Para auxiliar no processo de solução de problemas, o AD FS também registra o evento de ID do chamador sempre que o processo de emissão de token falhar em um servidor do AD FS. Esse evento contém o tipo de declaração e o valor de um dos seguintes tipos de declaração, supondo que essa informação foi passada para o serviço de federação como parte de uma solicitação de token:
- http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountnameh
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upnh
- http://schemas.microsoft.com/ws/2008/06/identity/claims/upn
- http://schemas.xmlsoap.org/claims/UPN
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddressh
- http://schemas.microsoft.com/ws/2008/06/identity/claims/emailaddress 
- http://schemas.xmlsoap.org/claims/EmailAddress
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
- http://schemas.microsoft.com/ws/2008/06/identity/claims/name
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier 

O evento de ID do chamador também registra a ID de atividade para permitir que você use a ID da atividade para filtrar ou pesquisar os logs de eventos para uma determinada solicitação.




## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)
