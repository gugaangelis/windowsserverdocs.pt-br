---
ms.assetid: 963a3d37-d5f1-4153-b8d5-2537038863cb
title: Práticas recomendadas para o planejamento e a implantação seguros do AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 95f9fd468df39525a2fe7d18647f399214486bbb
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280594"
---
# <a name="best-practices-for-secure-planning-and-deployment-of-ad-fs"></a>Práticas recomendadas para o planejamento e a implantação seguros do AD FS


Este tópico fornece informações de práticas recomendadas para ajudá-lo a planejar e avaliar a segurança ao projetar sua implantação de serviços de Federação do Active Directory (AD FS). Este tópico é um ponto de partida para revisar e avaliar considerações que afetam a segurança geral de uso do AD FS. As informações contidas neste tópico visam complementar e agregar ao seu planejamento de segurança existente e outras práticas recomendadas de design.  
  
## <a name="core-security-best-practices-for-ad-fs"></a>Principais práticas recomendadas de segurança do AD FS  
As seguintes práticas recomendadas de núcleo são comuns a todas as instalações do AD FS em que você deseja aprimorar ou estender a segurança do design ou implantação:  

-   **Proteger o AD FS como um sistema de "Nível 0"** 

    O AD FS é, basicamente, um sistema de autenticação.  Portanto, ele deve ser tratado como um sistema de "Nível 0" como outro sistema de identidade em sua rede.  [Microsoft Docs](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material) tem mais informações sobre o modelo de camadas administrativas do Active Directory. 


