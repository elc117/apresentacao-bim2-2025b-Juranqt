# Exercício: Entenda e Modifique o Jogo *Drop*  
--- 

### Funcionalidades adicionadas:
- **Contador de pingos capturados** (exibido no terminal e na tela)  
- **Aumento da velocidade** dos pingos após capturar um certo número (limite: **10**)  

---

## Tarefas e Respostas

### 1️ Localizar onde é criado um novo pingo  
**Localização:**  
No método `logic()`, dentro da condição:

```java
if (dropTimer > 1f) {
    dropTimer = 0;
    createDroplet();
}
```

**Detalhes:**  
O método `createDroplet()` instancia um novo **Sprite** com a textura do pingo, define tamanho (1x1), posição X aleatória e posição Y no topo da tela (`worldHeight`), adicionando-o à lista `Array<Sprite> dropSprites`.

---

### 2 Localizar onde ocorre a colisão do pingo com o balde  
**Localização:**  
Ainda no método `logic()`, dentro do loop que percorre `dropSprites` (de trás para frente).

**Detalhes:**  
A colisão é detectada por:

```java
if (bucketRectangle.overlaps(dropRectangle))
```

Se houver sobreposição:
- O pingo é removido com `dropSprites.removeIndex(i);`
- Um som é tocado: `dropSound.play();`

---

### 3️ Adicionar um contador de pingos no balde  

#### Exibição no terminal
- Adicione o campo:  
  ```java
  int score = 0;
  ```
- Dentro da colisão:
  ```java
  score++;
  System.out.println("Pingos capturados: " + score);
  ```

#### Exibição na tela
- Declare e inicialize:
  ```java
  BitmapFont font;
  font = new BitmapFont();
  ```
- No método `draw()`:
  ```java
  font.draw(spriteBatch, "Pingos: " + score, 10, viewport.getWorldHeight() - 10);
  ```

** Resultado:**  
O contador agora aparece **no terminal** e **na tela do jogo** a cada captura!

---

### Modificar a velocidade dos pingos após atingir um limite  

**Limite definido:** `10 pingos`

#### Modificações:
- Adicione:
  ```java
  float dropSpeed = 2f;
  int speedIncreaseThreshold = 10;
  ```
- Substitua o movimento:
  ```java
  dropSprite.translateY(-dropSpeed * delta);
  ```
- Após o incremento do score:
  ```java
  if (score >= speedIncreaseThreshold) {
      dropSpeed = 8f; // aumenta visivelmente a velocidade
  }
  ```

** Resultado:**  
Após capturar **10 pingos**, a queda dos próximos pingos fica **consideravelmente mais rápida**, tornando o jogo mais desafiador.

---



# Análise do Jogo Drop - LibGDX


---

## Análise da Classe `Drop`

### Método `create()`
- Configura os principais componentes do jogo, como o `SpriteBatch` e o `BitmapFont`.
- Define a tela inicial (`MainMenuScreen`) e passa a si mesmo como referência (`this`) para outras telas.

### Passagem de Referência
- `MainMenuScreen(this)` permite que outras telas acessem o objeto principal `Drop`.
- Recurso comum em LibGDX para compartilhar elementos como `batch` e `font`.

### Atributos Públicos
- Embora viole o princípio de encapsulamento, é usado por simplicidade.
- Facilita o acesso direto às propriedades em um projeto tutorial.

---

## Análise da Classe `MainMenuScreen`

### Construtor
- Recebe o objeto `Drop` (referenciado como `passed_game`), possibilitando acesso a recursos globais.

### Atributos
- **`final`** → Impede reatribuição após inicialização (garante imutabilidade).  
- **`static`** → Pertence à classe e não à instância (exemplo: `WIDTH`, `HEIGHT`).

### Método `render()`
- Responsável pela lógica principal da tela inicial:
  - Limpa a tela.
  - Configura a câmera.
  - Desenha o texto de boas-vindas.
  - Verifica se o jogador tocou ou clicou para iniciar o jogo.
