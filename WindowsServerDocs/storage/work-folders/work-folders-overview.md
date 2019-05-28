---
ms.assetid: c91c7196-ee0d-4856-8cfb-4c38494ccf1f
title: Visão geral de Pastas de Trabalho
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 6/11/2017
description: Uma visão geral de Pastas de Trabalho - uma função de servidor no Windows Server que fornece uma maneira consistente para os usuários acessarem arquivos de trabalho de computadores e dispositivos.
ms.openlocfilehash: e3e3c86efb2f781dc88cac6851ea22f19c6ae812
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222852"
---
# <a name="work-folders-overview"></a>Visão geral de Pastas de Trabalho

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10, Windows 8.1, Windows 7

Este tópico aborda as Pastas de Trabalho, um serviço de função para servidores de arquivos que executa o Windows Server e fornece uma maneira consistente para os usuários acessarem seus arquivos de trabalho nos computadores e nos dispositivos.  
  
Se você estiver procurando para baixar ou usar pastas de trabalho em um dispositivo Android ou iOS, Windows 7 ou Windows 10, consulte o seguinte:

- [Pastas de trabalho para Windows 10](https://support.microsoft.com/help/12370/windows-10-work-folders)
- [Pastas de trabalho para o Windows 7 (download de 64 bits)](https://www.microsoft.com/download/details.aspx?id=42558)
- [Pastas de trabalho para o Windows 7 (download de 32 bits)](https://www.microsoft.com/download/details.aspx?id=42559)
- [Pastas de trabalho para iOS](https://itunes.apple.com/app/work-folders/id950878067)
- [Pastas de trabalho para Android](https://play.google.com/store/apps/details?id=com.microsoft.workfolders)

## <a name="role-description"></a>Descrição da função  
 Com Pastas de trabalho, os usuários podem armazenar e acessar arquivos de trabalho em PCs e dispositivos, também conhecidos como BYOD (traga seu próprio dispositivo), além de PCs corporativos. Os usuários obtêm um local conveniente para armazenar arquivos de trabalho e acessá-los em qualquer lugar. As organizações mantêm controle sobre dados corporativos armazenando os arquivos em servidores de arquivos gerenciados centralmente e, opcionalmente, especificando políticas de dispositivo de usuário como criptografia e senhas de bloqueio de tela.  
  
 Pastas de Trabalho pode ser implantado com implantações existentes de Redirecionamento de Pasta, Arquivos Offline e pastas base. Pastas de Trabalho armazena arquivos do usuário em uma pasta no servidor chamada *compartilhamento de sincronização*. Você pode especificar uma pasta que já contém dados do usuário, o que permite a você adotar Pastas de Trabalho sem a migração de dados e servidores ou a finalização imediata da solução existente.  
  
## <a name="practical-applications"></a>Aplicações práticas  
 Os administradores podem usar Pastas de Trabalho para fornecer aos usuários acesso aos seus arquivos de trabalho, mantendo o armazenamento centralizado e o controle sobre os dados da organização. Estes são algumas aplicações específicas para Pastas de Trabalho:  
  
-   Fornecer um único ponto de acesso a arquivos de trabalho nos computadores e dispositivos pessoais e de trabalho de um usuário  
  
-   Acessar arquivos de trabalho enquanto estiver offline e, em seguida, sincronizar com o servidor de arquivos central quando o computador ou o dispositivo seguinte tiver conectividade de Internet ou de intranet  
  
-   Implantar com implantações existentes de Redirecionamento de Pasta, Arquivos Offline e pastas base  
  
-   Usar as tecnologias de gerenciamento de servidor de arquivos existentes, como classificação de arquivo e cotas de pasta, para gerenciar os dados do usuário  
  
-   Especificar políticas de segurança para instruir os computadores e os dispositivos do usuário a criptografar Pastas de Trabalho e usar uma senha de tela de bloqueio  
  
-   Usar o Clustering de Failover com Pastas de Trabalho para fornecer uma solução de alta disponibilidade  
  
## <a name="important-functionality"></a>Funcionalidade importante

 O serviço Pastas de Trabalho oferece a funcionalidade a seguir.  
  
|Funcionalidade|Disponibilidade|Descrição|  
|-------------------|------------------|-----------------|  
|Serviço de função Pastas de Trabalho no Gerenciador do Servidor|Windows Server 2012 R2 ou Windows Server 2016|Os Serviços de Arquivo e Armazenamento oferece uma maneira de configurar compartilhamentos de sincronização (pastas que armazenam arquivos de trabalho do usuário), monitora Pastas de Trabalho e gerencia compartilhamentos de sincronização e o acesso de usuários|  
|Cmdlets de Pastas de Trabalho|Windows Server 2012 R2 ou Windows Server 2016|Um módulo do Windows PowerShell que contém cmdlets abrangentes para gerenciar servidores de Pastas de Trabalho|  
|Integração de Pastas de Trabalho ao Windows|Windows 10<br /><br /> Windows 8.1<br /><br /> Windows RT 8.1<br /><br /> Windows 7 (é necessário download)|O serviço Pastas de Trabalho fornece a seguinte funcionalidade em computadores Windows:<br /><br /> -   Um item do Painel de Controle que configura e monitora Pastas de Trabalho<br />-   Integração ao Explorador de Arquivos que permite fácil acesso aos arquivos em Pastas de Trabalho<br />-   Um mecanismo de sincronização que transfere os arquivos de/para um servidor de arquivos central, maximizando, ao mesmo tempo, o desempenho do sistema e a duração da bateria|  
|Aplicativo Pastas de Trabalho para dispositivos|Android<br /><br /> Apple iPhone e iPad®|Um aplicativo que permite que dispositivos populares acessem arquivos em Pastas de Trabalho|  
  
## <a name="new-and-changed-functionality"></a>Funcionalidade nova e alterada
  
 A tabela a seguir descreve algumas das principais mudanças em Pastas de Trabalho.  
  
|Recurso/funcionalidade|Novo ou atualizado?|Descrição|  
|----------------------------|---------------------|-----------------|  
|Suporte do Proxy para aplicativo do Azure AD|Adicionado ao Windows 10 versão 1703, Android, iOS|Os usuários remotos com segurança podem acessar seus arquivos no servidor de pastas de trabalho usando o Proxy de aplicativo do Azure AD.|
|Replicação de alteração mais rápida|Atualizado no Windows 10 e no Windows Server 2016.|Para o Windows Server 2012 R2, quando as alterações do arquivo são sincronizadas com o servidor de pastas de trabalho, os clientes não são notificados sobre a alteração e aguardam até 10 minutos para receber a atualização. Ao usar o Windows Server 2016, o servidor de pastas de trabalho notifica imediatamente os clientes do Windows 10 e as alterações de arquivo são sincronizadas imediatamente. Essa funcionalidade é nova no Windows Server 2016 e requer um cliente do Windows 10. Se você estiver usando um cliente mais antigo ou o servidor de Pastas de Trabalho for o Windows Server 2012 R2, o cliente continuará sondando alterações a cada 10 minutos.|  
|Integrado a Proteção de Informações do Windows (WIP)|Adicionado ao Windows 10, versão 1607|Se um administrador implantar a WIP, Pastas de Trabalho pode impor a proteção de dados criptografando os dados no computador. A criptografia está usando uma chave associada à ID da Empresa, que pode ser apagada remotamente por meio de um pacote de gerenciamento de dispositivos móveis com suporte, como o Microsoft Intune.|  
|Integração do Microsoft Office|Adicionado ao Windows 10, versão 1511|No Windows 8.1, você pode navegar até Pastas de Trabalho nos aplicativos do Office, clicando ou tocando em Este PC e, em seguida, navegando até o local de Pastas de Trabalho no computador. No Windows 10, pode ser ainda mais fácil acessar Pastas de Trabalho; para isso, adicione o serviço à lista de locais exibida pelo Office ao salvar ou abrir arquivos. Para obter mais informações, consulte [Pastas de Trabalho no Windows 10](https://windows.microsoft.com/windows-10/work-folders-in-windows-10) e  [Solução de problemas sobre o uso de Pastas de Trabalho como um local no Microsoft Office](https://social.technet.microsoft.com/wiki/contents/articles/32881.troubleshooting-using-work-folders-as-a-place-in-microsoft-office.aspx).|  
  
## <a name="software-requirements"></a>Requisitos de software

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
  
-   Os computadores e dispositivos devem estar executando um dos seguintes sistemas operacionais:  
  
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
  
## <a name="work-folders-compared-to-other-sync-technologies"></a>Comparação de Pastas de Trabalho com outras tecnologias  

A tabela a seguir aborda como diversas tecnologias de sincronização da Microsoft são posicionadas e quando usar cada uma delas.  
  
||Pastas de trabalho|Arquivos Offline|OneDrive for Business|OneDrive|  
|-|------------------|-------------------|---------------------------|--------------|  
|**Resumo da tecnologia**|Sincroniza arquivos armazenados em um servidor de arquivos com computadores e dispositivos|Sincroniza arquivos armazenados em um servidor de arquivos com computadores que têm acesso à rede corporativa (pode ser substituído por Pastas de Trabalho)|Sincroniza arquivos armazenados no Office 365 ou no SharePoint com computadores e dispositivos dentro ou fora de uma rede corporativa e fornece funcionalidade de colaboração de documentos|Sincroniza arquivos pessoais armazenados no OneDrive com PCs, computadores Mac e dispositivos|  
|**O objetivo de fornecer acesso de usuário para arquivos de trabalho**|Sim|Sim|Sim|Não|  
|**Serviço de nuvem**|Nenhuma|Nenhuma|Office 365|Microsoft OneDrive|  
|**Servidores de rede interna**|Servidores de arquivos que executam o Windows Server 2012 R2 ou o Windows Server 2016|Servidores de arquivos|SharePoint Server (opcional)|Nenhuma|  
|**Clientes com suporte**|PCs, iOS, Android|PCs em uma rede corporativa ou conectado por meio do DirectAccess, VPNs ou outras tecnologias de acesso remoto|PCs, iOS, Android, Windows Phone|PCs, computadores Mac, Windows Phone, iOS, Android|  
  
> [!NOTE]
>  Além das tecnologias de sincronização listadas na tabela anterior, a Microsoft oferece outras tecnologias de replicação, incluindo Replicação do DFS, que é projetada para replicação de servidor a servidor, e BranchCache, que é projetado como uma tecnologia de aceleração WAN em filial. Para obter mais informações, consulte [Visão geral de namespaces do DFS e Replicação do DFS](https://technet.microsoft.com/library/jj127250(v=ws.11).aspx) e [Visão geral do BranchCache](https://technet.microsoft.com/library/hh831696(v=ws.11).aspx)  
  
## <a name="server-manager-information"></a>Informações sobre o Gerenciador do Servidor  

O serviço Pastas de trabalho faz parte da função Serviços de Arquivo e Armazenamento. Você pode instalar Pastas de Trabalho usando o Assistente de Adição de Funções e Recursos ou o cmdlet `Install-WindowsFeature`. Os dois métodos têm a seguinte função:  
  
-   Adiciona a página **Pastas de Trabalho** a **Serviços de Arquivo e Armazenamento** no Gerenciador do Servidor.  
  
-   Instala o serviço Compartilhamentos de Sincronização do Windows, que é usado pelo Windows Server para hospedar compartilhamentos de sincronização  
  
-   Instala o módulo SyncShare do Windows PowerShell para gerenciar Pastas de Trabalho no servidor  
  
## <a name="interoperability-with-windows-azure-virtual-machines"></a>Interoperabilidade com máquinas virtuais do Windows Azure

 Você pode executar esse serviço de função do Windows Server em uma máquina virtual no Windows Azure. Esse cenário foi testado com o Windows Server 2012 R2 e o Windows Server 2016.  
  
Para saber mais sobre como começar a usar máquinas virtuais do Windows Azure, acesse o [site do Windows Azure](http://www.windowsazure.com/documentation/services/virtual-machines).  
  
## <a name="see-also"></a>Consulte também  
 Para obter informações adicionais relacionadas, consulte os seguintes recursos.  
  
|Tipo de conteúdo|Referências|  
|------------------|----------------|  
|**Avaliação do produto**|-   [Pastas de trabalho para Android – liberados](https://blogs.technet.microsoft.com/filecab/2016/03/16/work-folders-for-android-released) (postagem de blog)<br />-   [Pastas de trabalho para iOS – versão de aplicativo do iPad](https://blogs.technet.com/b/filecab/archive/2015/01/16/work-folders-for-ios-ipad-app-release.aspx) (postagem de blog)<br />-   [Apresentando as pastas de trabalho no Windows Server 2012 R2](http://blogs.technet.com/b/filecab/archive/2013/07/09/introducing-work-folders-on-windows-server-2012-r2.aspx) (postagem de blog)<br />-   [Introdução às pastas de trabalho](http://channel9.msdn.com/posts/Introduction-to-Work-Folders) (vídeo do Channel 9)<br />-   [Implantação de laboratório de teste de pastas de trabalho](http://blogs.technet.com/b/filecab/archive/2013/07/10/work-folders-test-lab-deployment.aspx) (postagem de blog)<br />-   [Pastas de trabalho para o Windows 7](http://blogs.technet.com/b/filecab/archive/2014/04/24/work-folders-for-windows-7.aspx) (postagem de blog)|  
|**Implantação**|-   [Projetando uma implementação de pastas de trabalho](plan-work-folders.md)<br />-   [Implantando pastas de trabalho](deploy-work-folders.md)<br />-   [Implantando pastas de trabalho com o AD FS e Proxy de aplicativo Web (WAP)](deploy-work-folders-adfs-overview.md)<br />-   [Implantando pastas de trabalho com o Proxy de aplicativo do Azure AD](https://blogs.technet.microsoft.com/filecab/2017/05/31/enable-remote-access-to-work-folders-using-azure-active-directory-application-proxy/)<br />- [Arquivos offline (CSC) ao guia de migração de pastas de trabalho](https://blogs.technet.microsoft.com/filecab/2016/08/12/offline-files-csc-to-work-folders-migration-guide/)<br />-   [Considerações sobre desempenho para implantações de pastas de trabalho](https://blogs.technet.com/b/filecab/archive/2013/11/01/performance-considerations-for-large-scale-work-folders-deployments.aspx)<br />-   [Pastas de trabalho para o Windows 7 (download de 64 bits)](https://www.microsoft.com/download/details.aspx?id=42558)<br />-   [Pastas de trabalho para o Windows 7 (download de 32 bits)](https://www.microsoft.com/download/details.aspx?id=42559)|  
|**Operações**|-   [Aplicativo de iPad de pastas de trabalho: Perguntas frequentes sobre](https://windows.microsoft.com/windows/work-folders-ipad-faq) (para usuários)<br />-   [Gerenciamento de certificado de pastas de trabalho](https://blogs.technet.com/b/filecab/archive/2013/08/09/work-folders-certificate-management.aspx) (postagem de blog)<br />-   [Monitoramento de implantações de pastas de trabalho do Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/10/15/monitoring-windows-server-2012-r2-work-folders-deployments.aspx) (postagem de blog)<br />-   [Cmdlets de SyncShare (pastas de trabalho) no Windows PowerShell](https://docs.microsoft.com/powershell/module/syncshare/?view=win10-ps)<br />-   [Cartão de referência rápida de Cmdlets do PowerShell de serviços de arquivo e armazenamento para a edição do Windows Server 2012 R2 Preview](http://blogs.technet.com/b/filecab/archive/2013/07/30/storage-and-file-services-powershell-cmdlets-quick-reference-card-for-windows-server-2012-r2-preview-edition.aspx)|  
|**Solução de problemas**|-   [Windows Server 2012 R2 – porta Resolvendo conflito com sites do IIS e as pastas de trabalho](https://blogs.technet.com/b/filecab/archive/2013/10/15/windows-server-2012-r2-resolving-port-conflict-with-iis-websites-and-work-folders.aspx) (postagem de blog)<br />-   [Erros comuns em pastas de trabalho](https://social.technet.microsoft.com/wiki/contents/articles/30578.common-errors-in-work-folders.aspx)|  
|**Recursos da comunidade**|-   [Fórum de armazenamento e serviços de arquivo](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserverfiles)<br />-   [A equipe de armazenamento da Microsoft - Blog File Cabinet](http://blogs.technet.com/b/filecab/)<br />-   [Peça o blog da equipe de serviços de diretório](http://blogs.technet.com/b/askds/)|  
|**Tecnologias relacionadas**|-   [Armazenamento no Windows Server 2016](../storage.md)<br>-   [Serviços de arquivo e armazenamento](https://technet.microsoft.com/library/hh831487(v=ws.11).aspx)<br />-   [Gerenciador de recursos de servidor de arquivos](https://technet.microsoft.com/library/hh831701(v=ws.11).aspx)<br />-   [Redirecionamento de pasta, arquivos Offline e perfis de usuário móvel](https://technet.microsoft.com/library/hh848267(v=ws.11).aspx)<br />-   [BranchCache](https://technet.microsoft.com/library/hh831696(v=ws.11).aspx)<br />-   [Os Namespaces do DFS e replicação do DFS](https://technet.microsoft.com/library/jj127250(v=ws.11).aspx)|
