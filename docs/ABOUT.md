# 💻 Sobre o desafio

Nesse desafio, você irá implementar o GitHub Explorer: um aplicativo que consome a API do GitHub e exibe informações de qualquer repositório público a partir da busca pelo `usuario/nome-do-repositorio`, exemplo: `facebook/react`.

## Template da aplicação

Para realizar esse desafio, criamos para você esse modelo que você deve utilizar como um template do GitHub.

O template está disponível na seguinte URL:

[rocketseat-education/ignite-template-react-native-github-explorer](https://github.com/rocketseat-education/ignite-template-react-native-github-explorer)

<aside>
💡 **Dica**: Caso não saiba utilizar repositórios do GitHub como template, temos um guia em **[nosso FAQ](https://www.notion.so/FAQ-Desafios-ddd8fcdf2339436a816a0d9e45767664).**

</aside>

## Como deve ficar a aplicação ao final?

Está com dúvidas (ou curioso 👀) para ver como deve ficar a aplicação ao final do desafio? Deixamos abaixo um vídeo mostrando as principais funcionalidades que você deve implementar para te ajudar (ou matar sua curiosidade 👀).

<h4 align="center">
    <h1 align="center">
      <img width="200" border-radius: 10px" height="auto" alt="demo" title="Screenshot" src="videos/demo.gif" />
    </h1>
    <br><br>
</h4>

## O que devo editar na aplicação?

Com o template já clonado e as dependências instaladas, você deve completar onde não possui código com o código para atingir os objetivos de cada teste.

Nesse desafio, você deve editar os seguintes arquivos para completar as funcionalidades da aplicação:

- `[src/components/Card/CardAnimation/index.tsx]`
- `[src/screens/Dashboard/index.tsx]`
- `[src/screens/Repository/index.tsx]`

### `[src/components/Card/CardAnimation/index.tsx]`

Esse é o componente que será usado para exibir as informações dos repositórios na primeira tela e também para exibir as informações de issues abertas na tela de detalhes do repositório.

<aside>
💡 Observe que o componente **CardAnimation** serve apenas para englobar o componente **Card**. Ele é responsável pelas estilizações animadas. Você não precisará alterar o componente **Card**, que recebe as informações a serem renderizadas na tela.

</aside>

Aqui, você deve completar dois trechos para garantir que as animações do componente aconteçam como esperado. Observe onde está comentado no código para seguir as dicas:

- **TODO - setup animated style**
  Aqui você deve usar os valores `cardOpacity.value` e `cardOffset.value` para retornar um objeto com as estilizações de **opacidade** e **posição no eixo X** respectivamente.
  **Dica:** Você pode apertar `Ctrl + Espaço` ou `command + Espaço` dentro do objeto para o auto-complete do VS Code te ajudar com a estilização.
  ***
- **TODO - setup cardOpacity.value and cardOffset.value with `withTiming()`**
  Dentro do `useEffect`, você vai usar a função `withTiming` para alterar os valores `cardOpacity.value` e `cardOffset.value`.
  1. Observe que o `cardOpacity.value` foi iniciado com `0`. Usando o `withTiming`, você deve alterar o valor para `1` com uma duração de 1000ms (1 segundo).
  2. Já para o `cardOffset.value`, você deve alterar o valor para `0` com a mesma duração de 1000ms.

     ❓ Mas o que significa aquele valor que foi iniciado no useSharedValue de `0.25 * displayWidth`?

     ✅ Esse é um valor equivalente a 25% da largura da tela. Suponhamos que a largura da tela seja de `750px`, então o `cardOffset.value` fará com que o componente seja renderizado inicialmente a `187.5px` sobressaindo à direita da tela (já que você definiu isso no retorno do `useAnimatedStyle`. O que o useEffect irá fazer é que esse valor seja `0` durante a renderização do componente, fazendo uma animação da direita até o centro da tela.

     ***

### `[src/screens/Dashboard/index.tsx]`

Essa é a primeira tela da aplicação. Ela será responsável pela busca de repositórios no GitHub e listagem dos repositórios encontrados.

Aqui você usará um hook `useRepositories` feito para fazer as requisições necessárias e gerenciar os dados dos repositórios. Não será necessário modificar nada no contexto ou no hook.

O hook retorna quatro informações mas listaremos aqui as que serão usadas na screen `Dashboard`:

- `addRepository`: Uma função que recebe o nome de um repositório no formato `user/repo-name` e busca as informações do repositório e das issues abertas na API do GitHub.
- `repositories`: A lista de repositórios que foram adicionados com o uso da função mencionada acima.

A seguir estarão listados os trechos que possuem comentários e que devem ser completados:

- **TODO - ensure to disable button when inputText is empty (use disabled prop to this)**
  Você deve desabilitar o botão de busca quando o input de texto estiver vazio. Para isso, você pode usar a propriedade `disabled` que recebe uma condição (verdadeiro ou falso) que indica quando o botão está habilitado ou não.
  **Dica:** A condição deve ser feita com base no estado `inputText`.
  ***
- **TODO - update inputText value when input text value changes**
  O input de texto deve atualizar o estado `inputText` a cada vez que uma letra for digitada (ou apagada).
  ***
- **TODO:**
  **- call addRepository function sending inputText value;**
  **- clean inputText value.**
  Na função `handleAddRepository` você deve chamar a função `[addRepository]` passando o nome do repositório que está no estado. Caso o nome seja válido, isso vai fazer com que o estado `repositories` do hook `useRepositories` seja atualizado com as informações do repositório buscado. Caso contrário, um alerta será mostrado em tela.
  Além disso, será necessário limpar o valor do input após a busca ser feita.
  ***
- **TODO - navigate to the Repository screen sending repository id. Remember to use the correct prop name (repositoryId) to the repositoy id.**
  Na função `handleRepositoryPageNavigation`, você deve fazer a navegação do usuário para a tela `Repository` enviando também o id do repositório em uma propriedade chamada `repositoryId`. Lembre-se de usar esse exato nome, pois isso será usado para recuperar o valor na próxima tela.
  Essa navegação deverá acontecer ao clicar no card de algum repositório listado.

### `[src/screens/Repository/index.tsx]`

A tela Repository é onde as informações mais detalhadas do repositório serão mostradas.

Para que isso seja possível, o id do repositório é recuperado dos parâmetros de navegação e usado como argumento para a função `findRepositoryById` do hook `useRepositories`. Essa função recebe o id de um repositório e retorna todas as informações desse repositório que estão armazenadas no contexto:

```tsx
{
  id: number,
  full_name: string;
  owner: {
    avatar_url: string;
  };
  description: string;
  stargazers_count: number;
  forks_count: number;
  open_issues_count: number;
  issues_url: string;

  issues: Array<{
	  id: number;
	  title: string;
	  html_url: string;
	  user: {
	    login: string;
	  };
	}>
}
```

O primeiro passo aqui é colocar corretamente as informações do repositório em todos os lugares que possui comentários dentro do componente para que tudo seja exibido corretamente.

Após isso, você precisa completar mais alguns trechos que também estão comentados:

- **TODO - onPress prop calling**
  No componente `Card`, dentro do componente `IssuesList`, você precisa chamar a função `handleIssueNavigation` no onPress passando a URL da issue (`issue.html_url`).
- **TODO - use Linking to open issueUrl in a browser**
  Dentro da função `handleIssueNavigation` você precisa abrir o navegador padrão do dispositivo com o link da issue recebido como argumento. Para isso, o React Native nos dá o **[Linking](https://reactnative.dev/docs/linking)** (que já está importado para você).
  Com ele, é possível usar o método `openURL` passando como argumento, a URL a ser aberta. Exemplo: `Linking.openURL('http://minha-url-aqui.com')`.
  Se preferir, você pode também olhar a referência do método diretamente por esse link:
  [Linking · React Native](https://reactnative.dev/docs/linking#openurl)

# 📅 Entrega

Esse desafio deve ser entregue a partir da plataforma da Rocketseat. Envie o link do repositório que você fez suas alterações. Após concluir o desafio, além de ter mandado o código para o GitHub, fazer um post no LinkedIn é uma boa forma de demonstrar seus conhecimentos e esforços para evoluir na sua carreira para oportunidades futuras.

Feito com 💜 por Rocketseat 👋 Participe da nossa [comunidade aberta!](https://discord.gg/Ns86RQyVH8)
