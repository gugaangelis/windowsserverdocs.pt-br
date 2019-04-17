---
ms.assetid: 68db7f26-d6e3-4e67-859b-80f352e6ab6a
title: "A função do banco de dados de configuração do AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3372e1f051ba7f900753a4961d948ddabdef6f4d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-the-ad-fs-configuration-database"></a>A função do banco de dados de configuração do AD FS
O banco de dados de configuração do AD FS armazena todos os dados de configuração que representa uma única instância de \(AD FS\) \(that is, the Federation Service\) serviços de Federação do Active Directory. O banco de dados de configuração do AD FS define o conjunto de parâmetros que exige um serviço de federação para identificar os parceiros, certificados, repositórios de atributo, declarações e vários dados sobre essas entidades associadas. Você pode armazenar esses dados de configuração em um banco de dados de Microsoft SQL Server® ou o recurso de banco de dados interno do Windows \(WID\) que está incluído no Windows Server® 2008, Windows Server 2008 R2 e Windows Server® 2012.  
  
> [!NOTE]  
> Todo o conteúdo do banco de dados de configuração do AD FS pode ser armazenada em uma instância de trabalho ou em uma instância do banco de dados SQL, mas não ambos. Isso significa que você não pode ter alguns servidores de Federação usando trabalho e outros itens usando um banco de dados do SQL Server para a mesma instância do banco de dados de configuração do AD FS.  
  
