# curso-alura-docker-swarm

Modulo 1:
- Utiliar o comando abaixo deu erro: Error with pre-create check: "This computer doesn't have VT-X/AMD-v enabled. Enabling it in the BIOS is mandatory"
```
docker-machine create -d virtualbox vm1
```
- Entao tem que adicionar uma flag na frente
```
docker-machine create vm1 --virtualbox-no-vtx-check
```
-Criar um swarm
```
docker swarm init --advertise-addr <ip_da_maquina>
```
-Criar um swarm com as configuracoes de backup
Pode acontecer do manager morrer por sla qual motivo, entao eeh importante manter o backup das configuracoes mais atuais do swarm. Basta copiar oq tem na pasta abaixo:
```
/var/lib/docker/swarm/
```
Depois de trazer as informacoes do backup de novo pra pasta citada acima, informe o comando abaixo:
```
docker swarm init --force-new-cluster --advertise-addr <ip_da_maquina>
```

- Obter tocken da manager
```
docker swarm join-token worker
```
- Criar um servico no swarm
```
docker service create -p 8080:3000 <imagem_dockerhub>
```

- lista todos os nos do swarm
```
docker node ls
```
- lista todos os nos do swarm de forma mais formatada
```
docker node ls --format "{{.Hostname}} {{.ManagerStatus}}"
```
-Apagar manager
Primeiro deve rebaixala
```
docker node demote <nome_vm>
```
Depois apagar
```
docker node rm <nome_vm>
```
- Ipedir um no de rodar servicoes, em managers
```
docker node update --availability drain <id_servico>
```
- Restringir o servico a rodar apenas em workers
```
docker service update --constraint-add node.role==worker <id_servico>
```
- Restringir o servico a rodar apenas em um id de uma maquina
```
docker service update --constraint-add node.id==<id> <id_servico>
```
- Restringir o servico a rodar apenas em uma maquina pelo hostname
```
docker service update --constraint-add node.hostname==<vm1> <id_servico>
```
- Para remover as restricoes
```
docker service update --constraint-rm node.id==<id> <id_servico>
docker service update --constraint-rm node.hostname==<vm1> <id_servico>
docker service update --constraint-rm node.role==worker <id_servico>
```
- Gerar replicas dos servicoes
```
docker service scale <id_servico>=<quantidade_replicas>
```
