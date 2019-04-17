---
ms.assetid: 963a3d37-d5f1-4153-b8d5-2537038863cb
title: "Práticas recomendadas para segura de planejamento e implantação do AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ed8c36d4bec455879ffd00ad40b72fd5e90484ab
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="best-practices-for-secure-planning-and-deployment-of-ad-fs"></a>Práticas recomendadas para segura de planejamento e implantação do AD FS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico fornece informações de práticas recomendadas para ajudá-lo a planejar e avaliar a segurança ao projetar sua implantação de serviços de Federação do Active Directory (AD FS). Este tópico é um ponto de partida para examinar e avaliar considerações que afetam a segurança geral do seu uso do AD FS. As informações neste tópico destina-se para complementar e estender seu planejamento de segurança existentes e outras práticas recomendadas de design.  
  
## <a name="core-security-best-practices-for-ad-fs"></a>Práticas recomendadas de segurança básica do AD FS  
As seguintes práticas recomendadas de core são comuns a todas as instalações do AD FS onde você deseja melhorar ou estender a segurança do seu design ou implantação:  
  
-   **Use o Assistente de configuração de segurança para aplicar práticas recomendadas de segurança específicos do AD FS para federação servidores e computadores de proxy do servidor de Federação**  
  
    O Assistente de configuração de segurança (ACS) é uma ferramenta que vem pré-instalado em todos os Windows Server 2008, Windows Server 2008 R2 e Windows Server 2012 computadores. Você pode usá-lo para aplicar segurança práticas recomendadas que podem ajudar a reduzem a superfície de ataque para um servidor, com base nas funções de servidor que você está instalando.  
  
    Quando você instala o AD FS, o programa de instalação cria função arquivos de extensão que você pode usar com o ACS para criar uma política de segurança que serão aplicadas para a AD FS servidor função específica (servidor de Federação ou proxy do servidor de federação) que você escolher durante a instalação.  
  
    Cada arquivo de extensão de função que é instalado representa o tipo de função e subfunção para os quais cada computador é configurado. Os seguintes arquivos de extensão de função são instalados no diretório C:WindowsADFSScw:  
  
    -   Farm.XML  
  
    -   SQLFarm.xml  
  
    -   StandAlone.xml  
  
    -   Proxy.XML (esse arquivo está presente somente se você configurou o computador na função de proxy do servidor de federação).  
  
    Para aplicar as extensões de função do AD FS no ACS, conclua as seguintes etapas em ordem:  
  
    1.  Instalar o AD FS e escolher a função de servidor apropriado para aquele computador. Para obter mais informações, consulte [instalar o serviço de função de Proxy de serviço de Federação](../../ad-fs/deployment/Install-the-Federation-Service-Proxy-Role-Service.md) no guia de implantação do AD FS.  
  
    2.  Registre o arquivo de extensão de função apropriada usando a ferramenta de linha de comando Scwcmd. Consulte a tabela a seguir para obter detalhes sobre como usar essa ferramenta na função para que seu computador está configurado.  
  
    3.  Verifique se que o comando foi concluída com êxito, examinando o arquivo SCWRegister_log.xml, que está localizado no diretório WindowssecurityMsscwLogs.  
  
    Você deve executar todas essas etapas em cada servidor de Federação ou o computador de proxy de servidor de federação para o qual você deseja aplicar políticas de segurança do AD FS – com base em ACS.  
  
    A tabela a seguir explica como registrar o ACS função de extensão apropriado, com base na função do servidor do AD FS que você escolheu no computador em que você instalou o AD FS.  
  
    |Função de servidor do AD FS|Banco de dados de configuração do AD FS usado|Digite o seguinte comando em um prompt de comando:|  
    |---------------------|-------------------------------------|---------------------------------------------------|  
    |Servidor de Federação autônomo|Banco de dados interno do Windows|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwStandAlone.xml"`|  
    |Servidor de Federação ingressado no farm|Banco de dados interno do Windows|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwFarm.xml"`|  
    |Servidor de Federação ingressado no farm|SQL Server|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwSQLFarm.xml"`|  
    |Proxy do servidor de Federação|N/D|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwProxy.xml"`|  
  
    Para saber mais sobre os bancos de dados que você pode usar com o AD FS, consulte [a função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
-   **Use a detecção de repetição token em situações em que segurança é uma preocupação muito importante, por exemplo, quando quiosques são usados.**  
    Detecção de repetição token é um recurso do AD FS que garante que qualquer tentativa de uma solicitação de token é feita para o serviço de federação de repetição é detectada e a solicitação é descartada. Detecção de repetição token é habilitada por padrão. Ele funciona para o perfil passivo WS-Federation e o perfil de segurança asserção SAML Markup Language () WebSSO, garantindo que o mesmo token de nunca é usado mais de uma vez.  
  
    Quando o serviço de Federação é iniciado, ele inicia criar um cache de todas as solicitações tokens que ele atenda. Ao longo do tempo, à medida que as solicitações de tokens subsequentes são adicionadas no cache, a capacidade de detectar qualquer tentativa de uma solicitação de token de repetição várias vezes aumenta para o serviço de Federação. Se você desativar a detecção de repetição token e mais tarde optar por habilitá-lo novamente, lembre-se de que o serviço de Federação ainda aceitará tokens por um período de tempo que pode ter sido usado anteriormente, até que o cache repetição teve permissão tempo suficiente para recriar seu conteúdo. Para obter mais informações, consulte [a função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
-   **Use a criptografia de token, especialmente se você estiver usando a resolução de artefato SAML suporte.**  
  
    Criptografia de tokens é altamente recomendável para aumentar a segurança e proteção contra ataques man-in-the-middle (MITM) potenciais pode ser testado em relação a implantação do AD FS. Usando a criptografia de uso pode ter um pequeno impacto em todo, mas em geral, ele deve não seja normalmente observado e em implantações muitos benefícios para maior segurança excederam qualquer custo em termos de desempenho do servidor.  
  
    Para habilitar a criptografia de token, o primeiro conjunto adicionar um certificado de criptografia para suas terceira relações de confiança de terceiros. Você pode configurar um certificado de criptografia seja ao criar uma dependência de terceiros confiança ou posterior. Para adicionar um certificado de criptografia mais tarde para uma relação de confiança terceira por terceiros existente, você pode definir um certificado para uso no **criptografia** guia nas propriedades de confiança enquanto estiver usando o snap-in do AD FS. Para especificar um certificado de uma relação de confiança existente usando os cmdlets do AD FS, use o parâmetro EncryptionCertificate do **conjunto ClaimsProviderTrust** ou **conjunto RelyingPartyTrust** cmdlets. Para definir um certificado para o serviço de Federação usar quando descriptografia de tokens, use o **conjunto ADFSCertificate** cmdlet e especifique "`Token-Encryption`" para o *CertificateType* parâmetro. Habilitando e desabilitando a criptografia para específico terceiro confiam pode ser feito usando o *EncryptClaims* parâmetro do **conjunto RelyingPartyTrust** cmdlet.  
  
-   **Utilizar proteção estendida para autenticação**  
  
    Para ajudar a proteger as implantações, você pode definir e usar a proteção estendida para o recurso de autenticação com o AD FS. Esta configuração especifica o nível de proteção estendida para autenticação com suporte por um servidor de Federação.  
  
    Proteção estendida para autenticação ajuda a proteger de ataques man-in-the-middle (MITM), em que um invasor intercepta as credenciais de cliente e os encaminha para um servidor. Proteção contra esses ataques se torna possível por meio de um canal de associação Token (CBT) que pode ser necessária, permissão ou não é exigido pelo servidor quando ele estabelece comunicações com os clientes.  
  
    Para habilitar o recurso de proteção estendida, use o **ExtendedProtectionTokenCheck** parâmetro no **conjunto ADFSProperties** cmdlet. Valores possíveis para essa configuração e o nível de segurança que fornecem os valores são descritos na tabela a seguir.  
  
    |Valor do parâmetro|Nível de segurança|Configuração de proteção|  
    |-------------------|------------------|----------------------|  
    |Exigir|Servidor é totalmente protegido.|Proteção estendida é imposta e sempre necessária.|  
    |Permitir|Servidor é parcialmente protegido.|Proteção estendida é aplicada em sistemas envolvidos foram corrigidos para dar suporte a ele.|  
    |None|Servidor é vulnerável.|Proteção estendida não é obrigatório.|  
  
-   **Se você estiver usando o rastreamento e registro em log, garantir a privacidade das informações confidenciais.**  
  
    AD FS não, por padrão, expor ou rastrear informações pessoalmente identificáveis (PII) diretamente como parte do serviço de Federação ou operações normais. Quando o log de eventos e o log de rastreamento de depuração estão habilitados no AD FS, no entanto, dependendo da política de declarações que você configure algumas declarações de tipos e seus valores associados podem conter PII que pode ser registrado no evento AD FS ou logs de rastreamento.  
  
    Portanto, a aplicação de controle de acesso na configuração do AD FS e seus arquivos de log é altamente recomendável. Se você não quiser que esse tipo de informação a serem visível, você deve desabilitar efetuando ou filtrar qualquer PII ou dados confidenciais em seus logs antes de compartilhá-los com outras pessoas.  
  
    As dicas a seguir podem ajudá-lo a impedir que o conteúdo de um arquivo de log sejam expostos inadvertidamente:  
  
    -   Certifique-se de que o log de eventos do AD FS e arquivos de log de rastreamento são protegidos por listas de controle de acesso (ACL) que limitam o acesso apenas aos administradores confiáveis que exigem acesso a eles.  
  
    -   Não copiar ou arquivar os arquivos de log usando extensões de arquivo ou caminhos que podem ser veiculados facilmente usando uma solicitação da Web. Por exemplo, a extensão de nome de arquivo. XML não é uma opção de segurança. Você pode verificar o guia de administração do Internet Information Services (IIS) para ver uma lista de extensões que podem ser atendidas.  
  
    -   Se você revisar o caminho para o arquivo de log, certifique-se de especificar um caminho absoluto para o local de arquivo de log, que deve ser fora da Web host virtual (raiz virtual) público diretório raiz para impedir que ele seja acessada por um participante externo usando um navegador da Web.  

-   **AD FS bloqueio Extranet proteção**  
    
    Em caso de um ataque na forma de solicitações de autenticação com senhas invalid(bad) que vêm por meio do Proxy de aplicativo Web, bloqueio extranet AD FS permite proteger os usuários contra um bloqueio de conta AD FS. Além de proteger os usuários contra um AD FS bloqueio de conta, bloqueio extranet AD FS também protege contra ataques de detecção de senha de força bruta.  Para obter mais informações, consulte [AD FS Extranet bloqueio proteção](../../ad-fs/operations/Configure-AD-FS-Extranet-Lockout-Protection.md).  
  
## <a name="sql-serverspecific-security-best-practices-for-ad-fs"></a>SQL Server – específicas práticas de segurança do AD FS  
As seguintes práticas recomendadas de segurança são específicas para o uso de Microsoft SQL Server® ou banco de dados interno do Windows (trabalho) quando essas tecnologias de banco de dados são usadas para gerenciar os dados no AD FS design e implantação.  
  
> [!NOTE]  
> Essas recomendações se destinam a estender, mas não substituir, diretrizes de segurança de produto do SQL Server. Para obter mais informações sobre o planejamento de uma instalação do SQL Server segura, consulte [as considerações de segurança para uma instalação do SQL seguro](https://go.microsoft.com/fwlink/?LinkID=139831) (https://go.microsoft.com/fwlink/?LinkID=139831).  
  
-   **Implante o SQL Server sempre atrás de um firewall em um ambiente de rede fisicamente seguro.**  
  
    Uma instalação do SQL Server nunca deve ser exposta diretamente à Internet. Apenas os computadores que estão dentro de seu datacenter devem ser capazes de acessar a sua instalação do SQL server compatíveis com o AD FS. Para obter mais informações, consulte [lista de verificação de práticas recomendadas segurança](https://go.microsoft.com/fwlink/?LinkID=189229) (https://go.microsoft.com/fwlink/?LinkID=189229).  
  
-   **Execute o SQL Server em uma conta de serviço em vez de usar as contas de serviço de sistema padrão interna.**  
  
    Por padrão, o SQL Server geralmente é instalado e configurado para usar uma das contas de sistema interno com suporte, como as contas LocalSystem ou serviço de rede. Para melhorar a segurança da instalação do SQL Server do AD FS, onde possível usar uma conta de serviço separados para acessar seu serviço SQL Server e habilitar a autenticação Kerberos, registrando o segurança principal SPN (nome) dessa conta na implantação do Active Directory. Isso permite a autenticação mútua entre o cliente e servidor. Sem registro SPN de uma conta de serviço separado, SQL Server usará a autenticação baseada em NTLM para Windows, onde somente o cliente seja autenticado.  
  
-   **Minimize a área de superfície do SQL Server.**  
  
    Permitir que apenas esses SQL Server pontos de extremidade que são necessários. Por padrão, o SQL Server fornece uma única empresa TCP interna que não pode ser removida. Do AD FS, você deve habilitar esse ponto de extremidade TCP para autenticação Kerberos. Para examinar os pontos de extremidade TCP atuais para ver se as portas TCP adicionais de definidos pelo usuário são adicionadas a uma instalação do SQL, você pode usar o "Selecionar * de sys.tcp_endpoints" instrução em uma sessão de Transactsql (T-SQL) da consulta. Para obter mais informações sobre a configuração de ponto de extremidade do SQL Server, consulte [How To: configurar o mecanismo de banco de dados para ouvir em várias portas de TCP](https://go.microsoft.com/fwlink/?LinkID=189231) (https://go.microsoft.com/fwlink/?LinkID=189231).  
  
-   **Evite usar a autenticação baseada em SQL.**  
  
    Para evitar ter de transferir senhas como texto não criptografado pela sua rede ou armazenar senhas em definições de configuração, use a autenticação do Windows somente com a instalação do SQL Server. Autenticação do SQL Server é um modo de autenticação herdado. Armazenando credenciais de login da linguagem de consulta estruturada (SQL) (senhas e nomes de usuário SQL) quando você estiver usando autenticação do SQL Server não é recomendada. Para obter mais informações, consulte [modos de autenticação](https://go.microsoft.com/fwlink/?LinkID=189232) (https://go.microsoft.com/fwlink/?LinkID=189232).  
  
-   **Avalie cuidadosamente a necessidade de segurança de canal adicionais na instalação do SQL.**  
  
    Mesmo com autenticação Kerberos na verdade, o SQL Server segurança suporte à Interface do provedor (SSPI) não fornece segurança de nível de canal. No entanto, para instalações em que os servidores com segurança estão localizados em uma rede protegida por firewall, criptografar comunicações SQL talvez não seja necessário.  
  
    Embora a criptografia é uma ferramenta valiosa para ajudar a garantir a segurança, ele não deve ser considerado para todos os dados ou conexões. Ao decidir se é necessário implementar as criptografias, considere como os usuários terão acesso a dados. Se os usuários acessam os dados em uma rede pública, criptografia de dados pode ser solicitada a aumentar a segurança. No entanto, se todo o acesso de dados SQL pelo AD FS envolve uma configuração de segurança da intranet, criptografia não pode ser necessária. Qualquer uso da criptografia também deve incluir uma estratégia de manutenção para senhas, chaves e certificados.  
  
    Se houver uma preocupação que quaisquer dados do SQL podem ser vistos ou violado através de sua rede, usam o protocolo IPSec (IPsec) ou Secure Sockets Layer (SSL) para ajudar a proteger as conexões SQL. No entanto, isso pode ter um efeito negativo no desempenho do SQL Server, que pode afetar ou limitar o desempenho do AD FS em algumas situações. Por exemplo, o desempenho do AD FS na emissão de token pode degradar quando as pesquisas de atributo de um repositório de atributo baseado em SQL são essenciais para a emissão de token. Você pode eliminar melhor um SQL violação ameaça por ter uma configuração de segurança do perímetro forte. Por exemplo, a melhor solução para proteger sua instalação do SQL Server é garantir que ele permanecerá inacessível para os usuários de Internet e computadores e que ele permanece acessível apenas por usuários ou computadores dentro do ambiente de datacenter.  
  
    Para obter mais informações, consulte [criptografando conexões para SQL Server](https://go.microsoft.com/fwlink/?LinkID=189234) ou [criptografia do SQL Server](https://go.microsoft.com/fwlink/?LinkID=189233).  
  
-   **Configure o acesso com segurança projetado usando procedimentos para realizar pesquisas todas baseadas em SQL pelos dados armazenados FS do SQL de anúncios.**  
  
    Para fornecer o melhor serviço e o isolamento de dados, você pode criar procedimentos armazenados para todos os comandos de pesquisa de armazenamento de atributo. Você pode criar uma função de banco de dados ao qual você concede permissão para executar os procedimentos armazenados. Atribua a identidade de serviço do serviço do Windows do AD FS para essa função de banco de dados. O serviço do Windows do AD FS não deve ser capaz de executar qualquer outra instrução SQL, que não sejam os procedimentos apropriados armazenados que são usados para pesquisa de atributo. Bloquear acesso ao banco de dados SQL Server dessa maneira reduz o risco de um ataque de elevação de privilégio.  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
