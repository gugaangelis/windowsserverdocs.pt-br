---
redirect_url: /windows-server/windows-server
ms.openlocfilehash: aa1bc1d94f91a2b9584f72398385575d22db33a9
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="windows-server-2016"></a>Windows Server 2016

Esta biblioteca fornece informações para os profissionais de TI avaliarem, planejarem, implantarem, protegerem e gerenciarem o Windows Server 2016 na empresa.

> [!Note] 
> A próxima versão do Windows Server está mudando! É possível encontrar detalhes sobre as novidades No Horizonte visitando [Visão geral do Canal Semi-anual do Windows Server](./get-started/semi-annual-channel-overview.md). 

[![WVídeo de visão geral do Windows Server 2016(media/front-page-video.png)](https://www.youtube.com/embed/V8oF0JpDzaM)

<table border="0" width="100%" align='center'>
  <tr style="text-align:center;">
    <td align='center' style="width:25%; border:0;">
      <a href="/windows-server/get-started/what-s-new-in-windows-server-2016">
        <img height=145 src="media/whats-new-highlight.png" alt="What's new icon" title="Novidades no Windows Server 16"/></a>
        <br/>Novidades
    </td>
    <td align='center' style="width:25%; border:0;">
      <a href="/windows-server/get-started/server-basics">
        <img height=145 src="media/1-getstarted.png" alt="get started icon" title="Introdução ao Windows Server 16" /></a>
      <br/>Introdução </td>
    <td align='center' style="width:25%; border:0;">
      <a href="/windows-server/administration/index">
        <img height=145 src="media/8-management.png" alt="administer icon" title="Administrar Windows Server" /></a>
      <br/>Administrar </td>
    <td align='center' style="width:25%; border:0;">
      <a href="/windows-server/failover-clustering/failover-clustering-overview">
        <img height=145 src="media/3-failover.png" alt="Failover clustering icon" title="Clustering de failover do Windows Server" /></a>
      <br/>Clustering de failover </td>
  </tr>
  <tr style="text-align:center;">
    <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/identity/identity-and-access">
        <img height=145 src="media/4-identity.png" alt="Identity and access icon" title="Acesso e identidade do Windows Server" /></a>
      <br>Identidade e Acesso </td>
    <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/networking/networking">
        <img height=145 src="media/6-networking.png" alt="Networking icon" title="Rede do Windows Server" />
        </a>
      <br/>Rede </td>
    <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/remote/index">
        <img height=145 src="media/remote.png" alt="remote icon" title="Acesso remoto e gerenciamento de servidor" />
        </a>
      <br/>Acesso remoto </td>
    <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/security/security-and-assurance">
        <img height=145 src="media/5-security.png" alt="Security icon" title="Segurança e garantia do Windows Server" />
      </a>
      <br/>Segurança e garantia </td>
  </tr>
  <tr style="text-align:center;">
    <td align='center' style="width:25%; border:0;">&nbsp;</td>
    <td align='center' style="width:25%; border:0;"><br>
      <a href="/windows-server/storage/storage">
        <img height=145 src="media/7-storage.png" alt="Storage icon" title="Armazenamento do Windows Server" />
      </a>
      <br/>Armazenamento </td>
   <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/virtualization/virtualization">
        <img height=145 src="media/virtualization.png" alt="virtualization icon" title="Virtualização do Windows Server" /></a>
      <br/>Virtualização </td>
    <td align='center' style="width:25%; border:0;">&nbsp; </td>
  </tr>
</table>

<br/>

> [!Note] 
> Para conhecer os novos recursos e as funcionalidades disponíveis no Windows Server 2016 em primeira mão, baixe uma versão de avaliação em [Avaliações do Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016). 


## <a name="windows-server-2016-editions"></a>Edições do Windows Server 2016

O Windows Server 2016 está disponível nas edições Standard, Datacenter e Essentials. O Windows Server 2016 Datacenter inclui direitos de virtualização ilimitados, além de novos recursos para criar um data center definido por software. O Windows Server 2016 Standard oferece recursos de classe corporativa com direitos de virtualização limitados. O Windows Server Essentials é um servidor ideal para a primeira conexão com a nuvem. Ele tem sua própria [documentação abrangente](http://go.microsoft.com/fwlink/?LinkID=827171): o conteúdo aqui se concentra nas edições Standard e Datacenter. A tabela a seguir resume brevemente as principais diferenças entre as edições Standard e Datacenter:

|Recurso|Datacenter|Standard|  
|-------------------|----------|-----------------------|  
|Principal funcionalidade do Windows Server| sim| sim|
|Contêineres do Hyper-V/OSEs|ilimitado|   2|
|Contêineres do Windows Server|ilimitado|   ilimitado|
|Serviço Guardião de Host| sim| sim|
|Opção de instalação do Nano Server| sim| sim|
|Recursos de armazenamento, incluindo Espaços de Armazenamento Diretos e Réplica de Armazenamento| sim| não|
|Máquinas virtuais blindadas| sim| não|
|Infraestrutura de rede definida por software (controlador de rede, balanceador de carga de software e gateway multilocatário)| sim| não|

Para saber mais, confira [Preços e licenciamento do Windows Server 2016](https://www.microsoft.com/en-us/cloud-platform/windows-server-pricing) e [Comparar recursos em versões do Windows Server](https://www.microsoft.com/en-us/cloud-platform/windows-server-comparison).

## <a name="installation-options"></a>Opções de instalação

As edições Standard e Datacenter oferecem três opções de instalação:

- **Server Core**: reduz o espaço necessário em disco, a superfície de ataque possível e, especialmente, os requisitos de manutenção. Essa é a opção **recomendada**a menos que você tenha uma necessidade específica para elementos de interface de usuário adicionais e ferramentas gráficas de gerenciamento.
- **Servidor com Experiência Desktop**: instala a interface de usuário padrão e todas as ferramentas, incluindo recursos de experiência do cliente que exigiam uma instalação separada no Windows Server 2012 R2. As funções e recursos do servidor são instalados com o Gerenciador de Servidores ou por outros métodos.
- **Nano Server**: é um sistema operacional de servidor administrado remotamente e otimizado para data centers e nuvens privadas. É semelhante ao Windows Server no modo Server Core, mas significativamente menor, não tem nenhum recurso de logon local e só oferece suporte a agentes, ferramentas e aplicativos de 64 bits. Ele ocupa bem menos espaço em disco, tem uma configuração consideravelmente mais rápida e exige muito menos atualizações e reinicializações que as outras opções.

>[!Note]
> Ao contrário de algumas versões anteriores do Windows Server, não é possível converter entre Server Core e Server com Experiência Desktop após a instalação. Por exemplo, se você instalar o Server Core e mais tarde decidir usar o Server com Experiência Desktop, deverá fazer uma nova instalação (e vice-versa).


Agora que você sabe qual opção de edição e instalação é a certa para você, clique abaixo para começar com o Windows Server 2016.
<br/>
<br/>

<table border="0" width="100%" align='center'>
  <tr style="text-align:center;">
    <td align='center' style="width:33%; border:0;">
      <a  href="/windows-server/get-started/getting-started-with-nano-server"> <img width="175" src="media/nano.png" alt="Icon representing Nano server" title="Nano Server - Peso mais leve" /><br/>Nano Server - <br/>Peso mais leve</a>
    </td>
    <td align='center' style="width:33%; border:0;"><a href="/windows-server/get-started/getting-started-with-server-core"> <img width="175" src="media/servercore.png" alt="Icon representing the Server Core installation" title="Server Core - Recomendado" /><br/>Server Core - <br/>Recomendações</a></td>
   <td align='center' style="width:33%; border:0;"><a href="/windows-server/get-started/getting-started-with-server-with-desktop-experience"><img width="175" src="media/desktop.png" alt="Icon representing the full desktop experience installation option for Windows Server" title="Experiência da área de trabalho - Experiência completa" /><br/>Experiência Desktop - <br/>Interface completa</a></td>
  </tr>
</table>

## <a name="windows-server-software-defined-datacenter-sddc"></a>Datacenter Definido por Software (SDDC) do Windows Server

As tecnologias de armazenamento virtualizado, rede, segurança e gerenciamento são os blocos de construção do Datacenter Definido por Software (SDDC) do Windows Server.
<br/>
<br/>

<table border="0" width="100%" align='center'>
  <tr style="text-align:center;">
    <td align='center' style="width:10%; border:0;"></td>
    <td align='center' style="width:50%; border:0;"><a href="/windows-server/sddc"><img width="400" src="media/sddc/WS16-heading.png" alt="Icon representing SDDC" title="Datacenter Definido por Software (SDDC) do Windows Server" /><br/>Datacenter Definido por Software (SDDC) do Windows Server</a></td>
    <td align='center' style="width:10%; border:0;"></td>
  </tr>
</table>

Não consegue encontrar o conteúdo que você precisa? Usuários do Windows 10, nos digam o que vocês desejam em [Hub de Feedback](feedback-hub://?referrer=techDocsUcPage&tabid=2&contextid=898&newFeedback=true&topic=Windows-Server-2016.md). 
