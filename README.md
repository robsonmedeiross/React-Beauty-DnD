# React-Beuty-DnD
React Beautiful DnD é uma biblioteca de arrastar e soltar acessível da Atlassian. Uma interação que permite a alguém clicar e arrastar um item e, em seguida, soltá-lo em outro lugar, geralmente tendo um efeito colateral no aplicativo.


Referências:
https://cibersistemas.pt/tecnologia/como-adicionar-arrastar-e-soltar-no-react-com-react-beautiful-dnd/
https://github.com/colbyfayock/my-final-space-characters/tree/part-0-starting-point
https://github.com/atlassian/react-beautiful-dnd

# Intalação e inicio

primeiro criei uma matriz de objetos:

const finalSpaceCharacters = [
  {
    id: 'gary',
    name: 'Gary Goodspeed',
    thumb: '/images/gary.png'
  },
  ...
E então eu percorro eles para criar minha lista:

<ul className="characters">
  {finalSpaceCharacters.map(({id, name, thumb}) => {
    return (
      <li key={id}>
        <div className="characters-thumb">
          <img src={thumb} alt={`${name} Thumb`} />
        </div>
        <p>
          { name }
        </p>
      </li>
    );
  })}
</ul>
Acompanhe o commit!

Passo 1: Instalando o React Beautiful DnD
O primeiro passo é instalar a biblioteca via npm.

Dentro do seu projeto, execute o seguinte:

yarn add react-beautiful-dnd
# or
npm install react-beautiful-dnd --save
Isso adicionará a biblioteca ao nosso projeto e estaremos prontos para usá-la em nosso aplicativo.

Passo 2: Fazendo uma lista arrastável e solta com React Beautiful DnD
Com nossa biblioteca instalada, podemos dar à nossa lista a capacidade de arrastar e soltar.

Adicionando contexto de arrastar e soltar ao nosso aplicativo
Na parte superior do arquivo, importe DragDropContextda biblioteca com:

import { DragDropContext } from 'react-beautiful-dnd';
DragDropContext dará ao nosso aplicativo a capacidade de usar a biblioteca. Funciona de forma semelhante à Context API do React, onde a biblioteca agora pode ter acesso à árvore de componentes.

Nota: Se você planeja adicionar arrastar e soltar em mais de uma lista, você precisa ter certeza de que seu DragDropContext envolve todos esses itens, como na raiz do seu aplicativo. Você não pode aninhar DragDropContext.

Queremos envolver nossa lista com DragDropContext:

<DragDropContext>
  <ul className="characters">
  ...
</DragDropContext>
Neste ponto, nada terá mudado com o aplicativo e ele ainda deve carregar como antes.

Tornando nossa lista uma área para soltar
Em seguida, queremos criar uma área Droppable, ou seja, isso nos permitirá fornecer uma área específica onde nossos itens podem ser movidos para dentro.

Primeiro, adicione Droppableà nossa importação no topo do arquivo:

import { DragDropContext, Droppable } from 'react-beautiful-dnd';
Para nosso propósito, queremos que toda a nossa lista não ordenada ( <ul>) seja nossa zona para soltar, então vamos querer envolvê-la novamente com este componente:

<DragDropContext>
  <Droppable droppableId="characters">
    {(provided) => (
      <ul className="characters">
        ...
      </ul>
    )}
  </Droppable>
</DragDropContext>
Você notará que nós o envolvemos um pouco diferente desta vez. Primeiro, definimos um droppableIdem nosso <Droppable>componente. Isso permite que a biblioteca acompanhe essa instância específica entre as interações.

Também estamos criando uma função imediatamente dentro desse componente que passa o providedargumento.

Nota: Esta função pode passar dois argumentos, incluindo um snapshotargumento, mas não a usaremos neste exemplo.

O argumento fornecido inclui informações e referências ao código que a biblioteca precisa para funcionar corretamente.

Para usá-lo, em nosso elemento de lista, vamos adicionar:

<ul className="characters" {...provided.droppableProps} ref={provided.innerRef}>
Isso vai criar uma referência ( provided.innerRef) para a biblioteca acessar o elemento HTML do elemento da lista. Ele também aplica adereços ao elemento ( provided.droppableProps) que permite que a biblioteca acompanhe os movimentos e o posicionamento.

Novamente, neste ponto, não haverá nenhuma funcionalidade perceptível.

Tornando nossos itens arrastáveis
Agora a parte divertida!

A parte final de tornar nossos elementos de lista arrastáveis ​​e soltos é envolver cada item da lista com um componente semelhante ao que acabamos de fazer com a lista inteira.

Estaremos usando o componente Draggable , que novamente, semelhante ao componente Droppable, incluirá uma função onde passaremos adereços para nossos componentes de item de lista.

Primeiro, precisamos importar Draggable junto com o restante dos componentes.

import { DragDropContext, Droppable, Draggable } from 'react-beautiful-dnd';
Em seguida, dentro do nosso loop, vamos envolver o item da lista de retorno com o <Draggable />componente e sua função de nível superior.

{finalSpaceCharacters.map(({id, name, thumb}) => {
  return (
    <Draggable>
      {(provided) => (
        <li key={id}>
          ...
        </li>
      )}
    </Draggable>
Como agora temos um novo componente de nível superior em nosso loop, vamos mover o keyprop do elemento list para Draggable:

{finalSpaceCharacters.map(({id, name, thumb}) => {
  return (
    <Draggable key={id}>
      {(provided) => (
        <li>
Também precisamos definir duas props de adição em <Draggable>, a draggableIde an index.

Queremos adicionar indexcomo um argumento em nossa mapfunção e, em seguida, incluir esses adereços em nosso componente:

{finalSpaceCharacters.map(({id, name, thumb}, index) => {
  return (
    <Draggable key={id} draggableId={id} index={index}>
Finalmente, precisamos definir alguns adereços no próprio elemento da lista.

Em nosso elemento de lista, adicione isso refe espalhe adereços adicionais do providedargumento:

<Draggable key={id} draggableId={id} index={index}>
  {(provided) => (
    <li ref={provided.innerRef} {...provided.draggableProps} {...provided.dragHandleProps}>

