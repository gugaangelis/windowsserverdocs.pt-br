---
title: Comandos netsh para MBN (rede de banda larga móvel)
description: Use o netsh mbn para consultar e definir configurações e parâmetros de banda larga móvel.
ms.topic: article
author: apdutta
ms.author: apdutta
ms.date: 02/20/2020
ms.openlocfilehash: 926e28cff1815e5f6a82185ad78a3825ed188def
ms.sourcegitcommit: ad2c13b09044710bf14236308ade8d74877c3e0d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/30/2020
ms.locfileid: "91591156"
---
# <a name="netsh-mbn-commands"></a>Comandos netsh mbn


Use o **netsh mbn** para consultar e definir configurações e parâmetros de banda larga móvel.

> [!TIP]
> Você pode obter ajuda sobre o comando netsh mbn usando
>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;netsh mbn /?

Os comandos netsh mbn disponíveis são:

- [add](#add)
- [connect](#connect)
- [delete](#delete)
- [disconnect](#disconnect)
- [diagnose](#diagnose)
- [dump](#dump)
- [help](#help)
- [set](#set)
- [show](#show)
- [teste](#test)

## <a name="add"></a>adicionar

Adiciona uma entrada de configuração a uma tabela.

Os comandos netsh mbn add disponíveis são:

- [dmprofile](#dmprofile)
- [profile](#profile)

### <a name="dmprofile"></a>dmprofile

Adiciona um perfil de Configuração DM ao Armazenamento de Dados de Perfil.

**Sintaxe**

```powershell
add dmprofile [interface=]<string> [name=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |
| **name**      | Nome do arquivo XML do perfil. É o nome do arquivo XML que contém os dados do perfil.     | Necessária |


**Exemplo**

```powershell
add dmprofile interface="Cellular" name="Profile1.xml"
```


### <a name="profile"></a>perfil

Adiciona um perfil de rede ao Armazenamento de Dados do Perfil.

**Sintaxe**

```powershell
add profile [interface=]<string> [name=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |
| **name**      | Nome do arquivo XML do perfil. É o nome do arquivo XML que contém os dados do perfil.     | Necessária |


**Exemplo**

```powershell
add profile interface="Cellular" name="Profile1.xml"
```


## <a name="connect"></a>connect

Conecta-se a uma rede de Banda Larga Móvel.

**Sintaxe**

```powershell
connect [interface=]<string> [connmode=]tmp|name [name=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |
| **connmode**  | Especifica como os parâmetros de conexão estão sendo fornecidos. A conexão pode ser solicitada usando um XML de perfil ou usando o nome do perfil para o XML de perfil armazenado anteriormente no Armazenamento de Dados de Perfil de Banda Larga Móvel usando o comando "netsh mbn add profile". No primeiro caso, o parâmetro connmode deve conter "tmp" como valor. Enquanto isso, deve ser "name" no último caso                                       | Necessária |
| **name**      | Nome do arquivo XML do perfil. É o nome do arquivo XML que contém os dados do perfil.     | Necessária |


**Exemplos**

```powershell
connect interface="Cellular" connmode=tmp name=d:\profile.xml
connect interface="Cellular" connmode=name name=MyProfileName
```


## <a name="delete"></a>excluir

Exclui uma entrada de configuração de uma tabela.

Os comandos netsh mbn delete disponíveis são:

- [dmprofile](#dmprofile)
- [profile](#profile)

### <a name="dmprofile"></a>dmprofile

Exclui um perfil de Configuração DM do Armazenamento de Dados de Perfil.

**Sintaxe**

```powershell
delete dmprofile [interface=]<string> [name=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |
| **name**      | Nome do arquivo XML do perfil. É o nome do arquivo XML que contém os dados do perfil.     | Necessária |

**Exemplo**

```powershell
delete dmprofile interface="Cellular" name="myProfileName"
```

### <a name="profile"></a>perfil

Exclui um perfil de rede do Armazenamento de Dados de Perfil.

**Sintaxe**

```powershell
delete profile [interface=]<string> [name=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |
| **name**      | Nome do arquivo XML do perfil. É o nome do arquivo XML que contém os dados do perfil.     | Necessária |


**Exemplo**

```powershell
delete profile interface="Cellular" name="myProfileName"
```

## <a name="diagnose"></a>diagnose

Executa o diagnóstico para problemas básicos de celular.

**Sintaxe**

```powershell
diagnose [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
diagnose interface="Cellular"
```


## <a name="disconnect"></a>desconectar

Desconecta-se de uma rede de Banda Larga Móvel.

**Sintaxe**

```powershell
disconnect [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
disconnect interface="Cellular"
```


## <a name="dump"></a>dump

Exibe um script de configuração.

Cria um script que contém a configuração atual.  Se for salvo em um arquivo, esse script poderá ser usado para restaurar as definições de configuração alteradas.

**Sintaxe**

```powershell
dump
```

## <a name="help"></a>ajuda

Exibe uma lista de comandos.

**Sintaxe**

```powershell
help
```

## <a name="set"></a>set

Define as informações de configuração.

Os comandos netsh mbn set disponíveis são:

- [acstate](#acstate)
- [dataenablement](#dataenablement)
- [dataroamcontrol](#dataroamcontrol)
- [enterpriseapnparams](#enterpriseapnparams)
- [highestconncategory](#highestconncategory)
- [powerstate](#powerstate)
- [profileparameter](#profileparameter)
- [slotmapping](#slotmapping)
- [tracing](#tracing)

### <a name="acstate"></a>acstate

Define o estado de conexão automática de dados de Banda Larga Móvel para a interface fornecida.

**Sintaxe**

```powershell
set acstate [interface=]<string> [state=]autooff|autoon|manualoff|manualon
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |
| **name**      | O estado de conexão automática a ser definido. Um dos seguintes valores:<br>autooff: token de conexão automática desativado.<br>autoon: token de conexão automática ativado.<br>manualoff: token de conexão manual desativado.<br>manualon: token de conexão manual ativado. | Necessária |


**Exemplo**

```powershell
set acstate interface="Cellular" state=autoon
```


### <a name="dataenablement"></a>dataenablement

Ativa ou desativa os dados de Banda Larga Móvel para o conjunto de perfis e a interface especificados.

**Sintaxe**

```powershell
set dataenablement [interface=]<string> [profileset=]internet|mms|all [mode=]yes|no
```

**Parameters**

|                |                                                                                               |          |
|----------------|-----------------------------------------------------------------------------------------------|----------|
| **interface**  | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |
| **profileset** | Nome do conjunto de perfis.                                                                      | Necessária |
| **mode**       | Um dos seguintes valores:<br>yes: habilita o conjunto de perfis alvo.<br>não: desabilita o conjunto de perfis alvo.| Necessária |


**Exemplo**

```powershell
set dataenablement interface="Cellular" profileset=mms mode=yes
```


### <a name="dataroamcontrol"></a>dataroamcontrol

Define o estado do controle de roaming de Dados de Banda Larga Móvel para o conjunto de perfis e a interface especificados.

**Sintaxe**

```powershell
set dataroamcontrol [interface=]<string> [profileset=]internet|mms|all [state=]none|partner|all
```

**Parameters**

|                |                                                                                               |          |
|----------------|-----------------------------------------------------------------------------------------------|----------|
| **interface**  | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |
| **profileset** | Nome do conjunto de perfis.                                                                      | Necessária |
| **mode**       | Um dos seguintes valores:<br>none: somente operadora residencial.<br>partner: somente operadoras residencial e de parceiro.<br>all: operadoras residencial, do parceiro e de roaming.| Necessária |


**Exemplo**

```powershell
set dataroamcontrol interface="Cellular" profileset=mms mode=partner
```


### <a name="enterpriseapnparams"></a>enterpriseapnparams

Define os parâmetros enterpriseAPN de dados de Banda Larga Móvel para a interface fornecida.

**Sintaxe**

```powershell
set enterpriseapnparams [interface=]<string> [allowusercontrol=]yes|no|nc [allowuserview=]yes|no|nc [profileaction=]add|delete|modify|nc
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |
| **allowusercontrol** | Um dos seguintes valores:<br>yes: permitir ao usuário final controlar enterpriseAPN.<br>no: não permitir ao usuário final controlar enterpriseAPN.<br>nc: sem alteração. | Necessária |
| **allowuserview** |Um dos seguintes valores:<br>yes: permitir ao usuário final exibir enterpriseAPN.<br>no: não permitir ao usuário final exibir enterpriseAPN.<br>nc: sem alteração. | Necessária |
| **profileaction** | Um dos seguintes valores:<br>add: um perfil enterpriseAPN é apenas adicionado.<br>delete: um perfil enterpriseAPN é apenas excluído.<br>modify: um perfil enterpriseAPN é apenas modificado.<br>nc: sem alteração. | Necessária |


**Exemplo**

```powershell
set enterpriseapnparams interface="Cellular" profileset=mms mode=yes
```


### <a name="highestconncategory"></a>highestconncategory

Define a categoria de conexão mais alta de Dados de Banda Larga Móvel para a interface fornecida.

**Sintaxe**

```powershell
set highestconncategory [interface=]<string> [highestcc=]admim|user|operator|device
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |
| **highestcc** | Um dos seguintes valores:<br>admin: perfis provisionados pelo administrador.<br>user: perfis provisionados pelo usuário.<br>operator: perfis provisionados pelo operador.<br>device: perfis provisionados pelo dispositivo. | Necessária |


**Exemplo**

```powershell
set highestconncategory interface="Cellular" highestcc=operator
```


### <a name="powerstate"></a>powerstate

Ativa ou desativa o rádio de Banda Larga Móvel para a interface fornecida.

**Sintaxe**

```powershell
set powerstate [interface=]<string> [state=]on|off
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |
| **state**      | Um dos seguintes valores:<br>on: ligar o transceptor de rádio.<br>off: desligar o transceptor de rádio. | Necessária |


**Exemplo**

```powershell
set powerstate interface="Cellular" state=on
```


### <a name="profileparameter"></a>profileparameter

Definir parâmetros em um Perfil de Rede de Banda Larga Móvel.

**Sintaxe**

```powershell
set profileparameter [name=]<string> [[interface=]<string>] [[cost]=default|unrestricted|fixed|variable]
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **name**      | Nome do perfil a ser modificado. Se a interface for especificada, somente o perfil nessa interface será modificado. | Necessária |
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Opcional |
| **cost**      | Custo associado ao perfil.                                                             | Opcional |


**Comentários**

Pelo menos um parâmetro entre o nome da interface e o custo deve ser especificado.


**Exemplo**

```powershell
set profileparameter name="profile 1" cost=default
```


### <a name="slotmapping"></a>slotmapping

Define o mapeamento de slot de modem de Banda Larga Móvel para a interface fornecida.

**Sintaxe**

```powershell
set slotmapping [interface=]<string> [slotindex=]<integer>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |
| **slotindex** | Índice do slot a ser definido.                                                                         | Necessária |


**Exemplo**

```powershell
set slotmapping interface="Cellular" slotindex=1
```


### <a name="tracing"></a>tracing

Habilitar ou desabilitar o rastreamento.

**Sintaxe**

```powershell
set tracing [mode=]yes|no
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **mode**      | Um dos seguintes valores:<br>yes: habilita o rastreamento para Banda Larga Móvel.<br>não: desabilita o rastreamento para Banda Larga Móvel.     | Necessária |


**Exemplo**

```powershell
set tracing mode=yes
```

## <a name="show"></a>show

Exibe informações de rede de banda larga móvel.

Os comandos netsh mbn show disponíveis são:

- [acstate](#acstate-1)
- [capability](#capability)
- [connection](#connection)
- [dataenablement](#dataenablement-1)
- [dataroamcontrol](#dataroamcontrol-1)
- [dmprofiles](#dmprofiles)
- [enterpriseapnparams](#enterpriseapnparams-1)
- [highestconncategory](#highestconncategory-1)
- [homeprovider](#homeprovider)
- [interfaces](#interfaces)
- [netlteattachinfo](#netlteattachinfo)
- [pin](#pin)
- [pinlist](#pinlist)
- [preferredproviders](#preferredproviders)
- [profiles](#profiles)
- [profilestate](#profilestate)
- [provisionedcontexts](#provisionedcontexts)
- [purpose](#purpose)
- [radio](#radio)
- [readyinfo](#readyinfo)
- [signal](#signal)
- [slotmapping](#slotmapping-1)
- [slotstatus](#slotstatus)
- [smsconfig](#smsconfig)
- [tracing](#tracing-1)
- [visibleproviders](#visibleproviders)

### <a name="acstate"></a>acstate

Mostra o estado de conexão automática de dados de Banda Larga Móvel para a interface fornecida.

**Sintaxe**

```powershell
show acstate [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
show acstate interface="Cellular"
```


### <a name="capability"></a>capability

Mostra as informações de capacidade da interface para a interface fornecida.

**Sintaxe**

```powershell
show capability [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
show capability interface="Cellular"
```


### <a name="connection"></a>connection

Mostra as informações de conexão atuais para a interface fornecida.

**Sintaxe**

```powershell
show connection [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
show connection interface="Cellular"
```


### <a name="dataenablement"></a>dataenablement

Mostra o estado de habilitação de dados de Banda Larga Móvel para a interface fornecida.

**Sintaxe**

```powershell
show dataenablement [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
show dataenablement interface="Cellular"
```


### <a name="dataroamcontrol"></a>dataroamcontrol

Mostra o estado de controle de roaming de dados de Banda Larga Móvel para a interface fornecida.

**Sintaxe**

```powershell
show dataroamcontrol [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
show dataroamcontrol interface="Cellular"
```


### <a name="dmprofiles"></a>dmprofiles

Mostra uma lista de perfis de Configuração de DM configurados no sistema.

**Sintaxe**

```powershell
show dmprofiles [[name=]<string>] [[interface=]<string>]
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **name**      | Nome do perfil a ser exibido.                                                               | Opcional |
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Opcional |

**Comentários**

Mostra os dados do perfil ou lista os perfis no sistema.

Se o nome do perfil for fornecido, o conteúdo do perfil será exibido. Caso contrário, os perfis serão listados para a interface.

Se o nome da interface for fornecido, somente o perfil especificado na interface fornecida será listado. Caso contrário, o primeiro perfil correspondente será exibido.

**Exemplo**

```powershell
show dmprofiles name="profile 1" interface="Cellular"
show dmprofiles name="profile 2"
show dmprofiles
```


### <a name="enterpriseapnparams"></a>enterpriseapnparams

Mostra os parâmetros enterpriseAPN de dados de Banda Larga Móvel para a interface fornecida.

**Sintaxe**

```powershell
show enterpriseapnparams [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
show enterpriseapnparams interface="Cellular"
```


### <a name="highestconncategory"></a>highestconncategory

Mostra a categoria de conexão mais alta de Dados de Banda Larga Móvel para a interface fornecida.

**Sintaxe**

```powershell
show highestconncategory [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
show highestconncategory interface="Cellular"
```


### <a name="homeprovider"></a>homeprovider

Mostra as informações do provedor de residencial para a interface fornecida.

**Sintaxe**

```powershell
show homeprovider [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
show homeprovider interface="Cellular"
```


### <a name="interfaces"></a>interfaces

Mostra uma lista de interfaces de Banda Larga Móvel no sistema. Não há parâmetros para esse comando.

**Sintaxe**

```powershell
show interfaces
```


### <a name="netlteattachinfo"></a>netlteattachinfo

Mostra as informações de anexação LTE de rede de Banda Larga Móvel para a interface fornecida.

**Sintaxe**

```powershell
show netlteattachinfo [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
show netlteattachinfo interface="Cellular"
```

### <a name="pin"></a>pin

Mostra as informações de marcador para a interface fornecida.

**Sintaxe**

```powershell
show pin [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
show pin interface="Cellular"
```


### <a name="pinlist"></a>pinlist

Mostra as informações de lista de marcadores para a interface fornecida.

**Sintaxe**

```powershell
show pinlist [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
show pinlist interface="Cellular"
```


### <a name="preferredproviders"></a>preferredproviders

Mostra a lista de provedores preferenciais para a interface fornecida.

**Sintaxe**

```powershell
show preferredproviders [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
show preferredproviders interface="Cellular"
```


### <a name="profiles"></a>profiles

Mostra uma lista de perfis configurados no sistema.

**Sintaxe**

```powershell
show profiles [[name=]<string>] [[interface=]<string>] [[purpose=]<string>]
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **name**      | Nome do perfil a ser exibido.                                                               | Opcional |
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Opcional |
| **purpose**   | Finalidade | Opcional |

**Comentários**

Se o nome do perfil for fornecido, o conteúdo do perfil será exibido. Caso contrário, os perfis serão listados para a interface.

Se o nome da interface for fornecido, somente o perfil especificado na interface fornecida será listado. Caso contrário, o primeiro perfil correspondente será exibido.

Se a finalidade for fornecida, somente os perfis com o GUID de finalidade correspondente serão exibidos.  Caso contrário, os perfis não serão filtrados por finalidade.  A cadeia de caracteres pode ser um GUID com chaves ou uma das seguintes cadeias de caracteres: internet, supl, mms, ims ou allhost.

**Exemplo**

```powershell
show profiles interface="Cellular" purpose="{00000000-0000-0000-0000-000000000000}"
show profiles name="profile 1" interface="Cellular"
show profiles name="profile 2"
show profiles
```

### <a name="profilestate"></a>profilestate

Mostra o estado de um perfil de Banda Larga Móvel para a interface fornecida.

**Sintaxe**

```powershell
show profilestate [interface=]<string> [name=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |
| **name**      | Nome do perfil. É o nome do perfil que tem o estado a ser mostrado.            | Necessária |

**Exemplo**

```powershell
show profilestate interface="Cellular" name="myProfileName"
```

### <a name="provisionedcontexts"></a>provisionedcontexts

Mostra as informações de contextos provisionadas para a interface fornecida.

**Sintaxe**

```powershell
show provisionedcontexts [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
show provisionedcontexts interface="Cellular"
```


### <a name="purpose"></a>purpose

Mostra os GUIDs do grupo de finalidade que podem ser usados para filtrar perfis no dispositivo. Não há parâmetros para esse comando.

**Sintaxe**

```powershell
show purpose
```


### <a name="radio"></a>radio

Mostra as informações de estado de rádio para a interface fornecida.

**Sintaxe**

```powershell
show radio [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
show radio interface="Cellular"
```


### <a name="readyinfo"></a>readyinfo

Mostra as informações prontas para a interface fornecida.

**Sintaxe**

```powershell
show readyinfo [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
show readyinfo interface="Cellular"
```


### <a name="signal"></a>signal

Mostra as informações de sinal para a interface fornecida.

**Sintaxe**

```powershell
show signal [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
show signal interface="Cellular"
```


### <a name="slotmapping"></a>slotmapping

Mostra o mapeamento de slot de modem de Banda Larga Móvel para a interface fornecida.

**Sintaxe**

```powershell
show slotmapping [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
show slotmapping interface="Cellular"
```


### <a name="slotstatus"></a>slotstatus

Mostra o status do slot de modem de Banda Larga Móvel para a interface fornecida.

**Sintaxe**

```powershell
show slotstatus [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
show slotstatus interface="Cellular"
```


### <a name="smsconfig"></a>smsconfig

Mostra as informações de configuração do SMS para a interface fornecida.

**Sintaxe**

```powershell
show smsconfig [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
show smsconfig interface="Cellular"
```


### <a name="tracing"></a>tracing

Mostra se o rastreamento de Banda Larga Móvel está habilitado ou desabilitado.

**Sintaxe**

```powershell
show tracing
```


### <a name="visibleproviders"></a>visibleproviders

Mostra a lista de provedores visíveis para a interface fornecida.

**Sintaxe**

```powershell
show visibleproviders [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome da interface. É um dos nomes de interface mostrados pelo comando "netsh mbn show interfaces". | Necessária |


**Exemplo**

```powershell
show visibleproviders interface="Cellular"
```

## <a name="test"></a>test

Executa testes para uma área de recurso específica enquanto coleta logs.

**Sintaxe**
```
test [feature=<feature area>] [testPath=<path>] [taefPath=<path>] [param=<test input params>]
```

**Parâmetros**

| Marca | Valor | Opcional? |
|---|---|---|
| **feature** | Uma área de recursos das áreas de recursos compatíveis listadas abaixo | Obrigatório |
| **testpath** | Demarcador que contém os binários de teste | Opcional se o servidor HLK estiver instalado |
| **taefpath** | Demarcador que contém os binários TAEF | Opcional se o servidor HLK estiver instalado |
| **param** | Parâmetros separados por vírgulas, a serem usados para os testes | Necessários para determinadas áreas de recursos, opcionais para outros |

**Comentários**

As áreas de recursos compatíveis são:
- conectividade
- potência
- radio
- esim
- sms
- dssa
- lte
- bringup

Alguns testes exigem parâmetros de teste adicionais que precisam ser fornecidos no campo `param`.
Os parâmetros necessários para os recursos estão listados abaixo.
- **connectivity**: AccessString, UserName (se aplicável), Senha (se aplicável)
- **radio**: AccessString, UserName (se aplicável), Senha (se aplicável)
- **esim**: ActivationCode
- **bringup**: AccessString, UserName (se aplicável), Senha (se aplicável)

**Exemplos**

```
test feature=connectivity param="AccessString=internet"
test feature=lte testpath="C:\\data\\test\\bin" taefpath="C:\\data\\test\\bin"
test feature=lte
```
