## <a name="importance-of-time-protocols"></a>Importância dos protocolos de tempo
Protocolos de tempo se comunicar entre dois computadores para trocar informações de tempo e, em seguida, usar essas informações para sincronizar seus relógios. Com o protocolo de tempo de serviço de tempo do Windows, um cliente solicita informações de tempo de um servidor e sincroniza o relógio com base nas informações que são recebidas.
  
O serviço de tempo do Windows usa NTP para ajudar a sincronizar o tempo em uma rede. NTP é um protocolo de tempo de Internet que inclui os algoritmos de disciplina necessários para sincronizar relógios. NTP é um protocolo de tempo mais preciso do que a simples SNTP Network Time Protocol () que é usado em algumas versões do Windows; No entanto, W32Time continua a dar suporte a SNTP para habilitar a compatibilidade com computadores que executam serviços de tempo com base no SNTP como o Windows 2000.

Há muitos motivos diferentes, que talvez seja necessário tempo preciso.  O caso comum para o Windows é Kerberos, que requer 5 minutos de precisão entre o cliente e servidor.  No entanto, há muitas outras áreas que podem ser afetadas por tempo precisão incluindo:


- Leis, como:
    - Precisão de 50 ms para FINRA nos EUA
    - 1 ms ESMA (MiFID II) na UE.
- Algoritmos de criptografia
- Sistemas distribuídos como SQL/Cluster/troca e bancos de dados do documento
- Estrutura da Blockchain para transações com bitcoins
- Logs distribuídos e análise de ameaça 
- Replicação do AD
- PCI (DSS), precisão segunda atualmente 1