Você pode usar as informações a seguir neste tópico junto com o conteúdo fornecido no [considerações de topologia de implantação do AD FS](https://technet.microsoft.com/library/gg982489.aspx) para saber mais sobre as vantagens e desvantagens de escolha de trabalho ou SQL Server para armazenar o banco de dados de configuração do AD FS:  
  
Usos de trabalho um dados relacionais armazenar e não têm seu próprio gerenciamento de usuário interface \(UI\). Em vez disso, os administradores podem modificar o conteúdo do banco de dados de configuração do AD FS usando o AD FS gerenciamento snap\-in, Fsconfig.exe ou Windows PowerShell™ cmdlets.  
  
## <a name="using-wid-to-store-the-ad-fs-configuration-database"></a>Usando o trabalho para armazenar o banco de dados de configuração do AD FS  
Você pode criar o banco de dados de configuração do AD FS em trabalho como a loja usando a ferramenta de linha de command\ Fsconfig.exe ou o Assistente para configuração do servidor de Federação do AD FS. Quando você usa qualquer uma dessas ferramentas, você pode escolher qualquer uma das opções a seguir para criar a topologia de servidor de Federação. Cada uma dessas opções usa o trabalho para armazenar o banco de dados de configuração do AD FS:  
  
-   Criar um servidor de Federação stand\ sozinho  
  
-   Criar o primeiro servidor de Federação em um farm de servidores de Federação  
  
-   Adicionar um servidor de federação para um farm de servidores de Federação  
  
Se você selecionar a opção stand\ sozinho, o trabalho é usado para armazenar uma única instância do banco de dados de configuração do AD FS. Essa instância não pode ser compartilhada entre vários servidores de Federação. Ele se destina apenas ambientes de laboratório de teste. Para saber mais sobre a opção de servidor de Federação stand\ sozinho ou como configurar uma, consulte [usando o trabalho de servidor de Federação autônomo](https://technet.microsoft.com/library/gg982486.aspx) ou [criar um servidor de Federação autônomo](https://technet.microsoft.com/library/ee913579.aspx).  
  
Se você selecionar o servidor de Federação primeiro em uma opção de farm de servidor de federação, trabalho está configurado para escalabilidade que permitirá que os servidores de Federação adicional a ser adicionada ao sítio em um momento posterior. Para obter mais informações sobre como implantar um farm de trabalho ou como configurar uma, consulte [federação Server Farm usando trabalho](https://technet.microsoft.com/library/gg982492.aspx) ou [criar o primeiro servidor de Federação em um Farm de servidores de Federação](https://technet.microsoft.com/library/dd807070.aspx)  
  
Se você selecionar a adicionar uma opção de servidor de federação, trabalho está configurado para replicar as alterações de configuração do banco de dados para o novo servidor de Federação em intervalos definidos. Para obter mais informações sobre como adicionar um servidor de federação para um farm de trabalho, consulte [federação Server Farm usando trabalho](https://technet.microsoft.com/library/gg982492.aspx) ou [adicionar um servidor de federação para um Farm de servidores de Federação](https://technet.microsoft.com/library/ee913575.aspx).  
  
> [!NOTE]  
> Quando você implanta um farm de servidores de Federação usando o trabalho, alguns recursos do AD FS podem não estar disponíveis. Para ter acesso ao recurso completo definido quando você configura seu farm de servidores, considere usar o Microsoft SQL Server para armazenar o banco de dados de configuração do AD FS em vez disso. Para obter mais informações, consulte [considerações de topologia de implantação do AD FS](https://technet.microsoft.com/library/gg982489(v=ws.11).aspx).  
  
### <a name="how-a-wid-federation-server-farm-works"></a>Como funciona a um farm de servidores de Federação do trabalho  
Esta seção descreve conceitos importantes que descrevem como o farm de servidores de Federação do trabalho replica os dados entre um servidor de Federação principal e secundário federação servidores. .  
  
#### <a name="primary-federation-server"></a>Servidor de Federação principal  
Um servidor de Federação principal é um computador executando o Windows Server 2008, Windows Server 2008 R2 ou Windows Server® 2012 que tiver sido configurado na função de servidor de federação com o Assistente para configuração do servidor de Federação do AD FS e que tem uma cópia de leitura/gravação do banco de dados de configuração do AD FS. O servidor de Federação principal sempre é criado quando você usar o Assistente para configuração do servidor de Federação do AD FS e selecione a opção de criar um novo serviço de Federação e tornar o computador primeiro servidor de federação no farm. Todos os outros servidores de Federação neste farm, também conhecido como servidores de Federação secundário, precisará sincronizar as alterações feitas no servidor de Federação principal em uma cópia do banco de dados de configuração do AD FS que é armazenado localmente.  
  
#### <a name="secondary-federation-servers"></a>Servidores de Federação secundário  
Servidores de Federação secundário armazenam uma cópia do banco de dados de configuração do AD FS do servidor de Federação principal, mas essas cópias são somente read\. Servidores de Federação secundário conectem e sincronizam os dados com o servidor de Federação principal no farm sondando-lo em intervalos regulares para verificar se dados foram alterados. Os servidores de Federação secundário existem para fornecer tolerância para o servidor de Federação principal ao atuar às solicitações de acesso load\ saldo que são feitas em locais diferentes em todo o ambiente de rede.  
  
> [!NOTE]  
> Se um servidor de Federação principal trava e estiver offline, todos os servidores de Federação secundário continuam a processar solicitações como normal. No entanto, não há novas alterações podem ser feitas ao serviço de Federação até que o servidor de Federação principal foi trazido on-line. Você também pode indicar um servidor de Federação secundário para se tornar o servidor de Federação principal usando o Windows PowerShell. Para obter mais informações, consulte o [administração do AD FS com o Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
#### <a name="how-the-ad-fs-configuration-database-is-synchronized"></a>Como o banco de dados de configuração do AD FS é sincronizado  
Por causa da função importante que o banco de dados de configuração do AD FS reproduz, ela é disponibilizada em todos os servidores de federação na rede para fornecer tolerância e balanceamento load\ funcionalidades ao processar solicitações \ (quando balanceadores load\ de rede são used\). No entanto, para servidores de Federação secundário atuar nessa função, o banco de dados de configuração do AD FS que é armazenado no servidor de Federação principal deve ser sincronizado.  
  
Quando você adiciona um servidor de Federação ao farm, o novo computador que se tornará um servidor de Federação secundário se conecta ao servidor de Federação principal para replicar a cópia do banco de dados de configuração do AD FS. Neste ponto, o servidor de Federação novo continua a receber atualizações do servidor de Federação principal regularmente, conforme mostrado na ilustração a seguir.  
  
![Funções do AD FS](media/adfs2_WID.png)  
  
Cada servidor de Federação secundário sonda o servidor de Federação principal para alterações a cada cinco minutos. Você pode ajustar esse valor de hora five\ padrão ou forçar uma sincronização imediata a qualquer momento usando um cmdlet do Windows PowerShell. Para saber mais sobre como fazer isso, consulte [administração do AD FS com o Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
O processo de sincronização de trabalho também suporta transferências incrementais para transferências mais eficientes de alterações intermediárias. O processo de transferência incremental requer substancialmente menos tráfego em uma rede e as transferências são concluídas com mais rapidez.  
  
> [!NOTE]  
> Há suporte para a migração de um banco de dados de configuração do AD FS do trabalho para uma instância do SQL Server. Para saber mais sobre como fazer isso, consulte [AD FS: migrar seu banco de dados de configuração do AD FS para SQL Server](https://go.microsoft.com/fwlink/?LinkId=192232) no site do Wiki do TechNet.  
  
## <a name="using-sql-server-to-store-the-ad-fs-configuration-database"></a>Usando o SQL Server para armazenar o banco de dados de configuração do AD FS  
Você pode criar o banco de dados de configuração do AD FS em uma única instância de banco de dados do SQL Server como a loja usando a ferramenta de linha de command\ Fsconfig.exe. Usando um banco de dados do SQL Server como o banco de dados de configuração do AD FS fornece os seguintes benefícios ao longo de trabalho:  
  
-   Os administradores possam aproveitar os recursos de alta disponibilidade do SQL Server  
  
-   Ele fornece aumenta adicionais de desempenho de alto tráfego.  
  
-   Ele fornece suporte ao recurso de resolução de artefato SAML e WS\/SAML-federação repetição token detecção \(described below\).  
  
O termo "servidor de Federação principal" não é aplicada quando o banco de dados de configuração do AD FS é armazenado em uma instância do banco de dados SQL porque todos os servidores de Federação igualmente podem ler e gravar no banco de dados de configuração do AD FS que está usando a mesma instância do SQL Server clusterizada, conforme mostrado na ilustração a seguir.  
  
![Funções do AD FS](media/adfs2_SQL.png)  
  
Você pode usar o SQL Server para configurar dois ou mais servidores para funcionarem juntos como um cluster de servidor para garantir que o AD FS seja feito altamente disponível para solicitações de cliente recebidas do serviço. Alta disponibilidade fornece uma arquitetura de scale\-la no qual você pode aumentar a capacidade do servidor, adicionando servidores adicionais. Pontos de falha são mitigados pela automática de cluster de failover.  
  
Você pode obter alta disponibilidade, usando o load\-balanceamento de rede e serviços de failover que fornecem tecnologias de cluster SQL. Para obter mais informações sobre como configurar o SQL Server para alta disponibilidade, consulte [visão geral de soluções de alta disponibilidade](https://go.microsoft.com/fwlink/?LinkId=179853).  
  
### <a name="saml-artifact-resolution"></a>Resolução de artefato SAML  
Resolução de artefato de linguagem de marcação de asserção \(SAML\) de segurança é um ponto de extremidade com base na parte o protocolo SAML 2.0 que descreve como um terceiro pode recuperar um token diretamente de um provedor de declarações. Na primeira etapa do processo de resolução, um cliente de navegador entra em contato com um servidor de Federação do recurso e lhe fornece um artefato. Na segunda etapa, servidores de Federação do recurso enviam o artefato para uma URL de ponto de extremidade de artefato SAML que está hospedada em algum lugar em uma organização de parceiro de conta para resolver a mensagem artefato. No estágio final, o servidor de Federação conta emite o token para o servidor de Federação em nome do cliente do navegador.  
  
> [!NOTE]  
> Se você for um administrador em uma organização de parceiro de conta, certifique-se de atribuir ou associar um certificado SSL, que encadeia com um certificado raiz de um membro do programa de certificado raiz do Windows, no site da Web passivo de federação no IIS \ (<ComputerName>\\Sites\\Default Web Site\\adfs\\ls\) em todos os servidores de federação de conta no farm. Isso é importante para impedir que os servidores de Federação do recurso de precisar adicionar manualmente o certificado SSL para o repositório de certificados Local computadores confiáveis pessoas ou sendo incapaz de resolver o artefato que é publicado na sua organização.  
  
### <a name="samlws---federation-token-replay-detection"></a>Detecção de repetição SAML/WS - token de Federação  
O termo *repetição token* refere-se para o act pelo qual um cliente de navegador em uma organização de parceiro de conta tenta enviar o mesmo token recebida de um servidor de Federação conta várias vezes para se autenticar em um servidor de Federação do recurso.  Essa ação ocorre quando um usuário clica o **novamente** botão de navegador em um esforço para reenviar a página de autenticação.  
  
AD FS fornece um recurso conhecido como *token detecção de repetição* pelo quais várias solicitações de tokens usar o mesmo token pode ser detectado e, em seguida, descartado. Quando esse recurso está habilitado, a detecção de repetição token protege a integridade das solicitações de autenticação no perfil de Federação WS\ passivo e o perfil de SAML WebSSO, certificando-se de que o mesmo token de nunca é usado mais de uma vez. Esse recurso deve ser habilitado em situações onde a segurança é uma preocupação muito alta, como ao usar quiosques.  
  
No exemplo de quiosque, um usuário pode fazer logon em todos os sites e mais tarde, um usuário mal-intencionado pode tentar usar o histórico do navegador para reenviar a página de autenticação federada que foi carregada pelo usuário anterior. Este recurso atenua essa preocupação, armazenando as informações adicionais sobre cada autenticação bem-sucedida feita por uma organização de parceiro de conta para detectar repetições subsequentes do token e impedir que várias tentativas de autenticação êxito.  
  

