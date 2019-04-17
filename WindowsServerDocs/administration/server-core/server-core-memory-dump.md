---
title: Configurar os arquivos de despejo de memória para instalação do núcleo do servidor
description: Saiba como configurar os arquivos de despejo de memória para uma instalação Server Core do Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: bd22378ec7ce5a1ff4e39546246e6e85ca859c45
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1447937"
---
# <a name="configure-memory-dump-files-for-server-core-installation"></a>Configurar os arquivos de despejo de memória para instalação do núcleo do servidor

>Aplicável a: Windows Server (canal semestral) e Windows Server 2016

Use as seguintes etapas para configurar um despejo de memória para sua instalação do núcleo do servidor. 

## <a name="step-1-disable-the-automatic-system-page-file-management"></a>Etapa 1: Desabilitar o gerenciamento de arquivos de página automática do sistema

A primeira etapa é configurar manualmente suas opções de falha e recuperação do sistema. Isso é necessário para concluir as etapas restantes.

Execute o seguinte comando: 

```
wmic computersystem set AutomaticManagedPagefile=False
```
 
## <a name="step-2-configure-the-destination-path-for-a-memory-dump"></a>Etapa 2: Configurar o caminho de destino para um despejo de memória

Você não precisa ter o arquivo de página na partição em que o sistema operacional está instalado. Para colocar o arquivo de paginação em outra partição, você deve criar uma nova entrada de registro denominada **DedicatedDumpFile**. Você pode definir o tamanho do arquivo de paginação usando a entrada de registro **DumpFileSize** . Para criar as entradas de registro DedicatedDumpFile e DumpFileSize, siga estas etapas: 

1. No prompt de comando, execute o comando **regedit** para abrir o Editor do registro.
2. Localize e clique na seguinte subchave do registro: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl
3. Clique em **Editar > Novo > valor de cadeia de caracteres**.
4. Nome do novo valor **DedicatedDumpFile**e pressione ENTER.
5. Clique com o botão **DedicatedDumpFile**e, em seguida, clique em **Modificar**.
6. Em tipo de **dados do valor** **\ < Drive\ >: \ \ \ < Dedicateddumpfile.sys\ >** e clique em **Okey**.

   >[!NOTE] 
   > Substituir \ < Drive\ > com uma unidade que tem suficiente em disco de espaço do arquivo de paginação e substituir \ < Dedicateddumpfile.dmp\ > com o caminho completo para o arquivo dedicado.
 
7. Clique em **Editar > Novo > valor DWORD**.
8. Digite **DumpFileSize**e pressione ENTER.
9. Clique com o botão **DumpFileSize**e, em seguida, clique em **Modificar**.
10. Em **Editar valor DWORD**, em **Base**, clique em **Decimal**.
11. Nos **dados do valor**, digite o valor apropriado e clique em **Okey**.
   >[!NOTE]
   > O tamanho do arquivo de despejo é em megabytes (MB).
12. Saia do Editor do registro.

Depois de determinar o local de partição do despejo de memória, configure o caminho de destino para o arquivo de página. Para exibir o caminho de destino atual para o arquivo de página, execute o seguinte comando:

```
wmic RECOVEROS get DebugFilePath
```

O destino padrão para **DebugFilePath** é systemroot%\memory.dmp %. Para alterar o caminho de destino atual, execute o seguinte comando:

```
wmic RECOVEROS set DebugFilePath = <FilePath>
```

Definir \ < FilePath\ > o caminho de destino. Por exemplo, o comando a seguir define o caminho de destino de despejo de memória para C:\WINDOWS\MEMORY. DMP: 

```
wmic RECOVEROS set DebugFilePath = C:\WINDOWS\MEMORY.DMP
```
 
## <a name="step-3-set-the-type-of-memory-dump"></a>Etapa 3: Definir o tipo de despejo de memória

Determine o tipo de despejo de memória para configurar seu servidor. Para exibir o tipo de despejo de memória atual, execute o seguinte comando:

```
wmic RECOVEROS get DebugInfoType
```

Para alterar o tipo de despejo de memória atual, execute o seguinte comando: 

```
wmic RECOVEROS set DebugInfoType = <Value>
```

\ < Value\ > podem ser 0, 1, 2 ou 3, conforme definido abaixo.

- 0: desabilitar a remoção de um despejo de memória.
- 1: despejo de memória completo. Registra todo o conteúdo da memória do sistema quando seu computador foi interrompido inesperadamente. Um despejo de memória completo pode conter dados de processos que estavam em execução quando o despejo de memória foi coletado.
- 2: despejo de memória do kernel (padrão). Registra somente a memória do kernel. Isso acelera o processo de gravação de informações em um arquivo de log quando o computador foi interrompido inesperadamente.
- 3: despejo de memória pequeno. Registra o menor conjunto de informações úteis que podem ajudar a identificar a causa da parada inesperada do computador.

## <a name="step-4-configure-the-server-to-restart-automatically-after-generating-a-memory-dump"></a>Etapa 4: Configurar o servidor para reiniciar automaticamente após gerar um despejo de memória

Por padrão, o servidor for reiniciado automaticamente após ele gera um despejo de memória. Para exibir a configuração atual, execute o seguinte comando:

```
wmic RECOVEROS get AutoReboot
```