- Transfere o controle para `GameScreen` e chama `dispose()` para liberar recursos.

---

## Análise da Classe `GameScreen`

### Função Principal
Gerencia o **gameplay**, renderização, entrada, áudio e colisões.

### Construtor
- Recebe `Drop` como referência.
- Define diversos valores **hardcoded** (ex: resolução, velocidade, posição inicial).
- Esses valores poderiam ser transformados em **constantes configuráveis**.

### Atributos e Métodos
- Controla câmera, texturas, sons, colisões (`Rectangle`), pontuação (`dropsGathered`) e listas de gotas.

### Criação de Gotas Aleatórias
- Método responsável: `spawnRaindrop()`.
- Executado a cada 1 segundo (`TimeUtils.nanoTime()`).
- Cria uma gota (`Rectangle`) com:
  - `x` aleatório entre `0` e `800 - 64`.
  - `y` fixo em `480` (topo da tela).
- Adiciona a gota na lista `raindrops` e atualiza `lastDropTime` com `TimeUtils.nanoTime()` para controlar o intervalo de spawn.

### Detecção de Colisão (Balde x Gota)
- Verificada dentro do loop de atualização no método `render()`, utilizando um `Iterator` para iterar sobre `raindrops`.
- Para cada gota, verifica sobreposição com o balde usando `raindrop.overlaps(bucket)` para detectar colisão.
- Se houver colisão:
  - Incrementa `dropsGathered`.
  - Toca o som (`dropSound.play()`).
  - Remove a gota da lista (`iter.remove()`).
- A verificação ocorre após mover a gota para baixo (`raindrop.y -= 200 * Gdx.graphics.getDeltaTime()`) e antes de remover gotas que saem da tela (`if (raindrop.y + raindrop.height < 0)`).

---
## Construtores
- Classe MainMenuScreen: O construtor public MainMenuScreen(final Drop passed_game) é outro exemplo claro.
    - Ele recebe uma referência ao objeto Drop (armazenada em game = passed_game)
    - Inicializa a câmera com new OrthographicCamera()
    - Define constantes estáticas como WIDTH e HEIGHT para a resolução da tela. Isso permite acesso controlado a recursos compartilhados e configura a tela de menu sem valores hardcoded desnecessários.

## Criação de Objetos
- Em `GameScreen.java`, `bucket = new Rectangle();` cria um objeto retângulo para o balde. Também, `raindrops = new Array<Rectangle>();` cria uma lista dinâmica para armazenar gotas.

## Chamadas de Métodos
- Em `GameScreen.java`, `camera.update();` chama o método para atualizar a câmera.
- Em `game.batch.draw(bucketImage, bucket.x, bucket.y);` chama o método `draw` para renderizar a imagem do balde.

## Herança
- `Drop.java` estende `Game` (classe base do LibGDX), sobrescrevendo métodos como `create()` e `render()`.
- `GameScreen.java` e `MainMenuScreen.java` implementam a interface `Screen`, herdando comportamentos padrão de telas.

## Polimorfismo
- `game.setScreen(new GameScreen(game));` em `MainMenuScreen.java` demonstra polimorfismo, pois setScreen aceita qualquer objeto que implemente `Screen` (como `GameScreen` ou `MainMenuScreen`), permitindo troca dinâmica de telas sem conhecer o tipo exato em tempo de compilação.

## Ponto Mais Fácil na Tarefa
- Identificar Exemplos de POO: Foi relativamente fácil reconhecer conceitos como herança (`Drop` estendendo `Game`) e polimorfismo (`setScreen` aceitando diferentes implementações de `Screen`)
  
## Ponto Menos Fácil na Tarefa
- Compreender a Detecção de Colisões e Otimização: foi desafiador entender como `raindrop.overlaps(bucket)` funciona (sem ter visto a implementação da classe `Rectangle`).
