---
ms.assetid: 68db7f26-d6e3-4e67-859b-80f352e6ab6a
title: A função do banco de dados de configuração do AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 40f1f4952730fad0749a173fdc968714d043b1c1
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188511"
---
# <a name="the-role-of-the-ad-fs-configuration-database"></a>A função do banco de dados de configuração do AD FS
O banco de dados de configuração do AD FS armazena todos os dados de configuração que representa uma única instância dos serviços de Federação do Active Directory \(do AD FS\) \(, ou seja, o serviço de Federação\). O banco de dados de configuração do AD FS define o conjunto de parâmetros que um Serviço de Federação requer para identificar parceiros, certificados, repositórios de atributo, declarações e diversos dados sobre essas entidades associadas. Você pode armazenar esses dados de configuração em um banco de dados Microsoft SQL Server® ou o banco de dados interno do Windows \(WID\) recurso que está incluído no Windows Server® 2008, Windows Server 2008 R2 e Windows Server® 2012.  
  
> [!NOTE]  
> Todo o conteúdo do banco de dados de configuração do AD FS pode ser armazenado em uma instância do WID ou do banco de dados do SQL, mas não em ambas. Isso significa que não é possível ter alguns servidores de federação usando o WID e outros usando um banco de dados do SQL Server para a mesma instância do banco de dados de configuração do AD FS.  
  
