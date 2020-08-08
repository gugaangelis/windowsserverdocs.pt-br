---
ms.assetid: 963a3d37-d5f1-4153-b8d5-2537038863cb
title: Práticas recomendadas para o planejamento e a implantação seguros do AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: f21eb5737bb1729999ae6d298ca868dc3f7d52d6
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954342"
---
# <a name="best-practices-for-secure-planning-and-deployment-of-ad-fs"></a>Práticas recomendadas para o planejamento e a implantação seguros do AD FS


Este tópico fornece informações de práticas recomendadas para ajudá-lo a planejar e avaliar a segurança ao projetar sua implantação de Serviços de Federação do Active Directory (AD FS) (AD FS). Este tópico é um ponto de partida para revisar e avaliar considerações que afetam a segurança geral de seu uso de AD FS. As informações contidas neste tópico visam complementar e agregar ao seu planejamento de segurança existente e outras práticas recomendadas de design.

## <a name="core-security-best-practices-for-ad-fs"></a>Principais práticas recomendadas de segurança do AD FS
As seguintes práticas recomendadas principais são comuns a todas as instalações de AD FS em que você deseja melhorar ou estender a segurança de seu design ou implantação:

-   **Proteger AD FS como um sistema de "camada 0"**

    O AD FS é, fundamentalmente, um sistema de autenticação.  Portanto, ele deve ser tratado como um sistema de "camada 0" como outro sistema de identidade em sua rede.  [Microsoft docs](../../securing-privileged-access/securing-privileged-access-reference-material.md) tem mais informações sobre o modelo de camada administrativa do Active Directory.


-   **Use o Assistente de Configuração de Segurança para aplicar as práticas recomendadas de segurança específicas do AD FS aos servidores de federação e computadores de proxy do servidor de federação**

    O ACS (Assistente de configuração de segurança) é uma ferramenta que vem pré-instalado em todos os computadores com Windows Server 2008, Windows Server 2008 R2 e Windows Server 2012. Você pode usá-lo para aplicar as práticas recomendadas de segurança que podem ajudar a reduzir a superfície de ataques de um servidor com base nas funções do servidor instalado.

    Ao instalar o AD FS, o programa de configuração cria arquivos de extensão de função que podem ser usados com o SCW para criar uma política de segurança que aplicará a função de servidor específica do AD FS (seja servidor de federação ou proxy do servidor de federação) escolhida durante a instalação.

    Cada arquivo de extensão de função instalado representa o tipo de função e subfunção para as quais o computador foi configurado. Os seguintes arquivos de extensão de função são instalados no diretório C:WindowsADFSScw:

    -   Farm.xml

    -   SQLFarm.xml

    -   StandAlone.xml

    -   Proxy.xml (Este arquivo estará presente somente se você configurou o computador na função de proxy de servidor de federação.)

    Para aplicar as extensões de função do AD FS no SCW, execute as etapas a seguir na ordem indicada:

    1.  Instale o AD FS e escolha a função de servidor apropriada para o computador em questão. Para obter mais informações, consulte [instalar o serviço de função de proxy do serviço de Federação](../../ad-fs/deployment/Install-the-Federation-Service-Proxy-Role-Service.md) no guia de implantação de AD FS.

    2.  Registre o arquivo de extensão de função apropriado usando a ferramenta de linha de comando Scwcmd. Confira a tabela abaixo para ver mais detalhes sobre como usar esta ferramenta na função para qual o computador foi configurado.

    3.  Verifique se o comando foi concluído com êxito examinando o arquivo SCWRegister_log.xml, que está localizado no diretório WindowssecurityMsscwLogs

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
    A detecção de reprodução de token é um recurso de AD FS que garante que qualquer tentativa de reprodução de uma solicitação de token feita ao Serviço de Federação é detectada e a solicitação é descartada. A detecção de reprodução de token é habilitada por padrão. Ela funciona tanto no perfil passivo do Web Services Federation quanto no perfil de WebSSO SAML (Security Assertion Markup Language) ao garantir que o mesmo token nunca seja usado mais de uma vez.

    Quando o Serviço de federação é iniciado, ele começa a criar um cache com quaisquer solicitações de token executadas. Com o passar do tempo, as solicitações de token seguintes são adicionadas ao cache, multiplicando a capacidade de detectar qualquer tentativa de reproduzir uma solicitação de token para o serviço de federação. Se você desabilitar a detecção de reprodução de token e posteriormente decidir reativá-la, lembre-se de que o serviço de federação ainda aceitará tokens pelo período que possa ter sido usado anteriormente, até que o cache de reprodução tenha tido tempo suficiente para reconstruir seu conteúdo. Para saber mais, confira [A função do banco de dados de configuração de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).

