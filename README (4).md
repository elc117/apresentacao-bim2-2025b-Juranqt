# 💧 Exercício: Entenda e Modifique o Jogo *Drop*  

Este **README.md** documenta as respostas e modificações realizadas no exercício proposto para o jogo **“Drop”** — um jogo simples em que pingos caem e o jogador deve mover um balde para capturá-los.  
O projeto foi desenvolvido em **Java** utilizando a **LibGDX**.

---

## 🧩 Resumo das Modificações

**Arquivo principal modificado:** `Main.java`  

### ✨ Funcionalidades adicionadas:
- ✅ **Contador de pingos capturados** (exibido no terminal e na tela)  
- ⚡ **Aumento da velocidade** dos pingos após capturar um certo número (limite: **10**)  

📄 O código completo e modificado está disponível no arquivo **`Main.java`**.

---

## 🧠 Tarefas e Respostas

### 1️⃣ Localizar onde é criado um novo pingo  
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

### 2️⃣ Localizar onde ocorre a colisão do pingo com o balde  
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

### 3️⃣ Adicionar um contador de pingos no balde  

#### 🖥️ Exibição no terminal
- Adicione o campo:  
  ```java
  int score = 0;
  ```
- Dentro da colisão:
  ```java
  score++;
  System.out.println("Pingos capturados: " + score);
  ```

#### 🎮 Exibição na tela
- Declare e inicialize:
  ```java
  BitmapFont font;
  font = new BitmapFont();
  ```
- No método `draw()`:
  ```java
  font.draw(spriteBatch, "Pingos: " + score, 10, viewport.getWorldHeight() - 10);
  ```
- Libere o recurso no `dispose()`:
  ```java
  font.dispose();
  ```

**✅ Resultado:**  
O contador agora aparece **no terminal** e **na tela do jogo** a cada captura!

---

### 4️⃣ Modificar a velocidade dos pingos após atingir um limite  

**Limite definido:** `10 pingos`

#### 🛠️ Modificações:
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
      dropSpeed = 4f; // aumenta visivelmente a velocidade
  }
  ```

**⚡ Resultado:**  
Após capturar **10 pingos**, a queda dos próximos pingos fica **consideravelmente mais rápida**, tornando o jogo mais desafiador.

---

## ▶️ Como Executar

1. Certifique-se de ter um projeto **LibGDX** configurado.  
2. Substitua o conteúdo do arquivo `Main.java` pelo código modificado.  
3. Execute o jogo.  
4. Mova o balde com as **setas do teclado** ou **toque na tela** (em dispositivos móveis).  
5. Observe:  
   - O **contador de pingos** na tela e no terminal;  
   - O **aumento da velocidade** após **10 capturas**.  

---

## 🗒️ Notas Adicionais

- Todas as modificações estão comentadas no código com:
  ```java
  // MODIFICAÇÃO:
  ```
  facilitando a identificação.  
- O exercício reforça conceitos de **renderização**, **atualização de lógica de jogo**, **detecção de colisão** e **controle de estados** em **LibGDX**.  

---

### 👨‍💻 Desenvolvido para fins educacionais
Projeto baseado no exemplo oficial da **LibGDX** com modificações práticas para aprendizado de lógica de jogos e estrutura de código em Java.
