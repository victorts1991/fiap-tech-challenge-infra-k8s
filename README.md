# fiap-tech-challenge-infra-k8s

### Tech Challenge 4:
### Passos para homologação dos professores da Fiap

1. Execute todos os passos que estão no README.md dos repositórios abaixo para subir a imagem da api e dos microsserviços no ECR da AWS:
```
https://github.com/rhuandantas/fiap-tech-challenge-pagamentos
https://github.com/victorts1991/fiap-tech-challenge-api-producao
https://github.com/rhuandantas/fiap-tech-challenge-pedidos
```
2. Execute do passo 1 até o 22 do Tech Challenge 3 que se encontram abaixo com as mudanças abaixo:
```
- Ao invés da branch tech-challenge-3 utilize a branch main;
- Repita os passos 3, 4, 5 e 6 para cada um dos três bancos de dados que foram criados;
- No passo 8, ao invés de criar uma "repository secret" chamada DB_HOST, crie 3 "repository secret" chamadas DB_HOST_API, DB_HOST_PAGAMENTOS e DB_HOST_PRODUCAO;
```
3. Crie uma conta no Mongo Atlas:
```
https://www.mongodb.com/cloud/atlas/register
```
4. Crie um cluster free;
5. Em um dos passos da criação você irá criar um usuário, anote a senha do mesmo;
6. Após ter feito a criação vá até a aba "Data Services", no bloco "Application Development" e selecione a opção Go, após isso clique em “Get connection string”;
7. Copie a connection string que será algo semelhante ao exemplo abaixo:
```
mongodb+srv://victorts1991:<password>@cluster0.hkavkji.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0
```
8. Substitua a parte onde está <password> pela senha copiada na criação do cluster;
9. Em seu terminal, execute o comando abaixo com a connection string e copie a saída:
```
//Exemplo
echo -n 'mongodb+srv://victorts1991:<password>@cluster0.hkavkji.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0' | base64
```
10. Com a saída, adicione uma "repository secret" chamada MONGODB_URI;
**Revisar**
12. Após isso qualquer commit neste repositório que for para a branch "main", irá subir um cluster no EKS e toda a estrutura Kubernetes utilizando os arquivos yamls;
13. Após o pipeline ser concluído, antes de testar qualquer coisa, aguarde uns 5 minutos até que toda a estrutura tenha concluído todo o mapeamento interno da AWs;


### Validação da execução do Kubernetes

- A plataforma da AWS não permite inicialmente que um usuário root visualize os recursos do kubernetes nos detalhes do Cluster EKS, para que isso seja possível é necessário seguir um tutorial criado por eles mesmos:

(https://docs.aws.amazon.com/eks/latest/userguide/view-kubernetes-resources.html#view-kubernetes-resources-permissions)https://docs.aws.amazon.com/eks/latest/userguide/view-kubernetes-resources.html#view-kubernetes-resources-permissions

- Porém conseguimos visualizar toda a estrutura através do CLI ou chamando os endpoints da api mesmo.

# CLI:

```sh

// Será solicitado os seguintes valores neste comando:
// AWS Access Key ID: o mesmo de AWS_ACCESS_KEY_ID configurado em Settings->Security->Secrets and variables->Actions
// AWS Secret Access Key: o mesmo de AWS_SECRET_ACCESS_KEY configurado em Settings->Security->Secrets and variables->Actions
// Default region name: us-east-2
aws configure

aws eks update-kubeconfig --name fiap-tech-challenge-infra-k8s --region=us-east-2

kubectl get pods

kubectl describe pods
```

# Chamando os endpoints:

1. Na plataforma da AWS, vá até EC2 e clique em "Load balancers" e depois clique no Load Balancer criado;
2. Na tela que irá se abrir copie o valor do "Nome do DNS", ele será semelhante a este valor "acb1520c01aaf4b2cb76ddf05e045720-765794247.us-east-2.elb.amazonaws.com";
3. Após isso, concatene o valor copiado com a porta e a url de prova de vida(/liveness), depois disso, cole na barra de navegação do browser mesmo, o valor que você irá inserir na barra de navegação do browser será semelhante ao abaixo:

```sh
http://acb1520c01aaf4b2cb76ddf05e045720-765794247.us-east-2.elb.amazonaws.com:3000/liveness
```



### Tech Challenge 3:
### Passos para homologação dos professores da Fiap

Obs: Para executar os passos abaixo utilize a branch tech-challenge-3

Foi utilizada a nuvem da Amazon (AWS) para este tech challenge:

1. Execute todos os passos que estão no README.md do repositório abaixo para subir a imagem da api no ECR da AWS:
```
https://github.com/rhuandantas/fiap-tech-challenge-api
```
2. Após isso, execute todos os passos que estão no README.md do repositório abaixo para subir o banco de dados no RDS da AWS, lembre-se de guardar a senha utilizada, isso será necessário para os passos seguintes:
```
https://github.com/AlvaroRumpel/fiap-tech-challenge-infra-database
```
3. Agora acesse o menu "Bancos de dados" no RDS da AWS;
4. Acesse os detalhes do banco criado chamado db;
5. Em "Segurança e conexão" copie o valor que está em "Endpoint" e salve;
6. Agora precisamos criar o valor dos dados do Endpoint do banco de dados copiado anteriormente e da senha atribuída no passo 2, para isso, seguiremos a documentação de criação de segredos da documentação do Kubernetes:
```
https://kubernetes.io/docs/tasks/configmap-secret/managing-secret-using-config-file/
```
Basicamente será necessário executar comandos semelhantes aos do exemplo abaixo e copiar seus valores de saída:
```sh
//Endpoint
echo -n 'db.cbqgeakk0utc.us-east-2.rds.amazonaws.com' | base64
//OUTPUT: ZGIuY2JxZ2Vha2swdXRjLnVzLWVhc3QtMi5yZHMuYW1hem9uYXdzLmNvbQ==

//Senha
echo -n '123mudar' | base64
//OUTPUT: MTIzbXVkYXI=
```
7. Na plataforma do Github, acesse o menu "Settings" do projeto, na tela que se abrir, clique no menu Security->Secrets and variables->Actions;
8. Adicione uma "repository secret" chamada DB_HOST com o valor em base64 do Endpoint do banco de dados, e crie outra "repository secret" chamada DB_PASS com o valor em base64 da senha do banco de dados;
9. Na plataforma da AWS, acesse IAM->Usuários e crie um novo usuário chamado Github;
10. Com esse usuário criado, vá até a listagem de usuários e acesse os detalhes do mesmo;
11. No menu Permissões que irá aparecer na tela de detalhes, clique no botão "Adicionar permissões" que aparece no canto direito e selecione a opção "Criar política em linha";
12. No combo de serviços do formulário que será aberto, selecione a opção EC2, marque a opção "Todas as ações do EC2 (ec2:*)" que irá aparecer, e em Recursos marque a opção "Tudo", logo abaixo irá aparecer um botão "Adicionar mais permissões", clique nele e repita o mesmo processo que fez com o EC2 para os seguintes serviços: EKS, IAM e CloudWatch Logs;
13. Após avançar, defina um nome e clique em "Criar política";
14. Após isso, ainda no menu de Permissões, clique em "Adicionar permissões" mais um vez, porém dessa vez, selecione a opção "Adicionar permissões" ao invés de "Criar política em linha"; 
15. Na tela que irá aparecer, selecione a opção "Anexar políticas diretamente";
16. Pesquise pela permissão "AmazonEC2ContainerRegistryPowerUser" e adicione ela;
17. Após isso, de volta a tela de detalhes do usuário, clique na aba "Credenciais de Segurança", e no bloco "Chaves de acesso", clique em "Criar chave de acesso";
18. Na tela que irá se abrir, selecione a opção "Command Line Interface (CLI)" e clique em próximo;
19. No valor da etiqueta, coloque o valor "github actions" ou qualquer um que prefira para identificar posteriormente; 
20. Copie os valores dos campos "Chave de acesso" e "Chave de acesso secreta";
21. Na plataforma do Github, acesse o menu "Settings" do projeto, na tela que se abrir, clique no menu Security->Secrets and variables->Actions;
22. Adicione uma "repository secret" chamada AWS_ACCESS_KEY_ID com o valor copiado de "Chave de acesso", e crie outra "repository secret" chamada AWS_SECRET_ACCESS_KEY com o valor copiado de "Chave de acesso secreta";
23. Após isso qualquer commit neste repositório que for para a branch "main", irá subir um cluster no EKS e toda a estrutura Kubernetes utilizando os arquivos yamls;
24. Após o pipeline ser concluído, antes de testar qualquer coisa, aguarde uns 5 minutos até que toda a estrutura tenha concluído todo o mapeamento interno da AWs;


### Validação da execução do Kubernetes

- A plataforma da AWS não permite inicialmente que um usuário root visualize os recursos do kubernetes nos detalhes do Cluster EKS, para que isso seja possível é necessário seguir um tutorial criado por eles mesmos:

(https://docs.aws.amazon.com/eks/latest/userguide/view-kubernetes-resources.html#view-kubernetes-resources-permissions)https://docs.aws.amazon.com/eks/latest/userguide/view-kubernetes-resources.html#view-kubernetes-resources-permissions

- Porém conseguimos visualizar toda a estrutura através do CLI ou chamando os endpoints da api mesmo.

# CLI:

```sh

// Será solicitado os seguintes valores neste comando:
// AWS Access Key ID: o mesmo de AWS_ACCESS_KEY_ID configurado em Settings->Security->Secrets and variables->Actions
// AWS Secret Access Key: o mesmo de AWS_SECRET_ACCESS_KEY configurado em Settings->Security->Secrets and variables->Actions
// Default region name: us-east-2
aws configure

aws eks update-kubeconfig --name fiap-tech-challenge-infra-k8s --region=us-east-2

kubectl get pods

kubectl describe pods
```

# Chamando os endpoints:

1. Na plataforma da AWS, vá até EC2 e clique em "Load balancers" e depois clique no Load Balancer criado;
2. Na tela que irá se abrir copie o valor do "Nome do DNS", ele será semelhante a este valor "acb1520c01aaf4b2cb76ddf05e045720-765794247.us-east-2.elb.amazonaws.com";
3. Após isso, concatene o valor copiado com a porta e a url de prova de vida(/liveness), depois disso, cole na barra de navegação do browser mesmo, o valor que você irá inserir na barra de navegação do browser será semelhante ao abaixo:

```sh
http://acb1520c01aaf4b2cb76ddf05e045720-765794247.us-east-2.elb.amazonaws.com:3000/liveness
```