-   **Use o Assistente de configuração de segurança para aplicar práticas recomendadas de segurança específicas do AD FS aos servidores de Federação e computadores de proxy do servidor de Federação**  
  
    O Assistente de configuração de segurança (ACS) é uma ferramenta fornecida pré-instalada em todos os Windows Server 2008, Windows Server 2008 R2 e computadores do Windows Server 2012. Você pode usá-lo para aplicar as práticas recomendadas de segurança que podem ajudar a reduzir a superfície de ataques de um servidor com base nas funções do servidor instalado.  
  
    Ao instalar o AD FS, o programa de configuração cria arquivos de extensão de função que podem ser usados com o SCW para criar uma política de segurança que aplicará a função de servidor específica do AD FS (seja servidor de federação ou proxy do servidor de federação) escolhida durante a instalação.  
  
    Cada arquivo de extensão de função instalado representa o tipo de função e subfunção para as quais o computador foi configurado. Os seguintes arquivos de extensão de função são instalados no diretório C:WindowsADFSScw:  
  
    -   Farm.xml  
  
    -   SQLFarm.xml  
  
    -   StandAlone.xml  
  
    -   Proxy.xml (Este arquivo estará presente somente se você configurou o computador na função de proxy de servidor de federação.)  
  
    Para aplicar as extensões de função do AD FS no SCW, execute as etapas a seguir na ordem indicada:  
  
    1.  Instale o AD FS e escolha a função de servidor apropriada para o computador em questão. Para obter mais informações, consulte [instalar o serviço de função Proxy de serviço de Federação](../../ad-fs/deployment/Install-the-Federation-Service-Proxy-Role-Service.md) no guia de implantação do AD FS.  
  
    2.  Registre o arquivo de extensão de função apropriado usando a ferramenta de linha de comando Scwcmd. Confira a tabela abaixo para ver mais detalhes sobre como usar esta ferramenta na função para qual o computador foi configurado.  
  
    3.  Verifique se que o comando foi concluído com êxito examinando o arquivo Scwregister_log XML, que está localizado no diretório WindowssecurityMsscwLogs.  
  
    Você deve executar todas essas etapas em cada servidor de federação ou computador de proxy do servidor de federação aos quais deseja aplicar políticas de segurança do AD FS com o SCW.  
  
    A tabela a seguir explica como registrar a extensão de função de SCW apropriada com base na função do servidor AD FS escolhida no computador onde o AD FS foi instalado.  
  
    |Função do servidor AD FS|Banco de dados de configuração do AD FS usado|Digite o comando a seguir em um prompt de comando:|  
    |---------------------|-------------------------------------|---------------------------------------------------|  
    |Servidor de federação autônomo|Banco de Dados Interno do Windows|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwStandAlone.xml"`|  
    |Servidor de federação ingressado em farm|Banco de Dados Interno do Windows|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwFarm.xml"`|  
    |Servidor de federação ingressado em farm|SQL Server|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwSQLFarm.xml"`|  
    |Proxy do servidor de federação|N/D|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwProxy.xml"`|  
  
    Para saber mais sobre os bancos de dados que podem ser usados com o AD FS, confira [A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
-   **Use a detecção de reprodução de token em situações em que a segurança é uma questão muito importante, por exemplo, ao usar quiosques.**  
    Detecção de reprodução de token é um recurso do AD FS que garante que qualquer tentativa de reproduzir uma solicitação de token é feita ao serviço de Federação é detectada e a solicitação é descartada. A detecção de reprodução de token é habilitada por padrão. Ela funciona tanto no perfil passivo do Web Services Federation quanto no perfil de WebSSO SAML (Security Assertion Markup Language) ao garantir que o mesmo token nunca seja usado mais de uma vez.  
  
    Quando o Serviço de federação é iniciado, ele começa a criar um cache com quaisquer solicitações de token executadas. Com o passar do tempo, as solicitações de token seguintes são adicionadas ao cache, multiplicando a capacidade de detectar qualquer tentativa de reproduzir uma solicitação de token para o serviço de federação. Se você desabilitar a detecção de reprodução de token e posteriormente decidir reativá-la, lembre-se de que o serviço de federação ainda aceitará tokens pelo período que possa ter sido usado anteriormente, até que o cache de reprodução tenha tido tempo suficiente para reconstruir seu conteúdo. Para saber mais, confira [A função do banco de dados de configuração de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
-   **Use a criptografia de token, especialmente se você estiver usando o suporte resolução do artefato SAML.**  
  
    Criptografia de tokens é altamente recomendada para aumentar a segurança e proteção contra ataques man-in-the-middle (MITM) potenciais podem ameaçar sua implantação do AD FS. Usar criptografia de uso pode representar um ligeiro impacto no resultado geral, porém este geralmente não é notado em muitas implantações, e os benefícios oferecidos por uma segurança mais elevada ultrapassam e muito qualquer custo em termos de desempenho do sistema.  
  
    Para habilitar a criptografia de token, primeiramente adicione um certificado de criptografia à sua terceira parte confiável. Você pode configurar um certificado de criptografia ao criar um objeto de confiança de terceira parte confiável ou posteriormente. Para adicionar um certificado de criptografia posteriormente a um existente terceira parte confiável, você pode definir um certificado para uso na **criptografia** guia nas propriedades de relação de confiança usando o snap-in do AD FS. Para especificar um certificado para uma relação de confiança existente usando os cmdlets do AD FS, use o parâmetro EncryptionCertificate dos **Set-ClaimsProviderTrust** ou **Set-RelyingPartyTrust** cmdlets. Para definir um certificado para o serviço de Federação a ser usado ao descriptografar tokens, use o **Set-ADFSCertificate** cmdlet e especifique "`Token-Encryption`" para o *CertificateType* parâmetro. É possível habilitar e desativar a criptografia para um objeto de confiança de terceira parte confiável usando o parâmetro *EncryptClaims* do cmdlet **Set-RelyingPartyTrust** .  
  
-   **Utilizar proteção estendida para autenticação**  
  
    Para ajudar a proteger suas implantações, você pode definir e usar a proteção estendida para o recurso de autenticação com o AD FS. Essa configuração especifica o nível de proteção estendida para autenticação com suporte por um servidor de Federação.  
  
    A proteção estendida para autenticação ajuda a proteger contra ataques intermediários (MITM), nos quais um invasor intercepta as credenciais do cliente e encaminha-a para um servidor. A proteção contra tais ataques é possível por causa de um CBT (Token de Ligação de Canal), que poderá ser pedido, permitido ou não pedido pelo servidor quando este estabelecer comunicações com clientes.  
  
    Para habilitar o recurso de proteção estendida, use o parâmetro **ExtendedProtectionTokenCheck** no cmdlet **Set-ADFSProperties**. Os possíveis valores desta configuração e o nível de segurança fornecido por esses valores são descritos na tabela a seguir.  
  
    |Valor do parâmetro|Nível de segurança|Configuração de proteção|  
    |-------------------|------------------|----------------------|  
    |Requerer|O servidor está totalmente protegido.|A proteção estendida é imposta e sempre necessária.|  
    |Permitir|O servidor está parcialmente protegido.|A proteção estendida é imposta nos casos em que os sistemas envolvidos foram atualizados para dar suporte a ela.|  
    |Nenhuma|O servidor está vulnerável.|A proteção estendida não é imposta.|  
  
-   **Se você estiver usando o registro em log e rastreamento, garantir a privacidade de informações confidenciais.**  
  
    O AD FS não, por padrão, expor ou rastrear informações de identificação pessoal (PII) diretamente como parte do serviço de Federação ou operações normais. Quando o log de eventos e logs de rastreamento de depuração estão habilitados no AD FS, no entanto, dependendo da política de declarações que você configure algumas declarações tipos e seus valores associados podem conter PII que pode ser registrada no evento do AD FS ou logs de rastreamento.  
  
    Portanto, impor o controle de acesso na configuração do AD FS e seus arquivos de log é altamente recomendável. Se você não desejar que esse tipo de informação fique visível, deverá desabilitar o armazenamento de logs ou filtrar quaisquer dados confidenciais ou PII presentes nos logs antes de compartilhá-los.  
  
    As dicas a seguir podem ajudar a evitar que o conteúdo do arquivo de log seja exposto indesejadamente:  
  
    -   Certifique-se de que o log de eventos do AD FS e os arquivos de log de rastreamento são protegidos por listas de controle de acesso (ACL) que limitam o acesso a apenas os administradores confiáveis que necessitam acessá-los.  
  
    -   Não copie ou arquive logs usando extensões de arquivo ou caminhos que podem ser facilmente expostos com uma solicitação da web. Por exemplo, a extensão de nome de arquivo .xml não é uma opção segura. Você pode conferir o guia de administração de ISS (Serviços de Informações da Internet) para ver uma lista das extensões que podem ser usadas.  
  
    -   Se você revisar o caminho para o arquivo de log, lembre-se de especificar um caminho absoluto para o local do arquivo de log, que deverá estar fora da raiz virtual do host da web (vroot) para evitar que seja acessado por terceiros usando um navegador da web.  

-   **AD FS Extranet Lockout flexível e o AD FS Extranet inteligente de proteção de bloqueio**  
    
    No caso de um ataque na forma de solicitações de autenticação com invalid(bad) senhas que são fornecidos por meio do Proxy de aplicativo Web, o bloqueio de extranet do AD FS permite que você proteger os usuários contra um bloqueio de conta do AD FS. Além de proteger os usuários de um AD FS de bloqueio de conta, o bloqueio de extranet do AD FS também protege contra ataques de adivinhação de senha de força bruta.  
    
    Para bloqueio flexível de Extranet do AD FS no Windows Server 2012 R2, consulte [AD FS reversível proteção de bloqueio Extranet](../../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md).  

     Para bloqueio inteligente de Extranet do AD FS no Windows Server 2016, consulte [AD FS inteligente proteção de bloqueio Extranet](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md).  
  
## <a name="sql-serverspecific-security-best-practices-for-ad-fs"></a>Práticas recomendadas de segurança do AD FS específicas para SQL Server  
As seguintes práticas recomendadas de segurança são específicas para o uso de Microsoft SQL Server® ou banco de dados interno do Windows (WID) quando essas tecnologias de banco de dados são usadas para gerenciar dados no design do AD FS e a implantação.  
  
> [!NOTE]  
> Essas recomendações visam ampliar, e não substituir, o guia de segurança de produto do SQL Server. Para obter mais informações sobre como planejar uma instalação segura do SQL Server, consulte [considerações sobre segurança para uma instalação segura do SQL](https://go.microsoft.com/fwlink/?LinkID=139831) (https://go.microsoft.com/fwlink/?LinkID=139831).  
  
-   **Implante o SQL Server sempre atrás de um firewall em um ambiente de rede fisicamente seguro.**  
  
    Uma instalação do SQL Server nunca deverá estar diretamente exposta à Internet. Somente os computadores que estão dentro de seu data center devem ser capazes de acessar sua instalação do SQL server que dá suporte ao AD FS. Para obter mais informações, consulte [lista de verificação de práticas recomendadas Security](https://go.microsoft.com/fwlink/?LinkID=189229) (https://go.microsoft.com/fwlink/?LinkID=189229).  
  
-   **Execute o SQL Server em uma conta de serviço em vez de usar as contas de serviço de sistema padrão interna.**  
  
    Por padrão, o SQL Server geralmente é instalado e configurado para usar uma das contas integradas do sistema com suporte, tal como as contas LocalSystem ou NetworkService. Para aprimorar a segurança da instalação do SQL Server para o AD FS, sempre que possível, use um separado conta de serviço para acessar o serviço do SQL Server e habilite a autenticação Kerberos registrando o nome de entidade de segurança (SPN) dessa conta em seu Implantação do Active Directory. Isso habilita a autenticação mútua entre o cliente e o servidor. Sem registrar o SPN de uma conta de serviço separada, o SQL Server usará NTLM para autenticação do Windows, na qual somente o cliente é autenticado.  
  
-   **Minimize a área de superfície do SQL Server.**  
  
    Habilite somente os terminais do SQL Server necessários. Por padrão, o SQL Server fornece um único ponto de extremidade TCP integrado que não pode ser removido. Para o AD FS, você deve habilitar o ponto de extremidade TCP para a autenticação Kerberos. Para analisar os terminais de TCP atuais para ver se portas TCP definidas pelo usuário adicionais foram adicionados a uma instalação do SQL, use a instrução de consulta "SELECT * FROM sys.tcp_endpoints" em uma sessão Transact-SQL (T-SQL). Para obter mais informações sobre a configuração de ponto de extremidade do SQL Server, consulte [How To: Configurar o mecanismo de banco de dados para escutar em várias portas TCP](https://go.microsoft.com/fwlink/?LinkID=189231) (https://go.microsoft.com/fwlink/?LinkID=189231).  
  
-   **Evite usar a autenticação baseada em SQL.**  
  
    Para evitar transferir senhas como texto simples pela rede ou armazenar senhas nas definições de configuração, use a autenticação do Windows somente com sua instalação do SQL Server. A autenticação do SQL Server é um modo de autenticação legado. Não é recomendado armazenar credenciais de logon em linguagem SQL (ou seja, nomes de usuário e senhas do SQL) ao usar a autenticação do SQL Server. Para obter mais informações, consulte [modos de autenticação](https://go.microsoft.com/fwlink/?LinkID=189232) (https://go.microsoft.com/fwlink/?LinkID=189232).  
  
-   **Avalie a necessidade de segurança de canal adicional na instalação do SQL com cuidado.**  
  
    Mesmo com a autenticação do Kerberos em vigor, o SSPI (interface SSPI) do SQL Server não oferece segurança no nível do canal. Contudo, para instalações nas quais o servidor encontra-se em uma rede protegida com segurança por firewall, pode não ser necessário criptografar as comunicações do SQL.  
  
    Apesar de a criptografia ser uma ferramenta valiosa para ajudar a garantir a segurança, ela não deve ser considerada para todos os dados ou conexões. Quando você está decidindo se deve implementar criptografia, considere quantos usuários acessarão os dados. Se os usuários acessarem os dados de uma rede pública, a criptografia dos dados pode ser necessária para melhorar a segurança. No entanto, se todo o acesso de dados SQL pelo AD FS envolve uma configuração de intranet segura, a criptografia pode não ser necessária. Qualquer uso de criptografia também deve incluir uma estratégia de manutenção para senhas, chaves e certificados.  
  
    Se houver alguma preocupação que os dados do SQL podem ser vistos ou violados na sua rede, use o IPsec ou o SSL para ajudar a proteger as conexões do SQL. No entanto, isso pode ter um efeito negativo no desempenho do SQL Server, que pode afetar ou limitar o desempenho do AD FS em algumas situações. Por exemplo, o desempenho do AD FS na emissão de tokens pode decair quando pesquisas de atributos de um repositório de atributos baseados em SQL forem cruciais para emissão de token. Você pode eliminar a ameaça de violação do SQL com mais eficiência se possuir uma forte configuração de perímetro de segurança. Por exemplo, uma solução melhor para proteger sua instalação do SQL Server seria garantir que esta esteja inacessível a usuários e computadores da Internet, e que esteja acessível somente a usuários ou computadores no ambiente do seu datacenter.  
  
    Para obter mais informações, consulte [criptografando conexões com o SQL Server](https://go.microsoft.com/fwlink/?LinkID=189234) ou [criptografia do SQL Server](https://go.microsoft.com/fwlink/?LinkID=189233).  
  
-   **Configure o acesso designado com segurança usando procedimentos armazenados para realizar pesquisas de todos os SQL-base por dados do AD FS do SQL-armazenados.**  
  
    Para fornecer um melhor serviço e isolamento dos dados, você pode criar procedimentos armazenados para todos os comandos de pesquisa de armazenamento de atributo. Você pode criar uma função de banco de dados que receberá permissão para executar os procedimentos armazenados. Atribua a identidade de serviço do serviço Windows do AD FS para essa função de banco de dados. O serviço Windows do AD FS não deve ser capaz de executar qualquer outra instrução SQL além dos procedimentos devidamente armazenados usados para pesquisa de atributos. Bloquear o acesso ao banco de dados do SQL Server desta maneira reduz o risco de um ataque de elevação de privilégio.  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
