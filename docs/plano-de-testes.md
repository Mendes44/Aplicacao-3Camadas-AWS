# Plano de testes e coleta de evidências

Os testes a seguir são propostos; não foram executados na preparação do portfólio.

| Teste | Critério de aceitação | Registrar |
| --- | --- | --- |
| Caminho funcional | O requestId aceito aparece na tabela após consumo | ID, envio, processamento e horários |
| Falha de frontend | Health check retira o destino e ASG repõe a capacidade | Capacidade inicial, eventos e impacto |
| Falha de backend | Mensagens aguardam e são retomadas após recuperação | Profundidade, idade e registros |
| DLQ | Mensagem com falha persistente é encaminhada após os recebimentos configurados | Motivo, tentativas e redrive |
| Duplicata | Mesmo requestId produz um único efeito lógico | Estado antes e depois |
| Escala | Capacidade muda dentro dos limites e reduz atraso | Política, métrica e eventos |
| Proteção de origem | Caminho CloudFront funciona e acesso direto é recusado | Respostas e condições de rede |
| PITR | Tabela é restaurada e aplicação passa a ler os dados corretos | Ponto escolhido, nova tabela e validação |

Para cada teste, preencher: data, versão, condição inicial, ação, resultado esperado, resultado observado, evidência e limitações. Não marcar como aprovado antes de executar. Usar dados fictícios e interromper o ensaio ao ultrapassar o escopo definido.
