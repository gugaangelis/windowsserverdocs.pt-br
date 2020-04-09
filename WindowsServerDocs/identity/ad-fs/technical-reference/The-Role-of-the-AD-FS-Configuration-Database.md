---
ms.assetid: 68db7f26-d6e3-4e67-859b-80f352e6ab6a
title: A função do banco de dados de configuração do AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9ffdd1876e2dfbc044cebb65d7d6ef80880a64b8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860159"
---
# <a name="the-role-of-the-ad-fs-configuration-database"></a>A função do banco de dados de configuração do AD FS
O banco de dados de configuração do AD FS armazena todos as configurações que representam uma única instância do Serviços de Federação do Active Directory (AD FS) \(AD FS\) \(ou seja, serviço de Federação\). O banco de dados de configuração do AD FS define o conjunto de parâmetros que um Serviço de Federação requer para identificar parceiros, certificados, repositórios de atributo, declarações e diversos dados sobre essas entidades associadas. Você pode armazenar esses dados de configuração em um Microsoft SQL Server banco de&reg; ou no banco de dados interno do Windows \(recurso de\) do WID que está incluído no Windows Server&reg; 2008, no Windows Server 2008 R2 e no Windows Server&reg; 2012.  
  
> [!NOTE]  
> Todo o conteúdo do banco de dados de configuração do AD FS pode ser armazenado em uma instância do WID ou do banco de dados do SQL, mas não em ambas. Isso significa que não é possível ter alguns servidores de federação usando o WID e outros usando um banco de dados do SQL Server para a mesma instância do banco de dados de configuração do AD FS.  
  
