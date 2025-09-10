📘 Documentação Básica de Flutter
Foco: Componentes com/sem estado e Comunicação entre Widgets

🟢 1. Estrutura do Projeto

O Flutter organiza a interface em widgets, que podem ser:

🔹StatelessWidget → não mantém estado (imutável).

🔹StatefulWidget → mantém estado (dinâmico, pode mudar com interações).

No código exemplo temos:

🔹PerguntaApp (StatefulWidget) → gerencia o estado principal (pergunta atual e pontuação).

🔹Questionario, Questao, Resposta, Resultado (StatelessWidget) → recebem dados prontos e apenas exibem.

🟢 2. Componentes sem estado (StatelessWidget)

Um StatelessWidget é usado quando o conteúdo do widget não muda durante a execução.

Exemplo:

class Questao extends StatelessWidget {
  final String texto;

  const Questao(this.texto, {super.key});

  @override
  Widget build(BuildContext context) {
    return Container(
      width: double.infinity,
      margin: const EdgeInsets.all(10),
      child: Text(
        texto,
        style: const TextStyle(fontSize: 28),
        textAlign: TextAlign.center,
      ),
    );
  }
}


👉 Aqui, Questao só recebe o texto e exibe, não muda internamente.

🟢 3. Componentes com estado (StatefulWidget)

Um StatefulWidget é usado quando o widget precisa armazenar e modificar valores durante a execução.

Exemplo:

class PerguntaApp extends StatefulWidget {
  const PerguntaApp({super.key});

  @override
  _PerguntaAppState createState() {
    return _PerguntaAppState();
  }
}

class _PerguntaAppState extends State<PerguntaApp> {
  var _perguntaSelecionada = 0;
  var _pontuacaoTotal = 0;

  void _responder(int pontuacao) {
    setState(() {
      _perguntaSelecionada++;
      _pontuacaoTotal += pontuacao;
    });
  }
}


👉 Aqui, _perguntaSelecionada e _pontuacaoTotal são variáveis de estado.
Sempre que chamamos setState(), o Flutter reconstrói a interface com os novos valores.

🟢 4. Comunicação direta: Pai → Filho

A comunicação do componente pai para o filho acontece via parâmetros no construtor.

Exemplo (Questionario recebe dados do PerguntaApp):

Questionario(
  perguntas: _perguntas,
  perguntaSelecionada: _perguntaSelecionada,
  quandoResponder: _responder,
)


No Questionario, usamos:

final List<Map<String, Object>> perguntas;
final int perguntaSelecionada;
final void Function(int) quandoResponder;


👉 Isso permite que o pai envie dados prontos para o filho.

🟢 5. Comunicação direta com callback: Filho → Pai

Quando precisamos que o filho avise algo ao pai, passamos uma função como parâmetro.

Exemplo (Resposta chama a função passada do pai):

Resposta(
  resp['texto'] as String,
  () => quandoResponder(resp['pontuacao'] as int),
);


No Resposta:

class Resposta extends StatelessWidget {
  final String texto;
  final void Function() quandoSelecionado;

  const Resposta(this.texto, this.quandoSelecionado, {super.key});

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: quandoSelecionado,
      child: Text(texto),
    );
  }
}


👉 Aqui temos um callback: ao clicar no botão, o filho chama quandoSelecionado, que na prática executa a função _responder do pai.

🟢 6. Ciclo de Funcionamento do Exemplo

🔹PerguntaApp mostra a tela inicial.

🔹O usuário clica em uma resposta (Resposta).

🔹O filho chama a função enviada pelo pai → _responder.

🔹_responder atualiza o estado (setState).

🔹O Flutter reconstrói a tela mostrando a próxima pergunta.

🔹Quando não há mais perguntas, o widget Resultado é exibido.

🟢 7. Pontos Fundamentais do Flutter

✅ Tudo é widget → botões, textos, colunas, linhas, etc.
✅ Stateless vs Stateful → escolha dependendo da necessidade de estado.
✅ setState() → mecanismo para atualizar a tela.
✅ Comunicação Pai → Filho → parâmetros no construtor.
✅ Comunicação Filho → Pai → funções/callbacks passadas pelo pai.
✅ Reatividade → sempre que o estado muda, o Flutter reconstrói os widgets visíveis.

🟢 8. Diagrama Simplificado
PerguntaApp (StatefulWidget)
 ├── Questionario (StatelessWidget)
 │    ├── Questao (StatelessWidget)
 │    └── Resposta (StatelessWidget) -> callback para PerguntaApp
 └── Resultado (StatelessWidget) -> recebe pontuação e função de reinício


👉 Essa documentação cobre a base essencial para entender o Flutter:

🔹Diferença entre widgets com/sem estado.

🔹Comunicação entre componentes.

🔹Atualização de interface com setState.