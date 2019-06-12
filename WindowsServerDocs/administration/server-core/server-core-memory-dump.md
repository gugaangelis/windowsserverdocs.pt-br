---
title: Configurar arquivos de despejo de memória para a instalação do Server Core
description: Saiba como configurar arquivos de despejo de memória para uma instalação Server Core do Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: 235df6f681de51a12f82b9fad019dd2db45fd486
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435555"
---
# <a name="configure-memory-dump-files-for-server-core-installation"></a>Configurar arquivos de despejo de memória para a instalação do Server Core

>Aplica-se a: Windows Server (canal semestral) e Windows Server 2016

Use as etapas a seguir para configurar um despejo de memória para a instalação do Server Core. 

## <a name="step-1-disable-the-automatic-system-page-file-management"></a>Etapa 1: Desabilitar o gerenciamento de arquivo de página automática do sistema

A primeira etapa é configurar manualmente as opções de falha e recuperação do sistema. Isso é necessário para concluir as etapas restantes.

Execute o seguinte comando: 

```
wmic computersystem set AutomaticManagedPagefile=False
```
 
## <a name="step-2-configure-the-destination-path-for-a-memory-dump"></a>Etapa 2: Configure o caminho de destino para um despejo de memória

Você não precisa ter o arquivo de paginação na partição onde o sistema operacional está instalado. Para colocar o arquivo de paginação em outra partição, você deve criar uma nova entrada de registro denominada **DedicatedDumpFile**. Você pode definir o tamanho do arquivo de paginação usando o **DumpFileSize** entrada do registro. Para criar as entradas do registro DedicatedDumpFile e DumpFileSize, siga estas etapas: 

1. No prompt de comando, execute as **regedit** comando para abrir o Editor do registro.
2. Localize e clique na seguinte subchave do Registro: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl
3. Clique em **Editar > Novo > valor de cadeia de caracteres**.
4. Nomeie o novo valor **DedicatedDumpFile**, e pressione ENTER.
5. Clique com botão direito **DedicatedDumpFile**e, em seguida, clique em **modificar**.
6. Na **dados do valor** tipo  **\<unidade\>:\\\<Dedicateddumpfile.sys\>** e, em seguida, clique em **Okey**.

   >[!NOTE] 
   > Substitua \<unidade\> com uma unidade que tem suficiente disco de espaço do arquivo de paginação e substituir \<Dedicateddumpfile.dmp\> com o caminho completo para o arquivo dedicado.
 
7. Clique em **Editar > Novo > valor DWORD**.
8. Tipo de **DumpFileSize**, e pressione ENTER.
9. Clique com botão direito **DumpFileSize**e, em seguida, clique em **modificar**.
10. Na **Editar valor DWORD**, em **Base**, clique em **Decimal**.
11. Na **dados do valor**, digite o valor apropriado e, em seguida, clique em **Okey**.
    >[!NOTE]
    > É o tamanho do arquivo de despejo de memória em megabytes (MB).
12. Saia do Editor do registro.

Depois de determinar o local da partição do despejo de memória, configure o caminho de destino para o arquivo de paginação. Para exibir o caminho de destino atual para o arquivo de paginação, execute o seguinte comando:

```
wmic RECOVEROS get DebugFilePath
```

O destino padrão para **DebugFilePath** é % systemroot%\memory.dmp. Para alterar o caminho de destino atual, execute o seguinte comando:

```
wmic RECOVEROS set DebugFilePath = <FilePath>
```

Definir \<FilePath\> para o caminho de destino. Por exemplo, o comando a seguir define o caminho de destino de despejo de memória para C:\WINDOWS\MEMORY. DMP: 

```
wmic RECOVEROS set DebugFilePath = C:\WINDOWS\MEMORY.DMP
```
 
## <a name="step-3-set-the-type-of-memory-dump"></a>Etapa 3: Defina o tipo de despejo de memória

Determine o tipo de despejo de memória para configurar para seu servidor. Para exibir o tipo de despejo de memória atual, execute o seguinte comando:

```
wmic RECOVEROS get DebugInfoType
```

Para alterar o tipo de despejo de memória atual, execute o seguinte comando: 

```
wmic RECOVEROS set DebugInfoType = <Value>
```

\<Valor\> pode ser 0, 1, 2 ou 3, conforme definido abaixo.

- 0: Desabilite a remoção de um despejo de memória.
- 1: Despejo de memória completo. Registra todo o conteúdo da memória do sistema quando o computador é interrompido inesperadamente. Um despejo de memória completo pode conter dados de processos que estavam em execução quando o despejo de memória foi coletado.
- 2: Despejo de memória do kernel (padrão). Registra somente a memória do kernel. Isso acelera o processo de gravação de informações em um arquivo de log quando o computador é interrompido inesperadamente.
- 3: Despejo de memória pequeno. Registra o menor conjunto de informações úteis que podem ajudar a identificar por que o computador foi interrompido inesperadamente.

## <a name="step-4-configure-the-server-to-restart-automatically-after-generating-a-memory-dump"></a>Etapa 4: Configurar o servidor seja reiniciado automaticamente depois de gerar um despejo de memória

Por padrão, o servidor é reiniciado automaticamente depois que ele gera um despejo de memória. Para exibir a configuração atual, execute o seguinte comando:

```
wmic RECOVEROS get AutoReboot
```

