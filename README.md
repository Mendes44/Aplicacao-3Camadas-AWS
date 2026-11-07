# Arquitetura em três camadas na AWS

Estudo de caso de Marcos Mendes, desenvolvido a partir de um laboratório da Semana de Arquitetura da AWS, no contexto de estudos da Escola da Nuvem.

Construí uma aplicação que recebe solicitações em um frontend EC2, usa SQS para desacoplar o processamento e persiste os resultados no DynamoDB. Este repositório documenta o que fiz, as decisões e os limites observados.

![Arquitetura lógica](assets/arquitetura/arquitetura-logica.png)

## Diagrama de implantação

![Arquitetura detalhada](assets/arquitetura/arquitetura-detalhada.png)

O diagrama mostra posições possíveis dos grupos e separa os serviços regionais da VPC. [Confira a validação e as pendências](docs/validacao-diagrama.md).

## Fluxo da aplicação

Navegador → CloudFront → ALB → EC2 frontend → SQS → EC2 backend → DynamoDB.

As três camadas lógicas são apresentação, processamento e persistência. SQS desacopla os processos. CloudWatch, SNS, IAM e Parameter Store apoiam a operação.

## Configuração confirmada

Configuração confirmada pelo autor nesta conversa: região us-east-1; VPC projeto-vpc; CIDR 10.0.0.0/16; dois NAT Gateways zonais, um em cada sub-rede pública; Session Manager utilizado.

## O que foi documentado

- VPC em duas AZs, ALB em sub-redes públicas e EC2 em sub-redes privadas.
- IAM Role, security groups e parâmetros de configuração.
- Launch Templates e Auto Scaling separados para frontend e backend, com capacidade de 1 a 2.
- CloudFront com HTTPS para o usuário e HTTP para o ALB.
- SQS Standard, DLQ com máximo de três recebimentos e registros no DynamoDB.
- Dashboard CloudWatch, alertas SNS por e-mail e restrição de origem do ALB.
- PITR ativado no DynamoDB com janela de um dia na captura.

## Evidências

![Itens processados no DynamoDB](assets/evidencias/02-registros-dynamodb.png)

As capturas selecionadas estão em [assets/evidencias](assets/evidencias). Elas representam estados pontuais do laboratório. A identidade da conta e áreas com recursos de outros participantes foram excluídas dos recortes.

## Decisões e limites

Uma instância por grupo não garante duas AZs ativas simultaneamente. A escala por backlog é discutida como evolução, pois a aplicação da política não está comprovada. O projeto ainda usa permissões amplas, origem HTTP e configuração manual. Não há benchmark, custo mensal atribuído ao projeto, SLA ou teste completo de DR.

## Melhorias propostas

IaC, menor privilégio, idempotência, HTTPS na origem, logs centralizados, OpenTelemetry, CI/CD e testes de restauração são próximos passos. Nenhuma dessas propostas é declarada como entregue. O relatório detalha as 18 possibilidades de evolução e seus critérios.

## Documentação

- [Estudo de caso completo](docs/estudo-de-caso.md)
- [Roteiro de reprodução manual](docs/reproducao-manual.md)
- [Plano de testes e coleta de evidências](docs/plano-de-testes.md)
- [Custos e encerramento](docs/custos-e-encerramento.md)
- [Procedência das evidências](docs/evidencias.md)
- [Referências técnicas](docs/referencias.md)

Este é um repositório de documentação. Não inclui uma implementação executável, código da aplicação ou infraestrutura como código. O laboratório foi construído manualmente; a versão reproduzível em código é uma evolução futura.

## Créditos

Prática baseada na Semana de Arquitetura da AWS, no contexto da Escola da Nuvem. A documentação e o diagrama foram reorganizados para este portfólio. A apostila original e arquivos de credenciais não integram o repositório. Não foi aplicada licença aberta a materiais de terceiros.
# Aplicação-3Camadas-AWS