-   **Use criptografia de token, especialmente se usar resolução de artefato de SAML de suporte.**

    A criptografia de tokens é altamente recomendável para aumentar a segurança e a proteção contra ataques MITM (Man-in-the-Middle) potenciais que podem ser testados em sua implantação de AD FS. Usar criptografia de uso pode representar um ligeiro impacto no resultado geral, porém este geralmente não é notado em muitas implantações, e os benefícios oferecidos por uma segurança mais elevada ultrapassam e muito qualquer custo em termos de desempenho do sistema.

    Para habilitar a criptografia de token, primeiramente adicione um certificado de criptografia à sua terceira parte confiável. Você pode configurar um certificado de criptografia ao criar um objeto de confiança de terceira parte confiável ou posteriormente. Para adicionar um certificado de criptografia posteriormente a uma relação de confiança de terceira parte confiável existente, você pode definir um certificado para uso na guia **criptografia** em Propriedades de confiança ao usar o snap-in de AD FS. Para especificar um certificado para uma relação de confiança existente usando os cmdlets AD FS, use o parâmetro EncryptionCertificate dos cmdlets **set-ClaimsProviderTrust** ou **set-RelyingPartyTrust** . Para definir um certificado para o Serviço de Federação a ser usado ao descriptografar tokens, use o cmdlet **set-ADFSCertificate** e especifique " `Token-Encryption` " para o parâmetro *certificatetype* . É possível habilitar e desativar a criptografia para um objeto de confiança de terceira parte confiável usando o parâmetro *EncryptClaims* do cmdlet **Set-RelyingPartyTrust**.

-   **Utilize proteção estendida para autenticação**

    Para ajudar a proteger suas implantações, você pode definir e usar o recurso proteção estendida para autenticação com o AD FS. Essa configuração especifica o nível de proteção estendida para autenticação com suporte de um servidor de Federação.

    A proteção estendida para autenticação ajuda a proteger contra ataques intermediários (MITM), nos quais um invasor intercepta as credenciais do cliente e encaminha-a para um servidor. A proteção contra tais ataques é possível por causa de um CBT (Token de Ligação de Canal), que poderá ser pedido, permitido ou não pedido pelo servidor quando este estabelecer comunicações com clientes.

    Para habilitar o recurso de proteção estendida, use o parâmetro **ExtendedProtectionTokenCheck** no cmdlet **Set-ADFSProperties**. Os possíveis valores desta configuração e o nível de segurança fornecido por esses valores são descritos na tabela a seguir.

    |Valor de Parâmetro|Nível de segurança|Configuração de proteção|
    |-------------------|------------------|----------------------|
    |Exigir|O servidor está totalmente protegido.|A proteção estendida é imposta e sempre necessária.|
    |Allow|O servidor está parcialmente protegido.|A proteção estendida é imposta nos casos em que os sistemas envolvidos foram atualizados para dar suporte a ela.|
    |Nenhum|O servidor está vulnerável.|A proteção estendida não é imposta.|