Se o valor **AutoReboot** for TRUE, o servidor será reiniciado automaticamente após gerar um despejo de memória. Nenhuma configuração é necessária e você poderá prosseguir para a próxima etapa.

Se o valor **AutoReboot** for FALSE, o servidor não será reiniciado automaticamente. Execute o seguinte comando para alterar o valor:

```
wmic RECOVEROS set AutoReboot = true
```
 
## <a name="step-5-configure-the-server-to-overwrite-the-existing-memory-dump-file"></a>Etapa 5: Configurar o servidor para substituir o arquivo de despejo de memória existente

Por padrão, o servidor substitui o arquivo de despejo de memória existente quando um novo um é criado. Para determinar se os arquivos de despejo de memória existente já estiverem configurados para ser substituído, execute o seguinte comando:

```
wmic RECOVEROS get OverwriteExistingDebugFile
```

Se o valor for 1, o servidor substituirá o arquivo de despejo de memória existente. Nenhuma configuração será necessária, e você poderá prosseguir para a próxima etapa.

Se o valor for 0, o servidor não substitua o arquivo de despejo de memória existente. Execute o seguinte comando para alterar o valor: 

```
wmic RECOVEROS set OverwriteExistingDebugFile = 1
```
 
## <a name="step-6-set-an-administrative-alert"></a>Etapa 6: Configurar um alerta administrativo

Determinar se um alerta administrativo é apropriado e defina **SendAdminAlert** adequadamente. Para exibir o valor atual de SendAdminAlert, execute o seguinte comando:

```
wmic RECOVEROS get SendAdminAlert
```

Os valores possíveis para SendAdminAlert são TRUE ou FALSE. Para modificar o valor de SendAdminAlert existente como true, execute o seguinte comando: 

```
wmic RECOVEROS set SendAdminAlert = true
```
 
## <a name="step-7-set-the-memory-dumps-page-file-size"></a>Etapa 7: Definir o tamanho do arquivo de página do despejo de memória

Para verificar as configurações do arquivo de página atual, execute um dos seguintes comandos:

   ```
   wmic.exe pagefile
   ```

   ou

   ```
   wmic.exe pagefile list /format:list
   ```

Por exemplo, execute o seguinte comando para configurar os tamanhos iniciais e máximo do seu arquivo de página:

```
wmic pagefileset where name="c:\\pagefile.sys" set InitialSize=1000,MaximumSize=5000
```

## <a name="step-8-configure-the-server-to-generate-a-manual-memory-dump"></a>Etapa 8: Configurar o servidor para gerar um despejo de memória manual

Manualmente, você poderá gerar um despejo de memória, usando um teclado PS/2. Esse recurso é desabilitado por padrão, e ele não está disponível para teclados Universal USB (Barramento Serial).

Para ativar a memória manual descarta usando um teclado PS/2, execute o seguinte comando:

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\i8042prt\Parameters /v CrashOnCtrlScroll /t REG_DWORD /d 1 /f
```

Para determinar se o recurso tiver sido habilitado corretamente, execute o seguinte comando:

```
Reg query HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ i8042prt \ Parameters / v CrashOnCtrlScroll
```

Você deve reiniciar o servidor para que as alterações entrem em vigor. Você pode reiniciar o servidor executando o seguinte comando:

```
Shutdown / r / t 0
```

Você pode gerar despejos de memória manual com um teclado PS/2 que está conectado ao seu servidor, mantenha pressionada a tecla CTRL direita ao pressionar a tecla SCROLL LOCK duas vezes. Isso faz com que o computador do bug seleção com o código de erro 0xE2. Depois que você reiniciar o servidor, um novo arquivo de despejo aparece no caminho de destino que você estabeleceu na etapa 2.

## <a name="step-9-verify-that-memory-dump-files-are-being-created-correctly"></a>Etapa 9: Verificar arquivos estão sendo criados corretamente que despejo de memória

Você pode usar o utilitário dumpchk.exe para verificar se os arquivos de despejo de memória estão sendo criados corretamente. O utilitário de dumpchk.exe não está instalado com a opção de instalação Server Core, assim você terá para executá-lo a partir de um servidor com a experiência de área de trabalho ou Windows 10. Além disso, as ferramentas de depuração para os produtos do Windows devem ser instaladas.  

O utilitário dumpchk.exe lhe permite transferir o arquivo de despejo de memória da sua instalação Server Core do Windows Server 2008 para o outro computador usando a mídia de sua escolha.

> [!WARNING]
> Arquivos de página podem ser muito grandes, cuidadosamente considerar a transferência método e os recursos que o método requer.
 

Referências adicionais

Para obter informações gerais sobre como usar os arquivos de despejo de memória, consulte [Visão geral das opções de arquivo de despejo de memória para Windows](https://support.microsoft.com/help/254649/overview-of-memory-dump-file-options-for-windows).

Para obter mais informações sobre arquivos de despejo dedicado, consulte [como usar o valor de registro DedicatedDeumpFile para superar as limitações de espaço na unidade do sistema durante a captura de um despejo de memória do sistema](https://blogs.msdn.microsoft.com/ntdebugging/2010/04/02/how-to-use-the-dedicateddumpfile-registry-value-to-overcome-space-limitations-on-the-system-drive-when-capturing-a-system-memory-dump/).



