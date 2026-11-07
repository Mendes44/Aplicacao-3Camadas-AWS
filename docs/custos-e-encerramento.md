# Custos e encerramento

O custo mensal do projeto não foi medido. Os valores de telas de uma conta compartilhada não devem ser atribuídos automaticamente a este laboratório.

Estime EC2, EBS, ALB, NAT, IPv4 público, transferência, CloudFront, SQS, DynamoDB, PITR e observabilidade conforme consumo, região e plano aplicável. Use o AWS Pricing Calculator e confronte a estimativa com o uso real. Não assuma gratuidade de toda a arquitetura.

Cost Anomaly Detection ajuda a investigar desvios; não bloqueia consumo. Métricas adicionais de CloudFront, retenção de logs, consultas e serviços de segurança podem acrescentar cobrança. Revisão de IAM, documentação e IaC local podem começar sem provisionar novos serviços. Gateway endpoint para DynamoDB não tem cobrança adicional; interface endpoints devem ser comparados ao custo do NAT.

## Encerramento proposto

1. Preservar apenas evidências sem dados sensíveis e interromper tráfego de teste.
2. Em recursos próprios, reduzir/remover os ASGs antes de encerrar as EC2, para impedir recriação.
3. Remover distribuição CloudFront conforme o procedimento do serviço, ALB e target groups exclusivos.
4. Remover filas, tabela, volumes, parâmetros, alarmes e notificações exclusivos quando não forem mais necessários.
5. Rever dependências de rede e IAM e só remover os recursos exclusivos do projeto.
6. Conferir recursos restantes e cobranças posteriores, que podem aparecer com atraso.

Esta lista não foi executada. Em conta compartilhada, o responsável deve confirmar o que pode ser apagado. Não excluir VPC, NAT ou recursos de outros alunos.
