---
ms.assetid: a7c39656-81ee-4c2b-80ef-4d017dd11b07
title: Planejando uma implantação de Pastas de Trabalho
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
author: JasonGerend
manager: dongill
ms.author: jgerend
ms.date: 4/5/2017
description: Como planejar uma implantação de Pastas de Trabalho, incluindo os requisitos de sistema e como preparar o ambiente de rede.
ms.openlocfilehash: 06d56df7ce9ddb8c9822f62de383ccad0394b4f3
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447839"
---
# <a name="planning-a-work-folders-deployment"></a>Planejando uma implantação de Pastas de Trabalho

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10, Windows 8.1, Windows 7

Este tópico explica o processo de projeto de uma implementação de Pastas de Trabalho e supõe que você tenha o seguinte conhecimento básico:  
  
- Noção básica de Pastas de Trabalho (conforme descrito em [Pastas de Trabalho](work-folders-overview.md))  
  
- Noção básica de conceitos do AD DS (Active Directory Domain Services)  
  
- Noção básica de compartilhamento de arquivos do Windows e das tecnologias relacionadas  
  
- Noção básica de uso de certificados SSL  
  
- Noção básica de habilitação de acesso da Web a recursos internos por meio de um proxy reverso da Web  
  
  As seções a seguir ajudarão você no projeto de implementação de Pastas de Trabalho. A implantação de Pastas de Trabalho será abordada no próximo tópico, [Implantando Pastas de Trabalho](deploy-work-folders.md).  
  
##  <a name="BKMK_SOFT"></a> Requisitos de software  

As Pastas de Trabalho têm os seguintes requisitos de software para os servidores de arquivos e sua infraestrutura de rede:  
  
-   Um servidor que executa o Windows Server 2012 R2 ou o Windows Server 2016 para hospedar compartilhamentos de sincronização com arquivos de usuário  
  
-   Um volume formatado com o sistema de arquivos NTFS para armazenar arquivos de usuário  
  
-   Para forçar políticas de senha nos computadores Windows 7, você deve usar políticas de senha de Política de Grupo. Também é necessário excluir os PCs Windows 7 de políticas de senha de Pastas de Trabalho (caso use-as).

-   Um certificado de servidor para cada servidor de arquivo que hospedará Pastas de Trabalho. Esses certificados devem ser de uma autoridade de certificação (CA) confiável por seus usuários; o ideal é uma CA pública.

-   (Opcional) Uma floresta de Serviços de Domínio do Active Directory com extensões de esquema no Windows Server 2012 R2 para dar suporte à referência automática de computadores e dispositivos no servidor de arquivos correto ao usar vários servidores de arquivos. 
  
Para habilitar usuários a sincronizar na Internet, existem requisitos adicionais:  
  
-   A capacidade de tornar um servidor acessível pela Internet, criando regras de publicação no proxy reverso ou gateway de rede de sua organização  
  
-   (Opcional) Um nome de domínio registrado publicamente e a capacidade de criar registros DNS públicos adicionais para o domínio  
  
-   (Opcional) Infraestrutura AD FS (Serviços de Federação do Active Directory) ao usar a autenticação AD FS  
  
As Pastas de Trabalho têm os seguintes requisitos de software para computadores clientes:  
  
-   Os computadores devem estar executando um dos seguintes sistemas operacionais:  
  
    -   Windows 10  
  
    -   Windows 8.1  
  
    -   Windows RT 8.1  
  
    -   Windows 7  
  
    -   Android 4.4 KitKat e posterior  
  
    -   iOS 10.2 e posterior  
  
-   Os PCs Windows 7 devem estar executando uma das seguintes edições do Windows:  
  
    -   Windows 7 Professional  
  
    -   Windows 7 Ultimate  
  
    -   Windows 7 Enterprise  
  
-   Os computadores Windows 7 devem ser ingressados no domínio da sua organização (eles não podem ser ingressados em um grupo de trabalho).  
  
