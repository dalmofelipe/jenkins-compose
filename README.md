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

### Docker Compose UP

```sh
docker compose up
```

Dois container serão lançados, `agent` onde os pipelines serão executados e `jenkins` gerenciamento dos pipelines.

1. Copiar senha do jenkins gerado no terminal e usa-la para configurar `jenkins`
2. Instalar plugins padrão
3. Criar usuário admin do jenkins 
4. Configurar host do jenkins ou deixe o padrão

### Chaves SSH no container Agent

Gerar chaves SSH dentro do container do `agent`.

```sh
docker exec -it agent bash
ssh-keygen -f /root/.ssh/jenkins_agent
cat /root/.ssh/jenkins_agent
cat /root/.ssh/jenkins_agent.pub
```

Modificar no `docker-compose.yml` a variavel de ambiente `JENKINS_AGENT_SSH_PUBKEY` do container do agent, para nova chave publica gerada acima.

### Credentials

Criar credentias para container `jenkins` ter acesso ao container `agent`.

Acesse: `Painel de controle > Gerenciar Jenkins > Credentials > System Global credentials  (unrestricted)`

Clicar em `+ Add credentials`

```
- kind: SSH Username with private key
- Scope: System (jenkins and nodes only)
- ID: Agent Key
- Description: ...
- Username: jenkins ###Username definido na imagem do jenkins
- Private Key > Enter Directly > Adicionar
    - Key: Cole a chave ssh privada do container **Agent**
- Clicar em **Create**
```

### Adicionar `Node Agent` no Jenkins

Clique em Add Novo Node, e Preencha:

```
- Nome: Agent Node
- Description: -
- Numero de Excutores: 1
- Diretório raiz remoto: /home/jenkins/agent #volume indicado no docker-compose.yaml
- Rótulos: -
- uso: usar este nó o maximo possível
- Método de Lançamento: Launch agents via ssh
- Host: agent #Deve ser exatamente o nome do container
- Credentials: Usar a credential criada no passo anterior
- Host Key Verification Strategy: Non verifying Verification Strategy
- Ao clicar em salvar, o AGENT NÃO SERÁ LANÇADO!!!
- Encerre o comando do docker compose up com Ctrl + C
- Libere permissão de leitura e escrita no diretorio `data` na raiz deste repo
- Execute novamente o `docker compose up`, pronto AGENT ADICIONADO!
```