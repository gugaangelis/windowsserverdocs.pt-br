---
title: verifier
description: Artigo de referência para o comando do verificador, que executa o utilitário Gerenciador de verificação de driver.
ms.topic: reference
ms.assetid: 782173d6-f196-4bc4-a547-76109829453c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: df3a9826463ea4062fb661e04e8fb774275c09d7
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156269"
---
# <a name="verifier"></a>verifier

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O verificador de driver monitora drivers de modo kernel do Windows e drivers gráficos para detectar chamadas de função ilegais ou ações que podem corromper o sistema. O verificador de driver pode sujeitar drivers do Windows a uma variedade de estresse e testes para encontrar comportamentos inadequados. Você pode configurar quais testes executar, o que permite que você coloque um driver por meio de cargas de estresse pesadas ou por meio de um teste mais simplificado. Você também pode executar o verificador de driver em vários drivers simultaneamente ou em um driver por vez.

> [!IMPORTANT]
> Você deve estar no grupo Administradores no computador para usar o verificador de driver.
> Executar o verificador de driver pode fazer com que o computador falhe, portanto, você deve executar esse utilitário somente em computadores usados para teste e depuração.

## <a name="syntax"></a>Sintaxe

```
verifier /standard /all
verifier /standard /driver NAME [NAME ...]
verifier /flags <options> /all
verifier /flags <options> /driver NAME [NAME ...]
verifier /rules [OPTION ...]
verifier /query
verifier /querysettings
verifier /bootmode [persistent | disableafterfail | oneboot]
verifier /reset
verifier /faults [Probability] [PoolTags] [Applications] [DelayMins]
verifier /faultssystematic [OPTION ...]
verifier /log LOG_FILE_NAME [/interval SECONDS]
verifier /volatile /flags <options>
verifier /volatile /adddriver NAME [NAME ...]
verifier /volatile /removedriver NAME [NAME ...]
verifier /volatile /faults [Probability] [PoolTags] [Applications] [DelayMins]
verifier /domain <types> <options> /driver ... [/logging | /livedump]
verifier /logging
verifier /livedump
verifier /?
verifier /help
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /all | Direciona o utilitário de verificação de driver para verificar todos os drivers instalados após a próxima inicialização. |
| /BootMode `[persistent | disableafterfail | oneboot | resetonunusualshutdown]` | Controla se as configurações do utilitário verificador de driver estão habilitadas após uma reinicialização. Para definir ou alterar essa opção, você deve reinicializar o computador. Os seguintes modos estão disponíveis:<ul><li>**persistente** – garante que as configurações do verificador de driver persistam (permaneçam em vigor) em várias reinicializações. Essa é a configuração padrão.</li><li>**disableafterfail** -se o Windows não for iniciado, essa configuração desabilitará o utilitário de verificação de driver para reinicializações subsequentes.</li><li>**oneboot** – habilita apenas as configurações do verificador de driver para a próxima vez que o computador for iniciado. O utilitário de verificação de driver está desabilitado para reinicializações subsequentes.</li><li>**resetonunusualshutdown** -o utilitário de verificação de driver será mantido até que ocorra um desligamento incomum. Seu abbrevation, ' Rous ', pode ser usado.</li></ul> |
| /Driver `<driverlist>` | Especifica um ou mais drivers que serão verificados. O parâmetro **driverlist** é uma lista de drivers por nome binário, como *driver.sys*. Use um espaço para separar cada nome de driver. `n*.sys`Não há suporte para valores curinga, como,. |
| /driver.exclude `<driverlist>` | Especifica um ou mais drivers que serão excluídos da verificação. Esse parâmetro será aplicável somente se todos os drivers forem selecionados para verificação. O parâmetro **driverlist** é uma lista de drivers por nome binário, como *driver.sys*. Use um espaço para separar cada nome de driver. `n*.sys`Não há suporte para valores curinga, como,. |
| /faults | Habilita o recurso de **simulação de recursos baixos** no utilitário de verificação de driver. Você pode usar o **/Faults** no lugar de `/flags 0x4` . No entanto, você não pode usar `/flags 0x4` com os subparâmetros **/Faults** . Você pode usar os seguintes subparâmetros do parâmetro/Faults para configurar a simulação de recursos baixos:<ul><li>**Probabilidade** -especifica a probabilidade de o utilitário de verificação de driver falhar em uma determinada alocação. Digite um número (em decimal ou hexadecimal) para representar o número de chances em 10.000 que o utilitário de verificação de driver falhará na alocação. O valor padrão, 600, significa 600/10000 ou 6%.</li><li>**Marcas de pool** – limita as alocações que o utilitário de verificador de driver pode falhar em alocações com as marcas de pool especificadas. Você pode usar um caractere curinga (*) para representar várias marcas de pool. Para listar várias marcas de pool, separe as marcas com espaços. Por padrão, todas as alocações podem falhar. </li> <li> * * Aplicativos** – limita as alocações que o utilitário de verificação de driver pode falhar em alocações para o programa especificado. Digite o nome de um arquivo executável. Para listar programas, separe os nomes de programa com espaços. Por padrão, todas as alocações podem falhar.</li><li>**DelayMins** -especifica o número de minutos após a inicialização durante o qual o utilitário de verificação de driver não falha intencionalmente em nenhuma alocação. Esse atraso permite que os drivers sejam carregados e o sistema seja estabilizado antes do início do teste. Digite um número (em decimal ou hexadecimal). O valor padrão é 7 (minutos).</li></ul> |
| /faultssystematic | Especifica as opções para simulação **sistemática de recursos baixos** . Use o `0x40000` sinalizador para selecionar a opção de simulação de **recursos baixos sistemática** . As seguintes opções estão disponíveis:<ul><li>**enableboottime** -habilita as injeções de falha nas reinicializações do computador.</li><li>**disableboottime** -desabilita as injeções de falha nas reinicializações do computador (essa é a configuração padrão).</li><li>**recordboottime** -habilita as injeções de falha no modo que se faz em reinicializações do computador.</li><li>**resetboottime** -desabilita as injeções de falha nas reinicializações do computador e limpa a lista de exclusão de pilha.</li><li>**enableruntime** – habilita injeções de falha dinamicamente.</li><li>**disableruntime** -desabilita dinamicamente as injeções de falha.</li><li>**recordruntime** – habilita dinamicamente as injeções de falha no modo What If.</li><li>**resetruntime** -desabilita dinamicamente as injeções de falha e limpa a lista de pilhas com falha anteriormente.</li><li>**querystatistics** -mostra as atuais estatísticas de injeção de falha.</li><li>**incrementcounter** – incrementa o contador de aprovação de teste usado para identificar quando uma falha foi injetada.</li><li>**contador getstackid** – recupera o identificador de pilha injetado indicado.</li><li>**EXCLUDESTACK stackid** -exclui a pilha da injeção de falha.</li></ul> |
| /flags `<options>` | Ativa as opções especificadas após a próxima reinicialização. Esse número pode ser inserido em decimal ou em formato hexadecimal (com um prefixo 0x). Qualquer combinação dos seguintes valores é permitida:<ul><li>**Valor: 1 ou 0x1 (bit 0)**  -  [Verificação de pool especial](https://docs.microsoft.com/windows-hardware/drivers/devtest/special-pool)</li><li>**Valor: 2 ou 0x2 (bit 1)**  -  [Forçar verificação de IRQL](https://docs.microsoft.com/windows-hardware/drivers/devtest/force-irql-checking)</li><li>**Valor: 4 ou 0x4 (bit 2)**  -  [Simulação de recursos baixos](https://docs.microsoft.com/windows-hardware/drivers/devtest/low-resources-simulation)</li><li>**Valor: 8 ou 0x8 (bit 3)**  -  [Rastreamento de pool](https://docs.microsoft.com/windows-hardware/drivers/devtest/pool-tracking)</li><li>**Valor: 16 ou 0x10 (bit 4)**  -  [Verificação de e/s](https://docs.microsoft.com/windows-hardware/drivers/devtest/i-o-verification)</li><li>**Valor: 32 ou 0x20 (bit 5)**  -  [Detecção de deadlock](https://docs.microsoft.com/windows-hardware/drivers/devtest/deadlock-detection)</li><li>**Valor: 64 ou 0x40 (bit 6)**  -  [Verificação de e/s aprimorada](https://docs.microsoft.com/windows-hardware/drivers/devtest/enhanced-i-o-verification). Essa opção é ativada automaticamente quando você seleciona **verificação de e/s**.</li><li>**Valor: 128 ou 0x80 (bit 7)**  -  [Verificação de DMA](https://docs.microsoft.com/windows-hardware/drivers/devtest/dma-verification)</li><li>**Valor: 256 ou 0x100 (bit 8)**  -  [Verificações de segurança](https://docs.microsoft.com/windows-hardware/drivers/devtest/security-checks)</li><li>**Valor: 512 ou 0x200 (bit 9)**  -  [Forçar solicitações de e/s pendentes](https://docs.microsoft.com/windows-hardware/drivers/devtest/force-pending-i-o-requests)</li><li>**Valor: 1024 ou 0x400 (bit 10)**  -  [Log de IRP](https://docs.microsoft.com/windows-hardware/drivers/devtest/irp-logging)</li><li>**Valor: 2048 ou 0x800 (bit 11)**  -  [Verificações diversas](https://docs.microsoft.com/windows-hardware/drivers/devtest/miscellaneous-checks)</li><li>**Valor: 8192 ou 0x2000 (bit 13)**  -  [Verificação de MDL invariável para pilha](https://docs.microsoft.com/windows-hardware/drivers/devtest/invariant-mdl-checking-for-stack)</li><li>**Valor: 16384 ou 0x4000 (bit 14)**  -  [Verificação de MDL invariável para o driver](https://docs.microsoft.com/windows-hardware/drivers/devtest/invariant-mdl-checking-for-driver)</li><li>**Valor: 32768 ou 0x8000 (bit 15)**  -  [Difusão de atraso do Power Framework](https://docs.microsoft.com/windows-hardware/drivers/devtest/concurrency-stress-test)</li><li>**Valor: 65536 ou 0x10000 (bit 16)** -verificação de interface de porta/miniporta</li><li>**Valor: 131072 ou 0x20000 (bit 17)**  -  [Verificação de conformidade de DDI](https://docs.microsoft.com/windows-hardware/drivers/devtest/ddi-compliance-checking)</li><li>**Valor: 262144 ou 0x40000 (bit 18)**  -  [Simulação de recursos baixos sistemática](https://docs.microsoft.com/windows-hardware/drivers/devtest/systematic-low-resource-simulation)</li><li>**Valor: 524288 ou 0x80000 (bit 19)**  -  [Verificação de conformidade da DDI (adicional)](https://docs.microsoft.com/windows-hardware/drivers/devtest/ddi-compliance-checking)</li><li>**Valor: 2097152 ou 0x200000 (bit 21)**  -  [Verificação de NDIS/WiFi](https://docs.microsoft.com/windows-hardware/drivers/devtest/ndis-wifi-verification)</li><li>**Valor: 8388608 ou 0x800000 (bit 23)**  -  [Difusão do atraso na sincronização do kernel](https://docs.microsoft.com/windows-hardware/drivers/devtest/kernel-synchronization-delay-fuzzing)</li><li>**Valor: 16777216 ou 0x1000000 (bit 24)**  -  [Verificação de comutador de VM](https://docs.microsoft.com/windows-hardware/drivers/devtest/vm-switch-verification)</li><li>**Valor: 33554432 ou 0x2000000 (bit 25)** – verificações de integridade de código. Você não pode usar esse método para ativar a verificação de SCSI ou as opções de verificação de storport. Para obter mais informações, consulte [verificação de SCSI](https://docs.microsoft.com/windows-hardware/drivers/devtest/scsi-verification) e verificação de [Storport](https://docs.microsoft.com/windows-hardware/drivers/devtest/dv-storport-verification).</li></ul> |
| /flags `<volatileoptions>` | Especifica as opções do utilitário de verificação de driver que são alteradas imediatamente sem reinicialização. Esse número pode ser inserido em decimal ou em formato hexadecimal (com um prefixo 0x). Qualquer combinação dos seguintes valores é permitida:<ul><li>**Valor: 1 ou 0x1 (bit 0)** -pool especial</li><li>**Valor: 2 ou 0x2 (bit 1)** -forçar verificação de IRQL</li><li>**Valor: 4 ou 0x4 (bit 2)** -simulação de recursos baixos</li></ul> |
| `<probability>` | Número entre 1 e 10.000 especificando a probabilidade de injeção de falha. Por exemplo, especificar 100 significa uma probabilidade de injeção de falha de 1% (100/10000).<p>Se esse parâmetro não for especificado, a probabilidade padrão de 6% será usada. |
| `<tags>` | Especifica as marcas de pool que serão injetadas com falhas, separadas por caracteres de espaço. Se esse parâmetro não for especificado, qualquer alocação de pool poderá ser injetada com falhas. |
| `<apps>` | Especifica o nome do arquivo de imagem dos aplicativos que serão injetados com falhas, separados por caracteres de espaço. Se esse parâmetro não for especificado, a simulação de recursos Baixos poderá ocorrer em qualquer aplicativo. |
| `<minutes>` | Um número positivo que especifica o comprimento do período após a reinicialização, em minutos, durante o qual nenhuma injeção de falha ocorrerá. Se esse parâmetro não for especificado, o comprimento padrão de *8 minutos* será usado. |
| /iolevel `<level>` | Especifica o nível de verificação de e/s. O valor de [level] pode ser **1** -permite a verificação de e/s de nível 1 (padrão) ou **2** -habilita a verificação de e/s de nível 1 e a verificação de e/s de nível 2. Se a verificação de e/s não estiver habilitada (usando `/flags 0x10` ), **/iolevel** será ignorado. |
| /log `<logfilename> [/intervalseconds]` | Cria um arquivo de log usando o nome especificado. O utilitário verificador de driver periodicamente grava estatísticas para esse arquivo, com base no intervalo que você definir opcionalmente. O intervalo padrão é de *30 segundos*.<p> Se um comando Verifier **/log** for digitado na linha de comando, o prompt de comando não retornará. Para fechar o arquivo de log e retornar um prompt, use a tecla **Ctrl + C** . Após uma reinicialização, para criar um log, você deve enviar o comando Verifier **/log** novamente. |
| /Rules `<option>` | Opções para regras que podem ser desabilitadas, incluindo:<ul><li>**consulta** -mostra o status atual das regras controláveis.</li><li>**Redefinir** – redefine todas as regras para seu estado padrão.</li><li>**ID padrão** – define a ID da regra para seu estado padrão. Para as regras com suporte, a ID da regra é o valor do parâmetro 1 0xC4 (DRIVER_VERIFIER_DETECTED_VIOLATION) de verificação de bug.</li><li>**desabilitar ID** -DESABILITA a ID da regra especificada. Para as regras com suporte, a ID da regra é o valor do parâmetro 1 0xC4 (DRIVER_VERIFIER_DETECTED_VIOLATION) de verificação de bug.</li></ul> |
| /padrão | Ativa as opções "padrão" ou verificador de driver padrão após a próxima reinicialização. As opções padrão são pool especial, verificação de IRQL forçada, rastreamento de pool, verificação de e/s, detecção de deadlocks, verificação de DMA, verificações de segurança, verificações diversas e verificação de conformidade de DDI. Isso é equivalente a `/flags 0x209BB`.<p>[!NOTE] A partir das versões do Windows 10 após 1803, o uso do `/flags 0x209BB` não habilitará mais automaticamente a verificação de WDF. Use a sintaxe **/padrão** para habilitar as opções padrão, com a verificação de WDF incluída. |
| /volatile | Altera as configurações sem reinicializar o computador. As configurações voláteis entram em vigor imediatamente.<p>Você pode usar o parâmetro **/volatile** com o parâmetro **/flags** para habilitar e desabilitar algumas opções sem reinicializar. Você também pode usar **/volatile** com os parâmetros **/adddriver** e **/removedriver** para iniciar ou parar a verificação de um driver sem reinicialização, mesmo que o utilitário de verificação de driver não esteja em execução. Para obter mais informações, consulte [usando configurações voláteis](https://docs.microsoft.com/windows-hardware/drivers/devtest/using-volatile-settings). |
| /adddriver `<volatiledriverlist>` | Remove os drivers especificados das configurações voláteis. Para especificar vários drivers, Liste seus nomes, separados por espaços. Não há suporte para valores curinga, como *n.sys*. |
| /removedriver `<volatiledriverlist>` |
| /reset | Limpa todas as configurações do utilitário verificador de driver. Após a próxima reinicialização, nenhum driver será verificado. |
| /querysettings | Exibe um resumo das opções que serão ativadas e os drivers que serão verificados após a próxima inicialização. A exibição não inclui drivers e opções adicionadas usando o parâmetro **/volatile** . Para ver outras maneiras de exibir essas configurações, consulte [exibindo as configurações do verificador de driver](https://docs.microsoft.com/windows-hardware/drivers/devtest/viewing-driver-verifier-settings). |
| /Query | Exibe um resumo da atividade atual do utilitário verificador de driver. O campo **nível** na exibição é o valor hexadecimal das opções definidas com o parâmetro **/volatile** . Para obter explicações de cada estatística, consulte [monitorando contadores globais](https://docs.microsoft.com/windows-hardware/drivers/devtest/monitoring-global-counters) e [monitorando contadores individuais](https://docs.microsoft.com/windows-hardware/drivers/devtest/monitoring-individual-counters). |
| /Domain `<types> <options>` | Controla as configurações de extensão do verificador. Há suporte para os seguintes tipos de extensão do verificador:<ul><li>**WDM** – habilita a extensão do verificador para drivers WDM.</li><li>**NDIS** – habilita a extensão do verificador para drivers de rede.</li><li>**KS** – habilita a extensão do verificador para drivers de streaming do modo kernel.</li><li>**áudio** – habilita a extensão do verificador para drivers de áudio.</li></ul>. Há suporte para as seguintes opções de extensão:<ul><li>**Rules. Default** – habilita as regras de validação padrão para a extensão do verificador selecionada.</li><li>**Rules. All** – habilita todas as regras de validação para a extensão do verificador selecionada.</li></ul> |
| /logging | Habilita o registro em log para regras violadas detectadas pelas extensões do verificador selecionado. |
| /livedump | Habilita a coleta de despejo de memória dinâmica para regras violadas detectadas pelas extensões do verificador selecionado. |
| /? | Exibe a ajuda de linha de comando. |

### <a name="return-codes"></a>Códigos de retorno

Os valores a seguir são retornados após a execução do verificador de driver:

- 0: EXIT_CODE_SUCCESS

- 1: EXIT_CODE_ERROR

- 2: EXIT_CODE_REBOOT_NEEDED

#### <a name="remarks"></a>Comentários

- Você pode usar o parâmetro **/volatile** com algumas das opções do utilitário de verificação de driver **/flags** e com **/padrão**. Você não pode usar **/volatile** com as opções **/flags** para verificação de [conformidade com DDI](https://docs.microsoft.com/windows-hardware/drivers/devtest/ddi-compliance-checking), o [Power Framework atrasa a difusão](https://docs.microsoft.com/windows-hardware/drivers/devtest/concurrency-stress-test), verificação de [Storport](https://docs.microsoft.com/windows-hardware/drivers/devtest/dv-storport-verification)ou verificação de [SCSI](https://docs.microsoft.com/windows-hardware/drivers/devtest/scsi-verification). Para obter mais informações, consulte [usando configurações voláteis](https://docs.microsoft.com/windows-hardware/drivers/devtest/using-volatile-settings).

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Verificador de driver](https://docs.microsoft.com/windows-hardware/drivers/devtest/driver-verifier)

- [Controlando o verificador de driver](https://docs.microsoft.com/windows-hardware/drivers/devtest/controlling-driver-verifier)

- [Verificador de driver de monitoramento](https://docs.microsoft.com/windows-hardware/drivers/devtest/monitoring-driver-verifier)

- [Usando configurações voláteis](https://docs.microsoft.com/windows-hardware/drivers/devtest/using-volatile-settings)
