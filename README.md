# fiap-tech-challenge-infra-k8s

### Tech Challenge 3:
### Passos para homologação dos professores da Fiap

Foi utilizada a nuvem da Amazon (AWS) para este tech challenge:

1. Faça o login na plataforma da AWS;
2. Acesse IAM->Usuários e crie um novo usuário chamado Github;
3. Com esse usuário criado, vá até a listagem de usuários e acesse os detalhes do mesmo;
4. No menu Permissões que irá aparecer na tela de detalhes, clique no botão "Adicionar permissões" que aparece no canto direito e selecione a opção "Criar política em linha";
5. Em serviços, selecione (EC2, EKS, IAM e CloudWatch), selecione a opção de cada um "Todas as ações do..." e em Recursos de cada um, marque a opção "Tudo";
6. Após avançar, defina um nome e clique em "Criar política";
7. Após isso, de volta a tela de detalhes do usuário, clique na aba "Credenciais de Segurança", e no bloco "Chaves de acesso", clique em "Criar chave de acesso";
8. Na tela que irá se abrir, selecione a opção "Command Line Interface (CLI)" e clique em próximo;
9. No valor da etiqueta, coloque o valor "github actions" ou qualquer um que prefira para identificar posteriormente;
10. Copie os valores dos campos "Chave de acesso" e "Chave de acesso secreta";
11. Na plataforma do Github, acesse o menu "Settings" do projeto, na tela que se abrir, clique no menu Security->Secrets and variables->Actions;
12. Adicione uma "repository secret" chamada AWS_ACCESS_KEY_ID com o valor copiado de "Chave de acesso", e crie outra "repository secret" chamada AWS_SECRET_ACCESS_KEY com o valor copiado de "Chave de acesso secreta";
13. Após isso qualquer commit ou pull request neste repositório que for para a branch "main", irá subir um cluster no EKS e toda a estrutura Kubernetes utilizando os arquivos yamls;

### Validação da execução do Kubernetes

- A plataforma da AWS não permite inicialmente que um usuário root visualize os recursos do kubernetes nos detalhes do Cluster EKS, para que isso seja possível é necessário seguir um tutorial criado por eles mesmos:

(https://docs.aws.amazon.com/eks/latest/userguide/view-kubernetes-resources.html#view-kubernetes-resources-permissions)https://docs.aws.amazon.com/eks/latest/userguide/view-kubernetes-resources.html#view-kubernetes-resources-permissions

- Porém conseguimos visualizar toda a estrutura através do CloudShell. Para isso acesse ele e após ele abrir, digite os comandos abaixo:

```sh
aws eks update-kubeconfig --name fiap-tech-challenge-infra-k8s --region=us-east-1 
kubectl get pods
// e qualquer outro comando kubectl que deseje executar
```

