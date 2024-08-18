## Jenkins Compose

```sh
git clone https://github.com/dalmofelipe/jenkins-compose

cd jenkins-compose
```

### Walkthrough

1. Exec `docker compose up`
2. Copiar senha do jenkins gerado no terminal do docker compose up
3. Instalar plugins padrão
4. Criar usuário admin do jenkins
5. Configurar host do jenkins ou deixe o padrão
6. Configurar novo node `agent` no master `jenkins`
    6.1. Acessar o bash do container `agent`
    6.2. Gerar chaves ssh
    6.3. Copiar os valores publico e privado da key
    6.4. Colar a nova chave publica no docker-compose.yml subistuindo a antiga
    6.5. Exportar a variavel de ambiente tambem com valor da chave publica
        - export JENKINS_AGENT_SSH_PUBKEY="<chave-publica>"
7. Criar credentias no `jenkins` para acesso ao `agent`
8. 

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

Dois container serão lançados, `agent` onde os pipelines serão executados e `jenkins` gerenciamento dos pipelines.

### Chaves SSH para o Agent

Gerar chaves SSH dentro do container do `agent`.

```sh
docker exec -it agent bash
ssh-keygen -f /root/.ssh/jenkins_agent
```

Modificar no `docker-compose.yml` a variavel de ambiente `JENKINS_AGENT_SSH_PUBKEY` do container do agent, para nova chave publica gerada acima.
