# Instalando o Gremlin no Windows

https://www.gremlin.com/docs/getting-started/install-windows/


## Instalar o agente Gremlin

Abra o Powershell como Administrador:

```bash
msiexec /package https://windows.gremlin.com/installer/latest/gremlin_installer.msi
```

## Configurar Gremlin

A configuração mínima necessária para autenticar o Agente Gremlin é:

- ID da equipe (https://app.gremlin.com/settings/team/cea0b46c-9b07-4497-a0b4-6c9b075497ed/configuration)
- Segredo ou certificado

Se você baixou um arquivo de configuração do cliente, você já tem tudo o que precisa para registrar o Agent. 

Basta seguir estas instruções:

Copie o arquivo de configuração (chamado config.yaml ) para o diretório C:\ProgramData\Gremlin\Agent . O caminho final deve ser C:\ProgramData\Gremlin\Agent\config.yaml .

```bash
Copy-Item "config.yaml" -Destination "C:\ProgramData\Gremlin\Agent\config.yaml"

Copy-Item "config.yaml" -Destination "C:\Program Files\Gremlin\Agent\config.yaml"
```

## Reinicie o serviço gremlind :

```bash
Restart-Service -Name gremlind
```

## Validar a instalação

Há duas maneiras de garantir que sua instalação foi bem-sucedida e que seus agentes foram autenticados com sucesso:

- Verifique a lista de agentes no aplicativo da web Gremlin
- Verifique a conexão do agente com os servidores do Gremlin
- Verifique o aplicativo da web Gremlin

A maneira mais fácil de verificar a conectividade é abrir a lista de Agentes no aplicativo da web Gremlin . Verifique seu Agente recém-instalado por nome ou por tag. Você também pode usar a caixa de pesquisa para pesquisar por nome ou tag, versão do Agente, sistema operacional (SO) ou região. Se seu Agente não aparecer nesta lista, ele pode não ter sido instalado ou configurado corretamente, ou pode não conseguir alcançar os servidores do Gremlin.

Verifique o agente Gremlin
Primeiro, verifique se o Gremlin Agent está em execução no sistema de destino:

```bash
Get-Service gremlind
```
Isso deve retornar o seguinte:

Status   Name               DisplayName
------   ----               -----------
Running  gremlind           Gremlin Daemon

Se o serviço estiver sendo reportado como inativo ou com falha, tente reiniciá-lo usando:

```bash
Restart-Service -Name gremlind
```

Depois de verificar se o Gremlin Agent está em execução, use gremlin check auth para verificar o status de autenticação do Gremlin Agent:

```bash
& 'C:\Program Files\Gremlin\Agent\gremlin.exe' check auth
```

Se o Agente Gremlin for autenticado com sucesso, a saída será semelhante à seguinte:

auth
====================================================
Auth Input Type                      : Certificate
API Response                         : OK

Caso contrário, a saída explicará por que o Agente Gremlin não conseguiu autenticar:

auth
====================================================
Auth Input Type                      : No valid auth found
========= Identification ============:
Team ID Source                       : Team ID not found
Gremlin Identifier                   : [Host identifier]
Gremlin Identifier Source            : Not supplied explicitly, using the default
========= Secret/Token Info =========:
Team Secret Source                   : Team Secret not found
.credentials valid                   : /var/lib/gremlin/.credentials file not found
========= Certificate Info ==========:
Team Certificate Source              : Team Certificate not found
========= Private Key Info ==========:
Team Private Key Source              : Team Private Key not found

## Verifique a Conexão de Rede

Confirme que o sistema pode se conectar aos servidores Gremlin. Você pode tentar um comando ping para ver se o endereço é acessível:

```bash
ping app.gremlin.com

Disparando dvpvc1m0h5bac.cloudfront.net [2600:9000:213b:2200:c:6e46:5900:93a1] com 32 bytes de dados:
Resposta de 2600:9000:213b:2200:c:6e46:5900:93a1: tempo=20ms 
Resposta de 2600:9000:213b:2200:c:6e46:5900:93a1: tempo=21ms 
Resposta de 2600:9000:213b:2200:c:6e46:5900:93a1: tempo=19ms 
Resposta de 2600:9000:213b:2200:c:6e46:5900:93a1: tempo=19ms 

Estatísticas do Ping para 2600:9000:213b:2200:c:6e46:5900:93a1:
    Pacotes: Enviados = 4, Recebidos = 4, Perdidos = 0 (0% de
             perda),
Aproximar um número redondo de vezes em milissegundos:
    Mínimo = 19ms, Máximo = 21ms, Média = 19ms
```


## Testar Certificado e Chave Privada

Certifique-se de que a chave privada corresponde ao certificado. Você pode usar ferramentas como OpenSSL para verificar isso. Um comando simples pode ser:

```bash
openssl x509 -in horadoqa-client.pub_cert.pem -noout -text
openssl ec -in horadoqa-client.priv_key.pem -noout -text
```