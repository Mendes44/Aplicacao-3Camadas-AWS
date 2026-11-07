# Procedência das evidências

As capturas foram extraídas do documento fornecido pelo autor e recortadas para manter somente a área relevante. Não representam uma nova execução. Números de conta, cabeçalhos de sessão e listas de recursos de outros participantes foram excluídos dos arquivos selecionados.

| Arquivo | O que sustenta | O que não comprova |
| --- | --- | --- |
| 01-formulario | Interface com retorno de envio | Conclusão do processamento |
| 02-registros-dynamodb | Cinco itens exibidos com status processed | Correlação de uma transação e ausência de duplicatas |
| 03-dashboard | Métricas reunidas no painel | SLA, benchmark ou contagem correta sem revisar a estatística |
| 04-acesso-alb | Regra de rede da origem | Exclusividade da distribuição CloudFront |
| 05-pitr | PITR ativado com janela de um dia | Restauração testada |
| 06-alarme | Notificação de alarme recebida | Tempo de recuperação medido |
| 07-dlq-configuracao | Máximo de três recebimentos | Atributos de todas as filas |
| 08-dlq-mensagens | Mensagem visível durante o ensaio | Fila de origem, chegada à DLQ ou redrive com sucesso |
| 09-capacidade-frontend | Configuração de capacidade | Distribuição ativa e contínua nas duas AZs |
| 10-politica-cpu | Política com alvo de CPU | Escala por backlog implementada |

O diagrama em assets/arquitetura foi criado para explicar o fluxo lógico. Não é exportação do inventário da conta nem prova de implantação. Os arquivos originais e a apostila não devem ser enviados com este pacote.
