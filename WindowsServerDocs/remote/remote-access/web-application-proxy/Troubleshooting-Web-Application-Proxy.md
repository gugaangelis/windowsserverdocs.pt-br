---
title: Solução de problemas do Proxy de aplicativo Web
description: ''
author: kgremban
manager: femila
ms.date: 07/13/2016
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a2fef55d-747b-4e20-8f21-5f8807e7ef87
ms.technology: web-app-proxy
ms.openlocfilehash: c63dbc415765cbe60bfbde7bf180f3dd967d6ab8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841577"
---
# <a name="troubleshooting-web-application-proxy"></a>Solução de problemas do Proxy de aplicativo Web

>Aplica-se a: Windows Server 2016

**Este conteúdo é relevante para a versão local do Proxy de aplicativo Web. Para habilitar o acesso seguro a aplicativos locais pela nuvem, consulte o [conteúdo de Proxy de aplicativo do Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**  
  
Esta seção fornece procedimentos de solução de problemas para o Proxy de aplicativo de Web, incluindo soluções e explicações sobre o evento. Há três locais onde os erros são exibidos:  
  
-   No console do administrador de Proxy de aplicativo Web  
  
    Cada ID do evento listado no console do administrador pode ser exibido no Visualizador de eventos do Windows e descrições correspondentes e soluções forem descobertos abaixo.  
  
    Abra o Visualizador de eventos e procure eventos relacionados ao Proxy de aplicativo Web em **Applications and Services Logs** > **Microsoft** > **Windows**  >  **Proxy de aplicativo web** > **Admin**  
  
    ![](media/Troubleshooting-Web-Application-Proxy/WebApplicationProxyTroubleshooting.png)  
  
    Se necessário, logs detalhados estão disponíveis por ativar a análise e logs de depuração e ativar o log de sessão do Proxy de aplicativo Web, encontradas no Visualizador de eventos do Windows em \ Microsoft \ Windows \ Proxy de aplicativo Web \ administrador.  
  
-   Em erros do PowerShell  
  
    Eventos para problemas encontrados durante a configuração são exibidos no PowerShell.  
  
    Todos os erros são apresentados ao usuário do PowerShell usando os prompts de erro padrão do PowerShell. Todos os comandos do PowerShell são registrados como eventos. Todos os eventos que ocorrem no PowerShell são listados no Visualizador de eventos do Windows com o número de identificação 12016 e são definidos na seção PowerShell abaixo.  
  
-   No analisador de práticas recomendadas  
  
    Esses eventos são descritos no [analisador de práticas recomendadas para o Proxy de aplicativo Web](assetId:///995f5cea-e334-4ce6-a14c-6a2d03c0c79a)  
  
## <a name="powershell-messages"></a>Mensagens do PowerShell  
  
|Evento ou sintoma|Causa possível|Resolução|  
|--------------------|------------------|--------------|  
|O certificado de confiança ("ProxyTrust de ADFS - <WAP machine name>") não é válido|Isso pode ser provocado por qualquer um dos seguintes:<br /><br />-A máquina de Proxy de aplicativo estava inoperante por muito tempo.<br />-Desconexões entre o Proxy de aplicativo Web e o AD FS<br />-Problemas de infraestrutura de certificado<br />-Alterações no computador do AD FS ou o processo de renovação entre o Proxy de aplicativo Web e o AD FS não foram executado conforme planejado a cada 8 horas, então eles precisam para renovar a relação de confiança<br />-O relógio do computador Proxy de aplicativo Web e o AD FS não estão sincronizadas.|Verifique se que os relógios estão sincronizados. Execute o cmdlet Install-WebApplicationProxy.|  
|Dados de configuração não foi encontrados no AD FS|Isso pode ser porque o Proxy de aplicativo Web não foi totalmente instalado ainda ou devido a alterações no banco de dados do AD FS ou corrupção do banco de dados.|Execute o Cmdlet Install-WebApplicationProxy.|  
|Ocorreu um erro quando o Proxy de aplicativo Web tentou ler a configuração do AD FS.|Isso pode indicar que o AD FS não está acessível ou que o AD FS encontrou um problema interno ao tentar ler a configuração do banco de dados do AD FS.|Verifique se o AD FS está acessível e funcionando corretamente.|  
|Os dados de configuração armazenados no AD FS estão corrompidos ou o Proxy de aplicativo Web não foi possível analisá-lo.<br /><br />OU<br /><br />Proxywas do aplicativo Web não é possível recuperar a lista de partes confiáveis do AD FS.|Isso pode ocorrer se os dados de configuração foi modificados no AD FS.|Reinicie o Proxyservice de aplicativo Web. Se o problema persistir, execute o Cmdlet Install-WebApplicationProxy.|  
  
## <a name="administrator-console-events"></a>Eventos do Console de administrador  
Os seguintes eventos do console de administrador são geralmente indicativos de erros de autenticação, tokes inválidos ou expiraram cookies.  
  
|Evento ou sintoma|Causa possível|Resolução|  
|--------------------|------------------|--------------|  
|11005<br /><br />Proxy de aplicativo Web não foi possível criar a chave de criptografia de cookie usando o segredo da configuração.|O parâmetro de configuração global "AccessCookiesEncryptionKey" foi alterado pelo comando do PowerShell: Set-WebApplicationProxyConfiguration -RegenerateAccessCookiesEncryptionKey|Nenhuma ação é necessária. O cookie problemático foi removido e o usuário foi redirecionado para o STS para autenticação.|  
|12000<br /><br />Proxy de aplicativo Web não foi possível verificar as alterações de configuração pelo menos 60 minutos|Proxy de aplicativo Web não pode acessar a configuração de Proxy de aplicativo Web usando o comando Get-WebApplicationProxyConfiguration/aplicativo. Isso geralmente é causado por falta de conectividade com o AD FS ou a necessidade de renovar a confiança com o AD FS.|Verifique a conectividade com o AD FS. Você pode fazer isso usando o https://<FQDN_AD_FS_Proxy>/FederationMetadata/2007-06/FederationMetadata.xmlMake link-se de que não há relação de confiança estabelecida entre o AD FS e Proxy de aplicativo Web. Se essas soluções não funcionarem, execute o Cmdlet Install-WebApplicationProxy.|  
|12003<br /><br />Proxy de aplicativo Web não foi possível analisar o cookie de acesso.|Isso pode indicar que o Proxy de aplicativo Web e o AD FS não está conectado ou que não recebem a mesma configuração.|Verifique a conectividade com o AD FS. Você pode fazer isso usando o https://<FQDN_AD_FS_Proxy>/FederationMetadata/2007-06/FederationMetadata.xmlMake link-se de que não há relação de confiança estabelecida entre o AD FS e Proxy de aplicativo Web. Se essas soluções não funcionarem, execute o Cmdlet Install-WebApplicationProxy.|  
|12004<br /><br />Proxy de aplicativo Web recebeu uma solicitação com um cookie de acesso inválidos.|Esse evento pode indicar que o Proxy de aplicativo Web e o AD FS não está conectado ou que não recebem a mesma configuração.<br /><br />Se você executou o parâmetro "AccessCookiesEncryptionKey" foi chaged por Set-WebApplicationProxyConfiguration - RegenerateAccessCookiesEncryptionKey um comando do PowerShell, esse evento é normal e requer que não há etapas de resolução.|Verifique a conectividade com o AD FS. Você pode fazer isso usando o https://<FQDN_AD_FS_Proxy>/FederationMetadata/2007-06/FederationMetadata.xmlMake link-se de que não há relação de confiança estabelecida entre o AD FS e Proxy de aplicativo Web. Se essas soluções não funcionarem, execute o Cmdlet Install-WebApplicationProxy.|  
|12008<br /><br />Proxy de aplicativo Web excedeu o número máximo de tentativas de autenticação Kerberos permitidas para o servidor de back-end.|Esse evento pode indicar uma configuração incorreta entre o Proxy de aplicativo Web e o servidor de aplicativos de back-end ou um problema na configuração de data e hora nos dois computadores.|O servidor de back-end recusou o tíquete Kerberos criado pelo Proxy de aplicativo Web. Verifique se que a configuração de Proxy de aplicativo Web e o servidor de aplicativos de back-end estão configurados corretamente.<br /><br />Certifique-se de que a configuração de data e hora no Proxy de aplicativo Web e o servidor de aplicativos de back-end estão sincronizadas.|  
|12011<br /><br />Proxy de aplicativo Web recebeu uma solicitação com uma assinatura do cookie de acesso não é válido.|Esse evento pode indicar que o Proxy de aplicativo Web e o AD FS não está conectado ou que não recebem a mesma configuração. Se você executou o parâmetro "AccessCookiesEncryptionKey" foi chaged por Set-WebApplicationProxyConfiguration - RegenerateAccessCookiesEncryptionKey um comando do PowerShell, esse evento é normal e requer que não há etapas de resolução.|Verifique a conectividade com o AD FS. Você pode fazer isso usando o https://<FQDN_AD_FS_Proxy>/FederationMetadata/2007-06/FederationMetadata.xmlMake link-se de que não há relação de confiança estabelecida entre o AD FS e Proxy de aplicativo Web. Se essas soluções não funcionarem, execute o Cmdlet Install-WebApplicationProxy.|  
|12027<br /><br />Proxy encontrou um erro inesperado ao processar a solicitação. O nome fornecido não é um nome de conta corretamente formado.|Esse evento pode indicar uma configuração incorreta entre o Proxy de aplicativo Web e o servidor do controlador de domínio ou um problema na configuração de data e hora nos dois computadores.|O controlador de domínio recusou o tíquete Kerberos criado pelo Proxy de aplicativo Web. Verifique se a configuração de Proxy de aplicativo Web e o servidor de aplicativos de back-end estão configurados corretamente, especialmente a configuração de SPN. Verifique se o Proxy de aplicativo Web está integrado ao domínio no mesmo domínio que o controlador de domínio para garantir que o controlador de domínio estabeleça a relação de confiança com Proxy.Make de aplicativo Web se que a configuração de data e hora no Proxy de aplicativo da Web e o controlador de domínio são sincronizadas.|  
|13012<br /><br />Proxy de aplicativo Web recebeu uma assinatura de token de borda inválidos||Verifique se você atualizou o Proxy de aplicativo Web com [2955164 KB](https://go.microsoft.com/fwlink/?LinkId=400701).|  
|13013<br /><br />Proxy de aplicativo Web recebeu uma solicitação que continha um token de borda expirada.|Proxy de aplicativo Web e o AD FS não tem relógios sincronizados.|Sincronize os relógios entre o Proxy de aplicativo Web e o AD FS.|  
|13014<br /><br />Proxy de aplicativo Web recebeu uma solicitação com um token de borda inválidos. O token não é válido porque não pôde ser analisado.|Isso pode indicar um problema com a configuração do AD FS.|Verifique a configuração do AD FS e, se necessário, restaurar a configuração padrão.|  
|13015<br /><br />Proxy de aplicativo Web recebeu uma solicitação com um cookie de acesso expirado.|Isso pode indicar que os relógios não estão sincronizados.|Se você estiver trabalhando com um cluster de computadores de Proxy de aplicativo Web, certifique-se de que a data e hora de computadores esteja sincronizado.|  
|13016<br /><br />Proxy de aplicativo Web não é possível recuperar um tíquete Kerberos em nome do usuário porque não há nenhum UPN no token de borda ou no cookie de acesso.|Há um problema com a configuração de STS.|Corrigi a configuração de declaração UPN no STS.|  
|13019<br /><br />Proxy de aplicativo Web não é possível recuperar um tíquete Kerberos em nome do usuário devido ao seguinte erro geral da API|Esse evento pode indicar uma configuração incorreta entre o Proxy de aplicativo Web e o servidor do controlador de domínio ou um problema na configuração de data e hora nos dois computadores.|O controlador de domínio recusou o tíquete Kerberos criado pelo Proxy de aplicativo Web. Verifique se a configuração de Proxy de aplicativo Web e o servidor de aplicativos de back-end estão configurados corretamente, especialmente a configuração de SPN. Verifique se o Proxy de aplicativo Web está integrado ao domínio no mesmo domínio que o controlador de domínio para garantir que o controlador de domínio estabeleça a relação de confiança com Proxy.Make de aplicativo Web se que a configuração de data e hora no Proxy de aplicativo da Web e o controlador de domínio são sincronizadas.|  
|13020<br /><br />Proxy de aplicativo Web não é possível recuperar um tíquete Kerberos em nome do usuário porque o SPN do servidor de back-end não está definido.|Esse evento pode indicar uma configuração incorreta entre o Proxy de aplicativo Web e o servidor do controlador de domínio ou um problema na configuração de data e hora nos dois computadores.|O controlador de domínio recusou o tíquete Kerberos criado pelo Proxy de aplicativo Web. Verifique se a configuração de Proxy de aplicativo Web e o servidor de aplicativos de back-end estão configurados corretamente, especialmente a configuração de SPN. Verifique se o Proxy de aplicativo Web está integrado ao domínio no mesmo domínio que o controlador de domínio para garantir que o controlador de domínio estabeleça a relação de confiança com Proxy.Make de aplicativo Web se que a configuração de data e hora no Proxy de aplicativo da Web e o controlador de domínio são sincronizadas.|  
|13022<br /><br />Proxy de aplicativo Web não pode autenticar o usuário porque o servidor de back-end responde às tentativas de autenticação Kerberos com um erro HTTP 401.|Esse evento pode indicar uma configuração incorreta entre o Proxy de aplicativo Web e o servidor de aplicativos de back-end ou um problema na configuração de data e hora nos dois computadores.|O servidor de back-end recusou o tíquete Kerberos criado pelo Proxy de aplicativo Web. Verifique se que a configuração de Proxy de aplicativo Web e o servidor de aplicativos de back-end estão configurados corretamente. Certifique-se de que a configuração de data e hora no Proxy de aplicativo Web e o servidor de aplicativos de back-end estão sincronizadas.|  
|13025<br /><br />O cliente não apresentou um certificado SSL para o Proxy de aplicativo Web.|Esse evento pode indicar um problema na configuração de data e hora.|Certifique-se de que a infraestrutura de certificado é válida e que a data e hora de Proxy de aplicativo Web e o AD FS sejam sincronizadas. Certifique-se de que a impressão digital configurada para o Proxy de aplicativo Web é a correta.|  
|13026<br /><br />O cliente apresentado a um certificado SSL para o Proxy de aplicativo Web, mas o certificado não é válido: o certificado não coincide com a impressão digital.|Esse evento pode indicar um problema na configuração de data e hora.|Certifique-se de que a infraestrutura de certificado é válida e que a data e hora de Proxy de aplicativo Web e o AD FS sejam sincronizadas. Certifique-se de que a impressão digital configurada para o Proxy de aplicativo Web é a correta.|  
|13028<br /><br />Proxy de aplicativo Web recebeu uma solicitação que continha um token de borda que ainda não é válido.|Esse evento pode indicar um problema na configuração de data e hora.|Certifique-se de que a infraestrutura de certificado é válida e que a data e hora de Proxy de aplicativo Web e o AD FS sejam sincronizadas.|  
|13030<br /><br />O cliente apresentado a um certificado SSL para o Proxy de aplicativo Web, mas o provedor de confiabilidade não confia a autoridade de certificação que emitiu o certificado de cliente.|Esse evento pode indicar um problema na configuração de data e hora.|Certifique-se de que a infraestrutura de certificado é válida e que a data e hora de Proxy de aplicativo Web e o AD FS sejam sincronizadas. Certifique-se de que a impressão digital configurada para o Proxy de aplicativo Web é a correta.|  
|13031<br /><br />O cliente apresentado a um certificado SSL para o Proxy de aplicativo Web, mas a cadeia de certificados terminou em um certificado raiz que não é confiável pelo provedor de confiança.|Esse evento pode indicar um problema na configuração de data e hora.|Certifique-se de que a infraestrutura de certificado é válida e que a data e hora de Proxy de aplicativo Web e o AD FS sejam sincronizadas. Certifique-se de que a impressão digital configurada para o Proxy de aplicativo Web é a correta.|  
|13032<br /><br />O cliente apresentado a um certificado SSL para o Proxy de aplicativo Web, mas o certificado não era válido para o uso solicitado.|Esse evento pode indicar um problema na configuração de data e hora.|Certifique-se de que a infraestrutura de certificado é válida e que a data e hora de Proxy de aplicativo Web e o AD FS sejam sincronizadas. Certifique-se de que a impressão digital configurada para o Proxy de aplicativo Web é a correta.|  
|13033<br /><br />O cliente apresentado a um certificado SSL para o Proxy de aplicativo Web, mas o certificado não estava dentro do período de validade ao verificar no relógio do sistema atual ou o carimbo de hora no arquivo assinado.|Esse evento pode indicar um problema na configuração de data e hora.|Certifique-se de que a infraestrutura de certificado é válida e que a data e hora de Proxy de aplicativo Web e o AD FS sejam sincronizadas. Certifique-se de que a impressão digital configurada para o Proxy de aplicativo Web é a correta.|  
|13034<br /><br />O cliente apresentado a um certificado SSL para o Proxy de aplicativo Web, mas o certificado não era válido.|Esse evento pode indicar um problema na configuração de data e hora.|Certifique-se de que a infraestrutura de certificado é válida e que a data e hora de Proxy de aplicativo Web e o AD FS sejam sincronizadas. Certifique-se de que a impressão digital configurada para o Proxy de aplicativo Web é a correta.|  
  
Os seguintes eventos do console de administrador são geralmente uma indicação de problemas de ter que fazer com a configuração, como o provisionamento, a solicitação que não forem bem-sucedidas, os servidores de back-end que estão inacessíveis e estouros de buffer.  
  
|Evento ou sintoma|Causa possível|Resolução|  
|--------------------|------------------|--------------|  
|12019<br /><br />Proxy de aplicativo Web não foi possível criar um ouvinte para a URL a seguir.|Uma possível causa para o evento é que o outro serviço está escutando para a mesma URL.|O administrador deve garantir que ninguém ouve ou associa às mesmas URLs. Para verificar isso, execute o comando: netsh http show urlacl. Se essa URL é usada por outro componente em execução no computador Proxy de aplicativo Web, removê-lo ou usar uma URL diferente para publicar os aplicativos por meio do Proxy de aplicativo Web.|  
|12020<br /><br />Proxy de aplicativo Web não pôde criar uma reserva para a URL a seguir.|Uma possível causa para o evento é que o outro serviço tem uma reserva na mesma URL.|O administrador deve se certificar de que ninguém é associado às mesmas URLs. Para verificar isso, execute o comando: netsh http show urlacl. Se essa URL é usada por outro componente em execução no computador Proxy de aplicativo Web, removê-lo ou usar uma URL diferente para publicar os aplicativos por meio do Proxy de aplicativo Web.|  
|12021<br /><br />Proxy de aplicativo Web não foi possível associar o certificado SSL do servidor. Todas as outras definições de configuração foram aplicadas.|Não é possível criar e definir um registro de configuração de dados do certificado SSL.|Certifique-se de que as impressões digitais de certificado que estão configuradas para Proxyapplications de aplicativo Web esteja instaladas em todos os computadores de Proxy de aplicativo Web com uma chave privada no repositório do computador local.|  
|13001<br /><br />O certificado SSL do servidor apresentado ao Proxy de aplicativo Web pelo servidor de back-end não é válido. o certificado não é confiável.|Um ou mais erros foram encontrados no certificado do Secure Sockets Layer (SSL) enviado pelo servidor. Isso pode indicar que o servidor de back-end fornecido um SSL que não era válido ou que não há nenhuma relação de confiança entre o Proxy de aplicativo Web e o servidor de back-end.|Valide um certificado SSL do servidor de back-end. Certifique-se de que o computador de Proxy de aplicativo Web é configurado com a raiz de direito CAs para confiar no certificado do servidor de back-end.|  
|13006|Quando o código de erro é 0x80072ee7, o failurrre é causado pela incapacidade de resolver a URL do servidor de back-end. Outros códigos de erro são descritos em [https://msdn.microsoft.com/library/windows/desktop/aa384110(v=vs.85)](https://msdn.microsoft.com/library/windows/desktop/aa384110(v=vs.85))|Verifique se a URL do servidor de back-end está correta e se o seu nome pode ser resolvido corretamente no computador Proxy de aplicativo Web.|  
|13007<br /><br />A resposta HTTP do servidor de back-end não foi recebida dentro do intervalo esperado.|A solicitação do servidor de back-end atingiu o tempo limite ou está lenta ou não responde.|Verifique a configuração do servidor de back-end. Se estiver muito lento, verifique a conectividade com o servidor de back-end e considere também a alterar o cmdlet de parâmetro de configuração global do Proxy de aplicativo Web para InactiveTransactionsTimeoutSec.|  
  
## <a name="see-also"></a>Consulte também  
[O que há de novo no Proxy de aplicativo Web no Windows Server 2016](web-application-proxy-windows-server.md)  
[Trabalhando com o Proxy de aplicativo Web](assetId:///b607b717-2172-4271-98d1-fa8162e0bb2e)  
  

