# Roteiro de reprodução manual

Este roteiro organiza a arquitetura para uma nova prática. Não foi executado novamente e não constitui um deploy automatizado. Use uma conta própria ou uma conta de treinamento com autorização para cada serviço; estime os custos antes de iniciar. Não são necessários recursos AWS em execução para publicar a documentação.

## 1 Rede e acesso

Confirme a região, duas AZs, duas sub-redes públicas e quatro privadas. Revise as rotas do Internet Gateway e a saída privada via NAT. Na arquitetura do projeto de Marcos Mendes, há dois NAT Gateways zonais, um em cada sub-rede pública. Ao reproduzir, associe as rotas privadas ao NAT da respectiva AZ. Crie um grupo para o ALB e outro para os frontends, permitindo HTTP nas EC2 apenas a partir do grupo do ALB. Consumidores da fila não precisam receber tráfego HTTP do ALB.

## 2 Dados e mensageria

Crie uma tabela DynamoDB com requestId do tipo String como chave de partição, conforme o modelo observado. Crie uma fila SQS Standard e uma DLQ; associe a DLQ à fila principal com maxReceiveCount de 3. Revise retenção e timeout em cada fila separadamente. O roteiro registra 14 dias de retenção e 30 segundos de visibilidade na DLQ; configure o timeout da fila principal pelo tempo real de processamento.

## 3 Aplicação e permissões

É necessário fornecer a implementação do frontend e do consumidor, que não está neste repositório. O frontend recebe o formulário e envia JSON com requestId, data e timestamp ao SQS. O backend consome, grava requestId, data, processedAt e status no DynamoDB e só então exclui a mensagem. Use IAM Roles em vez de chaves estáticas. Na nova versão, prefira roles separadas e menor privilégio a repetir as políticas FullAccess do laboratório.

Exemplo de contrato, sem conexão com a conta real:

```json
{
  "requestId": "exemplo-de-identificador-unico",
  "data": "mensagem de teste",
  "timestamp": "2026-11-07T12:00:00Z"
}
```

## 4 Balanceamento

Crie um target group para os frontends em HTTP na porta 80, health check no caminho / e intervalo de 10 segundos como no roteiro. Associe o ALB às duas sub-redes públicas e valide os destinos antes de enviar dados. O teste inicial deve ser feito apenas com informações de exemplo.

## 5 Capacidade e distribuição

Crie Launch Templates e ASGs separados para frontend e backend. A referência do laboratório é mínimo 1, máximo 2 e alvo de CPU de 60%; para redundância simultânea em duas AZs é necessário rever o mínimo. Associe somente o grupo frontend ao target group e revise health checks, aquecimento e tempo de inicialização.

Configure CloudFront com o ALB como origem. O registro original usa HTTP na origem; para uma evolução, use certificado compatível e HTTPS. Permita os métodos necessários ao formulário, encaminhe os campos usados pela aplicação e desabilite cache onde houver resposta dinâmica ou personalizada. Valide o POST até o backend. Restrinja o ALB com prefix list e, como melhoria, valide um cabeçalho exclusivo da distribuição.

## 6 Configuração e operação

Use Parameter Store para configurações e controles apropriados para segredos. Monte o dashboard com métricas do ALB, ASGs e SQS. Crie SNS, confirme a assinatura por e-mail e associe os alarmes. Habilite PITR com a janela desejada e valide a restauração em um ensaio separado. Avalie Cost Anomaly Detection, sem tratar o serviço como bloqueio automático de consumo.

## 7 Validar e encerrar

Siga o plano de testes, registre resultados observados e revise o custo real. Ao terminar, remova somente recursos exclusivos do projeto, respeitando as dependências. Em conta compartilhada, combine o encerramento com o responsável.
