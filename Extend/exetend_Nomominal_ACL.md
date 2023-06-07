
## Parte 1: Configurar um ACL Estendido Nomeado

### Passo 1: Negar o acesso de PC1 aos serviços HTTP e HTTPS em Server1 e Server2

a. Para começar a configuração de um ACL estendido com o nome "ACL", utilize o seguinte comando no roteador RT1:

```
RT1(config)# ip access-list extended ACL
```

b. Insira a declaração que nega o acesso de PC1 a Server1 apenas para HTTP (porta 80). Consulte a tabela de endereços para obter o endereço IP de PC1 e Server1.
```
RT1(config-ext-nacl)# deny tcp host 172.31.1.101 host 64.101.255.254 eq 80
```

c. Em seguida, insira a declaração que nega o acesso de PC1 a Server1 apenas para HTTPS (porta 443).
```
RT1(config-ext-nacl)# deny tcp host 172.31.1.101 host 64.101.255.254 eq 443
```

d. Insira a declaração que nega o acesso de PC1 a Server2 apenas para HTTP. Consulte a tabela de endereços para obter o endereço IP de Server2.
```
RT1(config-ext-nacl)# deny tcp host 172.31.1.101 host 64.103.255.254 eq 80
```

e. Insira a declaração que nega o acesso de PC1 a Server2 apenas para HTTPS.
```
RT1(config-ext-nacl)# deny tcp host 172.31.1.101 host 64.103.255.254 eq 443
```

### Passo 2: Negar o acesso de PC2 aos serviços FTP em Server1 e Server2

Consulte a tabela de endereços para obter o endereço IP de PC2.

a. Insira a declaração que nega o acesso de PC2 a Server1 apenas para FTP (porta 21).
```
RT1(config-ext-nacl)# deny tcp host 172.31.1.102 host 64.101.255.254 eq 21
```

b. Insira a declaração que nega o acesso de PC2 a Server2 apenas para FTP (porta 21).
```
RT1(config-ext-nacl)# deny tcp host 172.31.1.102 host 64.103.255.254 eq 21
```

### Passo 3: Negar o ping de PC3 em Server1 e Server2

Consulte a tabela de endereços para obter o endereço IP de PC3.

a. Insira a declaração que nega o acesso ICMP de PC3 a Server1.
```
RT1(config-ext-nacl)# deny icmp host 172.31.1.103 host 64.101.255.254
```

b. Insira a declaração que nega o acesso ICMP de PC3 a Server2.
```
RT1(config-ext-nacl)# deny icmp host 172.31.1.103 host 64.103.255.254
```

### Passo 4: Permitir todo o tráfego IP restante

Por padrão, uma lista de acesso nega todo o tráfego que não corresponde a nenhuma regra na lista. Para permitir todo o tráfego que não corresponde às

 declarações configuradas, utilize o seguinte comando:
```
RT1(config-ext-nacl)# permit ip any any
```

### Passo 5: Verificar a configuração da lista de acesso antes de aplicá-la a uma interface

Antes de aplicar qualquer lista de acesso, é necessário verificar a configuração para garantir que não haja erros de digitação e que as declarações estejam na ordem correta. Para visualizar a configuração atual da lista de acesso, utilize o comando `show access-lists` ou `show running-config`.

## Parte 2: Aplicar e Verificar o ACL Estendido

### Passo 1: Aplicar o ACL à interface correta e na direção correta

Consulte a tabela de endereços para determinar a interface correta. Por exemplo, se o tráfego estiver saindo da interface G0/0 em direção às redes remotas, você aplicaria o ACL na interface G0/0 no modo de saída (outbound).

Exemplo de comando para aplicar o ACL à interface G0/0 no modo de saída:
```
RT1(config)# interface G0/0
RT1(config-if)# ip access-group ACL out
```

### Passo 2: Testar o acesso de cada PC

a. Acesse os sites de Server1 e Server2 usando o navegador da web de PC1, tanto com o protocolo HTTP quanto com HTTPS. Utilize o comando `show access-lists` para verificar qual declaração da lista de acesso permitiu ou negou o tráfego.

b. Acesse o FTP de Server1 e Server2 usando PC1. O nome de usuário e senha é "cisco".

c. Envie ping para Server1 e Server2 a partir de PC1.

d. Repita os passos de "a" a "c" com PC2 e PC3 para verificar o correto funcionamento da lista de acesso.

Lembre-se de que essas instruções são específicas para o Packet Tracer e podem não corresponder exatamente às configurações reais em dispositivos de rede reais.
```