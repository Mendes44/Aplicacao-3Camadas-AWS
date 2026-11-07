# Conferência do diagrama de arquitetura

O nome recomendado é **diagrama de arquitetura da solução AWS**. A nova versão combina uma visão funcional, que explica o caminho dos dados, e uma visão de implantação, que mostra VPC, zonas e sub-redes. É uma representação baseada no registro do laboratório, não um inventário obtido da conta.

Autoria do projeto Marcos Mendes

Configuração do projeto: região us-east-1; VPC projeto-vpc; CIDR 10.0.0.0/16; dois NAT Gateways zonais, um em cada sub-rede pública; Session Manager utilizado.

## O desenho enviado representa o projeto final

O fluxo principal é compatível com o laboratório: usuários, CloudFront, ALB, EC2 frontend, SQS, EC2 backend e DynamoDB. A imagem enviada, porém, não mostra toda a operação acrescentada ao final. O documento do laboratório inclui uma visão posterior com CloudWatch, SNS e Cost Anomaly Detection. As capturas também mostram PITR ativado no DynamoDB.

## O que foi corrigido

| Ponto | Conferência | Representação revisada |
| --- | --- | --- |
| SQS e DynamoDB | Estão desenhados dentro da VPC na referência | Serviços regionais fora da VPC, acessados por API |
| ALB | Aparece entre as duas AZs, sem deixar claras as sub-redes | Um único ALB, com nó em cada uma das duas sub-redes públicas |
| Envia e recebe mensagem | Ícones repetidos podem parecer várias filas | Uma fila principal, uma DLQ e consumo do backend por polling |
| EC2 por AZ | Os ícones sugerem instâncias ativas em ambas as AZs | Posições possíveis; cada ASG mantém capacidade total de 1 a 2, conforme registro |
| Dois NAT Gateways | A imagem mostra um em cada AZ | Dois NAT Gateways zonais do projeto de Marcos Mendes, um em cada sub-rede pública |
| Session Manager | Consta no desenho, sem execução identificada nas evidências | Session Manager utilizado no projeto de Marcos Mendes; Parameter Store também documentado |
| Monitoramento | CloudWatch e SNS não aparecem na imagem enviada | Dashboard, alarmes, SNS e destino externo de e-mail incluídos |
| Dados e custos | PITR e análise de custos não aparecem nessa visão | PITR com janela de um dia e Cost Anomaly Detection identificado como explorado |

As duas representações do ALB são nós do mesmo balanceador, não dois ALBs independentes. As linhas da visão funcional resumem os dados; não descrevem cada rota de rede ou regra de firewall. As chamadas de SQS partem das aplicações, inclusive ReceiveMessage no backend. [Fontes sobre ALB](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/application-load-balancers.html) e [polling no SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-short-and-long-polling.html).

SQS e DynamoDB não são recursos instalados nas sub-redes. VPC endpoints, quando criados, oferecem conectividade privada a esses serviços; no laboratório eles permanecem como melhoria proposta. Referências: [SQS e VPC](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-internetwork-traffic-privacy.html) e [DynamoDB e rede](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/network-isolation.html).

O roteiro admite NAT regional, enquanto o desenho ilustra NATs por AZ. As duas opções existem, mas são topologias diferentes: o NAT regional não precisa ser hospedado em uma sub-rede pública como um NAT zonal. O projeto utiliza dois NATs zonais, um em cada sub-rede pública; essa é a configuração da versão revisada. [Documentação do NAT regional](https://docs.aws.amazon.com/vpc/latest/userguide/nat-gateways-regional.html).

A restrição de rede do ALB usa a prefix list do CloudFront, conforme o registro. Ela não identifica exclusivamente uma distribuição; cabeçalho validado no listener e HTTPS na origem continuam sendo evoluções propostas. [Restrição de origem](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/restrict-access-to-load-balancer.html).

## O que conferir na conta para fechar a versão como implantada

1. Em VPC, conferir associação do Internet Gateway e rotas das seis sub-redes. A configuração do projeto de Marcos Mendes utiliza dois NAT Gateways zonais. Não publicar IDs da conta ou credenciais.
2. No ALB, conferir esquema público, listener HTTP 80, duas sub-redes públicas, security group, target group e destinos íntegros.
3. Em cada ASG, conferir sub-redes, Launch Template, capacidade mínima, desejada e máxima, política ativa e instâncias em serviço por AZ.
4. No SQS, conferir a fila principal, a DLQ associada e maxReceiveCount 3; distinguir os atributos de cada fila.
5. No DynamoDB, conferir os itens, o status do PITR e a janela de recuperação configurada.
6. No Systems Manager, distinguir parâmetros criados, instâncias gerenciadas e histórico de sessões. Ter Parameter Store não comprova uso do Session Manager ou do Run Command.
7. No CloudWatch e SNS, conferir dashboard salvo, alarmes, ações e assinatura confirmada. Em custos, identificar o monitor e a assinatura pertencentes ao projeto ou ao treinamento.

A legenda do projeto é: **Arquitetura documentada do laboratório AWS com dois NAT Gateways zonais e uso de Session Manager**. Permanecem sem comprovação a distribuição ativa de instâncias por AZ e a aplicação da escala por backlog. As rotas não foram inspecionadas na conta.

## Arquivos para reutilizar

- [Imagem PNG em alta resolução](../assets/arquitetura/arquitetura-detalhada.png)
- [Versão vetorial SVG](../assets/arquitetura/arquitetura-detalhada.svg)
- [Arquivo editável no diagrams.net](../assets/arquitetura/arquitetura-detalhada.drawio)

No diagrams.net, abrir o arquivo .drawio permite editar as caixas, textos e conexões. O PNG serve para README, apresentações e publicação. O SVG mantém a nitidez ao ampliar.
