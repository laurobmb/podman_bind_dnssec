# DNSSEC in podman with CentOS 8

##### note
note: The keys must be in the code before uploading the container, these within the project serve as an example.

## Configure
No arquivo **files/authority.zone** configure a zona que o DNS deve responder, bem como os endereços IPs dos hosts.

No arquivo **files/reverse.zone** configure a zona reversa que o DNS deve responder, bem como os endereços IPs dos hosts.

#### Gerar Chave KSK e ZSK
A configuração das chaves é muito importante para o funcionamento do DNSSEC, execute:

***dnssec-keygen -r /dev/urandom -a RSASHA256 -b 4096 -f KSK -n ZONE [ domínio que você escolheu ]***

***dnssec-keygen -r /dev/urandom -a RSASHA256 -b 1024 -n ZONE [ domínio que você escolheu ]***

Inclua as chaves geradas no arquivo **files/authority.zone** (já possui um exemplo dentro do arquivo)


Execute os scripts para criar o DSSET do domínio e a primeira assinatura:

- reassina.sh
- reassina.reverse.sh

O arquivo ***files/entrypoint.sh*** necessita de alterações, nele você coloca os endereços IPs para que ao subir o contêiner o podman possa reassinar a zona.


O arquivo ***files/named.conf*** necessita de alterações também, aqui você deve colocar o domínio. 

> zone "[ domínio que você escolheu ]" IN {
>     type master;
>     file "authority.zone.signed";
>     allow-update { none; };
> };
> 
> zone "123.168.192.in-addr.arpa" IN {
>     type master;
>     file "reverse.zone";
>     allow-update { none; };
> };