Se o valor para **AutoReboot** for TRUE, o servidor será reiniciado automaticamente depois de gerar um despejo de memória. Nenhuma configuração é necessária e você pode prosseguir para a próxima etapa.

Se o valor para **AutoReboot** é FALSE, o servidor não será reiniciado automaticamente. Execute o seguinte comando para alterar o valor:

```
wmic RECOVEROS set AutoReboot = true
```
 
## <a name="step-5-configure-the-server-to-overwrite-the-existing-memory-dump-file"></a>Etapa 5: Configurar o servidor para substituir o arquivo de despejo de memória existente

Por padrão, o servidor substituirá o arquivo de despejo de memória existente quando um novo será criado. Para determinar se os arquivos de despejo de memória existente já estão configurados a serem substituídos, execute o seguinte comando:

```
wmic RECOVEROS get OverwriteExistingDebugFile
```

Se o valor for 1, o servidor substituirá o arquivo de despejo de memória existente. Nenhuma configuração é necessária e você pode prosseguir para a próxima etapa.

Se o valor for 0, o servidor não substituirá o arquivo de despejo de memória existente. Execute o seguinte comando para alterar o valor: 

```
wmic RECOVEROS set OverwriteExistingDebugFile = 1
```
 
## <a name="step-6-set-an-administrative-alert"></a>Etapa 6: Definir um alerta administrativo

Determinar se um alerta administrativo é apropriado e defina **SendAdminAlert** adequadamente. Para exibir o valor atual de SendAdminAlert, execute o seguinte comando:

```
wmic RECOVEROS get SendAdminAlert
```

Os valores possíveis para SendAdminAlert são TRUE ou FALSE. Para modificar o valor de SendAdminAlert existente como true, execute o seguinte comando: 

```
wmic RECOVEROS set SendAdminAlert = true
```
 
## <a name="step-7-set-the-memory-dumps-page-file-size"></a>Etapa 7: Defina o tamanho de arquivo de paginação do despejo de memória

Para verificar as configurações do arquivo de página atual, execute um dos seguintes comandos:

   ```
   wmic.exe pagefile
   ```

   ou

   ```
   wmic.exe pagefile list /format:list
   ```

Por exemplo, execute o seguinte comando para configurar os tamanhos iniciais e máximo de seu arquivo de paginação:

```
wmic pagefileset where name="c:\\pagefile.sys" set InitialSize=1000,MaximumSize=5000
```

## <a name="step-8-configure-the-server-to-generate-a-manual-memory-dump"></a>Etapa 8: Configurar o servidor para gerar um despejo de memória manual

Você pode gerar um despejo de memória manualmente usando um teclado PS/2. Este recurso está desabilitado por padrão, e ele não está disponível para teclados de barramento Serial Universal (USB).

Para habilitar a memória manual despejos de memória usando um teclado PS/2, execute o seguinte comando:

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\i8042prt\Parameters /v CrashOnCtrlScroll /t REG_DWORD /d 1 /f
```

Para determinar se o recurso foi habilitado corretamente, execute o seguinte comando:

```
Reg query HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ i8042prt \ Parameters / v CrashOnCtrlScroll
```

Você deve reiniciar o servidor para que as alterações entrem em vigor. Você pode reiniciar o servidor executando o seguinte comando:

```
Shutdown / r / t 0
```

Você pode gerar despejos de memória manual com um teclado PS/2 que está conectado ao seu servidor, mantendo a tecla CTRL direita enquanto pressiona a tecla SCROLL LOCK duas vezes. Isso torna o computador seleção com código de erro 0xE2 de bugs. Depois de reiniciar o servidor, um novo arquivo de despejo é exibida no caminho de destino estabelecida por você na etapa 2.

## <a name="step-9-verify-that-memory-dump-files-are-being-created-correctly"></a>Etapa 9: Verifique se que arquivos estão sendo criados corretamente de despejo de memória

Você pode usar o utilitário dumpchk.exe para verificar se os arquivos de despejo de memória estão sendo criados corretamente. O utilitário dumpchk.exe não está instalado com a opção de instalação Server Core, portanto, você precisará executá-lo a partir de um servidor com a experiência Desktop ou Windows 10. Além disso, as ferramentas de depuração para produtos do Windows devem ser instaladas.  

O utilitário dumpchk.exe permite transferir o arquivo de despejo de memória de sua instalação do Server Core do Windows Server 2008 para o outro computador usando a mídia de sua escolha.

> [!WARNING]
> Arquivos de paginação podem ser muito grandes, cuidadosamente considere a transferência de método e os recursos exigidos pelo método.
 

Referências adicionais

Para obter informações gerais sobre como usar arquivos de despejo de memória, consulte [opções de visão geral do arquivo de despejo de memória para o Windows](https://support.microsoft.com/help/254649/overview-of-memory-dump-file-options-for-windows).

Para obter mais informações sobre arquivos de despejo de memória dedicada, consulte [como usar o valor do registro DedicatedDeumpFile para superar as limitações de espaço na unidade do sistema ao capturar um despejo de memória do sistema](https://blogs.msdn.microsoft.com/ntdebugging/2010/04/02/how-to-use-the-dedicateddumpfile-registry-value-to-overcome-space-limitations-on-the-system-drive-when-capturing-a-system-memory-dump/).