Use as informações fornecidas mais adiante neste tópico juntamente com o conteúdo oferecido em [Considerações de topologia de implantação do AD FS](https://technet.microsoft.com/library/gg982489.aspx) para saber mais sobre as vantagens e desvantagens de escolher o WID ou o SQL Server para armazenar o banco de dados de configuração do AD FS:  
  
O WID usa um repositório de dados relacional e não tem sua própria interface do usuário de gerenciamento \(\)da interface Em vez disso, os administradores podem modificar o conteúdo do banco de dados de configuração do AD FS usando o snap\-de gerenciamento de AD FS no, fsconfig. exe ou cmdlets&trade; do Windows PowerShell.  
  
## <a name="using-wid-to-store-the-ad-fs-configuration-database"></a>Usando o WID para armazenar o banco de dados de configuração do AD FS  
Você pode criar o banco de dados de configuração do AD FS usando o WID como o armazenamento usando o comando fsconfig. exe\-ferramenta de linha ou o assistente de configuração do servidor de Federação AD FS. Ao usar uma dessas ferramentas, escolha uma das opções a seguir para criar sua topologia do servidor de federação. Todas essas opções usam o WID para armazenar o banco de dados de configuração do AD FS:  
  
-   Criar um servidor de Federação autônomo\-  
  
-   Criar o primeiro servidor de federação em um farm de servidores de federação  
  
-   Adicionar um servidor de federação a um farm de servidores de federação  
  
Se você selecionar a opção\-autônoma, o WID será usado para armazenar uma única instância do banco de dados de configuração do AD FS. Esta instância não poderá ser compartilhada entre diversos servidores de federação. Ele destina-se somente a ambientes de laboratório de teste. Para obter mais informações sobre a opção de servidor de Federação autônomo\-ou como configurar um, consulte [servidor de Federação autônomo usando wid](https://technet.microsoft.com/library/gg982486.aspx) ou [criar um servidor de Federação](https://technet.microsoft.com/library/ee913579.aspx)autônomo.  
  
Se você selecionar a opção de primeiro servidor de federação em um farm de servidores de federação, o WID será configurado visando a escalabilidade para permitir que servidores de federação adicionais sejam adicionados ao farm posteriormente. Para ver mais informações sobre como implantar um farm no WID ou como configurá-lo, consulte [Farm de servidores de federação usando o WID](https://technet.microsoft.com/library/gg982492.aspx) ou [Criar o primeiro servidor de federação em um farm de servidores de federação](https://technet.microsoft.com/library/dd807070.aspx)  
  
Se você selecionar a opção de adicionar um servidor de federação, o WID será configurado para replicar as alterações do banco de dados de configuração realizadas no novo servidor de federação em intervalos definidos. Para mais informações sobre como adicionar um servidor de federação a um farm do WID, consulte [Farm de servidores de federação usando o WID](https://technet.microsoft.com/library/gg982492.aspx) ou [Adicionar um servidor de federação a um farm de servidores de federação](https://technet.microsoft.com/library/ee913575.aspx).  
  
> [!NOTE]  
> Quando você implanta um farm de servidores de Federação usando o WID, alguns recursos do AD FS podem não estar disponíveis. Para ter acesso ao conjunto completo de recursos ao configurar seu farm de servidores, considere usar o Microsoft SQL Server para armazenar o banco de dados de configuração do AD FS. Para mais informações, consulte [Considerações de topologia de implantação do AD FS](https://technet.microsoft.com/library/gg982489(v=ws.11).aspx).  
  
### <a name="how-a-wid-federation-server-farm-works"></a>Como funciona um farm de servidores de federação do WID  
Esta seção descreve os conceitos mais importantes que mostram como o farm de servidores de federação do WID replica os dados entre os servidores de federação primários e secundários. .  
  
#### <a name="primary-federation-server"></a>Servidor de federação primário  
Um servidor de Federação primário é um computador executando o Windows Server 2008, o Windows Server 2008 R2 ou o Windows Server&reg; 2012 que foi configurado na função de servidor de Federação com o assistente de configuração do servidor de Federação AD FS e que tem uma cópia de leitura/gravação do banco de dados de configuração do AD FS. O servidor de Federação primário sempre é criado quando você usa o assistente de configuração do servidor de Federação AD FS e seleciona a opção para criar um novo Serviço de Federação e tornar esse computador o primeiro servidor de Federação no farm. Todos os demais servidores de federação deste farm, também chamados de servidores de federação secundários, deverão sincronizar as alterações realizadas no servidor primário para uma cópia do banco de dados de configuração do AD FS armazenado localmente.  
  
#### <a name="secondary-federation-servers"></a>Servidores de federação secundários  
Os servidores de Federação secundários armazenam uma cópia do banco de dados de configuração do AD FS do servidor de Federação primário, mas essas cópias são somente leitura\-. Os servidores de federação secundários conectam-se e sincronizam os dados com o servidor de federação primário no farm sondando-o em intervalos regulares para verificar se os dados foram alterados. Os servidores de Federação secundários existem para fornecer tolerância a falhas para o servidor de Federação primário enquanto atuam para carregar\-balancear as solicitações de acesso feitas em diferentes sites em todo o ambiente de rede.  
  
> [!NOTE]  
> Se o servidor de federação primário ficar inativo e offline, todos os servidores de federação secundários continuarão a processar as solicitações normalmente. Contudo, nenhuma alteração poderá ser realizada no Serviço de Federação até que o servidor primário seja restaurado. Você também pode nominar um servidor de federação secundário para tornar-se o primário usando o Windows PowerShell. Para obter mais informações, consulte a [Administração do AD FS com o Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
#### <a name="how-the-adfs-configuration-database-is-synchronized"></a>Como o banco de dados de configuração do AD FS é sincronizado?  
Devido à função importante que o banco de dados de configuração do AD FS desempenha, ele é disponibilizado em todos os servidores de Federação na rede para fornecer tolerância a falhas e carregar\-capacidades de balanceamento durante o processamento de solicitações \(quando os balanceadores de\-carga de rede são usados\). Contudo, para que os servidores de federação secundária possuam esta capacidade, o banco de dados de configuração do AD FS armazenado no servidor de federação primário deve ser sincronizado.  
  
Ao adicionar um servidor de federação ao farm, o novo computador que será um servidor de federação secundário conecta-se ao servidor de federação primário para replicar a cópia do banco de dados de configuração do AD FS. A partir deste ponto, o novo servidor de federação continua a obter atualizações do servidor de federação primário regularmente, como mostrado na ilustração a seguir.  
  
![AD FS funções](media/adfs2_WID.png)  
  
Cada servidor de federação secundário sonda o primário a cada cinco minutos em busca de alterações. Você pode ajustar esse valor padrão de cinco\-minutos ou forçar uma sincronização imediata a qualquer momento usando um cmdlet do Windows PowerShell. Para obter mais informações sobre como fazer isso, consulte [AD FS administração com o Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
O processo de sincronização do WID também dá suporte a transferências incrementais para oferecer transferências mais eficientes de alterações intermediárias. O processo de transferência incremental exige um tráfego significativamente menor em uma rede e as transferências são concluídas muito mais rápido.  
  
> [!NOTE]  
> Há suporte para a migração de um banco de dados de configuração do AD FS do WID para uma instância do SQL Server. Para obter mais informações sobre como fazer isso, consulte [AD FS: migrar seu banco de dados de configuração de AD FS para SQL Server](https://go.microsoft.com/fwlink/?LinkId=192232) no site do TechNet wiki.  
  
## <a name="using-sql-server-to-store-the-ad-fs-configuration-database"></a>Usando o SQL Server para armazenar o banco de dados de configuração do AD FS  
Você pode criar o banco de dados de configuração do AD FS usando uma única instância de banco de dados SQL Server como a loja usando o comando fsconfig. exe\-ferramenta de linha. Usar um banco de dados do SQL Server como banco de dados de configuração do AD FS oferece as seguintes vantagens em comparação com o WID:  
  
-   Os administradores podem aproveitar os recursos de alta disponibilidade do SQL Server  
  
-   Ele fornece aumentos de desempenho adicionais para alto nível de tráfego.  
  
-   Ele fornece suporte a recursos de resolução de artefato SAML e detecção de reprodução de token de Federação SAML/WS\-\(descritas abaixo\).  
  
O termo "servidor de Federação primário" não se aplica quando o banco de dados de configuração do AD FS é armazenado em uma instância do banco de dados SQL porque todos os servidores de Federação podem ler e gravar igualmente no banco de dados de configuração do AD FS que está usando a mesma instância de SQL Server clusterizado, conforme mostrado na ilustração a seguir.  
  
![AD FS funções](media/adfs2_SQL.png)  
  
Você pode usar SQL Server para configurar dois ou mais servidores para que funcionem juntos como um cluster de servidor para garantir que AD FS se tornará altamente disponível para atender às solicitações de entrada do cliente. A alta disponibilidade fornece uma arquitetura de\-de expansão na qual você pode aumentar a capacidade do servidor adicionando mais servidores. Pontos de falha únicos são mitigados pelo failover de cluster automático.  
  
Você pode obter alta disponibilidade usando os serviços de balanceamento de carga de rede\-e failover que as tecnologias de cluster de SQL fornecem. Para obter mais informações sobre como configurar SQL Server para alta disponibilidade, consulte [visão geral das soluções de alta disponibilidade](https://go.microsoft.com/fwlink/?LinkId=179853).  
  
### <a name="saml-artifact-resolution"></a>Resolução do artefato SAML  
Security Assertion Markup Language \(a resolução de artefato\) SAML é um ponto de extremidade baseado na parte do protocolo SAML 2,0 que descreve como uma terceira parte confiável pode recuperar um token diretamente de um provedor de declarações. No primeiro estágio do processo de resolução, um cliente de navegador contata um servidor de federação do recurso e fornece um artefato a ele. No segundo estágio, os servidores de federação de recurso enviam o artefato para uma URL de terminal de artefato SAML hospedada em alguma organização do parceiro de conta para resolver a mensagem de artefato. No estágio final, o servidor de federação de conta emite o token para o servidor de federação em nome do navegador cliente.  
  
> [!NOTE]  
> Se você for um administrador em uma organização de parceiro de conta, certifique-se de atribuir ou associar um certificado SSL, que se encadeia a um certificado raiz de um membro do programa de certificado raiz do Windows, ao site passivo da Federação no IIS \(<ComputerName>sites \\\\site padrão\\ADFS\\ls\) em todos os servidores de Federação de conta no farm. Isso é importante para evitar que os servidores de federação de recurso tenham que adicionar manualmente o certificado de SSL ao repositório de certificados de Computadores locais de pessoas confiáveis ou evitar que não seja possível resolver o artefato publicado na sua organização.  
  
### <a name="samlws---federation-token-replay-detection"></a>Detecção de reprodução de token SAML/WS-Federation  
O termo *reprodução de token* refere-se ao ato pelo qual um cliente navegador em uma organização do parceiro de conta tenta enviar o mesmo token recebido de um servidor de federação de conta múltiplas vezes para autenticar-se a um servidor de federação de recurso.  Esse ato ocorre quando um usuário clica no botão **voltar** de seu navegador em um esforço para reenviar a página de autenticação.  
  
O AD FS oferece um recurso chamado de *detecção de reprodução de token* com o qual solicitações múltiplas usando o mesmo token são detectadas e descartadas. Quando esse recurso é habilitado, a detecção de reprodução de token protege a integridade das solicitações de autenticação no perfil passivo de Federação do WS\-e no perfil WebSSO SAML, garantindo que o mesmo token nunca seja usado mais de uma vez. Este recurso deve ser habilitado em situações nas quais a segurança é uma grande preocupação, como ao usar quiosques.  
  
No exemplo do quiosque, um usuário pode sair de todos os sites da web e depois um usuário mal-intencionado pode tentar usar o histórico de navegação para reenviar a página de autenticação federada carregada pelo usuário anterior. Esse recurso atenua essa preocupação ao armazenar informações adicionais sobre cada autenticação bem-sucedida feita por uma organização parceira de conta para detectar as repetições subsequentes do token e evitar que várias tentativas de autenticação sejam bem-sucedidas.  
  

