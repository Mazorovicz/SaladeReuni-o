Componente de código Power Drag Drop
O componente Power Drag Drop permite criar interfaces de usuário de arrastar e soltar dentro de aplicativos de tela e páginas personalizadas.

Aqui está um exemplo de interface do usuário estilo Kanban criada usando o componente PowerDragDrop sobre o Microsoft Dataverse.
![kanban](https://user-images.githubusercontent.com/124692478/228073637-f130f5fd-9cd1-45a7-88dd-82837b8ec021.gif)
Power Drag and Drop funciona com:
Páginas personalizadas com Escala para caber DESATIVADA
Aplicativos de tela de mesa responsivos com Escala para caber DESATIVADO
Observação: os aplicativos de tela de layout de telefone não são suportados devido às transformações CSS que eles aplicam - em vez disso, crie um aplicativo de tela de tablet responsivo.


Começo rápido
Siga estas etapas para começar a usar rapidamente o componente Power Drag Drop.

Configurar
Ative os componentes de código para aplicativos de tela em seu ambiente. Consulte Componentes de código para aplicativos de tela - Power Apps | Microsoft Learn
Importe a solução gerenciada PowerDragDrop para seu ambiente.
Dentro de uma solução, crie uma nova página personalizada ou aplicativo de tela.
Abra Configurações -> Exibição -> Definir escala para caber em DESLIGADO .
No menu Inserir da barra lateral , selecione Obter mais componentes do rodapé
Na guia Code que aparece dentro do painel Import Components que aparece, selecione o componente Power Drag Drop e selecione Import .
No menu Inserir da barra lateral, expanda Componentes de código e selecione o componente Power Drag Drop para colocá-lo em sua tela.
Defina a fonte de dados dos itens de arrastar e a zona mestre
O componente Power Drag Drop requer que haja uma única zona mestre por grupo de itens sendo arrastados/soltados. Por exemplo, se você tivesse uma lista de tarefas que deseja arrastar entre diferentes raias Kanban, teria apenas uma única zona mestre e, em seguida, uma zona filha separada para cada raia.

Adicione um botão à sua tela e, dentro do evento OnSelect , adicione o seguinte:

ClearCollect(
    colItems,
    ForAll(Sequence(10),
        {
            Id:Text(Value),
            Name:"Item " & Value,
            Zone:"zone1"
        }
    )
)
Clique no botão para criar os itens dentro colItems.

Selecione o PowerDragDropcomponente em sua tela e, dentro do painel de propriedades do controle, defina o seguinte:

Unid:colItems
Campos -> Adicionar campo -> Selecione a coluna Nome
Identificação da Zona de Queda:zone1
Outros IDs de zona de queda:zone2
Zona principal:On
Selecione a guia Avançado do painel de propriedades do controle , defina o seguinte:

IdColumn:"Id"
ZonaColuna:"Zone"
Agora você deve ver uma lista de itens aparecer no componente PowerDragDrop .

imagem-20221118113406938

Observação : Se você vir apenas um conjunto de itens vazios, verifique se a coluna Nome foi adicionada aos Campos conforme descrito acima.

Adicionar uma zona filho
Copie a zona mestre e cole-a na tela ao lado da original.

Nas propriedades da segunda zona, defina as seguintes propriedades:

Zona principal:Off
Identificação da zona de queda:zone2
Outras zonas de queda:<No Value>
Volte para a zona mestre original e alterne a zona mestre entre One Offpara redefini-la como mestre.

Agora você deve ser capaz de arrastar itens entre as duas zonas:

Arraste e solte

Each time an item is dropped, the `OnDrop` event is raised.
A CurrentItemscoleção na zona mestre acompanha as alterações feitas arrastando e soltando.

Se você quiser redefinir as alterações a qualquer momento (por exemplo, depois de salvar as alterações), você pode definir a InputEventpropriedade para uma variável de contexto como ctxDragDropInputEvente defini-la usando:

UpdateContext({ctxDragDropInputEvent: "ClearChanges" & Rand()});
Se precisar forçar uma nova renderização a qualquer momento devido à alteração dos dados do item, você pode definir a InputEventpropriedade usando:

UpdateContext({ctxDragDropInputEvent: "Reset" & Rand()});
Modelos e estilo
O componente PowerDragDrop possui propriedades de estilo, como borda/cor de fundo, no entanto, se você precisar de um estilo mais complexo, poderá usar a linguagem do guidão para criar um modelo HTML:

Adicione todas as colunas que você precisa referenciar no modelo usando Campos -> Adicionar campo -> Selecionar coluna
Defina a propriedade Item Template para algo semelhante a:
<div
	style="
		box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
		display:flex;
		flex-direction: column;
		margin: 16px;
        padding: 8px;
        border-radius: 8px;
	">
	<div>{{Name}}</div>
	<div>{{Id}}</div>
</div>
Isso será semelhante ao seguinte:

modelos

Nota : O html será despojado de todos os elementos do script.

Ações
Se você precisar adicionar botões de ação ao seu modelo, poderá fazê-lo adicionando um elemento clicável que tenha um nome de classe prefixado com action-seguido do nome da ação ( clickmeno exemplo abaixo):

<div
	style="
		box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
		display:flex;
		flex-direction: column;
		margin: 16px;
        padding: 8px;
        border-radius: 8px;
	">
	<div>{{Name}}</div>
	<button class="action-clickme">Click Me</buttom>
</div>
Se você adicionar um rótulo com o Texto definido como : $"{PowerDragDrop1.ActionName} {PowerDragDrop1.ActionItemId}", verá algo semelhante a:

ações

Nota : Os elementos clicáveis ​​não precisam ser botões, eles podem ser links ou divelementos clicáveis.

Cada vez que uma ação é selecionada, o OnActionevento é gerado.

Controle de Foco
Se você deseja definir programaticamente o foco no controle, pode usar o seguinte para atualizar a variável de contexto associada à Input Eventpropriedade:

UpdateContext({ctxDragDropInputEvent:"SetFocus" & Text(Rand())})
Se você precisar definir o foco em um item específico, poderá usar:

UpdateContext({ctxDragDropInputEvent:"FocusItem,SOME_ITEM_ID," & Text(Rand())})
Onde você substitui SOME_ITEM_IDpelo Id fornecido na coluna designada pelo IdColumn.
