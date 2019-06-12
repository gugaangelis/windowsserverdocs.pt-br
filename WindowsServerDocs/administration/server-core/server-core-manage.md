---
title: Gerenciar o Server Core
description: Saiba como gerenciar uma instalação Server Core do Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: 761bfc681d7e39059884977cd99997ea9996268b
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811358"
---
# <a name="manage-a-server-core-server"></a>Gerenciar um servidor server Core
 
> Aplica-se a: Windows Server (canal semestral) e Windows Server 2016

Você pode gerenciar um servidor server Core das seguintes maneiras:
- Usando [Windows Admin Center](../../manage/windows-admin-center/overview.md)
- Usando o [ferramentas de administração de servidor remoto](../../remote/remote-server-administration-tools.md) em execução no Windows 10
- Local e remotamente usando o Windows PowerShell
- Remotamente usando [Gerenciador do servidor](../server-manager/server-manager.md)
- Remotamente usando um [snap-in do MMC](#managing-with-microsoft-management-console)
- Remotamente com [dos serviços de área de trabalho remota](#managing-with-remote-desktop-services)

Você também pode adicionar hardware e gerenciar drivers localmente, desde que você pode fazer isso na linha de comando.

Há algumas limitações importantes e dicas para ter em mente ao trabalhar com o Server Core:

- Se você fecha todas as janelas de prompt de comando e deseja abrir uma nova janela de Prompt de comando, você pode fazer isso no Gerenciador de tarefas. Pressione **CTRL\+ALT\+excluir**, clique em **iniciar Gerenciador de tarefas**, clique em **mais detalhes > arquivo > executar**e, em seguida, digite  **cmd.exe**. (Tipo **Powershell.exe** para abrir uma janela de comando do PowerShell.) Como alternativa, você pode inscrever-se e entre novamente.
- Nenhum comando ou ferramenta que tente iniciar o Windows Explorer vai funcionar. Por exemplo, executando **iniciar.** em um prompt de comando não funcionará.
- Não há nenhum suporte para renderização de HTML ou ajuda HTML no Server Core.
- Server Core dá suporte ao Windows Installer no modo silencioso para que você possa instalar ferramentas e utilitários de arquivos do Windows Installer. Ao instalar pacotes do Windows Installer no Server Core, use o **/qb** opção para exibir a interface do usuário básica.
- Para alterar o fuso horário, execute **Set-Date**.
- Para alterar as configurações internacionais, execute **control intl. cpl**.
- **Control.exe** não será executado seu próprio. Você deve executá-lo com um **timedate. cpl** ou **intl. cpl**.
- **Winver.exe** não está disponível em Server Core. Para obter informações de versão, use **Systeminfo.exe**.

## <a name="managing-server-core-with-windows-admin-center"></a>Gerenciando o Server Core com Windows Admin Center
O [Windows Admin Center](../../manage/windows-admin-center/overview.md) é um aplicativo de gerenciamento baseado em navegador que permite a administração no local de servidores do Windows com nenhuma dependência do Azure ou nuvem. O Windows Admin Center oferece controle total sobre todos os aspectos da sua infraestrutura de servidor e é útil para o gerenciamento de redes privadas que não estão conectadas à Internet. Você pode instalar o Windows Admin Center no Windows 10, em um servidor de gateway ou em uma instalação do Windows Server com experiência Desktop e, em seguida, conecte-se ao sistema de núcleo do servidor que você deseja gerenciar.

## <a name="managing-server-core-remotely-with-server-manager"></a>Gerenciando o Server Core remotamente com o Gerenciador do servidor

Gerenciador do servidor é um console de gerenciamento do Windows Server que ajuda você a provisionar e gerenciar servidores locais e remotos baseados no Windows de suas áreas de trabalho, sem exigir acesso físico aos servidores, nem a necessidade de habilitar o protocolo de área de trabalho remota (RDP) conexões em cada servidor. Gerenciador do servidor oferece suporte a gerenciamento de vários servidor remoto.

Para habilitar o seu servidor local seja gerenciado pelo Gerenciador do servidor em execução em um servidor remoto, execute o cmdlet do Windows PowerShell **SMRemoting.exe configurar – habilitar**.

## <a name="managing-with-microsoft-management-console"></a>Gerenciando com o Console de gerenciamento Microsoft

Você pode usar vários snap-ins para o Console de gerenciamento Microsoft (MMC) remotamente para gerenciar seu servidor server Core.

Para usar um snap-in do MMC para gerenciar um servidor server Core, que é um membro do domínio: 

1. Inicie um snap-in do MMC, como gerenciamento do computador.
2. Clique com botão direito o snap-in e, em seguida, clique em **conectar a outro computador**.
2. Digite o nome do computador do servidor Server Core e, em seguida, clique em **Okey**. Agora você pode usar o snap-in do MMC para gerenciar o servidor server Core, como você faria com qualquer outro computador ou servidor.

Usar um snap-in do MMC para gerenciar um servidor server Core que está *não* um membro do domínio: 

1. Estabeleça credenciais alternativas para usar para se conectar ao computador Server Core digitando o seguinte comando no prompt de comando no computador remoto:
1. 
   ```
   cmdkey /add:<ServerName> /user:<UserName> /pass:<password>
   ```

   Se você quiser ser solicitado a fornecer uma senha, omita a **/passar** opção.

2. Quando solicitado, digite a senha para o nome de usuário especificado.
   Se o firewall no servidor server Core já não estiver configurado para permitir que o snap-ins do MMC para se conectar, siga as etapas abaixo para configurar o Firewall do Windows para permitir o snap-in do MMC. Em seguida, continue na etapa 3.
3. Em um computador diferente, inicie um snap-in do MMC, como **gerenciamento do computador**.
4. No painel esquerdo, clique com botão direito o snap-in e, em seguida, clique em **conectar a outro computador**. (Por exemplo, o exemplo de gerenciamento do computador, você faria com o botão direito **gerenciamento do computador (Local)** .)
5. Na **outro computador**, digite o nome do computador do servidor Server Core e, em seguida, clique em **Okey**. Agora, você pode usar o snap-in do MMC para gerenciar o servidor do Server Core, como faria com qualquer outro computador executando um sistema operacional Windows Server.

### <a name="to-configure-windows-firewall-to-allow-mmc-snap-ins-to-connect"></a>Para configurar o Firewall do Windows para permitir a conexão do(s) snap-in(s) do MMC
Para permitir que todos os snap-ins do MMC conectar-se, execute o seguinte comando:

```
Enable-NetFirewallRule -DisplayGroup "Remote Administration"
```

Para permitir que apenas MMC snap-ins específicos para se conectar, execute o seguinte:
```
Enable-NetFirewallRule -DisplayGroup "<rulegroup>"
```

Em que *rulegroup* é um dos seguintes, dependendo de qual snap-in para o qual você deseja se conectar:

| Snap-in do MMC                            | Grupo de regras                                            |
|----------------------------------------|-------------------------------------------------------|
| Visualizador de Eventos                           | Gerenciamento Remoto do Log de Eventos                           |
| Serviços                               | Gerenciamento Remoto de Serviços                             |
| Pastas compartilhadas                         | Compartilhamento de Arquivo e Impressora                              |
| Agendador de Tarefas                         | Logs de desempenho e alertas, compartilhamento de arquivos e impressora |
| Gerenciamento de disco                        | Gerenciamento de Volumes Remoto                              |
| Firewall do Windows e segurança avançada | Gerenciamento Remoto do Firewall do Windows                    |


> [!NOTE] 
> Alguns snap-ins do MMC não tiver um grupo de regras correspondentes que lhes permite se conectar por meio do firewall. No entanto, a habilitação dos grupos de regras para Visualizador de Eventos, Serviços ou Pastas Compartilhadas permitirá que a maioria dos outros snap-ins se conectem. 
>
> Além disso, determinados snap-ins exigem configuração adicional para se conectarem através do Firewall do Windows:
>
> - Gerenciamento de Disco. Primeiro, você deve iniciar o VDS (Serviço de Disco Virtual) no computador Server Core. Você também deve configurar as regras de Gerenciamento de Disco adequadamente no computador que está executando o snap-in do MMC.
> - Monitor de segurança IP. Primeiro, você deve habilitar o gerenciamento remoto deste snap-in. Para fazer isso, no prompt de comando, digite **Cscript \windows\system32\scregedit.wsf /im 1**
> - Confiabilidade e Desempenho. O snap-in não requer nenhuma configuração adicional, mas, quando você usá-lo para monitorar um computador Server Core, só poderá monitorar os dados de desempenho. Dados de confiabilidade não estão disponíveis.

## <a name="managing-with-remote-desktop-services"></a>Gerenciamento de serviços de área de trabalho remota

Você pode usar [área de trabalho remota](../../remote/remote-desktop-services/welcome-to-rds.md) para gerenciar um servidor server Core de computadores remotos.

Antes de poder acessar o Server Core, você precisará executar o comando a seguir: 
```
cscript C:\Windows\System32\Scregedit.wsf /ar 0
```
Isso permite que a Área de Trabalho Remota do modo Administração aceite conexões.

## <a name="add-hardware-and-manage-drivers-locally"></a>Adicionar hardware e gerenciar drivers localmente

Para adicionar hardware para um servidor server Core, siga as instruções fornecidas pelo fornecedor de hardware para instalar o novo hardware. 

Se o hardware não for plug and play, você precisará instalar manualmente o driver. Para fazer isso, copie os arquivos de driver para um local temporário no servidor e, em seguida, execute o seguinte comando:

```
pnputil –i –a <driverinf>
```

Em que *driverinf* é o nome de arquivo do arquivo. inf do driver.

Se solicitado, reinicie o computador.

Para ver quais drivers são instalados, execute o seguinte comando: 

```
sc query type= driver
```

> [!NOTE] 
> Você deve incluir o espaço após o sinal de igual para o comando ser concluído com êxito.

Para desabilitar um driver de dispositivo, execute o seguinte:

```
sc delete <service_name>
```

Em que *service_name* é o nome do serviço que você obteve quando você executou **tipo de consulta sc = driver**.