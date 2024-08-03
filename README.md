## Jenkins Compose

```sh
git clone https://github.com/dalmofelipe/jenkins-compose

cd jenkins-compose
```

### Diretório 'data'

Necessário liberar permissão de leitura e escrita no diretório **data**

```sh
sudo chown 1000:1000 -R data/
sudo chmod 777 -R data/
```


### docker compose up

```sh
docker compose up
```

Dois container serão lançados, __agent__ onde os pipelines serão executados e __jenkins__ gerenciamento dos pipelines.


### Chaves SSH para o Agent

Gerar chaves SSH dentro do container do __agent__.

```sh
docker exec -it agent bash
ssh-keygen -f /root/.ssh/jenkins_agent
```

Modificar no __docker-compose.yml__ a variavel de ambiente ```JENKINS_AGENT_SSH_PUBKEY``` do container do agent, para nova chave publica gerada acima.






1. docker compose up
2. copiar senha do jenkins gerado no terminal do docker compose up
3. instalar plugins padrão
4. criar usuário admin do jenkins
5. configurar host do jenkins
6. configurar agent no jenkins
    6.1. acessar o bash do container __agent__
    6.2. gerar chaves ssh
    6.3. copiar os valores publico e privado da key
    6.4. colar a nova chave publica no docker-compose.yml subistuindo a antiga
    6.5. exportar a variavel de ambiente tambem com valor da chave publica
        - export JENKINS_AGENT_SSH_PUBKEY="<chave-publica>"
7. criar credentias para acesso ao __agent__ a partir do master __jenkins__
8.