-   **Se você usar registro em log e rastreamento, certifique-se de manter a privacidade de informações confidenciais.**

    O AD FS não, por padrão, expõe ou rastreia informações de identificação pessoal (PII) diretamente como parte das operações de Serviço de Federação ou normal. No entanto, quando o log de eventos e o log de rastreamento de depuração estão habilitados no AD FS, dependendo da política de declarações que você configura alguns tipos de declarações e seus valores associados podem conter PII que podem ser registradas no evento AD FS ou nos logs de rastreamento.

    Portanto, a imposição do controle de acesso na configuração de AD FS e de seus arquivos de log é altamente recomendável. Se você não desejar que esse tipo de informação fique visível, deverá desabilitar o armazenamento de logs ou filtrar quaisquer dados confidenciais ou PII presentes nos logs antes de compartilhá-los.

    As dicas a seguir podem ajudar a evitar que o conteúdo do arquivo de log seja exposto indesejadamente:

    -   Verifique se o AD FS log de eventos e os arquivos de log de rastreamento estão protegidos por ACLs (listas de controle de acesso) que limitam o acesso apenas aos administradores confiáveis que exigem acesso a eles.

    -   Não copie ou arquive logs usando extensões de arquivo ou caminhos que podem ser facilmente expostos com uma solicitação da web. Por exemplo, a extensão de nome de arquivo .xml não é uma opção segura. Você pode conferir o guia de administração de ISS (Serviços de Informações da Internet) para ver uma lista das extensões que podem ser usadas.

    -   Se você revisar o caminho para o arquivo de log, lembre-se de especificar um caminho absoluto para o local do arquivo de log, que deverá estar fora da raiz virtual do host da web (vroot) para evitar que seja acessado por terceiros usando um navegador da web.

-   **AD FS o bloqueio de software de extranet e a proteção de bloqueio inteligente de AD FS extranet**

    No caso de um ataque na forma de solicitações de autenticação com senhas inválidas (ruins) que chegam pelo proxy de aplicativo Web, AD FS o bloqueio de extranet permite que você proteja seus usuários de um bloqueio de conta de AD FS. Além de proteger seus usuários de um bloqueio de conta AD FS, AD FS o bloqueio de extranet também protege contra ataques de adivinhação de senha de força bruta.

    Para o bloqueio virtual de extranet para AD FS no Windows Server 2012 R2, consulte [AD FS proteção de bloqueio flexível de extranet](../../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md).

     Para bloqueio inteligente de extranet para AD FS no Windows Server 2016, consulte [AD FS proteção de bloqueio inteligente de extranet](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md).

## <a name="sql-serverspecific-security-best-practices-for-ad-fs"></a>Práticas recomendadas de segurança do AD FS específicas para SQL Server
As práticas recomendadas de segurança a seguir são específicas do uso de Microsoft SQL Server &reg; ou do banco de dados interno do Windows (wid) quando essas tecnologias de banco de dados são usadas para gerenciar os dados no design e na implantação de AD FS.