Use as informações fornecidas mais adiante neste tópico juntamente com o conteúdo oferecido em [Considerações de topologia de implantação do AD FS](https://technet.microsoft.com/library/gg982489.aspx) para saber mais sobre as vantagens e desvantagens de escolher o WID ou o SQL Server para armazenar o banco de dados de configuração do AD FS:  
  
WID usa armazenamento de dados relacionais e não tem sua própria interface de usuário de gerenciamento \(interface do usuário\). Em vez disso, os administradores podem modificar o conteúdo do banco de dados de configuração do AD FS usando qualquer um dos snap de gerenciamento do AD FS a\-em Fsconfig.exe, ou os cmdlets Windows PowerShell™.  
  
## <a name="using-wid-to-store-the-ad-fs-configuration-database"></a>Usando o WID para armazenar o banco de dados de configuração do AD FS  
Você pode criar o banco de dados de configuração do AD FS usando o WID como o repositório usando o comando Fsconfig.exe\-ferramenta de linha ou o Assistente de configuração do servidor de Federação do AD FS. Ao usar uma dessas ferramentas, escolha uma das opções a seguir para criar sua topologia do servidor de federação. Todas essas opções usam o WID para armazenar o banco de dados de configuração do AD FS:  
  
-   Criar um suporte\-servidor de Federação autônomo  
  
-   Criar o primeiro servidor de federação em um farm de servidores de federação  
  
-   Adicionar um servidor de federação a um farm de servidores de federação  
  
Se você selecionar o suporte do\-opção sozinha, o WID é usada para armazenar uma única instância do banco de dados de configuração do AD FS. Esta instância não poderá ser compartilhada entre diversos servidores de federação. Ele destina-se somente a ambientes de laboratório de teste. Para obter mais informações sobre o suporte\-sozinho, opção de servidor de Federação ou como configurá-lo, consulte [stand-alone servidores de Federação usando WID](https://technet.microsoft.com/library/gg982486.aspx) ou [Create a stand-alone Federation Server](https://technet.microsoft.com/library/ee913579.aspx).  
  
Se você selecionar a opção de primeiro servidor de federação em um farm de servidores de federação, o WID será configurado visando a escalabilidade para permitir que servidores de federação adicionais sejam adicionados ao farm posteriormente. Para ver mais informações sobre como implantar um farm no WID ou como configurá-lo, consulte [Farm de servidores de federação usando o WID](https://technet.microsoft.com/library/gg982492.aspx) ou [Criar o primeiro servidor de federação em um farm de servidores de federação](https://technet.microsoft.com/library/dd807070.aspx)  
  
Se você selecionar a opção de adicionar um servidor de federação, o WID será configurado para replicar as alterações do banco de dados de configuração realizadas no novo servidor de federação em intervalos definidos. Para mais informações sobre como adicionar um servidor de federação a um farm do WID, consulte [Farm de servidores de federação usando o WID](https://technet.microsoft.com/library/gg982492.aspx) ou [Adicionar um servidor de federação a um farm de servidores de federação](https://technet.microsoft.com/library/ee913575.aspx).  
  
> [!NOTE]  
> Quando você implanta um farm de servidores de Federação usando WID, alguns recursos do AD FS podem não estar disponíveis. Para ter acesso ao conjunto completo de recursos ao configurar seu farm de servidores, considere usar o Microsoft SQL Server para armazenar o banco de dados de configuração do AD FS. Para mais informações, consulte [Considerações de topologia de implantação do AD FS](https://technet.microsoft.com/library/gg982489(v=ws.11).aspx).  
  
### <a name="how-a-wid-federation-server-farm-works"></a>Como funciona um farm de servidores de federação do WID  
Esta seção descreve os conceitos mais importantes que mostram como o farm de servidores de federação do WID replica os dados entre os servidores de federação primários e secundários. .  
  
#### <a name="primary-federation-server"></a>Servidor de federação primário  
Um servidor de Federação primário é um computador que executa o Windows Server 2008, Windows Server 2008 R2 ou Windows Server® 2012 que foi configurado na função de servidor de federação com o Assistente de configuração do servidor de Federação do AD FS e que tem uma cópia de leitura/gravação do AD FS banco de dados de configuração. O servidor de Federação primário sempre é criado quando você usa o Assistente de configuração do servidor de Federação do AD FS e selecione a opção para criar um novo serviço de Federação e transformar o computador o primeiro servidor de federação no farm. Todos os demais servidores de federação deste farm, também chamados de servidores de federação secundários, deverão sincronizar as alterações realizadas no servidor primário para uma cópia do banco de dados de configuração do AD FS armazenado localmente.  
  
#### <a name="secondary-federation-servers"></a>Servidores de federação secundários  
Servidores de Federação secundários armazenam uma cópia do banco de dados de configuração do AD FS do servidor de Federação primário, mas essas cópias são lidos\-apenas. Os servidores de federação secundários conectam-se e sincronizam os dados com o servidor de federação primário no farm sondando-o em intervalos regulares para verificar se os dados foram alterados. Os servidores de Federação secundária existem para fornecer tolerância a falhas para o servidor de Federação primário ao agir para carregar\-balancear as solicitações de acesso que são feitas em sites diferentes em todo o ambiente de rede.  
  
> [!NOTE]  
> Se o servidor de federação primário ficar inativo e offline, todos os servidores de federação secundários continuarão a processar as solicitações normalmente. Contudo, nenhuma alteração poderá ser realizada no Serviço de Federação até que o servidor primário seja restaurado. Você também pode nominar um servidor de federação secundário para tornar-se o primário usando o Windows PowerShell. Para obter mais informações, consulte o [administração do AD FS com o Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
#### <a name="how-the-adfs-configuration-database-is-synchronized"></a>Como o banco de dados de configuração do AD FS é sincronizado?  
Devido à importante função desempenhada pelo banco de dados de configuração do AD FS, ele é disponibilizado em todos os servidores de federação na rede para fornecer tolerância a falhas e carga\-balanceamento de recursos durante o processamento de solicitações \(quando carga de rede\-balanceadores são usados\). Contudo, para que os servidores de federação secundária possuam esta capacidade, o banco de dados de configuração do AD FS armazenado no servidor de federação primário deve ser sincronizado.  
  
Ao adicionar um servidor de federação ao farm, o novo computador que será um servidor de federação secundário conecta-se ao servidor de federação primário para replicar a cópia do banco de dados de configuração do AD FS. A partir deste ponto, o novo servidor de federação continua a obter atualizações do servidor de federação primário regularmente, como mostrado na ilustração a seguir.  
  
![Funções do AD FS](media/adfs2_WID.png)  
  
Cada servidor de federação secundário sonda o primário a cada cinco minutos em busca de alterações. Você pode ajustar esse padrão de cinco\-valor de minutos ou forçar uma sincronização imediata a qualquer momento usando um cmdlet do Windows PowerShell. Para obter mais informações sobre como fazer isso, consulte [administração do AD FS com o Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
O processo de sincronização do WID também dá suporte a transferências incrementais para oferecer transferências mais eficientes de alterações intermediárias. O processo de transferência incremental exige um tráfego significativamente menor em uma rede e as transferências são concluídas muito mais rápido.  
  
> [!NOTE]  
> Há suporte para a migração de um banco de dados de configuração do AD FS do WID para uma instância do SQL Server. Para obter mais informações sobre como fazer isso, consulte [do AD FS: Migrar seu banco de dados de configuração do AD FS para o SQL Server](https://go.microsoft.com/fwlink/?LinkId=192232) no site TechNet Wiki.  
  
## <a name="using-sql-server-to-store-the-ad-fs-configuration-database"></a>Usando o SQL Server para armazenar o banco de dados de configuração do AD FS  
Você pode criar o banco de dados de configuração do AD FS usando uma única instância de banco de dados do SQL Server como o repositório usando o comando Fsconfig.exe\-ferramenta de linha. Usar um banco de dados do SQL Server como banco de dados de configuração do AD FS oferece as seguintes vantagens em comparação com o WID:  
  
-   Os administradores podem aproveitar os recursos de alta disponibilidade do SQL Server  
  
-   Ele fornece aumentos de desempenho adicionais para alto nível de tráfego.  
  
-   Ele fornece suporte a recursos de resolução do artefato SAML e SAML/WS\-detecção de reprodução de token de Federação \(descrito abaixo\).  
  
O termo "servidor de federação primário" não se aplica quando o banco de dados de configuração do AD FS é armazenado em uma instância de banco de dados SQL, pois todos os servidores de federação podem ler e gravar igualmente o banco de dados de configuração do AD FS que está usando a mesma instância do SQL Server em cluster, como mostrado na ilustração a seguir.  
  
![Funções do AD FS](media/adfs2_SQL.png)  
  
Você pode usar o SQL Server para configurar dois ou mais servidores trabalhem juntos como um cluster de servidor para garantir que o AD FS se torna altamente disponível para as solicitações de cliente do serviço. Alta disponibilidade fornece uma escala\-out arquitetura na qual você pode aumentar a capacidade do servidor acrescentando servidores adicionais. Pontos de falha únicos são mitigados pelo failover de cluster automático.  
  
Você pode alcançar alta disponibilidade por meio da carga de rede\-failover e balanceamento de serviços que oferecem tecnologias de cluster do SQL. Para obter mais informações sobre como configurar o SQL Server para alta disponibilidade, consulte [visão geral de soluções de alta disponibilidade](https://go.microsoft.com/fwlink/?LinkId=179853).  
  
### <a name="saml-artifact-resolution"></a>Resolução do artefato SAML  
Security Assertion Markup Language \(SAML\) resolução do artefato é um ponto de extremidade com base na parte do protocolo SAML 2.0 que descreve como uma terceira parte confiável pode recuperar um token diretamente de um provedor de declarações. No primeiro estágio do processo de resolução, um cliente de navegador contata um servidor de federação do recurso e fornece um artefato a ele. No segundo estágio, os servidores de federação de recurso enviam o artefato para uma URL de terminal de artefato SAML hospedada em alguma organização do parceiro de conta para resolver a mensagem de artefato. No estágio final, o servidor de federação de conta emite o token para o servidor de federação em nome do navegador cliente.  
  
> [!NOTE]  
> Se você for um administrador em uma organização do parceiro de conta, certifique-se de atribuir ou associar um certificado SSL, que se encadeia a um certificado raiz de um membro do Windows Root Certificate Program, ao site de Web passivo da federação em IIS \( <ComputerName> \\Sites\\Site padrão\\adfs\\ls\) em todos os servidores de federação de conta no farm. Isso é importante para evitar que os servidores de federação de recurso tenham que adicionar manualmente o certificado de SSL ao repositório de certificados de Computadores locais de pessoas confiáveis ou evitar que não seja possível resolver o artefato publicado na sua organização.  
  
### <a name="samlws---federation-token-replay-detection"></a>SAML/WS - detecção de reprodução de token de Federação  
O termo *reprodução de token* refere-se ao ato pelo qual um cliente navegador em uma organização do parceiro de conta tenta enviar o mesmo token recebido de um servidor de federação de conta múltiplas vezes para autenticar-se a um servidor de federação de recurso.  Isso ocorre quando um usuário clica no botão **Voltar** do navegador para tentar reenviar a página de autenticação.  
  
O AD FS oferece um recurso chamado de *detecção de reprodução de token* com o qual solicitações múltiplas usando o mesmo token são detectadas e descartadas. Quando esse recurso está habilitado, detecção de reprodução de token protege a integridade das solicitações de autenticação em ambos os WS\-perfil passivo da federação e o perfil SAML WebSSO, certificando-se de que o mesmo token nunca seja usado mais de uma vez. Este recurso deve ser habilitado em situações nas quais a segurança é uma grande preocupação, como ao usar quiosques.  
  
No exemplo do quiosque, um usuário pode sair de todos os sites da web e depois um usuário mal-intencionado pode tentar usar o histórico de navegação para reenviar a página de autenticação federada carregada pelo usuário anterior. Este recurso minimiza esta preocupação ao armazenar informações adicionais sobre cada autenticação realizada com êxito por uma organização do parceiro de conta para detectar reproduções posteriores do token e evitar tentativas múltiplas de autenticação bem-sucedida.  
  

