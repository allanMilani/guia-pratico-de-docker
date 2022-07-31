# guia-pratico-de-docker
O objetivo do repositório e compartilhar meus singelos estudo com docker e contribuir para a comunidade :)
# Docker

## Como funcionam os container?

- Namespaces: Isola os processos (um conjunto de processos fazem referencia a um namespace).

![Untitled](Docker%20939e442e84654ad3997cfebaad5f9f1b/Untitled.png)

- Cgroups = Controla os recursos computacionais (CPU, Memória, etc)
- File System = OFS (Overlay File System) (Trabalha com camadas da aplicação, sem copiar todo o sistema operacional)
- Image é imutável
- Image Registry = repositório de imagens
- Toda e qualquer alteração feita no container ela pode ser perdida, pois a imagem é imutável
- exec “$@”: quando for criado um arquivo de entrepoint o seguinte comando deve ser passado para que comando após sejam executados

---

## WSL2 (Windows)

- Requisitos: Windows 10 Home ou Pro
- Versão do Windows ≥ 19.03
- Virtual Machine Plataform Habilitado
- Como instalar (https://github.com/codeedu/wsl2-docker-quickstart):
- **Sempre copiar os arquivos para o /home**

---

```docker
docker ps
```

Exibe uma lista com todos os container ativos no momento

- *-a* : lista todos os containers existentes
- *-q*: exibe apenas o id do container
    - *docker ps -a -q*

---

```docker
docker run <NOME_DA_IMAGEM>:<VERSÃO(OPCIONAL)>
```

Executa uma imagem e caso ela não exista a mesma será baixada do docker hub

- *-d* : executa um container sem travar o terminal
    - *docker run -d*
- *-p* : informa a porta que o container irá utilizar na máquina local
    - *docker run -p 8080:80* (neste caso é informado que a porta 80 do container deverá ser acessada pelo endereço local na porta 8080)
- *-v* : mapeia o caminho da máquina local usado para referenciar um outro caminho dentro do container (se a pasta não existir ele irá cria-lá)
    - *docker run -v ${pwd}:<CAMINHO_DO_CONTAINER>*
- -*e* : informa variáveis de ambiente para o container
    - *docker run -e <VARIAVEL>*
- *-it:* acessa o terminal da imagem que será executado
    - *docker run -it <IMAGEM> /bin/bash*
    - docker run -it  - -rm <*IMAGEM*> bash
        - ao informar o parâmetro *rm* o container é excluído quando terminado a execução
- - -*name*: nomeia um container
    - *docker run -d —name <NOME_DO_CONTAINER> <IMAGEM>*
- - *-mount*: monta um volume (se a pasta não existir ocorerrá um erro)
    - *docker run —mount type=bind,source=”$(pwd)”/html,target=/usr/share/nginx/html  <IMAGEM>*
- - *-network*: roda um container especificando sua rede
    - *docker run —network <NOME_DA_NETWORK>*

---

```docker
docker image ls
```

Lista todas as imagens contidas na máquina

---

```docker
docker stop <NOME/ID>
```

Para a execução de um container

---

```docker
docker exec <COMANDO> <NAME/ID> /bin/bash
```

Executa um comando no terminal do container

- *-it* : habilita o modo interativo do container, sendo possivel entra na imagem e executar comando dentro dela
    - *docker exec -it <NAME/ID> bash*

---

```docker
docker rm <NAME/ID>
```

Remove um container 

- *-rf* : remove um container em execução
- Removendo todas as imagens:
    - ***docker rm $(docker ps -a -q) -f***

---

```docker
docker rmi <NAME/ID>
```

Remove uma imagem

---

```docker
docker cp <ID>:<CAMINHO_DO_ARQUIVO_NO_CONTAINER> <CAMINHO_DA_MAQUINA>
```

 Copia um arquivo do container para a maquina local, invertendo a ordem dos itens a ação contraria ocorre

---

```docker
docker build -t <NOME_DA_IMAGEM> <DIRETORIO_DO_ARQUIVO_DE_BUILD>
```

Gera uma imagem a partir de um arquivo

---

```tsx
docker volume
```

Com os volumes é possível compartilhar arquivos entre os container

- *ls*: exibe todos os volumes que estão sendo utilizados
    - *docker volume ls*
- *create:* cria um volume
    - *docker volume create <NOME_DO_VOLUME>*
- *inspect:* exibe as configurações do volume criado
    - *docker volume inspect <NOME_DO_VOLUME>*
- *prune:* remove todos os volumes
    - *docker volume prune*

---

```yaml
docker attache <CONTAINER>
```

- Entra diretamente no terminal do container

---

```yaml
docker logs <CONTAINER>
```

- Exibe os logs de um container

---

### Networks

- *bridge:*  (ponte) comunicação entre containers
- *host*: mescla a network do container com o host(maquina local)
- *overlay*: comunicação entre maquinas diferentes
- *maclan:* Comunicação entre Macs adress
- *none*
- [http://host.docker.internal:8000](http://host.docker.internal:8000) : acessa a porta 8000 local da maquina

```yaml
docker network
```

- *prune:* remove todas as networks
    - *docker network prune*
- *ls*: **lista todas as networks
    - *docker networks ls*
- *inspect:* inspeciona a rede
    - *docker network inspect <TIPO_DA_NETWORK>*
    - dentro do container no json é mostrados os container utilizando aquela rede
- *create*: cria um rede
    - *docker network create —driver <TIPO_DA_NETWORK> <NOME_DA_REDE>*
- 

---

### Dockerfile

```docker
FROM <IMAGEM_BASE> #Exepecifica a imagem

WORKDIR <DIRETORIO_DO_CONTAINER> #Diretório que será utilizado no container 

COPY <DIRETORIO_LOCAL> <DIRETORIO_DO_CONTAINER> #Copia os dados para o container

RUN <COMANDO> #Executa um comando

USER <NOME_DO_USUÁRIO> #Expecifica um usuário pre-existente na imagem

EXPOSE <NUMERO_DA_PORTA> #Expoẽ a porta que aplicação esta rodando

CMD ["parte1", "parte2"] #Executa um comando. entretanto se for passado uma paramento na inicialização do container ele será substituido (**comando variável**)

ENTRYPOINT ["parte1", "parte2"] #Executa um comando (**comando fixo**)

ENV [VARIAVEL] #Adiciona uma variavel de ambiente

LABEL [ROTULO] #Adciona um rótulo como o nome de quem mantem a imagem

```

---

```tsx
docker build
```

- -*t :* Da um nome para a imagem que será gerada, o **ponto** representa o contexto para gerar a imagem
    - *docker build -t <USERNAME_DOCKER_HUB>/<NOME_DA_IMAGEM>:<TAG>  .*
- *-f:* Expecifica o arquivo dockerfile que será compilada
    - *docker build -f <DOCKERFILE> -t <USERNAME_DOCKER_HUB>/<NOME_DA_IMAGEM>:<TAG>  .*

---

### Docker-Compose.yaml

```yaml
version: '<VERSÃO>' #Expecifica a versão do docker-compose

services:
	<NOME_DO_CONTAINER>:
		buid: . #Cria um serviço a partir do Dockerfile do diretório
		image: <IMAGEM> #Expecifica uma imagem
		ports: 
			- <PORTA_DA_MAQUINA_LOCAL>:<PORTA_DO_CONTAINER>
		volumes:
			- <DIRETORIO_MAQUINA_LOCAL>:<DIRETORIO_CONTAINER>

```

---

### DockerHub

```docker
docker pull <IMAGEM> #Busca uma imagem no dockerhube

docker login #Realiza o login no dockerhub

docker push <IMAGEM> #quando criar a imagem utilizar a estrutura <USERNAME_DOCKER_HUB>/<NOME_DA_IMAGEM>
```

---
