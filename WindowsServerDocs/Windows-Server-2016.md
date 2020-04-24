---
redirect_url: /windows-server/windows-server
ms.openlocfilehash: 9ef5565af748b4dd592e71ec4bd34a2be58003d9
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "75947292"
---
# <a name="windows-server-2016"></a>Windows Server 2016

Esta biblioteca fornece informações para os profissionais de TI avaliarem, planejarem, implantarem, protegerem e gerenciarem o Windows Server 2016.

> [!Note] 
> A próxima versão do Windows Server está mudando. É possível encontrar detalhes sobre as novidades no horizonte acessando a [Visão geral do Canal Semestral do Windows Server](./get-started/semi-annual-channel-overview.md). 

[![Vídeo de visão geral do Windows Server 2016](media/front-page-video.png)](https://www.youtube-nocookie.com/embed/V8oF0JpDzaM)

<table border="0" width="100%" align='center'>
  <tr style="text-align:center;">
    <td align='center' style="width:25%; border:0;">
      <a href="/windows-server/get-started/what-s-new-in-windows-server-2016"> &lt;img height=145 src=&quot;media/whats-new-highlight.png&quot; alt=&quot;Ícone de Novidades&quot; title=&quot;Novidades no Windows Server 16?&quot;/&gt;</a>
        <br/>Quais são as novidades?
    </td>
    <td align='center' style="width:25%; border:0;">
      <a href="/windows-server/get-started/server-basics"> &lt;img height=145 src=&quot;media/1-getstarted.png&quot; alt=&quot;ícone de introdução&quot; title=&quot;Introdução ao Windows Server 16&quot; /&gt;</a>
      <br/>Introdução </td>
    <td align='center' style="width:25%; border:0;">
      <a href="/windows-server/administration/index"> &lt;img height=145 src=&quot;media/8-management.png&quot; alt=&quot;ícone de administrar&quot; title=&quot;Administrar o Windows Server&quot; /&gt;</a>
      <br/>Administrar </td>
    <td align='center' style="width:25%; border:0;">
      <a href="/windows-server/failover-clustering/failover-clustering-overview"> &lt;img height=145 src=&quot;media/3-failover.png&quot; alt=&quot;Failover ícone de clustering&quot; title=&quot;Clustering de Failover do Windows Server&quot; /&gt;</a>
      <br/>Clustering de failover </td>
  </tr>
  <tr style="text-align:center;">
    <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/identity/identity-and-access"> &lt;img height=145 src=&quot;media/4-identity.png&quot; alt=&quot;ícone de Identidade e acesso&quot; title=&quot;Identidade e Acesso do Windows Server&quot; /&gt;</a>
      <br>Identidade e acesso </td>
    <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/networking/networking"> &lt;img height=145 src=&quot;media/6-networking.png&quot; alt=&quot;ícone de Rede&quot; title=&quot;Rede do Windows Server&quot; /&gt; </a>
      <br/>Rede </td>
    <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/remote/index"> &lt;img height=145 src=&quot;media/remote.png&quot; alt=&quot;ícone remoto&quot; title=&quot;Gerenciamento de servidor e acesso remoto&quot; /&gt; </a>
      <br/>Acesso remoto </td>
    <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/security/security-and-assurance"> &lt;img height=145 src=&quot;media/5-security.png&quot; alt=&quot;ícone de Segurança&quot; title=&quot;Segurança e Garantia do Windows Server&quot; /&gt; </a>
      <br/>Segurança e garantia </td>
  </tr>
  <tr style="text-align:center;">
    <td align='center' style="width:25%; border:0;">&nbsp;</td>
    <td align='center' style="width:25%; border:0;"><br>
      <a href="/windows-server/storage/storage"> &lt;img height=145 src=&quot;media/7-storage.png&quot; alt=&quot;ícone de Armazenamento&quot; title=&quot;Armazenamento do Windows Server&quot; /&gt; </a>
      <br/>Armazenamento </td>
   <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/virtualization/virtualization"> &lt;img height=145 src=&quot;media/virtualization.png&quot; alt=&quot;ícone de virtualização&quot; title=&quot;Virtualização do Windows Server&quot; /&gt;</a>
      <br/>Virtualização </td>
    <td align='center' style="width:25%; border:0;">[https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/](&nbsp;) </td>
  </tr>
</table>

<br/>

> [!Note] 
> Para conhecer os novos recursos e funcionalidades disponíveis no Windows Server 2016 em primeira mão, baixe uma versão de avaliação em [Avaliações do Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016). 


## <a name="windows-server-2016-editions"></a>Edições do Windows Server 2016

O Windows Server 2016 está disponível nas edições Standard, Datacenter e Essentials. O Windows Server 2016 Datacenter inclui direitos de virtualização ilimitados, além de novos recursos para criar um data center definido por software. O Windows Server 2016 Standard oferece recursos de classe corporativa com direitos de virtualização limitados. O Windows Server Essentials é um servidor ideal para a primeira conexão com a nuvem. Ele tem sua própria [documentação abrangente](https://go.microsoft.com/fwlink/?LinkID=827171): o conteúdo aqui se concentra nas edições Standard e Datacenter. A tabela a seguir resume brevemente as principais diferenças entre as edições Standard e Datacenter:

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

Para saber mais, confira [Preços e licenciamento do Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server-pricing) e [Comparar recursos em versões do Windows Server](https://www.microsoft.com/cloud-platform/windows-server-comparison).

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
      <a  href="/windows-server/get-started/getting-started-with-nano-server"> <img width="175" src="media/nano.png" alt="Icon representing Nano server" title="Nano Server – Peso mais leve" /><br/>Nano Server – <br/>Peso mais leve</a>
    </td>
    <td align='center' style="width:33%; border:0;"><a href="/windows-server/get-started/getting-started-with-server-core"> <img width="175" src="media/servercore.png" alt="Icon representing the Server Core installation" title="Server Core – Recomendado" /><br/>Server Core – <br/>Recomendado</a></td>
   <td align='center' style="width:33%; border:0;"><a href="/windows-server/get-started/getting-started-with-server-with-desktop-experience"><img width="175" src="media/desktop.png" alt="Icon representing the full desktop experience installation option for Windows Server" title="Experiência Desktop – Experiência Completa" /><br/>Experiência Desktop – <br/>Interface completa</a></td>
  </tr>
</table>

## <a name="windows-server-software-defined-datacenter-sddc"></a>SDDC (Datacenter Definido por Software do Windows Server)

As tecnologias de armazenamento virtualizado, rede, segurança e gerenciamento são as bases do SDDC (Datacenter Definido por Software) do Windows Server.
<br/>
<br/>

<table border="0" width="100%" align='center'>
  <tr style="text-align:center;">
    <td align='center' style="width:10%; border:0;"></td>
    <td align='center' style="width:50%; border:0;"><a href="/windows-server/sddc"><img width="400" src="media/sddc/WS16-heading.png" alt="Icon representing SDDC" title="SDDC (Datacenter Definido por Software do Windows Server)" /><br/>SDDC (Datacenter Definido por Software do Windows Server)</a></td>
    <td align='center' style="width:10%; border:0;"></td>
  </tr>
</table>