> [!NOTE]
> Essas recomendações visam ampliar, e não substituir, o guia de segurança de produto do SQL Server. Para obter mais informações sobre como planejar uma instalação segura do SQL Server, consulte [considerações de segurança para uma instalação segura do SQL](https://go.microsoft.com/fwlink/?LinkID=139831) ( https://go.microsoft.com/fwlink/?LinkID=139831) .

-   **Sempre implante o SQL Server atrás de um firewall em um ambiente de rede fisicamente seguro.**

    Uma instalação do SQL Server nunca deverá estar diretamente exposta à Internet. Somente os computadores que estão dentro de seu datacenter devem ser capazes de acessar a instalação do SQL Server que dá suporte a AD FS. Para obter mais informações, consulte [lista de verificação de práticas recomendadas de segurança](https://go.microsoft.com/fwlink/?LinkID=189229) ( https://go.microsoft.com/fwlink/?LinkID=189229) .

-   **Execute o SQL Server em uma conta de serviço em vez de usar contas de serviço padrão integradas do sistema.**

    Por padrão, o SQL Server geralmente é instalado e configurado para usar uma das contas integradas do sistema com suporte, tal como as contas LocalSystem ou NetworkService. Para aprimorar a segurança de sua instalação do SQL Server para AD FS, sempre que possível, use uma conta de serviço separada para acessar seu serviço de SQL Server e habilitar a autenticação Kerberos registrando o SPN (nome da entidade de segurança) dessa conta em sua implantação do Active Directory. Isso habilita a autenticação mútua entre o cliente e o servidor. Sem registrar o SPN de uma conta de serviço separada, o SQL Server usará NTLM para autenticação do Windows, na qual somente o cliente é autenticado.

-   **Minimize a área da superfície do SQL Server.**

    Habilite somente os terminais do SQL Server necessários. Por padrão, o SQL Server fornece um único ponto de extremidade TCP integrado que não pode ser removido. Por AD FS, você deve habilitar esse ponto de extremidade TCP para autenticação Kerberos. Para analisar os terminais de TCP atuais para ver se portas TCP definidas pelo usuário adicionais foram adicionados a uma instalação do SQL, use a instrução de consulta "SELECT * FROM sys.tcp_endpoints" em uma sessão Transact-SQL (T-SQL). Para obter mais informações sobre SQL Server configuração de ponto de extremidade, consulte [como configurar o mecanismo de banco de dados para escutar em várias portas TCP](https://go.microsoft.com/fwlink/?LinkID=189231) ( https://go.microsoft.com/fwlink/?LinkID=189231) .

-   **Evite usar autenticação baseada no SQL.**

    Para evitar transferir senhas como texto simples pela rede ou armazenar senhas nas definições de configuração, use a autenticação do Windows somente com sua instalação do SQL Server. A autenticação do SQL Server é um modo de autenticação legado. Não é recomendado armazenar credenciais de logon em linguagem SQL (ou seja, nomes de usuário e senhas do SQL) ao usar a autenticação do SQL Server. Para obter mais informações, consulte [modos de autenticação](https://go.microsoft.com/fwlink/?LinkID=189232) ( https://go.microsoft.com/fwlink/?LinkID=189232) .

-   **Avalie com cautela a necessidade de usar segurança de canal adicional na instalação do SQL.**

    Mesmo com a autenticação do Kerberos em vigor, o SSPI (interface SSPI) do SQL Server não oferece segurança no nível do canal. Contudo, para instalações nas quais o servidor encontra-se em uma rede protegida com segurança por firewall, pode não ser necessário criptografar as comunicações do SQL.

    Embora a criptografia seja uma ferramenta valiosa para ajudar a garantir a segurança, não deve ser considerada em todos os dados ou conexões. Quando você estiver decidindo se a criptografia deve ser implementada, considere como os usuários acessarão os dados. Se os usuários acessarem dados por uma rede pública, a criptografia de dados poderá ser necessária para aumentar a segurança. No entanto, se todo o acesso de dados SQL por AD FS envolver uma configuração de intranet segura, a criptografia poderá não ser necessária. Qualquer uso de criptografia deve também incluir uma estratégia de manutenção de senhas, chaves e certificados.

    Se houver alguma preocupação que os dados do SQL podem ser vistos ou violados na sua rede, use o IPsec ou o SSL para ajudar a proteger as conexões do SQL. No entanto, isso pode ter um efeito negativo no desempenho do SQL Server, o que pode afetar ou limitar o desempenho AD FS em algumas situações. Por exemplo, AD FS desempenho na emissão de token pode diminuir quando as pesquisas de atributo de um repositório de atributos baseado em SQL são essenciais para a emissão de tokens. Você pode eliminar a ameaça de violação do SQL com mais eficiência se possuir uma forte configuração de perímetro de segurança. Por exemplo, uma solução melhor para proteger sua instalação do SQL Server seria garantir que esta esteja inacessível a usuários e computadores da Internet, e que esteja acessível somente a usuários ou computadores no ambiente do seu datacenter.

    Para obter mais informações, consulte [Criptografando conexões com SQL Server](https://go.microsoft.com/fwlink/?LinkID=189234) ou [SQL Server criptografia](https://go.microsoft.com/fwlink/?LinkID=189233).

-   **Configure o acesso criado com segurança usando procedimentos armazenados para executar todas as pesquisas baseadas em SQL por AD FS de dados armazenados em SQL.**

    Para fornecer um melhor serviço e isolamento dos dados, você pode criar procedimentos armazenados para todos os comandos de pesquisa de armazenamento de atributo. Você pode criar uma função de banco de dados que receberá permissão para executar os procedimentos armazenados. Atribua a identidade de serviço do AD FS serviço do Windows a essa função de banco de dados. O serviço do Windows AD FS não deve ser capaz de executar nenhuma outra instrução SQL, além dos procedimentos armazenados apropriados que são usados para pesquisa de atributo. Bloquear o acesso ao banco de dados do SQL Server desta maneira reduz o risco de um ataque de elevação de privilégio.

## <a name="see-also"></a>Consulte Também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