-   Espaço livre suficiente em uma unidade local formatada para NTFS para armazenar todos os arquivos de usuário em Pastas de Trabalho, mais 6 GB adicionais de espaço livre se o serviço Pastas de Trabalho estiver localizado na unidade de sistema, como acontece por padrão. Pastas de Trabalho usa o seguinte local por padrão: **%USERPROFILE%\Work Folders**  
  
     No entanto, os usuários podem alterar o local durante a configuração (cartões microSD e unidades USB formatadas com o sistema de arquivos NTFS são locais com suporte, mas a sincronização será interrompida se as unidades forem removidas).  
  
     O tamanho máximo para arquivos individuais é 10 GB, por padrão. Não há limite de armazenamento por usuário, embora os administradores possam usar a funcionalidade de cotas do Gerenciador de Recursos do Servidor de Arquivos para implementar cotas.  
  
-   O serviço Pastas de Trabalho não oferece suporte à reversão do estado das máquinas virtuais clientes. Em vez disso, execute operações de backup e restauração de dentro da máquina virtual cliente usando o Backup de Imagem do Sistema ou outro aplicativo de backup.  
  
> [!NOTE]
>  Verifique se instalou o pacote cumulativo de atualizações de disponibilidade geral do Windows Server 2012 R2 e do Windows 8.1 em todos os servidores de Pastas de Trabalho e todos os computadores clientes com Windows 8.1 ou Windows Server 2012 R2. Para obter mais informações, consulte o artigo [2883200](https://support.microsoft.com/kb/2883200) na Base de Dados de Conhecimento Microsoft.  
  
## <a name="deployment-scenarios"></a>Cenários de implantação  
 As Pastas de Trabalho podem ser implementadas em qualquer número de servidores de arquivo em um ambiente do cliente. Isso permite que as implementações de Pastas de Trabalho sejam dimensionadas com base nas necessidades do cliente e possam resultar em implantações altamente individualizadas. No entanto, a maioria das implantações cairá em um dos três cenários básicos a seguir.  
  
### <a name="single-site-deployment"></a>Implantação de Site Único  
 Em uma implantação de site único, os servidores de arquivos são hospedados em um site central na infraestrutura do cliente. Esse tipo de implantação é visto com mais frequência nos clientes com uma infraestrutura altamente centralizada ou com filiais menores que não mantêm servidores de arquivos locais. Esse modelo de implantação pode ser mais fácil para a equipe de TI administrar, uma vez que todos os ativos de servidores são locais e, provavelmente, a entrada/saída da Internet é centralizada nesse local também. No entanto, esse modelo de implantação também conta com uma boa conectividade de WAN entre o site central e qualquer filial, e os usuários nas filiais são vulneráveis a uma interrupção de serviço devido a condições da rede.  
  
### <a name="multiple-site-deployment"></a>Implantação multissite  
 Em uma implantação multissite, os servidores de arquivos são hospedados em vários locais na infraestrutura do cliente. Isso poderá significar vários datacenters ou poderá significar que as filiais mantêm servidores de arquivos individuais. Esse tipo de implantação geralmente é visto em ambientes de cliente de grande porte ou em clientes que têm várias filiais grandes que mantêm ativos de servidores locais. Esse modelo de implantação é mais complexo para a equipe de TI administrar e conta com uma coordenação cuidadosa de armazenamento de dados e manutenção do AD DS (Active Directory Domain Services) para garantir que os usuários usem o servidor de sincronização correto para Pastas de Trabalho.  
  
### <a name="hosted-deployment"></a>Implantação hospedada  
 Em uma implantação hospedada, os servidores de sincronização são implantados em uma solução IAAS (Infraestrutura como um Serviço), como o Windows Azure VM. Esse método de implantação tem a vantagem de tornar a disponibilidade dos servidores de arquivos menos dependente da conectividade WAN na empresa de um cliente. Se for possível para um dispositivo estabelecer conexão com a Internet, ele poderá obter seu servidor de sincronização. No entanto, os servidores implantados no ambiente hospedado ainda precisam atingir o domínio do Active Directory da organização para autenticar usuários, e o cliente negocia os requisitos da infraestrutura no local para complexidade adicional na manutenção dessa conexão.  
  
## <a name="deployment-technologies"></a>Tecnologias de implantação  
 As implantações de Pastas de Trabalho consistem em várias tecnologias que trabalham juntas para fornecer serviço a dispositivos nas redes internas e externas. Antes de projetar uma implantação de Pastas de Trabalho, os clientes devem estar familiarizados com os requisitos de cada uma das tecnologias a seguir.  
  
### <a name="active-directory-domain-services"></a>Active Directory Domain Services  
 O AD DS fornece dois serviços importantes em uma implantação de Pastas de Trabalho. Primeiro, como back-end para autenticação do Windows, o AD DS fornece os serviços de autenticação e segurança que são usados para conceder acesso aos dados do usuário. Se um controlador de domínio não puder ser atingido, um servidor de arquivos não poderá autenticar uma solicitação de entrada e o dispositivo não poderá acessar os dados armazenados no compartilhamento de sincronização desse servidor de arquivos.  
  
 Segundo, o AD DS (com a atualização de esquema do Windows Server 2012 R2) mantém o atributo msDS-SyncServerURL em cada usuário, que é usado para direcionar automaticamente os usuários para o servidor de sincronização apropriado.  
  
### <a name="file-servers"></a>Servidores de Arquivos  
 Os servidores de arquivos que executam o Windows Server 2012 R2 ou Windows Server 2016 hospedam o serviço de função de Pastas de Trabalho e os compartilhamentos de sincronização que armazenam dados de Pastas de Trabalho de usuários. Os servidores de arquivos também podem hospedar dados armazenados por outras tecnologias que operam na rede interna (como compartilhamentos de arquivo) e podem ser clusterizados para fornecer tolerância a falhas para dados de usuário.  
  
###  <a name="GroupPolicy"></a> Política de grupo  
 Se você tiver os computadores Windows 7 no seu ambiente, recomendamos o seguinte:  
  
- Usar a Política de Grupo para controlar políticas de senha para todos os computadores ingressados no domínio que usam Pastas de Trabalho.  
  
- Use a política **Bloquear tela automaticamente e solicitar uma senha** de Pastas de Trabalho em computadores que não são ingressados no domínio.  
  
  Você também pode usar a Política de Grupo para especificar um servidor de Pastas de Trabalho para os computadores ingressados no domínio. Isso simplifica um pouco a configuração de Pastas de Trabalho – de outra forma, os usuários precisariam inserir seu endereço de email de trabalho para procurar as configurações (supondo-se que as Pastas de Trabalho estejam configuradas adequadamente) ou inserir a URL de Pastas de Trabalho que você forneceu explicitamente por email ou outros meios de comunicação.  
  
  Você também pode usar a Política de Grupo para forçar a configuração de Pastas de Trabalho por usuário ou por computador, embora isso possa fazer com que as Pastas de Trabalho sincronizem em cada PC uma entrada de usuário (ao usar a configuração de política por usuário) e possa impedir que os usuários especifiquem um local alternativo para Pastas de Trabalho no PC (como um cartão microSD para conservar espaço na unidade principal). Sugerimos avaliar com atenção as necessidades do usuário antes de forçar a configuração automática.  
  
### <a name="windows-intune"></a>Windows Intune  
 O Windows Intune também oferece uma camada de segurança e capacidade de gerenciamento para dispositivos não ingressados no domínio que de outra forma não estariam presentes. Você pode usar o Windows Intune para configurar e gerenciar dispositivos pessoais dos usuários, como tablets, que se conectam a Pastas de Trabalho pela Internet. Windows Intune pode fornecer dispositivos com a URL do servidor de sincronização para usar – caso contrário, os usuários devem inserir seu endereço de email de trabalho para consultar as configurações (se você publicar uma URL de pastas de trabalho pública na forma de https://workfolders. <em>Contoso.com</em>), ou insira a URL do servidor de sincronização diretamente.  
  
 Sem uma implantação do Windows Intune, os usuários devem configurar dispositivos externos manualmente, o que pode resultar no aumento da demanda na equipe de suporte técnico de um cliente.  
  
 Você também pode usar o Windows Intune para limpar seletivamente os dados das Pastas de Trabalho em um dispositivo de usuário sem afetar o restante de seus dados – útil se um usuário sair de sua organização ou se tiver seu dispositivo roubado.  
  
### <a name="web-application-proxyazure-ad-application-proxy"></a>Proxy de aplicativo Web/Proxy de aplicativo Azure AD  
 As Pastas de Trabalho são criadas em torno do conceito de permitir que os dispositivos conectados à Internet recuperem com segurança dados corporativos da rede interna, o que permite que os usuários "carreguem seus dados com eles" em seus tablets e dispositivos para lugares onde normalmente não poderiam acessar arquivos de trabalho. Para fazer isso, um proxy reverso deve ser usado para publicar URLs de servidor de sincronização e torná-las disponíveis para clientes de Internet. 
 
Pastas de Trabalho usando proxy de aplicativo Web, proxy de aplicativo Azure AD ou soluções de proxy reversas de terceiros: 

-  Proxy de aplicativo Web é uma solução de proxy reversa no local. Para saber mais, consulte [Proxy de aplicativo Web no Windows Server 2016](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server).  
  
-  Proxy de aplicativo Web do Azure D é uma solução de proxy reversa na nuvem. Para saber mais, consulte [Como fornecer acesso remoto seguro para aplicativos locais](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

## <a name="additional-design-considerations"></a>Considerações de projeto adicionais  
 Além de entender cada um dos componentes referidos acima, os clientes precisam dedicar tempo a seus projetos pensando sobre o número de servidores de sincronização e compartilhamentos a operar, bem como se devem ou não usar clustering de failover para fornecer tolerância a falhas nesses servidores de sincronização  
  
### <a name="number-of-sync-servers"></a>Número de servidores de sincronização  
 É possível para um cliente operar vários servidores de sincronização em um ambiente. Esta pode ser uma configuração desejável por vários motivos:  
  
- Distribuição geográfica de usuários – por exemplo, servidores de arquivos de filiais ou datacenters regionais  
  
- Requisitos de armazenamento de dados – certos departamentos corporativos podem ter requisitos específicos de controle ou armazenamento de dados que são mais fáceis com um servidor dedicado  
  
- Balanceamento de carga – em grandes ambientes, o armazenamento de dados de usuário em vários servidores pode aumentar o desempenho e o tempo de atividade do servidor.  
  
  Para obter informações sobre dimensionamento e desempenho do servidor de Pastas de Trabalho, consulte [Considerações de desempenho para implantações de Pastas de Trabalho](http://blogs.technet.com/b/filecab/archive/2013/11/01/performance-considerations-for-large-scale-work-folders-deployments.aspx).  
  
> [!NOTE]
>  Ao usar vários servidores de sincronização, recomendamos configurar a descoberta automática de servidor para usuários. Esse processo conta com a configuração de um atributo em cada conta de usuário no AD DS. O atributo é denominado **msDS-SyncServerURL** e se torna disponível em contas de usuário depois que um controlador de domínio do Windows Server 2012 R2 é adicionado ao domínio ou as atualizações de esquema do Active Directory são aplicadas. Esse atributo deve ser definido para cada usuário, a fim de garantir que os usuários se conectem ao servidor de sincronização apropriado. Ao usar a descoberta automática do servidor, as organizações podem publicar pastas de trabalho por trás de uma URL "amigável", como *https://workfolders.contoso.com* , independentemente do número de servidores de sincronização em operação.  
  
### <a name="number-of-sync-shares"></a>Número de compartilhamentos de sincronização  
 Servidores de sincronização individuais podem manter vários compartilhamentos de sincronização. Isso pode ser útil pelos seguintes motivos:  
  
-   Requisitos de auditoria e segurança – se os dados usados por certo departamento devem ser auditados ou retidos por um período mais longo, compartilhamentos de sincronização separados podem ajudar os administradores a manter pastas de usuário com níveis diferentes de auditoria separadas.  
  
-   Cotas ou telas de arquivo diferentes – se você desejar definir diferentes cotas ou limites de armazenamento em que tipos de arquivo são permitidos em Pastas de Trabalho (telas de arquivo) para grupos diferentes de usuários, compartilhamentos de sincronização separados podem ajudar.  
  
-   Controle departamental – se obrigações administrativas forem distribuídas por departamento, a utilização de compartilhamentos separados para departamentos diferentes pode ajudar os administradores na imposição de cotas ou outras políticas.  
  
-   Diferentes políticas de dispositivo – se uma organização precisar manter várias políticas de dispositivo (como a criptografia de Pastas de Trabalho) para grupos diferentes de usuários, o uso de vários compartilhamentos permite isso.  
  
-   Capacidade de armazenamento – se um servidor de arquivos tiver vários volumes, compartilhamentos adicionais poderão ser usados para tirar proveito desses volumes adicionais. Um compartilhamento individual tem acesso apenas ao volume em que está hospedado e não pode tirar proveito de volumes adicionais em um servidor de arquivos.  
  
#### <a name="access-to-sync-shares"></a>Acesso a compartilhamentos de sincronização  

Embora o servidor de sincronização que um usuário acessa seja determinado pela URL inserida em seu cliente (ou a URL publicada para esse usuário no AD DS ao usar a descoberta automática de servidor), o acesso a compartilhamentos de sincronização individuais é determinado pelas permissões presentes no compartilhamento.  
  
Como resultado, se um cliente estiver hospedando vários compartilhamentos de sincronização no mesmo servidor, é necessário tomar cuidado para garantir que usuários individuais tenham permissões para acessar apenas um desses compartilhamentos. Caso contrário, quando os usuários conectarem ao servidor, seus clientes poderão conectar ao compartilhamento errado. Isso pode ser realizado criando um grupo de segurança separado para cada compartilhamento de sincronização.  
  
Além disso, o acesso a uma pasta de usuário individual em um compartilhamento de sincronização é determinado por direitos de propriedade na pasta. Ao criar um compartilhamento de sincronização, as Pastas de Trabalho concedem, por padrão, aos usuários acesso exclusivo a seus arquivos (desabilitando a herança e tornando-os proprietários de suas pastas individuais).  
  
## <a name="design-checklist"></a>Lista de verificação de projeto  

O conjunto de perguntas do projeto a seguir foi criado para auxiliar clientes no projeto de uma implementação de Pastas de Trabalho mais adequada para seu ambiente. Os clientes devem trabalhar nessa lista de verificação antes de tentar implantar servidores.  
  
-   Usuários pretendidos  
  
    -   Quais usuários usarão Pastas de Trabalho?  
  
    -   Como os usuários estão organizados? (Geograficamente, por escritório, por departamento, etc.)  
  
    -   Os usuários têm requisitos especiais de armazenamento de dados, segurança ou retenção?  
  
    -   Os usuários têm requisitos específicos de política de dispositivo, como criptografia?  
  
    -   A quais os computadores cliente e dispositivos é necessário dar suporte? (Windows 8.1, Windows RT 8.1, Windows 7)  
  
         Se você estiver oferecendo suporte a computadores Windows 7 e deseja usar políticas de senha, exclua o domínio que armazena as contas de computador da política de senha de Pastas de Trabalho e, em vez disso, nesse domínio, use políticas de senha de Política de Grupo para computadores ingressados no domínio.  
  
    -   Você precisa interoperar ou migrar de outras soluções de gerenciamento de dados de usuário, como Redirecionamento de Pasta?  
  
    -   Os usuários de vários domínios precisam sincronizar na Internet com um servidor único?  
  
    -   Você precisa dar suporte a usuários que não são membros do grupo Administradores Locais nos computadores ingressados no domínio? (Se sim, você precisará excluir os domínios relevantes das políticas de dispositivo das Pastas de Trabalho, como políticas de criptografia e de senha.)  
  
-   Planejamento de infraestrutura e capacidade  
  
    -   Em quais sites os servidores de sincronização devem estar localizados na rede?  
  
    -   Algum servidor de sincronização será hospedado por um provedor IaaS, como em uma VM do Azure?  
  
    -   Serão necessários servidores dedicados para todos os grupos específicos de usuários e, se sim, quantos usuários para cada servidor dedicado?  
  
    -   Onde estão os pontos de entrada/saída da Internet na rede?  
  
    -   Os servidores de sincronização serão clusterizados para tolerância a falhas?  
  
    -   Os servidores de sincronização precisarão manter vários volumes de dados para hospedar dados do usuário?  
  
-   Segurança de dados  
  
    -   Vários compartilhamentos de sincronização precisam ser criados em todos os servidores de sincronização?  
  
    -   Quais grupos serão usados para fornecer acesso a compartilhamentos de sincronização?  
  
    -   Se você estiver usando vários servidores de sincronização, qual grupo de segurança você criará para a capacidade delegada para modificar a propriedade msDS-SyncServerURL de objetos de usuário?  
  
    -   Há algum requisito específico de segurança ou auditoria para compartilhamentos de sincronização individuais?  
  
    -   A MFA (autenticação multifator) é necessária?  
  
    -   Você precisa limpar remotamente os dados das Pastas de Trabalho de computadores e dispositivos?  
  
-   Acesso ao dispositivo  
  
    -   Qual URL será usada para fornecer acesso a dispositivos baseados na Internet (*a URL padrão que é necessária para a descoberta automática de servidor baseada em email é workfolders.domainname*)?  
  
    -   Como a URL será publicada na Internet?  
  
    -   A descoberta automática do servidor será usada?  
  
    -   A Política de Grupo será usada para configurar computadores ingressados no domínio?  
  
    -   O Windows Intune será usado para configurar dispositivos externos?  
  
    -   O Registro de Dispositivo será necessário para conectar os dispositivos?  
  
## <a name="next-steps"></a>Próximas etapas  
 Após projetar a implementação de Pastas de Trabalho, é o momento de implantar Pastas de Trabalho. Para obter mais informações, consulte [Implantando Pastas de Trabalho](deploy-work-folders.md).  
  
## <a name="see-also"></a>Consulte também  
 Para obter informações adicionais relacionadas, consulte os seguintes recursos.  
  
|Tipo de conteúdo|Referências|  
|------------------|----------------|  
|**Avaliação do produto**|-   [Pastas de trabalho](work-folders-overview.md)<br />-   [Pastas de trabalho para o Windows 7](http://blogs.technet.com/b/filecab/archive/2014/04/24/work-folders-for-windows-7.aspx) (postagem de blog)|  
|**Implantação**|-   [Projetando uma implementação de pastas de trabalho](plan-work-folders.md)<br />-   [Implantando pastas de trabalho](deploy-work-folders.md)<br />-   [Implantando pastas de trabalho com o AD FS e Proxy de aplicativo Web (WAP)](deploy-work-folders-adfs-overview.md)<br />- [Implantando pastas de trabalho com o Proxy de aplicativo do Azure AD](https://blogs.technet.microsoft.com/filecab/2017/05/31/enable-remote-access-to-work-folders-using-azure-active-directory-application-proxy/)<br />-   [Considerações sobre desempenho para implantações de pastas de trabalho](https://blogs.technet.com/b/filecab/archive/2013/11/01/performance-considerations-for-large-scale-work-folders-deployments.aspx)<br />-   [Pastas de trabalho para o Windows 7 (download de 64 bits)](https://www.microsoft.com/download/details.aspx?id=42558)<br />-   [Pastas de trabalho para o Windows 7 (download de 32 bits)](https://www.microsoft.com/download/details.aspx?id=42559)<br />-   [Implantação de laboratório de teste de pastas de trabalho](http://blogs.technet.com/b/filecab/archive/2013/07/10/work-folders-test-lab-deployment.aspx) (postagem de blog)|
