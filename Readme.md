Exercício: Entenda e Modifique o Jogo
Este README.md documenta as respostas e modificações realizadas no exercício proposto para o jogo "Drop" (um jogo simples de pingos caindo em um balde, implementado em Java com LibGDX). O jogo original permite mover um balde para capturar pingos que caem do topo da tela. As modificações foram aplicadas no arquivo Main.java para adicionar funcionalidades como contador de pingos e ajuste de velocidade.

Resumo das Modificações
Arquivo principal modificado: Main.java
Funcionalidades adicionadas:
Contador de pingos capturados (exibido no terminal e na tela).
Aumento da velocidade dos pingos após capturar um certo número de pingos (limite: 10).
O código completo modificado está disponível no arquivo Main.java fornecido anteriormente.
Tarefas e Respostas
1. Localizar onde é criado um novo pingo
Localização: No método logic(), dentro da condição if (dropTimer > 1f) { dropTimer = 0; createDroplet(); }. Isso cria um novo pingo a cada 1 segundo (baseado no delta time).
Detalhes: O método createDroplet() instancia um novo Sprite com a textura do pingo, define seu tamanho (1x1), posição X aleatória e posição Y no topo da tela (worldHeight), e o adiciona à lista Array<Sprite> dropSprites.
2. Localizar onde ocorre a colisão do pingo com o balde
Localização: No método logic(), dentro do loop que itera sobre dropSprites (de trás para frente).
Detalhes: A colisão é verificada com if (bucketRectangle.overlaps(dropRectangle)), onde bucketRectangle representa o balde e dropRectangle o pingo. Se houver sobreposição, o pingo é removido da lista (dropSprites.removeIndex(i)) e um som é tocado (dropSound.play()). Isso ocorre após mover o pingo para baixo.
3. Adicionar um contador de pingos no balde
Primeiro, modificação para exibir no terminal:
Adicione uma variável int score = 0; como campo da classe.
No método logic(), dentro da condição de colisão, incremente o score (score++) e imprima no terminal com System.out.println("Pingos capturados: " + score);.
Depois, modificação para exibir na tela do jogo:
Adicione um campo BitmapFont font; e inicialize-o no create() com font = new BitmapFont();.
No método draw(), após spriteBatch.begin(), adicione font.draw(spriteBatch, "Pingos: " + score, 10, viewport.getWorldHeight() - 10); para renderizar o texto no topo esquerdo da tela.
Certifique-se de descartar a fonte no dispose() com font.dispose();.
Resultado: O contador agora aparece no terminal a cada captura e na tela durante o jogo, substituindo a ausência anterior.
4. Modificar a velocidade dos pingos depois que o jogo atingir um certo número de pingos no balde
Limite definido: 10 pingos (variável int speedIncreaseThreshold = 10;).
Modificações:
Adicione campos: float dropSpeed = 2f; (velocidade base) e o limite.
No método logic(), dentro do loop de movimento dos pingos, substitua a translação fixa por dropSprite.translateY(-dropSpeed * delta);.
Após incrementar o score na colisão, adicione if (score >= speedIncreaseThreshold) { dropSpeed = 4f; } para dobrar a velocidade (de 2f para 4f).
Isso afeta novos pingos e pode ser ajustado para pingos existentes verificando o score antes da translação.
Resultado: Após capturar 10 pingos, a velocidade dos pingos aumenta, tornando o jogo mais desafiador.
Como Executar
Certifique-se de ter um projeto LibGDX configurado.
Substitua o conteúdo do arquivo Main.java pelo código modificado fornecido.
Execute o jogo. Use as setas do teclado ou toque na tela para mover o balde.
Observe o contador no terminal e na tela, e note o aumento de velocidade após 10 capturas.
Notas Adicionais
Todas as modificações estão comentadas no código com // MODIFICAÇÃO: para facilitar a identificação.
Se o jogo não compilar, verifique se as dependências do LibGDX estão corretas (ex.: BitmapFont requer o módulo gdx-freetype ou similar para fontes customizadas, mas aqui usamos a padrão).
Para mais ajustes (ex.: sons, níveis ou gráficos), consulte a documentação do LibGDX.