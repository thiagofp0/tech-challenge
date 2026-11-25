# Dicionário de Linguagem Ubíqua

## Entidades e Agregados

- **Cliente:** Pessoa física ou jurídica que solicita um serviço e é identificada por um documento (CPF/CNPJ). É quem recebe notificações e aprova ou recusa orçamentos.
- **Veículo:** Bem pertencente ao cliente sobre o qual o serviço é realizado. É identificado por um código único (placa) e vinculado a um cliente. Em outro setor, seria o "ativo" ou "equipamento".
- **Ordem de Serviço (OS):** Registro central que acompanha todo o ciclo de um atendimento, do recebimento à entrega. Reúne serviços, peças, orçamento e status.
- **Peça/Insumo:** Item material cadastrado e controlado em estoque, que pode ser consumido na execução de uma OS.

## Atores

- **Cliente:** Quem solicita o serviço, consulta o andamento e decide sobre o orçamento.
- **Atendente:** Profissional que recebe o cliente, faz os cadastros, abre a OS e conduz a entrega ao final.
- **Mecânico:** Profissional técnico que realiza o diagnóstico e executa o serviço. Em outro setor, seria o "técnico" ou "executor".
- **Administrador:** Responsável pela gestão do cadastro e do estoque de peças/insumos.

## Sistemas Externos

- **Provedor de Serviço de E-mail:** Serviço de terceiros usado para enviar notificações e receber as respostas do cliente.

## Identificação e Cadastro

- **Cadastro de Cliente:** Processo de registrar um novo cliente. Começa com o início do cadastro e a validação do documento, e termina com o cliente cadastrado.
- **Cadastro de Veículo:** Processo de registrar um bem vinculado a um cliente, com validação do identificador único.
- **Validação de CPF/CNPJ:** Verificação de que o documento informado é válido antes de concluir o cadastro do cliente.
- **Validação de Placa:** Verificação de que o identificador do veículo é válido antes de concluir o cadastro.
- **Identificação de Cliente:** Busca de um cliente já existente no sistema. O resultado é "cliente identificado" ou "não identificado", e este último leva ao cadastro.
- **Identificação de Veículo:** Busca do bem na lista de veículos do cliente. O resultado é "veículo identificado" ou "não identificado", e este último leva ao cadastro.
- **Autenticação do Cliente:** Confirmação de identidade do cliente para que ele acesse o sistema.

## Atendimento e Ordem de Serviço

- **Atendimento:** Interação entre cliente e empresa que origina uma OS. Tem início e fim explícitos.
- **Inicialização da OS:** Abertura formal de uma ordem de serviço, que passa ao status "Recebida".
- **Serviço Solicitado:** Serviço pedido explicitamente pelo cliente e adicionado à OS.
- **Serviço Não Solicitado:** Serviço identificado pela empresa como necessário, mas não pedido pelo cliente. É adicionado à OS e passa pelo mesmo processo de aprovação.
- **Adição de Peça à OS:** Inclusão de um item de estoque na OS como componente previsto do serviço.
- **Status da OS:** Estado atual da OS no fluxo. Os estados identificados são: Recebida, Em Diagnóstico, Aguardando Aprovação, Em Execução, Finalizada e Entregue.
- **Consulta de Status:** Solicitação (via API) do estado atual da OS, que retorna o status vigente.

## Diagnóstico

- **Designação de Mecânico:** Atribuição de um profissional responsável pela avaliação técnica.
- **Diagnóstico:** Avaliação técnica que identifica o que precisa ser feito. Tem início e fim registrados no sistema.
- **Identificação de Peça:** Reconhecimento, durante o diagnóstico, dos itens necessários para o serviço.

## Orçamento

- **Orçamento:** Estimativa de serviços e peças a ser aprovada pelo cliente antes da execução. É gerado automaticamente ao fim do diagnóstico.
- **Aprovação do Orçamento:** Decisão do cliente que autoriza a execução do serviço.
- **Reprovação do Orçamento:** Decisão do cliente que recusa o orçamento e impede a execução.
- **Resposta por E-mail:** Canal pelo qual o cliente confirma ou recusa o orçamento.

## Execução e Entrega

- **Execução do Serviço:** Realização do trabalho pelo profissional. Tem início e fim registrados no sistema.
- **Finalização do Serviço:** Conclusão da execução, que muda o status da OS para "Finalizada" e dispara a notificação ao cliente.
- **Liberação para Entrega:** Fim do atendimento, quando o bem está pronto para ser retirado.
- **Entrega:** Retirada do bem pelo cliente. Registrada no sistema, move a OS para "Entregue".

## Notificações

- **Notificação:** Comunicação enviada ao cliente sobre marcos relevantes. Os marcos são: orçamento gerado e serviço finalizado.
- **Notificação por E-mail:** Notificação enviada pelo provedor de e-mail, com orçamento para aprovação ou aviso de conclusão.

## Estoque

- **Estoque:** Quantidade disponível de cada peça/insumo.
- **Cadastro de Peça/Insumo:** Registro de um novo item no catálogo, que pode ser editado, visualizado ou excluído.
- **Entrada de Estoque:** Acréscimo de unidades de um item já cadastrado.
- **Baixa de Estoque:** Redução automática da quantidade quando o item é usado em uma OS.
- **Reserva de Peça:** Separação de um item para uma OS em execução.
- **Abdicação de Reserva:** Liberação de uma peça antes reservada, que devolve o item à disponibilidade.
- **Utilização de Peça:** Consumo efetivo do item em uma OS.

## Políticas (Regras de Negócio)

- **Início da OS → Status "Recebida":** Ao abrir a OS, o status passa automaticamente para "Recebida".
- **Início do Diagnóstico → Status "Em Diagnóstico":** O diagnóstico iniciado atualiza o status da OS.
- **Fim do Diagnóstico → "Aguardando Aprovação" e Geração do Orçamento:** Encerrar o diagnóstico gera o orçamento e coloca a OS em espera pela decisão do cliente.
- **Orçamento Gerado → Notificação ao Cliente:** O cliente é avisado por e-mail e solicitado a aprovar.
- **Início do Serviço → Status "Em Execução":** Iniciar a execução atualiza o status.
- **Fim do Serviço → Status "Finalizada" e Notificação:** Concluir o serviço atualiza o status e avisa o cliente.
- **Retirada do Bem → Status "Entregue":** A saída do bem registra a entrega.
- **Uso de Peça → Baixa de Estoque:** Consumir um item em uma OS diminui o saldo.
- **Validação de Documento e Identificador:** CPF/CNPJ e placa são validados antes de concluir os cadastros.
- **Consulta de Status:** Uma requisição de consulta retorna o status atual da OS.

## Modelos de Leitura (Visões de Consulta)

- **Listagem de Clientes:** Visão geral dos clientes cadastrados.
- **Veículos do Cliente:** Visão dos bens vinculados a um cliente.
- **Listagem de OS Recebidas:** Visão das ordens abertas e aguardando tratamento.
- **Listagem de Peças Cadastradas:** Visão do catálogo e do saldo de estoque.
- **Detalhes do Item:** Visão detalhada de uma peça/insumo.

